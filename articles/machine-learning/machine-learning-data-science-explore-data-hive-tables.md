<properties
    pageTitle="Istraživanje podataka u grozd tablica s upitima grozd | Microsoft Azure"
    description="Istraživanje podataka u tablicama grozd korištenje grozd upita."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Istraživanje podataka u grozd tablica s upitima grozd

Ovaj dokument sadrži ogledne skripte grozd koji se koriste za istraživanje podataka u tablicama grozd programa klasteru HDInsight Hadoop.

Sljedeće veze **izbornika** na temu koja opisuje kako pomoću alata za istraživanje podataka iz različitih okruženja za pohranu.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da imate:

* Stvorili račun Azure prostora za pohranu. Ako vam je potrebna upute, potražite u članku [Stvaranje računa za pohranu za Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Dodjeli prilagođene klaster Hadoop sa servisom HDInsight. Ako vam je potrebna upute, potražite u članku [Prilagodba Azure HDInsight Hadoop klastere za napredne analize](machine-learning-data-science-customize-hadoop-cluster.md).
* Podaci prenesena grozd tablica u klastere Azure HDInsight Hadoop. Ako nije, slijedite upute u [Stvaranje i učitavanja podataka s tablicama grozd](machine-learning-data-science-move-hive-tables.md) da je najprije prenesete podataka grozd tablice.
* Omogućeno daljinski pristup klaster. Ako vam je potrebna upute, potražite u članku [pristup glave čvor od Hadoop klaster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Ako vam je potrebna upute za slanje upita grozd, potražite u članku [Slanje vrste Hive upitima](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Primjer grozd upita skripte za istraživanje podataka

1. Broj opažanja po partition `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Broj opažanja prema danu `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Dohvaćanje razina u categorical stupca  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Početak broj razina u kombinaciji s dva stupca categorical `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Početak distribucije za brojčanih stupaca  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Izdvajanje zapisa pridruživanje dvije tablice

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Dodatni upita skripte za taksi putovanja podataka scenariji

Primjeri upite specifične za scenarijima [NEW taksi putovanja podataka](http://chriswhong.com/open-data/foil_nyc_taxi/) također se navode u [spremištu Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Tih upita već imate navedeni shema podataka i spremni ste za slanje da biste pokrenuli.
