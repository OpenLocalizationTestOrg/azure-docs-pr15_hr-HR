<properties
   pageTitle="Stvaranje i konfiguriranje pristupnik za aplikaciju s internim raspoređivača opterećenja (ILB) pomoću upravitelja resursa Azure | Microsoft Azure"
   description="Ova stranica sadrži upute za stvaranje konfiguriranje, pokrenuti i brisanje pristupnika za Azure aplikacije s internim opterećenja (ILB) za Azure Voditelj resursa"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Stvaranje pristupnika za aplikaciju s internim raspoređivača opterećenja (ILB) pomoću upravitelja resursa za Azure

> [AZURE.SELECTOR]
- [Azure klasični korake](application-gateway-ilb.md)
- [Upute za PowerShell Voditelj resursa](application-gateway-ilb-arm.md)

Azure pristupnika računala može se konfigurirati s VIP na mjesto na Internetu ili interne krajnju točku koja nije izložen putem Interneta, poznata i kao Interna učitavanja opterećenja (ILB) krajnje. Konfiguraciju pristupnika pomoću programa ILB je korisno za interne redak tvrtke aplikacije koje su izložen putem Interneta. Preporučuje se i korisne su za servise i razine unutar više razina aplikacije koje sjesti u granicu sigurnosti koje je izložen putem Interneta, ali i dalje potreban kružnog učitavanje raspodjele, stickiness sesiju ili prekid Secure Sockets Layer (SSL).

U ovom se članku vodit će vas kroz korake da biste konfigurirali pristupnik za aplikaciju programa ILB.

## <a name="before-you-begin"></a>Prije početka

