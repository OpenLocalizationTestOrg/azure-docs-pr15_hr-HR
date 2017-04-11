## <a name="virtual-network-basics"></a>Osnove virtualne mreže

### <a name="what-is-an-azure-virtual-network-vnet"></a>Što je Azure virtualne mreža (VNet)?

Možete koristiti VNets za dodjelu resursa i upravljanje virtualne privatne mreže (VPN-ovi) u Azure i, Neobavezno, veza VNets s drugim VNets u Azure ili lokalnih IT infrastrukturu radi stvaranja hibridnog ili više lokacija rješenja. Svaki VNet stvorite ima zasebnom bloka CIDR, a može povezati s drugim mrežama VNets i lokalnog pod uvjetom da blokira CIDR sudaraju. Imate kontrole DNS postavke poslužitelja za VNets i segmente u VNet u podmreže.

Koristite VNets za:

- Stvaranje namjenski samo oblak virtualne privatnom

    Ponekad ne zahtijeva više lokacija konfiguracije rješenje. Kada stvorite na VNet, servisa i VMs unutar vaše VNet možete izravno i sigurno međusobno komunicirati u oblaku. Time se zadržava promet sigurno unutar na VNet, ali se i dalje omogućuje konfiguriranje veza za krajnju točku za VMs i servise koje je potrebno internetskih komunikacija kao dio vašeg rješenja.

- Sigurno proširivanje podatkovnog centra

    U VNets, možete izraditi klasične web-mjesto (S2S) VPN-ovi za sigurno skaliranje na kapacitet podatkovnog centra. S2S VPN-ovi koriste IPSEC daju sigurnu vezu između korporativni VPN pristupnika i Azure.

- Omogućivanje scenarija hibridnog oblaka

    VNets što je fleksibilnosti za podršku raspon scenarija hibridnog oblaka. Možete se povezati sigurno aplikacije utemeljene na oblaku na bilo koju vrstu lokalnog sustava kao što je mainframes i Unix sustavima.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Kako znati je li nužno virtualne mreže?

Posjetite [Virtualne pregled mreže](../articles/virtual-network/virtual-networks-overview.md) da biste vidjeli tablicu odluke koje će vam pomoći da odlučite najbolja mogućnost dizajn mreža za.

### <a name="how-do-i-get-started"></a>Kako započeti rad?

