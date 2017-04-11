<properties
    pageTitle="Izlazni oblik programa Azure Redis predmemorije, pomoću funkcija Azure, servisa Azure strujanje analize | Microsoft Azure"
    description="Saznajte kako koristiti funkciju Azure povezani Bus reda servisa, za popunjavanje programa Redis predmemorije Azure iz izlaza posao strujanje analize."
    keywords="Podaci strujanje predmemorije redis red čekanja bus servisu"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Upute za spremanje podataka iz Azure strujanje analize u programa Azure Redis predmemoriju pomoću funkcije Azure

Azure strujanje analize možete brzo razviti i implementacija rješenja najniža cijena da bi se dobio u stvarnom vremenu uvida s uređaja, senzori, Infrastruktura i aplikacije ili bilo koji niz podataka. Omogućuje različite slučajeva koristi kao što su u stvarnom vremenu upravljanje i nadzor, naredba i kontrola, prijevare otkrivanje, povezanog automobilima i mnogo više. U mnogim takvo situacijama, trebali biste pohranjujete podatke u spremištu raspodijeljeno podataka kao što su predmemoriju za Azure Redis outputted po Azure strujanje analize.

Pretpostavimo da su dio Telekomunikacije tvrtke. Pokušavate da biste otkrili SIM prijevare koju više pozive koji dolaze iz istog identiteta u isto vrijeme, ali u različite geografski mjesta. Su tasked s spremanje svih potencijalne lažno telefonske pozive u predmemoriji za Azure Redis. U ovom blogu smo navedene su upute o kako možete jednostavno Dovršavanje zadatka. 

## <a name="prerequisites"></a>Preduvjeti
Dovršavanje [u stvarnom vremenu prijevare otkrivanje] [ fraud-detection] walk-through za ASA

## <a name="architecture-overview"></a>Pregled arhitekture
![Snimka zaslona arhitekture](./media/stream-analytics-functions-redis/architecture-overview.png)

Kao što je prikazano na prethodnoj slici, strujanje Analytics omogućuje strujanje ulaznih podataka da biste mu i poslane na izlaz. Na temelju izlaz Azure funkcije možete zatim potaknuti neke vrste događaja. 

U ovom blogu smo fokus na dio funkcije Azure ovaj kanal ili preciznije pokretanje događaja koje pohranjuje lažno podatke u predmemoriju.
Nakon dovršetka postupka [u stvarnom vremenu prijevaru otkrivanje] [ fraud-detection] ćete praktičnom vodiču imate unos (događaj koncentrator), upita i na izlaz (blobova) već konfigurirali i pokretanje. U ovom blogu smo promijenite Izlaz namijenjenu reda Bus servisa. Nakon toga ne možemo povezati funkciji Azure ovaj red. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Stvaranje i povezivanje servisa Bus red Izlaz
Da biste stvorili reda Bus servisa, slijedite korake 1 i 2 u odjeljku .NET [Početak rada sa servisa Bus redovima][servicebus-getstarted].
Sada ćemo reda čekanja povezati strujanje analize posao koja je stvorena u starijoj walk-through otkrivanje prijevara.



