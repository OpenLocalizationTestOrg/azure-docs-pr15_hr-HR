<properties
    pageTitle="Konfiguriranje SSL pravila i završetka za kraj SSL s računala pristupnika | Microsoft Azure"
    description="U ovom se članku objašnjava kako konfigurirati end da biste završetka SSL s računala pristupnika pomoću komponente PowerShell resursima Azure"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>Konfiguriranje SSL pravila i SSL završetka na kraj s računala pristupnika pomoću komponente PowerShell

## <a name="overview"></a>Pregled

Pristupnik aplikacija podržava šifriranje end da biste završetka prometa. Pristupnik za aplikaciju to tako da prekidanje veze SSL pristupnika za aplikacije. Pristupnik zatim primjenjuje pravila za usmjeravanje promet, ponovno šifrira paket i prosljeđuje paketa za odgovarajući pozadinskog temelju definirana pravila za usmjeravanje. Bilo koji odgovor na web-poslužitelju prolaze kroz isti postupak natrag za krajnjeg korisnika.

Drugi značajka tu aplikaciju pristupnika podržava je onemogućivanje određene SSL protokol verzije. Pristupnik aplikacija podržava onemogućivanje sljedeće verziju protokola; TLSv1.0, TLSv1.1 i TLSv1.2.

> [AZURE.NOTE] SSL 2.0 i SSL 3.0 po zadanom su onemogućeni i ne mogu biti omogućene. Mogu se smatraju nesigurnim i nećete koristiti u sklopu aplikacije pristupnika

![scenarij slike][scenario]

## <a name="scenario"></a>Scenarij

U ovom scenariju, Saznajte kako stvaranje pristupnika za aplikaciju putem SSL-end da biste završetka a pomoću komponente PowerShell.

Ovaj scenarij će:

- Stvaranje grupe resursa pod nazivom "appgw ru"
- Stvaranje virtualne mreže pod nazivom "appgwvnet" s rezerviranim blok CIDR 10.0.0.0/16.
- Stvorite dva podmreže pod nazivom "appgwsubnet" i "appsubnet".
- Stvaranje pristupnika mala aplikacija završetka na kraj SSL šifriranje koje onemogućuje određene SSL protokola za podršku.

## <a name="before-you-begin"></a>Prije početka

Da biste konfigurirali završetka na kraj SSL pristupnik za aplikaciju, potrebna je potvrda pristupnika i certifikati su potrebni za pozadinskog poslužitelja. Certifikat pristupnika koristi se za šifriranje i dešifriranje promet za slanje putem SSL-a. Certifikat pristupnika mora biti u obliku Razmjena osobnih podataka (pfx). U ovom obliku datoteke omogućuje moguće izvesti privatni ključ koji je potreban pristupnik za aplikaciju za izvođenje šifriranje i dešifriranje promet.

Za kraj na kraj ssl šifriranje pozadinski mora biti whitelisted s računala pristupnika. To možete učiniti tako da prenesete javni certifikat s pozadinskom sustavu pristupnika za aplikacije. Time se osigurava da pristupnika aplikacije obavještava korisnike samo s poznatim pozadinskog instancama. To dodatno secures komunikacije završetka i završetka.

Ovaj postupak je opisan u na sljedeći način:

## <a name="create-the-resource-group"></a>Stvaranje grupa resursa

U ovom se odjeljku vodit će vas voditi kroz stvaranje grupa resursa koja sadrži aplikaciju pristupnika.

### <a name="step-1"></a>Korak 1

Prijavite se na račun za Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Korak 2

Odaberite pretplatu za taj scenarij.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Korak 3

Stvorite grupu resursa (preskoči ovaj korak ako koristite postojeću grupu resursa).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Stvaranje virtualne mreže i podmreže za pristupnik za aplikaciju

