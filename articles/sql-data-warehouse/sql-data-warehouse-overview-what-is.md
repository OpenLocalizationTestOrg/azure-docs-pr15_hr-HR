<properties
   pageTitle="Što je Azure SQL Data Warehouse? | Microsoft Azure"
   description="Enterprise predmete distributed instaliranog obradu petabyte količine podataka relacijski i koje nisu relacijske baze podataka. Je na industrijskih prvi oblaka podataka skladištu s Povećaj, Smanjivanje i zadržite pokazivač u sekundama."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Što je Azure SQL Data Warehouse?

Azure SQL Data Warehouse je u oblaku, skala iz baze podataka može obrade pretraživanje velikog količine podataka, relacijski i koje nisu relacijske. Ugrađena na našem arhitektura massively paralelno obrada (MPP), SQL Data Warehouse možete rukovati radno opterećenje vaše tvrtke.

SQL Data Warehouse:

- Kombinira relacijske baze podataka SQL Server s mogućnostima skaliranje iz Azure oblaka. Možete povećati, Smanji, zadržite pokazivač ili nastavak računalnim u sekundama. Spremite troškove skaliranje out procesora kada vam je potrebna i vrijeme koje nisu Vršna izrezivanje ponovno korištenje.
- Upravlja Azure platforme. Je jednostavno je za implementaciju, jednostavno održava, a potpuno kvara pogreške zbog automatsko sigurnosne kopije.
- Dopuna zajednici sustava SQL Server. Razviti s alatima i poznatih SQL Server Transact-SQL (T-SQL).

U ovom se članku opisuju ključne značajke programa SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Arhitektura massively paralelno obrada

SQL Data Warehouse je massively paralelno obrada (MPP) distribuirati bazu podataka sustava. Dijeljenje podataka i obrada mogućnost preko više čvorove, SQL Data Warehouse može ponuditi veliki skalabilnost - daleko izvan sistemske jedan.  U pozadini SQL Data Warehouse ili duplerica podataka preko mnoge zajedničke nothing prostora za pohranu i obrada jedinice. Podaci biti pohranjene u Premium lokalno suvišnih prostora za pohranu i povezana za izračunavanje čvorove za izvršavanje upita. Pomoću ovog arhitektura SQL Data Warehouse vodi "Ukrotite" pristup opterećenje i složene upite. Zahtjevi za su primio čvor kontrola, optimizirane i proslijediti čvorove računalnim kako rade paralelno.

Kombiniranjem mogućnosti Azure prostora za pohranu i MPP arhitektura SQL Data Warehouse učiniti sljedeće:

- Povećavanje ili smanjivanje prostora za pohranu neovisno o računalnim.
- Povećaj ili Smanji računalnim bez premještanja podataka.
- Zaustavljanje izračunati kapaciteta zadržavanje podataka ostaje netaknuta.
- Životopis za izračun kapacitetu obavijest na trenutak.

Sljedeći dijagram prikazuje arhitekturu detaljnije.

![Arhitektura skladištu podataka za SQL][1]


**Čvor kontrole:** Kontrola čvor upravlja i optimizira upita. To je sučelje koje podržava interakciju sa svim aplikacijama i veze. U SQL Data Warehouse čvor kontrola se pokreće SQL baze podataka i povezivanja s njom Traži i izgleda isto. U odjeljku površina, čvor kontrola koordinate sve premještanje podataka i izračuni koji su potrebni za pokretanje paralelno upita na raspodijeljeno podataka. Kada pošaljete T SQL upita u SQL Data Warehouse, čvor kontrola je pretvara u zasebnom upite koji se izvode na svakom čvor računalnim paralelno.

**Izračunati čvorove:** Čvorovi računalnim služe kao power iza SQL Data Warehouse. To su baze podataka SQL spremanje podataka i obradu upita. Kada dodate podatke, SQL Data Warehouse distribuira reci koje treba vaše računalnim čvorove. Čvorovi računalnim su zaposlenici zaduženi za koji se izvode paralelno upita nad podacima. Nakon obrade, oni prenesite rezultate natrag čvor kontrolu. Da biste završili upit, čvor kontrola zbrajaju rezultati i vraća konačni rezultat.

**Prostor za pohranu:** Vaš podaci se pohranjuju u spremište blobova platforme Azure. Kada izračunati čvorove interakcije s njima, oni pisanje i pročitajte izravno i iz spremišta blobova. Budući da Azure prostora za pohranu proširuje proziran i vastly, SQL Data Warehouse možete isto. Budući da su nezavisne računalnim i pohranu SQL Data Warehouse može automatski promijeniti omjer prostora za pohranu odvojeno od skaliranja računalnim i obratno. Azure blobova je potpuno kvara pogreške, i pojednostavnjuje postupak sigurnosnog kopiranja i vraćanja.

