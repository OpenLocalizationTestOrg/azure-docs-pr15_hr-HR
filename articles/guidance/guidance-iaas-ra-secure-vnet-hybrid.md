<properties
   pageTitle="Implementacijom arhitekturu mreže sigurne hibridnog u Azure | Microsoft Azure"
   description="Kako implementirati arhitekturu mreže sigurne hibridnog u Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Implementacijom DMZ između Azure i vaše lokalne podatkovnim centrom

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

U ovom se članku opisuje najbolje prakse za implementacije sigurno hibridnog mreži s kojom se prenosi i lokalne mreže Azure. Ovu referenca arhitektura implementira DMZ između lokalne mreže i Azure virtualne mreže pomoću korisnički definirane usmjerava (UDRs). Na DMZ sadrži vrlo dostupnih mrežnih virtualne aparata (NVAs) koja implementira sigurnosnim funkcijama kao što su vatrozida i provjere paketa. Odlazni promet iz na VNet je prisilno tunneled s internetom putem lokalne mreže pa ih se provjeravati. 

Tu arhitekturu zahtijeva vezu lokalnog podatkovnim centrom implementirati pomoću ili [VPN pristupnika][ra-vpn], ili [ExpressRoute] [ ra-expressroute] veze.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. Ovu referenca arhitektura koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

Tipična Upotreba slučajeva za tu arhitekturu obuhvaćaju sljedeće:

- Hibridno aplikacije mjesto cilja radnih opterećenja djelomično lokalnog i djelomično u Azure.

- Azure infrastrukture koji usmjerava promet s lokalnog poslužitelja i s Internetom.

- Aplikacije potreban za odlazni promet za nadzor. Preduvjet je snimanje često propisima mnogo komercijalne sustavi i omogućuju da biste spriječili javnu objavu osobne podatke.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu ističu važne komponente u toj arhitekturi:

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram nalazi se na stranici "DMZ – privatne".

[! [0]][0]

- **Lokalne mreže.** Ovo je mreža računala i uređaja koji su povezani putem privatnog područje za lokalne mreže implementirana u tvrtki ili ustanovi.

- **Azure virtualne mreže (VNet).** Na VNet hostira aplikacija i ostale resurse koji se izvodi u oblaku.

- **Pristupnik.** Pristupnik nudi veze između usmjerivača u lokalne mreže i s VNet.

- **Mrežni virtualne potražite (NVA).** NVA je generički izraz koji opisuje VM obavljaju zadatke kao što je dopuštanje ili odbijanje pristupa kao vatrozid, optimiziranje (uključujući spajanje mreža) je WAN operacije, prilagođene usmjeravanje ili druge funkcije mreže. 

- **U web-sloju, sloju tvrtke i podmreže sloju podataka.** To su podmreže hosting VMs i servise koje implementirati aplikaciju 3 sloja primjer koja se izvodi u oblaku. Potražite u članku [Izvodi Windows VMs za Arhitektura programa N sloja na Azure] [ ra-n-tier] dodatne informacije.

- **Korisnički definirane usmjerava (UDR).** [Korisnički definirane usmjerava] [ udr-overview] definiranje tijek IP promet unutar Azure VNets. 

> [AZURE.NOTE] Ovisno o preduvjetima za VPN vezu, možete konfigurirati usmjerava obrub pristupnika Protocol (BGP) kao zamjena za da biste pomoću UDRs implementaciju prosljeđivanje pravila koja se usmjerili promet unatrag kroz lokalne mreže.

- **Upravljanje podmreže.** U ovom podmreže sadrži VMs koja implementira upravljanje i nadzor mogućnosti za komponente izvodi u na VNet. 

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke u osim ako imate određene zahtjeva koji ne stane i preporuke.

### <a name="rbac-recommendations"></a>Preporuke za RBAC