Sljedeći primjer stvara virtualne mreže i dva podmreže. Jedan podmreži koristi se za čuvanje pristupnik za aplikacije. Ostale podmreži koristi se za pozadinski sustav hostiranja web-aplikaciju.

### <a name="step-1"></a>Korak 1

Dodijelite raspon adresa za podmreži će se koristiti za pristupnik za aplikaciju sam.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Podmreže konfiguriran za pristupnik za aplikaciju treba ispravno veličine. Do 10 instanci moguće je konfigurirati pristupnik za aplikacije. Svaku instancu vodi 1 IP adresa iz podmreži. Previše male podmreži mogu negativno utjecati na skaliranje out pristupnik za aplikacije.

### <a name="step-2"></a>Korak 2

Dodijelite raspon adresa će se koristiti za skupna adresa pozadinskog.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Korak 3

Stvaranje virtualne mreže s podmreže definirano u prethodne korake.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Korak 4

Dohvaćanje virtualne mrežnom resursu i podmreže resursa koji će se koristiti u sljedećim koracima:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Stvaranje javne IP adresa za konfiguraciju sučelja

Stvorite javnu IP resursa koja će se koristiti za pristupnik za aplikacije. Koristi se javnu IP adresu na sljedeći korak.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Pristupnik za aplikaciju ne podržava korištenje javnu IP adresu stvorene pomoću domene oznaku definiranu. Podržana je samo na javnu IP adresu s natpisom dinamički stvoreni domene. Ako tražite dns neslužbeni naziv za pristupnik za aplikaciju, preporučuje se da biste upotrijebili zapis cname pseudonima.

## <a name="create-an-application-gateway-configuration-object"></a>Stvaranje objekta konfiguracije pristupnika za aplikacije

Sve stavke konfiguracija morate postaviti prije stvaranja aplikacije pristupnika. Sljedeći koraci stvorite konfiguracije stavke koje su vam potrebne pristupnika resursa aplikacije.

### <a name="step-1"></a>Korak 1

Stvaranje programa IP konfiguracije pristupnika za aplikacije, tu postavku konfigurira koje podmreže koristi pristupnik za aplikacije. Prilikom pokretanja aplikacije pristupnika uzima IP adresu iz podmreže konfigurirati i usmjerava mrežni promet na IP adresa u skupna pozadinsku IP. Imajte na umu da svaku instancu traje jednu IP adresu.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Korak 2

Stvaranje pristupne IP konfiguracija, ova postavka karte ili privatnu ip adresa sučelja aplikacije pristupnika. Sljedeći korak na javnu IP adresu u prethodnom koraku pridružuje sučelja IP konfiguracija.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Korak 3

Konfigurirajte skupna pozadinsku IP adresa IP adrese web-poslužiteljima pozadinskog. Ove IP adrese nisu IP adrese koje primate mrežnog prometa koji dolazi s krajnju točku sučelja IP. Zamjena sljedeće IP adrese da biste dodali vlastite krajnje točke aplikacije IP adresa.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Na potpuno kvalificirani naziv domene (FQDN) ujedno valjana vrijednost umjesto ip adresa za poslužitelje pozadinskog pomoću parametar - BackendFqdns.

### <a name="step-4"></a>Korak 4

Konfiguriranje sučelja IP priključak za krajnju točku javnu IP. Ovaj priključak je priključka koji je krajnji korisnici povezati.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Korak 5

Konfiguriranje certifikata za pristupnik za aplikacije. Taj certifikat koristi se za šifriranje i ponovno Šifriraj promet na računala pristupnika.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] Ovaj primjer konfigurira certifikat koji je korišten za SSL veze. Certifikat mora biti u obliku .pfx i lozinka mora biti između 4 do 12 znakova.

### <a name="step-6"></a>Korak 6

Stvorite ga slušatelj HTTP za pristupnik za aplikacije. Dodijelite sučelja ip konfiguracije, priključak i ssl certifikat za korištenje.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Korak 7

Prijenos certifikat za korištenje ssl omogućen pozadinskog skup resursa.

