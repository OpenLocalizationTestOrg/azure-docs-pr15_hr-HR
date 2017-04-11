<properties
   pageTitle="Topologija oluja Apache s Visual Studio i C# | Microsoft Azure"
   description="Saznajte kako stvoriti oluja topologija u C# tako da stvorite topologiju count jednostavne riječ u Visual Studio pomoću alata za HDInsight za Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Razvoj C# topologija za Apache oluja na HDInsight pomoću alata za Hadoop za Visual Studio

Saznajte kako pomoću alata za HDInsight za Visual Studio stvaranje C# oluja topologije. Pomoću ovog praktičnog vodiča vodi kroz postupak stvaranja novog projekta oluja u Visual Studio je lokalno i testiranje uvođenja programa oluja Apache na klasteru HDInsight.

Će Saznajte i kako stvoriti topologija hibridnog koji koriste C# i Java komponente.

> [AZURE.IMPORTANT] Dok je korake u ovom dokumentu za razvojno okruženje za Windows s Visual Studio, kompilirane projekta može poslati klaster Linux ili HDInsight utemeljen na sustavu Windows. Samo sustavom Linux klastere stvorene nakon 28/10/2016 podršku topologija SCP.NET.
>
> Da biste koristili C# topologije sa sustavom Linux klaster, morate ažurirati paket Microsoft.SCP.Net.SDK NuGet koristi projekta verziju 0.10.0.6 ili noviji. Verziju paketa mora odgovarati i glavna verzija oluja instalirana na HDInsight. Primjerice, koristite oluja u verzijama HDInsight 3,3 i 3.4 oluja verzija 0.10.x dok HDInsight 3.5 koristi oluja 1.0.x.
> 
> C# topologija na sustavom Linux klastere morate koristiti .NET 4,5 i koristiti Mono pokrenuti klaster HDInsight. Većina značajki funkcioniraju, no treba provjeriti [Kompatibilnost Mono](http://www.mono-project.com/docs/about-mono/compatibility/) dokument za potencijalne nekompatibilnosti.

## <a name="prerequisites"></a>Preduvjeti

