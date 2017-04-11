<properties 
   pageTitle="Stvaranje i konfiguriranje pristupnik za aplikaciju s internim opterećenja (ILB) u virtualne mreže | Microsoft Azure"
   description="Ova stranica sadrži upute za konfiguriranje programa pristupnika aplikacije Azure s krajnje Interna raspoređen učitavanja"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Stvaranje pristupnika za aplikaciju s internim raspoređivača opterećenja (ILB)

> [AZURE.SELECTOR]
- [Azure klasični korake](application-gateway-ilb.md)
- [Upute za Powershell Voditelj resursa](application-gateway-ilb-arm.md)

Pristupnik za računala može se konfigurirati s Interneta nasuprotne virtualne IP ili krajnja točka za interne izložen s Internetom, poznata i kao krajnja točka Interna raspoređivača opterećenja (ILB). Konfiguraciju pristupnika pomoću programa ILB je korisno za interne redak tvrtke aplikacije ne prikazuje na Internetu. Također je korisno za usluge i razine u aplikaciji više razina, koji se nalazi u granicu sigurnost nije vidljiva na Internetu, ali i dalje potreban kružnog učitavanja raspodjele, stickiness sesiju ili prekid SSL. U ovom se članku vodit će vas kroz korake da biste konfigurirali pristupnik za aplikaciju programa ILB.

## <a name="before-you-begin"></a>Prije početka

1. Instalirajte najnoviju verziju programa Azure PowerShell cmdleti pomoću platforme Installer Web. Možete preuzeti i instalirati najnoviju verziju **Komponente Windows PowerShell** iz odjeljka s na [stranici za preuzimanje](https://azure.microsoft.com/downloads/).
2. Provjerite je li funkcionalnu virtualna mrežu s valjanu.
3. Provjerite imate li pozadinskog poslužitelja u virtualne mreže ili s na javnu IP/VIP dodijeljeni.


Da biste stvorili pristupnik za aplikaciju, navedenim redoslijedom poduzeti sljedeće korake. 

1. [Stvaranje pristupnika za aplikaciju](#create-a-new-application-gateway)
2. [Konfiguriranje pristupnika](#configure-the-gateway)
3. [Postavljanje konfiguracije pristupnika](#set-the-gateway-configuration)
4. [Pokretanje pristupnika](#start-the-gateway)
4. [Provjerite je li pristupnik](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Stvaranje pristupnika za aplikaciju:

**Stvaranje pristupnika**, koristite na `New-AzureApplicationGateway` cmdlet, zamjenu vrijednosti s vlastitim. Imajte na umu da naplata pristupnika ne pokreće se sada. Naplata počinje u noviji koraku pristupnika uspješno pokrenut.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Da biste provjerili valjanost** pristupnika stvoren, možete koristiti u `Get-AzureApplicationGateway` cmdlet. 

Na primjer, *Opis*, *InstanceCount*i *GatewaySize* su neobavezni parametri. Zadane vrijednosti za *InstanceCount* je 2, s maksimalnom vrijednošću od 10. Zadana vrijednost za *GatewaySize* je srednja. Male i velike su druge dostupne vrijednosti. *VIP* i *DnsName* prikazani su sljedeći prazno jer pristupnika još nije započelo. Ove se stvaraju kada pristupnik je u stanju izvršavanja. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Konfiguriranje pristupnika

Konfiguracije pristupnika za aplikacije se sastoji od više vrijednosti. Vrijednosti možete uz zajedno za sastavljanje konfiguracije.
 
Vrijednosti su:

- **Skup pozadinskog poslužitelja:** Popis IP adrese pozadinskog poslužitelja. IP adrese na popisu ili trebao bi se nalaziti podmreže VNet ili mora biti javnu IP/VIP. 
- **Postavke grupe aplikacija za pozadinskog poslužitelja:** Svaki skup ima postavke kao što su priključak, protokol i sustavom kolačića afinitet. Ove postavke je uz zajedničko područje, a primjenjuju se na svim poslužiteljima u na resurse.
- **Sučelju priključka:** Ovaj priključak je javno priključak otvoriti na računala pristupnika. Promet dodirne priključak, a zatim prosljeđuje na neki od pozadinskog poslužitelja.
- **Ga slušatelj:** Ga slušatelj sadrži sučelju priključak protokol (Http ili Https, to su velika i mala slova), a naziv SSL certifikata (Ako je konfiguriranje SSL offload). 
- **Pravilo:** Povezuje ga slušatelj i resurse poslužitelja pozadinskog sustava i pravila definira skup poslužitelja koji pozadinskog promet treba usmjeriti na kada je dodirne određeni ga slušatelj. Trenutno podržana je samo *Osnovna* pravila. *Osnovno* je pravilo je opterećenja kružnog distribucije.

Konfiguraciju možete sastaviti stvaranjem konfiguracije objekt ili pomoću konfiguracije XML datoteke. Sastavljanje konfiguraciju pomoću konfiguracije XML datoteku, koristite uzorak u nastavku.



Imajte na umu sljedeće:


- *FrontendIPConfigurations* element opisuju važnih za konfiguriranje aplikacije pristupnika pomoću programa ILB ILB detalje. 

- IP sučelju *Vrsta* mora biti postavljeno na "Privatnu"

- *StaticIPAddress* mora biti postavljena na željeni interne IP na kojem pristupnika prima promet. Imajte na umu da je *StaticIPAddress* element neobavezno. Ako ne želite skupa dostupna interne IP iz distribuiranih podmreže je odabran. 

- Vrijednost element *naziv* koji je naveden u *FrontendIPConfiguration* moraju se koristiti u element u HTTPListener *FrontendIP* da biste se pozvali na FrontendIPConfiguration.

 **Ogledna XML konfiguracija**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Postavljanje konfiguracije pristupnika

Nakon toga ćete postaviti pristupnik za aplikacije. Možete koristiti u `Set-AzureApplicationGatewayConfig` cmdlet s objektom konfiguraciju ili s XML datotekom konfiguracije. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Pokretanje pristupnika

Kada konfigurirao pristupnika pomoću na `Start-AzureApplicationGateway` cmdlet da biste pokrenuli pristupnika. Naplata za pristupnik za aplikaciju započinje nakon pristupnika uspješno pokrenut. 


> [AZURE.NOTE] Na `Start-AzureApplicationGateway` cmdlet može potrajati do 15 do 20 minuta. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Provjerite je li stanje pristupnika

Korištenje na `Get-AzureApplicationGateway` cmdlet da biste provjerili status pristupnika. Ako *Start AzureApplicationGateway* uspješno u prethodnom koraku, stanje mora biti *pokrenut*i Vip i DnsName mora imati valjanih unosa. Ovaj primjer prikazuje cmdlet u prvom retku slijedi izlaz. U ovom primjeru pristupnika radi pa je spremni za bilježenje promet. 

> [AZURE.NOTE] Pristupnik za aplikaciju je konfiguriran tako da prihvaća prometa na ILB krajnjoj točki konfiguriran u 10.0.0.10 u ovom primjeru.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Daljnji koraci


Dodatne informacije o učitavanje ujednačavanje mogućnosti Općenito, potražite u članku:

- [Azure opterećenja](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Upravitelj promet](https://azure.microsoft.com/documentation/services/traffic-manager/)
