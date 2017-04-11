<properties
    pageTitle="Stvaranje prve podataka tvorničke (Voditelj resursa predložak) | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču stvorite kanal za Azure podataka tvorničke uzorka pomoću predloška Azure Voditelj resursa."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Praktični vodič: Stvaranje vaš prvi tvorničke Azure podataka pomoću predloška Azure Voditelj resursa
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-build-your-first-pipeline.md)
- [Portal za Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Voditelj resursa predloška](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-JA](data-factory-build-your-first-pipeline-using-rest-api.md)

U ovom se članku predložak Voditelj resursa Azure koristite da biste stvorili vaš prvi tvorničke Azure podataka.

## <a name="prerequisites"></a>Preduvjeti
- Pročitajte članak [Vodič](data-factory-build-your-first-pipeline.md) , a zatim dovršite korake **preduvjeta** .
- Slijedite upute u članku [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md) da biste instalirali najnoviju verziju programa Azure PowerShell na vašem računalu.
- Pogledajte [Za izradu predložaka Azure resursima](../resource-group-authoring-templates.md) dodatne informacije o predlošcima Azure Voditelj resursa. 

## <a name="in-this-tutorial"></a>Pomoću ovog praktičnog vodiča
Entitet | Opis  
------ | ----------- 
Azure servis za pohranu povezana | Račun za Azure pohranu veze na tvorničke podataka. Račun za Azure pohranu sadrži podatke ulazni i izlazni za kanal u ovom primjeru. 
Povezane usluge na zahtjev za HDInsight| Veza na HDInsight osvježavati skupine na tvorničke podataka. Klaster automatski se stvara u podacima procesa, a briše se po završetku obrade.
Azure Blob unos dataset | Odnosi se na servis za pohranu Azure povezani. Servis povezane se odnosi na račun za Azure prostora za pohranu, a dataset blobova platforme Azure određuje kontejner, mapa i naziv datoteke u spremište koja sadrži ulazne podatke. 
Blobova platforme Azure izlazni skup podataka | Odnosi se na servis za pohranu Azure povezani. Servis za povezane se odnosi na račun za Azure prostora za pohranu, a dataset blobova platforme Azure određuje kontejner, mapa i naziv datoteke u spremište koja sadrži podatke. 
Kanal podataka | Kanal postoji aktivnosti vrste HDInsightHive troši unos skup podataka i daje izlazni skup podataka.   

Podaci tvorničke može imati jednu ili više kanali. Na kanal može imati jednu ili više aktivnosti u njoj. Postoje dvije vrste aktivnosti: [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) i [aktivnosti transformacije podataka](data-factory-data-transformation-activities.md). U ovom ćete praktičnom vodiču stvorite na kanal s jednog aktivnosti (aktivnosti Kopiraj).

