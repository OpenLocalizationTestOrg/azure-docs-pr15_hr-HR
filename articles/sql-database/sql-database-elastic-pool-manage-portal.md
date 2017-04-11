<properties
    pageTitle="Nadzor i upravljanje za skup elastic baze podataka pomoću portala za Azure | Microsoft Azure"
    description="Saznajte kako pomoću portala za Azure i ugrađene obavještavanje SQL baze podataka na upravljanje, praćenje i desno veličina skalabilni elastic baze podataka skup optimiziranja performansi baze podataka i upravljanje trošak."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Nadzor i upravljanje za skup elastic baze podataka pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Portal za Azure možete koristiti za nadzor i upravljanje za skup elastic baze podataka i bazama podataka u. S portala sustava možete pratiti korištenjem elastic grupe aplikacija i baze podataka u tu grupu. Provjerite skup promjena vaše elastic resurse i Pošalji sve promjene istovremeno. Te promjene uključuju dodavanjem ili uklanjanjem baze podataka, promijenite postavke elastic skup ili promjena postavki baze podataka.

Grafike u nastavku prikazuje se primjer elastic skup. Prikaz uključuje:

*  Za nadzor korištenja resursa elastic resurse i bazama podataka koje se nalaze u grafikona.
*  Skup gumb **Konfiguracija** , da biste promijenili elastic grupe aplikacija.
*  Gumb za **Stvaranje baze podataka** koji stvara novu bazu podataka i dodaje trenutni elastic resurse.
*  Elastic poslove koje olakšavaju upravljanje velikim brojem baze podataka tako da pokrenete transakcija SQL skripte protiv sve baze podataka na popisu.

![Prikaz grupe aplikacija][2]

