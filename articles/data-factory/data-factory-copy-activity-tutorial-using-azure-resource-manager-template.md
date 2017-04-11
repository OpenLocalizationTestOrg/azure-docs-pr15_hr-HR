<properties
    pageTitle="Praktični vodič: Stvaranje kanala pomoću predloška za resursima | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču stvorite kanal na tvorničke podataka Azure s Kopiraj aktivnosti pomoću predloška Azure Voditelj resursa."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću predloška Azure Voditelj resursa
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopiranje čarobnjaka](data-factory-copy-data-wizard-tutorial.md)
- [Portal za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Predloška Azure Voditelj resursa](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-JA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API-JA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Pomoću ovog praktičnog vodiča pokazuje kako stvoriti i nadzirati na tvorničke Azure podataka pomoću predloška Azure Voditelj resursa. Kanal na tvorničke podataka kopira podatke iz spremišta blobova platforme Azure s bazom podataka SQL Azure.

## <a name="prerequisites"></a>Preduvjeti
- Prođite kroz [vodič pregled i preduvjetima](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) i dovršite korake **preduvjeta** .
- Slijedite upute u članku [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md) da biste instalirali najnoviju verziju programa Azure PowerShell na vašem računalu. U ovom ćete praktičnom vodiču za implementaciju entiteti tvorničke podataka pomoću komponente PowerShell. 
- (neobavezno) Pogledajte [Za izradu predložaka Azure resursima](../resource-group-authoring-templates.md) dodatne informacije o predlošcima Azure Voditelj resursa.


## <a name="in-this-tutorial"></a>Pomoću ovog praktičnog vodiča

U ovom ćete praktičnom vodiču stvoriti podataka tvorničke s sljedeće entiteti tvorničke podataka:

Entitet | Opis  
------ | ----------- 
Azure servis za pohranu povezana | Račun za Azure pohranu veze na tvorničke podataka. Azure prostora za pohranu je spremište izvora podataka i baze podataka Azure SQL je primatelj spremišta podataka za aktivnost Kopiraj u ovom praktičnom vodiču. Navodi račun za pohranu koji sadrži ulazne podatke za aktivnost Kopiraj. 
Azure povezana SQL baze podataka usluge| Povezuje baze podataka Azure SQL tvorničke podataka. Određuje baze podataka Azure SQL koja sadrži podatke za aktivnost Kopiraj. 
Azure Blob unos dataset | Odnosi se na servis za pohranu Azure povezani. Servis povezane se odnosi na račun za Azure prostora za pohranu, a dataset blobova platforme Azure određuje kontejner, mapa i naziv datoteke u spremište koja sadrži ulazne podatke. 
Azure SQL izlazni skup podataka | Odnosi se na servis SQL Azure povezani. Servis SQL Azure povezana odnosi na poslužitelju komponente Azure SQL te skup podataka Azure SQL određuje naziv tablice koja sadrži podatke. 
Kanal podataka | Kanal sadrži jednu aktivnost vrste kopirajte koja vas vodi blobova platforme Azure skupu podataka kao ulaz i skup podataka Azure SQL kao u izlaz. Kopiraj aktivnosti kopira podatke iz programa Azure blob u tablicu u bazi podataka Azure SQL.  

Podaci tvorničke može imati jednu ili više kanali. Na kanal može imati jednu ili više aktivnosti u njoj. Postoje dvije vrste aktivnosti: [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) i [aktivnosti transformacije podataka](data-factory-data-transformation-activities.md). U ovom ćete praktičnom vodiču stvorite na kanal s jednog aktivnosti (aktivnosti Kopiraj).

![Kopiranje blobova platforme Azure s bazom podataka Azure SQL](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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

Stvaranje JSON datoteku pod nazivom **ADFCopyTutorialARM.json** u mapi **C:\ADFGetStarted** pomoću sljedećeg sadržaja:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
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
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
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
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
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
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Parametri JSON 
Stvaranje JSON datoteku pod nazivom **ADFCopyTutorialARM Parameters.json** sadrži parametara predloška Azure Voditelj resursa. 

> [AZURE.IMPORTANT] Navedite naziv i ključ računa za Azure prostora za pohranu za **storageAccountName** i **storageAccountKey** parametre.  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Možda ćete imati zasebne parametar JSON datoteka za razvoj i testiranje radnog okruženja koje možete koristiti s isti predložak JSON tvorničke podataka. Pomoću skripte ljuske Power možete automatizirati implementacija entiteti tvorničke podataka u te okruženja.  

## <a name="create-data-factory"></a>Stvaranje tvorničke podataka
1. Pokretanje **Programa PowerShell Azure** i pokrenite sljedeću naredbu:
    - Pokrenite `Login-AzureRmAccount` , a zatim unesite korisničko ime i lozinku koje koristite za prijavu na portal za Azure.  
    - Pokrenite `Get-AzureRmSubscription` da biste pogledali sve pretplate za taj račun.
    - Pokrenite `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` da biste odabrali pretplatu u koju želite raditi. 
2. Pokrenite sljedeću naredbu za implementaciju entiteti tvorničke podataka pomoću predloška za Voditelj resursa koji ste stvorili u koraku 1.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor kanal
1. Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa za Azure.
2. **Factories podataka** na lijevom izborniku kliknite (ili) kliknite **Dodatne servise** , a zatim **factories podataka** kategoriji **OBAVJEŠTAVANJE + ANALIZE** .

    ![Izbornik podaci factories](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Na stranici **factories podataka** tražiti i pronaći na tvorničke podataka. 

    ![Traženje tvorničke podataka](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Kliknite na tvorničke Azure podataka. Posjetite početnu stranicu za tvorničke podataka.

    ![Početna stranica za tvorničke podataka](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Kliknite pločicu **Dijagram** da biste pogledali dijagrama na tvorničke podataka.

    ![Prikaz dijagrama tvorničke podataka](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. U prikazu dijagrama dvokliknite dataset **SQLOutputDataset**. Potražite taj status isječak. Kada završi postupak kopiranja status postavite **spreman**.

    ![Isječak izlaza spreman stanja](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Kada je isječak u stanju **spremno** , provjerite je li kopirati podatke **emp** tablice u bazi podataka Azure SQL.

Potražite u članku [Monitor skupova podataka i kanal](data-factory-monitor-manage-pipelines.md) za upute o tome kako koristiti Azure blades portala za praćenje kanal i skupova podataka koji ste stvorili u ovom ćete praktičnom vodiču.

Možete koristiti i nadzor i upravljanje aplikaciju za praćenje kanali vaše podatke. Potražite u članku [nadzor i upravljanje kanali tvorničke Azure podataka pomoću aplikacije za nadzor](data-factory-monitor-manage-app.md) dodatne informacije o korištenju aplikacije.


## <a name="data-factory-entities-in-the-template"></a>Entiteti tvorničke podatke u predložak

### <a name="define-data-factory"></a>Definiranje tvorničke podataka
Definiranje podataka tvorničke u predlošku upravitelj resursa kao što prikazuje sljedeći primjer:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

U dataFactoryName se definira kao: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

To je jedinstveni niz koji se na temelju ID resursa za grupu.  

### <a name="defining-data-factory-entities"></a>Definiranje entiteti tvorničke podataka
Sljedeći podaci tvorničke entiteti su definirani u predlošku JSON: 

1. [Azure servis za pohranu povezana](#azure-storage-linked-service)
2. [Azure SQL povezana usluga](#azure-sql-database-linked-service)
3. [Skup podataka blobova platforme Azure](#azure-blob-dataset)
4. [Skup podataka za Azure SQL](#azure-sql-dataset)
5. [Kanal podataka s Kopiraj aktivnosti](#data-pipeline)

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

Na connectionString koristi storageAccountName i storageAccountKey parametre. Vrijednosti za ove parametara pomoću konfiguracijska datoteka. Definicija koristi i varijable: azureStroageLinkedService i dataFactoryName definiran u predlošku. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure povezana SQL baze podataka usluge
U ovom odjeljku Navedite naziv poslužitelja Azure SQL, naziv baze podataka, korisničko ime i lozinku za korisnika. Potražite u članku [Azure SQL povezana servisa](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) detalje o svojstvima JSON koji se koriste za definiranje na servis SQL Azure povezani.  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

Na connectionString koristi sqlServerName, ImeBazePodataka, sqlServerUserName i sqlServerPassword parametara čije vrijednosti su pomoću konfiguracijska datoteka. Definicija koristi i sljedeće varijable iz predloška: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Skup podataka blobova platforme Azure
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
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Skup podataka za Azure SQL
Navedite naziv tablice u bazi podataka Azure SQL koja sadrži kopirane podatke iz spremišta blobova platforme Azure. Detalje o svojstvima JSON koji se koriste za definiranje skup podataka sustava Azure SQL potražite u članku [svojstava za skup podataka Azure SQL](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) . 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Kanal podataka
Definiranje kanal koja se kopira podatke iz skupa blobova platforme Azure Azure SQL skup podataka. Opisi elemenata JSON koristi za definiranje na kanal u ovom primjeru potražite u članku [JSON kanal](data-factory-create-pipelines.md#pipeline-json) . 

    {
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
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>Ponovno korištenje predloška 
U ovom praktičnom vodiču, stvorite predložak za definiranje podataka tvorničke entiteti i predloška za prosljeđivanje vrijednosti za parametre. Kanal kopira podataka s računa za pohranu Azure s bazom podataka Azure SQL navedeno putem parametara. Da biste koristili isti predložak za implementaciju entiteti tvorničke podataka u različitim okruženjima, stvoriti datoteku parametar za svako okruženje te ga koristiti pri implementaciji njegovom okruženju.     

Primjer:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Obratite pozornost na to da prve naredbe koristi parametar datoteka za razvojno okruženje, drugu test okruženju, a treći nešto radnom okruženju.  

Možete koristiti i predložak za obavljanje zadataka koji se ponavljaju. Na primjer, morate stvoriti mnogo factories podataka s jednog ili više kanali koje implementirati isti logike, ali svaki tvorničke podataka koristi baze podataka SQL Azure računi i drugi Azure prostor za pohranu. U ovom scenariju koristite isti predložak u istom okruženju (razvojni, testiranje ili radni) s različitim parametar datoteke da biste stvorili factories podataka.   

