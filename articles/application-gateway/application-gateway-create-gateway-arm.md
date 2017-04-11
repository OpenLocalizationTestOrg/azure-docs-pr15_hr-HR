<properties
   pageTitle="Stvaranje, pokretanje i brisanje pristupnika za aplikaciju pomoću upravitelja resursa Azure | Microsoft Azure"
   description="Ova stranica sadrži upute za stvaranje konfiguriranje, pokrenuti i brisanje pristupnika za Azure aplikacije pomoću upravitelja resursa za Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Stvaranje, pokretanje i brisanje pristupnika za aplikaciju pomoću upravitelja resursa za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-gateway-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-gateway-arm.md)
- [Azure klasični PowerShell](application-gateway-create-gateway.md)
- [Predloška Azure Voditelj resursa](application-gateway-create-gateway-arm-template.md)
- [Azure EŽA](application-gateway-create-gateway-cli.md)

Azure aplikacije pristupnik je sloj 7 opterećenja. Hoće li se nalaze na oblaku ili na lokalnim poslužiteljima, omogućuje prebacivanje, performanse usmjeravanje HTTP zahtjeva između različitih poslužitelja. Pristupnik za aplikacije sadrži brojne aplikacije isporuke kontroleru (ADC) značajke uključujući HTTP opterećenja afinitet utemeljen na kolačića sesije, offload Secure Sockets Layer (SSL), prilagođene stanja probes, podrška za više web-mjesta i mnoge druge. Da biste pronašli popis svih podržane značajke, posjetite [Aplikacije pristupnik za pregled](application-gateway-introduction.md)

U ovom se članku vodit će vas kroz korake za stvaranje, konfiguriranje, pokrenuti i brisanje pristupnika za aplikacije.

