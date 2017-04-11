<properties
   pageTitle="Dizajniranje obrazaca za složene SaaS aplikacije i baze podataka SQL Azure | Microsoft Azure"
   description="U ovom se članku govori o preduvjeti i uobičajene uzorke podataka arhitektura aplikacija u okruženju oblaka treba uzeti u obzir složene baze podataka i različite tradeoffs povezan te uzorcima. Je također objašnjava kako baze podataka SQL Azure, s elastic grupe i Alati za elastic pomoći adresa te uvjete u ne-narušena pouzdanost način."
   keywords=""
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
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Dizajniranje obrazaca za složene SaaS aplikacije i baze podataka SQL Azure

U ovom se članku možete saznati o preduvjetima i zajedničkim podacima arhitektura uzoraka složene softvera kao aplikacija za baze podataka usluge (SaaS) koji se izvode u okruženje oblaka. Također objašnjava čimbenici koje treba uzeti u obzir i na gubitke uzorka različite dizajn znakova. Elastic Alati u bazi podataka SQL Azure i elastic grupe omogućuju vašim potrebama ne ugrozi druge ciljeva.

Razvojni inženjeri ponekad izvršiti odabir koji rade prema njihovim Dugoročne najbolje interesima kada ih dizajniranje tenancy modela za razine podataka složene aplikacija. Prethodno, barem razvojni inženjer može to olakšani razvoja i donjem oblaka davatelja troškove servisa kao važnija od odvajanja klijenta ili skalabilnost aplikacije. Taj odabir može dovesti do opasnosti zadovoljstvo klijenta i skup ispravak tečaja kasnije.

Aplikacija smješten u okruženje oblaka je složene aplikacije i koji sadrži isti skup usluga stotine ili tisuće drugih korisnika koji zajednički koristite ili potražite u članku tuđe podataka. Primjer je SaaS aplikacije koja omogućuje servise drugih korisnika u okruženju oblak hostira.

## <a name="multitenant-applications"></a>Složene aplikacije

U složene aplikacije podataka i radno opterećenje mogu se jednostavno particije. Koje možete particija podataka i radno opterećenje, na primjer, preko granica klijentu jer zahtjeva za većinu pojaviti unutar ograničenja koja nalaže klijentom. Svojstvo je postojećih podataka i povećavaju pa ga bolje uzoraka aplikacije koji se spominju u ovom članku.

Razvojni inženjeri koristiti ovu vrstu aplikacije preko cijele spektra oblaku aplikacije, uključujući:

- Aplikacija za baze podataka partnera koje su se prebačena u oblak kao SaaS aplikacije
- Aplikacija SaaS komponenti za oblak na prema gore
- Izravni, kupca dostupnog aplikacije
- Zaposlenik dostupnog aplikacijama

SaaS namijenjene u oblak i one s korjenovanje kao partnera aplikacija za baze podataka obično su aplikacije složene aplikacije. Ove aplikacije za SaaS isporučiti specijalizirane softverom kao servis svojim korisnicima. Samoposlužni možete pristupiti servisa aplikacije i imaju potpunu vlasništvo nad povezane podatke pohranjene kao dio aplikacije. No da biste iskoristili prednosti SaaS, morate klijenata surrender neke kontrole nad vlastite podatke. Oni pouzdanost usluga SaaS da biste zadržali sigurnih i Izolirani svoje podatke iz podataka s drugim korisnicima. Primjeri ove vrste složene aplikacije SaaS su MYOB, SnelStart i Salesforce.com. Svaki od tih aplikacija možete particije duž granice klijenta i podršku aplikacije dizajniranje obrazaca ćemo razmotriti u ovom se članku.