1. Instalirajte najnoviju verziju cmdleta ljuske PowerShell Azure pomoću platforme Installer Web. Možete preuzeti i instalirati najnoviju verziju [preuzimanja stranice](https://azure.microsoft.com/downloads/)u odjeljku **Komponente Windows PowerShell** .
2. Stvaranje virtualne mreže i podmreži za pristupnik za aplikacije. Pripazite da virtualnim strojevima ni oblaka implementaciji koristite podmreži. Pristupnik za aplikaciju mora biti sam podmreži virtualne mreže.
3. Mora postojati poslužitelje sustava konfigurirati da biste koristili aplikaciju pristupnika ili dodijeljenih njihove krajnje točke u virtualne mreže ili s javnu IP/VIP stvorili.

## <a name="what-is-required-to-create-an-application-gateway"></a>Što je potrebno da biste stvorili pristupnik za aplikaciju?


- **Skup pozadinskih poslužitelja:** Popis IP adrese pozadinskih poslužitelja. IP adrese na popisu ili trebao bi se nalaziti virtualne mreže, ali u drugoj podmreži za pristupnik za aplikaciju ili mora biti javnu IP/VIP.
- **Skup postavke poslužitelja za pozadinsku:** Svaki skup ima postavke kao što su priključak, protokol i sustavom kolačića afinitet. Ove postavke je uz zajedničko područje, a primjenjuju se na svim poslužiteljima u na resurse.
- **Sučelja priključka:** U ovom priključak je javno priključak koja je otvorena na računala pristupnika. Promet dodirne priključak, a zatim prosljeđuje na neki od pozadinskih poslužitelja.
- **Ga slušatelj:** Ga slušatelj sadrži sučelja priključak protokol (Http ili Https, to su velika i mala slova), a naziv SSL certifikata (Ako je konfiguriranje SSL offload).
- **Pravilo:** Povezuje ga slušatelj i skup pozadinskih poslužitelja i pravila definira grupe aplikacija pozadinskih poslužitelja promet treba usmjeriti na kada je dodirne određeni ga slušatelj. Trenutno podržana je samo *Osnovna* pravila. *Osnovno* je pravilo je opterećenja kružnog distribucije.



## <a name="create-an-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Razlika između korištenja Azure klasične i upravljanja resursima Azure je redoslijed u kojem stvarate pristupnika aplikacije i stavke koje je potrebno je konfigurirati.
S Voditelj resursa, sve stavke koje se pristupnik za aplikaciju je pojedinačno konfigurirani i smjestiti zajedno da biste stvorili resursa za pristupnik za aplikacije.


Evo nekoliko koraka koji su potrebni za stvaranje pristupnika za aplikaciju:

1. Stvaranje grupe resursa za Voditelj resursa
2. Stvaranje virtualne mreže i podmreže za pristupnik za aplikaciju
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

Stvorite novu grupu resursa (preskoči ovaj korak ako koristite postojeću grupu resursa).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Voditelj resursa zahtijeva da svih grupa resursa navedite mjesto. Koristi se kao zadano mjesto za resurse u toj grupi resursa. Provjerite koristi li sve naredbe za stvaranje pristupnika za aplikaciju istoj grupi resursa.

U gornjem primjeru koju smo stvorili grupu resursa pod nazivom "appgw ru" i mjesto "Zapad NAM".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Stvaranje virtualne mreže i podmreže za pristupnik za aplikaciju

Sljedeći primjer prikazuje način da biste stvorili virtualne mreže pomoću upravitelja resursa:

### <a name="step-1"></a>Korak 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

To 10.0.0.0/24 raspon adresa dodjeljuje podmreže varijabla koja će se koristiti za stvaranje virtualne mreže.

### <a name="step-2"></a>Korak 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Time se stvara virtualne mreže pod nazivom "appgwvnet" u resursa grupe "appgw-ru" za područje Zapad SAD-a pomoću 10.0.0.0/16 prefiks 10.0.0.0/24 podmreže.

### <a name="step-3"></a>Korak 3

    $subnet = $vnet.subnets[0]

Taj objekt podmreže dodjeljuje varijabla $subnet za sljedeće korake.

## <a name="create-an-application-gateway-configuration-object"></a>Stvaranje objekta konfiguracije pristupnika za aplikacije

### <a name="step-1"></a>Korak 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Time se stvara konfiguracije aplikacije pristupnik IP pod nazivom "gatewayIP01". Prilikom pokretanja aplikacije pristupnika uzima IP adresu iz podmreže konfigurirati i mrežni promet usmjerili na IP adresa u skupna pozadinsku IP. Imajte na umu da svaku instancu traje jednu IP adresu.


### <a name="step-2"></a>Korak 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

To konfigurira skupna pozadinsku IP adresa pod nazivom "pool01" s IP adresama "134.170.185.46, 134.170.188.221,134.170.185.50". To su IP adrese koje primate mrežnog prometa koji dolazi s krajnju točku sučelja IP. Zamjena IP adrese da biste dodali vlastite krajnje točke aplikacije IP adresa.

### <a name="step-3"></a>Korak 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

To konfigurira aplikacije pristupnika postavka "poolsetting01" opterećenja raspoređen mrežnog prometa u pozadinskoj.

### <a name="step-4"></a>Korak 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

To konfigurira sučelja priključak IP pod nazivom "frontendport01" za na ILB.

### <a name="step-5"></a>Korak 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Stvara sučelja IP konfiguracija pod nazivom "fipconfig01" i povezuje s privatno IP iz trenutne podmreže virtualne mreže.

### <a name="step-6"></a>Korak 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Time se stvara ga slušatelj pod nazivom "listener01" i povezuje sučelja priključak sučelja konfiguraciju IP.

### <a name="step-7"></a>Korak 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Time se stvara učitavanja opterećenja usmjeravanje pravilo pod nazivom "rule01", koji se konfigurira funkcioniranje raspoređivača učitavanja.

### <a name="step-8"></a>Korak 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

To konfigurira veličinu instanci pristupnika aplikacije.

>[AZURE.NOTE]  Zadane vrijednosti za *InstanceCount* je 2, s maksimalnom vrijednošću od 10. Zadana vrijednost za *GatewaySize* je srednja. Na raspolaganju su vam Standard_Small, Standard_Medium i Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Stvaranje pristupnika za aplikaciju pomoću novo AzureApplicationGateway

Stvara pristupnik za aplikaciju s mogućnošću sve stavke konfiguracije od gore navedene korake. U ovom primjeru pristupnika aplikacije jest "appgwtest".


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Time ste stvorili pristupnik za aplikaciju s sve stavke konfiguracije sa gore navedene korake. U ovom primjeru pristupnika aplikacije jest "appgwtest".


## <a name="delete-an-application-gateway"></a>Brisanje pristupnika za aplikaciju

Da biste izbrisali pristupnik za računala, morat ćete u redoslijed učinite sljedeće:

1. Pomoću cmdleta **Zaustavi AzureRmApplicationGateway** da biste prestali pristupnika.
2. Pomoću cmdleta **Ukloni AzureRmApplicationGateway** da biste uklonili pristupnika.
3. Provjerite da je uklonjen pristupnika pomoću cmdleta **Get-AzureApplicationGateway** .


### <a name="step-1"></a>Korak 1

Pronađite objekt pristupnika aplikacije i pridruživanje varijabli "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Korak 2

Koristite **Stop AzureRmApplicationGateway** da biste prestali pristupnik za aplikacije. Ovaj primjer prikazuje cmdlet **Zaustavi AzureRmApplicationGateway** u prvom retku slijedi izlaz.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Kad se pristupnik za aplikacije u prestao stanju, pomoću cmdleta **Ukloni AzureRmApplicationGateway** da biste uklonili servis.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] Na **– prisilno** parametar može se koristiti izostavlja Ukloni potvrdnu poruku.


Da biste provjerili je li servis je uklonjen, koristite cmdlet **Get-AzureRmApplicationGateway** . Ovaj korak nije potrebna.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Daljnji koraci

Ako želite konfigurirati rasterećivanje SSL, potražite u članku [Konfiguriranje pristupnik za aplikaciju za SSL offload](application-gateway-ssl.md).

Ako želite konfigurirati za pristupnik za aplikacije za uporabu programa ILB potražite u članku [Stvaranje pristupnika za aplikaciju s internim raspoređivača opterećenja (ILB)](application-gateway-ilb.md).

Dodatne informacije o učitavanje ujednačavanje mogućnosti Općenito, potražite u članku:

- [Azure opterećenja](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Upravitelj promet](https://azure.microsoft.com/documentation/services/traffic-manager/)
