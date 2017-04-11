<properties
   pageTitle="Korištenje centra za sigurnost Azure incidenta odgovor | Microsoft Azure"
   description="Ovaj dokument objašnjava kako koristiti centar za sigurnost Azure u scenariju incidenta odgovor."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Korištenje centra za sigurnost Azure incidenta odgovor
Mnoge organizacije Saznajte kako odgovoriti sigurnošću tek nakon što u slučaju napada suffering. Da biste smanjili troškove i oštećenja, je važno da bi je incidenta odgovor planiranje na mjestu prije nego što u slučaju napada odvija. Centar za sigurnost Azure možete koristiti u različite faze incidenta odgovor.

## <a name="incident-response-planning"></a>Planiranje incidenta odgovor

Učinkovita plan ovisi o tri mogućnosti core: koja se može zaštiti, otkrivanje i odgovaranje na prijetnji. Zaštita je o sprječava kupljenih, otkrivanje o ranijeg prepoznavanje prijetnji, a odgovor o evicting napadača i vraćanju sustava da biste smanjili učinke na kršenja.

U ovom se članku će koristiti faza incidenta odgovor sigurnost iz članka [Microsoft Azure sigurnost odgovor u Oblaku](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) kao što je prikazano na sljedećem su dijagramu:

![Životni ciklus incidenta odgovor](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Centar za sigurnost možete koristiti tijekom faze Otkrij, Assess i dijagnosticiranje. Evo primjera kako centar za sigurnost može biti korisno tijekom tri faze početne incidenta odgovor:

- **Otkrivanje**: pregled prvi oznaka istrazi događaj.
    - Primjer: pregled početne potvrdu da je visokog prioriteta sigurnosno upozorenje podignut na nadzornoj ploči centar za sigurnost.
- **Assess**: izvođenje početne procjenu da biste dobili dodatne informacije o sumnjivoj aktivnosti.
    - Primjer: dobiti dodatne informacije o sigurnosnom upozorenju.
- **Dijagnoza**: vođenje tehničke istraživanja i definirati strategije zaustavljanja, ublažiti i zaobilazno rješenje.
    - Primjer: uputa olakšava opisan centar za sigurnost u tom određenom sigurnosnom upozorenju.

Scenarij u kojem se nalazi iza objašnjava odražava centar za sigurnost tijekom Otkrij, Assess i dijagnosticiranje/odgovor faza incident vezan uz sigurnost. U centru za sigurnost [incident vezan uz sigurnost](security-center-incident.md) je prikupljene sva upozorenja za resurs poravnavanje uzorcima [Ukloni lanac](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Kupljenih se prikazivati na pločici [sigurnosnih upozorenja](security-center-managing-and-responding-alerts.md) i plohu. Incident otkriva popis povezanih upozorenja, koji omogućuje da biste dobili dodatne informacije o svako pojavljivanje. Centar za sigurnost i predstavlja samostalne sigurnosnih upozorenja koja se može se koristiti za praćenje dolje sumnjivoj aktivnosti.

## <a name="scenario"></a>Scenarij

Contoso nedavno migrirati neke od njihovih lokalnog resursa Azure, uključujući neke utemeljen na virtualnog računala radnih opterećenja redak tvrtke i baze podataka SQL. Trenutno Contoso-Core računalo sigurnost Incident odgovor tima (CSIRT) ima problema istražuje sigurnosnim problemima zbog sigurnosti obavještavanje neće se integriran s svoje trenutne Alati incidenta odgovor. Nemaju Integracija predstavlja problem tijekom faze Otkrij (positives previše false), kao i tijekom faze Assess i dijagnosticiranje. Kao dio ovog migracije, oni odlučili uključivanje za centar za sigurnost da biste lakše ih riješio taj problem.

Prva faza ovaj migraciju Završi nakon oni onboarded sve resurse i adresirana sve preporuke o sigurnosti centra za sigurnost. Contoso CSIRT je točka Žarišna za sigurnošću na računalu. Tim se sastoji od grupa osoba s odgovornosti postupanja s bilo kojeg incident vezan uz sigurnost. Članovi tima jasno definirani obavezama koje imate kako bi bez područje odgovor ostao neotkrivenih.

Radi ovaj scenarij smo ćete fokus na ulogama sljedeće osobe koje su dio Contoso CSIRT:

![Životni ciklus incidenta odgovor](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judy se postupci za sigurnost. Svoj obaveze obuhvaćaju sljedeće:

- Nadzor i odgovaranje na sigurnosnih prijetnji oko clock.
- Escalating oblaka radno opterećenje vlasniku ili sigurnost analitičar prema potrebi.

Sam je sigurnost analitičar i njegovog obaveze biti:

- Istražuje napada.
- Remediating upozorenja.
- Rad s radno opterećenje vlasnici da biste odredili i primijenili mitigations.

Kao što vidite, Judy i Sam imaju različite obaveze, a oni morate Suradnja radi razmjene informacija centar za sigurnost.

## <a name="recommended-solution"></a>Preporučeno rješenje

Budući da Judy i Sam imaju različite uloge, oni hoćete li koristiti različite dijelove centar za sigurnost da biste dobili odgovarajuće podatke o njihovim aktivnostima dnevno. Judy koristit će **sigurnosnih upozorenja** kao dio svoj dnevnih nadzor.

![Sigurnosna upozorenja](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Judy koristit će sigurnosnih upozorenja tijekom faze otkrivanje i Assess. Kada Judy Završi početne rizika, Ana može pretvaranje problem Sam ako je potrebna dodatna istrage. U ovom trenutku Sam koristit će informacije koje ste dobili od centar za sigurnost, ponekad zajedno s drugim izvorima podataka da biste premjestili dijagnosticiranje fazu.


## <a name="how-to-implement-this-solution"></a>Kako implementirati rješenja

Da biste vidjeli kako će koristiti centar za sigurnost Azure u scenariju do incidenta odgovor, ne možemo ćete Judy, koraci u fazama otkrivanje i Assess i pogledajte Sam funkcija dijagnosticiranje problema.

### <a name="detect-and-assess-incident-response-stages"></a>Otkrivanje i procijenite faza incidenta odgovor

Judy prijavljeni na portal za Azure i radi li se u Centar za sigurnost konzoli sustava. Kao dio svoj dnevnu nadzor aktivnosti, Ana rada visokog prioriteta sigurnosnih upozorenja za pregled pomoću sljedećih koraka:

1. Kliknite pločicu **sigurnosnih upozorenja** i pristup plohu **sigurnosnih upozorenja** .
    ![Sigurnosna upozorenja plohu](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Radi ovaj scenarij Judy će procjenu izvedete aktivnosti upozorenje zlonamjerni SQL u prethodnoj slici.
2. Kliknite upozorenje **zlonamjerni SQL aktivnosti** i pregledajte attacked resursa u **aktivnosti zlonamjerni SQL** plohu:  ![Incident pojedinosti](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    U ovom plohu Judy možete stvarati bilješke o attacked resursa, kako se dogodilo s više puta ovaj napadi i kada je otkriven.
3. Kliknite na **napada resursa** da biste dobili dodatne informacije o ovom napada.

Kad pročitate opis, Judy convinced da to nije pogrešno otkrivanje i ona mora pretvaranje slučaj Sam.

### <a name="diagnose-incident-response-stage"></a>Dijagnosticiranje faza incidenta odgovor

Sam prima slučaj iz Judy i pokrenuti pregledavanje olakšava korake predložene centar za sigurnost.

![Životni ciklus incidenta odgovor](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Dodatni resursi

U tim incidenta odgovor također možete iskoristiti prednost mogućnost [Power BI centar za sigurnost](security-center-powerbi.md) da biste vidjeli različite vrste izvješća. Ta izvješća omogućuju ih tijekom daljnje istrage vizualizacija, analizirati i filtrirati preporuke i sigurnosna upozorenja. Tvrtkama koje koriste njihove informacije o sigurnosti i rješenje za upravljanje (SIEM) događaj tijekom postupka istraga, oni također možete [integrirati centar za sigurnost njihova rješenja](security-center-integrating-alerts-with-log-integration.md). Pomoću [alata za integraciju Azure zapisnika](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/)možete integrirati i Azure nadzornih zapisnika i događaje sigurnost virtualnog računala (VM). Da biste istražili u slučaju napada, možete koristiti taj podatak u kombinaciji s podacima koje nudi centar za sigurnost.


## <a name="conclusion"></a>Zaključak

Sastavljanju tim prije incident dođe do važno vašoj tvrtki ili ustanovi te će sigurnošću može utjecati kako se upravlja kupljenih. Imate pravim alatima praćenje resursa može pridonijeti tog tima morati poduzeti točne korake za remediate incident vezan uz sigurnost. Centar za sigurnost [Mogućnosti otkrivanja](security-center-detection-capabilities.md) mogu pomoći IT da biste brzo sa sigurnošću i remediate sigurnosnim problemima.