Da biste se kroz korake u ovom članku, potreban vam je SQL server na Azure s barem jedna baza podataka i elastic grupe aplikacija. Ako nemate elastic skup potražite u članku [Stvaranje zajedničko područje](sql-database-elastic-pool-create-portal.md); Ako nemate baze podataka, potražite u članku [vodič SQL baze podataka](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Nadzor elastic skup

Možete posjetiti određeni skup da biste vidjeli njezin Upotreba resursa. Prema zadanim postavkama, na resurse je konfiguriran za prikaz prostora za pohranu i eDTU korištenje zadnje sata. Grafikon se može konfigurirati da biste prikazali različite metriku preko različitih prozora vremena.

1. Odaberite grupu za rad s.
2. U odjeljku **Elastic nadzor skup** je grafikon s natpisom **Upotreba resursa**. Kliknite grafikon.

    ![Nadzor elastic skup][3]

    Otvorit će plohu **metriku** prikazuje detaljni prikaz navedeni metriku iznad navedenom vremenskom.   

    ![Metričkim plohu][9]

### <a name="to-customize-the-chart-display"></a>Da biste prilagodili prikaz grafikona

Možete urediti na grafikonu i metrike plohu za prikaz drugih metriku kao što je postotak procesora, postotak IO podataka i zapisnika IO postotak koji se koristi.

2. Na metrike plohu kliknite **Uređivanje**.

    ![Kliknite Uređivanje][6]

- U plohu **Uredi grafikon** odaberite novi raspon vrijeme (prethodnih sata, today, ili prošli tjedan) ili kliknite **Prilagođena** da biste odabrali sve datumskom rasponu u zadnja dva tjedna. Odaberite vrstu grafikona (traka ili retka), a zatim odaberite resurse za praćenje.

> Napomena: Samo metriku s istu mjernu jedinicu mogu se prikazati u grafikonu u isto vrijeme. Na primjer, ako odaberete "eDTU postotak" pa samo moći da biste odabrali druge metriku s postotkom kao mjernu jedinicu.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Zatim kliknite **u redu**.


## <a name="elastic-database-monitoring"></a>Nadzor elastic baze podataka

Pojedinačne baze podataka i moguće nadzirati za potencijalnih problema.

1. U odjeljku **Elastic baze podataka za nadzor**, postoji grafikon koji prikazuje metriku za pet baze podataka. Prema zadanim postavkama, grafikon prikazuje gornji 5 baze podataka u po korištenje Prosječna eDTU u zadnjem satu. Kliknite grafikon.

    ![Nadzor elastic skup][4]

2. Pojavit će se plohu **Upotreba resursa za bazu podataka** . To pruža detaljan prikaz korištenja baze podataka u. Koristite rešetku u donjem dijelu na plohu, možete odabrati sve baze podataka u da biste prikazali njihove upotrebe na grafikonu (do 5 baza podataka). Možete prilagoditi i prozor metriku i vrijeme prikazane u grafikonu tako da kliknete **Uređivanje grafikona**.

    ![Upotreba plohu resursa za bazu podataka][8]

### <a name="to-customize-the-view"></a>Da biste prilagodili prikaz

1. U plohu **Upotreba resursa za bazu podataka** , kliknite **Uređivanje grafikona**.

    ![Kliknite Uređivanje grafikona](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. U grafikon plohu **Uređivanje** odaberite novi raspon vrijeme (proteklih sat ili zadnjih 24 sata) ili kliknite **Prilagođena** da biste odabrali neki drugi dan u zadnjih dva tjedna da bi se prikazao.

    ![Kliknite Prilagođeno](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Kliknite padajući izbornik **Usporedba baza podataka tako** da biste odabrali drugu metriku za korištenje prilikom usporedbe baze podataka.

    ![Uređivanje grafikona](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Da biste odabrali baza podataka za praćenje

Na popisu baza podataka u plohu **Upotreba resursa za bazu podataka** , možete pronaći određenu baze podataka zatrebaju do stranice na popisu ili tako da upišete naziv baze podataka. Koristite okvir za odabir baze podataka.

![Pretraživanje za baze podataka za praćenje][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Dodavanje upozorenja u skup resursa

Resursi za slanje e-pošte osobe ili upozorenja nizova za krajnje točke URL-a kada resurs dodirne Upotreba prag koji ste postavili možete dodati pravila.

> [AZURE.IMPORTANT]Upotreba resursa nadzor za Elastic grupe ima kašnjenja barem 20 minuta. Postavljanje upozorenja manje od 30 minuta za Elastic grupe trenutno nisu podržani. Bilo koji upozorenja postavljena za Elastic grupe točkom (parametar pod nazivom "-WindowSize" u PowerShell API-JA) manje od 30 minuta možda neće se pokrenuti. Provjerite da bilo koje ste definirali za Elastic grupe koriste tijekom razdoblja (WindowSize) 30 minuta ili više njih.

**Da biste dodali upozorenja bilo kojeg resursa:**

1. Kliknite grafikon **Upotreba resursa** da biste otvorili plohu **metrika** , kliknite **Dodaj upozorenja**, a zatim unesite podatke u plohu **dodajte pravilo upozorenja** (**resursa** je automatski postaviti tako da se skup radite s).
2. Upišite **naziv** i **Opis** koja služi za identifikaciju upozorenje da biste vas i primatelja.
3. Odaberite **metrički** koji želite da se upozorenje s popisa.

    Grafikon dinamički prikazuje Upotreba resursa za tu metriku da biste lakše odabrali praga.

4. Odaberite **uvjet** (veće od manje od, etc.) i **praga**.
5. Kliknite **u redu**.



## <a name="move-a-database-into-an-elastic-pool"></a>Premještanje baze podataka u elastic grupe aplikacija

Možete dodati ili ukloniti baze podataka iz postojeće grupe aplikacija. Baza podataka može biti u druge grupe. Međutim, možete dodati samo bazama podataka koje se nalaze na istom poslužitelju logičke.

1. U plohu za na resurse, u odjeljku **Elastic baza podataka** kliknite **Konfiguriraj skup**.

    ![Kliknite Konfiguriraj skup][1]

2. U plohu **Konfiguriraj skup** kliknite **Dodaj u grupu**.

    ![Kliknite Dodaj grupu](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. U plohu **Dodaj baze podataka** odaberite bazu podataka ili baze podataka da biste dodali u grupu. Zatim kliknite **Odaberi**.

    ![Odaberite baze podataka da biste dodali](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    **Konfiguriranje skup** plohu sada popis bazu podataka koju ste odabrali koja će se dodati, s njezin status postavljen na **čekanju**.

    ![Na čekanju skup dodaci](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. U **plohu Konfiguriraj skup**, kliknite **Spremi**.

    ![Kliknite Spremi](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Premještanje baze podataka iz elastic grupe aplikacija

1. U plohu **Konfiguriraj skup** odaberite bazu podataka ili baze podataka da biste uklonili.

    ![Baza podataka za stavku](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Kliknite **Ukloni iz grupe aplikacija**.

    ![Baza podataka za stavku](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    **Konfiguriranje skup** plohu sada popis bazu podataka koju ste odabrali koje će biti uklonjene s njezin status postavljen na **čekanju**.

    ![Zbrajanje pretpregled baze podataka i uklanjanje](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. U **plohu Konfiguriraj skup**, kliknite **Spremi**.

    ![Kliknite Spremi](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Promjena postavki performansi zajedničko područje

Kao što je praćenje korištenjem resursa u grupu, možda otkrijete da su neke prilagodbe potrebne. Možda na resurse potrebne promjene u ograničenja performanse ili prostora za pohranu. Možda želite promijeniti postavke baze podataka u. Postavljanje na resurse možete promijeniti u bilo kojem trenutku da biste dobili najbolje saldo performanse i trošak. U odjeljku [kada je skup elastic baze podataka želite koristiti?](sql-database-elastic-pool-guidance.md) dodatne informacije.

**Da biste promijenili ograničenja eDTUs ili prostora za pohranu po skup i eDTUs po bazi podataka:**

1. Otvorite plohu **Konfiguriraj skup** .

    U odjeljku **Postavke grupe aplikacija Elastic baze podataka**pomoću ili klizač da biste promijenili postavke grupe aplikacija.

    ![Upotreba elastic skup resursa](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Kada se promijeni postavku, prikaz prikazuje Procijenjena mjesečni trošak promjene.

    ![Ažuriranje skup i novi mjesečni trošak](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Stvaranje i upravljanje elastic poslove

Elastic poslove omogućuju pokrećete Transact-SQL skripte za bilo koji broj baza podataka u. Možete stvoriti nove zadatke ili postojeće poslove pomoću portala za upravljanje.

![Stvaranje i upravljanje elastic poslove][5]


Prije korištenja zadacima, instalirajte komponente elastic zadacima i unijeti vjerodajnice. Dodatne informacije potražite u članku [Pregled zadataka Elastic baze podataka](sql-database-elastic-jobs-overview.md).

U odjeljku [Skaliranje odgovor s bazom podataka SQL Azure](sql-database-elastic-scale-introduction.md): pomoću Alati baze podataka za elastic mjerilo Izlaz, premještanje podataka, upit ili stvorite transakcije.



## <a name="additional-resources"></a>Dodatni resursi

- [Baze podataka SQL elastic skup](sql-database-elastic-pool.md)
- [Stvaranje grupe aplikacija programa elastic baze podataka pomoću portala](sql-database-elastic-pool-create-csharp.md)
- [Stvaranje grupe aplikacija programa elastic baze podataka pomoću komponente PowerShell](sql-database-elastic-pool-create-powershell.md)
- [Stvaranje grupe aplikacija programa elastic bazu podataka s C#](sql-database-elastic-pool-create-csharp.md)
- [Cijene i performanse zahtjevi za grupe elastic baze podataka](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