**Usluga za premještanje podataka:** Servis za premještanje na podataka (DMS) premješta podatke između čvorove. DMS omogućuje pristup čvorove računalnim podataka koje su im potrebne za spojevi i agregacije. DMS nije Azure servisa. Je servis Windows koja se izvršava duž bazom podataka sustava SQL na sve čvorove. Budući da DMS izvođenja u pozadini, nećete interakcije s njom izravno. Međutim, kada pogledate tarife upita, primijetit ćete to su neki postupci DMS Budući da je potrebno da biste pokrenuli svaki upit paralelno premještanje podataka.


## <a name="optimized-for-data-warehouse-workloads"></a>Optimizirana radnih opterećenja skladištu podataka

Pristup MPP je pomoću prema broju podataka skladištenje optimizacije karakteristično performanse, uključujući:

- Alat za optimizaciju raspodijeljenom upitu i skup složene Statistika preko svih podataka. Korištenje informacija na web-mjesto veličina podataka i u okvir za raspodjelu, servis je moći optimizira upita assessing trošak operacije određene raspodijeljeno upita.

- Napredni algoritmi i tehnika kojima se integrirati u postupka premještanje podataka učinkovito premještanje podataka među računalstvo resurse potrebne za izvršavanje upita. Ove postupke za premještanje podataka ugrađeni u, a sve optimizacije na servis za premještanje podataka automatski se dogoditi.

- Grupirani **columnstore** indeksa prema zadanim postavkama. Pomoću prostora za pohranu s operacijskim sustavom SQL Data Warehouse može vidjeti u 5 x sažimanja pozitivne putem klasične orijentirana prostora za pohranu i najviše 10 x ili više pozitivne performanse upita. Analize upita koje je potrebno pregledati velikog broja redaka rad sjajno na columnstore indeksi.


## <a name="predictable-and-scalable-performance"></a>Performanse predvidljivi i skalabilni

SQL Data Warehouse odvaja i računalnim koji omogućuje svakom skaliranje neovisno prostor za pohranu. SQL Data Warehouse možete brzo i jednostavno mjerilo da biste dodali dodatne računalnim resursa na obavijest na trenutak. Korištenje spremište blobova platforme Azure complementing to je. Blob-ova omogućavaju stabilan, repliciranu prostora za pohranu, ali i Infrastruktura jednostavno proširenja pri niskoj trošak. Pomoću kombinacije oblaka skaliranje prostora za pohranu i računalnim Azure SQL Data Warehouse omogućuje platiti performanse upita i pohranu kada vam je potrebna. Promjena količine računalnim jednostavan je premjestite klizač na portalu za Azure lijeve ili desne strane ili također može se zakazati pomoću T SQL i PowerShell.

Uz mogućnost potpuno kontrolu količine računalnim neovisno o prostora za pohranu, SQL Data Warehouse omogućuje potpuno zadržite pokazivač na skladištu podataka, što znači da ne platite računalnim kada ga nije potrebno. Zadržavanje prostora za pohranu mjestu sve računalnim je objavio u glavni skup Azure, spremanje novca. Po potrebi, jednostavno nastaviti s računalnim i imate podatke i izračunati dostupne za svoje radno opterećenje.

## <a name="data-warehouse-units"></a>Jedinice skladištu podataka

Dodjela resursa za SQL Data Warehouse mjeri se u jedinicama skladištu podataka (DWUs). DWUs su mjera podlozi resurse kao što su procesora, memorije, IOPS koji su dodijeliti SQL Data Warehouse. Povećanje broja DWUs povećava resurse i performanse. Konkretno, DWUs pomoći provjerite:

- Prikazuje vam se jednostavno skaliranja skladištu podataka bez brige o pozadini hardver ili softver.

- Poboljšanje performansi za jednu razinu DWU možete predviđanje prije nego što promijenite veličinu skladištu podataka.

- Temeljni hardvera i softvera na instance možete promijeniti i premještanje bez utjecaja na performanse radno opterećenje.

- Microsoft može mijenjati podlozi arhitektura servisa bez utjecaja na performanse svoje radno opterećenje.

- Microsoft hitro možete poboljšati performanse u SQL Data Warehouse na način koji je skalabilni i ravnomjerno efekte u sustavu.

Jedinice skladištu podataka navedite mjera širine tri precizno metriku koja iznimno povezanog s podacima skladištenje radno opterećenje performansi. Cilj je da sljedećeg ključa radno opterećenje metriku će promijeniti veličinu Linearno s DWUs koje ste odabrali za skladištu podataka.

