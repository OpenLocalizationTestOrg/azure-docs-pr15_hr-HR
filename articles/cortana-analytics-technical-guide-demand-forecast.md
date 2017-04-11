<properties
    pageTitle="Potražnje predviđanje u energiju Tehnički vodič | Microsoft Azure"
    description="Tehnički vodič s predloškom rješenja s Microsoft Cortana obavještavanje za zahtjev predviđanje u energiju."
    services="cortana-analytics"
    documentationCenter=""
    authors="yijichen"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="inqiu;yijichen;ilanr9"/>

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Tehnički vodič s predloškom Cortana obavještavanje rješenja za zahtjev predviđanje u energiju

## <a name="overview"></a>**Pregled**

Predlošci rješenja su osmišljeni za ubrzano postupka izgradnje E2E pokazni videozapis pri vrhu Cortana obavještavanje paket. Predložak distribuiranih će Dodjela pretplate s komponentom potrebne obavještavanje Cortana i stvaranje odnosa između. Također seeds kanal podataka s oglednim podacima početak stvorene iz aplikacije za simulacije podacima. Preuzimanje simulator podatkovne veze i instalirati ga na lokalno računalo, pogledajte u datoteci readme.txt pogledajte upute o koristite simulator. Podaci koje su stvorene iz simulator će hydrate podataka kanali i početak generiranje strojnog učenja predviđanje koje se mogu vizualizirati pa na nadzornoj ploči za Power BI.

Predložak rješenje možete pronaći [ovdje](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1) 

Postupak implementacije vodit će vas kroz nekoliko koraka možete postaviti vjerodajnice za rješenja. Provjerite je li snimanje te vjerodajnice, kao što su naziv rješenja, korisničko ime i lozinku unesete tijekom uvođenja.

Cilj ovog dokumenta je objašnjavaju arhitektura reference i razne komponente dodjeli u vašoj pretplati kao dio ovog predloška rješenja. Dokument i govori o da biste zamijenili uzorak podataka s stvarnih podataka sami moći vidjeti uvida/predviđanja od vas osvojili podataka. Uz to, u dokumentu talks o dijelove rješenje predložak koji morate promijeniti ako želite da biste prilagodili rješenja s vlastitim podacima. Na kraju se daju upute za stvaranje nadzorne ploče za Power BI za ovaj predložak rješenja.

## <a name="big-picture"></a>**Širu sliku**

