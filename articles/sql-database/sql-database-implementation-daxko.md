<properties
   pageTitle="Azure Studije slučaja - Daxko/CSI baze podataka Azure SQL | Microsoft Azure"
   description="Informirajte se o tome kako Daxko/CSI koristi baze podataka SQL accelerate njegov ciklusa razvoja i poboljšati njegov služba i performanse"
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
   ms.date="09/08/2016"
   ms.author="carlrab"/>
   
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI koriste Azure accelerate njegov ciklusa razvoja i poboljšati njegov službi i performanse

![Logotip Daxko/CSI](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Softver Daxko/CSI prečica na test: je njegova baze klijenata od središta tjelovježbe i rekreacija brzo, rast zahvaljujući uspjeh rješenja njegov potpun enterprise softver, ali čuvanja s potrebama IT infrastrukturu za sve veći kupca osnovni je testiranje tvrtke IT osoblju. Tvrtka sve je ograničeno po porasta operacije pomoćni proces, osobito za upravljanje njegov sve veći baze podataka. Lošiji, te operacije indirektni izrezivanje je u razvoju resursi za nove inicijative, kao što su nove značajke za mobilnost za tvrtke softver.

Prema Nevena Molina, Director od razvoj proizvoda na Daxko/CSI Azure dao softver CSI model (PaaS) platforme kao-na-usluga koje je potrebno za Pojednostavnite upravljanje bazom podataka, povećajte skalabilnost i oslobodili resurse radi fokusiranja na softver umjesto ops. "Izvrstan za nam je azure SQL baze podataka. Ne morate brinuti o zaštiti sustava SQL Server, prebacivanje klaster i sve druge infrastrukture mora je prikladno za nam."

Nakon migracije Azure, softver CSI mora operacije drugom osoblju od samo dva upravljanje bazama podataka za iznad 600 klijenta. Tvrtka koristi baze podataka SQL Azure elastic grupe da biste premjestili baze podataka kupca ovisno o veličini i morate.

Molina ne riješi, "naših korisnika pogađa promjene odmah. Prije elastic grupe su ponekad je prodao vremensko ograničenje i ostalih problema tijekom razdoblja neprekidnog rada. S Azure elastic grupe, mogu brzi niz prema potrebi i koristiti softver bez problema."

Uz spomenuta poboljšanja performansi za korisnike, Azure elastic baze podataka grupe oslobodi softver CSI resurse za fokusiranje na razvoj novih servisa i značajki, umjesto Postupanje s operacije i upravljanje njima. Tih resursa IT pomaže CSI softver poboljšati njezin softver enterprise koja nudi SpectrumNG da biste lakše sudjelovati gym članova, poboljšanje učinkovitosti osoblje i dati osoblja i članovi mobilni pristup za interaktivno zadaci i obavijesti u stvarnom vremenu.

Azure sadrži i pomaže CSI softver accelerate i poboljšati razvoj i ciklus kvalitete (značajke pitanja i odgovora) tako da omogućite Automatizacija mogućnosti. S tvrtke Azure implementacije, upravitelji Sastavi možete objediniti komponente klikom na gumb. Kao što je Molina opisuje, "kao dio ciklusa izdanje, značajke pitanja i odgovora je sada moći implementacija okruženje za testiranje u Azure, blisko oponaša naš stogu radnog. Izgradi odmah ćemo uvesti naš razvojni okruženje za vet promjene. To je prevelik win bismo, jer smo niste imaju slične značajke za testiranje prije koji."

## <a name="offloading-to-the-cloud"></a>Rasteretite u oblak

Prije prelaska u oblak, softver CSI imali uspješno ugrađeni gore vlastitu složene infrastrukture u lokalnom podatkovnog centra u Houston. Kao što je proširen tvrtke, prečica rastuće pains growing kupnju, dodjele resursa i održavanje sve hardver i softver potreban za podršku svojih klijenata. IT organiziraju učiniti operacije postala drugi usko grlo koji su doveli usporenje u nove resurse za dodjelu resursa i vodoravnim out nove servise klijentima.

Softver CSI pokušavali u oblak mogućnosti za uklanjanje taj pomoćni proces, tako da ga nije moguće usredotočite se na njegov kod, umjesto operacije. Tvrtke koje da Mnogi davatelji gornji oblaka samo nude infrastrukture-kao-na-service (IaaS) rješenja koje je i dalje potreban velike IT osoblju za upravljanje IaaS stog. Na kraju, softver CSI određuje da rješenja Azure PaaS je najbolje pristajanje za njegov potrebama. Objašnjava Molina, "Azure dohvaća hardvera i sustava softver van, pa ćemo možete se fokusirati na našu ponudu softver tijekom smanjivanje naš IT indirektni."

## <a name="making-the-transition-to-azure"></a>Napravite prijelaz Azure

Kad odaberete Azure njegov PaaS rješenja, softver CSI počeli migracije njegov pozadinskog Infrastruktura i baza podataka s oblakom. Prije no što Azure, klijentima SpectrumNG potrebno za instalaciju klijentska aplikacija koja prenosi sa servisom Windows Communication Foundation (WCF) na pozadinska. Prema Molina, "iako se neki klijenti hostira sve u vlastite podatkovnim centrima smo ugrađeni proizvoda biti složene. Ne možemo hostira sve u podatkovnog centra u Houston, pomoću SQL Server kao spremište podataka.

"Naš proizvod i daje uključeni člana dostupnog web portal (napisan ASP.net), koji je dizajniran biti označena bijelo tako da odgovara prisutnosti web klijenta i SOAP API-JA za podršku online stranice i integraciju drugih proizvođača."

Migracija s oblakom jeste li potrajati dugo za arhitekturu. Prema Molina, "Većina trud riješili izmjena način smo čitanje informacija o datoteci konfiguracije, izmjene na središnje niz za povezivanje i automatske pakiranje, prijenos i implementaciju naš izdanja."

Za razvoj Automatizacija Sastavi inženjeri CSI softver koristi Azure PowerShell i REST API-ji za stvaranje paketa i prenesite ih pripremna okruženja za svaki noći izdanje.
Cjelokupan prijelaz na Azure oblaku implementaciju nije brzo i provođenju za tim za IT CSI softvera. Objašnjava Molina, "u svim, ne možemo imali okruženju beta u oblak unutar tri i četiri tjedna ponijeti na projektu. Koji je surprising win bismo."

Nakon konfiguriranja i testiranje okruženja, softver CSI počeli Migracija korisnika. Za korisnike koji se već koriste hostiranje CSI softver, prijelaz nije gotovo objedinjenog. U slučaju korisnika Migracija s lokalnom implementacijom migracije u oblak traje neko vrijeme dodatne, ali i dalje prvenstveno je besplatno bol za korisnika i CSI softver.

Za nove korisnike, CSI softver, IT osoblju sljedeći postupak ploči ih koristiti za Azure:

1.  Azure skripte komponente PowerShell koriste se za Okretni nove baze podataka za klijenta; Svi korisnici pokrenuti na sloju premium da biste bili sigurni dovoljno početne propusnost za prijelaz.
2.  Kada je to moguće, softver CSI koristi čarobnjak za migraciju SQL Azure da biste premjestili postojeće podatke s bazom podataka SQL Azure instancom.
3.  Na kraju, Microsoft SQL Server integraciju servisa (SSIS) se koriste za usklađivanje nepodudaranja u podatke ili da biste izvršili bilo koju čišćenja podataka po potrebi.

Danas, oko 99 postotak CSI softver klijenata nalaze se u Azure, preko četiri regionalnim podatkovnim centrima (Sjeverna središnje, Južna središnje, Istok i Zapad). Postavljanjem podatkovnim centrima u svakom kupcu zemljopisnom području Latencija zadržavaju se na najmanje.

## <a name="azure-elastic-database-pools-free-up-it-resources"></a>Azure elastic baze podataka grupe oslobodili resurse za IT

Nekoliko značajki Azure ste pomaže CSI softver shift onemogućuje infrastrukturu i operacije odnosila da biste se značajka i razvoja odnosila. Možda je najvećih prednosti iz grupe elastic baze podataka.

Softver CSI trenutno sadrži oko 550 baze podataka za korisnike. Prije nego što elastic grupe je teško upravljanje toliko bazama podataka unutar razina strukture. Upravitelji OPS morali dodijeliti performanse razine na temelju potreba neprekidnog rada kupaca koji potreban značajan IT-resursa indirektni. S grupe elastic baze podataka upravitelja možete dodijeliti klijenata premium ili Standardni skup, po potrebi, i zatim premjestite kupci ovisno o veličini i potrebna. Klijenti odmah; pogađa efekata grupe elastic baze podataka Prije elastic grupe korisnici imali vremensko ograničenje i ostalih problema tijekom razdoblja neprekidnog rada korištenje ali s elastic grupe korisnika možete doći aktivnosti bursts prema potrebi, a zatim ih možete nastaviti koristiti SpectrumNG bez problema.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure Active replikacije zemlj ubrzavati izvješćivanje o pogreškama

Nekoliko kupaca CSI softver i izvodite prednosti programa Azure Active zemlj. – replikacije. S aktivni zemlj. – replikacije, do četiri čitljiv sekundarne baze podataka može konfigurirati u istom ili drugom podatkovnog centra regijama. CSI softver za provjeru pomoću sustava Active zemlj. – replikacije na dva načina: najprije nisu dostupne u slučaju podatkovnog centra prekida ili Nemogućnost povezivanja s primarnom bazom podataka; sekundarne baze podataka i drugo, bazama podataka za sekundarne su čitljiv i može se koristiti za offload radnih opterećenja samo za čitanje kao što su izvješća zadataka. Neki klijenti CSI softver pomoću ova pogodnost accelerate izvješćivanja tijekova rada.

## <a name="csi-software-application-logic-and-architecture"></a>Logika aplikacije CSI softver i arhitekture

SpectrumNG koristi web uloge. Budući da je aplikacija više klijent, WCF servisa koristi se za izvršenje zahtjeva prvotne veze klijenata. Kao što je Molina statusi, "zahtjev označava svaki klijent, što omogućuje nam stvaranje niza za povezivanje odgovor na svoje baze podataka da biste učinili ono moramo učinite."

Za sloj web njegovu servisu CSI softvera traje prednost Azure automatske promjene veličine, ovisno o dan i vrijeme. Dostupnih resursa automatski povećati kako bi odgovarao veći korištenje tijekom radno vrijeme, prema vremensku zonu svaki regionalnog podatkovnog centra. Resursi se postaviti za promjenu veličine dane vikenda kad su niže potrebe kupca.

     
![Daxko/CSI arhitekture](./media/sql-database-implementation-daxko/figure1.png)

Slika 1. Oblak uloge servisa radnih crta strukturiranih podataka iz baze podataka SQL Azure i djelomično strukturirane podatke iz spremišta tablica. SpectrumNG korisnici upotrebljavaju podataka putem prikazom oblaka services je uloga web.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Pomoću web-aplikacije i sloj web plan za mobilne aplikacije

Koristite baze podataka SQL Azure oslobodi resursi za softver CSI da biste omogućili nove inicijative, uključujući dovršavanje platforme za mobilne na temelju prilagođenog API hostirane u Azure web-aplikacijama. Platforme omogućuje gym članova i drugo osoblje provjeriti rasporede, rezervirajte klase i primanje poruka pomoću mobilnog uređaja.

Platforme koristi vezanima uz servis arhitektura (SOA) da biste preuzeli jedan komponente – kao što su point-of-sale sustava (Položaj) ili prodaje sustava – Premjesti u hodu na neku drugu tarifu za web, a zatim Okretni prema gore na servis za podršku tu komponentu ostavite sve ostalo na izvorni plan za web. Ova mogućnost daje CSI softver ogromne mogućnosti, a on olakšava održavanje troškove prema dolje.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure omogućuje CSI softver razvojnim inženjerima fokus na aplikacijama i servisima

Azure SQL baze podataka nije samo boon SpectrumNG korisnicima, uživati servisa fast, pouzdan, preporučuje se i velike win CSI softvera IT osoblje i razvojnim inženjerima. Rasterećivanjem ops za Azure u oblaku CSI softver smanjene njegov indirektnih troškova za resurse i infrastrukture, znatno micromanage baze podataka radi optimiziranja performansi za njegov klijenata ubrzana njegov ciklusa razvoja i više ne potrebama.

## <a name="more-information"></a>Dodatne informacije

- Da biste saznali više o grupe Azure elastic baze podataka, u odjeljku [grupe elastic baze podataka](sql-database-elastic-pool.md).

- Dodatne informacije o Alati baze podataka i elastic skaliranja potražite u članku [Alati elastic baze podataka i elastic skaliranja](sql-database-elastic-scale-get-started.md).

- Dodatne informacije o migraciji baze podataka SQL Server potražite u članku [Čarobnjak za migraciju SQL Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md).

- Dodatne informacije o Active zemlj. – replikacijom potražite u članku [Aktivni zemlj replikacijom](sql-database-geo-replication-overview.md).

- Da biste saznali više o Web uloge i uloge suradnika, potražite u članku [uloga suradnika](../fundamentals-introduction-to-azure.md#compute). 

- Da biste saznali više o Bus servisa Azure, potražite u članku [Bus servisa Azure](https://azure.microsoft.com/services/service-bus/).

- Da biste saznali više o automatsko skaliranje, u odjeljku [Skaliranje servise u oblaku](../cloud-services/cloud-services-how-to-scale.md).
