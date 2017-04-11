<properties
    pageTitle="Stvaranje rješenja programa IoT pomoću analize strujanje | Microsoft Azure"
    description="Početak rada vodič za strujanje analize IoT rješenje tollbooth scenarija"
    keywords="iot rješenja, prozor funkcije"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Stvaranje rješenja programa IoT pomoću strujanje Analytics

## <a name="introduction"></a>Uvod

U ovom ćete praktičnom vodiču će Saznajte kako koristiti Azure strujanje analize da biste dobili uvid u stvarnom vremenu iz podataka. Razvojni inženjeri jednostavno možete kombinirati strujanja podataka, kao što su kliknite strujanja, zapisnika i događaje koje generira uređaj s povijesnim zapisa ili referentnih podataka za dobivanje glavne uvida tvrtke. Kao izračuni servisa strujanje potpuno upravljanog, u stvarnom vremenu, koji se nalazi u Microsoft Azure Azure strujanje Analytics nudi ugrađene otpornost, niske latencije i skalabilnost da biste dobili ste prema gore i pokretanje u minutama.

Po dovršetku ovog praktičnog vodiča će moći:

-   Upoznajte se s portala za Azure strujanje analize.
-   Konfiguriranje i implementacija strujanje posao.
-   Articulate stvarnog života probleme i rješavanje ih pomoću jezika za upite strujanje analize.
-   Razvijajte strujanje rješenja za klijente pomoću analize strujanje pouzdano.
-   Koristite na nadzora i bilježenja sučelje za otklanjanje poteškoća.

## <a name="prerequisites"></a>Preduvjeti

Potrebno je sljedeće preduvjete za dovršetak ovog praktičnog vodiča:

