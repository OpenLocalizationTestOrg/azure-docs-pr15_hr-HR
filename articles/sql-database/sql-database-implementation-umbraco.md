<properties
   pageTitle="Azure Studije slučaja - Umbraco baze podataka Azure SQL | Microsoft Azure"
   description="Saznajte kako Umbraco koristi baze podataka SQL brzo Dodjela i skaliranje servise za tisuće drugih korisnika u oblaku"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco koristi baze podataka SQL Azure brzo dodjele resursa i omjere services da biste tisuće drugih korisnika u oblaku

![Logotip Umbraco](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco je popularne Otvori izvor sadržaja za upravljanje sustavom (a CMS) koji se može izvoditi ništa od mali kampanje ili brošura s web-mjesta u složene aplikacije za web-mjesto tvrtke Fortune 500 i u okvir za globalnu medijskih sadržaja web-mjesta. 

> "Imamo vrlo velike zajednice razvojni inženjeri koji koristite u sustavu s više od 100 000 razvojnim inženjerima na forumima i više od 350,000 web-mjesta koja su uživo, radi Umbraco."

> – Umbraco Morten Christensen, tehničke potencijalnog klijenta

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Da biste pojednostavnili kupca implementacijama Umbraco dodali Umbraco-kao-na-Service (UaaS): nuditi softver-kao-na-service (SaaS) koji nema potrebe za lokalnim implementacijama nudi ugrađene skaliranje i indirektni uklanja upravljanje omogućivanjem inženjerima omogućuje fokusiranje na inovaciju umjesto rješenje upravljanja proizvoda. Umbraco je osigurati tih prednosti po potrebe za oslanjanjem na fleksibilne model (PaaS) platforme kao-na-usluga koje nudi Microsoft Azure.

UaaS omogućuje SaaS klijentima da biste koristili mogućnosti a CMS Umbraco koji su prethodno iz svoje razgovor. Ti klijenti su tamo uz okruženja za rad a CMS koje sadrži bazu podataka. Korisnici možete dodati do dva dodatna baze podataka za razvoj i pripremna okruženja, ovisno o njihovim preduvjeti. Novo okruženje je primitku automatiziranog procesa dodjeljuje tog klijenta baze podataka automatski. Nove baze podataka spreman u sekundama, jer je baza podataka je već unaprijed stvorena po Umbraco iz Azure elastic skup dostupnih baza podataka (pogledajte slika 1).


![Umbraco dodjele resursa životni ciklus](./media/sql-database-implementation-umbraco/figure1.png)

Slika 1. Dodjeljivanje životni ciklus za Umbraco kao Service (UaaS)
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure elastic grupe i Automatizacija Pojednostavnite implementacije

S bazom podataka SQL Azure i drugih servisa za Azure, možete Umbraco kupci koja se sama Dodjela resursa njihove okruženja, a Umbraco možete jednostavno nadzor i upravljanje bazama podataka kao dio intuitivno tijeka rada:

1.  Dodjela resursa

    Umbraco održava kapacitet 200 dostupne unaprijed dodijeljenu baze podataka iz elastic grupe. Kad novog klijenta registrira za UaaS, Umbraco nudi klijenta s novom okruženju a CMS u najbliži stvarnom vremenu tako da im dodijelite baze podataka iz grupe aplikacija dostupnost.

    Kada se skup dostupnost dosegne praga, stvara se novi skup elastic, a nove baze podataka su unaprijed dodijeljenu želite dodijeliti korisnicima prema potrebi.

    Implementacija u potpunosti je automatizirano pomoću biblioteke upravljanja C# "i" Redovi Bus servisa Azure.

2.  Korištenje

    Korisnici koriste jedan do tri okruženja (za radni, pripremna i/ili razvoj), svaku s vlastitu bazu podataka. Baza podataka za klijenta su u grupe elastic baze podataka koja omogućuje Umbraco omogućuje učinkovito skaliranje bez potrebe za previše Dodjela.

    ![Pregled projekta Umbraco](./media/sql-database-implementation-umbraco/figure2.png)

    ![Pojedinosti o projektima Umbraco](./media/sql-database-implementation-umbraco/figure3.png)

    Slika 2. Umbraco-kao-na-Service (UaaS) kupca web-mjestu s pregledom projekta i detalja

    Baze podataka SQL Azure koristi jedinice transakcije baze podataka (DTUs) za predstavljanje relativni power potrebne za transakcije baze podataka stvarnog života. Za korisnike UaaS baze podataka obično rade oko 10 DTUs, ali svaka ima elasticity za promjenu veličine na zahtjev. To znači da UaaS možete biti sigurni da korisnici uvijek imate potrebne resurse, čak i tijekom vremena Vršna. Ako, na primjer, tijekom na nedavni nedjelja noći Sportski događaj, jedne UaaS kupca iskusnih baze podataka peaks do 100 DTUs trajanja igre. Azure elastic grupe sada je moguće Umbraco za podršku te visoke zahtjev bez smanjene performanse performanse.

3.  Monitora

    Umbraco monitora baze podataka aktivnosti pomoću nadzornih ploča unutar Azure portala, uz upozorenja prilagođene e-pošte.

4.  Izrada oporavak

    Azure postoje dvije mogućnosti za oporavak Izrada (DR): aktivna replikacije zemlj i zemlj vraćanja. Mogućnost DR tvrtke trebali biste odabrati ovisi o [ciljevi poslovanje](sql-database-business-continuity.md).

    Aktivni replikacije zemlj nudi najbrže razinu odgovor u slučaju isključiti. Korištenje aktivni replikacije zemlj., možete stvoriti do četiri čitljiv secondaries na poslužiteljima u različitim područjima i zatim možete pokrenuti prebacivanje na neku razinu na secondaries u slučaju pogreške.

    Umbraco ne zahtijeva zemlj replikacije, ali je iskoristiti od Azure zemlj.-Vrati da biste bili minimalne nedostupnost slučaju se prekida. Zemlj vraćanja ovisi o sigurnosne kopije baze podataka u zemlj suvišnih Azure prostora za pohranu. Koje korisnicima omogućuje vraćanje iz sigurnosne kopije kada u području primarni se prekida.

5.  Dodjela de-resursa za

    Nakon brisanja okruženja projekta sve pridružene baze podataka (razvoj pripremna ili uživo) uklanjaju se tijekom čišćenje reda čekanja Bus servisa Azure. Postupak automatskog Umbraco na skup elastic dostupnost baze podataka, što ih čini dostupnima za buduće dodjeljivanje uz zadržavanje Maksimalna Upotreba vraća Neiskorišteni baze podataka.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Elastic grupe omogućuju UaaS za promjenu veličine jednostavno

Putem grupe Azure elastic baze podataka, Umbraco možete optimizirati izvedbe svojih klijenata bez potrebe za pokazivač ili under-dodjele resursa. Umbraco trenutno je gotovo 3000 baze podataka preko 19 grupe elastic baze podataka, uz mogućnost jednostavno skaliranje prema potrebi kako bi odgovarao bilo koji od svoje postojeće korisnike 325,000 ili novi korisnici koji su spremni za implementaciju a CMS u oblaku.

Zapravo, prema Morten Christensen tehnička vođenje pri Umbraco, "UaaS sada ima growth otprilike 30 novim klijentima prema danu. Naših korisnika su delighted pomoću moći dodjela novih projekata u sekundama, trenutačno objavljivanje ažuriranja svoja uživo web-mjesta iz razvojno okruženje pomoću "jednim klikom implementacije", a promjene kao što brzo ako se pronađe pogreške."

Ako klijent okruženju drugi i/ili treći zahtijeva više, jednostavno možete ukloniti te okruženja. Koje oslobađa resursa koje je moguće koristiti za druge korisnike kao dio grupe aplikacija za Umbraco elastic dostupnost baze podataka.

![Umbraco implementaciju arhitekture](./media/sql-database-implementation-umbraco/figure4.png)

Slika 3. Arhitektura implementaciju UaaS na Microsoft Azure

##<a name="the-path-from-datacenter-to-cloud"></a>Put od podatkovnim centrom na cloud

Kada razvojnim inženjerima Umbraco prethodno odluku da biste premjestili SaaS model, oni znao da bi moraju učinkovit i skalabilni način izgradnje servis.

> "Elastic baze podataka grupe su savršen odgovara naše SaaS koja nudi jer smo kapaciteta možete birati prema gore i dolje po potrebi. Dodjeljivanje je lako i s oglednim postavljanjem smo možete zadržati Upotreba na najviše."

> – Umbraco Morten Christensen, tehničku potencijalnog klijenta

"Smo željeli trošiti vrijeme na našem na riješiti probleme naših korisnika nije upravljanje infrastrukture. Ne možemo željeli da biste lakše naših korisnika da biste dohvatili Većina vrijednost"piše Niels Hartvig, founder Umbraco. "Smo prethodno smatra hosting poslužitelje nas, ali planiranje kapaciteta činite su na nightmare." Coincidentally, Umbraco ne uključivanja administratori sve baze podataka koji underscores broj vrijednost ključa za korištenje UaaS.

Jedan važne cilj za razvojne inženjere Umbraco je pružanje način za kupce UaaS Dodjela okruženja brzo i bez ograničenja kapaciteta. No pruža namjenski servis u podatkovnim centrima Umbraco bi ste potreban veći broj suvišno kapaciteta učiniti bursts u obradi. Koji bi ste željeli dodavanje infrastrukture dosta računalnim koji bi ste je redovito underutilized.

Uz to, tim za razvoj Umbraco željeli činite dopustite da biste ponovno koristili koliko je kodu, postojeće moguće rješenje. Kao Umbraco za razvojne inženjere Mikkel Madsen statusi, "nismo zadovoljni s alatima za razvoj Microsoft koji su mi već poznajete, kao što je Microsoft SQL Server, baza podataka Microsoft Azure SQL, ASP.net i Internet Information Services (IIS). Prije nego što morate uložiti u programa IaaS ili na PaaS cloud rješenja, ne možemo željeli da biste bili sigurni da je želite podržavaju naš Alati sustava Microsoft i platforme, tako da ne imamo da biste promijenili pretraživanje velikog naš kod osnovni. "

Da biste zadovoljavaju sve kriterijima, Umbraco tražio cloud partner s kvalifikacijama za sljedeće:

-   Dovoljno kapacitetu i pouzdanosti
-   Podrška za razvoj Alati sustava Microsoft, tako da se Umbraco inženjeri će biti prisilno da biste potpuno reinvent njihove razvojno okruženje
-   Podaci o prisutnosti u svim geografske tržištima u kojem UaaS competes (tvrtke potrebno da biste bili sigurni da oni mogli pristupati svojim podacima brzo i njihovih podataka pohranjena na mjestu koje odgovara regionalnih zakonskih preduvjeta)

##<a name="why-umbraco-chose-azure-for-uaas"></a>Zašto Umbraco odabrali Azure za UaaS

S obzirom na Morten Christensen "nakon odabira na našem mogućnosti, ne možemo odabrana Azure jer je zadovoljili sve naše kriterija, mogućnost upravljanja i skalabilnost poznavanje i Isplativost. Ne možemo postaviti okruženja na Azure VMs, a svako okruženje ima baze podataka SQL Azure vlastito, uz sve instance grupe elastic baze podataka. Tako što baza podataka između razvojem, pripremna i okruženja u kojima se uživo nudimo naših korisnika robusne performanse odvajanja uparuje skaliranje – velikog win. "

Morten ne riješi, "prije, ne možemo morali ručno Dodjela poslužitelji za web-baze podataka. Sada ćemo nemate da razmislite o tome. Sve se automatski – s dodjelom resursa za čišćenje. "

Morten zadovoljni se s mogućnosti skaliranja nudi Azure. "Elastic baze podataka grupe su savršen odgovara naše SaaS koja nudi jer smo kapaciteta možete birati prema gore i dolje po potrebi. Dodjeljivanje je lako i s oglednim postavljanjem smo možete zadržati Upotreba na najviše." Država Morten "jednostavnosti elastic grupe, uz jamstvo servisa sloju utemeljen na DTUs daje nam power Dodjela nove grupe resursa na zahtjev. Nedavno, jednu od veće klijentima peaked za 100 DTUs u okruženju uživo. Korištenje Azure, naš elastic grupe navedeni baze podataka klijenta s resursima koji su potrebni u stvarnom vremenu bez potrebe za predviđanje DTU preduvjeti. Jednostavno umetnite, naših korisnika se oko Uključi vrijeme koje očekuju i smo možete upoznati naš ugovore o razini usluge performanse."

Mikkel Madsen zbraja: "Ne možemo ste embraced Napredna Azure algoritam koja se povezuje uobičajeni scenarij SaaS (novim klijentima za za uhodavanje u stvarnom vremenu na razini) naš uzorak aplikacije (unaprijed dodjeljivanje baze podataka, obje razvoj i live) pri vrhu pozadinsku tehnologiju (zajedno s bazom podataka SQL Azure pomoću redova Azure servisa Bus)."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>S Azure, UaaS je premašuju očekivanja klijenta

Nakon odabira Azure kao njegov cloud partner, Umbraco je osigurati UaaS Kupci s performansama optimizirana upravljanje sadržajem, bez ulaganja IT resursa potrebne iz rješenja koja se sama glavnom računalu. Kao što je Morten kaže "bismo za razvojne inženjere praktičnosti i skalabilnost Azure pruža nam i naših korisnika su thrilled sa značajkama i pouzdanosti. Ukupni, prošla sjajno win bismo!"
 
## <a name="more-information"></a>Dodatne informacije

- Da biste saznali više o grupe Azure elastic baze podataka, u odjeljku [grupe elastic baze podataka](sql-database-elastic-pool.md).

- Da biste saznali više o Bus servisa Azure, potražite u članku [Bus servisa Azure](https://azure.microsoft.com/services/service-bus/).

- Da biste saznali više o Web uloge i uloge suradnika, potražite u članku [uloga suradnika](../fundamentals-introduction-to-azure.md#compute). 

- Dodatne informacije o virtualne umrežavanje potražite u članku [virtualne mreže](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Da biste saznali više o i oporavak sigurnosne kopije, pogledajte [poslovanje](sql-database-business-continuity.md).  

- Dodatne informacije o nadzoru ppols potražite u članku [praćenje grupe](sql-database-elastic-pool-manage-portal.md). 

- Da biste saznali više o Umbraco kao servis, pogledajte [Umbraco](https://umbraco.com/cloud).

