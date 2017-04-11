<properties
    pageTitle="Optimiziranje okruženju sustava SQL procjenu rješenjem u prijava analitiku | Microsoft Azure"
    description="Možete koristiti rješenja SQL procjenu u procjeni rizika i stanje vašeg okruženja poslužitelja na obični interval."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-sql-assessment-solution-in-log-analytics"></a>Optimiziranje okruženju sustava SQL procjenu rješenjem u zapisnik Analytics


Možete koristiti rješenja SQL procjenu u procjeni rizika i stanje vašeg okruženja poslužitelja na obični interval. U ovom se članku pronaći ćete instalirati rješenje tako da možete poduzeti ispravka za potencijalne probleme.

Rješenje sadrži prioritet popis preporuke određene infrastrukture distribuiranih poslužitelja. Preporuke kategorizirane preko šest fokus područja koje olakšavaju brzo svjesni rizika i poduzmite akciju ispravljanja.

Preporuke unijeli temelje se na znanja i sučelje stekli po Microsoftovi inženjeri od tisuće posjeta klijenta. Svaki preporuke sadrži smjernice o tome zašto problem može važno ima i kako se provode predložene promjene.

Možete odabrati fokus područja koje su najvažnije vašoj tvrtki ili ustanovi te praćenje tijeka prema izvodi okruženju rizika besplatne i dokumenata.

Nakon što dodate rješenje i procjenu je dovršena, sažetak prikazuju se podaci za područja fokus na nadzornoj ploči za **Procjenu SQL** za infrastrukturu u svom okruženju. U sljedećim se odjeljcima opisuju kako koristiti podatke na **Vrednovanje SQL** nadzornu ploču, gdje možete pogledati i zatim poduzeti preporučenih za sustavu SQL server.