Potražite [u dokumentaciji virtualne mreže](https://azure.microsoft.com/documentation/services/virtual-network/) za početak rada. Ova stranica sadrži veze na uobičajeni navedeni koraci za konfiguraciju, kao i informacije koje će vam pomoći shvatiti stvari koje morate uzeti u obzir prilikom dizajniranja virtualne mreže.

### <a name="what-services-can-i-use-with-vnets"></a>Servisima na koje možete koristiti s VNets?

VNets može se koristiti s nizom različitih Azure servisa, kao što su servisi u Oblaku (PaaS), virtualnih računala i web-aplikacije. Međutim, postoji nekoliko servise koje nisu podržane na na VNet. Provjerite je li određene servis koji želite koristiti i provjerite je li kompatibilne.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Mogu li koristiti VNets bez više lokacija povezivanje?

Da. Možete koristiti na VNet bez korištenja povezivanje web-mjesto. To je posebno korisno ako želite da biste pokrenuli kontrolera domena i farmama sustava SharePoint u Azure.

## <a name="virtual-network-configuration"></a>Konfiguracija virtualne mreže

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Koje alate koristiti da biste stvorili na VNet?

Možete koristiti sljedeće alate za stvaranje i konfiguriranje virtualne mreže:

- Azure Portal (za klasični i resursima VNets).

- Na mreži konfiguracijska datoteka (netcfg - za klasični VNets). Potražite u članku [Konfiguriranje virtualne mreže pomoću datoteke konfiguracije mreže](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShell (za klasični i resursima VNets).

- Azure EŽA (za klasični i resursima VNets).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Rasponi koje adresa možete koristiti u moj VNets?

Koristite javnoj rasponi IP adresa i sve rasponu IP adresa definirano u [RFC 1918](http://tools.ietf.org/html/rfc1918).

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Možete imati javnu IP adresa u moj VNets?

Da. Dodatne informacije o javnim rasponi IP adresa potražite u članku [javnu IP adresa prostor u virtualne mreže (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Imajte na umu da na javnu IP-ovi neće biti izravno dostupno putem Interneta.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Postoji li ograničenje broja podmreže u mojoj mreži virtualne?

Nema ograničenja broja podmreže potonji koristite na VNet. Sve podmreže mora biti potpuno sadržana u adresni prostor virtualne mreže i mora se preklapaju međusobno.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Postoje li ograničenja o korištenju IP adrese iz ove podmreže?

Azure rezervira neke IP adrese unutar svake podmreže. Ime i prezime IP adrese sustava podmreže su rezervirana za usklađenosti protokol, zajedno s 3 više adresa koristiti za Azure servise.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Kako se mala i kako velike možete VNets i podmreže biti?

Najmanji podmreži podržavamo na /29 a najvećeg /8 (pomoću CIDR podmreži definicije).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Možete I unijeti Moja VLANs za Azure pomoću VNets?

ne. VNets su prekrivanja Layer 3. Azure ne podržava sve semantiku sloja 2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Odrediti prilagođene pravila za usmjeravanje na moje VNets i podmreže?

Da. Možete koristiti korisnički definirana usmjeravanje (UDR). Dodatne informacije o UDR posjetite [korisnički definirana usmjerava i IP prosljeđivanje](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>VNets podržavaju višesmjernog ili emitiranje?

ne. Ne podržavamo višesmjernog ili emitiranje.

### <a name="what-protocols-can-i-use-within-vnets"></a>Koje protokole mogu koristiti unutar VNets?

Možete koristiti standardne utemeljen na IP protokoli unutar VNets. Međutim, višesmjernog emitiranje, IP u IP encapsulated pakete generički usmjeravanje Encapsulation (GRE) pakete blokirane su i unutar VNets. Standardni protokola koji rade obuhvaćaju sljedeće:

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Mogu li pomoću naredbe ping Moje zadane usmjerivača unutar na VNet?

ne.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Mogu li koristiti tracert dijagnosticiranje povezivanje?

ne.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Je li moguće dodati podmreže nakon stvaranja na VNet?

Da. Podmreže mogu se dodati VNets u bilo kojem trenutku dok god adresu podmreži nije dio drugi podmreži u na VNet.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Možete promijeniti veličinu Moje podmreži nakon što ga stvoriti?

Možete dodati, ukloniti, proširite ili Smanji podmreži ako nema VMs ili servisa u uveden u njemu pomoću cmdleta ljuske PowerShell ili NETCFG datoteku. Možete dodati, ukloniti, proširite ili Smanji sve prefiksi dok god podmreže koje sadrže VMs ili usluge ne utječu promjene.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Možete promijeniti podmreže kada ih sam stvorio?

Da. Možete dodati, ukloniti i izmijenite CIDR blokove koji se koriste u VNet.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Načini povezivanja s Internetom ako se radi servisima u na VNet?

Da. Sve servise implementiran unutar na VNet možete se povezati s Internetom. Svaki servis u oblaku implementiran u Azure ima javno moguće adresirati VIP dodijeljena. Morate definirati unos krajnje točke za PaaS uloge i krajnje točke za virtualnih računala da biste omogućili te servise za prihvaćanje veza s Internetom.

### <a name="do-vnets-support-ipv6"></a>VNets podržava IPv6?

ne. Trenutno ne možete koristiti IPv6 s VNets.

### <a name="can-a-vnet-span-regions"></a>Mogu li u VNet obuhvaćati regije

ne. Na VNet ograničeno je na jedno područje.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Moguće istodobno povezati s VNet drugi VNet u Azure?

Da. Da biste VNet komunikacije možete stvoriti VNet pomoću REST API-ji ili komponente Windows PowerShell. Možete povezati i VNets putem VNet Peering. Pogledali dodatne pojedinosti o peering [ovdje.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Razrješavanje imena u pošti (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Što mogu učiniti DNS-a za VNets?

Pomoću tablice odluka na stranici [Razlučivanje naziva za VMs i uloga instance](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) će vas voditi kroz sve dostupne mogućnosti DNS-a.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Odrediti DNS poslužitelji za na VNet?

Da. U odjeljku postavke VNet možete odrediti DNS poslužitelja IP adrese. To će se primijeniti kao DNS poslužitelji zadane za sve VMs u na VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Koliko DNS poslužitelji možete odrediti?

Možete navesti do 12 DNS poslužitelji.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Možete promijeniti Moja DNS poslužitelji nakon stvorio sam na mreži?

Da. Popis DNS poslužitelja za vaše VNet možete promijeniti u bilo kojem trenutku. Ako promijenite popis DNS poslužitelja, morat ćete ponovo pokrenuti svaki na VMs u VNet da bi ih mogao preuzeti novi DNS poslužitelj.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Što je navedeno za Azure DNS i funkcionira VNets?

DNS-ovi za Azure je servis više klijentu DNS-a nudi Microsoft. Azure registrira sve VMs i uloga instance taj servis. Ovaj servis nudi razlučivanje naziva naziv glavnog računala za VMs i uloga instance nalaze u istom servis u oblaku i FQDN za VMs i instance uloga u istoj VNet.

> [AZURE.NOTE] Nema ograničenja vrijeme za prvi 100 servisa u oblaku u virtualne mreže za razlučivanje naziva unakrsno klijentu pomoću DNS-ovi za Azure. Ako koristite vlastitu DNS poslužitelj, to ograničenje se neće primijeniti.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Može li se nadjačati postavke DNS-a na na po VM / temelj za servis?

Da. DNS poslužitelji možete postaviti na temelju servis u oblaku da biste nadjačali zadane postavke mreže. No preporučujemo da koristite cijelu mrežu DNS najveću moguću.

### <a name="can-i-bring-my-own-dns-suffix"></a>Možete I unijeti vlastitu DNS sufiks?

ne. Prilagođeni DNS sufiks nije moguće navesti za vaše VNets.

## <a name="vnets-and-vms"></a>VNets i VMs

### <a name="can-i-deploy-vms-to-a-vnet"></a>Možete implementirati VMs na VNet?

Da.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Možete implementirati Linux VMs na VNet?

Da. Možete implementirati sve distro Linux podržava Azure.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Koja je razlika između javno VIP i internu IP adresu?

- Interna IP adresa je IP adresu koja je dodijelila svaki VM unutar na VNet DHCP. Nije javno dostupnu. Ako ste stvorili u VNet, internog IP adresa se dodjeljuje iz raspona koji ste naveli u postavkama podmreže vaše VNet. Ako nemate na VNet, dodijelit će se i dalje interne IP adresa. Interna IP adresa će ostati s na VM njegov vijek osim ako koji je deallocated VM.

- Javno VIP je javna IP adresa koju vam je dodijeljen oblaka servisa ili učitavanje opterećenja. Nije dodijeljeno izravno na vašem NIC. VM Na VIP ostaje sa servisom cloud što vam je dodijeljen dok sve VMs u tom servisu u oblaku deallocated ili izbrisati. U tom trenutku objavljivanja.

### <a name="what-ip-address-will-my-vm-receive"></a>Što je IP adresa primit će Moja VM?

- **Interna IP adresa –** Ako je VM implementirati na VNet, na VM prima internu IP adresu iz skupa interne IP adrese koju navedete. VMs komunikacije unutar na VNets putem interne IP adrese. Premda Azure dodjeljuje dinamičku interne IP adresu, možete zatražiti statičke adrese za vaše VM. Da biste saznali više o statičke interne IP adrese, posjetite [kako postaviti statičke IP Interna](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** Vaš VM i povezan je s VIP, premda je VIP se nikad ne dodjeljuje na VM izravno. Na VIP je javna IP adresa koji se mogu dodijeliti servis u oblaku. Po želji možete rezervirati na VIP za servis u oblaku.

- **ILPIP-** Možete konfigurirati i instancu razinom javnu IP adresu (ILPIP). ILPIPs izravno su povezane s na VM umjesto servisa u oblaku. Da biste saznali više o ILPIPs, posjetite [Instancu razinom javnu IP pregled](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Li rezervirati interne IP adresa za VM koji će stvoriti kasnije?

ne. Ne možete rezervirati interne IP adresa. Ako je dostupan interne IP adresa je će se dodijeliti ulogu ili VM instancu DHCP poslužitelj. Taj VM možda ili možda neće biti onaj koji želite da se interne IP adresa želite dodijeliti. Međutim, promijenite interne IP adresu već stvoreni VM za sve dostupne interne IP adresa.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Učinite Interna promjena IP adrese za VMs u na VNet?

Da. Interna IP adrese ostaju na VM za njegov vijek osim ako je na VM deallocated. Kada je na VM deallocated, internog IP adresa je objavio osim ako ste definirali statični interne IP adrese za vaše VM. Ako na VM je jednostavno zaustavljen (i stavljati status **zaustavljen (Deallocated)**) ostat će dodijeljeno u VM IP adresa.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Možete I ručno dodijeliti IP adrese NIC-ovi u VMs?

ne. Ne možete promijeniti svojstva VMs sve sučelja. Promjene mogu dovesti do potencijalno prekida veze da biste na VM.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Što se događa s Moje IP adresama isključiti na VM?

Ništa. IP adrese (javno VIP i interne IP adrese) će ostati u oblaku ili VM.

> [AZURE.NOTE] Ako želite jednostavno isključivanja s VM, nemojte koristiti Portal za upravljanje da biste to učinili. Trenutačno isključivanje gumb će deallocate virtualnog računala.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Mogu li prijeći VMs s jednog podmreži u drugom podmreži u na VNet bez ponovno implementacija?

Da. Dodatne informacije možete pronaći [u nastavku](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Ću konfigurirati statičke MAC adrese za moj VM?

ne. MAC adresa statički ne može konfigurirati.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>MAC adresa ostat će isti za moj VM kada je stvorena?

Da, MAC adresu će ostati isto je VM čak i ako na VM je zaustavljena (deallocated) i Sender objavljen.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Možete povezati s Internetom iz VM u na VNet?

Da. Sve servise implementiran unutar na VNet možete se povezati s Internetom. Uz to, svaki servis u oblaku implementiran u Azure ima javno moguće adresirati VIP dodijeljena. Morate definirati unos krajnje točke za PaaS uloge i krajnje točke za VMs da biste omogućili te servise za prihvaćanje veza s Internetom.

## <a name="vnets-and-services"></a>VNets i usluga

### <a name="what-services-can-i-use-with-vnets"></a>Servisima na koje možete koristiti s VNets?

Možete koristiti samo računalnim servisa unutar VNets. Izračun services ograničeni su na servise u Oblaku (uloge web i tempiranja) i VMs.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Možete koristiti web-aplikacije s virtualne mreže?

Da. Možete implementirati web-aplikacije unutar VNet pomoću elika i mala slova (aplikaciju servisa okruženja). Dodavanje, web-aplikacije možete sigurno povezivanje i pristup resursa u vašem Azure VNet ako imate točke-na-web-mjesta konfigurirana za vaše VNet. Dodatne informacije potražite u sljedećim člancima:


- [Stvaranje web-aplikacije u okruženju aplikacije servisa](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Integracija virtualne mreže aplikacija za web](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [Integracija VNet i hibridnog veze pomoću web-aplikacije](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Web-aplikacijama integrirati Azure virtualne mreže](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Možete implementirati servise u oblaku s web- a tempiranja uloga (PaaS) na VNet?

Da. Možete implementirati PaaS servisa unutar VNets.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Kako uvesti PaaS uloge u s VNet?

Koje možete izvršiti navođenjem naziv VNet i /subnet mapiranja uloga u odjeljku Konfiguriranje mreže za konfiguraciju servisa. Ne morate ažurirati bilo koji od vašeg binarne datoteke.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Je li moguće premještati servisima i VNets?

ne. Nije moguće premjestiti servise i VNets. Morat ćete izbrisati, a zatim ponovno Implementacija servisa da biste premjestili neki drugi VNet.

## <a name="vnets-and-security"></a>VNets i sigurnost

### <a name="what-is-the-security-model-for-vnets"></a>Što je model sigurnosti za VNets?

VNets su potpuno Izolirani iz međusobno i ostale servise smješten u Azure infrastrukture. U VNet je granicu za pouzdanost.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Može se definirati ACL-a ili NSGs na moj VNets?

ne. Ne možete pridružiti ACL-a ili NSGs za VNets. Međutim, ACL-a može se definirati na unos krajnje točke za VMs koji je uveden u VNets i NSGs može biti povezan podmreže ili NIC-ovi.

### <a name="is-there-a-vnet-security-whitepaper"></a>Je li koje za sigurnost VNet?

Da. Možete preuzeti [ovdje](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>API-ji, sheme i alate

### <a name="can-i-manage-vnets-from-code"></a>Možete upravljati VNets iz koda?

Da. Koristite REST API-ji za Upravljajte povezivanjem s VNets i više lokacija. Dodatne informacije možete pronaći [u nastavku](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Je li tooling podrška za VNets?

Da. Možete koristiti alate za PowerShell i naredbenog retka za različite platforme. Dodatne informacije možete pronaći [u nastavku](http://go.microsoft.com/fwlink/?LinkId=317721).
