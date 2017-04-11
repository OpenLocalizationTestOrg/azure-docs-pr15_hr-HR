<properties
   pageTitle="Konfiguriranje vatrozida aplikaciju za Web na novi ili postojeći pristupnika aplikacije | Microsoft Azure"
   description="Ovaj članak sadrži upute o tome kako početi koristiti vatrozid web aplikacije na postojeći ili novi pristupnik aplikacije."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfiguriranje vatrozida aplikaciju za Web na računala pristupnika novi ili postojeći

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-web-application-firewall-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-web-application-firewall-powershell.md)

Vatrozid web aplikacije (WAF) u Azure aplikacije pristupnika štiti web-aplikacije od uobičajenih utemeljen na webu napada, kao što je unos SQL, web-mjesta skriptiranja i hijacks sesiju.

Azure aplikacije pristupnik je sloj 7 opterećenja. Hoće li se nalaze na oblaku ili na lokalnim poslužiteljima, omogućuje prebacivanje, performanse usmjeravanje HTTP zahtjeva između različitih poslužitelja. Aplikacije sadrži brojne aplikacije isporuke kontroleru (ADC) značajke uključujući HTTP opterećenja afinitet utemeljen na kolačića sesije, offload Secure Sockets Layer (SSL), prilagođene stanja probes, podrška za više web-mjesta i mnoge druge. Da biste pronašli cjelovit popis značajki podržanih, posjetite aplikacije pristupnik za pregled