**Pregleda/zbrajanja:** Ova metrika radno opterećenje vodi standardni podaci skladištenje upit koji skenira velikog broja redaka, a zatim složene zbrajanja. Ovo je/i i procesora intenzivno operacije.

**Opterećenja:** Ova metrika mjeri mogućnost ingest podataka na servis. Opterećenje završe s PolyBase učitavanje predstavnik za skup podataka iz spremišta blobova platforme Azure. Ova metrika namijenjen je opterećenjem mreže i procesora vidove servisa.

**Stvaranje tablice kao potvrdite (CTAS):** CTAS mjeri mogućnost kopiranja tablice. To uključuje čitanja podataka iz spremišta, distribucija preko čvorove na uređaj i ponovno ga pisanje prostora za pohranu. To je intenzivno operacija procesora, IO i mreže.

## <a name="pause-and-scale-on-demand"></a>Zaustavljanje i skaliranje na zahtjev

Kada vam je potrebna brže rezultate, povećajte vaše DWUs i platiti za veće performansi. Kada morate manje izračunati power, smanjite vaše DWUs i platiti samo za što vam je potrebno. Što mislite o promjeni vaše DWUs u sljedećim scenarijima:

- Kada ne morate omogućiti pokretanje upita, možda u večeri ili vikenda, postupno upite. Zatim zadržite pokazivač računalnim resurse da biste izbjegli plaćanja DWUs kada vam nisu potrebni.

- Ako vaš sustav ima manje zahtjev, razmislite o smanjivanju DWU malu veličinu. I dalje možete pristupiti podacima, ali ušteda značajan troškova.

- Prilikom izvršavanja podebljanom podataka operacije učitavanje i transformaciju, preporučujemo vam skaliranja tako da se podaci su dostupni brže.

Da biste razumjeli što je to prikladno DWU vrijednosti, pokušajte skaliranje gore i dolje i pokrenut nekoliko upita nakon učitavanja podataka. Budući da je brzi skaliranje, možete ga isprobati broj različite razine performanse u jedan sat ili manje.  Imajte na umu SQL Data Warehouse namijenjen za obradu velike količine podataka te da biste vidjeli prave mogućnosti skaliranja, osobito na veću nudimo ljestvice ćete koji želite koristiti u velikom skupu podataka koji izvršenja ili premašuje 1 TB.


## <a name="built-on-sql-server"></a>Utemeljena na SQL Server

SQL Data Warehouse temelji se na modul relacijske baze podataka sustava SQL Server, a sadrži brojne značajke očekivati od programa skladištu podataka tvrtke. Ako znate već T SQL, jednostavno je za prijenos svoje znanje SQL Data Warehouse. Hoće li se dodatno ili tek ste počeli raditi, Primjeri preko dokumentaciju će vam pomoći da počnete. Ukupni, razmislite o način da ne možemo ste sastavljena jezik elemenata SQL Data Warehouse na sljedeći način:

- SQL Data Warehouse koristi T SQL sintaksa za mnoge operacije. Podržava i općenite skup tradicionalni konstrukta SQL, kao što su pohranjene procedure, korisnički definirane funkcije, particija tablice, indekse i uparivanja.

- SQL Data Warehouse sadrži i broj novije značajke sustava SQL Server, uključujući: grupirani **columnstore** indeksi, Integracija PolyBase i podataka iz nadzor (dovršeno uz prijetnju Procjena).

- Određene elemente T SQL jezika koji je rjeđe podataka skladištenje radnih opterećenja ili su novije sa sustavom SQL Server možda neće biti dostupan. Dodatne informacije potražite u [dokumentaciji za migraciju][].

S zajedništvo između sustava SQL Server, SQL Data Warehouse, SQL baze podataka i analize platformu sustava u Transact-SQL i značajka razviti rješenje koje odgovara vašim potrebama podataka. Možete odabir mjesta da biste zadržali podatke, na temelju performanse, sigurnost i preduvjeti mjerilo, a zatim prijenos podataka prema potrebi između različitih sustava.

## <a name="data-protection"></a>Zaštita podataka

SQL Data Warehouse sprema sve podatke u Azure Premium lokalno suvišnih prostora za pohranu. U centru za lokalnih podataka da bi zaštita prozirne podataka u slučaju pogreške lokalizirane održavaju se većeg broja primjeraka sinkrono podataka. Osim toga, SQL Data Warehouse automatski stvara sigurnosnu kopiju aktivnog (nepotvrđen pauzirano) baza podataka u pravilnim vremenskim razmacima pomoću Azure prostora za pohranu snimke. Da biste saznali više o korištenju sigurnosnog kopiranja i vraćanja funkcionira, potražite u članku [Pregled sigurnosnog kopiranja i vraćanja][].

