<properties 
    pageTitle="Tvorničke podataka – Evidencija promjena API .NET | Microsoft Azure" 
    description="U članku se opisuje prijelom promjene, značajka dodaci, a zatim popravci programskih pogrešaka itd... u određenu verziju .NET API-JA za podataka tvorničke Azure." 
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
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure tvorničke podataka – Evidencija promjena .NET API 
Ovaj članak sadrži informacije o promjenama Azure podataka tvorničke SDK u određenu verziju. Možete pronaći najnovije NuGet paket za Azure podataka tvorničke [ovdje](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Verzija 4.11.0
Značajka dodatke:

- Dodane su sljedeće vrste povezane servisa:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Dodane su sljedeće vrste skup podataka: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Dodane su sljedeće vrste izvora kopija:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Verzija 4.10.0
- Sljedeća svojstva neobavezno dodani TextFormat:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Dodane su sljedeće vrste povezane servisa:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Dodane su sljedeće vrste skup podataka:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Dodane su sljedeće vrste izvora kopija:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- Dodavanje [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) svojstvo AzureMLBatchExecutionActivity 
    - Omogućivanje prosljeđivanje više web servisa unosi u pokusa Azure strojnog učenja


## <a name="version-491"></a>Verzija 4.9.1

### <a name="bug-fix"></a>Otklanjanje problema

- Zastarijevanje provjere autentičnosti utemeljene na WebApi za [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Verzija 4.9.0

### <a name="feature-additions"></a>Značajka dodaci

- Dodajte svojstva [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) i [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) CopyActivity. Detalje o značajku potražite u članku [postupna kopiranje](data-factory-copy-activity-performance.md#staged-copy) .


### <a name="bug-fix"></a>Otklanjanje problema

- Upoznavanje programa preopterećenja [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) metodu koja vodi [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) instance.
- Označavanje [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) i [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) kao Neobavezno u CopySink.

## <a name="version-480"></a>Verzija 4.8.0

### <a name="feature-additions"></a>Značajka dodaci
- Sljedeća svojstva neobavezno dodani Kopiraj vrsta aktivnosti da biste omogućili ugađanje performansi za kopiranje:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Verzija 4.7.0

### <a name="feature-additions"></a>Značajka dodaci
* Dodati novu vrstu StorageFormat vrsta [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) da biste kopirali datoteke u obliku zapisa optimizirana retka stupčastu (ORC).
* Dodajte svojstva [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) i PolyBaseSettings SqlDWSink.
    * Omogućuje korištenje PolyBase da biste kopirali podatke u SQL Data Warehouse.

## <a name="version-461"></a>Verzija 4.6.1

### <a name="bug-fixes"></a>Popravci programskih pogrešaka
* U članku se rješava HTTP zahtjev za unos windows aktivnosti.
    * Uklanja naziv za grupu resursa i tvorničke podataka iz opseg zahtjev.

## <a name="version-460"></a>Verzija 4.6.0

### <a name="feature-additions"></a>Značajka dodaci

- [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx)dodane su sljedeća svojstva:
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Skupove podataka](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx)dodane su sljedeća svojstva:
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Dodana nova [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) upišite vrstu [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) da biste definirali skupove podataka čiji podaci nalaze u JSON OSNOVNI oblik. 

## <a name="version-450"></a>Verzija 4.5.0

### <a name="feature-additions"></a>Značajka dodaci
* Dodana [popisu operacija za prozor aktivnost](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Dodane načina za dohvaćanje windows aktivnosti s filtrima koji se temelje na vrsti entitet (to jest, factories podataka, skupova podataka, kanali i aktivnosti).
* Dodane su sljedeće vrste povezane servisa: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Dodane su sljedeće vrste skup podataka: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Dodane su sljedeće vrste izvora kopija:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Verzija 4.4.0

### <a name="feature-additions"></a>Značajka dodaci

- Sljedećih vrsta povezane servisa dodana kao izvora podataka i primatelja za kopiranje aktivnosti:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Pročitajte članak [Povezani servis SAS Azure za pohranu](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) za konceptualnih informacija i primjeri. 

## <a name="version-430"></a>Verzija 4.3.0

### <a name="feature-additions"></a>Značajka dodaci

- Sljedeće vrste haven za servis za povezane su dodani kao izvora podataka za kopiranje aktivnosti:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Potražite u članku [Premještanje podataka iz HDFS pomoću tvorničke podataka](data-factory-hdfs-connector.md) za konceptualnih informacija i primjeri. 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Potražite u članku [Premještanje podataka iz ODBC podataka pohranjuje pomoću tvorničke Azure podataka](data-factory-odbc-connector.md) za konceptualnih informacija i primjeri. 

## <a name="version-420"></a>Verzija 4.2.0

### <a name="feature-additions"></a>Značajka dodaci

- Sljedeće nove vrste aktivnosti dodan: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Pojedinosti o aktivnosti, potražite u članku [Ažuriranje ML Azure modela pomoću resursa aktivnosti ažuriranja](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Novi neobavezni svojstvo [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) dodana [AzureMLLinkedService predmete](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- Svojstva [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) i [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) dodane [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) predmete. 
- Dopusti konfiguracije vremenskih ograničenja za pozive klijent sa servisom tvorničke podataka. 


## <a name="version-410"></a>Verzija 4.1.0

### <a name="feature-additions"></a>Značajka dodaci
* Dodane su sljedeće vrste povezane servisa: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Dodane su sljedeće vrste aktivnosti: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Dodane su sljedeće vrste skup podataka: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Dodane su sljedeće vrste izvora i primatelj Kopiraj aktivnosti:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Verzija 4.0.1

### <a name="breaking-changes"></a>Najnovije promjene
Preimenovana sljedeće klase. Nova imena su izvorni nazivi klasa prije nego što pustite 4.0.0. 
 
Naziv u 4.0.0 | Naziv u 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Verzija 4.0.0

### <a name="breaking-changes"></a>Najnovije promjene



- Preimenovana sljedeće klase/sučelja.

| Stari naziv | Novi naziv |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Tablica | [Skup podataka](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- Načini **popis** sada vratite Straničeno rezultate. Ako odgovor sadrži **NextLink** svojstva koje nisu prazne, klijentska aplikacija treba nastavak pribavljanja na sljedećoj stranici dok se ne vraćaju se sve stranice.  Evo jednog primjera: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Popis** kanala API vraća samo sažetak kanala umjesto sve detalje. Ako, primjerice, aktivnosti u sažetku za kanal sadržavati samo naziv i vrsta.

### <a name="feature-additions"></a>Značajka dodaci
- Klase [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) podržava dva nova svojstva, **SliceIdentifierColumnName** i **SqlWriterCleanupScript**podržano kopiranje idempotent za Azure SQL Data Warehouse. Potražite u članku [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) , Konkretno, [mehanizam 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) i [2 mehanizam](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) sekcije, detalje o tih svojstava.

- Sada je podržavamo pokrenut pohranjena procedura s bazom podataka SQL Azure i Azure SQL Data Warehouse izvora kao dio aktivnosti Kopiraj. Klase [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) i [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) imaju sljedeća svojstva: **SqlReaderStoredProcedureName** i **StoredProcedureParameters**. Potražite u člancima [Baze podataka SQL Azure](data-factory-azure-sql-connector.md#sqlsource) i [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) na Azure.com detalje o tih svojstava.  