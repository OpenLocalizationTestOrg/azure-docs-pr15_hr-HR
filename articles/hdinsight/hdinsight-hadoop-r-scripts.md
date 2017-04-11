<properties
    pageTitle="Korištenje R HDInsight da biste prilagodili klastere | Microsoft Azure"
    description="Saznajte kako instalirati R pomoću akcije skripte i korištenje R na klastere HDInsight."
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
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Instaliranje i korištenje R na klastere HDInsight Hadoop

Saznajte kako prilagoditi Windows temelje HDInsight klaster s R pomoću akcije skripte i upute za korištenje R na HDInsight klaster. [Premium sloju](https://azure.microsoft.com/pricing/details/hdinsight/) koja nudi za HDInsight obuhvaća R poslužitelju kao dio svoj klaster HDInsight. Time se omogućuje skripte R da biste koristili MapReduce i Spark da biste pokrenuli raspodijeljeno computations. Dodatne informacije potražite u članku [Prvi koraci pri korištenju R Normalni prikaz na HDInsight](hdinsight-hadoop-r-server-get-started.md). Informacije o korištenju R sa sustavom Linux klaster potražite u članku [Instalacija i korištenje R na klastere HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
R na bilo koju vrstu klaster (Hadoop, oluja, HBase, Spark) možete instalirati na Azure HDInsight pomoću *Skripte akcija*. Ogledne skripte da biste instalirali R na programa HDInsight klaster dostupna samo za čitanje Azure spremišta blobova na [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1). 

**Povezani članci**

- [Instaliranje i korištenje R na klastere HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Stvaranje Hadoop klastere u HDInsight](hdinsight-provision-clusters.md): opće informacije o stvaranju klastere HDInsight
- [Prilagodba klaster HDInsight pomoću skripte akcije][hdinsight-cluster-customize]: opće informacije o prilagođavanju klastere HDInsight pomoću skripte akcije
- [Razvoj skripti skripte akcija za HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Što je R?

<a href="http://www.r-project.org/" target="_blank">R projekta za statističke računalstvo</a> je Otvori izvor jezik i okruženje za računalstvo Statistika. R nudi stotine ugrađenih statističke funkcije i vlastitu programski jezik koji kombinira aspekte funkcionirati i vezanima uz objekt programiranje. Također nudi proširenom grafička mogućnosti. R je Preferirani okruženje za programiranje za većinu profesionalan statisticians i fizičari u razna polja.

R je kompatibilan s Azure Blob prostora za pohranu (WASB) tako da možete podataka koji je pohranjen na dva moguća pomoću R HDInsight.  

## <a name="install-r"></a>Instalacija R

[Ogledne skripte](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) da biste instalirali R na programa HDInsight klaster dostupna samo za čitanje blobova platforme Azure pohrane u. U ovom se odjeljku daju upute o tome kako koristiti ogledne skripte prilikom stvaranja klaster pomoću portala za Azure.

> [AZURE.NOTE] Ogledne skripte uvedena HDInsight klaster verzijom 3.1. Dodatne informacije o verzijama klaster servisa HDInsight potražite u članku [HDInsight klaster verzije](hdinsight-component-versioning.md).

1. Kada stvorite programa HDInsight klaster s portala, kliknite **Neobavezno konfiguracija**, a zatim **Akcije skripte**.
2. Na stranici **Akcije skripte** unesite sljedeće vrijednosti:

    ![Korištenje akcija skriptu da biste prilagodili klaster] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Korištenje akcija skriptu da biste prilagodili klaster")

    <table border='1'>
        <tr><th>Svojstvo</th><th>Vrijednost</th></tr>
        <tr><td>Ime</td>
            <td>Unesite naziv za skripte akcija, na primjer, <b>R instalirati</b>.</td></tr>
        <tr><td>Skripta URI-JA</td>
            <td>Odredite URI za skriptu koja se poziva da biste prilagodili klaster, na primjer, <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Vrsta čvora</td>
            <td>Navedite čvorove na kojem se izvodi skriptu prilagodbe. Samo možete odabrati <b>Sve čvorove</b>, <b>samo čvorove glave</b>ili <b>čvorove tempiranja</b> .
        <tr><td>Parametri</td>
            <td>Navedite parametre, ako je potrebno skripta. Međutim, skriptu da biste instalirali R ne zahtijeva parametre, pa možete ostaviti to prazno.</td></tr>
    </table>

    Možete dodati više akcija skriptu da biste instalirali više komponenti na klaster. Nakon što dodate skripte, kliknite kvačicu da biste pokrenuli crating klaster.

Možete koristiti i skriptu da biste instalirali R na HDInsight pomoću Azure PowerShell i HDInsight .NET SDK. Upute za ove postupke su navedene u nastavku ovog članka.

## <a name="run-r-scripts"></a>Pokretanje skripti R
U ovom se odjeljku opisuje kako pokrenuti skripte R klaster Hadoop s HDInsight.

1. **Uspostavljanje veze udaljene radne površine klaster**: na portalu Omogućivanje udaljene radne površine za klaster stvorenim pomoću R instalirana i povežite se s klaster. Upute potražite u članku [Povezivanje s klastere HDInsight pomoću RDP](hdinsight-administer-use-management-portal.md#rdp).

2. **Otvorite konzolu za R**: instalacija u R stavlja vezu na konzolu R na radnoj površini glavni čvor. Kliknite da biste otvorili konzolu za R.

3. **Pokrenuti skriptu R**: U R skripte mogu se izvoditi izravno s konzole R, zalijepite je, odaberite je i pritisnite tipku ENTER. Evo jednostavnog primjera skriptu koja generira brojevi od 1 do 100, a zatim ih množi s 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Prva dva retka poziva RHadoop biblioteke koje se instaliraju pomoću R. Posljednjeg retka ispisuje rezultata na konzoli sustava. Izlaz trebao bi izgledati ovako:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Instalacija R pomoću komponente Aure PowerShell

U odjeljku [Prilagodba HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Na primjer pokazuje kako instalirati Spark pomoću komponente PowerShell Azure. Morate li prilagoditi skriptu da biste koristili [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>Instalacija R pomoću .NET SDK-a

U odjeljku [Prilagodba HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Uzorak pokazuje kako se instalira Spark pomoću .NET SDK-a. Morate li prilagoditi skriptu da biste koristili [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).


## <a name="see-also"></a>Vidi također

- [Instaliranje i korištenje R na klastere HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Stvaranje Hadoop klastere u HDInsight](hdinsight-provision-clusters.md): opće informacije o stvaranju klastere HDInsight
- [Prilagodba klaster HDInsight pomoću skripte akcije][hdinsight-cluster-customize]: opće informacije o prilagođavanju klastere HDInsight pomoću skripte akcije
- [Razvoj skripti skripte akcija za HDInsight](hdinsight-hadoop-script-actions.md)
- [Instaliranje i korištenje Spark na klastere HDInsight][hdinsight-install-spark]: ogledne skripte akcija o instaliranju Spark
- [Instalacija Giraph na HDInsight klastere](hdinsight-hadoop-giraph-install.md): ogledne skripte akcija o instaliranju Giraph
- [Instalacija Solr na HDInsight klastere](hdinsight-hadoop-solr-install-linux.md): ogledne skripte akcija o instaliranju Solr.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
