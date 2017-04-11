<properties
   pageTitle="Pregled sigurnosti Azure mreže | Microsoft Azure"
   description=" U ovom se članku olakšava razumijevanje što Microsoft Azure sadrži nudi u području mrežne sigurnosti. Dajemo osnovni objašnjenja za osnovni koncepti sigurnosti za mreže i preduvjeti i informacije na što se Azure sadrži nudi u svakoj od tih područja. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Pregled sigurnosti Azure mreže

Microsoft Azure obuhvaća robusne mrežne infrastrukture za podršku aplikacije i servisni zahtjevi za povezivanje. Veza s mrežom je moguće između resursa koji se nalazi u Azure, između lokalnog i Azure hostira resursa, a i s Interneta i Azure.

Cilj ovog članka je olakšati razumijevanje što Microsoft Azure sadrži nudi u području mrežne sigurnosti. Ovdje dajemo osnovni objašnjenja za osnovni koncepti sigurnosti za mreže i preduvjeti. Također dobivate informacije na što se Azure sadrži nudi u svakoj od tih područja. Postoje brojne veze na drugi sadržaj koji će vam omogućiti da se detaljnije objašnjenje za područja u kojem koji vas zanima.

U ovom se članku Pregled sigurnosti mreže Azure usredotočite se na sljedeće:

- Azure s mrežom
- Kontrola pristupa na mreži
- Sigurne daljinski pristup i više lokacija povezivanje
- Dostupnost
- Bilježenje u zapisnik
- Razlučivanje naziva
- DMZ arhitekture
- Centar za sigurnost Azure

## <a name="azure-networking"></a>Azure s mrežom

Virtualnim strojevima potreban vam je veza s mrežom. Da biste podržavaju taj zahtjev, Azure mora imati virtualnim strojevima biti povezani s mrežom virtualne Azure. Azure virtualne mreže je konstrukta logičke on nudi tkanina fizičke Azure mreže. Svaki logičke Azure virtualne mreže je Izolirani s sve druge Azure virtualne mreže. Na taj način osigurali mrežni promet na vaše implementacije nije dostupna u drugim klijentima Microsoft Azure.

uči više:

