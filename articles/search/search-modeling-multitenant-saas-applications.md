<properties
    pageTitle="Modeliranje Multitenancy Azure pretraživanja | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Informirajte se o uobičajene uzorke dizajna za složene aplikacije SaaS tijekom korištenja Azure pretraživanja."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Dizajniranje obrazaca za web-mjesto složene SaaS aplikacije i u okvir za pretraživanje Azure

Složene aplikacije jest ona koja nudi iste usluge i mogućnosti bilo koji broj drugih korisnika koji ne možete vidjeti ili zajedničko korištenje podataka drugi klijent. Ovaj dokument govori o klijentu odvajanja strategije složene aplikacije načinjene pomoću pretraživanja Azure.

## <a name="azure-search-concepts"></a>Azure pojmovi za pretraživanje
Kao rješenje kao-na-servisa za pretraživanje, pretraživanje Azure razvojnim inženjerima omogućuje dodavanje obogaćenog pretraživanje sučelja aplikacije bez upravljanje sve infrastrukture ili postanete stručnjak pretraživanja. Podaci prenijeli na servis i zatim pohranjen u oblaku. Pomoću jednostavne zahtjevi za pretraživanje API Azure, podatke možete biti izmijeniti i pretraživati. Pregled usluge pronaći ćete u [ovom članku](http://aka.ms/whatisazsearch). Prije nego razgovarate o dizajn obrazaca, dobro je razumjeti neke koncepte Azure pretraživanja.

### <a name="search-services-indexes-fields-and-documents"></a>Usluge pretraživanja, indeksi, polja i dokumenata
Prilikom korištenja Azure pretraživanja, jedan pretplaćuje _servis za pretraživanje_. Kao što je podataka se prenosi Azure pretraživanja, on se sprema u _indeks_ unutar servisa za pretraživanje. Može biti broj indeksa u jedan servis. Da biste koristili poznatih konceptima baze podataka, servis za pretraživanje može biti likened s bazom podataka dok indeksi unutar servisa može biti likened tablice u bazi podataka.

Svaki indeks unutar servisa za pretraživanje ima vlastitu shemu, koja je definirana prema broju prilagodljive _polja_. Podaci dodaju se u indeks pretraživanja Azure u obliku pojedinačnih _dokumenata_. Svaki dokument mora moguće prenijeti na određeni indeksa i morate prilagoditi to kazalo sheme. Pri pretraživanju podataka pomoću pretraživanja Azure upita za pretraživanje po cijelom tekstu izdavanja prema određenom indeksa.  Da biste usporedili koncepte onima baze podataka, polja mogu biti likened u stupce u tablicu i dokumente koje se mogu likened u retke.

### <a name="scalability"></a>Skalabilnost
Bilo koji servis za pretraživanje Azure u standardne [cijene sloju](https://azure.microsoft.com/pricing/details/search/) mogu mijenjati veličinu u dvije dimenzije: pohrana i dostupnost.
* Da biste povećali prostor za pohranu servisa za pretraživanje moguće je dodavati _particije_ .
* _Replike_ mogu se dodati na servis da biste povećali propusnost zahtjeva za servis za pretraživanje možete učiniti.

Dodavanje i uklanjanje particije i replike pri omogućuje kapacitet servisa za pretraživanje da biste rast s količinu podataka i traffic zahtjeva za aplikaciju. U redoslijedu za servis za pretraživanje da biste postigli čitanje [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)zahtijeva dvije replike. U redoslijedu servisa da biste postigli čitanja i pisanja [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)zahtijeva tri replike.


### <a name="service-and-index-limits-in-azure-search"></a>Ograničenja servisa i indeks pretraživanja Azure
Postoji nekoliko različitih [cijene razine](https://azure.microsoft.com/pricing/details/search/) u pretraživanju Azure, svaki od na razine ima različite [ograničenja i kvote](search-limits-quotas-capacity.md). Neke od tih ograničenja na razini usluge, neke su na razini indeksa i neke su na razini particije.


|                                  | Osnovni     | Standard1   | Standard2   | Standard3   | Standard3 HD  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Maksimalna replike po servisa     | 3         | 12          | 12          | 12          | 12            |
| Maksimalna particije po servisa   | 1         | 12          | 12          | 12          | 1             |
| Maksimalna pretraživanje jedinice (replike * particije) po servisa | 3         | 36          | 36          | 36          | 36 (particije Maks 3)            |
| Najveći broj dokumenata po servisa    | milijuna 1 | milijuna 180 | 720 milijuna | 1.4 milijarde | milijuna 600   |
| Maksimalna prostor za pohranu po servisa      | 2 GB      | 300 GB      | 1.2 TB      | 2.4 TB      | 600 GB        |
| Najveći broj dokumenata po Partition  | milijuna 1 | milijuna 15  | 60 milijuna  | milijuna 120 | 200 milijuna   |
| Maksimalna prostor za pohranu po Partition    | 2 GB      | 25 GB       | 100 GB      | 200 GB      | 200 GB        |
| Maksimalna indeksi po servisa      | 5         | 50          | 200         | 200         | 3000 (Maks 1000 indeksi/particija)          |


#### <a name="s3-high-density"></a>S3 Visoke gustoće "
U sloju cijene S3 Azure pretraživanja, postoji mogućnost načinu Visoki gustoće (HD) namijenjenih složene scenarijima. U mnogim slučajevima je potrebno podržava velik broj manji drugih korisnika u odjeljku servis za jednu da biste postigli prednosti učinkovitosti jednostavnosti i trošak.

S3 HD omogućuje brojne small indekse do zapakirane u odjeljku Upravljanje servisa za jednu pretraživanje po trgovina mogućnost skaliranja out indeksi pomoću particije za mogućnost za hostiranje više indeksa u jedan servis.

Concretely, servis za S3 može imati između 1 i 200 indeksa koji nije zajedno hostira do 1.4 milijarde dokumenata. S druge strane programa HD S3 Dopusti pojedinačne indeksa samo idite do 1 milijuna dokumenata, ali možete rukovati do 1000 indeksi po particija (do 3000 po servisa) s ukupni dokument 200 milijuna po particija (najviše 600 milijuna po servisa).



## <a name="considerations-for-multitenant-applications"></a>Zahtjevi za složene aplikacije
Složene aplikacije učinkovito morate distribuirati resursi među na klijenata uz čuvanje neke razinu zaštite privatnosti između klijenata za različite. Postoji nekoliko pitanja vezana uz prilikom dizajniranja arhitektura za pretraživanje kao aplikaciju:

* _Smjernice odvajanja:_ Aplikacija razvojnim inženjerima mora poduzeti odgovarajuće mjere da biste bili sigurni da nema klijenata neovlašteno ili neželjeni pristup podacima drugim korisnicima. Izvan perspektive podataka o zaštiti privatnosti, strategije odvajanja klijentu zahtijevaju učinkovitih upravljanja zajedničkim resursima i zaštita od oscilirajuće susjeda.
* _Trošak resursa u oblaku:_ Kao i kod neke druge aplikacije softverskih rješenja mora biti trošak konkurencije kao komponenta složene aplikacije.
* _Olakšani operacije:_ Prilikom razvoja složene arhitektura utjecaj na operacije i složenosti aplikacije je važna činjenica. Azure pretraživanje sadrži [SLA 99.9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* _Globalni ostavlja manji trag pri:_ Složene aplikacije možda morati učinkovito služe drugih korisnika koji su distributed diljem svijeta.
* _Skalabilnost:_ Razvojnim inženjerima treba uzeti u obzir kako se uskladiti između održavanje potpuno nisku razinu složenosti aplikacije i dizajniranja aplikacije da biste skalirali broj klijenata i veličine podataka i radno opterećenje klijenata.

Azure pretraživanje nudi nekoliko ograničenja koja se može koristiti za izdvajanje podataka i radno opterećenje klijenata.

## <a name="modeling-multitenancy-with-azure-search"></a>Modeliranje multitenancy pomoću pretraživanja Azure
U slučaju složene scenarij razvojni inženjer troši usluge pretraživanja i dijeljenje njihove klijenata među services, indeksa ili oboje. Azure pretraživanje sadrži nekoliko uobičajenih uzoraka kada Modeliranje složene scenarija:

1. _Indeks po klijentu:_ Svaki klijent ima vlastitu indeks unutar servisa za pretraživanje koje je omogućeno zajedničko korištenje s drugim korisnicima.
1. _Servisa po klijentu:_ Svaki klijent ima vlastitu namjenski servisa za pretraživanje Azure, koja nudi najvišu razinu podataka i radno opterećenje odvojenosti.
1. _Mix obje:_ Veće, više aktivno klijenata dodjeljuju namjenski servisima dok klijenata za manje dodjeljuju pojedinačne indeksi unutar usluga za zajedničko korištenje.

## <a name="1-index-per-tenant"></a>1. indeksirati po klijentu
![Portrayal modela indeks po klijentu](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

U modelu indeks po klijentu više klijenata zauzimaju jedan servis za pretraživanje Azure gdje svaki klijent ima vlastite indeksa.

Samoposlužni postigli odvajanja podataka jer sve zahtjeve za pretraživanja i operacije dokument izdavanja na razini indeks pretraživanja Azure. U aplikacijskom sloju postoji Informiranost potrebe da biste usmjerili promet klijenata za različite proper indeksi tijekom i upravljanja resursima na razini preko svih drugih korisnika.

Ključ atributa modela indeks po klijentu je mogućnost razvojni inženjer da biste oversubscribe kapacitet servisa za pretraživanje između klijenata za aplikacije. Ako na klijenata nejednaki distribucija radno opterećenje, optimalnih kombinaciju drugih korisnika možete raspodijeliti indeksi pretraživanja servisa kako bi odgovarao broj iznimno Aktivno, resursa ćete morati usko klijenata tijekom istodobno posluživanje dugih manje aktivni klijenata. U trade-off je nemogućnosti modela u raznim situacijama gdje svaki klijent aktivan istovremeno visoko.

Model indeks po klijentu služi kao temelj za varijabla trošak model, na kojoj je kupili unaprijed cijelu servis za pretraživanje Azure te naknadno ispunjava s klijenata. Time se omogućuje kapaciteta koji se ne koriste za označavanje za besplatne račune i probne verzije.

Za aplikacije s globalnog ostavlja manji trag pri indeks po klijentu modela možda neće biti u najučinkovitiji. Ako su aplikacije sustava klijenata distribuirali diljem svijeta, odvojene servis možda neće biti potrebne za svako područje koje možda dupliciranje troškove preko svaku od njih.

Azure pretraživanja omogućuje skale pojedinačne indekse i ukupan broj indeksa radi povećanja. Ako odgovarajuće cijene sloju je odabran, particije i replike mogu se dodati na servis za cijelu pretraživanja kada indeksa pojedinačne unutar servisa rastom prevelika pomoću prostora za pohranu ili promet.

Ako ukupan broj indeksi poveća prevelika za jedan servis, drugi servis mora biti dodijeljena da bi odgovarao klijenata za nove. Ako indeksi će se premjestiti između servisa za pretraživanje kao što je dodat će se nove servise, podaci iz indeksa mora se ručno kopirati iz jednog indeksa drugome kao Azure pretraživanje ne dopušta indeks će se premjestiti.


## <a name="2-service-per-tenant"></a>2. servisa po klijentu
![Portrayal modela servisa po klijentu](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Svaki klijent arhitekturu servisa po klijentu ima vlastitu servisa za pretraživanje.

U ovom modelu aplikacije postiže Maksimalna razinu odvajanja za njegov drugih korisnika. Svaki servis je namjenski prostor za pohranu i propusnost za obrade zahtjeva za pretraživanje, kao i zasebnom tipke API-JA.

Za aplikacije, gdje svaki klijent ima velike ostavlja manji trag pri ili povećavaju ima malo fleksibilnosti iz klijenta za klijenta, modela servisa po klijentu je učinkovit odabir kao resurse su zajedničke radnih opterećenja različitih drugih korisnika.

Servis po klijentu modelu također nudi prednosti modela predvidljivi, fiksni trošak. Postoji bez unaprijed ulaganja u cijeli pokrenite sve dok se klijent za ispunu, no trošak-po-klijentu veća je od modelu indeks po klijentu.

Model servisa po klijentu je učinkovit odabir za aplikacije s globalnog ostavlja manji trag pri. S geografski distributed klijenata je lako u odgovarajuću regiju imati servis za svaki klijent.

Izazove u skaliranje ovaj uzorak pojaviti pri klijenata za pojedinačne outgrow njihove servisa. Azure pretraživanja u trenutno ne podržava nadogradnje cijene sloju servisa za pretraživanje da biste ručno kopirat novog servisa bi morali sve podatke.

## <a name="3-mixing-both-models"></a>3. Miješanje i modela
Drugi uzorka za Modeliranje multitenancy je Miješanje strategije indeks po klijentu i usluge po klijentu.

Po Miješanje dva uzoraka najveću klijenata za aplikacije sustava možete proširiti namjenski servisa dok dugih klijenata za manje Aktivno, manja možete proširiti indeksa u zajednički servis. Ovaj model omogućuje koji najveću klijenata dosljedno visoke performanse iz servisa dok zaštita klijenata za manje od bilo kojeg oscilirajuće susjeda.

Međutim, implementacijom ovaj strategije oslanja foresight u procjenjivanje klijenata za koje je potrebno namjenski servisa nasuprot indeksa u zajednički servis. Aplikacija složenosti povećava s potrebe za upravljanje obje te multitenancy modela.

## <a name="achieving-even-finer-granularity"></a>Postizanje još precizniji preciznosti
Iznad uzoraka dizajn da biste modela složene scenariji u pretraživanju Azure pretpostavlja opseg uniform pri čemu svaki klijent je cijeli instance programa. Međutim, aplikacija možete ponekad ručicu za mnoge manje opsega.

Ako servis po klijentu i indeksa po klijentu modeli nisu potpuno small dosega, moguće je model indeksa za postizanje još precizniji stupanj preciznosti.

Da bi se jednom indeks se drugačije ponašaju za krajnje točke drugi klijent, polja mogu se dodati indeks koji određuje određene vrijednosti za svaki mogući klijent. Svaki put kada klijentsko poziva Azure pretraživanja upita ili izmijeniti indeks, koda iz klijentske aplikacije određuje odgovarajuće vrijednosti za to polje pomoću mogućnosti [filtriranja](https://msdn.microsoft.com/library/azure/dn798921.aspx) za Azure pretraživanja u trenutku upita.

Ovaj postupak možete koristiti da biste postigli funkcionalnost zasebne korisničke račune, odvojene razina, i čak i potpuno odvojite aplikacije.

## <a name="next-steps"></a>Daljnji koraci
Azure pretraživanje je atraktivnog izbor za mnoge aplikacije, [Saznajte više o servisa robusne mogućnosti](http://aka.ms/whatisazsearch). Prilikom procjene razne uzorke dizajna za složene aplikacije, razmislite o [različite razine cijena](https://azure.microsoft.com/pricing/details/search/) i odgovarajući [ograničenja servisa](search-limits-quotas-capacity.md) za najbolje neće morati prilagođavati Azure pretraživanja tako da stane radnih opterećenja aplikacije i arhitekturi svih veličina.

Pitanja o pretraživanju Azure složene scenariji mogu upućivati na azuresearch_contact@microsoft.com.
