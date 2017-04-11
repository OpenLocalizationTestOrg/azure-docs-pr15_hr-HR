<properties
    pageTitle="Aplikacije servisa za API aplikacije – što se promijenilo | Microsoft Azure"
    description="Saznajte što je novo API aplikacije u aplikacije servisa za Azure."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>Aplikacije servisa za API aplikacije – što se promijenilo

Na događaj funkcija Connect() studenom 2015 nekoliko poboljšanja aplikacije servisa za Azure su [objaviti](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Te poboljšanja obuhvaćaju podlozi promjene API aplikacije da bi bolje poravnali s Mobile i web-aplikacije, smanjite broj pojam te Poboljšanja implementacije i performanse prilikom izvođenja. Pokretanje novih aplikacija API studenom 30 2015., stvorite pomoću portala za Azure upravljanja web-mjesta ili najnovije tooling odražavat će te promjene. U ovom se članku opisuju te promjene, kao i kako implementirati postojeće aplikacije da biste iskoristili mogućnosti.

## <a name="feature-changes"></a>Promjene značajki
Ključne značajke programa API aplikacije – provjeru autentičnosti, CORS i API metapodatke – premještena izravno u aplikaciji servisa. Uz tu promjenu značajke dostupne su na Web, Mobile i aplikacije API-JA. Zapravo, sva tri zajednički koristiti iste vrste **Microsoft.Web/sites** resursa u upravitelju resursa. Pristupnik API aplikacije više nije potrebno ili ponuđen s aplikacijama API-JA. To također olakšava koristiti Azure API upravljanje jer će biti samo za jedan API pristupnik za upravljanje.

![Pregled aplikacija API-JA](./media/app-service-api-whats-changed/api-apps-overview.png)

Načelo ključa dizajna s ažuriranjem aplikacije API je omogućuju vam da bi se prikazala API kao što je na jeziku po izboru.  Ako vaš API već implementiran kao web-aplikacijama ili mobilnu aplikaciju, nemate implementirati aplikaciju da iskoristite prednost novih značajki. Ako trenutno na pretpregled aplikacije API-JA, upute za migraciju je detaljan ispod.

### <a name="authentication"></a>Provjera autentičnosti
Na postojeće kodirane API aplikacije, servise aplikacije Mobile i web-aplikacije provjere autentičnosti značajke imaju omogućeno Sjedinjeno komuniciranje i dostupne u jednom plohu provjere autentičnosti Azure aplikacije servisa na portalu za upravljanje. Uvod u servise za provjeru autentičnosti u servisu aplikacije, potražite u članku [provjere autentičnosti aplikacije servisa za proširivanje / autorizacije](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Za scenarije API-JA postoje broj odgovarajuće nove mogućnosti:

- **Podrška za korištenje Azure izravno Active Directory**, bez potrebe za razmjenu token AAD token sesiju koda za klijent: klijent možete samo obuhvatiti tokeni AAD zaglavlje autorizacije prema specifikacija tokena nošenja. To također znači da nema SDK servisa specifičnih za aplikacije potreban je na strani klijenta ili poslužitelju. 
- **Servis za servis ili access "Internog"**: Ako imate u filtru ili neke klijentski pristup API-ji bez sučelje koje je potrebno, možete zatražiti token pomoću programa Upravitelj servisa AAD i prenesite ga aplikacije servisa za provjeru autentičnosti s aplikacijom.
- **Autorizacija odgođeno**: mnoge aplikacije imaju različiti ograničenja pristupa za različite dijelove aplikacija. Možda želite neke API-ji bude javno dostupno dok drugi obaveznu prijavu. Izvorni značajku provjere autentičnosti/Autorizacija nije all-or-nothing, s čitavog web-mjesta koje je obavezna prijava. Ta mogućnost i dalje postoji, ali umjesto toga možete omogućiti kodu aplikacije za prikazivanje pristup odluke nakon aplikacije servisa sadrži provjere autentičnosti korisnika.
 
Dodatne informacije o novim značajkama provjere autentičnosti potražite u članku [provjere autentičnosti i autorizacije API aplikacije u aplikacije servisa za Azure](app-service-api-authentication.md). Informacije o migriranju postojeće aplikacije API iz prethodne modela aplikacija za API na novu, pročitajte članak [pod prijenos postojeće API aplikacija](#migrating-existing-api-apps) u nastavku ovog članka.
 
### <a name="cors"></a>CORS
Umjesto razdvojen zarezom **MS_CrossDomainOrigins** aplikacije postavku, postoji sada je plohu na portalu za upravljanje Azure za konfiguriranje CORS. Umjesto toga ga možete konfigurirati pomoću upravitelja resursa tooling kao što su Azure PowerShell, EŽA ili [Explorer resursa](https://resources.azure.com/). Postavite svojstvo **cors** na vrstu **Microsoft.Web/sites/config** resursa za vaše ** &lt;naziv web-mjesta&gt;/web-** resursa. Ako, na primjer:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API metapodataka
Definicija plohu API sada je dostupan na Web, Mobile i aplikacije API-JA. Na portalu za upravljanje možete odrediti relativni url ili apsolutni url koja pokazuje na krajnje taj prikaz domaćini 2.0 za Swagger vaše API-JA. Umjesto toga ga možete konfigurirati pomoću tooling Voditelj resursa. Postavite svojstvo **apiDefinition** na vrstu **Microsoft.Web/sites/config** resursa za vaše ** &lt;naziv web-mjesta&gt;/web-** resursa. Ako, na primjer:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Trenutačno krajnju točku metapodataka mora biti javno dostupnu bez provjere autentičnosti za mnoge do klijente (npr Visual Studio REST API-JA klijenta generacije i PowerApps "Dodaj API-JA" tijek) trošiti ga. To znači ako koristite aplikacije servisa za provjeru autentičnosti, a želite izložiti definicija API iz same aplikacije, morat ćete koristiti mogućnost Odgođeni provjere autentičnosti opisanih tako da je rute do metapodatka Swagger javno.

## <a name="management-portal"></a>Portal za upravljanje
Odabir **Novo > Web + Mobile > API aplikacije** na portalu će stvoriti API aplikacije koje odražavaju nove mogućnosti opisane u članku. **Pregled > aplikacije za API** će prikazati samo nove aplikacije API-JA. Kada pregledavate u aplikaciju API-JA na plohu dijeli isti raspored i mogućnosti kao sadržaj Web- a mobilne aplikacije. Samo razlike su sadržaja za brzi početak rada i redoslijed postavke.

Postojeće aplikacije API-JA (ili trgovine API aplikacije stvorene iz aplikacija logike) uz pretpregled programa prethodne mogućnosti će i dalje biti vidljivi u dizajneru logike aplikacije i pri pregledavanju sve resursa u grupu resursa.

## <a name="visual-studio"></a>Visual Studio

Većina web-aplikacije tooling će funkcionirati novih aplikacija API jer ih zajednički koristiti iste osnovne vrste resursa **Microsoft.Web/sites** . Visual Studio Azure tooling, međutim, mora biti nadograđena verziju 2.8.1 ili noviji jer je izlaže brojne mogućnosti specifične za API-ji. Preuzmite SDK sa [stranice Azure preuzimanja](https://azure.microsoft.com/downloads/).

Uz rationalization aplikacije usluge vrste, objavljivati i Sjedinjene u odjeljku **Objavi > aplikacije servisa za Microsoft Azure**:

![Objavljivanje aplikacija API-JA](./media/app-service-api-whats-changed/api-apps-publish.png)

Da biste saznali više o SDK 2.8.1, pročitajte objava [bloga](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Umjesto toga možete ručno iz možete uvesti profil Objavi portal za upravljanje da biste omogućili objavljivanje. Međutim, Explorer oblaka, generiranje koda i odabira/stvaranja aplikacije API će potreban SDK 2.8.1 ili noviji.

## <a name="migrating-existing-api-apps"></a>Migracija postojeće aplikacije API-JA
Ako je prilagođeni API-JA implementiran na prethodnu verziju za pretpregled aplikacija API-JA, dospije koje preseliti novi model za API aplikacije po prosinac 31, 2015. Budući da i model stara i nova temelje se na webu API-ji smješten u aplikacije servisa, većina postojeći kod se može ponovno koristiti.

### <a name="hosting-and-redeployment"></a>Hostiranje i ponovno uvođenje
Koraci za redeploying isti su kao uvođenja bilo koje postojeće API Web aplikacije servisa. Koraci:

1. Stvorite prazan API aplikacije. To možete učiniti na portalu s novo > API aplikaciju, u Visual Studio Objavi ili iz tooling Voditelj resursa. Ako koristite tooling Voditelj resursa ili predložaka, postavite vrijednost **vrstu** na **api** na vrsti resursa **Microsoft.Web/sites** da bi postavke i početak rada u portal za upravljanje usmjerena prema scenariji API-JA.
2. Povezivanje i implementacija projekta aplikaciju API prazan na bilo koji od mehanizme za implementaciju podržava aplikacije servisa. Pročitajte [dokumentaciju za implementaciju aplikacije servisa za Azure](../app-service-web/web-sites-deploy.md) da biste saznali više. 
  
### <a name="authentication"></a>Provjera autentičnosti
Aplikacije servisa za servise za provjeru autentičnosti podržava iste mogućnosti koje su bile dostupne s prethodne modelom API aplikacije. Ako koristite tokeni sesije i zahtijevaju SDK-ovi, koristite na sljedeći postupak klijentske i poslužiteljske SDK-ovi:

- Klijent: [Azure mobilnu verziju klijentskog SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Poslužitelj: [Microsoft Azure mobilne aplikacije .NET provjere autentičnosti proširenja](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Ako ste koristili umjesto aplikacije servisa za alfa SDK-ovi, te su sada izostavljen je:

- Klijent: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Poslužitelj: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Posebno s Azure Active Directory, no bez aplikacije servisa specifične potreban je ako koristite AAD token izravno.

### <a name="internal-access"></a>Interna programa access
Prethodni modela aplikacija API uključiti na razinu ugrađene pristupa internim. To je potrebno korištenje SDK za potpisivanje zahtjeva. Opisan ranije, pomoću novog modela aplikacija API AAD servisa upravitelji može se koristiti kao zamjenski izraz za provjeru autentičnosti servis za servis bez potrebe za aplikaciju servisa specifičnih SDK. Saznajte više u [glavni provjere autentičnosti API aplikacije u Azure aplikacije servisa za servis](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Otkrivanje
Prethodni modela API aplikacije je prodao API-ji za otkrivanje drugih aplikacija API prilikom izvođenja u istoj grupi resursa iza istom Pristupniku. To je posebno korisno u arhitekturi koja implementira microservice uzoraka. Dok izravno nije podržano, dostupne su nekoliko mogućnosti:

1. Koristite Azure API resursima za predočavanje elektroničkih dokumenata.
2. Postavite Azure upravljanja API ispred vaše API-ji aplikacije servisa za hostira. Azure upravljanja API služi kao u façade i pružaju stabilan vanjski dostupnog url čak i ako ste Interna topologije promijeni.
3. Stvoriti vlastite aplikaciju API-JA za otkrivanje i imaju druge aplikacije API registrirati pomoću aplikacije za otkrivanje prilikom pokretanja.
4. Uvođenje trenutku popuniti postavki aplikacije svih aplikacija API-JA (i klijenti) krajnje točke od drugih aplikacija API-JA. Ovo je izgledna u implementacijama predloška i jer API aplikacije sada vam dati kontrolu nad URL-a.

## <a name="using-api-apps-with-logic-apps"></a>Korištenje aplikacija API-JA s aplikacijama logike

Novog modela aplikacija API dobro funkcionira s [Logike aplikacije verzija sheme 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više, pročitajte u člancima u [dokumentaciji aplikacije API sekcije](https://azure.microsoft.com/documentation/services/app-service/api/). Su ažurirane u skladu s vizualnim novog modela za API aplikacije. Osim toga, stupili na forumima za dodatne informacije i smjernice o migracije:

- [MSDN forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Preljev stoga](http://stackoverflow.com/questions/tagged/azure-api-apps)
