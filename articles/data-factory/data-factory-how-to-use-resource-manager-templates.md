<properties 
    pageTitle="Korištenje upravitelja resursa predlošci u tvorničke podataka | Microsoft Azure" 
    description="Saznajte kako stvoriti i koristiti Voditelj resursa Azure predloške da biste stvorili entiteti tvorničke podataka." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Koristite predloške da biste stvorili entiteti tvorničke Azure podataka

## <a name="overview"></a>Pregled
Prilikom korištenja tvorničke Azure podataka za integraciju s potrebama podataka može pronaći sami ponovno korištenje isti uzorak u različitim okruženjima ili implementacijom isti zadatak repetitively unutar iste rješenja. Predlošci vam implementirati i upravljanje sljedećim scenarijima jednostavan način. Predlošci u Azure podataka tvorničke idealne su za scenarija koji obuhvaćaju reusability i ponavljanja.
 
Razmislite o situaciji u kojoj tvrtki ili ustanovi ima 10 proizvodnje biljaka širom svijeta. Zapisnici iz svakog biljke spremaju se u zasebnom lokalne baze podataka SQL Server. Tvrtka želi da biste sastavili jedan podatkovni skladištu u oblaku za ad hoc analize. Također se želi imati isti logike, ali različitih konfiguracija za razvoj, testiranje i radnog okruženja. 

U ovom slučaju zadatka treba ponoviti isti okruženja, ali imaju različite vrijednosti preko factories 10 podatke za svaku biljke proizvodnje. Na snazi **ponavljanja** postoji. Templating omogućuje apstrakcije ovaj generički tijek (to jest, kanali imaju isti aktivnosti u svakom tvorničke podataka), ali koristi parametar zasebne datoteke za svaki biljke proizvodnje.

Osim toga, kao što je u tvrtki ili ustanovi želi uvesti te podatke 10 factories više puta u različitim okruženjima, predloške možete koristiti **reusability** pomoću parametar zasebne datoteke za razvoj, testiranje i radnog okruženja.

## <a name="templating-with-azure-resource-manager"></a>Templating s Azure Voditelj resursa
[Voditelj resursa Azure predlošci](../azure-resource-manager/resource-group-overview.md#template-deployment) su odličan način da biste postigli templating na tvorničke Azure podataka. Voditelj resursa predlošci definiraju Infrastruktura i konfiguraciji vašeg rješenja Azure pomoću JSON datoteke. Voditelj resursa Azure predložaka ne funkcioniraju pomoću servisa Azure sve/Većina, široko korištenja jednostavno upravljanje sve resurse Azure imovine. Pogledajte [Voditelj resursa za Azure za izradu predložaka](../resource-group-authoring-templates.md) da biste saznali više o predlošcima resursima Općenito. 

## <a name="tutorials"></a>Vodiči za
Sljedeći vodiči za detaljne upute za stvaranje entiteti tvorničke podataka pomoću predložaka Voditelj resursa u sljedećim člancima:

- [Praktični vodič: Stvaranje kanala da biste kopirali podatke pomoću predloška Azure Voditelj resursa](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Praktični vodič: Stvaranje kanal postupak podataka pomoću predloška Azure Voditelj resursa](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Predlošci tvorničke podataka na Github
Pogledajte sljedeće predloške za Azure brzi početak rada na Github: 

- [Stvaranje tvorničke podataka da biste kopirali podatke iz spremišta blobova platforme Azure s bazom podataka SQL Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Stvaranje podataka tvorničke s grozd aktivnost klaster Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Stvaranje tvorničke podataka da biste kopirali podatke iz Salesforce Azure blob-ova](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Slobodno zajedničko korištenje predložaka Azure podataka tvorničke pri [Azure brzi početak rada](https://azure.microsoft.com/documentation/templates/). Odnose se na [Vodič za doprinos](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) pri razvoju predloške koje možete zajednički koristiti putem ovo spremište. 

Sljedeći odjeljci sadrže detalje o definiranju tvorničke podataka resursa u predlošku Voditelj resursa. 

## <a name="defining-data-factory-resources-in-templates"></a>Definiranje tvorničke podataka resursa u predložaka
Najviše razine predložak za definiranje podataka tvorničke je:

    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
    {
        "name": "[parameters('dataFactoryName')]",
        "apiVersion": "[variables('apiVersion')]",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "westus",
        "resources": [
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Definiranje tvorničke podataka

Definiranje podataka tvorničke u predlošku resursima kao što prikazuje sljedeći primjer:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

Na dataFactoryName definirana je u "varijable" kao:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Definiranje povezani servisi 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Potražite u članku [Povezani servis za pohranu](data-factory-azure-blob-connector.md#azure-storage-linked-service) ili [Izračunati povezani servisi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) dodatne informacije o svojstvima JSON servisa za određene povezane koju želite uvesti. "DependsOn" parametar određuje naziv odgovarajuće tvorničke podataka. Primjer definiranje povezane servis za pohranu Azure prikazuju se u JSON definiciju sljedeće:

### <a name="define-datasets"></a>Definiranje skupova podataka

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Dodatne informacije o svojstvima JSON za vrstu određeni skup podataka koje želite uvesti odnose se na [podržani služi za pohranu podataka](data-factory-data-movement-activities.md#supported-data-stores-and-formats) . Napomena "dependsOn" parametar određuje naziv odgovarajuće tvorničke podataka i pohranu povezana servisa. Primjer koji definira skup podataka vrstu blobova platforme Azure prikazuju se u JSON definiciju sljedeće:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Definiranje kanali

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Pogledajte [koji definira kanali](data-factory-create-pipelines.md#pipeline-json) detalje o JSON svojstva za definiranje određeni kanal i aktivnosti koje želite uvesti. Imajte na umu "dependsOn" parametar određuje naziv tvorničke podataka i bilo koji odgovara povezanog services ili skupova podataka. Primjer kanala koja se kopira podatke iz spremišta blobova platforme Azure s bazom podataka SQL Azure je prikazano u sljedećim JSON isječak:

    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('azureSqlLinkedServiceName')]",
        "[variables('blobInputDatasetName')]",
        "[variables('sqlOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "activities": [
        {
            "name": "CopyFromAzureBlobToAzureSQL",
            "description": "Copy data frm Azure blob to Azure SQL",
            "type": "Copy",
            "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
            ],
            "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
            ],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink",
                    "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "columnMappings": "Column0:FirstName,Column1:LastName"
                }
            },
            "Policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }
        ],
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Predložak parameterizing tvorničke podataka
Za najbolje prakse na parameterizing potražite u članku [najbolje postupke za stvaranje predložaka Voditelj resursa Azure](../resource-manager-template-best-practices.md#parameters) članka. Općenito govoreći, korištenje parametar mora moguće minimizirati, osobito ako varijable može se koristiti umjesto toga. Samo pružaju parametara u sljedećim scenarijima:

- Postavke razlikuju se po okruženju (primjer: razvoj, testiranje i proizvodnje)
- Tajne (kao što su lozinke)

Ako morate povući tajne iz [Zbirke ključeva ključ Azure](../key-vault/key-vault-get-started.md) pri implementaciji entiteti tvorničke Azure podataka pomoću predložaka, odredite **tajnu naziv** i **ključne sigurnog** kao što je prikazano u sljedećem primjeru:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Prilikom izvoza predloške za postojeće podatke factories trenutno još nije podržan, on se otvara u funkcionira. 


