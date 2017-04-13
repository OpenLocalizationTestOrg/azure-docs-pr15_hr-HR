<properties
   pageTitle="Vodič za dizajn i planiranje Azure virtualne mreže (VNet) | Microsoft Azure"
   description="Saznajte kako planiranje i dizajniranje virtualne mreže u Azure svojim potrebama odvajanja, povezivanje i mjesto."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Planiranje i dizajniranje Azure virtualne mreže

Stvaranje VNet eksperimentiranje s dovoljno jednostavno je, no vjerojatno će uvesti više VNets tijekom vremena za podršku radnog potrebama vaše tvrtke ili ustanove. Neke planiranja i dizajna, moći implementacije VNets i povezivanje resursi morate učinkovitije. Ako niste upoznati s VNets, preporučuje koje [Dodatne informacije o VNets](virtual-networks-overview.md) i [Kako implementirati](virtual-networks-create-vnet-arm-pportal.md) nešto prije nastavka. 

## <a name="plan"></a>Planiranje

Temeljito razumijevanja Azure pretplate, regije i mrežni resursi je važnosti za uspjeh. Na popisu pitanja vezana uz ispod možete koristiti kao početnu točku. Kada saznate te pitanja vezana uz možete definirati zahtjevima za dizajn mreže.

### <a name="considerations"></a>Razmatranja

Prije nego što odgovorite planiranje pitanja ispod, imajte na umu sljedeće:

