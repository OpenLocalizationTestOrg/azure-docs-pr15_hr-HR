<properties
    pageTitle="U stvarnom vremenu Twitter šalju analize pomoću analize strujanje | Microsoft Azure"
    description="Saznajte kako koristiti strujanje Analytics za u stvarnom vremenu analizu šalju Twitter. Detaljne upute iz generiranje događaja na podatke na nadzornoj ploči za uživo."
    keywords="u stvarnom vremenu twitter trend analizu, šalju analizu, analizu društvene mreže, trend analizu primjer"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Analiza društvene mreže: u stvarnom vremenu na Twitteru analizu šalju u Azure strujanje Analytics

Naučiti kako stvoriti rješenje analizu šalju analitičkih društvenih mreža tako da unošenje u stvarnom vremenu Twitter događaje u koncentratora Azure događaj. Pišete ćete upit Azure strujanje Analytics za analizu podataka. Koje ćete zatim pohrana rezultata za kasnije perusal ili korištenje nadzorne ploče i [Power BI](https://powerbi.com/) omogućuje uvid u stvarnom vremenu.

Alati analytics za društvene mreže pomoći razumjeti trendova teme, tj., teme i attitudes koji imaju veliku količinu objave u društvenih mreža tvrtke ili ustanove. Analiza šalju, koja se zove *dubinsko pretraživanje mišljenje*, koristi Alati analytics za društvene mreže da biste odredili attitudes prema proizvoda, ideja i tako dalje. U stvarnom vremenu analize trenda Twitter je odličan primjer jer modela pretplate oznaku omogućuje poslušati ključnim i razvoj analizu šalju na sažetak sadržaja.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Scenarij: Šalju analize u stvarnom vremenu

Tvrtke koje ima medijskih sadržaja vijesti web-mjesto je želi dobiti prednosti u odnosu na njegov konkurenata po sa svojstvima sadržaja web-mjesta koji se nalazi odmah odgovarajuću njegov čitateljima. Tvrtka koristi analizu društvenih mreža na temama koje su relevantni čitateljima tako da učinite u stvarnom vremenu šalju analizi podataka Twitter. Konkretno, za prepoznavanje trendova teme u stvarnom vremenu na Twitteru tvrtke potrebno u stvarnom vremenu analitičke podatke o tweet glasnoću i šalju ključa teme. Dakle, zapravo, morate je šalju analitički modul analizu koji se temelji na ovom društvenih mreža sažetka sadržaja.

## <a name="prerequisites"></a>Preduvjeti
-   Račun i [OAuth pristupni token](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) na twitteru
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) iz Microsoftova centra za preuzimanje
-   Neobavezno: Izvornog koda za klijenta twitter iz [GitHub](https://aka.ms/azure-stream-analytics-twitterclient)

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Stvaranje unosa koncentratora za događaj i grupu korisnika

Primjer aplikacije će Generiraj događaje i automatske ih instancu koncentratora događaja (u događaj središte za kratki). Servis Bus događaj koncentratora su željeni način događaj ingestion analitičkih strujanje. Potražite u dokumentaciji koncentratora događaja u [dokumentaciji Bus servisa](/documentation/services/service-bus/).

Poduzmite sljedeće korake da biste stvorili koncentratora za događaj.

1.  Na portalu Azure kliknite **NOVO** > **Aplikacije SERVISA** > **BUS SERVISA** > **DOGAĐAJ KONCENTRATOR** > **Brzo stvaranje**, i navedite naziv, regija i polja naziva novi ili postojeći.  
2.  Kao preporučenim načinom rada svaki zadatak analize strujanje namijenjen iz jedne grupe potrošača koncentratora za događaj. Ne možemo će vas voditi kroz postupak stvaranja grupe potrošača kasnije. Dodatne informacije o korisničke grupe na [Pregled Azure koncentratora za događaj](https://msdn.microsoft.com/library/azure/dn836025.aspx). Da biste stvorili grupu korisnika, otvorite koncentrator novostvoreni događaja kliknite karticu **KORISNIČKE GRUPE** , kliknite **Stvori** na dno stranice, a zatim navedite naziv za grupu korisnika.
3.  Da biste dodijelili pristup središtu događaj, moramo stvoriti pravilo zajednički pristup. Kliknite karticu **KONFIGURACIJA** koncentratora za događaj.
4.  U odjeljku **Pravila za zajedničko korištenje programa ACCESS**, stvaranje novog pravila s dozvolama za **Upravljanje** .

    ![Zajedničko korištenje gdje možete stvoriti pravila s dozvolama za upravljanje pravilnike za pristup.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  Kliknite **SPREMI** pri dnu stranice.
6.  Idite na **nadzornu PLOČU**, kliknite **Informacije o VEZI** u donjem desnom kutu stranice i zatim kopirajte i spremiti podatke o vezi. (Pomoću ikone kopiju koja se prikazuje ispod ikonu pretraživanja).

## <a name="configure-and-start-the-twitter-client-application"></a>Konfigurirajte i započnite klijentska aplikacija Twitter

Ne možemo je navesti klijentska aplikacija koja će se povezati s Twitter podataka putem [Twitter, strujanje API -ji](https://dev.twitter.com/streaming/overview) za prikupljanje Tweet događaja o s parametrima skup tema. Alat za Otvori izvor [Sentiment140](http://help.sentiment140.com/) koristi se za vrijednost šalju dodijeliti svakom tweet.

- 0 = negativan
- 2 = neutralno
- 4 = pozitivan

Nakon toga Tweet događaji pomiču se koncentrator za događaj.  

Slijedite ove korake da biste postavili aplikacija:

1.  [Preuzmite TwitterClient rješenja](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Otvorite TwitterClient.exe.config i oauth_consumer_key, oauth_consumer_secret, oauth_token i oauth_token_secret zamijenite Twitter tokeni koje sadrže vrijednosti.  

    [Koraci za generiranje OAuth pristupni token](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Imajte na umu da ćete da biste prazan aplikacije da biste generirali token.  
3.  Zamjena vrijednosti EventHubConnectionString i EventHubName u TwitterClient.exe.config niz za povezivanje i naziv koncentratora za događaj. Niz za povezivanje koju ste ranije kopirali vam niz za povezivanje i naziv koncentratora za događaj, stoga svakako odvojite ih i postavili svaki unos u odgovarajuće polje. Na primjer, razmotrite sljedeće niz za povezivanje:

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    Datoteka TwitterClient.exe.config mora sadržavati postavki kao u sljedećem primjeru:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    Važno Imajte na umu da je tekst "EntityPath =" ne __ne__ pojavljuju se u vrijednost EventHubName.

4.  *Neobavezno:* Prilagodite ključnih riječi za pretraživanje.  Po zadanom ovu aplikaciju Traži "Azure, Skype, XBox, Microsoft, Seattle".  Možete prilagoditi vrijednosti za **twitter_keywords** u TwitterClient.exe.config, po želji.
5.  Pokrenite TwitterClient.exe da biste pokrenuli aplikaciju. Vidjet ćete događaji Tweet s vrijednostima **CreatedAt**, **tema**i **SentimentScore** šalje koncentratora za događaj.

    ![Analiza šalju: SentimentScore vrijednosti koji se šalju koncentratora za događaj.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Stvaranje zadatka strujanje Analytics

Nakon što su događaji Tweet strujanje u stvarnom vremenu iz Twitter, ne možemo možete postaviti posao strujanje analize da biste analizirali te događaje u stvarnom vremenu.

### <a name="provision-a-stream-analytics-job"></a>Dodjela resursa za posao strujanje Analytics

1.  [Portal za Azure](https://manage.windowsazure.com/)kliknite **NOVO** > **DATA SERVICES** > **Strujanje ANALIZE** > **Brzo stvaranje**.
2.  Navedite sljedeće vrijednosti, a zatim **STVORITE zadatak ANALIZE strujanje**:

    * **Naziv ZADATKA**: Unesite naziv zadatka.
    * **REGIJA**: Odaberite mjesto na koje želite pokrenuti posla regiju. Razmotrite smještanje posla i događaja središte na istom području da bi bolje performanse, a da biste bili sigurni da vam će neće biti plaćanja za prijenos podataka između područja.
    * **RAČUN za POHRANU**: Odaberite račun za Azure prostora za pohranu koji želite koristiti za pohranu nadzora podataka za sve zadatke strujanje analize koji se izvode u tom području. Imate mogućnost da biste odabrali postojeći račun za pohranu ili stvorite novi.

3.  Kliknite **ANALITIKA strujanje** u lijevom oknu na popisu poslova strujanje analize.  
    ![Ikona usluge za strujanje Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    Prikazat će se novi zadatak sa statusom **STVORENA**. Obratite pozornost na to je onemogućen gumb **START** na dno stranice. Prije no što počnete posla već morate konfigurirati unos posla, izlazne i upit.


### <a name="specify-job-input"></a>Odredite unos zadatka
1.  U svoj posao strujanje analize kliknite **UNOSA** na vrhu stranice, a zatim **Dodajte unos**. U dijaloškom okviru koji će se otvoriti će vas voditi kroz nekoliko koraka možete postaviti unos.
2.  Kliknite **STRUJANJA podataka**, a zatim na desni gumb.
3.  Kliknite **DOGAĐAJ KONCENTRATOR**, a zatim na desni gumb.
4.  Upišite ili odaberite sljedeće vrijednosti na trećoj stranici:

    * **PSEUDONIM za unos**: Upišite neslužbeni naziv za ovaj zadatak za unos, kao što su *TwitterStream*. Imajte na umu da ćete koristiti taj naziv u upitu kasnije.
    **KONCENTRATOR za DOGAĐAJ**: Ako je središtu događaj koji ste stvorili u okviru iste pretplate kao posao strujanje analize, odaberite prostora za naziv koji se nalazi središtu događaja u.

        Ako je koncentratora za događaj u neku drugu pretplatu, kliknite **Koristi događaj središte na drugu pretplatu**, a ručno unesite podatke za **Polje naziva BUS SERVISA**, **Naziv događaja KONCENTRATOR**, **Naziv pravila KONCENTRATOR događaja**, **KLJUČ pravila KONCENTRATOR za DOGAĐAJ**i **COUNT PARTICIJA KONCENTRATOR za DOGAĐAJ**.

    * **Naziv događaja KONCENTRATOR**: odaberite naziv koncentratora za događaj.

    * **Naziv pravila KONCENTRATOR događaja**: Odaberite pravila koncentrator događaj koji ste stvorili ranije u ovom praktičnom vodiču.

    * **DOGAĐAJ KONCENTRATOR KORISNIČKE GRUPE**: upišite naziv grupe korisnika koju ste stvorili ranije u ovom praktičnom vodiču.
5.  Kliknite gumb desno.
6.  Navedite sljedeće vrijednosti:

    * **OBLIKOVANJE SERIJALIZATORA DOGAĐAJ**: JSON
    * **Kodiranje**: UTF8

7.  Kliknite gumb **PROVJERI** da biste dodali izvoru i da biste provjerili možete li strujanje Analytics uspješno povezati s koncentratora za događaj.

### <a name="specify-job-query"></a>Odredite upit poslova

Analitički strujanje podržava modela jednostavne, deklarativno upit koji opisuje transformacije. Da biste saznali više o jeziku, potražite u članku [Referenca jezik za Azure strujanje analize Query](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Pomoću ovog praktičnog vodiča pomoći će vam autora i testiranje nekoliko upita preko Twitter podataka.

#### <a name="sample-data-input"></a>Ogledni unos podataka

Da biste provjerili valjanost upita na temelju podataka stvarni posao, koristite značajku **OGLEDNIH podataka** za izdvajanje događaje iz vaše toka i stvoriti datoteku .json događaje za testiranje.

1.  Odaberite unos koncentrator događaja, a zatim **OGLEDNE podatke** pri dnu stranice.
2.  U dijaloškom okviru odredite **vrijeme POČETKA** da biste pokrenuli prikupljanje podataka i **TRAJANJE** koliko dodatne podatke za korištenje.
3.  Kliknite gumb **pojedinosti** , a zatim kliknite vezu **kliknite ovdje** da biste preuzeli i spremite datoteku generirani .json.

#### <a name="pass-through-query"></a>Prolazni upit
Da biste započeli, ne možemo neće imati jednostavne prolazni upit projekata sva polja u događaj.

1.  Kliknite **UPITA** pri vrhu stranice posao strujanje analize.
2.  U uređivaču kod zamijeniti predloškom početni upit sljedeće:

        SELECT * FROM TwitterStream

    Provjerite podudara li se naziv izvora unos naziva unos koji ste prethodno naveli.

3.  Kliknite **Testiraj** u uređivaču upita.
4.  Otvorite oglednu datoteku .json.
5.  Kliknite gumb **Provjera** pa u odjeljku Rezultati ispod definicije upita.

    ![Rezultati ispod definicije upita](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Broj tweets po temi: Tumbling prozor s zbrajanja

Da biste usporedili broj spominjanja među temama ćemo pomoću [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) Brojanje spominjanja po temi svakih pet sekundi.

1.  Promjena upita u uređivaču kod da biste:

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    Ovaj upit koristi **Vremensku OZNAKU po** ključnoj riječi da biste naveli polje s vremenskom oznakom opseg će se koristiti u vremenski izračuni. Ako to polje nije naveden, prikaz u prozorima operacija se želite izvršiti pomoću vrijeme koje svaki događaj stigli na središte događaj.  Dodatne informacije u odjeljku "Kašnjenja vrijeme Dodavanje veze za vanjskih aplikacija vrijeme" [strujanje analize upita](https://msdn.microsoft.com/library/azure/dn834998.aspx)reference.

    Ovaj upit pristupa i vremenske oznake za kraj svakog prozora pomoću **System.Timestamp** svojstva.

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.

#### <a name="identify-trending-topics-sliding-window"></a>Prepoznavanje trendova teme: klizača prozora

Prepoznavanje trendova teme, ne možemo ćete potražite temama koje Unakrsna vrijednosti praga za @spominjanja u određenom vremenskog razdoblja. Za potrebe ovog praktičnog vodiča, ne možemo ćete potražiti temama koje se spominju više od 20 puta u zadnjoj pet sekundi pomoću [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx).

1.  Promjena upita u uređivaču kod da biste: Odaberite System.Timestamp kao vrijeme, temi COUNT (*) kao Spominjanja iz TwitterStream vremenske oznake tako da CreatedAt GRUPE tako da SLIDINGWINDOW(s, 5), tema koje se POJAVLJUJU BROJANJE (*) > 20

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.

    ![Izlaz upita klizača prozora](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Broj spominjanja i šalju: Tumbling prozor s zbrajanja
Konačni upit koji će ispitivanja koristi **TumblingWindow** da biste dobili broj spominjanja, prosjek, postavke minimum, maksimum i standardnu devijaciju šalju rezultat za svaku temu svakih pet sekundi.

1.  Promjena upita u uređivaču kod da biste:

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.
3.  Ovo je upit koji koristimo za naše nadzorne ploče.  Kliknite **SPREMI** pri dnu stranice.


## <a name="create-output-sink"></a>Stvaranje primatelj Izlaz

Sad kad ste definirali strujanje događaja, unos za ingest događaja i upit za izvođenje transformaciju toka, koncentratora za događaj posljednji korak je da biste definirali je primatelj izlaz za posao.  Ne možemo ćete pisati Zbrojeno tweet događaje iz naših upita posla spremište blobova platforme Azure.  Rezultati nije automatske i Azure SQL bazu podataka, prostor za pohranu Azure tablice, ili koncentratora događaj, ovisno o određenoj aplikaciji mora.

Ako nemate jednu, poduzmite sljedeće korake da biste stvorili spremnik za spremište blobova platforme:

1.  Koristi postojeći račun za pohranu ili stvorite novi račun za pohranu tako da kliknete **Nova** > **DATA SERVICES** > **prostora za POHRANU** > **Brzo stvaranje**, a zatim slijedite upute na zaslonu.
2.  Odaberite račun za pohranu, kliknite **SPREMNIKA** pri vrhu stranice, a zatim **DODAJ**.
3.  Navedite **naziv** vaše spremnik i postavite **programa ACCESS** u **Javnom Blob**.

## <a name="specify-job-output"></a>Odredite izlazni posla

1.  U svoj posao strujanje analize kliknite **IZLAZ** pri vrhu stranice, a zatim **Dodajte IZLAZ**. U dijaloškom okviru koji će se otvoriti će vas voditi kroz nekoliko koraka da biste postavili izlaz.
2.  Kliknite **Spremište blobova PLATFORME**, a zatim kliknite gumb desno.
3.  Upišite ili odaberite sljedeće vrijednosti na trećoj stranici:

    * **PSEUDONIM za IZLAZ**: Upišite neslužbeni naziv za ovaj zadatak izlaz.

    * **PRETPLATU**: Ako je blobova koji ste stvorili u okviru iste pretplate kao posao strujanje analize, kliknite **Koristi račun za pohranu iz trenutne pretplate**. Ako je prostora za pohranu u neku drugu pretplatu, kliknite **Koristi račun za pohranu iz drugu pretplatu**, a Ručni unos informacija za **POHRANU RAČUNA**, **KLJUČ za RAČUN za POHRANU**i **SPREMNIK**.

    * **RAČUN za POHRANU**: odaberite naziv računa za pohranu.

    * **SPREMNIK**: odaberite naziv spremnika.

    * **PREFIKS naziva datoteke**: upišite prefiks datoteka za korištenje prilikom pisanja blob izlaz.

4.  Kliknite gumb desno.
5.  Navedite sljedeće vrijednosti:
    * **OBLIKOVANJE SERIJALIZATORA DOGAĐAJ**: JSON
    * **Kodiranje**: UTF8
6.  Kliknite gumb **PROVJERI** da biste dodali izvoru i da biste provjerili možete li strujanje Analytics uspješno povezati s računom za pohranu.

## <a name="start-job"></a>Pokretanje zadatka

Jer unos posla, upita i izlazni sve nisu navedeni, ne možemo spremni za početak posao strujanje analize.

1.  Iz posla **nadzorne PLOČE**, kliknite **pokretanje** pri dnu stranice.
2.  U dijaloškom okviru koji će se otvoriti kliknite **vrijeme POČETKA POSLA**, a zatim kliknite gumb **PROVJERI** pri dnu dijaloškog okvira. Status zadatka promijenit će se **Početni** i promijenit će se uskoro **pokrenut**.


## <a name="view-output-for-sentiment-analysis"></a>Prikaz izlaz za analizu šalju

Kada vaš posao je pokrenut i obradu u stvarnom vremenu strujanje Twitter, odaberite kako želite da biste pogledali izlaz za analizu šalju. Pomoću alata kao što su [Azure prostora za pohranu Explorer](https://azurestorageexplorer.codeplex.com/) ili [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) da biste pogledali izlaz posla u stvarnom vremenu. Na tom mjestu možete koristiti [Power BI](https://powerbi.com/) da biste proširili aplikacije da biste dodali prilagođenu nadzorne ploče nalik na ovaj u sljedeće snimku zaslona.

![Analiza društvene mreže: strujanje analize šalju analize (dubinsko pretraživanje mišljenje) Izlaz na nadzornoj ploči Power BI.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Podrška
Za dodatnu pomoć, pokušajte našem [forumu Azure strujanje analize](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
