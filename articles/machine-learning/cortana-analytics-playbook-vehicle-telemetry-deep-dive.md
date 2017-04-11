<properties 
    pageTitle="Vozila telemetrijskih analize rješenje playbook: precizno dive u rješenje | Microsoft Azure" 
    description="Korištenje mogućnosti programa obavještavanje Cortana da bi se dobio u stvarnom vremenu i predvidljivu uvida na vozila stanja i vožnju navike." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Vozila telemetrijskih analize rješenje playbook: precizno dive u rješenje

U ovom **izbornik** navode veze na odjeljke u ovom playbook: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

U ovom odjeljku bušilice prema dolje u svaku faze izmišljeni arhitekturu rješenja s uputama i savjeta za prilagodbu. 

## <a name="data-sources"></a>Izvori podataka

Rješenje koristi dvije različitih izvora podataka:

- **Simulirani vozila signale i skup dijagnostičkih podataka** i 
- **katalog vozila**

Simulator za telematics vozila je dio paketa rješenja. Emits dijagnostičke informacije i signale odgovara stanje vozila i vožnju uzorak u određenom trenutku u vremenu. Kliknite [Vozila Telematics Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) da biste preuzeli **Vozila Telematics Simulator Visual Studio rješenja** za prilagodbe svojim potrebama. Katalog vozila sadrži referencu skup podataka s VIN za mapiranje modela.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Slika 2 – Simulator Telematics vozila*

Ovo je JSON oblikovani skup podataka koji sadrži sljedeće shemu.

Stupac | Opis | Vrijednosti 
 ------- | ----------- | --------- 
VIN | Slučajno generiranim vozila identifikacijskog broja | To je dobiven od glavni popis 10 000 slučajno generiranim vozila identifikacijskih brojeva.
Vanjski temperatura | Vanjski temperatura gdje je vožnju vozila | Slučajno generiranim broj od 0 do 100
Modul temperatura | Modul temperatura vozila | Slučajno generiranim broj od 0 do 500
Brzina | Modul Brzina kojom je vožnju vozila | Slučajno generiranim broj od 0 do 100
Goriva | Razina goriva vozila | Slučajno generiranim broj od 0 do 100 (označava postotak razine goriva)
EngineOil | Modul ULJE razinu vozila | Slučajno generiranim broj od 0 do 100 (označava modul ULJE razine postotak)
Guma tlaka | Tlaka guma vozila | Nasumično generirani broj od 0 do 50 (označava guma tlaka razine postotak)
Brojač kilometara | Čitanje brojač kilometara vozila | Slučajno generiranim broj od 0 do 200000
Accelerator_pedal_position | Položaj papučicom accelerator vozila | Slučajno generiranim broj od 0 do 100 (označava postotak razine accelerator)
Parking_brake_status | Označava hoće li vozila je Grupirani stupčasti grafikon ili ne | TRUE ili False
Headlamp_status | Upućuje na to gdje se headlamp uključeno ili ne | TRUE ili False
Brake_pedal_status | Označava hoće li papučicom kočnica pritiska na tipku ili ne | TRUE ili False
Transmission_gear_position | Položaj brzine prijenosa vozila | Statusi: najprije u drugoj, treći, četvrti, peti, šestog, seventh, eighth
Ignition_status | Označava je li vozila pokrenut ili zaustavljen | TRUE ili False
Windshield_wiper_status | Označava hoće li windshield wiper uključen ili ne | TRUE ili False
ABS | Označava hoće li se ABS aktivan ili ne | TRUE ili False
Vremenska oznaka | Vremenska oznaka stvaranja točke podataka | Datum
Grad | Mjesto vozila | 4 gradova u rješenje: S, Redmond, Sammamish, Seattle


Referenca skup podataka vozila model sadrži VIN mapiranje modela. 