Sljedeći članak prikazuje kako [dodati aplikacije Vatrozid za web u postojeće pristupnik za aplikaciju](#add-web-application-firewall-to-an-existing-application-gateway) i [Stvaranje pristupnika za aplikacije koji koristi vatrozid aplikaciju za web](#create-an-application-gateway-with-web-application-firewall).

![scenarij slike][scenario]

## <a name="waf-configuration-differences"></a>Razlike u konfiguraciji WAF

Ako ste pročitali [Stvaranje pristupnika za aplikacije sa servisom PowerShell](application-gateway-create-gateway-arm.md), upoznajte se postavke SKU da biste konfigurirali prilikom stvaranja pristupnik za aplikacije. WAF pruža dodatne postavke da biste definirali prilikom konfiguriranja se SKU na pristupnik za aplikacije. Nema dodatnih promjena koje ste izvršili na aplikaciju pristupnika sam.

**SKU** - pristupnik normalni aplikacija ne podržava WAF **standardni\_Small**, **standardni\_srednje**, i **standardni\_Large** veličine. Uz Uvod u WAF, postoje dvije dodatne SKU-ove, **WAF\_srednje** i **WAF\_Large**. WAF nije podržana na mala aplikacija pristupnika.

**Razina** - dostupne vrijednosti su **Standardno** ili **WAF**. Kada koristite vatrozid aplikaciju za Web, morate odabrati **WAF** .

**Način rada** – Ova postavka je način WAF. dopuštene vrijednosti su **otkrivanje** i **Sprječavanje**. Kada WAF postavljeno je u načinu rada za otkrivanje, sve prijetnji spremaju se u datoteci zapisnika. U načinu rada za sprječavanje događaja i dalje prijavljeni, ali napadač primi 403 neovlašteno odgovor s računala pristupnika.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Dodavanje vatrozid aplikaciju za web u postojeće pristupnik za aplikaciju

Provjerite koristite li najnoviju verziju programa Azure PowerShell. Dodatne informacije o dostupna je na [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Korak 1

Prijavite se na račun za Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Korak 2

Odaberite pretplatu za taj scenarij.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Korak 3

Dohvaćanje pristupnika koje dodajete Web aplikacije Vatrozid za.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Korak 4

Konfiguriranje vatrozida SKU-om aplikacije web. Su dostupne količine **WAF\_Large** i **WAF\_srednje**. Kada se koristi vatrozid aplikaciju za web u sloju mora biti **WAF**, kapacitet mora potvrditi prilikom postavljanja se sku.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Korak 5

Konfiguriranje postavki WAF kako je definirano u sljedećem primjeru:

Za postavku **WafMode** dostupne vrijednosti su sprječavanje i otkrivanje.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Korak 6

Ažurirajte pristupnik aplikacije s postavkama definirano u prethodnom koraku.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Ta se naredba ažurira pristupnika aplikacije s vatrozidom aplikaciju za Web. Preporučuje se da biste pogledali [Dijagnostika pristupnika aplikacije](application-gateway-diagnostics.md) da biste razumjeli kako zapisnicima za pristupnik za aplikacije. Zbog prirode sigurnost WAF zapisnika moraju biti redovito pregledava da biste shvatili posture sigurnost web-aplikacija.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Stvaranje pristupnika za aplikaciju s vatrozidom aplikaciju za Web

Sljedeći koraci vas kroz cijeli postupak od početka do kraja za stvaranje pristupnika za aplikaciju s vatrozidom aplikaciju za Web.

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

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Korak 4

Stvorite grupu resursa (preskoči ovaj korak ako koristite postojeću grupu resursa).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Voditelj resursa zahtijeva da svih grupa resursa navedite mjesto. To mjesto koristi se kao zadano mjesto za resurse u toj grupi resursa. Provjerite koristi li sve naredbe za stvaranje pristupnika za aplikaciju istoj grupi resursa.

U prethodnom primjeru koju smo stvorili grupu resursa pod nazivom "appgw ru" i mjesto "Zapad NAM".

>[AZURE.NOTE] Ako morate konfigurirati prilagođene probni za pristupnik za aplikaciju, potražite u članku [Stvaranje pristupnika za aplikaciju s prilagođene probes pomoću komponente PowerShell](application-gateway-create-probe-ps.md). Pogledajte [prilagođene probes i nadzor stanja](application-gateway-probe-overview.md) da biste saznali više.

### <a name="step-5"></a>Korak 5

Dodijelite raspon adresa za podmreži će se koristiti za pristupnik za aplikaciju sam.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Podmreže za aplikaciju mora sadržavati najmanje 28 maske bitova. Ta vrijednost ostavlja 10 dostupna adrese u podmreže instanci pristupnika aplikacije. S podmreži manje ne možda ćete moći dodati više instanci pristupnika za aplikacije u budućnosti.

### <a name="step-6"></a>Korak 6

Dodijelite raspon adresa će se koristiti za skupna adresa pozadinskog.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Korak 7

Stvaranje virtualne mreže s prethodnom podmreže u grupi resursa stvorili u koraku: [Stvaranje grupa resursa](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Korak 8

Dohvaćanje virtualne mrežnom resursu i podmreže resursa koji će se koristiti u sljedećim koracima:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Korak 9

Stvorite javnu IP resursa koja će se koristiti za pristupnik za aplikacije. Javnu IP adresu koristi se u jednom od sljedećih koraka:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Pristupnik za aplikaciju ne podržava korištenje javnu IP adresu stvorene pomoću domene oznaku definiranu. Podržana je samo na javnu IP adresu s natpisom dinamički stvoreni domene. Ako tražite dns neslužbeni naziv pristupnika aplikacije, preporučuje se da biste upotrijebili zapis cname pseudonima.

### <a name="step-10"></a>Korak 10

Sve stavke konfiguracije morate postaviti prije stvaranja aplikacije pristupnika. Sljedeći koraci stvorite konfiguracije stavke koje su vam potrebne pristupnika resursa aplikacije.

Stvaranje programa IP konfiguracije pristupnika za aplikacije, to konfigurira koje podmreže koristi pristupnik za aplikacije. Prilikom pokretanja aplikacije pristupnika uzima IP adresu iz podmreže konfigurirati i usmjerava mrežni promet na IP adresa u skupna pozadinsku IP. Imajte na umu da svaku instancu traje jednu IP adresu.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Korak 11

Konfigurirajte skupna pozadinsku IP adresa IP adrese web-poslužiteljima pozadinskog. Ove IP adrese nisu IP adrese koje primate mrežnog prometa koji dolazi s krajnju točku sučelja IP. Zamjena sljedeće IP adrese da biste dodali vlastite krajnje točke aplikacije IP adresa.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Korak 12

Prijenos certifikat za korištenje ssl omogućen pozadinskog skup resursa.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Korak 13

Konfiguriranje postavki za http pozadinske aplikacije pristupnika. Dodijelite certifikat prenijeli u prethodnom koraku http postavke.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Korak 14

Konfiguriranje sučelja IP priključak za krajnju točku javnu IP. Ovaj priključak je priključka koji je krajnji korisnici povezati.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Korak 15

Stvaranje pristupne IP konfiguracija, tu postavku karte ili privatnu ip adresa sučelja aplikacije pristupnika. Sljedeći korak na javnu IP adresu u prethodnom koraku pridružuje sučelja IP konfiguracija.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Korak 16

Konfiguriranje certifikata za pristupnik za aplikacije. Taj certifikat koristi se za dešifrirati i ponovno Šifriraj promet na računala pristupnika.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Korak 17

Stvorite ga slušatelj HTTP za pristupnik za aplikacije. Dodijelite sučelja ip konfiguracije, priključak i ssl certifikat za korištenje.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Korak 18

Stvorite pravilo usmjeravanje raspoređivača opterećenja koje konfigurira funkcioniranje raspoređivača učitavanja. U ovom primjeru stvara se osnovni kružnog pravila.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Korak 19

Konfiguracija instancu veličine pristupnika aplikacije.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Na raspolaganju **WAF\_srednje** i **WAF\_Large**, sloju prilikom korištenja WAF uvijek je **WAF**. Kapacitet je bilo koji broj između 1 i 10.

### <a name="step-20"></a>Korak 20

Konfiguriranje načina rada za WAF, prihvatljiva **Sprječavanje** i **otkrivanje**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Korak 21

Stvaranje pristupnika za aplikaciju s sve konfiguracije stavke iz prethodnih koraka. U ovom primjeru pristupnika aplikacije jest "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

Upute za konfiguriranje dijagnostičkog zapisnika za zapisivanje događaja koji su otkrivena ili spriječio s vatrozidom aplikaciju za Web tako što ste posjetili [Dijagnostika pristupnika aplikacije](application-gateway-diagnostics.md)


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png