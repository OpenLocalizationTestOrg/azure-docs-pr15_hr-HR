<properties
   pageTitle="Kontrolni popis za skalabilnost | Microsoft Azure"
   description="Skalabilnost kontrolnog popisa smjernicama opasnosti dizajna za Azure Autoscaling."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="scalability-checklist"></a>Kontrolni popis za skalabilnost

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="service-design"></a>Servis za dizajn
- **Particija povećavaju**. Dizajnirajte dijelovi postupka samostalni i decomposable. Smanjite veličinu svaki dio tijekom pratiti uobičajeni pravila za odvojenosti opasnosti i jedan odgovornosti načelo. Time se omogućuje dijelove komponente koje će biti podijeljene na način koji Maksimizira iskoristili svaku jedinicu računalnim (kao što su uloga ili bazu podataka poslužitelja). Ga i olakšava skaliranje aplikacija dodavanjem pojavljivanja određene resurse. Dodatne informacije potražite u članku [Smjernice za izračunavanje particija](https://msdn.microsoft.com/library/dn589773.aspx).
- **Dizajn skaliranja**. Skaliranje omogućuje aplikacijama Upoznajte varijable učitavanju povećavanjem i smanjiti broj instanci uloge redovi, a koriste druge servise. Međutim, aplikacija mora biti dizajnirane Ovime na umu. Na primjer, aplikacija i servisa koristi mora biti bez praćenja stanja, da biste omogućili zahtjeve za usmjeriti sve instance. To također sprječava Dodavanje ili uklanjanje određene instance negativno koje utječu na trenutnog korisnika. Trebali biste i implementirate konfiguraciju ili automatsko otkrivanje instanci dodali i uklonili, tako da se kod u aplikaciji možete izvršiti potrebne usmjeravanja. Na primjer, web-aplikacije mogu koristiti skup redova u pristup kružnog dodjeljivanja za usmjeravanje zahtjeva za pozadinu servise uloge suradnika. Web-aplikacije moraju imati mogućnost da biste otkrili promjene u okvir broj redova uspješno usmjeravanje zahtjeve i saldo opterećenje aplikacije.
- **Promjena veličine kao jedinica**. Plan za dodatne resurse kako bi odgovarao rasta. Za svaki resurs znati gornjem skaliranje ograničenja, a pomoću sharding ili razlaganja okružje ta ograničenja. Odredite skaliranje jedinice za sustav pomoću dobro definirani skupovi resursa. Ovime zatvaranje operacije skaliranje iz lakše i manje podložni negativno utjecati na aplikaciju putem ograničenja nameće nedostatka resursa u neki dio cjelokupnog sustava. Na primjer, dodavanjem x broj uloge web i tempiranja možda će biti potrebno broj dodatne redove y i z broj računa za pohranu za rukovanje dodatne radno opterećenje generira uloge. Da bi se skaliranje jedinica ne može se sastojati od x uloge web i tempiranja, redova _y_ i _z_ prostora za pohranu računa. Dizajnirati aplikacija tako da se jednostavno skalirana dodavanjem jednu ili više jedinica mjerilo.
- **Izbjegavajte klijent afinitet**. Gdje je to moguće, provjerite je li aplikacija ne zahtijeva afinitet. Zahtjevi za stoga moguće usmjeriti sve instance, a koliko je instanci je nevažnih. Ovo izbjegava i indirektni spremanje, preuzimanja i održavanje informacije o stanju za svakog korisnika.
- **Iskoristite prednost platformu autoscaling značajke**. Mjesto pohrane platformu podržava autoscaling mogućnost, kao što je automatsko skaliranje Azure, radije prilagođeni ili drugih proizvođača mehanizme osim ako ugrađene mehanizam ne ispunjavaju svojim potrebama. Korištenje zakazani skaliranja pravila gdje je moguće da biste bili sigurni resursa dostupnih bez pokretanja odgode, ali dodati ponovno aktiviranje autoscaling pravila primjereno da biste cope promjenama neočekivana u zahtjev. Operacije autoscaling možete koristiti u upravljanju API servisa da biste prilagodili autoscaling, a da biste dodali prilagođenu mjerača pravila. Dodatne informacije potražite u članku [Automatsko skaliranje smjernice](best-practices-auto-scaling.md).
- **Offload intenzivno CPU-IO zadatke kao što su pozadinske zadatke**. Ako je zahtjev za uslugu se očekuje zahtijeva puno vremena da biste pokrenuli ili apsorbira dosta resursa, offload obrada za ovaj zahtjev zasebnom zadatak. Pomoću tempiranja uloge ili pozadine poslove (ovisno o hostinga platformu) izvršiti sljedeće zadatke. U ovom strategija omogućuje servisa da biste nastavili primati daljnje zahtjeve i ostaju odredište.  Dodatne informacije potražite u članku [smjernice za zadatke pozadine](best-practices-background-jobs.md).
- **Raspodijeli radno opterećenje za pozadinske zadatke**. Gdje su mnoge zadatke pozadine ili zadaci zahtijevaju dosta vremena ili resursa, šire rad preko više jedinica računalnim (primjerice uloge suradnika ili pozadine zadaci). Jedan dostupno rješenje potražite u članku [Natječu uzorak koje korisnici](https://msdn.microsoft.com/library/dn568101.aspx).
- **Razmislite o premještanju pri na _zajedničko korištenje nothing_ arhitektura**. Zajedničko korištenje nothing arhitektura koristi neovisan, samodostatan čvorove koji imaju bez jednu točku Nadmetanje (primjerice uslugama za zajedničko korištenje ili prostora za pohranu). U teorija, kao što je sustav mogu mijenjati veličinu gotovo beskonačno. Dok se potpuno zajednički-nothing pristup obično nije praktično za većinu aplikacija, mogu nuditi prilika za dizajniranje za bolje skalabilnost. Na primjer, izbjegavanje korištenja stanje sesije poslužiteljsko afinitet klijenta i particija podataka dobro Primjeri su premještanje prema arhitekturu zajednički nothing.

## <a name="data-management"></a>Upravljanje podacima

- **Korištenje podataka particija**. Dijeljenje podataka preko više baza podataka i poslužitelja baze podataka ili dizajn aplikaciju koju želite koristiti za pohranu podataka usluge koje možete unijeti ovu particija proziran (Primjeri Azure SQL baza podataka Elastic baze podataka i spremište tablica Azure). Taj se način može pridonijeti poboljšajte performanse i omogućuju lakše skaliranja. Postoje različite tehnike, kao što su vodoravno, okomito particija i funkcionirati. Koristite kombinaciju da biste postigli Maksimalno iskoristite performanse povećana upita, jednostavniji skalabilnost, fleksibilnije upravljanja, bolje dostupnosti i tako da odgovara vrstu spremišta podataka sadržavat će. Osim toga, razmislite o korištenju različitih vrsta podataka za različite vrste podataka, vrsta i načinu na koji su optimizirani za određene vrste podataka na temelju odabira. To može obuhvaćati korištenje spremišta tablica, baze podataka za dokument ili izvor podataka stupca obitelji, umjesto, tablica ili kao dobro kao relacijske baze podataka. Dodatne informacije potražite u članku [smjernice za stvaranje particija podataka](best-practices-data-partitioning.md).
- **Dizajn usmjerenog dosljednost**. Usmjerenog dosljednost poboljšava skalabilnost smanjivanje ili uklanjanjem vremena potrebnog za sinkronizaciju povezanih podataka particije preko više trgovine. Trošak je da podataka nije uvijek dosljedan kada ga je za čitanje, a neke pisanje operacije može uzrokovati sukobe. Usmjerenog dosljednost idealna je za situacije u kojima često čitate ali napisali diskovni iste podatke. Dodatne informacije potražite u članku [Primer dosljednost podataka](https://msdn.microsoft.com/library/dn589800.aspx).
- **Smanjivanje chatty interakcije između komponente i usluge**. Izbjegavanje dizajniranje interakcije aplikacije u kojima je potreban za više pozive na neki servis (od kojih svaka vraća malu količinu podataka), umjesto jednog poziva koje mogu vratiti sve podatke. Gdje je to moguće, Kombinirajte nekoliko povezanih operacija u zahtjev za jedan kada je poziv za servis ili komponenti uočljivijih Latencija. To olakšava praćenje performansi i optimizirati složene operacije. Ako, na primjer, Enkapsulacija složene logike pomoću pohranjene procedure u bazama podataka, a smanjiti broj zaokružiti trips i zaključavanje resursa.
- **Korištenje reda čekanja za razine opterećenje za zapisivanje podataka visoke brzine**. Izboja u zahtjev za uslugu možete pretrpati tu uslugu i uzrokovati escalating pogreške. Da biste spriječili, razmislite o implementacijom [utemeljen na red uzorak Leveling Učitaj](https://msdn.microsoft.com/library/dn589783.aspx). Koristite red koji funkcionira kao međuspremnik između zadatka i usluge koja se poziva. To možete glačanje Povremeni podebljanom opterećenje koji mogu uzrokovati inače servisa uvoza ili zadatak istek vremena.
- **Gumb za minimiziranje opterećenje spremišta podataka**. Spremište podataka obično je usko grlo obrada, skup resursa i često nije lako Vremensko mjerilo. Gdje je to moguće, uklonite logike (kao što su obrade XML dokumente ili objekte JSON) iz spremišta podataka i izvode obradu u aplikaciji. Ako, na primjer, umjesto prosljeđivanje XML u bazu podataka (ostalo od kao neprozirne niz za pohranu), Serijalizacija ili ukloniti serijski broj XML unutar aplikacijskom sloju i prosljeđivanje u obliku koji je izvorni spremište podataka. Je obično jednostavniji skaliranje izvan aplikacije od spremišta podataka da bi se trebali biste pokušati učinite koliko je obrada računalnim ćete morati usko moguće u aplikaciji.
- **Dohvaća Minimiziraj količinu podataka**. Vrati samo podatke ako su vam potrebne navodeći stupaca i korištenjem kriterija za odabir redaka. Korištenje parametara vrijednost tablice i razinu odvajanja odgovarajuće učiniti. Mehanizmi za korištenje kao što su entitet oznake da bi se izbjeglo nepotrebno dohvaćanja podataka.
- **Omogućuju agresivno koristiti predmemoriju**. Koristite predmemoriranje gdje god je moguće da biste smanjili opterećenje resursa i servise koje generirati ili izlaganje podataka. Predmemoriranje obično najprikladniji podatke koji je relativno statične ili koji zahtijeva dosta obradu da biste nabavili. Predmemoriranje se trebale provoditi na svim razinama primjereno svaki sloju aplikacije, uključujući podataka programa access i korisničko sučelje generacije. Dodatne informacije potražite u članku [Smjernice za predmemoriranje](best-practices-caching.md).
- **Rukovanje growth podataka i zadržavanje**. Vremenom poveća količinu podataka koji su pohranjeni aplikacija. Ovaj growth povećava troškove prostora za pohranu i povećava Latencija prilikom pristupa podacima – koji utječe na propusnost aplikacije i performanse. Možda ćete moći povremeno arhiviranje neke stare podatke koji se više ne pristupa ili premještanje podataka koje se rijetko pristupa Dugoročne prostora za pohranu koji se nalazi više trošak učinkovitog, čak i ako je Latencija pristup veći.
- **Optimiziranje podataka prijenos objekata (DTOs) pomoću učinkovitog binarnom obliku**. DTOs prosljeđuju između Slojevi aplikacije više puta. Minimiziranje veličinu smanjuje opterećenje resursa i na mreži. Međutim, saldo računanje s indirektnog pretvaranja podatke potrebne oblik na svakom mjestu gdje se koristi. Prihvaćaju oblik koji ima najveći interoperabilnost da biste omogućili jednostavno ponovno korištenje komponente.
- **Kontrola predmemorije skupa**. Dizajniranje i konfiguriranje aplikacije da biste koristili izlaznog predmemoriranja ili fragmentiraj predmemoriranje gdje je to moguće, da biste minimizirali opterećenje obrade.
- **Omogućivanje predmemoriranja na strani klijenta**. Web-aplikacije trebate omogućiti postavke predmemorije na sadržaj koji se mogu predmemorirane. To najčešće onemogućeno je prema zadanim postavkama. Konfiguriranje poslužitelja izlaganje predmemoriju odgovarajuće kontrole zaglavlja da biste omogućili predmemoriranje sadržaja na proxy poslužitelji i klijentske programe.
- **Spremište blobova platforme Azure koristi i Azure sadržaja isporuke mreže da biste smanjili mogućnost Učitaj u aplikaciji**. Razmislite o spremanju statične ili relativno statičke javno sadržaja, kao što su slike, resursi, skripte i listove stila, u spremište blobova platforme. Taj se način relieves aplikacije opterećenje uzrokovanih dinamičko stvaranje sadržaja za svaki zahtjev za. Uz to, razmislite o korištenju mreže za isporuku sadržaja za predmemoriranje sadržaja i izlaganje klijentima. Pomoću mreže za isporuku sadržaja možete poboljšati performanse na klijentskom računalu Budući da sadržaj se isporučuje s geografski najbližim podatkovnim centrom koja sadrži predmemoriju mreže za isporuku sadržaja. Dodatne informacije potražite u članku [Sadržaja upute mreže za isporuku](best-practices-cdn.md).
- **Optimizacija i podešavanje SQL upite i indeksi**. Neke T SQL naredbe ili konstrukta može imati utjecaj na performanse koje možete smanjiti tako da optimiziranje kod u pohranjena procedura. Na primjer, nemojte Pretvorba vrste **datuma i vremena** u **varchar** prije usporedbu s pisana vrijednost **datuma i vremena** . Umjesto toga koristite funkcije usporedbu datuma/vremena. Nedostatak odgovarajuće indeksi također može usporiti izvršavanje upita. Ako koristite objekt relacijski framework za mapiranje, razumijevanje načina funkcioniranja tijeka rada i kako to može utjecati na performanse sloj pristupa podacima. Dodatne informacije potražite u članku [Usklađivanje upita](https://technet.microsoft.com/library/ms176005.aspx).
- **Razmislite o deaktivirali normalizacija podataka**. Normalizacija podataka omogućuje da biste izbjegli dupliciranje i nedosljednosti. Međutim, održavanje višestrukih indeksa, provjera referencijalnog integriteta, izvođenje više pristupa za small blokova podataka i pridruživanje tablice za reassemble podataka nameće programa indirektni koji mogu utjecati na performanse. Preporučujemo ako neki glasnoću dodatni prostor za pohranu i dupliciranje je prihvatljiva da biste smanjili opterećenje spremišta podataka. Uzmite u obzir ako same aplikacije (što je obično jednostavnije za promjenu veličine) može biti relied nakon za preuzimanje uloge zadatke kao što su upravljanje referencijalnog integriteta da biste smanjili opterećenje spremišta podataka. Dodatne informacije potražite u članku [smjernice za stvaranje particija podataka](https://github.com/mspnp/azure-guidance/blob/master/Data%20partitioning.md).

## <a name="service-implementation"></a>Servis za implementaciju
- **Korištenje asinkronog poziva**. Korištenje asinkronog kod gdje god je moguće kada pristupanju resursa ili servisa koji se može biti ograničeno količinom/i ili propusnost mreže ili koji uočljivijih Latencija, da biste izbjegli zaključavanje pozivanja niti. Da biste implementirate asinkronog operacije, koristite [Utemeljenoj na zadacima asinkronog uzorak (DODIRNITE)](https://msdn.microsoft.com/library/hh873175.aspx).
- **Izbjegavanje zaključavanje resursa, i upotrijebite optimistična Procjena pristup**. Nikad ne zaključavanje pristupa resursima kao što je prostor za pohranu ili druge servise koji imaju uočljivijih Latencija jer je primarni uzroka slabe performanse. Uvijek koristi optimistična Procjena postupke za upravljanje istovremene operacije, kao što je prostor za pohranu za pisanje. Pomoću značajki sloja za pohranu za upravljanje sukobe. U raspodijeljeno aplikacije podataka može biti samo naposljetku dosljedni.
- **Komprimiranje iznimno compressible podataka putem visokom latencijom, niske propusnosti mreže**. U većini slučajeva u web-aplikaciji najveću količinu podataka generirala aplikacija i proslijeđena putem mreže je HTTP odgovore na zahtjeve klijenta. Sažimanje HTTP možete smanjiti to znatno, posebice za statični sadržaj. Iako sažimanje dinamičkog sadržaja primjenjiv fractionally veći Učitaj na poslužitelju to može smanjiti trošak kao i smanjuje mogućnost Učitaj u mreži. U drugi, više generalizirano okruženjima spajanja podataka možete smanjiti količinu podataka koji se šalju i minimiziranja vrijeme prijenosa i troškove, ali procesa sažimanja i dekompresiju plaćati indirektni. Kao takve, sažimanje mogu koristiti samo kada je demonstrable rast performanse. Druge načine serijalizacije, kao što su JSON ili binarni kodiranja može smanjiti tereta tijekom imate manje utjecaj na performanse, dok je vjerojatnost da bi ga povećala XML.
- **Vrijeme koje veze i resursima se koriste za minimiziranje**. Održavanje veza i resursima samo za sve dok ih koristite. Na primjer, kašnjenja moguće otvorite veze, a dopustite vratiti skup veza čim. Kašnjenja moguće pribavite resurse i čim rashoda od njih.
- **Obavezno Smanji broj veza**. Povezivanje sa servisom apsorbira resursi. Ograničiti broj rezultata koji su potrebni i provjerite je li se ponovno postojeće veze kad god je moguće. Ako, na primjer, nakon izvršavanja provjere autentičnosti, koristite oponašanja primjereno da biste pokrenuli kod kao određeni identitet. To se može pomoći da bi najbolje korištenje skup veza ponovnim korištenjem veze.

    > [AZURE.NOTE]: APIs for some services automatically reuse connections, provided service-specific guidelines are followed. It's important that you understand the conditions that enable connection reuse for each service that your application uses.

- **Slanje zahtjeva za skupna da biste optimizirali mreže**. Ako, na primjer, slanje pročitanim porukama u serijama prilikom pristupanja reda i obavljanje više čitanja i pisanja obliku serije prilikom pristupanja za pohranu ili predmemoriju. To može pridonijeti Maksimiziranje učinkovitost trgovine servisa i podataka tako da smanjite broj poziva putem mreže.
- **Izbjegavajte zahtjeva za pohranu stanje sesije poslužiteljsko** gdje je to moguće. Upravljanje poslužiteljskim sesiju stanje obično zahtijeva klijent afinitet (to, usmjerava svaki zahtjev u istoj instanci server), koje utječu na mogućnost sustava za promjenu veličine. Najbolje treba dizajnirati klijenti mogli biti bez praćenja stanja vezana uz s poslužiteljima koji koriste. Međutim, ako aplikacija morate zadržati stanje sesije, spremite povjerljive podatke ili velike količine podataka po klijenta u raspodijeljenoj predmemoriji poslužiteljsko koji mogu pristupiti sve instance programa.
- **Optimizacija tablice za pohranu sheme**. Prilikom korištenja trgovine tablice koje je potrebno nazive tablica i stupaca proslijeđena i obrađuju s svaki upit, kao što je spremište tablica platforme Azure, preporučuje se korištenje kraće nazive da biste smanjili ovaj indirektni. Međutim, nije sacrifice čitljivosti i mogućnost upravljanja pomoću pretjerano sažetom naziva.
- **Korištenje na zadatak paralelno biblioteke (TPL) za izvođenje asinkronih operacija**. Na TPL olakšava napišite asinkronog kod koji se izvodi li/uđu operacije vezane. Koristite _ConfigureAwait(false)_ gdje god je moguće da biste uklonili ovisnost nastavka na određene sinkronizacije kontekstu. Time se smanjuje mogućnost niti – zastoja zbog koje su se pojavile.
- **Stvaranje resursa ovisnosti tijekom implementacije ili prilikom pokretanja aplikacije**. Izbjegavajte koji se ponavljaju pozive na metode koje testiranje postojanje resursa, a zatim stvorite resursa ako ne postoji. (Metoda kao što su _CloudTable.CreateIfNotExists_ i _CloudQueue.CreateIfNotExists_ u biblioteci Azure prostora za pohranu klijenta, slijedite ovaj uzorak). Ove metode možete nametnuti dosta indirektni ako oni su pozvani prije pristup tablice za pohranu ili reda čekanja za pohranu. Umjesto toga:
 - Stvaranje potrebni resursi kada je implementiran aplikacije ili kada prvi put pokrene (jedan poziv _CreateIfNotExists_ za svaki resurs kod prilikom pokretanja za web ili tempiranja ulogu je prihvatljivi). Međutim, obavezno rukovati iznimke koje se može pojaviti ako kod pokuša pristup resursu koji ne postoji. U tim situacijama treba prijavite iznimku i upozoriti operator nedostaje resursa.
 - U nekim okolnostima, možda će biti odgovarajuće da biste stvorili nedostaje resursa kao dio iznimku kod za rukovanje. No prihvaćaju takvog oprezni s u kao što je koji nisu-postojanje resursa možda indikativnih pogrešku (naziv napisane resursa, primjerice) ili neki problem infrastrukture razinu.
- **Korištenje laganih okviri**. Pažljivo odaberite API-ji i okviri koristite da biste minimizirali korištenje resursa, vrijeme izvođenja i cjelokupan opterećenje aplikacije. Ako, na primjer, pomoću Web API rukovati zahtjevima za uslugu možete smanjiti ostavlja manji trag pri za aplikacije i povećali brzinu izvođenja, ali možda nije prikladna za napredne scenarije gdje su potrebne dodatne mogućnosti Windows Communication Foundation.
- **Razmislite o minimiziranje broj računa servisa**. Primjerice, koristite određeni račun za pristup resursima ili servise koji se postavljaju ograničenje na veze ili ponovno bolje gdje se održavaju manje veze. Taj se način je zajednička za servise kao što je baza podataka, ali može utjecati na mogućnost točno nadzora operacije zbog oponašanja izvorne korisnika.
- **Izvedi Profiliranje performanse i učitavanje testiranja** tijekom razvoja u sklopu test postupke, a prije konačnu da biste bili sigurni izvodi aplikacije i ljestvice prema potrebi. Testiranje bi se trebale provoditi na istu vrstu hardver kao platformu proizvodnje i s iste vrste i količine podataka i korisnik učitati kao naići ćete u radnog. Dodatne informacije potražite u članku [testiranje performanse servis u oblaku](vs-azure-tools-performance-profiling-cloud-services.md).