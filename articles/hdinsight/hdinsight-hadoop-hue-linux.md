<properties
    pageTitle="Korištenje nijanse s Hadoop na klastere HDInsight Linux | Microsoft Azure"
    description="Saznajte kako instalirati i koristiti nijanse s klastere Hadoop na HDInsight Linux."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Instaliranje i korištenje nijanse na klastere HDInsight Hadoop

Saznajte kako instalirati nijanse na klastere HDInsight Linux i koristiti tuneliranje da biste usmjerili zahtjeve na nijanse.

## <a name="what-is-hue"></a>Što je nijanse?

Nijanse je skup web-aplikacije koje se koriste za interakciju s Hadoop klaster. Možete koristiti nijansu pregled prostora za pohranu pridružene klaster Hadoop (WASB slučaju HDInsight klastere), pokrenite grozd zadacima i Svinja skripte itd. Sljedeće komponente su dostupni nijanse instalacije na programa klaster HDInsight Hadoop.

* Uređivač grozd beeswax
* Svinja
* Upravitelj Metastore
* Oozie
* FileBrowser (koji razgovara WASB zadani spremnik)
* Preglednik posla

> [AZURE.WARNING] Komponente dao klaster HDInsight potpuno podržane, a Microsoft Support pomoći će vam da biste izdvojili i rješavanje problema vezanih uz te komponente.
>
> Prilagođene komponente dobili komercijalno pametnije podršku radi daljnje rješavanje problema. To može rezultirati rješavanju problema ili s pitanjem želite li sudjelovati dostupnih kanala tehnologija Otvori izvor gdje se nalazi niže stručna znanja za taj tehnologiju. Na primjer, postoje mnogo web-mjesta zajednice koje je moguće koristiti, npr.: [MSDN forum za HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekti Apache imaju web-mjesta projekta na [http://apache.org](http://apache.org), na primjer: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Instalacija nijanse pomoću akcije skripte

Da biste instalirali nijanse na sustavom Linux HDInsight Klaster se poslužite sljedeću skriptu akciju.
https://hdiconfigactions.blob.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.SH
    
Ovo poglavlje sadrži upute o tome kako koristiti skriptu prilikom dodjele resursa klaster pomoću portala za Azure. 

> [AZURE.NOTE] Azure PowerShell, EŽA Azure, HDInsight .NET SDK ili Voditelj resursa Azure predložaka također se poslužite da biste primijenili akcije skripte. Akcije skripte možete primijeniti i na već pokrenut klastere. Dodatne informacije potražite u članku [Prilagodba HDInsight klastere s akcijama skripte](hdinsight-hadoop-customize-cluster-linux.md).

1. Pokretanje dodjeljivanje klaster pomoću koraka u [klastere HDInsight dodjele resursa na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), ali ne dovrši dodjele resursa.

    > [AZURE.NOTE] Da biste instalirali nijanse na HDInsight klastere, veličina preporučene headnode barem je A4 (8 jezgri, 14 GB memorije).

2. Na plohu **Neobavezno konfiguracije** odaberite **Akcije skripte**i navedite podatke kao što je prikazano u nastavku:

    ![Osiguraj parametara radnje za skripte za nijanse] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Osiguraj parametara radnje za skripte za nijanse")

    * __Naziv__: Upišite neslužbeni naziv za skripte akciju.
    * __SKRIPTA URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __LAKŠI__: Odaberite ovu mogućnost
    * __TEMPIRANJA__:, polje ostavite prazno.
    * __ZOOKEEPER__:, polje ostavite prazno.
    * __Parametri__:, polje ostavite prazno.

3. Pri dnu **Akcije skripte**, pomoću gumba **Odaberite** spremanje konfiguracije. Na kraju, pomoću gumba **Odaberite** pri dnu plohu **Neobavezno konfiguracije** da biste spremili informacije o konfiguraciji nije obavezno.

4. Nastavite dodjeljivanje klaster, kao što je opisano u [klastere HDInsight dodjele resursa na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="use-hue-with-hdinsight-clusters"></a>Korištenje nijanse s klastere HDInsight

SSH Tunneling je jedini način da biste pristupili nijanse na klaster kada je pokrenut. Tuneliranje putem SSH dopušta promet da biste izravno otvorili headnode klaster kojem se pokreće nijanse. Nakon klaster Završi dodjele resursa, koristiti sljedeće korake nijanse na programa klaster HDInsight Linux.

1. Pomoću informacija u [Pomoću SSH tuneliranje da biste pristupili Ambari web korisničkog Sučelja, ResourceManager, JobHistory, NameNode, Oozie, i druge web-mjesta korisničkog Sučelja](hdinsight-linux-ambari-ssh-tunnel.md) da biste stvorili programa tunelom SSH klijentskim sustavima HDInsight klaster, a zatim konfigurirajte web-preglednik da biste upotrijebili tunelom SSH proxy poslužitelj.

2. Kada ste stvorili u tunelom SSH i konfigurirali vaš preglednik da biste promet proxy kroz nju, morate pronaći naziv glavnog računala primarni glavni čvora. To možete učiniti povezivanjem klaster pomoću SSH na priključak 22. Na primjer, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` pri čemu je __korisničko ime__ SSH korisničko ime, a __CLUSTERNAME__ naziv svoj klaster.

    Dodatne informacije o korištenju SSH potražite u članku sljedeće dokumente:

    * [Korištenje SSH sa sustavom Linux HDInsight od klijenta Linux, Unix ili Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Korištenje SSH sa sustavom Linux HDInsight iz klijenta za Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Nakon uspostave, koristite sljedeću naredbu da biste nabavili na potpuno kvalificirani naziv domene od primarni headnode:

        hostname -f

    To će vratiti naziv sličnu ovoj:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    To je naziv glavnog računala od primarni headnode gdje se nalazi nijanse web-mjesta.

2. Pomoću web-pregledniku otvorite portal nijanse na http://HOSTNAME:8888. Naziv glavnog računala zamijenite nazivom ste nabavili u prethodnom koraku.

    > [AZURE.NOTE] Kad se prijavite u prvi put, zatražit će se da biste stvorili račun za prijavu na portal nijanse. Vjerodajnice koje navedete bit će ograničeni na portalu i nisu povezane administrator ili SSH korisničke vjerodajnice za koje ste naveli prilikom dodjele resursa klaster.

    ![Prijavite se na portal nijanse] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Navedite vjerodajnice za portal nijanse")

### <a name="run-a-hive-query"></a>Izvođenje upita grozd

1. Na portalu nijanse **Uređivača upita**kliknite, a zatim otvorite uređivač grozd **vrste Hive** .

    ![Korištenje grozd] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Korištenje grozd")

2. Na kartici **Pomoć** u odjeljku **baze podataka**trebali biste vidjeti **hivesampletable**. Ovo je primjer tablice koje se isporučuju uz sve klastere Hadoop na HDInsight. Unesite primjer upita u desnom oknu, a zatim potražite u članku Izlaz na kartici **rezultate** u oknu ispod, kao što je prikazano u snimku zaslona.

    ![Pokretanje vrste Hive upita] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Pokretanje vrste Hive upita")

    Kartica **grafikon** možete koristiti i da biste vidjeli vizualni prikaz rezultata.

### <a name="browse-the-cluster-storage"></a>Pregled prostora za pohranu klaster

1. Na portalu nijanse kliknite **Preglednik datoteka** u gornjem desnom kutu traku izbornika.

2. Po zadanom pregledniku datoteka se otvara u direktorij **/user/myuser** . Kliknite kosa crta udesno prije imenika korisnika na putu da biste prešli na korijenskom spremnika Azure prostora za pohranu pridružene klaster.

    ![Korištenje datoteka preglednika] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Korištenje datoteka preglednika")

3. Desnom tipkom miša kliknite datoteku ili mapu da biste vidjeli dostupne operacije. Pomoću gumba za **Prijenos** u desnom kutu da biste prenijeli datoteke trenutnog direktorija. Pomoću gumba **Novo** da biste stvorili novi datoteke ili mape.

> [AZURE.NOTE] Nijanse datoteke web-pregledniku možete prikazati samo sadržaj spremnik zadani pridružene klaster HDInsight. Bilo koji dodatni prostor za pohranu računi/spremnika koji ste možda pridružili klaster neće biti dostupna u pregledniku datoteke. Međutim, dodatne spremnika pridružene klaster uvijek će biti dostupna za poslove grozd. Na primjer, ako unesete naredbu `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` u uređivaču grozd, možete vidjeti sadržaj kao i dodatnih spremnika. U toj naredbi **newcontainer** nije zadani spremnik pridružene klaster.

## <a name="important-considerations"></a>Važne napomene

1. Skripta koja se koristi da biste instalirali nijanse se instalira samo na primarni headnode od klaster.

2. Tijekom instalacije većem broju servisa Hadoop (HDFS, YARN, MR2, Oozie) ponovno pokrenete o ažuriranju konfiguracije. Završetku skriptu instalacije nijanse, može potrajati neko vrijeme za druge servise Hadoop pokretanje prema gore. To može utjecati na performanse nijanse na početku. Kada sve servise pokretanja, nijanse bit će potpuno funkcionalni.

3.  Nijanse ne razumije Tez zadataka, koja je trenutno zadane postavke grozd. Ako želite da biste upotrijebili MapReduce izvođenja modul grozd, ažurirajte skriptu da koristite sljedeću naredbu u skriptu:

        set hive.execution.engine=mr;

4.  Možete odrediti scenarij u kojem servisa su pokrenuti na primarni headnode pokrenutom Voditelj resursa može se na sekundarnom s klastere Linux. Takve scenarij može rezultirati pogreške (prikazano u nastavku) prilikom korištenja nijanse da biste vidjeli detalje o zadacima POKRENUT na klaster. Međutim, možete vidjeti detalje zadatka kada je zadatak dovršen.

    ![Nijanse portala pogreške] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Nijanse portala pogreške")

    To je zbog poznat problem. Kao zaobilazno rješenje, izmijeniti Ambari tako da se aktivni Voditelj resursa i pokreće na primarni headnode.

5.  Nijanse razumije WebHDFS dok HDInsight klastere koristite za pohranu Azure pomoću `wasbs://`. Tako, prilagođene skripte koja se koristi s akcijom skripte instalira WebWasb koji je servis kompatibilan s preglednikom WebHDFS razgovor WASB. Tako, čak i ako je portal nijanse kaže HDFS u mjesta (na primjer, kada pomaknite miš preko **Preglednik datoteka**) je potrebno ga je protumačiti kao WASB.


## <a name="next-steps"></a>Daljnji koraci

- [Instalacija Giraph na klastere HDInsight](hdinsight-hadoop-giraph-install-linux.md). Da biste instalirali Giraph na HDInsight Hadoop klastere pomoću klaster prilagodbe. Giraph omogućuje vam da biste izvršili obrada grafikonu pomoću Hadoop i može se koristiti sa servisom Azure HDInsight.

- [Instalacija Solr na klastere HDInsight](hdinsight-hadoop-solr-install-linux.md). Da biste instalirali Solr na HDInsight Hadoop klastere pomoću klaster prilagodbe. Solr omogućuje izvođenje operacija napredna pretraživanja na pohranjene podatke.

- [Instalacija R na klastere HDInsight](hdinsight-hadoop-r-scripts-linux.md). Instalirajte R na klastere HDInsight Hadoop pomoću klaster prilagodbe. R je Otvori izvor jezik i okruženje za računalstvo Statistika. Pruža stotine ugrađene statističke funkcije i vlastitu programski jezik koji kombinira aspekte funkcionirati i vezanima uz objekt programiranje. Također nudi proširenom grafička mogućnosti.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
