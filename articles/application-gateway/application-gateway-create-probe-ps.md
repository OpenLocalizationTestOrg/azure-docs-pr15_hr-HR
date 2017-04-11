<properties
   pageTitle="Stvaranje prilagođene probni za pristupnik za aplikaciju pomoću komponente PowerShell u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako stvoriti prilagođene probni za pristupnik za aplikaciju pomoću komponente PowerShell u upravitelju resursa"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Stvaranje prilagođene probni za Azure računala pristupnika pomoću ljuske PowerShell za Azure Voditelj resursa

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-probe-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-probe-ps.md)
- [Azure klasični PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][uvođenje klasičnog modela](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Korak 1

Prijava AzureRmAccount koristiti za provjeru autentičnosti.

    Login-AzureRmAccount

### <a name="step-2"></a>Korak 2

Provjerite pretplate za račun.

    Get-AzureRmSubscription

### <a name="step-3"></a>Korak 3

Odabir pretplate Azure da biste koristili. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Korak 4

Stvorite grupu resursa (preskoči ovaj korak ako koristite postojeću grupu resursa).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Voditelj resursa zahtijeva da svih grupa resursa navedite mjesto. To mjesto koristi se kao zadano mjesto za resurse u toj grupi resursa. Pripazite da sve naredbe za stvaranje pristupnika za aplikaciju koristiti istu grupu resursa.

U gornjem primjeru koju smo stvorili grupu resursa pod nazivom "appgw ru" i mjesto "Zapad NAM".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Stvaranje virtualne mreže i podmreže za pristupnik za aplikaciju

Sljedeći koraci stvorite virtualne mreže i podmreže za pristupnik za aplikacije.

### <a name="step-1"></a>Korak 1


Raspon 10.0.0.0/24 adresa dodijeliti podmreže varijabla koja će se koristiti za stvaranje virtualne mreže.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Korak 2

Stvaranje virtualne mreže pod nazivom "appgwvnet" u resursa grupe "appgw-ru" za područje Zapad SAD-a pomoću 10.0.0.0/16 prefiks 10.0.0.0/24 podmreže.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Korak 3

Dodijelite varijabla podmreže za sljedeće korake koji stvaranje pristupnika za aplikacije.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Stvaranje javne IP adresa za konfiguraciju sučelja


Stvaranje javne IP resursa "publicIP01" u resursa grupe "appgw-ru" za područje Zapad SAD-a.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Stvaranje objekta konfiguracije pristupnika za aplikaciju pomoću prilagođenih probni

Postavljanje svih stavki konfiguracije prije stvaranja aplikacije pristupnika. Sljedeći koraci stvorite konfiguracije stavke koje su vam potrebne pristupnika resursa aplikacije.

### <a name="step-1"></a>Korak 1

Stvaranje konfiguracije aplikacije pristupnik IP pod nazivom "gatewayIP01". Prilikom pokretanja aplikacije pristupnika uzima IP adresu iz podmreže konfigurirati i mrežni promet usmjerili na IP adresa u skupna pozadinsku IP. Imajte na umu da svaku instancu traje jednu IP adresu.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Korak 2


Konfiguriranje skupna pozadinsku IP adresa pod nazivom "pool01" s IP adresama "134.170.185.46, 134.170.188.221,134.170.185.50". Te se vrijednosti su IP adrese koje primate mrežnog prometa koji dolazi s krajnju točku sučelja IP. Zamjena IP adrese da biste dodali vlastite krajnje točke aplikacije IP adresa.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Korak 3


Prilagođeni probni konfiguriran u ovom ćete koraku.

Parametri korišteni su:

- **Interval** - konfigurira provjere interval probni u sekundama.
- **Prekoračenje vremena** - definira isteka probni za provjeru za odgovor na HTTP-a.
- **Naziv glavnog računala:- i put** – put cijeli URL koji se poziva pristupnik za računala da biste odredili stanje instance. Ako, na primjer, ako imate web-mjesto http://contoso.com/, zatim prilagođene probni možete konfigurirati za "http://contoso.com/path/custompath.htm" za probni provjere da bi uspješno HTTP odgovor.
- **UnhealthyThreshold** – broj neuspjelih HTTP odgovora potrebne da biste označili instancu pozadinske kao *dobro*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Korak 4

Konfiguriranje postavki pristupnika na aplikaciju "poolsetting01" za promet u pozadinskoj. Ovaj korak ima i konfiguracija isteka za skup pozadinske odgovor na zahtjev za pristupnik za aplikacije. Kada odgovor pozadinske dodirne vremenskog ograničenja, aplikacija pristupnika otkazuje zahtjev. Ta vrijednost razlikuje se od probni isteka koji su samo za odgovor pozadinske isprobati provjere.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Korak 5

Konfiguriranje sučelja priključak IP pod nazivom "frontendport01" za krajnju točku javnu IP.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Korak 6

Stvaranje pristupne IP konfiguracija pod nazivom "fipconfig01" i povezivanje na javnu IP adresu s pristupne IP konfiguracija.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Korak 7

Stvaranje ga slušatelj naziv "listener01" i pridruživanje sučelja priključak sučelja IP konfiguracije.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Korak 8

Stvaranje učitavanja opterećenja usmjeravanje pravilo pod nazivom "rule01", koji se konfigurira funkcioniranje raspoređivača učitavanja.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Korak 9

Konfiguracija instancu veličine pristupnika aplikacije.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  Zadane vrijednosti za *InstanceCount* je 2, s maksimalnom vrijednošću od 10. Zadana vrijednost za *GatewaySize* je srednja. Na raspolaganju su vam Standard_Small, Standard_Medium i Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Stvaranje pristupnika za aplikaciju pomoću novo AzureRmApplicationGateway

Stvaranje pristupnika za aplikaciju s sve stavke konfiguracije sa gore navedene korake. U ovom primjeru pristupnika aplikacije jest "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Dodavanje u probni postojeće pristupnik za aplikaciju

Imate četiri koraka da biste dodali prilagođenu probni postojeće pristupnik za aplikacije.

### <a name="step-1"></a>Korak 1

Učitavanje resursa za pristupnik za aplikacije u varijablu PowerShell pomoću **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Korak 2

Dodajte na probni postojeću konfiguraciju pristupnika.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


U ovom primjeru prilagođene probni je konfiguriran za provjeru contoso.com/path/custompath.htm put URL-a svakih 30 sekundi. Prag isteka od 120 sekundi konfiguriran maksimalan broj 8 zahtjeva nije uspjelo probni.

### <a name="step-3"></a>Korak 3

Dodavanje pozadinske skup postavke konfiguracije i vrijeme isteka na probni pomoću **– Postavljanje-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Korak 4

Spremiti konfiguracije računala pristupnika pomoću **Skupa AzureRmApplicationGateway**.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Uklanjanje programa probni iz postojeće pristupnik za aplikaciju

Evo nekoliko koraka da biste uklonili prilagođeni probni iz postojeće pristupnik za aplikacije.

### <a name="step-1"></a>Korak 1

Učitavanje resursa za pristupnik za aplikacije u varijablu PowerShell pomoću **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Korak 2

Uklanjanje konfiguracije probni s računala pristupnika pomoću **Ukloni AzureRmApplicationGatewayProbeConfig**.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Korak 3

Ažuriranje pozadinske skup postavke da biste uklonili na probni i vremensko razdoblje pomoću **– Postavljanje-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Korak 4

Spremiti konfiguracije računala pristupnika pomoću **Skupa AzureRmApplicationGateway**. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="get-application-gateway-dns-name"></a>Dohvaćanje aplikacije pristupnika DNS naziva

Nakon stvaranja pristupnika, sljedeći je korak konfiguriranje sučelje za komunikaciju. Kada koristite javnoj IP pristupnik za aplikacije potreban je dinamički dodijeljeni DNS naziv koji nije neslužbeni. Da biste bili sigurni krajnji korisnici mogu pogoditi pristupnika aplikacije CNAME zapis mogu se pokažite na javno krajnjoj točki pristupnika aplikacije. [Konfiguriranje naziv prilagođene domene u Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Da biste to učinili, dohvaćanje detalja pristupnika aplikacije i nazivu IP-DNS povezane pomoću element PublicIPAddress priložiti pristupnik za aplikacije. Naziv pristupnika aplikacije DNS-a trebali biste mogu koristiti za stvaranje CNAME zapis koji pokazuje dvije web-aplikacije za DNS naziv. Korištenje A zapisa ne preporučuje se jer se VIP može se promijeniti ponovno pokretanje računala pristupnika.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako konfigurirati SSL rasteretite web-mjestu [Konfiguriranje SSL Offload](application-gateway-ssl-arm.md)

