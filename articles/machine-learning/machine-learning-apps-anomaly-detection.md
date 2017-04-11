<properties 
    pageTitle="Strojnog učenja aplikacije: značajkom prepoznavanja servisa | Microsoft Azure" 
    description="Primjer načinjene pomoću Microsoft Azure strojnog učenja koji otkrije anomalies u vrijeme niza podataka pomoću brojčanih vrijednosti koje su jednoliko raspoređene u vremenu je značajkom prepoznavanja API-JA." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Učenje značajkom prepoznavanja usluge#

##<a name="overview"></a>Pregled

Primjer načinjene pomoću Azure strojnog učenja koji otkrije anomalies u vrijeme niza podataka pomoću brojčanih vrijednosti koje su jednoliko raspoređene u vremenu je [Značajkom prepoznavanja API -JA](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) . 

Taj API može prepoznati sljedeće vrste anomalous uzoraka u vrijeme niza podataka:

* **Pozitivne i negativne trendova**:, na primjer, kada nadzor memorije u računalstvu prema gore trend možda interesa kao što je možda indikativnih od curenje memorije

* **Promjene u dinamičku raspon vrijednosti**:, na primjer, kada nadzor iznimke je izbacio servis u oblaku, sve promjene u dinamičku raspon vrijednosti ukazuje nestabilnost u stanju servisa, a

* **Krivina i Dips**:, na primjer, kada nadzor broj neuspjelih Prijava na servis ili broj Odjava odjedanput u web-mjesto sustava e-trgovine, krivina ili dips može upućivati neuobičajenog ponašanje.

Ove detectors strojnog učenja pratite takve promjene u vrijednostima tijekom vremena i izvješće daljnje promjene u njihove vrijednosti kao rezultati značajkom. Ne trebaju ad hoc prag usklađivanje i njihove rezultate koje se mogu koristiti da biste odredili false pozitivan stopa. Otkrivanje značajkom API je korisno u nekoliko scenarija poput servisa nadzor praćenjem KPI-ja s vremenom korištenje nadzor kroz metriku kao što su broj pretraživanja, broj klikova praćenje performansi kroz mjerača kao što su memorije, procesora, čita datoteku, itd tijekom vremena.

U sklopu nuditi značajkom prepoznavanja korisne alate za početak rada. 

