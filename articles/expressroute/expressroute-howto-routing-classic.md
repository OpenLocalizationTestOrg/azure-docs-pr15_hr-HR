<properties
   pageTitle="Kako konfigurirati usmjeravanja za je elektronička ExpressRoute za uvođenje klasičnog modela pomoću komponente PowerShell | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz korake za stvaranje i dodjela resursa u privatno, javno i Microsoft peering od programa elektronička ExpressRoute. U ovom se članku se objašnjava da biste provjerili status, ažuriranje i brisanje peerings za vaše elektronička."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Stvaranje i izmjena usmjeravanja za je elektronička ExpressRoute

> [AZURE.SELECTOR]
[Azure portala - resursima](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell – resursima](expressroute-howto-routing-arm.md)
[PowerShell – klasični](expressroute-howto-routing-classic.md)



U ovom se članku vodit će vas kroz korake za stvaranje i upravljanje usmjeravanje Konfiguracija programa elektronička ExpressRoute pomoću komponente PowerShell i model klasični implementacije. Koraci u nastavku i vidjet ćete kako provjeriti stanje, ažuriranje ili brisanje pa deprovision peerings za je elektronička ExpressRoute.


**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Preduvjeti za konfiguraciju

- Morate najnoviju verziju modula Azure PowerShell. Najnovije modul ljuske PowerShell možete preuzeti iz odjeljka PowerShell [stranice s preuzimanjima Azure](https://azure.microsoft.com/downloads/). Slijedite upute na stranici [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) postupne upute o tome kako konfigurirati računalo da biste koristili modula Azure PowerShell. 
- Pripazite da pregledate stranici [preduvjeti](expressroute-prerequisites.md) , stranice [usmjeravanju preduvjeti](expressroute-routing.md) i stranica [tijekova rada](expressroute-workflows.md) prije nego što počnete konfiguracije.
- Mora imati aktivan elektronička za ExpressRoute. Slijedite upute da biste [stvorili je elektronička ExpressRoute](expressroute-howto-circuit-classic.md) i imati elektronička omogućeno vaš davatelj povezivanje prije nastavka. Elektronička ExpressRoute mora biti u stanju dodijeljenu i omogućena za moći pokretati Cmdlete opisanim.

>[AZURE.IMPORTANT] Ove se upute odnose samo na krugova stvorene pomoću davateljima usluga koja nudi servisa za povezivanje sloja 2. Ako koristite usluga koja nudi upravljanih Layer 3 services (obično je IPVPN, kao što je MPLS), vaš davatelj usluga za povezivanje će konfigurirati i upravljanje usmjeravanje umjesto vas.

Možete konfigurirati jednu, dvije ili sve tri peerings (Azure privatne, Azure javan i Microsoft) za je elektronička ExpressRoute. Možete konfigurirati peerings u nekom redoslijedu koje odaberete. Međutim, morate učiniti da ste dovršili konfiguraciju svaki peering jedan po jedan. 

## <a name="azure-private-peering"></a>Azure privatne peering

U ovom se odjeljku daju upute o tome kako stvoriti, se, ažuriranje i brisanje Azure privatne peering konfiguracija za je elektronička ExpressRoute. 

### <a name="to-create-azure-private-peering"></a>Da biste stvorili Azure privatne peering

1. **Uvoz modul ljuske PowerShell za ExpressRoute.**
    
    Da biste počeli koristiti cmdleta ExpressRoute morate uvesti modula Azure i ExpressRoute u sesiju ljuske PowerShell. Pokrenite sljedeće naredbe da biste uvezli modula Azure i ExpressRoute u sesiju ljuske PowerShell.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Stvaranje je elektronička ExpressRoute.**
    
    Slijedite upute da biste stvorili [ExpressRoute elektronička](expressroute-howto-circuit-classic.md) i ga dodjeli davatelj povezivanje. Ako vaš davatelj usluga za povezivanje nudi upravljanih Layer 3 services, možete zatražiti davatelja povezivanje da biste omogućili Azure privatne peering umjesto vas. U tom slučaju nećete morati slijedite upute navedene u sljedećim odjeljcima. Međutim, ako vaš davatelj usluga za povezivanje upravljanje usmjeravanja, nakon stvaranja elektronička, slijedite upute u nastavku. 

3. **Provjerite elektronička ExpressRoute da biste bili sigurni da su mu dodijeljeni resursi.**

    Morate najprije provjerite je li elektronička ExpressRoute dodjeli te omogućuje. Pogledajte primjer u nastavku.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Provjerite je li u elektronička prikazuju se kao Provisioned i omogućeno. Ako ga ne funkcioniraju s davateljem povezivanje da biste dobili vaše elektronička potrebna stanje i status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfiguriranje Azure privatne peering za na elektronička.**

    Pripazite da imate sljedeće stavke prije nego što nastavite sa sljedećim koracima:

    - Na /30 podmreže primarni veze. To ne smije biti dio prostor bilo koje adrese rezervirano virtualna mrežu.
    - Na /30 podmreže sekundarne veze. To ne smije biti dio prostor bilo koje adrese rezervirano virtualna mrežu.
    - Valjani VLAN ID da biste uspostavili ovaj peering na. Provjerite da nema drugih peering u na elektronička koristi isti VLAN ID.
    - KAO broj za peering. Možete koristiti 2-bajtni i 4-bajtnim kao brojevi. Možete koristiti privatnu kao broj za ovaj peering. Provjerite je li se pomoću 65515.
    - Programa MD5 raspršivanje Ako odaberete da biste koristili neki. **Ovo nije obavezno**.
    
    Možete pokrenuti sljedeći cmdlet da biste konfigurirali Azure privatne peering za vaše elektronička.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Cmdlet u nastavku možete koristiti ako odlučite koristiti programa MD5 raspršivanje.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Provjerite je li vaš telefon kao odredite kao peering ASN, ne klijente ASN.

### <a name="to-view-azure-private-peering-details"></a>Da biste pogledali Azure privatne peering detalje

Možete dohvatiti pojedinosti konfiguracije pomoću sljedeći cmdlet

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Da biste ažurirali Azure privatne peering konfiguracija

Bilo koji dio konfiguracije pomoću sljedeći cmdlet možete ažurirati. U primjeru u nastavku ID VLAN na elektronička ažurira od 100 za 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Da biste izbrisali Azure privatne peering

Konfiguraciju peering možete ukloniti tako da pokrenete sljedeći cmdlet.

>[AZURE.WARNING] Osigurati sve virtualne mreže jesu li nepovezana iz elektronička ExpressRoute prije pokretanja ovaj cmdlet. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure javno peering

U ovom se odjeljku daju upute o tome kako stvoriti, se, ažuriranje i brisanje Azure javno peering konfiguracija za je elektronička ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Da biste stvorili Azure javno peering

1. **Uvoz modul ljuske PowerShell za ExpressRoute.**
    
    Da biste počeli koristiti cmdleta ExpressRoute morate uvesti modula Azure i ExpressRoute u sesiju ljuske PowerShell. Pokrenite sljedeće naredbe da biste uvezli modula Azure i ExpressRoute u sesiju ljuske PowerShell. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Stvaranje je elektronička ExpressRoute**
    
    Slijedite upute da biste stvorili [ExpressRoute elektronička](expressroute-howto-circuit-classic.md) i ga dodjeli davatelj povezivanje. Ako vaš davatelj usluga za povezivanje nudi upravljanih Layer 3 services, možete zatražiti davatelja povezivanje da biste omogućili Azure javno peering umjesto vas. U tom slučaju nećete morati slijedite upute navedene u sljedećim odjeljcima. Međutim, ako vaš davatelj usluga za povezivanje upravljanje usmjeravanja, nakon stvaranja elektronička, slijedite upute u nastavku.

3. **Provjera ExpressRoute elektronička da biste bili sigurni da su mu dodijeljeni resursi**

    Morate najprije provjerite je li elektronička ExpressRoute dodjeli te omogućuje. Pogledajte primjer u nastavku.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Provjerite je li u elektronička prikazuju se kao Provisioned i omogućeno. Ako ga ne funkcioniraju s davateljem povezivanje da biste dobili vaše elektronička potrebna stanje i status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Konfiguriranje Azure javno peering za na elektronička**

    Provjerite možete li se sljedeće informacije prije nego što nastavite Dodatno.

    - Na /30 podmreže primarni veze. To mora biti valjani javno IPv4 prefiks.
    - Na /30 podmreže sekundarne veze. To mora biti valjani javno IPv4 prefiks.
    - Valjani VLAN ID da biste uspostavili ovaj peering na. Provjerite da nema drugih peering u na elektronička koristi isti VLAN ID.
    - KAO broj za peering. Možete koristiti 2-bajtni i 4-bajtnim kao brojevi.
    - Programa MD5 raspršivanje Ako odaberete da biste koristili neki. **Ovo nije obavezno**.
    
    Možete pokrenuti sljedeći cmdlet da biste konfigurirali Azure javno peering za svoje sklopovske

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    Cmdlet u nastavku možete koristiti ako se odlučite za korištenje programa raspršivanje MD5

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Provjerite je li vaš telefon kao odredite kao peering ASN, a ne klijente ASN.

### <a name="to-view-azure-public-peering-details"></a>Da biste pogledali Azure javno peering detalje

Možete dohvatiti pojedinosti konfiguracije pomoću sljedeći cmdlet

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Da biste ažurirali Azure javno peering konfiguracija

Možete ažurirati bilo koji dio konfiguracije pomoću sljedeći cmdlet

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

ID VLAN na elektronička se ažuriraju od 200 600 u primjeru iznad.

### <a name="to-delete-azure-public-peering"></a>Da biste izbrisali Azure javno peering

Konfiguraciju peering možete ukloniti tako da pokrenete sljedeći cmdlet

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft peering

U ovom se odjeljku daju upute o tome kako stvoriti, se, ažuriranje i brisanje Microsoft peering configuration je elektronička ExpressRoute. 

### <a name="to-create-microsoft-peering"></a>Da biste stvorili Microsoft peering

1. **Uvoz modul ljuske PowerShell za ExpressRoute.**
    
    Da biste počeli koristiti cmdleta ExpressRoute morate uvesti modula Azure i ExpressRoute u sesiju ljuske PowerShell. Pokrenite sljedeće naredbe da biste uvezli modula Azure i ExpressRoute u sesiju ljuske PowerShell.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Stvaranje je elektronička ExpressRoute**
    
    Slijedite upute da biste stvorili [ExpressRoute elektronička](expressroute-howto-circuit-classic.md) i ga dodjeli davatelj povezivanje. Ako vaš davatelj usluga za povezivanje nudi upravljanih Layer 3 services, možete zatražiti davatelja povezivanje da biste omogućili Azure privatne peering umjesto vas. U tom slučaju nećete morati slijedite upute navedene u sljedećim odjeljcima. Međutim, ako vaš davatelj usluga za povezivanje upravljanje usmjeravanja, nakon stvaranja elektronička, slijedite upute u nastavku.

3. **Provjera ExpressRoute elektronička da biste bili sigurni da su mu dodijeljeni resursi**

    Morate se najprije provjerite je li elektronička ExpressRoute Provisioned i omogućeno stanje.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Provjerite je li u elektronička prikazuju se kao Provisioned i omogućeno. Ako ga ne funkcioniraju s davateljem povezivanje da biste dobili vaše elektronička potrebna stanje i status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfiguriranje Microsoft peering za na elektronička**

    Pripazite da imate sljedeće informacije prije nastavka.

    - Na /30 podmreže primarni veze. To mora biti valjani javno IPv4 prefiks vlasništvu i registrirana u programa RIR / IRR.
    - Na /30 podmreže sekundarne veze. To mora biti valjani javno IPv4 prefiks vlasništvu i registrirana u programa RIR / IRR.
    - Valjani VLAN ID da biste uspostavili ovaj peering na. Provjerite da nema drugih peering u na elektronička koristi isti VLAN ID.
    - KAO broj za peering. Možete koristiti 2-bajtni i 4-bajtnim kao brojevi.
    - Objavljeno prefiksi: Navedite popis svih prefiksi namjeravate Oglasite putem BGP sesiju. Samo javnu IP adresa prefiksi prihvaćaju. Ako planirate slati skup prefiksi možete poslati popis s vrijednostima odvojenima zarezom. Ove prefiksi mora biti registriran vam u programa RIR / IRR.
    - Korisnička ASN: Ako ste prefiksi oglašavanje koji nije registriran na peering kao broj, možete odrediti kao broj na koji su registrirane. **Ovo nije obavezno**.
    - Usmjeravanje naziv u registru: Možete odrediti na RIR / IRR prema kojima je kao što je broj i prefiksi registrirane.
    - Programa MD5 raspršivanje, ako odlučite koristiti jedan. **Ovo nije obavezno.**
    
    Možete pokrenuti sljedeći cmdlet da biste konfigurirali Microsoft pering za svoje sklopovske

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Da biste pogledali Microsoft peering detalje

Detalji o konfiguraciji pomoću sljedeći cmdlet možete dobiti.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Da biste ažurirali Microsoft peering konfiguracija

Bilo koji dio konfiguracije pomoću sljedeći cmdlet možete ažurirati.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Da biste izbrisali Microsoft peering

Konfiguraciju peering možete ukloniti tako da pokrenete sljedeći cmdlet.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Daljnji koraci

Sljedeće [veze VNet da biste je elektronička ExpressRoute](expressroute-howto-linkvnet-classic.md).


-  Dodatne informacije o tijekovima rada potražite u članku [ExpressRoute tijekova rada](expressroute-workflows.md).
-  Dodatne informacije o elektronička peering potražite u članku [ExpressRoute krugova i usmjeravanje domene](expressroute-circuit-peerings.md).

