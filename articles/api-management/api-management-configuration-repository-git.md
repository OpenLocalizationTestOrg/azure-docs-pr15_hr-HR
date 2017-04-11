<properties 
    pageTitle="Spremanje i konfiguriranje konfiguraciju upravljanja API servisa pomoću brojka" 
    description="Saznajte kako spremiti i konfiguriranje konfiguraciju upravljanja API servisa pomoću brojka." 
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


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Spremanje i konfiguriranje konfiguraciju upravljanja API servisa pomoću brojka

>[AZURE.IMPORTANT] Konfiguracija brojka za upravljanje API trenutno u pretpregledu. Functionally završi, ali je u pretpregledu jer smo traže aktivno povratne informacije o toj značajci. Moguće je da dajemo možda prijelom promjena povratnih, stoga preporučujemo ne ovisi o značajki za korištenje u radnog okruženja. Ako imate povratne informacije ili pitanja, obratite nam se keyboardshortcuts@Microsoft.commailto na `apimgmt@microsoft.com`.

Svaku instancu servisa za upravljanje API održava konfiguracija baze podataka koja sadrži informacije o konfiguraciji i metapodataka za instancu servisa. Promjene može unijeti na instancu servisa promijeniti postavku na portalu za publisher, pomoću komponente PowerShell cmdlet ili upućivanje poziva REST API-JA. Osim ovih metoda možete upravljati i konfiguracija instancu servisa pomoću brojka, kao što su omogućivanjem scenarija upravljanja servisa:

-   Rad s verzijama konfiguracija – preuzimanje i spremanje različitih verzija konfiguracije servisa
-   Skupno promjene konfiguracije - promjene više dijelova konfiguracije servisa u vašem lokalnom spremištu i integracija promjene na poslužitelj s jedne operacije
-   Poznati toolchain brojka i tijek rada – koriste tooling brojka i tijekova rada koji su vam već poznati

Sljedeći dijagram prikazuje pregled različitih načina da biste konfigurirali vaše instanca servisa za upravljanje API-JA.

![Konfiguriranje brojka][api-management-git-configure]

Kada unesete promjene u funkcioniranju servisa pomoću portala za publisher, cmdleta ljuske PowerShell ili REST API-JA, upravljate pomoću baze podataka za servis za konfiguraciju na `https://{name}.management.azure-api.net` krajnje točke, kao što je prikazano na desnoj strani dijagrama. S lijeve strane dijagram prikazuje kako možete upravljati konfiguraciju servisa pomoću brojka i brojka spremištu servisa nalazi se na `https://{name}.scm.azure-api.net`.

Sljedeći koraci nude pregled upravljanja na instancu upravljanje API servisa pomoću brojka.

1.  Omogućivanje pristupa brojka na servisu
2.  Spremanje baze podataka usluge konfiguracije vaše brojka spremište
3.  Kloniraj repo brojka na lokalno računalo
4.  Povući najnovije repo na lokalno računalo i potvrdi i automatske promjene natrag na vašem repo
5.  Implementacija promjene iz vaše repo u konfiguraciji baze podataka usluge

U ovom se članku opisuje kako omogućiti i koristiti brojka da biste upravljali konfiguraciju servisa i sadrži referencu za datoteke i mape u spremištu brojka.

## <a name="to-enable-git-access"></a>Da biste omogućili pristup brojka

Brzo možete prikazati stanje konfiguracije brojka pregledom brojka ikonu u gornjem desnom kutu portal za publisher. U ovom primjeru access brojka još nije omogućeno.

![Status brojka][api-management-git-icon-enable]

Da biste pogledali i konfiguriranje postavki konfiguriranje brojka, možete ili kliknite ikonu brojka ili kliknite izbornik **Sigurnost** i prijeđite na karticu **Konfiguracija spremište** .

![Omogućivanje BROJKA][api-management-enable-git]

Da biste omogućili pristup brojka, potvrdite okvir **Omogući brojka pristup** .

Nakon trenutak se sprema promjene i prikazuje se poruka potvrde. Imajte na umu da se ikona brojka promijenio boju da biste naznačili da je omogućen pristup brojka i poruka o stanju sada ukazuje na to koji postoje nespremljene promjene u spremište. To je zato upravljanje API servisa bazi podataka za konfiguraciju još nije spremljen u spremištu.

![Brojka omogućeno][api-management-git-enabled]