- [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Kontrola pristupa na mreži
Kontrola pristupa na mreži je čin ograničavanje povezivanje i s određenim uređajima ili podmreže unutar Azure virtualne mreže. Cilj kontrola pristupa na mreži je da biste bili sigurni da su dostupna samo korisnicima i uređajima na koji želite im pristupiti virtualnih računala i servisa. Pristup kontrolama se temelje na dopustiti ili zabraniti odluke vezane uz veze i s virtualnog računala ili servisa.

Azure podržava nekoliko vrsta kontrola pristupa na mreži. To obuhvaća:

- Kontrola sloja mreže
- Usmjeravanje kontrole i prisilno tuneliranja
- Aparata sigurnost virtualne mreže

### <a name="network-layer-control"></a>Kontrola sloja mreže
Sve implementacije sigurno zahtijeva neke mjere kontrola pristupa na mreži. Cilj kontrola pristupa na mreži je da biste bili sigurni da virtualnim strojevima i mrežnih usluga koji se izvode na te virtualnim strojevima komuniciraju samo s drugim uređajima umreženi koje su im potrebne za komunikaciju s, a sve druge pokušaja povezivanja blokiraju.

Ako vam je potrebna kontrola razinu pristupa za osnovne mreže (koja se temelji na IP adresa i protokoli TCP i UDP), možete koristiti mreže sigurnosne grupe. Mrežni grupe sigurnosti (NSG) je osnovni s praćenjem stanja paketa Vatrozid za filtriranje i omogućuje kontrolu pristupa na temelju [5-n-torke](https://www.techopedia.com/definition/28190/5-tuple). NSGs podijelite provjere sloja aplikacije ili je autentičnost pristup kontrolama.

uči više:

- [Mrežni sigurnosne grupe](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Usmjeravanje kontrole i prisilne tuneliranje
Mogućnost da biste odredili usmjeravanje ponašanje na virtualne mreže Azure je ključnih mreže sigurnošću i pristupom kontrola mogućnost. Ako usmjeravanje neispravno konfiguriran, aplikacija i usluga virtualnog računala mogu povezati s uređaja ne želite povezati, uključujući uređaje posjeduje i njihovo potencijalne Napadači.

Azure umrežavanje podržava mogućnost da biste prilagodili usmjeravanje ponašanje za mrežni promet na Azure virtualne mreže. To vam omogućuje da promijenite zadani usmjeravanje tablice stavke u Azure virtualne mreže. Kontrola usmjeravanje ponašanje pridonosi li sve promet na uređaj ili grupu uređaja unosi ostavlja virtualne mreže Azure kroz na određeno mjesto.

Na primjer, možda uređaj za sigurnost virtualne mreže na mreži virtualne Azure. Želite provjeriti je li sav promet i s mrežom virtualne Azure prolazi potražite taj virtualne sigurnost. To možete učiniti konfiguriranjem [Korisnički definirana usmjerava](../virtual-network/virtual-networks-udr-overview.md) u Azure.

[Prisilno tuneliranje](https://www.petri.com/azure-forced-tunneling) je mehanizam možete koristiti da biste bili sigurni da servisa nije dopušteno da biste započeli vezu s uređajima na Internetu. Imajte na umu da se razlikuje od prihvaćanje dolazne veze, a zatim odgovaranje na njih. Pristupnim web-poslužiteljima potrebno odgovoriti na zahtjev iz Internet domaćini i tako Internet – izvorni termini promet je dopušteno dolazni te web-poslužiteljima i web-poslužiteljima dopušteno odgovoriti.

Što ne želite dopustiti je sučelja web-poslužitelju da pošalju zahtjev za izlazni. Takve zahtjeve mogu predstavljati sigurnosni rizik jer te veze ne može se koristiti za preuzimanje zlonamjernog softvera. Čak i ako želite da se ove sučelja da biste započeli izlazne zahtjeve s Internetom, možda ćete morati tjerajte ih da bude obrađen vaše lokalne web proxyji tako da možete iskoristiti prednost URL filtriranje i bilježenje u zapisnik.

Umjesto toga bi koji želite koristiti prisilne tuneliranje da biste to spriječili. Kada omogućite prisilne tuneliranje, sve veze s Internetom su prisilno putem vaše lokalne pristupnika. Možete konfigurirati prisilne tuneliranje prihvaćanjem prednost usmjerava definirane korisniku.

uči više:

- [Što su korisnički definirana usmjerava i prosljeđivanje IP](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Aparata sigurnost virtualne mreže
Dok mreže sigurnosne grupe, korisnički definirana usmjerava i prisilne tuneliranje omogućuju razinu sigurnosti na mrežu i prijenosa Slojevi [modela upravljački](https://en.wikipedia.org/wiki/OSI_model), može biti i vrijeme kada želite omogućiti sigurnosti na razini veće od mreže.

Na primjer, mogu sadržavati sigurnosne preduvjete:

- Provjere autentičnosti i autorizacije prije dopuštanja pristupa aplikacije
- Otkrivanje podataka i podataka odgovor
- Aplikacija sloj provjere za više razine protokola
- Filtriranje URL-ova
- Mrežni razine antivirusnog softvera i antimalware
- Zaštita protiv robot
- Kontrola pristupa aplikacije
- Dodatnu zaštitu DDoS (iznad DDoS zaštitu dobili Azure tkanina sam)

Ove značajke sigurnosti poboljšanu mrežnu možete pristupiti pomoću rješenja programa Azure partnera. Najnovije Azure partnera možete pronaći rješenja za sigurnost mreže posjeta [Azure Marketplace](https://azure.microsoft.com/marketplace/) i traženje "Sigurnost" i "mrežne sigurnosti".

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Sigurna daljinski pristup i povezivanje unakrsni lokalno
Postavljanje, konfiguriranje i upravljanje vašim potrebama Azure resursi za daljinsko gotovo. Osim toga, možda želite implementirati [hibridnog IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) rješenja koja ste komponente lokalnog i u oblaku Azure javno. Sljedećim scenarijima zahtijeva sigurnu daljinski pristup.

Azure umrežavanje podržava sljedeće scenarije sigurne daljinski pristup:

- Povezivanje pojedinačne radne stanice Azure virtualne mreže
- Povezivanje lokalne mreže Azure virtualne mreže s VPN
- Povezivanje lokalne mreže Azure virtualne mreže s namjenski WAN veze
- Međusobno povezivanje Azure virtualne mreže

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Povezivanje pojedinačne radne stanice Azure virtualne mreže
Mogu postojati vrijeme kada želite omogućiti pojedinačne osoblje razvojnim inženjerima ili operacije da biste upravljali virtualnih računala i servisa u Azure. Na primjer, morate pristupiti poštanskim virtualnog računala na mreži virtualne Azure i sigurnosna pravila ne dopušta RDP ili SSH daljinski pristup pojedinačne virtualnih računala. U tom slučaju možete koristiti VPN veza točke na web.

VPN veza točke web koristi protokol [SSTP VPN-a](https://technet.microsoft.com/library/cc731352.aspx) koji omogućuje postavljanje privatnosti i sigurnosti veze između korisnika i Azure virtualne mreže. Kada je uspostavljena VPN veza, korisnik će moći RDP ili SSH putem VPN vezu u bilo kojem virtualnog računala na mreži virtualne Azure (uz pretpostavku da korisnik može provjeru autentičnosti i ovlašten).

uči više:

- [Konfiguriranje veze točke web virtualne mreže pomoću komponente PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Povežite lokalne mreže Azure virtualne mrežom Zahvaljujući VPN-u
Preporučujemo vam da biste se povezali cijelu korporacijskom mrežom ili dijelove, Azure virtualne mreže. To nije zajednička hibridnog IT scenariji kojima tvrtke [Proširivanje njihove lokalnog podatkovnog centra u Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). U mnogim slučajevima tvrtke će hostira dijelovi servisa Azure i dijelove lokalnom sustavu, kao što su kada rješenje sadrži pristupnim web-poslužiteljima u Azure i pozadinske baze podataka na lokalni. Ove vrste veze "-lokacija" Provjerite je i upravljanje Azure nalazi resurse više sigurne i omogućivanje scenariji kao što su proširivanje kontrolera domene servisa Active Directory u Azure.

Da biste to postigli je koristiti [VPN-a web-mjesto](https://www.techopedia.com/definition/30747/site-to-site-vpn). Razlika između web-mjesto VPN-a i zareza web VPN-a je da točke web VPN-a povezuje jedan uređaj s mrežom virtualne Azure, dok VPN-a web-mjesto povezuje čitava mreža (primjerice lokalne mreže) Azure virtualne mreže. Web-mjesto VPN-ovi za Azure virtualne mreže pomoću iznimno sigurnom načinu IPsec tunelom protokol VPN-a.

uči više:

- [Stvaranje resursima VNet s vezom za VPN-a web-mjesto pomoću portala za Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Planiranje i dizajniranje za pristupnik za VPN-a](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Lokalne mreže povezati Azure virtualne mreže s namjenski WAN veze
Točke na mjesta i web-mjesto VPN veze su učinkoviti za omogućivanje više lokacija povezivanje. Međutim, neke tvrtke i ustanove Imajte na umu da su sljedeće Nedostaci:

- VPN veza premještanje podataka s Interneta – to izlaže ove veze da biste potencijalnih sigurnosnih problema vezanih uz premještanju podataka putem javne mreže. Osim toga, pouzdanosti i dostupnost internetske veze ne može se zajamčiti.
- VPN veza Azure virtualne mreže mogu smatrati propusnosti ograničeno za neke aplikacije i svrhe, čim Maks odgovor na oko 200Mbps.

Tvrtke i ustanove najvišu razinu sigurnosti i dostupnost za svoje više lokacija veze obično namjenski WAN veze pomoću kojih možete povezati s udaljenog mjesta. Azure nudi mogućnost korištenja namjenski WAN veze koje možete koristiti da biste se povezali lokalne mreže za Azure virtualne mreže. Omogućena je kroz Azure ExpressRoute.

uči više:

- [ExpressRoute Tehnički pregled](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Međusobno povezivanje Azure virtualne mreže
Moguće je za korištenje mnogo Azure virtualne mreže za vaše implementacije. Postoji mnogo razloga zašto to možete napraviti. Jedan od razloga može biti da biste pojednostavnili upravljanja; drugi mogu sigurnosnih vam razloga. Bez obzira na motivation ili rationale stavljanja resursa na različitim Azure virtualne mreže mogu postojati vrijeme kada želite da se Resursi za svaku od mreža za međusobno povezivanje.

Jedna od mogućnosti bio za usluge na Azure virtualne mreža za povezivanje sa servisa na drugom Azure virtualne mreže putem "Ponavljanje natrag" putem Interneta. Veze bi pokretanja Azure virtualne mreža, otvorite putem Interneta, a zatim vratite se na odredište Azure virtualne mreže. Ta mogućnost izlaže veza za sve komunikacije internetske ugrađeno sigurnosnim problemima.

Da biste stvorili programa Azure virtualne mreže Azure virtualne mreže web-mjesto VPN-a mogu se bolje. Ovaj Azure virtualne mreže Azure virtualne mreže web-mjesto VPN-a koristi protokol isti [način tunelom IPsec](https://technet.microsoft.com/library/cc786385.aspx) kao vezu VPN-a web-mjesta na web-lokacija spomenutih.

Prednost korištenja programa Azure virtualne mreže Azure virtualne mreže web-mjesto VPN-a je putem tkanina Azure mreže; uspostavljanja VPN veze povezivanje putem Interneta. Ovo vam omogućuje dodatni slojeva zaštite u usporedbi s web-mjesto VPN-ovi koji se povezuju putem Interneta.

uči više:

- [Konfiguriranje veze VNet VNet pomoću upravitelja resursa Azure i komponente PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Dostupnost
Dostupnost je ključa komponenta bilo kojeg programa za sigurnost. Ako korisnici i sustavi ne možete pristupiti koje su im potrebne za pristup putem mreže, možete se smatra servis ugrožena. Azure sadrži mrežne tehnologije koji podržavaju sljedeće mehanizme visoku dostupnost:

- Za ujednačavanje opterećenja koji se temelji na HTTP
- Mreže za ujednačavanje razine opterećenja
- Globalni za ujednačavanje opterećenja

Opterećenja je mehanizam namijenjenu ravnomjerno distribucija veze između više uređaja. Ciljeva opterećenja su:

- Povećavanje raspoloživosti – kada učitavanje saldo veze na više uređaja, neke uređaje mogu postati nedostupne i servise preostale mrežne uređaje možete se i dalje koristi sadržaj iz servisa
- Poboljšati performanse – kad učitate saldo veze na više uređaja, jedan uređaj ne morati poduzeti uspješnosti procesor. Umjesto toga, obrada i memorija zahtjeva za posluživanje sadržaj je šire preko više uređaja.

### <a name="http-based-load-balancing"></a>Za ujednačavanje opterećenja koji se temelji na HTTP
Tvrtke i ustanove namijenjeni koji se temelji na web servisa često žele da bi se utemeljen na HTTP opterećenja ispred te web-servisi omogućuju osigurali odgovarajuće razine performanse i visoke dostupnosti. Za razliku od balancers tradicionalni utemeljen na mreži učitavanja odluke načinio HTTP-poštu za ujednačavanje opterećenja učitavanja balancers temelje se na karakteristike HTTP protokola, ne i na sloj protokoli za mreže i prijenosa.

Da bi vam za koji se temelji na web servisa za ujednačavanje opterećenja koji se temelji na HTTP, Azure omogućuje pristupnika Azure aplikacije. Pristupnik za aplikaciju Azure podržava:

- Koji se temelji na HTTP uravnoteženje opterećenja – odluke za ujednačavanje opterećenja vrše utemeljene na karakteristikama posebno protokola HTTP
- Afinitet utemeljen na kolačića sesije – tu mogućnost jamči ostaje li veze uspostaviti na neki od poslužitelje iza tog opterećenja ostaje netaknuta između klijenta i poslužitelja. To insures stabilnosti transakcije.
- SSL rasterećivanje – kada klijent je uspostaviti vezu s raspoređivača opterećenja da je sesiju između klijenta i raspoređivača opterećenja šifriran pomoću na HTTP (SSL /) protokol. Međutim, da biste poboljšali performanse, imate mogućnost da bi veze između raspoređivača opterećenja i web-poslužitelj iza korištenje raspoređivača opterećenja protokol HTTP (nešifrirana). To se naziva kao "SSL offload" jer web-poslužiteljima iza raspoređivača opterećenja ne pojave indirektnog procesor vezanih uz šifriranja i stoga trebali biste moći zahtjevima za uslugu brže.
- Koji se temelji na URL usmjeravanja sadržaja – Ova značajka omogućuje opterećenja za donošenje odluka gdje se prosljeđivanje veza na temelju odredišni URL. To omogućuje znatno veću fleksibilnost od rješenja koje učitajte ujednačavanje odluka utemeljenih na IP adrese.

uči više:

- [Pregled aplikacija pristupnika](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Mreže za ujednačavanje razine opterećenja
Za razliku od utemeljen na HTTP opterećenja, mrežnog razine opterećenja čini odluka ujednačavanje opterećenja utemeljenih na IP adresa i (TCP i UDP) broj priključka.
Možete dobiti prednosti ujednačavanja u Azure pomoću Azure raspoređivača opterećenja razine opterećenja mreže. Neke ključne karakteristike opterećenja Azure obuhvaćaju sljedeće:

- Mrežnog razine opterećenja na temelju IP adresa i broj priključka
- Podrška za bilo koju aplikaciju protokol sloja
- Učitavanje salda za Azure virtualnim strojevima i oblaka servisima instance uloga
- Moguće je koristiti za – mjesto na Internetu (vanjski opterećenja) i ne Internet nasuprotne aplikacije (ujednačavanja Interna opterećenja) i virtualnih računala
- Krajnja točka nadzor, koji se koristi za određivanje Ako bilo koji servis iza raspoređivača opterećenja imati više nije dostupan

uči više:

- [Internet nasuprotne raspoređivača opterećenja između više virtualnih računala ili servisa](../load-balancer/load-balancer-internet-overview.md)
- [Pregled opterećenja Interna učitavanja](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globalni za ujednačavanje opterećenja
Neke tvrtke i ustanove ćete najvišu razinu dostupnost moguće. Jednosmjerna dosegne taj cilj je aplikacija za glavno računalo u globalno raspodijeljeno podatkovnim centrima. Kada aplikacija nalazi se u podaci nalaze u cijelom svijetu, nije moguće za cijelo Geopolitički područje postaju nedostupne i još aplikacija prema gore i pokrenuta.

Osim prednosti dostupnost se po nalaze aplikacije u globalno raspodijeljeno podatkovnim centrima, također možete dobiti performansama. Sljedeće pogodnosti performanse mogu biti dobiven korištenjem mehanizam koji usmjerava zahtjeva za uslugu s podatkovnim centrom koji je nearest to uređaja koji je upućivanje zahtjeva.

Globalni opterećenja dat će vam i sljedeće pogodnosti. U Azure, možete dobiti prednosti ujednačavanja pomoću upravitelja promet Azure globalni opterećenja.

uči više:

- [Što je upravitelj promet?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Bilježenje u zapisnik
Zapisivanje na razini mreže je funkcija ključa za sve scenarij sigurnost mreže. U Azure, možete evidentirati informacije za mrežni sigurnosne grupe da biste dobili mreže razinu zapisivanja informacije. Uz zapisivanje NSG Dohvati informacije iz:

- Zapisnike nadzora – ti zapisnici koriste se da biste pogledali sve operacije koje se šalje na pretplate Azure. Ti zapisnici omogućena je prema zadanim postavkama i može se koristiti unutar Azure portal.
- Zapisnike događaja – te zapisnika nalaze informacije o zatvorene koje NSG pravila.
- Brojača zapisnika – ti zapisnici obavijesti koliko je puta svako pravilo NSG primijenjen zabranu ili dopušta promet.

[Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), alat napredne vizualizacije, možete koristiti i radi prikaza i analize te zapisnika.

uči više:

- [Zapisnik analize mreže sigurnosnih grupa (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Razlučivanje naziva
Razlučivanje naziva je ključnih funkcija za sve servise hostira u Azure. Iz perspektive sigurnost, narušena pouzdanost naziv funkcije razlučivost može dovesti do napadaču preusmjeravanja zahtjeve iz web-mjesta na web-mjesto programa napadaču. Razlučivost sigurne naziv je zahtjev za sve servise za oblak koji se nalazi.

Postoje dvije vrste razlučivanje naziva morate riješiti:

- Naziv internog razlučivost – naziv internog razlučivost koristi usluge na virtualne mreže Azure, vaše lokalne mreže ili oboje. Nazive koji se koriste za rješavanje naziv internog nisu dostupne putem Interneta. Radi optimalnog sigurnosti je važno shemu razlučivost naziv internog nije dostupna za vanjske korisnike.
- Naziv eksternog razlučivost – naziv vanjskog razlučivost koristi ljudima i uređajima izvan vaše lokalne i Azure virtualne mreže. To su nazivi koji su vidljive na Internet i koristi se za usmjeravanje veze sa servisa u oblaku.

Za naziv internog razlučivosti, imate dvije mogućnosti:

- Poslužitelj za Azure virtualne mreže DNS-a – prilikom stvaranja novog Azure virtualne mreže, DNS poslužitelj je stvoriti. DNS poslužitelj mogli biste riješiti naziva računala koja se nalazi na tom Azure virtualne mreže. DNS poslužitelj nije moguće konfigurirati i upravlja u upravitelju Azure tkanina zato što rješenje razlučivost sigurne naziv.
- Premjesti vlastite DNS poslužitelj – imate mogućnost umetanja DNS poslužitelj vlastite odabiru Azure virtualne mreže. DNS poslužitelj može biti Active Directory integriran DNS poslužitelj ili namjenski rješenje DNS poslužitelj nudi Azure partnera koje možete dobiti iz trgovine Azure Marketplace.

uči više:

- [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md)
- [Upravljanje DNS poslužitelji koriste virtualne mreže (VNet)](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Za vanjske Razrješavanje DNS, imate dvije mogućnosti:

- Glavno računalo vlastite vanjski DNS informacije o lokalnom poslužitelju
- Hostirate vlastite DNS poslužitelj kod davatelja usluge

Mnoge velikim tvrtkama i ustanovama će hostira vlastitih DNS poslužitelji lokalnog. To će moći učiniti jer su umrežavanje stručna znanja i globalni prisutnosti da biste to učinili.

U većini slučajeva je bolje za hostiranje DNS servise za naziv razlučivost kod davatelja usluge. Ove davateljima usluga imaju stručna znanja mreže i globalni prisutnosti da biste bili sigurni vrlo visoka dostupnost servisa razlučivost naziv. Dostupnost ključno je za DNS servise jer ako naziv razlučivost servisa ne uspije, nitko moći dosegne internetska nasuprotne services.

Azure omogućuje iznimno dostupne i performant vanjski DNS rješenja u obliku Azure DNS-a. Naziv vanjskog razlučivost rješenje vodi prednost diljem svijeta infrastrukture Azure DNS. Omogućuje za hostiranje vaše domene u Azure pomoću alata vjerodajnice, API-ji, i naplata kao drugih Azure servisa. Kao dio Azure, također nasljeđuje istaknuti sigurnosne kontrole ugrađenu platforme.

uči više:

- [Pregled Azure DNS-a](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ arhitekture
Mnoge organizacije enterprise pomoću DMZs fazi svoje mreže da biste stvorili u zonu međuspremnik između interneta i njihovih usluga. Dio DMZ mreže smatra najniža sigurnosne zone i bez imovine jakih spremaju se u toj segmenta mreže. Prikazat će se obično mrežne uređaje sigurnosti koji imaju mrežno sučelje na DMZ segment i drugog mrežnog sučelja povezani s mrežom na virtualnim računalima i servise koji su prihvatili unutarnje veze s Interneta.

Mnoga varijacija DMZ dizajna i odluku da želite implementirati DMZ, a zatim koju vrstu DMZ koristite ako odlučite koristiti onaj koji se temelji se na na mreži sigurnosne preduvjete koje je.

uči više:

- [Web-mjesto servisa Microsoft Cloud i u okvir za mrežne sigurnosti](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Centar za sigurnost Azure
Centar za sigurnost pomaže spriječiti, otkrivanje i odgovaranje na prijetnji, a omogućuje povećati uvid u, i kontrolu nad, sigurnost Azure resurse. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate Azure, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

Centar za sigurnost Azure pomaže vam optimiziranje i nadzirati sigurnost mreže:

- Pružanje preporuke o sigurnosti mreže
- Praćenje stanja mrežna konfiguracija sigurnosti
- Koja vas upozorava na mreža temeljena prijetnji oba na razinama krajnjoj točki i mreže

uči više:

- [Uvod u Centar za sigurnost Azure](../security-center/security-center-intro.md)