* [Web-aplikacije](http://anomalydetection-aml.azurewebsites.net/) pomaže vam procijeniti i vizualizacija rezultate značajkom prepoznavanja API-ji nad podacima. 

* [Ogledni kod](http://adresultparser.codeplex.com/) prikazuje kako programatski pristup s API-JA i raščlaniti rezultate u C#.

>[AZURE.NOTE]
>Pokušajte **uvida značajkom IT rješenja** pokreće [ovaj API-JA](https://datamarket.azure.com/dataset/aml_labs/anomalydetection)
>
>Da biste dobili rješenje end da biste završetka implementiran u pretplatu za Azure <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **, počnite ovdje >**</a>


##<a name="api-definition"></a>Definicija API-JA

Usluga omogućuje i utemeljen na REST API putem HTTP koji se može trošiti na različite načine uključujući weba ili mobilnu aplikaciju, R, Python, Excel, itd.  Niz podataka vrijeme poslati taj servis putem poziva REST API-JA, a pokrene kombinaciju gore opisanih vrsta tri značajkom. Pokreće servis platforme Azure strojnog učenja koji mijenja veličinu za svoje poslovne potrebe jednostavno i njihovi SLA.

###<a name="headers"></a>Zaglavlja
Provjerite je li uključiti točan zaglavlja u zahtjevu za koje treba izgledati ovako:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Ključ za račun s računa možete pronaći u [Azure Data Market](https://datamarket.azure.com/account/keys). 

###<a name="score-api"></a>Rezultat API-JA

API rezultat će se koristiti radi otkrivanja značajkom na podataka koje nisu sezonski vremena. U API izvodi broj značajkom detectors na podacima i vraća njihove rezultate značajkom. Na slici u nastavku prikazuje se primjer anomalies koji može otkriti API rezultat. U ovom vremenski niz ima 2 distinct promjene na razini i 3 krivina. Crvena točke prikazuje vrijeme na kojoj se razine promjena je otkriven, dok crni točke prikaz otkriven krivina.

![Rezultat API-JA][1]
    
**URL-A**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Primjer zahtjev tijelo**

U zahtjevu za ispod šaljemo vrijeme niza (prikazanom odrezana) s točaka podataka od 3/1/2016 do 3/10/2016 i parametre šiljka detectors API-JA za prepoznavanje ako su neki od ove točke podataka anomalous. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Odgovor na to će biti: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API-JA

ScoreWithSeasonality API koristi se za otkrivanje značajkom sustavom vremenski niz koji imaju sezonski uzoraka. U ovom API koristan je da biste otkrili odstupanja u sezonski uzoraka.  

Na sljedećoj slici prikazani Primjeri anomalies u nizu sezonski vremena. Vremenski niz sadrži jedan šiljka (1st crna točka), dva dips (2 crna točka i na kraju) i jednu razinu promjenu (crveno točka). Imajte na umu da i na dip sredini vremenski niz i promjena razina samo discernable nakon sezonski komponente uklanjaju se iz serije.

![Sezonalnost API-JA][2]

**URL-A**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Primjer zahtjev tijelo**

U zahtjevu za ispod šaljemo vrijeme niz (prikazanom odrezana) s točaka podataka od 3/1/2016 do 3/10/2016 i parametrima API-JA za prepoznavanje ako su neki od ove točke podataka anomalous. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Odgovor na to će biti: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Detectors

Otkrivanje značajkom API-JA ne podržava detectors u 3 široke kategorije. Detalje o određenim ulazne parametre i izlaza za svaki otkrivatelja pronaći ćete u tablici u nastavku.

|Kategorija otkrivatelja|Otkrivatelja|Opis|Ulaznih parametara|Izlaze
|---|---|---|---|---|
|Detectors šiljka|TSpike otkrivatelja|Otkrivanje su krivina i dips koji se temelji na kraju vrijednosti iz prvog i treće Kvartili|*tspikedetector.sensitivity:* vodi cjelobrojnu vrijednost u rasponu od 1-10, zadani: 3; Veće vrijednosti će Uhvatite zato što manje osjetljive više ekstremne vrijednosti|TSpike: Binarni vrijednosti – "1" Ako je otkriven šiljka/dip, "0" neki drugi način|
||ZSpike otkrivatelja|Otkrivanje krivina i dips na temelju do na datapoints su od svojih srednjih vrijednosti.|*zspikedetector.sensitivity:* poduzeti cjelobrojnu vrijednost u rasponu od 1-10, zadani: 3; Veće vrijednosti će Uhvatite upućivanje manje osjetljive više ekstremne vrijednosti|ZSpike: Binarni vrijednosti – "1" Ako je otkriven šiljka/dip, "0" neki drugi način|
|Otkrivatelja sporo Trend|Otkrivatelja sporo Trend|Otkrivanje sporo pozitivan trend po skupu osjetljivost|*trenddetector.sensitivity:* prag na otkrivatelja rezultat (zadani: 3,25, 3,25 – 5 je pametnije raspon da biste odabrali to iz; Veći na manje osjetljivih)|TScore: pluta broj koji predstavlja značajkom rezultat na trenda.|
|Promjena razine Detectors|Unidirectional razine promjena otkrivatelja|Otkrivanje razinu prema gore promjena u skladu sa osjetljivost skup|*upleveldetector.sensitivity:* prag na otkrivatelja rezultat (zadani: 3,25, 3,25 – 5 je pametnije raspon da biste odabrali to iz; Veći na manje osjetljivih)|PScore: pluta broj koji predstavlja značajkom rezultat na prema gore na razini promjena|
||Dvosmjeran razinu promjena otkrivatelja|Otkrivanje razine promjena prema gore i prema dolje po skupu osjetljivost|*bileveldetector.sensitivity:* prag na otkrivatelja rezultat (zadani: 3,25, 3,25 – 5 je pametnije raspon da biste odabrali to iz; Veći na manje osjetljivih)|RPScore: broj koji predstavlja značajkom rezultat na prema gore i dolje razinu promijenili pomičnim zarezom

###<a name="parameters"></a>Parametri

Detaljnije informacije o te ulaznih parametara nalazi se u tablici u nastavku:

|Ulaznih parametara|Opis|Zadana postavka|Vrsta|Valjani raspon|Predloženi raspon|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Interval zbrajanja u sekundama za zbrajanje unos vremenski niz|0 (provodi Zbrajanje nije)|cijeli broj|0: inače preskočite zbrajanja, > 0|pet minuta 1 dan, ovisno o vremenski niz
|preprocess.aggregationFunc|Funkciju koja se koristi za zbrajanje podataka u navedenom AggregationInterval|srednje vrijednosti|videouređaja|Srednja, sum, duljine|N/D|
|preprocess.replaceMissing|Vrijednosti koje se koriste za impute podatke koji nedostaju|lkv (Zadnja poznata i vrijednost)|videouređaja|nula, lkv, srednja vrijednost|N/D|
|detectors.historyWindow|Povijest (u broj točaka podataka) koristiti za izračunavanje značajkom rezultat|500|cijeli broj|10 2000|Zavisne vremenski niz|
|upleveldetector.sensitivity|Osjetljivost za prema gore razinu promijenili otkrivatelja. |3,25|Dvostruki|Ništa|3,25 5(Lesser values mean more sensitive)|
|bileveldetector.sensitivity|Osjetljivost za dvosmjerni razinu promijenili otkrivatelja. |3,25|Dvostruki|Ništa|3,25 5(Lesser values mean more sensitive)|
|trenddetector.sensitivity|Osjetljivost za pozitivne trend otkrivatelja. |3,25|Dvostruki|Ništa|3,25 5(Lesser values mean more sensitive)|
|tspikedetector.sensitivity |Osjetljivost za TSpike otkrivatelja|3|cijeli broj|1 – 10|3 5(Lesser values mean more sensitive)|
|zspikedetector.sensitivity |Osjetljivost za ZSpike otkrivatelja|3|cijeli broj|1 – 10 |3 5(Lesser values mean more sensitive)|
|seasonality.Enable|Je li sezonalnost analizu koje je potrebno izvršiti|TRUE|Booleove vrijednosti|TRUE, false|Zavisne vremenski niz|
|seasonality.numSeasonality |Maksimalni broj periodičku ciklusa da biste se provjeravati|1|cijeli broj|1, 2|1-2|
|seasonality.TRANSFORM |Hoće li je sezonski (i) trend komponente moraju biti uklonjen prije primjene značajkom prepoznavanja|deseason|videouređaja|ništa, deseason, deseasontrend|N/D|
|postprocess.tailRows |Broj najnovije točaka za ostane u rezultatima Izlaz|0|cijeli broj|0 (ostati svim točkama podataka), ili navedite broj točaka da bi bili rezultata|N/D|

###<a name="output"></a>Izlaz
U API pokreće sve detectors na vrijeme niz podataka, a vraća rezultate značajkom i pokazatelje binarni šiljka za svaku točku u vremenu. U tablici u nastavku navedeni izlaza iz API Sučelja. 

|Izlaze|Opis|
|---|---|
|Vrijeme|Vremenske oznake iz sirovim podacima ili zbroja (i/ili) imputed podataka ako zbrajanja (i/ili) nedostaje primjenjuje imputation podataka|
|OriginalData|Vrijednosti iz neobrađenog podatke ili podatke Zbrojeno (i/ili) imputed ako zbrajanja (i/ili) nedostaje primjenjuje imputation podataka|
|ProcessedData|Nešto od sljedećeg: <ul><li>Seasonally prilagođavaju vremenski niz ako značajan sezonalnosti otkrivena i deseason odabir mogućnosti;</li><li>seasonally prilagođene i Oscilator vremenski niz ako otkrio značajan sezonalnost i odabranom mogućnošću deseasontrend</li><li>u suprotnom, to je jednako OriginalData</li>|
|TSpike|Binarni pokazatelj koji određuju hoće li u šiljak otkrio TSpike otkrivatelja|
|ZSpike|Binarni pokazatelj koji određuju hoće li u šiljak otkrio ZSpike otkrivatelja|
|Pscore|Plutajući broj koji predstavlja značajkom rezultat na razinu prema gore promjena|
|Palert|vrijednost 1/0 kojoj stoji da nema razinu prema gore promijeniti značajkom koji se temelji na unos osjetljivosti|
|RPScore|Plutajući broj representing značajkom rezultatu Dvosmjeran razine promjena|
|RPAlert|vrijednost 1/0 kojoj stoji da nema razinu Dvosmjeran promijeniti značajkom koji se temelji na unos osjetljivosti|
|TScore|Plutajući broj representing značajkom rezultatu na pozitivne trend|
|TAlert|vrijednost 1/0 označava da nema je pozitivan trend značajkom na temelju unos osjetljivosti|


Ovaj izlaz možete mogu raščlaniti pomoću [jednostavne analizator](https://adresultparser.codeplex.com/) – sadrži ogledni kod koji pokazuje kako se povezati s API-JA i analizirati izlaz. Anomalies otkriven mogu vizualizirati na nadzornoj ploči i/ili proslijeđen Ljudski stručnjaka za korektivne akcije ili integriranom izdavanje ulaznica sustave.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
