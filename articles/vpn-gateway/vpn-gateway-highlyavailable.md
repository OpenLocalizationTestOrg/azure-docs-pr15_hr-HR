<properties
   pageTitle="Pregled iznimno dostupna konfiguracije s Azure VPN pristupnika | Microsoft Azure"
   description="Ovaj članak sadrži pregled iznimno dostupnih mogućnosti konfiguracije pomoću Azure VPN pristupnika."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Iznimno dostupno više lokacija i povezivanje VNet VNet

Ovaj članak sadrži pregled iznimno dostupnim mogućnostima konfiguracije više lokacija i VNet VNet povezivanje pomoću Azure VPN pristupnika.

## <a name = "activestandby"></a>O zalihosti pristupnika Azure VPN-a

Svaki pristupnika Azure VPN sastoji se od dvije instance u konfiguraciji aktivno čekanja. Za svaku planiranog održavanja ili neplanirano prekidu koji se događa u instancu komponente active čekanja instancu će automatski preuzeti (prebacivanje) i nastaviti S2S VPN-a ili VNet VNet veze. Prijelaz preko uzrokovat će kratak prekida. Za planiranog održavanja na povezivanje mora vratiti 10 do 15 sekundi. Neplanirano probleme oporavak veze će biti dulji, oko 1 minutu 1 i pol minuta najgorim velikim slovom. P2S VPN klijentskog veza pristupnika, veze P2S će prekidanjem i korisnici će morati ponovno povezali s klijentskim računalima.

