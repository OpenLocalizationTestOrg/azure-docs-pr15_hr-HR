<properties     
    pageTitle="Azure podataka tvorničke - uzorka" 
    description="Navedeni Detalji o uzorke koje se isporučuju sa servisom Azure podataka tvorničke." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure podataka tvorničke - uzorka

## <a name="samples-on-github"></a>Uzorci na GitHub
[Spremište GitHub Azure DataFactory](https://github.com/azure/azure-datafactory) sadrži nekoliko primjera koje olakšavaju brzo krivulja gore sa servisom Azure podataka tvorničke (ili) izmjena skripte i koristiti u vlastitim aplikaciji. Mapa Samples\JSON sadrži JSON isječci za uobičajeni scenariji.

| Uzorak | Opis |
| :----- | :---------- | 
| [Vodič za ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | Ovaj primjer nudi do kraja do kraja vodič za obradu pomoću tvorničke Azure podataka da biste uključili podatke iz datoteka zapisnika uvid u datoteke zapisnika. <br/><br/>U ovom vodiču kanal tvorničke podataka prikuplja uzorka zapisnike, procesa obogaćuje podaci iz zapisnika s referentnih podataka i pretvorbe podataka za procjenu učinkovitosti marketinške kampanje koja nedavno pokrenuo. |
| [Uzorci JSON](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | Ovaj primjer nudi primjere JSON uobičajeni scenariji. | 
| [Uzorak Downloader HTTP podataka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | U ovom primjer showcases preuzimanje podataka iz krajnje HTTP spremište blobova platforme Azure pomoću prilagođene aktivnosti .NET. |
| [Unakrsni uzorka neto aktivnosti AppDomain točka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | Ovaj primjer omogućuje stvaranje prilagođene aktivnosti .NET koje je ograničeno na verzije skupa koristi pokretač ADF (na primjer, WindowsAzure.Storage v4.3.0 Newtonsoft.Json v6.0.x, itd.). |
| [Pokrenuti skriptu R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  Ovaj primjer uključuje podataka tvorničke prilagođene aktivnosti koje je moguće koristiti za pozivanje RScript.exe. Ovaj primjer funkcionira samo s vlastite klaster HDInsight (ne na zahtjev) koji je već instaliran R na njemu. |
| [Pozivanje Spark poslove na HDInsight Hadoop klaster](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | Ovaj primjer pokazuje kako pomoću aktivnosti MapReduce poziva Spark programa. Spark program samo kopira podatke iz jedne spremnik blobova platforme Azure na drugi. |
| [Analiza twitter pomoću Azure strojnog učenja obradu bilježenje rezultata aktivnosti](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | Ovaj primjer pokazuje kako koristiti AzureMLBatchScoringActivity za pozivanje modelu Azure strojnog učenja koji se izvodi analizu šalju twitter, bilježenje rezultata predviđanje itd. |
| [Analiza twitter pomoću prilagođene aktivnosti](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  Ovaj primjer pokazuje kako koristiti prilagođene aktivnosti .NET pozvati modelu Azure strojnog učenja koji se izvodi analizu šalju twitter, bilježenje rezultata predviđanje itd. |
| [S parametrima kanali za Azure strojno učenje](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | Uzorak daje do kraja do kraja C# kod za implementaciju kanali N za bilježenje rezultata i svaku s parametrom drugo područje u kojem popisom regija dolazi iz datoteke parameters.txt, koji se isporučuje se uz ovaj uzorak retraining. | 
| [Referenca za Azure strujanje analize poslove osvježavanje podataka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  Ovaj primjer pokazuje kako zajedničko korištenje web-mjesto tvorničke Azure podataka i u okvir za Azure strujanje Analytics za pokretanje upita s referentnih podataka i postavljanje osvježavanja podataka referenca na raspored. |
| [Hibridno kanal s lokalnim Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | Uzorak koristi na lokalni Hadoop klaster kao cilj računalnim za izvođenje zadataka na tvorničke podataka kao dodajte druge ciljeve računalnim kao što je HDInsight temelji klaster Hadoop u oblaku. |
| [Alat za pretvaranje JSON](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | Ovaj alat omogućuje pretvaranje JSONs iz verzije prije 2015-07-01 – pregled da biste najnovije ili 2015-07-01 – pregled (zadano). |  
| [U SQL oglednu datoteku za unos](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  Datoteka je oglednu datoteku koja se koristi U SQL aktivnost. | 

## <a name="azure-resource-manager-templates"></a>Predlošci Azure Voditelj resursa
O tvorničke podataka na Github, potražite sljedeće predloške za Azure Voditelj resursa. 

| Predložak | Opis |
| -------- | ----------- | 
| [Kopiranje iz spremišta blobova platforme Azure s bazom podataka Azure SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Implementacija ovaj predložak stvara na tvorničke Azure podataka s kanal koja se kopira podatke iz na navedeni blobova platforme Azure s bazom podataka Azure SQL |    
| [Kopiranje Salesforce blobova platforme Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Implementacija ovaj predložak stvara na tvorničke Azure podataka s kanal koja se kopira podatke iz navedeni račun servisa Salesforce spremište blobova platforme Azure. |    
| [Pretvaranje podataka tako da pokrenete grozd skripte na programa Azure HDInsight klaster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Implementacija ovaj predložak stvara na tvorničke Azure podataka s kanal koji pretvorbe podataka tako da pokrenete primjer grozd skripte na programa Azure HDInsight Hadoop klaster. | 


## <a name="samples-in-azure-portal"></a>Uzorci Azure portalu
Pločicu **uzorka kanali** na početnoj stranici na tvorničke podataka možete koristiti za implementaciju uzorka kanali i njihovih pridruženi entiteti (skupova podataka i povezani servisi) u tvorničke vaše podatke. 

1. Stvorite podataka tvorničke ili otvorite postojeću tvorničke za podatke. Potražite u članku [Uvod u rad s tvorničke Azure podataka](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) za korake da biste stvorili podataka tvorničke.
2. U plohu **TVORNIČKE podataka** za tvorničke podataka, kliknite pločicu **kanali uzorka** .

    ![Ogledna kanali pločica](./media/data-factory-samples/SamplePipelinesTile.png)

2. U plohu **kanali uzorka** kliknite **uzorka** koji želite uvesti. 
    
    ![Uzorak kanali plohu](./media/data-factory-samples/SampleTile.png)

3. Određivanje postavki konfiguriranje ogledne. Na primjer, vaš Azure račun imenom i računom ključa za pohranu, naziv poslužitelja Azure SQL, baze podataka, korisnički ID i lozinku itd. 

    ![Ogledna plohu](./media/data-factory-samples/SampleBlade.png)

4. Kada završite s određenim konfiguracijske postavke, kliknite **Stvori** za stvaranje/implementaciju uzorka kanali i povezane usluge/tablice koristi u kanali.
5. Prikaz statusa implementacije na pločici uzorka ste kliknuli ranije plohu **kanali uzorka** .

    ![Status uvođenja](./media/data-factory-samples/DeploymentStatus.png)

6. Kad se pojavi poruka **uspješno uvođenje** na pločici za uzorka, zatvorite plohu **kanali uzorka** .  
5. Na **TVORNIČKE podataka** plohu, vidjet ćete povezani servisi, skupova podataka i kanali dodaju na tvorničke vaše podatke.  

    ![Plohu tvorničke podataka](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>Uzorci u Visual Studio

### <a name="prerequisites"></a>Preduvjeti

Morate imati instaliran na vašem računalu sljedeće: 

- Visual Studio 2013 ili Visual Studio 2015.
- Preuzmite Azure SDK za Visual Studio 2013 ili Visual Studio 2015. Dođite do [Stranice za preuzimanje Azure](https://azure.microsoft.com/downloads/) i kliknite **Dodavanje veze za VANJSKIH 2013** ili **Dodavanje veze za VANJSKIH 2015** u odjeljku **.NET** .
- Preuzmite najnovije Azure podataka tvorničke dodatak za Visual Studio: [Dodavanje veze za VANJSKIH 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) ili [Dodavanje veze za VANJSKIH 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Ako koristite Visual Studio 2013, dodatak možete ažurirati tako da učinite sljedeće: na izborniku kliknite **Alati** -> **proširenja i ažuriranja** -> **Online** -> **Visual Studio Galerija** -> **Microsoft Azure podataka tvorničke Tools za Visual Studio** -> **Ažuriranje**.

### <a name="use-data-factory-templates"></a>Korištenje predložaka tvorničke podataka

1. Na izborniku kliknite **datoteka** , pokažite na **Novo**, a kliknite **projekt**. 
2. U dijaloškom okviru **Novi projekt** , učinite sljedeće: 
    1. Odaberite **DataFactory** u odjeljku **Predlošci**. 
    2. U desnom oknu odaberite **Predložaka tvorničke podataka** . 
    3. Unesite **naziv** za projekt. 
    4. Odaberite **mjesto** za projekt. 
    5. Kliknite **u redu**. 

    ![Dijaloški okvir za novi projekt](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. U dijaloškom okviru **Predložaka tvorničke podataka** odaberite oglednog predloška iz odjeljka **Koristi slučaj predložaka** pa kliknite **Dalje**. Sljedeći koraci će vas voditi kroz pomoću predloška za **Profiliranje klijenta** . Koraci su slične za ostale primjere. 

    ![Dijaloški okvir tvorničke predložaka podataka](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. U dijaloškom okviru **Konfiguriranje tvorničke podataka** na stranici **Osnove podatkovnih tvorničke** kliknite **Dalje** .
8. Na stranici **Konfiguracija podataka tvorničke** , učinite sljedeće: 
    1. Odaberite **Stvaranje nove tvorničke podataka**. Možete odabrati i **Korištenje postojećih podataka tvorničke**.
    2. Unesite **naziv** za tvorničke podataka.
    3. Odaberite **Azure pretplatu** u koju želite tvorničke podataka će biti stvoren. 
    4. Odaberite **grupu resursa** za tvorničke podataka.
    5. Odaberite **Zapad SAD -a**, **Istočni SAD -a**ili **Sjeverna Europa** **regija**.
    6. Kliknite **Dalje**. 
9. Na stranici **Konfiguracija podataka pohranjuje** navedite postojeće **baze podataka Azure SQL** i **račun za Azure prostora za pohranu** (ili) stvaranje baze podataka/prostora za pohranu, a zatim kliknite Dalje. 
10. Na stranici **Konfiguracija izračunati** odaberite zadane postavke, a zatim kliknite **Dalje**. 
11. Na stranici **Sažetak** pregledajte sve postavke, a zatim kliknite **Dalje**. 
12. Na stranici **Stanje implementacije** Pričekajte da se završi implementaciju, a zatim kliknite **Završi**.
13. Desnom tipkom miša kliknite projekt u pregledniku rješenja, a zatim kliknite **Objavi**. 
19. Ako se prikaže dijaloški okvir **prijavite se na Microsoftov račun** , unesite vjerodajnice za račun koji ima Azure pretplatu, a zatim kliknite **Prijava**.
20. Trebali biste vidjeti sljedeći dijaloški okvir:

    ![Dijaloški okvir objavljivanje](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Na stranici **Konfiguracija podataka tvorničke** učinite sljedeće: 
    1. Provjerite je li tu mogućnost **korištenja postojećih podataka tvorničke** .
    2. Odabir **podataka tvorničke** koji ste imali odabir prilikom korištenja predloška. 
    6. Kliknite **Dalje** da biste prešli na stranicu za **Objavljivanje stavki** . (Pritisnite **TAB** da biste premjestili iz polja Naziv da biste je gumb **Dalje** onemogućen). 
23. Na stranici **Objavljivanje stavki** Pobrinite se da sve na podataka Factories entiteti odabrane pa kliknite **Dalje** da biste prešli na stranicu **Sažetak** .     
24. Pregledajte sažetak i kliknite **Dalje** da biste započeli postupak implementacije i prikaz **Statusa implementacije**.
25. Na stranici **Stanje implementacije** trebali biste vidjeti status postupka implementacije. Po završetku implementaciju, kliknite Završi. 

Potražite u članku [Stvaranje prve podataka tvorničke (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) dodatne informacije o korištenju Visual Studio da biste izradili entiteti tvorničke podataka i njihovo objavljivanje na Azure.          