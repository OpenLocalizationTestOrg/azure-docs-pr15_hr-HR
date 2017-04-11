<properties
    pageTitle="Optimiziranje okruženju sustava Active Directory procjenu rješenjem u prijava analitiku | Microsoft Azure"
    description="Možete koristiti rješenja Active Directory procjenu u procjeni rizika i stanje vašeg okruženja poslužitelja na obični interval."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimiziranje okruženju sustava Active Directory procjenu rješenjem u zapisnik Analytics

Možete koristiti rješenja Active Directory procjenu u procjeni rizika i stanje vašeg okruženja poslužitelja na obični interval. U ovom se članku pronaći ćete instalaciju i korištenje rješenje tako da možete poduzeti ispravka za potencijalne probleme.

Rješenje sadrži prioritet popis preporuke određene infrastrukture distribuiranih poslužitelja. Preporuke kategorizirane preko četiri područja fokus koji olakšavaju brzo razumijevanje rizik i poduzmite akciju.

Preporuke temelje se na znanja i sučelje stekli po Microsoftovi inženjeri od tisuće posjeta klijenta. Svaki preporuke sadrži smjernice o tome zašto problem može važno ima i kako se provode predložene promjene.

Možete odabrati fokus područja koje su najvažnije vašoj tvrtki ili ustanovi te praćenje tijeka prema izvodi okruženju rizika besplatne i dokumenata.

Nakon što dodate rješenje i procjenu je dovršena, sažetak prikazuju se podaci za područja fokus na nadzornoj ploči za **procjenu AD** za infrastrukturu u svom okruženju. U sljedećim se odjeljcima opisuju kako koristiti podatke na **Vrednovanje AD** nadzornu ploču, gdje možete pogledati i zatim poduzeti preporučenih za infrastruktura za poslužitelj servisa Active Directory.

![Slika SQL procjenu pločica](./media/log-analytics-ad-assessment/ad-tile.png)

