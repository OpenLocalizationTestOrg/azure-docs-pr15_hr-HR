<properties
    pageTitle="Upute za rad s poslužiteljem pozadinske .NET SDK za mobilne aplikacije | Aplikacije servisa za Azure"
    description="Saznajte kako raditi s poslužiteljem pozadinske .NET SDK za Azure servisa mobilna aplikacija."
    keywords="aplikacije servisa za azure aplikacije servisa, mobilne aplikacije, servis za mobilne uređaje, mjerilo skalabilni, aplikacija azure implementaciju aplikacije implementacije"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

U ovoj se temi objašnjava korištenje pozadinskog poslužitelja .NET SDK u ključni scenariji Azure servisa mobilna aplikacija. Azure mobilne aplikacije SDK pomaže u radu s mobilnim klijentima iz ASP.NET aplikacija.

>[AZURE.TIP] [.NET server SDK za Azure mobilne aplikacije] [ 2] je Otvori izvor na GitHub. Spremište sadrži sve izvornog koda uključujući paket za testiranje cijelom poslužitelju SDK jedinica i neke ogledne projekata.

## <a name="reference-documentation"></a>Dokumentacija reference

Poslužitelj SDK potražite u dokumentaciji referenca nalazi se ovdje: [Azure mobilne aplikacije .NET referenca][1].

## <a name="create-app"></a>Kako: Stvaranje .NET mobilnu aplikaciju pozadinskog

Ako pokrećete novi projekt, možete stvoriti pomoću [portala za Azure] ili Visual Studio aplikacije servisa za aplikaciju. Možete pokrenuti aplikaciju aplikacije servisa za lokalno ili objavite projekt oblaku aplikacije servisa za mobilne aplikacije.  

