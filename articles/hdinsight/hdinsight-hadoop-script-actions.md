<properties
    pageTitle="Skripte razvoja akciju s HDInsight | Microsoft Azure"
    description="Saznajte kako prilagoditi Hadoop klastere skripte akciju. Skripta akciju mogu se instalirati dodatni softver koji se izvodi na Hadoop klaster ili promijenili konfiguraciju aplikacija instalirana na klaster."
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

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Razvoj skripte akcija skripte za klastere utemeljen na sustavu HDInsight Windows

Saznajte kako napisati skripte akcija skripte za HDInsight. Informacije o korištenju skripte skripte akciju, potražite u članku [Prilagodba HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster.md). U istom članku sastavljene za klastere sustavom Linux HDInsight potražite u članku [razviti akcija skripte skripte za HDInsight](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] Informacije u ovom dokumentu je za klastere HDInsight utemeljen na sustavu Windows. Informacije o korištenju akcije skripte s klastere utemeljen na sustavu Windows, potražite u članku [skripte razvoja akciju s HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


Skripta akciju mogu se instalirati dodatni softver koji se izvodi na Hadoop klaster ili promijenili konfiguraciju aplikacija instalirana na klaster. Akcije skripte su skripte koji se pokreću na čvorove klaster HDInsight klastere uvode i se provode kada čvorovi u klasteru dovršili konfiguraciju HDInsight. Skripta akcija se izvršava u odjeljku ovlasti računa za administratore sustava i njihovi puni pristup prava čvorove klaster. Svaki Klaster se može pružati s popisom akcije skripte želite izvršiti redoslijedom u kojem su navedeni.

> [AZURE.NOTE] Ako naiđete na sljedeću poruku o pogrešci:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> To znači da niste uključili metode preglednika.  Pogledajte [metode Pomoćnik za prilagođene skripte](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

## <a name="sample-scripts"></a>Ogledne skripte

Za stvaranje HDInsight klastere na operacijskog sustava Windows, je akcija skripte skriptu Azure PowerShell. Slijedi primjer skripte za konfiguriranje datoteka konfiguracije web-mjesta:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Skripta traje četiri parametra, naziv datoteke za konfiguraciju, svojstvo koje želite izmijeniti, a zatim vrijednost koju želite postaviti, i opis. Ako, na primjer:

    hive-site.xml hive.metastore.client.socket.timeout 90

Parametara postavite vrijednost hive.metastore.client.socket.timeout 90 u datoteci grozd site.xml.  Zadana vrijednost je 60 sekundi.

Ovaj primjer skripte moguće je pronaći i u [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight nudi nekoliko skripte da biste instalirali dodatne komponente na klastere HDInsight:

Ime | Skripta
----- | -----
**Instalacija Spark** | https://hdiconfigactions.blob.Core.Windows.NET/sparkconfigactionv03/spark-Installer-v03.ps1. Potražite u članku [Instalacija i korištenje povećati klastere HDInsight][hdinsight-install-spark].
**Instalacija R** | https://hdiconfigactions.blob.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Potražite u članku [Instalacija i korištenje R na klastere HDInsight][hdinsight-r-scripts].
**Instalacija Solr** | https://hdiconfigactions.blob.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Potražite u članku [Instalacija i korištenje klaster Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- **Instalacija Giraph** | https://hdiconfigactions.blob.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Potražite u članku [Instalacija i korištenje klaster Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

Akcija skripte može uvesti Azure, na portalu Azure PowerShell ili pomoću HDInsight .NET SDK.  Dodatne informacije potražite u članku [Prilagodba HDInsight klastere pomoću skripte akcije][hdinsight-cluster-customize].

> [AZURE.NOTE] Ogledne skripte funkcioniraju samo s HDInsight klaster verzijom 3.1 ili noviji. Dodatne informacije o verzijama klaster servisa HDInsight potražite u članku [HDInsight klaster verzije](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Metode Pomoćnik za prilagođene skripte

Skripta akcija helper metode su uslužni programi koje možete koristiti tijekom pisanja prilagođene skripte. Te su definirani u [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)i moguće ih je uvrstiti u pomoću sljedeće skripte:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Slijedi nekoliko načina preglednika koje nudi Ova skripta:

Način pomoći | Opis
-------------- | -----------
**Spremi HDIFile** | Preuzimanje datoteke iz na navedeni Uniform Resource Identifier (URI) na mjesto na lokalni disk koji je povezan s čvor Azure VM dodijeljene klaster.
**Proširivanje HDIZippedFile** | Raspakiraj zipane datoteku.
**Pozivanje HDICmdScript** | Pokrenuti skriptu iz cmd.exe.
**Pisanje HDILog** | Pisanje Izlaz iz prilagođene skripte koja se koristi za skripte akciju.
**Get-servisima** | Dobit ćete popis services pokrenut na računalu koje se skripta izvršava.
**Get-usluge** | Pod nazivom određenog servisa, kao ulaz, potražite detaljne informacije za određenu uslugu (servisa naziv, obradu ID-a, stanje, itd.) na računalu na kojem se skripta izvršava.
**Get-HDIServices** | Umetnite popis servisa HDInsight pokrenute na računalu na kojem se skripta izvršava.
**Get-HDIService** | S određenim HDInsight servisa nazivu unos, potražite detaljne informacije za određenu uslugu (servisa naziv, obradu ID-a, stanje, itd.) na računalu na kojem se skripta izvršava.
**Get-ServicesRunning** | Dobit ćete popis servise koji su pokrenuti na računalu na kojem se skripta izvršava.
**Get-ServiceRunning** | Potvrdite okvir ako određenog servisa (po nazivu) radi na računalu gdje se skripta izvršava.
**Get-HDIServicesRunning** | Umetnite popis servisa HDInsight pokrenute na računalu na kojem se skripta izvršava.
**Get-HDIServiceRunning** | Potvrdite okvir ako određenog servisa HDInsight (po nazivu) radi na računalu gdje se skripta izvršava.
**Get-HDIHadoopVersion** | Pronađite verziju Hadoop instaliran na računalu na kojem se skripta izvršava.
**Test IsHDIHeadNode** | Provjera je li na računalu gdje se skripta izvršava glavni čvor.
**Test IsActiveHDIHeadNode** | Provjera je li na računalu gdje se skripta izvršava aktivni glavni čvor.
**Test IsHDIDataNode** | Provjera je li na računalu gdje se skripta izvršava čvor podataka.
**Uređivanje HDIConfigFile** | Uređivanje konfiguracijska datoteka grozd-site.xml, temeljni site.xml, hdfs site.xml, mapred site.xml ili yarn site.xml.


## <a name="best-practices-for-script-development"></a>Najbolje prakse za razvoj skripti

Prilikom razvoja prilagođene skripte za programa HDInsight klaster, postoji nekoliko najbolje prakse treba imati na umu:

- Provjeri ima li verzija Hadoop

    Samo HDInsight verzijom 3.1 (Hadoop 2.4) i iznad podršku pomoću akcije skriptu da biste instalirali prilagođene komponente na klaster. U prilagođenoj skripti, morate koristiti metodu Pomoćnik za **Dohvaćanje HDIHadoopVersion** da biste provjerili verziju Hadoop prije nastavka izvođenjem drugih zadataka u skripti.


- Nudimo stabilan veze na resurse za skripte

    Korisnici trebali biste provjerite da ostanu sve skripte i druge artefakte koristi prilagođavanje klaster dostupnom vijek klaster te da verzijama te se datoteke ne mijenjaju tijekom trajanja. Ako je za ponovno obradu slike od čvorovi u klasteru potrebna potrebni su ove resurse. Najbolje je da biste preuzeli i arhivirati sve na računu za pohranu koji određuje korisnika. To se može biti zadani račun za pohranu ili bilo koji od dodatne račune za pohranu koji je naveden u trenutku implementacije za prilagođene klaster.
    Prilagođeni klaster uzoraka navedene u dokumentaciji, na primjer, ne možemo ste napravili lokalnu kopiju resursa u taj račun za pohranu u Spark i R: https://hdiconfigactions.blob.core.windows.net/.


- Provjerite je li Prilagodba skripte klaster idempotent

    Morate očekivati da čvorove programa klaster HDInsight će se ponovno preslikani tijekom života klaster. Skripta za prilagodbu Klaster se izvodi kad god je ponovno preslikani klaster. Ova skripta mora se dizajnirati da budu idempotent u smislu da nakon ponovnog obradu slike, skripte potrebno je provjeriti da Klaster se vraćaju iste prilagođene stanje u kojem je bila samo kada skriptu pokrenuli prvo na početku stvaranja klaster. Na primjer, ako prilagođene skripte instalirali aplikaciju na D:\AppLocation na prvom pokrenuti, a zatim na svaki sljedeći cilja, nakon ponovnog obradu slike, skripte provjerite aplikaciju postoji li na lokaciji D:\AppLocation prije nego što nastavite s drugim koraka u skripti.


- Instalacija prilagođene komponente optimalnih mjestu

    Kad ponovno preslikani čvorove klaster, pogon C:\ resursa i D:\ sistemski pogon mogu biti ponovno oblikovane, rezultira gubitka podataka i aplikacije koji ste instaliran na te pogona. To se može dogoditi i ako je čvor Azure virtualnog računala (VM) koja je dio klaster funkcionira i zamijenjen je novi čvor. Komponente možete instalirati na pogon D:\ ili na lokaciji C:\apps klaster. Su sva mjesta na disku C:\ ostavlja. Navedite mjesto gdje su aplikacije ili biblioteke da budu instalirani u skriptu za prilagodbu klaster.


- Visoke osiguraj arhitektura klaster

    HDInsight ima je aktivno pasivni arhitektura za visoke dostupnosti koje jedan glavni čvor je u način rada za aktivni (mjesto servisa HDInsight koristite) i druge glavni čvor je u stanju čekanja (u koje HDInsight services ne koristite). Čvorove prijeđite Načini rada s aktivnim i pasivni ako su prekinuta servisa HDInsight. Ako neku akciju skripte koristi se za instaliranje servisa na obje glavni čvorove za visoke dostupnosti, imajte na umu da prebacivanje mehanizam HDInsight nećete moći automatski uvoza preko tih servisa korisničkog instaliran. Tako da korisnik instaliranu usluge na HDInsight glavni čvorove koje se očekuje da će se dostupni su vlastita mehanizam prebacivanje ako je u načinu rada za aktivno pasivni ili je u načinu aktivno aktivno.

    I akcija HDInsight skripta naredbi se pokreće na oba glavni čvorove kad je uloga glave čvor određen kao vrijednost u parametru *ClusterRoleCollection* . Pa prilikom dizajniranja prilagođene skripte, provjerite je li skriptu svjesni ovaj će instalacijski program. Trebali biste naići probleme gdje iste usluge su instalirani i rada na oba glavni čvorove i oni završe natječu međusobno. Osim toga, imajte na umu da podaci bit će izgubljeni tijekom ponovno obradu slike, pa softvera instaliranog putem skripte akcija mora biti prebacuju na takav događaje. Aplikacije mora biti osmišljeno za vrlo dostupne podatke koje je raspodijeliti mnogo čvorove. Imajte na umu da koliko god 1/5 čvorove klasteru mogu biti ponovno digitalno obrađen u isto vrijeme.


- Konfiguriranje prilagođene komponente da biste koristili spremište blobova platforme Azure

    Prilagođene komponente koje instalirate na čvorove klaster možda zadanu konfiguraciju da biste koristili za pohranu Hadoop Distributed datoteka sustava (HDFS). Trebali biste promijeniti konfiguraciju namijenjenu spremište blobova platforme Azure. Na na klaster ponovno slike, dobiti oblikovani HDFS datotečnog sustava, a izgubit ćete sve podatke koji je pohranjen tamo. Spremište blobova platforme Azure umjesto omogućuje će biti zadržani podataka.

## <a name="common-usage-patterns"></a>Uobičajeni uzoraka korištenja

Ovo poglavlje sadrži upute na neke od uobičajenih uzoraka korištenja koje možete naići tijekom pisanja vlastite prilagođene skripte implementacije.

### <a name="configure-environment-variables"></a>Konfiguriranje varijable okruženja

Često u razvoju akcija skripte, će čini potrebe za postavljanje varijable okruženja. Ako, primjerice, najvjerojatnije scenarij je kada preuzeti u binarni s vanjskog web-mjesta, instalirajte ga na klaster i dodavanje mjesta kojem je instaliran na varijablu okruženja 'PATH'. Sljedeći isječak prikazuje upute za postavljanje varijable okruženja prilagođene skripte.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Izjavu o zaštiti postavlja varijablu okruženja **MDS_RUNNER_CUSTOM_CLUSTER** vrijednost "true" i postavlja opsega ova varijabla biti razini računala. Ponekad je važno varijable okruženja postavljene na odgovarajuće opseg – računala ili korisnika. Pogledajte [ovdje] [ 1] dodatne informacije o postavljanju varijable okruženja.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Pristup do mjesta na kojem su pohranjene prilagođene skripte

Skripte koje se koriste za prilagodbu klaster potrebno da biste se u zadani račun za pohranu za klaster ili u javno spremnik samo za čitanje na neki drugi račun za pohranu. Ako skriptu pristupa resursima drugdje smještenima te moraju biti javno dostupnu (najmanje javno samo za čitanje). Na primjer možda ćete pristupiti datoteci i spremite ga pomoću naredbe za SaveFile HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

U ovom primjeru osigurati da je spremnik somecontainer na računu za pohranu 'somestorageaccount' javno dostupno. Skripta u suprotnom će vratiti iznimku za "Nije pronađena" i neće uspjeti.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Prosljeđivanje parametara cmdlet za dodavanje AzureRmHDInsightScriptAction

Za prosljeđivanje više parametara cmdlet za dodavanje AzureRmHDInsightScriptAction, morate oblikovati vrijednost niza koja će sadržavati sve parametre skripte. Ako, na primjer:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

ili

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Vratiti iznimku nije uspjelo klaster implementacije

Ako želite da se točno obavještava o činjenica da skupine prilagodbe nije uspjela kako treba, važno je vratiti iznimku, a neće uspjeti stvaranja klaster. Na primjer, možda ćete morati obradi datoteke ako on postoji i obradu pogreške slučaj kojem se datoteka ne postoji. To želite li skriptu zatvara bez poteškoća i stanje klaster pravilno poznati. Sljedeći isječak daje primjera kako postići sljedeće:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

U ovom isječak ako datoteka ne postoji, ga želite dovesti do stanja pri čemu skriptu zapravo zatvara bez poteškoća nakon ispis poruka o pogrešci, a klaster je dostigne stanje izvršavanja pretpostavkom "uspješno" Završi proces prilagodbe klaster. Ako želite da se točno obavještava o činjenica da zapravo skupine prilagodbe nije uspjela očekivani zbog datoteku koja nedostaje, to je prikladnije vratiti iznimku i neće uspjeti korak za prilagodbu klaster. Da biste to postigli koristite sljedeće ogledne koda umjesto toga.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Kontrolni popis za implementaciju skripte akcija
Evo nekoliko koraka ne možemo snimljene prilikom pripreme za implementaciju te skripte:

1. Stavljanje datoteka koje sadrže prilagođene skripte na mjestu na kojemu se može pristupiti čvorovi klaster tijekom implementacije. To može biti bilo koji od zadane ili dodatne račune za pohranu koji su naveden u trenutku implementacije klaster ili javno dostupno za pohranu kontejner.
2. Dodavanje provjere u skripti da biste bili sigurni da ih koristite idempotently, tako da se skripta može izvršiti više puta na istom čvor.
3. Pomoću cmdleta Azure PowerShell **Pisanje izlaz** za ispis STDOUT, kao i STDERR. Nemojte koristiti **Pisanje Host**.
4. Korištenje mape privremene datoteke, kao što su $env: TEMP, da biste zadržali preuzetu datoteku koja se koristi skripte i zatim Očisti ih nakon skripte izvršiti.
5. Instalacija softvera prilagođene samo na D:\ ili C:\apps. Drugim mjestima na pogon C: treba koristiti kao što su rezervirana. Imajte na umu da instalacije datoteka na pogon C: izvan mapu C:\apps može uzrokovati pogreške postavljanje tijekom ponovno slike čvora.
6. U slučaju da su promijenjene postavke na razini OS ili Hadoop servisa konfiguracijske datoteke, preporučujemo vam da biste ponovno pokrenuli servisa HDInsight tako da ih obraditi sve postavke OS razine, kao što su varijable okruženja postavljanje u skripte.

## <a name="debug-custom-scripts"></a>Prilagođene skripte za ispravljanje pogrešaka

Evidencije pogrešaka za skripte spremaju zajedno s drugim Izlaz u zadani račun za pohranu koji ste naveli za klaster pri njegova stvaranja. U zapisnicima spremaju se u tablicu pod nazivom *u < \cluster-name-fragment >< \time-stamp > setuplog*. To su zbroja zapisnika koje sadrže zapise iz svih čvorove (glavni čvor i tempiranja čvorove) na kojem se skripta izvodi u klasteru.
Jednostavan način Provjerite zapisnike je pomoću alata za HDInsight za Visual Studio. Instaliranje alata, potražite u članku [Prvi koraci pri korištenju alata Visual Studio Hadoop za HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio)

**Da biste provjerili prijavite koristeći Visual Studio**

1. Otvorite Visual Studio.
2. Kliknite **Prikaz**, a zatim kliknite **Explorer poslužitelja**.
3. Desnom tipkom miša kliknite "Azure", kliknite Poveži se s **Pretplate na Microsoft Azure**i unesite vjerodajnice.
4. Proširite **prostora za pohranu**, proširite račun Azure prostora za pohranu koji se koristi kao zadani datotečni sustav, proširivanje **tablice**, a zatim dvokliknite naziv tablice.


Možete i Udaljena u čvorove klaster da biste vidjeli na oba STDOUT i STDERR za prilagođene skripte. U zapisnicima na svakom čvor odnose samo na taj čvor se i ste prijavljeni putem **C:\HDInsightLogs\DeploymentAgent.log**. Ove datoteke zapisnika snimite sve izlaze iz prilagođene skripte. Na primjer evidencije isječak za skripte akciju Spark izgleda ovako:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


U ovom zapisniku je Očisti da izvrši akciju Spark skripte na VM pod nazivom HEADNODE0 i da su tijekom izvršavanja izbačena izuzetaka.

Za slučaj da je izvođenja nestane, izlazna s opisom je također se nalaze u datoteci zapisnika. Informacije koje se nalaze u ti zapisnici mora biti korisno u ispravljanje pogrešaka skripte problemi koji se može pojaviti.


## <a name="see-also"></a>Vidi također

- [Prilagodba klastere HDInsight pomoću skripte akcije][hdinsight-cluster-customize]
- [Instaliranje i korištenje Spark na klastere HDInsight][hdinsight-install-spark]
- [Instaliranje i korištenje R na klastere HDInsight][hdinsight-r-scripts]
- [Instalacija i korištenje klaster Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- [Instalacija i korištenje klaster Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