VIN | Model |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Sedan |
8J0U8XCPRGW4Z3NQE | Hibridno |
WORG68Z2PLTNZDBI7 | Obiteljski Saloon |
JTHMYHQTEPP4WBMRN | Sedan |
W9FTHG27LZN1YWO0Y | Hibridno |
MHTP9N792PHK08WJM | Obiteljski Saloon |
EI4QXI2AXVQQING4I | Sedan |
5KKR2VB4WHQH97PF8 | Hibridno |
W9NSZ423XZHAONYXB | Obiteljski Saloon |
26WJSGHX4MA5ROHNL | Convertible |
GHLUB6ONKMOSI7E77 | Wagon stanice |
9C2RHVRVLMEJDBXLP | Sažmi automobila |
BRNHVMZOUJ6EOCP32 | Mali SUV |
VCYVW0WUZNBTM594J | Sportski automobil |
HNVCE6YFZSA5M82NY | Srednje SUV |
4R30FOR7NUOBL05GJ | Wagon stanice |
WYNIIY42VKV6OQS1J | Veliki SUV |
8Y5QKG27QET1RBK7I | Veliki SUV |
DF6OX2WSRA6511BVG | Coupe |
Z2EOZWZBXAEW3E60T | Sedan |
M4TV6IEALD5QDS3IR | Hibridno |
VHRA1Y2TGTA84F00H | Obiteljski Saloon |
R0JAUHT1L1R3BIKI0 | Sedan |
9230C202Z60XX84AU | Hibridno |
T8DNDN5UDCWL7M72H | Obiteljski Saloon |
4WPYRUZII5YV7YA42 | Sedan |
D1ZVY26UV2BFGHZNO | Hibridno |
XUF99EW9OIQOMV7Q7 | Obiteljski Saloon
8OMCL3LGI7XNCC21U | Convertible |
…….  |   |


### <a name="to-generate-simulated-data"></a>Da biste generirali Simulirani podataka
1.  Da biste preuzeli paket simulator podataka, kliknite strelicu u gornjem desnom kutu čvor Simulator Telematics vozila. Spremite i izdvojiti datoteke lokalno na vašem računalu. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Slika 3 – vozila Telemetrijskih analize rješenje nacrt*

2.  Na lokalnom računalu, otvorite mapu u kojoj dobivenih vozila Telematics Simulator paketa. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Slika 4 – mapu Simulator Telematics vozila*

3.  Izvršavanje aplikacije **CarEventGenerator.exe**.

### <a name="references"></a>Reference