![Aktivni čekanja](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Povezivanje s iznimno dostupno više lokacija

Omogućuje bolju dostupnost za svoje veze unakrsne lokalni dostupno nekoliko mogućnosti:

- Više uređaja lokalnog VPN-a
- Aktivni aktivno Azure VPN pristupnika
- Kombinacijom tih dvaju načina

### <a name = "activeactiveonprem"></a>Više uređaja lokalnog VPN-a

Koristite više uređaja VPN-a iz lokalne mreže za povezivanje s pristupnikom Azure VPN, kao što je prikazano na sljedećem su dijagramu:

![Više lokalnog VPN-a](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Tu konfiguraciju sadrži više aktivni tunnels s istom Azure VPN pristupnika na uređajima lokalnog na istom mjestu. Postoje neki zahtjevi i ograničenja:

1. Morate stvoriti više S2S VPN veza s uređaja VPN Azure. Kada povežete više uređaja VPN-a iz iste lokalne mreže Azure, morate stvarati jedan pristupnik lokalne mreže za svaki uređaj VPN-a i jednu vezu s pristupnikom Azure VPN-a u lokalnoj mreži pristupnik.

2. Lokalne mreže pristupnika odgovara uređajima VPN-a mora imati jedinstveni javnu IP adresa u svojstvu "GatewayIpAddress".

3. Za tu konfiguraciju potreban je BGP. Svaki lokalne mreže pristupnika koji predstavlja VPN uređaj mora imati jedinstveni BGP ravnopravnih članova IP adresu naveden u svojstvu "BgpPeerIpAddress".

4. Polja svojstava AddressPrefix u svakom pristupnika lokalne mreže morate preklapaju. Navedite "BgpPeerIpAddress" /32 CIDR oblikovanje u polju AddressPrefix, na primjer, 10.200.200.254/32.

5. Trebate koristiti BGP za Oglasite isti prefiksi iste lokalne mreže prefiksi u pristupnik za Azure VPN i promet će proslijediti kroz ove tunnels istodobno.

6. Svaku vezu s broje se protiv maksimalni broj tunnels za pristupnik Azure VPN, 10 za osnovne i standardne SKU-ove i 30 za HighPerformance SKU-om. 

U ovoj konfiguraciji pristupnika Azure VPN je i dalje u načinu aktivno čekanja pa istog ponašanja prebacivanje i kratak prekida dogodit će se i dalje kao što je opisano [iznad](#activestandby). No, ovaj će instalacijski program čuva protiv pogreške ili prekide na lokalnu mrežu i uređaje VPN-a.
 
### <a name="active-active-azure-vpn-gateway"></a>Aktivni aktivno Azure VPN pristupnika

Sad možete stvarati pristupnik za Azure VPN u konfiguraciji aktivno Aktivno, gdje oba instance pristupnika VMs stvorit će se tunnels S2S VPN-a na uređaj VPN lokalnog kao što je prikazano na sljedećem su dijagramu:

![Aktivni aktivno](./media/vpn-gateway-highlyavailable/active-active.png)

U ovoj konfiguraciji svaku instancu Azure pristupnika će imati jedinstveni javnu IP adresu, a svakoj stvorit će se IPsec-IKE S2S VPN tunelom s uređajem VPN lokalnog navedene u lokalnoj mreži pristupnika i veze. Imajte na umu da su oba tunnels VPN zapravo dio iste veze. I dalje ćete konfigurirati uređaju lokalnog VPN-a da biste prihvatili ili uspostaviti dvije S2S VPN tunnels te dvije Azure VPN pristupnika javno IP adrese.

Budući da instance Azure pristupnika u konfiguraciji aktivno Aktivno, promet Azure virtualne mreže vaše lokalne mreže će biti usmjerena kroz oba tunnels istodobno, čak i ako je uređaj VPN lokalnog može odabrati jedan tunelom s drugim. Imajte na umu iako isti tijek TCP i UDP se uvijek prolaziti iste tunelom ili put, osim ako će se dogoditi održavanja događaja na jedan od instance.

Kada planiranog održavanja ili neplanirano događaja se događa s jednog pristupnika instancu, tunelom IPsec iz tu instancu na lokalni VPN uređaj će prekinuta. Odgovarajući usmjerava na uređajima VPN-a trebali biste ukloniti ili automatski povući tako da se promet će prijeći na druge aktivni IPsec tunelom. Na strani Azure parametar pokazivač će se dogoditi automatski iz problematične instance u instancu komponente active.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Lišće zalihosti: aktivna aktivno VPN pristupnika za Azure i lokalnih mreže

Mogućnost najčešće pouzdanog je da biste kombinirali aktivno aktivno pristupnika na vašoj mreži i Azure, kao što je prikazano u dijagramu u nastavku.

![Slika zaslona dvostrukih zalihosti](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Ovdje stvarate i postavljanje Azure VPN pristupnika u konfiguraciji aktivno Aktivno, te dvije lokalne mreže pristupnika i dvije veze za vaše dva lokalnog VPN uređaja prethodno opisan. Rezultat je vezu s puno mrežasto tkanje 4 tunnels IPsec između Azure virtualne mreže i lokalne mreže.

Svim pristupnicima i tunnels su aktivni sa strane Azure pa promet će biti širenje među svim 4 tunnels istodobno, iako svaki protok TCP i UDP će ponovno slijedite isti tunelom ili put sa strane Azure. Iako širenjem promet, vidjet ćete malo bolje propusnost putem IPsec tunnels, primarnog cilja tu konfiguraciju namijenjen visoke dostupnosti. I zbog statističke prirode širenja na je teško pružaju mjerne jedinice na kako različitim uvjetima promet aplikacije utjecat će na zbrajanja propusnost.

Ovaj topologije potrebno dva pristupnika lokalne mreže i dvije veze za podršku par lokalnog VPN uređaja, a BGP je potrebno da biste omogućili dvije veze na istom lokalne mreže. Tim preduvjetima isti su kao na [iznad](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Iznimno dostupne VNet VNet Povezivost putem pristupnika Azure VPN-a

Ista konfiguracija aktivno aktivno možete primijeniti na Azure VNet-na-VNet veze. Možete stvoriti aktivno aktivno pristupnika VPN-a za oba virtualne mreže i povezati ih zajedno obrasca isti povezivanje puno mrežasto tkanje 4 tunnels između dva VNets kao što je prikazano u dijagramu u nastavku:

![VNet VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Na taj način uvijek postoje par tunnels između dvije virtualne mreže za Svi događaji planiranog održavanja pruža još bolje dostupnost. Iako iste topologije za više lokacija povezivanje zahtijeva dvije veze, topologije VNet VNet gore navedenoj sintaksi ćete samo jednu vezu za svaki pristupnik. Uz to, BGP nije obavezno osim ako prijenosa usmjeravanje putem veze VNet VNet je potrebna.


## <a name="next-steps"></a>Daljnji koraci

Potražite u članku [Konfiguriranje pristupnika aktivno aktivno VPN-a za više lokacija i veze VNet VNet](vpn-gateway-activeactive-rm-powershell.md) upute za konfiguriranje aktivno aktivno-lokacija i VNet VNet veze.
