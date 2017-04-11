<properties 
    pageTitle="Aplikacije servisa za API aplikacije okidača | Microsoft Azure" 
    description="Kako implementirati okidača u aplikaciju API-JA u aplikacije servisa za Azure" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure okidača API aplikacije servisa za aplikaciju

>[AZURE.NOTE] Ovu verziju članka odnosi API aplikacije 2014. – 12-01 – pregled shema verziju.


## <a name="overview"></a>Pregled

U ovom se članku objašnjava kako implementirati okidača aplikacije API-JA i trošiti ih logike aplikacije.

Sve koda u ovoj temi se kopiraju iz [aplikacije API FileWatcher uzorak koda](http://go.microsoft.com/fwlink/?LinkId=534802). 

Imajte na umu da ćete morati preuzeti sljedeće nuget paket za kod u ovom se članku Stvaranje i pokretanje: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Što su okidača aplikacije API-JA?

To je uobičajeni scenarij za aplikaciju API početka događaja tako da klijenti aplikacije API-JA možete poduzeti odgovarajuće akcije kao odgovor na događaj. Mehanizam REST API-JA koji se temelji na koji podržavaju taj scenarij naziva aplikacije okidača API-JA. 

Ako, na primjer, pretpostavimo da kod klijent koristi [na Twitteru API poveznik aplikacija](../app-service-logic/app-service-logic-connector-twitter.md) , a kod potrebne za izvođenje akcija na temelju nove tweets sadrže određene riječi. U tom slučaju možete postaviti anketu ili automatske okidača da biste olakšali ta potreba.

## <a name="poll-trigger-versus-push-trigger"></a>Pokretanje ankete i automatske okidača

Trenutno podržani su dvije vrste okidača:

- Pokretanje ankete - klijent polls aplikaciju API-JA za obavijest o događaju pojavljuju je pokrenuta 
- Automatske okidača - klijent obavijesti aplikacija API kada aktivira događaja 

### <a name="poll-trigger"></a>Ankete okidača

Ankete okidača se implementira kao obični REST API-JA i očekuje svojim klijentima (kao što su logike aplikacija) provjeriti da biste dobili obavijest. Dok klijent možda zadržali stanje, ankete okidača sam je bez praćenja stanja. 

Sljedeće informacije o pakete zahtjeva i odgovora objašnjavaju neke ključne aspekte okidača ugovora anketu:

- Zahtjev
    - HTTP metoda: početak
    - Parametri
        - triggerState - ova neobavezan parametar omogućuje klijentima da biste odredili njihovo stanje tako da se okidača ankete pravilno možete odlučiti želite li da biste se vratili obavijesti ili ne temelji na navedenom stanju.
        - Parametri specifične za API-JA
- Odgovor
    - Šifra stanja **200** – zahtjev je valjan i nema obavijesti iz okidača. Sadržaj obavijesti o bit će tijelo odgovor. Zaglavlje "Pokušaj nakon" u odgovoru označava dodatne obavijesti podataka morate moguće dohvatiti s pozivom zahtjev za kasnije.
    - Šifra stanja **202** - zahtjev je valjan, ali postoji bez obavijest o novoj iz okidača.
    - Šifra stanja **4xx** – zahtjev nije valjan. Klijent treba ponoviti zahtjev.
    - Šifra stanja **5xx** – zahtjev je rezultirala se interna pogreška poslužitelja i/ili privremeni problem. Klijent treba ponoviti zahtjev.

Sljedeći isječak koda je primjera kako implementirati okidač anketu.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Da biste testirali ovog okidača ankete, slijedite ove korake:

1. Implementacija aplikacije API-JA u postavci provjere autentičnosti **javno anonimni**.
2. Nazovite operacije **dodirnite** dodir datoteke. Sljedeća slika prikazuje uzorak zahtjeva putem Postman.
   ![Operacija dodirom poziva putem Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Nazovite okidača ankete s parametrom **triggerState** postavite vremensku oznaku prije no što korak #2. Sljedeća slika prikazuje uzorak zahtjeva putem Postman.
   ![Pokretanje ankete poziva putem Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Automatske okidača

Automatske okidača je implementirana kao u pravilnim REST API-JA koji ih gura obavijesti o klijentima koje ste registrirali ste da biste primili obavijest kada određene događaje pokreću.

Sljedeće informacije o pakete zahtjeva i odgovora prikazuju neke ključne aspekte automatske okidača ugovora.

- Zahtjev
    - HTTP metoda: postavljanje
    - Parametri
        - triggerId: potrebna - neprozirne niz (kao što su GUID) koji predstavlja Registracija automatske okidača.
        - callbackUrl: potrebna - URL povratnog pozvati kada aktivira se događaj. Pozivanje je jednostavno objavu HTTP poziv.
        - Parametri specifične za API-JA
- Odgovor
    - Status kod **200** – zahtjev registrirati klijentski uspješno.
    - Šifra stanja **4xx** – zahtjev nije valjan. Klijent treba ponoviti zahtjev.
    - Šifra stanja **5xx** – zahtjev je rezultirala se interna pogreška poslužitelja i/ili privremeni problem. Klijent treba ponoviti zahtjev.
- Povratnog
    - HTTP metoda: objavu
    - Zahtjev za tijelo: sadržaj obavijesti.

Sljedeći isječak koda je primjera kako implementirati automatske okidača:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Da biste testirali ovog okidača ankete, slijedite ove korake:

1. Implementacija aplikacije API-JA u postavci provjere autentičnosti **javno anonimni**.
2. Pronađite [http://requestb.in/](http://requestb.in/) da biste stvorili RequestBin koja će služiti kao povratnog URL-a.
3. Nazovite okidača automatske s GUID kao **triggerId** i URL RequestBin kao **callbackUrl**.
   ![Pokretanje automatske poziva putem Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Nazovite operacije **dodirnite** dodir datoteke. Sljedeća slika prikazuje uzorak zahtjeva putem Postman.
   ![Operacija dodirom poziva putem Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Provjerite RequestBin da biste potvrdili da se automatske okidača povratnog poziva s svojstvo izlaz.
   ![Pokretanje ankete poziva putem Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Opišite okidača u definiciji API-JA

Nakon implementacije u okidača i implementacija aplikacije API-JA za Azure, dođite do plohu **Definicija API -JA** na portalu za Azure pretpregled i vidjet ćete da se automatski okidača prepoznaju u korisničkom Sučelju koje pokreću Swagger 2.0 API definiciju aplikacije API-JA.

![Plohu definicija API-JA](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Ako kliknite gumb **Preuzmi Swagger** i otvorite datoteku JSON, prikazat će vam se rezultati sličnu ovoj:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

Svojstvo proširenje **x-ms-schedular-okidača** je na način opisan u definiciji API okidača i automatski se dodaju pristupnik za aplikaciju API kada zatražite definiciju API putem pristupnika ako zahtjev za jednu od sljedećih kriterija. (Možete dodati svojstvo koje je potrebno ručno.)

- Ankete okidača
    - Ako je način HTTP **DOBITI**.
    - Ako je svojstvo **operationId** sadrži niz **okidača**.
    - Ako je svojstvo **parametara** sadrži parametar s **nazivom** svojstvo postavljeno na **triggerState**.
- Automatske okidača
    - Ako je način HTTP **STAVITI**.
    - Ako je svojstvo **operationId** sadrži niz **okidača**.
    - Ako je svojstvo **parametara** sadrži parametar s **nazivom** svojstvo postavljeno na **triggerId**.

## <a name="use-api-app-triggers-in-logic-apps"></a>Uporaba okidača aplikacije API-JA u aplikacijama logike

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Popis i konfiguriranje okidača API aplikacije u dizajneru logike aplikacije

Ako stvorite aplikaciju logike u istoj grupi resursa aplikacija API-JA, moći da biste ga dodali dizajnera crtanja jednostavno tako da ga kliknete. Sljedeća slika to prikazuju:

![Okidača u dizajneru logike aplikacije](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfiguriranje okidača ankete u dizajneru logike aplikacije](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfiguriranje automatske okidača u dizajneru logike aplikacije](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Optimiziranje API aplikacije okidača za logike aplikacije

Nakon što dodate okidača za aplikaciju API-JA, postoji nekoliko stvari koje možete učiniti da biste poboljšali iskustvo prilikom korištenja aplikacije API-JA u aplikaciji logike.

Ako, primjerice, parametar **triggerState** za anketu okidača potrebno postaviti na sljedeći izraz u aplikaciji logike. Ovaj izraz treba procijeniti posljednje pozivanje okidača iz aplikacije logiku i vratite tu vrijednost.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Napomena: Objašnjenje funkcija u izrazu iznad potražite u dokumentaciji na [Jezika za definiranje logike aplikacije tijeka rada](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Korisnici aplikacije logike morate unijeti izraz iznad za parametar **triggerState** tijekom korištenja okidača. Moguće je da bi sadržavao ovu vrijednost unaprijed pomoću dizajnera aplikacije logiku putem svojstvo proširenje **x-ms-raspored-preporuke**.  Svojstvo proširenje **x ms vidljivost** moguće je postaviti vrijednost *Interna* tako da se parametar sam ne prikazuju se na dizajnera.  Sljedeći isječak ilustrira koji.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Za okidača automatske **triggerId** parametar mora identificirati samo logike aplikacije. Najbolji preporučljivo je postavljanje tog svojstva za naziv tijeka rada pomoću sljedeći izraz:

    @workflow().name

Pomoću proširenje svojstva **x-ms-raspored-preporuke** i **x-ms-vidljivost** u njegov definiton API-JA, aplikaciju API-JA možete značenja Designer aplikacije logike automatski Postavi ovaj izraz za korisnika.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Proširenje svojstva dodatka ažurirane API-JA

Metapodatke – kao što su svojstva proširenje **x-ms-raspored-preporuke** i **x ms vidljivost** - u možete dodati i ažurirane API-JA na jedan od dva načina: statički ili dinamički.

Za statične metapodatke možete izravno uređivati datoteku */metadata/apiDefinition.swagger.json* u projektu i ručno dodati svojstva.

Za aplikacije API pomoću dinamičke metapodataka, možete uređivati datoteku SwaggerConfig.cs da biste dodali filtar za postupak koji možete dodati te datotečne nastavke.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Slijedi primjer kako se klase se implementirati da biste olakšali scenarij dinamički metapodataka.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