>[AZURE.IMPORTANT] Prije početka rada s resursima za Azure, važno je da biste shvatili Azure trenutno sadrži dvije implementacije modela: Voditelj resursa i classic. Provjerite da razumijete [Alati i implementaciju modelima](../azure-classic-rm.md) prije početka rada s bilo kojeg Azure resursa. U dokumentaciji različitih alata možete vidjeti klikom na karticama pri vrhu ovog članka. Ovaj dokument pokriva stvaranje pristupnika za aplikaciju pomoću upravitelja resursa Azure. Da biste koristili klasičnu verziju, otvorite članak [Stvaranje aplikacije pristupnika klasični implementacije sustava pomoću komponente PowerShell](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Prije početka

1. Instalirajte najnoviju verziju cmdleta ljuske PowerShell Azure pomoću platforme Installer Web. Možete preuzeti i instalirati najnoviju verziju [preuzimanja stranice](https://azure.microsoft.com/downloads/)u odjeljku **Komponente Windows PowerShell** .
2. Ako imate postojeću virtualne mrežu, odaberite postojeći prazan podmreže ili stvorite podmreži u postojeće virtualne mreže isključivo za korištenje pristupnik za aplikacije. Ne možete implementirati aplikaciju pristupnik drugoj virtualne mreži resursi namjeravate iza pristupnik za aplikaciju.
3. Mora postojati poslužitelje sustava konfigurirati da biste koristili aplikaciju pristupnika ili dodijeljenih svoje krajnje točke u virtualne mreže ili s javnu IP/VIP stvorili.

## <a name="what-is-required-to-create-an-application-gateway"></a>Što je potrebno da biste stvorili pristupnik za aplikaciju?

- **Skup pozadinskih poslužitelja:** Popis IP adrese pozadinskih poslužitelja. IP adrese na popisu ili trebao bi se nalaziti podmreže virtualne mreže ili mora biti javnu IP/VIP.
- **Skup postavke poslužitelja za pozadinsku:** Svaki skup ima postavke kao što su priključak, protokol i sustavom kolačića afinitet. Ove postavke je uz zajedničko područje, a primjenjuju se na svim poslužiteljima u na resurse.
- **Sučelja priključka:** U ovom priključak je javno priključak koja je otvorena na računala pristupnika. Promet dodirne priključak, a zatim prosljeđuje na neki od pozadinskih poslužitelja.
- **Ga slušatelj:** Ga slušatelj sadrži sučelja priključak protokol (Http ili Https, te su vrijednosti velika i mala slova), a naziv SSL certifikata (Ako je konfiguriranje SSL offload).
- **Pravilo:** Povezuje ga slušatelj, skup pozadinskih poslužitelja i pravila definira grupe aplikacija pozadinskih poslužitelja promet treba usmjeriti na kada je dodirne određeni ga slušatelj.

## <a name="create-an-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Razlika između korištenja Azure klasične i upravljanja resursima Azure je redoslijed u kojem stvarate pristupnika aplikacije i stavke koje je potrebno je konfigurirati.

S Voditelj resursa, sve stavke koje se pristupnik za aplikaciju su pojedinačno konfigurirani i smjestiti zajedno da biste stvorili resursa za pristupnik za aplikacije.

U nastavku su koraci koje su potrebne za stvaranje pristupnika za aplikacije.

## <a name="create-a-resource-group-for-resource-manager"></a>Stvaranje grupe resursa za Voditelj resursa

Provjerite koristite li najnoviju verziju programa Azure PowerShell. Dodatne informacije o dostupna je na [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Korak 1

Prijavite se na Azure

    Login-AzureRmAccount

Se od vas zatraži provjeru s vjerodajnice.

### <a name="step-2"></a>Korak 2

Provjerite pretplate za račun.

    Get-AzureRmSubscription

### <a name="step-3"></a>Korak 3

Odabir pretplate Azure da biste koristili.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Korak 4

Stvorite grupu resursa (preskoči ovaj korak ako koristite postojeću grupu resursa).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Voditelj resursa zahtijeva da svih grupa resursa navedite mjesto. To mjesto koristi se kao zadano mjesto za resurse u toj grupi resursa. Provjerite koristi li sve naredbe za stvaranje pristupnika za aplikaciju istoj grupi resursa.

U gornjem primjeru koju smo stvorili grupu resursa pod nazivom "appgw ru" i mjesto "Zapad NAM".

>[AZURE.NOTE] Ako morate konfigurirati prilagođene probni za pristupnik za aplikaciju, potražite u članku [Stvaranje pristupnika za aplikaciju s prilagođene probes pomoću komponente PowerShell](application-gateway-create-probe-ps.md). Pogledajte [prilagođene probes i nadzor stanja](application-gateway-probe-overview.md) da biste saznali više.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Stvaranje virtualne mreže i podmreže za pristupnik za aplikaciju

Sljedeći primjer pokazuje kako stvoriti virtualne mreže pomoću upravitelja resursa.

### <a name="step-1"></a>Korak 1

Raspon 10.0.0.0/24 adresa dodijeliti podmreže varijabla koja će se koristiti za stvaranje virtualne mreže.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Korak 2

Stvaranje virtualne mreže pod nazivom "appgwvnet" u resursa grupe "appgw – ru" za područje Zapad SAD-a pomoću 10.0.0.0/16 prefiks 10.0.0.0/24 podmreže.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Korak 3

Dodijelite varijabla podmreže za sljedeće korake koji stvaranje pristupnika za aplikacije.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Stvaranje javnu IP adresa za konfiguraciju sučelja

Stvaranje javne IP resursa "publicIP01" u resursa grupe "appgw – ru" za područje Zapad SAD-a.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Stvaranje objekta konfiguracije pristupnika za aplikacije

Sve stavke konfiguracije morate postaviti prije stvaranja aplikacije pristupnika. Sljedeći koraci stvorite konfiguracije stavke koje su vam potrebne pristupnika resursa aplikacije.

### <a name="step-1"></a>Korak 1

Stvaranje konfiguracije aplikacije pristupnik IP pod nazivom "gatewayIP01". Prilikom pokretanja aplikacije pristupnika uzima IP adresu iz podmreže konfigurirati i usmjerava mrežni promet na IP adresa u skupna pozadinsku IP. Imajte na umu da svaku instancu traje jednu IP adresu.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Korak 2

Konfiguriranje skupna pozadinsku IP adresa pod nazivom "pool01" s IP adresama "134.170.185.46, 134.170.188.221,134.170.185.50". Ove IP adrese nisu IP adrese koje primate mrežnog prometa koji dolazi s krajnju točku sučelja IP. Zamjena prethodni IP adrese da biste dodali vlastite krajnje točke aplikacije IP adresa.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Korak 3

Konfiguriranje postavke računala pristupnika "poolsetting01" za mrežni promet uravnoteženja u pozadinskoj.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Koraku 4

Konfiguriranje sučelja priključak IP pod nazivom "frontendport01" za krajnju točku javnu IP.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Korak 5

Stvaranje pristupne IP konfiguracija pod nazivom "fipconfig01" i pridružiti javnu IP adresu pristupne IP konfiguracija.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Korak 6

Stvaranje ga slušatelj naziv "listener01" i pridruživanje sučelja priključak sučelja IP konfiguracije.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Korak 7

Stvaranje učitavanja opterećenja usmjeravanje pravilo pod nazivom "rule01", koji se konfigurira funkcioniranje raspoređivača učitavanja.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Korak 8

Konfiguracija instancu veličine pristupnika aplikacije.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  Zadane vrijednosti za *InstanceCount* je 2, s maksimalnom vrijednošću od 10. Zadana vrijednost za *GatewaySize* je srednja. Na raspolaganju su vam Standard_Small, Standard_Medium i Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Stvaranje pristupnika za aplikaciju pomoću novo AzureRmApplicationGateway

Stvaranje pristupnika za aplikaciju s sve konfiguracije stavke iz prethodnih koraka. U ovom primjeru pristupnika aplikacije jest "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Korak 9

Dohvaćanje detalja DNS-a i VIP pristupnika aplikacije javnu IP resursa priložiti pristupnik za aplikacije.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Brisanje pristupnika za aplikaciju

Da biste izbrisali pristupnik za računala, slijedite ove korake:

### <a name="step-1"></a>Korak 1

Pronađite objekt pristupnika aplikacije i pridruživanje varijabli "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Korak 2

Koristite **Stop AzureRmApplicationGateway** da biste prestali pristupnik za aplikacije.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Kad se pristupnik za aplikacije u prestao stanju, pomoću cmdleta **Ukloni AzureRmApplicationGateway** da biste uklonili servis.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] Na **– prisilno** parametar može se koristiti izostavlja Ukloni potvrdnu poruku.

Da biste provjerili je li servis je uklonjen, koristite cmdlet **Get-AzureRmApplicationGateway** . Ovaj korak nije potrebna.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

## <a name="get-application-gateway-dns-name"></a>Dohvaćanje aplikacije pristupnika DNS naziva

Nakon stvaranja pristupnika, sljedeći je korak konfiguriranje sučelje za komunikaciju. Kada koristite javnoj IP pristupnik za aplikacije potreban je dinamički dodijeljeni DNS naziv koji nije neslužbeni. Da biste bili sigurni krajnji korisnici mogu pogoditi pristupnika aplikacije CNAME zapis mogu se pokažite na javno krajnjoj točki pristupnika aplikacije. [Konfiguriranje naziv prilagođene domene u Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Da biste to učinili, dohvaćanje pojedinosti pristupnika aplikacije i nazivu IP-DNS povezane pomoću element PublicIPAddress priložiti pristupnik za aplikacije. Naziv pristupnika aplikacije DNS-a trebali biste mogu koristiti za stvaranje CNAME zapis koji pokazuje dvije web-aplikacije za DNS naziv. Korištenje A zapisa ne preporučuje se jer se VIP može se promijeniti ponovno pokretanje računala pristupnika.
    
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

Ako želite konfigurirati rasterećivanje SSL, potražite u članku [Konfiguriranje pristupnik za aplikaciju za SSL offload](application-gateway-ssl.md).

Ako želite konfigurirati pristupnik programa aplikacija će se koristiti za interne opterećenja potražite u članku [Stvaranje pristupnika za aplikaciju s interne raspoređivača opterećenja (ILB)](application-gateway-ilb.md).

Dodatne informacije o učitavanje ujednačavanje mogućnosti Općenito, potražite u članku:

- [Azure opterećenja](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Upravitelj promet](https://azure.microsoft.com/documentation/services/traffic-manager/)
