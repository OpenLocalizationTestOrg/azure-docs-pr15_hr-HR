<properties 
    pageTitle="Pozivanje Spark programe tvorničke Azure podataka" 
    description="Saznajte kako pozvati Spark programe na tvorničke Azure podataka pomoću MapReduce aktivnosti." 
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
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Pozivanje Spark programe tvorničke podataka
## <a name="introduction"></a>Uvod
Aktivnosti MapReduce u kanalu na tvorničke podataka možete koristiti za pokretanje programa Spark na svoj klaster HDInsight Spark. Pogledajte članak [MapReduce aktivnosti](data-factory-map-reduce.md) detaljne informacije o korištenju aktivnosti prije čitanja u ovom se članku. 

## <a name="spark-sample-on-github"></a>Ogledna Spark na GitHub
[Spark - oglednih podataka tvorničke na GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) pokazuje kako pomoću aktivnosti MapReduce poziva Spark programa. Spark program samo kopira podatke iz jedne spremnik blobova platforme Azure na drugi. 

## <a name="data-factory-entities"></a>Entiteti tvorničke podataka
Mapa **Spark-ADF/src/ADFJsons** sadrži datoteke za entiteti tvorničke podataka (povezani servisi, skupova podataka, kanala).  

U ovom primjeru postoje dva **povezani servisi** : Azure i prostor za pohranu Azure HDInsight. Navedite naziv Azure prostora za pohranu i vrijednosti ključa u **StorageLinkedService.json** i clusterUri, korisničko ime i lozinku u **HDInsightLinkedService.json**.

U ovom primjeru postoje dva **skupove podataka** : **input.json** i **output.json**. Te se datoteke nalaze se u mapi **skupova podataka** .  Ulazni i izlazni skupova podataka za aktivnost MapReduce se odnositi na te datoteke

Ogledna kanali pronaći u mapi **ADFJsons/kanal** . Pregledajte kanal znati kako pozvati Spark program pomoću MapReduce aktivnosti. 

Aktivnosti MapReduce je konfiguriran za pozivanje **com.adf.sparklauncher.jar** u spremniku **adflibs** u Azure prostora za pohranu (naveden u StorageLinkedService.json). Izvorni kod za ovaj program je u Spark-ADF/src/glavne/java/com/adf/mape i poziva spark slanje i pokrenite Spark zadatke. 

> [AZURE.IMPORTANT] 
> Pročitajte [README. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) najnovije i dodatne informacije prije korištenja uzorka. 
>  
> Koristite vlastitu HDInsight Spark klaster na taj se način pozvati Spark programa pomoću MapReduce aktivnosti. Korištenje programa klaster HDInsight na zahtjev nije podržano.   


## <a name="see-also"></a>Vidi također
- [Grozd aktivnosti](data-factory-hive-activity.md)
- [Svinja aktivnosti](data-factory-pig-activity.md)
- [MapReduce aktivnosti](data-factory-map-reduce.md)
- [Hadoop strujanje aktivnosti](data-factory-hadoop-streaming-activity.md)
- [Pozivanje R skripti](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
