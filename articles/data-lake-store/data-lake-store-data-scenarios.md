<properties 
   pageTitle="Scenariji podataka koje obuhvaćaju spremišta podataka Lake | Microsoft Azure" 
   description="Razumijevanje scenarija i alati koristeći podatke koji mogu ingested obrađuju, preuzimanje i vizualizirati u Lake izvor podataka" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Korištenje Lake spremišta podataka za Azure preduvjete velikih skupova podataka

Postoje četiri ključa faza u širu obradu podataka:

* Ingesting velikih količina podataka u izvor podataka, u stvarnom vremenu ili grupama
* Obrada podataka
* Preuzimanje podataka
* Vizualizacija podataka

U ovom se članku smo pogledajte te su faze prikazane vezana uz pohranu Lake Azure podataka da biste razumjeli mogućnosti i alata koji su raspoloživi potrebama velikih skupova podataka.

## <a name="ingest-data-into-data-lake-store"></a>Ingest podataka u spremištu Lake podataka

U ovom se odjeljku ističe različitih izvora podataka i načine u kojoj te podatke možete ingested spremišta podataka Lake račun.

![Ingest podatke u spremištu Lake podataka] (./media/data-lake-store-data-scenarios/ingest-data.png "Ingest podatke u spremištu Lake podataka")

### <a name="ad-hoc-data"></a>Ad hoc podataka

Time se povećava manje skupova podataka koji su koristi za prototyping velikih skupova podataka aplikacije. Nekoliko je načina ingesting ad-hoc podataka, ovisno o izvoru podataka.

