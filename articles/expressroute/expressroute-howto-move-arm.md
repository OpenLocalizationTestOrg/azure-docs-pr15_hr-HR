<properties
   pageTitle="Premještanje krugova ExpressRoute od klasičnog upravitelju resursa | Microsoft Azure"
   description="Ova stranica opisuje kako premjestiti klasični elektronička model implementacije Voditelj resursa."
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Premještanje ExpressRoute krugova s na klasični model implementacije Voditelj resursa

## <a name="configuration-prerequisites"></a>Preduvjeti za konfiguraciju

- Morate najnoviju verziju modula Azure PowerShell (najmanje verzije 1.0).
- Pripazite da pregledate [preduvjeti](expressroute-prerequisites.md), [usmjeravanje preduvjeti](expressroute-routing.md)i [tijekove rada](expressroute-workflows.md) prije nego što počnete konfiguracije.
- Prije ispred Dodatno, pregledajte informacije o u odjeljku [Premještanje je elektronička ExpressRoute iz klasični upravitelju resursa](expressroute-move.md). Provjerite je li da koje su potpuno prepoznata ograničenja i ograničenja koja je moguće.
- Ako želite da biste premjestili programa Azure ExpressRoute elektronička iz modela uvođenje klasičnog model implementacije Azure Voditelj resursa, morate imati elektronička potpuno konfiguriran i radu u modelu klasični implementacije.
- Provjerite je li grupa resursa koja je stvorena u model implementacije Voditelj resursa.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Premjestite elektronička ExpressRoute model implementacije Voditelj resursa

Morate premjestiti je elektronička ExpressRoute model implementacije Voditelj resursa tako da ga možete koristiti na klasični i implementaciju modelima Voditelj resursa. To možete učiniti pokretanjem sljedeće naredbe ljuske PowerShell.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Korak 1: Prikupljanje elektronička pojedinosti iz modela klasični implementacije

Morate najprije prikupljanje podataka o vašem elektronička ExpressRoute.

Prijavite se u Azure klasični okruženje i prikupite ključa usluge. Možete koristiti sljedeće komponente PowerShell isječak za prikupljanje informacija:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Kopirajte **ključa usluge** elektronička koju želite premjestiti u model implementacije Voditelj resursa.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Korak 2: Prijavite se u okruženje za upravljanje resursima i stvorite novu grupu resursa

Možete stvoriti novu grupu resursa pomoću sljedećih isječak:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Postojeću grupu resursa možete koristiti i ako već postoji.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Korak 3: Premještanje elektronička ExpressRoute model za uvođenje Voditelj resursa

Sada ste spremni za premještanje putem vaše elektronička ExpressRoute iz na klasični model implementacije Voditelj resursa. Pregledajte informacije navedene u odjeljku [Premještanje je elektronička ExpressRoute od klasičnog model implementacije resursima](expressroute-move.md) prije nastavka Dodatno.

To možete učiniti tako da pokrenete sljedeći isječak:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Kada se završi, novi naziv koji je naveden u prethodno cmdlet će se koristiti za adrese resurs. Na elektronička zapravo moguće preimenovati.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Omogućivanje programa elektronička ExpressRoute za oba modele implementacije

Morate premjestiti vaše elektronička ExpressRoute implementaciju model upravljanja resursima prije pristupom model implementacije.

Pokrenite sljedeći cmdlet da biste omogućili pristup i implementaciju modela:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Kada ovaj postupak uspješno je dovršena, moći da biste pogledali na elektronička u modelu klasični implementacije.

Pokrenite sljedeće da biste vidjeli detalje elektronička ExpressRoute:

    get-azurededicatedcircuit

Morate moći vidjeti ključa usluge na popisu. Sada mogu upravljati veze na elektronička ExpressRoute pomoću vaše standardne klasični implementacije model za klasični VNets i na standardni ARM naredbe za VNETs OKVIRA. U sljedećim člancima će vas voditi kroz Upravljanje vezama na elektronička ExpressRoute:

- [Povezivanje virtualne mreže vaše elektronička ExpressRoute u modelu implementacije Voditelj resursa](expressroute-howto-linkvnet-arm.md)
- [Povezivanje virtualne mreže vaše elektronička ExpressRoute u modelu klasični implementacije](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Onemogućivanje elektronička ExpressRoute model klasični implementacije

Pokrenite sljedeći cmdlet da biste onemogućili pristup modelu klasični implementacije:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nakon operacija je uspješno dovršena, nećete moći pregledati na elektronička u modelu klasični implementacije.

## <a name="next-steps"></a>Daljnji koraci

Kada stvorite vaše elektronička, provjerite je li da to učinite sljedeće:

- [Stvaranje i izmjena usmjeravanja za vaše elektronička ExpressRoute](expressroute-howto-routing-arm.md)
- [Povezivanje virtualne mreže vaše elektronička ExpressRoute](expressroute-howto-linkvnet-arm.md)
