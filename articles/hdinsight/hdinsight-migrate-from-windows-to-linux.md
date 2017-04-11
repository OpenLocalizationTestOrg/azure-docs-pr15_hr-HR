<properties
pageTitle="Migrirati iz HDInsight utemeljen na sustavu Windows da biste sustavom Linux HDInsight | Azure"
description="Saznajte kako migrirati iz utemeljen na sustavu Windows HDInsight klaster sustavom Linux HDInsight klaster."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migrirati iz utemeljen na sustavu Windows HDInsight klaster klaster sa sustavom Linux

Dok je utemeljen na sustavu Windows HDInsight omogućuje jednostavnu da biste koristili Hadoop u oblaku, možda otkrijete morate sustavom Linux klaster da iskoristite prednost Alati i tehnologije koji su potrebni za rješenje. Mnogo je toga u zajednici Hadoop su razvijene na Linux sustave, a neke možda neće biti dostupno za korištenje sa sustavom Windows HDInsight. Osim toga, mnogo knjige, videozapise i druge materijala za obuku pretpostavlja da koristite Linux sustava prilikom rada s Hadoop.

Ovaj dokument sadrži detalje na razlike između HDInsight u sustavu Windows i Linux i upute za migriranje postojećih radnih opterećenja klaster sa sustavom Linux.

