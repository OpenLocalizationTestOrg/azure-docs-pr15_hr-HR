<properties 
    pageTitle="Pokretanje U SQL skripte na Lake analize podataka za Azure s tvorničke Azure podataka" 
    description="Upute za obradu podataka tako da pokrenete U SQL skripte na servisu Azure podataka Lake analize računalnim." 
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
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>Pokretanje U SQL skripte na Lake analize podataka za Azure s tvorničke Azure podataka
> [AZURE.SELECTOR]
[Grozd](data-factory-hive-activity.md)  
[Svinja](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md)
[Strojnog učenja](data-factory-azure-ml-batch-execution-activity.md) 
[Pohranjene Procedure](data-factory-stored-proc-activity.md)
[Podataka U u analize Lake-SQL](data-factory-usql-activity.md)
[.NET prilagođene](data-factory-use-custom-activities.md)
 
Kanal u na tvorničke Azure podataka obrađuje podatke u servise za pohranu povezane pomoću povezane računalnim services. Sadrži niz aktivnosti, gdje svaki aktivnosti izvodi operacije na određene obrada. U ovom se članku opisuje **Podataka Lake analize U SQL aktivnosti** koji se izvodi **U SQL** skripte na na servis računalnim povezana **Azure podataka Lake analize** . 

> [AZURE.NOTE] 
> Stvorite račun za Azure podataka Lake analize prije stvaranja na kanal s podacima Lake analize U SQL aktivnosti. Da biste saznali više o Lake analize podataka za Azure, potražite u članku [Početak rada s Azure podataka Lake analize](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Pregledajte na [Sastavljanje prvom praktičnom vodiču kanal](data-factory-build-your-first-pipeline.md) detaljne upute za stvaranje podataka tvorničke, povezani servisi, skupova podataka i na kanal. Pomoću JSON isječci uređivač tvorničke podataka ili Visual Studio ili Azure PowerShell da biste stvorili entiteti tvorničke podataka.

## <a name="azure-data-lake-analytics-linked-service"></a>Analitički Lake Azure podataka povezan servisa
Stvorite na servis **Lake analize podataka za Azure** povezana da biste se povezali na servis računalnim Azure podataka Lake analize na tvorničke Azure podataka. Aktivnosti podataka U u analize Lake-SQL u kanalu se odnosi na taj servis povezani. 

U sljedećem primjeru sadrži JSON definicija servisa za povezane podatke Lake analize Azure. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


Sljedeća tablica sadrži opise svojstava koja se koristi u definiciji JSON. 

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
Vrsta | Svojstvo vrsta mora biti postavljena na: **AzureDataLakeAnalytics**. | Da
accountName | Naziv računa analize Lake Azure podataka. | Da
dataLakeAnalyticsUri | URI Lake analize Azure podataka. |  ne 
autorizacija | Šifra autorizacija dohvaća se automatski nakon klikom na gumb **ovlasti** u uređivač tvorničke podataka i dovršavanje OAuth prijava.  | Da 
subscriptionId | Id pretplate Azure | Ne (Ako nije naveden, pretplatu na tvorničke podataka služi). 
resourceGroupName | Naziv grupe Azure resursa |  Ne (Ako nije naveden, grupu resursa tvorničke podataka služi).
ID sesije | id sesije iz sesije autorizacija OAuth. Svaki id sesije jedinstven i može se koristiti samo jedanput. Sesije Id automatski se generira u uređivač tvorničke podataka. | Da

Kod autorizacije generira pomoću gumba **ovlasti** istječe nakon tijekom. Istek vremena za različite vrste korisničkih računa potražite u članku u sljedećoj tablici. Vidjet ćete sljedeće pogreške kada poruka provjere autentičnosti **istekne tokena**: vjerodajnica operacija pogreške: invalid_grant - AADSTS70002: pogreške provjere valjanosti vjerodajnice. AADSTS70008: Dodjela navedeni pristup je istekla ili je povučen. ID praćenje: ID korelacije koji se d18629e8-af88-43c5-88e3-d8419eb1fca1: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 vremenska oznaka: 2015 12 15 21:09:31Z

 
| Vrsta korisnika | Istječe nakon |
| :-------- | :----------- | 
| Korisnički računi ne upravlja Azure Active Directory (@hotmail.com, @live.com, itd.) | 12 sati |
| Računa korisnika upravlja po Azure Active Directory (AAD) | Pokrenite 14 dana nakon zadnjeg isječak. <br/><br/>90 dana, ako odsječak na temelju OAuth usluzi utemeljenoj na povezane izvodi najmanje jedanput 14 dana. |

Da biste izbjegli/Razriješi tu pogrešku, reauthorize pomoću **autorizacija** gumb kada **istekne token** i redeploy povezane servisa. Možete stvoriti i vrijednosti za svojstva **ID sesije** i **autorizacije** programski pomoću koda u sljedećem odjeljku. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Da biste generirali programski ID sesije i autorizacije vrijednosti 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Potražite u temama [Klase AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService predmete](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)i [AuthorizationSessionGetResponse predmete](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) detalje o klase tvorničke podataka koji se koristi u kodu. Dodavanje reference: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll WindowsFormsWebAuthenticationDialog predmet. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Aktivnosti U SQL za Lake analize podataka 

Sljedeći isječak JSON definira kanal s podacima Lake analize U SQL aktivnosti. Definiciju aktivnosti ima referencu sa servisom Azure podataka Lake analize povezana koji ste prethodno stvorili.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


U sljedećoj tablici opisane naziva i opisa svojstva koja se odnose na tu aktivnost. 

Svojstvo | Opis | Obavezno
:-------- | :----------- | :--------
Vrsta | Svojstvo vrsta mora biti postavljeno na **DataLakeAnalyticsU SQL**. | Da
scriptPath | Put do mape koja sadrži skripte U SQL. Naziv datoteke je velika i mala slova. | Ne (Ako je pomoću sljedeće skripte)
scriptLinkedService | Povezane servis koji povezuje prostora za pohranu koji sadrži skriptu da biste tvorničke podataka | Ne (Ako je pomoću sljedeće skripte)
skripta | Navedite skripte u istoj razini umjesto navodeći scriptPath i scriptLinkedService. Na primjer: "skripte": "Test za stvaranje baze podataka". | Ne (Ako koristite scriptPath i scriptLinkedService)
degreeOfParallelism | Maksimalan broj čvorove istodobno koristiti za izvođenje zadatka. | ne
Prioritet | Određuje koje zadatke iz sve u redu čekanja mora biti označena da biste pokrenuli prvi. U donjem broj, veći prioritet. | ne 
Parametri | Parametri za skripte U SQL | ne 

Definiciju skripte potražite u članku [SearchLogProcessing.txt skripte definicija](#script-definition) . 

## <a name="sample-input-and-output-datasets"></a>Poslušajte ulazni i izlazni skupova podataka

### <a name="input-dataset"></a>Skup podataka za unos
U ovom primjeru ulaznih podataka nalazi se u spremištu Lake Azure podataka (SearchLog.tsv datoteka u mapi datalake/unos). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Izlazni skup podataka
U ovom primjeru za izlazne podatke koje je stvorio U SQL skripta pohranjen u spremištu Lake Azure podataka (datalake/Izlazna datoteka). 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Uzorak podataka Lake spremište povezana servisa
Evo definiciju uzorka Lake spremišta podataka za Azure povezana servis koristi skupove podataka ulaza i izlaza. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Članak [Premještanje podatke iz spremišta Lake za Azure podataka](data-factory-azure-datalake-connector.md) potražite u članku za opise svojstava JSON. 

## <a name="sample-u-sql-script"></a>Primjer U SQL skripte 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

Vrijednosti za **@in** i **@out** parametara U SQL skripta prosljeđuju se dinamički po ADF pomoću sekcija 'parametri'. U odjeljku 'parametri' u definiciji kanala.

Druga svojstva kao što su degreeOfParallelism i prioritet možete odrediti kao i u definicije kanal za zadatke koji se izvode na servisu Azure podataka Lake analize.

## <a name="dynamic-parameters"></a>Dinamični parametara
U kanal definiciju uzorak i smanjivati parametara dodjeljuju s programiranih vrijednosti. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Moguće je umjesto toga koristite dinamičke parametara. Ako, na primjer: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

U ovom slučaju ulaznih datoteka i dalje se izdvajaju iz mape /datalake/input i izlazna datoteka generiraju u mapi /datalake/output. Nazivi datoteka su dinamičke na temelju vremena početka isječak.  