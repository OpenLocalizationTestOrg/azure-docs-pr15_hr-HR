<properties
    pageTitle="Tehnički vodič za predložak rješenje obavještavanje Cortana za predvidljivu održavanja u aerospace i drugim tvrtkama | Microsoft Azure"
    description="Tehnički vodič s predloškom rješenja s Microsoft Cortana obavještavanje za predvidljivu održavanja u aerospace, uslužni programi i prijevoza."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Tehnički vodič za predložak rješenje obavještavanje Cortana za predvidljivu održavanja u aerospace i drugim tvrtkama

## <a name="acknowledgements"></a>**Acknowledgements**
U ovom se članku je autor podataka fizičari Yan Zhang Gauher Shaheen, Fidan Boylu Uz i softver inženjeringom Dan Grecoe Microsoft.

## <a name="overview"></a>**Pregled**

Predlošci rješenja su osmišljeni za ubrzano postupka izgradnje E2E pokazni videozapis pri vrhu paket obavještavanje Cortana. Distribuiranih predložak će se Dodjela resursa za pretplatu potrebne komponente obavještavanje Cortana i stvaranje odnosa među njima. Također seeds kanal podataka s oglednim podacima generiran iz aplikacije generator podataka koji će preuzeti i instalirati na lokalno računalo nakon implementacije rješenja predložak. Podataka koji je generirao na generator će hydrate podataka kanal i početak generiranje strojnog učenja predviđanja koje se mogu vizualizirati pa na nadzornoj ploči za Power BI. Postupak implementacije će vas voditi kroz nekoliko koraka možete postaviti vjerodajnice za rješenja. Provjerite je li snimanje te vjerodajnice, kao što su naziv rješenja, korisničko ime i lozinku unesete tijekom uvođenja.  

Cilj ovog dokumenta je objašnjavaju arhitektura reference i razne komponente dodjeli u vašoj pretplati kao dio ovog predloška rješenja. Dokument i govori o tome da biste zamijenili uzorak podataka realni podacima sami moći vidjeti uvida i predviđanja s vlastitim podacima. Osim toga, dokument govori o dijelove rješenje predložak koji morate promijeniti ako ste željeli da biste prilagodili rješenja s vlastitim podacima. Na kraju se daju upute za stvaranje nadzorne ploče za Power BI za ovaj predložak rješenja.

