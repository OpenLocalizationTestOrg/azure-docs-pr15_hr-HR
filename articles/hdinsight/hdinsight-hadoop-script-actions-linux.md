<properties
    pageTitle="Skripte razvoja akciju s operacijskim sustavom Linux HDInsight | Microsoft Azure"
    description="Kako prilagoditi sustavom Linux HDInsight klaster skripte akciju. Akcije skripte su način za prilagodbu Azure HDInsight klastere određivanje postavki konfiguriranje klaster ili instalacije dodatna servisa, Alati za ili neki drugi softver na klaster. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Skripta razvoja akciju s HDInsight

Akcije skripte su način za prilagodbu Azure HDInsight klastere određivanje postavki konfiguriranje klaster ili instalacije dodatna servisa, Alati za ili neki drugi softver na klaster. Akcije skripte možete koristiti tijekom stvaranja klaster ili na tekući klaster.

> [AZURE.NOTE] Informacije u ovom dokumentu je za klastere sustavom Linux HDInsight. Informacije o korištenju akcije skripte s klastere utemeljen na sustavu Windows, potražite u članku [skripte razvoja akciju s HDInsight (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Što su akcije skripte?

Akcije skripte su tulumu skripte koje Azure pokreće čvorove klaster promjene konfiguracije stvaranjem ili instalacija softvera. Skripta akcija se izvršava kao korijen i njihovi puni pristup prava čvorove klaster.

Akcije skripte se mogu primijeniti do sljedećih načina:

| Tom se mogućnošću poslužite da biste primijenili skriptu... | Tijekom skupine stvaranja... | Na tekući klaster... |
| ----- |:-----:|:-----:|
| Portal za Azure | ✓ | ✓ |
| Azure PowerShell | ✓ | ✓ |
| Azure EŽA | &nbsp; | ✓ |
| HDInsight .NET SDK | ✓ | ✓ |
| Predloška Azure Voditelj resursa | ✓ | &nbsp; |

Dodatne informacije o korištenju ove metode da biste primijenili akcije skripte potražite u članku [Prilagodba HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Najbolje prakse za razvoj skripti

Prilikom razvoja prilagođene skripte za programa HDInsight klaster, postoji nekoliko najbolje prakse treba imati na umu:

- [Cilj Hadoop verzija](#bPS1)
- [Cilj verzija OS-a](#bps10)
- [Nudimo stabilan veze na resurse za skripte](#bPS2)
- [Korištenje unaprijed kompilirane resursi](#bPS4)
- [Provjerite je li Prilagodba skripte klaster idempotent](#bPS3)
- [Visoke osiguraj arhitektura klaster](#bPS5)
- [Konfiguriranje prilagođene komponente da biste koristili spremište blobova platforme Azure](#bPS6)
- [Pisati na STDOUT i STDERR](#bPS7)
- [Spremanje datoteka u obliku ASCII s LF crte kod](#bps8)
- [Korištenje pokušaj logike za oporavak iz tranzitne pogreške](#bps9)

> [AZURE.IMPORTANT] Akcije skripte morate dovršiti nekoliko 60 minuta ili neće biti vremenskog ograničenja. Tijekom čvor dodjeljivanja, skripta pokrenut će se čine istovremeno s drugim procesa instalacije i konfiguracije. Konkurencije za resurse kao što su procesora vremena ili mreže propusnosti može uzrokovati skripta za dulje da biste dovršili nego u razvojno okruženje.

### <a name="bPS1"></a>Cilj Hadoop verzija

Različite verzije sustava HDInsight koriste različite verzije sustava Hadoop services i instalirane komponente. Ako skriptu očekuje određenu verziju servisa ili komponente, samo pomoću skripte s verzijom HDInsight koji sadrži potrebne komponente. Možete pronaći informacije o komponenti verzije uključene HDInsight pomoću [HDInsight komponente verzijama](hdinsight-component-versioning.md) dokumenta.

###<a name="bps10"></a>Ciljani verzija OS-a

Sustavom Linux HDInsight temelji se na raspodjele Ubuntu Linux. Različite verzije sustava HDInsight je za različite verzije sustava Ubuntu koji mogu utjecati ponašanje skriptu. Na primjer, HDInsight 3.4 i noviji temelje se na Ubuntu verzije koje koriste Upstart. Verzije 3.5 temelji se na 16.04 Ubuntu koji koristi Systemd. Systemd i Upstart drugačije naredbe oslanjate tako skriptu moraju biti napisani za rad s oba.

Drugi važne razlika između servisa HDInsight 3.4 i 3.5 je koji `JAVA_HOME` sada upućuje na Java 8.

Verzija OS-a možete provjeriti pomoću `lsb_release`. Sljedeće koda iz instalacije skripte nijanse pokazuje kako odrediti je li pokrenut skripte na Ubuntu 14 ili 16:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

Možete pronaći cijelog skriptu koja sadrži te isječci pri https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Verziju Ubuntu koji koriste HDInsight potražite u članku [HDInsight komponente verziju](hdinsight-component-versioning.md) dokumenta.

Da biste razumjeli razlike između Systemd i Upstart, potražite u članku [Systemd Upstart korisnicima](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Nudimo stabilan veze na resurse za skripte

Provjerite je li skripte i resursima koristi skriptu ostaju dostupnom vijek klaster, a da verzijama te se datoteke ne mijenjaju tijekom trajanja. Ove resurse potrebni su Ako novi čvorovi dodaju klaster tijekom promjene veličine operacije.

Najbolje je da biste preuzeli i arhivirati sve u račun Azure prostor za pohranu u pretplatu.

> [AZURE.IMPORTANT] Račun za pohranu koji se koristi mora biti zadani prostor za pohranu račun za klaster ili spremnik javno, samo za čitanje na neki drugi račun za pohranu.

Na primjer, uzoraka pruža Microsoft spremaju se u računu za pohranu [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) , što je javno, samo za čitanje spremnik održava tima HDInsight.

### <a name="bPS4"></a>Korištenje unaprijed kompilirane resursi

Da biste smanjili vrijeme potrebno za pokretanje skriptu, nemojte operacije koje Kompiliranje resursa iz izvornog koda. Umjesto toga, unaprijed Kompiliranje resurse i pohranjivati binarni verzija u spremište blobova platforme Azure tako da ga možete brzo preuzet klaster iz skriptu.

### <a name="bPS3"></a>Provjerite je li Prilagodba skripte klaster idempotent

Skripte moraju se dizajnirati da budu idempotent u smislu koji ako je skripta pokrenuli više puta, trebali biste provjerite je li Klaster se vraćaju u istom stanju svaki put kada ga je pokrenuo.

Ako, na primjer, ako prilagođene skripte instalira aplikacije na /usr/local/bin na prvom pokrenuti, a zatim pri svakom pokretanju sljedeće skripte provjerite hoće li se aplikacija već postoji lokaciji /usr/local/bin prije nego što nastavite s drugim koraka u skripti.

### <a name="bPS5"></a>Visoke osiguraj arhitektura klaster

Sustavom Linux klastere HDInsight nudi dva glavni čvorove koji su aktivni unutar klaster i pokrenuli skripte radnje za oba čvorove. Ako instalirate komponente očekujete samo jedan glavni čvor, morate dizajnirati skriptu koja samo instalirati komponentu na jedan od dva glavni čvorovi u klasteru.

> [AZURE.IMPORTANT] Zadane servise koji se instalira u sklopu HDInsight dizajnirani uvoza putem između dva glavni čvorove po potrebi, no ta je funkcija nije proširena prilagođene komponente instalira putem skripte ctions. Ako vam je potrebna instalirane putem skripte akcija će biti vrlo dostupni komponente, morate provesti vlastite prebacivanje mehanizam koji koristi dvije dostupna glavni čvorove.

### <a name="bPS6"></a>Konfiguriranje prilagođene komponente da biste koristili spremište blobova platforme Azure

Komponente koje instalirate na klaster možda zadanu konfiguraciju koji koristi pohranu Hadoop Distributed datoteka sustava (HDFS). HDInsight koristi spremište blobova na Azure (WASB) kao zadani prostor za pohranu. To omogućuje u HDFS kompatibilni datotečni sustav koji nastavi podataka, čak i ako se briše klaster. Trebali biste konfigurirati komponente instalacije za značajku WASB HDFS.

Na primjer, sljedeće kopira giraph examples.jar datoteku s lokalnom sustavu datoteka WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Pisati na STDOUT i STDERR

Informacije zapisuju STDOUT i STDERR tijekom izvođenja skripte prijavljen je, a moguće je prikazati putem weba Ambari korisničkog Sučelja.

> [AZURE.NOTE] Ambari samo će biti dostupna ako klaster uspješno stvorili. Ako koristite akciju skripte tijekom stvaranja klaster, a ne uspije stvaranja, u odjeljku otklanjanje poteškoća [Prilagodba HDInsight klaster pomoću akcije skripte](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) za drugi načini pristupanja zabilježeni podaci.

Većina uslužni programi i instalaciju paketa će već pisati na STDOUT i STDERR, no preporučujemo vam da biste dodali dodatne zapisivanje. Da biste poslali teksta pomoću STDOUT `echo`. Ako, na primjer:

        echo "Getting ready to install Foo"

Prema zadanim postavkama `echo` poslat će niz STDOUT. Da biste izravno na STDERR, dodajte `>&2` prije `echo`. Ako, na primjer:

        >&2 echo "An error occurred installing Foo"

Podaci koji se šalju STDOUT (1, što je zadani tako da nije ovdje naveden) to preusmjerava STDERR (2). Dodatne informacije o preusmjeravanju IO potražite u članku [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Dodatne informacije o prikazu informacije zapisuje akcije skripte potražite u članku [Prilagodba HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>Spremanje datoteka u obliku ASCII s LF crte kod

S crtama prekinuta LF bi se trebale nalaziti tulumu skripte ASCII obliku. Ako se datoteke spremaju kao UTF-8, koji mogu sadržavati oznaku redoslijed bajt na početku datoteke ili s crte kod od riječ o CRLF, koja je zajednička za Windows uređivači, skripta se neće uspjeti s pogreškama sličnu ovoj:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Korištenje pokušaj logike za oporavak iz tranzitne pogreške

Prilikom preuzimanja datoteke, instalaciju paketa pomoću Zemaljska get ili drugih akcija koje prijenos podataka putem Interneta, akcije možda neće uspjeti zbog tranzitne umrežavanje pogrešaka. Ako, na primjer, udaljene resursa su komunikaciju s možda u tijeku neuspjeh putem sigurnosne kopije čvor.

Da biste unijeli skriptu prebacuju tranzitne pogreške, možete implementirati logika pokušajte ponovno. Slijedi primjer funkcije koja će se izvoditi bilo kojoj naredbi proslijeđena u nju i (Ako je naredba ne uspije,) ponovite do tri puta. To će Pričekajte dvije sekunde između svakog pokušajte ponovno.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Slijede primjeri korištenja ove funkcije.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Metode Pomoćnik za prilagođene skripte

Skripta akcija helper metode su uslužni programi koje možete koristiti tijekom pisanja prilagođene skripte. Te su definirani u [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)i moguće ih je uvrstiti u pomoću sljedeće skripte:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

To čini sljedeće helpers dostupan za korištenje u skriptu:

| Korištenje preglednika | Opis |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | Preuzimanje datoteke iz izvora URL-a na navedeni put datoteke. Prema zadanim postavkama, on neće prebrisati postojeću datoteku. |
| `untar_file TARFILE DESTDIR` | Izdvaja datoteke tar (pomoću `-xf`,) u odredišni imenik. |
| `test_is_headnode` | Ako je na glavni klaster čvor, vraća 1; u suprotnom, 0. |
| `test_is_datanode` | Ako je trenutni čvor čvor podataka (tempiranja), vraća se 1; u suprotnom, 0. |
| `test_is_first_datanode` | Ako je trenutni čvor prvi čvor podataka (tempiranja) (pod nazivom workernode0), vraća se 1; u suprotnom, 0. |
| `get_headnodes` | Vraća na potpuno kvalificirani naziv domene od na headnodes u klasteru. Nazivi su zarezima. Pogreške, vraća se prazan niz. |
| `get_primary_headnode` | Dohvaća na potpuno kvalificirani naziv domene od primarni headnode. Pogreške, vraća se prazan niz. |
| `get_secondary_headnode` | Dohvaća na potpuno kvalificirani naziv domene od sekundarne headnode. Pogreške, vraća se prazan niz. |
| `get_primary_headnode_number` | Dobiva numeričku sufiks primarni headnode. Pogreške, vraća se prazan niz. |
| `get_secondary_headnode_number` | Dobiva numeričku sufiks sekundarne headnode. Pogreške, vraća se prazan niz. |

## <a name="commonusage"></a>Uobičajeni uzoraka korištenja

Ovo poglavlje sadrži upute na neke od uobičajenih uzoraka korištenja koje možete naići tijekom pisanja vlastite prilagođene skripte implementacije.

### <a name="passing-parameters-to-a-script"></a>Prosljeđivanje parametara skripte

U nekim slučajevima skriptu možda ćete morati parametara. Na primjer, morat ćete administratorsku lozinku za klaster dohvaćanje podataka iz Ambari REST API-JA.

Parametara u skriptu nazivaju _Pozicijske parametre_i dodjeljuju `$1` za prvi parametar `$2` za drugi i tako da na. `$0`sadrži naziv skripte sam.

Vrijednosti proslijeđen skriptu kao parametar mora biti zatvoren u jednostruke navodnike (') da bi proslijeđeni vrijednost se smatra je literal i poseban tretman nije dodijeljen uključene znakove kao što su "!".

### <a name="setting-environment-variables"></a>Postavljanje varijable okruženja

Postavljanje varijabla okruženja je izvesti sljedeće:

    VARIABLENAME=value

Pri čemu je VARIABLENAME naziv varijable. Da biste pristupili varijabla nakon toga, koristite `$VARIABLENAME`. Na primjer, se dodjeljuje vrijednost nudi Pozicijske parametra kao varijabla okruženja pod nazivom lozinku, koristite sljedeće:

    PASSWORD=$1

Zatim pomoću naknadni pristup informacijama `$PASSWORD`.

Varijable okruženja postavljene unutar skriptu postoje samo unutar opsega skriptu. U nekim slučajevima, morate dodati varijable širine okruženja sustava koji će i dalje pojavljuje kada je završio skriptu. Obično to je da bi korisnici povezati klaster putem SSH mogli koristiti instalirao skriptu komponente. Koje možete izvršiti dodavanjem varijablu okruženja za `/etc/environment`. Na primjer, sljedeće dodaje __HADOOP\_KONF\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Pristup do mjesta na kojem su pohranjene prilagođene skripte

Skripte koje se koriste za prilagodbu klaster potrebne za bilo koju biti u zadani prostor za pohranu račun za klaster ili, ako na drugi račun za pohranu, u javnim spremnik samo za čitanje. Ako skriptu pristupa resursima drugdje smještenima te također moraju biti javno dostupnu (najmanje javno samo za čitanje). Na primjer možda želite preuzeti datoteku pomoću klaster `download_file`.

Spremanje datoteke u račun za Azure prostora za pohranu pristupačnih po klaster (kao što je zadani račun za pohranu), dat će brzog pristupa, kao što je ovaj prostor za pohranu unutar Azure mreže.

### <a name="checking-the-operating-system-version"></a>Provjera verzija operacijskog sustava

Različite verzije sustava HDInsight oslanjate određenih verzija Ubuntu. Možda postoje razlike između verzija OS-a koji morate prijaviti za skripte. Na primjer, možda morate instalirati binarni koji je vezan uz verziju Ubuntu.

Da biste provjerili verzija OS-a, koristiti `lsb_release`. Ako, na primjer, sljedeće pokazuje kako pozvati različite tar datoteke ovisno o verziji OS:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Kontrolni popis za implementaciju skripte akcija

Evo nekoliko koraka ne možemo snimljene prilikom pripreme za implementaciju te skripte:

- Stavljanje datoteka koje sadrže prilagođene skripte na mjestu na kojemu se može pristupiti čvorovi klaster tijekom implementacije. To može biti bilo koji od zadane ili dodatne račune za pohranu koji su naveden u trenutku implementacije klaster ili javno dostupno za pohranu kontejner.

- Dodavanje provjere u skripti da biste bili sigurni da se izvoditi impotently, tako da se skripta može izvršiti više puta na istom čvor.

- Da biste zadržali preuzete datoteke koje koristi za skripte i zatim Očisti ih nakon izvršiti skripte pomoću /tmp za direktorija privremene datoteke.

- U slučaju da su promijenjene postavke na razini OS ili Hadoop servisa konfiguracijske datoteke, preporučujemo vam da biste ponovno pokrenuli servisa HDInsight tako da ih obraditi sve postavke OS razine, kao što su varijable okruženja postavljanje u skripte.

## <a name="runScriptAction"></a>Kako pokrenuti skriptu akcija

Akcije skripte možete koristiti da biste prilagodili klastere HDInsight pomoću Azure portalu Azure PowerShell predložaka Azure resursa Manager (ARM) ili HDInsight .NET SDK. Upute potražite u članku [kako koristiti skriptu akciju](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Prilagođene skripte uzoraka

Microsoft pruža ogledne skripte da biste instalirali komponente na programa klaster HDInsight. Ogledne skripte i upute o tome kako ih koristiti dostupne su na veze u nastavku:

- [Instaliranje i korištenje nijanse na klastere HDInsight](hdinsight-hadoop-hue-linux.md)
- [Instaliranje i korištenje R na klastere HDInsight Hadoop](hdinsight-hadoop-r-scripts-linux.md)
- [Instaliranje i korištenje Solr na klastere HDInsight](hdinsight-hadoop-solr-install-linux.md)
- [Instaliranje i korištenje Giraph na klastere HDInsight](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] Dokumenti povezana iznad su specifične za klastere sustavom Linux HDInsight. Skripte koje funkcioniraju sa sustavom Windows HDInsight potražite [skriptu razvoja akciju s HDInsight (Windows)](hdinsight-hadoop-script-actions.md) ili pomoću veze dostupan pri vrhu svake članka.

##<a name="troubleshooting"></a>Otklanjanje poteškoća

Slijede pogreške koji se mogu pojaviti prilikom korištenja skripte razvili ste:

__Error__: `$'\r': command not found`. Ponekad slijedi `syntax error: unexpected end of file`.

_Uzrok_: ta se pogreška se događa kada se reci u skripti završavati riječ o CRLF. UNIX sustavima očekivati samo LF kao završetak crte.

Taj se problem najčešće pojavljuje kada je skripta autor u okruženju sustava Windows kao da je riječ o CRLF uobičajenih redak završni za mnoge uređivači teksta u sustavu Windows.

_Rješenje_: Ako je mogućnost u uređivaču teksta, odaberite oblik Unix ili LF za završetak crte. Da biste promijenili na riječ o CRLF programa LF možda koristi i sljedeće naredbe u sustavu Unix:

> [AZURE.NOTE] Sljedeće naredbe su otprilike ekvivalentan po tome trebali biste promijeniti riječ o CRLF crte kod da biste LF. Odaberite temelji na uslužni programi dostupan u sustavu.

| Naredba | Bilješke |
| ------- | ----- |
| `unix2dos -b INFILE` | Izvorna datoteka sigurnosno će se s na. Proširenje BAK |
| `tr -d '\r' < INFILE > OUTFILE` | OUTFILE će sadržavati verzije s samo LF završetaka |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | To će izmjena datoteka izravno bez stvaranja nove datoteke |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | OUTFILE će sadržavati verzije s samo LF završetaka.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_Uzrok_: ta se pogreška pojavljuje kada skriptu koja je spremljena kao UTF-8 s bajt redoslijed oznaka (Sastavnice).

_Razlučivost_: spremite datoteku kao ASCII ili kao UTF-8 bez Sastavnice. Da biste stvorili novu datoteku bez Sastavnicu možda koristi i sljedeću naredbu Linux ili Unix sustavu:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Za naredbu iznad zamijenite __INFILE__ datoteku koja sadrži Sastavnicu. __OUTFILE__ mora biti novi naziv datoteke koje će sadržavati skripte bez Sastavnicu.

## <a name="seeAlso"></a>Daljnji koraci

* Saznajte kako [prilagoditi HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster-linux.md)

* [Referenca HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx) koristiti da biste saznali više o stvaranju .NET aplikacijama upravljanje HDInsight

* Da biste saznali kako koristiti OSTALE za izvođenje akcija upravljanja na klastere HDInsight pomoću [HDInsight REST API -JA](https://msdn.microsoft.com/library/azure/mt622197.aspx) .
