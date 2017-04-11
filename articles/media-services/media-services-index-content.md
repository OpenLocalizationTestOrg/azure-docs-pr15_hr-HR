<properties
    pageTitle="Indeksiranje medijskih datoteka s Azure Media indeksiranje"
    description="Azure Media indeksiranje omogućuje vam da biste sadržaj medijske datoteke mogu pretraživati i da biste generirali transkript cijelog teksta za skriveni titlovi i ključne riječi. U ovoj se temi objašnjava korištenje medijskih sadržaja za indeksiranje."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Indeksiranje medijskih datoteka s Azure Media indeksiranje


Azure Media indeksiranje omogućuje vam da biste sadržaj medijske datoteke mogu pretraživati i da biste generirali transkript cijelog teksta za skriveni titlovi i ključne riječi. Možete obraditi jedan medijskih datoteka ili većeg broja medijskih datoteka u grupu.  

>[AZURE.IMPORTANT] Kada indeksiranje sadržaja, provjerite je li za korištenje medijske datoteke koje ste vrlo Očisti govora (bez glazbe u pozadini, Šum, efekata ili mikrofona hiss). Neki primjeri odgovarajući sadržaj: snimljena sastanaka, predavanja ili prezentacije. Sljedeći sadržaj možda nije prikladna za indeksiranje: filmovi, prikazuje TV, bilo što s kombiniranim audio i zvučnih efekata loše snimljeni sadržaj s pozadinski Šum (hiss).


Indeksiranja posao možete generirati izlaze za sljedeće:

- Zatvorena opisa datoteke sljedećih oblika: **SAMI**, **TTML**i **WebVTT**.

    Datoteke titlova uključuju oznake naziva Recognizability, koji rezultati indeksiranja posao ovisno o kako može prepoznati govora u videozapisu izvora.  Vrijednost Recognizability možete koristiti na zaslonu Izlazna datoteka za upotrebljivosti. Niska rezultat bi znači nisku indeksiranja rezultate zbog kvalitetu zvuka.