- Jedan od sljedećih verzija programa Visual Studio

    - Visual Studio 2012 s [obnove 4](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 [ažuriranje 4](http://www.microsoft.com/download/details.aspx?id=44921) ili [Visual Studio 2013 zajednice](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 ili [Visual Studio 2015 zajednice](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 ili noviji

- Alati za HDInsight za Visual Studio: potražite u članku [Prvi koraci pri korištenju HDInsight alate za Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) instaliranja i konfiguriranja HDInsight alate za Visual Studio.

    > [AZURE.NOTE] Alati za HDInsight za Visual Studio nisu podržane na Visual Studio Express

-   Apache oluja na HDInsight klaster: potražite u članku [Uvod u rad s Apache oluja na HDInsight](hdinsight-apache-storm-tutorial-get-started.md) korake da biste stvorili klaster.

## <a name="templates"></a>Predlošci

Alati za HDInsight za Visual Studio navedite sljedeće predloške::

| Vrsta projekta | Pokazuje |
| ------------ | ------------- |
| Oluja aplikacije | Prazni oluja topologije projekt |
| Oluja Azure SQL Writer uzorka | Kako napisati s bazom podataka SQL Azure |
| Ogledna oluja DocumentDB Reader | Kako pročitati iz Azure DocumentDB |
| Ogledna DocumentDB Writer oluja | Kako napisati Azure DocumentDB |
| Ogledna oluja EventHub Reader | Kako pročitati iz koncentratora Azure događaja |
| Ogledna EventHub Writer oluja | Kako napisati Azure događaj koncentratora |
| Ogledna čitač HBase oluja | Kako pročitati iz HBase na HDInsight klaster |
| Ogledna HBase Writer oluja | Kako HDInsight pisali HBase klaster |
| Ogledna hibridnog oluja | Kako koristiti komponentu Java |
| Ogledna oluja | Count topologiju osnovni programa word |

> [AZURE.NOTE] HBase uzoraka čitanje i pisanje pomoću HBase REST API-JA možete komunicirati s programa HBase na HDInsight klaster, ne HBase API Java.

U koracima u ovom dokumentu, da biste stvorili novu topologiju koristit ćete osnovni vrsta projekta oluja aplikacije.

## <a name="create-a-c-topology"></a>Stvorite topologiju C#

1. Ako već niste instalirali najnoviju verziju HDInsight alate za Visual Studio, potražite u članku [Prvi koraci pri korištenju HDInsight alate za Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Otvorite Visual Studio, odaberite **datoteku** > **Novo**, a zatim **projekta**.

3. Na zaslonu **Novi projekt** proširite **instalirani** > **predložaka**i odaberite **HDInsight**. Popis predložaka, odaberite **Oluja aplikacije**. Pri dnu zaslona, unesite **WordCount** kao naziv aplikacije.

    ![Slika](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Nakon stvaranja projekta imate sljedeće datoteke:

    - **Program.CS**: to definira topologija projekta. Imajte na umu po zadanom stvaraju zadane topologije koji se sastoji od jednog spout i jedan munje.

    - **Spout.CS**: na primjer spout koji emits slučajne brojeve.

    - **Bolt.CS**: na primjer munje koji zadržava ukupan broj brojeva čuje tako da na spout.

    Kao dio stvaranjem projekta, najnovije [pakete SCP.NET](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) će se preuzeti iz NuGet.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

U sljedećim odjeljcima će promijeniti taj projekt u osnovni WordCount aplikaciju.

### <a name="implement-the-spout"></a>Implementacija u spout

1. Otvorite **Spout.cs**. Spouts koriste se za čitanje podataka u topologije iz vanjskog izvora. Glavne komponente za na spout su:

    - **NextTuple**: pozove oluja kada je dopušteno u spout šalji novi redni.

    - **Ack** (samo za topologije transakcijskih): rukuje acknowledgements pokrenuo drugih komponenti u topologiji za redni poslane s ovom spout. Acknowledging n-torke omogućuje spout znaju da ga je uspješno obrađen do komponente.

    - **Neće funkcionirati** (samo za topologije transakcijskih): rukuje redni koji su nije uspjelo obradu drugih komponenti u topologiji. To nudi mogućnost ponovno šalji torke tako da ga možete obrađuju ponovno.

2. Zamijenite sadržaj klase **Spout** sljedeće. Time se stvara spout koji slučajno emits rečenice u topologiji.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Potrajati čitati kroz komentare da biste shvatili funkcija kod.

### <a name="implement-the-bolts"></a>Implementacija u Vijci

1. Da biste izbrisali postojeću datoteku **Bolt.cs** iz projekta.

2. U **Pregledniku rješenja**, desnom tipkom miša kliknite projekt i odaberite **Dodaj** > **Nova stavka**. Na popisu odaberite **Oluja munje**pa unesite **Splitter.cs** kao naziv. Ponovite to da biste stvorili drugi bolt imenovani **Counter.cs**.

    - **Splitter.CS**: implementira munje koji dijeli rečenica na pojedinačne riječi i emits novi strujanje riječi.

    - **COUNTER.CS**: implementira munje koji broji svake riječi i emits novi strujanje riječi i broj za svaku riječ.

    > [AZURE.NOTE] Ove Vijci jednostavno čitanja i pisanja strujanja, ali vam može poslužiti u munje možete komunicirati s izvora kao što su baze podataka ili servis.

3. Otvorite **Splitter.cs**. Napomena da sadrži samo jedan način po zadanom: **izvršavanje**. To se naziva kada se munje primi n-torke za obradu. Ovdje možete pročitati obraditi dolazne redni i šalji izlaznog redni.

4. Zamijenite sadržaj predmete **razdjelnika** sljedeći kod:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Potrajati čitati kroz komentare da biste shvatili funkcija kod.

5. Otvorite **Counter.cs** i zamijenite sadržaj predmete sljedeće:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Potrajati čitati kroz komentare da biste shvatili funkcija kod.

### <a name="define-the-topology"></a>Definiranje topologije

Spouts i Vijci raspoređena su u grafikonu koji definira tok podataka između komponente. Za ovaj topologije grafikonu je kako slijedi:

![Slika raspored komponente](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Su rečenica čuje iz spout, u koji koji su distribuirati da bi pojavljivanja munje razdjelnika. Munje razdjelnika dijeli rečenica na riječi, čime se distribuirati u munje brojač.

Budući da riječi čuva se lokalnu instancu brojač, želimo da biste bili sigurni da se određene riječi tijek u istoj instanci brojač munje, pa imamo samo jedna instanca čuvanja gdje se nalaze određene riječi. No za munje razdjelnika zaista nije važno koju munje prima koje rečenice tako da ne možemo jednostavno želite učitati saldo rečenica kroz sve te instance.

Otvorite **Program.cs**. Važno metoda je **GetTopologyBuilder**, koji se koristi da biste definirali topologije koja je poslana na oluja. Zamijenite sadržaj **GetTopologyBuilder** sljedeći kod za implementaciju topologije prethodno opisane:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Potrajati koji trenutak da biste pročitali kroz komentare da biste shvatili funkcija kod.

## <a name="submit-the-topology"></a>Slanje topologije

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite projekt, a zatim odaberite **Pošalji da biste oluja na HDInsight**.

    > [AZURE.NOTE] Ako se to od vas zatraži, unesite vjerodajnice za prijavu za pretplatu Azure. Ako imate više pretplata, prijavite se u onu koja sadrži vaše oluja na klasteru HDInsight.

2. Odaberite vaše oluja na klasteru HDInsight s padajućeg popisa **Oluja klaster** , a zatim odaberite **Pošalji**. Možete nadzirati ako predavanje uspije pomoću u **izlaznom** prozoru.

3. Kada topologije uspješno je poslana, **Topologija oluja** za klaster prikazivati. Odaberite topologije **WordCount** s popisa da biste vidjeli informacije o topologiji izvodi.

    > [AZURE.NOTE] Možete pogledati i **Oluja topologija** iz **Programa Explorer poslužitelja**: proširivanje **Azure** > **HDInsight**, desnom tipkom miša kliknite oluja na HDInsight klaster, a zatim odaberite **Prikaz oluja topologija**.

    Pomoću veze za spouts ili Vijci za prikaz informacija o te komponente. Otvorit će se novi prozor za svaki odabrani.

4. U prikazu **Sažetka topologije** kliknite **Ukloni** da biste prestali topologije.

    > [AZURE.NOTE] Topologija oluja nastaviti s radom dok se ne mogu se deaktivirati i klaster je izbrisana.

## <a name="transactional-topology"></a>Transakcijskih topologije

Prethodni Topologija nije koje nisu transakcijskih. Komponente unutar topologiju implementirate sve funkcije za replaying poruke ne uspijete obrada komponenta u topologiji. Topologija transakcijskih primjer stvaranja novog projekta i odaberite **Uzorak oluja** kao vrsta projekta.

Topologija transakcijskih implementirati na sljedeći način podržava Ponovi podataka:

- **Predmemoriranje metapodataka**: U spout morate pohranjivati metapodatke o podacima čuje tako da se podaci mogu dohvatiti i ponovno čuje ako dođe do pogreške. Budući da podataka čuje po uzorku small, sirovim podacima za svaki n-torke pohranjuju se u rječnik za Ponovi.

- **Ack**: svaki munje u topologiji da biste uputili poziv `this.ctx.Ack(tuple)` da biste ack ima uspješno obrađuju n-torke. Kada sve Vijci acked n-torke, u `Ack` način u spout se poziva. Time se omogućuje spout da biste uklonili predmemoriranih podataka Ponovi jer potpuno obrade podatke.

- **Neuspjeh**: svaki munje da biste uputili poziv `this.ctx.Fail(tuple)` da biste naznačili da obrada nije uspjela za n-torke. Neuspjeh propagira da biste na `Fail` način spout, pomoću koje je moguće replayed torke predmemorirani metapodataka.

- **ID slijeda**: kada emitira n-torke možete navesti ID slijeda. To mora biti vrijednost koja određuje n-torke za obradu Ponovi (Ack i nije uspjelo). Ako, na primjer, spout u programu project **Oluja uzorka** koristi sljedeće kada emitira podataka:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    To emits novi n-torke koja sadrži rečenicu strujanje zadani s slijed ID vrijednost koja se nalazi u **lastSeqId**. U ovom primjeru **lastSeqId** jednostavno se povećava za svakih n-torke čuje.

Kao što je prikazano u programu project **Oluja uzorka** , je li komponenta transakcijskih mogu postaviti vrijeme izvođenja, ovisno o konfiguraciji.

## <a name="hybrid-topology"></a>Topologija hibridnog

Alati za HDInsight za Visual Studio je moguće koristiti i za stvaranje hibridnog topologija gdje neke komponente su C# i drugima Java.

Topologija hibridnog primjer stvoriti novi projekt, pa odaberite **Oluja hibridnog uzorka**. Time ste stvorili potpuno komentirane uzorak koji sadrži nekoliko topologija kojima se ilustrira sljedeće:

- **Java spout** i **C# munje**: definirano u **HybridTopology_javaSpout_csharpBolt**

    - Transakcijskih verziju definiranju u **HybridTopologyTx_javaSpout_csharpBolt**

- **C# spout** i **Java munje**: definirano u **HybridTopology_csharpSpout_javaBolt**

    - Transakcijskih verziju definiran u **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] Ova verzija pokazuje i kako koristiti kod Clojure iz tekstne datoteke kao Java komponentu.

Da biste se prebacivali između topologije koji se koristi prilikom slanja projekt, jednostavno premjestite na `[Active(true)]` izjava topologiji koji želite koristiti prije slanja klaster.

> [AZURE.NOTE] Datoteka jezika Java koji su potrebni su navedene kao dio taj projekt u mapi **JavaDependency** .

Imajte na umu sljedeće stvaranje i slanje hibridnog topologija:

- Da biste stvorili novu instancu klase jezika Java spout ili munje moraju se koristiti **JavaComponentConstructor** .

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** treba koristiti serijalizirati podataka u da biste i smanjivanje Java komponente iz objekata Java JSON.

- Prilikom slanja topologije na poslužitelj, morate koristiti mogućnost **dodatna konfiguracija** da biste odredili **putova datoteka jezika Java**. Navedeni put mora biti direktorij koji sadrži POSUDU datoteka koja sadrži vaše klase jezika Java.

### <a name="azure-event-hubs"></a>Azure događaj koncentratora

Verzija SCP.Net 0.9.4.203 predstavlja nove predmete i način posebno za rad s na događaj koncentrator Spout (u Java spout koji se čita iz centra za događaj.) Prilikom stvaranja topologije koja koristi ovaj spout, pomoću sljedećih načina:

- **EventHubSpoutConfig** klasa: stvara objekt koji sadrži konfiguraciju za komponentu spout

- **TopologyBuilder.SetEventHubSpout** metoda: dodaje komponentu događaj koncentrator Spout topologiji

> [AZURE.NOTE] Dok ih lakše raditi na događaj koncentrator Spout od drugih komponenti Java, morat ćete koristiti u CustomizedInteropJSONSerializer serijalizirati podataka koje je stvorio u spout.

## <a id="configurationmanager"></a>Korištenje ConfigurationManager

Ne koristite ConfigurationManager retreive konfiguracije vrijednosti iz munje i spout komponente; To će dovesti do nul-pokazivač iznimke. Umjesto toga konfiguraciju projekta prenosi u topologiji oluja u paru ključa vrijednosti u kontekstu topologije. Svaku komponentu koji se zasniva na konfiguracijskih vrijednosti mora njihovo dohvaćanje iz konteksta tijekom pokretanja.

Sljedeći kod pokazuje kako dohvatiti ove vrijednosti:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Ako koristite na `Get` način da biste se vratili instance komponente, morate osigurati ona prosljeđuje oba na `Context` i `Dictionary<string, Object>` parametre na Graditelj. Sljedeći primjer je u basic `Get` metode pravilno prosljeđuje te vrijednosti:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>Kako ažurirati SCP.NET

Nedavno korištenih izdanja SCP.NET podržava paket nadogradnje putem NuGet. Kada je novo ažuriranje dostupan, primit ćete obavijest o nadogradnji. Da biste ručno provjerili za nadogradnju, poduzmite ove korake:

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite projekt, a zatim odaberite **Upravljanje NuGet paketa**.

2. Upravitelj paketa odaberite **ažuriranja**. Ako je dostupno ažuriranje, on će biti naveden. Kliknite gumb **Ažuriraj** paketa da biste ga instalirali.

> [AZURE.IMPORTANT] Ako projekt stvorena je pomoću neke starije verzije SCP.NET koje niste koristili NuGet paketa ažuriranja sustava, morate poduzeti sljedeće korake da biste ažurirali novu verziju:
>
> 1. U **Pregledniku rješenja**, desnom tipkom miša kliknite projekt, a zatim odaberite **Upravljanje NuGet pakete**.
> 2. Koristite polje za **pretraživanje** , tražiti, a zatim dodajte **Microsoft.SCP.Net.SDK** u projekt.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a name="null-pointer-exceptions"></a>Null pokazivač iznimke

Prilikom korištenja topologiju C# sa sustavom Linux HDInsight klaster, munje i spout komponente koje koriste ConfigurationManager čitati konfiguracijske postavke prilikom izvođenja može vratiti nul-pokazivač iznimke. To se događa jer konfiguracija učitati domene nije iz skupa koji sadrži projekta.

Konfiguriranje projekta se prenosi u topologiji oluja u paru ključa vrijednosti u kontekstu topologije, a može biti dohvaćeni iz rječnika objekt koji se prenosi komponente kada ih se pokrenuti.

Sljedeći primjer prikazuje učitavanja konfiguracijskih vrijednosti iz konteksta topologije potražite u odjeljku [ConfigurationManager](#configurationmanager) ovog dokumenta.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Prilikom korištenja C# topologije sa sustavom Linux HDInsight klaster, koji se mogu pojaviti sljedeća pogreška:

    System.TypeLoadException: Failure has occurred while loading a type.

To obično occurrs prilikom korištenja binarne koji nije kompatibilan s verzijom .NET koji podržava mono.

Za klastere sustavom Linux HDInsight, provjerite je li da projekt koristi binarne datoteke za .NET 4,5 prevesti.


### <a name="test-a-topology-locally"></a>Testiranje topologije lokalno

Iako je lako za implementaciju topologije klaster, u nekim slučajevima možda ćete morati testiranje topologije lokalno. Poduzmite sljedeće korake da biste pokrenuli i testiranje topologije primjer ovog praktičnog vodiča lokalno u razvojno okruženje.

> [AZURE.WARNING] Lokalni testiranje funkcionira samo za osnovni, C# samo topologija. Nemojte koristiti lokalne testiranje za hibridno topologija ili topologija koje koriste većeg broja strujanja, kao što je primit će pogreške.

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite projekt, a zatim odaberite **Svojstva**. U svojstvima projekta promijenite **vrstu izlaza** **Konzole za**aplikaciju.

    ![Vrsta izlaza](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Imajte na umu da biste promijenili **vrstu izlaza** natrag na **Biblioteka klasa** prije implementacije topologije klaster.

2. U **Pregledniku rješenja**, desnom tipkom miša kliknite projekt, a zatim odaberite **Dodaj** > **Nova stavka**. Odaberite **Predmet** i **LocalTest.cs** kao unesite naziv klase. Na kraju, kliknite **Dodaj**.

3. Otvorite **LocalTest.cs** i dodajte sljedeća naredba za **Korištenje** pri vrhu:

        using Microsoft.SCP;

4. Sadržaj klase **LocalTest** , koristite sljedeće:

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Potrajati koji trenutak da biste pročitali kroz kod komentare. Kod koristi **LocalContext** da biste pokrenuli komponente u okruženje za razvoj, a ne riješi toka podataka između komponente tekstnih datoteka na lokalni disk.

5. Otvorite **Program.cs** , a zatim dodajte sljedeće **Glavna** načina:

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Spremite promjene, a zatim pritisnite **F5** ili odaberite **ispravljanje pogrešaka** > **Pokrenuti ispravljanje pogrešaka** da biste pokrenuli projekta. Prozor konzole moraju se, a prijaviti status kao tijeku testira. Kada se pojavi **testira završite** , pritisnite tipku sve da biste zatvorili prozor.

7. Pomoću **Programa Windows Explorer** da biste pronašli direktorij koji sadrži projekta, na primjer, **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. U ovom direktoriju otvorite **smeće**, a zatim **za ispravljanje pogrešaka**. Trebali biste vidjeti tekstne datoteke koje su proizvodi po pokrenuli testove: sentences.txt, counter.txt i splitter.txt. Otvorite svaki tekstne datoteke, a zatim Provjera podatke.

    > [AZURE.NOTE] Niz podataka je ista i kao niz decimalni vrijednosti u tim datotekama. Na primjer, \[[97,103,111]] u **splitter.txt** datoteka je riječ "i".

Iako testiranje osnovni word Brojanje aplikacije lokalno je vrlo trivial, stvarna vrijednost dolazi kada imate složene topologije komunicira s vanjskim izvorima podataka ili izvodi složene analize podataka. Kada radite na takav projekt, morate da biste postavili prekidne točke i prolaziti kroz kod u komponente izdvojiti problema.

> [AZURE.NOTE] Ne zaboravite **Vrsta projekta** ponovno postaviti **Biblioteka klasa** prije no što implementirate oluja na klasteru HDInsight.

### <a name="log-information"></a>Informacije u zapisniku

Možete jednostavno evidentirati podataka iz komponente topologije pomoću `Context.Logger`. Sljedeće će, primjerice, stvorili unos Informativna zapisnika:

    Context.Logger.Info("Component started");

Zabilježeni podaci moguće je prikazati iz **Zapisnika Hadoop servisa**, koji se nalazi u **Programu Explorer poslužitelja**. Proširite unos za vaše oluja na HDInsight klaster, a zatim proširite **Hadoop servisa zapisnika**. Na kraju, odaberite datoteke zapisnika da biste pogledali.

> [AZURE.NOTE] U zapisnicima spremaju se u račun za Azure prostora za pohranu koji koriste svoj klaster. Ako neku drugu pretplatu od one koju ste prijavljeni s Visual Studio, morate prijaviti na pretplatu koja sadrži račun za pohranu da biste prikazali taj podatak.

### <a name="view-error-information"></a>Prikaz informacija o pogrešci

Da biste pogledali pogreške koji se odvijaju u izvodi topologiji, poduzmite sljedeće korake:

1. **Poslužitelj Explorer**desnom tipkom miša kliknite oluja na HDInsight klaster i odaberite **Prikaz oluja topologija**.

2. Za **Spout** i **Vijci s Maticama**, stupac **Zadnje pogreške** će imati podatke na zadnji pogreške koji se pojavio.

3. Odaberite **Spout Id** ili **Munje Id** za komponentu koja sadrži navedene pogreške. Na stranici pojedinosti koje se prikazuju dodatne informacije o pogrešci će se prikazati u odjeljku **pogreške** pri dnu stranice.

4. Da biste dobili dodatne informacije, odaberite **priključak** iz odjeljka **Executors** stranice da biste vidjeli tempiranja zapisnika oluja posljednje nekoliko minuta.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili kako razviti i implementacija oluja topologija iz alata za HDInsight za Visual Studio, Saznajte kako [postupak događaje iz centra za događaj Azure s oluja na HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Primjer C# topologije koju strujanja podataka dijeli većeg broja strujanja potražite u članku [primjer C# oluja](https://github.com/Blackmist/csharp-storm-example).

Da biste otkrili dodatne informacije o stvaranju C# topologija, posjetite [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Dodatni načini rada s HDInsight i dodatne oluja na HDinsight uzorka, pogledajte sljedeće:

**Microsoft SCP.NET**

* [Vodič za programiranja Pronađenim](hdinsight-storm-scp-programming-guide.md)

**Apache oluja na HDInsight**

- [Implementacija i nadzirati topologija s Apache oluja na HDInsight](hdinsight-storm-deploy-monitor-topology.md)

- [Primjer topologija za oluja na HDInsight](hdinsight-storm-example-topology.md)

**Hadoop Apache na HDInsight**

- [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

- [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

- [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase na HDInsight**

- [Uvod u HBase na HDInsight](hdinsight-hbase-tutorial-get-started.md)
