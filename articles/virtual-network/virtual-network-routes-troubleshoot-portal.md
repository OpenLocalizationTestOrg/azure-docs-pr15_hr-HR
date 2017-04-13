<properties 
   pageTitle="Otklanjanje poteškoća s usmjerava – Portal | Microsoft Azure"
   description="Saznajte kako otkloniti poteškoće usmjerava u modelu implementaciju upravljanja resursima Azure pomoću portala za Azure."
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

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Otklanjanje poteškoća s usmjerava pomoću portala za Azure

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

1. Prijavite se na portal sustava Azure na https://portal.azure.com.
2. Kliknite **nove servise**, a zatim na popisu koji će se pojaviti kliknite **virtualnim računalima** .
3. Odaberite VM otklanjanje poteškoća s popisa koji će se pojaviti, a pojavljuje se VM plohu s mogućnostima.
4. Kliknite **Dijagnosticiranje & rješavanje problema s** , a zatim odaberite uobičajenih problema. U ovom primjeru **se moguće povezati s moj Windows VM** odabran. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. Koraci se prikazuju u odjeljku problem, kao što je prikazano na sljedećoj slici: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Kliknite *učinkovitih usmjerava* na popisu preporučena koraka.

6. **Učinkovita usmjerava** plohu pojavit će se, kao što je prikazano na sljedećoj slici:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Ako vaš VM sadrži samo jedan NIC, odabire se po zadanom. Ako imate više SLIKA, odaberite NIC-a za koji želite da biste pogledali učinkovitih usmjerava.

    >[AZURE.NOTE] Ako VM pridružene NIC-a nije u stanju izvršavanja, učinkovitih usmjerava se ne prikazuje. Samo prvih 200 učinkovitih usmjerava prikazane su na portalu. Za potpuni popis kliknite **Preuzmi**. Dodatno možete filtrirati rezultate iz preuzete .csv datoteke.

    Obratite pozornost na sljedeće u izlaz:
    - **Izvor**: navedena je vrsta usmjeravanje. Usmjerava sustava prikazuju se kao *zadani*, UDRs prikazuju se kao usmjerava *korisnika* i pristupnika (statične ili BGP) prikazuju se kao *VPNGateway*.
    - **Stanje**: pokazuje status učinkovitih usmjeravanje. Moguće vrijednosti su *aktivni* ili *nije valjana*.
    - **AddressPrefixes**: Određuje prefiks adresu učinkovitih usmjeravanje CIDR notacijom. 
    - **nextHopType**: označava sljedeći put za zadani smjer. Moguće vrijednosti su *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*ili *Null*. Vrijednost *Null* za **nextHopType** u na UDR mogu upućivati koji nisu valjani usmjeravanje. Ako, na primjer, ako je **nextHopType** *VirtualAppliance* i virtualne mreže potražite VM nije u stanju dodjeli/pokrenut. Ako **nextHopType** je *VPNGateway* i nema nema pristupnika dodjeli/izvršavanje u određenom VNet, smjer može biti valjane.
    
7. Postoji smjera navedenu na VNet *WestUS VNET3* (prefiks 10.10.0.0/16) iz na *WestUS VNet1* (prefiks 10.9.0.0/16) na slici u prethodnom koraku. Na sljedećoj slici vezu peering je u stanju *nije povezan* :
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Veza za dvosmjerni za na peering prekine, koja objašnjava zašto VM1 ne može povezati s VM3 u VNet *WestUS VNet3* .

8. Sljedeća slika prikazuje smjerovima nakon uspostave veze peering dvosmjerni:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Za otklanjanje poteškoća scenariji za prisilno tuneliranje i procjenu usmjeravanje pročitajte odjeljak [pitanja vezana uz](virtual-network-routes-troubleshoot-portal.md#Considerations) ovog članka.

### <a name="view-effective-routes-for-a-network-interface"></a>Prikaz učinkovitih usmjerava za mrežno sučelje

Ako je mrežni promet tijek utjecati za određeni mrežnog sučelja (NIC), možete pogledati cijeli popis učinkovitih usmjerava na na NIC izravno. Da biste vidjeli zbrajanja usmjerava koji se primjenjuju na NIC, slijedite sljedeće korake:

1. Prijavite se na portal sustava Azure na https://portal.azure.com.
2. Kliknite **Dodatne servise**, a zatim kliknite **sučelje mreže**
3. Pretražite popis za naziv programa NIC ili odaberite s popisa koji će se prikazati. U ovom se primjeru **VM1 NIC1** je odabrano.
4. Odaberite **učinkovitih usmjerava** plohu **mrežnog sučelja** kao što je prikazano na sljedećoj slici:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    **Opseg** zadane postavke da biste odabrali mrežnog sučelja.

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>Prikaz učinkovitih usmjerava za usmjeravanje tablicu

Prilikom izmjene korisnički definirane usmjerava (UDRs) u tablici usmjeravanje, preporučujemo vam da biste pregledali utjecaj usmjerava dodani na određeni VM. Tablica za usmjeravanje može biti povezan s bilo kojeg broja podmreže. Sada možete vidjeti sve važeće usmjerava za sve mrežne kartice zadani smjer tablice primijenjen, bez potrebe da biste se prebacili kontekstu iz tablice plohu zadani smjer.

U ovom primjeru UDR (*UDRoute*) navedeni su u tablici usmjeravanje (*UDRouteTable*). U ovom usmjeravanje šalje sve internetski promet iz *Subnet1* u VNet *WestUS VNet1* kroz na mreži virtualne potražite (NVA) u *Subnet2* iste VNet. Smjer kako je prikazano na sljedećoj slici:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Da biste vidjeli zbrajanja usmjerava za usmjeravanje tablicu, slijedite sljedeće korake:

1. Prijavite se na portal sustava Azure na https://portal.azure.com.
2. Kliknite **nove servise**, a zatim kliknite **smjer tablice**
3. Pretražite popis za usmjeravanje tablicu u kojoj želite da biste vidjeli zbrajanja usmjerava za i odaberite ga. U ovom se primjeru **UDRouteTable** je odabrano. Plohu za usmjeravanje odabrane tablice prikazuje se kao što je prikazano na sljedećoj slici:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. Odaberite **Učinkovitih usmjerava** plohu **usmjeravanje tablice** . **Opseg** postavljen je na tablice usmjeravanje koju ste odabrali.
5. Usmjeravanje tablicu primjenjuje se na više podmreže. Odaberite **podmreže** na koje želite pregledati na popisu. U ovom se primjeru **Subnet1** je odabrano.
6. Odaberite **mrežno sučelje**. Navedene su sve NIC-ovi s odabranog podmreže. U ovom se primjeru **VM1 NIC1** je odabrano.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Ako NIC-a nije povezan s izvodi VM, prikazuju se bez učinkovitih usmjerava.

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
    - Pravila mreže sigurnosne grupe (NSG) može biti koje utječu na promet tokova. Dodatne informacije potražite u članku [Otklanjanje poteškoća s mrežom sigurnosne grupe](virtual-network-nsg-troubleshoot-portal.md) .