>[AZURE.TIP] Možete preuzeti i ispisati [verzija ovog dokumenta u PDF-a](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Širu sliku**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Kada je rješenje implementiran, različite servise za Azure unutar Cortana analize paket aktiviraju (*odnosno* središte događaj analize strujanje HDInsight, tvorničke podataka, strojnog učenja, *itd.*). Arhitektura dijagrama gore prikazuje, na visoku razinu kako konstruirana predvidljivu održavanja Aerospace rješenje predloška iz završetka do kraja. Moći da biste istražili tih servisa na portalu za azure tako da kliknete na njima na dijagramu predložak rješenja stvorena pomoću implementacije rješenja osim HDInsight po taj servis dodjeli na zahtjev kada aktivnosti povezana kanal su potrebne za pokretanje i izbrisane naknadno.
Možete preuzeti [punoj veličini verzija dijagrama](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

U sljedećim odjeljcima opisuju svakom dijelu.

## <a name="data-source-and-ingestion"></a>**Izvor podataka i ingestion**

### <a name="synthetic-data-source"></a>Izvor podataka stilova sintetičkih

Za ovaj predložak izvor podataka koristi se generira iz aplikaciji za stolna računala koja će se preuzeti i pokrenuti lokalno nakon uspješne implementacije. Vidjet ćete upute da biste preuzeli i instalirali ovu aplikaciju na traka svojstava kad odaberete prvu čvor naziva predvidljivu Generator održavanje podataka na dijagramu predložak rješenja. Ovu aplikaciju sažetke sadržaja servisa [Azure događaj koncentrator](#azure-event-hub) točke podataka ili događaja koji će se koristiti u ostatku tijek rješenja. Ovaj izvor podataka se sastoji od ili izvedene iz javno dostupna podatke iz [spremišta podataka NASA](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) pomoću [Turbofan modul smanjene performanse simulacije zadani skup podataka](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Aplikacije za generiranje događaja će popuniti središtu događaj Azure samo dok ga se izvršava na vašem računalu.

### <a name="azure-event-hub"></a>Koncentrator Azure događaja

[Koncentrator za događaj Azure](https://azure.microsoft.com/services/event-hubs/) servis je primatelj unos nudi stilova sintetičkih izvora podataka na prethodno opisan.

## <a name="data-preparation-and-analysis"></a>**Priprema podataka i analize**


### <a name="azure-stream-analytics"></a>Azure strujanje Analytics

Servisa [Azure strujanje analize](https://azure.microsoft.com/services/stream-analytics/) koristi se za omogućuje blizu u stvarnom vremenu analize na unos toka iz servisa [Azure koncentrator za događaj](#azure-event-hub) i objavljivanje rezultate nadzorne ploče komponente [Power BI](https://powerbi.microsoft.com) kao i arhiviranje svih događaja neobrađenog dolazne servisa [Azure prostora za pohranu](https://azure.microsoft.com/services/storage/) za kasniju obradu servis [Tvorničke Azure podataka](https://azure.microsoft.com/documentation/services/data-factory/) .

### <a name="hd-insights-custom-aggregation"></a>Prilagođena zbrajanja HD uvida

Servisa Azure HD uvid služi da pokreću skripte za [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (orchestrated tvorničke podataka za Azure) za navođenje zbrajanja na neobrađenog događaje koje su arhiviraju pomoću servisa Azure strujanje analize.

### <a name="azure-machine-learning"></a>Azure strojnog učenja

Servis za [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) koristi (orchestrated tvorničke podataka za Azure) da bi bili predviđanja preostale upotrebljivosti (RUL) određeni aircraft modul dali unosa primili.

## <a name="data-publishing"></a>**Objavljivanje podataka**


### <a name="azure-sql-database-service"></a>Servis baze podataka Azure SQL

Servis [Baze podataka SQL Azure](https://azure.microsoft.com/services/sql-database/) služi za pohranu (upravlja tvorničke Azure podataka) predviđanja primio servisa Azure strojnog učenja koji će se potrošiti na nadzornoj ploči [Power BI](https://powerbi.microsoft.com) .

## <a name="data-consumption"></a>**Potrošnje podataka**

### <a name="power-bi"></a>Power BI

Servis [Power BI](https://powerbi.microsoft.com) koristi se za prikaz nadzorne ploče koja sadrži zbrajanja i upozorenja koje ste dobili od servisa [Azure strujanje analize](https://azure.microsoft.com/services/stream-analytics/) kao i RUL predviđanja pohranjuju u [Bazi podataka SQL Azure](https://azure.microsoft.com/services/sql-database/) , a su proizvodi pomoću servisa [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) . Upute za stvaranje nadzorne ploče za Power BI za ovaj predložak rješenja, pogledajte odjeljak u nastavku.

## <a name="how-to-bring-in-your-own-data"></a>**Kako unijeti na vlastitim podacima**

U ovom se odjeljku opisuje kako prenijeti vlastite podatke za Azure i koja su područja za podatke u toj arhitekturi unijeti činite potrebne promjene.

Je vjerojatno sa skupovima podataka prenesete odgovarati dataset koristi [Turbofan modul smanjene performanse simulacije zadani skup podataka](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) koji koristi za ovaj predložak rješenja. Razumijevanje podataka i preduvjetima bit će presudne u kako promijeniti ovaj predložak za rad s vlastitim podacima. Ako je ovo prvi izlaganje servisa Azure strojnog učenja, možete dobiti Uvod u ga pomoću u primjeru u [stvaranju vaš prvi eksperiment](machine-learning-create-experiment.md).

U sljedećim se odjeljcima navode će sekcije predloška koji ćete morati izmjene kada uvodi se novi skup podataka.

### <a name="azure-event-hub"></a>Koncentrator Azure događaja

Servis za Azure događaj koncentrator je vrlo generički tako da se podaci mogu će se objaviti u središtu u obliku CSV ili JSON. Bez posebne obrade pojavljuje se u središtu Azure događaj, ali je važno brzo razumijevanje podataka koji se umeće.

Ovaj dokument opisuju kako ingest podataka, ali možete jednostavno poslati događaje ili podataka koncentrator za događaj programa Azure pomoću API koncentrator za događaj.

### <a name="azure-stream-analytics"></a>Azure strujanje Analytics

Servis za Azure strujanje Analytics koristi radi pri u stvarnom vremenu analize čitanju strujanja podataka i izvozite podatke u bilo koji broj izvora.

Za održavanje predvidljivu Aerospace rješenje predloška Azure strujanje analize upita sastoji se od četiri podređenu upita, svaki dosta događaje iz servisa Azure koncentrator za događaj i pritom izlaze četiri različita mjesta. Te izlaze sastoje se od tri skupova podataka za Power BI i jedno mjesto za pohranu Azure.

Azure strujanje analize upita možete pronaći tako da:

-   Prijava na portal sustava Azure

-   Pronalaženje poslove analize strujanje ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) koji su generirani kada je uveden rješenje (*npr.*, **maintenancesa02asapbi** i **maintenancesa02asablob** predvidljivu održavanja rješenja)

-   Odabir

    -   ***UNOSE*** da biste pogledali unos upita

    -   ***UPIT*** za prikaz samog upita

    -   Da biste prikazali različite izlaze ***PROIZVODI***

Informacije o Azure strujanje analize upita izgradnju pronaći ćete u [Strujanje analize upita referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx) na MSDN-u.

U ovom rješenju upiti izlaz tri skupove podataka s blizu u stvarnom vremenu analize informacije o dolazne toka podataka na nadzornoj ploči za Power BI, koja se nudi kao dio ovog predloška rješenja. Budući implicitno znanja o ulaznom oblik podataka, te upite potrebni za promjenu ovisno o oblik podataka.

Upit u programu u drugom strujanje analize posao **maintenancesa02asablob** jednostavno proizvodi Svi događaji [Događaj koncentrator](https://azure.microsoft.com/services/event-hubs/) za pohranu [Azure](https://azure.microsoft.com/services/storage/) i zato zahtijeva bez promjena bez obzira na to oblik podataka kao što je strujanjem cijelog događaj informacije za pohranu.

### <a name="azure-data-factory"></a>Tvorničke Azure podataka

Servis za [Azure podataka tvorničke](https://azure.microsoft.com/documentation/services/data-factory/) orchestrates premještanja i obrada podataka. U predvidljivu održavanje Aerospace rješenje predloška tvorničke podataka sastoji se od tri [kanali](../data-factory/data-factory-create-pipelines.md) koje premjestiti i obrada podataka pomoću različite tehnologije.  Možete pristupiti na tvorničke podataka tako da otvorite na čvor tvorničke podataka pri dnu predložak dijagram rješenja stvorena pomoću implementacije rješenja. To će vas odvesti na tvorničke podataka portala za Azure. Ako vam se pojave pogreške u odjeljku svoje skupove podataka, možete zanemariti one kao što su zbog podataka tvorničke uvodi prije pokretanja generator podataka. Te pogreške ne sprječava na tvorničke podataka radi.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

U ovom se odjeljku opisuje potrebne [kanali](../data-factory/data-factory-create-pipelines.md) i [aktivnosti](../data-factory/data-factory-create-pipelines.md) koje se nalaze na [Tvorničke Azure podataka](https://azure.microsoft.com/documentation/services/data-factory/). U nastavku je prikaz dijagrama rješenja.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Dva kanali ovaj tvorničke sadržavati [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripte koje se koriste za particija i prikupljati podatke. Kada je naznačeno, skripte će se nalaziti na računu [Za pohranu Azure](https://azure.microsoft.com/services/storage/) stvorili tijekom postavljanja. Bit će njihovo mjesto: maintenancesascript\\\\skripte\\\\grozd\\ \\ (ili https://[Your rješenje name].blob.core.windows.net/maintenancesascript).

Slično [Azure strujanje analize](#azure-stream-analytics-1) upita, skripte [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) imati implicitno znanja o ulaznom oblik podataka, te upite potrebni za promjenu ovisno o potrebama podataka i [značajka inženjering](machine-learning-feature-selection-and-engineering.md) oblika.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

[Kanal](../data-factory/data-factory-create-pipelines.md) sadrži jednu aktivnost - aktivnost [HDInsightHive](../data-factory/data-factory-hive-activity.md) pomoću [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) koji će se pokrenuti skriptu [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) particija podatke staviti u [Prostor za pohranu Azure](https://azure.microsoft.com/services/storage/) tijekom [Analize strujanje Azure](https://azure.microsoft.com/services/stream-analytics/) zadatka.

Skripta [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) za stvaranje particija zadatak je ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

[Kanal](../data-factory/data-factory-create-pipelines.md) sadrži nekoliko aktivnosti i čiji rezultat je scored predviđanja iz [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) Poigrajte povezan s ovim predloškom rješenja.

Aktivnosti koje se nalaze u ovoj su:

-   Aktivnost [HDInsightHive](../data-factory/data-factory-hive-activity.md) pomoću programa [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) koja se pokreće [vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skriptu da biste izvršili zbrajanja i značajka inženjering potrebne za pokusa [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) .
    [Vrste Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripta za stvaranje particija zadatak je ***PrepareMLInput.hql***.

-   [Kopiraj](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivnosti koje premješta rezultate iz [HDInsightHive](../data-factory/data-factory-hive-activity.md) aktivnosti u jednom blob [Azure prostora za pohranu](https://azure.microsoft.com/services/storage/) koji mogu pristupati [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivnosti.

-   Aktivnost [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) koja se poziva [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) isprobati što rezultira rezultata koji se vratite na jednom blobova platforme [Azure prostora za pohranu](https://azure.microsoft.com/services/storage/) .

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

[Kanal](../data-factory/data-factory-create-pipelines.md) sadrži jednu aktivnost – [Kopiraj](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivnosti koje premješta rezultate [Azure strojnog učenja](#azure-machine-learning) Eksperimentirajte s ***MLScoringPipeline*** s [Bazom podataka SQL Azure](https://azure.microsoft.com/services/sql-database/) koja vam je dodijeljena kao dio instalaciju predložak rješenja.

### <a name="azure-machine-learning"></a>Azure strojnog učenja

Stalna [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) koji se koristi za ovaj predložak rješenja nudi na ostale korisne vijek (RUL) od mehanizam aircraft. Stalna specifičnu za skup podataka potrošena i stoga je potrebno izmjena ili zamjena specifični za podatke koji se ne unese u.

Informacije o načinu stvaranja pokusa Azure strojnog učenja potražite u članku [predvidljivu održavanje: korak 1 od 3 i Priprema podataka i značajka inženjering](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Nadzor napretka**
 Kada se pokrene Generator podataka, kanal počinje da biste dobili hydrated i različite komponente rješenje pokretanje opusti se malo u akciji sljedeće naredbe izdala tvorničke podataka. Možete nadzirati kanal na dva načina.

1. Jedan zadatak analize strujanje piše neobrađenog ulazne podatke sa spremištem blobova. Ako kliknete na komponenti blobova vašeg rješenja na zaslonu koje uspješno implementiran rješenje i zatim kliknite Otvori u desnom oknu, ono će vas odvesti na [portal za upravljanje](https://portal.azure.com/). Kad dođete postoji, kliknite na blob-ova. Na ploči sljedeći vidjet ćete popis spremnika. Kliknite **maintenancesadata**. Na ploči sljedeći vidjet ćete mapu **rawdata** . U mapi rawdata prikazat će se mapa s nazivima kao što su h = 17, h = 18 itd. Ako vidite tih mapa, znači da uspješno je sirovim podacima koje generira na računalu i spremljene u spremište blobova platforme. Trebali biste vidjeti csv datoteke koje smiju konačne veličina u MB u tim mapama.

2. Posljednji korak kanal je zapisivanje podataka (primjerice predviđanja iz strojnog učenja) u SQL baze podataka. Možda ćete morati pričekati najviše tri vremena da se prikazuju u SQL baze podataka. Jedan praćenje kojom količinom podataka dostupna je u SQL baze podataka se putem [portala za azure](https://manage.windowsazure.com/). Na lijevoj ploči pronađite baze podataka SQL ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) i kliknite ga. Zatim pronađite bazu podataka **pmaintenancedb** i kliknite je. Na sljedećoj stranici pri dnu kliknite Upravljanje

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    Ovdje možete kliknuti novi upit i upit za broj redaka (npr., odaberite count(*) iz PMResult). Kao što je poveća bazu podataka, broj redaka u tablici trebala povećati.


## <a name="power-bi-dashboard"></a>**Nadzorna ploča za Power BI**

### <a name="overview"></a>Pregled

U ovom se odjeljku opisuje način da biste postavili Power BI nadzorne ploče radi vizualnog prikaza podataka stvarnom vremenu iz Azure strujanje analize (tipkovni put) te grupe za predviđanje rezultate iz Azure strojnog učenja (Hladna put).

### <a name="setup-cold-path-dashboard"></a>Postavljanje Hladna put nadzorne ploče

Podaci kanal Hladna put ključna cilj je da biste dobili predvidljivu RUL (preostalo upotrebljivosti) svaki modul aircraft kada završi leta (ciklusa). Rezultat predviđanje se ažurira svakih 3 sata za procjenjivanje modula aircraft koji ste završili leta tijekom proteklih 3 sata.

Power BI se povezuje s bazom podataka Azure SQL kao izvor podataka koji se pohranjuju rezultate za predviđanje. Napomena: 1) nakon implementacija rješenja realni predviđanje prikazat će se u bazi podataka unutar 3 sata.
Pbix datoteke koje ste dobili uz preuzimanje Generator sadrži neke podatke Početni broj tako da možete odmah stvoriti nadzorne ploče za Power BI. 2) u ovom koraku u preduvjeta je preuzmite i instalirajte besplatan softver [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Sljedeći koraci vodit će vas kako povezati datoteku pbix SQL baze podataka koji je spun prema gore u trenutku implementacije rješenja koja sadrži podatke (*npr*. Predviđanje rezultate) za vizualizaciju.

1.  Pronađite vjerodajnice za bazu podataka.

    Morat ćete **naziv poslužitelja baze podataka, naziv baze podataka, korisničko ime i lozinku** prije prelaska na sljedeće korake. Evo koraka koji će vas voditi kroz kako ih pronaći.

    -   Kada **' baze podataka SQL Azure'** na dijagram predložak rješenje Pretvori zeleni, kliknite ga, a zatim kliknite **"Otvori"**.

    -   Prikazat će se nova kartica/prozoru preglednika koji prikazuje Azure stranici portala. U lijevom oknu kliknite **"Resursa grupe"** .

    -   Odaberite pretplatu koju koristite za implementacije rješenja sustava, a zatim odaberite **' YourSolutionName\_ResourceGroup'**.

    -   U novi izdvajanje ploča, kliknite na ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) ikonu za pristup bazi podataka. Naziv baze podataka je uz ovaj ikona (*npr.*, **"pmaintenancedb"**) i **naziv poslužitelja baze podataka** nalazi u odjeljku svojstva za naziv poslužitelja, a trebala bi izgledati slično **YourSoutionName.database.windows.net**.

    -   Baze podataka **korisničko ime** i **Lozinka** isti su kao korisničko ime i lozinka koje ste prethodno snimili tijekom implementacije rješenja.

2.  Ažuriranje izvora podataka Hladna put datoteke izvješća s Power BI Desktop.

    -   U mapi na Računalu koju ste preuzeli i raspakirana Generator datoteke, dvokliknite na **PowerBI\\PredictiveMaintenanceAerospace.pbix** datoteku. Ako vam se prikazuje sve poruke upozorenja kada otvorite datoteku, zanemarite ih. Pri vrhu stranice datoteku, kliknite **' uređivanje upita '**.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Vidjet ćete dvije tablice, **RemainingUsefulLife** i **PMResult**. Odabir prve tablice, a zatim kliknite ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) pokraj **'Izvora'** u odjeljku **"PRIMIJENJENI KORACI"** na desnoj ploči **' postavke upita '** . Zanemari sve poruke upozorenja koje se prikazuju.

    -   U izdvajanje prozora zamijenite **"Poslužitelj"** i **"Baza podataka"** vlastite poslužitelja i nazive baze podataka, a zatim **"U redu"**. Za naziv poslužitelja, provjerite je li Navedite priključak 1433 (**YourSoutionName.database.windows.net, 1433**). Ostavite polje Database kao **pmaintenancedb**. Zanemari poruka upozorenja koja se pojavljuju na zaslonu.

    -   U sljedeći izdvajanje prozora, vidjet ćete dvije mogućnosti u lijevom oknu (**Windows** i **baze podataka**). Kliknite **"Baza podataka"**, **"Username"** i **"Lozinku"** (to je korisničko ime i lozinka koje ste unijeli kada prvi put implementiran rješenje i stvara baze podataka Azure SQL). U ***Odaberite kojoj razini da biste primijenili postavke***, potvrdite mogućnost razine baze podataka. Kliknite **'Poveži'**.

    -   Kliknite na druge tablice **PMResult** , a zatim kliknite ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) pokraj **'Izvora'** u odjeljku **' PRIMIJENJENI KORACI '** u desnom oknu **"postavke upita"** Ažuriraj nazive poslužitelj i bazu podataka kao prema gore navedenim uputama i kliknite u redu.

    -   Nakon što ste Vođena povratak na prethodnu stranicu, zatvorite prozor. Pojavit će se poruka Odjava: kliknite **Primijeni**. Na kraju, kliknite gumb **Spremi** da biste spremili promjene. Datoteku dodatka Power BI sada je uspostaviti vezu s poslužiteljem. Ako svoje vizualizacije su prazni, provjerite je li poništite odabiri vizualizacije planirate vizualizacija sve podatke klikom na ikonu Brisalo u gornjem desnom kutu na legendi. Gumbom Osvježi u skladu s vizualnim nove podatke na vizualizacije. Na početku, vidjet ćete samo Početni broj podataka na svoje vizualizacije kao što je zakazano osvježavanje svaki 3 sata tvorničke podataka. Nakon 3 sata, prikazat će se novi predviđanja odražavaju u svoje vizualizacije kada osvježite podatke.

3.  (Neobavezno) Objavi na nadzornoj ploči Hladna put na [Internetu Power BI](http://www.powerbi.com/). Imajte na umu da se ovaj korak treba je Power BI račun (ili račun sustava Office 365).

    -   Kliknite **"Objavi"** i nekoliko sekundi prije nego kasnije pojavljuje se prozor prikaz "Objavljivanje Power BI uspjeh!" s Zelena kvačica. Kliknite vezu ispod "Otvori PredictiveMaintenanceAerospace.pbix u Power BI". Da biste pronašli detaljne upute potražite u članku [objavljivanje iz dodatka Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Da biste stvorili novu nadzornu ploču: kliknite na **+** znak pokraj sekcije **nadzorne ploče** u lijevom oknu. Unesite naziv "Predvidljivu održavanja pokazni videozapis" za ovu novu nadzornu ploču.

    -   Kad otvorite izvješća, kliknite ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) da biste prikvačili sve vizualizacije na nadzornu ploču. Detaljne upute potražite [Pin pločica na nadzornu ploču dodatka Power BI iz izvješća](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Idite na stranicu nadzorne ploče i prilagoditi veličinu i mjesto svoje vizualizacije i uređivanje naslovima. Da biste pronašli detaljne upute za uređivanje pločica, potražite u članku [Uređivanje pločice – promjena veličine, premještanje, Preimenuj, a zatim PIN-a, brisanje, dodajte hipervezu](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Slijedi primjer nadzorne ploče s neke vizualizacije Hladna put prikvačena na njega.  Ovisno o tome koliko pokrenete generator vaše podatke, brojeve na vizualizacije mogu se razlikovati.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   Da biste zakaži osvježavanje podataka, zadržite pokazivač miša **PredictiveMaintenanceAerospace** dataset, kliknite ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) , a zatim odaberite **Zakaži osvježavanje**.
<br/>
        **Bilješke:** Ako se prikaže upozorenje massage, kliknite **Uređivanje vjerodajnice** i provjerite je li vjerodajnice za bazu podataka su isti kao što je opisano u koraku 1.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   Proširite odjeljak **Zakaži osvježavanje** . Uključite "ažurnima podataka".
    <br/>
    -   Zakazivanje osvježavanja ovisno o vašim potrebama. Da biste pronašli dodatne informacije potražite u članku [Osvježavanje podataka u dodatku Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Instalacija najnovije put nadzorne ploče

Sljedeći koraci vodit će vas kako vizualizacija izlaza podataka stvarnom vremenu iz strujanje analize zadataka koji su generirani prilikom implementacije rješenja. Za sljedeće korake potrebna je na račun [Servisa Power BI putem Interneta](http://www.powerbi.com/) . Ako nemate račun, možete ga [stvoriti](https://powerbi.microsoft.com/pricing).

1.  Dodajte izlazna Power BI u Azure strujanje analize (ASA).

    -  Morat ćete slijedite upute iz članka [Azure strujanje analize i Power BI: nadzorne ploče u stvarnom vremenu analytics za strujanje podatke u stvarnom vremenu vidljivost](stream-analytics-power-bi-dashboard.md) da biste postavili izlaz posla Azure strujanje analize kao nadzornu ploču dodatka Power BI.
    - ASA upit sadrži tri izlaze koje su **aircraftmonitor**, **aircraftalert**i **flightsbyhour**. Upit možete vidjeti klikom na karticu upita. Odgovara svake od ovih tablica, morat ćete dodati na Izlaz ASA. Prilikom dodavanja prvog izlaz (*npr.* **aircraftmonitor**) provjerite jesu li **Pseudonim za izlaz**, **Dataset ime** i **Naziv tablice** u istom (**aircraftmonitor**). Ponovite korake da biste dodali izlaza za **aircraftalert**i **flightsbyhour**. Kada dodate sva tri izlaz tablice i posao ASA s radom, dobit potvrdnu poruku (*npr.*, "Početni strujanje analize posao maintenancesa02asapbi uspjela").

2. Prijavite se na [Power BI na Internetu](http://www.powerbi.com)

    -   Na lijevoj ploči sekcije skupove podataka u radnom prostoru za moj imena **aircraftmonitor** ***skup podataka*** , **aircraftalert**i **flightsbyhour** prikazivati. Ovo je strujanja podataka pomiču iz Azure strujanje analize u prethodnom koraku. Skup podataka **flightsbyhour** možda neće se prikazivati u isto vrijeme kao na druge dvaju skupova podataka zbog prirode SQL upita u pozadini. Međutim, ona treba prikazati nakon sata.
    -   Provjerite je li u oknu ***vizualizacije*** otvorena te se prikazuje na desnoj strani zaslona.

3. Nakon što dodate podataka slijedi u Power BI, možete početi vizualizacija strujanja podataka. Ispod je Ogledna nadzorna ploča s nekim vizualizacijama tipkovni put prikvačena na njega. Možete stvoriti ostale pločice nadzorne ploče na temelju odgovarajuće skupova podataka. Ovisno o tome koliko pokrenete generator vaše podatke, brojeve na vizualizacije mogu se razlikovati.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Evo nekoliko koraka da biste stvorili biblioteku gornje pločice – pločica na "tim prikaz od senzora 11 nasuprot prag 48.26":

    -   Kliknite skup podataka **aircraftmonitor** na lijevoj ploči sekcije skupova podataka.

    -   Kliknite ikonu **Linijski grafikon** .

    -   U oknu **polja** kliknite **obrađuju** tako da se prikazuje u odjeljku "Osi" u oknu **vizualizacije** .

    -   Kliknite "s11" i "s11\_upozorenje" da bi se oba prikazivali u odjeljku "Vrijednosti". Kliknite malu strelicu pokraj **s11** i **s11\_upozorenje**, promijenite "Zbroji" u "Prosjek".

    -   Na vrhu kliknite **SPREMI** , a zatim dajte naziv izvješću "aircraftmonitor". U odjeljku **izvješća** **navigacijskog** okna na lijevoj strani prikazat će se izvješća pod nazivom "aircraftmonitor".

    -   Kliknite ikonu **Vizualne PIN-a** na gornjem desnom kutu ovaj linijski grafikon. Prozor "PIN-a za tablu" može prikazivati možete odabrati na nadzornoj ploči. Odaberite "Predvidljivu održavanja pokazni videozapis", a zatim kliknite "Prikvači".

    -   Pokazivač miša na toj pločici na nadzornoj ploči, kliknite ikonu "Uredi" u gornjem desnom kutu da biste promijenili njegov naslov "Tim prikaz od senzora 11 nasuprot prag 48.26" i podnaslov "Prosjek svim tim tijekom vremena".

## <a name="how-to-delete-your-solution"></a>**Kako izbrisati rješenje**
Provjerite je li zaustaviti generator podataka kada ne aktivno pomoću rješenja kao što je pokrenut generator podataka uzrokovati veće troškove. Izbrišite rješenje ako ne koristite. Brisanje rješenje izbrisat će se sve komponente dodjeli u vašoj pretplati kada implementiran rješenja. Da biste izbrisali rješenja kliknite na naziv rješenje na ploči lijevom rješenje predložak, a zatim kliknite Izbriši.

## <a name="cost-estimation-tools"></a>**Cijena procjeni Alati**

Sljedeća dva alata dostupnih radi bolje razumjeli ukupni uvrštene u radi predvidljivu održavanja Aerospace predloška rješenja u svoju pretplatu troškovi:

-   [Microsoft Azure trošak Estimator alat za (na mreži)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure trošak Estimator alat (računalo)](http://www.microsoft.com/download/details.aspx?id=43376)