![Slika nadzorne ploče na vrednovanje SQL](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja
Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Na kontrolera domena koje su članovi domene na vrednovanje mora biti instaliran agenata.
- Rješenja Active Directory rizika zahtijeva .NET Framework 4 na svim računalima na kojima je OMS agent instaliran.
- Dodavanje rješenja Active Directory rizika OMS radni prostor pomoću postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  Postoji bez daljnje konfiguracije obavezno.

    >[AZURE.NOTE] Kada dodate rješenje, AdvisorAssessment.exe datoteke dodaje se poslužitelji agenata. Čitanje i slati OMS servis u oblaku za obradu podataka konfiguracije. Logika primjenjuje se na primljene podataka i servisa u oblaku zapisa podataka.

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory procjenu Detalji zbirke podataka

Active Directory procjenu prikuplja podatke, registra podataka i podataka o performansama pomoću agenata koji ste omogućili WMI.

Sljedeća tablica prikazuje metode zbirke podataka za agenata, hoće li potreban je operacije Manager (SCOM) i kako se često podatke prikupljene putem agent.

| platforme | Izravni Agent | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows|![Da](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Da](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![ne](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![ne](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![Da](./media/log-analytics-ad-assessment/oms-bullet-green.png)| sedam dana|


## <a name="understanding-how-recommendations-are-prioritized"></a>Objašnjenje kako su prioritet preporuke

Svaki preporuke unijeli dobiva razine vrijednost koja određuje relativnu važnost za preporuke. Prikazuju se samo deset najvažnijih preporuke.

### <a name="how-weights-are-calculated"></a>Način izračuna debljine

Weightings su zbrajanja vrijednosti koje se temelje na tri ključne čimbenika:

- *Vjerojatnost* prepoznati problem će uzrokovati probleme. Veća vjerojatnost daje rezultat u obliku veći ukupni rezultat u preporuke za.

- *Utjecaj na* problem na vaše organizacije ako je uzrokovati problem. Veća utjecaj daje rezultat u obliku veći ukupni rezultat u preporuke za.

- *Trud* potreban za implementaciju sustava preporuke. Veći trud daje rezultat u obliku manji ukupni rezultat u preporuke za.

Razine za svaki preporuke izražen je kao postotak ukupni rezultat dostupne za svako područje fokus. Ako, na primjer, ako je preporuke u fokusu područje sigurnost i usklađenost rezultat 5%, implementacijom te preporuke povećava vaše cjelokupan sigurnost i usklađenost rezultat po 5%.

### <a name="focus-areas"></a>Fokus područja

**Sigurnost i usklađenost** – u ovom području fokus prikazuje preporuke za potencijalnih sigurnosnih rizika i breaches, korporativnih pravila i tehničke, pravne i propisima zahtjevima za usklađenosti.

**Dostupnost i poslovanje** – u ovom području fokus prikazuje preporuke za dostupnost servisa, otpornost Infrastruktura i zaštitu tvrtke.

**Performanse i skalabilnost** – u ovom području fokus prikazuje preporuke za vaše tvrtke ili ustanove IT infrastrukturu rasta, provjerite zadovoljava preduvjete za trenutni performanse okruženje za IT i može odgovoriti promjena infrastrukture potrebama.

**Nadogradnja migracije i implementacija** – u ovom području fokus prikazuje preporuke za nadogradnju, migrirati i Implementacija servisa Active Directory postojeću infrastrukturu.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Trebali biste usmjerite rezultatu 100% svakog području fokus?

Ne nužno. Preporuke temelje se na znanja i iskustva stekli po Microsoftovi inženjeri preko tisuće posjeta klijenta. Međutim, ne infrastructures dva poslužitelja jednaki, a određene preporuke može biti veći ili manji relevantnosti. Na primjer, neke preporuke o sigurnosti možda manje važna virtualnim strojevima su izložen putem Interneta. Neke preporuke dostupnost možda manje važna za servise koji omogućuju prikupljanje niskog prioriteta ad-hoc podataka i izvješćivanje. Problemi koji su vam važne starijih tvrtke može biti manje važna je pokretanja. Preporučujemo vam da odredite koja će se žarište područja su vaših prioriteta, a zatim pogledajte kako se rezultati mijenjaju tijekom vremena.

Svaki preporuke sadrži upute o zašto je važno. Koristite ovaj vodič za procjenu li implementacije u preporuke odgovarajuće, dali prirode INFORMATIČKE usluge i poslovnim potrebama vaše tvrtke ili ustanove.

## <a name="use-assessment-focus-area-recommendations"></a>Koristite preporuke za procjenu fokus područje

Rješenje za procjenu mogli koristiti u OMS, morate imati instaliran rješenja. Za dodatne informacije o instaliranju rješenja, potražite u članku [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md). Nakon instalacije, možete vidjeti sažetak preporuke pomoću pločicu procjenu na stranici pregled u OMS.

Prikaz konfiguracije sažete usklađenost za infrastrukturu i zatim dubinske analize u preporuke.


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Da biste prikazali preporuke za područje specijalizacije i poduzmite akciju ispravljanja

1. Na stranici **Pregled** kliknite na **Vrednovanje** pločici infrastruktura za server.
2. Na stranici **procjenu** pregledali sažetak informacija u jednom području blades fokus, a zatim kliknite jedan da biste prikazali preporuke za to područje fokus.
3. Na bilo kojoj od stranica područje fokus, možete pogledati prioritet preporuke za vaše okruženje. Kliknite i preporuke u odjeljku **Utječe na objekte** da biste pogledali detalje o zašto je izvršena na preporuke.  
    ![Slika preporuke za procjenu](./media/log-analytics-ad-assessment/ad-focus.png)
4. Možete poduzeti korektivne predložena u **Predložene korake**. Kada je je adresirana stavku, kasnije konfiguracije će zapis koji preporučuje okrenutim akcije i usklađenost rezultat će se povećati. Ispravnog stavki kao **Proslijeđena objekte**.

## <a name="ignore-recommendations"></a>Zanemarivanje preporuke

Ako imate preporuke za koje želite zanemariti, možete stvoriti tekstna datoteka koja OMS će koristiti da biste spriječili preporuke prikazivao na vrednovanje rezultatima.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Da biste odredili preporuke za koje će zanemariti

1.  Prijavite se u radni prostor i otvorite zapisnika pretraživanja. Koristite sljedeći upit za preporuke popis koji nije uspjela za računala u svom okruženju.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Evo snimka zaslona prikazuje upit za pretraživanje zapisnika: ![nije uspjela preporuke](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Odaberite preporuke za koje želite zanemariti. Koristit ćete vrijednosti za RecommendationId u sljedeći postupak.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Stvaranje i korištenje programa IgnoreRecommendations.txt tekstne datoteke

1.  Stvorite datoteku pod nazivom IgnoreRecommendations.txt.
2.  Zalijepite ili upišite svaki RecommendationId za svaki preporuke želite evidencije analize da biste zanemarili u zasebnom retku, a zatim spremite i zatvorite datoteku.
3.  Datoteku smjestite u sljedeću mapu na svim računalima na mjesto na kojem želite da se OMS da biste zanemarili preporuke.
    - Na računalima s Microsoft nadzor agenta (povezani izravno ili putem Operations Manager) – *sistemski pogon*: \Programske Agent\Agent praćenja
    - Na poslužitelju za upravljanje Operations Manager - *sistemski pogon*: \Programske sustava centra 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Da biste potvrdili da se ignoriraju preporuke

Nakon sljedeće zakazano pokreće rizika, po zadanom svaki 7 dana, navedeni preporuke označavaju *Zanemareno* i prikazat će se na nadzornoj ploči za procjenu.

1. Da biste popis zanemarene preporuke, poslužite se sljedećim upita za pretraživanje zapisnika.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Ako kasnije odlučite želite li vidjeti zanemarene preporuke, uklonite sve datoteke IgnoreRecommendations.txt ili RecommendationIDs možete ukloniti iz njih.

## <a name="ad-assessment-solutions-faq"></a>Rješenja za procjenu AD najčešća Pitanja

*Koliko često se pokreće procjenu?*
- Na vrednovanje pokreće svaki 7 dana.

*Postoji način da biste konfigurirali koliko često se izvodi na vrednovanje?*
- Zasad ne.

*Drugi poslužitelj za je otkrivanja nakon Dodao sam rješenje za procjenu, će je moguće kažnjeni?*
- Da, kada se pronađu ga je kažnjeni iz zatim, svaka 7 dana.

*Ako na poslužitelju prestanka korištenja računala, kada ga uklonit će se na vrednovanje?*
- Ako na poslužitelju slanje podataka za 3 tjedna, znači da je uklonjena.

*Što je naziv procesa koji se ne prikupljanje podataka?*
- AdvisorAssessment.exe

*Koliko dugo traje podataka prikupljenih?*
- Prikupljanje stvarnih podataka na poslužitelju vodi 1 sat. To može trajati dulje na poslužiteljima s velikim brojem poslužitelje servisa Active Directory.

*Koju vrstu podataka koji se prikupljaju?*
- Prikuplja sljedeće vrste podataka:
    - WMI
    - Registra
    - Mjerača performansi

*Postoji način da biste konfigurirali kada prikupljanja podataka?*
- Zasad ne.

*Zašto se prikazuje samo prvih 10 preporuke?*
- Umjesto da vam daje iscrpan izvršilo popis zadataka, preporučujemo da fokus na prvo adresiranja prioritet preporuke. Nakon što riješite ih dodatne preporuke postaju dostupne. Ako biste radije da biste vidjeli detaljan popis, možete pogledati sve preporuke pomoću zapisnika pretraživanja.

*Postoji način da biste zanemarili preporuku?*
- Da, potražite u članku [Zanemari preporuke](#ignore-recommendations) prethodnom odjeljku.


## <a name="next-steps"></a>Daljnji koraci

- Korištenje [zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md) da biste pogledali detaljne podatke za procjenu AD i preporuke.
