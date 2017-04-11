<properties
   pageTitle="Stvaranje, pokretanje i brisanje pristupnika za aplikaciju | Microsoft Azure"
   description="Ova stranica sadrži upute za stvaranje konfiguriranje, pokrenuti i brisanje pristupnika za Azure aplikacije"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Stvaranje, pokretanje i brisanje pristupnika za aplikaciju

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-gateway-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-gateway-arm.md)
- [Azure klasični PowerShell](application-gateway-create-gateway.md)
- [Predloška Azure Voditelj resursa](application-gateway-create-gateway-arm-template.md)
- [Azure EŽA](application-gateway-create-gateway-cli.md)

Azure aplikacije pristupnik je sloj 7 opterećenja. Hoće li se nalaze na oblaku ili na lokalnim poslužiteljima, omogućuje prebacivanje, performanse usmjeravanje HTTP zahtjeva između različitih poslužitelja. Pristupnik za aplikacije sadrži brojne aplikacije isporuke kontroleru (ADC) značajke uključujući HTTP opterećenja afinitet utemeljen na kolačića sesije, offload Secure Sockets Layer (SSL), prilagođene stanja probes, podrška za više web-mjesta i mnoge druge. Da biste pronašli cjelovit popis značajki podržanih, posjetite [Aplikacije pristupnik za pregled](application-gateway-introduction.md)

U ovom se članku vodit će vas kroz korake za stvaranje, konfiguriranje, pokrenuti i brisanje pristupnika za aplikacije.

## <a name="before-you-begin"></a>Prije početka

