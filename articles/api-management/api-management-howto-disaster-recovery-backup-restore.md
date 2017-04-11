<properties 
    pageTitle="Kako implementirati Izrada oporavak pomoću servisa sigurnosno kopiranje i vraćanje u upravljanju API Azure | Microsoft Azure" 
    description="Saznajte kako pomoću sigurnosnog kopiranja i vraćanja obaviti oporavak Izrada u upravljanju Azure API-JA." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Kako implementirati Izrada oporavak pomoću servisa sigurnosno kopiranje i vraćanje u upravljanju API Azure

Odabirom za objavljivanje i upravljanje API-ji putem Azure API upravljanje izvodite prednost mnogo odstupanje kvara i infrastrukture mogućnosti koje bi inače imate dizajnirati implementirati i upravljanje. Azure platforme mitigates velike koliki dio potencijalne pogreške pri razlomak troška.

Da biste vratili iz dostupnost problemima koji mogu utjecati regija u kojoj vaše upravljanje API servis je hostira koje mora biti spreman za reconstitute usluge u nekoj drugoj regiji u bilo kojem trenutku. Ovisno o dostupnosti ciljeva i oporavak vrijeme cilj možda ćete morati rezervirati sigurnosne kopije servisa u jednu ili više područja, a zatim pokušajte da biste zadržali sinkronizaciju sa servisom active njihova konfiguracija i sadržaj. Servis sigurnosnog kopiranja i vraćanja značajka omogućuje potrebne sastavnog bloka za implementaciju strategije Izrada oporavak.

Ovaj vodič prikazuje kako Voditelj resursa Azure zahtjeva za provjeru autentičnosti i kako sigurnosnog kopiranja i vraćanja na instanci upravljanje API servisa.

>[AZURE.NOTE] Postupak za sigurnosno kopiranje i vraćanje instanca servisa upravljanje API-JA za oporavak Izrada može se koristiti za replikaciju instanci servisa za upravljanje API scenarije kao što su pripremna.
>
>Imajte na umu da svaki sigurnosne kopije istječe nakon 7 dana. Ako pokušate vratiti sigurnosnu kopiju nakon isteka razdoblja 7 dana istekla, vraćanje neće uspjeti s na `Cannot restore: backup expired` poruku.

## <a name="authenticating-azure-resource-manager-requests"></a>Zahtijeva provjeru autentičnosti Voditelj resursa za Azure

