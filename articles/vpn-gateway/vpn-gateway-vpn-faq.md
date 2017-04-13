<properties 
   pageTitle="Virtualne mreže najčešća pitanja vezana uz VPN pristupnika | Microsoft Azure"
   description="Pristupnik VPN najčešća Pitanja. Najčešća pitanja o Aplikaciji Microsoft Azure virtualne mreže-lokacija veze, veze za konfiguraciju hibridnog i pristupnika VPN-a"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>Najčešća pitanja vezana uz pristupnik za VPN-a

## <a name="connecting-to-virtual-networks"></a>Povezivanje s virtualne mreže

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Može li se povezati virtualne mreže u različitim područjima Azure?
Da. Zapravo, postoji nema regija ograničenja. Virtualna mreža možete povezati s drugom virtualne mreže na istom području ili u nekoj drugoj regiji Azure.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Može li se povezati virtualne mreže u različite pretplate?
Da.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Možete povezati s više web-mjesta s jedne virtualne mreže?

Možete se povezati s više web-mjesta pomoću komponente Windows PowerShell i Azure REST API-ji. U odjeljku [više web-mjesta i povezivanje VNet VNet](#multi-site-and-vnet-to-vnet-connectivity) najčešća Pitanja.
## <a name="what-are-my-cross-premises-connection-options"></a>Što mogu učiniti više lokacija veze?

Podržane su sljedeće više lokacija veze:

- [Web-mjesto](vpn-gateway-site-to-site-create.md) – VPN vezu putem IPsec (IKE v1 i IKE v2). Tu vrstu veze zahtijeva VPN uređaja ili RRAS.

- [Točke-na-web-mjesta](vpn-gateway-point-to-site-create.md) – VPN vezu putem SSTP (Secure Socket protokol preusmjeravanja). Ove veze ne zahtijeva VPN uređaja.

- [VNet VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – tu vrstu veze je isti kao web-mjesto konfiguracije. VNet za VNet je VPN veza putem IPsec (IKE v1 i IKE v2). Ne zahtijeva VPN uređaja.

- [Više web-mjesta](vpn-gateway-multi-site.md) – to je varijacije konfiguracije web-mjesto koji omogućuje povezivanje više lokalnog web-mjesta s virtualne mreže.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute je izravne veze s Azure s vašeg WAN, a ne putem javnog Interneta. Pogledajte [Tehnički pregled ExpressRoute](../expressroute/expressroute-introduction.md) i [Najčešća pitanja vezana uz ExpressRoute](../expressroute/expressroute-faqs.md) za dodatne informacije.

Dodatne informacije o vezama potražite u članku [O pristupnika VPN-a](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Koja je razlika između veze s web-mjesto i zareza mjesta?

Veze na **Web-mjesto** omogućuju povezivanje između bilo kojeg računala koja se nalazi lokalno na bilo koje virtualnog računala ili instancu uloga u mreži virtualne, ovisno o tome kako odabrati da biste konfigurirali usmjeravanja. Videobilješki vrlo je korisno za uvijek dostupno više lokalnu vezu, a je dobro prikladniji hibridne konfiguracije. Tu vrstu veze ovisi o uređaj IPsec VPN-a (hardver ili meki potražite), koji se moraju biti implementirano na rubu mreže. Da biste stvorili tu vrstu veze, morate imati potrebne hardverske VPN-a i vanjsko dostupnog IPv4 adresa.

**Točke-na-web-mjesta** veze omogućuju vam da se povežete s jednog računala s bilo kojeg mjesta sa sadržajem koja se nalazi u virtualne mreže. Koristi Windows u okviru VPN klijent. Kao dio konfiguracije točke na web, instalirajte certifikat i VPN klijentskog konfiguracije paket, koja sadrži postavke koje omogućuju računalu na koje želite povezati s bilo kojeg virtualnog računala ili uloga instance unutar virtualne mreže. Sjajan je kada želite povezati virtualne mreže, ali ne nalazi na lokalni. Preporučuje se i dobar izbor kada nemate pristup VPN hardver ili vanjsko dostupnog IPv4 adresa, oba koji su potrebni za povezivanje web-mjesto. 

Virtualne mrežu da biste istodobno, koristite web-mjesto i zareza web možete konfigurirati pod uvjetom da se stvoriti web-mjesto povezivanje s VPN vrstu usmjeravanje temelje za pristupnikom. Utemeljene na usmjeravanje vrste VPN nazivaju pristupnika dinamički u modelu klasični implementacije.

### <a name="what-is-expressroute"></a>Što je ExpressRoute?

ExpressRoute omogućuje stvaranje privatne veze između podatkovnim centrima Microsoft i infrastrukture lokalno ili u okruženju zajednički mjesto. S ExpressRoute, uspostavljanje veze s Microsoftovim servisima u oblaku kao što je Microsoft Azure i Office 365 u funkciji za suautorstvo mjesto partnera programa ExpressRoute ili izravno povezivanje s mrežom postojeće WAN (kao što su MPLS VPN koji se koje ste dobili od davatelja usluga mreže).

ExpressRoute veze nude veću sigurnost, više pouzdanosti, veću propusnost i donjem latencies od standardne veze putem Interneta. U nekim slučajevima pomoću veza ExpressRoute za prijenos podataka između lokalne mreže i Azure možete i yield pogodnosti značajnu trošak. Ako već ste stvorili više lokacija veze iz lokalne mreže za Azure, možete migrirati je veza ExpressRoute uz zadržavanje virtualne mreže ostaje netaknuta.

[Najčešća pitanja vezana uz ExpressRoute](../expressroute/expressroute-faqs.md) dodatne pojedinosti potražite u članku.

## <a name="site-to-site-connections-and-vpn-devices"></a>Veza web-mjesto i uređaje VPN-a

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Što treba uzeti u obzir pri odabiru VPN uređaja?

Ne možemo ste provjeriti valjanost skup standardne uređaja VPN-a web-mjesto u partnerstvo s dobavljačima uređaja. Popis poznatih kompatibilnim uređajima VPN-a, njihove odgovarajuće upute za konfiguriranje ili uzorka i specifikacija uređaja možete pronaći [u nastavku](vpn-gateway-about-vpn-devices.md). Sve uređaje u linije uređaja koji se prikazuju u obliku poznati kompatibilan s surađivati s virtualne mreže. Da biste konfigurirali uređaju VPN, priručniku uzorka Konfiguracija uređaja ili vezu koja odgovara liniju odgovarajući uređaj.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Što učiniti ako imam VPN uređaj koji nije na popisu poznati kompatibilan s uređaja?

Ako ne vidite svoj uređaj naveden kao poznati kompatibilne VPN uređaj, a želite koristiti za VPN vezu, morat ćete provjerite ispunjava li na podržani IPsec-IKE konfiguracija mogućnosti i parametre navedene [ovdje](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list). Uređaji minimalni preduvjeti za sastanak surađivati s VPN pristupnika. Dodatne upute za podršku i konfiguraciji zatražite od proizvođača uređaja.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Zašto ne Moje VPN tunelom pravila utemeljen prijeći promet je u stanju mirovanja?

Ovo je očekivano za pravila utemeljen (poznat i kao statične usmjeravanje) VPN pristupnika. Kada je promet putem na tunelom neaktivnosti za više od pet minuta, u tunelom će biti raskinut. Prilikom pokretanja slijedi u bilo kojem smjeru promet na tunelom će se ponovo uspostaviti odmah.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Mogu li koristiti softver VPN-ovi za povezivanje s Azure?

Za web-mjesta na web-lokacija konfiguraciju podržavamo Windows Server 2012 usmjeravanje i daljinski pristup (RRAS) poslužitelja.

Druga rješenja VPN softver surađivati s oglednim pristupnika pod uvjetom da se u skladu sa industrijskih standardne IPsec implementacije. Upute za konfiguriranje i podršku zatražite od dobavljača softvera.

## <a name="point-to-site-connections"></a>Točka web veze

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Koje operacijske sustave možete koristiti s točke mjesta?

Podržani su operacijski sustavi u nastavku:

- Windows 7 (32-bitni i 64-bitni)

- Windows Server 2008 R2 (64-bitni samo)

- Windows 8 (32-bitni i 64-bitni)

- Windows 8.1 (32-bitni i 64-bitni)

- Windows Server 2012 (64-bitni samo)

- Windows Server 2012 R2 (64-bitni samo)

- Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Možete koristiti bilo koji klijentski softver VPN-a za točke-na-mjesto koje podržava SSTP?

ne. Ograničeno samo na verzija operacijskog sustava Windows naveden je podrška.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Koliko VPN klijentskog krajnje točke mogu imati u konfiguraciji Moje točke mjesta?

Podržavamo 128 klijenti VPN-a da biste se mogli povezati virtualne mreže u isto vrijeme.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Mogu li koristiti vlastitu Interna PKI korijenski CA za povezivanje točke web?

Da. Prethodno, samo samopotpisani korijenskih certifikata može koristiti. I dalje možete prenijeti 20 korijenskih certifikata.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Li prolaziti poslužitelji i vatrozidi pomoću mogućnost točke web?

Da. Da biste tunelom kroz vatrozid koristimo SSTP (Secure Socket protokol preusmjeravanja). U ovom tunelom prikazat će se kao vezu s HTTPs.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Ako se ponovno pokrenite klijentsko računalo konfigurirana za web točke, će VPN-om automatski ponovno povezati?

Prema zadanim postavkama klijentsko računalo će ne ponovno uspostaviti VPN veza automatski.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Ne podršku za točke web automatski-ponovno povezati i DDNS na klijente VPN-a?

Automatski povežite i DDNS trenutno nisu podržane u VPN-ovi zareza web.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Mogu li web-mjesto i konfiguracija točke web supostojanje za istu virtualne mreže?

Da. Ako imate vrstu RouteBased VPN-a za pristupnikom funkcionirat će obiju situacija. Model klasični implementacije morate dinamički pristupnika. Moramo ne podršku točke-da biste – web-mjesta za statične usmjeravanje VPN pristupnika ili pristupnika pomoću - VpnType PolicyBased.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Ću konfigurirati klijenta točke web povezati s više mreža virtualne u isto vrijeme?

Da, moguće je. Ali virtualne mreže ne smije sadržavati IP prefiksi koji se preklapaju i razmake adresu točke na web morate preklapaju između virtualne mreže.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Koliko je propusnost možete očekivati putem veza web-mjesto ili točke na mjesto?

Je teško da biste zadržali točne propusnost tunnels VPN-a. IPsec i SSTP su protokoli šifriranu Tamni VPN-a. Propusnost i ograničen Latencija i propusnosti između vaši lokalno i Interneta.

## <a name="gateways"></a>Pristupnika

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Što je pristupnika pravila utemeljen (statičnu-usmjeravanje)?

Pravila utemeljen pristupnika implementirati pravila utemeljen VPN-ovima. Pravila utemeljen VPN-ovima šifriranje i izravno pakete kroz IPsec tunnels na temelju kombinacije adresu prefiksi između lokalne mreže i Azure VNet. Pravila (ili odabir promet) obično se definira kao popisa pristupa u konfiguraciji VPN-a.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Što je pristupnika utemeljen na usmjeravanje (dinamičkih-usmjeravanje)?

Usmjeravanje sustavom pristupnika implementirati VPN-ovi koji se temelji na usmjeravanje. Usmjeravanje sustavom VPN-ovi u IP prosljeđivanje ili postupak tablice da biste izravno pakete u svoje odgovarajuće tunelom sučelja koristiti "usmjerava". Sučelja tunelom zatim šifriranje ili dešifriranje pakete i na tunnels. Birač pravila ili promet za usmjeravanje temelji VPN-ovi su konfigurirana kao bilo koje-na-bilo koje (ili koji kartica).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Mogu li dobiti Moja IP adresa pristupnika VPN-a prije no što ga stvoriti?

ne. Morate stvoriti pristupnikom da biste dobili IP adresu. Ako izbrišete i ponovno stvaranje pristupnika za VPN-a, mijenja se s IP adresom.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Kako Moje tunelom VPN-a ne dobiti autentičnost?

Azure VPN koristi provjeru autentičnosti PSK (Pre-Shared ključ). Ne možemo generira zajednički ključ (PSK) kada ćemo stvoriti tunelom VPN-a. Automatski generirani PSK možete promijeniti u vlastiti pomoću cmdleta komponente PowerShell postavite ključ Pre-Shared ili REST API-JA.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Mogu li koristiti API za postavljanje Pre-Shared ključ da biste konfigurirali sustavom Moje pravila (statične usmjeravanje) pristupnika VPN-a?

Da, postavite Pre-Shared ključ API-jem i PowerShell cmdlet može se koristiti za konfiguriranje Azure pravila utemeljen (statične) VPN-ovima i sustavom usmjeravanje (dinamički) usmjeravanje VPN-ovima.

### <a name="can-i-use-other-authentication-options"></a>Mogu li koristiti druge mogućnosti provjere autentičnosti?

Ne možemo ograničeni su na korištenje zajednički tipki (PSK) za provjeru autentičnosti.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Što je "podmreže pristupnika" i zašto je to potrebno?

Imamo pristupnika servisa koji ne možemo pokrenuti da biste omogućili povezivanje više lokacija. 

Morat ćete stvoriti podmreži pristupnik za svoje VNet konfigurirati pristupnik za VPN-a. Sve podmreže pristupnika moraju biti imenovane GatewaySubnet ispravno funkcionirao. Ne dodijelite naziv vašoj podmreži pristupnika nešto drugo. A ne uvesti VMs ili nešto drugo podmreže pristupnika.

Minimalna veličina podmreže pristupnika u cijelosti ovisi konfiguracije koji želite stvoriti. Iako je moguće da biste stvorili pristupnik podmreže što manje /29 za neke konfiguracije, preporučujemo stvorite podmreži pristupnika /28 ili veća (/ 28, /27, /26 itd.). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Možete implementirati virtualnim strojevima ili uloga instance Moje podmreže pristupnika?

ne.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Kako odrediti koje promet prolazi kroz pristupnika VPN-a?

Ako koristite klasični Portal za Azure, dodajte svaki raspon koji želite poslati putem pristupnika virtualne mreže na stranici mreža u odjeljku lokalne mreže.

### <a name="can-i-configure-forced-tunneling"></a>Ću konfigurirati prisilno tuneliranje?

Da. Potražite u članku [Konfiguriranje prisilno tuneliranje](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Možete postaviti vlastitu VPN poslužitelja u Azure i koristi za povezivanje s moju lokalnu mrežu?

Da, možete implementirati vlastite VPN pristupnika i poslužitelji u Azure ili iz trgovine Azure ili stvaranja vlastitog usmjerivača VPN-a. Morat ćete konfigurirati usmjerava korisnički definirano u virtualne mreže da biste bili sigurni pravilno usmjeriti promet između lokalnog mreža i vaše podmreže virtualne mreže.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Zašto se otvaraju određenih priključaka na moj pristupnika VPN-a?

To su potrebni za Azure infrastrukture komunikacije. Nisu zaštićeni (zaključao) Azure certifikata. Bez odgovarajuće certifikata vanjskih entiteti, uključujući klijentima te pristupnika nećete moći uzrokuju utječe na ta krajnje točke.

Pristupnik za VPN bitno je i navođeni na više uređaja s jednog NIC dodirom u privatne mreže klijenta i jedan NIC nasuprotne javnim mrežama. Infrastruktura za Azure entiteti nije moguće pristupite kupca privatne mreže usklađenost razloga tako da morate koristiti javno krajnje točke za komunikaciju infrastrukture. Javni krajnje točke povremeno skenirati Azure sigurnosnog nadzora.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Dodatne informacije o vrstama pristupnika, preduvjeti i propusnost

Dodatne informacije potražite u članku [O postavki za VPN pristupnika](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Više web-mjesta i povezivanje VNet VNet

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Koju vrstu pristupnika možete podržavaju više web-mjesta i povezivanje VNet VNet?

Samo usmjeravanje sustavom (dinamički usmjeravanje) VPN-ovima.

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Načini povezivanja s VNet s vrstom RouteBased VPN-a za neki drugi VNet s vrstom PolicyBased VPN-a?

Ne, oba virtualne mreže se mora koristiti utemeljen na usmjeravanje (dinamički usmjeravanje) VPN-ovima.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Sigurna promet VNet VNet?

Da, je zaštićena upravljanjem IPsec-IKE šifriranje.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Ne VNet VNet promet putuju preko Azure backbone?

Da.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Koliko lokalnog web-mjesta i virtualne mreže mreža virtualne povežite?

Max. 10 kombinirati za pristupnika osnovne i standardne dinamički usmjeravanje; 30 za visoke performanse VPN pristupnika.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Mogu li VPN-ovima korištenje točke-na-web-mjesta s virtualne mrežu s više tunnels VPN-a?

Da, VPN-ovi zareza-na-web-mjesta (P2S) može se koristiti s pristupnika VPN povezivanja s više lokalnog web-mjesta i druge virtualne mreže.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Ću konfigurirati više tunnels između virtualne mrežu i Moje lokalno mjesto putem VPN-a više web-mjesta?

Ne, suvišne tunnels između Azure virtualne mreže i lokalno mjesto nisu podržane.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Mogu li koji se preklapaju adresu razmaka između povezanog virtualne mreže i lokalne lokalna web-mjesta?

ne. Razmaci adresa koji se preklapaju uzrokovat će prijenosa datoteka konfiguracije mreže ili "Stvaranje virtualne mreže" nije uspjela.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Dobivam li više propusnosti s više web-mjesto VPN-ovima od za virtualne mreže?

Ne, sve tunnels VPN-a, uključujući VPN-ovi zareza web, zajedničko korištenje istom Pristupniku Azure VPN i raspoložive propusnosti.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Mogu li pomoću pristupnika Azure VPN prijenosa promet između Moja web-mjesta na lokalni ili nekom drugom virtualne mrežom?

**Model klasični implementacije**<br>
Promet prijenosa putem Azure VPN pristupnik je moguće pomoću modela klasični implementaciju, ali ovisi statički definirani adresu razmaka u konfiguracijskoj datoteci mreže. BGP još nije podržana s Azure virtualne mreže i VPN pristupnika pomoću klasične implementacije modela. Bez BGP, ručno definiranje prijenosa adresu razmake je vrlo pogreška podložni, a ne preporučuje.<br>
**Voditelj resursa model implementacije**<br>
Ako koristite model implementacije Voditelj resursa, u odjeljku [BGP](#bgp) dodatne informacije.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Ne Azure generiranje isti IPsec-IKE zajednički ključ za sve moje VPN veza za istu virtualne mreže?

Ne, Azure prema zadanim postavkama stvara različite zajednički tipke za različite VPN veza. Međutim, koristite cmdlet za postavljanje VPN pristupni ključ REST API-JA ili PowerShell da biste postavili vrijednost ključa radije. Ključ mora biti alphanumerical niz duljinu između 1 i 128 znakova.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Ne Azure naplatiti promet između virtualne mreže?

Za promet između različitih Azure virtualne mreže Azure naknada samo za promet traversing iz jednog Azure regije u drugu. Stopa trošak nalazi se na stranici Azure [Cijene za VPN pristupnika](https://azure.microsoft.com/pricing/details/vpn-gateway/) .


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Moguće istodobno povezati virtualne mreže s VPN-ovima IPsec moj sklopovske ExpressRoute?

Da, to nije podržano. Dodatne informacije potražite u članku [Konfiguriranje ExpressRoute i web-mjesto VPN veza koje supostojanje](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Povezivanje s više lokacija i VMs

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Ako je moje virtualnog računala u virtualne mreže i mogu imati više lokacija veze, trebali biste načini povezivanja s na VM?

Imate nekoliko mogućnosti. Ako imate RDP omogućeno i stvorite krajnje, možete se povezati s virtualnog računala pomoću na VIP. U tom slučaju želite navesti na VIP i priključka koji želite povezati. Morat ćete konfigurirati priključak na virtualnog računala za promet. Obično činite idite na Portal klasični Azure i spremanje postavki RDP veze na računalo. Postavke sadrže podatke potrebne veze.

Ako imate virtualne mreže s povezivanjem više lokacija konfigurirana, možete se povezati s virtualnog računala pomoću internog DIP ili privatne IP adrese. Možete povezati virtualnog računala putem interne DIP iz drugog virtualnog računala koja se nalazi na istom virtualne mreže. Ne možete RDP da biste virtualnog računala pomoću u DIP ako se povezujete s mjesta izvan virtualne mreže. Na primjer, ako točka web virtualne mreže konfigurirana, a ne uspostaviti vezu s računala, ne može se povezati s virtualnog računala po DIP.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Ako je moje virtualnog računala virtualne mreže s povezivanjem više lokacija, ne promet iz moje VM proći kroz tu vezu?

ne. Promet koji ima odredište IP koji je sadržan u rasponima virtualne mreže lokalne mreže IP adresu koju ste naveli idu putem virtualne mreže pristupnika. Promet ima odredišni IP koja se nalazi unutar virtualne mreže prekorači virtualne mreže. Ostali promet je šalje putem raspoređivača opterećenja s javnim mrežama ili ako prisilne tuneliranje koristi se šalju putem pristupnika Azure VPN-a. Ako su otklanjanje poteškoća, važno je da biste provjerili imate li svi rasponi naveden u lokalnoj mreži koji želite slati putem pristupnika. Provjerite da rasponi adresa lokalne mreže preklapa s bilo kojeg od raspona adresa u virtualne mreže. Osim toga, želite provjeriti DNS poslužitelj koristite je rješavanje naziv proper IP adresu.


## <a name="virtual-network-faq"></a>Najčešća pitanja vezana uz virtualne mreže

Prikaz dodatnih virtualne mreže informacije u [Virtualne mreže najčešća pitanja vezana uz](../virtual-network/virtual-networks-faq.md).
 
