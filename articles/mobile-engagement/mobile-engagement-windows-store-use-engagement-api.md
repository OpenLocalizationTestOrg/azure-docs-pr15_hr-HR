<properties 
    pageTitle="Upute za korištenje radnje API-JA na univerzalno za Windows" 
    description="Upute za korištenje radnje API-JA na univerzalno za Windows"            
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-universal"></a>Upute za korištenje radnje API-JA na univerzalno za Windows

Dokument je dodatak dokument [kako integrirati radnje na Windows univerzalni](mobile-engagement-windows-store-integrate-engagement.md): pruža dubine detalje o načinu korištenja API radnje da biste prijavili statistici aplikacije.

Imajte na umu da ako želite samo radnje da biste prijavili vaše aplikacije sesije, aktivnosti, ruši i tehničke informacije, zatim najjednostavnije je da biste sve svoje `Page` podređenu klase nasljeđuju od na `EngagementPage` predmete.

Ako želite dodatnu, na primjer ako morate aplikacije određene događaje, pogrešaka i zadacima, izvješća ili ako imate da biste prijavili aktivnosti vaše aplikacije na drugačiji način od onog implementirana u na `EngagementPage` klasa iz registra, zatim morate koristiti API radnje.

Nudi API radnje u `EngagementAgent` predmete. Možete pristupiti putem te načinima `EngagementAgent.Instance`.

Čak i ako modul agent nije pokrenut, svaki poziv na API je odgođena pa će se izvršavati ponovno kada agenta dostupna.

##<a name="engagement-concepts"></a>Pojmovi radnje

Sljedeći dijelovi suzite uobičajenih [Koncepata radnje Mobile](mobile-engagement-concepts.md) za Windows univerzalni platformu.

### <a name="session-and-activity"></a>`Session`i`Activity`

*Aktivnosti* obično je povezana s jedne stranice aplikacije, što znači *aktivnosti* se pokreće pri stranica se prikazuje i zaustavlja kad je zatvoren stranice: to je to slučaj kada SDK radnje integriran pomoću na `EngagementPage` predmete.

No *aktivnosti* i upravlja ručno pomoću API radnje. Omogućuje Podjela određene stranice u nekoliko dijelova sub da biste dobili dodatne informacije o korištenju ove stranice (na primjer Saznajte koliko često i koliko dijaloški okviri koriste se unutar Ova stranica).

##<a name="reporting-activities"></a>Izvješćivanje o aktivnosti

### <a name="user-starts-a-new-activity"></a>Korisnik pokrene novu aktivnost

#### <a name="reference"></a>Referenca

            void StartActivity(string name, Dictionary<object, object> extras = null)

Morate nazvati `StartActivity()` svaki put kada se promijeni aktivnosti korisnika. Prvi poziv Ova funkcija pokreće novu sesiju korisnika.

> [AZURE.IMPORTANT] SDK automatski poziva metodu EndActivity prilikom zatvaranja. Stoga preporučujemo da biste nazvali metodu StartActivity kad god se aktivnosti korisnika promjene i NIKAD pozvati metodu EndActivity Budući da nazovete podršku za ovu metodu navodi trenutna sesija da biste ga je prekinuti.

#### <a name="example"></a>Primjer

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Korisnik završava svoje trenutne aktivnosti

#### <a name="reference"></a>Referenca

            void EndActivity()

To je završava aktivnost i sesiju. Ne morate pozvati ovu metodu osim ako zaista znate što radite.

#### <a name="example"></a>Primjer

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Izvješćivanje o zadacima

### <a name="start-a-job"></a>Pokretanje zadatka

#### <a name="reference"></a>Referenca

            void StartJob(string name, Dictionary<object, object> extras = null)

Zadatak možete koristiti za praćenje zadataka certains vremenskom razdoblju.

#### <a name="example"></a>Primjer

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Završavanje zadatka

#### <a name="reference"></a>Referenca

            void EndJob(string name)

Čim prekinut je zadatak pratiti zadatak, nazovite metodu EndJob za ovaj zadatak, tako da unošenju podataka naziva zadatka.

#### <a name="example"></a>Primjer

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Izvješćivanje o događajima

Postoji tri vrste događaja:

-   Samostalne događaja
-   Sesije događaja
-   Događaji posla

### <a name="standalone-events"></a>Samostalne događaja

#### <a name="reference"></a>Referenca

            void SendEvent(string name, Dictionary<object, object> extras = null)

Samostalne događaja može se pojaviti izvan konteksta sesije.

#### <a name="example"></a>Primjer

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Sesije događaja

#### <a name="reference"></a>Referenca

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Sesije događaje obično koriste se za izvješća radnji koja korisnik svoj sesiji.

#### <a name="example"></a>Primjer

**Bez podataka:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**S podacima:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Događaji posla

#### <a name="reference"></a>Referenca

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Događaji zadatak obično koriste se za izvješća radnji koja korisnik tijekom posla.

#### <a name="example"></a>Primjer

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Izvješćivanje o pogreškama

Postoje tri vrste pogrešaka:

-   Samostalne pogreške
-   Sesije pogreške
-   Poslove

### <a name="standalone-errors"></a>Samostalne pogreške

