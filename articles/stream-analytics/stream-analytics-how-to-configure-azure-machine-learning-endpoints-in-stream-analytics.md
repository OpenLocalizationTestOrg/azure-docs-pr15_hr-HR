<properties 
    pageTitle="Konfiguriranje Azure strojnog učenja krajnje točke u strujanje analize | Microsoft Azure" 
    description="Jezik za strojno korisnički definirane funkcije u strujanje Analytics"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Učenje integraciju u analize strujanje na računalu

Analitički strujanje podržava korisnički definirane funkcije koje koriste za krajnje točke Azure strojnog učenja. Podrška za REST API-JA za ovu značajku detaljne je u [biblioteci strujanje analize REST API -JA](https://msdn.microsoft.com/library/azure/dn835031.aspx). Ovaj članak sadrži dodatne podatke koji su potrebni za uspješan implementaciju tu mogućnost u strujanje analize. Na Praktični vodič i poslali i dostupna je [u nastavku](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Pregled: Azure strojnog učenja terminologija

Microsoft Azure strojnog učenja omogućuje suradnju, povucite i ispustite alata koji omogućuju stvaraju, testirajte i implementacija rješenja predvidljivu analize nad podacima. Ovaj alat zove *Azure strojnog učenja Studio*. Na studio služi za interakciju s resursi za učenje za računala i jednostavno stvaranje, testiranje i iteracija na dizajnu. Ove resurse i definicije su ispod.

- **Radni prostor**: *radni prostor* je spremnik koja sadrži sve strojnog učenja resurse zajedno u spremniku za upravljanje i kontrola.
- **Eksperiment**: *eksperimenata* stvaraju fizičari podataka koristi funkciju skupova podataka i obuci za učenje model računala.
- **Krajnja točka**: *krajnje točke* se koriste za značajke postupka unos, Primjena model za učenje navedeni računala i povratna izlaza testu dobije objekt Azure strojnog učenja.
- **Bilježenje rezultata Webservice**: zbirka krajnje točke na *bilježenje rezultata webservice* je kao što je rečeno iznad.

Svaki krajnja točka ima API-ji za obradu izvođenja i sinkrono izvršavanja. Analitički strujanje koristi sinkrono izvršavanja. Servis za određene pod nazivom [Servisnog zahtjeva i odgovora](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) u AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Strojnog učenja resursima koji su potrebni za strujanje analize poslove

U svrhu analize strujanje obrada posla, krajnjoj točki zahtjeva i odgovora, [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)i definiciju swagger su sve potrebne za izvršavanje uspješno. Strujanje analize ima dodatne krajnjoj točki konstrukata URL-a za krajnju točku swagger, pretražuje sučelje i vraća definiciju UDF zadani korisniku.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfiguriranje strujanje analize i strojnog učenja UDF putem REST API-JA

Pomoću REST API-ji možete konfigurirati posla da biste uputili poziv funkcije Azure stroj jezik. Koraci su na sljedeći način:

1. Stvaranje zadatka strujanje Analytics
2. Definiranje ulazni
3. Definiranje sustava Izlaz
4. Stvorite korisnički definirane funkcije (UDF)
5. Pisanje transformaciju strujanje analize koja se poziva na UDF
6. Pokretanje zadatka

## <a name="creating-a-udf-with-basic-properties"></a>Stvaranje na UDF pomoću osnovnih svojstava

Na primjer, sljedeći kod uzorka stvara skalarna UDF pod nazivom *newudf* povezuje krajnje Azure strojnog učenja. Imajte na umu da *krajnju točku* (servis URI) pronaći ćete na stranici pomoći API-JA za odabranu uslugu i *apiKey* pronaći ćete na glavnoj stranici servisa.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Primjer tijelo zahtjev:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Nazovite RetrieveDefaultDefinition krajnja točka za zadani UDF

Kada je stvorena u skeleton UDF potreban je dovršena definiciju na UDF. Krajnja točka RetreiveDefaultDefinition pomaže vam definiciju zadano za skalarnu funkciju koja je povezana s krajnje Azure strojnog učenja. Opseg ispod potrebno da biste dobili definiciju UDF zadano za skalarnu funkciju koja je povezana s krajnje Azure strojnog učenja. Kao što je već dodijeljen tijekom zahtjeva STAVI ga ne odredite stvarni krajnjoj točki. Analitički strujanje poziva krajnju točku naveden u zahtjevu za ako su izričito dani. U suprotnom koristi jedan izvorno poziva. Ovdje UDF traje jedan niz parametar (rečenice) i vraća jedan izlaz vrste niz što znači "šalju" natpis za tu rečenicu.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Primjer tijelo zahtjev:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Uzorak Izlaz te pogledajte nešto želite ispod.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>UDF zakrpu odgovorom 

Sada na UDF mora biti patched s prethodni odgovor, kao što je prikazano u nastavku.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Zahtjev za tijelo (izlaza iz RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Implementacija transformaciju strujanje analize da biste uputili poziv na UDF

Sada upit UDF (ovdje pod nazivom scoreTweet) za svaki unos događaj i pisati odgovor za taj događaj na izlaz.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
