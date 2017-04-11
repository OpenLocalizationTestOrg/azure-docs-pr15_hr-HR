<properties
    pageTitle="Stvaranje prve podataka tvorničke (REST) | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču stvorite kanal za Azure podataka tvorničke uzorka pomoću podataka tvorničke REST API-JA."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Praktični vodič: Stvaranje vaš prvi tvorničke Azure podataka pomoću podataka tvorničke REST API-JA
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-build-your-first-pipeline.md)
- [Portal za Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Voditelj resursa predloška](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-JA](data-factory-build-your-first-pipeline-using-rest-api.md)

U ovom se članku da biste stvorili vaš prvi tvorničke Azure podataka pomoću podataka tvorničke REST API-JA.

## <a name="prerequisites"></a>Preduvjeti
- Pročitajte članak [Vodič](data-factory-build-your-first-pipeline.md) , a zatim dovršite korake **preduvjeta** .
- Instalirajte [Curl](https://curl.haxx.se/dlwiz/) na vašem računalu. Pomoću alata ZAKRETANJA s naredbama za OSTALE da biste stvorili podataka tvorničke. 
- Slijedite upute iz [članka](../resource-group-create-service-principal-portal.md) : 
    1. Stvaranje web-aplikacije pod nazivom **ADFGetStartedApp** servisa Azure Active Directory.
    2. Zatražite **ID klijenta** i **tajnu ključ**. 
    3. **ID klijenta**dobiti. 
    4. Dodijelite aplikacije **ADFGetStartedApp** ulogu **Suradnika tvorničke podataka** .  
- Instalirajte [Azure PowerShell](../powershell-install-configure.md).  
- Pokrenite **PowerShell** i pokrenite sljedeću naredbu. Ostavite Azure PowerShell otvorenu do kraja ovog praktičnog vodiča. Ako zatvorite i ponovno otvorite, morate je ponovno pokrenite naredbe.
    1. Pokrenite **Prijava AzureRmAccount** i unesite korisničko ime i lozinku koje koristite za prijavu na portal za Azure.  
    2. Pokrenite **Get-AzureRmSubscription** da biste pogledali sve pretplate za taj račun.
    3. Pokrenite **Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Postavljanje AzureRmContext** da biste odabrali pretplatu u koju želite raditi. Zamijenite **NameOfAzureSubscription** naziv pretplate za Azure. 
3. Stvaranje grupe Azure resursa pod nazivom **ADFTutorialResourceGroup** pokretanjem sljedeće naredbe u u ljusci PowerShell:  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Neke korake ovog praktičnog vodiča pretpostavlja da koristite grupu resursa pod nazivom ADFTutorialResourceGroup. Ako koristite grupu različite resursa, morate je nazovite grupu resursa umjesto ADFTutorialResourceGroup ovog praktičnog vodiča.

## <a name="create-json-definitions"></a>Stvaranje JSON definicija
Stvaranje pratiti JSON datoteke u mapu u kojoj se nalazi curl.exe. 

### <a name="datafactoryjson"></a>datafactory.JSON 
> [AZURE.IMPORTANT] Naziv mora biti globalno jedinstveni da bi se trebali biste prefiks/sufiks ADFCopyTutorialDF da biste ga jedinstveni naziv. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] Zamijenite **accountname** i **accountkey** ime i ključ računa za Azure prostora za pohranu. Da biste saznali kako dobiti pristup ključ za pohranu, potražite u članku [Prikaz, Kopiraj i regenerate prostora za pohranu pristupnih tipki](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Sljedeća tablica sadrži opise JSON svojstvima koji se koriste u u isječak:

| Svojstvo | Opis |
| :------- | :---------- |
| Verzija | Određuje stvaraju li se treba 3,2 verziju na HDInsight. | 
| ClusterSize | Veličina klaster HDInsight. | 
| TimeToLive | Određuje koje vrijeme neaktivnosti za klaster HDInsight prije nego što je izbrisana. |
| linkedServiceName | Određuje računa za pohranu koji se koristi za spremanje zapisnika koje generira HDInsight |

Imajte na umu sljedeće točke: 

- Tvorničke podataka HDInsight klaster **utemeljen na sustavu Windows** za vas stvara s iznad JSON. Nije moguće imate ga stvoriti HDInsight klaster **sustavom Linux** . Detalje potražite u članku [Osvježavati HDInsight povezane servisa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Koristite **vlastitu klaster HDInsight** umjesto korištenja programa klaster HDInsight na zahtjev. Detalje potražite u članku [Povezani servisa HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- HDInsight klaster stvara **zadani spremnika** u spremište blobova platforme koju ste naveli u JSON (**linkedServiceName**). HDInsight izbrisati ovaj spremnik nakon brisanja klaster. Ovo je zadano ponašanje dizajna. HDInsight povezana uslugom na zahtjev HDInsight klaster stvara se svaki put kada isječak obrađuje osim ako je postojeći live klaster (**timeToLive**), a briše se nakon dovršetka obrade.

    Dok se obrađuju više isječaka, vidjet ćete mnogo spremnika u Azure blobova. Ako ne morate ih za otklanjanje poteškoća s zadataka, preporučujemo vam da biste izbrisali im smanjiti resursa za pohranu. Imena tih spremnike uzorcima: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Da biste izbrisali spremnika u vašem blobova platforme Azure pomoću alata kao što je [Microsoft Explorer prostora za pohranu](http://storageexplorer.com/) .

Detalje potražite u članku [Osvježavati HDInsight povezane servisa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 

### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


Na JSON definira skup podataka pod nazivom **AzureBlobInput**, koji predstavlja ulaznih podataka za neku aktivnost u kanalu. Osim toga, navodi ulaznih podataka nalazi u blob spremnika koji se naziva **adfgetstarted** i mapu pod nazivom **inputdata**.

Sljedeća tablica sadrži opise JSON svojstvima koji se koriste u u isječak:

| Svojstvo | Opis |
| :------- | :---------- |
| Vrsta | Svojstvo vrsta postavljen je na AzureBlob jer se podaci nalaze u spremište blobova platforme Azure. |  
| linkedServiceName | odnosi se na StorageLinkedService koji ste prethodno stvorili. |
| Naziv datoteke | To svojstvo nije obavezno. Ako izostavite to svojstvo, sve datoteke iz na folderPath se izdvajaju. U ovom slučaju odvija se samo input.log. |
| Vrsta | Datoteke zapisnika su u tekstnom obliku, pa koristimo TextFormat. | 
| columnDelimiter | Stupci u datoteke zapisnika su zarezima znakom zarez () |
| Učestalost/interval | Učestalost postavite na mjesec i interval je 1, što znači unos isječke dostupnosti mjesečno. | 
| Vanjski | Ovo svojstvo postavljen na vrijednost true ako ulaznih podataka ne generira servis tvorničke podataka. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }

Na JSON definira skup podataka pod nazivom **AzureBlobOutput**, koja predstavlja podatke za neku aktivnost u kanalu. Osim toga, navodi rezultate pohranjuju u blob spremnika koji se naziva **adfgetstarted** i mapu pod nazivom **partitioneddata**. U odjeljku **dostupnost** određuje da su dataset izlaz proizvodi mjesec.

### <a name="pipelinejson"></a>pipeline.JSON
> [AZURE.IMPORTANT] Zamijenite **storageaccountname** brzih dijelova Azure prostora za pohranu. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

U isječak JSON stvarate kanal koji se sastoji od jedne aktivnosti koje koristi grozd postupak podacima na HDInsight klaster.

Datoteka skripte grozd, **partitionweblogs.hql**pohranjuju se u račun za Azure prostora za pohranu (određen scriptLinkedService, pod nazivom **StorageLinkedService**) i mapa za **skripte** u spremniku **adfgetstarted**.

U odjeljku **definira** određuje runtime postavke koje prosljeđuju skripte grozd kao grozd konfiguracijskih vrijednosti (npr ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

**Početak** i **Kraj** svojstva kanal određuje aktivni razdoblje kanal.

U aktivnosti JSON, navedite skripte grozd pokreće se na računalnim naveo u **linkedServiceName** – **HDInsightOnDemandLinkedService**.

> [AZURE.NOTE] Detalje o svojstvima JSON koji se koriste u prethodnom primjeru potražite u članku [Anatomy na kanal](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 

## <a name="set-global-variables"></a>Postavljanje globalne varijable

U Azure PowerShell nakon zamjenu vrijednosti s vlastitim izvršiti sljedeće naredbe:

> [AZURE.IMPORTANT] Smjernice za sekcije [preduvjete](#prerequisites) potražite u članku upute za početak ID klijenta, tajna klijent ID i ID pretplate.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>Provjeru s AAD

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Stvaranje tvorničke podataka

U ovom ćete koraku stvoriti tvorničke podataka Azure koja se naziva **FirstDataFactoryREST**. Podaci tvorničke može imati jednu ili više kanali. Na kanal može imati jednu ili više aktivnosti u njoj. Na primjer, Kopiraj aktivnost da biste kopirali podatke iz izvora Odredišno spremište podataka i HDInsight grozd aktivnosti da biste pokrenuli grozd skripta za pretvaranje podataka. Pokrenite sljedeće naredbe za stvaranje tvorničke podataka: 

1. Naredba dodijeliti varijablu pod nazivom **cmd**. 

    Provjerite je li naziv tvorničke podataka koje navedete (ADFCopyTutorialDF) odgovara nazivu naveden u **datafactory.json**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Pokrenite naredbu pomoću **Pozovite naredbe**.

        $results = Invoke-Command -scriptblock $cmd;
3. Prikaz rezultata. Ako tvorničke podataka uspješno je stvorena, pročitajte članak JSON za tvorničke podataka u **rezultatima**; u suprotnom, vidite poruku o pogrešci.  

        Write-Host $results

Imajte na umu sljedeće točke:
 
- Naziv podataka tvorničke Azure mora biti globalno jedinstveni. Ako se prikaže poruka u rezultatima: **nije dostupna je naziv tvorničke podataka "FirstDataFactoryREST"**, učinite sljedeće:  
    1. Promijenite naziv (na primjer, yournameFirstDataFactoryREST) u datoteci **datafactory.json** . Pogledajte temu [Tvorničke podataka – pravila imenovanja](data-factory-naming-rules.md) za imenovanja pravila za artefakte tvorničke podataka.
    2. U prvi naredbu gdje varijabla **$cmd** se dodjeljuje vrijednost, zamijenite FirstDataFactoryREST pod novim nazivom i pokrenite naredbu. 
    3. Pokrenite sljedeće dvije naredbe za pozivanje REST API-JA za stvaranje tvorničke podataka i ispis rezultata operacije. 
- Da biste stvorili instance tvorničke podataka, morate biti suradnik/administrator Azure pretplate
- Naziv tvorničke podataka možda registrirana kao naziv DNS-a u budućnosti i zato postaju javno vidljivi.
- Ako vam se prikaže pogreška: "**Da biste koristili polje naziva Microsoft.DataFactory nije registriran ove pretplate**", učinite nešto od sljedećeg i pokušajte ponovno objaviti: 

    - U Azure PowerShell pokrenite sljedeću naredbu da biste registrirali davatelja tvorničke podataka: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Možete pokrenite sljedeću naredbu da biste potvrdili koji tvorničke podataka koji je registriran davatelja usluga: 
    
            Get-AzureRmResourceProvider
    - Prijavite se pomoću Azure pretplate na [portal za Azure](https://portal.azure.com) i idite na tvorničke podataka plohu (ili) stvaranje podataka tvorničke na portalu za Azure. Ova akcija automatski registrira davatelj usluga za vas.

Prije stvaranja na kanal, morate najprije stvoriti nekoliko entiteti tvorničke podataka. Najprije stvorite povezani servisi formula izračunava/podataka trgovine povezati spremište podataka, definiranje ulazni i izlazni skupove podataka da biste predstavili podatke u povezane podatke trgovine. 

## <a name="create-linked-services"></a>Stvorite povezani servisi 
U ovom ćete koraku povežete račun za Azure prostora za pohranu i programa klaster Azure HDInsight na zahtjev na tvorničke podataka. Račun za Azure pohranu sadrži podatke ulazni i izlazni za kanal u ovom primjeru. Servis za HDInsight povezana koristi se za pokrenuti skriptu grozd naveden u aktivnosti kanal u ovom primjeru. 

### <a name="create-azure-storage-linked-service"></a>Stvaranje servisa Azure prostora za pohranu povezana
U ovom ćete koraku povezati s računom za pohranu Azure na tvorničke vaše podatke. Pomoću ovog praktičnog vodiča koristiti isti prostor za pohranu Azure račun za pohranu podataka ulaza i izlaza i HQL skriptna datoteka.

1. Naredba dodijeliti varijablu pod nazivom **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Pokrenite naredbu pomoću **Pozovite naredbe**.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Prikaz rezultata. Ako povezane usluge uspješno je stvorena, pročitajte članak JSON za servis povezane **rezultate**; u suprotnom, vidite poruku o pogrešci.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Stvaranje servisa Azure HDInsight povezana
U ovom ćete koraku veza do klaster HDInsight na zahtjev na tvorničke vaše podatke. HDInsight Klaster se automatski stvara prilikom izvođenja i izbrisati nakon što to učiniti obradi i neaktivnosti za određeno vrijeme. Koristite vlastitu klaster HDInsight umjesto korištenja programa klaster HDInsight na zahtjev. Detalje potražite u članku [Izračunati povezani servisi](data-factory-compute-linked-services.md) .  

1. Naredba dodijeliti varijablu pod nazivom **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Pokrenite naredbu pomoću **Pozovite naredbe**.

        $results = Invoke-Command -scriptblock $cmd;
3. Prikaz rezultata. Ako povezane usluge uspješno je stvorena, pročitajte članak JSON za servis povezane **rezultate**; u suprotnom, vidite poruku o pogrešci.  

        Write-Host $results

## <a name="create-datasets"></a>Stvaranje skupova podataka
U ovom ćete koraku stvoriti skupova podataka za predstavljanje unos i poslati podatke za obradu grozd. Ove skupove podataka odnose se na **StorageLinkedService** koji ste stvorili ranije u ovom praktičnom vodiču. Povezane servisa upućuje na račun za Azure prostora za pohranu i skupova podataka odredite spremnik, mapu, a zatim naziv datoteke u prostora za pohranu koja sadrži unos i poslati podatke.   

### <a name="create-input-dataset"></a>Stvaranje unosa skup podataka
U ovom ćete koraku stvoriti unos dataset predstavlja ulazne podatke pohranjene u spremište blobova platforme Azure.

1. Naredba dodijeliti varijablu pod nazivom **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Pokrenite naredbu pomoću **Pozovite naredbe**.

        $results = Invoke-Command -scriptblock $cmd;
3. Prikaz rezultata. Ako skup podataka uspješno je stvorena, pročitajte članak JSON za skup podataka u **rezultatima**; u suprotnom, vidite poruku o pogrešci.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Stvaranje skupa podataka za izlaz
U ovom ćete koraku stvoriti izlazni skup podataka da biste predstavili podatke pohranjene u spremište blobova platforme Azure.

1. Naredba dodijeliti varijablu pod nazivom **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Pokrenite naredbu pomoću **Pozovite naredbe**.

        $results = Invoke-Command -scriptblock $cmd;
3. Prikaz rezultata. Ako skup podataka uspješno je stvorena, pročitajte članak JSON za skup podataka u **rezultatima**; u suprotnom, vidite poruku o pogrešci.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Stvaranje kanala
U ovom ćete koraku stvoriti na prvom kanal s **HDInsightHive** aktivnosti. Unos isječak dostupna mjesečno (učestalost: mjesec, interval: 1), izlazna isječak mjesečno proizvodi i svojstvo raspored aktivnosti i postavljeno na mjesečno. Postavke za skup podataka izlazne i raspored aktivnosti moraju se podudarati. Trenutno dataset izlaz je što pogoni rasporedu, tako da je izlazni skup podataka morate stvoriti čak i ako se aktivnosti neće proizvesti bilo kakav izlaz. Ako želite da aktivnosti ne svi se unosi, možete preskočiti stvaranje unosa skupu podataka.  

Provjerite potražite u članku **input.log** datoteka u mapi **adfgetstarted/inputdata** u spremište blobova platforme Azure i pokrenite sljedeću naredbu za implementaciju kanal. Budući da se vrijeme **početka** i **završetka** postavljaju u prošlosti, a **isPaused** postavljen na false, kanala (aktivnosti u kanalu) pokreće odmah nakon implementacije. 

1. Naredba dodijeliti varijablu pod nazivom **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Pokrenite naredbu pomoću **Pozovite naredbe**.

        $results = Invoke-Command -scriptblock $cmd;
3. Prikaz rezultata. Ako skup podataka uspješno je stvorena, pročitajte članak JSON za skup podataka u **rezultatima**; u suprotnom, vidite poruku o pogrešci.  

        Write-Host $results
5. Čestitamo, uspješno ste stvorili na prvi kanal pomoću komponente PowerShell Azure!

## <a name="monitor-pipeline"></a>Monitor kanal
U ovom ćete koraku koristiti podatke tvorničke REST API-JA za nadzor isječke osnovu kanal.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Stvaranje programa klaster HDInsight osvježavati obično traje tijekom (otprilike 20 minuta). Stoga očekivati kanal za **približno 30 minuta** za obradu isječak.  

Pokrenite naredbu pozovite i sljedeći dok se ne prikaže isječak u stanju **spremno** ili stanje **nije uspjelo** . Kada je isječak u stanju spremno, provjerite mapu **partitioneddata** u spremniku **adfgetstarted** u spremištu blobova za izlazne podatke.  Stvaranje programa klaster HDInsight osvježavati obično traje neko vrijeme.

![izlazne podatke](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Kada je isječak uspješno obrađuju se briše ulazne datoteke. Dakle, ako želite ponovno pokrenite isječak ili ponovno učinite vodič, prijenos unos datoteke (input.log) u mapu inputdata spremnika adfgetstarted.

Portal za Azure možete koristiti i praćenje isječaka i otklanjanje poteškoća. [Praćenje kanali pomoću portala za Azure](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) detalje potražite u članku.  

## <a name="summary"></a>Sažetak 
U ovom ćete praktičnom vodiču ste stvorili na tvorničke Azure podataka s podacima postupak tako da pokrenete grozd skripte na klaster hadoop HDInsight. Koristi tvorničke uređivač podataka na portalu za Azure poduzeti sljedeće korake:  

1.  Stvara Azure **tvorničke podataka**.
2.  Stvara dva **povezani servisi**:
    1.  **Prostor za pohranu Azure** povezana servisa za povezivanje vašeg blobova platforme Azure koja sadrži datoteke ulaza i izlaza na tvorničke podataka.
    2.  Servis povezane **Azure HDInsight** na zahtjev za povezivanje programa klaster HDInsight Hadoop osvježavati tvorničke podataka. Azure tvorničke podataka stvara HDInsight Hadoop klaster samo-na-jedan za obradu ulaznih podataka i dobivanje izlazne podatke. 
3.  Stvara dvaju **skupova podataka**, koji opisuje ulazni i izlazni podaci za HDInsight grozd aktivnosti u kanalu. 
4.  Da biste stvorili **kanal** s **HDInsight grozd** aktivnosti. 

## <a name="next-steps"></a>Daljnji koraci
U ovom se članku ste stvorili na kanal s transformaciju aktivnosti (HDInsight aktivnosti) koja se izvršava grozd skripte na programa klaster Azure HDInsight na zahtjev. Da biste vidjeli kako koristiti kopiju aktivnosti da biste kopirali podatke iz programa blobova platforme Azure Azure SQL, potražite u članku [Praktični vodič: kopiranje podataka iz programa blobova platforme Azure Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Vidi također
| Tema | Opis |
| :---- | :---- |
| [Podaci tvorničke REST API Reference](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Potražite u dokumentaciji potpun na tvorničke podataka cmdleta |
| [Aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) | Ovaj članak sadrži popis aktivnosti transformacije podataka (primjerice HDInsight grozd transformaciju ste koristili u ovom ćete praktičnom vodiču) podržava tvorničke Azure podataka. |
| [Planiranje i izvođenja](data-factory-scheduling-and-execution.md) | U ovom se članku objašnjava zakazivanja i izvođenja aspekte Azure podataka tvorničke modelu. |
| [Kanali](data-factory-create-pipelines.md) | U ovom se članku olakšava razumijevanje kanali i aktivnosti u tvorničke Azure podataka i kako ih koristiti za sastavljanje završetka do kraja utemeljenih na podacima tijekova rada za scenarija ili tvrtke. |
| [Skupove podataka](data-factory-create-datasets.md) | U ovom se članku olakšava razumijevanje skupova podataka na tvorničke Azure podataka.
| [Nadzor i upravljanje kanali pomoću Azure blades portala](data-factory-monitor-manage-pipelines.md) | U ovom se članku opisuje kako nadzor, upravljanje i vaše kanali pomoću Azure blades portala za ispravljanje pogrešaka. |
| [Nadzor i upravljanje kanali pomoću aplikacije za praćenje](data-factory-monitor-manage-app.md) | U ovom se članku opisuje kako nadzor, upravljanje i kanali pomoću nadzor i upravljanje aplikacija za ispravljanje pogrešaka. 

