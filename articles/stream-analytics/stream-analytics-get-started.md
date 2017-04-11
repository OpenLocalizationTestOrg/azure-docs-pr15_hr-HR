<properties
    pageTitle="Početak rada s strujanje analize: u stvarnom vremenu prijevare otkrivanje | Microsoft Azure"
    description="Saznajte kako stvoriti otkrivanje rješenje u stvarnom vremenu prijevaru s strujanje analize. Koristite koncentratora za događaj za obradu u stvarnom vremenu događaj."
    keywords="Otkrivanje značajkom, prijevara otkrivanje, a zatim otkrivanje značajkom stvarnom vremenu"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Prvi koraci pri korištenju Azure strujanje analize: otkrivanje prijevarama dostupne su u stvarnom vremenu

Upute za stvaranje završetka do kraja rješenja za otkrivanje u stvarnom vremenu prijevare s Azure strujanje analize. Događaji dohvaćali koncentratora za Azure događaj temu, napisati strujanje analize upita za zbrajanje ili upozorenjem i poslali rezultate u izlaz primatelj da biste dobili uvid iznad podataka s obradom u stvarnom vremenu. Otkrivanje značajkom stvarnom vremenu za Telekomunikacije opisana, ali postupak primjer ravnomjerno najprikladniji za ostale vrste otkrivanje prijevara kao što su kreditnom karticom ili identitet scenariji krađe.