> [AZURE.NOTE] Zadani probni dobiva javni ključ od povezivanje SSL **zadani** na pozadinske je IP adresa i uspoređuje javno vrijednost ključa primi javno vrijednost ključa ovdje unesete. Dohvaćeni javni ključ nužno možda nije svrhu web-mjesta na kojem će promet tijeka **Ako** koristite zaglavlja glavnog računala i SNI na pozadinske. Ako se nalazite u niste sigurni, posjetite https://127.0.0.1/ na natrag završava da biste potvrdili koji certifikat služi za povezivanje SSL **zadani** . Koristite javni ključ iz tog zahtjeva u ovom odjeljku. Ako koristite glavno računalo zaglavlja i SNI na HTTPS povezivanja, a ne dobivate odgovor i potvrda iz zahtjeva za ručno preglednika da biste https://127.0.0.1/ na stražnjoj završava, morate postaviti zadanu SSL vezu na stražnjoj završava. Ako to ne učinite, probes neće uspjeti i pozadinske se neće biti whitelisted.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Potvrda navedena u ovom ćete koraku mora biti javni ključ certifikata pfx prisutne pozadinski. Izvezite certifikat (nije korijenski certifikat) instalirana na poslužitelju pozadinskog u. CER oblikovanje i koristiti u ovom ćete koraku. U ovom koraku whitelists pozadinskog s računala pristupnika.

### <a name="step-8"></a>Korak 8

Konfiguriranje postavki za http pozadinskih aplikacije pristupnika. Dodijelite certifikat prenijeli u prethodnom koraku http postavke.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Korak 9

Stvorite pravilo usmjeravanje raspoređivača opterećenja koje konfigurira funkcioniranje raspoređivača učitavanja. U ovom primjeru stvara se osnovni kružnog pravila.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Korak 10

Konfiguriranje veličinu instanci pristupnika aplikacije.  Su dostupne količine **standardni\_Small**, **standardni\_srednje**, i **standardni\_Large**.  Kapaciteta, dostupne vrijednosti su od 1 do 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Instance broj 1 možete odabrati za testiranje. Nije važno je znati da sve instance broj u odjeljku dvije instance ne pokriva u SLA i stoga ne preporučuje se. Mali pristupnika su koja će se koristiti za testiranje razvojni, a ne za namjene.

### <a name="step-11"></a>Korak 11

Konfiguriranje SSL pravila za korištenje aplikacije pristupnika. Pristupnik aplikacija podržava onemogućiti određene SSL protokol verzije.

Sljedeće vrijednosti su popis verzija protokol koji može se onemogućiti.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

U sljedećem primjeru onemogućuje TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Pomoću prethodnih koraka, stvorite aplikaciju pristupnika. Stvaranje pristupnika je dugo pokrenuti postupak.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>Onemogućivanje SSL protokol verzije na postojeće pristupnik za aplikaciju

Prethodne korake proći kroz stvaranje aplikacije s ssl end da biste završetka i onemogućivanje određene SSL protokol verzije. U sljedećem primjeru onemogućuje određene SSL pravila na postojeće pristupnik za aplikacije.

### <a name="step-1"></a>Korak 1

Dohvaćanje aplikacije pristupnika da biste ažurirali.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Korak 2

Definirati pravilo SSL. U sljedećem primjeru TLSv1.0 i TLSv1.1 su onemogućene.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Korak 3

Na kraju, ažurirajte pristupnik. Važno Imajte na umu da ova posljednji korak dugo izvodi zadatak je. Kada završi, end da biste završetka ssl je konfiguriran na računala pristupnika.

    $gw | Set-AzureRmApplicationGateway

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

Dodatne informacije o utvrđivanje sigurnost web-aplikacije s Web aplikacije vatrozid putem aplikacije pristupnika tako što ste posjetili [Web aplikacije Vatrozid za pregled](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