1. Instalirajte najnoviju verziju cmdleta ljuske PowerShell Azure pomoću platforme Installer Web. Možete preuzeti i instalirati najnoviju verziju [preuzimanja stranice](https://azure.microsoft.com/downloads/)u odjeljku **Komponente Windows PowerShell** .
2. Ako imate postojeću virtualne mrežu, odaberite postojeći prazan podmreže ili stvorite novi podmreže u postojeće virtualne mreže isključivo za korištenje pristupnik za aplikaciju. Ne možete implementirati pristupnik aplikacije drugoj virtualne mreži resursi namjeravate iza pristupnika aplikacije osim ako vnet peering koristi. Da biste saznali dodatne informacije pročitajte [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)
3. Provjerite je li funkcionalnu virtualna mrežu s valjani podmreže. Pripazite da virtualnim strojevima ni oblaka implementaciji koristite podmreži. Pristupnik za aplikaciju mora biti sam podmreži virtualne mreže.
3. Mora postojati poslužitelje sustava konfigurirati da biste koristili aplikaciju pristupnika ili dodijeljenih njihove krajnje točke u virtualne mreže ili s javnu IP/VIP stvorili.

## <a name="what-is-required-to-create-an-application-gateway"></a>Što je potrebno da biste stvorili pristupnik za aplikaciju?

Kada koristite naredbe **Nova AzureApplicationGateway** da biste stvorili pristupnik aplikacije, bez konfiguracije sada postavljeno, a konfigurirane novostvorenu resursa pomoću XML-a ili konfiguracije objekta.

Vrijednosti su:

- **Skup pozadinskih poslužitelja:** Popis IP adrese pozadinskih poslužitelja. IP adrese na popisu ili trebao bi se nalaziti podmreže virtualne mreže ili mora biti javnu IP/VIP.
- **Skup postavke poslužitelja za pozadinsku:** Svaki skup ima postavke kao što su priključak, protokol i sustavom kolačića afinitet. Ove postavke je uz zajedničko područje, a primjenjuju se na svim poslužiteljima u na resurse.
- **Sučelja priključka:** U ovom priključak je javno priključak koja je otvorena na računala pristupnika. Promet dodirne priključak, a zatim prosljeđuje na neki od pozadinskih poslužitelja.
- **Ga slušatelj:** Ga slušatelj sadrži sučelja priključak protokol (Http ili Https, te su vrijednosti velika i mala slova), a naziv SSL certifikata (Ako je konfiguriranje SSL offload).
- **Pravilo:** Povezuje ga slušatelj i skup pozadinskih poslužitelja i pravila definira grupe aplikacija pozadinskih poslužitelja promet treba usmjeriti na kada je dodirne određeni ga slušatelj.

## <a name="create-an-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Da biste stvorili pristupnik za aplikaciju:

1. Stvaranje resursa za pristupnik programa aplikacije.
2. Stvaranje XML datoteku za konfiguraciju ili konfiguracije objekta.
3. Izvršavanje konfiguraciju pristupnika resursu novostvorenu aplikacije.

>[AZURE.NOTE] Ako morate konfigurirati prilagođene probni za pristupnik za aplikaciju, potražite u članku [Stvaranje pristupnika za aplikaciju s prilagođene probes pomoću komponente PowerShell](application-gateway-create-probe-classic-ps.md). Pogledajte [prilagođene probes i nadzor stanja](application-gateway-probe-overview.md) da biste saznali više.

![Primjer scenarija][scenario]

### <a name="create-an-application-gateway-resource"></a>Stvaranje resursa za pristupnik programa aplikacije

Da biste stvorili pristupnik, koristite cmdlet **Novo AzureApplicationGateway** zamjenu vrijednosti s vlastitim. Naplata pristupnika ne pokreće se sada. Naplata počinje u noviji koraku pristupnika uspješno pokrenut.

Sljedeći primjer stvara pristupnik za aplikaciju putem virtualne mreže pod nazivom "testvnet1" i podmreže pod nazivom "podmreže-1".


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Opis*, *InstanceCount*i *GatewaySize* su neobavezni parametri.

Da biste provjerili je li stvoren pristupnika, koristite cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  Zadane vrijednosti za *InstanceCount* je 2, s maksimalnom vrijednošću od 10. Zadana vrijednost za *GatewaySize* je srednja. Na raspolaganju su vam male i Srednje velika.

*VirtualIPs* i *DnsName* prikazani su sljedeći prazno jer pristupnika još nije započelo. Ove se stvaraju kada pristupnik je u stanju izvršavanja.

## <a name="configure-the-application-gateway"></a>Konfigurirati pristupnik za aplikaciju

Možete konfigurirati pristupnik aplikacije pomoću XML-a ili konfiguracije objekta.

## <a name="configure-the-application-gateway-by-using-xml"></a>Konfiguriranje računala pristupnika pomoću XML

U sljedećem primjeru pomoću XML datoteku možete konfigurirati postavke pristupnika sve aplikacije i primjenu resursa za pristupnik za aplikacije.  

### <a name="step-1"></a>Korak 1  

Kopirajte sljedeći tekst u blok za pisanje.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Uredite vrijednosti u zagradama za konfiguraciju stavke. Spremite datoteku s nastavkom .xml.

>[AZURE.IMPORTANT] Stavka protokol Http ili Https je velika i mala slova.

Sljedeći primjer pokazuje kako koristiti konfiguracijska datoteka za postavljanje pristupnika aplikacije. Primjer opterećenje salda HTTP promet na javno priključak 80 i šalje mrežni promet pozadinske priključak 80 između dvije IP adrese.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Korak 2

Nakon toga postavite pristupnik za aplikacije. Pomoću cmdleta **Skup AzureApplicationGatewayConfig** s XML datotekom konfiguracije.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Konfiguriranje računala pristupnika pomoću konfiguracije objekta

Sljedeći primjer pokazuje kako konfigurirati pristupnik aplikacije pomoću konfiguracije objekte. Sve stavke konfiguracije mora biti konfigurirana pojedinačno i dodati objekt za konfiguraciju pristupnika aplikacija. Nakon stvaranja konfiguracija objekt, koristite naredbu **Skup AzureApplicationGateway** da biste potvrdili konfiguraciju pristupnika resursu prethodno stvorena aplikacija.

>[AZURE.NOTE] Prije dodijelite vrijednost za svaki objekt konfiguracije, morate deklarirati Kakvu vrstu objekta PowerShell koristi se za pohranu. U prvom retku da biste stvorili pojedinačne stavke definira koje Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (naziv objekta) koriste.

### <a name="step-1"></a>Korak 1

Stvorite sve konfiguracije pojedinačne stavke.

Stvaranje pristupne IP kao što je prikazano u sljedećem primjeru.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Stvaranje pristupne priključak kao što je prikazano u sljedećem primjeru.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Stvaranje grupe aplikacija pozadinskih poslužitelja.

 Definiranje IP adrese koje su dodane skup pozadinskih poslužitelja kao što je prikazano u sljedećem primjeru.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Pomoću objekta $server za zbrajanje vrijednosti na objekt pozadinske skup ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Stvaranje grupe aplikacija postavku pozadinskih poslužitelja.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Stvorite ga slušatelj.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Stvorite pravilo.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Korak 2

Dodijelite sve stavke za pojedinačne konfiguracije pristupnika konfiguracije objekt aplikacija ($appgwconfig).

Dodajte sučelja IP konfiguracije.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Dodajte sučelja priključak konfiguracije.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Dodavanje pozadinskih poslužitelja skup konfiguracije.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

Dodajte postavku pozadinske skup konfiguraciju.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Dodajte ga slušatelj konfiguracije.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Dodajte pravilo konfiguracije.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Korak 3

Primjenu objekt konfiguracije resursa za pristupnik aplikacije pomoću **Skupa AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Pokretanje pristupnika

Kada konfigurirao pristupnika pomoću cmdleta **Start AzureApplicationGateway** da biste pokrenuli pristupnika. Naplata za pristupnik za aplikaciju započinje nakon pristupnika uspješno pokrenut.

> [AZURE.NOTE] Cmdlet **Start AzureApplicationGateway** može potrajati do 15 do 20 minuta da biste završili.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Provjerite je li stanje pristupnika

Pomoću cmdleta **Get-AzureApplicationGateway** da biste provjerili status pristupnika. Ako **Start AzureApplicationGateway** uspješno u prethodnom koraku, *Stanje* trebao bi biti pokrenut i *Vip* i *DnsName* mora imati valjanih unosa.

Sljedeći primjer prikazuje pristupnik za aplikaciju koja je prema gore, pokretanje, i spremno za preuzimanje promet namijenjene prikazivanju na `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Brisanje pristupnika za aplikaciju

Da biste izbrisali pristupnik za aplikaciju:

1. Pomoću cmdleta **Zaustavi AzureApplicationGateway** da biste prestali pristupnika.
2. Pomoću cmdleta **Ukloni AzureApplicationGateway** da biste uklonili pristupnika.
3. Provjerite da je uklonjen pristupnika pomoću cmdleta **Get-AzureApplicationGateway** .

Sljedeći primjer prikazuje cmdlet **Zaustavi AzureApplicationGateway** u prvom retku, a zatim izlaz.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Kad se pristupnik za aplikacije u prestao stanju, pomoću cmdleta **Ukloni AzureApplicationGateway** da biste uklonili servis.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Da biste provjerili je li servis je uklonjen, koristite cmdlet **Get-AzureApplicationGateway** . Ovaj korak nije potrebna.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Daljnji koraci

Ako želite konfigurirati rasterećivanje SSL, potražite u članku [Konfiguriranje pristupnik za aplikaciju za SSL offload](application-gateway-ssl.md).

Ako želite konfigurirati pristupnik programa aplikacija će se koristiti za interne opterećenja potražite u članku [Stvaranje pristupnika za aplikaciju s internim raspoređivača opterećenja (ILB)](application-gateway-ilb.md).

Dodatne informacije o učitavanje ujednačavanje mogućnosti Općenito, potražite u članku:

- [Azure opterećenja](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Upravitelj promet](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png