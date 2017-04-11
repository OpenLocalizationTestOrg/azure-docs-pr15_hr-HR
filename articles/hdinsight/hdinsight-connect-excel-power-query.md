<properties
    pageTitle="Povezivanje programa Excel s Hadoop pomoću značajke Power Query | Microsoft Azure"
    description="Saznajte kako koristiti prednosti poslovne inteligencije komponente i pomoću dodatka Power Query za Excel za pristup podacima koji su spremljeni u Hadoop na HDInsight."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Povezivanje programa Excel s Hadoop pomoću dodatka Power Query

Jedan ključa značajka rješenja za Microsoft velikih skupova podataka je integraciju komponente Microsoft poslovne inteligencije (BI) s Hadoop klastere u Azure HDInsight. Primjer Ta integracija s primarni je mogućnost povezivanje programa Excel s računom za pohranu Azure koja sadrži podatke povezane s svoj klaster Hadoop pomoću dodatka Microsoft Power Query za dodatka programa Excel. U ovom se članku vodit će vas kroz upute za postavljanje i korištenje dodatka Power Query za upit podatke povezane s Hadoop klaster upravlja sa servisa HDInsight.

> [AZURE.NOTE] Dok se koraci u ovom članku može se koristiti s Linux ili utemeljen na sustavu Windows HDInsight klaster, za klijent radne stanice potreban je Windows.

### <a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Klaster programa HDInsight**. Da biste konfigurirali, potražite u članku [Početak rada sa servisom Azure HDInsight][hdinsight-get-started].
- **A radne stanice** sa sustavom Windows 7, Windows Server 2008 R2 ili noviji operacijski sustav.
- **Office 2013 Professional Plus, Office 365 ProPlus, samostalno izdanje programa Excel 2013 ili Office 2010 Professional Plus**.


## <a name="install-power-query"></a>Instalacija dodatka Power Query

Power Query može se koristiti za uvoz podataka iz različitih izvora u programu Microsoft Excel, gdje je power BI alata kao što je PowerPivot i Power View. Posebno Power Query možete uvesti podatke koji Izlaz ili koje generira Hadoop posla sustavom programa klaster HDInsight.

Preuzmite Microsoft Power Query za Excel iz [Microsoftova centra za preuzimanje] [ powerquery-download] i instalirajte ga.

## <a name="import-hdinsight-data-into-excel"></a>Uvoz podataka HDInsight u programu Excel

Dodatak za Power Query za Excel olakšava uvoz podataka iz svoj klaster HDInsight u Excel, gdje alatima za Poslovno obavještavanje kao što je PowerPivot i Power Map mogu se koristiti za provjeru, analizu i izlaganje podataka.

**Da biste uvezli podatke iz programa HDInsight klaster**

1. Otvorite Excel.

2. Stvaranje nove prazne radne knjige.

3. Kliknite izbornik **Power Query** , kliknite **Azure iz**, a zatim **Iz servisa Microsoft Azure HDInsight**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Bilješke:** Ako ne vidite izbornik **Power Query** , idite na **datoteka** > **Mogućnosti** > **Dodaci**, a zatim odaberite **COM dodaci** , u padajućem okviru **Upravljanje** pri dnu stranice. Odaberite gumb **Idi...** i provjerite da je potvrđen okvir za Power Query za dodatka programa Excel.

    **Bilješke:** Power Query omogućuje i uvoz podataka iz HDFS tako da kliknete **Iz drugih izvora**.

3. Za **Naziv računa**, unesite naziv računa spremišta blobova platforme Azure pridružene svoj klaster, a zatim kliknite **u redu**. To se može biti [zadani prostor za pohranu računa](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) ili računa povezane prostora za pohranu.  Odaberite oblik *https://<StorageAccountName>.blob.core.windows.net/*.

4. **Ključ za račun**unesite ključ za račun spremište blobova platforme, a zatim kliknite **Spremi**. (Morate učiniti samo prvi put kada pristupite ovo spremište.)

5. U oknu **Navigator** na lijevoj strani uređivača upita, dvokliknite ime za spremnik spremišta blobova platforme. Po zadanom je naziv spremnik isti naziv kao naziv klaster.

6. Pronađite **hivesampledata.txt to** u stupcu **naziv** (put do mape je **.. / vrste hive/skladištu/hivesampletable/**), a zatim kliknite **Binarni** na lijevoj strani hivesampledata.txt to. U sklopu hivesampledata.txt to sve klaster. Po želji možete koristiti vlastitu datoteku.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Ako želite, možete preimenovati nazive stupaca. Kada ste spremni, kliknite **Zatvori i Učitaj**.  Učitan je podatke u radnu knjigu:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku naučili kako pomoću dodatka Power Query za dohvaćanje podataka iz servisa HDInsight u programu Excel. Isto tako, možete dohvatiti podatke iz servisa HDInsight u bazu podataka SQL Azure. Također je moguće prenijeti podatke u HDInsight. Dodatne informacije potražite u sljedećim člancima:

* [Povezivanje programa Excel sa servisom HDInsight pomoću ODBC upravljački program Microsoft grozd][hdinsight-ODBC]
* [Prijenos podataka HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
