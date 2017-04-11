<properties
    pageTitle="Prostor za pohranu Azure Premium: Dizajniranje performanse | Microsoft Azure"
    description="Dizajnirajte visokih performansi aplikacije koje se koriste za pohranu Premium Azure. Premium prostora za pohranu nudi visokih performansi, najniža Latencija disk podrška za radnih opterećenja li/O-ćete morati usko izvodi virtualnim računalima sustava na Azure."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>Prostor za pohranu Azure Premium: Dizajna za visoke performanse

## <a name="overview"></a>Pregled  
Ovaj članak sadrži upute za stvaranje visoke performanse aplikacije koje se koriste za pohranu Premium Azure. Poslužite se uputama u ovom dokumentu u kombinaciji s performansama najbolje prakse dodijeljen tehnologije koristi aplikacija. Da biste ilustrirali smjernice upotrijebili smo SQL Server na ostalo Premium za pohranu kao primjer u ovom dokumentu.

Dok se ne možemo adresa performanse scenariji za sloj prostora za pohranu u ovom članku, morat ćete optimizirati aplikacijskom sloju. Na primjer, ako su domaćini farme sustava SharePoint na Azure Premium prostora za pohranu, SQL Server primjere iz ovog članka možete koristiti da biste optimizirali poslužitelj baze podataka. Uz to, optimiziranje web-poslužitelj i poslužitelj aplikacije da biste dobili Većina performanse farme sustava SharePoint.

Ovaj će vam članak pomoći odgovora praćenja najčešća pitanja o optimiziranje performanse aplikacije na Azure Premium prostor za pohranu

-   Kako se mjere performanse računala?  
-   Zašto su vam ne prikazuju očekivani visoke performanse?  
-   Koji faktori utjecati na performanse aplikacije na Premium prostora za pohranu?  
-   Kako sljedećih čimbenika utječe performanse aplikacije na Premium prostora za pohranu?  
-   Kako mogu optimizirate za IOPS, propusnost i Latencija?  

Ne možemo je navesti ove smjernice posebno za pohranu Premium jer radnih opterećenja na ostalo za pohranu Premium vrlo su performanse i mala slova. Primjereno smo su navedeni primjeri. Možete primijeniti i neke od ove smjernice aplikacijama sustavom IaaS VMs diskova standardne prostora za pohranu.