#### <a name="reference"></a>Referenca

            void SendError(string name, Dictionary<object, object> extras = null)

Contrary to sesiju pogreške, samostalne pogreške se mogu pojaviti izvan konteksta sesije.

#### <a name="example"></a>Primjer

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Sesije pogreške

#### <a name="reference"></a>Referenca

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Sesije pogreške obično koriste se za pogreške koje utječu na korisnik svoj sesiji.

#### <a name="example"></a>Primjer

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Poslove

#### <a name="reference"></a>Referenca

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Pogreške se može povezati s izvodi posla umjesto koji se odnose na trenutnoj sesiji korisnika.

#### <a name="example"></a>Primjer

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Izvješćivanje o pogreškama ruši

Agenta nudi baviti ruši na dva načina.

### <a name="send-an-exception"></a>Slanje iznimku

#### <a name="reference"></a>Referenca

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Primjer

Iznimke u bilo kojem trenutku možete poslati tako da nazovete:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Da biste prekinuli sesiju radnje u isto vrijeme od slanja na pad sustava možete koristiti i neobavezan parametar. Da biste to učinili, nazovite:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Ako to učinite, sesije i zadacima zatvorit će se tek nakon slanja na pad sustava.

### <a name="send-an-unhandled-exception"></a>Slanje neobrađenu iznimku

#### <a name="reference"></a>Referenca

            void SendCrash(Exception e)

Radnje omogućuje način da biste poslali neobrađenu iznimke ako imate **ONEMOGUĆENE** radnje automatsko **rušenje** izvješćivanje. To je posebno korisno kada se koristi u aplikaciji UnhandledException rukovatelj događajima.

Ova će metoda **UVIJEK** prekid sesije radnje i zadacima nakon koja se poziva.

#### <a name="example"></a>Primjer

Možete je koristiti za implementaciju vlastite rukovatelj UnhandledExceptionEventArgs. Na primjer, dodati u `Current_UnhandledException` način u `App.xaml.cs` datoteke:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

U App.xaml.cs u "Javno App() {}" dodati:

            Application.Current.UnhandledException += Current_UnhandledException;

##<a name="device-id"></a>Id uređaja

            String EngagementAgent.Instance.GetDeviceId()

Id uređaja radnje možete pristupiti tako da nazovete ovu metodu.

##<a name="extras-parameters"></a>Parametri dodataka

Proizvoljne podataka mogu priložiti događaja, pogreška, aktivnost ili posao. Ove podatke možete strukturiran pomoću rječnika. Ključeva i vrijednosti može biti bilo koje vrste.

Dodaci podaci su serijalizirani tako da ako želite da biste umetnuli utemeljiti novu vrstu u dodataka morate dodati ugovor za tu vrstu podataka.

### <a name="example"></a>Primjer

Ćemo stvoriti novi klasa "Osobe".

            using System.Runtime.Serialization;
            
            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Nakon toga dodat ćemo u `Person` instance programa extra.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Ako postavite druge vrste objekata, provjerite je li način njihova ToString() je implementirana za vraćanje Ljudski čitljiv niza.

### <a name="limits"></a>Ograničenja

#### <a name="keys"></a>Tipke

Svaki ključ u objektu moraju se podudarati običnog sljedeći izraz:

`^[a-zA-Z][a-zA-Z_0-9]*$`

To znači da tipke mora počinjati barem jedno slovo, nakon čega slijedi slova, brojki ili podvlaka (\_).

#### <a name="size"></a>Veličina

Dodaci ograničeni su na **1024** znakova po poziv.

##<a name="reporting-application-information"></a>Informacije o aplikaciji za izvješćivanje o pogreškama

### <a name="reference"></a>Referenca

            void SendAppInfo(Dictionary<object, object> appInfos)

Možete je prijaviti ručno praćenje informacija (ili bilo koje druge aplikacije određene informacije) pomoću na SendAppInfo() (opis funkcije).

Imajte na umu da se ti podaci mogu poslati postupno: samo najkasnija vrijednost za dani ključ će biti zadržane za zadani uređaj. Kao što su dodaci događaj koristiti rječnik\<objekta, objekt\> priložiti podataka.

### <a name="example"></a>Primjer

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Ograničenja

#### <a name="keys"></a>Tipke

Svaki ključ u objektu moraju se podudarati običnog sljedeći izraz:

`^[a-zA-Z][a-zA-Z_0-9]*$`

To znači da tipke mora počinjati barem jedno slovo, nakon čega slijedi slova, brojki ili podvlaka (\_).

#### <a name="size"></a>Veličina

Informacije o aplikaciji ograničeno je na **1024** znakova po poziv.

U prethodnom primjeru, JSON poslane na poslužitelj je 44 znakova:

            {"birthdate":"1983-12-07","gender":"female"}

##<a name="logging"></a>Bilježenje u zapisnik
###<a name="enable-logging"></a>Omogućite zapisivanje

Čime se dobiva test zapisnika na konzoli IDE moguće je konfigurirati SDK-a.
Po zadanom su aktivirane te zapisnika. Da biste prilagodili, ažurirali svojstvo `EngagementAgent.Instance.TestLogEnabled` na jednu vrijednost dostupna na `EngagementTestLogLevel` Enumeracije, na primjer:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
 