Stvorite nekoliko RBAC uloga Upravljanje resursima u aplikaciji. Razmislite o stvaranju DevOps [prilagođene uloge] [ rbac-custom-roles] s dozvolama za administriranje infrastrukture za aplikaciju. Razmislite o stvaranju središnje IT administrator [prilagođene uloge] [ rbac-custom-roles] da biste upravljali mrežne resurse i odvojiti sigurnosne IT administrator [prilagođene uloge] [ rbac-custom-roles] da biste upravljali sigurne mrežne resurse kao što su u NVAs. 

Uloga DevOps trebali biste uključiti dozvole za implementaciju aplikacije komponente, kao i monitora i ponovno pokrenite VMs. Središnje ulogu administratora za IT mora sadržavati dozvole za praćenje mrežni resursi. Niti jedno od sljedećih uloga morate imati pristup NVA resurse kao što je to bi trebalo biti ograničeno na sigurnost IT administratorsku ulogu.

### <a name="resource-group-recommendations"></a>Preporuke za grupu resursa

Azure resurse kao što su VMs, VNets i učitavanje balancers možete jednostavno upravlja tako da ih grupirate zajedno u grupe resursa. Zatim možete dodijeliti uloge iznad za svaku grupu resursa ograničiti pristup.

Preporučujemo stvaranje od sljedećeg:

- Grupa resursa koji sadrži podmreže (bez u VMs), NSGs i resursa za pristupnik za povezivanje s mrežom na lokalni. Dodijeliti administratorsku ulogu za IT središnje ovu grupu resursa.

- Grupa resursa koji sadrži na VMs za NVAs (uključujući raspoređivača opterećenja), skočni okvir i druge upravljanje VMs i UDR za podmreže pristupnika koji nameće početak sveg prometa na NVAs. Sigurnost IT administratorsku ulogu dodijeliti ovu grupu resursa.

- Razdvajanje grupa resursa za svaki razina aplikacije koje sadrže opterećenja i VMs. Imajte na umu da bi trebalo ovu grupu resursa obuhvaća podmreže za svaki sloju. Dodijeli ulogu DevOps ovu grupu resursa.

### <a name="virtual-network-gateway-recommendations"></a>Preporuke za pristupnik virtualne mreže

Promet na lokalni prosljeđuje na VNet putem virtualne mreže pristupnika. Preporučujemo da se [pristupnik za Azure VPN] [ guidance-vpn-gateway] ili [pristupnika Azure ExpressRoute][guidance-expressroute]. 

### <a name="nva-recommendations"></a>Preporuke za NVA

NVAs sadrže različite servise za Upravljanje projektom i praćenje mrežnog prometa. Trgovina Azure nudi nekoliko NVAs proizvođača, uključujući:

- [Barracuda web-aplikacije vatrozid] [ barracuda-waf] i [Barracuda NextGen vatrozid][barracuda-nf]

- [Povezujuće mreža VNS3 vatrozid/usmjerivača/VPN-a][vns3]

- [Fortinet FortiGate VM][fortinet]

- [Vatrozid SecureSphere Web aplikacije][securesphere]

- [Vatrozid DenyAll Web aplikacije][denyall]

- [Provjera vSEC točka][checkpoint]

Ako nijedan te drugih proizvođača NVAs odgovara potrebama, možete stvoriti prilagođeni NVA pomoću VMs. Primjer stvaranje prilagođenih NVAs, potražite u članku DMZ u ovu referenca arhitekturi koji implementira sljedeće funkcije:

- Promet usmjeravanja pomoću [prosljeđivanja IP] [ ip-forwarding] na mrežne kartice NVA.

- Promet je dopušteno proći kroz na NVA samo ako je potrebno. Svaki NVA VM arhitekture reference je jednostavan usmjerivač Linux s dolazni promet stiže na mreži sučelja *eth0*i pravila podudaranja odlazni promet definira prilagođene skripte poslanih putem mreže sučelje *eth1*. 

