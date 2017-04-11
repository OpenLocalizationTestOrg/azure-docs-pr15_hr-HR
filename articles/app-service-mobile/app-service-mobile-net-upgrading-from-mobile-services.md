<properties
    pageTitle="Nadogradnja s mobilnim servisa na aplikacije servisa za Azure"
    description="Saznajte kako jednostavno nadograditi aplikaciju mobilnih usluga mobilne aplikacije servisa aplikacija"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Nadogradite postojeći servis .NET Azure mobilne aplikacije servisa

Mobilne aplikacije servisa za je novi način da biste sastavili mobilne aplikacije koje se koriste Microsoft Azure. Da biste saznali više, pročitajte članak [koje su mobilne aplikacije?].

U ovoj se temi opisuje kako nadograditi postojeće .NET pozadinskog aplikacije servisa Azure Mobile novih aplikacija mobilne aplikacije servisa. Tijekom izvođenja te nadogradnje, postojeće usluga mobilne aplikacije možete nastaviti raditi.   Ako je potrebno nadograditi aplikaciju za pozadinskog Node.js odnose [nadogradnje servisa Node.js Mobile](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Kada se mobilni pozadinskog će se nadograditi aplikacije servisa za Azure, ima pristup svim značajkama aplikacije servisa za i naplatu prema [aplikacije servisa za cijene], cijene ne mobilne usluge.

##<a name="migrate-vs-upgrade"></a>Migriranje nasuprot nadogradnje

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Preporučuje se taj [Migracija](app-service-mobile-migrating-from-mobile-services.md) prije prolaze kroz nadogradnju. Na taj način možete staviti obje verzije aplikacije na istoj Plan za aplikaciju servisa i plaćati bez dodatnih troškova.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Poboljšanja u mobilne aplikacije .NET server SDK

Nadogradnje na novu [Mobilne aplikacije SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) pruža sljedeće prednosti:

- Veću fleksibilnost na NuGet ovisnosti. Da biste mogli koristiti druge kompatibilne verzije okruženja za hosting više ne navode vlastitu verzije paketa NuGet. Međutim, ako postoje novi ključna bugfixes ili sigurnosna ažuriranja radi Mobile Server SDK ili ovisnosti, morate na servisu ručno ažurirati.

- Više fleksibilnosti u mobilnom SDK. Eksplicitno možete kontrolirati koje značajke i usmjerava postavljeni, primjerice provjeru autentičnosti, tablice API-ji i krajnja točka za registraciju automatske. Da biste saznali više, pogledajte [upute za korištenje .NET server SDK za Azure mobilne aplikacije](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Podrška za druge platforme ASP.NET projekta vrste usmjerava. Sada možete hostirati MVC i Web API kontrolera u istom projektu kao mobilne pozadinskog projekta.

- Podrška za nove aplikacije servisa za provjeru autentičnosti značajke koje omogućuju korištenje uobičajenih konfiguriranje provjere preko web i mobilne aplikacije.

##<a name="overview"></a>Osnovni pregled nadogradnje

U mnogim slučajevima nadogradnje bit će jednostavan prijelaz na novi poslužitelj SDK za mobilne aplikacije i ponovno objavljivanje kod na novu instancu za mobilnu aplikaciju. Postoje, no neke scenarije koje je potrebno neke dodatne konfiguracijske, kao što su scenariji napredne provjeru autentičnosti i radu s zakazano zadatke. Svaki od ovih opisana su u noviji sekcije.

>[AZURE.TIP] Se preporučuje čitanje i razumijevanje ostatak ove teme potpuno prije pokretanja nadogradnju. Zabilježite sve značajke koristite koji su oblačićima ispod.

Klijent mobilne usluge SDK-ovi su **nije** kompatibilna s novi poslužitelj SDK za mobilne aplikacije. Da bi se omogućuje continuity pružanja usluge za aplikaciju, trebali biste ne objavite promjene na web-mjesto trenutno posluživanje objavljene klijente. Umjesto toga, stvorite novi mobilne aplikacije koje služi kao duplikat. Ovu aplikaciju možete staviti na istu tarifu aplikacije servisa za izbjegavanje dodatnih troškova financijske nakon toga.

Imat ćete dvije verzije aplikacije: nešto što ostaje isto i služi aplikacije objavljenim u članku koji, i drugi koji zatim možete nadograditi i cilja sa novo izdanje klijenta. Možete premjestiti i testirati kod vašeg tempom, ali morate biti sigurni da sve popravci programskih pogrešaka unesete se primjenjuje se na oba. Nakon što mislite da željeni broj klijentskih aplikacija koji ste ažurirali na najnoviju verziju, možete izbrisati izvorne migriranim aplikacije ako po volji.

Potpuna konture za postupak nadogradnje je na sljedeći način:

1. Stvorite novu aplikaciju Mobile
2. Ažuriranje projekta da biste koristili novi Server SDK-ovi
3. Pustite novu verziju klijentska aplikacija
4. (Neobavezno) Brisanje na izvornom migriranim instanci

##<a name="mobile-app-version"></a>Stvaranje drugog instanci aplikacije
Prvi korak pri nadogradnji je da biste stvorili mobilnu aplikaciju resursa koji će biti smješteno novu verziju aplikacije. Ako ste već migrirati postojeću mobilnu uslugu, želite da biste stvorili ovu verziju na istu tarifu hostinga. Otvorite [portal za Azure] , a zatim otvorite migriranim aplikacije. Zabilježite u aplikaciju servisa Planiranje se izvodi na.

Nakon toga stvorite drugu aplikaciju pojavu slijedeći [upute za stvaranje pozadinske .NET](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Kada se to od vas zatraži da biste odabrali ste Plan usluge za aplikaciju ili "hosting plan" odaberite plan migriranim aplikacije.

Vjerojatno ćete koristiti istu bazu podataka i obavijesti koncentrator kao mobilne usluge. Možete kopirati te vrijednosti tako da otvorite [portal za Azure] i kretanje u izvornom programu, a zatim kliknite **Postavke** > **Postavke aplikacije**. U odjeljku **Nizove veze**, kopirajte `MS_NotificationHubConnectionString` i `MS_TableConnectionString`. Idite na novu nadogradnje web-mjesto, pa ih zalijepite u Prebrisivanje postojećih vrijednosti. Ponovite taj postupak za aplikaciju postavke vašim potrebama aplikacije. Ako ne koristite migriranim servisa, možete čitati nizove veze i postavki aplikacije na kartici **Konfiguriraj** odjeljka mobilne usluge [Azure klasični portal].

Napravite njezinu kopiju projekta ASP.NET aplikacije i objavite je na novo web-mjesto. Kopiranjem klijentsku aplikaciju ažurirati novim URL-om, provjeriti radi li sve prema očekivanjima.

## <a name="updating-the-server-project"></a>Ažuriranje server project

Mobilne aplikacije sadrži nove [Mobilne aplikacije Server SDK] koji omogućuje veći dio ista funkcija kao runtime mobilne usluge. Najprije uklonite sve pakete usluga mobilne reference. U upravitelju paket NuGet potražite `WindowsAzure.MobileServices.Backend`. Većina aplikacija će vidjeti nekoliko paketa ovdje, uključujući `WindowsAzure.MobileServices.Backend.Tables` i `WindowsAzure.MobileServices.Backend.Entity`. U tom slučaju počinju najniže paketa u stablu ovisnosti, kao što su `Entity`, i uklonite ga. Kada se to od vas zatraži, uklonite sve pakete zavisne. Ponavljajte postupak dok ste uklonili `WindowsAzure.MobileServices.Backend` sam.

Sada će imati projekt koji više referenci mobilne usluge SDK-ovi.

Sljedeće će dodati reference SDK za mobilne aplikacije. Za tu nadogradnju Većina razvojnim inženjerima ćete preuzeti i instalirati na `Microsoft.Azure.Mobile.Server.Quickstart` pakiranje, kao što je to će izvukli cijeli skup potrebna.

Će biti vrlo nekoliko alata za Kompiliranje pogreške koja je rezultat razlike u SDK-ovi, ali ih je lako adresa i možete pronaći u ostatku u ovom se odjeljku.

### <a name="base-configuration"></a>Osnovnu konfiguraciju

Nakon toga u WebApiConfig.cs, možete zamijeniti:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

s

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Ako želite da biste saznali više o novom poslužitelju .NET SDK i upute za dodavanje i uklanjanje značajke iz aplikacije, pročitajte članak [kako koristiti .NET server SDK] temu.

Ako aplikacija omogućuje, pomoću značajke provjere autentičnosti i morate registrirati u OWIN proizvod. U ovom slučaju, trebali biste premjestiti iznad koda za konfiguraciju u novi OWIN početnu klasu.

1. Dodavanje paketa NuGet `Microsoft.Owin.Host.SystemWeb` ako ne bude već uključena u projektu.
2. U Visual Studio, desnom tipkom miša kliknite na projektu i odaberite **Dodaj** -> **Nova stavka**. Odaberite **Web** -> **Općenito** -> **OWIN početnu klasu**.
3. Pomicanje gore kod za MobileAppConfiguration iz `WebApiConfig.Register()` da biste na `Configuration()` način novi početnu klasu.

Provjerite je li u `Configuration()` završava metoda:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Postoje dodatne promjene vezane uz provjeru autentičnosti koji se nalaze u odjeljku punu provjeru autentičnosti.

### <a name="working-with-data"></a>Rad s podacima

U mobilnom servise, naziv mobilne aplikacije poslužena kao zadani naziv sheme prilikom postavljanja Framework entitet.

Da biste bili sigurni da imate na istu shemu referencira kao što prije, koristite sljedeće da biste postavili shemi u DbContext aplikacije za:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Provjerite ima li MS_MobileServiceName postavite li navedenog. Možete unijeti i drugi naziv sheme ako program to prilagodili prethodno.

### <a name="system-properties"></a>Svojstva sustava

#### <a name="naming"></a>Dodjela naziva

U poslužitelj za Azure mobilne usluge SDK svojstva sustava uvijek sadržavati dvostruke podvlake (`__`) prefiks za svojstva:

- __createdAt
- __updatedAt
- __deleted
- __version

Klijent mobilne usluge SDK-ovi imaju posebno logike Raščlanjivanje svojstva sustava u tom obliku zapisa.

U aplikacijama za Azure Mobile, više svojstva sustava imaju poseban oblik i imati sljedeće nazive:

- createdAt
- updatedAt
- izbrisana
- verzija

Mobilne aplikacije klijenta SDK-ovi koristi nova svojstva imena sustava da bi se bez promjene su potrebne za klijent kod. Međutim, ako su izravno upućivanje poziva OSTALE u funkcioniranju servisa pa promijenite upitima sukladno tome.

#### <a name="local-store"></a>Lokalno spremište

Promjena naziva svojstva sustava znači izvanmrežnu sinkronizaciju lokalne baze podataka za mobilne usluge nije kompatibilno s mobilne aplikacije. Ako je to moguće, izbjegavajte nadogradnje klijentskim aplikacijama servisa za mobilne aplikacije Mobile tek nakon promjene na čekanju su poslane na poslužitelj. Nakon toga nadograđena aplikacija trebali biste koristiti novi naziv datoteke baze podataka.

Ako klijentskih aplikacija će se nadograditi servisa za mobilne aplikacije Mobile dok su izvanmrežne promjene na čekanju u redu čekanja za operaciju, baze podataka sustava morate ažurirati da koristi nove nazive stupaca. Na iOS, to se može postići pomoću laganih Migracija da biste promijenili nazive stupaca. Na Android i .NET upravljanih klijent, trebate upisati prilagođeni SQL da biste preimenovali stupce za podatkovne tablice za objekt.

Na iOS, trebali biste promijeniti sheme temeljni podaci za vaše podatke entiteti da sadrže sljedeće. Imajte na umu da svojstva `createdAt`, `updatedAt` i `version` više ne morate je `ms_` prefiks:

| Atribut |  Vrsta   | Napomena                                                 |
|---------- |  ------ | -----------------------------------------------------|
| ID-a        | Niz, označen kao obavezan  | primarni ključ u udaljenog spremišta         |
| createdAt | Datum    | (neobavezno) karte da biste svojstvo createdAt sustava         |
| updatedAt | Datum    | (neobavezno) karte da biste svojstvo updatedAt sustava         |
| verzija   | Niz  | (neobavezno) koristiti da biste otkrili sukobe, karte verziju |

#### <a name="querying-system-properties"></a>Slanje upita svojstva sustava

U Azure mobilne usluge, svojstva sustava ne šalju prema zadanim postavkama, ali samo kada su zatražili pomoću niza upita `__systemProperties`. Nasuprot tome, u sustavu Azure mobilne aplikacije su svojstva **uvijek odabrana** jer su dio objektni model SDK poslužitelja.

Ta promjena uglavnom utječe prilagođene implementacije upravitelja domena, kao što su nastavci `MappedEntityDomainManager`. U mobilne usluge, klijent nikad ne zahtijeva sva svojstva sustava je li moguće koristiti s `MappedEntityDomainManager` koji ne zapravo mapirati sva svojstva. No u aplikacijama za Azure Mobile, tih svojstava nemapirana uzrokovat će pogrešku u upitima GET.

Da biste riješili taj problem najjednostavnije da biste izmijenili vaše DTOs tako da ih nasljeđuju od `ITableData` umjesto `EntityData`. Zatim dodajte na `[NotMapped]` atributa polja koja će ispušteni.

Na primjer, definira sljedeće `TodoItem` sa svojstvima bez sustava:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Napomena: Ako se pojavi pogreške `NotMapped`, dodajte referencu u sklopu `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Mobilne usluge nalaze određene podrške za CORS po prelamanje ASP.NET CORS rješenja. Ovaj sloja prelamanje uklonjen je da bi se dobilo programer veću kontrolu pa izravno na raspolaganju [CORS ASP.NET podržavaju](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

Glavna područja složen ako koristite CORS koji su u `eTag` i `Location` zaglavlja mora imati dopuštenje za klijentski program SDK-ovi kako bi funkcionirao.

### <a name="push-notifications"></a>Automatske obavijesti
Za slanje, glavni stavke koja se može pronaći nedostaje iz Server SDK je klasa PushRegistrationHandler. Registracija rukuje malo drukčije u mobilne aplikacije, a tagless registracije omogućena je prema zadanim postavkama. Upravljanje oznake može napraviti pomoću prilagođenih API-ji. Pročitajte članak upute [registracije za oznake](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) za dodatne informacije.

### <a name="scheduled-jobs"></a>Zakazani zadaci
Zakazani zadaci su ugrađeni u mobilne aplikacije, pa postojeće poslove koji se nalaze u vašem pozadinskog .NET morate nadograditi pojedinačno. Jedna od mogućnosti jest da biste stvorili zakazani [Zadatak Web] na web-mjesta kod mobilnu aplikaciju. Mogli postaviti kontroler koja sadrži kôd posla i konfiguriranje [Rasporeda Azure] pogoditi iz prvog te krajnjoj točki očekivani raspored.

### <a name="miscellaneous-changes"></a>Ostale promjene
Sve ApiControllers koji će se troše mobilnu verziju klijentskog odmah morate imati na `[MobileAppController]` atribut. Više je po zadanom uključena tako da vam se drugi ApiControllers da biste se ne utječe na mobilni formatters.

Na `ApiServices` objekt više nije dio SDK-a. Da biste pristupili postavkama za mobilnu aplikaciju, možete koristiti sljedeće:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Isto tako, zapisivanje je sada napraviti pomoću standardnih pisanje praćenje ASP.NET:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Pitanja vezana uz provjeru autentičnosti

Komponenti provjere autentičnosti mobilnih usluga se sada je premještena u značajku provjere autentičnosti/autorizacije aplikacije servisa. Možete saznati o omogućivanju ovo web-mjesta potražite u temi [Dodaj provjere autentičnosti za aplikacije za mobilne uređaje](app-service-mobile-ios-get-started-users.md) .

Za neki davatelji usluga, kao što su AAD, Facebook i Google, trebali odražava postojeće Registracija iz aplikacije za kopiranje. Jednostavno morati otvorite centar za davatelja identiteta i dodajte nove preusmjeravanja URL za registraciju. ID klijenta i tajna konfigurirate aplikacije usluge provjere autentičnosti/autorizacije.

### <a name="controller-action-authorization"></a>Autorizacija kontroler akcija
Sve pojave na `[AuthorizeLevel(AuthorizationLevel.User)]` atributa mora se promijeniti sada da biste koristili standardne ASP.NET `[Authorize]` atribut. Uz to, kontrolera sada su anonimni prema zadanim postavkama, kao i u drugim aplikacijama ASP.NET.
Ako ste koristili neku od drugih AuthorizeLevel mogućnosti, kao što je administrator ili aplikacije, uzmite u obzir da se razlikuju nema više. Umjesto toga možete postaviti AuthorizationFilters za zajedničke tajne ili konfiguriranje programa Upravitelj servisa AAD da biste omogućili servis za servis pozive sigurno.

### <a name="getting-additional-user-information"></a>Dobivanje dodatnih korisničkih podataka

Možete dobiti dodatne korisničke podatke, uključujući tokeni pristup putem na `GetAppServiceIdentityAsync()` metoda:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Uz to, ako aplikacija traje ovisnosti na korisničkog ID-ove, kao što su pohranite u bazi podataka, važno je obratiti pažnju na korisničke ID-ove između mobilnih usluga i mobilna aplikacija servisa razlikuju. Mobilna usluga korisničkog ID-a, i dalje možete dobiti kroz. Sve podklase ProviderCredentials imaju svojstvo ID korisnika. Stoga nastavka iz primjera prije:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Ako aplikaciju preuzeli sve ovisnosti korisničkog ID-a, važno je da ste pod utjecajem isti Registracija s davateljem identiteta ako je to moguće. ID-ove korisnika obično ograničena za registraciju za aplikaciju koja se koristila, tako da Uvod u novi Registracija nije moguće stvoriti probleme s odgovarajućim korisnicima svoje podatke.

### <a name="custom-authentication"></a>Prilagođena provjera autentičnosti

Ako aplikacija koristi prilagođene provjere autentičnosti rješenje, želite da biste bili sigurni da nadograđeno web-mjesto ima pristup sustavu. Slijedite nove upute za prilagođene provjere autentičnosti u [.NET server SDK pregled] da biste integrirali rješenje. Provjerite Imajte na umu da komponente prilagođene provjere autentičnosti i dalje u pretpregledu.

##<a name="updating-clients"></a>Ažuriranja klijenata
Nakon što dodate radu pozadinskog za mobilnu aplikaciju, možete raditi na novu verziju klijentsku aplikaciju koja se koristi. Mobilne aplikacije sadrži i nova verzija klijenta SDK-ovi i slično nadogradnje poslužitelja iznad, morat ćete ukloniti sve reference mobilne usluge SDK-ovi prije instalacije verzije aplikacije Mobile.

Jedna od glavne razlike između verzija je na constructors više nije potreban aplikacijsku tipku. Sada jednostavno proslijedite u URL-u mobilnu aplikaciju. Na primjer, u klijentima .NET na `MobileServiceClient` Graditelj sada je:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Možete pročitati o instaliranju novi SDK-ovi i korištenju novu strukturu putem veze u nastavku:

- [iOS verzije 3.0.0 ili novije verzije](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) verzije 2.0.0 ili novije verzije](app-service-mobile-dotnet-how-to-use-client-library.md)

Ako aplikacija omogućuje korištenje automatske obavijesti, zabilježite određene Registracija upute za svaku platforme kao što su neke promjene kao i.

Kada je nova verzija klijenta spreman, isprobajte prema projektu nadograđena poslužitelja. Nakon provjere radi li možete pustite novu verziju aplikacije klijentima. Naposljetku, kada korisnici stigli prima ažuriranja, možete izbrisati Services mobilne verzije aplikacije. Nakon što potpuno nadogradite na aplikacije aplikacije servisa Mobile pomoću najnovije poslužitelj SDK za mobilne aplikacije.

<!-- URLs. -->

[Portal za Azure]: https://portal.azure.com/
[Azure klasični portal]: https://manage.windowsazure.com/
[Što su mobilne aplikacije?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobilne aplikacije Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure rasporeda]: /en-us/documentation/services/scheduler/
[Web-posao]: ../app-service-web/websites-webjobs-resources.md
[Kako koristiti .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Aplikacije servisa za cijene]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Pregled SDK poslužitelja za .NET]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
