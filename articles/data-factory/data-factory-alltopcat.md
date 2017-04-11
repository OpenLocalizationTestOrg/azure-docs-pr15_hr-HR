<properties
    pageTitle="Svih tema vezanih uz tvorničke podataka servisa | Microsoft Azure"
    description="Popis svih tema vezanih uz servisa za Azure pod nazivom tvorničke podataka koji postoje na http://azure.microsoft.com/documentation/articles/, naslova i opisa."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Svih tema vezanih uz servisa Azure podataka tvorničke

Ova tema sadrži popis svaki temi koja se odnosi izravno na **Tvorničke podataka** servisa programa Azure. Ova web-stranica za ključne riječi možete pretraživati pomoću **Ctrl + F**, da biste pronašli teme trenutni kamata.




## <a name="new"></a>Novi

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 1 | [Premještanje podataka iz Amazon Redshift pomoću tvorničke Azure podataka](data-factory-amazon-redshift-connector.md) | Saznajte kako premjestiti podatke iz Amazon Redshift pomoću tvorničke Azure podataka. |
| 2 | [Premještanje podataka iz Amazon jednostavne servis za pohranu pomoću tvorničke Azure podataka](data-factory-amazon-simple-storage-service-connector.md) | Saznajte više o premještanje podataka iz Amazon jednostavne servis za pohranu (S3) pomoću tvorničke Azure podataka. |
| 3 | [Azure podataka tvorničke – Kopiraj čarobnjaka](data-factory-azure-copy-wizard.md) | Saznajte kako pomoću čarobnjaka za kopiranje podataka tvorničke Azure kopirajte podatke iz podržani izvori podataka u primatelji. |
| 4 | [Praktični vodič: Stvaranje vaš prvi tvorničke Azure podataka pomoću podataka tvorničke REST API-JA](data-factory-build-your-first-pipeline-using-rest-api.md) | U ovom ćete praktičnom vodiču stvorite kanal za Azure podataka tvorničke uzorka pomoću podataka tvorničke REST API-JA. |
| 5 | [Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću .NET API-JA](data-factory-copy-activity-tutorial-using-dotnet-api.md) | U ovom ćete praktičnom vodiču stvarate kanal na tvorničke Azure podataka s Kopiraj aktivnosti pomoću .NET API-JA. |
| 6 | [Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću REST API-JA](data-factory-copy-activity-tutorial-using-rest-api.md) | U ovom ćete praktičnom vodiču stvorite kanal na tvorničke podataka Azure s Kopiraj aktivnosti pomoću REST API-JA. |
| 7 | [Čarobnjak za kopiranje tvorničke podataka](data-factory-copy-wizard.md) | Saznajte kako pomoću čarobnjaka za kopiranje tvorničke podataka da biste kopirali podatke iz podržani izvori podataka primatelji. |
| 8 | [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md) | Premještanje podataka s lokalnog poslužitelja u oblak i postavljanje pristupnika podacima. Premještanje podataka pomoću pristupnika za upravljanje podacima u tvorničke Azure podataka. |
| 9 | [Premještanje podataka iz lokalnog Cassandra baze podataka pomoću tvorničke Azure podataka](data-factory-onprem-cassandra-connector.md) | Saznajte kako premjestiti podatke iz lokalnog Cassandra baze podataka pomoću tvorničke Azure podataka. |
| 10 | [Premještanje podataka iz MongoDB pomoću tvorničke Azure podataka](data-factory-on-premises-mongodb-connector.md) | Saznajte više o premještanje podataka iz baze podataka MongoDB pomoću tvorničke Azure podataka. |
| 11 | [Premještanje podataka iz servisa Salesforce pomoću tvorničke Azure podataka](data-factory-salesforce-connector.md) | Saznajte kako premještanje podataka iz servisa Salesforce pomoću tvorničke Azure podataka. |


## <a name="updated-articles-data-factory"></a>Ažurirani članke tvorničke podataka

