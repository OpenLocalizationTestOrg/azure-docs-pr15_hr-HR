<properties
    pageTitle="Što je R na HDInsight? Uvod u R Server na HDInsight (pretpregled) | Microsoft Azure"
    description="Što je poslužitelj R na HDInsight (pretpregled) te o tome kako koristiti R poslužitelj za stvaranje aplikacija za analizu velikih skupova podataka."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Pregled R Normalni prikaz na HDInsight \(pretpregled\)

Uz Microsoft Azure HDInsight Premium Microsoft R Server sada je dostupan kao mogućnost prilikom stvaranja HDInsight klastere u Azure. Tu mogućnost Nova nudi fizičari podataka, statisticians i R programere osvježavati pristup skalabilni, distributed metode analize na HDInsight.

Klastere možete s projektima i zadacima konkretnom veličinom i raskinut kada ste više nije potrebna. Budući da su sadržane Azure HDInsight, te klastere se isporučuju uz podršku 24-7 na razini tvrtke, na SLA vrijeme 99.9% aktivnosti i fleksibilnost za integraciju s druge komponente Azure zajednici.

>[AZURE.NOTE] Poslužitelj R je dostupna samo s HDInsight Premium. Trenutno HDInsight Premium dostupna je samo za klastere Hadoop i Spark. Tako, trenutno koristite R poslužitelja samo s Hadoop i Spark klastere na HDInsight. Dodatne informacije potražite u članku [koji su servis za različite razine i Hadoop komponente dostupno u sklopu HDInsight?](hdinsight-component-versioning.md).

R Normalni prikaz na HDInsight daju najnovije mogućnosti za analizu koji se temelji na R velikih skupova podataka učitavaju spremište blobova platforme Azure. Budući da je poslužitelj R utemeljena na Otvori izvor R, stvaranja aplikacije utemeljene na R možete koristiti bilo koji od Otvori izvor R paketa 8000 +, kao i postupke u ScaleR, tvrtke Microsoft velikih skupova podataka analize paket koji je uključen u R poslužitelja.

Čvor rub od Premium klastere omogućuje praktično povezati klaster, a da biste pokrenuli skripte R. S čvor na rub imate mogućnost pokretanja ScaleR na parallelized raspodijeljeno funkcije preko jezgri čvor rubni poslužitelj. Imate mogućnost da biste pokrenuli ih preko čvorove klaster pomoću ScaleR, Hadoop karte smanjiti ili Spark izračunati konteksta.

