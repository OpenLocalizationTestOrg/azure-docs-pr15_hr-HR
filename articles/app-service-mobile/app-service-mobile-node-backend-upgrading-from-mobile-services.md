<properties
    pageTitle="Nadogradnja s mobilnim servisa na aplikacije servisa Azure - Node.js"
    description="Saznajte kako jednostavno nadograditi aplikaciju mobilnih usluga mobilne aplikacije servisa aplikacija"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Nadogradite postojeći servis Node.js Azure mobilne aplikacije servisa

Mobilne aplikacije servisa za je novi način da biste sastavili mobilne aplikacije koje se koriste Microsoft Azure. Da biste saznali više, pročitajte članak [koje su mobilne aplikacije?].

U ovoj se temi opisuje kako nadograditi postojeće Node.js pozadinskog aplikacije servisa Azure Mobile novih aplikacija mobilne aplikacije servisa. Tijekom izvođenja te nadogradnje, postojeće usluga mobilne aplikacije možete nastaviti raditi.  Ako je potrebno nadograditi aplikaciju za pozadinskog Node.js odnose [nadogradnje servisa .NET Mobile](./app-service-mobile-net-upgrading-from-mobile-services.md).

Kada se mobilni pozadinskog će se nadograditi aplikacije servisa za Azure, ima pristup svim značajkama aplikacije servisa za i naplatu prema [aplikacije servisa za cijene], cijene ne mobilne usluge.

## <a name="migrate-vs-upgrade"></a>Migriranje nasuprot nadogradnje

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Preporučuje se taj [Migracija](app-service-mobile-migrating-from-mobile-services.md) prije prolaze kroz nadogradnju. Na taj način možete staviti obje verzije aplikacije na istoj Plan za aplikaciju servisa i plaćati bez dodatnih troškova.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Poboljšanja u mobilne aplikacije Node.js server SDK