U ovom se odjeljku navedeni članci koje su ažurirane nedavno, gdje je ažuriranje veliku ili značajan. Za svaki ažurirani članak gruba isječak teksta dodane markdown prikazat će se. Članci su ažurirani unutar datumskog raspona **2016, 08 i 22** **2016, 10 i 05**.

| &nbsp; | Članak | Ažurirani tekst, isječka | Kada ažurirati |
| --: | :-- | :-- | :-- |
| 12 | [Azure tvorničke podataka – Evidencija promjena .NET API](data-factory-api-change-log.md) | Ovaj članak sadrži informacije o promjenama Azure podataka tvorničke SDK u određenu verziju. Možete pronaći najnovije NuGet paket za Azure podataka tvorničke ovdje (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **verzija 4.11.0** značajka dodatke: / dodani su sljedeće vrste povezane servisa: AmazonRedshiftLinkedService - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - (https://msdn.microsoft.com/library/mt765121.aspx) – AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) / dodani su sljedeće vrste dataset: AmazonS3Dataset - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - (https://msdn.microsoft.com/library/mt765112.aspx) / dodani su sljedeće vrste izvora Kopiraj :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **verzija 4.10.0** / sljedeća svojstva neobavezno dodani TextFormat:-skijaške opreme | 2016-09-22 |
| 13 | [Premještanje podataka i iz blobova platforme Azure pomoću tvorničke Azure podataka](data-factory-azure-blob-connector.md) |  / copyBehavior / definira ponašanje kopija kada je izvor BlobSource ili datotečnom sustavu.  / **PreserveHierarchy:** zadržava hijerarhiji datoteka u odredišnoj. Relativni put izvornu datoteku da biste mapa izvora jednak relativni put ciljnu datoteku da biste ciljnu mapu... Br /... Br /. **FlattenHierarchy:** su sve datoteke iz mape izvora u prva razina ciljne mape. Ciljne datoteke imati automatski generira naziv. .Br /... Br /. **MergeFiles: (zadano)** spaja sve datoteke iz mape izvora u jednu datoteku. Ako je naveden naziv datoteke/Blob, naziv datoteke spojenih bio je naziv naveden; u suprotnom, bilo bi automatski generirani naziv datoteke.  / Ne / **BlobSource** podržava ta dva svojstva za kompatibilnost sa starijim verzijama. / **treatEmptyAsNull**: određuje hoće li se Smatraj null ili praznu vrijednost null. / **skipHeaderLineCount** - određuje koliko reci moraju se preskočiti. Nije primjenjivo samo kada unos dataset koristi TextFormat. Isto tako, **BlobSink** podržava ti | 2016 09 28 |
| 14 | [Stvaranje predvidljivu kanali pomoću Azure strojnog učenja aktivnosti](data-factory-azure-ml-batch-execution-activity.md) | **Web-servisa zahtijeva više ulaza** Ako web-servisa traje više unosa, koristite svojstvo **webServiceInputs** umjesto korištenja **webServiceInput**. Skupove podataka koje se pozivaju **webServiceInputs** također moraju biti obuhvaćene aktivnosti **unosa**. U svoje eksperiment Azure ML web servis za unos i izlazni priključci i globalni parametara su zadane nazive ("input1", "input2") koje možete prilagoditi. Nazive koji koristite za webServiceInputs, webServiceOutputs i postavke globalParameters mora u potpunosti odgovarati imenima u eksperimenata. Na stranici pomoć za grupe za izvršavanje za Azure ML krajnju točku da biste provjerili očekivani mapiranje možete pogledati opseg zahtjev uzorka.    {"naziv": "PredictivePipeline", "Svojstva": {"Opis": "koristi AzureML model", "aktivnosti": {"naziv": "MLActivity", "Vrsta": "AzureMLBatchExecution", "Opis": "predviđanje analizi grupe za unos", "ulazi": {"naziv": "inputDataset1"}, {"naziv": "inputDatase | 2016-09-13 |
| 15 | [Kopiranje aktivnosti performanse i ugađanje vodiča](data-factory-copy-activity-performance.md) | 1. **uspostavljanje referentne vrijednosti**. Tijekom faze razvoja testirajte na kanal pomoću aktivnosti Kopiraj na temelju uzorka predstavniku podataka. Tvorničke podataka istjecanju model (podataka – tvorničke-zakazivanje – i – execution.md time-series-datasets-and-data-slices) možete koristiti da biste ograničili količinu podataka kojima surađujete.  Prikupljanje vrijeme izvođenja i karakteristike performansi pomoću **nadzor i upravljanje aplikacija**. Na početnoj stranici tvorničke podataka odaberite **nadzor i upravljanje** . U prikazu stabla odaberite **Izlazni skup podataka**. Na popisu **Aktivnosti Windows** odaberite Kopiraj aktivnosti pokrenuti. **Windows aktivnost** navodi trajanje Kopiraj aktivnosti i veličina podataka koja se kopira. Propusnost nalazi se u **Programu Explorer prozora aktivnosti**. Dodatne informacije o aplikaciji potražite u članku praćenje i upravljanje kanali tvorničke Azure podataka pomoću nadzor i upravljanje aplikacije (podataka – tvorničke-monitor-upravljanje-app.md).  ! Aktivnosti pokrenuti pojedinosti (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn | 2016-09-27 |
| 16 | [Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Imajte na umu sljedeće točke:-dataset **Vrsta** postavljena na **AzureBlob**.     - **linkedServiceName** postavljen na **AzureStorageLinkedService**. Stvara povezane servis u koraku 2.     - **folderPath** postavljen na spremnik **adftutorial** . Možete odrediti i naziv blob unutar mape pomoću svojstvo **naziv datoteke** . Budući da se ne koji određuje naziv blob-om, podaci iz svih blob-ova u spremniku smatra se kao ulaznih podataka.    -oblik **Vrsta** postavljena na **TextFormat** - postoje dva polja na tekst datoteke ΓÇô **ime** i **Prezime** ΓÇô odvojene znakom zarezom (**columnDelimiter**) – **dostupnost** postavljen na **zaračunava** (**Učestalost** postavljena na **sat** i **interval** postavljeno na **1**). Stoga tvorničke podataka traži ulazne podatke svaki sat u korijenskoj mapi spremnika blob (**adftutorial**) koje ste naveli.  Ako ne odredite **naziv datoteke** za **unos** skup podataka, sve datoteke/blob-ova iz mape ulazne (**folderPath**) su consid | 2016 09 29 |
| 17 | [Stvaranje, praćenje i upravljanje factories Azure podataka pomoću podataka tvorničke .NET SDK-a](data-factory-create-data-factories-programmatically.md) | Imajte na umu dolje aplikacije ID i lozinka (klijent tajna) te ga koristiti u prikazu. **Pretplatu za dobivanje Azure i klijentu ID-a** Ako nemate najnoviju verziju programa PowerShell instaliran na vašem računalu, slijedite upute kako instalirati i konfigurirati Azure PowerShell (.. / powershell Instaliraj configure.md) članak da biste ga instalirali. 1. Pokrenite Azure PowerShell i pokrenite sljedeću naredbu 2. Pokrenite sljedeću naredbu, a zatim unesite korisničko ime i lozinku koje koristite za prijavu na portal za Azure.         Prijava AzureRmAccount ako imate samo jedan Azure pretplatu povezan s ovim računom, ne morate izvršiti sljedeća dva koraka. 3. Pokrenite sljedeću naredbu da biste pogledali sve pretplate za taj račun.       Get-AzureRmSubscription 4. Pokrenite sljedeću naredbu da biste odabrali pretplatu u koju želite raditi. Zamijenite **NameOfAzureSubscription** naziv pretplate za Azure.       Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription / skup AzureRmCo | 2016 09 14 |
| 18 | [Kanali i aktivnosti u tvorničke Azure podataka](data-factory-create-pipelines.md) |       "Pokreni": "2016-07-12T00:00:00Z", "Završi": "2016-07-13T00:00:00Z"}} Imajte na umu sljedeće točke: / u odjeljku aktivnosti je samo jedan aktivnosti čija je **Vrsta** postavljen na **kopiji**. / Unos aktivnosti je **InputDataset** i izlazni aktivnosti postavljen na **OutputDataset**. / U odjeljku **typeProperties** **BlobSource** navedena je kao vrsta izvora i **SqlSink** je naveden kao vrstu primatelj. Prikaz stvaranja ovaj kanal potražite u članku vodič: kopirajte podatke iz spremišta blobova u SQL baze podataka (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Ogledna transformaciju kanal** U sljedeće ogledne kanal, postoji jedan aktivnosti vrste **HDInsightHive** u odjeljku **aktivnosti** . U ovom primjeru HDInsight grozd aktivnosti (podataka – tvorničke-grozd-activity.md) pretvorbe podataka iz Azure blobova tako da pokrenete datoteku skripte grozd na programa Azure HDInsight Hadoop klaster.  {"naziv": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Pretvaranje podataka u tvorničke Azure podataka](data-factory-data-transformation-activities.md) | Tvorničke podataka podržava sljedećih aktivnosti transformacije podataka koje je moguće dodati kanali (podataka – tvorničke – stvaranje-pipelines.md) ili pojedinačno ili povezane s druge aktivnosti. .  AZURE. Napomena vodič s detaljne upute potražite u članku Stvaranje kanal s članak grozd transformaciju (data-factory-build-your-first-pipeline.md). **HDInsight grozd aktivnosti** HDInsight grozd aktivnosti u podataka tvorničke kanalu izvršava grozd upita vlastite ili klaster/Linux utemeljen na sustavu Windows HDInsight na zahtjev. U članku grozd aktivnosti (podataka – tvorničke-grozd-activity.md) detalje o aktivnost. **Svinja HDInsight aktivnosti** Svinja HDInsight aktivnosti u podataka tvorničke kanalu izvršava Svinja upiti vlastite ili klaster/Linux utemeljen na sustavu Windows HDInsight na zahtjev. U članku Svinja aktivnosti (podataka – tvorničke-svinja-activity.md) detalje o aktivnost. **HDInsight MapReduce aktivnosti** HDInsight MapReduce aktivnosti u podataka tvorničke kanalu izvršava MapReduce programe na y | 2016 09 26 |
| 20 | [Planiranje na tvorničke podataka i izvođenja](data-factory-scheduling-and-execution.md) | CopyActivity2 će se pokrenuti samo ako je na CopyActivity1 uspješne i dostupna je Dataset2. Evo oglednih kanal JSON: {"naziv": "ChainActivities", "Svojstva": {"Opis": "Pokreni aktivnosti u nizu", "aktivnosti": {"Vrsta": "Kopirati", "typeProperties": {"izvor": {"Vrsta": "BlobSource"}, "Primatelj": {"Vrsta": "BlobSink", "copyBehavior": "PreserveHierarchy", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "unosa": {"naziv": "Dataset1"}, "Proizvodi": {"naziv": "Dataset2"}, "pravila": {"vremenskog ograničenja": "01: 00:00"}, "raspored": {"učestalost": "H", "interval": 1}, "naziv": "CopyFromBlob1ToBlob2", "Opis": "Kopirajte podatke iz blob u drugu"}, {"Vrsta": "Kopirati", "typeProperties": {"izvor": {"Vrsta": "BlobSource"}, "Primatelj" : {"Vrsta": "BlobSink", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "u | 2016 09 28 |





## <a name="tutorials"></a>Vodiči za

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 21 | [Praktični vodič: Stvaranje vaš prvi kanal postupak podatke pomoću Hadoop klaster](data-factory-build-your-first-pipeline.md) | Pomoću ovog praktičnog vodiča Azure podataka tvorničke prikazuje kako stvoriti i zakazivanje tvorničke podataka koji obrađuje podatke pomoću skripte grozd Hadoop klaster. |
| 22 | [Praktični vodič: Stvaranje vaš prvi tvorničke Azure podataka pomoću predloška Azure Voditelj resursa](data-factory-build-your-first-pipeline-using-arm.md) | U ovom ćete praktičnom vodiču stvorite kanal za Azure podataka tvorničke uzorka pomoću predloška Azure Voditelj resursa. |
| 23 | [Praktični vodič: Stvaranje vaš prvi tvorničke Azure podataka pomoću portala za Azure](data-factory-build-your-first-pipeline-using-editor.md) | U ovom ćete praktičnom vodiču stvorite kanal za Azure podataka tvorničke uzorka pomoću uređivača tvorničke podataka na portalu za Azure. |
| 24 | [Praktični vodič: Stvaranje vaš prvi tvorničke Azure podataka pomoću komponente PowerShell Azure](data-factory-build-your-first-pipeline-using-powershell.md) | U ovom ćete praktičnom vodiču stvorite kanal za Azure podataka tvorničke uzorka pomoću komponente PowerShell Azure. |
| 25 | [Praktični vodič: Stvaranje prve Azure podataka tvorničke koristeći Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | U ovom ćete praktičnom vodiču stvorite kanal za Azure podataka tvorničke uzorka pomoću Visual Studio. |
| 26 | [Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) | U ovom ćete praktičnom vodiču stvorite kanal na tvorničke podataka Azure s Kopiraj aktivnosti pomoću tvorničke uređivač podataka na portalu za Azure. |
| 27 | [Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću komponente PowerShell Azure](data-factory-copy-activity-tutorial-using-powershell.md) | U ovom ćete praktičnom vodiču stvorite kanal na tvorničke podataka Azure s Kopiraj aktivnosti pomoću Azure PowerShell. |
| 28 | [Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) | U ovom ćete praktičnom vodiču stvorite kanal na tvorničke podataka Azure s Kopiraj aktivnosti pomoću Visual Studio. |
| 29 | [Kopirajte podatke iz spremišta blobova u SQL baze podataka pomoću tvorničke podataka](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Pomoću ovog praktičnog vodiča pokazuje kako pomoću aktivnosti Kopiraj u kanalu na tvorničke Azure podataka da biste kopirali podatke iz spremišta blobova s bazom podataka SQL. |
| 30 | [Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću čarobnjaka za kopiranje tvorničke podacima](data-factory-copy-data-wizard-tutorial.md) | U ovom ćete praktičnom vodiču stvorite kanal na tvorničke podataka Azure s Kopiraj aktivnosti pomoću čarobnjaka za kopiranje podržava tvorničke podataka |



## <a name="data-movement"></a>Premještanje podataka

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 31 | [Premještanje podataka i iz blobova platforme Azure pomoću tvorničke Azure podataka](data-factory-azure-blob-connector.md) | Saznajte kako da biste kopirali podatke blobova platforme Azure podataka tvorničke. Korištenje oglednim: kako kopirati podatke i iz spremište blobova platforme Azure i baze podataka SQL Azure. |
| 32 | [Premještanje podataka i iz spremišta Lake podataka za Azure pomoću tvorničke Azure podataka](data-factory-azure-datalake-connector.md) | Upute za premještanje podataka iz spremišta Lake podataka za Azure pomoću tvorničke Azure podataka |
| 33 | [Premještanje podataka i s DocumentDB pomoću tvorničke Azure podataka](data-factory-azure-documentdb-connector.md) | Saznajte kako premjestiti podatke/iz zbirke Azure DocumentDB pomoću tvorničke Azure podataka |
| 34 | [Premještanje podataka i s bazom podataka SQL Azure pomoću tvorničke Azure podataka](data-factory-azure-sql-connector.md) | Upute za premještanje podataka iz baze podataka SQL Azure pomoću tvorničke Azure podataka. |
| 35 | [Premještanje podataka i iz Azure SQL Data Warehouse pomoću tvorničke Azure podataka](data-factory-azure-sql-data-warehouse-connector.md) | Upute za premještanje podataka iz Azure SQL Data Warehouse pomoću tvorničke Azure podataka |
| 36 | [Premještanje podataka i iz tablice Azure pomoću tvorničke Azure podataka](data-factory-azure-table-connector.md) | Upute za premještanje podataka iz spremišta tablica Azure pomoću tvorničke Azure podataka. |
| 37 | [Kopiranje aktivnosti performanse i ugađanje vodiča](data-factory-copy-activity-performance.md) | Saznajte više o ključa čimbenici koji utječu na performanse premještanje podataka na tvorničke Azure podataka kada koristite aktivnost Kopiraj. |
| 38 | [Premještanje podataka pomoću naredbe Kopiraj aktivnosti](data-factory-data-movement-activities.md) | Dodatne informacije o premještanju podataka u kanali tvorničke podataka: prijenos podataka između trgovine oblaka i između programa lokalne pohrane i spremište u oblaku. Koristite aktivnost Kopiraj. |
| 39 | [Napomene za pristupnik za upravljanje podacima](data-factory-gateway-release-notes.md) | Podaci pristupnik za upravljanje tory napomene |
| 40 | [Premještanje podataka iz lokalnog HDFS pomoću tvorničke Azure podataka](data-factory-hdfs-connector.md) | Saznajte kako premjestiti podatke iz lokalnog HDFS pomoću tvorničke Azure podataka. |
| 41 | [Nadzor i upravljanje kanali tvorničke Azure podataka pomoću novih nadzor i upravljanje aplikacije](data-factory-monitor-manage-app.md) | Saznajte kako koristiti nadzor i upravljanje aplikacija za nadzor i upravljanje factories Azure podataka i kanali. |
| 42 | [Premještanje podataka s lokalnim izvorima poslužitelja u oblak i s pristupnik za upravljanje podacima](data-factory-move-data-between-onprem-and-cloud.md) | Premještanje podataka s lokalnog poslužitelja u oblak i postavljanje pristupnika podacima. Premještanje podataka pomoću pristupnika za upravljanje podacima u tvorničke Azure podataka. |
| 43 | [Premještanje podataka iz za OData izvora pomoću tvorničke Azure podataka](data-factory-odata-connector.md) | Saznajte više o premještanje podataka iz izvora OData pomoću tvorničke Azure podataka. |
| 44 | [Premještanje podataka iz ODBC podataka trgovine pomoću tvorničke Azure podataka](data-factory-odbc-connector.md) | Saznajte kako premještanje podataka iz ODBC podataka trgovine pomoću tvorničke Azure podataka. |
| 45 | [Premještanje podataka iz DB2 pomoću tvorničke Azure podataka](data-factory-onprem-db2-connector.md) | Dodatne informacije o korištenju premještanje podataka iz baze podataka DB2 pomoću tvorničke Azure podataka |
| 46 | [Premještanje podataka i s lokalnim sustavom za datoteku pomoću tvorničke Azure podataka](data-factory-onprem-file-system-connector.md) | Saznajte kako možete premjestiti i s lokalnim sustavom za datoteke podataka pomoću tvorničke Azure podataka. |
| 47 | [Premještanje podataka iz MySQL pomoću tvorničke Azure podataka](data-factory-onprem-mysql-connector.md) | Saznajte više o premještanje podataka iz baze podataka MySQL pomoću tvorničke Azure podataka. |
| 48 | [Premještanje podataka iz lokalnog Oracle pomoću tvorničke Azure podataka](data-factory-onprem-oracle-connector.md) | Saznajte kako možete premjestiti podataka iz baze podataka tvrtke Oracle koja je lokalnog pomoću tvorničke Azure podataka. |
| 49 | [Premještanje podataka iz PostgreSQL pomoću tvorničke Azure podataka](data-factory-onprem-postgresql-connector.md) | Saznajte više o premještanje podataka iz baze podataka PostgreSQL pomoću tvorničke Azure podataka. |
| 50 | [Premještanje podataka iz Sybase pomoću tvorničke Azure podataka](data-factory-onprem-sybase-connector.md) | Saznajte više o premještanje podataka iz baze podataka Sybase pomoću tvorničke Azure podataka. |
| 51 | [Premještanje podataka iz tvrtke Teradata pomoću tvorničke Azure podataka](data-factory-onprem-teradata-connector.md) | Saznajte više o Teradata poveznik za servis tvorničke podataka koji omogućuje premještanje podataka iz baze podataka tvrtke Teradata |
| 52 | [Premještanje podataka za i SQL Server lokalnim ili na IaaS (Azure VM) pomoću tvorničke Azure podataka](data-factory-sqlserver-connector.md) | Saznajte kako premjestiti podatke iz baze podataka SQL Server koja je na lokalno ili na VM Azure pomoću tvorničke Azure podataka. |
| 53 | [Premještanje podataka iz web-tablice izvor pomoću tvorničke Azure podataka](data-factory-web-table-connector.md) | Saznajte kako premjestiti podatke iz lokalnih tablice u web-stranicu pomoću tvorničke Azure podataka. |



## <a name="data-transformation"></a>Transformaciju podataka

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 54 | [Stvaranje predvidljivu kanali pomoću Azure strojnog učenja aktivnosti](data-factory-azure-ml-batch-execution-activity.md) | U članku se opisuje kako stvoriti stvaranje predvidljivu kanali pomoću tvorničke Azure podataka i Azure strojnog učenja |
| 55 | [Povezani servisi za izračun](data-factory-compute-linked-services.md) | Informirajte se o računalnim enviornments da možete koristiti u Azure podataka tvorničke kanali pretvorbe/postupak podataka. |
| 56 | [Obradne velikih skupova podataka pomoću tvorničke podataka i serije](data-factory-data-processing-using-batch.md) | U članku se opisuje način obrade velikog količina podataka u kanalu na tvorničke Azure podataka pomoću paralelno obrada mogućnost grupe za Azure. |
| 57 | [Pretvaranje podataka u tvorničke Azure podataka](data-factory-data-transformation-activities.md) | Saznajte kako Pretvorba podatke ili podatke postupak u Azure podataka tvorničke pomoću Hadoop, strojnog učenja ili Azure podataka Lake analize. |
| 58 | [Hadoop strujanje aktivnosti](data-factory-hadoop-streaming-activity.md) | Saznajte kako koristiti Hadoop strujanje aktivnosti u na tvorničke Azure podataka za pokretanje programa Hadoop strujanje na na – zahtjev/vaše vlastite klaster za HDInsight. |
| 59 | [Grozd aktivnosti](data-factory-hive-activity.md) | Saznajte kako koristiti aktivnosti vrste Hive u na tvorničke Azure podataka pokrenuti grozd upita na – zahtjev/vaše vlastite klaster za HDInsight. |
| 60 | [Pozivanje MapReduce programe tvorničke podataka](data-factory-map-reduce.md) | Upute za obradu podataka tako da pokrenete MapReduce programe na programa Azure HDInsight klaster iz programa tvorničke Azure podataka. |
| 61 | [Svinja aktivnosti](data-factory-pig-activity.md) | Saznajte kako koristiti Svinja aktivnosti u na tvorničke Azure podataka da pokreću skripte Svinja na na – zahtjev/vaše vlastite klaster za HDInsight. |
| 62 | [Pozivanje Spark programe tvorničke podataka](data-factory-spark.md) | Saznajte kako pozvati Spark programe na tvorničke Azure podataka pomoću MapReduce aktivnosti. |
| 63 | [SQL Server pohranjene Procedure aktivnosti](data-factory-stored-proc-activity.md) | Saznajte kako možete koristiti u SQL Server pohranjene Procedure aktivnosti pozvati pohranjena procedura baze podataka SQL Azure ili Azure SQL Data Warehouse iz podataka tvorničke kanal. |
| 64 | [Korištenje prilagođene aktivnosti u kanalu na tvorničke Azure podataka](data-factory-use-custom-activities.md) | Saznajte kako stvoriti prilagođene aktivnosti i njihovu korištenju u kanalu na tvorničke Azure podataka. |
| 65 | [Pokretanje U SQL skripte na Lake analize podataka za Azure s tvorničke Azure podataka](data-factory-usql-activity.md) | Upute za obradu podataka tako da pokrenete U SQL skripte na servisu Azure podataka Lake analize računalnim. |



## <a name="samples"></a>Uzorci

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 66 | [Azure podataka tvorničke - uzorka](data-factory-samples.md) | Navedeni Detalji o uzorke koje se isporučuju sa servisom Azure podataka tvorničke. |



## <a name="use-cases"></a>Korištenje slučajeva

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 67 | [Studije slučaja klijenta](data-factory-customer-case-studies.md) | Saznajte kako neke od naših korisnika ste koristili tvorničke Azure podataka. |
| 68 | [Korištenje predmeta – kupca Profiliranje](data-factory-customer-profiling-usecase.md) | Saznajte korištenja tvorničke Azure podataka da biste stvorili utemeljenih na podacima tijek rada (kanal) profila igraće klijentima. |
| 69 | [Korištenje predmeta – preporuke proizvoda](data-factory-product-reco-usecase.md) | Saznajte više o slučaj koristi implementiran putem tvorničke Azure podataka zajedno s ostalim servisima. |



## <a name="monitor-and-manage"></a>Praćenje i upravljanje njima

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 70 | [Nadzor i upravljanje kanali tvorničke Azure podataka](data-factory-monitor-manage-pipelines.md) | Saznajte kako pomoću portala za Azure i Azure PowerShell za nadzor i upravljanje factories Azure podataka i kanali koje ste stvorili. |



## <a name="sdk"></a>SDK

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 71 | [Azure tvorničke podataka – Evidencija promjena .NET API](data-factory-api-change-log.md) | U članku se opisuje prijelom promjene, značajka dodaci, a zatim popravci programskih pogrešaka itd... u određenu verziju .NET API-JA za podataka tvorničke Azure. |
| 72 | [Stvaranje, praćenje i upravljanje factories Azure podataka pomoću podataka tvorničke .NET SDK-a](data-factory-create-data-factories-programmatically.md) | Saznajte kako programski stvaranje, praćenje i upravljanje factories Azure podataka pomoću SDK tvorničke podataka. |
| 73 | [Referenca za razvojne inženjere na tvorničke Azure podataka](data-factory-sdks.md) | Informirajte se o različitim načinima možete stvarati, praćenje i upravljanje factories Azure podataka |



## <a name="miscellaneous"></a>Različiti

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 74 | [Tvorničke Azure podataka – najčešća pitanja](data-factory-faq.md) | Najčešća pitanja o tvorničke Azure podataka. |
| 75 | [Azure podataka tvorničke - funkcije i sistemske varijable](data-factory-functions-variables.md) | Daje popis funkcije tvorničke Azure podataka i sistemske varijable |
| 76 | [Azure podataka tvorničke - imenovanja pravila](data-factory-naming-rules.md) | U članku se opisuje imenovanja pravila za entiteti tvorničke podataka. |
| 77 | [Otklanjanje poteškoća tvorničke podataka](data-factory-troubleshoot.md) | Saznajte kako otkloniti poteškoće vezane uz korištenje tvorničke Azure podataka. |

