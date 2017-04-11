<properties
   pageTitle="Kopirajte podatke i iz WASB u spremištu Lake podataka pomoću Distcp | Microsoft Azure"
   description="Kopirajte podatke i s blob-ova za pohranu Azure spremište Lake podataka pomoću alata za Distcp"
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
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Korištenje Distcp da biste kopirali podatke između blob polja Azure prostora za pohranu i spremište Lake podataka

> [AZURE.SELECTOR]
- [Korištenje DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Korištenje AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)


Nakon stvaranja klaster za HDInsight koji ima pristup računu za spremište Lake podataka, možete koristiti alate za zajednici Hadoop kao što su Distcp da biste kopirali podatke **iz** spremišta klaster HDInsight (WASB) na račun za spremište Lake podataka. Ovaj članak sadrži upute o tome kako biste to postigli.

##<a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- **Omogućivanje pretplate Azure** spremišta podataka Lake javno pretpregled. Pročitajte [upute](data-lake-store-get-started-portal.md#signup).
- **Klaster azure HDInsight** pomoću programa access s računom spremišta Lake podataka. Potražite u članku [Stvaranje programa HDInsight klaster s trgovinom Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md). Provjerite je li za klaster Omogući udaljenu radnu površinu.

## <a name="do-you-learn-fast-with-videos"></a>Učite brzo s videozapisima?

[Pogledajte ovaj videozapis](https://mix.office.com/watch/1liuojvdx6sie) o tome da biste kopirali podatke između blob polja Azure prostora za pohranu i spremište Lake podataka pomoću DistCp.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Korištenje Distcp iz udaljene radne površine (Windows klaster) ili SSH (Linux klaster)

U sklopu programa HDInsight klaster uslužni Distcp koji se može koristiti da biste kopirali podatke iz različitih izvora u programa klaster HDInsight. Ako ste konfigurirali klaster HDInsight da koristi spremište Lake podataka kao dodatan prostor za pohranu, uslužni Distcp može biti korištenih u novom alatu-tvorničke da biste kopirali podatke i s računom spremišta Lake podataka. U ovom odjeljku ne možemo pogledajte upute za korištenje uslužni Distcp.

1. Ako imate Windows klaster, udaljene u programa klaster HDInsight koji ima pristup računu za spremište Lake podataka. Upute potražite u članku [Povezivanje s klastere pomoću RDP](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). Iz skupine radne površine, otvorite naredbeni redak Hadoop.

    Ako imate Linux klaster, koristite SSH za povezivanje s klaster. Pogledajte članak [Povezivanje sa sustavom Linux HDInsight klaster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). Pokretanje naredbe iz SSH upit.

3. Provjerite možete li pristupiti Azure blob polja za pohranu (WASB). Pokrenite sljedeću naredbu:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    To mora sadržavati popis sadržaja u spremište blobova.

4. Isto tako, provjerite ima li pristup računa spremišta Lake podataka iz skupine. Pokrenite sljedeću naredbu:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    To mora sadržavati popis datoteka/mapa u spremištu Lake podataka računa.

5. Da biste kopirali podatke iz WASB spremišta podataka Lake račun pomoću Distcp.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Sadržaj **** mape/primjer/podataka/gutenberg/će kopirati u WASB na **/myfolder** u spremištu Lake podataka računa.

6. Slično tome, koristite Distcp da biste kopirali podatke iz spremišta podataka Lake računa WASB.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Sadržaj **/myfolder** će kopirati u računa spremišta podataka **** Lake/primjer/podataka/gutenberg/mapu WASB.

## <a name="see-also"></a>Vidi također

- [Kopirajte podatke iz blob polja za pohranu Azure u spremištu Lake podataka](data-lake-store-copy-data-azure-storage-blob.md)
- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