Strujanje analize je potpuno Upravljani servis nudi obrada najniža Latencija iznimno dostupna, skalabilni složene događaja putem strujanja podataka u oblaku. Dodatne informacije potražite u članku [Uvod u Azure strujanje analize](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenarij: Telekomunikacije i SIM prijevara otkrivanja u stvarnom vremenu

Telekomunikacije tvrtka ima veliku količinu podataka za dolazne pozive. Tvrtka mora sljedeće iz svoje podatke:
* Poredite ove podatke na kojima se upravlja iznos i dobiti uvida korištenje klijenta s vremenom i zemljopisnim područjima.
* Otkriva SIM prijevare (više pozivi koji dolaze iz istog identiteta oko istovremeno, ali geografski različitim mjestima) u stvarnom vremenu tako da možete lako odgovori obavješćivati klijentima ili isključivanja servisa.

U dva kanonskog scenarijima Internet stvari (IoT) se tona telemetrijskih ili senzor podataka generira – i klijentima želite da ih aggregate ili upozorenja putem anomalies u stvarnom vremenu.

## <a name="prerequisites"></a>Preduvjeti

- Preuzmite [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) iz Microsoftova centra za preuzimanje 
- Neobavezno: Izvornog koda programa generator događaj iz [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator)

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>Stvaranje ulazni Azure događaj koncentratora i korisničke grupe

Primjer aplikacije će Generiraj događaje i automatske ih instancu događaj koncentrator za obradu u stvarnom vremenu. Servis Bus događaj koncentratora su željeni način događaj ingestion za strujanje analize i dodatne informacije o događajima koncentratora u [dokumentaciji Bus servisa Azure](/documentation/services/service-bus/).

Da biste stvorili koncentratora za događaj:

1.  [Portal za Azure](https://manage.windowsazure.com/) kliknite **Novo** > **Aplikacije servisa** > **Bus servisa** > **Događaj koncentrator** > **Brzo stvaranje**. Navedite naziv, regija i prostora za novi ili postojeći naziv da biste stvorili novi događaj koncentrator.  
2.  Kao preporučenim načinom rada svaki zadatak analize strujanje namijenjen iz jedne grupe potrošača koncentrator događaj. Ne možemo će vas voditi kroz postupak stvaranja grupu korisnika, a možete [Saznajte više o grupama potrošača](https://msdn.microsoft.com/library/azure/dn836025.aspx). Da biste stvorili grupu korisnika, dođite do novostvorenu koncentratora za događaj i kliknite karticu **Korisničke grupe** , a zatim kliknite **Stvori** na dno stranice i navedite naziv za grupu korisnika.
3.  Da biste dodijelili pristup koncentrator događaj, moramo stvoriti pravilo zajednički pristup.  Kliknite karticu **Konfiguracija** koncentratora za događaj.
4.  U odjeljku **Pravila za zajedničko korištenje programa Access**, stvaranje novog pravila s dozvolama za **Upravljanje** .

    ![Zajedničko korištenje gdje možete stvoriti pravila s dozvolama za upravljanje pravilnike za pristup.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  Kliknite **Spremi** pri dnu stranice.
6.  Idite na **nadzornoj ploči** i kliknite **Informacije o vezi** u donjem desnom kutu stranice i zatim kopirajte i spremiti podatke o vezi.

## <a name="configure-and-start-event-generator-application"></a>Konfiguraciju i pokretanje aplikacija generator događaja

Ne možemo je navesti klijentska aplikacija koja će generiranje uzorka dolazni poziv metapodataka i automatske koncentrator za događaj. Slijedite korake u nastavku da biste postavili te aplikacije.  

1.  Preuzimanje [datoteke TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip). Zatim raspakiraj ga u direktorij.

    **Napomena**: Windows možda blokira zip preuzete datoteke. Desnom tipkom miša kliknite datoteku, a zatim odaberite Svojstva. Ako je poruka "datoteka stigla s drugog računala i možda je blokirana da biste zaštitili ovo računalo." zatim na osi okvir "Unblock", a zatim kliknite Primijeni na zip datoteke.

2.  Zamijenite vrijednosti Microsoft.ServiceBus.ConnectionString i EventHubName u **telcodatagen.exe.config** niz za povezivanje koncentratora za događaj i naziv.

    **Napomena**: kopirali niz za povezivanje iz Azure mjesta portala naziv veze na kraju. Ne zaboravite da biste uklonili na "; EntityPath =<value>"iz ključa Dodaj polje =.

3.  Pokrenite aplikaciju. Korištenje je na sljedeći način:

   telcodatagen.exe [#NumCDRsPerHour] [SIM kartica prijevara vjerojatnost] [#DurationHours]

Sljedeći primjer generirat će 1000 događaji s 20% vjerojatnosti prijevara tijekom dvotjednog dva sata.

    telcodatagen.exe 1000 .2 2

Vidjet ćete zapise koji se šalje na koncentratora za događaj. Neke ključne polja koja ne možemo ovu aplikaciju u stvarnom vremenu prijevare otkrivanje pomoću definiraju se ovdje:

| Zapis | Definicija |
| ------------- | ------------- |
| CallrecTime | Vrijeme početka vremenske oznake za poziv. |
| SwitchNum | Telefon parametar koji se koristi za povezivanje poziv. |
| CallingNum | Telefonski broj pozivatelja. |
| CallingIMSI | Međunarodne mobilne pretplatnika identiteta (IMSI).  Jedinstveni identifikator pozivatelja. |
| CalledNum | Telefonski broj poziva primatelju. |
| CalledIMSI | Međunarodne mobilne pretplatnika identiteta (IMSI).  Jedinstveni identifikator poziva primatelju. |


## <a name="create-stream-analytics-job"></a>Stvaranje zadatka strujanje Analytics
Imamo strujanje Telekomunikacije događaja, ne možemo možete postaviti posao strujanje analize da biste analizirali te događaje u stvarnom vremenu.

### <a name="provision-a-stream-analytics-job"></a>Dodjela resursa za posao strujanje Analytics

1.  Na portalu Azure kliknite **Novo > podatkovne usluge > strujanje analize > brzo stvaranje**.
2.  Navedite sljedeće vrijednosti, a zatim **Stvorite zadatak analize strujanje**:

    * **Naziv zadatka**: Unesite naziv zadatka.

    * **Regija**: Odaberite mjesto na koje želite pokrenuti posla regiju. Razmotrite smještanje posla i događaja središte na istom području da bi bolje performanse, a da biste bili sigurni da vam će neće biti plaćanja za prijenos podataka između područja.

    * **Račun za pohranu**: Odaberite račun za Azure prostora za pohranu koji želite koristiti za pohranu nadzora podataka za sve zadatke strujanje analize koji se izvodi u tom području. Imate mogućnost da biste odabrali postojeći račun za pohranu ili stvorite novi.

3.  Kliknite **Analitika strujanje** u lijevom oknu za prikaz popisa zadataka strujanje analize.

    ![Ikona usluge za strujanje Analytics](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  Prikazat će se novi zadatak sa statusom **stvorena**. Obratite pozornost na to je onemogućen gumb **Start** na dno stranice. Prije no što počnete posla već morate konfigurirati unos posla, izlazne i upit.

### <a name="specify-job-input"></a>Odredite unos zadatka
1.  U svoj posao strujanje analize kliknite **unosa** na vrhu stranice, a zatim **Dodajte unos**. U dijaloškom okviru koji će se otvoriti će vas voditi kroz nekoliko koraka možete postaviti unos.
2.  Odaberite **strujanja podataka**, a zatim kliknite na desni gumb.
3.  Odaberite **Događaj koncentrator**, a zatim kliknite na desni gumb.
4.  Upišite ili odaberite sljedeće vrijednosti na trećoj stranici:

    * **Pseudonim za unos**: Upišite neslužbeni naziv za ovaj zadatak unos kao što je *CallStream*. Imajte na umu da ćete koristiti taj naziv u upitu kasnije.
    * **Koncentrator za događaj**: Ako je središtu događaj koji ste stvorili u okviru iste pretplate kao posao strujanje analize, odaberite prostora za naziv koji se nalazi središtu događaja u.

    Ako je koncentratora za događaj u neku drugu pretplatu, odaberite **Koristi događaj središte na drugu pretplatu** i ručno unesite podatke za **Polje naziva Bus servisa**, **Naziv događaja koncentrator**, **Naziv pravila koncentrator događaja**, **Ključ pravila koncentrator za događaj**i **Count particija koncentrator za događaj**.

    * **Naziv događaja koncentrator**: odaberite naziv koncentratora za događaj.

    * **Naziv pravila koncentrator događaja**: odaberite događaj koncentrator pravila stvorili ranije u ovom ćete praktičnom vodiču.

    * **Događaj koncentrator korisničke grupe**: spajanje standardnih potrošača stvorili ranije u ovom ćete praktičnom vodiču.
5.  Kliknite gumb desno.
6.  Navedite sljedeće vrijednosti:

    * **Oblikovanje serijalizatora događaj**: JSON
    * **Kodiranje**: UTF8
7.  Kliknite gumb Provjeri da biste dodali izvoru i da biste provjerili možete li strujanje Analytics uspješno povezati s koncentratora za događaj.

### <a name="specify-job-query"></a>Odredite upit poslova

Analitički strujanje podržava model jednostavne, deklarativno upit za s opisom transformacije za obradu u stvarnom vremenu. Da biste saznali više o jeziku, potražite u članku [Referenca jezik za Azure strujanje analize Query](https://msdn.microsoft.com/library/dn834998.aspx). Pomoću ovog praktičnog vodiča pomoći će vam autora i testiranje nekoliko upita putem vaše u stvarnom vremenu strujanja podataka poziv.

#### <a name="optional-sample-input-data"></a>Neobavezno: Ogledne ulazne podatke
Da biste provjerili valjanost upita na temelju podataka stvarni posao, možete koristiti značajku **Oglednih podataka** možete izdvojiti događaje iz vaše strujanje i stvoriti na. Datoteka JSON događaje za testiranje.  Sljedeći koraci prikazuju kako to učiniti i smo ste naveli oglednu datoteku [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) za testiranje.

1.  Odaberite unos koncentratora za događaj i kliknite **Ogledne podatke** pri dnu stranice.
2.  U dijaloškom okviru koji će se pojaviti odredite **Vrijeme početka** da biste pokrenuli prikupljanje podataka iz i **trajanje** koliko dodatne podatke trošiti.
3.  Kliknite gumb potvrdite da biste pokrenuli uzorkovanje podataka iz unos.  To može potrajati minutu ili dvije podatkovne datoteke koja se treba proizvesti.  Po dovršetku postupka kliknite **Detalji** i preuzeti i spremiti u. JSON datoteku.

    ![Preuzimanje i spremanje podataka iz obrađeni u datoteci JSON](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>PASSTHROUGH upit

Ako želite arhivirati svaki događaj passthrough upit možete koristiti da biste pročitali sva polja u opseg događaj ili poruke. Za početak s učinite jednostavne passthrough upit projekata sva polja u događaj.

1.  Kliknite **upit** s vrha stranice posao strujanje analize.
2.  Uređivač kod dodajte sljedeće:

        SELECT * FROM CallStream

    > Provjerite podudara li se naziv izvora unos naziva unos koju ste prethodno naveli.

3.  Kliknite **Testiraj** u uređivaču upita.
4.  Navedite testiranje datoteke koji ste stvorili pomoću prethodnih koraka ili koristite [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json).
5.  Kliknite gumb Provjeri i prikazali rezultate ispod definicije upita.

    ![Rezultati definicije upita](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Stupac projekcije

Ne možemo ćete sada smanjiti broj vraćenih polja u skupu manje.

1.  Promjena upita u uređivaču kod da biste:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.

    ![Izlaz u uređivaču upita.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Broj dolazni pozivi po regijama: Tumbling prozor s zbrajanja

Da biste usporedili iznos te dolazne pozive po regijama smo ćete pod utjecajem [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) da biste dobili ukupan broj dolazne pozive grupirane SwitchNum svakih 5 sekundi.

1.  Promjena upita u uređivaču kod da biste:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Ovaj upit koristi **Vremensku oznaku po** ključnoj riječi da biste naveli polje s vremenskom oznakom opseg će se koristiti u vremenski izračuni. Ako to polje nije naveden, operacija prikaz u prozorima želite izvršiti pomoću vrijeme svaki događaj stigli na središte događaj. U odjeljku ["Kašnjenja vrijeme Dodavanje veze za vanjskih aplikacija vrijeme" u analize strujanje upit jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Imajte na umu da mogu pristupati vremenske oznake za kraj svakog prozora pomoću svojstvo **System.Timestamp** .

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.

    ![Rezultati upita za Timestand po](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Otkrivanje prijevare SIM s na samostalno uključivanje

Da biste odredili potencijalno lažno korištenje smo ćete potražite pozive potječu iz istog korisnika, ali na različitim mjestima za manje od 5 sekundi.  Ne možemo [spoj](https://msdn.microsoft.com/library/azure/dn835026.aspx) strujanje događaja poziva sa samim sobom da biste provjerili tim slučajevima.

1.  Promjena upita u uređivaču kod da biste:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.

    ![Rezultati upita dio spoja](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Stvaranje primatelj Izlaz

Sad kad ste definirali strujanje događaja, programa događaja koncentrator za ingest događaja i upit za unos za izvođenje transformaciju toka, posljednji korak je da biste definirali je primatelj izlaz za posao.  Ne možemo ćete pisati događaje lažno ponašanje spremište blobova platforme.

Slijedite korake u nastavku da biste stvorili spremnik za spremište blobova platforme ako ga već nemate.

1.  Koristi postojeći račun za pohranu ili stvorite novi račun za pohranu tako da kliknete **NOVO > podatkovne usluge > SPREMANJE > brzo stvaranje** i slijedite upute.
2.  Odaberite račun za pohranu, kliknite **SPREMNIKA** pri vrhu stranice, a zatim **DODAJ**.
3.  Navedite **naziv** vaše spremnik i postavite **PRISTUP** javnim Blob.

## <a name="specify-job-output"></a>Odredite izlazni posla

1.  U svoj posao strujanje analize kliknite **IZLAZ** iz vrha stranice, a zatim **Dodajte IZLAZ**. U dijaloškom okviru koji će se otvoriti će vas voditi kroz nekoliko koraka možete postaviti izlaz.
2.  Odaberite **Spremište blobova PLATFORME**, a zatim kliknite na desni gumb.
3.  Upišite ili odaberite sljedeće vrijednosti na trećoj stranici:

    * **PSEUDONIM za IZLAZ**: Upišite neslužbeni naziv za ovaj zadatak izlaz.
    * **PRETPLATU**: Ako je spremište blobova platforme koji ste stvorili u okviru iste pretplate kao posao strujanje analize, odaberite **Koristi prostora za pohranu račun s trenutne pretplate**. Ako je prostora za pohranu u neku drugu pretplatu, odaberite **Račun za pohranu koristite na drugu pretplatu** i ručno unijeti podatke za **RAČUN za POHRANU**, **KLJUČ za RAČUN za POHRANU** **SPREMNIK**.
    * **RAČUN za POHRANU**: odaberite naziv računa za pohranu.
    * **SPREMNIK**: odaberite naziv spremnika.
    * **PREFIKS naziva datoteke**: vrstu u prefiks datoteke koju ćete koristiti prilikom pisanja blob izlaz.

4.  Kliknite gumb desno.
5.  Navedite sljedeće vrijednosti:

    * **OBLIKOVANJE SERIJALIZATORA DOGAĐAJ**: JSON
    * **Kodiranje**: UTF8

6.  Kliknite gumb Provjeri da biste dodali izvoru i da biste provjerili možete li strujanje Analytics uspješno povezati s računom za pohranu.

## <a name="start-job-for-real-time-processing"></a>Pokretanje zadatka za obradu u stvarnom vremenu

Budući da unos posla, upita i izlazni sve nisu navedeni, ne možemo spremni za početak posao strujanje Analytics za otkrivanje prijevara u stvarnom vremenu.

1.  Iz posla **nadzorne PLOČE**, kliknite **pokretanje** pri dnu stranice.
2.  U dijaloškom okviru koji će se prikazati odaberite **vrijeme POČETKA POSLA** , a zatim kliknite gumb Provjeri pri dnu dijaloškog okvira. Status zadatka promijenit će se **Početni** i uskoro premjestit će **pokrenuti**.

## <a name="view-fraud-detection-output"></a>Prikaz prijevaru otkrivanje Izlaz

Pomoću alata kao što su [Azure prostora za pohranu Explorer](https://azurestorageexplorer.codeplex.com/) ili [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) da biste pogledali lažno događaje kao što su zapisuju Izlaz u stvarnom vremenu.  

![Otkrivanje prijevare: lažno događaje prikazati u stvarnom vremenu](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Podrška
Za dodatnu pomoć, pokušajte našem [forumu Azure strujanje analize](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