Programi koji pružanje usluge Izravni klijentima ili zaposlenike u tvrtki ili ustanovi (često se nazivaju korisnika, a ne klijenata) su drugu kategoriju na razni načini složene aplikacije. Kupcima pretplatiti na servis i vlasnik davatelj usluga prikuplja i sprema podatke. Davatelji usluga tretiraju manje stroga da se podaci svoje kupce Izolirani međusobno izvan propisa državne mandated izjave o zaštiti privatnosti. Primjeri ove vrste klijenta dostupnog složene aplikacije su davatelje usluga medijskih sadržaja kao što su Netflix, Spotify i Xbox LIVE. Još primjera jednostavno partitionable aplikacije klijenta dostupnog, Internet skaliranje aplikacije ili Internet stvari (IoT) aplikacije koje svakog korisnika ili uređaj može poslužiti kao particije. Ograničenja particija možete razdvojiti korisnicima i uređajima.

Sve particije aplikacije jednostavno duž jednog svojstva, primjerice klijent, klijenta, korisnika ili uređaj. Resurs složene enterprise planiranje aplikacije (ERP), na primjer, ima proizvode, narudžbe i klijenti. Obično je složene sheme s tisuće iznimno međusobno povezanih tablica.

Nema strategije jedan particija možete primijeniti na sve tablice i radi preko radno opterećenje aplikacije sustava. U ovom se članku usredotočuje se na složene aplikacije koje ste radnih opterećenja i jednostavno partitionable podataka.

## <a name="multitenant-application-design-trade-offs"></a>Složene aplikacije dizajna gubitke

Uzorak dizajna koji složene aplikacije za razvojne inženjere odabire obično se temelji na važna od sljedećih čimbenika:

-   **Odvajanja klijenta**. Programer mora provjerite imaju li bez klijentu neželjene pristup podacima s drugim korisnicima. Obavezne odvajanja prenosi i druga svojstva, primjerice omogućuje zaštitu od oscilirajuće susjeda, moći vratiti podatke na klijentu i implementacije prilagodbe specifične za klijenta.
-   **Oblak trošak resursa**. Zatvaranje SaaS mora biti konkurencije trošak. Složene aplikacije za razvojne inženjere možda odlučite optimizirati za donjem trošak koristi oblaka resurse, kao što je prostor za pohranu, a zatim izračunati troškove.
-   **Jednostavnost DevOps**. Složene aplikacije za razvojne inženjere mora uključivanje odvajanja zaštitu, zadržali, praćenje stanja svoje aplikacije i shemu baze podataka i otklanjanje poteškoća s klijentom. Složenosti razvoj aplikacija i operacija prevodi izravno na veću trošak i izazove s zadovoljstvo klijenta.
-   **Skalabilnost**. Mogućnost da biste postupno dodali više klijenata i kapacitet klijenata kojima je potreban je je izuzetne za uspješan rad SaaS.

Svaki od sljedećih čimbenika ima gubitke u usporedbi s drugom. Najniže trošak oblaka koja nudi možda nude najpraktičnija razvojno okruženje. Nije važno da razvojni inženjer da biste odabire informirali o tim mogućnostima i njihovih gubitke tijekom postupka dizajniranja aplikacije.

Popularne razvoj uzorak je paket više klijenata u jednoj ili nekoliko baza podataka. Prednosti takvog su niže trošak jer plaćate za nekoliko baze podataka i relativne jednostavnosti u radu s ograničen broj baza podataka. No, s vremenom će na SaaS složene razvojni inženjer shvatite da taj odabir ima znatno downsides u klijentu odvajanja i skalabilnost. Ako odvajanja klijent postane važne, dodatne trud potreban je zaštititi podatke klijenta u zajedničkog prostora za pohranu od neovlaštenog pristupa ili oscilirajuće susjeda. Ovaj dodatni trud možda značajno povećati naporima razvoj i troškova održavanja odvajanja. Isto tako, ako je potrebno dodavanje klijenata, ovaj uzorak dizajn obično zahtijeva stručna znanja da biste ponovno Raspodijeli klijentu podataka preko baze podataka da bi se ispravno skaliranje sloju podataka aplikacije.  