![Slika pločice SQL rizika](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![Slika nadzorne ploče na vrednovanje SQL](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja
Procjena SQL funkcionira sa svim trenutno podržanim verzijama sustava SQL Server za izdanja Standard, za razvojne inženjere i Enterprise.

Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Agenata mora biti instaliran na poslužiteljima koji ste instalirali je SQL Server.
- Rješenja za procjenu SQL zahtijeva .NET Framework 4 na svim računalima na kojima je OMS agent instaliran.
- Prilikom korištenja agent za komponentu Operations Manager sa SQL procjenu, morat ćete koristiti operacija Upravitelj Run-As račun. Dodatne informacije potražite u odjeljku [Operations Manager izvoditi račune za OMS](#operations-manager-run-as-accounts-for-oms) ispod.

    >[AZURE.NOTE] Agent za MMA ne podržava Run-As operacije Upravitelj računa.

- Dodavanje rješenja SQL procjenu OMS radni prostor pomoću postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md). Postoji bez daljnje konfiguracije obavezno.

>[AZURE.NOTE] Kada dodate rješenje, AdvisorAssessment.exe datoteke dodaje se poslužitelji agenata. Čitanje i slati OMS servis u oblaku za obradu podataka konfiguracije. Logika primjenjuje se na primljene podataka i servisa u oblaku zapisa podataka.

## <a name="sql-assessment-data-collection-details"></a>Detalji zbirke podataka za SQL procjenu

Procjena SQL prikuplja WMI podataka, podatke u registru, podataka o performansama i SQL Server dinamički upravljanje prikaz rezultata pomoću agenata koji ste omogućili.

Sljedeća tablica prikazuje metode zbirke podataka za agenata, hoće li potreban je operacije Manager (SCOM) i kako se često podatke prikupljene putem agent.

| platforme | Izravni Agent | Agent za SCOM | Azure prostora za pohranu | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows|![Da](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Da](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![ne](./media/log-analytics-sql-assessment/oms-bullet-red.png)|    ![ne](./media/log-analytics-sql-assessment/oms-bullet-red.png)|![Da](./media/log-analytics-sql-assessment/oms-bullet-green.png)|   sedam dana|

## <a name="operations-manager-run-as-accounts-for-oms"></a>Operations Manager izvoditi račune za OMS

Prijavite se analize OMS koristi agent i upravljanje grupe Operations Manager mogli ste prikupljati i slanje podataka u OMS usluga. OMS izgradi nakon upravljanje paketi za radnih opterećenja možete unijeti vrijednosti Dodavanje servisa. Svaki radno opterećenje potrebne ovlasti specifične za radno opterećenje da biste pokrenuli paketi za upravljanje u drugoj sigurnosnoj kontekstu, kao što je račun domene. Morate unijeti podatke vjerodajnica tako da konfigurirate račun operacije Upravitelj Pokreni kao.

Da biste postavili račun Pokreni kao Operations Manager za procjenu SQL poslužite se sljedećim informacijama.

### <a name="set-the-run-as-account-for-sql-assessment"></a>Postavljanje računa Pokreni kao za procjenu SQL

 Ako već koristite paket za upravljanje sustava SQL Server, trebali biste koristi taj račun za pokretanje kao.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>Konfiguriranje računa za SQL Pokreni kao na konzoli operacije

>[AZURE.NOTE] Ako koristite agent Izravni OMS umjesto SCOM agent paket za upravljanje uvijek pokreće se u sigurnosnom kontekstu korisničkog računa lokalnog sustava. Preskočite korake 1-5 ispod i pokrenuti T SQL ili Powershell uzorka, pri određivanju NT AUTHORITY\SYSTEM korisničko ime.

1. U Operations Manager otvorite konzolu za operacije pa kliknite **Administracija**.

2. U odjeljku **Pokreni kao konfiguracije**kliknite **profile**pa otvorite **OMS SQL procjenu pokrenuti kao profil**.

3. Na stranici **Pokretanje kao računi** kliknite **Dodaj**.

4. Odaberite račun za pokretanje sustava Windows kao koja sadrži vjerodajnice potrebne za SQL Server ili kliknite **Novo** da biste je stvorili.
    >[AZURE.NOTE] Pokrenite kao vrstu računa mora biti Windows. Račun Pokreni kao mora biti dio lokalne grupe administratora na svim poslužiteljima sustava Windows koji se nalaze instance sustava SQL Server.

5. Kliknite **Spremi**.

6. Izmijenite, a zatim izvršite sljedeće ogledne T SQL na svaku instancu SQL Server da biste dodijelili minimalne potrebne dozvole da biste pokrenuli kao račun za izvođenje SQL procjenu. Međutim, ne morate učiniti ako je pokrenuti kao račun je već dio uloga poslužitelja sustava na SQL Server instancama.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Konfiguriranje računa SQL Pokreni kao pomoću komponente Windows PowerShell

Otvorite prozor PowerShell i pokrenite sljedeću skriptu nakon što ste ažurirali uz svoje podatke:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Objašnjenje kako su prioritet preporuke

Svaki preporuke unijeli dobiva razine vrijednost koja određuje relativnu važnost za preporuke. Prikazuju se samo deset najvažnijih preporuke.

### <a name="how-weights-are-calculated"></a>Način izračuna debljine

Weightings su zbrajanja vrijednosti koje se temelje na tri ključne čimbenika:

- *Vjerojatnost* da je problem identificirati uzrokovati probleme. Veća vjerojatnost daje rezultat u obliku veći ukupni rezultat u preporuke za.

- *Utjecaj na* problem na vaše organizacije ako je uzrokovati problem. Veća utjecaj daje rezultat u obliku veći ukupni rezultat u preporuke za.

- *Trud* potreban za implementaciju sustava preporuke. Veći trud daje rezultat u obliku manji ukupni rezultat u preporuke za.

Razine za svaki preporuke izražen je kao postotak ukupni rezultat dostupne za svako područje fokus. Ako, na primjer, ako je preporuke u fokusu područje sigurnost i usklađenost rezultat 5%, implementacijom te preporuke povećava vaše cjelokupan sigurnost i usklađenost rezultat po 5%.

### <a name="focus-areas"></a>Fokus područja

**Sigurnost i usklađenost** – u ovom području fokus prikazuje preporuke za potencijalnih sigurnosnih rizika i breaches, korporativnih pravila i tehničke, pravne i propisima zahtjevima za usklađenosti.

**Dostupnost i poslovanje** – u ovom području fokus prikazuje preporuke za dostupnost servisa, otpornost Infrastruktura i zaštitu tvrtke.

**Performanse i skalabilnost** – u ovom području fokus prikazuje preporuke za vaše tvrtke ili ustanove IT infrastrukturu rasta, provjerite zadovoljava preduvjete za trenutni performanse okruženje za IT i može odgovoriti promjena infrastrukture potrebama.

**Nadogradnja migracije i implementacija** – u ovom području fokus prikazuje preporuke za nadogradnju, migrirati i implementacija sustava SQL Server da biste postojeću infrastrukturu.

**Operacije i nadzor** – u ovom području fokus prikazuje preporuke za poboljšanje pojednostavnjuju vaše operacije IT implementirati Preventivno održavanje te poboljšajte performanse.

**Promjena i upravljanje konfiguracija** – u ovom području fokus prikazuje preporuke radi zaštita svakodnevnih, provjerite da promjene ne negativno utjecati na sustavu, uspostaviti promjena kontrola postupaka, a za praćenje i nadzor konfiguracijama sustava.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Trebali biste usmjerite rezultatu 100% svakog području fokus?

Ne nužno. Preporuke temelje se na znanja i iskustva stekli po Microsoftovi inženjeri preko tisuće posjeta klijenta. Međutim, ne infrastructures dva poslužitelja jednake i određene preporuke može biti veći ili manji relevantnost. Na primjer, neke preporuke o sigurnosti možda manje važna virtualnim strojevima su izložen putem Interneta. Neke preporuke dostupnost možda manje važna za servise koji omogućuju prikupljanje niskog prioriteta ad-hoc podataka i izvješćivanje. Problemi koji su vam važne starijih tvrtke može biti manje važna je pokretanja. Preporučujemo vam da odredite koja će se žarište područja su vaših prioriteta, a zatim pogledajte kako se rezultati mijenjaju tijekom vremena.

Svaki preporuke sadrži upute o zašto je važno. Koristite ovaj vodič za procjenu li implementacije u preporuke odgovarajuće, dali prirode INFORMATIČKE usluge i poslovnim potrebama vaše tvrtke ili ustanove.

## <a name="use-assessment-focus-area-recommendations"></a>Koristite preporuke za procjenu fokus područje

Rješenje za procjenu mogli koristiti u OMS, morate imati instaliran rješenja. Za dodatne informacije o instaliranju rješenja, potražite u članku [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md). Nakon instalacije, možete vidjeti sažetak preporuke pomoću pločicu sustava SQL na stranici pregled u OMS.

Prikaz konfiguracije sažete usklađenost za infrastrukturu i zatim dubinske analize u preporuke.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Da biste prikazali preporuke za područje specijalizacije i poduzmite akciju ispravljanja

1. Na stranici **Pregled** kliknite pločicu **Procjenu SQL** .
2. Na stranici **SQL procjenu** pregledali sažetak informacija u jednom području blades fokus, a zatim kliknite jedan da biste prikazali preporuke za to područje fokus.
3. Na bilo kojoj od stranica područje fokus, možete vidjeti prioritet preporuke za vaše okruženje. Kliknite i preporuke u odjeljku **Utječe na objekte** da biste pogledali detalje o zašto je izvršena na preporuke.  
    ![Slika preporuke za procjenu SQL](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Možete poduzeti korektivne predložena u **Predložene korake**. Kada je je adresirana stavku, kasnije konfiguracije će zapis koji preporučuje okrenutim akcije i usklađenost rezultat će se povećati. Ispravnog stavki kao **Proslijeđena objekte**.

## <a name="ignore-recommendations"></a>Zanemarivanje preporuke

Ako imate preporuke za koje želite zanemariti, možete stvoriti tekstna datoteka koja OMS će koristiti da biste spriječili preporuke prikazivao na vrednovanje rezultatima.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Da biste odredili preporuke za koje će zanemariti

1.  Prijavite se u radni prostor i otvorite zapisnika pretraživanja. Koristite sljedeći upit za preporuke popis koji nije uspjela za računala u svom okruženju.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Evo snimka zaslona prikazuje upit za pretraživanje zapisnika: ![nije uspjela preporuke](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)

2.  Odaberite preporuke za koje želite zanemariti. Koristit ćete vrijednosti za RecommendationId u sljedeći postupak.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Stvaranje i korištenje programa IgnoreRecommendations.txt tekstne datoteke

1.  Stvorite datoteku pod nazivom IgnoreRecommendations.txt.
2.  Zalijepite ili upišite svaki RecommendationId za svaki preporuke želite OMS da biste zanemarili u zasebnom retku, a zatim spremite i zatvorite datoteku.
3.  Datoteku smjestite u sljedeću mapu na svim računalima na mjesto na kojem želite da se OMS da biste zanemarili preporuke.
    - Na računalima s Microsoft nadzor agenta (povezani izravno ili putem Operations Manager) – *sistemski pogon*: \Programske Agent\Agent praćenja
    - Na poslužitelju za upravljanje Operations Manager - *sistemski pogon*: \Programske sustava centra 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Da biste potvrdili da se ignoriraju preporuke

1.  Nakon sljedeće zakazano pokreće procjenu po zadanom svaki 7 dana, navedeni preporuke označavaju Zanemareno i prikazat će se na nadzornoj ploči za procjenu.
2.  Da biste popis zanemarene preporuke, poslužite se sljedećim upita za pretraživanje zapisnika.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
3.  Ako kasnije odlučite želite li vidjeti zanemarene preporuke, uklonite sve datoteke IgnoreRecommendations.txt ili RecommendationIDs možete ukloniti iz njih.

## <a name="sql-assessment-solution-faq"></a>Procjena SQL rješenje najčešća Pitanja

*Koliko često se pokreće procjenu?*
- Na vrednovanje pokreće svaki 7 dana.

*Postoji način da biste konfigurirali koliko često se izvodi na vrednovanje?*
- Zasad ne.

*Drugi poslužitelj za je otkrivanja nakon Dodao sam rješenja SQL rizika, će je moguće kažnjeni?*
- Da, kada se pronađu ga je kažnjeni iz zatim, svaka 7 dana.

*Ako na poslužitelju prestanka korištenja računala, kada ga uklonit će se na vrednovanje?*
- Ako na poslužitelju slanje podataka za 3 tjedna, znači da je uklonjena.

*Što je naziv procesa koji se ne prikupljanje podataka?*
- AdvisorAssessment.exe

*Koliko dugo traje podataka prikupljenih?*
- Prikupljanje stvarnih podataka na poslužitelju vodi 1 sat. To može trajati dulje na poslužiteljima koji imaju velik broj instanci SQL ili baze podataka.

*Koju vrstu podataka koji se prikupljaju?*
- Prikuplja sljedeće vrste podataka:
    - WMI
    - Registra
    - Mjerača performansi
    - Prikazi SQL dinamičkog upravljanja (DMV).

*Postoji način da biste konfigurirali kada prikupljanja podataka?*
- Zasad ne.

*Zašto je morate konfigurirati pokretanje kao račun?*
- Za SQL Server će se izvoditi mali broj SQL upita. Kako bi ih da biste pokrenuli, mora se koristiti pokrenuti kao račun s dozvolama za PRIKAZ STANJE POSLUŽITELJA za SQL.  Osim toga, da bi upit WMI lokalne administratorske vjerodajnice su potrebni.

*Zašto se prikazuje samo prvih 10 preporuke?*
- Umjesto da vam daje iscrpan izvršilo popis zadataka, preporučujemo da fokus na prvo adresiranja prioritet preporuke. Nakon što riješite ih dodatne preporuke postaju dostupne. Ako biste radije da biste vidjeli detaljan popis, možete pogledati sve preporuke pomoću zapisnika pretraživanja OMS.

*Postoji način da biste zanemarili preporuku?*
- Da, potražite u članku [Zanemari preporuke](#ignore-recommendations) prethodnom odjeljku.



## <a name="next-steps"></a>Daljnji koraci

- [Zapisnika pretraživanja](log-analytics-log-searches.md) da biste pogledali detaljne podatke za procjenu SQL i preporuke.