Prije početka, ako ste novi korisnik prostora za pohranu Premium, pročitajte u [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](storage-premium-storage.md) članak i okvir [Azure Premium prostora za pohranu skalabilnost i ciljeve](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Pokazatelji aplikacije  
Ćemo procijenite hoće li se aplikacija izvršava dobro ili ne koriste performanse pokazatelja sviđa, brzinu aplikaciju obrađuje zahtjev korisnika s kojom količinom podataka aplikacije obrađuje po zahtjev, zahtjeve koliko je programa obrada aplikacije u određeno vrijeme trajanja korisnik mora čekati da biste dobiti odgovor nakon slanja njihov zahtjev. Tehnički pojmovi za ove pokazatelji jesu, IOPS, propusnost ili propusnost i Latencija.

U ovom ćete odjeljku smo obrađuje uobičajenih pokazatelji u kontekstu Premium prostora za pohranu. U sljedećem odjeljku preduvjeti za aplikaciju prikupljanje ćete naučiti za mjerenje te pokazatelji aplikacije. U nastavku optimiziranje performanse aplikacije će Naučite čimbenika utječe ovih pokazatelja uspješnosti i preporuke za optimiziranje ih.

## <a name="iops"></a>IOPS  
IOPS je broj zahtjeva za aplikaciju šalje diskova za pohranu jednu sekundu. Ulazni i izlazni postupak mogu pročitati ili napisati uzastopnih ili izravnim. OLTP aplikacija kao što je web-mjesto internetska maloprodaja potrebno odmah obrade zahtjeva za mnoge Istodobni korisnika. Zahtjevi za korisnika su umetanje i ažuriranje transakcije intenzivno bazu podataka koju aplikaciju morate brzo obraditi. Stoga OLTP aplikacije potreban je vrlo visoka IOPS. Takve aplikacije rukovati milijune zahtjeva za male i izravnim IO. Ako imate takvo aplikaciju, morate dizajnirati infrastrukture aplikacije da biste optimizirali za IOPS. U nastavku, *Optimiziranje performanse računala*, ne možemo navode detaljno čimbenika koja morate imati na umu da biste dobili visoke IOPS.

Kada priložite na disku premium prostora za pohranu na visok skaliranje VM Azure dodjeljuje resurse za sigurno broj IOPS po specifikacija disk. Na primjer, na disku P30 dodjeljuje 5000 IOPS. Svaki visoke skaliranje veličina VM ima određene ograničenje IOPS nastavka možete rješenja. Ako, na primjer, standardni GS5 VM ima 80,000 IOPS ograničiti.

## <a name="throughput"></a>Propusnost  
Propusnost ili propusnost je količinu podataka aplikacije šalje diskova za pohranu u određenim intervalima. Ako aplikacija izvršava ulaza i izlaza operacija s velikim veličine jedinica IO, potrebna visoka propusnost. Aplikacije za skladištu podatke često problema pregleda intenzivno operacije koje pristup velike dijelove podataka istovremeno i često obavljanja skupnih operacija. Drugim riječima, takve aplikacije potreban je veću propusnost. Ako imate takav aplikaciju, morate dizajnirati njegov infrastrukture da biste optimizirali za propusnost. U sljedećem odjeljku ćemo razmotriti detaljno čimbenika morate ugađanje da biste to postigli.

Kada priložite na disku premium prostora za pohranu na visok skaliranje VM Azure dodjeljuje resurse propusnost po tom specifikacija disk. Na primjer, na disku P30 dodjeljuje 200 MB po drugom diskovnom propusnost. Svaki visoke skaliranje veličina VM ima kao određene propusnost ograničenja koja se nastavka možete rješenja. Na primjer, standardni GS5 VM ima najveći propusnost 2000 MB sekundi.

Postoji odnos između propusnost i IOPS kao što je prikazano u nastavku formulu.

![](media/storage-premium-storage-performance/image1.png)

Stoga je važno da biste odredili optimalnih propusnost i IOPS vrijednosti koje je potrebno aplikacije. Prilikom pokušaja optimiziranje jedan s drugim utječe i dobiti na. U nastavku, *Optimiziranje performanse računala*, ne možemo obrađuje u više pojedinosti o optimiziranje IOPS i propusnost.

## <a name="latency"></a>Latencija  
Kašnjenje je vrijeme potrebno aplikaciju primiti zahtjev za jednu, pošaljite ga diskova za pohranu i slati odgovor s klijent. To je mjera ključnih performansi aplikacije osim IOPS i propusnost. Kašnjenje od premium prostora za pohranu disk je vrijeme potrebno za dohvaćanje informacija o zahtjeva i komunikaciju natrag u aplikaciji. Prostor za pohranu Premium omogućuje dosljedan niskoj latencies. Ako omogućite samo za čitanje host predmemoriranje diskova premium prostora za pohranu, možete dobiti koliko donjem čitanja Latencija. Ne možemo obrađuje Disk predmemoriranje detaljno u nastavku na *Optimiziranje performanse računala*.

Kada su Optimizacija aplikacije da biste dobili veće IOPS i propusnost, to će utjecati na Latencija aplikacije. Nakon ugađanje performansi za aplikaciju, uvijek rezultirati Latencija aplikacije da biste izbjegli neočekivane visokom latencijom ponašanje.

## <a name="gather-application-performance-requirements"></a>Prikupite preduvjeti performanse aplikacije  
Prvi korak pri dizajniranju visoke performanse aplikacije koje rade na pohranu Premium Azure je razumjeti preduvjeti performanse aplikacije. Kada prikupite preduvjeti performanse, možete optimizirati aplikacije da biste postigli najčešće optimalne performanse.

U prethodnom odjeljku smo objašnjenje uobičajenih pokazatelja uspješnosti, IOPS, propusnost i Latencija. Morate odredite koju od tih pokazatelji su ključnih u aplikaciji izlaganje željeni korisničkog sučelja. Ako, na primjer, Visoko IOPS najbitnije OLTP aplikacije obrade milijune transakcije u drugi. Dok je visoke propusnost za aplikacije Data Warehouse obrada velike količine podataka u drugi. Iznimno niske latencije je presudne za u stvarnom vremenu aplikacije kao što su videozapis uživo strujanje web-mjesta.

Nakon toga Izmjerite preduvjeti maksimalne performanse aplikacije njegov života. Kontrolni popis za uzorak ispod upotrijebili početka. Zapis maksimalne performanse preduvjeti tijekom normalno, vrh i off-hours radno opterećenje razdoblja. Tako da odredite preduvjeti za sve razine radnih opterećenja moći da biste odredili obavezne performanse cjelokupnog aplikacije. Na primjer, normalno radno opterećenje programa e-trgovine web-mjesta bit će transakcije služi tijekom Većina dana u godini. Radno opterećenje Vršna web-mjesta bit će transakcije služi tijekom praznika ili događaji posebno Prodaja. Radno opterećenje Vršna je obično iskusnih na ograničeno, ali možete zatražiti aplikacija za promjenu veličine dva ili više puta njegov normalnu aktivnost. Saznajte više 50 percentil, 90 percentil i 99 percentil preduvjeti. Pomaže filtriraju sve outliers u zahtjevima za performanse i pokušajima istakli optimizacija za desni vrijednosti.

**Kontrolni popis za aplikaciju performanse preduvjeti**

| **Preduvjeti za performanse** | **50 percentila** | **90 percentil** | **99 percentila** |
|---|---|---|---|
| Max. Transakcije u sekundi | | | |
| Operacija čitanja %            | | | |
| Operacije pisanja %           | | | |
| Slučajni operacije %          | | | |
| % Uzastopnih operacije      | | | |
| Veličina IO zahtjev              | | | |
| Prosječna propusnost           | | | |
| Max. Propusnost              | | | |
| Min. Latencija                 | | | |
| Prosječne latencije              | | | |
| Max. PROCESOR                     | | | |
| Prosječna CPU-a                  | | | |
| Max. Memorije                  | | | |
| Prosječna memorije               | | | |
| Dubina reda čekanja                  | | | |

>**Napomena o važnosti:**  
>Razmislite o skaliranje tih brojeva na temelju očekivani budućeg rasta aplikacije. Nije dobro plan za rast na vrijeme, jer je možda nije moguće je teže da biste promijenili infrastrukture za unaprjeđenje kasnije.

Ako imate postojeću aplikaciju, a želite premjestiti Premium prostora za pohranu, najprije Izrada kontrolnog popisa iznad za postojeće aplikacije. Nakon toga sastavljanje predložak aplikacije na Premium prostora za pohranu i dizajniranje aplikacije na temelju smjernice opisane u *Optimiziranje performanse aplikacije* u nastavku ovog dokumenta. U sljedećem odjeljku opisuju alate koje možete koristiti da biste prikupili performanse mjere.

Stvaranje slično postojeće aplikacije za u predložak kontrolnog popisa. Pomoću alata za Benchmarking možete kao zamjenu za radnih opterećenja i mjerenje performanse aplikacije predložak. U odjeljku [Benchmarking](#benchmarking) da biste saznali više. Način tako da možete odrediti hoće li Premium prostora za pohranu možete povezati ili surpass potrebama performanse aplikacije. Možete uvesti istu smjernice za svoju aplikaciju za proizvodnju.

### <a name="counters-to-measure-application-performance-requirements"></a>Mjerača preduvjeti za aplikaciju performanse izmjeriti  
Najbolji način za mjerenje preduvjeti performanse aplikacije, je pomoću alata za praćenje performansi nudi operacijski sustav poslužitelja. Možete koristiti programom PerfMon za Windows i iostat za Linux. Ti Alati snimite mjerača koje odgovaraju svakom mjere objašnjeni u odjeljku iznad. Vrijednosti te mjerača morate snimite kada program izvodi njegov normalno, vrh i off-hours radnih opterećenja.

Mjerača programom PerfMon dostupni su za procesora i memorije i, svaki logičkog diska i fizičke na disku poslužitelja. Kada koristite diskova za pohranu premium s na VM, mjerača fizički disk svaki od premium prostora za pohranu može te mjerača logičkog diska za svaku jedinicu stvoreno diskova za pohranu premium. Morate snimite vrijednosti za diskova koji hostira svoje radno opterećenje aplikacije. Ako postoji jedan prema jedan mapiranja između logičke i fizička disk, možete se referirati mjerača fizički disk; u suprotnom odnose mjerača logičkog diska. Na Linux, naredba iostat generira izvješće Upotreba procesora i disk. Upotreba izvješća na disku nudi Statistika po fizički uređaj ili particije. Ako poslužitelj baze podataka s podacima i zapisnika na zasebnom diskova, prikupite te podatke za oba diskova. Tablicu u nastavku opisuju mjerača diskova, procesora i memorije:

| Brojač | Opis | Programom PerfMon | Iostat |
|---|---|---|---|
| **IOPS ili transakcije u sekundi** | Broj zahtjeva/i izdavanja disk prostora za pohranu u sekundi. | Na disku čitanja/sec <br> Zapisivanje s diska | tps <br> r-s <br> w/s |
| **Na disku čitanja i pisanja** | % od čitanja i pisanja operacije obavljene na disku. | Vrijeme čitanja disk % <br> Vrijeme pisanja disk % | r-s <br> w/s |
| **Propusnost** | Količinu podataka čitanja ili zapisati na disku sekundi. | Disk Čitaj bajtova/sec <br> Na disku pisanje bajtova/sec | kB_read/s <br> kB_wrtn/s |
| **Latencija** | Ukupno vrijeme da biste dovršili zahtjeva za IO diska. | Prosječna Disk sec/pročitano <br> Prosječna disk sec/pisanje | await <br> svctm |
| **Veličina IO** | Veličina/i zahtjeve vezanih uz diskova prostora za pohranu. | Prosječna Disk bajtova/pročitano <br> Prosječna Disk bajtova/pisanje | avgrq sz |
| **Dubina reda čekanja** | Broj preostala/i zahtjeva za čekanje mogu čitati obrasca ili zapisati na disku za pohranu. | Trenutna Duljina reda čekanja na disku | avgqu sz |
| **Max. Memorije** | Memorija morati funkcionirati aplikacije | % Izvršenom bajtova koristi | Korištenje vmstat |
| **Max. PROCESOR** | Iznos procesora koji su potrebni za pokretanje aplikacije provođenju | Vrijeme procesor % | % util |

Dodatne informacije o [iostat](http://linuxcommand.org/man_pages/iostat1.html) i [programom PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).


## <a name="optimizing-application-performance"></a>Optimiziranje performanse aplikacije  
Glavni čimbenici koji utječu na performanse aplikacije koje se izvode na pohranu Premium su prirode od IO zahtjeve, veličina VM, veličina diska, broj diskova, predmemoriranja na disku, Multithreading i dubina reda čekanja. Neke od sljedećih čimbenika možete kontrolirati s knobs nudi sustav. Većina aplikacija nije vam dati mogućnost izravno promijeniti veličinu IO i dubina reda čekanja. Ako, na primjer, ako koristite SQL Server, ne možete odabrati IO dubine veličina i red. SQL Server odluči optimalnih IO veličina i reda čekanja dubine vrijednosti da biste dobili Većina performansi. Razumjeti utjecaju obje vrste čimbenika na performanse vašeg računala je tako da možete Dodjela odgovarajuće resurse potrebama performansi.

U ovom odjeljku odnose se na aplikaciju preduvjeti kontrolnog popisa koji ste stvorili, da biste odredili koliko morate optimiziranja performansi aplikacije. Ovisno o tome, moći da biste utvrdili koji faktori iz ovog odjeljka morat ćete ugađanje. Da biste witness Učinci svaki faktor na performanse računala, pokrenite benchmarking Alati na postavki aplikacije. Pogledajte odjeljak [Benchmarking](#Benchmarking) na kraju ovog članka korake da biste pokrenuli uobičajenih benchmarking alata za Windows i Linux VMs.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimiziranje IOPS, propusnost i Latencija na prvi pogled  
U tablici u nastavku navedene sve Čimbenici performansi te korake da biste optimizirali IOPS, propusnost i Latencija. U odjeljcima slijedite ovaj sažetak opisuju svaki faktor je mnogo detaljnije dubinu.

| | **IOPS** | **Propusnost** | **Latencija** |
|---|---|---|---|
| **Primjer scenarija** | Enterprise OLTP aplikacija koje je obavezna vrlo visoka transakcije po drugi stopa.                                                                                                                                 | Podataka tvrtke skladištenje aplikacije obrada velike količine podataka. | Pri u stvarnom vremenu aplikacije koje je obavezna izravne odgovore na zahtjeve za korisnika, kao što su online igraće. |
| Čimbenici performansi  | | | |
| **Veličina IO** | Manja veličina IO rezultira veći IOPS.                                                                                                                                                                           | Veća IO da biste daje veću propusnost. | |
| **Veličina VM** | Koristite veličina VM koja nudi IOPS veći od svojim potrebama aplikacije. Potražite u članku VM veličine i njihovih IOPS ograničenja ovdje. | Veličina VM pomoću propusnost ograničenje veće od svojim potrebama aplikacije. U odjeljku veličina VM i njihovih propusnost ograničenja ovdje. | Korištenje veličine VM ponuda promjena veličine ograničenja veći od svojim potrebama aplikacije. U odjeljku veličina VM i njihove ograničenja. |
| **Veličina diska** | Korištenje veličine disk koji nudi IOPS veći od svojim potrebama aplikacije. Potražite na disku veličine i njihove IOPS ograničenja ovdje. | Veličina diska pomoću propusnost ograničenje veće od svojim potrebama aplikacije. Potražite na disku veličine i njihove propusnost ograničenja ovdje. | Korištenje diska veličine ponuda promjena veličine ograničenja veći od svojim potrebama aplikacije. Potražite na disku veličine i njihovih ograničenja. |
| **VM i ograničenja skaliranje na disku** | IOPS ograničenje veličine VM odabrali mora biti veći od ukupno IOPS pokreću premium pridružen diskova za pohranu. | Propusnost ograničenje veličine VM odabrali mora biti veći od ukupne propusnost pokreću premium prostora za pohranu diskova povezan. | Promjena veličine ograničenja veličine VM odabrali mora biti veći od ograničenja ukupne mjerila diskova priloženu premium prostora za pohranu. |
| **Predmemoriranje na disku** | Omogućiti predmemorije samo za čitanje na diskova za pohranu premium s čitanje podebljanom operacije da biste dobili veći IOPS za čitanje. | | Na diskova za pohranu premium s spreman podebljanom operacije da biste dobili nisku čitanje latencies omogućite predmemorije samo za čitanje. |
| **Podjela na disku** | Koristite više diskova i stripe zajedno da biste dohvatili kombinirane veći IOPS i propusnost ograničenje. Imajte na umu kombinirane ograničenje po VM mora biti veća od kombinirane ograničenja od premium priložene diskova. | |
| **Veličina pruga** | Manja veličina pruga za slučajni small IO uzorak vidjeti u OLTP aplikacije. Za SQL Server OLTP aplikacije – primjerice, koristite pruga veličini 64KB. | Pruga većom za uzastopnih velike IO uzorak vidjeti u aplikacijama Data Warehouse. – Primjerice, koristite veličina pruge 256 iz baze znanja za SQL Server Data warehouse aplikaciju. | |
| **Multithreading** | Korištenje multithreading automatske veći broj zahtjeva za pohranu Premium koji vodi do veće IOPS i propusnost. Ako, na primjer, na SQL Server postavljanje visoke vrijednosti MAXDOP želite dodijeliti više CPU-ovi sustava SQL Server. | |
| **Dubina reda čekanja** | Veća dubina reda čekanja rezultira veći IOPS. | Veća dubina reda čekanja daje veću propusnost. | Manje dubina reda čekanja rezultira donjem latencies. |

## <a name="nature-of-io-requests"></a>Prirode IO zahtjeva  
Zahtjev za IO je jedinica ulaza i izlaza postupka koji se izvoditi aplikacije. Označavanje prirode IO zahtjeve, slučajni ili uzastopnih, čitanje ili pisanje, mala ili veliko, će vam pomoći odrediti preduvjeti performanse aplikacije. Vrlo je važno da biste shvatili prirode IO zahtjeve, da biste desno odluke prilikom dizajniranja infrastruktura za aplikacije.

Veličina IO jedan je od važnijih čimbenike. Veličina IO je veličina zahtjeva za operaciju ulaza i izlaza generira aplikacije. Veličina IO ima značajan utjecaj na performanse osobito na IOPS i propusnosti koji se nalazi mogućnost da biste postigli aplikacije. Sljedeća formula prikazuje odnos između IOPS, veličina IO i propusnosti/propusnost.  
    ![](media/storage-premium-storage-performance/image1.png)

Neke aplikacije jednostavan način možete mijenjati im veličinu IO dok se neki programi ne. Na primjer, SQL Server određuje optimalnih veličinu IO sam i navedite korisnike s bilo kojeg knobs da biste ga promijenili. S druge strane, Oracle nudi parametar pod nazivom [DB\_BLOKIRATI\_VELIČINA](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) pomoću koje možete konfigurirati veličine zahtjev/i baze podataka.

Ako koristite aplikacije, koja ne dopušta da biste promijenili veličinu IO, koristiti smjernice u ovom članku radi optimiziranja performansi KPI koji je najrelevantnije u aplikaciji. Na primjer,

-   Zatvaranje OLTP generira milijune IO zahtjeva za male i izravnim. Učiniti ove vrste IO zahtjeve, morate dizajnirati infrastruktura za računala da biste dobili više IOPS.  
-   Podataka skladištenje aplikacije generira velikih i uzastopnih IO zahtjeva. Rukovanje ove vrste IO zahtjeve, morate dizajnirati infrastruktura za računala da biste dobili veću propusnost veze ili propusnost.

Ako koristite aplikacije, koja omogućuje vam da biste promijenili veličinu IO, koristite pravilo palca za veličinu IO uz druge smjernice za performanse

-   Manja veličina IO da biste dobili više IOPS. Na primjer, 8 KB programa OLTP.  
-   IO većom da biste dobili veću propusnost/propusnost. Na primjer, 1024 iz baze znanja za aplikaciju skladištu podataka.

Slijedi primjer na kako možete izračunati IOPS i propusnost/propusnosti za svoju aplikaciju. Razmislite o aplikacije pomoću P30 diska. Maksimalna propusnost/propusnost na disku P30 i IOPS možete postići je 5000 IOPS i 200 MB sekundi odnosno. Ako aplikacije potreban je maksimalna IOPS s diska P30 i koristite manja veličina IO kao 8 KB, rezultirajući propusnosti moći ćete dobiti je sada 40 MB sekundi. Međutim, ako aplikacije potreban je Maksimalna propusnost/propusnost s P30 diska, a koristite veći IO kao što je 1024 KB, rezultirajući IOPS neće biti manje, 200 IOPS. Stoga ugađanje veličine IO tako da ispunjava oba svoje aplikacije propusnost/propusnost i IOPS potrebama. Niže navedenoj tablici nalaze različitim veličinama IO i njihove odgovarajuće IOPS i propusnost za P30 disk.

| **Zahtjevi za aplikaciju** | **/ I veličine** | **IOPS** | **Propusnost/propusnosti** |
|-----------------------------|--------------|----------|--------------------------|
| Max IOPS                    | 8 KB         | 5000    | 40 MB u sekundi         |
| Max propusnost              | 1024 KB      | 200      | 200 MB u sekundi        |
| Max propusnost + visoke IOPS  | 64 KB        | 3,200    | 200 MB u sekundi        |
| Max IOPS + visoke propusnost  | 32 KB        | 5000    | 160 MB u sekundi        |

Da biste IOPS i propusnosti veća od maksimalne vrijednosti na disku jedan premium prostora za pohranu, koristite više premium diskova Prugasta zajedno. Na primjer, pruga dva P30 diskova da biste dobili u kombinirani IOPS od 10 000 IOPS ili kombinirani propusnost 400 MB sekundi. Kao što je opisano u sljedećem odjeljku, morate koristiti veličina VM koji podržava na kombinirane disk IOPS i propusnost.

>**Napomena:**  
>Kako povećavate IOPS ili propusnost drugi i povećava, provjerite ne kliknete propusnost ili ograničenja IOPS diska ili VM kada povećati bilo.

Da biste witness efekata IO veličine na performanse računala, možete pokrenuti benchmarking Alati VM i diskova. Stvaranje više test nizove i korištenje drugu veličinu IO za svaki Pokreni da biste vidjeli posljedice. Pogledajte odjeljak [Benchmarking](#Benchmarking) na kraju ovog članka dodatne informacije.

## <a name="high-scale-vm-sizes"></a>Veličina VM visoke skala  
Kada pokrenete dizajniranja aplikacije, prvi što učiniti još, odaberite VM za hostiranje vaše aplikacije. Prostor za pohranu Premium isporučuje se s visoke skaliranje VM veličine koje možete pokrenuti aplikacije potrebno veće računalnim power i visoke lokalni disk/i performanse. Ove VMs omogućavaju procesora koji se brže, veća omjer memorije core i na Solid-State pogon (SSD) na lokalnom disku. Primjeri visoke skaliranje VMs podrške za pohranu Premium su niz VMs DS, DSv2 i Oznaka.

Visoke VMs skaliranje su dostupne u različitim veličinama s različit broj procesora jezgri, memorije, OS i veličinu privremene diska. Svaki veličina VM ima najveći broj diskova podataka koje možete priložiti u VM. Stoga utjecat će na odabranom veličina VM koliko obrada memorije i kapacitet pohrane dostupna je za aplikacije. Utječe na računalnim i troškova prostora za pohranu. Na primjer, u nastavku su specifikacije najveću veličina VM u nizu DS, DSv2 niza i Oznaka niz:

| Veličina VM | Jezgri procesora | Memorije | Veličina VM disk | Max. diskova podataka | Veličina predmemorije | IOPS | Ograničenja propusnosti IO predmemorije |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GB | OS = 1023 GB <br> Lokalni SSD = 224 GB | 32 | 576 GB | 50.000 IOPS <br> 512 MB u sekundi | 4000 IOPS i 33 MB u sekundi |
| Standard_GS5 | 32 | 448 GB | OS = 1023 GB <br> Lokalni SSD = 896 GB | 64 | 4224 GB | 80,000 IOPS <br> 2000 MB u sekundi | 5000 IOPS i 50 MB u sekundi |

Da biste pogledali popis svih dostupnih veličina Azure VM, priručniku [Windows VM veličine](../virtual-machines/virtual-machines-windows-sizes.md) ili [veličine Linux VM](../virtual-machines/virtual-machines-linux-sizes.md). Odaberite veličinu VM koji mogu zadovoljavaju i Skaliranje s potrebama performanse željenu aplikaciju. Osim toga, stupiti u obzir pratiti važne stavke prilikom odabira VM veličine.


*Promjena veličine ograničenja*  
Maksimalna ograničenja IOPS po VM i po disk su različite i međusobno nezavisni. Provjerite je li aplikacija je vožnju IOPS unutar ograničenja na VM, kao i diskova premium pridružen. U suprotnom performanse aplikacija će se dogoditi Reguliranje.

Na primjer, pretpostavimo da se aplikacija potrebna je najviše 4000 IOPS. Da biste to postigli, dodijelite na disk P30 DS1 VM. Na disku P30 možete održati do 5000 IOPS. Međutim, DS1 VM ograničeno je na 3,200 IOPS. Zbog toga performanse aplikacije će biti ograničeno po ograničenje VM pri 3,200 IOPS, a bit će su smanjene performanse. Da biste spriječili u ovom slučaju, odaberite veličina VM i disk koji će i zadovoljava preduvjete za aplikacije.

*Trošak operacije*  
U mnogim slučajevima, moguće je da je vaš ukupni trošak operacije pomoću prostora za pohranu Premium manja od pomoću standardnih prostora za pohranu.

Na primjer, razmotrite aplikacije koje je obavezna 16,000 IOPS. Da biste postigli ovaj performanse, potrebno je Standard\_D14 Azure IaaS VM, koji može dati Maksimalna IOPS od 16,000 pomoću 32 diskova 1TB standardne prostora za pohranu. Svakom disku standardne prostora za pohranu 1TB možete postići najviše 500 IOPS. Procijenjena trošak ovaj VM mjesečno bit će $1,570. Mjesečni trošak 32 standardne prostora za pohranu diskova bit će $1,638. Procijenjena ukupni trošak mjesečni bit će $3,208.

Međutim, ako se hostira iste aplikacije na Premium prostora za pohranu, morate manja veličina VM i manje diskova premium prostora za pohranu, tako smanjuju ukupni trošak. Standardno\_DS13 VM možete upoznati obavezne IOPS 16,000 pomoću četiri P30 diskova. DS13 VM ima najveći IOPS od 25,600 i svaki P30 disk ima najveći IOPS od 5000. Ukupni, tu konfiguraciju možete postići 5000 x 4 = 20 000 IOPS. Procijenjena trošak ovaj VM mjesečno bit će $1,003. Mjesečni trošak od četiri P30 diskova za pohranu premium bit će $544.34. Procijenjena ukupni trošak mjesečni bit će $1,544.

Niže navedenoj tablici nalaze trošak razrada svega scenarij za standardnu i Premium prostora za pohranu.

| | **Standardna** | **Premium** |
|---|---|---|
| **Trošak VM mjesečno** | $1,570.58 (standardni\_D14)   | $1,003.66 (standardni\_DS13) |
| **Trošak diskova mjesečno** | $1,638.40 (32 x 1 TB diskova) | $544.34 (4 x P30 diskova) |
| **Ukupni trošak po mjesecu**  | $3,208.98 | $1,544.34 |

*Linux Distros*  

Pomoću Azure Premium prostor za pohranu, dobit jednaku razinu izvedbe VMs izvodi Windows i Linux. Podržavamo mnogo flavors Linux distros i vidjet ćete popis [ovdje](../virtual-machines/virtual-machines-linux-endorsed-distros.md). Važno Imajte na umu da drugi distros prikladnije su za različite vrste radnih opterećenja je. Vidjet ćete različite razine performanse ovisno o distro svoje radno opterećenje radi. Testiranje distros Linux s aplikacijom, a zatim odaberite onaj koji najbolje funkcionira.


Kada se pokrene Linux s pohranom Premium, provjerite najnovija ažuriranja o potrebne upravljačke programe da biste bili sigurni visoke performanse.

## <a name="premium-storage-disk-sizes"></a>Premium prostora za pohranu na disku veličine  
Azure Premium prostora za pohranu trenutno nudi tri veličine disk. Svaki veličina diska ima ograničenje različite mjerilo za IOPS, propusnost i prostora za pohranu. Odaberite s desne strane veličina Premium prostora za pohranu diska ovisno o preduvjeti za aplikaciju i visoke skale veličina VM. U tablici u nastavku prikazuje tri diskova veličine i svoje mogućnosti.

| **Vrsta diska**       | **P10**           | **P20**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Veličina diska           | 128 giB           | 512 giB           | 1024 giB (1 TB)   |
| IOPS po na disku       | 500               | 2300              | 5000              |
| Propusnost po na disku | 100 MB u sekundi | 150 MB u sekundi | 200 MB u sekundi |

Koliko mediji ovisi o disk veličinu odabrali. Možete koristiti jedan P30 disk ili više diskova P10 da bi odgovarao svojim potrebama aplikacije. Stupiti u račun pitanja vezana uz navedene pri stvaranju izbora.

*Promjena veličine ograničenja (IOPS i propusnost)*  
Ograničenja IOPS i propusnost svaki veličina diska Premium razlikuju se i neovisno od ograničenja skaliranje VM. Provjerite jesu li da Ukupno IOPS i propusnost iz na diskova unutar ograničenja skaliranje odabranom VM veličine.

Na primjer, ako se aplikacija potrebna je maksimalno 250 MB/sec propusnost i koristite DS4 VM sa jedan P30 disk. DS4 VM ponudit će do 256 MB/sec propusnost. Međutim, jedan P30 disk je propusnost ograničenje od 200 MB/sec. Zbog toga aplikacija će ograničeno na 200 MB/sec zbog ograničenja disk. Kako bi se prevladala to ograničenje, dodijelite više diskova podataka da biste na VM.

>**Napomena:**  
>Čitanje posluživanje sustavom predmemoriju nisu obuhvaćeni disk IOPS i propusnost, dakle ne predmeta ograničenja disk. Predmemorija ima zasebnu IOPS i propusnost ograničenjem po VM.
>
>Na primjer, prethodno čitanja i pisanja su 60MB/sec i 40MB/sec odnosno. S vremenom predmemorije warms prema gore i služi itd više na čitanja iz predmemorije. Nakon toga možete dobiti veću propusnost za pisanje s diska.

*Broj diskova*  
Utvrdite broj diskova ćete tako da assessing preduvjeti za aplikaciju. Svaki veličina VM ima ograničenje broja diskova koje možete priložiti u VM. Obično je dvaput broj jezgri. Provjerite je li veličina VM odaberete mogu podržavati broj diskova potrebno.

Imajte na umu diskova Premium prostora za pohranu imati veći performanse mogućnosti u usporedbi s diskova standardne prostora za pohranu. Stoga ako premještate aplikacije iz Azure IaaS VM pomoću standardnih prostora za pohranu za pohranu Premium, vjerojatno ćete manje premium diskova da biste postigli iste ili noviji performanse aplikacije.

## <a name="disk-caching"></a>Predmemoriranje na disku  
Visoke VMs mjerilo pod utjecajem Azure Premium pohranu imati više razina predmemoriranja tehnologiju koja se naziva BlobCache. BlobCache koristi kombinaciju virtualnog računala RAM-a i lokalne SSD za predmemoriranje. U ovom predmemorije dostupna je za diskova stalnih prostor za pohranu Premium i diskova lokalne VM. Prema zadanim postavkama, tu postavku predmemorije postavljen je za čitanje/pisanje za OS diskova i samo za čitanje za podataka diskova hostirane na Premium prostora za pohranu. S diska predmemoriranje omogućena na diskova Premium prostora za pohranu, Visoko skale VMs možete postići iznimno visoke razine performansi koje premašuju podlozi performanse diska.

>[AZURE.WARNING] Promjena postavki predmemorije Azure diska odvaja i ponovno pridružuje Ciljni disk. Ako je operacijski sustav disk, na VM ponovnog pokretanja. Zaustavite sve aplikacije i servise koji može utjecati ovaj prekidu prije promjene postavki predmemorije diska.

Da biste saznali više o funkcioniranje BlobCache, odnosi s unutrašnje [Azure Premium prostora za pohranu](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) na blogu.

Važno je da Omogući predmemoriju na desni skup diskova. Tome trebate omogućiti disk predmemoriranje na disku premium ili ne ovise o uzorak radno opterećenje koji će biti rukovanje. Dolje navedenoj tablici prikazane zadane postavke predmemorije za OS i podataka diskova.

| **Vrsta diska** | **Zadane postavke predmemorije** |
|---|---|
| OS disk | ReadWrite |
| Podatkovni disk | Ništa |

Slijede preporučene disk postavki predmemorije za diskova podataka

| **Na disku predmemoriranje postavka** | **Preporuka na kada se koriste tu postavku** |
|---|---|
| Ništa | Konfiguriranje glavnog računala predmemorije kao nema za samo za unos i pisanje Tamni diskova. |
| Samo za čitanje | Konfiguriranje glavnog računala predmemorije kao samo za čitanje za samo za čitanje i čitanja i pisanja disk. |
| ReadWrite | Konfiguriranje glavnog računala predmemorije kao ReadWrite samo ako aplikacija ispravno rukuje stalni diskova po potrebi pisanja predmemoriranih podataka. |

*Samo za čitanje*  
Konfiguriranjem predmemoriranje Premium prostora za pohranu podataka diskova samo za čitanje možete postigli niske latencije čitanje i zatražite vrlo visoka IOPS za čitanje i propusnost za svoju aplikaciju. Ovo je zbog dva razloga

1.  Čitanje izvesti iz predmemorije, koja se nalazi na VM memorije i lokalne SSD, mnogo brže od čitanja s diska podataka koja se nalazi na spremište blobova platforme Azure.  
2.  Prostor za pohranu Premium Brojanje čitanja poslužena iz predmemorije, pri disk IOPS propusnost. Stoga aplikacija je da biste postigli veći ukupni IOPS i propusnost.

*ReadWrite*  
Prema zadanom diskova OS imaju ReadWrite Predmemoriranje omogućeno. Ste nedavno dodali smo podrška za ReadWrite predmemoriranja podataka kao i diskova. Ako koristite ReadWrite predmemoriranja, morate imati odgovarajuću način za zapisivanje podataka iz predmemorije stalni diskova. Na primjer, SQL Server rukuje pisanja predmemoriranim podacima diskova stalnih prostor za pohranu na vlastitu. Predmemoriju ReadWrite pomoću aplikacije koja će se rukovati persisting potrebnih podataka može dovesti do gubitka podataka ubuduće, ako se ruši u VM.

Na primjer, možete primijeniti ovim smjernicama SQL Server na ostalo Premium za pohranu na sljedeće načine

1.  Konfiguriranje predmemorije "Samo za čitanje" na premium prostora za pohranu diskova hosting podatkovne datoteke.  
    na.  Brzo čita iz predmemorije donjem vrijeme upita SQL Server jer stranicama podataka mnogo brže dohvaćeni iz predmemorije u usporedbi s izravno iz podataka diskova.  
    b.  Posluživanje čitanja iz predmemorije, znači da je dodatne propusnost dostupna od premium podataka diskova. SQL Server možete koristiti ovaj dodatne propusnost pri dohvaćanju podataka stranica i ostalih operacija kao što su sigurnosnog kopiranja i vraćanja, skupna opterećenje i ponovno indeksiranje.  
2.  Konfiguriranje "Nema" predmemoriju na premium prostora za pohranu diskova nalaze u datoteke zapisnika.  
    na.  Datoteke zapisnika imati prvenstveno pisanje Tamni operacija. Zbog toga ih nije prednosti iz predmemorije samo za čitanje.

## <a name="disk-striping"></a>Podjela na disku  
Kada visoke skalu VM povezan s nekoliko premium prostora za pohranu stalni diskova, na diskova možete biti zajedno Prugasta za zbrajanje njihove IOPs, propusnost i kapacitet pohrane.

U sustavu Windows, možete koristiti za pohranu razmake da biste je pruge diskova zajedno. Morate konfigurirati jedan stupac za svaki disk u zajedničko područje. U suprotnom cjelokupnoj izvedbi prugastim jedinica mogu biti niže nego je očekivano zbog nejednaki raspodjele prometa na na diskova.

Važno: Pomoću korisničkog Sučelja Upravitelj poslužitelja, možete postaviti ukupan broj stupaca najviše 8 za prugastim jedinicu. Prilikom pridruživanja više od 8 diskova pomoću komponente PowerShell da biste stvorili glasnoću. Pomoću komponente PowerShell postavite željeni broj stupaca jednak broju diskova. Na primjer, ako postoje 16 diskova u skupu jedan pruga Odredite 16 stupce u parametar *NumberOfColumns* PowerShell cmdleta *New-VirtualDisk* .


Na Linux, koristiti uslužni MDADM da biste je pruge diskova zajedno. Detaljne upute za Podjela diskova na Linux odnose [Konfiguriranje RAID softver na Linux](../virtual-machines/virtual-machines-linux-configure-raid.md).


*Veličina pruga*  
Važno konfiguraciju u Podjela disk je pruge veličina. Veličina pruge ili veličina bloka je najmanja blok podataka koji aplikaciju možete pozabaviti prugastim jedinice. Veličina pruge konfigurirate ovisi o vrsti aplikacije i uzorak zahtjev. Ako odaberete veličina pogrešan pruge, on može dovesti do IO nepravilno poravnanje, koji vodi na su smanjene performanse aplikacije.

Na primjer, ako je zahtjev za IO generira aplikacije veća od pruga veličina diska, sustav za pohranu piše ga preko granica jedinica pruga na više od jednog diska. Kad dođe vrijeme za pristup tim podacima, potrebno je traženje preko više pruga jedinice da biste dovršili zahtjev. Kumulativni učinak takvo ponašanje može dovesti do smanjene performanse znatno performansi. S druge strane, ako je zahtjev za veličina IO manja veličina pruge, a ako je slučajni u prirode, zahtjeva za IO možda zbrojite vremenske vrijednosti na istom disku uzrokuje usko grlo i konačni Smanji IO performansi.


Ovisno o vrsti radno opterećenje program izvodi, odabir veličine odgovarajuće pruga. Slučajni small IO zahtjeve, koristite manja veličina pruga. Dok velike uzastopnih IO zahtjeve korištenja veći pruga. Saznajte više pruga veličina preporuke za aplikacije koje će se izvoditi na Premium prostora za pohranu. Konfigurirati pruga veličini 64KB za OLTP radnih opterećenja i 256KB za skladištenje radnih opterećenja podataka za SQL Server. Potražite u članku [performanse najbolje prakse za SQL Server na Azure VMs](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) da biste saznali više.


>**Napomena:**  
>Koje možete stripe zajedno maksimalno 32 diskova premium prostora za pohranu na niz DS VM i 64 diskova premium prostora za pohranu na skupu Oznaka VM.

## <a name="multi-threading"></a>Usporedno izvršavanje zadataka  
Azure vam omogućuje pohranu Premium platforme biti massively paralelno. Stoga Usporedni aplikacije postiže mnogo veći performanse od u jednom niti aplikaciju. Usporedni aplikacije Razdijeli njegov zadaci prilikom preko više niti i korištenja resursa VM i diska maksimalnoj povećava učinkovitost njegova izvršavanja.

Ako, na primjer, ako je na jednom core VM pomoću dva niti pokrenut aplikacije, Procesor možete se prebacivati između dva niti da biste postigli učinkovitosti. Dok niti čeka se na disku IO da biste dovršili, Procesor možete prijeći na drugi niti. Na taj način dva niti koje možete obaviti na više od jedne niti će. Ako na VM ima više od jedne core, ga dodatno smanjuje razdoblje jer se svaki core možete izvršavanje zadataka paralelno.

Nećete moći promijeniti način na koji implementira off-the-shelf aplikacije jedan usporedno izvršavanje zadataka ili usporedno izvršavanje zadataka. Na primjer, SQL Server se može rad s više procesora i više core. Međutim, SQL Server odlučuje koje uvjetima će korištenje niti jedan ili više za obradu upita. Može izvoditi upite i sastavljanje indeksi pomoću usporedno izvršavanje zadataka. Za upit koji obuhvaća uključivanja velike tablice i sortiranje podataka prije vraćanja korisnika, SQL Server vjerojatno će koristiti više niti. Međutim, korisnik ne može kontrolirati izvršava li SQL Server pomoću niti jedan ili više niti upita.

Postoje konfiguracijske postavke koje možete promijeniti da biste to utječe usporedno izvršavanje zadataka ili paralelni obrade aplikacije. Ako, na primjer, u slučaju SQL Server je maksimalna stupanj Parallelism konfiguracije. Ta postavka naziva MAXDOP, omogućuje vam da biste konfigurirali maksimalni broj procesora koji se SQL Server možete koristiti kad je paralelno obrada. MAXDOP možete konfigurirati za pojedinačne upita ili operacije indeksa. To je korisno kada želite saldo resursi sustava za aplikaciju ključnih performansi.

Na primjer, pretpostavimo aplikacije pomoću SQL Server izvršava velike upita i operaciju indeksa u isto vrijeme. Javite nam pretpostavlja da ste željeli da bi se više performant u usporedbi s velikim upita operacija indeksa. U tom slučaju možete postaviti vrijednost MAXDOP operacije indeks biti veća od vrijednosti MAXDOP za upit. Na taj način, SQL Server s većim brojem procesora koji ga možete koristiti za operaciju indeksa u usporedbi s broja procesora koji se mogu odvojiti velike upit. Imajte na umu omogućuje upravljanje broj niti SQL Server će se koristiti za svaku operaciju. Možete kontrolirati maksimalni broj procesora koji se namjenski za usporedno izvršavanje zadataka.

Saznajte više o [Stupnjeva Parallelism](https://technet.microsoft.com/library/ms188611.aspx) u sustavu SQL Server. Saznajte više takvih postavke koje utječe usporedno izvršavanje zadataka u aplikaciji i njihova konfiguracija radi optimiziranja performansi.

## <a name="queue-depth"></a>Dubina reda čekanja  
Dubina reda čekanja ili Duljina reda čekanja ili red veličina je broj na čekanju IO zahtjeve u sustavu. Vrijednost dubina reda čekanja određuje koliko IO operacije aplikacija možete poravnati, koji diskova za pohranu će obrađivati. Utječe na sve na tri aplikacije pokazatelji koje ćemo se spominju u ovom viz. članka, IOPS, propusnost i Latencija.

Red čekanja dubine i usporedno izvršavanje zadataka su blisko povezana. Vrijednost dubina reda čekanja upućuje na to koliko usporedno izvršavanje zadataka može se postići aplikacija. Ako je prevelik dubina reda čekanja aplikacije mogu izvršiti dodatne operacije istovremeno, drugim riječima, više usporedno izvršavanje zadataka. Ako je dubina reda čekanja mali, čak i ako je aplikacija Usporedni, neće imati dovoljno zahtjeve poravnani za istodobni izvršavanje.

Obično isključivanje police aplikacija ne dopušta mijenjanje dubina reda čekanja jer ako Postavi neispravno neće imati više nanošenje od dobro. Aplikacija će postaviti desne vrijednosti dubina reda čekanja da biste postigli optimalne performanse. Međutim, važno je da biste shvatili tog koncepta tako da se s aplikacijom otklonite probleme s performansama. Efekti dubina reda čekanja mogu Primijetit i tako da pokrenete benchmarking Alati u sustavu.

Neke aplikacije Navedite postavke da biste utjecati dubina reda čekanja. Na primjer, MAXDOP (maksimalno stupanj parallelism) postavku u sustavu SQL Server objašnjeno u prethodnom odjeljku. MAXDOP je način utjecati dubina reda čekanja i usporedno izvršavanje zadataka, iako izravno ne mijenja vrijednost dubina reda čekanja sustava SQL Server.

*Dubina visoke reda čekanja*  
Visoke red dubine recima gore dodatne operacije na disku. Na disku zna sljedeći zahtjev u njegovom redu čekanja na vrijeme. Zbog toga diska možete Zakazivanje operacije na vrijeme i obrada ih optimalnih redoslijedom. Budući da se aplikacija šalje zahtjevi na disk, disk možete procesa više paralelnih IOs. Konačni aplikacija moći da biste postigli veću IOPS. Budući da se aplikacija je obrade zahtjeva za dodatne, ukupnu propusnost aplikacije i povećava.

Obično aplikaciju možete postići Maksimalna propusnost sa 8-16 + preostala IOs po priloženu disk. Ako je dubina reda čekanja jedan, aplikacija je margina dovoljno IOs sa sustavom, a će obraditi manje količinu u određenom razdoblju. Drugim riječima, manje propusnost.

Na primjer, u sustavu SQL Server postavljanje MAXDOP vrijednosti za upit "4" obavještava SQL Server da ga možete koristiti do četiri jezgri izvršavanje upita. SQL Server se određuje koja je najbolja vrijednost dubine reda čekanja i broj jezgri za izvršavanje upita.

*Dubina optimalnih reda čekanja*  
Vrlo visoka reda čekanja dubine vrijednost sadrži i njegov Nedostaci. Ako je vrijednost dubine reda čekanja prevelika, aplikacija će pokušati pogona vrlo visoka IOPS. Osim ako se aplikacija ima stalni diskova s dovoljno dodijeljenu IOPS, to mogu negativno utjecati na latencies aplikacije. Praćenje formula prikazuje odnos između IOPS, Latencija i dubina reda čekanja.  
    ![](media/storage-premium-storage-performance/image6.png)

Ne trebali biste konfigurirati dubina reda čekanja sve visoke vrijednosti, ali optimalne vrijednosti koje je moguće isporučiti dovoljno IOPS za aplikaciju bez utjecaja na latencies. Na primjer, ako Latencija aplikacije mora biti 1 od milisekundi, dubina reda čekanja potrebne da biste postigli 5000 IOPS je, QD = 5000 x 0,001 = 5.

*Dubina reda čekanja za Prugastim glasnoće*  
Za prugastim jedinicu imaju dovoljno visoku red dubine tako da svaki disk ima dubina reda čekanja za Vršna pojedinačno. Na primjer, razmotrite aplikacije koja ih gura dubina reda čekanja broja 2, a postoje 4 diskova na traci. Dva zahtjeva IO idu u dva diska, a preostale dvije diskova će biti Neaktivno. Stoga konfigurirati dubina reda čekanja tako da sva diskova može biti zauzet. Formula ispod pokazuje kako odrediti dubina reda čekanja prugastim jedinica.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Ograničavanje  
Azure dodjeljuje resurse za pohranu Premium navedeni broj IOPS i propusnost ovisno o VM veličine i na disku veličine odaberete. Svaki put kad se aplikacija pokušava pogon IOPS ili propusnost iznad te ograničenja što VM ili disk možete rukovati, Premium prostora za pohranu će je throttle. Manifesti se u obliku su smanjene performanse u aplikaciji. To možete znače veću Latencija, smanjite propusnost ili spustiti IOPS. Ako Premium prostora za pohranu throttle, aplikacija nije potpuno uspjeti po premašuju koji su svojih resursa instaliranog postizanje. Tako, da biste izbjegli probleme s performansama zbog reguliranje, uvijek Dodjela dovoljno resursa za svoju aplikaciju. Uzeti u obzir što smo spominju u VM veličine i na disku veličine odlomcima. Benchmarking je najbolji način za utvrđivanje resursima morat ćete hostira vaša aplikacija.

## <a name="benchmarking"></a>Benchmarking  
Benchmarking je postupak simulaciju različitih radnih opterećenja na aplikaciju i performanse aplikacije za svaku radno opterećenje za mjerenje. Korištenje korake opisane u prethodnom odjeljku ste prikupili preduvjeti performanse računala. Tako da pokrenete benchmarking Alati na VMs nalaze aplikacije, možete odrediti željene razine performanse aplikacije možete postići pomoću Premium prostora za pohranu. U ovom ćete odjeljku dobivate Primjeri benchmarking standardni DS14 VM nudi diskova Azure Premium prostora za pohranu.

Ne možemo koriste Iometer i FIO, uobičajenih benchmarking alata za Windows i Linux odnosno. Ti Alati rezultiraju više niti simulaciju radnog kao što su radno opterećenje i mjerenje performanse sustava. Pomoću alata za možete konfigurirati parametara, kao što je blok veličina i red dubinu, koji se obično ne možete promijeniti za aplikaciju. To vam daje veću fleksibilnost na pogon maksimalne performanse na visok skali VM tamo uz premium diskova za različite vrste radnih opterećenja aplikacije. Da biste saznali više o svakom alatu benchmarking posjetite [Iometer](http://www.iometer.org/) i [FIO](http://freecode.com/projects/fio).

Da biste pratili primjere u nastavku, stvorite standardni VM DS14 i priložite 11 diskova Premium prostora za pohranu na VM. 11 diskova konfiguriranje 10 diskova pomoću glavnog računala predmemoriranje kao "Nema" i stripe ih u glasnoću pod nazivom NoCacheWrites. Konfiguriranje glavnog računala predmemoriranje kao "Samo za čitanje" na disku preostalih i stvorite jedinicu nazivom CacheReads s diska. Koristite ovaj će instalacijski program, moći da biste vidjeli maksimalne performanse čitanje i pisanje iz standardnog VM DS14. Detaljne upute o stvaranju DS14 VM s premium diskova idite na [Stvaranje i korištenje prostora za pohranu Premium račun za virtualnog računala podatkovni disk](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk).

*Zagrijava predmemoriju*  
Na disku pomoću glavnog računala samo za čitanje predmemoriranje moći da bi se dobilo IOPS veća od ograničenja disk. Da biste dobili maksimalne performanse čitanja iz predmemorije glavno računalo, najprije vam mora Toplo gore predmemoriju diska. Time se osigurava da IOs čitanje koji benchmarking alat će pogon na CacheReads glasnoću zapravo dodirne predmemorije, a ne na disku izravno. Rezultat pristupa predmemorije dodatne IOPS iz jedne predmemorije omogućena na disku.

>**Važno:**  
>Morate Toplo gore predmemoriju prije pokretanja benchmarking, svaki put kada je VM sustava.

#### <a name="iometer"></a>Iometer   
[Preuzmite alat Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) na VM.

*Datoteka za testiranje*  
Iometer koristi testiranje datoteke koja je spremljena na glasnoće na kojem će pokrenuti benchmarking test. Pogoni čitanja i pisanja na toj datoteci test za mjerenje disk IOPS i propusnost. Iometer stvara datoteka za testiranje ako niste naveli jedan. Stvaranje datoteke test 200GB naziva iobw.tst na jedinicama CacheReads i NoCacheWrites.

*Specifikacije programa Access*  
Specifikacije, zatražite IO veličina, % čitanje/pisanje, % slučajni/uzastopnih konfigurirane putem kartice "Pristup specifikacije" u Iometer. Stvaranje specifikacije programa access za svaku scenariji opisan u nastavku. Stvaranje specifikacije programa access i "Spremi" s odgovarajuću naziv – primjerice RandomWrites\_8K, RandomReads\_8K. Odaberite odgovarajući specifikacija kada se pokrene scenarij test.

Primjer specifikacije programa access za maksimalnu pisanje IOPS scenarij je prikazano u nastavku,  
    ![](media/storage-premium-storage-performance/image8.png)

*Specifikacije Maksimalna IOPS Test*  
Da bismo pokazali Maksimalna IOPs, koristite manja veličina zahtjev. Koristite 8K zahtjev veličina i stvorite postavke vezane uz slučajni piše i čitanja.

| Specifikacije programa Access | Veličina zahtjev | Slučajni % | Čitanje % |
|----------------------|--------------|----------|--------|
| RandomWrites\_8K     | 8K           | 100      | 0      |
| RandomReads\_8K      | 8K           | 100      | 100    |

*Maksimalna propusnost Test specifikacije*  
Da bismo pokazali Maksimalna propusnost, koristite veći zahtjev. Koristite 64K zahtjev veličina i stvorite postavke vezane uz slučajni piše i čitanja.

| Specifikacije programa Access | Veličina zahtjev | Slučajni % | Čitanje % |
|----------------------|--------------|----------|--------|
| RandomWrites\_64K    | 64 K.          | 100      | 0      |
| RandomReads\_64K     | 64 K.          | 100      | 100    |

*Pokretanje Iometer Test*  
Izvođenje korake u nastavku da biste Toplo gore predmemorije

1.  Stvaranje dva specifikacije programa access s vrijednostima prikazano u nastavku,

  	| Ime              | Veličina zahtjev | Slučajni % | Čitanje % |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1MB | 1MB          | 100      | 0      |
  	| RandomReads\_1MB  | 1MB          | 100      | 100    |

2.  Pokrenite test Iometer za pokretanje predmemorije disk sa sljedećih parametara. Korištenje tri radnih niti Ciljna jedinica i dubina reda čekanja od 128. Postavite trajanje "Vrijeme izvođenja" test na 2hrs na kartici "Postavljanje testiranje".

  	| Scenarij              | Ciljna jedinica | Ime              | Trajanje |
  	|-----------------------|---------------|-------------------|----------|
  	| Pokretanje predmemorije diska | CacheReads    | RandomWrites\_1MB | 2hrs     |

3.  Pokrenite test Iometer zagrijava predmemorije disk sa sljedećih parametara. Korištenje tri radnih niti Ciljna jedinica i dubina reda čekanja od 128. Postavite trajanje "Vrijeme izvođenja" test na 2hrs na kartici "Postavljanje testiranje".

  	| Scenarij           | Ciljna jedinica | Ime             | Trajanje |
  	|--------------------|---------------|------------------|----------|
  	| Toplo gore predmemorije diska | CacheReads    | RandomReads\_1MB | 2hrs     |

Nakon predmemorije disk je warmed prema gore, nastavite s scenariji test navedena u nastavku. Da biste pokrenuli Iometer test, koristite najmanje tri radnih niti za **svaki** Ciljna jedinica. Za svaki radnoj niti odaberite Ciljna jedinica, postavljanje dubina reda čekanja, a zatim odaberite jednu od specifikacije testiranja spremljenog kao što je prikazano u tablici u nastavku, da biste pokrenuli odgovarajući scenarij test. U tablici i prikazuje očekivane rezultate za IOPS i propusnost prilikom izvođenja te testiranja. Za sve scenarije malu veličinu IO 8 KB i na visok dubina reda čekanja od 128 koristi.

| Scenarij test      | Ciljna jedinica | Ime              | Rezultat       |
|--------------------|---------------|-------------------|--------------|
| Max. Čitanje IOPS     | CacheReads    | RandomWrites\_8K  | 50.000 IOPS  |
| Max. Pisanje IOPS    | NoCacheWrites | RandomReads\_8K   | 64 000 IOPS  |
| Max. Kombinirani IOPS | CacheReads    | RandomWrites\_8K  | 100 000 IOPS |
|                    | NoCacheWrites | RandomReads\_8K   |              |
| Max. Čitanje MB/sec   | CacheReads    | RandomWrites\_64K | 524 MB/sec   |
| Max. Pisanje MB/sec  | NoCacheWrites | RandomReads\_64K  | 524 MB/sec   |
| Kombinirani MB/sec    | CacheReads    | RandomWrites\_64K | 1000 MB/sec  |
|                    | NoCacheWrites | RandomReads\_64K  |              |

U nastavku su snimke zaslona na Iometer testiranje rezultate za kombinirane scenariji za IOPS i propusnost.

*Kombinirani čitanja i pisanja Maksimalna IOPS*  
![](media/storage-premium-storage-performance/image9.png)

*Kombinirani čitanja i pisanja Maksimalna propusnost*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO je alat za popularne usporednih za pohranu na Linux VMs. Fleksibilnost da biste odabrali drugu IO veličine, uzastopnih ili izravnim čitanja i pisanja. Ga spawns radnih niti ili postupke za izvođenje navedeni/i operacija. Možete odrediti vrstu operacije/i svaki radnoj niti morate provesti pomoću datoteke posao. Koju smo stvorili jedne datoteke posla po scenarij podržali primjere u nastavku. Možete promijeniti specifikacije u datotekama te posao usporednih različitih radnih opterećenja sustavom Premium prostora za pohranu. U primjerima smo koriste u standardni DS 14 VM pokrenut **Ubuntu**. Korištenje jednako postavljanje što je opisano u početku [Benchmarking sekcije](#Benchmarking) i Toplo predmemoriju prije pokretanja benchmarking testova.

Prije nego što počnete, [Preuzmite FIO](https://github.com/axboe/fio) i instalirajte ga na virtualnog računala.

Pokrenite sljedeću naredbu za Ubuntu,

        apt-get install fio

Koristit ćemo četiri radnih niti za vožnju operacije pisanja i četiri radnih niti za vožnju čitanje operacije na s diska. Zaposlenici zaduženi za pisanje će biti vožnju promet na opseg "nocache", koja ima 10 diskova s predmemorije postavljen na "Ništa". Zaposlenici zaduženi za čitanje će biti vožnju promet na opseg "readcache", koja ima 1 na disku sa skupom predmemorije za "Samo za čitanje".

*Maksimalna pisanje IOPS*  
Stvaranje datoteke za posao sljedeće specifikacijama da biste dobili maksimalno IOPS za pisanje. Nazovite ga "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Imajte na umu prati ključne stvari usklađenih smjernice dizajna koji se spominju u ranijim odlomcima. Ove specifikacije su ključne za Poticanje Maksimalna IOPS  
-   Visoke reda čekanja dubinu 256.  
-   Mali bloka veličina 8KB.  
-   Više niti izvođenje slučajni zapisivanja.

Pokrenite sljedeću naredbu da biste izbaciti test FIO 30 sekundi  

    sudo fio --runtime 30 fiowrite.ini

Tijekom test, moći da biste vidjeli broj pisanje IOPS na VM i održavanja Premium diskova. Kao što je prikazano u primjeru u nastavku DS14 VM isporučuje njegov Maksimalna pisanje IOPS ograničenje od 50.000 IOPS.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maksimalna čitanje IOPS*  
Stvaranje datoteke za posao sljedeće specifikacijama da biste dobili maksimalno IOPS za čitanje. Nazovite ga "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Imajte na umu prati ključne stvari usklađenih smjernice dizajna koji se spominju u ranijim odlomcima. Ove specifikacije su ključne za Poticanje Maksimalna IOPS

-   Visoke reda čekanja dubinu 256.  
-   Mali bloka veličina 8KB.  
-   Više niti izvođenje slučajni zapisivanja.

Pokrenite sljedeću naredbu da biste izbaciti test FIO 30 sekundi

    sudo fio --runtime 30 fioread.ini

Tijekom test, moći da biste vidjeli broj čitanja IOPS na VM i održavanja Premium diskova. Kao što je prikazano u primjeru u nastavku DS14 VM isporučuje više od 64 000 IOPS za čitanje. Ovo je kombinaciju disk i performanse predmemoriju.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maksimalno za čitanje i pisanje IOPS*  
Stvaranje datoteke za posao s sljedeće specifikacije da biste dobili maksimalno kombinirati čitanje i pisanje IOPS. Nazovite ga "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Imajte na umu prati ključne stvari usklađenih smjernice dizajna koji se spominju u ranijim odlomcima. Ove specifikacije su ključne za Poticanje Maksimalna IOPS

-   Visoke reda čekanja dubinu 128.  
-   Veličina small bloka 4 KB.  
-   Više niti izvođenje slučajni čita i zapisuje.

Pokrenite sljedeću naredbu da biste izbaciti test FIO 30 sekundi

    sudo fio --runtime 30 fioreadwrite.ini

Tijekom test, će moći vidjeti broj kombinirane čitanje i pisanje na njih IOPS na VM i održavanja Premium diskova. Kao što je prikazano u primjeru u nastavku DS14 VM isporučuje više od 100 000 kombinirane čitanje i pisanje IOPS. Ovo je kombinaciju disk i performanse predmemoriju.  
    ![](media/storage-premium-storage-performance/image13.png)

*Maksimalno kombinirati propusnost*  
Da biste dobili maksimum kombinirane čitanje i pisanje propusnost, koristite veći blok i dubina velike reda čekanja s više niti izvođenje čitanja i pisanja. Možete koristiti veličina bloka od 64KB i dubinu reda čekanja 128.

## <a name="next-steps"></a>Daljnji koraci  

Dodatne informacije o pohranu Azure Premium:

- [Prostor za pohranu Premium: Visokih performansi prostor za pohranu radnih opterećenja Azure virtualnog računala](storage-premium-storage.md)  

Za korisnike sustava SQL Server, pročitajte članke na performanse najbolje prakse za SQL Server:

- [Performanse najbolje prakse za SQL Server na virtualnim računalima za Azure](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure Premium prostora za pohranu pruža najveće performanse za SQL Server na Azure VM](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 
