<properties
   pageTitle="Konfigurirati pristupnik za aplikaciju za rasterećivanje SSL pomoću upravitelja resursa Azure | Microsoft Azure"
   description="Ova stranica sadrži upute za stvaranje pristupnika za aplikaciju SSL offload pomoću upravitelja resursa za Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurirati pristupnik za aplikaciju za rasterećivanje SSL pomoću upravitelja resursa za Azure

> [AZURE.SELECTOR]
-[Portal za Azure](application-gateway-ssl-portal.md)
-[Azure resursima PowerShell](application-gateway-ssl-arm.md)
-[Azure klasični PowerShell](application-gateway-ssl.md)

 Da biste prekinuli sesiju Secure Sockets Layer (SSL) pristupnika da biste izbjegli skup zadaci dešifriranje SSL da se dogodi u farmi web moguće je konfigurirati pristupnik Azure aplikacije. SSL rasterećivanje pojednostavljuje i postavljanje poslužitelj sučelja i upravljanja web-aplikacije.

## <a name="before-you-begin"></a>Prije početka

1. Instalirajte najnoviju verziju programa Azure PowerShell cmdleti pomoću platforme Installer Web. Možete preuzeti i instalirati najnoviju verziju [preuzimanja stranice](https://azure.microsoft.com/downloads/)u odjeljku **Komponente Windows PowerShell** .
2. Stvorite virtualne mreže i podmreže za pristupnik za aplikacije. Pripazite da virtualnim strojevima ni oblaka implementaciji koristite podmreži. Pristupnik za aplikaciju mora biti sam podmreži virtualne mreže.
3. Mora postojati poslužitelje konfigurirati da biste koristili aplikaciju pristupnika ili dodijeljenih njihove krajnje točke u virtualne mreže ili s javnu IP/VIP stvorili.

## <a name="what-is-required-to-create-an-application-gateway"></a>Što je potrebno da biste stvorili pristupnik za aplikaciju?


- **Skup pozadinskih poslužitelja:** Popis IP adrese pozadinskih poslužitelja. IP adrese na popisu ili trebao bi se nalaziti podmreže virtualne mreže ili mora biti javnu IP/VIP.
- **Skup postavke poslužitelja za pozadinsku:** Svaki skup ima postavke kao što su priključak, protokol i sustavom kolačića afinitet. Ove postavke je uz zajedničko područje, a primjenjuju se na svim poslužiteljima u na resurse.
- **Sučelja priključka:** U ovom priključak je javno priključak koja je otvorena na računala pristupnika. Promet dodirne priključak, a zatim prosljeđuje na neki od pozadinskih poslužitelja.
- **Ga slušatelj:** Ga slušatelj sadrži sučelja priključak protokol (Http ili Https, te se postavke velika i mala slova), a naziv SSL certifikata (Ako je konfiguriranje SSL offload).
- **Pravilo:** Povezuje ga slušatelj i skup pozadinskih poslužitelja i pravila definira grupe aplikacija pozadinskih poslužitelja promet treba usmjeriti na kada je dodirne određeni ga slušatelj. Trenutno podržana je samo *Osnovna* pravila. *Osnovno* je pravilo je opterećenja kružnog distribucije.

**Dodatna konfiguracija bilješke**

Za SSL certifikate konfiguracija protokol u **HttpListener** trebali biste promijeniti *Https* (velika i mala slova). **SslCertificate** element dodaje se **HttpListener** vrijednošću varijable konfigurirana za SSL certifikata. Trebali biste se ažurirati sučelja priključak 443.

**Omogućivanje kolačića sustavom afinitet**: pristupnik za aplikacije koje se mogu konfigurirati da biste bili sigurni da zahtjev iz klijentske sesije uvijek usmjereni na isti VM u web-farme. Ovaj scenarij obavlja tvrtka unos kolačića sesije koja omogućuje pristupnika da biste usmjerili promet pravilno. Da biste omogućili afinitet utemeljen na kolačića, postavite **CookieBasedAffinity** na *omogućeno* **BackendHttpSettings** element.


## <a name="create-an-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Razlike između korištenja Azure klasični model implementacije i upravljanja resursima Azure je redoslijed koji ste stvorili pristupnik za aplikacije i stavke koje je potrebno je konfigurirati.

S Voditelj resursa sve komponente pristupnika za aplikaciju su pojedinačno konfigurirani i smjestiti zajedno da biste stvorili programa resursa za pristupnik za aplikacije.


Evo korake potrebne za stvaranje pristupnika za aplikaciju:

1. Stvaranje grupe resursa za Voditelj resursa
2. Stvaranje virtualne mreže, podmreže i javnu IP za pristupnik za aplikaciju
3. Stvaranje objekta konfiguracije pristupnika za aplikacije
4. Stvaranje resursa za pristupnik programa aplikacije


## <a name="create-a-resource-group-for-resource-manager"></a>Stvaranje grupe resursa za Voditelj resursa

Provjerite je li prijeđete PowerShell način da bi koristio Cmdlete Azure Voditelj resursa. Dodatne informacije o dostupna je na [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Korak 1

    Login-AzureRmAccount

### <a name="step-2"></a>Korak 2

Provjerite pretplate za račun.

    Get-AzureRmSubscription

Se od vas zatraži provjeru s vjerodajnice.<BR>

### <a name="step-3"></a>Korak 3

Odabir pretplate Azure da biste koristili. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Korak 4

Stvorite grupu resursa (preskoči ovaj korak ako koristite postojeću grupu resursa).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Voditelj resursa zahtijeva da svih grupa resursa navedite mjesto. Ta postavka koristi se kao zadano mjesto za resurse u toj grupi resursa. Provjerite koristi li sve naredbe za stvaranje pristupnika za aplikaciju istoj grupi resursa.

U gornjem primjeru koju smo stvorili grupu resursa pod nazivom "appgw ru" i mjesto "Zapad NAM".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Stvaranje virtualne mreže i podmreže za pristupnik za aplikaciju

Sljedeći primjer prikazuje način da biste stvorili virtualne mreže pomoću upravitelja resursa:

### <a name="step-1"></a>Korak 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Ovaj primjer dodjeljuje raspon 10.0.0.0/24 adresa podmreže varijabla koja će se koristiti za stvaranje virtualne mreže.

### <a name="step-2"></a>Korak 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

U ovom Ogledna formula stvara virtualne mreže pod nazivom "appgwvnet" u resursa grupe "appgw-ru" za područje Zapad SAD-a pomoću 10.0.0.0/16 prefiks 10.0.0.0/24 podmreže.

### <a name="step-3"></a>Korak 3

    $subnet = $vnet.Subnets[0]

Ovaj primjer dodjeljuje objekt podmreže varijabli $subnet za sljedeće korake.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Stvaranje javne IP adresa za konfiguraciju sučelja

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

U ovom Ogledna formula stvara javnu IP resursa "publicIP01" u resursa grupe "appgw-ru" za područje Zapad SAD-a.


## <a name="create-an-application-gateway-configuration-object"></a>Stvaranje objekta konfiguracije pristupnika za aplikacije

### <a name="step-1"></a>Korak 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

U ovom Ogledna formula stvara konfiguracije aplikacije pristupnik IP pod nazivom "gatewayIP01". Prilikom pokretanja aplikacije pristupnika uzima IP adresu iz podmreže konfigurirati i mrežni promet usmjerili na IP adresa u skupna pozadinsku IP. Imajte na umu da svaku instancu traje jednu IP adresu.

### <a name="step-2"></a>Korak 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Ovaj primjer konfigurira skupna pozadinsku IP adresa pod nazivom "pool01" s IP adresama "134.170.185.46, 134.170.188.221,134.170.185.50." Te se vrijednosti su IP adrese koje primate mrežnog prometa koji dolazi s krajnju točku sučelja IP. Zamijenite IP adresa u prethodnom primjeru IP adrese krajnje točke za aplikacije na web.

### <a name="step-3"></a>Korak 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Ovaj primjer konfigurira aplikacije pristupnika postavka "poolsetting01" da biste uravnoteženja mrežnog prometa u pozadinskoj.

### <a name="step-4"></a>Korak 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

Ovaj primjer konfigurira sučelja priključak IP pod nazivom "frontendport01" za krajnju točku javnu IP.

### <a name="step-5"></a>Korak 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

Ovaj primjer konfigurira certifikat koji je korišten za SSL veze. Certifikat mora biti u obliku .pfx i lozinka mora biti između 4 do 12 znakova.

### <a name="step-6"></a>Korak 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Ovaj primjer stvara sučelja IP konfiguracija pod nazivom "fipconfig01" i na javnu IP adresu pridružuje sučelja IP konfiguracija.

### <a name="step-7"></a>Korak 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Ovaj primjer stvara naziv ga slušatelj "listener01" i povezuje sučelja priključak sučelja IP konfiguracija i certifikata.

### <a name="step-8"></a>Korak 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

U ovom Ogledna formula stvara učitavanja opterećenja usmjeravanje pravilo pod nazivom "rule01", koji se konfigurira funkcioniranje raspoređivača učitavanja.

### <a name="step-9"></a>Korak 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Ovaj primjer konfigurira veličinu instanci pristupnika aplikacije.

>[AZURE.NOTE]  Zadane vrijednosti za *InstanceCount* je 2, s maksimalnom vrijednošću od 10. Zadana vrijednost za *GatewaySize* je srednja. Na raspolaganju su vam Standard_Small, Standard_Medium i Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Stvaranje pristupnika za aplikaciju pomoću novo AzureApplicationGateway

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

U ovom Ogledna formula stvara pristupnik za aplikaciju s mogućnošću sve stavke konfiguracije iz prethodne korake. U ovom primjeru pristupnika aplikacije jest "appgwtest".

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

Ako želite konfigurirati pristupnik programa aplikacija će se koristiti za interne raspoređivača opterećenja (ILB) potražite u članku [Stvaranje pristupnika za aplikaciju s internim raspoređivača opterećenja (ILB)](application-gateway-ilb.md).

Dodatne informacije o učitavanje ujednačavanje mogućnosti Općenito, potražite u članku:

- [Azure opterećenja](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Upravitelj promet](https://azure.microsoft.com/documentation/services/traffic-manager/)