>[AZURE.IMPORTANT] Sve tajne koji su definirani kao svojstava će biti pohranjene u spremištu i će ostati u njegov povijest dok ne onemogućivanje i ponovno Omogućivanje pristupa brojka. Svojstva omogućuju sigurno je mjesto za upravljanje vrijednosti konstanta niza, uključujući tajne, preko svih API konfiguracija i pravila, pa ne morate ih spremiti izravno u svojih Izjava pravila. Dodatne informacije potražite u članku [upute za korištenje svojstava u pravila upravljanja Azure API -JA](api-management-howto-properties.md).

Informacije o omogućavanje ili onemogućavanje pristupa brojka pomoću REST API-JA, potražite u članku [Omogućavanje ili onemogućavanje pristupa brojka pomoću REST API -JA](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Da biste spremili konfiguracija servisa za spremište brojka

U prvom koraku prije kloniranje spremište je da biste spremili trenutno stanje servisa konfiguraciju u spremištu. Kliknite **Spremi konfiguraciju u spremištu**.

![Spremanje konfiguracija][api-management-save-configuration]

Napravite željene promjene na zaslonu za potvrdu, a zatim kliknite **u redu** da biste spremili.

![Spremanje konfiguracija][api-management-save-configuration-confirm]

Nakon nekoliko sekundi dok se sprema konfiguraciju i status konfiguraciju u spremištu prikazuju se, uključujući datum i vrijeme zadnje promjene konfiguracije i posljednje sinkronizacije između konfiguracija servisa i spremište.

![Stanje konfiguracije][api-management-configuration-status]

Konfiguracija spremljena u spremište možete klonirana.

Informacije o obavljanju ovog postupka pomoću REST API-JA, potražite u članku [provođenja konfiguracije snimke pomoću REST API -JA](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Da biste Kloniraj spremište na lokalno računalo

Da biste Kloniraj spremište, morate URL vaše spremište, korisničko ime i lozinku. Korisničko ime i URL-a prikazuju se pri vrhu karticu **Konfiguracija spremište** .

![Kloniraj brojka][api-management-configuration-git-clone]

Lozinka se generira pri dnu kartice **spremište konfiguracije** .

![Generiranje lozinke][api-management-generate-password]

Da biste generirali lozinke, najprije provjerite da su **isteka** postavljen tako da na željeni datum i vrijeme važenja pa kliknite **Generiraj tokena**.

![Lozinke][api-management-password]

>[AZURE.IMPORTANT] Zabilježite lozinku. Kada napusti ovu stranicu lozinku ne prikazat će se ponovno.

U sljedećem se primjeru koristi alat za tulumu brojka iz [Brojka za Windows](http://www.git-scm.com/downloads) , ali možete koristiti bilo koji brojka alat koji ste upoznati s.

Otvorite alat za brojka u željenu mapu, a zatim pokrenite sljedeću naredbu da biste Kloniraj brojka spremište na lokalno računalo pomoću naredbe koje ste dobili od portal za publisher.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Navedite korisničko ime i lozinku kada se to od vas zatraži.

Ako vam se prikaže sve pogreške, pokušajte izmjena vaše `git clone` naredbu da biste uključili korisničko ime i lozinku, kao što je prikazano u sljedećem primjeru.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Ako je to daje pogrešku, pokušajte URL kodiranje lozinku dio naredbu. Jedan brz način za to je da biste otvorili Visual Studio i problema sljedeću naredbu u **Izvršnog prozora**. Da biste otvorili **Izvršnog prozora**, otvorite bilo koji rješenja ili projekta u Visual Studio (ili stvoriti novu aplikaciju prazan konzola), a zatim odaberite **Windows**, **izvršnom** na izborniku **za ispravljanje pogrešaka** .

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Koristite šifriranu lozinku uz korisničko ime i spremište mjesto za sastavljanje naredbi brojka.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

Kada je spremište klonirana možete pregledavati i raditi s njim u lokalnom datotečnom sustavu. Dodatne informacije potražite u članku [datoteka i mapa strukturu referenca lokalnom spremištu brojka](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Da biste ažurirali vašem lokalnom spremištu s najnovije konfiguracija instancu servisa

Ako unesete promjene u vašoj instanci upravljanje API servisa na portalu za publisher ili pomoću REST API-JA, spremite promjene u spremište prije najnovije promjene možete ažurirati na lokalnom spremištu. Da biste to učinili, kliknite **Spremi konfiguracije spremište** na kartici **konfiguracije spremište** na portalu za publisher, a problema sljedeću naredbu u vašem lokalnom spremištu.

    git pull

Prije pokretanja `git pull` provjerite radite li u mapi za vašem lokalnom spremištu. Ako ste završili s `git clone` naredbu, a zatim imenika morate promijeniti svoje repo tako da pokrenete naredbu ovako.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Da biste automatske promjene iz vaše lokalne repo repo poslužitelja

Da biste promjene na lokalnom spremištu spremištu poslužitelja, morate Zapiši promjene i automatske ih u spremište poslužitelja. Da biste potvrdili promjene, otvorite alat za naredbu brojka, prijeđite u direktorij na lokalnom spremištu i problema sljedeće naredbe.

    git add --all
    git commit -m "Description of your changes"

Da biste sve se pohranjuje na poslužitelju, pokrenite sljedeću naredbu.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Za implementaciju promjene konfiguracije servisa za upravljanje API instanca servisa

Kada lokalne promjene izvršene i pomiču spremište server, možete ih implementirati vašoj instanci upravljanje API servisa.

![Implementacija][api-management-configuration-deploy]

Informacije o obavljanju ovog postupka pomoću REST API-JA, potražite u članku [Implementacija brojka promjene baza podataka pomoću REST API -JA](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Datoteke i mape strukturu referenca lokalnom spremištu brojka

Datoteka i mapa u spremištu lokalne brojka sadrže informacije o konfiguraciji o instanca servisa.

| Stavke                       | Opis                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| Korijenska mapa api-upravljanje | Sadrži najviše razine konfiguracija instancu servisa                                  |
| API-ji mape                | Sadrži konfiguraciju za API-ji instanca servisa                            |
| grupa mapa              | Sadrži konfiguraciju za grupe u instanca servisa                          |
| pravila mape            | Sadrži pravila u instanca servisa                                              |
| portalStyles mape        | Sadrži konfiguraciju za prilagodbe portala za razvojne inženjere u instanca servisa |
| Premještanje gore            | Sadrži konfiguraciju za proizvode u instanca servisa                        |
| mapu Predlošci           | Sadrži konfiguraciju za predloške e-pošte u instanca servisa                 |

Svaka mapa može sadržavati jednu ili više datoteka, a u nekim jednu ili više mapa slučajevima, primjerice mapa za svaki API-JA, proizvoda ili grupu. Datoteka u svakoj mapi vrijede za entitet vrste opisan naziv mape.

| Vrsta datoteke | Svrha                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Informacije o konfiguraciji o odgovarajući entitet                  |
| HTML      | Opisi o entitet, često prikazuju u portala za razvojne inženjere |
| XML       | Izraze pravila                                                      |
| CSS-a       | Listovi stilova za prilagodbu portala za razvojne inženjere                        |

Te se datoteke mogu stvoriti, izbrisane, uređivati, i upravlja na lokalni datotečni sustav i implementirati opet na vaše instanca servisa za upravljanje API-JA.

>[AZURE.NOTE] Sljedeći entiteti se ne nalaze u spremištu brojka i ne može konfigurirati pomoću brojka.
>
>-    Korisnici
>-    Pretplate
>-    Svojstva
>-    Entiteti portala za razvojne inženjere osim stilova

### <a name="root-api-management-folder"></a>Korijenska mapa api-upravljanje

Korijenska `api-management` mapa sadrži na `configuration.json` datoteku koja sadrži najviše razine informacije o instanca servisa u sljedećem obliku.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

Prva četiri postavke (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, a `UserRegistrationTermsConsentRequired`) karta sljedeće postavke na kartici **identiteta** u odjeljku **Sigurnost** .

| Postavka za identitet                     | Karte                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | **Preusmjeravanje anonimnim korisnicima stranica za prijavu u** potvrdni okvir |
| UserRegistrationTerms                | **Uvjeti korištenja na Registracija korisnika za** tekstni okvir               |
| UserRegistrationTermsEnabled         | Potvrdni okvir za **Prikaz Uvjeti korištenja na stranici za prijavu**         |
| UserRegistrationTermsConsentRequired | **Obavezan pristanak** potvrdnog okvira                          |

![Postavke za identitet][api-management-identity-settings]

Sljedeća četiri postavke (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, a `DelegationValidationKey`) karta sljedeće postavke na kartici **delegiranje** u odjeljku **Sigurnost** .

| Postavka delegiranje           | Karte                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | Potvrdni okvir **delegat prijave i registracije**    |
| DelegationUrl                | Tekstni okvir **URL delegiranje krajnje točke**        |
| DelegatedSubscriptionEnabled | Potvrdni okvir **delegat proizvoda pretplate** |
| DelegationValidationKey      | Tekstni okvir **Delegat ključ za provjeru valjanosti**        |

![Postavke delegiranja][api-management-delegation-settings]

Konačni postavka `$ref-policy`, mapira datoteka izjave globalni pravila za instancu servisa.

### <a name="apis-folder"></a>API-ji mape

Na `apis` mapa sadrži mapu za svaki API-JA u instanca servisa koji sadrži sljedeće stavke.

-   `apis\<api name>\configuration.json`– To je konfiguraciju za API Sučelja i sadrži informacije o URL servisa za pozadinskog sustava i operacije. Ovo je iste podatke koji će se vratiti ako ste radili da biste nazvali [dobili određene API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) `export=true` u `application/json` oblik.
-   `apis\<api name>\api.description.html`– To je opis u API-JA i odgovara na `description` svojstvo [entitet API-JA](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
-   `apis\<api name>\operations\`-Ova mapa sadrži `<operation name>.description.html` datoteke koje mapiranje postupke u na API-JA. Svaka datoteka sadrži opis jedne operacije u API koji karte na `description` svojstvo [entitet operacije](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) u REST API-JA.

### <a name="groups-folder"></a>grupa mapa

Na `groups` mapa sadrži mapu za svaku grupu definirano u instanca servisa.

-   `groups\<group name>\configuration.json`– To je konfiguraciju za grupu. Ovo je iste podatke koji će se vratiti ako ste radili da biste nazvali operacija [dobili određene grupe](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) .
-   `groups\<group name>\description.html`– To je opis grupe i odgovara na `description` svojstvo [entitet grupe](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>pravila mape

Na `policies` mapa sadrži izraze pravila za vaše instancu servisa.

-   `policies\global.xml`-sadrži pravila koje je definirao na globalni opseg za vaše instanca servisa.
-   `policies\apis\<api name>\`– Ako imate pravila koje je definirao u dosegu API-JA, se nalaze u ovu mapu.
-   `policies\apis\<api name>\<operation name>\`mapa – ako imate pravila koje je definirao u dosegu operacija, se nalaze u ovu mapu u `<operation name>.xml` datoteke koje mapiranje izjave pravila za svaku operaciju.
-   `policies\products\`– Ako imate pravila koje je definirao u dosegu proizvoda, se nalaze u ovu mapu koja sadrži `<product name>.xml` datoteke koje mapiranje izjave pravila za svaki proizvod.

### <a name="portalstyles-folder"></a>portalStyles mape

Na `portalStyles` mapa sadrži konfiguraciju i stil listove prilagodbe portala za razvojne inženjere za instancu servisa.

-   `portalStyles\configuration.json`– koji sadrži nazive listova stilova koristi portala za razvojne inženjere
-   `portalStyles\<style name>.css`-svaki `<style name>.css` datoteka sadrži stilove za portala za razvojne inženjere (`Preview.css` i `Production.css` po zadanom).

### <a name="products-folder"></a>Premještanje gore

Na `products` mapa sadrži mapu za svaki proizvod definirano u instanca servisa.

-   `products\<product name>\configuration.json`– To je konfiguraciju za proizvod. Ovo je iste podatke koji će se vratiti ako ste radili da biste nazvali operacija [se određeni proizvod](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) .
-   `products\<product name>\product.description.html`-Ovo je opis proizvoda koja odgovara na `description` svojstvo [entitet proizvoda](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) u REST API-JA.

### <a name="templates"></a>Predlošci

Na `templates` mapa sadrži konfiguracije [Predlošci e-pošte](api-management-howto-configure-notifications.md) instanca servisa.

-   `<template name>\configuration.json`– To je konfiguraciju za predložak e-pošte.
-   `<template name>\body.html`– To je tijelo predložak e-pošte.

## <a name="next-steps"></a>Daljnji koraci

Informacije o drugi načini upravljanja vaše instanca servisa potražite u članku:

-   Upravljanje vaše instanca servisa pomoću sljedeće Cmdlete ljuske PowerShell
    -   [Uvođenje servisa referenca cmdlet komponente PowerShell](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Upravljanje servisom PowerShell cmdlet reference](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Upravljanje vaše instanca servisa na portalu za publisher
    -   [Upravljanje prvi API-JA](api-management-get-started.md)
-   Upravljanje vaše instanca servisa pomoću REST API-JA
    -   [Referenca API upravljanje REST API-JA](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Pogledajte videozapis pregled

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