1. Na portalu Azure otvorite plohu **izlaze** vašeg posla, a zatim odaberite **Dodaj** pri vrhu stranice.

    ![Dodavanje izlaze](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Odaberite **Red čekanja Bus servisu** kao **sita** , a zatim slijedite upute na zaslonu. Obavezno odaberite polje naziva reda čekanja Bus servis koji ste stvorili u [Početak rada sa servisa Bus redovima][servicebus-getstarted]. Kada završite, kliknite gumb "desno".
3. Navedite sljedeće vrijednosti:
    - **Oblikovanje serijalizatora događaj**: JSON
    - **Kodiranje**: UTF8
    - **OBLIK**: redak zarezom

4. Kliknite gumb **Stvori** da biste dodali izvoru i da biste provjerili možete li strujanje Analytics uspješno povezati s računom za pohranu.

5. Na kartici **Query** zamijenite trenutni upit sljedeće. Zamijenite *[Vaše SERVICE BUS NAME]* naziv izlazne koji ste stvorili u 3. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Stvaranje programa Azure Redis predmemorije
Stvorite predmemoriju za Azure Redis sljedeći odjeljak .NET u [upute za korištenje Azure Redis predmemorije] [ use-rediscache] dok je u odjeljku naziva ***Konfiguriranje predmemorije klijente***.
Kada završi, imate novu predmemoriju Redis. U odjeljku **sve postavke**odaberite **pristupnih tipki** i bilješke prema dolje ***primarni veza_niz***.

![Snimka zaslona arhitekture](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Stvaranje Azure (funkcija)
Slijedite [Stvaranje prve funkcija Azure] [ functions-getstarted] vodič za početak rada s Azure funkcije. Ako već imate funkciji Azure u koju želite koristiti, zatim prijeđite [Pisanje Redis predmemoriju](#Writing-to-Redis-Cache)

1. Na portalu odaberite usluge aplikacije u navigaciji s, a zatim kliknite naziv aplikacije Azure funkcija da biste pristupili funkcije aplikacije web-mjesta.
    ![Snimka zaslona popis funkcija servisa aplikacija](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Kliknite **novoj funkciji > ServiceBusQueueTrigger – C#**. Za sljedeća polja, slijedite ove upute:
    - **Naziv reda čekanja**: isti naziv kao i ime koje ste unijeli prilikom stvaranja reda čekanja u [Početak rada sa servisa Bus redovima] [ servicebus-getstarted] (ne naziv servisa bus). Provjerite jeste li koristili red koji je povezan s izlaz strujanje analize.
    - **Povezivanje servisa Bus**: odaberite **Dodaj niza za povezivanje**. Da biste pronašli niz za povezivanje, idite na klasični portal, odaberite **Servis Bus**, bus servis koji ste stvorili i **Podatke o VEZI** pri dnu zaslona. Provjerite jeste li na glavnom zaslonu na ovoj stranici. Kopirajte i zalijepite niz za povezivanje. Slobodno unesite naziv sve veze.
    
        ![Snimka zaslona veze bus usluge](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: odaberite **Upravljanje**


3. Kliknite **Stvori**

## <a name="writing-to-redis-cache"></a>Pisanje na Redis predmemorije
Sada stvorili smo Azure (funkcija) koji se čita iz reda Bus servisa. Sve što je preostalo je naš funkciju pisati ove podatke u predmemoriju Redis. 

1. Odaberite novostvorenu **ServiceBusQueueTrigger**pa kliknite **funkcija postavki aplikacije** u gornjem desnom kutu. Odaberite **idite na postavke servisa za aplikacije > Postavke > postavke aplikacije**

2. U odjeljku niza veze stvaranje naziva u odjeljku **naziv** . Zalijepite primarni veza_niz ste pronašli u drugom koraku **Stvori predmemoriju Redis** u odjeljku **vrijednosti** . Odaberite mjesto na kojem piše **Baze podataka SQL** **Prilagođeno** .

3. Kliknite **Spremi** pri vrhu.

    ![Snimka zaslona veze bus usluge](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Sada otvorite postavke servisa za aplikaciju i odaberite **Alati > aplikacije servisa Editor (pretpregled) > na > Idi**.

    ![Snimka zaslona veze bus usluge](./media/stream-analytics-functions-redis/app-service-editor.png)

5. U uređivaču po izboru, stvorite JSON datoteku pod nazivom **project.json** sa sljedećim i spremanje na lokalni disk.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Prenesite datoteku u korijenskom direktoriju funkcija (ne WWWROOT). Trebali biste vidjeti datoteku pod nazivom **project.lock.json** automatski se prikazuju, koja potvrđuje da je u Nuget paketi "StackExchange.Redis" i "Newtonsoft.Json" uvezete.

7. U datoteci **run.csx** zamijenite unaprijed generirane kod sljedeći kod. U funkciji lazyConnection zamijenite "VEZA naziv" naziv koji ste stvorili u korak 2 od **sprema podatke u predmemoriju Redis**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Pokretanje analize strujanje zadatka

1. Pokretanje aplikacije telcodatagen.exe. Korištenje je na sljedeći način:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Plohu strujanje analize posla, na portalu, kliknite **pokretanje** pri vrhu stranice.

    ![Snimka zaslona s početka posla](./media/stream-analytics-functions-redis/starting-job.png)

3. U na **pokretanje zadatka** plohu koji će se prikazati, odaberite **odmah** , a zatim kliknite gumb **Start** pri dnu zaslona. Status promjene posao početni i nakon neke promjene vrijeme pokretanje.
 
    ![Snimka zaslona s odabirom za vrijeme početka posla](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Pokrenite rješenja i provjerite rezultata
Odlazak natrag na stranicu **ServiceBusQueueTrigger** , trebali biste sada vidjeti izvješća zapisnika. Ti zapisnici prikaz dobili nešto od servisa Bus reda čekanja, stavite u bazu podataka, a dohvaćanja odgovor pomoću vrijeme kao ključ!

Da biste potvrdili da se podaci nalaze u predmemoriju Redis, idite na stranicu predmemorije Redis na novom portalu (kao što je prikazano u prethodnom koraku [Stvaranje programa Azure Redis predmemorije](#Create-an-Azure-Redis-Cache) ) i odaberite konzolu.

Sada možete napisati Redis naredbe da biste potvrdili zapravo su podaci u predmemoriju.

![Snimka zaslona s konzole za Redis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Daljnji koraci
Ispričavamo se Probuđeni što nove funkcije Azure i analize strujanje zajedno možete učiniti i Nadamo Time ćete otključati nove mogućnosti za vas. Ako imate povratne informacije na ono što želite dalje, slobodno koristiti [Azure UserVoice web-mjesta](https://feedback.azure.com/forums/270577-stream-analytics).

Ako ste novi Microsoft Azure, Pozivamo vas da biste isprobali neku tako da se prijavite za [besplatnu probnu račun za Azure](https://azure.microsoft.com/pricing/free-trial/). Ako ste novi korisnik strujanje analize, zatim Pozivamo vas da biste [stvorili prvi posla strujanje analize](stream-analytics-create-a-job.md).

Ako vam trebaju pomoć ili imate pitanja, objavu na [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) i [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forume. 

Možete vidjeti i u sljedećim resursima:

- [Azure funkcije reference za razvojne inženjere](../azure-functions/functions-reference.md)
- [Azure funkcije C# reference za razvojne inženjere](../azure-functions/functions-reference-csharp.md)
- [Azure funkcije F # reference za razvojne inženjere](../azure-functions/functions-reference-fsharp.md)
- [Azure NodeJS funkcije reference za razvojne inženjere](../azure-functions/functions-reference.md)
- [Azure okidača funkcije i poveznici](../azure-functions/functions-triggers-bindings.md)
- [Upute za praćenje predmemorije Redis Azure](../redis-cache/cache-how-to-monitor.md)

Da biste ostali u tijeku sve najnovije vijesti i značajke, slijedite [@AzureStreaming](https://twitter.com/AzureStreaming) na Twitteru.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