- Sve koje stvorite u Azure sastoji se od jedan ili više resursa. Resurs je virtualnog računala (VM), mrežnog prilagodnika sučelja (NIC) koristi u VM je resurs, resurs je koji koriste na NIC javnu IP adresu, VNet NIC-a povezan je resurs.
- Stvaranje resursa unutar [Azure regija](https://azure.microsoft.com/regions/#services) i pretplate. I resursima se možete povezati samo s VNet koji postoji u istom regija i pretplata se ona nalazi u. 
- Možete se povezati VNets međusobno pomoću Azure [VPN pristupnika](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md). Možete povezati VNets svim regijama i pretplata na ovaj način.
- VNets možete povezati s lokalne mreže pomoću neke od [mogućnosti povezivanja](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) dostupne Azure. 
- Drugi resursi mogu biti grupirane u [grupe resursa](../azure-resource-manager/resource-group-overview.md#resource-groups), čime ćete olakšati upravljanje resursa kao jedinica. Grupa resursa mogu sadržavati resurse iz više područja dok god resurse pripadaju iste pretplate.

### <a name="define-requirements"></a>Definiranje preduvjeti

Korištenje pitanja ispod kao početnu točku za dizajn Azure mreže.  

1. Što Azure mjesta ćete koristiti na glavnom računalu VNets?
2. Morate omogućuje komunikaciju između tih Azure mjesta?
3. Potrebne za pružanje komunikacije između sustava VNet(s) Azure i vaše lokalne datacenter(s)?
4. Koliko infrastrukture kao VMs na servis (IaaS) oblaka services uloge, a web apps učinite potrebne za rješenje?
5. Morate li izdvajanja promet na temelju grupa VMs (odnosno sučelje web-poslužiteljima i pozadinskih poslužitelja baze podataka)?
6. Morate li kontrolirati tijek prometa pomoću virtualne aparata?
7. Moram li korisnici različitih skupova dozvola na različite resurse Azure?

### <a name="understand-vnet-and-subnet-properties"></a>Razumijevanje svojstva VNet i podmreže

VNet i podmreže resursima olakšali definiranje granicu sigurnosti za radnih opterećenja izvodi u Azure. U VNet je određene argumentima zbirka razmake adresu definiran kao CIDR blokova. 

>[AZURE.NOTE] Mrežni administratori upoznati s CIDR notacije. Ako niste upoznati s CIDR, [Saznajte više o tome](http://whatismyipaddress.com/cidr).

VNets sadrže sljedeća svojstva.

|Svojstvo|Opis|Ograničenja|
|---|---|---|
|**ime**|Naziv VNet|Niz znakova do 80. Može sadržavati slova, brojeve, podvlake, razdoblja ili crtice. Morate pokrenuti s slovo ili broj. Mora završavati o slovo, broj ili podvlake. Možete sadrži upper ili slova u mala slova.|  
|**mjesto**|Mjesto za Azure (naziva regija).|Mora biti valjane Azure mjesta.|
|**addressSpace**|Zbirka prefiksi adresa koji čine VNet CIDR notacijom.|Mora biti polja valjani blokova adresu CIDR, uključujući javne rasponi IP adresa.|
|**podmreže**|Skup podmreže koji čine na VNet|Pogledajte podmreže svojstva tablice u nastavku.||
|**dhcpOptions**|Objekt koji sadrži jedan svojstvo obavezno pod nazivom **dnsServers**.||
|**dnsServers**|Polje DNS poslužitelja koji se koriste u VNet. Ako nijedan poslužitelj nije naveden, koristit će se rješenja Azure interni naziv.|Mora biti polja do 10 DNS poslužitelja, po IP adresa.| 

Podmreži je resurs dijete od na VNet, a omogućuje definiranje segmenata adresu razmake unutar bloka CIDR, pomoću prefiksi IP adresa. NIC-ovi možete dodati podmreže i povezani VMs, koja omogućuje povezivanje za različite radnih opterećenja.

Podmreže sadrže sljedeća svojstva. 

|Svojstvo|Opis|Ograničenja|
|---|---|---|
|**ime**|Naziv podmreže|Niz znakova do 80. Može sadržavati slova, brojeve, podvlake, razdobljima ili crtice. Morate pokrenuti s slovo ili broj. Mora završavati o slovo, broj ili podvlake. Možete sadrži upper ili slova u mala slova.|
|**mjesto**|Mjesto za Azure (naziva regija).|Mora biti valjane Azure mjesta.|
|**addressPrefix**|Jedna adresa prefiks koji čine podmreže notacijom CIDR|Mora biti jedan blok CIDR koja je dio nešto na VNet adresa razmake.|
|**networkSecurityGroup**|NSG primjenjuje se na podmreži|u odjeljku [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Usmjeravanje tablicu primjenjuje se na podmreži|u odjeljku [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|Zbirka IP konfiguracije objekata koristi NIC-ovi s podmreži|u odjeljku [konfiguracije IP](../resource-groups-networking.md#IP-configurations)|

### <a name="name-resolution"></a>Razlučivanje naziva

Po zadanom koristi vaš VNet [razlučivanje naziva pod uvjetom Azure.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) Da biste riješili imena unutar u VNet, a zatim na javnog Interneta. Međutim, ako vaš VNets s centre za podatke na lokalni, morate [vlastite DNS poslužitelj](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) za razrješavanje imena između mreža pruža.  

### <a name="limits"></a>Ograničenja

Pregledajte umrežavanje ograničenja u članku [Azure ograničava](../azure-subscription-service-limits.md#networking-limits) da biste bili sigurni da ne dizajnu sukobu s nekim od ograničenja. Neka ograničenja možete povećati tako da otvorite zahtjev za podršku možete.

### <a name="role-based-access-control-rbac"></a>Kontrola pristupa na temelju uloga (RBAC)

[Azure RBAC](../active-directory/role-based-access-built-in-roles.md) možete koristiti da biste odredili razinu pristupa drugi korisnici možda imaju različite resursima servisa Azure. Na taj način možete segregate posla tako da vaš tim ovisno o njihovim potrebama. 

Koliko god se brinuti virtualne mreže, korisnici u ulogu **Suradnika mreže** imaju potpunu kontrolu nad Voditelj resursa Azure virtualne mrežni resursi. Isto tako, korisnici u ulogu **Suradnika klasični mreže** imaju potpunu kontrolu nad klasični virtualne mrežni resursi.

>[AZURE.NOTE] Možete i [stvoriti vlastiti uloge](../active-directory/role-based-access-control-configure.md) za razdvajanje potrebama administratora.

## <a name="design"></a>Dizajn

Kada znate odgovore na pitanja u odjeljku [Planiranje](#Plan) , pregledajte sljedeće prije definiranje vaše VNets.

### <a name="number-of-subscriptions-and-vnets"></a>Broj pretplate i VNets

Razmislite o stvaranju više VNets u sljedećim scenarijima:

- **VMs koji moraju se staviti na različitim mjestima Azure**. VNets u Azure su regionalne. Oni ne može obuhvaćati mjesta. Stoga morate imati barem jedan VNet za svaki Azure mjesto na koje želite VMs glavno računalo u.
- **Radnih opterećenja koji moraju biti potpuno Izolirani međusobno**. Možete stvoriti zaseban VNets koje čak i koristiti isti IP adresa razmake izdvojiti različitih radnih opterećenja međusobno. 

Imajte na umu da se prikaže iznad ograničenja su po regijama po pretplati. To znači da koristite višestruke pretplate da biste povećali ograničenja resursa možete održavati u Azure. Možete koristiti web-mjesto VPN-a ili je elektronička ExpressRoute povezati VNets za različite pretplate.

### <a name="subscription-and-vnet-design-patterns"></a>Pretplata i VNet dizajn obrazaca

U sljedećoj tablici prikazane su neke uobičajene uzorke dizajna za korištenje pretplate i VNets.

|Scenarij|Dijagram|Profesionalce|Protiv|
|---|---|---|---|
|Jedan pretplatu, dva VNets po aplikacije|![Jedan pretplate](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Samo jedan pretplata za upravljanje.|Maksimalan broj VNets po regijama Azure. Potreban vam je više pretplate nakon toga. Pročitajte članak [Azure ograničava](../azure-subscription-service-limits.md#networking-limits) detalje.|
|Pretplata na po aplikacije, dva VNets po aplikacije|![Jedan pretplate](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Koristi samo dva VNets po pretplati.|Teže da biste upravljali kad ima previše aplikacije.|
|Pretplata na po jedinici tvrtke, dva VNets po aplikacije.|![Jedan pretplate](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Saldo između broj pretplate i VNets.|Maksimalan broj VNets po jedinici za tvrtke (pretplatu). Pročitajte članak [Azure ograničava](../azure-subscription-service-limits.md#networking-limits) detalje.|
|Pretplata na po jedinici tvrtke, dva VNets po grupi aplikacija.|![Jedan pretplate](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Saldo između broj pretplate i VNets.|Aplikacija se mora isolated pomoću podmreže i NSGs.|


### <a name="number-of-subnets"></a>Broj podmreže

Razmislite o više podmreže u VNet u sljedećim scenarijima:

- **Nema dovoljno privatne IP adresa za sve NIC-ovi u podmreži**. Ako prostor podmreže adresa ne sadrži dovoljno IP adresa za broj NIC-ovi u podmreži, morate stvoriti više podmreže. Imajte na umu da Azure rezervira 5 privatne IP adrese iz svakog podmreže koju nije moguće koristiti: ime i prezime adrese prostor adrese (za adresu podmreže i višesmjernog) i 3 adrese koja će se koristiti interno (svrhe DHCP-a i DNS- a). 
- **Sigurnost**. Možete koristiti podmreže razdvajanje grupe VMs međusobno za radnih opterećenja koji imaju više sloja strukture i primijeniti različite [mreže sigurnosnih grupa (NSGs)](virtual-networks-nsg.md#subnets) za te podmreže.
- **Povezivanje hibridnog**. Možete koristiti VPN pristupnika i krugova ExpressRoute za [Povezivanje](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) VNets međusobno i vaše lokalne podatke center(s). VPN pristupnika i ExpressRoute krugova potreban je vlastite će biti stvoren podmreži.
- **Virtualna aparata**. Virtualna uređaj, kao što je vatrozid, WAN accelerator ili VPN pristupnika možete koristiti u programa Azure VNet. Kada to učinite, morate [usmjeravanje prometa](virtual-networks-udr-overview.md) u tim aparata, a izdvajanja ih u vlastite podmreže.

### <a name="subnet-and-nsg-design-patterns"></a>Podmreže i NSG dizajniranje obrazaca

U sljedećoj tablici prikazane su neke uobičajene uzorke dizajna za korištenje podmreže.

|Scenarij|Dijagram|Profesionalce|Protiv|
|---|---|---|---|
|Jedan podmreže, NSGs po aplikacijskom sloju po aplikacije|![Jedan podmreže](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Samo jedan podmreže za upravljanje.|Više NSGs potrebno izdvojiti svaku aplikaciju.|
|Jedan podmreže po aplikaciji NSGs po aplikacijskom sloju|![Podmreže po aplikacije](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Manje NSGs za upravljanje.|Više podmreže za upravljanje.|
|Jedan podmreže po aplikacijskom sloju, NSGs po aplikacije.|![Podmreže po sloja](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Saldo između broj podmreže i NSGs.|Maksimalan broj NSGs po pretplati. Pročitajte članak [Azure ograničava](../azure-subscription-service-limits.md#networking-limits) detalje.|
|Jedan podmreže po aplikacijskom sloju po aplikaciji NSGs po podmreže|![Podmreže po sloj po aplikacije](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Vjerojatno manji broj NSGs.|Više podmreže za upravljanje.|

## <a name="sample-design"></a>Primjer dizajna

Da biste ilustrirali aplikacije informacije u ovom članku, zamislite sljedeće.

Radi tvrtka koja ima 2 centri podataka u Sjevernoj Americi i dva podataka centri Europe. Označena 6 drugom kupcu nasuprotne aplikacije održava 2 različite poslovne jedinice koje želite migrirati Azure kao pilota. Slijede osnovni arhitektura za aplikacije:

- App1, App2, App3 i App4 su web-aplikacije koje se nalaze na poslužiteljima Linux koji se izvode Ubuntu. Svaku aplikaciju povezuje s poslužiteljem zasebna aplikacija hosts RESTful usluge na poslužiteljima Linux. RESTful services povežite s bazom podataka MySQL pozadinskih.
- App5 i App6 su web-aplikacije koje se nalaze na poslužiteljima sustava Windows koji se izvode Windows Server 2012 R2. Svaku aplikaciju povezuje se s bazom podataka sustava SQL Server pozadinskih.
- Sve aplikacije trenutno nalaze se u jednom od tvrtke centri podataka u Sjevernoj Americi.
- Centri za lokalnih podataka pomoću 10.0.0.0/8 adresni prostor.

Morate dizajniranje rješenja virtualne mreže koji zadovoljava sljedeće uvjete:

- Svaku poslovnu jedinicu treba neće utjecati na potrošnje resursa drugih poslovnih jedinica.
- Trebali biste smanjite količinu VNets i podmreže da biste olakšali upravljanje.
- Svaku poslovnu jedinicu treba imati na jednom test/razvoj VNet koji se koristi za sve aplikacije.
- Svaku aplikaciju nalazi se u 2 različitih Azure podataka po kontinent (Sjeverna Amerika i Europe).
- Svaka aplikacija nije potpuno Izolirani međusobno.
- Svaku aplikaciju možete pristupiti tako da klijenti putem Interneta putem HTTP.
- Korisnici koji su povezani s lokalnog centre za podatke pomoću šifrirane tunelom može pristupiti svaku aplikaciju.
- Povezivanje lokalnih podataka centrima trebali biste koristiti postojeće VPN uređaja.
- Grupe za umrežavanje tvrtke treba imaju potpunu kontrolu nad VNet konfiguracije.
- Razvojni inženjeri u svaku poslovnu jedinicu trebali biste moći samo implementacija VMs postojeće podmreže.
- Sve aplikacije će se premjestiti kao i za Azure (Dizalica-a-shift).
- Baza podataka na svakom mjestu treba replicirati na druga mjesta Azure jednom dnevno.
- Svaku aplikaciju trebali biste koristiti 5 sučelje web-poslužiteljima, 2 poslužitelja aplikacije (kada je potrebno) i 2 poslužitelja baze podataka.

### <a name="plan"></a>Planiranje

Trebala dizajnu planiranje odgovorima na pitanja u odjeljku [Definiranje preduvjeti](#Define-requirements) kao što je prikazano u nastavku.

1. Što Azure mjesta ćete koristiti na glavnom računalu VNets?

    2 mjesta u Sjevernoj Americi i 2 mjesta u Europi. Trebali biste odaberite one na temelju fizičke lokacije na postojeće centrima lokalnih podataka. Na taj način vezu iz fizičkih lokacija Azure će imati bolje Latencija.

2. Morate omogućuje komunikaciju između tih Azure mjesta?

    Da. Budući da baze podataka, morate je replicirati na sva mjesta.

3. Potrebne za pružanje komunikacije između sustava VNet(s) Azure i lokalnih podataka center(s)?

    Da. Budući da korisnici priključeni centre za lokalne podatke moraju imati mogućnost pristup aplikacijama putem šifrirane tunelom.
 
4. Koliko IaaS VMs potrebne za rješenje?

    200 IaaS VMs. App1, App2 i App3 potrebne 5 web-poslužiteljima svaki, 2 aplikacije poslužitelje svaki i 2 poslužitelja baze podataka svaki. To je ukupno 9 VMs IaaS po aplikaciji ili 36 IaaS VMs. App5 i App6 potreban je 5 web i 2 bazu podataka poslužitelja svaki. To je ukupno 7 VMs IaaS po aplikaciji ili 14 IaaS VMs. Stoga ćete 50 IaaS VMs sve aplikacije na svakom području Azure. Budući da moramo 4 područja, pojavit će se 200 IaaS VMs.

    Također morat ćete unijeti DNS poslužitelji u svakom VNet ili lokalni centre za podatke u da biste riješili naziv između vaše VMs IaaS Azure i lokalne mreže. 

5. Morate li izdvajanja promet na temelju grupa VMs (odnosno sučelje web-poslužiteljima i pozadinskih poslužitelja baze podataka)?

    Da. Svaku aplikaciju mora biti potpuno Izolirani međusobno, a svaki aplikacijskom sloju smatraju Izolirani. 

6. Morate li kontrolirati tijek prometa pomoću virtualne aparata?

    ne. Virtualna aparata može se koristiti omogućuju veću kontrolu nad tijek prometa, uključujući detaljnije zapisivanje ravnini podataka. 

7. Moram li korisnici različitih skupova dozvola na različite resurse Azure?

    Da. Tim za umrežavanje mora Potpuna kontrola na stranici virtualne mrežne postavke dok razvojnim inženjerima trebali biste moći samo implementacija njihove VMs postojećih podmreže. 

### <a name="design"></a>Dizajn

Morate slijediti dizajn navodeći pretplate, VNets, podmreže i NSGs. Ne možemo obrađuje NSGs ovdje, ali treba dodatne informacije o [NSGs](virtual-networks-nsg.md) prije dovršetka dizajna.

**Broj pretplate i VNets**

Preduvjeti vezane uz pretplate i VNets:

- Svaku poslovnu jedinicu treba neće utjecati na potrošnje resursa drugih poslovnih jedinica.
- Trebali biste smanjite količinu VNets i podmreže.
- Svaku poslovnu jedinicu treba imati na jednom test/razvoj VNet koji se koristi za sve aplikacije.
- Svaku aplikaciju nalazi se u 2 različitih Azure podataka po kontinent (Sjeverna Amerika i Europe).

Na temelju tih zahtjeva, morate pretplatu za svaku poslovnu jedinicu. Na taj način potrošnje resursa iz poslovne jedinice će ne broji prema ograničenja za ostale poslovne jedinice. I nakon želite smanjiti broj VNets, razmislite o korištenju uzorak **Pretplata po jedinici za tvrtke, dva VNets po grupi aplikacija** kao što se vidi ispod.

![Jedan pretplate](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Morate navesti prostor adrese za svaku VNet. Budući da vam je potrebna veze između lokalne podatke centara Azure područja prostora adresa koji se koristi za Azure VNets ne clash s lokalne mreže i prostor adresa koji se koristi svaki VNet treba clash s drugim postojeće VNets. Za to predviđen adresa u tablici u nastavku možete koristiti na način koji zadovoljava te preduvjete.  

|**Pretplate**|**VNet**|**Azure regija**|**Prostor adrese**|
|---|---|---|---|
|BU1|ProdBU1US1|Zapad SAD-a|172.16.0.0/16|
|BU1|ProdBU1US2|Istočni SAD-a|172.17.0.0/16|
|BU1|ProdBU1EU1|Sjeverna Europa|172.18.0.0/16|
|BU1|ProdBU1EU2|Europa Zapad|172.19.0.0/16|
|BU1|TestDevBU1|Zapad SAD-a|172.20.0.0/16|
|BU2|TestDevBU2|Zapad SAD-a|172.21.0.0/16|
|BU2|ProdBU2US1|Zapad SAD-a|172.22.0.0/16|
|BU2|ProdBU2US2|Istočni SAD-a|172.23.0.0/16|
|BU2|ProdBU2EU1|Sjeverna Europa|172.24.0.0/16|
|BU2|ProdBU2EU2|Europa Zapad|172.25.0.0/16|

**Broj podmreže i NSGs**

Preduvjeti vezane uz podmreže i NSGs:

- Trebali biste smanjite količinu VNets i podmreže.
- Svaka aplikacija nije potpuno Izolirani međusobno.
- Svaku aplikaciju možete pristupiti tako da klijenti putem Interneta putem HTTP.
- Korisnici koji su povezani s lokalnog centre za podatke pomoću šifrirane tunelom može pristupiti svaku aplikaciju.
- Povezivanje lokalnih podataka centrima trebali biste koristiti postojeće VPN uređaja.
- Baza podataka na svakom mjestu treba replicirati na druga mjesta Azure jednom dnevno.

Na temelju tih zahtjeva nije koristite jednu podmreže po aplikacijskom sloju, a pomoću NSGs filtar promet po aplikacije. Na taj način, dovoljno je 3 podmreže u svakom VNet (sučelje, aplikacijskom sloju i podataka layer) i jedan NSG svaku aplikaciju po podmreže. U ovom slučaju, razmislite o korištenju dizajn uzorak **jedan podmreže po aplikacijskom sloju, NSGs po aplikacije** . Na slici u nastavku prikazuje korištenje uzorka dizajna koji predstavlja **ProdBU1US1** VNet.

![Jedan podmreže po sloja, jedan NSG svaku aplikaciju po sloja](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Međutim, morate da biste stvorili dodatni podmreže za povezivanje VPN između na VNets i centre za podatke na lokalni. I Navedite adresu prostora za svaki podmreže. Na slici u nastavku prikazuje uzorak rješenja za **ProdBU1US1** VNet. Ovaj scenarij bi replicirati za svaku VNet. Svaka boja označava neku drugu aplikaciju.

![Ogledni VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Kontrola pristupa**

Da biste pristupili kontrole povezane su sljedeće preduvjete:

- Grupe za umrežavanje tvrtke treba imaju potpunu kontrolu nad VNet konfiguracije.
- Razvojni inženjeri u svaku poslovnu jedinicu trebali biste moći samo implementacija VMs postojeće podmreže.

Na temelju tih zahtjeva, možete dodati korisnike tima za umrežavanje ugrađene uloga **Mreža suradnika** u svake pretplate i stvoriti prilagođene uloge za razvojne inženjere aplikacije u svaku pretplatu ih pružanje prava da biste dodali VMs postojeće podmreže.

## <a name="next-steps"></a>Daljnji koraci

- [Uvođenje virtualne mreže](virtual-networks-create-vnet-arm-template-click.md) na temelju scenarij.
- Objašnjenje kako [Učitavanje saldo](../load-balancer/load-balancer-overview.md) VMs IaaS i [upravljati usmjeravanje preko više Azure regija](../traffic-manager/traffic-manager-overview.md).
- Dodatne informacije o [NSGs i planiranje i dizajniranje](virtual-networks-nsg.md) rješenja programa NSG.
- Dodatne informacije o [više lokacija i mogućnosti povezivanja VNet](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