## <a name="integrated-with-microsoft-tools"></a>Integrirani Alati sustava Microsoft

SQL Data Warehouse također se integrira Brojni alati taj SQL Server može biti upoznati s korisnicima. To obuhvaća:

**Tradicionalni SQL Server Alati:** SQL Data Warehouse posve integriran s SQL Server Analysis Services i servisima za integraciju komponente Reporting Services.

**Oblaku Alati:** SQL Data Warehouse može se koristiti uz broj novih alata u Azure, uključujući tvorničke podataka, strujanje analize, strojnog učenja i Power BI. Popis potpuniji potražite u članku [Pregled integrirani Alati][].

**Alata drugih proizvođača:** Velikog broja davatelja alata drugih proizvođača imaju potvrđeni Integracija njihove alata s SQL Data Warehouse. Potpuni popis potražite u članku [SQL Data Warehouse rješenja partnera][].

## <a name="hybrid-data-sources-scenarios"></a>Hibridno scenarijima za izvore podataka

Pomoću SQL Data Warehouse PolyBase omogućuje korisnicima neviđen da biste premjestili sadržaj preko njihove zajednici otključavanje omogućuje postavljanje scenarija napredne hibridnog s koje nisu relacijski i lokalnih izvora podataka.

Polybase omogućuje vam korištenje podataka iz različitih izvora pomoću poznate T SQL naredbe. Polybase omogućuje upita koji nisu relacijskih podataka koji sadrži spremište blobova platforme Azure kao da je obične tablice. Pomoću Polybase upita koji nisu relacijskih podataka ili da biste uvezli koje nisu relacijskih podataka u SQL Data Warehouse.

- PolyBase koristi vanjski tablice da biste pristupili koje nisu relacijskih podataka. Definicija tablice spremaju se u SQL Data Warehouse im možete pristupiti pomoću SQL i alate kao što ste želite pristupiti normalni relacijskih podataka.

- Polybase je agnostic u integracija. Ga izlaže iste značajke i funkcije svi izvori koje podržava. Podaci koji se pročitati Polybase mogu biti u različitim formatima, uključujući razgraničene datoteke ili datoteke ORC.

- PolyBase mogu se koristiti za pristup blobova koje se i koristi kao prostora za pohranu za programa klaster HDInsight. To s istim podacima pomoću alata za relacijske i koje nisu relacijske omogućuje vam pristup.

## <a name="sla"></a>SLA

SQL Data Warehouse nudi na proizvod razine razine ugovor o usluzi (SLA) kao dio sustava Microsoft Online Services SLA. Dodatne informacije potražite u članku [SLA za SQL Data Warehouse][]. SLA informacije o svim drugim proizvodima možete posjetiti Azure [Ugovore o razini usluge] stranice ili ih preuzeti na stranici [Količinskog licenciranja][] . 

## <a name="next-steps"></a>Daljnji koraci

Sad kad znate nekoliko riječi o SQL Data Warehouse, Saznajte kako brzo [stvoriti SQL Data Warehouse][] i [Učitavanje ogledne podatke][]. Ako ste novi korisnik Azure, vidjet ćete [Azure Pojmovnik][] korisni kao naiđete na novu terminologija. Ili, pogledajte neke od tih druge SQL skladištu izvor podataka.  

- [Priče o uspjehu korisnika klijenta]
- [Blogovi]
- [Zahtjeve za značajke]
- [Videozapisi]
- [Korisnička savjeta blogova]
- [Stvaranje zahtjev za podršku možete]
- [MSDN forum]
- [Forum prelijevanja stogu]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Stvaranje zahtjev za podršku možete]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Učitavanje oglednih podataka]: ./sql-data-warehouse-load-sample-databases.md
[Stvaranje SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Dokumentacija za migraciju]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse rješenja partnera]: ./sql-data-warehouse-partner-business-intelligence.md
[Kratki pregled integrirani alata]: ./sql-data-warehouse-overview-integrate.md
[Pregled sigurnosnog kopiranja i vraćanja]: ./sql-data-warehouse-restore-database-overview.md
[Pojmovnik za Azure]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Priče o uspjehu korisnika klijenta]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogovi]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Korisnička savjeta blogova]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Zahtjeve za značajke]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Forum prelijevanja stogu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videozapisi]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA za SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Količinsko licenciranje]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Ugovore o razini usluge]: https://azure.microsoft.com/en-us/support/legal/sla/