-   Najnoviju verziju programa [Azure PowerShell](../powershell-install-configure.md)
-   Visual Studio 2015 ili besplatne [Visual Studio zajednice](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
-   [Azure pretplate](https://azure.microsoft.com/pricing/free-trial/)
-   Administratorske ovlasti na računalu
-   Preuzimanje [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) iz Microsoftova centra za preuzimanje
-   Neobavezno: Izvornog koda za događaj generator TollApp u [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Scenarij Uvod: "Zdravo, naknada!"


Broj stanice je uobičajenih phenomenon. Naiđete na njih na mnogo expressways, bridges i tunnels širom svijeta. Svaki broj stanice ima više booths naknada. Na ručno booths prestanete platiti u naknada za govorni Automat. Pri automatiziranog booths senzora pri vrhu svake štandu pregledava karticu sustava RFID je ponuđenima windshield vaše vozila kao što prođete štandu naknada. Jednostavno je vizualizacija dugačak postizanja kroz ove stanice naknada kao na događaj strujanje putem kojeg zanimljive operacije može izvoditi.

![Slika s automobilima na booths naknada](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Ulazne podatke

Pomoću ovog praktičnog vodiča funkcionira s dva strujanja podataka. Senzori instaliran u uvodne i završne stanice naknada proizvesti prvi toka. Drugi tok nije dataset statične pretraživanja koja sadrži podatke registracije vozila.

### <a name="entry-data-stream"></a>Unos podataka strujanje

Unos podataka toka sadrži informacije o automobili čim uđu naknada stanice.


| TollID | EntryTime               | LicensePlate | Stanje | Provjerite   | Model   | VehicleType | VehicleWeight | Broj | Oznaka       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 12:01:00.000 2014. 09 10 | JNB 7001     | NY    | Honda  | CRV     | 1           | 0             | 7    |           |
| 1      | 12:02:00.000 2014. 09 10 | YXZ 1001     | NY    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 12:02:00.000 2014. 09 10 | ABC 1004     | CT    | Ford   | Taurus  | 1           | 0             | 5    | 456789123 |
| 2      | 12:03:00.000 2014. 09 10 | NEKI 1003     | CT    | Toyota | Corolla | 1           | 0             | 4    |           |
| 1      | 12:03:00.000 2014. 09 10 | BNJ 1007     | NY    | Honda  | CRV     | 1           | 0             | 5    | 789123456 |
| 2      | 12:05:00.000 2014. 09 10 | CDE 1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Ovdje je kratak opis stupaca:

| Stupac | Opis |
|--------------|--------------------------------------------------------------------|
| TollID       | ID štandu naknada koje služi kao jedinstvena identifikacija naknada štandu            |
| EntryTime    | Datum i vrijeme unosa vozila na štandu naknada u UTC-u |
| LicensePlate | Broj licenci ploča vozila                            |
| Stanje        | Stanje u Sjedinjenim Američkim Državama                                           |
| Provjerite         | Proizvođač na automobili                                 |
| Model        | Broj modela na automobili                                 |
| VehicleType  | Neki 1 za vozila putnika ili 2 za komercijalne vozila       |
| WeightType   | Težina vozila u tonama; 0 za putnika vozila                   |
| Broj         | Broj vrijednosti u USD                                              |
| Oznaka          | E-oznake na automobili za automatiziranje plaćanja; prazno gdje je vrijednost uplata učinjeno ručno |


### <a name="exit-data-stream"></a>Strujanja podataka za izlaz

Izlaz toka podataka sadrži informacije o automobilima ostavite stanice naknada.


| **TollId** | **ExitTime**                 | **LicensePlate** |
|------------|------------------------------|------------------|
| 1          | 2014.-09-10T12:03:00.0000000Z | JNB 7001         |
| 1          | 2014.-09-10T12:03:00.0000000Z | YXZ 1001         |
| 3          | 2014.-09-10T12:04:00.0000000Z | ABC 1004         |
| 2          | 2014.-09-10T12:07:00.0000000Z | NEKI 1003         |
| 1          | 2014.-09-10T12:08:00.0000000Z | BNJ 1007         |
| 2          | 2014.-09-10T12:07:00.0000000Z | CDE 1007         |

Ovdje je kratak opis stupaca:


| Stupac | Opis |
|--------------|-----------------------------------------------------------------|
| TollID       | ID štandu naknada koje služi kao jedinstvena identifikacija naknada štandu         |
| ExitTime     | Datum i vrijeme izlaza vozila iz same naknada u UTC-u |
| LicensePlate | Broj licenci ploča vozila                         |

### <a name="commercial-vehicle-registration-data"></a>Komercijalna vozila Registracija podataka

Vodič koristi statična snimka komercijalne vozila Registracija baze podataka.


| LicensePlate | Atributima RegistrationId | Istekla |
|--------------|----------------|---------|
| SVT 6023     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| PO 1005     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Ovdje je kratak opis stupaca:


| Stupac | Opis |
|--------------|-----------------------------------------------------------------|
| LicensePlate | Broj licenci ploča vozila                         |
| Atributima RegistrationId     | ID-a za registraciju na vozila |
| Istekla | Status Registracija vozila: 0 Ako registracija vozila je aktivna, 1 ako Registracija je istekla                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Postavljanje okruženja za analizu strujanje Azure

Da biste dovršili ovaj Praktični vodič, morate pretplatu na Microsoft Azure. Microsoft pruža besplatnu probnu verziju servisa Microsoft Azure.

Ako nemate račun za Azure, možete ga [zahtjev za besplatnu probnu verziju](http://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Da biste prijavite se za besplatnu probnu verziju, morate mobilnog uređaja koji možete primati SMS-ove i valjani kreditne kartice.

Ne zaboravite, tako da unesete najbolje koristi vaš $ 200 slobodno Azure kreditne kartice, slijedite korake u odjeljku "Čišćenja račun za Azure" na kraju ovog članka.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Azure resursi koji su potrebni za vodič za dodjelu resursa

Pomoću ovog praktičnog vodiča potrebna dva događaja koncentratora za primanje strujanja podataka *stavke* i *izlazak iz njega* . Baze podataka SQL Azure proizvodi rezultate analize strujanje zadataka. Azure prostora za pohranu pohranjuje referentnih podataka o vozila registracije.

Skripta Setup.ps1 u mapi TollApp na GitHub možete koristiti da biste stvorili sve potrebne resurse. Svrhu vrijeme, preporučujemo da ga pokrenuti. Ako želite da biste saznali više o konfiguriranju ove resurse na portalu za Azure, pogledajte dodatak "Konfiguriranje vodiča resursi Azure portalu".

Preuzmite i spremanje podrške [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) mape i datoteke.

Otvorite u **Programu Microsoft Azure PowerShell** prozora _kao administrator_. Ako još nemate Azure PowerShell, slijedite upute u [instalirate i konfigurirate Azure PowerShell](../powershell-install-configure.md) da biste ga instalirali.

Jer Windows automatski blokira .ps1, .dll i .exe datoteke, morate postaviti Pravilnik za izvršavanje pokrenuti skriptu. Provjerite je li instaliran prozoru Azure PowerShell _kao administrator_. Pokrenite **Postavljanje-pravilnik izvođenja neograničen**. Kad se to od vas zatraži, unesite **Y**.

![Snimka zaslona "Postavljanje-pravilnik izvođenja neograničen" izvodi u prozoru Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Pokrenite **Get-pravilnik izvođenja** da biste bili sigurni da je radio naredbu.

![Snimka zaslona "Get-pravilnik izvođenja" izvodi u prozoru Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Idite u direktoriju skripte i generator aplikacije.

![Snimka zaslona "cd .\TollApp\TollApp" izvodi u prozoru Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Vrsta **.\\ Setup.ps1** da biste postavili račun za Azure, stvaranje i konfiguriranje sve potrebne resurse i početak da biste generirali događaja. Skripta slučajno uzima područja da biste stvorili resurse. Izričito navesti područja, može proći u **– mjesto** parametar kao u sljedećem primjeru:

**. \\Setup.ps1-mjesto "Središnje sad"**

![Snimka zaslona Azure stranice za prijavu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Skripta otvorit će se stranica za **Prijavu u** za Microsoft Azure. Unesite vjerodajnice za račun.

> [AZURE.NOTE] Ako vaš račun ima pristup višestruke pretplate, koji će se tražiti da biste unijeli naziv pretplate koju želite koristiti za vodič.

Skripta može potrajati nekoliko minuta da biste pokrenuli. Kada završi, rezultat će izgledati kao sljedeće snimku zaslona.

![Snimka zaslona izlaza skripte u prozoru Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Prikazat će se i neki drugi prozor koja je slična sljedeće snimku zaslona. Ovu aplikaciju šalje događaje s koncentratorima Azure događaja koji je potreban za pokretanje vodič. Tako, ne zaustavlja se aplikacija ili zatvorite prozor dok ne završite vodič.

![Snimka zaslona "Slanje događaj koncentrator data"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Trebali biste moći sada vidjeti sve stvorene resurse Azure portalu. Idite na <https://manage.windowsazure.com>i prijavite se pomoću vjerodajnica za račun.

### <a name="azure-event-hubs"></a>Azure događaj koncentratora

Kliknite **KNJIŽNA SERVISA** na lijevoj strani Azure portal da biste vidjeli koncentratora događaj koji skripte stvorili u prethodnom odjeljku.

![Servis Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Prikazat će se svi se prostori naziva dostupno za pretplatu. Odaberite onaj koji započinje *tolldata* (tolldata4637388511 u našem primjeru). Kliknite karticu **DOGAĐAJ KONCENTRATORA** .

![Kartica koncentratorima događaja na portalu za Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Prikazat će se dva događaja koncentratorima imenovane *stavke* i *izlazak iz njega* stvorili u ovo polje naziva.

![Snimka zaslona "unos" i "Izlaz" koncentratora događaja na portalu za Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure spremnik za pohranu

1. Kliknite **prostor za POHRANU** na lijevoj strani portala za Azure da biste vidjeli spremnik Azure prostora za pohranu koji se koristi u ovom praktičnom vodiču.

    ![Stavka izbornika za pohranu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Kliknite onaj koji počinju *tolldata* (tolldata4637388511 u našem primjeru). Kliknite karticu **SPREMNIKA** da biste vidjeli spremnik stvoren.

    ![Kartica spremnika na portalu za Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Kliknite kontejner **tolldata** da biste pogledali prenesene JSON datoteku koja sadrži podatke registracije vozila.

    ![Snimka zaslona registration.json datoteku u spremniku](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Baze podataka Azure SQL

1. Kliknite **Baze podataka SQL** na lijevoj strani portala za Azure da biste vidjeli SQL baze podataka koja će se koristiti u ovom praktičnom vodiču.

    ![Baze podataka SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Kliknite **tolldatadb**.

    ![Snimka zaslona stvorenu SQL baze podataka](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. Kopiranje i naziva poslužitelja bez broj priključka (*naziv poslužitelja*. database.windows.net, na primjer).

## <a name="connect-to-the-database-from-visual-studio"></a>Povezivanje s bazom podataka iz Visual Studio

Pomoću programa Visual Studio da biste pristupili rezultate upita u bazi podataka za izlaz.

Povezivanje s bazom podataka SQL (odredište) iz Visual Studio:

1. Otvorite Visual Studio, a zatim kliknite **Alati** > **za povezivanje s bazom podataka**.

2. Ako se od vas zatraži, kliknite **Microsoft SQL Server** kao izvor podataka.

    ![Dijaloški okvir Promjena izvora podataka](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. U polje **naziv poslužitelja** zalijepite naziv koji ste kopirali u prethodnom odjeljku na portalu Azure (to jest, *naziv poslužitelja*. database.windows.net).

4. Kliknite **Provjera autentičnosti pomoću SQL poslužitelja**.

5. Unesite **tolladmin** u polje **korisničko ime** i **123toll!** u polje za **lozinku** .

6. Kliknite **Odaberite ili unesite naziv baze podataka**, a **TollDataDB** kao odaberite bazu podataka.

    ![Dodavanje dijaloški okvir veza](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Kliknite **u redu**.

8. Otvorite Eksplorer za poslužitelj.

    ![Poslužitelj programa Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Pogledajte četiri tablice u bazi podataka TollDataDB.

    ![Tablica u bazi podataka TollDataDB](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Generator događaj: TollApp uzorka projekta

Skriptu PowerShell automatski se pokreće da biste poslali događaja pomoću programa za aplikaciju TollApp uzorka. Ne morate izvršiti dodatnih koraka.

Međutim, koji vas zanima Detalji o implementaciji, možete pronaći šifru izvora TollApp aplikacije u GitHub [Uzoraka/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Snimka zaslona ogledni kod koji se prikazuje u Visual Studio](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Stvaranje zadatka strujanje Analytics

1. Na portalu Azure otvorite strujanje analize i kliknite **NOVO** u donjem lijevom kutu stranice da biste stvorili novi zadatak analize.

    ![Gumb novi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Kliknite **Stvori brzo**. Odaberite gdje ostali resursi stvaraju skriptu isti regiju.

3. Za postavku **REGIONALNE NADZOR prostora za POHRANU RAČUNA** odaberite **Stvori novi RAČUN za POHRANU** i dajte mu jedinstveni naziv. Azure strujanje analize će koristiti taj račun za spremanje informacija za nadzor za sve buduće poslove.

4. Kliknite **Stvaranje ZADATKA strujanje ANALIZE** pri dnu stranice.

    ![Stvaranje mogućnost strujanje analize posla](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Definirajte izvore za unos

1. Kliknite zadatak stvoren analize na portalu.

    ![Snimka zaslona s posla analize na portalu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. Kliknite karticu **UNOSA** za definiranje izvora podataka.

    ![Na kartici unosa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Kliknite **DODAJ ULAZNI**.

    ![Dodavanje unosa mogućnost](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. Na prvoj stranici kliknite **STRUJANJA podataka** .

    ![Mogućnost strujanja podataka](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. Kliknite **DOGAĐAJ SREDIŠTE** na drugoj stranici čarobnjaka.

    ![Mogućnost koncentratora za događaj](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. Unesite **EntryStream** kao **PSEUDONIM za unos**.

7. Kliknite **Događaj koncentrator** padajući popis i odaberite onaj koji započinje rečenicom "TollData" (na primjer, TollData9518658221).

8. Odaberite **stavku** kao naziv događaja koncentratora i **svih** kao naziv događaja koncentrator pravila.

    Postavke će izgledati:

    ![Postavke koncentrator za događaj](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. Na sljedećoj stranici odaberite **JSON** za **DOGAĐAJ SERIJALIZIRANOG OBLIKA** i **UTF8** za **kodiranje**.

    ![Postavke serijalizacije](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Kliknite **u redu** pri dnu stranice da biste dovršili Čarobnjak.

    ![Snimka zaslona EntryStream unos na portalu za Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Sad kad ste stvorili unos toka, će poduzmite iste korake da biste stvorili strujanje Izlaz. Na trećoj stranici čarobnjaka, obavezno unesite vrijednosti kao na sljedeće snimku zaslona.

    ![Postavke za strujanje Izlaz](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    Dva unosa strujanja ste definirali:

    ![Definirani unos strujanja na portalu za Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Nakon toga će dodati unosa podataka referenca za blob datoteka koja sadrži podatke za registraciju automobila.

11. Kliknite **Dodavanje unos**, a zatim **REFERENTNIH podataka**.

    !["Dodaj ulazni" s mogućnostima odabrani su podaci u referenci](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. Na sljedećoj stranici odaberite račun za pohranu koji započinje **tolldata**. Naziv spremnika mora biti **tolldata**, a naziv blob u odjeljku **Put uzorak** mora biti **registration.json**. Ovaj naziv datoteke velika i mala slova i mora biti mala slova.

    ![Postavke za pohranu bloga](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Odaberite vrijednosti, kao što je prikazano u sljedećim snimka zaslona na sljedećoj stranici, a zatim **u redu** da biste dovršili Čarobnjak.

    ![Odabir JSON "čak serijaliziranog oblika" i UTF8 za "Kodiranje"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Sada se definiraju sve unose.

![Snimka zaslona s tri definirani unosa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Definiranje Izlaz

1. Kliknite karticu **IZLAZNI** , a zatim kliknite **DODAJ programa IZLAZ**.

    ![Na kartici izlazni i mogućnošću "Dodavanje programa Izlaz"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Kliknite **SQL baze podataka**.

3. Odaberite naziv poslužitelja koja se koristila u odjeljku "Povezivanje za bazu podataka iz Visual Studio" u članku. Naziv baze podataka je **TollDataDB**.

4. Unesite **tolladmin** u polje **USERNAME** **123toll!** u polje **LOZINKE** i **TollDataRefJoin** u polju **TABLICE** .

    ![Postavke baze podataka SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure strujanje analize upita

Na kartici **UPIT** sadrži SQL upit koji pretvara ulazne podatke.

![Upit dodali na karticu upita](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

Pomoću ovog praktičnog vodiča pokušava pitanja nekoliko tvrtke koji se odnose na uz plaćanje upita strujanje analize podataka i konstrukta koje je moguće koristiti u Azure strujanje Analytics omogućuje odgovarajući odgovor.

Prije nego što počnete prvi analize toka posla, Upoznajmo nekoliko scenarija i sintaksu upita.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Uvod u Azure strujanje analize jezik upita
-----------------------------------------------------

Recimo da morate prebrojiti vozila koje unosite broj štandu. Budući da je neprekinuti strujanje događaja, potrebno je definirati "vremensko razdoblje." Pogledajmo izmijeniti pitanje biti "koliko vozila unesite broj štandu svake tri minute?". To je obično se nazivaju tumbling count.

Pogledajmo Azure strujanje analize upita koja odgovara ovo pitanje:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Kao što vidite, Azure strujanje analize koristi jezika za upite koji je poput SQL i dodaje nekoliko proširenja da biste naveli vrijeme povezane aspekte upit.

Dodatne informacije, Saznajte više o [Upravljanje vremenom](https://msdn.microsoft.com/library/azure/mt582045.aspx) i [Prikaz u prozorima](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstrukta koriste u upitu MSDN.

## <a name="testing-azure-stream-analytics-queries"></a>Testiranje Azure strujanje analize upita

Sad kad ste napisali prvi Azure strujanje analize upit, vrijeme je da biste testirali pomoću ogledne podatkovne datoteke koja se nalazi u mapi TollApp u sljedeći put:

**.. \\TollApp\\TollApp\\podataka**

Ova mapa sadrži sljedeće datoteke:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Pitanje 1: Broj vozila unos naknada štandu

1. Otvorite portal sustava Azure i idite na stvoreni posla Azure strujanje analize. Kliknite karticu **UPITA** i zalijepite upit iz prethodnog odjeljka.

    ![Upit koji se zalijepiti na kartici upit](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. Da biste provjerili valjanost ovaj upit s oglednim podacima, kliknite gumb za **testiranje** . U dijaloškom okviru koji će se otvoriti, idite na Entry.json, datoteku koja sadrži ogledne podatke iz **EntryTime** strujanje događaja.

    ![Snimka zaslona Entry.json datoteka](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Provjeriti je li izlaz upita prema očekivanjima:

    ![Rezultati test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Pitanje 2: Izvješće Ukupno vrijeme za svaki automobil tako da prolazi kroz štandu naknada

Prosječno vrijeme koje je potrebno za automobil tako da prolazi kroz na naknada olakšava procijenite učinkovitost postupak i iskustva kupca.

Da biste pronašli Ukupno vrijeme, morate uključiti strujanje EntryTime s ExitTime toka. Će uključiti strujanja TollId i LicencePlate stupaca. Operator **Uključivanje** potrebno navesti vremenski leeway koji opisuje prihvatljiva vrijeme razlika između spojene događaja. **Funkcija DateDiff** će koristiti da biste odredili događaja mora biti više od 15 minuta međusobno. Će se primijeniti i **Funkcija DateDiff da biste izašli iz** i unos vremena za izračunavanje Stvarno vrijeme koje automobila provodi u stanice naknada. Kada se koristi u **Odaberite** naredbu umjesto uvjeta **Uključivanje** Imajte na umu razlika korištenja **DATEDIFF** .

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Da biste testirali upit, ažuriranje upita na kartici **UPIT** vaš posao:

    ![Ažurirani upita na kartici upit](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Kliknite **Testiraj** i navedite ogledne datoteke za unos za EntryTime i ExitTime.

    ![Snimka zaslona s odabranih unosa datoteka](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Odaberite potvrdni okvir da biste testirali upit i prikazali izlaz:

    ![Izlaz testiranja](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Pitanje 3: Izvješće sve postizanja komercijalne s istekle Registracija

Azure strujanje analize pomoću statičke snimke podataka uključivanje putem strujanja vremenski podataka. Da bismo pokazali tu mogućnost, koristite sljedeće ogledne pitanje.

Ako je komercijalne vozila registriran s naknada tvrtke, ga mogu prolaziti kroz štandu naknada bez zaustavlja radi provjere. Tablica za traženje komercijalne vozila Registracija će se koristiti za identificiranje sve postizanja komercijalne koja je istekla registracije.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Da biste testirali upit pomoću referentnih podataka, morate definirati ulazni izvor za referentnih podataka koje ste napravili već.

1. Da biste testirali upit, zalijepite upita na kartici **UPIT** , kliknite **testiranje**i navedite dva unosa izvora:

    ![Snimka zaslona s odabranih unosa datoteka](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. Prikaz izlaz upita:

    ![Snimka zaslona izlaz upita](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Pokretanje analize strujanje zadatka


Sada je vrijeme da biste dovršili konfiguraciju i pokretanje zadatka. Spremite upit iz pitanje 3, koji će stvoriti izlaza koja odgovara sheme **TollDataRefJoin** izlaznu tablicu.

Idite na poslu **nadzorne PLOČE**, a zatim kliknite **START**.

![Snimka zaslona gumba Start na nadzornoj ploči posla](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

U dijaloškom okviru **Prilagođeno**vrijeme promijeniti vrijeme **Pokretanje IZLAZ** . Da biste promijenili sat na jedan sat prije trenutnog vremena. Ta promjena osigurava Svi događaji u središtu događaj obrađuju jer ste pokrenuli da biste generirali događaje na početku vodič. Sada kliknite kvačicu da biste pokrenuli posao.

![Odabir prilagođene vremena](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Pokretanje posao može potrajati nekoliko minuta. Na stranici najviše razine možete vidjeti status analitičkih strujanje.

![Snimka zaslona stanje posla](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>Rezultati provjere u Visual Studio

1. Otvorite Eksplorer za poslužitelj za Visual Studio pa desnom tipkom miša kliknite tablicu **TollDataRefJoin** .
2. Kliknite **Prikaz podatkovne tablice** da biste vidjeli rezultat posla.

    ![Odabir "Prikaži tablicu podataka" u programu Explorer poslužitelja](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Promjena veličine izgleda Azure strujanje analize poslova

Azure strujanje analize namijenjen je elastically mjerilo tako da možete rukovati velike količine podataka. Upit Azure strujanje analize pomoću uvjet **PARTICIJE tako da** upućuje sustav koji ovaj korak će promijeniti veličinu izgleda. **PartitionId** je posebno stupac koji sustav dodaje tako da odgovara ID particije unos (koncentratora za događaj).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Zaustavljanje trenutnog posla, upit na kartici **UPIT** za ažuriranje i otvorite karticu **MJERILO** .

    **Strujanje JEDINICE** odrediti količinu power računalnim koji možete primati posao.

2. Pomaknite klizač na 6.

    ![Snimka zaslona s odabirom 6 strujanje jedinice](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Idite na karticu **IZLAZE** i promijenite naziv tablice SQL u **TollDataTumblingCountPartitioned**.

Ako pokrenete posao sada Azure strujanje analize možete raspodijelite rad više računalnim resursa i postigli bolje propusnost. Napominjemo da TollApp aplikacije i šalje događaje particije po TollId.

## <a name="monitor"></a>Monitora

Na kartici **MONITOR** nalaze statističkih podataka o izvodi posao.

![Snimka zaslona statističkih podataka o izvođenje zadataka](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

**Zapisnik postupka** možete pristupiti s kartice **nadzorne PLOČE** .

![Mogućnost "Zapisnik postupka"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Snimka zaslona zapisnik postupka gdje možete vidjeti status poslova](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Da biste vidjeli dodatne informacije o određenom događaj, kliknite događaj, a zatim gumb **pojedinosti** .

![Snimka zaslona detalje o odabranog događaja](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Zaključak

Pomoću ovog praktičnog vodiča uvedena servisa Azure strujanje analize. Ga planirati upute za konfiguriranje unosa i izlaza za posao strujanje analize. Korištenje scenarij broj podataka, vodič objašnjenje uobičajenih vrste problema koje biste prostor podataka u kretanja i kako ih možete otkloniti pomoću jednostavne upite SQL nalik u Azure strujanje analize. Vodič opisane SQL konstrukta proširenja za rad s podacima vremenski. Ga prikazivao kako se uključiti strujanja podataka, kako obogaćivanje tijeku podataka s statične referentnih podataka i upute za promjenu veličine out upita da biste postigli veću propusnost.

Iako ovaj Praktični vodič omogućuje dobar Uvod, nije dovršena na bilo koji način. Možete pronaći dodatne upita uzoraka pomoću jezika SAQL pri [primjeri upita za uobičajene uzoraka korištenja strujanje analize](stream-analytics-stream-analytics-query-patterns.md).
Pogledajte [mrežne dokumentacije](https://azure.microsoft.com/documentation/services/stream-analytics/) da biste saznali više o Azure strujanje analize.

## <a name="clean-up-your-azure-account"></a>Čišćenje račun za Azure

1. Zaustavite posao strujanje analize na portalu za Azure.

    Skripta Setup.ps1 stvara dva koncentratora za događaj i bazom podataka SQL. Sljedeće upute pomoći očistiti resursa na kraju vodič.

2. U prozoru PowerShell upišite **.\\ Cleanup.ps1** da biste pokrenuli skriptu koja briše resursa koji se koriste u praktičnom vodiču.

    > [AZURE.NOTE] Resursi prepoznaju po nazivu. Provjerite je li pažljivo pregledajte svake stavke prije uklanjanja potvrde.

    ![Snimka zaslona postupka čišćenje](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