> [AZURE.NOTE] HDInsight klastere korištenje Ubuntu dugoročnu službe za podršku (LTS) kao operacijskog sustava za čvorove u klasteru. Informacije o verziji Ubuntu dostupne s HDInsight, uz ostale podatke za rad s verzijama komponente potražite u članku [HDInsight komponente verzije](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Postupak migracije

Općenito tijek rada preseljenja je na sljedeći način.

![Dijagram tijeka rada za migraciju](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Pročitajte svaki odjeljak ovog dokumenta da biste shvatili promjene koje mogu biti potrebne prilikom migracije postojećeg tijeka rada, zadacima koje itd klaster sa sustavom Linux.

2.  Stvorite klaster sa sustavom Linux kao okruženje za testiranje/kvaliteti jamstvo. Dodatne informacije o stvaranju klaster sa sustavom Linux potražite u članku [Stvaranje Linux sustavom klastere u HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3.  Kopirajte postojeće poslove, izvora podataka i primatelji novom okruženju. Potražite u članku kopiranje podataka do odjeljka okruženje za testiranje više pojedinosti.

4.  Izvođenje provjere valjanosti testiranja da biste bili sigurni da poslova funkcioniraju na novu klaster.

Nakon što ste provjeriti radi li sve kako treba, raspored nedostupnost za migraciju. Tijekom ovog nedostupnost učinite sljedeće radnje.

1.  Sigurnosno kopirajte sve podatke za tranzitne spremiti lokalno na čvorove klaster. Na primjer, ako imate podatke pohranjene izravno na glavni čvor.

2.  Brisanje klaster utemeljen na sustavu Windows.

3.  Stvaranje sustavom Linux klaster u istom spremištu podataka zadani koji koriste klaster utemeljen na sustavu Windows. Tako ćete omogućiti novi klaster da biste nastavili raditi na temelju postojeće podatke radnog.

4.  Uvezite podatke tranzitne sigurnosne kopije.

5.  Početak poslove/nastaviti obradu pomoću novog klaster.

### <a name="copy-data-to-the-test-environment"></a>Kopiranje podataka u okruženje za testiranje

Da biste kopirali podatke i zadacima mnogo načina, no izravno dva koji se spominju u ovom odjeljku su najjednostavniji načina za premještanje datoteka test klaster.

#### <a name="hdfs-dfs-copy"></a>Kopiraj HDFS DFS

Da biste izravno kopirali podatke iz spremišta za svoj klaster postojećih radnih prostora za pohranu za novi test klaster na sljedeći način možete koristiti naredbu Hdps HADOOP.

1. Pronaći prostora za pohranu računa i zadane spremnik informacije za svoj klaster postojeće. To možete učiniti pomoću sljedeće skripte Azure PowerShell.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Slijedite korake u klastere utemeljen na stvaranje Linux u dokumentu HDInsight da biste stvorili novo okruženje za testiranje. Zaustavljanje prije stvaranja klaster, a umjesto toga odaberite **Neobavezno konfiguracije**.

3. Plohu neobavezno konfiguracije odaberite **Povezane poslovne subjekte prostora za pohranu**.

4. Odaberite **Dodaj ključa za pohranu**, a kada se to od vas zatraži, odaberite račun za pohranu koji je vratio skriptu PowerShell u koraku 1. Kliknite **Odaberi** na svakom plohu zatvaranja. Na kraju, stvorite klaster.

5. Nakon što klaster, s njim povezati pomoću **SSH.** Ako niste upoznati s komponentom SSH s HDInsight, potražite u nekom od sljedećih članaka.

    * [Korištenje SSH sa sustavom Linux HDInsight klijenata za Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Korištenje SSH sa sustavom Linux HDInsight klijenata Linux, Unix i Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Iz sesije SSH, koristite sljedeću naredbu da biste kopirali datoteke s računa za povezane prostora za pohranu novi zadani račun za pohranu. Zamijenite KONTEJNER i RAČUN kontejner i podatke o računu vratio skriptu PowerShell u koraku 1. Put do podataka zamijenite put u podatkovnu datoteku.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Ako na okruženje za testiranje postojeće strukturu direktorija koji sadrži podatke, možete stvoriti pomoću sljedeće naredbe.

        hdfs dfs -mkdir -p /new/path/to/create

    Na `-p` promjenu omogućuje stvaranje sve imenika put.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Izravni Kopiraj između blob polja za pohranu za Azure

Osim toga, trebali biste koristiti u `Start-AzureStorageBlobCopy` Azure PowerShell cmdlet da biste kopirali blob-ova između računa za pohranu izvan HDInsight. Dodatne informacije potražite u članku kako da biste upravljali Azure blob-Ova sekcija pomoću PowerShell Azure s Azure prostora za pohranu.

##<a name="client-side-technologies"></a>Klijentsko tehnologije

Općenito govoreći, klijentsko tehnologije kao što su [Azure PowerShell cmdleti](../powershell-install-configure.md), [Azure EŽA](../xplat-cli-install.md) ili [.NET SDK Hadoop](https://hadoopsdk.codeplex.com/) će i dalje funkcionira na isti način s operacijskim sustavom Linux klastere kao oslanjate REST API-ji koji su jednaki kroz obje vrste klaster OS.

##<a name="server-side-technologies"></a>Poslužiteljska tehnologija

Sljedeća tablica sadrži smjernice na migriranje poslužiteljsko komponente koje su određene u sustavu Windows.

| Ako koristite tehnologije... | Akcija... |
| ----- | ----- |
| **PowerShell** (poslužiteljsko skripte, uključujući akcije skripte koristi prilikom stvaranja klaster) | Dopune kao tulumu skripti. Akcije skripte potražite u članku [utemeljen na Prilagodba Linux HDInsight s akcijama skripte](hdinsight-hadoop-customize-cluster-linux.md) i [razvoj akcija skripte za HDInsight koji se temelji na Linux](hdinsight-hadoop-script-actions-linux.md). |
| **Azure EŽA** (poslužiteljsko skripte) | Azure EŽA bit će dostupni na Linux, ona ne dolaze instalirana na glavni čvorove klaster HDInsight. Ako vam je potrebna je za skriptiranje poslužiteljsko, potražite u članku [Instalacija EŽA Azure](../xplat-cli-install.md) informacije o instaliranju platforme sustavom Linux. |
| **Komponente .NET** | .NET nisu u potpunosti podržane na klastere sustavom Linux HDInsight. Sustavom Linux oluja na HDInsight klaster stvorene nakon 28/10/2017 podršku C# oluja topologija pomoću SCP.NET framework. Dodatnu podršku za .NET dodat će se u budućnosti ažuriranja. |
| **Komponente Win32 ili druge tehnologije samo za Windows** | Smjernice ovisi o komponenti ili tehnologiju; Možda ćete moći pronaći verziju kompatibilan s Linux ili možda ćete morati pronaći zamjenska rješenja ili dopune komponente. |

##<a name="cluster-creation"></a>Stvaranje klaster

U ovom se odjeljku nudi informacije o razlikama između stvaranja klaster.

### <a name="ssh-user"></a>SSH korisnika

Sustavom Linux klastere HDInsight koristi protokol **Secure ljuske (SSH)** za daljinski pristup čvorove klaster. Za razliku od udaljene radne površine za utemeljen na sustavu Windows klastere Većina SSH klijenti nude grafičko korisničko sučelje, ali umjesto nudi na naredbenog retka koji omogućuje vam pokretanje naredbe na klaster. Neki klijenti (kao što su [MobaXterm](http://mobaxterm.mobatek.net/),) navedite preglednika za sustav grafičke datoteke osim u alat za analizu daljinske naredbenog retka.

Tijekom stvaranja klaster, morate navesti korisnik sustava SSH i ili **lozinku** ili **javni ključ certifikat** za provjeru autentičnosti.

Preporučujemo korištenje certifikata javnog ključa, kao što je dodatno zaštititi od lozinke. Provjera autentičnosti potvrda radi tako da generiranje traje par ključa javno/privatno, a zatim pruža javni ključ prilikom stvaranja klaster. Prilikom povezivanja s poslužiteljem pomoću SSH, privatni ključ na klijentskom računalu omogućuje provjeru autentičnosti za vezu.

Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u članku:

- [Korištenje SSH s HDInsight klijenata za Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- [Korištenje SSH s HDInsight klijenata Linux, Unix i OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Prilagodba klaster

Mora koristiti sa sustavom Linux klastere **Akcije skripte** pisane tulumu skripte. Dok akcije skripte, može koristiti prilikom stvaranja klaster za klastere Linux temelji mogu biti koriste se za prilagodbu program izvrši nakon klaster je prema gore i pokrenut. Dodatne informacije potražite u članku [utemeljen na Prilagodba Linux HDInsight s akcijama skripte](hdinsight-hadoop-customize-cluster-linux.md) i [razvoj akcija skripte za HDInsight koji se temelji na Linux](hdinsight-hadoop-script-actions-linux.md).

Druge prilagodbe značajka je **Samopokretanje**. Za klastere Windows Time da biste odredili mjesto dodatne biblioteke za korištenje s grozd. Nakon stvaranja klaster te biblioteke automatski dostupni su za korištenje s upitima grozd bez potrebe za korištenje `ADD JAR`.

Samopokretanja programa za klastere sustavom Linux ne nudi tu funkciju. Umjesto toga koristite akciju skripte navedenih u [bibliotekama dodavanje vrste Hive tijekom stvaranja klaster](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtualne mreže

Utemeljen na sustavu Windows HDInsight klaster samo rad s klasični virtualne mreže dok sustavom Linux HDInsight klastere zahtijevaju resursima virtualne mreže. Ako imate resursa u klasični virtualne mreže koje morate povezati Linux HDInsight klaster, potražite u članku [Povezivanje Classic virtualne mreže s mrežom virtualne Voditelj resursa](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Dodatne informacije o konfiguraciji preduvjeti za Azure virtualne mreže pomoću servisa HDInsight potražite u članku [Mogućnosti proširivanje servisa HDInsight pomoću virtualne mreže](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Upravljanje i nadzor

Mnoge UIs ste možda koristili s utemeljen na sustavu Windows HDInsight, kao što su dosadašnje iskustvo ili Yarn korisničko Sučelje web-mjesta dostupnih putem Ambari. Osim toga, prikazu vrste Hive Ambari omogućuje pokretanje grozd upita pomoću web-preglednika. Korisničko Sučelje Web Ambari dostupna je na sustavom Linux klastere pri https://CLUSTERNAME.azurehdinsight.net.

Dodatne informacije o radu s Ambari potražite u članku sljedeće dokumente:

- [Ambari Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST API-JA](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari upozorenja

Ambari ima upozorenja sustav koji mogu sadržavati od mogućih problema s klaster. Upozorenja prikazuju se kao žutu ili crvenu stavke u korisničkom Sučelju Web Ambari, no možete ih učitati i putem REST API-JA.

> [AZURE.IMPORTANT] Upozorenja Ambari označavaju te postoji *možda* se problem ne te došlo *je* problem. Ako, na primjer, možda ćete primiti upozorenje da HiveServer2 nije moguće pristupiti čak i ako se obično pristup.
>
> Broj upozorenja primjenjuju kao utemeljen na interval upite odabiranja servisa, a očekivati odgovor u određeni vremenski okvir. Da bi se upozorenje ne mora značiti da je servis prema dolje, samo da je dao rezultate u očekivanom vremenski okvir.

Općenito govoreći, trebali biste procijeniti li upozorenje koje je omogućeno pojavljivanja dulje ili zrcali probleme korisnika koje ste prethodno prijavljena klaster prije poduzimanja akcija na njemu.

##<a name="file-system-locations"></a>Mjesta datoteka sustava

Datotečni sustav klaster Linux položeno je drugačije klastere HDInsight utemeljen na sustavu Windows. U sljedećoj su tablici omogućuje pronalaženje korištenih datoteka.

| Morate pronaći... | Se nalazi... |
| ----- | ----- |
| Konfiguracija | `/etc`. Na primjer,`/etc/hadoop/conf/core-site.xml` |
| Datoteke zapisnika | `/var/logs` |
| Platforme Hortonworks podataka (HDP) | `/usr/hdp`. Postoje dvije mape koja se nalazi ovdje, onu koja je trenutno HDP verzija (na primjer, `2.2.9.1-1`,) i `current`. Na `current` directory sadrži simboličke veze na datoteke i mape koja se nalazi u direktoriju broj verzije pa je navedeno kao praktičan način pristupa datotekama HDP od verzije broj promijenit će kao što je verzija HDP ažurira. |
| hadoop streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Općenito govoreći, ako znate naziv datoteke, koristite sljedeću naredbu iz sesiju SSH da biste pronašli put datoteke:

    find / -name FILENAME 2>/dev/null

Zamjenske znakove možete koristiti i s nazivom datoteke. Na primjer, `find / -name *streaming*.jar 2>/dev/null` će vratiti put u posudu datoteke koje sadrže riječ "strujanje" kao dio naziva datoteke.

##<a name="hive-pig-and-mapreduce"></a>Grozd, Svinja i MapReduce

Svinja i MapReduce radnih opterećenja vrlo su slične na sustavom Linux klastere – razlika ako koristite udaljenu radnu površinu za povezivanje s klaster utemeljen na sustavu Windows i pokrenuli zadacima, SSH će koristiti sa sustavom Linux klastere.

- [Korištenje Svinja s SSH](hdinsight-hadoop-use-pig-ssh.md)

- [Korištenje MapReduce s SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Grozd

Sljedeći grafikon sadrži smjernice o migraciji radnih opterećenja sustava grozd.

| Na utemeljen na sustavu Windows, koristiti... | Na koji se temelji na Linux... |
| ----- | ----- |
| **Uređivač grozd** | [Prikaz vrste Hive u Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Da biste omogućili Tez | Tez je modul izvođenja zadano za klastere sustavom Linux tako izjava skup više nije potrebna. |
| Datoteka CMD ili skripte na poslužitelju pozvati kao dio posla grozd | Koristite tulumu skripte |
| `hive`Naredba za udaljene radne površine | Korištenje [Beeline](hdinsight-hadoop-use-hive-beeline.md) ili [vrste Hive iz sesiju SSH](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Oluja

| Na utemeljen na sustavu Windows, koristiti... | Na koji se temelji na Linux... |
| ----- | ----- |
| Oluja nadzorne ploče | Na nadzornoj ploči oluja nije dostupna. U odjeljku [Topologija uvođenje i upravljanje oluja na sustavom Linux HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) za načina slanja topologija |
| Oluja korisničkog Sučelja | Korisničko Sučelje oluja dostupna je na https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio da biste stvorili, uvođenje i upravljanje C# ili hibridnog topologija | Visual Studio se može koristiti za stvaranje, uvođenje i upravljanje C# (SCP.NET) ili hibridnog topologija na sustavom Linux oluja na HDInsight klastere stvorene nakon 28/10/2017. |

##<a name="hbase"></a>HBase

Na koji se temelji na Linux klastere znode nadređene HBase je `/hbase-unsecure`. Morate postaviti to u konfiguraciji za bilo kojeg klijenta Java aplikacije koje koriste izvorni HBase Java API-JA.

Potražite u članku [Sastavljanje HBase utemeljena na aplikaciju](hdinsight-hbase-build-java-maven.md) za klijent za primjer postavlja tu vrijednost.

##<a name="spark"></a>Spark

Spark klastere su bili dostupni na Windows klastere tijekom pretpregleda; Međutim, za izdanje Spark dostupna je samo sa sustavom Linux klastere. Postoji bez migracije put od klaster za pretpregled utemeljen na sustavu Windows Spark na klaster sustavom Linux Spark izdanje.

##<a name="known-issues"></a>Poznati problemi

### <a name="azure-data-factory-custom-net-activities"></a>Azure podataka tvorničke prilagođeni .NET aktivnosti

Azure podataka tvorničke prilagođeni .NET aktivnosti trenutno nisu podržane na klastere sustavom Linux HDInsight. Umjesto toga koristite jedan od sljedećih načina za implementaciju prilagođene aktivnosti kao dio sustava ADF kanal.

-   Izvršavanje aktivnosti .NET na obradu Azure skup. U odjeljku grupe za Azure pomoću povezane servis [Koristi prilagođene](../data-factory/data-factory-use-custom-activities.md#AzureBatch) aktivnosti u kanalu na tvorničke Azure podataka

-   Implementacija aktivnosti kao MapReduce aktivnost. Dodatne informacije potražite u članku [Pozivanje programe MapReduce tvorničke podataka](../data-factory/data-factory-map-reduce.md) .

### <a name="line-endings"></a>Crte kod

Općenito govoreći, crte kod na sustave Windows koriste riječ o CRLF, dok Linux sustave koriste LF. Ako proizvesti ili očekivati podataka pomoću riječ o CRLF crte kod ćete morati izmijeniti proizvođača ili koje korisnici za rad s završetak crte LF.

Ako, na primjer, pomoću komponente PowerShell Azure HDInsight upit na klaster utemeljen na sustavu Windows, vratiti podatke s riječ o CRLF. Isti upit sa sustavom Linux klaster će vratiti LF. U mnogim slučajevima, to nije važno ima korisničke podatke, no trebali biste imaju prije migracije klaster sa sustavom Linux.

Ako imate skripte koje će se izvršavati izravno na čvorove Linux klaster (kao što su skripta Python koja se koristi s grozd ili posao MapReduce), koristite uvijek LF kao završetak crte. Ako koristite riječ o CRLF, možda ćete vidjeti pogreške prilikom izvođenja skripte na klasteru sa sustavom Linux.

Ako znate da skripte sadrže nizove ugrađene CR znakova, možete je masovno promjena crte kod na jedan od sljedećih načina:

-   **Ako imate skripte koje namjeravate na prijenos klaster**, koristite sljedeće naredbe ljuske PowerShell da biste promijenili crte kod riječ o CRLF u LF prije prijenosa skriptu da biste klaster.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Ako imate skripte koje se već nalaze u prostor za pohranu koriste klaster**, koristite sljedeću naredbu iz sesiju SSH klaster sustavom Linux da biste izmijenili skriptu.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Daljnji koraci

-   [Saznajte kako stvoriti klastere sustavom Linux HDInsight](hdinsight-hadoop-provision-linux-clusters.md)

-   [Povezivanje sa sustavom Linux klaster pomoću SSH iz klijenta za Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Povezivanje sa sustavom Linux klaster pomoću SSH od klijenta Linux, Unix ili Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Upravljanje sustavom Linux klaster pomoću Ambari](hdinsight-hadoop-manage-ambari.md)
