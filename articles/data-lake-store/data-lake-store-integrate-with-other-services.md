<properties
   pageTitle="Integriranje spremišta Lake podataka s drugih servisa za Azure | Microsoft Azure"
   description="Objašnjenje kako će se spremišta podataka Lake integrira s drugih servisa za Azure"
   documentationCenter=""
   services="data-lake-store"
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integriranje spremišta Lake podataka s drugih servisa za Azure

Da biste omogućili širem krugu scenariji zajedno s ostalim servisima Azure se poslužite Lake spremišta podataka za Azure. U sljedećem članku navedene servise koje spremišta Lake podataka može se integrirati s.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Korištenje podataka Lake trgovine s Azure HDInsight

Možete Dodjela klaster programa [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) koji koristi pohranu Lake podataka kao HDFS usklađen prostora za pohranu. U ovom izdanju za klastere Hadoop i oluja sustavu Windows i Linux, možete koristiti spremišta podataka Lake samo kao dodatan prostor za pohranu. Takve klastere Azure prostora za pohranu (WASB) i dalje koristiti kao zadani prostor za pohranu. Međutim, za klastere HBase sustavu Windows i Linux, spremišta Lake podataka možete koristiti kao zadani prostor za pohranu ili dodatni prostor za pohranu ili oboje.

Upute za dodjelu resursa za HDInsight klaster s trgovinom Lake podataka potražite u članku:

* [Dodjela resursa za HDInsight klaster s spremišta Lake podataka pomoću portala za Azure](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Dodjela resursa za HDInsight klaster s spremišta Lake podataka pomoću komponente PowerShell Azure](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Videozapisi se više sviđa?** Slijedite veze u nastavku pogledajte videozapise s uputama za korištenje spremišta Lake podataka s klastere HDInsight.

* [Stvaranje programa HDInsight klaster s pristupom spremišta Lake podataka](https://mix.office.com/watch/l93xri2yhtp2)
* Kada je klaster postavite, [podataka programa Access u spremištu Lake podataka pomoću grozd i Svinja skripti](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Korištenje podataka Lake trgovine s analize Lake Azure podataka

[Azure podataka Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) omogućuje rad s velikih skupova podataka na razini oblaka. Dinamički dodjeljuje resurse i omogućuje vam da učinite analize terabajta ili čak i exabytes podataka koje se mogu pohraniti u broju podržanih izvora podataka, jedan od njih se spremište Lake podataka. Analize podataka Lake posebno optimiziran je za rad s Lake spremišta podataka Azure - najvišu razinu performanse, propusnost i parallelization za pružanje radnih opterećenja velikih skupova podataka.

Upute o korištenju analize podataka Lake s trgovinom Lake podataka potražite u članku [Početak rada s podacima Lake analize podataka Lake spremištu](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Videozapisi se više sviđa?** Slijedite veze u nastavku pogledajte videozapise s uputama za korištenje spremišta Lake podataka s klastere HDInsight.

* [Povezivanje Azure podataka Lake analize Lake spremišta podataka za Azure](https://mix.office.com/watch/qwji0dc9rx9k)
* [Pohrana podataka programa Access Azure Lake putem analize podataka Lake](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Korištenje podataka Lake trgovine s tvorničke Azure podataka

[Tvorničke Azure podataka](https://azure.microsoft.com/services/data-factory/) možete koristiti da biste ingest podatke iz baze podataka SQL Azure Azure tablice, Azure SQL DataWarehouse, blob polja za pohranu Azure i lokalne baze podataka. U tijeku prve klase građanima u zajednici za Azure, tvorničke Azure podataka mogu se orkestrirali ingestion podataka iz tih izvora Azure podataka Lake Store.

Upute o korištenju tvorničke podataka Azure s trgovinom Lake podataka potražite u članku [Premještanje podataka i iz spremišta Lake podataka pomoću tvorničke podataka](../data-factory/data-factory-azure-datalake-connector.md).

**Ponovno videozapisi!** U odjeljku [Djelovanje podataka pomoću Azure podataka tvorničke Azure podataka Lake trgovine](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Kopirajte podatke iz blob polja za pohranu Azure u spremište Lake podataka

Spremište Lake podataka za Azure nudi alat naredbenog retka, AdlCopy, koji omogućuje vam da biste kopirali podatke iz spremišta blobova platforme Azure spremišta podataka Lake račun. Dodatne informacije potražite u članku [Kopiranje podataka iz Azure blob polja za pohranu podataka Lake Store](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopiranje podataka između baze podataka SQL Azure i spremište Lake podataka

Uvoz i izvoz podataka između baze podataka SQL Azure i spremište Lake podataka možete koristiti Apache Sqoop. Dodatne informacije potražite u članku [Kopiranje podataka između spremišta Lake podataka i baze podataka Azure SQL pomoću Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

**Pogledajte ovaj videozapis** na [Pomoću Sqoop Apache da biste premjestili sadržaj između relacijske izvore i Lake spremišta podataka za Azure](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Korištenje podataka Lake trgovine s strujanje analize

Spremište Lake podataka možete koristiti kao jedan na izlaze radi pohrane podataka strujanjem pomoću Azure strujanje analize. Dodatne informacije potražite u članku [strujanja podataka iz blobova platforme Azure prostora za pohranu u spremištu Lake podataka pomoću Azure strujanje analize](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Korištenje spremišta za Lake podataka pomoću dodatka Power BI

Power BI možete koristiti da biste uvezli podatke iz spremišta Lake podataka računa za analizu i Vizualizirajte podatke. Dodatne informacije potražite u članku [Analiza podataka u spremištu Lake podataka pomoću dodatka Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Korištenje spremišta za Lake podataka pomoću kataloga podataka

Podatke iz spremišta podataka Lake možete registrirati u katalogu podataka servisa Azure da biste podatke učinili vidljivim cijeloj tvrtki ili ustanovi. Dodatne informacije potražite u članku [registrirati podatke iz spremišta Lake podataka u katalogu podataka Azure](data-lake-store-with-data-catalog.md).


## <a name="see-also"></a>Vidi također

- [Pregled Lake spremišta podataka za Azure](data-lake-store-overview.md)
- [Početak rada s spremišta Lake podataka pomoću portala](data-lake-store-get-started-portal.md)
- [Početak rada s spremišta Lake podataka pomoću komponente PowerShell](data-lake-store-get-started-powershell.md)  
