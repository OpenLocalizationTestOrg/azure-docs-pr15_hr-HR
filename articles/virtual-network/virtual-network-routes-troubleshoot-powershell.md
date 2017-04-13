<properties 
   pageTitle="Otklanjanje poteškoća s usmjerava - PowerShell | Microsoft Azure"
   description="Saznajte kako otkloniti poteškoće usmjerava u modelu implementaciju upravljanja resursima Azure pomoću komponente PowerShell Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-routes-using-azure-powershell"></a>Otklanjanje poteškoća s usmjerava pomoću komponente PowerShell Azure

> [AZURE.SELECTOR]
- [Portal za Azure](virtual-network-routes-troubleshoot-portal.md)
- [PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Kada se pojave problemi s mrežom ili iz vaše Azure virtualnog računala (VM), možda usmjerava utječu na VM promet tokova. Ovaj članak sadrži pregled Dijagnostika mogućnosti za usmjerava pomoći u rješavanju problema Dodatno.

Usmjeravanje tablice povezane s podmreže i učinkovitih na sva mreže sučelja (NIC) u tom podmreži. Sljedeće vrste usmjerava se mogu primijeniti na svaki mrežnog sučelja:

- **Sustava usmjerava:** Prema zadanim postavkama, svaki podmreži stvorene u programa Azure virtualne mreže (VNet) sadrži tablice za usmjeravanje sustava koje omogućuju lokalni VNet promet, promet na lokalni putem VPN pristupnika i internetski promet. Usmjerava sustava i postoje za peered VNets.
- **BGP usmjerava:** Prenose se na sučelje mreže putem ExpressRoute ili web-mjesto VPN veza. Dodatne informacije o usmjeravanju BGP tako da u člancima [BGP s VPN pristupnika](../vpn-gateway/vpn-gateway-bgp-overview.md) i [Pregled ExpressRoute](../expressroute/expressroute-introduction.md) za čitanje.
- **Korisnički definirane usmjerava (UDR):** Ako koristite mrežni virtualne aparata ili su prisilno tuneliranje promet za lokalnu mrežu putem web-mjesto VPN-a, možda ćete korisnički definirane usmjerava (UDRs) pridružene podmreže usmjeravanje tablice. Ako niste upoznati s UDRs, pročitajte članak [korisnički definirane usmjerava](virtual-networks-udr-overview.md#user-defined-routes) .

S različitim usmjerava koji se mogu primijeniti na mrežnom sučelju može biti teško da bi se utvrdilo koji usmjerava zbrajanja učinkovitih. Da biste otklonili VM mrežne veze, možete pogledati sve važeće usmjerava za mrežno sučelje u modelu implementaciju upravljanja resursima Azure.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Pomoću učinkovitih usmjerava otklanjanje poteškoća s VM tijek prometa

U ovom se članku sljedeći scenarij kao primjer koristi za ilustraciju kako otkloniti poteškoće učinkovitih usmjerava za mrežno sučelje:

VM (*VM1*) povezani s VNet (*VNet1*, prefiks: 10.9.0.0/16) ne uspije povezati s VM(VM3) u upravo peered VNet (*VNet3*, 10.10.0.0/16 prefiks). Postoje UDRs ni BGP usmjerava primjenjuje se na VM1 NIC1 mrežno sučelje povezani s VM, primjenjuju se samo usmjerava sustava.

U ovom se članku objašnjava kako uzrok prekid veze pomoću mogućnost učinkovitih usmjerava u modelu implementaciju upravljanja Azure resursa.
Dok se primjeru koristi samo usmjerava sustava, mogu se iste korake da biste odredili neuspjeha ulazni i izlazni veze u bilo kojoj vrsti usmjeravanje.

>[AZURE.NOTE] Ako vaš VM ima više od jedne NIC priložene, potvrdite okvir učinkovitih usmjerava za svaku mrežne kartice za dijagnosticiranje mreže i s na VM problema s povezivanjem.

### <a name="view-effective-routes-for-a-virtual-machine"></a>Prikaz učinkovitih usmjerava za virtualnog računala

Da biste vidjeli zbrajanja usmjerava koji se primjenjuju na VM, slijedite sljedeće korake:

### <a name="view-effective-routes-for-a-network-interface"></a>Prikaz učinkovitih usmjerava za mrežno sučelje

Da biste vidjeli zbrajanja usmjerava koja se primjenjuju na mrežnom sučelju, slijedite sljedeće korake:

1.  Započnite sesiju ljuske PowerShell Azure i prijavite se na Azure. Ako niste upoznati s Azure PowerShell, pročitajte članak [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

2.  Sljedeća naredba vraća sve usmjerava primjenjuje se na mrežnom sučelju pod nazivom *VM1 NIC1* u grupi resursa *RG1*.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Ako ne znate naziv mrežnog sučelja, upišite sljedeću naredbu za dohvaćanje imena svih sučelje mreže u resursa group.*

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    Izlaz za svaki usmjeravanje primjenjuje se na podmreži NIC-a je povezano s sliči sljedeći rezultat:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    Obratite pozornost na sljedeće u izlaz:
    - **Naziv**: naziv učinkovitih usmjeravanje možda je prazan, osim ako izričito navedeno, za korisnički definirane usmjerava. 
    - **Stanje**: pokazuje status učinkovitih usmjeravanje. Moguće vrijednosti su "Aktivan" ili "Nije valjana"
    - **AddressPrefixes**: Određuje prefiks adresu učinkovitih usmjeravanje CIDR notacijom. 
    - **nextHopType**: označava sljedeći put za zadani smjer. Moguće vrijednosti su *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*ili *Null*. Vrijednost *Null* za **nextHopType** u na UDR mogu upućivati koji nisu valjani usmjeravanje. Ako, na primjer, ako je **nextHopType** *VirtualAppliance* i virtualne mreže potražite VM nije u stanju dodjeli/pokrenut. Ako **nextHopType** je *VPNGateway* i nema nema pristupnika dodjeli/izvršavanje u određenom VNet, smjer može biti valjane.
    - **NextHopIpAddress**: određuje IP adresu sljedeći put učinkovitih smjera.
    
    Sljedeća naredba vraća smjerovima je lakše prikaz tablice:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    Sljedeći rezultat je dio izlaz primili scenariju prethodno opisan:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. Postoji smjera naveden na na *WestUS VNet3* VNet (prefiks 10.10.0.0/16)* * iz *WestUS VNet1* (prefiks 10.9.0.0/16) u Izlaz u prethodnom koraku. Kao što je prikazano na sljedećoj slici, VNet peering veza s VNet *WestUS VNet3* je u stanju *nije povezan* .
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Veza za dvosmjerni za na peering prekine, koja objašnjava zašto VM1 ne može povezati s VM3 u VNet *WestUS VNet3* . Ponovno postavljanje dvosmjerni VNet peering vezu za *WestUS VNet1* i *WestUS VNet3* VNets. Izlaz vraća kad vezu peering VNet pravilno uspostaviti slijedi:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Kad utvrdite problem, možete dodati, ukloniti, ili promijenite usmjerava i usmjeravanje tablice. Upišite sljedeću naredbu da biste vidjeli popis naredbi koje se koriste da biste to učinili:

        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Razmatranja

Što treba imati na umu prilikom pregleda popis smjerova vraća:

- Usmjeravanje temelji se na najdulje prefiks Match (LPM) između UDRs, BGP i sustava usmjerava. Ako postoji više od jedne usmjeravanje s istom podudaranje LPM, zatim rutu je odabran na temelju njezinu izvoru sljedećim redoslijedom:
    - Korisnički definirane usmjeravanje
    - Usmjeravanje BGP
    - Usmjeravanje sustava (zadano)

    S učinkovitih usmjerava možete vidjeti samo učinkovitih usmjerava koje se podudaraju LPM na temelju svih dostupnih usmjerava. Prikazom kako smjerovima zapravo vrednuju se za navedeni NIC Ovime se jednostavnije otklanjanje poteškoća s određenim usmjerava koji utječu povezivanje iz vaše VM.

- Ako imate UDRs i šaljete promet na mreži virtualne potražite (NVA) s *VirtualAppliance* kao **nextHopType**, provjerite je li da prosljeđivanje IP omogućena na NVA primanje promet ili ispuštaju pakete. 
- Ako je omogućen Forced tuneliranje, sve izlazne internetski promet moguće usmjeriti lokalnog. RDP/SSH s Interneta da biste na VM možda neće funkcionirati uz tu postavku, ovisno o način postupanja s lokalnim tog prometa. 
  Prisilno tuneliranje mogu biti omogućene:
    - Ako koristite web-mjesto VPN, postavljanjem korisnički definirane usmjeravanje (UDR) s nextHopType kao pristupnika VPN-a
    - Ako je zadani smjer objavljeno putem BGP
- Za VNet peering promet da bi ispravno funkcionirala usmjeravanje sustava s **nextHopType** *VNetPeering* mora postojati peered VNet prefiks raspona. Ako ne postoji takvo usmjeravanje i vezu peering VNet izgleda u redu:
    - Pričekajte nekoliko sekundi i pokušajte ponovno ako je upravo uspostaviti vezu peering. Povremeno traje dulje proširiti usmjerava sve mreže sučeljima podmreži.
    - Pravila mreže sigurnosne grupe (NSG) može biti koje utječu na promet tokova. Dodatne informacije potražite u članku [Otklanjanje poteškoća s mrežom sigurnosne grupe](virtual-network-nsg-troubleshoot-powershell.md) .
