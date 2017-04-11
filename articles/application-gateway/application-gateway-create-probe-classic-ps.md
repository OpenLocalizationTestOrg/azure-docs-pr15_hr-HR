<properties
   pageTitle="Stvaranje prilagođene probni za pristupnik za aplikaciju pomoću komponente PowerShell u modelu uvođenje klasičnog | Microsoft Azure"
   description="Saznajte kako stvoriti prilagođene probni za pristupnik za aplikaciju pomoću komponente PowerShell u modelu klasični implementacije"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Stvaranje prilagođene probni za Azure aplikacije pristupnika (klasični) pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-probe-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-probe-ps.md)
- [Azure klasični PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Da biste stvorili pristupnik za aplikaciju:

1. Stvaranje resursa za pristupnik programa aplikacije.
2. Stvaranje XML datoteku za konfiguraciju ili konfiguracije objekta.
3. Izvršavanje konfiguraciju pristupnika resursu novostvorenu aplikacije.

### <a name="create-an-application-gateway-resource"></a>Stvaranje resursa za pristupnik programa aplikacije

Da biste stvorili pristupnik, koristite cmdlet **Novo AzureApplicationGateway** zamjenu vrijednosti s vlastitim. Naplata pristupnika ne pokreće se sada. Naplata počinje u noviji koraku pristupnika uspješno pokrenut.

Sljedeći primjer stvara pristupnik za aplikaciju putem virtualne mreže pod nazivom "testvnet1" i podmreže pod nazivom "podmreže-1".

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Da biste provjerili je li stvoren pristupnika, koristite cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  Zadane vrijednosti za *InstanceCount* je 2, s maksimalnom vrijednošću od 10. Zadana vrijednost za *GatewaySize* je srednja. Na raspolaganju su vam male i Srednje velika.

 *VirtualIPs* i *DnsName* prikazani su sljedeći prazno jer pristupnika još nije započelo. Ove se stvaraju kada pristupnik je u stanju izvršavanja.

## <a name="configure-an-application-gateway"></a>Konfigurirati pristupnik za aplikaciju

Možete konfigurirati pristupnik aplikacije pomoću XML-a ili konfiguracije objekta.

## <a name="configure-an-application-gateway-by-using-xml"></a>Konfigurirati pristupnik za aplikaciju pomoću XML

U sljedećem primjeru pomoću XML datoteku možete konfigurirati postavke pristupnika sve aplikacije i primjenu resursa za pristupnik za aplikacije.  

### <a name="step-1"></a>Korak 1

Kopirajte sljedeći tekst u blok za pisanje.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Uredite vrijednosti u zagradama za konfiguraciju stavke. Spremite datoteku s nastavkom .xml.

Sljedeći primjer pokazuje kako koristiti konfiguracijska datoteka da biste postavili pristupnik za aplikaciju učitavanje saldo HTTP promet na javno priključak 80 i poslati mrežni promet pozadinske priključak 80 između dvije IP adrese pomoću prilagođenih probni.

>[AZURE.IMPORTANT] Stavka protokol Http ili Https je velika i mala slova.

Nova stavka konfiguracije \<isprobati\> dodaje se konfigurirati prilagođene probes.

Konfiguriranje parametre su:

- **Ime** - referenca naziv prilagođene probni.
- **Protokol** - protokol koji se koristi (mogućih vrijednosti su HTTP ili HTTPS).
- **Glavno računalo** i **put** – put cijeli URL koji se poziva pristupnik za računala da biste odredili stanje instance. Ako, na primjer, ako imate web-mjesto http://contoso.com/, zatim prilagođene probni možete konfigurirati za "http://contoso.com/path/custompath.htm" za probni provjere da bi uspješno HTTP odgovor.
- **Interval** - konfigurira provjere interval probni u sekundama.
- **Prekoračenje vremena** - definira isteka probni za provjeru za odgovor na HTTP-a.
- **UnhealthyThreshold** – broj neuspjelih HTTP odgovora potrebne da biste označili instancu pozadinske kao *dobro*.

Naziv probni su navedeni u na <BackendHttpSettings> konfiguracije da biste dodijelili koji skup pozadinske koristi probni prilagođene postavke.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Dodavanje prilagođene probni konfiguracije postojeće pristupnik za aplikaciju

Promjena trenutnog konfiguracije pristupnika za aplikacije potreban je tri koraka: početak trenutne konfiguracijska datoteka XML, izmijeniti da bi prilagođene probni i konfigurirati pristupnik aplikacije s novim postavkama XML.

### <a name="step-1"></a>Korak 1

Dohvaćanje XML datoteke pomoću get-AzureApplicationGatewayConfig. To izvoz konfiguracije XML za mijenjanje da biste dodali probni postavku.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Korak 2

Otvorite XML datoteku u uređivaču teksta. Dodavanje u `<probe>` sekcije nakon `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

U odjeljku backendHttpSettings XML dodajte naziv probni kao što je prikazano u sljedećem primjeru:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Spremanje XML datoteke.

### <a name="step-3"></a>Korak 3

Ažurirajte pristupnik konfiguracije aplikacije novu XML datoteku pomoću **Skupa AzureApplicationGatewayConfig**. Time će se obnoviti pristupnik za aplikacije u konfiguraciji novi.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Daljnji koraci

Ako želite konfigurirati rasterećivanje Secure Sockets Layer (SSL), potražite u članku [Konfiguriranje pristupnik za aplikaciju za SSL offload](application-gateway-ssl.md).

Ako želite konfigurirati pristupnik programa aplikacija će se koristiti za interne opterećenja potražite u članku [Stvaranje pristupnika za aplikaciju s internim raspoređivača opterećenja (ILB)](application-gateway-ilb.md).
