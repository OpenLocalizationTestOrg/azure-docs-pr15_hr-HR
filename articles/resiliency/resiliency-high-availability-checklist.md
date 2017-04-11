<properties
   pageTitle="Kontrolni popis za visoke dostupnosti | Microsoft Azure"
   description="Brzi kontrolnog popisa postavki i akcije koje možete poduzeti da bi vam se poboljšanje dostupnosti aplikacija s Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-checklist"></a>Kontrolni popis za visoke dostupnosti
Jedna od sjajno prednosti korištenja Azure je mogućnost da biste povećali dostupnosti (i skalabilnost) aplikacija uz pomoć oblaka. Da biste bili sigurni da su izvući najviše od tih mogućnosti, ispod kontrolnog popisa je namijenjena će vam pomoći s nekim osnove ključa infrastrukture za osiguravanje jesu li aplikacija prebacuju. 

>[AZURE.NOTE] Većina prijedloga ispod stvari koje možete implementirati u bilo kojem trenutku u aplikaciji i stoga su odlične za "brzo rješenja". Najbolje dugoročno rješenje često obuhvaća dizajniranje za aplikacije koja je ugrađena za oblaka.  Za kontrolnog popisa na te (više dizajna usmjerena područja, pročitajte našu [dostupnost kontrolnog popisa](../best-practices-availability-checklist.md).

###<a name="are-you-using-traffic-manager-in-front-of-your-resources"></a>Koristite upravitelj promet ispred resursa?
Pomoću upravitelja promet pomaže usmjeravate internetski promet preko Azure područja ili Azure i lokalnih mjesta. To možete učiniti nekoliko razloga uključujući Latencija i dostupnost. Ako želite da biste saznali dodatne informacije o korištenju upravitelja promet kako biste povećali vaše stabilnosti i širenje prometa na više područja, pročitajte [Pokretanje VMs u više podatkovnim centrima na Azure za visoke dostupnosti](../guidance/guidance-compute-multiple-datacenters.md).

__Što se događa ako ne koristite Upravitelj promet?__ Ako ne koristite Upravitelj promet ispred aplikacije, ste ograničeni su na jedno područje za resurse. To ograničava vaš skaliranje povećava Latencija korisnicima koji nisu blizu regiju u kojoj ste odabrali, a Spušta zaštite slučaju regija razini usluge prekidu.

###<a name="have-you-avoided-using-a-single-virtual-machine-for-any-role"></a>Imate izbjegava pomoću jednog virtualnog računala za sve uloga?
Dobar dizajn izbjegava sve jednu točku nije uspjelo. To je važno u dizajnu sve usluge (lokalni u oblaku), ali je posebno korisno u oblaku kao što je možete povećati skalabilnost i otpornost kroz skaliranje out (Dodavanje virtualnim strojevima) umjesto skaliranje (jače virtualnog računala, pomoću). Ako želite da biste saznali dodatne informacije o dizajniranje skalabilni aplikacije, pročitajte [visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](resiliency-high-availability-azure-applications.md).

__Što se događa ako imate jedan virtualnog računala uloge?__ Na jednom računalu je jednu točku pogreške i nije dostupna za [Azure ugovor o razini usluge virtualnog računala](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). U slučajevima najbolje aplikacija će se izvoditi samo precizno, no to nije prebacuju dizajna, ne pokriva virtualnog računala SLA Azure i sve jednu točku neuspjeh povećava izgledi nedostupnost ako nešto ne uspije.

###<a name="are-you-using-a-load-balancer-in-front-of-your-applications-internet-facing-vms"></a>Koristite li opterećenja ispred VMs mjesto na Internetu vaše aplikacije?
Učitavanje balancers omogućuju podjele dolazne promet na vaše aplikacije preko proizvoljne broj strojeva. Koje možete Dodaj/Ukloni strojeva iz raspoređivača opterećenja u bilo kojem trenutku, koji funkcionira dobro s virtualnim strojevima (i i automatsko skaliranje s virtualnog računala skaliranje skupovima) da biste mogli jednostavno učiniti povećava promet ili VM pogreške. Ako želite saznati više o balancers opterećenja, pročitajte [Pregled Azure raspoređivača opterećenja](../load-balancer/load-balancer-overview.md) i [pokrenuti više VMs na Azure skalabilnost i dostupnost](../guidance/guidance-compute-multi-vm.md).

__Što se događa ako se ne koriste raspoređivača opterećenja ispred VMs na mjesto na Internetu?__ Bez raspoređivača opterećenja će nećete moći Vremensko mjerilo (Dodaj dodatne virtualnim strojevima) i bit će vaša jedina mogućnosti skaliranja (povećati virtualnog računala na web-mjesto). Također će biti namijenjeno jednu točku s tom virtualnog računala. Također ćete DNS-a kod da biste primijetiti ako ste izgubili računala na mjesto na Internetu i ponovno mapirati unos DNS-a za novo računalo nego što započnete njegov odvija.

###<a name="are-you-using-availability-sets-for-your-stateless-application-and-web-servers"></a>Jesu li koristite dostupnost skupovi za bez praćenja stanja računala i web-poslužiteljima?
Umetanja na računalima u istoj razina aplikacije u skupu dostupnost čini vaš VMs uvjete za Azure VM SLA. Dio Raspoloživost i postavljanje osigurava vašeg računala smještaju u različitim ažuriranje domene (odnosno drugo glavno strojevima koji su patched na različite vremenske) i kvara domene (odnosno strojeva glavno računalo zajednički koristite zajednički izvor napajanja i mrežne uključivanje). Bez u tijeku u skupu dostupnost vaše VMs može nalaziti na istom računalu glavnog računala i time mogu postojati jednu točku pogreške koja nije vidljiva na. Ako želite da biste saznali dodatne informacije o povećava dostupnost vaša VMs korištenje skupova dostupnost, pročitajte [Upravljanje dostupnost virtualnih računala](../virtual-machines/virtual-machines-windows-manage-availability.md).

__Što se događa ako ne koristite raspoloživost postaviti bez praćenja stanja aplikacije i web-poslužiteljima?__ Ne koristite raspoloživost postavite znači da ne možete iskoristiti Azure VM SLA. Također znači da strojeva u taj sloj aplikacija nije sve izvanmrežni ako je raspoloživo ažuriranje na glavnom računalu (računalo koje hostira VMs koristite) ili uobičajene pogreške hardvera.

###<a name="are-you-using-virtual-machine-scale-sets-vmss-for-your-stateless-application-or-web-servers"></a>Koristite virtualnog računala skaliranje skupove (VMSS) za bez praćenja stanja računala ili web-poslužitelja?
Dobar skalabilni i prebacuju dizajn koristi VMSS da biste bili sigurni da vam možete Povećaj/Stisni broj strojeva u sloju aplikacije (kao što je vaše web razina). VMSS omogućuje vam da biste odredili kako se vaše razina aplikacije mijenja veličinu (dodavanjem ili uklanjanjem poslužitelje ovisno o odabranim kriterijima). Ako želite da biste saznali dodatne informacije o korištenju Azure virtualnog računala skaliranje skupove da biste povećali vaše otpornost na promet krivina, pročitajte [Virtualnog računala skaliranje skupove pregled](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

__Što se događa ako ne nam virtualnog računala skaliranje skup Moje aplikacijom bez praćenja stanja web-poslužitelju?__ Bez VMSS, ograničiti mogućnost skaliranja bez ograničenja i da biste optimizirali korištenje resursa. Dizajn koji nedostaju VMSS ima gornju granicu skaliranja čiji će se rukovati umeće dodatni kod (ili ručno). Nemaju na VMSS i znači da aplikacija možete jednostavno ne dodavati i uklanjati strojeva (bez obzira na to skaliranje) da biste lakše rukovati velike krivina prometa (takve tijekom promociju ili ako je web-mjesta/aplikacije/proizvoda dolazi širenjem virusa te).

###<a name="are-you-using-premium-storage-and-separate-storage-accounts-for-each-of-your-virtual-machines"></a>Koristite premium i prostor za pohranu računi zasebnom prostora za pohranu za svaku virtualnih računala?
Je najbolji način da biste koristili premium prostora za pohranu za proizvodnju virtualnim strojevima sa sustavom. Osim toga, provjerite je li koristite račun zasebnom prostora za pohranu za svaku virtualnog računala (to vrijedi za manjih razmjera implementacije. Za veće implementacije možete iskoristiti za pohranu račune za ali postoji više računala je opterećenja koje je potrebno poduzeti da biste bili sigurni da su raspoređen ažuriranje domene i razine aplikacije). Ako želite da biste saznali dodatne informacije o Azure prostora za pohranu performanse i skalabilnost, pročitajte [Microsoft Azure prostora za pohranu performanse i skalabilnost kontrolnog popisa](../storage/storage-performance-checklist.md).

__Što se događa ako ne koristite odvojene prostora za pohranu računa za svaki virtualnog računala?__ Račun za pohranu, kao što je mnogo drugih resursa je jednu točku nije uspjelo. Premda su mnoge zaštiti i značajke otpornost Azure prostora za pohranu, jednu točku neuspjeh nikad nije dobro dizajna. Na primjer, ako prava pristupa oštećena na račun, ne pronalazi ograničenje prostora za pohranu ili je [ograničiti IOPS](../azure-subscription-service-limits.md#virtual-machine-disk-limits) zbirka dostigne, utjecati su sve virtualnim strojevima računa za pohranu. Osim toga, ako postoji prekidu servis koji utječe oznaku prostora za pohranu koji sadrži taj račun za određeni prostor za pohranu može imati više virtualnim strojevima utjecati.

###<a name="are-you-using-a-load-balancer-or-a-queue-between-each-tier-of-your-application"></a>Koristite raspoređivača opterećenja ili red između svakog sloju aplikacije?
Pomoću balancers učitavanja ili redova između svakog sloju aplikacije omogućuje vam da jednostavno skaliranje svaki sloju aplikacije jednostavno i zasebno. Trebali biste odabrati između tih tehnologija Latencija, složenosti, na temelju i mora raspodjele (odnosno raspršenosti distribuirate aplikacije). Općenito govoreći, redove obično imate veće Latencija i dodati složenosti, ali vam se više prebacuju i što omogućuje distribuiranje aplikacija iznad područja za veće koristiti (kao što su preko područja). Ako želite da biste saznali dodatne informacije o korištenju balancers Interna učitavanja ili redove, pročitajte [Interna raspoređivača opterećenja pregled](../load-balancer/load-balancer-internal-overview.md) i [Azure redovima i usluge Bus redovi – usporedbi i iz Outlooka nasuprot](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

__Što se događa ako ne koristite opterećenja ili red između svakog sloju aplikacije?__ Bez opterećenja ili red, između svakog sloju aplikacije je teško skaliranje aplikacija prema gore ili prema dolje i njegov učitavanja raspodijelite više računala. Ne taj postupak može dovesti do iznad ili ispod dodjeljivanje resursa i rizik od nedostupnost ili nisku korisnički doživljaj ako imate neočekivane promjene u promet ili sustava.
 
###<a name="are-your-sql-databases-using-active-geo-replication"></a>Koristite li baza podataka za SQL aktivni replikacije zemlj.? 
Aktivni replikacije zemlj omogućuje vam da biste konfigurirali do 4 čitljiv sekundarne baze podataka u istom ili drugačije regijama. Sekundarni baza podataka dostupnih slučaju servisa prekidu ili Nemogućnost povezivanja s primarnom bazom podataka. Ako želite saznati više o SQL replikacije baze podataka active zemlj. –, pročitajte [Pregled: SQL replikacije baze podataka Active zemlj.-](../sql-database/sql-database-geo-replication-overview.md).
 
 __Što se događa ako ne koristite active replikacije zemlj s bazama podataka programa SQL?__ Bez aktivni zemlj. – replikacije, ako bazu podataka primarni ikad dolazi izvanmrežne (planiranog održavanja servisa prekidu, hardver pogreška, itd.) aplikacije baze podataka bit će Izvanmrežna dok dobar stanja možete unijeti na primarnom bazom podataka mrežni način. 
 
###<a name="are-you-using-a-cache-azure-redis-cache-in-front-of-your-databases"></a>Koristite predmemorije (Azure Redis predmemorije) ispred baze podataka?
Ako je vaša aplikacija opterećenjem visoke baze podataka gdje se najčešće poziva baze podataka čitanja, možete povećati brzinu aplikacije i smanjivanje opterećenje na bazu podataka implementacijom predmemoriranja sloj ispred offload te operacije čitanja bazu podataka. Možete povećati brzinu aplikacije i smanjivanje vaše učitavanja baze podataka (dakle skale to možete učiniti za povećavanje) potvrđivanjem predmemoriranja sloj ispred bazu podataka. Ako želite da biste saznali više o predmemoriju Azure Redis, pročitajte [upute Međuspremanje](../best-practices-caching.md).
 
 __Što se događa ako ne koristite predmemoriju ispred bazu podataka?__ Ako je vaše računalo baze podataka dovoljno Napredna učiniti opterećenje promet stavite na ga, a zatim aplikacija će odgovoriti kao normalne, iako to može značiti da na donjem opterećenja koji će biti plaćanja za računalo za baze podataka koji je više od potrebne. Ako računalo baze podataka nije Napredna dovoljno učiniti vaše opterećenja, a zatim će početi nisku korisničko sučelje doći do s aplikacijom (Latencija, vremenska ograničenja i vjerojatno nedostupnost servisa).
 
###<a name="have-you-contacted-microsoft-azure-support-if-you-are-expecting-a-high-scale-event"></a>Imate kojoj ste se obratili Microsoftovoj službi za podršku Azure ako očekujete visoke skaliranje događaja?
Podrška za Azure mogu pomoći u povećanju ograničenja servisa baviti planiranog velikog prometa događaja (kao što je novi pokreće proizvod ili posebne praznike). Podrška za Azure i moći povezivanje s experts koji možete pomoći Pregledajte dizajn s timom računa i olakšavaju pronaći najbolje rješenje svojim potrebama visoke skaliranje događaj. Ako želite da biste saznali dodatne informacije o tome kako obratite se podršci za Azure, pročitajte [Najčešća pitanja podržava Azure](https://azure.microsoft.com/support/faq/).

__Što se događa ako se ne obratite podršci za Azure za događaj visoke skaliranje?__ Ako ne komunikaciju ili planiranje, u događaj velikog prometa koji rizikom odlazak određene [ograničenjima servisa Azure](../azure-subscription-service-limits.md) i stoga stvaranje nisku korisničko sučelje (ili lošiji, nedostupnost) tijekom događaja. Arhitektonski preglede i komunikaciju na izboja olakšavaju prevladavanje tih rizika.

###<a name="are-you-using-a-content-delivery-network-azure-cdn-in-front-of-your-web-facing-storage-blobs-and-static-assets"></a>Koristite sadržaja mreže za isporuku (Azure CDN) ispred web-mjesto za pohranu blob-ova i statične imovine?
Pomoću ustvari CDN omogućuje da iskoristite učitavanja isključivanje poslužitelja tako da predmemoriranje sadržaj na POP rub CDN mjesta koje se nalaze diljem svijeta. To možete učiniti Smanji Latencija, povećajte skalabilnost, smanjite opterećenje poslužitelja i kao dio strategije zaštite od uskraćivanje napada service(DOS). Ako želite da biste saznali dodatne informacije o korištenju Azure CDN za vaše otpornost Povećavanje i smanjivanje vaše Latencija klijenta, pročitajte [Pregled od na Azure sadržaja isporuke mreže (CDN)](../cdn/cdn-overview.md).

__Što se događa ako ne koristite ustvari CDN?__ Ako ne koristite ustvari CDN pa sve korisnike prometa dolazi izravno na resurse. To znači da će vidjeti veći opterećenje na poslužitelje koji smanjuje njihove skalabilnost. Uz to, korisnici će možda primijetiti veći latencies kao CDN-ovi nude mjesta diljem svijeta koji su vjerojatno bliže klijentima.

##<a name="next-steps"></a>Sljedeće korake:
Ako želite da biste pročitali više o tome kako dizajnirati aplikacija za visoke dostupnosti, pročitajte [visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](resiliency-high-availability-azure-applications.md).