Klijent odvajanja često je bitno preduvjet u aplikacijama složene SaaS cater za tvrtke ili ustanove. Razvojni inženjer može biti tempted tako da dojam prednosti jednostavnosti i trošak pred odvajanja klijenta i skalabilnost. Ovaj trade-off mogu biti složene i skupi rastom usluge i preduvjeti odvajanja klijentu postaju važna i upravljani na aplikacijskom sloju. Međutim, u složene aplikacije koje nude Izravni, potrošača dostupnog servisa kupci, odvajanja klijentu možda niži prioritet nego optimizacija za oblak trošak resursa.

## <a name="multitenant-data-models"></a>Složene podatkovne modele

Uobičajeni dizajn postupci za smještanje podataka klijentu slijedite tri različita modele, prikazano slika 1.


  ![Aplikacija složene podatkovne modele](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 slika 1: uobičajene dizajn postupci za složene podatkovne modele

-   **Baza podataka po klijentu**. Svaki klijent ima vlastitu bazu podataka. Sve specifične za klijent podaci ograničeni na baze podataka na klijentu i Izolirani iz drugih klijenata i svoje podatke.
-   **Zajedničko korištenje baze podataka sharded**. Omogućite zajedničko korištenje više klijenata za jedan od više baza podataka. Distinct skup klijenata dodijeljene svaki baze podataka pomoću stvaranje particija strategije kao što su predmemorije, raspon ili popis particija. Ovaj strategije distribuciju podataka često se nazivaju sharding.
-   **Zajedničko korištenje baze podataka-jednostruki**. Baze podataka jedan, ponekad velike sadrži podatke za sve drugih korisnika koji su disambiguated u stupcu ID klijenta.

> [AZURE.NOTE] Programa razvojni inženjer može odabrati klijenata za različite smjestili u drugu bazu podataka sheme, a zatim pomoću naziv sheme pri klijenata za različite. Ne preporučujemo takvog jer je obično zahtijeva korištenje dinamički SQL, a može biti efektivnu u predmemoriju plan. U ostatak ovog članka možemo fokus na pristup zajedničkoj tablice za kategoriju složene aplikacije.

## <a name="popular-multitenant-data-models"></a>Popularne složene podatkovne modele

Važno da biste procijenili različite vrste složene podatkovne modele kategorijama u aplikaciji dizajn gubitke smo ste već označena je. Ovi faktori pomoći karakteriziraju na tri najčešće složene podatkovne modele opisanih i način korištenja svoje baze podataka kao što je prikazano na slici 2.

-   **Odvajanja**. Stupanj odvajanja klijenata može biti mjera koliko odvajanja klijentu postiže podatkovnog modela.
-   **Oblak trošak resursa**. Iznos resursa zajedničko korištenje između klijenata možete optimizirati trošak resursa oblaka. Resurs može se definirati kao trošak računalnim i pohranu.
-   **Trošak DevOps**. Jednostavnost razvoj aplikacija, uvođenje i mogućnost upravljanja smanjuje ukupni trošak operacije SaaS.  

U slika 2 osi Y prikazuje razinu odvajanja klijenta. OS X prikazuje razinu zajedničkog korištenja resursa. Sivi, dijagonalno strelicu u sredini naznačuje smjer troškova DevOps tending da biste povećali ili smanjili.

![Popularne složene aplikacije dizajna uzoraka](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) slika 2: popularne složene podatkovne modele

U donjem desnom kvadrantu u slika 2 prikazuje se uzorka aplikacije koji koristi potencijalno velikih, dijeljenom bazom podataka jedan, i zajedničke tablice (ili zasebnom sheme) pristup. Dobro resursa zajedničkog korištenja jer klijenata za sve koristiti istu bazu podataka resursa (CPU-a, memorije, ulaza i izlaza) u bazi podataka jedan je. No ograničen odvajanja klijenta. Možda ćete morati poduzeti dodatne korake da biste zaštitili klijenata međusobno na aplikacijskom sloju. Sljedeće dodatne korake možete povećati znatno trošak DevOps razvoja i upravljanje aplikacija. Skalabilnost ograničen skale hardver koji hostira baze podataka.

U donjem lijevom Kvadrant u slika 2 ilustrira više klijenata za sharded preko više baza podataka (obično različite hardverske skaliranja jedinice). Svaku bazu podataka hostira podskup klijenata, koje rješava problem skalabilnost uzorka drugih znakova. Ako je više kapacitet potrebni za više klijenata, možete jednostavno postavite na klijenata na dodijeliti nove jedinice skaliranje hardver nove baze podataka. Međutim, je smanjiti količinu zajedničko korištenje resursa. Samo klijenata stavili istim jedinicama skaliranje resursi za zajedničko korištenje. Taj se način nudi malo unaprjeđivanja za klijent odvajanja jer više klijenata i dalje su collocated bez koja se automatski zaštićeni iz svih ostalih akcije. Aplikacija složenosti ostaje visoka.

Gornji lijevi Kvadrant u slika 2 je treća pristup. Postavlja svaki klijent podataka na vlastitu bazu podataka. Taj se način ima dobar klijentu odvajanja svojstva, ali ograničenjima prilikom svakog baza podataka ima vlastitu namjenski resursa, zajedničko korištenje resursa. Taj se način je dobar ako klijenata za sve predvidljivi radnih opterećenja. Ako radnih opterećenja klijent postane manje predvidljivi, davatelja usluge ne možete optimizirati zajedničko korištenje resursa. Unpredictability nije zajednička SaaS aplikacija. Davatelj usluge mora biti ili prekoračenja dodjele resursa da bi odgovarao zahtjeve ili niže resursi. Rezultati akcija ili veće troškove ili u donjem zadovoljstvo klijenta. Veći stupanj resursa zajedničko korištenje u klijenata postaje poželjno da bi se više učinkovit rješenja. Povećanje broja baze podataka i povećava DevOps trošak implementacije i održavanje aplikacije. Bez obzira na ove opasnosti ovu metodu nudi odvajanja najboljim i najjednostavnije za drugih korisnika.

Sljedećih čimbenika utječe i dizajn uzorak klijent odluči:

-   **Vlasništvo nad klijentu podataka**. Aplikacija u kojoj klijenata zadržati vlasništvo nad vlastite podatke bolje uzorak jednu bazu podataka po klijentu.
-   **Promjena veličine**. Zatvaranje namijenjen stotine tisućica ili milijune klijenata bolje pristupa kao što su sharding za zajedničko korištenje baze podataka. Preduvjeti za odvajanja i dalje mogu predstavljati izazove.
-   **Vrijednost i model tvrtke**. Ako se aplikacija po-klijentu prihod ako male (manje od dolar), preduvjeti odvajanja postaju manje od ključne važnosti i sa zajedničkom bazom podataka smisla. Ako je prihod po klijentu nekoliko dolarima ili više njih, model baze podataka po klijentu je više izvedivo. Možda će vam pomoći smanjiti troškove razvoj.

Dani u dizajn gubitke prikazan na slici 2, idealna složene modela mora Uključivanje svojstva odvajanja dobar klijenta s optimalnih resursa zajedničko korištenje između klijenata. Ovaj model stane u kategoriji opisane u gornjem desnom kutu Kvadrant slika 2.

## <a name="multitenancy-support-in-azure-sql-database"></a>Podrška za multitenancy u bazi podataka SQL Azure

Baze podataka SQL Azure podržava sve složene aplikacije uzoraka navedene u slika 2. Elastic grupe i podržava programa uzorak aplikacije koje kombinira dobar izbor zajedničko korištenje i odvajanja prednosti baze podataka-po-klijentu postići (pogledajte Kvadrant gornjem desnom kutu u slika 3). Alati elastic baze podataka i mogućnosti u bazi podataka SQL pomažu smanjenju troškova razvijaju i funkcioniranje aplikacije koja ima mnoge baze podataka (prikazan u području osjenčani slika 3). Ti alati omogućuju stvaranje i upravljanje aplikacijama koje koristite neku od uzoraka više baze podataka.

![Uzorci u bazi podataka SQL Azure](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) slika 3: uzorci složene aplikacije u bazi podataka SQL Azure

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Baza podataka po klijentu model s alatima i elastic grupe

Elastic grupe u bazi podataka SQL kombinirati odvajanja klijenta s resursa zajedničko korištenje među klijentske baze podataka da bi bolje podrške pristup bazi podataka po klijentu. Baze podataka SQL je rješenje sloju podataka za davatelja usluga SaaS tko izraditi složene aplikacije. Teret resursa zajedničko korištenje između klijenata se pomiče na aplikacijskom sloju servisa sloju baze podataka. Složenost upita na razini preko baze podataka i upravljanje je pojednostavnjeni elastic zadacima, elastic upita, elastic transakcije i klijentska biblioteka elastic baze podataka.

| Preduvjeti za aplikaciju | Mogućnosti baze podataka SQL |
| ------------------------ | ------------------------- |
| Odvajanja klijenta i zajedničko korištenje resursa | [Elastic grupe](sql-database-elastic-pool.md): dodijeliti skup resursa SQL baze podataka i zajedničko korištenje resursa preko različitih baza podataka. Osim toga, pojedinačne baze podataka možete crtati suvišni resursa iz grupe aplikacija prema potrebi kako bi odgovarao kapaciteta zahtjev krivina zbog promjene u klijentu radnih opterećenja. Elastic skup sam može biti skalirana prema gore ili dolje po potrebi. Elastic grupe omogućuju i jednostavno mogućnost upravljanja i nadzor i otklanjanje poteškoća na razini grupe aplikacija. |
| Jednostavnost DevOps preko baze podataka | [Elastic grupe](sql-database-elastic-pool.md): kao što je naznačeno neke starije verzije.|
||[Elastic upita](sql-database-elastic-query-horizontal-partitioning.md): upita u bazama podataka za izvješćivanje ili unakrsno klijent analize.|
||[Elastic poslovi](sql-database-elastic-jobs-overview.md): paketa i pouzdano Implementirajte ga operacije održavanja baze podataka ili promjene sheme baze podataka u više baza podataka.|
||[Elastic transakcije](sql-database-elastic-transactions-overview.md): postupak promjene na nekoliko baze podataka u atomske i izolirani način. Kada aplikacija potrebna "sve ili ništa" jamstva putem nekoliko postupaka baze podataka, potrebne su elastic transakcije. |
||[Biblioteka klijentski elastic baze podataka](sql-database-elastic-database-client-library.md): Upravljanje distribucija podataka i mapirajte klijenata s bazama podataka. |

## <a name="shared-models"></a>Zajedničke modela

Kako je opisano ranije, većina davatelja usluga SaaS zajedničko model pristup mogu predstavljati probleme s poteškoćama odvajanja klijenta i complexities s razvoj aplikacija i održavanje. Međutim, za složene aplikacije koje nude usluge izravno koje korisnici, klijentu odvajanja preduvjeti možda neće biti visokim prioritet kao minimiziranje trošak. Možda ih moći paket klijenata u jednu ili više baza podataka na visok gustoće da biste smanjili troškove. Modeli zajedničko korištenje baze podataka pomoću baze podataka za jedan ili više sharded baza podataka može rezultirati dodatne učinkovitosti u zajednički koristiti i ukupni trošak resursa. Baze podataka SQL Azure sadrži neke značajke koje korisnicima olakšava stvaranje odvajanja za poboljšanje sigurnosti i upravljanje na razini sloju podataka.

| Preduvjeti za aplikaciju | Mogućnosti baze podataka SQL |
| ------------------------ | ------------------------- |
| Značajke sigurnosti odvajanja | [Sigurnost na razini retka](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Shemu baze podataka](https://msdn.microsoft.com/library/dd207005.aspx) |
| Jednostavnost DevOps preko baze podataka | [Elastic upita](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Elastic poslova](sql-database-elastic-jobs-overview.md) |
|| [Elastic transakcije](sql-database-elastic-transactions-overview.md) |
|| [Biblioteka klijentski elastic baze podataka](sql-database-elastic-database-client-library.md) |
|| [Spajanje i Podjela elastic baze podataka](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Sažetak

Preduvjeti odvajanja klijenta važna su za većinu SaaS složene aplikacija. Najbolja mogućnost za davanje odvajanja intenzivnog leans prema pristup bazi podataka po klijentu. U dva pristupa zahtijevaju ulaganja u slojevima složene aplikacije koje je potrebno osoblje stručne razvoj omogućuju odvajanja što znatno povećava trošak i rizika. Ako odvajanja preduvjeti ne bi za ranije u razvoju servisa, retrofitting ih može biti počinjati još u prve dvije modelima. Glavni Nedostaci pridružene baze podataka po klijentu modela vezane uz troškovi resursa povećana oblaka zbog smanjene zajedničko korištenje i održavanje i upravljanje mnoge baze podataka. SaaS razvojnim inženjerima često imati poteškoća reprodukcijom prilikom postavljanja tih gubitke.

Iako gubitke možda glavne barriers s Većina oblaka baze podataka usluga, baze podataka SQL Azure uklanja barriers s elastic skup i mogućnosti elastic baze podataka. Razvojni inženjeri SaaS možete kombinirati osobina odvajanja modela baze podataka po klijentu i optimizirati zajedničko korištenje resursa i mogućnost upravljanja poboljšanja mnogo baza podataka pomoću elastic grupe i pridruženi Alati.

Davatelji složene aplikacije koje imaju bez preduvjeti odvajanja klijenta i tko paket klijenata u bazi podataka na visok gustoće ponekad podataka za zajedničko korištenje modelima rezultat dodatne učinkovitosti u zajedničko korištenje resursa, i smanjivanje ukupni trošak. Azure Alati baze podataka SQL elastic baze podataka, sharding biblioteke i sigurnosnim značajkama pomoći SaaS davatelji stvaranje i upravljanje složene aplikacijama.

## <a name="next-steps"></a>Daljnji koraci

[Početak rada s alatima elastic baze podataka](sql-database-elastic-scale-get-started.md) pomoću aplikacije za uzorak koji pokazuje klijentska biblioteka.

Stvaranje [elastic skup prilagođene nadzorne ploče za SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) s aplikacijom uzorka koji koristi elastic grupe za rješenje učinkovit, skalabilni bazu podataka.

Pomoću alata baze podataka SQL Azure Migracija [postojeće baze podataka za prikaz izgleda](sql-database-elastic-convert-to-use-elastic-tools.md).

Prikaz naše vodič za [Stvaranje elastic grupe aplikacija](sql-database-elastic-pool-create-portal.md).  

Saznajte kako [nadzor i upravljanje elastic grupe aplikacija](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Dodatni resursi

- [Što je Azure elastic skup?](sql-database-elastic-pool.md)
- [Promjena veličine odgovor s bazom podataka SQL Azure](sql-database-elastic-scale-introduction.md)
- [Složene aplikacije s Alati elastic baze podataka i sigurnost na razini retka](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Provjera autentičnosti u složene aplikacije pomoću servisa Azure Active Directory i povezivanje OpenID](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin ankete aplikacije](../guidance/guidance-multitenant-identity-tailspin.md)
- [Rješenje brzog pokretanja](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Pitanja i zahtjeve za značajke

Za pitanja, nam možete pronaći u na [forumu SQL baze podataka](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Dodavanje zahtjev za značajku u [SQL baze podataka forum za povratne informacije](https://feedback.azure.com/forums/217321-sql-database/).