>[AZURE.IMPORTANT] REST API-JA za sigurnosno kopiranje i vraćanje koristi Voditelj resursa Azure i ima različite provjere autentičnosti mehanizam od REST API-ji za upravljanje sustava upravljanja API entiteti. Koraci u ovom se odjeljku opisuju kako Voditelj resursa Azure zahtjeva za provjeru autentičnosti. Dodatne informacije potražite u članku [Provjera autentičnosti Voditelj resursa Azure zahtjeva](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Svi zadaci koje obaviti na resurse pomoću upravitelja resursa Azure morate moguće provjeriti autentičnost kod Azure Active Directory na sljedeći način.

-   Dodaj aplikaciju na klijentu Azure Active Directory.
-   Postavljanje dozvola za aplikaciju koju ste dodali.
-   Pronađite token za provjeru autentičnosti zahtjeva za Azure Voditelj resursa.

U prvi je korak da biste stvorili programa Azure Active Directory. Prijavite se u [Klasične Portal Azure](http://manage.windowsazure.com/) koristite pretplatu koja sadrži vaše instanca servisa za upravljanje API-JA, a zatim otvorite karticu **aplikacije** za zadani Azure Active Directory.

>[AZURE.NOTE] Ako zadani direktorij Azure Active Directory nije vidljiva na vaš račun, obratite se administratoru Azure pretplate da biste dodijelili odgovarajuće dozvole za vaš račun. Informacije o pronalaženju zadani direktorij, potražite u članku [Pronalaženje zadani direktorij](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Stvaranje aplikacije za Azure Active Directory][api-management-add-aad-application]

Kliknite **Dodaj**, **Dodavanje aplikacije je moje tvrtke ili ustanove razvoju**i odaberite **aplikaciju za nativni klijent**. Unesite opisni naziv pa kliknite Dalje strelicu. Unesite URL za rezervirano mjesto, primjerice `http://resources` za **Preusmjeravanje URI**, kao što je je polje obavezno, ali se ne koristi vrijednost kasnije. Kliknite potvrdni okvir da biste spremili aplikacije.

Nakon spremanja aplikacije kliknite **Konfiguriraj**, pomaknite se prema dolje do odjeljka **dozvole za druge aplikacije** pa kliknite **Dodaj aplikaciju**.

![Dodavanje dozvola][api-management-aad-permissions-add]

Odaberite **Windows** **Azure usluga upravljanja API -JA** , a zatim kliknite potvrdni okvir da biste dodali aplikaciju.

![Dodavanje dozvola][api-management-aad-permissions]

Kliknite **Dodijeliti dozvole** pokraj novododani aplikacija **Windows** **Azure usluga upravljanja API -JA** , potvrdite okvir za **Upravljanje servisom Azure programa Access (pretpregled)**i kliknite **Spremi**.

![Dodavanje dozvola][api-management-aad-delegated-permissions]

Prije no što pozivanje API-ji koji generiranje sigurnosnog kopiranja i vraćanja ga je potrebno da biste dobili token. U sljedećem primjeru pomoću značajke pakiranja nuget [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) za dohvaćanje token.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Zamjena `{tentand id}`, `{application id}`, a `{redirect uri}` prema sljedećim uputama.

Zamjena `{tenant id}` ID klijenta programa Azure Active Directory koji ste upravo stvorili. Id možete pristupiti tako da kliknete **Prikaz krajnje točke**.

![Krajnje točke][api-management-aad-default-directory]

![Krajnje točke][api-management-endpoint]

Zamjena `{application id}` i `{redirect uri}` pomoću **Id klijenta** i URL iz odjeljka **Preusmjeravanje ji** iz aplikacije Azure Active Directory **Konfiguriraj** kartice. 

![Resursi][api-management-aad-resources]

Kada se određeni su klauzulom vrijednosti, na primjer koda mora vratiti token slično kao u sljedećem primjeru.

![Tokena][api-management-arm-token]

Prije pozivanja sigurnosnog kopiranja i vraćanja Postupci opisani u sljedećim odjeljcima Postavljanje zaglavlje autorizacije zahtjev za OSTALE poziva.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Sigurnosno kopiranje servis za upravljanje API-JA
Za sigurnosno kopiranje API upravljanje problem sljedeće HTTP servisni zahtjev:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

pri čemu je:

* `subscriptionId`-ID pretplatu koja sadrži pokušavate servisa za upravljanje API-ja za stvaranje sigurnosne kopije
* `resourceGroupName`-niz u obliku "Api - zadana – {servis regija}" gdje `service-region` označava Azure regije gdje upravljanje API uslugu koju pokušavate sigurnosno kopiranje nalazi, npr.`North-Central-US`
* `serviceName`-Naziv servis za upravljanje API želite napraviti sigurnosnu kopiju naveden u trenutku njegovog stvaranja
* `api-version`-Zamijenite`2014-02-14`

U tijelu zahtjeva za Navedite naziv računa za ciljnu Azure prostora za pohranu, tipkovni prečac, naziv spremnika blob i ime sigurnosne kopije:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Postavite vrijednost u `Content-Type` zaglavlje zahtjev za `application/json`.

Sigurnosno kopiranje je dugo izvodi postupak koji može potrajati više minuta.  Ako je zahtjev uspio, a zatim je pokrenuo postupak sigurnosnog kopiranja će se pojaviti u `202 Accepted` Šifra stanja odgovor s na `Location` zaglavlje.  Provjerite "DOHVATI' zahtjevi za URL u na `Location` zaglavlje da biste saznali status operacije. Dok je u tijeku je sigurnosno kopiranje nastavit ćete primati kod stanja '202 prihvaćena'. Kod odgovor `200 OK` će vas upozoriti uspješan dovršetak postupka sigurnosne kopije.

**Napomena**:

- **Spremnik** naveden u na zahtjev za tijelo **mora postojati**.
* Tijekom sigurnosne kopije koju **treba pokušati sve operacije upravljanja servisa** kao što su SKU nadograditi ili prijeći na nižu, promjena naziva domene, itd. 
* Vraćanje na **sigurnosnu kopiju uvijek je samo za 7 dana** od trenutka njegova stvaranja. 
* Koristiti za stvaranje analize **podataka o korištenju** izvješća **nije uključen** u sigurnosnoj kopiji. Koristite [Azure API upravljanje REST API -JA][] povremeno dohvatiti analize izvješća da biste ih sačuvali.
* Učestalost kojom izvođenje servisa sigurnosne kopije utjecat će na vaš cilj točke za oporavak. Da biste minimizirali je preporučljivo implementacijom redovitog sigurnosnog kopiranja, kao i izvođenje osvježavati kopija nakon toga važne izmjene u funkcioniranju servisa za upravljanje API-JA.
* **Promjene** unesene u Konfiguracija servisa (npr. API-ji pravila, izgled portala za razvojne inženjere) tijekom operacije sigurnosne kopije se postupak **možda neće biti obuhvaćen sigurnosnog kopiranja i zbog toga će se izgubiti**.

## <a name="step2"> </a>Vraćanje servis za upravljanje API-JA
Da biste vratili na upravljanje API servisa već stvorili sigurnosnu kopiju provjerite sljedeće HTTP zahtjev:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

pri čemu je:

* `subscriptionId`-id pretplate koja sadrži vraćate sigurnosnu kopiju u servisa za upravljanje API-JA
* `resourceGroupName`-niz u obliku "Api - zadana – {servis regija}" gdje `service-region` označava gdje se hostira servisa za upravljanje API vraćate sigurnosnu kopiju u području Azure npr.`North-Central-US`
* `serviceName`-Naziv upravljanje API servisa koji se vraćaju u navedeni prilikom njegova stvaranja
* `api-version`-Zamijenite`2014-02-14`

U tijelu zahtjeva za navedite mjesto za datoteku sigurnosne kopije, odnosno naziv računa Azure prostora za pohranu, tipkovni prečac, naziv spremnika blob i ime sigurnosne kopije:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Postavite vrijednost u `Content-Type` zaglavlje zahtjev za `application/json`.

Vraćanje je dugo izvodi postupak koji može potrajati 30 ili više minuta da biste dovršili.  Ako je zahtjev uspio, a zatim je pokrenuo postupak vraćanja će se pojaviti u `202 Accepted` Šifra stanja odgovor s na `Location` zaglavlje.  Provjerite "DOHVATI' zahtjevi za URL u na `Location` zaglavlje da biste saznali status operacije. Dok je u tijeku je obnavljanja nastavit ćete primati '202 prihvaćena' Šifra stanja. Kod odgovor `200 OK` će vas upozoriti uspješan dovršetak postupka vraćanja.

>[AZURE.IMPORTANT] **U SKU** od servisa koji se vratiti u **moraju se podudarati** SKU servis sigurnosne vraćaju.
>
>**Promjene** unesene u Konfiguracija servisa (npr. API-ji pravila, izgled portala za razvojne inženjere) tijekom postupak vraćanja je u tijeku **može se prebrisati**.

## <a name="next-steps"></a>Daljnji koraci
Pogledajte sljedeće Microsoft blogovi za dvije različite vodiči postupaka sigurnosnog kopiranja i vraćanja.

-   [Replicirati Azure API za upravljanje računima](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Zahvaljujemo na Gisela za svoj udio u ovom članku.
-   [Upravljanje Azure API-JA: Sigurnosno kopiranje i vraćanje konfiguracija](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Pristup detaljne po Stuart ne odgovara službeni smjernice, ali je vrlo zanimljive.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Upravljanje Azure API REST API-JA]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
