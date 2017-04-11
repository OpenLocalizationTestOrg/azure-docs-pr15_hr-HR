<properties
   pageTitle="Stvaranje i izmjena je elektronička ExpressRoute pomoću upravitelja resursa i PowerShell | Microsoft Azure"
   description="U ovom se članku opisuje kako stvoriti, dodijelite, provjerite je li, ažuriranje, brisanje i deprovision je elektronička ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Stvaranje i izmjena je elektronička ExpressRoute

> [AZURE.SELECTOR]
[Azure portala - resursima](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell – resursima](expressroute-howto-circuit-arm.md)
[PowerShell – klasični](expressroute-howto-circuit-classic.md)


U ovom se članku opisuje kako stvoriti programa Azure ExpressRoute elektronička pomoću cmdleta ljuske Windows PowerShell i model implementacije Azure Voditelj resursa. U ovom se članku i vidjet ćete kako Provjera statusa u elektronička, ažurirati, ili brisanje i deprovision ga.

**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Prije početka


- Nabavite najnoviju verziju modula Azure PowerShell (najmanje verzije 1.0). Detaljne upute o tome kako konfigurirati računalo da biste koristili moduli PowerShell slijedite upute [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

- Pregledajte [preduvjeti](expressroute-prerequisites.md) i [tijekove rada](expressroute-workflows.md) prije nego što počnete konfiguracije.

## <a name="create-and-provision-an-expressroute-circuit"></a>Stvaranje i Dodjela je elektronička ExpressRoute

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. prijavite se na račun za Azure, a zatim odaberite pretplate

Da biste započeli s konfiguracijom, prijavite se na račun za Azure. Dodatne informacije o PowerShell potražite u članku [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md). Pomoću sljedećih primjera možete povezati:

    Login-AzureRmAccount

Provjerite pretplate za račun:

    Get-AzureRmSubscription

Odaberite pretplatu u koju želite stvoriti je elektronička ExpressRoute za:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. dohvatiti popis podržanih davatelje usluga, mjesta i bandwidths

Stvorite je elektronička ExpressRoute, morate na popisu podržanih povezivanje davatelji, mjesta i mogućnosti propusnosti.

Cmdlet ljuske PowerShell `Get-AzureRmExpressRouteServiceProvider` vraća ove informacije koje će se koristiti u noviji korake:

    Get-AzureRmExpressRouteServiceProvider

Provjerite nalazi li se vaš davatelj usluga za povezivanje dva. Zabilježite sljedeće informacije jer morat ćete ga kasnije prilikom stvaranja je elektronička:

- Ime

- PeeringLocations

- BandwidthsOffered

Sada ste spremni stvoriti je elektronička ExpressRoute.   

### <a name="3-create-an-expressroute-circuit"></a>3. stvaranje je elektronička ExpressRoute

Ako još nemate grupu resursa, morate stvoriti jednu prije stvaranja vaše elektronička ExpressRoute. To možete učiniti tako da pokrenete sljedeću naredbu:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Sljedeći primjer pokazuje kako stvoriti 200 Mbps elektronička ExpressRoute kroz Equinix u Silicon udubljenje. Ako koristite nekog drugog davatelja usluge i različite postavke, zamijenite te podatke kada vaš zahtjev. Slijedi primjer traženje novog ključa servisa:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Provjerite je li navedite točne SKU sloju i obitelji SKU:

- SKU sloju određuje hoće li je omogućen ExpressRoute standard ili premium dodatak ExpressRoute. Možete navesti *standardne* da biste dobili standardne SKU ili *Premium* za dodatak premium.

- Obitelj SKU određuje vrstu naplate. Možete navesti *Metereddata* za tarifu s ograničenim prometom podataka i *Unlimiteddata* za tarifu neograničeno podataka. Imajte na umu da možete promijeniti vrstu naplate iz *Metereddata* u *Unlimiteddata*, ali ne možete promijeniti vrstu iz *Unlimiteddata* za *Metereddata*.


>[AZURE.IMPORTANT] Vaš elektronička ExpressRoute će se naplatiti od trenutka izdavanja ključa usluge. Provjerite je li kada davatelj usluga za povezivanje je spremna za dodjelu resursa u elektronička izvođenje ovog postupka.

Odgovor sadrži ključa usluge. Detaljne opise sve parametre možete dobiti pokretanjem sljedeće:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. popis svih ExpressRoute krugova

Da biste dobili popis krugova ExpressRoute koji ste stvorili, pokrenite na `Get-AzureRmExpressRouteCircuit` naredba:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Odgovor izgledat će slično kao u sljedećem primjeru:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Ove informacije u bilo kojem trenutku možete dohvatiti pomoću na `Get-AzureRmExpressRouteCircuit` cmdlet. Uputite poziv pomoću bez parametara navodi sve krugova. Ključ servisa će se prikazati u polju *ServiceKey* :


    Get-AzureRmExpressRouteCircuit


Odgovor izgledat će slično kao u sljedećem primjeru:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Detaljne opise sve parametre možete dobiti pokretanjem sljedeće:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. poslati ključa usluge povezivanja davatelju za dodjelu resursa

*ServiceProviderProvisioningState* daje informacije o trenutnom stanju aktiviranja na strani davatelja usluga. Status nudi stanje na strani Microsoft. Dodatne informacije o elektronička dodjeljivanje stanja potražite u članku [Tijekovi rada](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Prilikom stvaranja novog elektronička ExpressRoute na elektronička bit će u stanju sljedeće:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Na elektronička će se promijeniti sljedeće stanje kada je u tijeku omogućiti za koje je davatelj usluga za povezivanje:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Umjesto vas moći koristiti u ExpressRoute elektronička mora biti u stanju sljedeće:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. povremeno status i stanje ključa elektronička

Provjera statusa i stanje ključa elektronička obavještava kada vaš davatelj usluga omogućio vaše elektronička. Nakon na elektronička konfiguriran, pojavit će se *ServiceProviderProvisioningState* kao *Provisioned*, kao što je prikazano u sljedećem primjeru:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Odgovor izgledat će slično kao u sljedećem primjeru:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                    }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. stvaranje konfiguraciju usmjeravanje

Detaljne upute potražite u članku [konfiguracija za usmjeravanje elektronička ExpressRoute](expressroute-howto-routing-arm.md) za stvaranje i mijenjanje elektronička peerings.


>[AZURE.IMPORTANT] Ove se upute odnose samo na krugova koje su stvorene pomoću davateljima usluga koje nude usluge za povezivanje 2 sloja. Ako koristite usluga koje nudi upravljanih slojeva 3 services (obično je IP VPN, kao što je MPLS), vaš davatelj usluga za povezivanje će konfigurirati i upravljati njima usmjeravanje umjesto vas.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. povezati virtualne mreže je elektronička ExpressRoute

Nakon toga veza virtualne mreže vaše elektronička ExpressRoute. Pomoću članka [Linking virtualne mreže ExpressRoute krugova](expressroute-howto-linkvnet-arm.md) kada radite s modelom implementacije Voditelj resursa.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Početak status je elektronička ExpressRoute

Ove informacije u bilo kojem trenutku možete dohvatiti pomoću na `Get-AzureRmExpressRouteCircuit` cmdlet. Uputite poziv pomoću bez parametara navodi sve krugova.

    Get-AzureRmExpressRouteCircuit


Odgovor bit će slično kao u sljedećem primjeru:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Informacije o određenim elektronička ExpressRoute možete dobiti prosljeđivanjem naziv grupe resursa i naziv elektronička kao parametar poziv:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Odgovor izgledat će slično kao u sljedećem primjeru:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Detaljne opise sve parametre možete dobiti pokretanjem sljedeće:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>Izmjena je elektronička ExpressRoute

Neka svojstva programa ExpressRoute elektronička možete izmijeniti bez utjecaja povezivanje.

Možete učiniti sljedeće s bez nedostupnost:

- Omogućite ili onemogućite dodatak premium ExpressRoute za svoje sklopovske ExpressRoute.
- Povećanje propusnosti vaše elektronička ExpressRoute. Imajte na umu da prijelaz na stariju propusnosti je elektronička nije podržana.
- Promjena mjerne plana iz podataka s ograničenim prometom neograničeno podataka. Imajte na umu da promjena mjerne tarifu iz neograničeno podataka s ograničenim prometom podataka nije podržana.
-  Možete omogućiti i onemogućiti *Dopusti klasični operacije*.

Dodatne informacije o ograničenjima i ograničenja odnose se na [Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Da biste omogućili dodatak premium ExpressRoute

Možete omogućiti dodatak premium ExpressRoute za svoje postojeće elektronička pomoću sljedeće komponente PowerShell isječak:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Na elektronička imat će značajke ExpressRoute premium dodatak omogućen. Imajte na umu da će početak naplate za mogućnost dodatak premium čim uspješno pokrenuta naredbu.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Da biste onemogućili premium dodatak ExpressRoute

>[AZURE.IMPORTANT] Ovaj postupak možete neće uspjeti ako koristite resursa koje su veće od što je dopušteno za standardne elektronička.

Imajte na umu sljedeće:

- Prije nego što ste prijeći na nižu od premium Standard, koje morate provjerite je li broj virtualne mreže koji su povezani s instalacijom manje od 10. Ako to ne učinite, zahtjevom za ažuriranje ne uspije, a koje će šaljemo pri premium stope.

- Sve virtualne mreže u drugim regijama Geopolitički morate prekinuti vezu. Ako to ne učinite, neće uspjeti zahtjevom za ažuriranje, a koje će šaljemo pri premium stope.

- Usmjeravanje tablici mora biti manja od 4000 usmjerava za privatne peering. Ako je vaš Veličina tablice usmjeravanje veći od 4000 usmjerava, sesiju BGP ispušta i neće biti reenabled dok je broj oglašenu prefiksi dolazi ispod 4000.

Dodatak za premium ExpressRoute za postojeće elektronička možete onemogućiti pomoću cmdleta komponente PowerShell sustava sljedeće:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Da biste ažurirali elektronička propusnosti ExpressRoute

Mogućnosti podržane propusnosti za svog davatelja usluga provjerite [Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md). Možete odabrati bilo koje veličine veći od veličina vaše postojeće elektronička.

>[AZURE.IMPORTANT] Ne možete smanjiti propusnosti programa ExpressRoute elektronička bez prekidu. Prijelaz na stariju propusnosti zahtijeva deprovision elektronička ExpressRoute i reprovision nove ExpressRoute elektronička.

Kada odlučite koju veličinu morate, koristite sljedeću naredbu da biste promijenili veličinu vaše elektronička:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Vaša elektronička će veličine prema gore na strani Microsoft. Zatim morate se obratiti davatelja povezivanje da biste ažurirali konfiguracije kod sebe da odgovara tu promjenu. Kad unesete ta se obavijest, ne možemo započet će vam naplate za mogućnost ažurirane propusnosti.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Da biste premjestili SKU iz s ograničenim prometom na neograničeno

SKU je elektronička ExpressRoute možete promijeniti pomoću sljedećih isječak PowerShell:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Da biste kontrolirali pristup na klasični i resursima okruženja  

Pregledajte upute navedene u članku [Premještanje ExpressRoute krugova od klasičnog model implementacije Voditelj resursa](expressroute-howto-move-arm.md).  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning i brisanje je elektronička ExpressRoute

Imajte na umu sljedeće:

- Morate prekinuti vezu virtualne mreže s sklopovske ExpressRoute. Ako ovaj postupak ne uspije, provjerite ako sve virtualne mreže povezani s instalacijom.

- Ako je ExpressRoute elektronička servisa davatelja dodjele resursa stanje **Provisioning** ili **Provisioned** morate raditi s vaš davatelj usluga za deprovision elektronička kod sebe. Nastavit ćemo rezervirati resurse i fakturu dok davatelj usluga dovršava deprovisioning na elektronička i obavještava us.

- Ako davatelj usluga sadrži pretplati su uklonjeni resursi za elektronička (stanje dodjele resursa servisa davatelja postavljeno je na **nisu dodijeljeni resursi**) na elektronička moći ćete izbrisati. Time će se zaustaviti naplatom za na elektronička

Vaša elektronička ExpressRoute možete izbrisati tako da pokretanjem sljedeće naredbe:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Daljnji koraci

Kada stvorite vaše elektronička, provjerite je li da to učinite sljedeće:

- [Stvaranje i izmjena usmjeravanja za vaše elektronička ExpressRoute](expressroute-howto-routing-arm.md)
- [Povezivanje virtualne mreže vaše elektronička ExpressRoute](expressroute-howto-linkvnet-arm.md)