- Ključna riječ datoteka (XML).
- Audio indeksiranje blob datoteka (AIB) za korištenje sa sustavom SQL server.

    Dodatne informacije potražite u članku [Korištenje AIB datoteke s Azure Media indeksiranje i SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


U ovoj se temi objašnjava da biste stvorili poslova indeksiranja **indeks sredstvo** i **indeksirati više datoteka**.

Najnovija ažuriranja za indeksiranje Azure Media potražite u članku [blogovi Media Services](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Korištenje konfiguracija i manifest datoteka za indeksiranje zadataka

Možete navesti dodatne detalje za indeksiranje zadataka pomoću konfiguracije zadatka. Na primjer, možete odrediti koje metapodataka za medijske datoteke. U ovom metapodataka koristi modul jezik da biste proširili njegov rječnik i znatno poboljšava točnost prepoznavanja govora.  Vam se i da biste odredili željeni Izlazna datoteka.

Više medijske datoteke možete obraditi i odjednom pomoću manifesta datoteke.

Dodatne informacije potražite u članku [Zadatka unaprijed postavljeni za Azure Media indeksiranje](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Indeksiranje imovine

Sa sljedećim korakom prenosi medijske datoteke kao sredstvo i stvara zadatak za indeksiranje sredstava.

Imajte na umu da ako nema konfiguracijska datoteka nije naveden, medijske datoteke će biti indeksirana uz sve zadane postavke.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
<!-- __ -->
### <a id="output_files"></a>Izlazna datoteka

Prema zadanim postavkama indeksiranja posao generira sljedećih Izlazna datoteka. Datoteka će se spremati prvi resursa izlaz.

Ako postoji više od jedne unos medijske datoteke, indeksiranje generirat će manifesta datoteke za izlaze posla, pod nazivom "JobResult.txt". Za svaki unos medijske datoteke, rezultirajući AIB, SAMI, TTML, WebVTT i datoteke ključnu riječ, su sekvencijalno numerirani i pod nazivom pomoću "Pseudonim".

Naziv datoteke | Opis
----------|------------
__InputFileName.aib__ | Indeksiranje blob audiodatoteku. <br /><br /> Audiodatoteka indeksiranje Blob (AIB) je binarne datoteke koja se mogu pretraživati Microsoft SQL server pomoću pretraživanja cijelog teksta.  Datoteka AIB je jače od datoteke jednostavne opisa jer sadrži alternative za svaku riječ, što omogućuje mnogo pretraživanje učinkovitije. <br/> <br/>Je potrebna je instalacija dodatka za indeksiranje SQL na na računalu pokrenuti Microsoft SQL server 2008 ili noviji. Pretraživanje AIB koristeći Microsoft SQL server pretraživanje po cijelom tekstu daje točnije rezultate pretraživanja od pretraživanje datoteka titlova generira WAMI. To je zato na AIB zvukom zamjenske riječi koje slične dok datoteke titlova sadrže najveće pouzdanosti riječ za svaki segment zvuka. Ako je traženje izgovorenih riječi upmost važnosti, a zatim se preporučuje za uporabu zajedno AIB u Microsoft SQL Server.<br/><br/> Da biste preuzeli dodatak, kliknite <a href="http://aka.ms/indexersql">Azure Media indeksiranje SQL dodatak</a>. <br/><br/>Također je moguće koristiti ostale tražilice kao što su Apache Lucene/Solr jednostavno indeksirati videozapisa na temelju titlova i datoteke s XML ključnu riječ, ali to će rezultirati manje točne rezultate pretraživanja.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Zatvoreni opisa (CC) datoteke SAMI, TTML i WebVTT oblika.<br/><br/>Mogu se može koristiti za audio i videodatoteka čine pristupačnim osobe oštećena sluha pročita.<br/><br/>Zatvoreni opisa datoteke uključuju oznake naziva <b>Recognizability</b> je koji rezultati indeksiranja posla na temelju kako može prepoznati govora izvorni videozapis.  Vrijednost <b>Recognizability</b> možete koristiti na zaslonu Izlazna datoteka za upotrebljivosti. Niska rezultat bi znači nisku indeksiranja rezultate zbog kvalitetu zvuka.
__InputFileName.kw.xml<br />InputFileName.info__ |Ključne riječi i informacije o datotekama. <br/><br/>Ključna riječ datoteka je XML datoteku koja sadrži ključne riječi dobivenih iz sadržaja govora s učestalost i offset informacije. <br/><br/>Datoteka je datoteka običnog teksta koje sadrži zrnastog informacije o svakom termina prepoznala. U prvom retku je posebno i sadrži Recognizability rezultat. Svaki sljedeći redak je popis tabulatorom sljedeće podatke: pokretanje vrijeme, vrijeme završetka, riječ fraza, confidence. Ponudit će vam vrijeme u sekundama i i dobiva kao broj od 0 do 1. <br/><br/>Primjer retka: "1.20 1.45 riječi 0.67" <br/><br/>Te datoteke možete koristiti za broj svrhe, kao što su za izvođenje analize govora ili izložen tražilice kao što su Bing, Google ili Microsoft SharePoint da biste medijske datoteke više vidljivim ili čak koristi za isporuku relevantnije reklame.
__JobResult.txt__ |Izlaz manifest, postoje samo kada indeksiranje više datoteka koja sadrži sljedeće podatke:<br/><br/><table border="1"><tr><th>InputFile</th><th>Pseudonim</th><th>MediaLength</th><th>Pogreška</th></tr><tr><td>a.MP4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.MP4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.MP4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Ako nije uspješno indeksirani sve unos medijske datoteke, posao indeksiranja neće uspjeti kod pogreške 4000. Dodatne informacije potražite u članku [Kodovi pogreške](#error_codes).

## <a name="index-multiple-files"></a>Indeksiranje više datoteka

Sa sljedećim korakom prenosi više medijske datoteke kao sredstvo i stvara zadatak za indeksiranje te datoteke u grupu.

Manifesta datoteku s nastavkom .lst su stvorene i prijenos u sredstava. Datoteka manifesta sadrži popis svih datoteka resursa. Dodatne informacije potražite u članku [Zadatka unaprijed postavljeni za Azure Media indeksiranje](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Djelomično je uspjela posla

Ako nije uspješno indeksirani sve unos medijske datoteke, posao indeksiranja neće uspjeti kod pogreške 4000. Dodatne informacije potražite u članku [Kodovi pogreške](#error_codes).


Generiraju se isti izlaze (kao što je uspjela zadaci). Može se odnositi na izlazna datoteka manifesta da biste saznali koji ulaznih datoteka se nije uspjela, prema vrijednosti u stupcu pogreške. Za unos datoteke koje nije uspjela, rezultirajući AIB, SAMI, TTML, WebVTT i ključnih riječi datoteka ne generiraju se.

### <a id="preset"></a>Zadana Postava zadatak za indeksiranje Azure Media

Obrada iz Azure Media indeksiranje moguće je prilagoditi unosom neobavezno zadataka koji se unaprijed uz zadatak.  Format ovaj xml konfiguracije opisuju.

Ime | Obavezan | Opis
----|----|---
__unos__ | FALSE | Datoteka resursa koje želite indeksirati.</p><p>Azure Media indeksiranje podržava sljedeće oblike medijskih datoteka: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Naziv datoteke (s) možete odrediti u atribut **ime** ili **popisa** **ulazni** element (kao što je prikazano u nastavku). Ako ne navedete datoteka resursa koje će se indeksirati, izdvojeni je primarnu datoteku. Ako je datoteka ne primarni resursa, prvu datoteku u unos resursa će se indeksirati.</p><p>Da biste izričito odredite naziv datoteke resursa, učinite:<br />`<input name="TestFile.wmv">`<br /><br />Možete indeksirati i više resursa datoteka odjednom (do 10 datoteke). Da biste to učinili:<br /><br /><ol class="ordered"><li><p>Stvaranje tekstne datoteke (manifesta datoteka) i dajte mu datotečni nastavak .lst. </p></li><li><p>Dodajte popis svih nazivima datoteka resursa u vaš unos resursa manifesta datoteke. </p></li><li><p>Dodavanje datoteke (prijenos) thanifest sredstava.  </p></li><li><p>Odredite naziv datoteke manifesta u atribut popisa za unos.<br />`<input list="input.lst">`</li></ol><br /><br />Napomena: Ako dodali više od 10 datoteke manifesta datoteku, posao indeksiranja neće uspjeti kod pogreške 2006.
__metapodataka__ | FALSE | Metapodaci za datoteke navedeni resursa za adaptacijski rječnik.  Korisno je da biste se pripremili za indeksiranje prepoznavanje riječi nestandardne rječnik kao što su vlastite imenice.<br />`<metadata key="..." value="..."/>` <br /><br />Možete navesti __vrijednosti__ za unaprijed definirane __tipke__. Trenutno podržani su sljedeći ključevi:<br /><br />"Naslov" i "Opis" - za adaptacijski rječnik za dotjerati jezik modela za svoj posao i poboljšati točnost prepoznavanja govora.  Polazne vrijednosti pretraživanja Internet da biste pronašli dokumente contextually odgovarajući tekst pomoću sadržaja za dodavanje internog rječnika za trajanje zadatka indeksiranja.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__značajke__ <br /><br /> Dodaje verzija 1.2. Trenutno je samo podržanu značajku prepoznavanja govora ("ASR").| FALSE | Značajka prepoznavanja govora sadrži tipke za sljedeće postavke:<table><tr><th><p>Ključ</p></th>     <th><p>Opis</p></th><th><p>Primjer vrijednost</p></th></tr><tr><td><p>Jezik</p></td><td><p>Prirodnim jezikom se prepoznaju u multimedijske datoteke.</p></td><td><p>Engleski, španjolski</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>popis opisa željeni izlaznom obliku (ako ih ima) odvojenih zarezom</p></td><td><p>ttml sami; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Zastavica Booleova vrijednost koja određuje hoće li se AIB potrebna je datoteka (za korištenje sa SQL Server i klijenta za indeksiranje IFilter).  Dodatne informacije potražite u članku <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Korištenje AIB datoteke s Azure Media indeksiranje i SQL Server</a>.</p></td><td><p>TRUE; FALSE</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Zastavica Booleova vrijednost koja određuje hoće li ključnu riječ XML datoteke potreban je.</p></td><td><p>TRUE; FALSE. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Zastavica Booleova vrijednost koja određuje hoće li prisilno cijelog opise (bez obzira na to razine pouzdanosti).  </p><p>Zadana je vrijednost false, u tom slučaju riječi i izraza koji imaju manje od razine pouzdanosti 50% su izostavljena iz izlaza konačni opisa i zamjenjuje tri točke ("...").  Tri točke korisne su za kontrolu kvalitete opisa i nadzor.</p></td><td><p>TRUE; FALSE. </p></td></tr></table>

### <a id="error_codes"></a>Kodovi pogrešaka

U slučaju pogreške, trebali biste izvješća komponente za indeksiranje Azure Media sigurnosno jednu od sljedećih šifri pogreške:

Kod | Ime | Mogućih razloga
-----|------|------------------
2000 | Konfiguracija nije valjana | Konfiguracija nije valjana
2001. | Nije valjan unos resursi | Nedostaje unos imovine ili prazan resursa.
2002 | Manifest koji nisu valjani | Manifest je prazna ili manifest sadrži stavke koje nisu valjane.
2003 | Nije uspjelo preuzimanje medijske datoteke | URL u datoteci manifesta nije valjan.
2004 | Nepodržane protokola | Protokol medijskih sadržaja URL-a nisu podržani.
2005 | Vrsta datoteke koji nije podržan | Vrsta datoteke unos medijskih sadržaja nije podržana.
2006. | Previše ulaznih datoteka | Nema više od 10 datoteke u unos manifest.
3000 | Nije uspjelo dekodiranje medijske datoteke | Nepodržane medijskog kodeka <br/>ili<br/> Oštećeni medijske datoteke <br/>ili<br/> Nema strujanje audiozapisa u unos medijske sadržaje.
4000 | Grupe za indeksiranje djelomično uspjela | Su neke od unos medijskih datoteka nije uspjelo indeksirati. Dodatne informacije potražite u članku <a href="#output_files">Izlazna datoteka</a>.
drugi | Interne pogreške | Obratite se timu za podršku. indexer@microsoft.com


## <a id="supported_languages"></a>Podržani jezici

Trenutno podržani su jezici engleskom ili španjolskom. Dodatne informacije potražite u članku [v1.2 izdanje bloga](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Srodne veze

[Azure Media Services pregled Analytics](media-services-analytics-overview.md)

[AIB datoteke pomoću programa za indeksiranje Azure Media i SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indeksiranje medijskih datoteka uz pretpregled Azure Media indeksiranje 2](media-services-process-content-with-indexer2.md)
