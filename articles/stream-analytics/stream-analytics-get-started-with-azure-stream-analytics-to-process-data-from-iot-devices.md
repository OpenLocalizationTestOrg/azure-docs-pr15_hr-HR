<properties
    pageTitle="Početak rada s Azure strujanje analize podataka procesa s IoT uređaja. | Microsoft Azure"
    description="IoT senzor oznake i podatke strujanja s strujanje analize i u stvarnom vremenu obrada podataka"
    keywords="rješenje iot početak rada s iot"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Početak rada s Azure strujanje analize postupak podatke s uređaja IoT

U ovom ćete praktičnom vodiču će Saznajte kako stvoriti strujanje obrada logike Prikupite podatke s uređaja Internet stvari (IoT). Koristit ćemo stvarnog života Internet stvari (IoT) koristi slučaja da bismo pokazali kako izraditi rješenje brzo i economically.

## <a name="prerequisites"></a>Preduvjeti

-   [Azure pretplate](https://azure.microsoft.com/pricing/free-trial/)
-   Ogledna upita i podatkovne datoteke koje je moguće preuzeti iz [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Scenarij

Contoso koja je u tvrtki u prostoru industrijske Automatizacija, sadrži potpuno automatski njegov procesa proizvodnje. Strojeva u ovom biljke ima senzori mogu emitira strujanja podataka u stvarnom vremenu. U ovom scenariju floor upravitelja radnog želi uvida u stvarnom vremenu iz podataka senzor za traženje uzoraka i poduzeti na njima. Koristit ćemo strujanje analize upita jezik (SAQL) nad podacima senzor za pronalaženje zanimljivih uzoraka iz dolazne toka podataka.

Ovdje podataka se generira s uređaja Texas instrumenti senzor oznaka.

![Oznaka senzor Texas instrumenti](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Opseg podataka u obliku JSON i izgleda ovako:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

U scenariju s stvarnog života može imati stotine te senzori generiranje događaje kao tok. Najbolje pristupni uređaj želite pokrenuti kod za automatske te događaje [Koncentratora Azure događaj](https://azure.microsoft.com/services/event-hubs/) ili [Azure IoT koncentratora](https://azure.microsoft.com/services/iot-hub/). Vaš posao strujanje analize bi ingest te događaje iz koncentratora za događaj i pokretanje upita u stvarnom vremenu analize protiv strujanja. Nakon toga nije poslali rezultate na neki od [podržane izlaza](stream-analytics-define-outputs.md).

Radi jednostavnog korištenja vodiču za dohvaćanje rada nudi datoteku s oglednim podacima, koji je zabilježene realni senzor oznaka uređaja. Možete izvoditi upite na ogledne podatke i pogledajte rezultate. U sljedećim vodičima ćete naučiti kako povezati posla unosa i Proizvodi i uvesti ih sa servisom Azure.

## <a name="create-a-stream-analytics-job"></a>Stvaranje zadatka strujanje Analytics

1. [Azure portal](http://portal.azure.com)kliknite znak plus i upišite **Strujanje ANALIZE** u prozoru tekst desno. Zatim odaberite **strujanje analize zadatak** na popisu rezultata.

    ![Stvaranje novog zadatka strujanje analize](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Unesite naziv jedinstveni posla, a zatim provjerite je li pretplata onaj za svoj posao. Zatim stvorite novu grupu resursa ili odaberite postojeći pretplate na.

3. Zatim odaberite mjesto za svoj posao. Brzina obrade i smanjivanje troškova podataka preporučuje se prijenos odabirom na isto mjesto kao grupu resursa i račun svrhu prostora za pohranu.

    ![Stvorite novi zadatak analize strujanje pojedinosti](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] Potrebno je stvoriti ovog prostora za pohranu računa samo jedanput po regijama. U ovom prostora za pohranu će zajedničke sve analize strujanje zadatke koje stvorite u tom području.

4. Potvrdite okvir da biste postavili posla na nadzornu ploču, a zatim kliknite **Stvori**.

    ![Stvaranje zadatka u tijeku](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Trebali biste vidjeti "Implementacija rada..." prikazati u gornjem desnom kutu prozora preglednika. Uskoro on će se promijeniti dovršene prozor kao što je prikazano u nastavku.

    ![Stvaranje zadatka u tijeku](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Stvaranje upita s Azure strujanje Analytics

Nakon stvaranja posla vrijeme je da biste je otvorili i sastavljanje upita. Vaš posao možete jednostavno pristupiti tako da kliknete pločicu za njega.

![Pločica posla](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

U oknu **Zadatka topologije** kliknite okvir za **UPITE** da biste prešli u uređivač upita. Uređivač **UPITA** omogućuje vam da biste unijeli T SQL upit koji se izvodi transformaciju putem dolazne podaci o događaju.

![Okvir upita](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Upit: Arhiviranje sirovim podacima

Najjednostavniji oblik upita je prolazni upit koji arhivira ulaznih podataka za određenu rezultat. Preuzimanje datoteke s oglednim podacima iz [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) na mjesto na računalu. 

1. Zalijepite upit iz datoteke PassThrough.txt. 

    ![Testiranje ulaznog toka](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Kliknite tri točke pokraj unos, a zatim potvrdite okvir **Prenesi ogledne podatke iz datoteke** .

    ![Testiranje ulaznog toka](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Okno otvorit će se na desnoj strani kao rezultat, u ga odaberite podatkovnu datoteku HelloWorldASA InputStream.json preuzete lokacije i kliknite **u redu** pri dnu okna.

    ![Testiranje ulaznog toka](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Zatim kliknite **testiranje** zupčanika u gornjem lijevom kutu prozora i obraditi test upit na temelju uzorka skupu podataka. Prozor rezultati će se otvoriti ispod upit kao obrada ne dovrši.

    ![Testiranje rezultata](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Upit: Filtriranje podataka na temelju uvjeta

Pokušajmo radi filtriranja rezultata na temelju uvjeta. Želimo za prikaz rezultata za samo događaje koje dolazi iz "sensorA." Upit je u datoteci Filtering.txt.

![Filtriranje podataka strujanje](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Imajte na umu da se velika i mala slova upita uspoređuje vrijednost niza. Kliknite **Test** zupčanika ponovno dopušta izvršavanje upita. Upit mora vratiti 389 redaka iz 1860 događaja.

![Drugi izlazni rezultat testiranje upita](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Upit: Upozorenja za pokretanje tijeka rada za tvrtke

Pogledajmo naš upita provjerite detaljnije. Za svaku vrstu senzor želimo praćenje Prosječna temperatura po 30 sekundi prozor i prikaz rezultata samo ako je prosječna temperatura veća od 100 stupnjeva. Ne možemo će pisanje sljedeći upit i kliknite **Testiraj** da biste vidjeli rezultate. Upit je u datoteci ThresholdAlerting.txt.

![Filtar 30 sekundi upita](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Sada vidjet ćete rezultate koji sadrže samo 245 redaka i imena senzori kojima je prosjek temperate veća od 100. Ovaj upit grupira strujanje događaja po **dspl**, što je naziv senzor putem **Tumbling prozora** 30 sekundi. Vremenski upita mora navedite kako želimo vrijeme u tijeku. Korištenjem uvjeta **Vremenske oznake tako da** ne možemo ste naveli stupcu **OUTPUTTIME** želite pridružiti puta sve vremenski izračune. Detaljne informacije potražite u članku MSDN članke vezane uz [Upravljanje vremenom](https://msdn.microsoft.com/library/azure/mt582045.aspx) i [Prikaz u prozorima funkcije](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Upit: Otkriti Izostanak događaja

Kako ćemo pisanje upit da biste pronašli nedostatka unos događaja? Pogledajmo pronađite zadnji put senzora poslati podatke, a zatim nije poslao događaje sljedeći min. Upit je u datoteci AbsenseOfEvent.txt.

![Otkrivanje Izostanak događaja](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Ovdje koristimo u **Lijevom VANJSKI** spoj isti tijeku podataka (koja se sama pridružiti). Samo kada se pronađe podudarajući kontakt za **UNUTARNJI** spoj, vraća se rezultat  Za **Lijevo VANJSKI** spoj, ako je događaj s lijeve strane spoja neuparenih, vraća se redak koji sadrži vrijednost NULL za sve stupce s desne strane. Ovu tehniku vrlo je praktična da biste pronašli za izostanak događaja. Pogledajte naš MSDN dokumentaciju za dodatne informacije o [PRIDRUŽI se](https://msdn.microsoft.com/library/azure/dn835026.aspx).

## <a name="conclusion"></a>Zaključak

Svrha ovog praktičnog vodiča je da bismo pokazali kako pisati različite strujanje analize Query Language upite i pogledajte rezultate u web-pregledniku. No to se tek ste počeli raditi. To možete učiniti toliko neke mogućnosti vezane uz strujanje analize. Analitički strujanje podržava brojne unosa i Proizvodi i čak i funkcije možete koristiti u Azure strojnog učenja da biste ga robusne alata za analizu strujanja podataka. Možete pokrenuti da biste istražili dodatne informacije o strujanje analize pomoću naš [tečaj karta](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Dodatne informacije o pisanju upita, pročitajte članak o [uobičajene uzorke upita](./stream-analytics-stream-analytics-query-patterns.md).