Modeli ili predviđanja koje su rezultat analize možete preuzeti za koristite lokalni. Ih možete također biti operationalized negdje drugdje u Azure, kao što su putem [Azure strojnog učenja Studio](http://studio.azureml.net) [web-servisa](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Početak rada s R na HDInsight

Da biste obuhvatili R poslužitelj programa HDInsight klaster, morate stvoriti Hadoop ili Spark klaster s mogućnošću Premium kada stvarate klaster pomoću portala za Azure. Obje vrste klaster koristiti istu konfiguraciju koja obuhvaća R poslužitelja na čvorove podataka klaster, a na rub čvor kao odredišna zone R poslužiteljska analitičkih. Pročitajte članak [Početak rada s poslužiteljem R na HDInsight](hdinsight-hadoop-r-server-get-started.md) za detaljne walk-through o stvaranju klaster.

## <a name="learn-about-data-storage-options"></a>Dodatne informacije o mogućnosti pohrane podataka

Zadani prostor za pohranu za klastere HDInsight je spremište blobova platforme s datotečnim sustavom HDFS mapirati u spremniku blob. Time se osigurava da bilo kakve podatke je prenijeti klaster za pohranu ili zapisuju klaster prostora za pohranu tijekom analize postala je stalni. Možete koristiti uslužni [AzCopy](../storage/storage-use-azcopy.md) da biste kopirali podatke i s blob-om.

Uz blobova imate mogućnost [Lake pohrana podataka za Azure](https://azure.microsoft.com/services/data-lake-store/) pomoću svoj klaster. Ako koristite Lake podataka, zatim koristite blobova i Lake podataka za pohranu HDFS.

[Azure datoteke](../storage/storage-how-to-use-files-linux.md) može poslužiti kao mogućnost za pohranu za korištenje na rub čvor. Azure datoteka omogućuje postavljanje zajedničke datoteke koja je stvorena u Azure prostora za pohranu u datotečni sustav Linux. Dodatne informacije o mogućnosti pohrane podataka za poslužitelj R na klasteru servisa HDInsight potražite u članku [Mogućnosti pohrane za poslužitelj R na klastere HDInsight](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Poslužitelj za pristup R na klaster

Nakon stvaranja klaster s poslužiteljem R, možete se povezati s konzole R na rub čvora klaster kroz SSH/PuTTY. Možete povezati putem preglednika ako odaberete da biste instalirali neobavezno IDE poslužitelja RStudio na rub čvor. Dodatne informacije o instaliranju RStudio Server potražite u članku [Instaliranje RStudio poslužitelju za klastere HDInsight](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Razvoj i pokreću skripte R

R skripte stvaranje i pokretanje možete koristiti bilo koji 8000 + Otvori izvor R pakete osim parallelized i raspodijeljeno postupke u biblioteci ScaleR. Općenito govoreći, skriptu koja se izvršava na poslužitelju R čvor rub pokreće unutar tumačenja R taj čvor. Iznimka je ti koraci koje mogu pozivati na ScaleR funkcionirati s računalnim kontekstu to su postavljena na Hadoop karte smanjite (RxHadoopMR) ili Spark (RxSpark).

U tim slučajevima funkciju pokreće raspodijeljeno način preko tih podataka (zadatak) čvorove klaster koje su vezane uz referencirani podataka. Dodatne informacije o kontekstu mogućnosti različite računalnim potražite u članku [izračunati kontekst mogućnosti za poslužitelj R HDInsight Premium](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Operationalize modela

Po dovršetku vaše Modeliranje podataka možete operationalize model da biste stvarali predviđanja za nove podatke u Azure i lokalnih. Ovaj postupak zove bilježenje rezultata. Evo nekoliko primjera.

### <a name="score-in-hdinsight"></a>Rezultat u HDInsight

Da biste rezultat u HDInsight, upišite R (funkcija) koja se poziva na model da biste stvarali predviđanja za novu podatkovnu datoteku koju ste učitati na račun servisa za pohranu. Povratak na račun za pohranu spremiti na predviđanja. Možete pokrenuti na redovno na zahtjev na rub čvor svoj klaster ili pomoću zakazani zadatak.  

### <a name="score-in-azure-machine-learning"></a>Rezultat u Azure strojnog učenja

Da biste rezultat pomoću programa web-servisa Azure strojnog učenja, pomoću značajke pakiranja Azure strojnog učenja R Otvori izvor naziva [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) objava modela kao servis Azure web. Pogodnost, taj paket je instalirana na rub čvor. Nakon toga pomoću funkcije u strojnog učenja da biste stvorili korisničko sučelje za web-servisa, a poziva web-servisa, kao što je potrebno za bilježenje rezultata.

Ako odaberete tu mogućnost, morat ćete sve objekte modela ScaleR pretvorite objekti ekvivalentan Otvori izvor modela za rad s web-servisa. To možete učiniti pomoću funkcije prinude ScaleR, kao što su `as.randomForest()` utemeljen na ensemble modela.


### <a name="score-on-premises"></a>Rezultat lokalnog

U rezultatu lokalnog nakon stvaranja modelu, možete serijalizirati modela u R, preuzmite ga, poništite serijalizirati i koristiti ga za bilježenje rezultata nove podatke. Nove podatke možete rezultat pomoću na način opisan u prethodnom [Bodovanje u HDInsight](#scoring-in-hdinsight) ili pomoću [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Održavanje klaster

### <a name="install-and-maintain-r-packages"></a>Instalacija i održavanje R paketa

Većina paketa R koju koristite bit će vam potreban na rub čvor Budući da se većina skripte R postoji će se pokrenuti. Da biste instalirali dodatne R paketa na rub čvor, možete koristiti na uobičajeni `install.packages()` metoda u R.

U većini slučajeva, ne morate instalirati dodatne R paketa na čvorove podataka ako upravo koristite postupke iz biblioteke ScaleR preko klaster. Međutim, bilo bi dobro dodatne paketa za podršku korištenje **rxExec** ili **RxDataStep** izvođenja na čvorove podataka.

U tim slučajevima dodatne paketa mora biti navedena kroz korištenje skripte akcije nakon stvaranja klaster. Dodatne informacije potražite u članku [Stvaranje programa HDInsight klaster s poslužiteljem R](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Promjena postavki za smanjivanje karte Hadoop memorije

Klaster moguće je izmijeniti da biste promijenili količinu memorije koja je dostupna R poslužitelj kada je pokrenuta posao smanjiti karta. Da biste izmijenili klaster, koristite Apache Ambari korisničkog Sučelja koji je dostupan putem Azure plohu portala za svoj klaster. Upute o tome kako pristupiti Ambari korisničko Sučelje za svoj klaster, potražite u članku [Upravljanje HDInsight klastere pomoću korisničkog Sučelja Web Ambari](hdinsight-hadoop-manage-ambari.md).

Moguće je da biste promijenili količinu memorije koja je dostupna na poslužitelj R pomoću Hadoop parametri u poziv **RxHadoopMR** na sljedeći način:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Promjena veličine svoj klaster

Postojeće klaster može biti skalirana prema gore ili dolje putem portala sustava. Promjena veličine, možete dobiti dodatne kapacitet koji je možda će biti potrebno za veće obrada zadatke ili koje mogu mijenjati veličinu natrag klaster kada je neaktivno. Upute za promjenu veličine na klaster, potražite u članku [Upravljanje HDInsight klastere](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Održavanje sustava

Održavanje se izvodi na pozadini Linux VMs programa klasteru HDInsight tijekom off-hours da biste primijenili zakrpa OS i ostala ažuriranja. Obično održavanja obavlja pri 3:30 AM (na temelju lokalno vrijeme za na VM) svaki ponedjeljak i četvrtak. Ažuriranja se izvode na način da ne utječe više od jedne kvartal klaster odjednom.  

Budući da su glavni čvorovi suvišne i su utjecati čvorove sve podatke, sve zadatke koji su pokrenuti tijekom određenog razdoblja usporiti. Oni i dalje pokretati do završetka, no. Prilagođeni softver ni lokalne podatke koje imate zadržava se preko te održavanje događaja osim ako Katastrofalna pogreška pojavljuje koje je potrebno ponovno stvaranje klaster.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Dodatne informacije o mogućnostima IDE za poslužitelj R na programa HDInsight klaster

Čvor rub Linux od programa HDInsight Premium klaster je odredišna zone za analizu koji se temelji na R. Nakon povezivanja s klaster, možete pokrenuti sučelja konzole R poslužitelj tako da upišete **R** u naredbenom retku Linux. Korištenje sučelja konzole je poboljšana Ako naiđete uređivaču teksta za razvoj skripte R u neki drugi prozor i izrezivanje te zalijepite sekcije skripte konzole R prema potrebi.

Više sofisticirane alata za razvoj skriptu R je IDE R-poštu za korištenje na radnoj površini, kao što je Microsoftov nedavno objaviti [R alate za Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS). Ovo je obitelji alata za stolna računala i poslužitelja sa [RStudio](https://www.rstudio.com/products/rstudio-server/). Možete koristiti i Walware korisnika koji se temelji na Eclipse [StatET](http://www.walware.de/goto/statet).

Da biste instalirali na IDE na rub čvor Linux sam je i mogućnost.  Popularne mogućnosti je [RStudio poslužitelj](https://www.rstudio.com/products/rstudio-server/), koji omogućuje IDE za utemeljenima na pregledniku koriste udaljenim klijentima. Instalacije poslužitelja RStudio na rub čvora programa HDInsight Premium klaster nudi sve funkcije programa IDE za razvoj i izvršavanje skripti R s poslužiteljem R na klaster. Možda ćete znatno produktivnosti od R konzolu.  Ako želite koristiti RStudio Server potražite u članku [Instaliranje RStudio poslužitelju za klastere HDInsight](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Dodatne informacije o cijenama

Naknade za koje su vezane uz programa klaster HDInsight Premium s poslužiteljem R strukturirane su isto tako da biste naknade za standardne klastere HDInsight. Mogu se temelje na veličine podlozi VMs preko ime, podataka i ruba čvorove, s dodavanja core-satnom uplift Premium. Dodatne informacije o HDInsight Premium cijene, uključujući cijene tijekom javno pretpregled i dostupnost 30 dana besplatnu probnu verziju, potražite u članku [HDInsight cijene](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Daljnji koraci

Slijedite veze u nastavku potražite dodatne informacije o načinu korištenja R poslužitelja s klastere HDInsight.

- [Početak rada s poslužiteljem R na HDInsight](hdinsight-hadoop-r-server-get-started.md)

- [Dodavanje poslužitelja RStudio HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)

- [Izračunavanje kontekst mogućnosti R poslužitelja na HDInsight (pretpregled)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure mogućnosti prostora za pohranu za poslužitelj R na HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