[Vozila Telematics Simulator Visual Studio rješenja](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Koncentrator Azure događaja](https://azure.microsoft.com/services/event-hubs/)

[Tvorničke Azure podataka](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Ingestion
Kombinacije Azure događaj koncentratora, strujanje analize i tvorničke podaci su leveraged da biste ingest signale vozila, dijagnostičkih događaja i u stvarnom vremenu i obrade analize. Te komponente stvara i konfigurirati kao dio implementacije rješenja. 

### <a name="real-time-analysis"></a>U stvarnom vremenu analize
Događaji koje generira Simulator Telematics vozila se objavljuju u središtu događaja pomoću SDK koncentrator za događaj. Zadatak analize strujanje ingests te događaje iz centra za događaj i obrađuje podatke u stvarnom vremenu radi analize stanja vozila. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Slika 5 - događaj koncentrator nadzorne ploče*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Slika 6 - strujanje analize posao obrada podataka*

Analitički posao strujanje;

- ingests podatke iz koncentratora za događaj 
- izvodi spoja s referentnih podataka da biste mapirali vozila VIN odgovarajuće model 
- Nastavi ih u spremište blobova platforme Azure za obogaćene obrade analize. 

Sljedeći upit analize strujanje koristi se za zadržava podatke u spremište blobova platforme Azure. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Slika 7 – strujanje analize posao upita za podatke ingestion*

### <a name="batch-analysis"></a>Grupe za analizu
Ne možemo su generiranje dodatne količinu signale Simulirani vozila i dijagnostičkih skup podataka za bogatiji obrade analize. To je potrebna da bi dobro predstavniku podatkovna jedinica za obrade. U tu svrhu smo koriste kanal pod nazivom "PrepareSampleDataPipeline" u tijeku rada tvorničke Azure podataka da biste generirali jednu godinu vrijedne signale Simulirani vozila i skup dijagnostičkih podataka. Kliknite [Podaci tvorničke prilagođene aktivnosti](http://go.microsoft.com/fwlink/?LinkId=717077) da biste preuzeli na tvorničke podataka prilagođene DotNet aktivnosti Visual Studio rješenje za prilagodbe svojim potrebama. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Slika 8 - Priprema oglednih podataka za tijek rada za obradu Obrada*

Kanal sastoji se od prilagođene .net ADF aktivnosti, a zatim Prikaži ovdje:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Slika 9 – PrepareSampleDataPipeline*

Kad kanal izvršava uspješno i "RawCarEventsTable" dataset označen "Spreman", jednogodišnju vrijedi signala Simulirani vozila i dijagnostičkih podataka proizvedeno. Vidite sljedeće mape i datoteke stvorene u račun za pohranu u kontejneru "connectedcar":

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Slika 10 - izlaz PrepareSampleDataPipeline*

### <a name="references"></a>Reference

[Azure SDK događaj koncentrator za strujanje ingestion](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure podataka tvorničke mogućnosti premještanja podataka](../data-factory/data-factory-data-movement-activities.md)
[Azure podataka tvorničke DotNet aktivnosti](../data-factory/data-factory-use-custom-activities.md)

[Azure podataka tvorničke DotNet aktivnosti visual studio rješenje za Priprema oglednih podataka](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Particija skup podataka

Signale neobrađenog vozila djelomično strukturirani i skup dijagnostičkih podataka imaju se u koraku Priprema podataka u oblik godine/MJESEC. U ovom particija promiče učinkovitiji upita i skalabilni dugoročno spremanje omogućivanjem kvara pokazivač s blob jednog računa na sljedeću kao prvi račun puni. 

>[AZURE.NOTE] U ovom koraku u rješenje je odnosi se samo na skupna obrada.

Ulazni i izlazni podataka upravljanje podacima:

- **Izlazne podatke** (označen *PartitionedCarEventsTable*) će biti zadržane dugo vrijeme kao foundational / "rawest" obrasca podataka u u klijentovoj "podataka Lake". 
- **Ulaznih podataka** za ovaj kanal se obično želite odbaciti kao podatke sadrži potpunoj za unos – samo sprema se (particije) bolje za kasnije korištenje.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Slika 11 – particija automobila događaje tijeka rada*

Sirovim podacima particije je korištenje vrste Hive HDInsight aktivnosti u "PartitionCarEventsPipeline". Ogledni podaci za godinu generira u koraku 1 je particije po GODINI i MJESECU. Particije se koriste za generiranje signale vozila i dijagnostičkih podataka za svaki mjesec (ukupno 12 particije) godine. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Slika 12 - PartitionCarEventsPipeline*

Sljedeću skriptu grozd pod nazivom "partitioncarevents.hql" koristi se za particija i nalazi se u mapi "\demo\src\connectedcar\scripts" preuzete zip. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Slika 13 - PartitionConnectedCarEvents grozd skripte*

Kada je kanal uspješno izvrši, vidjet ćete sljedeće particije generira na vašem računu za pohranu u kontejneru "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Slika 14 - particioniranom Izlaz*

Podatke sada optimiziran je, nije moguće upravljati i jeste li spremni za daljnje obrade da bi se dobio uvida obogaćenog grupe. 

## <a name="data-analysis"></a>Analiza podataka

U ovom odjeljku pogledajte kako da biste spojili Azure strujanje analize, Azure strojnog učenja, tvorničke Azure podataka i Azure HDInsight za obogaćeni napredne analize na stanje vozila i uputa za vožnju navike. Postoje tri Pododlomci koji ovdje:

1.  **Strojnog učenja**: Ova Podsekcija sadrži informacije o pokusa značajkom prepoznavanja koji smo koristili ovog rješenja za predviđanje vozila koje je obavezna održavanje održavanja i vozila koje je obavezna opoziv zbog problema s sigurnost.
2.  **U stvarnom vremenu analize**: Ova Podsekcija sadrži podatak u stvarnom vremenu analize pomoću jezika upita strujanje analize i operationalizing pokusa strojnog učenja u stvarnom vremenu pomoću prilagođene aplikacije.
3.  **Obrade analize**: ovaj Podsekcija sadrži informacije odnose na Pretvorba i obrada obradu podataka pomoću servisa Azure HDInsight i Azure strojnog učenja operationalized po tvorničke Azure podataka.

### <a name="machine-learning"></a>Upoznavanje s računala

Naš cilj je za predviđanje vozila koji zahtijeva održavanja ili opoziv na temelju određenih heath Statistika. Dajemo sljedeće pretpostavke

- Ako su zadovoljena jedan od sljedeća tri uvjeta, u vozila obavezan **Održavanje održavanja**učinite sljedeće:
    - Nedostaje guma tlaka
    - Modul ULJE razinu nedostaje
    - Modul temperature je dovoljno Jaka

- Ako su ispunjeni neki od sljedećih uvjeta, u vozila možda ste **Sigurnost problem** i potrebna **opoziv**:
    - Modul temperatura je visoka, ali izvan temperatura nedostaje
    - Modul temperatura nedostaje, ali je izvan temperatura visoke

Na temelju prethodne preduvjeti stvorili smo dva zasebna modela da biste otkrili anomalies, jedan za otkrivanje održavanja vozila i jedan za otkrivanje opoziv vozila. U oba te modelima algoritam ugrađene glavnice komponente Analysis (PCA) koristi se za otkrivanje značajkom. 

**Održavanje otkrivanje modela**

Ako neku od tri pokazatelja - tlaka guma, modul ULJE ili modul temperatura - zadovoljava njegov odgovarajući uvjet, otkrivanje modela održavanja izvješća programa značajkom. Kao rezultat ćemo samo treba uzeti u obzir sljedeće tri varijable u modelu. U našem eksperiment Azure strojnog učenja najprije koristimo modula **Odabir stupaca u skupu podataka** da biste izdvojili ove tri varijable. Sljedeće modul za otkrivanje utemeljen na PCA značajkom koristimo za izradu modela za otkrivanje značajkom. 

Uobičajene tehnika u strojnog učenja koje se mogu primijeniti odabir značajki, klasifikaciju i značajkom prepoznavanja je glavnice komponente Analysis (PCA). PCA pretvara skup koji sadrži vjerojatno povezanog varijable u skupu vrijednosti koje se nazivaju glavni komponente. Ključni ideju utemeljen na PCA Modeliranje je projektnih podataka na donjem dimenzionalni prostor tako da se značajke i anomalies se može lakše identificirati.
 
Za svaki novi unos u model otkrivanje otkrivatelja značajkom najprije formula izračunava njegov projekciju na na eigenvectors, a zatim formula izračunava normaliziranu reconstruction pogreške. Ta se pogreška normaliziranu je rezultat značajkom. Veći pogreške, anomalous više instanci je. 

U otkrivanje problema održavanja svaki zapis možete smatrati točku 3 dimenzionalni prostor definiranog tlaka guma, engine ULJE i temperatura modul koordinata. Da biste snimili te anomalies, ne možemo možete projekta izvorne podatke u 3 dimenzionalni prostor na 2 dimenzionalni razmak pomoću PCA. Dakle, ne možemo postaviti parametar broj komponenti za korištenje u PCA da je 2. Ovaj parametar reproducira važnu ulogu u primjene utemeljen na PCA značajkom prepoznavanja. Nakon predviđanju podataka pomoću PCA, ne možemo jednostavnije prepoznavanje te anomalies.

**Opoziv značajkom prepoznavanja modela** U tom modelu za otkrivanje značajkom opoziv koristimo odabir stupaca u skupu podataka i sustavom PCA značajkom moduli za prepoznavanje na sličan način. Konkretno, ne možemo najprije izdvojite tri varijable - temperatura modul, vanjski temperature i brzina - modul **Odabir stupaca u skupu podataka** . Ne možemo uvrštavati varijabla brzinu jer temperatura modul obično se povezuju na brzinu. Sljedeće modul za otkrivanje utemeljen na PCA značajkom koristimo za projektnih podataka iz 3 dimenzionalni prostora na 2 dimenzionalni razmak. Kriterij opoziv su zadovoljena, a tako vozila zahtijeva opoziv kada modul temperature i vanjski temperatura vrlo negativno povezanog. Pomoću algoritam prepoznavanja utemeljen na PCA značajkom, ne možemo možete snimiti na anomalies nakon izvršavanja PCA. 

Kada je obuka ili model, moramo normalni podatke koji ne zahtijeva održavanje ili opoziv kao ulaznih podataka za uvježbati otkrivanje modela utemeljen na PCA značajkom. U bilježenja rezultata pokusa koristimo otkrivanje modela obučeni značajkom da biste otkrili hoće li vozila zahtijeva održavanje ili opoziv. 


### <a name="real-time-analysis"></a>U stvarnom vremenu analize

Sljedeći upit SQL analize strujanje koristi se za dohvaćanje prosjek sve parametre važne vozila poput vozila brzinu, goriva razine, temperatura modul, brojač kilometara čitanje, guma tlaka, modul ULJE razine i drugim korisnicima. U prosjeke koriste se za otkrivanje anomalies, problema upozorenja i određivanje cjelokupan stanja uvjetima vozila kojim se upravlja u određenu regiju i pa je povezivanje demografskih podataka. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Slika 15 – strujanje analize upita za obradu u stvarnom vremenu

Sve prosjeci izračunavaju se putem 3 druge TumblingWindow. Ne možemo koriste TubmlingWindow u tom slučaju jer tražimo koje se ne preklapaju i neprekinute vremenske intervale. 

Da biste saznali više o svim mogućnostima "prikaz u prozorima" Azure strujanje analize, kliknite [Prikaz u prozorima (Azure strujanje analize)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Predviđanje u stvarnom vremenu**

Aplikacija je dio rješenje operationalize modela strojnog učenja u stvarnom vremenu. Ove aplikacije pod nazivom "RealTimeDashboardApp" se stvara i konfigurirati kao dio implementacije rješenja. Aplikacija izvršava sljedeće:

1.  Očekuje podatke instancu događaj koncentrator gdje strujanje analize objavljuje događaje u uzorku kontinuirano. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Slika 16 – strujanje analize upit za objavljivanje podataka za izlaz instancu koncentratora za događaj* 

2.  Za svaki događaj koji prima ovu aplikaciju: 

    - Obrađuje podataka pomoću strojnog učenja odgovor na zahtjev bilježenje rezultata (RRS) krajnjoj točki. Krajnja točka RRS automatski objavljuju kao dio uvođenje.
    - Izlaz RRS se objavljuje pomoću automatske API-ji PowerBI skup podataka.

Ovaj uzorak je dodijeljen scenariji u kojima želite integrirati redak tvrtke (LoB) aplikacije tokove u stvarnom vremenu analize scenarije kao što su upozorenja, obavijesti i poruka.

Kliknite [Preuzmi RealtimeDashboardApp](http://go.microsoft.com/fwlink/?LinkId=717078) da biste preuzeli RealtimeDashboardApp Visual Studio rješenje za prilagodbe. 

**Za izvođenje aplikacije nadzorne ploče u stvarnom vremenu**

1.  Kliknite čvor PowerBI u prikazu dijagrama, a zatim kliknite vezu "Preuzimanje u stvarnom vremenu nadzorne ploče aplikacije" u oknu svojstva. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Slika 17 – upute za postavljanje PowerBI nadzorne ploče*
2.  Izdvajanje i spremiti lokalno ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *slici 18 – RealtimeDashboardApp mape*
3.  Izvršavanje aplikacije RealtimeDashboardApp.exe
4.  Unesite vjerodajnice za valjane Power BI, prijavite se i kliknite Prihvati ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Slika 19 – RealtimeDashboardApp: Prijava na PowerBI*

>[AZURE.NOTE] Ako želite pražnjenje PowerBI dataset izvršavanje RealtimeDashboardApp s parametrom "flushdata": 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Grupe za analizu

Cilj ovdje je da biste prikazali kako Contoso Motors koristi Azure računalnim mogućnosti za analitička velikih skupova podataka da bi se dobio obogaćenog uvida na vožnju uzorak, korištenje ponašanje i vozila stanja. Time se omogućuje:

- Poboljšanja iskustva kupca i njezino jeftinijim unosom uvida na vožnju navike i ponašanja učinkovitog vožnju goriva
- Upoznavanje s doći Kupci i njihovih vožnju patters upravljaju poslovnih odluka i dati preporučeni u predmete proizvoda i usluga

U ovom rješenju ćemo birate metriku sljedeće:

1.  **Snažna postavka vožnju ponašanje**: služi za identifikaciju trend modela, mjesta, vožnju uvjeta, i vremena u godini da bi se dobio uvida na izrazito vožnju uzoraka. Contoso Motors možete koristiti te uvida marketinške kampanje vožnju personalizirane koje su nove značajke i utemeljen na korištenje osiguranja.
2.  **Ponašanje učinkovitog vožnju goriva**: služi za identifikaciju trend modela, mjesta, vožnju uvjeta, i vremena u godini da bi se dobio uvida na goriva učinkovitog vožnju uzoraka. Contoso Motors pomoću ove uvida za marketinške kampanje, vožnju nove značajke i određene proaktivne izvješćivanje upravljačke programe za cijena vrijedi i okruženja navike neslužbeni vožnju. 
3.  **Opoziv modela**: služi za identifikaciju modela koji zahtijevaju opoziv tako da se značajkom prepoznavanja strojnog učenja eksperiment operationalizing

Pogledajmo u pojedinosti svake od ovih mjerenja


**Snažna postavka vožnju uzorak**

U kanalu pod nazivom "AggresiveDrivingPatternPipeline" pomoću grozd da biste odredili u modelima, mjesto, vozila, vožnju uvjete i drugi parametri koji prikazuje izrazito vožnju uzorak se obrađuju signale particioniranom vozila i dijagnostičkih podataka.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Slika 20 – Aggressive vožnju uzorak tijeka rada*

Skripta grozd pod nazivom "aggresivedriving.hql" koristi za analizu izrazito vožnju uvjet uzorak nalazi se na mapu "\demo\src\connectedcar\scripts" preuzete zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Slika 21 – Aggressive vožnju uzorak grozd upita*

Koristi kombinacije položaj brzine prijenosa, kočnica papučicom stanja i brzinu vozila, da biste otkrili reckless/izrazito vožnju ponašanje na temelju braking uzorak velikom brzinom. 

Kada je kanal uspješno izvrši, vidjet ćete sljedeće particije generira na vašem računu za pohranu u kontejneru "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Slika 22 – AggressiveDrivingPatternPipeline Izlaz*


**Uzorak učinkovitog vožnju goriva**

U kanalu pod nazivom "FuelEfficientDrivingPatternPipeline" se obrađuju signale particioniranom vozila i dijagnostičkih podataka. Grozd koristi se za određivanje u modelima, mjesto, vozila, vožnju uvjete i druga svojstva koji se ponašaju goriva učinkovitog vožnju uzorka.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Slika 23 – goriva učinkovitog vožnju uzorak tijeka rada*

Skripta grozd pod nazivom "fuelefficientdriving.hql" koristi za analizu izrazito vožnju uvjet uzorak nalazi se na mapu "\demo\src\connectedcar\scripts" preuzete zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Slika 24 – goriva učinkovitog vožnju uzorak grozd upita*

Koristi kombinaciju vozila, prijenos zupčanika položaj, kočnica papučicom status, brzinu i ubrzivača Papučica položaj da biste otkrili goriva učinkovitog vožnju ponašanje ubrzanja, braking, na temelju i brzinu uzoraka. 

Kada je kanal uspješno izvrši, vidjet ćete sljedeće particije generira na vašem računu za pohranu u kontejneru "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Slika 25 – FuelEfficientDrivingPatternPipeline Izlaz*


**Opoziv predviđanja**

Strojnog učenja eksperiment je dodjeli i objavili kao web-servisa kao dio implementacije rješenja. Grupe za bilježenje rezultata krajnju točku je leveraged u ovaj tijek rada registrirana kao podatkovnog servisa tvorničke povezana i operationalized pomoću podataka tvorničke obradu bilježenje rezultata aktivnosti.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Slika 26 – strojnog učenja krajnja točka registrirana kao povezane servisa u tvorničke podataka*

Registrirani povezane servis koristi u na DetectAnomalyPipeline da bi rezultat podataka pomoću modela značajkom prepoznavanja. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Slika 27 – Azure strojnog učenja obradu bilježenje rezultata aktivnosti u tvorničke podataka* 

Postoji nekoliko koraka koji se izvode u ovom kanalu za pripremu podataka tako da ga možete operationalized pomoću obrade bilježenje rezultata web-servisa. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Slika 28 – DetectAnomalyPipeline za procjenjivanje vozila koje je obavezna opoziv* 

Kada u bilježenje rezultata dovrši, aktivnost HDInsight koristi se za obradu i prikupljati podatke koji su kategorizirana kao anomalies model s rezultatom vjerojatnost 0.60 ili noviji.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Kada je kanal uspješno izvrši, vidjet ćete sljedeće particije generira na vašem računu za pohranu u kontejneru "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Slika 30 – 30 slika – DetectAnomalyPipeline Izlaz*


## <a name="publish"></a>Objavljivanje

### <a name="real-time-analysis"></a>U stvarnom vremenu analize

Jedan od upita u posao analize strujanje objavljuje događaje u izlaz instancu koncentratora za događaj. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Slika 31 – strujanje analize posao objavljuje na programa izlaz instancu koncentratora za događaj*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Slika 32 – strujanje analize upit da biste objavili na Izlaz instancu koncentratora za događaj*

Ovaj tok događaja koriste RealTimeDashboardApp obuhvaćene rješenjem. Ovu aplikaciju upravlja web-servis strojnog učenja-odgovor na zahtjev za u stvarnom vremenu bilježenje rezultata i objavljuje konačni podataka za skup podataka PowerBI za potrošnju. 

### <a name="batch-analysis"></a>Grupe za analizu

Rezultate grupe, a u stvarnom vremenu obrada se objavljuju u tablicama baze podataka SQL Azure za potrošnju. Azure SQL Server, baze podataka i tablice automatski se stvaraju kao dio skripta za instalaciju. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Slika 33 – skupna obrada Kopiraj rezultate tijek rada čavanje podataka*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Slika 34 – strujanje analize posao objavljuje na čavanje podataka*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Slika 35 – čavanje podataka u strujanje analize posla*


## <a name="consume"></a>Korištenje

Power BI daje rješenja obogaćenog nadzorne ploče za podatke u stvarnom vremenu i vizualizacije predvidljivu analize. 

Kliknite ovdje da biste detaljne upute za postavljanje PowerBI izvješća i nadzorne ploče. Konačni nadzorne ploče izgleda ovako:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Slika 36 - PowerBI nadzorne ploče*

## <a name="summary"></a>Sažetak

Ovaj dokument sadrži na detaljne naniže analize rješenja za Telemetriju vozila. To showcases lambda arhitektura uzorak u stvarnom vremenu i obrade analize s predviđanja i akcije. Ovaj uzorak odnosi se na širokog raspona koristite slučajeva koje je potrebno tipkovni put (u stvarnom vremenu) i analitiku Hladna put (grupe). 