- Promet usmjeriti podmreže upravljanje proći kroz na NVAs i u NVAs samo moguće konfigurirati podmreže upravljanje. Promet za upravljanje podmreži eventualno da biste se usmjerena kroz na NVAs postoji smjera u podmreži upravljanje da biste riješili problem s NVAs ako oni treba neće uspjeti.  

- Raspoloživost postavite iza raspoređivača opterećenja obuhvaćeni su VMs za na NVA. UDR u podmreže pristupnika usmjerava NVA zahtjevi za raspoređivača opterećenja.

Drugi preporuka uzeti u obzir povezuje više NVAs u nizu sa svakom NVA izvođenje zadatka specijalizirane sigurnost. Time se omogućuje svake funkcije sigurnost da biste se upravlja na temelju po NVA. Ako, na primjer, programa NVA implementacijom vatrozid nije nalaziti u nizu pomoću programa NVA pokretanje identiteta servisa. Tradeoff radi lakšeg upravljanja je dodatak preskakanja dodatni mrežni koji mogu povećati Latencija, pa provjerite da to ne utječe na performanse vaše aplikacije.

### <a name="nsg-recommendations"></a>Preporuke za NSG

Pristupnik za VPN izlaže javnu IP adresa za povezivanje s lokalne mreže. Preporučujemo stvaranje mreže sigurnosne grupe (NSG) za dolazni NVA podmreže implementing pravila da biste blokirali sve promet ne potječu iz lokalne mreže.

Preporučujemo i implementirate NSGs za svaki podmreže omogućuje drugu razinu zaštite od dolazni promet zaobilaženje programa NVA neispravno konfigurirani ili onemogućene. Na primjer, podmreži web razina arhitekture referenca implementira NSG pomoću pravila da biste zanemarili sve zahtjeve za osim onih primili iz lokalne mreže (192.168.0.0/16) ili u VNet i drugo pravilo zanemaruje sve zahtjeve za nije učinio na priključak 80. 

### <a name="internet-access-recommendations"></a>Preporuke za pristup Internetu

[Prisilno tunelom] [ azure-forced-tunneling] sve izlazne internetski promet putem lokalne mreže pomoću tunelom VPN-a web-mjesto i usmjeravanje s Internetom koristeći mreže tranlation adresa (NAT). To će sprječava nenamjerno oštećenja sve povjerljive podatke pohranjene u sloju vaše podatke i omogućuju ispitivanje i nadzor sve odlazni promet.

> [AZURE.NOTE] Potpuno ne blokiraj internetski promet iz razine web, tvrtke i aplikacije. Ako te razine pomoću servisa Azure PaaS oslanjate javnu IP adresa za dijagnostiku VM zapisivanje, preuzmite VM proširenja i druge funkcije. Azure Dijagnostika zahtijeva da komponente lakše čitali i pisali račun internetske ovisne Azure prostora za pohranu.

