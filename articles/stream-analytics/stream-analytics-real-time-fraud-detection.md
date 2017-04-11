<properties
    pageTitle="Analytics strujanje: Otkrivanje u stvarnom vremenu prijevare | Microsoft Azure"
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

Upute za stvaranje završetka do kraja rješenja za otkrivanje u stvarnom vremenu prijevare s Azure strujanje analize. Događaji dohvaćali Azure događaj koncentratora temu, napisati strujanje analize upita za zbrajanje ili upozorenjem i poslali rezultate u izlaz primatelj da bi se dobio uvida iznad podataka s obradom u stvarnom vremenu. Objašnjeni u stvarnom vremenu značajkom otkrivanje za Telekomunikacije, ali postupak primjer ravnomjerno najprikladniji za ostale vrste prijevaru otkrivanje kao što su scenariji krađe kreditnom karticom ili identitet.

Analitički strujanje je potpuno upravljanog servis koji obrada najniža Latencija, Visoko dostupna, prilagodljivi, složene događaja putem strujanja podataka u oblaku. Dodatne informacije potražite u članku [Uvod u Azure strujanje analize](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenarij: Telekomunikacije i SIM prijevara otkrivanja u stvarnom vremenu

Telekomunikacije tvrtka ima veliku količinu podataka za dolazne pozive. Tvrtka mora sljedeće iz svoje podatke:

* Smanjite podataka moguće upravljati iznos i dobili uvid o korištenju klijenta s vremenom i svim zemljopisnim regijama.
* Otkriva SIM prijevare (više pozive iz istog identiteta oko istovremeno, ali geografski različitim mjestima) u stvarnom vremenu tako da se tvrtke jednostavno možete odgovoriti obavješćivati klijentima ili isključivanja servisa.

Kanonski scenariji Internet stvari (IoT) imati tona od telemetrijskih podataka iz senzora. Klijenti želite grupiranja podataka ili primati upozorenja o anomalies u stvarnom vremenu.

## <a name="prerequisites"></a>Preduvjeti

- Preuzmite [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) iz Microsoftova centra za preuzimanje
- Neobavezno: Izvornog koda programa generator događaj iz [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Stvaranje grupe za unos i potrošača Azure događaj koncentratora

Primjer aplikacije će Generiraj događaje i automatske ih instancu događaj koncentratora za obradu u stvarnom vremenu. Servis Bus događaj koncentratora su željeni način događaj ingestion za strujanje analize. Dodatne informacije o događajima koncentratora u [dokumentaciji Bus servisa Azure](/documentation/services/service-bus/).

Da biste stvorili koncentratora za događaj:

1.  [Portal za Azure](https://manage.windowsazure.com/)kliknite **NOVO** > **Aplikacije SERVISA** > **BUS SERVISA** > **DOGAĐAJ KONCENTRATOR** > **Brzo stvaranje**. Navedite naziv, regija i prostora za novi ili postojeći naziv da biste stvorili novi događaj koncentrator.  
2.  Kao preporučenim načinom rada svaki zadatak analize strujanje namijenjen iz grupe potrošača koncentrator jedan događaj. Ne možemo će vas voditi kroz postupak stvaranja grupe potrošača kasnije. [Dodatne informacije o grupama potrošača](https://msdn.microsoft.com/library/azure/dn836025.aspx). Da biste stvorili grupu korisnika, otvorite koncentrator novostvoreni događaja kliknite karticu **KORISNIČKE GRUPE** , kliknite **Stvori** na dno stranice, a zatim navedite naziv za grupu korisnika.
3.  Da biste dodijelili pristup središtu događaj, moramo stvoriti pravilo zajednički pristup. Kliknite karticu **KONFIGURACIJA** koncentratora za događaj.
4.  U odjeljku **Pravila za zajedničko korištenje programa ACCESS**, stvaranje novog pravila koja ima dozvole za **Upravljanje** .

    ![Zajedničko korištenje gdje možete stvoriti pravila s dozvolama za upravljanje pravilnike za pristup.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  Kliknite **SPREMI** pri dnu stranice.
6.  Idite na **nadzornu ploču**, kliknite **Informacije o VEZI** u donjem desnom kutu stranice i zatim kopirajte i spremiti podatke o vezi.

## <a name="configure-and-start-the-event-generator-application"></a>Konfiguraciju i pokretanje aplikacija generator događaja

Ne možemo je navesti klijentska aplikacija koja će generiranje uzorka dolazni poziv metapodataka i automatske s koncentratorima događaj. Poduzmite sljedeće korake da biste postavili te aplikacije.  

1.  Preuzimanje [datoteke TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)i raspakirajte direktoriju.

    > [AZURE.NOTE] Windows možda blokira zip preuzete datoteke. Desnom tipkom miša kliknite datoteku, a zatim odaberite **Svojstva**. Ako se prikaže "datoteka stigla s drugog računala i možda je blokirana da biste zaštitili ovo računalo" poruka, potvrdite okvir **Unblock** , a zatim kliknite Primijeni na zip datoteke.

2.  Zamjena vrijednosti Microsoft.ServiceBus.ConnectionString i EventHubName u telcodatagen.exe.config niz za povezivanje koncentrator događaja i naziv.

    Niz za povezivanje koju ste kopirali iz Azure portal smjestiti naziv veze na kraju. Ne zaboravite da biste uklonili "; EntityPath =<value>"iz na" Dodavanje ključa = "polje.

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


## <a name="create-a-stream-analytics-job"></a>Stvaranje zadatka strujanje Analytics
Imamo strujanje Telekomunikacije događaja, ne možemo možete postaviti posao strujanje analize da biste analizirali te događaje u stvarnom vremenu.

### <a name="provision-a-stream-analytics-job"></a>Dodjela resursa za posao strujanje Analytics

1.  Na portalu Azure kliknite **NOVO** > **DATA SERVICES** > **Strujanje ANALIZE** > **Brzo stvaranje**.
2.  Navedite sljedeće vrijednosti, a zatim **STVORITE zadatak ANALIZE strujanje**:

    * **Naziv ZADATKA**: Unesite naziv zadatka.

    * **REGIJA**: Odaberite mjesto na koje želite pokrenuti posla regiju. Razmotrite smještanje posla i događaja središte na istom području da bi bolje performanse, a da biste bili sigurni da vam će neće biti plaćanja za prijenos podataka između područja.

    * **RAČUN za POHRANU**: Odaberite račun za Azure prostora za pohranu koji želite koristiti za pohranu nadzora podataka za sve zadatke strujanje analize koji se izvode u tom području. Imate mogućnost da biste odabrali postojeći račun za pohranu ili stvorite novi.

3.  Kliknite **ANALITIKA strujanje** u lijevom oknu na popisu poslova strujanje analize.

    ![Ikona usluge za strujanje Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    Prikazat će se novi zadatak sa statusom **STVORENA**. Obratite pozornost na to je onemogućen gumb **START** na dno stranice. Prije no što počnete posla već morate konfigurirati unos posla, izlazne i upit.

### <a name="specify-job-input"></a>Odredite unos zadatka
1.  U svoj posao strujanje analize kliknite **UNOSA** pri vrhu stranice, a zatim **Dodajte unos**. U dijaloškom okviru koji će se otvoriti će vas voditi kroz nekoliko koraka da biste postavili unos.
2.  Kliknite **STRUJANJA podataka**, a zatim na desni gumb.
3.  Kliknite **DOGAĐAJ KONCENTRATOR**, a zatim na desni gumb.
4.  Upišite ili odaberite sljedeće vrijednosti na trećoj stranici:

    * **PSEUDONIM za unos**: Upišite neslužbeni naziv, kao što su *CallStream*za ovaj zadatak. Imajte na umu da ćete koristiti taj naziv u upitu kasnije.

    * **KONCENTRATOR za DOGAĐAJ**: Ako je središtu događaj koji ste stvorili u okviru iste pretplate kao posao strujanje analize, odaberite prostora za naziv koji se nalazi središtu događaja u.

        Ako je koncentratora za događaj u neku drugu pretplatu, odaberite **Koristi događaj središte na drugu pretplatu**, a ručno unesite podatke za **Polje naziva BUS SERVISA**, **Naziv događaja KONCENTRATOR**, **Naziv pravila KONCENTRATOR događaja**, **KLJUČ pravila KONCENTRATOR za DOGAĐAJ**i **COUNT PARTICIJA KONCENTRATOR za DOGAĐAJ**.

    * **Naziv događaja KONCENTRATOR**: odaberite naziv koncentratora za događaj.

    * **Naziv pravila KONCENTRATOR događaja**: Odaberite pravila koncentrator događaj koji ste stvorili ranije u ovom praktičnom vodiču.

    * **DOGAĐAJ KONCENTRATOR KORISNIČKE GRUPE**: upišite naziv grupe korisnika koju ste stvorili ranije u ovom praktičnom vodiču.

5.  Kliknite gumb desno.
6.  Navedite sljedeće vrijednosti:

    * **OBLIKOVANJE SERIJALIZATORA DOGAĐAJ**: JSON
    * **Kodiranje**: UTF8
7.  Kliknite gumb **PROVJERI** da biste dodali izvoru i da biste provjerili možete li strujanje Analytics uspješno povezati s koncentratora za događaj.

### <a name="specify-job-query"></a>Odredite upit poslova

Analitički strujanje podržava modela jednostavne, deklarativno upit koji opisuje transformacije za obradu u stvarnom vremenu. Da biste saznali više o jeziku, potražite u članku [Referenca jezik za Azure strujanje analize Query](https://msdn.microsoft.com/library/dn834998.aspx). Pomoću ovog praktičnog vodiča pomoći će vam autora i testiranje nekoliko upita putem vaše u stvarnom vremenu strujanja podataka poziv.

#### <a name="optional-sample-input-data"></a>Neobavezno: Ogledne ulazne podatke
Da biste provjerili valjanost upita na temelju podataka stvarni posao, možete koristiti značajku **OGLEDNIH podataka** možete izdvojiti događaje iz vaše strujanje i stvoriti na. Datoteka JSON događaje za testiranje.  Na sljedeći način prikažite kako to učiniti. Ne možemo je navesti [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) Ogledna datoteka za testiranje.

1.  Odaberite unos koncentrator događaja, a zatim **OGLEDNE podatke** pri dnu stranice.
2.  U dijaloškom okviru odredite **vrijeme POČETKA** da biste pokrenuli prikupljanje podataka i **TRAJANJE** koliko dodatne podatke za korištenje.
3.  Kliknite gumb **PROVJERI** pokretanje uzorkovanje podataka iz unos.  To može potrajati minutu ili dvije podatkovne datoteke koja se treba proizvesti.  Po dovršetku postupka kliknite **DETALJI**, preuzmite na generirani. JSON datoteku i spremite je.

    ![Preuzimanje i spremanje podataka iz obrađeni u datoteci JSON](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Prolazni upit

Ako želite arhivirati svaki događaj prolazni upit možete koristiti da biste pročitali sva polja u opseg događaj ili poruke. Da biste započeli, učinite jednostavne prolazni upit projekata sva polja u događaj.

1.  Kliknite **UPIT** s vrha stranice posao strujanje analize.
2.  Uređivač kod dodajte sljedeće:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Provjerite podudara li se naziv izvora unos naziva unos koji ste prethodno naveli.

3.  Kliknite **Testiraj** u uređivaču upita.
4.  Navedite datoteka za testiranje. Koristite onaj koji ste stvorili pomoću prethodnih koraka ili [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Kliknite gumb **PROVJERI** , te rezultate pregledati ispod definicije upita.

    ![Rezultati definicije upita](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Stupac projekcije

Možemo sada ćete smanjiti vraćeni polja u skupu manje.

1.  Promjena upita u uređivaču kod da biste:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.

    ![Izlaz u uređivaču upita.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Broj dolazni pozivi po regijama: Tumbling prozor s zbrajanja

Da biste usporedili broj dolazne pozive po regijama ćemo pomoću [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) Brojanje dolazne pozive grupirane **SwitchNum** svakih pet sekundi.

1.  Promjena upita u uređivaču kod da biste:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Ovaj upit koristi **Vremensku oznaku po** ključnoj riječi da biste naveli polje s vremenskom oznakom opseg će se koristiti u vremenski izračuni. Ako to polje nije naveden, prikaz u prozorima operacija se želite izvršiti pomoću vrijeme koje svaki događaj stigli na središte događaj. U odjeljku ["Kašnjenja vrijeme Dodavanje veze za vanjskih aplikacija vrijeme" u analize strujanje upit jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Imajte na umu da mogu pristupati vremenske oznake za kraj svakog prozora pomoću svojstvo **System.Timestamp** .

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.

    ![Rezultati upita za Timestand po](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Otkrivanje prijevare SIM s na samostalno uključivanje

Da biste odredili potencijalno lažno korištenje, ne možemo ćete potražite pozive koja proizlazi iz istog korisnika, ali na različitim mjestima za manje od 5 sekundi.  Ne možemo [spoj](https://msdn.microsoft.com/library/azure/dn835026.aspx) strujanje događaja poziva sa samim sobom da biste provjerili tim slučajevima.

1.  Promjena upita u uređivaču kod da biste:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Kliknite **ponovno pokrenite** u uređivaču upita da biste vidjeli rezultate upita.

    ![Rezultati upita dio spoja](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Stvaranje primatelj Izlaz

Sad kad ste definirali strujanje događaja, unos za ingest događaja i upit za izvođenje transformaciju toka, koncentratora za događaj posljednji korak je da biste definirali je primatelj izlaz za posao. Ne možemo ćete pisati događaje lažno ponašanje spremište blobova platforme Azure.

Poduzmite sljedeće korake da biste stvorili spremnik za spremište blobova platforme ako ga već nemate.

1.  Koristi postojeći račun za pohranu ili stvorite novi račun za pohranu tako da kliknete **NOVO > podatkovne usluge > SPREMANJE > brzo stvaranje**, i slijedite upute.
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

## <a name="start-job-for-real-time-processing"></a>Pokretanje zadatka za obradu u stvarnom vremenu

Jer unos posla, upita i izlazni sve nisu navedeni, ne možemo spremni za početak posao strujanje Analytics za otkrivanje prijevarama dostupne su u stvarnom vremenu.

1.  Iz posla **nadzorne PLOČE**, kliknite **pokretanje** pri dnu stranice.
2.  U dijaloškom okviru koji će se otvoriti kliknite **vrijeme POČETKA POSLA**, a zatim kliknite gumb **PROVJERI** pri dnu dijaloškog okvira. Status zadatka promijenit će se **Početni** i promijenit će se uskoro **pokrenut**.

## <a name="view-fraud-detection-output"></a>Prikaz prijevaru otkrivanje Izlaz

Pomoću alata kao što su [Azure prostora za pohranu Explorer](http://storageexplorer.com/) ili [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) da biste pogledali lažno događaje kao što su zapisuju Izlaz u stvarnom vremenu.  

![Otkrivanje prijevare: lažno događaje prikazati u stvarnom vremenu](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Podrška
Za dodatnu pomoć, pokušajte našem [forumu Azure strujanje analize](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
