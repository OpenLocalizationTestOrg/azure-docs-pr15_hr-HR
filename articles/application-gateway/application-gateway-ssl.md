<properties
   pageTitle="Konfigurirati pristupnik za aplikaciju za SSL rasterećivanje pomoću klasične implementacije | Microsoft Azure"
   description="Ovaj članak sadrži upute za stvaranje pristupnika za aplikaciju SSL offload pomoću Azure uvođenje klasičnog modela."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Konfigurirati pristupnik za aplikaciju za rasterećivanje SSL pomoću model klasični implementacije

> [AZURE.SELECTOR]
-[Portal za Azure](application-gateway-ssl-portal.md)
-[Azure resursima PowerShell](application-gateway-ssl-arm.md)
-[Azure klasični PowerShell](application-gateway-ssl.md)

Da biste prekinuli sesiju Secure Sockets Layer (SSL) pristupnika da biste izbjegli skup zadaci dešifriranje SSL da se dogodi u farmi web moguće je konfigurirati pristupnik Azure aplikacije. SSL rasterećivanje pojednostavljuje i postavljanje poslužitelj sučelja i upravljanja web-aplikacije.

## <a name="before-you-begin"></a>Prije početka

1. Instalirajte najnoviju verziju cmdleta ljuske PowerShell Azure pomoću platforme Installer Web. Možete preuzeti i instalirati najnoviju verziju [preuzimanja stranice](https://azure.microsoft.com/downloads/)u odjeljku **Komponente Windows PowerShell** .
2. Provjerite je li mreža virtualne radne uz valjani podmreže. Pripazite da virtualnim strojevima ni oblaka implementaciji koristite podmreži. Pristupnik za aplikaciju mora biti sam podmreži virtualne mreže.
3. Mora postojati poslužitelje sustava konfigurirati da biste koristili aplikaciju pristupnika ili dodijeljenih svoje krajnje točke u virtualne mreže ili s javnu IP/VIP stvorili.

Da biste konfigurirali SSL rasterećivanje pristupnik za aplikaciju, učinite sljedeće korake, navedenim redoslijedom:

1. [Stvaranje pristupnika za aplikaciju](#create-an-application-gateway)
2. [Prijenos SSL certifikata](#upload-ssl-certificates)
3. [Konfiguriranje pristupnika](#configure-the-gateway)
4. [Postavljanje konfiguracije pristupnika](#set-the-gateway-configuration)
5. [Pokretanje pristupnika](#start-the-gateway)
6. [Provjerite je li stanje pristupnika](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Da biste stvorili pristupnik, koristite cmdlet **Novo AzureApplicationGateway** zamjenu vrijednosti s vlastitim. Naplata pristupnika ne pokreće se sada. Naplata počinje u noviji koraku pristupnika uspješno pokrenut.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Da biste provjerili je li stvoren pristupnika, koristite cmdlet **Get-AzureApplicationGateway** .

Na primjer, *Opis*, *InstanceCount*i *GatewaySize* su neobavezni parametri. Zadane vrijednosti za *InstanceCount* je 2, s maksimalnom vrijednošću od 10. Zadana vrijednost za *GatewaySize* je srednja. Male i velike su druge dostupne vrijednosti. *VirtualIPs* i *DnsName* prikazani su sljedeći prazno jer pristupnika još nije započelo. Ove vrijednosti se stvaraju kada pristupnik je u stanju izvršavanja.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>Prijenos SSL certifikata

**Dodavanje AzureApplicationGatewaySslCertificate** koristiti da biste prenijeli certifikat poslužitelja u obliku *pfx* pristupnika za aplikacije. Naziv certifikata je odabrao korisnik naziv i mora biti jedinstvena u pristupnik za aplikacije. Taj certifikat naziva s tim nazivom u sve operacije upravljanja certifikata na računala pristupnika.

U ovom sljedeći primjer prikazuje cmdlet, zamijenite vrijednosti u uzorku vlastitu.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Nakon toga provjeru valjanosti certifikata prijenos. Pomoću cmdleta **Get-AzureApplicationGatewayCertificate** .

Ovaj primjer prikazuje cmdlet u prvom retku slijedi izlaz.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Lozinka certifikat mora biti između 4 do 12 znakova, slova i brojeva. Posebni znakovi se prihvaćaju.

## <a name="configure-the-gateway"></a>Konfiguriranje pristupnika

Konfiguracije pristupnika za aplikacije se sastoji od više vrijednosti. Vrijednosti možete uz zajedno za sastavljanje konfiguracije.

Vrijednosti su:

- **Skup pozadinskih poslužitelja:** Popis IP adrese pozadinskih poslužitelja. IP adrese na popisu ili trebao bi se nalaziti podmreže virtualne mreže ili mora biti javnu IP/VIP.
- **Skup postavke poslužitelja za pozadinsku:** Svaki skup ima postavke kao što su priključak, protokol i sustavom kolačića afinitet. Ove postavke je uz zajedničko područje, a primjenjuju se na svim poslužiteljima u na resurse.
- **Sučelja priključka:** U ovom priključak je javno priključak koja je otvorena na računala pristupnika. Promet dodirne priključak, a zatim prosljeđuje na neki od pozadinskih poslužitelja.
- **Ga slušatelj:** Ga slušatelj sadrži sučelja priključak protokol (Http ili Https, te su vrijednosti velika i mala slova), a naziv SSL certifikata (Ako je konfiguriranje SSL offload).
- **Pravilo:** Povezuje ga slušatelj i skup pozadinskih poslužitelja i pravila definira grupe aplikacija pozadinskih poslužitelja promet treba usmjeriti na kada je dodirne određeni ga slušatelj. Trenutno podržana je samo *Osnovna* pravila. *Osnovno* je pravilo je opterećenja kružnog distribucije.

**Dodatna konfiguracija bilješke**

Za SSL certifikate konfiguracija protokol u **HttpListener** trebali biste promijeniti *Https* (velika i mala slova). **SslCert** element dodaje se **HttpListener** vrijednost postavljena na isti naziv kao korištenih u prijenos ispred SSL certifikata sekcije. Trebali biste se ažurirati sučelja priključak 443.

**Omogućivanje kolačića sustavom afinitet**: pristupnik za aplikacije koje se mogu konfigurirati da biste bili sigurni da zahtjev iz klijentske sesije uvijek usmjereni na isti VM u web-farme. Ovaj scenarij obavlja tvrtka unos kolačića sesije koja omogućuje pristupnika da biste usmjerili promet pravilno. Da biste omogućili afinitet utemeljen na kolačića, postavite **CookieBasedAffinity** na *omogućeni* u **BackendHttpSettings** element.



Konfiguraciju možete sastaviti stvaranjem konfiguracije objekt ili pomoću konfiguracije XML datoteke.
Sastavljanje konfiguraciju pomoću konfiguracije XML datoteku, koristite sljedeći primjer:

**Ogledna XML konfiguracija**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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

Nakon toga postavite pristupnik za aplikacije. **Postavljanje AzureApplicationGatewayConfig** cmdlet možete koristiti s objektom konfiguraciju ili s XML datotekom konfiguracije.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Pokretanje pristupnika

Kada konfigurirao pristupnika pomoću cmdleta **Start AzureApplicationGateway** da biste pokrenuli pristupnika. Naplata za pristupnik za aplikaciju započinje nakon pristupnika uspješno pokrenut.


**Bilješke:** Cmdlet **Start AzureApplicationGateway** može potrajati do 15 do 20 minuta da biste završili.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Provjerite je li stanje pristupnika

Pomoću cmdleta **Get-AzureApplicationGateway** da biste provjerili status pristupnika. Ako **Start AzureApplicationGateway** uspješno u prethodnom koraku, *Stanje* trebao bi biti pokrenut i *VirtualIPs* i *DnsName* mora imati valjanih unosa.

Ovaj primjer prikazuje pristupnik za aplikacije koji je prema gore, sustav, a spremni za bilježenje promet.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Daljnji koraci


Dodatne informacije o učitavanje ujednačavanje mogućnosti Općenito, potražite u članku:

- [Azure opterećenja](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Upravitelj promet](https://azure.microsoft.com/documentation/services/traffic-manager/)