![](media\cortana-analytics-technical-guide-demand-forecast\ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Arhitektura objašnjenje
Kada je rješenje implementiran, različite servise za Azure unutar Cortana analize paket aktiviraju (*odnosno* središte događaj analize strujanje HDInsight, tvorničke podataka, strojnog učenja, *itd.*). Arhitektura dijagrama gore prikazuje, na visoku razinu kako predviđanje zahtjev za predložak rješenje energiju konstruirana iz završetka do kraja. Moći da biste istražili te servise klikom na njima na dijagramu predložak rješenja stvorena pomoću implementacije rješenja. U sljedećim odjeljcima opisuju svakom dijelu.

## <a name="data-source-and-ingestion"></a>**Izvor podataka i Ingestion**

### <a name="synthetic-data-source"></a>Izvor podataka stilova sintetičkih

Za ovaj predložak izvor podataka koristi se generira iz aplikaciji za stolna računala koja će se preuzeti i pokrenuti lokalno nakon uspješne implementacije. Vidjet ćete upute da biste preuzeli i instalirali ovu aplikaciju na traka svojstava kad odaberete prvu čvor naziva energiju predviđanje podataka Simulator na dijagramu predložak rješenja. Ovu aplikaciju sažetke sadržaja servisa [Azure događaj koncentrator](#azure-event-hub) točke podataka ili događaja koji će se koristiti u ostatku tijek rješenja.

Aplikacije za generiranje događaja će popuniti središtu događaj Azure samo dok ga se izvršava na vašem računalu.

### <a name="azure-event-hub"></a>Koncentrator Azure događaja

[Koncentrator za događaj Azure](https://azure.microsoft.com/services/event-hubs/) servis je primatelj unos nudi stilova sintetičkih izvora podataka na prethodno opisan.

## <a name="data-preparation-and-analysis"></a>**Priprema podataka i analize**


### <a name="azure-stream-analytics"></a>Azure strujanje Analytics

Servisa [Azure strujanje analize](https://azure.microsoft.com/services/stream-analytics/) koristi se za omogućuje blizu u stvarnom vremenu analize na unos toka iz servisa [Azure koncentrator za događaj](#azure-event-hub) i objavljivanje rezultate nadzorne ploče komponente [Power BI](https://powerbi.microsoft.com) kao i arhiviranje svih događaja neobrađenog dolazne servisa [Azure prostora za pohranu](https://azure.microsoft.com/services/storage/) za kasniju obradu servis [Tvorničke Azure podataka](https://azure.microsoft.com/documentation/services/data-factory/) .

### <a name="hd-insights-custom-aggregation"></a>HD uvida prilagođena zbrajanja

Servisa Azure HD uvid služi da pokreću skripte za [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (orchestrated tvorničke podataka za Azure) za navođenje zbrajanja na neobrađenog događaje koje su arhiviraju pomoću servisa Azure strujanje analize.

### <a name="azure-machine-learning"></a>Azure strojnog učenja

Servis za [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) koristi (orchestrated tvorničke podataka za Azure) da bi bili predviđanja buduće potrošnju energije određenog područja dali unosa primili.

## <a name="data-publishing"></a>**Objavljivanje podataka**


### <a name="azure-sql-database-service"></a>Servis baze podataka Azure SQL

Servis [Baze podataka SQL Azure](https://azure.microsoft.com/services/sql-database/) služi za pohranu (upravlja tvorničke Azure podataka) predviđanja primio servisa Azure strojnog učenja koji će se potrošiti na nadzornoj ploči [Power BI](https://powerbi.microsoft.com) .

## <a name="data-consumption"></a>**Potrošnje podataka**

### <a name="power-bi"></a>Power BI

Servis [Power BI](https://powerbi.microsoft.com) koristi se za prikaz nadzorne ploče koja sadrži zbrajanja pod uvjetom da po [Azure strujanje analize](https://azure.microsoft.com/services/stream-analytics/) servisa, kao i zahtjev predviđanje rezultate pohranjuju u [Bazi podataka SQL Azure](https://azure.microsoft.com/services/sql-database/) , a koji su proizvodi pomoću servisa [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) . Upute za stvaranje nadzorne ploče za Power BI za ovaj predložak rješenja, pogledajte odjeljak u nastavku.

## <a name="how-to-bring-in-your-own-data"></a>**Kako unijeti na vlastitim podacima**

U ovom se odjeljku opisuje kako prenijeti vlastite podatke za Azure i koja su područja je potrebna promjena podataka u toj arhitekturi unijeti.

Je vjerojatno sa skupovima podataka prenesete odgovarati dataset koji se koristi za ovaj predložak rješenja. Razumijevanje podataka i preduvjetima bit će presudne u kako promijeniti ovaj predložak za rad s vlastitim podacima. Ako je ovo prvi izlaganje servisa Azure strojnog učenja, možete dobiti Uvod u ga pomoću u primjeru u [stvaranju vaš prvi eksperiment](machine-learning\machine-learning-create-experiment.md).

U sljedećim se odjeljcima navode će sekcije predloška koji ćete morati izmjene kada uvodi se novi skup podataka.

### <a name="azure-event-hub"></a>Koncentrator Azure događaja

Servis za [Azure događaj koncentrator](https://azure.microsoft.com/services/event-hubs/) je vrlo generički tako da se podaci mogu će se objaviti u središtu u obliku CSV ili JSON. Bez posebne obrade pojavljuje se u središtu Azure događaj, ali je važno razumijevanje podataka koji se umeće.

Ovaj dokument opisuju kako ingest podataka, ali nešto može jednostavno slati događaje ili podataka koncentrator za događaj programa Azure pomoću [API koncentrator za događaj](event-hubs\event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure strujanje Analytics

Servis za [Azure strujanje Analytics](https://azure.microsoft.com/services/stream-analytics/) koristi radi pri u stvarnom vremenu analize čitanju strujanja podataka i izvozite podatke u bilo koji broj izvora.

Za na zahtjev predviđanje energiju rješenje predloška, upit Azure strujanje analize sastoji se od dva podređenu upita, svaki troše događaje iz servisa Azure događaj koncentrator kao unosa i pritom izlaze dva različita mjesta. Te izlaze se sastoje od jednog skupa podataka za Power BI i jedno mjesto za pohranu Azure.

[Azure strujanje analize](https://azure.microsoft.com/services/stream-analytics/) upita možete pronaći tako da:

-   Prijava na [portal za upravljanje Azure](https://manage.windowsazure.com/)

-   Pronalaženje poslove analize strujanje ![](media\cortana-analytics-technical-guide-demand-forecast\icon-stream-analytics.png) koji su generirani kada je uveden rješenja. Jedna je za margina podataka bloba prostora za pohranu (npr. mytest1streaming432822asablob), a drugu sadrži margina podataka dodatka Power bi (npr. mytest1streaming432822asapbi).


-   Odabir

    -   ***UNOSE*** da biste pogledali unos upita

    -   ***UPIT*** za prikaz samog upita

    -   Da biste prikazali različite izlaze ***PROIZVODI***

Informacije o Azure strujanje analize upita izgradnju pronaći ćete u [Strujanje analize upita referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx) na MSDN-u.

U ovom rješenju Azure strujanje analize posao koja proizvodi dataset s blizu u stvarnom vremenu analize informacije o ulaznom toka podataka na nadzornu ploču dodatka Power BI prikazuje se kao dio ovog predloška rješenja. Budući implicitno znanja o ulaznom oblik podataka, te upite potrebni za promjenu ovisno o oblik podataka.

U Azure strujanje analize posao proizvodi Svi događaji [Događaj koncentrator](https://azure.microsoft.com/services/event-hubs/) za pohranu [Azure](https://azure.microsoft.com/services/storage/) i zato zahtijeva bez promjena bez obzira na to oblik podataka kao što je strujanjem cijelog događaj informacije za pohranu.

### <a name="azure-data-factory"></a>Tvorničke Azure podataka

Servis za [Azure podataka tvorničke](https://azure.microsoft.com/documentation/services/data-factory/) orchestrates premještanja i obrada podataka. U zahtjev predviđanje energiju rješenje predloška tvorničke podataka sastoji se od dvanaest [kanali](data-factory\data-factory-create-pipelines.md) koje premjestiti i obrada podataka pomoću različite tehnologije.

  Na tvorničke podataka možete pristupiti tako da otvorite čvor tvorničke podataka pri dnu dijagram predložak rješenja stvorena pomoću implementacije rješenja. To će vas odvesti na tvorničke podataka portala za upravljanje Azure. Ako vam se pojave pogreške u odjeljku svoje skupove podataka, možete zanemariti one kao što su zbog podataka tvorničke uvodi prije pokretanja generator podataka. Te pogreške ne sprječava na tvorničke podataka radi.

U ovom se odjeljku opisuje potrebne [kanali](data-factory\data-factory-create-pipelines.md) i [aktivnosti](data-factory\data-factory-create-pipelines.md) koje se nalaze na [Tvorničke Azure podataka](https://azure.microsoft.com/documentation/services/data-factory/). U nastavku je prikaz dijagrama rješenja.

![](media\cortana-analytics-technical-guide-demand-forecast\ADF2.png)

Pet kanali ovaj tvorničke sadržavati [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripte koje se koriste za particija i prikupljati podatke. Kada je naznačeno, skripte će se nalaziti na računu [Za pohranu Azure](https://azure.microsoft.com/services/storage/) stvorili tijekom postavljanja. Bit će njihovo mjesto: demandforecasting\\\\skripte\\\\grozd\\ \\ (ili https://[Your rješenje name].blob.core.windows.net/demandforecasting).

Slično [Azure strujanje analize](#azure-stream-analytics-1) upita, skripte [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) imati implicitno znanja o ulaznom oblik podataka, te upite potrebni za promjenu ovisno o potrebama podataka i [značajka inženjering](machine-learning\machine-learning-feature-selection-and-engineering.md) oblika.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*

Ovaj kanal za [kanal](data-factory\data-factory-create-pipelines.md) sadrži jednu aktivnost – aktivnost [HDInsightHive](data-factory\data-factory-hive-activity.md) pomoću [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) koji će se pokrenuti skriptu [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) aggregate svakih 10 sekundi kvalitetom strujanja podataka zahtjev substation razini zaračunava razinu regija i staviti u [Prostor za pohranu Azure](https://azure.microsoft.com/services/storage/) kroz posao Azure strujanje analize.

Skripta [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) za stvaranje particija zadatak je ***AggregateDemandRegion1Hr.hql***


#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*

[Kanal](data-factory\data-factory-create-pipelines.md) sadrži dvije aktivnosti:
- Aktivnosti [HDInsightHive](data-factory\data-factory-hive-activity.md) pomoću [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) koji će se pokrenuti skriptu grozd grupiranja svaki sat podataka zahtjev povijest substation razini zaračunava razinu regija i staviti u spremište Azure tijekom analize strujanje Azure posla

- [Kopiraj](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivnosti koje premješta skupne podatke iz Azure spremište blobova platforme Azure SQL baze podataka koja vam je dodijeljena kao dio instalaciju predložak rješenja.

[Vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripte za taj zadatak je ***AggregateDemandHistoryRegion.hql***.


#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*

Ove [kanali](data-factory\data-factory-create-pipelines.md) sadrže nekoliko aktivnosti i čiji rezultat je scored predviđanja iz pokusa Azure strojnog učenja povezan s ovim predloškom rješenja. Gotovo jednake su osim svaku od njih samo rukuje drugo područje u kojem se obavlja tvrtka različite RegionID proslijeđena u kanal ADF i grozd skripte za svako područje.  
Aktivnosti koje se nalaze u ovoj su:
-   Aktivnost [HDInsightHive](data-factory\data-factory-hive-activity.md) pomoću [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) koja se pokreće grozd skriptu da biste izvršili zbrajanja i značajka inženjering potrebne za pokusa Azure strojnog učenja. Grozd skripte za taj zadatak su odgovarajući ***PrepareMLInputRegionX.hql***.

-   [Kopiraj](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivnosti koje premješta rezultate iz [HDInsightHive](data-factory\data-factory-hive-activity.md) aktivnosti u jednom blob Azure prostora za pohranu koji mogu pristupati [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivnosti.

-   Aktivnost [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) koja se poziva strojnog učenja Azure isprobati bloba što rezultira rezultata koji se vratite na jednom Azure prostora za pohranu.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Ove [kanali](data-factory\data-factory-create-pipelines.md) sadrži jednu aktivnost - aktivnost [kopiju](https://msdn.microsoft.com/library/azure/dn835035.aspx) koja se pomiče rezultate pokusa Azure strojnog učenja odgovarajući ***MLScoringRegionXPipeline*** Azure SQL baze podataka koja vam je dodijeljena kao dio instalaciju predložak rješenja.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
[Kanali](data-factory\data-factory-create-pipelines.md) sadrži jednu aktivnost – [Kopiraj](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivnosti koje premješta Zbrojeno tijeku zahtjev podatke s ***LoadHistoryDemandDataPipeline*** Azure SQL baze podataka koja vam je dodijeljena kao dio instalaciju predložak rješenja.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Ove [kanali](data-factory\data-factory-create-pipelines.md) sadrži jednu aktivnost – [Kopiraj](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivnosti koje premješta referentnih podataka od Substation/regije/Topologygeo prenesenih Azure spremište blobova platforme kao dio instalaciju predložak rješenja Azure SQL bazu podataka koja vam je dodijeljena kao dio instalaciju predložak rješenja.

### <a name="azure-machine-learning"></a>Azure strojnog učenja
Stalna [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) koji se koristi za ovaj predložak rješenja nudi predviđanje zahtjev područja. Stalna specifičnu za skup podataka potrošena i stoga je potrebno izmjena ili zamjena specifični za podatke koji se ne unese u.

## <a name="monitor-progress"></a>**Nadzor napretka**
Kada se pokrene Generator podataka, kanal počinje da biste dobili hydrated i različite komponente rješenje pokretanje opusti se malo u akciji sljedeće naredbe izdala tvorničke podataka. Možete nadzirati kanal na dva načina.

1. Potvrdite podatke iz spremišta blobova Azure.

    Jedan zadatak analize strujanje piše neobrađenog ulazne podatke sa spremištem blobova. Ako kliknete na komponenti **Spremište blobova platforme Azure** vašeg rješenja na zaslonu koje uspješno implementiran rješenje i kliknite **Otvori** u desnom oknu, ono će vas odvesti na [portal za upravljanje Azure](https://portal.azure.com). Kad dođete njemu, kliknite na **blob-ova**. Na ploči sljedeći vidjet ćete popis spremnika. Kliknite **"energysadata"**. Na ploči sljedeći vidjet ćete mapu **"demandongoing"** . U mapi rawdata prikazat će se mapa s nazivima kao što su datum = 2016 01 28 itd. Ako vidite te mape, to znači da sirovim podacima uspješno generira na računalu i spremljene u spremište blobova platforme. Trebali biste vidjeti datoteke koje smiju konačne veličina u MB u tim mapama.

2. Provjera podataka iz baze podataka SQL Azure.

    Posljednji korak kanal je zapisivanje podataka (primjerice predviđanja iz strojnog učenja) u SQL baze podataka. Možda ćete morati pričekati maksimalno 2 sata za da se prikazuju u SQL baze podataka. Jedan praćenje kojom količinom podataka dostupna je u SQL baze podataka se putem [portala za upravljanje Azure](https://manage.windowsazure.com/). Na lijevoj ploči pronađite baze podataka SQL![](media\cortana-analytics-technical-guide-demand-forecast\SQLicon2.png) i kliknite ga. Zatim pronađite bazu podataka (odnosno demo123456db) i kliknite je. Na sljedećoj stranici u odjeljku **"Povezivanje s bazom podataka"** , kliknite **"Pokreni Transact-SQL upite odabiranja baze podataka sustava SQL"**.

    Ovdje možete kliknuti novi upit i upit za broj redaka (npr. "odaberite count(*) iz DemandRealHourly)" kao poveća bazu podataka, broj redaka u tablici trebala povećati.)

3. Potvrdite podatke iz dodatka Power BI nadzorne ploče.

    Nadzorna ploča tipkovni put Power BI možete postaviti praćenje neobrađenog ulazne podatke. Slijedite upute u odjeljku "Power BI nadzorne ploče".



## <a name="power-bi-dashboard"></a>**Nadzorna ploča za Power BI**

### <a name="overview"></a>Pregled

U ovom se odjeljku opisuje kako postaviti Power BI nadzorne ploče radi vizualnog prikaza podataka stvarnom vremenu iz Azure strujanje analize (tipkovni put), kao i predviđanje rezultate iz Azure strojnog učenja (Hladna put).


### <a name="setup-hot-path-dashboard"></a>Instalacija najnovije put nadzorne ploče

Sljedeći koraci vodit će vas kako vizualizacija izlaza podataka stvarnom vremenu iz strujanje analize zadataka koji su generirani prilikom implementacije rješenja. Za sljedeće korake potrebna je na račun [Servisa Power BI putem Interneta](http://www.powerbi.com/) . Ako nemate račun, možete ga [stvoriti](https://powerbi.microsoft.com/pricing).

1.  Dodajte izlazna Power BI u Azure strujanje analize (ASA).

    -  Morat ćete slijedite upute iz članka [Azure strujanje analize i Power BI: nadzorne ploče u stvarnom vremenu analytics za strujanje podatke u stvarnom vremenu vidljivost](stream-analytics-power-bi-dashboard.md) da biste postavili izlaz posla Azure strujanje analize kao nadzornu ploču dodatka Power BI.

    - Pronađite analize posao strujanje [portal za upravljanje Azure](https://manage.windowsazure.com). Naziv posao mora biti: YourSolutionName + "streamingjob" + slučajni broj + "asapbi" (odnosno demostreamingjob123456asapbi).

    - Dodajte izlazna PowerBI za posao ASA. Postavljanje **Izlaz pseudonim** kao **'PBIoutput'**. Postavljanje **Dataset ime** i **naziv tablice** kao **'EnergyStreamData'**. Nakon dodavanja Izlaz, kliknite **"Pokreni"** pri dnu stranice da biste pokrenuli posao strujanje analize. Trebali biste dobiti potvrdnu poruku (*npr.*, "Početni strujanje analize posao myteststreamingjob12345asablob uspjela").

2. Prijavite se na [Power BI na Internetu](http://www.powerbi.com)

    -   Na lijevoj ploči sekcije skupove podataka u radnom prostoru za moj mora biti da biste vidjeli novi skup podataka koji se prikazuje na lijevoj ploči Power BI. Ovo je strujanja podataka pomiču iz Azure strujanje analize u prethodnom koraku.

    -   Provjerite je li u oknu ***vizualizacije*** otvorena te se prikazuje na desnoj strani zaslona.

3. Stvaranje pločicu "Zahtjev po pečata":
    -   Kliknite skup podataka **'EnergyStreamData'** na lijevoj ploči sekcije skupova podataka.

    -   Kliknite ikonu **"linijski grafikon"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic8.png).

    -   Kliknite 'EnergyStreamData' na ploči **polja** .

    -   Kliknite **"Vremenske oznake"** , a zatim provjerite je li prikazuje se u odjeljku "Osi". Kliknite **"Opterećenje"** i provjerite je li prikazuje se u odjeljku "Vrijednosti".

    -   Na vrhu kliknite **SPREMI** , a zatim dajte naziv izvješću kao "EnergyStreamDataReport". U odjeljku Reports u oknu Navigator lijeve strane prikazat će se izvješća pod nazivom "EnergyStreamDataReport".

    -   Kliknite **"Pin vizualne"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) ikonu u gornjem desnom kutu ovaj linijski grafikon, prozor "PIN-a za tablu" mogu se prikazati možete odabrati na nadzornoj ploči. Odaberite "EnergyStreamDataReport", a zatim kliknite "Prikvači".

    -   Držite pokazivač miša iznad toj pločici na nadzornoj ploči kliknite "Uređivanje" ikonu u gornjem desnom kutu da biste promijenili njegov naslov kao "Zahtjev po pečata"

4.  Stvorite ostale pločice nadzorne ploče na temelju odgovarajuće skupova podataka. Prikaz konačni nadzorne ploče prikazano u nastavku.
        ![](media\cortana-analytics-technical-guide-demand-forecast\PBIFullScreen.png)


### <a name="setup-cold-path-dashboard"></a>Postavljanje Hladna put nadzorne ploče
Hladna put podataka kanal, ključna cilj je da biste dobili predviđanje zahtjev za svako područje. Power BI se povezuje s bazom podataka Azure SQL kao izvor podataka koji se pohranjuju predviđanje rezultata.

> [AZURE.NOTE] 1) u svega nekoliko sati da biste prikupili dovoljno predviđanja rezultata na nadzornoj ploči. Preporučujemo da pokrenete postupak 2-3 sata nakon ručak generator podataka. 2) u ovom koraku u preduvjeta je preuzmite i instalirajte besplatan softver [Power BI desktop](https://powerbi.microsoft.com/desktop).



1.  Pronađite vjerodajnice za bazu podataka.

    Trebat će vam **naziv poslužitelja baze podataka, naziv baze podataka, korisničko ime i lozinku** prije prelaska na sljedeće korake. Evo koraka koji će vas voditi kroz kako ih pronaći.

    -   Kada **"baze podataka SQL Azure"** na dijagram predložak rješenje Pretvori zeleni, kliknite ga, a zatim kliknite **"Otvori"**. Ćete dobiti upute i za portal za upravljanje Azure i informacije o stranici baze podataka te će se otvoriti.

    -   Na stranici, možete pronaći sekcije "Baza podataka". Popis korištenje baze podataka koji ste stvorili. Naziv baze podataka mora biti **"Vaš naziv rješenja + slučajnog broja + 'db'"** (npr. "mytest12345db").

    -   Kliknite svoju bazu podataka u novu izdvajanje pannel, možete pronaći naziv poslužitelja za bazu podataka na vrhu. Shoud s podrškom za bazu podataka poslužitelja naziva naziv može **"Vaš naziv rješenja + slučajnog broja + 'database.windows.net,1433'"** (npr. "mytest12345.database.windows.net,1433").

    -   Baze podataka **korisničko ime** i **Lozinka** isti su kao korisničko ime i lozinka koje ste prethodno snimili tijekom implementacije rješenja.

2.  Ažuriranje izvora podataka Hladna put datoteke Power BI
    -  Provjerite jeste li instalirali najnoviju verziju [dodatka Power BI desktop](https://powerbi.microsoft.com/desktop).

    -   U mapi **"DemandForecastingDataGeneratorv1.0"** koji ste preuzeli, dvokliknite datoteku **"Power BI Template\DemandForecastPowerBI.pbix"** . Početna vizualizacije temelje se na taj podataka. **Bilješke:** Ako vam se prikazuje pogreška massage, provjerite je li instalirali najnoviju verziju dodatka Power BI Desktop.

        Nakon otvaranja, pri vrhu stranice datoteku, kliknite **' uređivanje upita '**. U izdvajanje prozora, dvaput pritisnite **'Izvora'** na desnoj ploči.
    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic1.png)

    -   U izdvajanje prozora zamijenite **"Poslužitelja"** i **"Baze podataka"** vlastite poslužitelja i nazive baze podataka, a zatim **"U redu"**. Za naziv poslužitelja, provjerite je li Navedite priključak 1433 (**YourSolutionName.database.windows.net, 1433**). Zanemari poruka upozorenja koja se pojavljuju na zaslonu.

    -   U sljedeći izdvajanje prozora, vidjet ćete dvije mogućnosti u lijevom oknu (**Windows** i **baze podataka**). Kliknite **"Baza podataka"**, **"Korisničko ime"** i **"Lozinku"** (to je korisničko ime i lozinka koje ste unijeli kada prvi put implementiran rješenje i stvara baze podataka Azure SQL). U ***Odaberite kojoj razini da biste primijenili postavke***, potvrdite mogućnost razine baze podataka. Kliknite **"Povezivanje"**.

    -   Nakon što ste Vođena povratak na prethodnu stranicu, zatvorite prozor. Pojavit će se poruka Odjava: kliknite **Primijeni**. Na kraju, kliknite gumb **Spremi** da biste spremili promjene. Datoteku dodatka Power BI sada je uspostaviti vezu s poslužiteljem. Ako svoje vizualizacije su prazni, provjerite je li poništite odabiri vizualizacije planirate vizualizacija sve podatke klikom na ikonu Brisalo u gornjem desnom kutu na legendi. Gumbom Osvježi u skladu s vizualnim nove podatke na vizualizacije. Na početku, vidjet ćete samo Početni broj podataka na svoje vizualizacije kao što je zakazano osvježavanje svaki 3 sata tvorničke podataka. Nakon 3 sata, prikazat će se novi predviđanja odražavaju u svoje vizualizacije kada osvježite podatke.

3. (Neobavezno) Objavi na nadzornoj ploči Hladna put na [Internetu Power BI](http://www.powerbi.com/). Imajte na umu da se ovaj korak treba je Power BI račun (ili račun sustava Office 365).

    -   Kliknite **"Objavi"** i nekoliko sekundi prije nego kasnije će se prozor s prikazom "Objavljivanje Power BI uspjeh!" s Zelena kvačica. Kliknite vezu ispod "Otvori demoprediction.pbix u dodatku Power BI". Detaljne upute potražite u [Objavi na Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Da biste stvorili novu nadzornu ploču: kliknite na **+** znak pokraj sekcije **nadzorne ploče** u lijevom oknu. Unesite naziv "Zahtjev predviđanje pokazni videozapis" za ovu novu nadzornu ploču.

    -   Kad otvorite izvješća, kliknite ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) da biste prikvačili sve vizualizacije na nadzornu ploču. Detaljne upute potražite [Pin pločica na nadzornu ploču dodatka Power BI iz izvješća](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
        Idite na stranicu nadzorne ploče i prilagoditi veličinu i mjesto svoje vizualizacije i uređivanje naslovima. Da biste pronašli detaljne upute za uređivanje pločica, potražite u članku [Uređivanje pločice – promjena veličine, premještanje, Preimenuj, a zatim PIN-a, brisanje, dodajte hipervezu](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Slijedi primjer nadzorne ploče s neke vizualizacije Hladna put prikvačena na njega.

        ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic7.png)

4. (Neobavezno) Zakaži osvježavanje izvora podataka.
    -     Da biste zakaži osvježavanje podataka, zadržite pokazivač miša dataset **EnergyBPI konačne** , kliknite ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic3.png) , a zatim odaberite **Zakaži osvježavanje**.
    **Bilješke:** Ako se prikaže upozorenje massage, kliknite **Uređivanje vjerodajnice** i provjerite je li vjerodajnice za bazu podataka su isti kao što je opisano u koraku 1.

    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic4.png)

    -   Proširite odjeljak **Zakaži osvježavanje** . Uključite "ažurnima podataka".

    -   Zakazivanje osvježavanja ovisno o vašim potrebama. Da biste pronašli dodatne informacije potražite u članku [Osvježavanje podataka u dodatku Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).


## <a name="how-to-delete-your-solution"></a>**Kako izbrisati rješenje**
Provjerite je li zaustaviti generator podataka kada ne aktivno pomoću rješenja kao što je pokrenut generator podataka uzrokovati veće troškove. Izbrišite rješenje ako ne koristite. Brisanje rješenje izbrisat će se sve komponente dodjeli u vašoj pretplati kada implementiran rješenja. Da biste izbrisali rješenja kliknite na naziv rješenje na ploči lijevom rješenje predložak, a zatim kliknite Izbriši.

## <a name="cost-estimation-tools"></a>**Cijena procjeni Alati**

Sljedeća dva alata dostupnih radi bolje razumjeli ukupni uvrštene u radi predviđanje zahtjev za štednju rješenje predložak u svoju pretplatu troškovi:

-   [Microsoft Azure trošak Estimator alat za (na mreži)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure trošak Estimator alat (računalo)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Acknowledgements**
U ovom se članku je autor podataka pozvana simboli Yijing Chen i softver inženjeringom Min Qiu Microsoft.