Sljedeći odjeljak sadrži potpuni predložak resursima za definiranje podataka tvorničke entiteti tako da možete brzo pokretanje kroz vodič i testirati predložak. Da biste razumjeli kako se svaki entitet podataka tvorničke definirati, potražite u članku [entiteti tvorničke podataka u predlošku](#data-factory-entities-in-the-template) sekcije.

## <a name="data-factory-json-template"></a>Predložak tvorničke JSON podataka
Predložak najviše razine resursima za definiranje podataka tvorničke je: 

    {
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
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Stvaranje JSON datoteku pod nazivom **ADFTutorialARM.json** u mapi **C:\ADFGetStarted** pomoću sljedećeg sadržaja:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
        },
        "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
            {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureStorage",
                    "description": "Azure Storage linked service",
                    "typeProperties": {
                        "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                    }
                }
            },
            {
                "type": "linkedservices",
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
                    }
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
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
            },
            {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]",
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Drugi primjer Voditelj resursa predložak možete pronaći za stvaranje na tvorničke Azure podataka na [Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću predloška Azure Voditelj resursa](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>Parametri JSON 
Stvaranje JSON datoteku pod nazivom **ADFTutorialARM Parameters.json** sadrži parametara predloška Azure Voditelj resursa.  

> [AZURE.IMPORTANT] Navedite naziv i ključ računa za pohranu Azure **storageAccountName** i **storageAccountKey** parametara u ovoj datoteci parametar. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Možda ćete imati zasebne parametar JSON datoteka za razvoj i testiranje radnog okruženja koje možete koristiti s isti predložak JSON tvorničke podataka. Pomoću skripte ljuske Power možete automatizirati implementacija entiteti tvorničke podataka u te okruženja. 

## <a name="create-data-factory"></a>Stvaranje tvorničke podataka

1. Pokretanje **Programa PowerShell Azure** i pokrenite sljedeću naredbu: 
    - Pokrenite `Login-AzureRmAccount` , a zatim unesite korisničko ime i lozinku koje koristite za prijavu na portal za Azure.  
    - Pokrenite `Get-AzureRmSubscription` da biste pogledali sve pretplate za taj račun.
    - Pokrenite `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` da biste odabrali pretplatu u koju želite raditi. Ove pretplate mora biti jednak onome koji ste koristili na portalu za Azure.
1. Pokrenite sljedeću naredbu za implementaciju entiteti tvorničke podataka pomoću predloška za Voditelj resursa koji ste stvorili u koraku 1. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor kanal
 
1.  Nakon prijave na [portal za Azure](https://portal.azure.com/), kliknite **Pregledaj** i odaberite **factories podataka**.
        ![Pregled -> factories podataka](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  U plohu **Factories podataka** kliknite tvorničke podataka (**TutorialFactoryARM**) koju ste stvorili.   
2.  U plohu **Tvorničke podataka** za vaše podatke tvorničke, kliknite **dijagrama**.
        ![Pločica dijagrama](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  U **Prikazu dijagrama**, pogledajte pregled kanali i skupova podataka koristi ovog praktičnog vodiča.
    
    ![Prikaz dijagrama](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. U prikazu dijagrama dvokliknite dataset **AzureBlobOutput**. Koji se prikaže isječak koji se trenutno obrađuju.

    ![Skup podataka](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Kada završi obrada, vidjet ćete isječak u stanju **spremno** . Stvaranje programa klaster HDInsight osvježavati obično traje tijekom (otprilike 20 minuta). Stoga očekivati kanal za **približno 30 minuta** za obradu isječak.

    ![Skup podataka](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Kada je isječak u stanju **spremno** , provjerite mapu **partitioneddata** u spremniku **adfgetstarted** u spremištu blobova za izlazne podatke.  

Potražite u članku [Monitor skupova podataka i kanal](data-factory-monitor-manage-pipelines.md) za upute o tome kako koristiti Azure blades portala za praćenje kanal i skupova podataka koji ste stvorili u ovom ćete praktičnom vodiču.

Možete koristiti i nadzor i upravljanje aplikaciju za praćenje kanali vaše podatke. Potražite u članku [nadzor i upravljanje kanali tvorničke Azure podataka pomoću aplikacije za nadzor](data-factory-monitor-manage-app.md) dodatne informacije o korištenju aplikacije. 

> [AZURE.IMPORTANT] Kada je isječak uspješno obrađuju se briše ulazne datoteke. Dakle, ako želite ponovno pokrenite isječak ili ponovno učinite vodič, prijenos unos datoteke (input.log) u mapu inputdata spremnika adfgetstarted.

## <a name="data-factory-entities-in-the-template"></a>Entiteti tvorničke podatke u predložak
### <a name="define-data-factory"></a>Definiranje tvorničke podataka
Definiranje podataka tvorničke u predlošku resursima kao što prikazuje sljedeći primjer:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

U dataFactoryName se definira kao: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

To je jedinstveni niza na temelju ID resursa za grupu.  

### <a name="defining-data-factory-entities"></a>Definiranje entiteti tvorničke podataka
Sljedeći podaci tvorničke entiteti su definirani u predlošku JSON: 

- [Azure servis za pohranu povezana](#azure-storage-linked-service)
- [Povezane usluge na zahtjev za HDInsight](#hdinsight-on-demand-linked-service)
- [Unos dataset blobova platforme Azure](#azure-blob-input-dataset)
- [Skup podataka za izlaz blobova platforme Azure](#azure-blob-output-dataset)
- [Kanal podataka s Kopiraj aktivnosti](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure servis za pohranu povezana
Navedite naziv i ključ računa za Azure prostora za pohranu u ovom odjeljku. Potražite u članku [Azure prostora za pohranu povezana servisa](data-factory-azure-blob-connector.md#azure-storage-linked-service) detalje o svojstvima JSON koji se koriste za definiranje servis za pohranu Azure povezani. 

      {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureStorage",
          "description": "Azure Storage linked service",
          "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
          }
        }
      }

**ConnectionString** koristi storageAccountName i storageAccountKey parametre. Vrijednosti za ove parametara pomoću konfiguracijska datoteka. Definicija koristi i varijable: azureStroageLinkedService i dataFactoryName definiran u predlošku. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>Povezane usluge na zahtjev za HDInsight
[Povezani servisi za izračun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) članku potražite u članku dodatne informacije o svojstvima JSON koji se koriste za definiranje na servis povezane HDInsight na zahtjev.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Imajte na umu sljedeće točke: 

- Tvorničke podataka HDInsight klaster **utemeljen na sustavu Windows** za vas stvara s iznad JSON. Nije moguće imate ga stvoriti HDInsight klaster **sustavom Linux** . Detalje potražite u članku [Osvježavati HDInsight povezane servisa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Koristite **vlastitu klaster HDInsight** umjesto korištenja programa klaster HDInsight na zahtjev. Detalje potražite u članku [Povezani servisa HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- HDInsight klaster stvara **zadani spremnika** u spremište blobova platforme koju ste naveli u JSON (**linkedServiceName**). HDInsight izbrisati ovaj spremnik nakon brisanja klaster. Ovo je zadano ponašanje dizajna. HDInsight povezana uslugom na zahtjev HDInsight klaster stvara se svaki put kad isječak treba obraditi osim ako je postojeći live klaster (**timeToLive**), a briše se nakon dovršetka obrade.

    Dok se obrađuju više isječaka, vidjet ćete mnogo spremnika u Azure blobova. Ako ne morate ih za otklanjanje poteškoća s zadataka, preporučujemo vam da biste izbrisali im smanjiti resursa za pohranu. Imena tih spremnike uzorcima: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Da biste izbrisali spremnika u vašem blobova platforme Azure pomoću alata kao što je [Microsoft Explorer prostora za pohranu](http://storageexplorer.com/) .

Detalje potražite u članku [Osvježavati HDInsight povezane servisa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .



#### <a name="azure-blob-input-dataset"></a>Unos dataset blobova platforme Azure
Navedite imena blob spremnik, mape i datoteke koja sadrži ulazne podatke. Detalje o svojstvima JSON koji se koriste za definiranje programa blobova platforme Azure skup podataka potražite u članku [blobova platforme Azure svojstava za skup podataka](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) . 

      {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Ove definicije koristi sljedećih parametara definiran u predlošku parametar: blobContainer, inputBlobFolder, i inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Blobova platforme Azure izlazni skup podataka
Navedite imena blob spremnik i mapu koja sadrži podatke. Detalje o svojstvima JSON koji se koriste za definiranje programa blobova platforme Azure skup podataka potražite u članku [blobova platforme Azure svojstava za skup podataka](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
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

U ovom definicija koristi sljedećih parametara definiran u predlošku parametar: blobContainer i outputBlobFolder. 

#### <a name="data-pipeline"></a>Kanal podataka
Definiranje kanal koji pretvaranje podataka tako da pokrenete grozd skripte na programa klaster Azure HDInsight na zahtjev. Opisi elemenata JSON koristi za definiranje na kanal u ovom primjeru potražite u članku [JSON kanal](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>Ponovno korištenje predloška 
U ovom praktičnom vodiču, stvorite predložak za definiranje podataka tvorničke entiteti i predloška za prosljeđivanje vrijednosti za parametre. Da biste koristili isti predložak za implementaciju entiteti tvorničke podataka u različitim okruženjima, stvoriti datoteku parametar za svako okruženje te ga koristiti pri implementaciji njegovom okruženju.     

Primjer:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Obratite pozornost na to da prve naredbe koristi parametar datoteka za razvojno okruženje, drugu test okruženju, a treći nešto radnom okruženju.  

Možete koristiti i predložak za obavljanje zadataka koji se ponavljaju. Na primjer, morate stvoriti mnogo factories podataka s jednog ili više kanali koje implementirati isti logike, ali svaki tvorničke podataka koristi baze podataka SQL Azure računi i drugi Azure prostor za pohranu. U ovom scenariju koristite isti predložak u istom okruženju (razvojni, testiranje ili radni) s različitim parametar datoteke da biste stvorili factories podataka. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Voditelj resursa predloška za stvaranje pristupnika
Evo oglednog resursima predloška za stvaranje logičke pristupnika na stražnjoj. Instalirajte pristupnika na lokalno ili Azure IaaS VM i registrirati pristupnik sa servisom tvorničke podataka putem ključa. Potražite u članku [Premještanje podataka između lokalnog i oblaka](data-factory-move-data-between-onprem-and-cloud.md) detalje.

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Ovaj predložak stvara podataka tvorničke pod nazivom GatewayUsingArmDF s pristupnika pod nazivom: GatewayUsingARM. 

## <a name="see-also"></a>Vidi također
| Tema | Opis |
| :---- | :---- |
| [Aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) | Ovaj članak sadrži popis aktivnosti transformacije podataka (primjerice HDInsight grozd transformaciju ste koristili u ovom ćete praktičnom vodiču) podržava tvorničke Azure podataka. |
| [Planiranje i izvođenja](data-factory-scheduling-and-execution.md) | U ovom se članku objašnjava zakazivanja i izvođenja aspekte Azure podataka tvorničke modelu. |
| [Kanali](data-factory-create-pipelines.md) | U ovom se članku olakšava razumijevanje kanali i aktivnosti u tvorničke Azure podataka i kako ih koristiti za sastavljanje završetka do kraja utemeljenih na podacima tijekova rada za scenarija ili tvrtke. |
| [Skupove podataka](data-factory-create-datasets.md) | U ovom se članku olakšava razumijevanje skupova podataka na tvorničke Azure podataka.
| [Nadzor i upravljanje kanali pomoću aplikacije za praćenje](data-factory-monitor-manage-app.md) | U ovom se članku opisuje kako nadzor, upravljanje i kanali pomoću nadzor i upravljanje aplikacija za ispravljanje pogrešaka. 

  

