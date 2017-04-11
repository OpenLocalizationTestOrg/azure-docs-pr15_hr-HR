<properties
    pageTitle="Akcijom skriptu da biste instalirali Solr na Hadoop klaster | Microsoft Azure"
    description="Saznajte kako prilagoditi HDInsight klaster Solr pomoću skripta akcije."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Instaliranje i korištenje Solr na klastere HDInsight Hadoop

Saznajte kako prilagoditi Windows na temelju HDInsight klaster s Solr pomoću akcije skripte i kako koristiti Solr za pretraživanje podataka. Informacije o korištenju Solr sa sustavom Linux klaster potražite u članku [Instalacija i korištenje Solr na klastere HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
Solr na bilo koju vrstu klaster (Hadoop, oluja, HBase, Spark) možete instalirati na Azure HDInsight pomoću *Skripte akcija*. Ogledne skripte da biste instalirali Solr na programa HDInsight klaster dostupna samo za čitanje Azure spremišta blobova na [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1). 

Ogledne skripte funkcionira samo s HDInsight klaster verzijom 3.1. Dodatne informacije o verzijama klaster servisa HDInsight potražite u članku [HDInsight klaster verzije](hdinsight-component-versioning.md).

Ogledna skripta koja se koristi u ovoj temi stvara utemeljen na sustavu Windows Solr klaster s određenu konfiguraciju. Ako želite da biste konfigurirali klaster Solr različite skupove shards, sheme, replike, itd., morate izmijeniti skripte i binarne datoteke Solr sukladno tome.

**Povezani članci**

- [Instaliranje i korištenje Solr na klastere HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Stvaranje Hadoop klastere u HDInsight](hdinsight-provision-clusters.md): opće informacije o stvaranju klastere HDInsight.
- [Prilagodba klaster HDInsight pomoću skripte akcije][hdinsight-cluster-customize]: opće informacije o prilagođavanju klastere HDInsight pomoću skripte akcije.
- [Razvoj akcija skripte skripte za HDInsight](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Što je Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> je platforme enterprise pretraživanje koji omogućuje napredna pretraživanja cijelog teksta na podacima. Dok Hadoop omogućuje pohranu i upravljanje veliku količine podataka, Apache Solr pruža mogućnosti pretraživanja za brzo dohvaćanje podataka. 

## <a name="install-solr-using-portal"></a>Instalacija Solr pomoću portala

1. Stvaranje klaster pomoću mogućnosti za **Stvaranje PRILAGOĐENE** kao što je opisano na [Stvaranje Hadoop klastere u HDInsight](hdinsight-provision-clusters.md#portal).
2. Na stranici čarobnjaka **Akcije skripte** kliknite **dodajte akciju skripte** možete unijeti detalje o akciju skripte kao što je prikazano u nastavku:

    ![Korištenje akcija skriptu da biste prilagodili klaster] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Korištenje akcija skriptu da biste prilagodili klaster")

    <table border='1'>
        <tr><th>Svojstvo</th><th>Vrijednost</th></tr>
        <tr><td>Ime</td>
            <td>Unesite naziv za skripte akciju. Na primjer, <b>Instalirajte Solr</b>.</td></tr>
        <tr><td>URI skripta</td>
            <td>Navedite Uniform Resource Identifier (URI) da biste skriptu koja se poziva da biste prilagodili klaster. Na primjer, <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Vrsta čvora</td>
            <td>Navedite čvorove na kojem se izvodi skripta za prilagodbu. Možete odabrati <b>sve čvorove</b>, <b>samo čvorove glave</b>ili <b>samo čvorove tempiranja</b>.
        <tr><td>Parametri</td>
            <td>Odredite parametre, ako je potrebno skripta. Skripta za instalaciju Solr nisu potrebne parametre, pa možete ostaviti to prazno.</td></tr>
    </table>

    Možete dodati više akcija skriptu da biste instalirali više komponenti na klaster. Nakon što dodate skripte, kliknite kvačicu da biste započeli stvarati klaster.


## <a name="use-solr"></a>Korištenje Solr

Morate pokrenuti s indeksiranje Solr s nekim podatkovnim datotekama. Pokretanje upita za pretraživanje na indeksiranih podataka možete koristiti Solr. Poduzmite sljedeće korake da biste koristili Solr programa klasteru HDInsight:

1. **Korištenje Remote Desktop Protocol (RDP) na udaljenom u klaster HDInsight s Solr instaliran**. Na portalu Azure omogućite udaljene radne površine klaster stvorena pomoću Solr instalirati, a zatim daljinski u klasteru. Upute potražite u članku [Povezivanje s klastere HDInsight pomoću RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Indeks Solr tako da prenesete podatkovne datoteke**. Kada indeksirate Solr, smjestite dokumente u koju je možda ćete morati za pretraživanje. Indeksirati Solr pomoću RDP udaljene u klaster, idite na radnu površinu, otvorite naredbeni redak Hadoop i dođite do **C:\apps\dist\solr-4.7.2\example\exampledocs**. Pokrenite sljedeću naredbu:

        java -jar post.jar solr.xml monitor.xml

    Primijetit ćete sljedeće Izlaz na konzoli sustava:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Uslužni post.jar indeksi Solr s dva uzorka dokumente, **solr.xml** i **monitor.xml**. Uslužni post.jar i ogledni dokumenti dostupni su u Solr instalaciju.

3. **Korištenje Solr nadzornu ploču da biste pretraživali unutar indeksirani dokumenti**. U sesiji RDP HDInsight klaster, otvorite Internet Explorer i pokretanje na nadzornoj ploči Solr pri **solr/http://headnodehost:8983 / #/**. Na lijevom oknu iz **Core birač** padajućem popisu, odaberite **collection1**i unutar, kliknite **upit**. Na primjer, da biste odabrali, a zatim Vrati sve dokumente u Solr, navedite sljedeće vrijednosti:

    * U tekstni okvir **pitanja** unesite ** \*:**\*. To će vratiti u svim dokumentima indeksiranih Solr. Ako želite da biste potražili određeni niz unutar dokumenta, možete unijeti tog niza.
    
    * U tekstni okvir **wt** odaberite izlazni oblik. Zadana vrijednost je **json**. Kliknite **izvršavanje upita**.

    ![Korištenje akcija skriptu da biste prilagodili klaster] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Izvođenje upita na nadzornoj ploči za Solr")
    
    Izlaz vraća dva dokumente koji se koristi za indeksiranje Solr. Rezultat izgleda otprilike ovako:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Preporučeno: sigurnosno kopiranje indeksiranih podataka iz Solr sa spremištem blobova platforme Azure pridružene klaster HDInsight**. Kao dobro, trebali biste sigurnosno kopirajte indeksiranih podataka iz čvorove klaster Solr na spremište blobova platforme Azure. Poduzmite sljedeće korake da biste to učinili:

    1. Iz sesije RDP otvorite Internet Explorer, a zatim pokažite na sljedeći URL:

            http://localhost:8983/solr/replication?command=backup

        Trebali biste vidjeti odgovor ovako:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. U udaljenu sesiju dođite do {SOLR_HOME}\{zbirke} \data. Za klaster stvoren putem ogledne skripte, to mora biti **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Na tom mjestu prikazat će vam snimka mapu stvorena pod nazivom koja je slična * *snimke.* Vremenska oznaka***.

    3. Poštanski broj mapu snimku, a zatim prenese na spremište blobova platforme Azure. Iz naredbenog retka Hadoop idite na mjesto mape snimke pomoću sljedeće naredbe:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Ta se naredba kopira snimka /example/data/u kontejneru unutar zadani prostor za pohranu račun povezan s klaster.

## <a name="install-solr-using-aure-powershell"></a>Instalacija Solr pomoću komponente Aure PowerShell

U odjeljku [Prilagodba HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Na primjer pokazuje kako instalirati Spark pomoću komponente PowerShell Azure. Morate li prilagoditi skriptu da biste koristili [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Instalacija Solr pomoću .NET SDK-a

U odjeljku [Prilagodba HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Uzorak pokazuje kako se instalira Spark pomoću .NET SDK-a. Morate li prilagoditi skriptu da biste koristili [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).



## <a name="see-also"></a>Vidi također

- [Instaliranje i korištenje Solr na klastere HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Stvaranje Hadoop klastere u HDInsight](hdinsight-provision-clusters.md): opće informacije o stvaranju klastere HDInsight.
- [Prilagodba klaster HDInsight pomoću akcije skripta][hdinsight-cluster-customize]: opće informacije o prilagođavanju klastere HDInsight pomoću skripta akcije.
- [Skripte razvoj akcija skripte za HDInsight](hdinsight-hadoop-script-actions.md).
- [Instaliranje i korištenje Spark na klastere HDInsight][hdinsight-install-spark]: ogledne skripte akcija o instaliranju Spark.
- [Kliknite pločicu R klastere HDInsight][hdinsight-install-r]: ogledne skripte akcija o instaliranju R.
- [Instalacija Giraph na HDInsight klastere](hdinsight-hadoop-giraph-install.md): ogledne skripte akcija o instaliranju Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