Ako dodajete mobilnim mogućnostima u postojeći projekt, u odjeljku [preuzeti i pokrenuti SDK-a](#install-sdk) .

### <a name="create-a-net-backend-using-the-azure-portal"></a>Stvaranje .NET pozadinskog pomoću portala za Azure

Da biste stvorili programa pozadinskog mobilne aplikacije servisa, ili slijedite [Vodič za brzi početak rada] [ 3] ili slijedite ove korake:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Vratite se u plohu _Početak rada_ u odjeljku **Stvaranje tablice API-JA**, odaberite **C#** kao **pozadinskom jezik**. Kliknite **Preuzmi**, izdvojiti komprimirane projektnih datoteka s vašim lokalnim računalom i otvorite rješenje u Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Stvaranje .NET pozadinskog pomoću Visual Studio 2013 i Visual Studio 2015.

Instalacija [Azure SDK za .NET] [ 4] (verzija 2.9.0 ili noviji) da biste stvorili projekta programa Azure mobilne aplikacije u Visual Studio. Nakon instalacije SDK, stvorite aplikaciju ASP.NET na sljedeći način:

1. Otvorite dijaloški okvir **Novi projekt** (iz *datoteke* > **Novo** > **projekta...**).
2. Proširite **predložaka** > **Visual C#**i odaberite **Web**.
3. Odaberite **ASP.NET web-aplikacije**.
4. Unesite naziv projekta. Zatim kliknite **u redu**.
5. U odjeljku _ASP.NET 4.5.2 predlošci_, odaberite **Azure mobilnu aplikaciju**. Provjerite **glavnog računala u oblaku** da biste stvorili mobilne pozadinskog u oblaku na koju možete objaviti taj projekt.
6. Kliknite **u redu**.

## <a name="install-sdk"></a>Kako: preuzimanje i pokretanje SDK-a

SDK dostupan je na [NuGet.org]. Paket sadrži osnovnu funkciju potrebne da biste počeli koristiti SDK-a. Inicijalizacija SDK potrebne za izvođenje akcija na objekt **HttpConfiguration** .

### <a name="install-the-sdk"></a>Instalacija SDK-a

Da biste instalirali SDK, desnom tipkom miša kliknite project server u Visual Studio, odaberite **Upravljanje NuGet paketa**, traženje paketa [Microsoft.Azure.Mobile.Server] , a zatim kliknite **Instaliraj**.

###<a name="server-project-setup"></a>Pokretanje server project

Uključivanjem klase za pokretanje programa OWIN se pokreće slične projekte druge platforme ASP.NET .NET pozadinskog poslužitelja projekta. Provjerite je li pozivati paket NuGet `Microsoft.Owin.Host.SystemWeb`. Da biste dodali klase u Visual Studio, desnom tipkom miša kliknite na poslužitelju projektu i odaberite **Dodaj** > 
**Nove stavke**, a zatim **Web** > **Općenito** > **OWIN početnu klasu**.  Klase je generirao atribut sljedeće:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

U na `Configuration()` način klase za pokretanje sustava OWIN pomoću objekta **HttpConfiguration** konfigurirati okruženje Azure mobilne aplikacije.
U sljedećem primjeru inicijalizira project server sa značajkama za dodatnu:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Da biste omogućili pojedinačne značajke, morate pozvati načina nastavka na objekt **MobileAppConfiguration** prije pozivanja **ApplyTo**. Na primjer, sljedeći kod dodaje usmjerava zadani sve API kontrolera koji imaju atribut `[MobileAppController]` tijekom pokretanja:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Poslužitelj za brzi početak rada s portala za Azure poziva **UseDefaultConfiguration()**. U ovom ekvivalent sljedećih postavki:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Proširenje metode koristi su:

* `AddMobileAppHomeController()`pruža zadane aplikacije Mobile Azure početne stranice.
* `MapApiControllers()`nudi prilagođene mogućnosti API-JA za kontrolera WebAPI ukrašene s na `[MobileAppController]` atribut.
* `AddTables()`omogućuje njihov prikaz u `/tables` krajnje točke za kontrolera tablice.
* `AddTablesWithEntityFramework()`je oblik šake kratki za mapiranje na `/tables` krajnje točke pomoću entitet Framework temelji kontrolera.
* `AddPushNotifications()`pruža jednostavan način Registracija uređaja za obavijesti koncentratora.
* `MapLegacyCrossDomainController()`nudi standardnih CORS zaglavlja za lokalni razvoj.

### <a name="sdk-extensions"></a>SDK proširenja

Sljedeće paketa utemeljen na NuGet proširenje omogućuju razne značajki za mobilne uređaje koji se mogu koristiti aplikaciju. Omogući proširenja tijekom pokretanja pomoću objekta **MobileAppConfiguration** .

- [Microsoft.Azure.Mobile.Server.Quickstart] podržava Osnovno postavljanje mobilne aplikacije. Dodaje konfiguraciju tako da nazovete proširenje metodu **UseDefaultConfiguration** tijekom pokretanja. Ovaj kućni broj sadrži pratiti proširenja: obavijesti, provjere autentičnosti, entitet, tablice, domenama, a paketi Polazno. Paket se koristi u mobilne aplikacije brzi početak rada na portalu za Azure.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   implementira zadani *mobilne aplikacije je s radom stranice* za korijensko web-mjesta. Dodajte konfiguraciju tako da nazovete metodu   **AddMobileAppHomeController** nastavak.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   sadrži Tečajevi za rad s podacima i skupova kopirane podatke kanal. Dodajte konfiguraciju tako da nazovete metodu **AddTables** nastavak.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   omogućuje Framework entitet za pristup podacima u bazi podataka za SQL. Dodajte konfiguraciju tako da nazovete metodu **AddTablesWithEntityFramework** nastavak.

- [Microsoft.Azure.Mobile.Server.Authentication] omogućuje provjeru autentičnosti i skupova kopirane OWIN proizvod koji se koriste za provjeru valjanosti tokena. Dodavanje konfiguraciju tako da nazovete **AddAppServiceAuthentication**  
   i **IAppBuilder**. Metode za nastavak **UseAppServiceAuthentication** .

- [Microsoft.Azure.Mobile.Server.Notifications] omogućuje automatske obavijesti i definira krajnje za registraciju pribadače. Dodajte konfiguraciju tako da nazovete metodu **AddPushNotifications** nastavak.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   stvara kontroler koji služi podataka prema starim web-preglednicima iz mobilne aplikacije. Dodajte konfiguraciju tako da nazovete metodu   **MapLegacyCrossDomainController** nastavak.

- [Microsoft.Azure.Mobile.Server.Login] nudi metoda AppServiceLoginHandler.CreateToken(), statične način koji se koristi tijekom scenariji prilagođene provjere autentičnosti.   

## <a name="publish-server-project"></a>Kako: objavljivanje server project

U ovom se odjeljku objašnjava objavite projekt pozadinskog iz Visual Studio .NET. Možete i implementirati pozadinskog projektu pomoću brojka ili neke druge metode u [dokumentaciji za implementaciju aplikacije servisa za Azure](../app-service-web/web-sites-deploy.md).

1. U Visual Studio, ponovno stvaranje projekta da biste vratili NuGet paketa.

2. U pregledniku rješenja, desnom tipkom miša kliknite projekt, kliknite **Objavi**. Objavite, prvo morate definirati objavljivanje profila. Kada već imate profil definirane, odaberite ga, a kliknite **Objavi**.

2. Ako se od vas zatraži da biste odabrali Objavi cilj, kliknite **Servis za aplikaciju Microsoft Azure** > **sljedeće**, a zatim (Ako je potrebno) prijavite se pomoću vjerodajnica za Azure. 
   Visual Studio preuzimanja i sigurno pohranjuje vaše postavke izravno iz Azure objavljivanja.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Odaberite **pretplatu**, odaberite **Vrstu resursa** **prikazu**, proširite **Mobilnu aplikaciju**i kliknite u pozadinskom mobilnu aplikaciju, a zatim kliknite **u redu**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Provjerite je li informacije o profilu Objavi, a zatim kliknite **Objavi**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Kada u pozadinskom mobilnu aplikaciju uspješno je objavljen, vidjet ćete odredišna stranica koji označava uspjeh.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Kako: definiranje kontroler tablice

Definiranje kontroler tablice da biste otkrili tablice SQL za mobilne klijente.  Konfiguriranje kontroler tablice zahtijeva tri koraka:

1. Stvaranje objekta prijenos podataka (DTO) predmete.
2. Konfiguriranje referenci tablice u predmete Mobile DbContext.
3. Stvaranje tablice kontroler.

S podacima za prijenos objekta (DTO) je obično C# objekt koji nasljeđuju od `EntityData`.  Ako, na primjer:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

Na DTO koristi se za definiranje tablice u SQL baze podataka.  Da biste stvorili unos u bazu podataka, dodajte je `DbSet<>` svojstvo DbContext koristite.  U zadani predložak projekta za Azure mobilne aplikacije, na DbContext naziva `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Ako imate instaliran SDK Azure, sada možete stvoriti tablicu kontroler predloška na sljedeći način:

1. Desnom tipkom miša kliknite mapu kontrolera, a zatim odaberite **Dodaj** > **kontroler...**.
2. Odaberite mogućnost **Kontroler tablice za Azure mobilne aplikacije** , a zatim kliknite **Dodaj**.
3. U dijaloškom okviru **Dodavanje kontrolera** :
    * Na padajućem popisu **Model predmete** odaberite svoje nove DTO.
    * Na padajućem popisu **DbContext** odaberite predmet DbContext mobilne usluge.
    * Naziv kontrolera stvara se za vas.
4. Kliknite **Dodaj**.

Project server brzi početak rada sadrži primjer jednostavne **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Kako: prilagođavanje veličine tablice broja stranica

Prema zadanim postavkama Azure mobilne aplikacije vraća 50 zapisa po zahtjev.  Broja stranica osigurava da klijent ne sve kopije niti njihova korisničkog Sučelja ni poslužitelja predugo, osiguravanje dobar doživljaja. Da biste promijenili veličinu tablice broja stranica, povećajte strani poslužitelja "dopuštene veličine upita" i stranice klijentsko veličinu na strani poslužitelja "dopuštene veličine upita" Prilagodi pomoću na `EnableQuery` atribut:

    [EnableQuery(PageSize = 500)]

Provjerite je li u PageSize isti ili veća od veličine zatražio klijent.  Pogledajte određenog klijenta NEMODERIRANA dokumentaciju za pojedinosti o promjeni veličine stranice za klijenta.

## <a name="how-to-define-a-custom-api-controller"></a>Kako: definiranje prilagođeni kontroler API-JA

Prilagođeni kontroler API pruža osnovne funkcije pozadinskog sustava mobilnu aplikaciju tako da će se krajnje točke. Možete registrirati kontroler API namijenjene mobilnoj pomoću atribut [MobileAppController]. Na `MobileAppController` atribut registrira smjer, postavlja serijalizatora JSON mobilne aplikacije i uključuje [provjere verzija klijenta](app-service-mobile-client-and-server-versioning.md).

1. U Visual Studio, desnom tipkom miša kliknite mapu kontrolera, a zatim kliknite **Dodaj** > **kontroler**odaberite **Web API 2 kontroler&mdash;prazan** i kliknite **Dodaj**.

2. Navedite **naziv kontroler**, kao što su `CustomController`, a zatim kliknite **Dodaj**.

3. U novu datoteku kontroler predmete, dodajte sljedeće pomoću naredbe:

        using Microsoft.Azure.Mobile.Server.Config;

4. Primjena atribut **[MobileAppController]** definicije predmete kontroler API kao u sljedećem primjeru:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. U datoteci App_Start/Startup.MobileApp.cs dodajte poziv za nastavak metodu **MapApiControllers** kao u sljedećem primjeru:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Možete koristiti u `UseDefaultConfiguration()` proširenje način umjesto `MapApiControllers()`. Bilo koji kontroler koji imaju **MobileAppControllerAttribute** primijeniti i dalje mogu pristupati klijentima, ali je možda neće biti ispravno troše klijente pomoću bilo kojeg klijenta za mobilnu aplikaciju SDK.

## <a name="how-to-work-with-authentication"></a>Kako: rad s provjerom autentičnosti

Azure mobilne aplikacije koristi provjera autentičnosti za aplikaciju servisa / autorizacije radi zaštite vaše mobilne pozadinskog.  U ovom se odjeljku objašnjava izvođenje sljedećih zadataka vezanih uz provjeru autentičnosti u projektu .NET pozadinskog poslužitelja:

+ [Kako: dodavanje provjere autentičnosti u programu project server](#add-auth)
+ [Kako: korištenje prilagođene provjere autentičnosti za aplikaciju](#custom-auth)
+ [Kako: dohvaćanje autentičnost korisničkih podataka](#user-info)
+ [Kako: ograničavanje pristupa podacima za autoriziranih korisnika](#authorize)

### <a name="add-auth"></a>Kako: dodavanje provjere autentičnosti u programu project server

Možete dodati provjeru autentičnosti sustava project server proširivanje objekt **MobileAppConfiguration** i konfiguriranju OWIN proizvod. Kada instalirate paket [Microsoft.Azure.Mobile.Server.Quickstart] i pozvati proširenje metodu **UseDefaultConfiguration** , preskočite na 3.

1. U Visual Studio, instalirajte paket [Microsoft.Azure.Mobile.Server.Authentication] .

2. U datoteci Startup.cs projekt, dodajte sljedeći redak koda na početku **konfiguracije** metoda:

        app.UseAppServiceAuthentication(config);

    Proizvod komponente OWIN Provjeri valjanost tokena koje izdaje povezani pristupnik aplikacije servisa.

3. Dodavanje u `[Authorize]` atributa za bilo koju drugu kontroler način koji zahtijeva provjeru autentičnosti. 

Da biste saznali kako provjeriti autentičnost klijenata za vaše pozadinskog mobilne aplikacije, potražite u članku [Dodavanje provjere autentičnosti za aplikaciju](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Kako: korištenje prilagođene provjere autentičnosti za aplikaciju

Ako ne želite da biste koristili Neki davatelji provjere autentičnosti/autorizacije aplikacije servisa, možete implementirati sustav prijava. Instalirajte paket [Microsoft.Azure.Mobile.Server.Login] radi jednostavnijeg tokena generacije provjere autentičnosti.  Navedite vlastitog koda za provjeru valjanosti korisničke vjerodajnice. Na primjer, možda potvrdite protiv salted i hashed lozinke u bazi podataka. U primjeru u nastavku, u `isValidAssertion()` način (definirano negdje drugdje) je zadužen za sljedeće provjere.

Prilagođena provjera autentičnosti predstavljeni stvaranjem programa ApiController te će se `register` i `login` akcije. Klijent trebali biste koristiti prilagodbe korisničkog Sučelja da biste prikupili podatke od korisnika.  Informacije zatim šalje na API pomoću standardnih HTTP POST poziv. Kada poslužitelj Provjeri valjanost tipkom pridruživanju, token izdavanja pomoću na `AppServiceLoginHandler.CreateToken()` način.  **Neispravno** korištenje ApiController na `[MobileAppController]` atribut. 

Primjer `login` akcija:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

U prethodnom primjeru, LoginResult i LoginResultUser su serializable objekata koji će se odgovarajuća svojstva. Klijent očekuje odgovore prijavi se vraćaju kao objekti JSON obrasca:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

Na `AppServiceLoginHandler.CreateToken()` način obuhvaća _ciljne skupine_ i parametar je _izdavač_ . Oba od tih parametara su postavljena na URL korijensko računala, pomoću HTTPS shemu. Na sličan način postavite _secretKey_ se vrijednost aplikacija je prijava ključ. Distribucija potpisivanja ključ u klijentu kao koristi za dobiti tipke i oponašati korisnika. Možete nabaviti potpisivanja ključ dok smješten u aplikacije servisa za Upućivanjem na _web-mjesto\_provjere autentičnosti\_POTPISNIK\_KLJUČ_ varijablu okruženja. Ako je potrebno u lokalnom kontekst za ispravljanje pogrešaka, slijedite upute u odjeljku [lokalne ispravljanje pogrešaka s provjerom autentičnosti](#local-debug) za dohvaćanje tipku i pohraniti kao na postavku aplikacije.

Izdana token može obuhvaćati i ostali zahtjevi i datumom isteka.  Izdana token dodali, morate uključiti zahtjeva za predmet (**sub**).

Podržava standardnu klijent `loginAsync()` način tako da preopterećenje usmjeravanje provjere autentičnosti.  Ako klijent nazove `client.loginAsync('custom');` prijaviti, morate biti na usmjeravanje `/.auth/login/custom`.  Možete postaviti usmjeravanje za korištenje prilagođene provjere autentičnosti kontroler `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Korištenje na `loginAsync()` pristup osigurava svaki sljedeći poziv na servis je pridružen token za provjeru autentičnosti.

###<a name="user-info"></a>Kako: dohvaćanje autentičnost korisničkih podataka

Kada korisnik provjere autentičnosti putem aplikacije usluge možete pristupiti dodijeljeni korisnički ID i druge informacije u kodu pozadinskog .NET. Podaci o korisniku moguće je koristiti za upućivanje autorizacije odluka u pozadinski. Sljedeći kod dohvaća ID korisnika povezanog s zahtjev:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

SID proizlazi iz karakteristične za davatelja korisničkog ID-a i statična za dani korisnika i davatelj prijave.  SID je null za tokeni provjera autentičnosti nije valjana.

Aplikacije servisa omogućuje vam određene zahtjevima zatražite od davatelja usluga za prijavu. Dodatne informacije o korištenju davatelja identiteta SDK unijeti svaki davatelja identiteta.  Na primjer, možete koristiti grafikonu API servisa Facebook da biste saznali prijateljima.  Možete odrediti zahtjevima koje su zatražili u plohu davatelja usluge na portalu za Azure. Neke pritužbe potrebna dodatna konfiguracija pomoću davatelja identiteta.

Sljedeći kod pozive metodu proširenje **GetAppServiceIdentityAsync** da biste dobili vjerodajnice za prijavu, što obuhvaća token za pristup potrebne za izvršavanje zahtjeve u odnosu na grafikonu API servisa Facebook:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Dodavati pomoću naredbe za `System.Security.Principal` možete unijeti kućni metodu **GetAppServiceIdentityAsync** .

### <a name="authorize"></a>Kako: ograničavanje pristupa podacima za autoriziranih korisnika

U prethodnom odjeljku smo prikazivao kako dohvatiti korisnički ID korisnika čija je autentičnost provjerena. Možete ograničiti pristup s podacima i ostale resurse koji se temelji na tu vrijednost. Ako, na primjer, dodavanje stupca ID korisnika s tablicama i filtriranje rezultata upita prema korisničkog ID-a je jednostavan način za ograničavanje vraćenih podataka samo autoriziranih korisnika. Sljedeći kod vraća retke podataka samo kada SID usklađuje sljedeću vrijednost u stupcu ID korisnika u tablici TodoItem:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

Na `Query()` način vraća programa `IQueryable` koje možete upravljati tako da LINQ učiniti filtriranje.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Kako: dodavanje automatske obavijesti u programu project server

Dodati automatske obavijesti sustava project server proširivanje objekt **MobileAppConfiguration** i stvaranje klijent koncentratora obavijesti.

1. U Visual Studio, desnom tipkom miša kliknite server project i kliknite **Upravljanje NuGet paketa**, potražite `Microsoft.Azure.Mobile.Server.Notifications`, zatim kliknite **Instaliraj**. 

2. Ponovite ovaj korak da biste instalirali na `Microsoft.Azure.NotificationHubs` paket koji sadrži biblioteku klijent koncentratora obavijesti.

3. U App_Start/Startup.MobileApp.cs, i dodajte poziv za nastavak metodu **AddPushNotifications()** tijekom pokretanja:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Dodajte sljedeći kod koji stvara klijent koncentratora obavijesti:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Sada možete koristiti klijent koncentratora obavijesti da biste poslali automatske obavijesti registrirani uređaja. Dodatne informacije potražite u članku [Dodavanje automatske obavijesti u aplikaciju](app-service-mobile-ios-get-started-push.md). Da biste saznali više o koncentratora obavijesti, potražite u članku [Pregled koncentratora obavijesti](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Kako: Omogući ciljani automatske pomoću oznaka

Obavijesti koncentratora možete poslati ciljano obavijesti određene registracije pomoću oznaka. Nekoliko oznaka se stvaraju automatski:

* ID instalacije označava određeni uređaj.
* Id korisnika čija je autentičnost provjerena SID na temelju prepoznaje određenog korisnika.

ID instalacije možete pristupiti iz svojstvo **installationId** na **MobileServiceClient**.  Sljedeći primjer pokazuje kako koristiti instalacijski ID oznake da biste dodali određene instalacija u koncentratora obavijesti:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Sve oznake koji vam je dao klijent tijekom automatske obavijesti registracije zanemaruju se po pozadinski prilikom stvaranja instalaciju. Da biste omogućili klijenta za dodavanje oznaka u instalaciju, morate stvoriti prilagođene API koji dodaje oznake pomoću prethodnog uzorka. 

Potražite u članku [klijent dodati automatske obavijesti oznake] [ 5] u uzorku dovršene brzi početak rada aplikacija mobilne usluge programa, primjerice.

##<a name="push-user"></a>Kako: Pošalji automatske obavijesti čija je autentičnost provjerena korisniku

Kada korisnik sustava čija je autentičnost provjerena registrira za slanje obavijesti, korisnički ID oznake automatski se dodaju za registraciju. Pomoću ove oznake automatske obavijesti možete poslati svim uređajima registrirane toj osobi. Sljedeći kod dobiva SID korisnika upućivanje zahtjeva i šalje automatske obavijesti predložak svaki Registracija uređaja te osobe:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Kada se registrirate za slanje obavijesti putem čija je autentičnost provjerena klijenta, provjerite je li autentičnosti dovršetka prije registracije. Dodatne informacije potražite u članku [automatske korisnicima] [ 6] u uzorku aplikacija mobilne usluge dovršene brzi početak rada za .NET pozadinskog.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Kako: ispravljanje pogrešaka i otklanjanje poteškoća s .NET Server SDK

Aplikacije servisa za Azure nudi nekoliko ispravljanje pogrešaka i otklanjanjem ASP.NET aplikacija:

- [Nadzor Azure aplikacije servisa](../app-service-web/web-sites-monitor.md)
- [Uključite Zapisivanje dijagnostičkih podataka u aplikacije servisa za Azure](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Otklanjanje poteškoća s Azure aplikacije servisa u Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Bilježenje u zapisnik

Pomoću standardnih pisanje praćenje ASP.NET možete napisati aplikacije servisa za dijagnostičke zapisnike. Prije nego što možete napisati u zapisnicima, morate omogućiti Dijagnostika u vašem pozadinskog mobilnu aplikaciju.

Da biste omogućili dijagnostika i pisanje zapisnike:

1. Slijedite korake u [kako omogućiti i dijagnostiku](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Dodajte sljedeće pomoću izjave u datoteci kod:

        using System.Web.Http.Tracing;

3. Stvaranje praćenje writer pisati iz pozadine .NET u zapisnicima dijagnostičkih podataka, na sljedeći način:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Ponovno objaviti sustava project server, a pristupiti pozadinskog mobilnu aplikaciju za izvršavanje put kod s zapisivanje.

5. Preuzimanje i procjenu zapisnike, kao što je opisano u [Kako: preuzimanje zapisnika](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Lokalni ispravljanje pogrešaka s provjerom autentičnosti

Možete pokrenuti aplikacije lokalno da biste testirali promjene prije nego što ih objavite u oblak. Za većinu pozadinski sustav Azure mobilne aplikacije, pritisnite *F5* u Visual Studio. No postoje neke dodatne okolnosti pri korištenju provjere autentičnosti.

Mora imati oblaku mobilne aplikacije s aplikacije usluge provjere autentičnosti/autorizacije konfigurirana i klijent moraju imati krajnju točku oblaka naveden kao glavno računalo za zamjenski prijava. Potražite u dokumentaciji za svoju platformu klijenta za određene korake potrebne.

Provjerite imaju li vaš mobilni pozadinskog [Microsoft.Azure.Mobile.Server.Authentication] instaliran. Nakon toga u vaše aplikacije OWIN početnu klasu dodajte sljedeće nakon `MobileAppConfiguration` primijenjen na `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

U prethodnom primjeru, trebali biste konfigurirati postavke aplikacije _authAudience_ i _authIssuer_ unutar Web.config datoteke za svaki se URL korijensko računala, pomoću HTTPS shemu. Na sličan način postavite _authSigningKey_ se vrijednost aplikacija je prijava ključ. Da biste dobili tipku potpisivanja:

1. Idite na aplikaciju unutar [portal za Azure] 
2. Kliknite **Alati**, **Kudu**, **otvorite**.
3. U web-mjesta Kudu Upravljanje kliknite **okruženju**.
4. Potražite vrijednost za _web-mjesto\_AUTH\_POTPISNIK\_KLJUČ_. 

Pomoću potpisivanja tipke za parametar _authSigningKey_ u datoteci config lokalnog računala.  Vaš mobilni pozadinskog sada zvučnu da biste provjerili valjanost tokeni kada se izvodi lokalno, koji se klijent dohvaća tokena iz krajnje točke utemeljene na oblaku.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Portal za Azure]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