| Izvor podataka        | Pomoću ingest                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Lokalno računalo     | <ul> <li>[Portal za Azure](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure EŽA različite platforme](data-lake-store-get-started-cli.md)</li> <li>[Pomoću alata za Lake podataka za Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Azure spremište blobova platforme | <ul> <li>[Tvorničke Azure podataka](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[Alat za AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp koji se izvode na HDInsight klaster](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Strujanjem podataka

Time se povećava podataka koji se mogu stvoriti različitih izvora, kao što je aplikacija uređaja, senzori, itd. Ove podatke možete ingested u Lake izvor podataka pomoću alata za različite. Ti Alati obično snimiti i obrada podataka na osnovi događaj tako da događaju u stvarnom vremenu, a zatim napišite događaje u grupama u spremištu Lake podataka tako da ih mogu dalje obrađivati. 

Slijede alate koje možete koristiti:
 
* [Azure strujanje analize] (.. / strujanje analytics – podataka – lake-izlaza) – događaji ingested u događaj koncentratora možete zapisivanje Lake Azure podataka pomoću programa Azure podataka Lake spremište izlaz.
* [Azure HDInsight oluja](../hdinsight/hdinsight-storm-write-data-lake-store.md) - zapisivanje podataka izravno u spremištu Lake podataka iz skupine oluja.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) – možete primati događaje iz koncentratora za događaj i zatim zapisivati spremište Lake podataka pomoću [Podataka Lake spremište .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relacijskih podataka

Možete i izvora podataka iz relacijskih baza podataka. Tijekom vremenskog razdoblja, relacijskim bazama podataka prikupljanje velikog količine podataka koje pružaju ključa uvida obrađuju kroz kanal za velikih skupova podataka. Da biste premjestili tih podataka u spremištu Lake podataka možete koristiti sljedeće alate.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Tvorničke Azure podataka](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Poslužitelj zapisnika podataka iz alata web (prijenos pomoću prilagođenih aplikacija)

Ovu vrstu dataset je izričito oblačićima jer analize podataka u zapisniku poslužitelja web uobičajena slučaja koristi za aplikacije velikih skupova podataka i zahtijeva velike količine datoteke zapisnika prenijeti Lake pohrane podataka. Možete koristiti bilo koji od sljedećih alata za pisanje vlastite skripte i aplikacije za prijenos takvog podataka.

* [Azure EŽA različite platforme](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [SDK .NET Lake spremišta podataka za Azure](data-lake-store-get-started-net-sdk.md)
* [Tvorničke Azure podataka](../data-factory/data-factory-data-movement-activities.md)

Za prijenos podaci iz zapisnika web server i za prijenos druge vrste podataka (primjerice društvene sentiments podataka), je dobar način pisanja vlastite prilagođene skripte/aplikacije jer pruža fleksibilnost za uključivanje podataka komponente prenesete kao dio veći aplikacije velikih skupova podataka. U nekim slučajevima kod može potrajati obliku skripte ili uslužni program jednostavan naredbenog retka. U drugim slučajevima kod može se integrirati veliki obradu podataka u poslovnoj aplikaciji ili rješenja.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Podaci koji su povezani s klastere Azure HDInsight

HDInsight klaster većinu (Hadoop, HBase, oluja) podržava spremišta podataka Lake kao spremište pohrane podataka. HDInsight klastere pristupiti podacima s blob-ova Azure prostora za pohranu (WASB). Bolje performanse, možete kopirati podatke iz WASB spremišta podataka Lake račun povezan s klaster. Možete koristiti sljedeće alate za kopiranje podataka.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [Servis za AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
* [Tvorničke Azure podataka](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Podaci spremljeni u na lokaciji ili IaaS Hadoop klastere

Velike količine podataka može biti pohranjeno u postojeće Hadoop klastere lokalno na računalima pomoću HDFS. Skupina Hadoop možda implementacije sustava na lokaciji ili može biti unutar programa IaaS klaster na Azure. Mogući su preduvjete za kopiranje tih podataka Azure podataka Lake spremište za jednokratni pristup ili ponavljajućem način. Dostupne su različite mogućnosti koje možete koristiti da biste to postigli. U nastavku je popis alternativa i na povezane gubitke.

| Pristup  | Pojedinosti | Prednosti   | Razmatranja  |
|-----------|---------|--------------|-----------------|
| Korištenje tvorničke Azure podataka (ADF) da biste kopirali podatke izravno iz Hadoop klastere Lake spremišta podataka za Azure | [ADF podržava HDFS kao izvor podataka](../data-factory/data-factory-hdfs-connector.md) | ADF pruža podršku za izlaz u-tvorničke za HDFS i upravljanje najprije class završetka do kraja i nadzor | Pristupnik za upravljanje podacima uvesti na lokaciji ili IaaS klasteru potrebna |
| Izvoz podataka iz Hadoop kao datoteke. Zatim kopirajte datoteke pomoću odgovarajuće mehanizam podataka Lake trgovina Azure.                                   | Datoteke možete kopirati u spremištu Lake podataka za Azure pomoću: <ul><li>[Azure PowerShell s operacijskim Sustavom Windows](data-lake-store-get-started-powershell.md)</li><li>[Azure EŽA različite platforme za koje nisu – Windows OS](data-lake-store-get-started-cli.md)</li><li>Prilagođene aplikacije pomoću bilo koje podatke Lake spremište SDK-a</li></ul> | Brzi početak rada. Možete učiniti prilagođene prijenosima                                                   | Više koraka koji uključuje više tehnologija. Upravljanje i nadzor će rasti u postati složeno vremenom dali prilagođene prirode alata |
| Koristite Distcp da biste kopirali podatke iz Hadoop Azure prostora za pohranu. Zatim kopirajte podatke iz spremišta Azure spremišta Lake podataka pomoću odgovarajuće mehanizam. | Kopirajte podatke iz spremišta Azure u spremištu Lake podataka pomoću: <ul><li>[Tvorničke Azure podataka](../data-factory/data-factory-data-movement-activities.md)</li><li>[Alat za AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp Apache sustavom klastere HDInsight](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| Možete koristiti alate za Otvori izvor. | Više koraka koji uključuje više tehnologije |

### <a name="really-large-datasets"></a>Zaista velikih skupova podataka

Za prijenos skupove podataka u rasponu u nekoliko terabajta pomoću metode prethodno opisan ponekad može sporu i skup. U tim slučajevima, možete koristiti mogućnosti u nastavku.

* **Korištenje Azure ExpressRoute**. Azure ExpressRoute omogućuje stvaranje privatne veze između Azure podatkovnim centrima i infrastrukture lokalno. To omogućuje pouzdanog mogućnost za prijenos velikih količina podataka. Dodatne informacije potražite u [dokumentaciji za Azure ExpressRoute](../expressroute/expressroute-introduction.md).


* **"Izvanmrežno" prijenos podataka**. Ako pomoću Azure ExpressRoute nije izvedivo iz bilo kojeg razloga, pomoću [servisa Azure uvoz/izvoz](../storage/storage-import-export-service.md) isporuka tvrdom diskovnih pogona s podacima u Centar za Azure podataka. Blob polja za pohranu Azure najprije prenošenja podataka. Da biste kopirali podatke iz blob polja za pohranu Azure spremišta Lake podataka, pa možete koristiti [Tvorničke Azure podataka](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) ili [Alat za AdlCopy](data-lake-store-copy-data-azure-storage-blob.md) .

    >[AZURE.NOTE] Tijekom korištenja servisa uvoz/izvoz, veličina datoteka na diskova isporučene Azure podatkovnog centra ne smije biti veća od 200 GB.


## <a name="process-data-stored-in-data-lake-store"></a>Obrada podataka pohranjenih u spremištu Lake podataka

Kada su podaci dostupni su u spremištu Lake podataka možete pokrenuti analizu na tih podataka pomoću aplikacija podržani velikih skupova podataka. Trenutno koristite Azure HDInsight i Azure podataka Lake Analytics za izvođenje zadataka za analizu podataka na podatke pohranjene u spremištu Lake podataka. 

![Analiza podataka u spremištu Lake podataka] (./media/data-lake-store-data-scenarios/analyze-data.png "Analiza podataka u spremištu Lake podataka")

Možete pogledati sljedeće primjere.

* [Stvaranje programa HDInsight klaster s trgovinom Lake podataka kao prostora za pohranu](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Preuzimanje podataka iz spremišta Lake podataka

Možda želite preuzeti ili premještanje podataka iz spremišta Lake podataka za Azure scenarije kao što su:

* Premještanje podataka u drugim spremištima sučelje s vaše postojeće kanali obradu podataka. Na primjer, možda ćete morati premještanje podataka iz spremišta Lake podataka u bazi podataka SQL Azure ili lokalnog sustava SQL Server.
* Preuzimanje podataka s vašim lokalnim računalom za obradu u okruženju IDE tijekom sastavljanja prototipovi aplikacije.

![Izlazne podatke iz spremišta Lake podataka] (./media/data-lake-store-data-scenarios/egress-data.png "Izlazne podatke iz spremišta Lake podataka")

U tim slučajevima, možete koristiti bilo koju od sljedećih mogućnosti:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Tvorničke Azure podataka](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Na sljedeće načine možete koristiti i za pisanje vlastite skripte/aplikacije za preuzimanje podataka iz spremišta Lake podataka.

* [Azure EŽA različite platforme](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [SDK .NET Lake spremišta podataka za Azure](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Vizualni prikaz podataka u spremištu Lake podataka

Pomoću kombinacije services možete stvarati vizualne prikaze podataka pohranjenih u spremištu Lake podataka.

![Vizualiziraj podataka u spremištu Lake podataka] (./media/data-lake-store-data-scenarios/visualize-data.png "Vizualiziraj podataka u spremištu Lake podataka")

* Možete pokrenuti pomoću [Azure tvorničke podataka za premještanje podataka iz spremišta podataka Lake Azure SQL Data Warehouse](../data-factory/data-factory-data-movement-activities.md#supported-data-stores)
* Nakon toga možete [integrirati s Azure SQL Data Warehouse servisa Power BI](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) da biste stvorili vizualni prikaz podataka.