Nadogradnje na novu [Mobilne aplikacije SDK](https://www.npmjs.com/package/azure-mobile-apps) nudi mnogo poboljšanja, uključujući:

- Ovisno o tome [Express framework](http://expressjs.com/en/index.html), novi SDK čvor je osnovne i namijenjenu pratiti nove verzije čvor kao što su dođete. Možete prilagoditi ponašanje aplikacije s Express proizvod.

- Značajan poboljšali performanse u odnosu u usporedbi s Mobile Services SDK.

- Web-mjesto možete hostirati sada zajedno s mobilnim pozadinskog; Isto tako, jednostavno je da biste dodali Azure Mobile SDK bilo koje postojeće express.v4 aplikacije.

- U komponenti za različite platforme i lokalne razvoj SDK za mobilne aplikacije mogu biti razvio i izvodi lokalno platforme Windows, Linux i OSX. Sada je jednostavno je za korištenje uobičajenih čvor razvojnih tehnika kao što je pokrenut [Mocha](https://mochajs.org/) testova prije implementacije.

## <a name="overview"></a>Osnovni pregled nadogradnje

Da biste olakšali nadogradnje Node.js pozadinskog, aplikacije servisa za Azure razvio je paket za kompatibilnost.  Nakon nadogradnje, imat ćete niew web-mjesta na koje možete uvesti na novo web-mjesto aplikacije servisa.

Klijent mobilne usluge SDK-ovi su **nije** kompatibilna s novi poslužitelj SDK za mobilne aplikacije. Da bi se omogućuje continuity pružanja usluge za aplikaciju, trebali biste ne objavite promjene na web-mjesto trenutno posluživanje objavljene klijente. Umjesto toga, stvorite novi mobilne aplikacije koje služi kao duplikat. Ovu aplikaciju možete smjestiti na isti aplikacije servisa za plan da biste izbjegli povećavanja dodatni trošak financijske.

Imat ćete dvije verzije aplikacije: aplikacija koji će ostati isto i služi objavljenim u članku koji, i drugi koji zatim možete nadograditi i cilja sa novo izdanje klijenta. Možete premjestiti i testirati kod vašeg tempom, ali morate biti sigurni da sve popravci programskih pogrešaka unesete se primjenjuje se na oba. Nakon što mislite da željeni broj klijentskih aplikacija koji ste ažurirali na najnoviju verziju, možete izbrisati izvorne migriranim aplikacije ako po volji. Ako smješten u istu tarifu aplikacije servisa za kao mobilnu aplikaciju ga ne uzrokovati sve dodatne novčani troškove.

Potpuna strukture za postupak nadogradnje je na sljedeći način:

1. Preuzmite vaše postojeće (migriranim) Azure mobilne usluge.
2. Mobilne aplikacije Azure pomoću paket za kompatibilnost pretvorili u projekt.
3. Ispravite sve razlike (kao što su postavke provjere autentičnosti).
4. Implementacija pretvorene projekta Azure mobilnu aplikaciju nove aplikacije usluge.
4. Pustite novu verziju klijentska aplikacija koje koriste nove mobilne aplikacije.
5. (Neobavezno) Izbrišite izvornu aplikacije migriranim servis za mobilne uređaje.

Brisanje se može dogoditi ako ne vidite sve promet na vaše izvorne migriranim servis za mobilne uređaje.

## <a name="install-npm-package"></a>Instalacija prije requisites

[Čvor] instalirajte na lokalnom računalu.  I instalirajte paket za kompatibilnost.  Nakon instalacije čvor novi cmd ili odzivniku komponente PowerShell možete pokrenite sljedeću naredbu:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Nabavite skripte Azure mobilne usluge

- Prijavite se na [Portal za Azure].
- Koristite **Svih resursa** ili **Aplikacije servisa**, pronaći vaše web-mjesto za mobilne usluge.
- Unutar web-mjesta, kliknite **Alati** -> **Kudu** -> **otvorite** da biste otvorili Kudu web-mjesta.
- Kliknite na **Konzoli za ispravljanje pogrešaka** -> **ljuske PowerShell** za otvaranje konzole za ispravljanje pogrešaka.
- Dođite do `site/wwwroot/App_Data/config` klikom na svakom direktorija u nizu
- Kliknite ikonu preuzimanja uz stavku u `scripts` direktorija.

To će se preuzeti skripte u obliku ZIP.  Stvorite novi direktorij na lokalnom računalu i otpakiravanje u `scripts.ZIP` datoteka u direktoriju.  Time ćete stvoriti na `scripts` direktorija.

## <a name="scaffold-app"></a>Scaffold novi pozadinskog Azure mobilne aplikacije

Iz imenika koji sadrži skripte direktorija, pokrenite sljedeću naredbu:

```scaffold-mobile-app scripts out```

To će stvoriti scaffolded pozadinskog Azure mobilne aplikacije u na `out` direktorija.  Iako nije potrebno je dobro provjeriti na `out` imeničkog u spremištu kod izvora po izboru.

## <a name="deploy-ama-app"></a>Implementacija sustava pozadinskog Azure mobilne aplikacije

Tijekom implementacije, morat ćete učiniti sljedeće:

1. Stvorite novu aplikaciju Mobile [Azure Portal].
2. Pokretanje u `createViews.sql` skripte na povezanim baze podataka.
3. Povežite bazu podataka koja je povezana s mobilne usluge vašem novom servisu aplikacije.
4. Ostali resursi (kao što su obavijesti koncentratora) veza sa servisom nove aplikacije.
5. Implementacija stvoreni kod novo web-mjesto.

### <a name="create-a-new-mobile-app"></a>Stvorite novu aplikaciju Mobile

1. Prijavite se na [Portal za Azure].

2. Kliknite **+ NOVO** > **Web + Mobile** > **Mobilnu aplikaciju**, navedite naziv vaše pozadinskog mobilnu aplikaciju.

3. **Grupa resursa**, odaberite postojeću grupu resursa ili stvorite novi (pomoću isti naziv kao aplikacijom.) 
 
    Možete odabrati neku drugu tarifu za aplikacije servisa ili stvorite novi. Dodatne informacije o aplikaciji servisa tarife i kako stvoriti novi plan u različitim cijene sloju i u željenom mjestu potražite u članku [Pregled za detaljnije tarife aplikacije servisa za Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Aplikacije servisa za planiranje**, odabire se zadani plan (u [standardni sloju](https://azure.microsoft.com/pricing/details/app-service/)). Možete odabrati i neku drugu tarifu, ili [stvorite novi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Aplikacije servisa za planiranje postavki određuju [mjesto, značajke, cijena i resursi za izračun](https://azure.microsoft.com/pricing/details/app-service/) povezan s vašom aplikacijom. 

    Kada odlučite u planu, kliknite **Stvori**. Time se stvara pozadinskog mobilnu aplikaciju. 


### <a name="run-createviewssql"></a>Pokretanje CreateViews.SQL

Aplikaciju scaffolded sadrži datoteku s nazivom `createViews.sql`.  Ova skripta mora izvršiti protiv ciljna baza podataka.  Niz za povezivanje za ciljnu bazu podataka možete dobivenog migriranim mobilne usluge iz plohu **Postavke** u odjeljku **Nizove veze**.  To se naziva `MS_TableConnectionString`.

Možete pokrenuti skriptu iz programa SQL Server Management Studio ili Visual Studio.

### <a name="link-the-database-to-your-app-service"></a>Povezivanje baze podataka aplikacije servisa

Veza na postojeću bazu podataka aplikacije servisa:

- [Portal za Azure]otvorite aplikacije servisa.
- Odaberite **sve postavke** -> **podatkovne veze**.
- Kliknite **Dodaj**.
- Na padajućem popisu odaberite **SQL baze podataka**
- U odjeljku **SQL baze podataka**odaberite postojeću bazu podataka, a zatim kliknite **Odabir**.
- U odjeljku **niz za povezivanje**, unesite korisničko ime i lozinku za bazu podataka, a zatim kliknite u **redu**.
- U plohu **Dodavanje podatkovne veze** , kliknite **u redu**.

Korisničko ime i lozinku možete pronaći pregledom niz za povezivanje za ciljna baza podataka u migriranim mobilne usluge.


### <a name="set-up-authentication"></a>Postavljanje provjere autentičnosti

Azure aplikacije Mobile omogućuje vam da biste konfigurirali Azure Active Directory, Facebook, Google, Microsoft i Twitter provjere autentičnosti unutar servisa.  Prilagođene provjere autentičnosti ćete morati zasebno razvio.  Pogledajte dokumentaciju [Koncepata provjeru autentičnosti] i [Provjeru autentičnosti brzi početak rada] u dokumentaciji dodatne informacije.  

## <a name="updating-clients"></a>Mobilni klijenti za ažuriranje

Nakon što dodate radu pozadinskog za mobilnu aplikaciju, možete raditi na novu verziju klijentsku aplikaciju koja se koristi. Mobilne aplikacije sadrži i nova verzija klijenta SDK-ovi i slično nadogradnje poslužitelja iznad, morat ćete ukloniti sve reference mobilne usluge SDK-ovi prije instalacije verzije aplikacije Mobile.

Jedna od glavne razlike između verzija je na constructors više nije potreban aplikacijsku tipku. Sada jednostavno proslijedite u URL-u mobilnu aplikaciju. Na primjer, u klijentima .NET na `MobileServiceClient` Graditelj sada je:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Možete pročitati o instaliranju novi SDK-ovi i korištenju novu strukturu putem veze u nastavku:

- [Android verzije 2.2 ili noviji](app-service-mobile-android-how-to-use-client-library.md)
- [iOS verzije 3.0.0 ili novije verzije](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) verzije 2.0.0 ili novije verzije](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova 2.0 ili novija verzija](app-service-mobile-cordova-how-to-use-client-library.md)

Ako aplikacija omogućuje korištenje automatske obavijesti, zabilježite određene Registracija upute za svaku platforme kao što su neke promjene kao i.

Kada je nova verzija klijenta spreman, isprobajte prema projektu nadograđena poslužitelja. Nakon provjere radi li možete pustite novu verziju aplikacije klijentima. Naposljetku, kada korisnici stigli prima ažuriranja, možete izbrisati Services mobilne verzije aplikacije. Nakon što potpuno nadogradite na aplikacije aplikacije servisa Mobile pomoću najnovije poslužitelj SDK za mobilne aplikacije.

<!-- URLs. -->

[Portal za Azure]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Što su mobilne aplikacije?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Aplikacije servisa za cijene]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Provjera autentičnosti koncepti]: ../app-service/app-service-authentication-overview.md
[Brzi početak rada za provjeru autentičnosti]: app-service-mobile-auth.md

[Portal za Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