Daljnje preporučujemo da provjerite odlazne internetski promet je prisilno tunneled ispravno. Ako koristite VPN vezu s [usmjeravanje i daljinski pristup servisa] [ routing-and-remote-access-service] na lokalni poslužitelj pomoću alata kao što je [WireShark] [ wireshark] ili [Microsoftov analizator poruke](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Upravljanje podmreže preporuke

Upravljanje podmreži sadrži skočni okvir koji se izvršava nadzora funkcije upravljanja i. Implementacija sljedećim preporukama za skočni okvir:
- Stvaranje javne IP adrese za skočni okvir. 
- Stvaranje jednog usmjeravanje da biste pristupili skočni okvir putem dolazne pristupnika i implementaciju sustava NSG u podmreže upravljanje samo odgovora na zahtjeve iz dopuštene usmjeravanje.
- Ograničiti izvođenja svih zadataka upravljanja sigurne skočni okvir.

### <a name="nva-recommendations"></a>Preporuke za NVA

Uključite sloj 7 NVA za prekid veze aplikacije na razini NVA i održavanje afinitet s pozadinskom razine. Time će se jamčiti simetričnu povezivanje u kojoj promet odgovor s pozadinskom razine vraća kroz na NVA.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Arhitektura referenca implementira raspoređivača opterećenja koja usmjeruje mrežni promet na lokalni resurse NVA uređaja. Spomenuli, uređaji NVA su VMs izvršavanja pravila za usmjeravanje prometa mreže i uvode se u programa [Postavljanje dostupnosti][availability-set]. Ovaj dizajn omogućuje vam praćenje propusnost u NVAs tijekom vremena i dodajte NVA uređaje u odgovoru povećanja u Učitaj.

Standardni pristupnika SKU VPN podržava osigurale trajne propusnost do 100 MB/s. Visoke performanse SKU daje do 200 MB/s. Veća bandwidths razmislite o nadogradnji na pristupnik za ExpressRoute. ExpressRoute daje do 10 Gbps propusnosti donjem latencije od VPN veza.

> [AZURE.NOTE] Članci [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN] [ guidance-vpn-gateway] i [implementaciju arhitekturu hibridnog mreže s Azure ExpressRoute] [ guidance-expressroute] opisuju problemi oko skalabilnost Azure pristupnika.

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Arhitektura referenca implementira raspoređivača opterećenja distribucija zahtjeve iz lokalnog skup NVA uređaja u Azure. Uređaji NVA su VMs izvršavanja pravila za usmjeravanje prometa mreže i uvode se u programa [Postavljanje dostupnosti][availability-set]. Raspoređivača opterećenja redovito upiti stanja probni na svakom NVA i bilo koji ne reagira NVAs će uklonili na resurse. 

Ako koristite Azure ExpressRoute omogućuje povezivanje između VNet i lokalne mreže, [konfigurirati pristupnik za VPN omogućuje prebacivanje] [ guidance-vpn-failover] ako ExpressRoute veza postane nedostupan.

Dodatne informacije o zaštiti dostupnost VPN-a i ExpressRoute veze, potražite u člancima [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN] [ guidance-vpn-gateway] i [implementaciju arhitekturu hibridnog mreže s Azure ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

Sve aplikacije i nadzor resursa u sklopu trebaju izvesti skočni okvir u podmreže upravljanje. Ovisno o preduvjetima za aplikaciju, možda ćete morati dodavati još resursa koji nadzora u podmreže upravljanja, no ponovno neki od ovih dodatne resurse treba pristupiti putem skočni okvir.

Ako je pristupnika iz lokalne mreže veza sa sustavom Azure prema dolje, i dalje možete dobiti skočni okvir uvođenjem TOČAKA, dodavanjem skočni okvir, a remoting u s Interneta.

Svaki sloju podmreže arhitekture referenca zaštićen NSG pravila, a možda će biti potrebno da biste stvorili pravilo da biste otvorili priključak 3389 za pristup RDP na Windows VMs ili priključak 22 za pristup SSH Linux VMs. Ostale upravljanje i Alati za nadzor može zahtijevati pravila da biste otvorili dodatne priključke.

Ako koristite ExpressRoute omogućuje povezivanje između lokalnog podatkovnog centra i Azure pomoću [Alata za povezivanje Azure (AzureCT)] [ azurect] za praćenje i rješavanje problema s povezivanjem.

> [AZURE.NOTE] Možete pronaći dodatne informacije posebno usmjerenih pri nadzor i upravljanje njima VPN-a i ExpressRoute veze na članke [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN] [ guidance-vpn-gateway] i [implementaciju arhitekturu hibridnog mreže s Azure ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Ovu referenca arhitektura implementira više razine sigurnosti: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Usmjeravanje sve zahtjeve za korisnika lokalnog kroz na NVA

UDR u podmreže pristupnika blokira sve zahtjeve za korisnika osim onih s lokalnim primio. Prolaze UDR dopuštene zahtjevi za na NVAs u privatnu podmreže DMZ, a te zahtjeve prenose se aplikaciju ako smiju NVA pravila. Druge usmjerava mogu dodati na UDR, ali bi se ne slučajno zaobići NVAs ili bloka promet administratora za upravljanje podmreže.

Opterećenja ispred na NVAs i djeluje kao sigurnosni uređaj tako da zanemarujući promet na priključke koje nisu otvorene u pravila za ujednačavanje opterećenja. Učitavanje balancers arhitekture referenca samo poslušali HTTP zahtjeve za na priključak 80 i zahtjevi za HTTPS na priključak 443. Dodatna pravila dodaje balancers učitavanja mora biti navedenih i promet treba moguće nadzirati da biste bili sigurni da nema problema sigurnost.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Korištenje NSGs za blokiranje/računanja promet između aplikacija razine

Svaki od razine web, tvrtke i podataka ograničiti promet između njih pomoću NSGs. To jest, sloju tvrtke koristi se NSG da biste blokirali sve promet koja ne potiče u sloju web i razina podataka koristi se NSG da biste blokirali sve promet koja ne potiče u sloju tvrtke. Ako imate zahtjev da biste proširili pravila NSG dopustili šira pristup te razine, obzir uzeli i te preduvjete protiv sigurnosni rizik. Svaki novi ulaznog pathway predstavlja priliku oštećenja ili aplikacija oštećenja slučajne ili purposeful podataka.

### <a name="devops-access"></a>Pristup DevOps

Ograničavanje operacije koje DevOps može izvoditi na svakom sloju pomoću [RBAC] [ rbac] za upravljanje dozvolama. Kad dodjelu dozvola, slijedite [načelo najmanjih ovlasti][security-principle-of-least-privilege]. Prijavite se sve operacije administratora i izvođenje običnog revizije da biste bili sigurni da su promjene konfiguracije planirano.

> [AZURE.NOTE] Širi informacije, primjere i scenariji o upravljanju mreže vrijednosnice s Azure, potražite u članku [Microsoftovim servisima u oblaku i sigurnost mrežne][cloud-services-network-security]. Detaljne informacije o zaštiti resursa u oblak potražite u članku [Uvod u Microsoft Azure sigurnost][getting-started-with-azure-security]. Dodatne pojedinosti o adresiranja problemima sigurnosti preko vezu s Azure pristupnika potražite [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN] [ guidance-vpn-gateway] i [implementaciju arhitekturu hibridnog mreže s Azure ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Uvođenje rješenja

Uvođenje za referencu arhitektura koji implementira te preporuke i dalje dostupna je na Github. Arhitektura ovu referenca sadrži virtualne mreže (VNet), mreže sigurnosne grupe (NSG), opterećenja i dva virtualnim strojevima (VMs).

Arhitektura referenca može uvesti pomoću sustava Windows ili Linux VMs slijedeći upute u nastavku: 

1. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Kada na portalu za Azure otvorio vezu, morate unijeti vrijednosti za neke postavke: 
    - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-private-dmz-rg` u tekstni okvir.
    - **Mjesto** padajućem okviru odaberite regiju.
    - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
    - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
    - Kliknite gumb **za kupnju** .

3. Pričekajte implementaciju da biste dovršili.

4. Datoteka parametar obuhvaćaju programiranih administrator korisničko ime i lozinku za sve VMs i preporučuje da odmah promijeniti oboje. Za svaki VM implementacije odaberite na portalu za Azure, a zatim kliknite na **ponovno postavljanje lozinke** u plohu **podršku + otklanjanje poteškoća** . U okviru **način** padajućeg izbornika odaberite **ponovno postavljanje lozinke** , a zatim odaberite novo **korisničko ime** i **lozinku**. Kliknite gumb **Ažuriraj** održati.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako implementirati [DMZ između Azure i Interneta](./guidance-iaas-ra-secure-vnet-dmz.md).
- Saznajte kako implementirati [arhitektura iznimno dostupna hibridnog mreže](./guidance-hybrid-network-expressroute-vpn-failover.md).

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Sigurne arhitektura hibridnog mreže"
