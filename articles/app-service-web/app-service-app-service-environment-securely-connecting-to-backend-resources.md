<properties 
    pageTitle="Sigurno povezivanje s pozadinskom resursa iz okruženja aplikacije servisa" 
    description="Saznajte kako sigurno povezivanje s resursima pozadinskog iz okruženja aplikacije servisa." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Sigurno povezivanje s pozadinskom resursa iz okruženja aplikacije servisa #

## <a name="overview"></a>Pregled ##
Budući da uvijek stvaranja okruženja aplikacije servisa u **svakom** Voditelj resursa Azure virtualne mreže, **ili** klasični implementaciju modela [virtualne mreže][virtualnetwork], možete izlazne veze iz okruženja aplikacije servisa na druge resurse za pozadinskog tijeka isključivo putem virtualne mreže.  S nedavnih promjena u lipnja 2016, ASEs također može uvesti u virtualne mreže koje koriste rasponi javno adresa ili razmake RFC1918 adresa (odnosno Privatna adresa).  

Na primjer, možda postoji SQL Server koja se izvodi na klaster virtualnih računala s priključkom 1433 zaključan prema dolje.  Krajnja točka možda ACLd samo dopustiti pristup s drugih resursa na isti virtualne mreže.  

Drugi primjer osjetljive krajnje točke pokrenuti lokalnog i povezati se Azure putem [Web-mjesto] [ SiteToSite] ili [Azure ExpressRoute] [ ExpressRoute] veze.  Zbog toga samo resursa u virtualne mreže povezano s web-mjesto ili ExpressRoute tunnels moći za pristup lokalnog krajnje točke.

Za sve scenarija aplikacije koji se izvode na okruženja aplikacije servisa će moći sigurno povezivanje na razne poslužitelje i resurse.  Izlazni promet iz aplikacija sa sustavom u okruženju aplikacije servisa privatne krajnje točke u istom virtualne mreže (ili povezani s istom virtualne) će samo tijek putem virtualne mreže.  Odlazni promet privatne krajnje točke će preljeva putem javnog Interneta.

Jedan caveat odnosi se na izlazni promet iz okruženja aplikacije servisa za krajnje točke unutar virtualne mreže.  Aplikacije servisa za okruženja u kojima se ne može pristupiti krajnje točke virtualnih računala koja se nalazi u **istoj** podmreži okruženje za aplikaciju servisa.  To obično ne smije biti problem dok god okruženja aplikacije servisa uvode u podmreže rezervirana za isključivo samo u okruženju servisa aplikacija.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Izlazni povezivanje i preduvjete za DNS-a ##
Za servis okruženju aplikacije ispravan zahtijeva izlaznog pristup različite krajnje točke. U odjeljku "Potrebna veza s mrežom" u članku [Konfiguracija mreže za ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) je se cijeli popis vanjskih krajnje točke koristi se elika i mala slova.

Aplikacije servisa za okruženja potreban je valjan DNS infrastrukture konfigurirana za virtualne mreže.  Ako iz bilo kojeg razloga DNS konfiguraciju promijeniti nakon stvaranja okruženja aplikacije servisa, razvojni inženjeri možete pokrenuti aplikaciju servisa okruženje za obraditi Nova konfiguracija DNS-a.  Pokretanje rolling okruženje za ponovno korištenje ikona "Ponovno pokretanje" koja se nalazi pri vrhu plohu na portalu za upravljanje okruženja aplikacije servisa može dovesti do okruženje za obraditi Nova konfiguracija DNS-a.

Također preporučuje se da se neki prilagođeni DNS poslužitelji na na vnet postavljanje na vrijeme prije stvaranja okruženja aplikacije servisa.  Ako virtualne mreže DNS konfiguraciju Promijeni tijekom stvaranja okruženja aplikacije servisa, koji će rezultirati neuspjeh za postupak stvaranja okruženja aplikacije servisa.  U slične vein, ako prilagođeni DNS poslužitelj postoji dijametralno VPN pristupnika i DNS poslužitelj nije dostupan ili nije dostupan, postupak stvaranja okruženja aplikacije servisa također neće uspjeti.

## <a name="connecting-to-a-sql-server"></a>Povezivanje sa sustavom SQL Server
Uobičajeni konfiguracije SQL poslužitelja sastoji se od krajnje slušanje na priključak 1433:

![Krajnja točka za SQL Server][SqlServerEndpoint]

Postoje dva načina prikaza za ograničavanje promet ovaj krajnja točka:


- [Mrežni popise kontrole pristupa] [ NetworkAccessControlLists] (mreže ACL-a)

- [Mrežni sigurnosne grupe][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Ograničavanje pristupa s mrežom ACL-a

Priključak 1433 mogu zaštiti pomoću popis za kontrolu pristupa mreže.  Primjer ispod whitelists klijenta adrese potječu unutar virtualne mreže i blokira pristup za sve klijente.

![Primjer popisa za mrežni pristup kontrola][NetworkAccessControlListExample]

Sve programe koji se izvodi u okruženju aplikacije servisa u istom virtualne mreže kao SQL Server bit će moći spojiti na instancu sustava SQL Server pomoću **VNet interne** IP adresa za SQL Server virtualnog računala.  

Primjer niza za povezivanje ispod reference SQL Server pomoću privatne IP adrese.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Iako virtualnog računala ima kao i javne krajnje, pokušaje vezu na javnu IP adresu pomoću će razlozi za mrežni ACL-a. 

## <a name="restricting-access-with-a-network-security-group"></a>Ograničavanje pristupa s mreže sigurnosne grupe
Zamjenski pristup za osiguravanje access je s mreže sigurnosne grupe.  Mrežni sigurnosne grupe mogu primijeniti pojedinačne virtualnim strojevima ili podmreži koja sadrži virtualnim strojevima.

Najprije sigurnosne grupe mreže treba stvoriti:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Ograničavanje pristupa samo VNet Interna promet je vrlo jednostavne s mreže sigurnosne grupe.  Zadana pravila u sigurnosnoj grupi mrežni samo dopustiti pristup s drugim klijentima mreže u istom virtualne mreže.

Jednostavno primjene sigurnosne grupe mreže s njegova zadana pravila ili na virtualnim strojevima sa sustavom SQL Server ili podmreže koja sadrži virtualnim strojevima je kao rezultat zaključavanje dolje pristup sustava SQL Server.

Ogledna ispod primjenjuje mreže sigurnosne grupe u kojem se nalazi podmreži:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Rezultat je skup sigurnosnih pravila koja Blokiranje vanjskog pristupa istovremeni VNet Interna access:

![Zadana mreže sigurnosna pravila][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Početak rada
Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Da biste započeli aplikacije servisa okruženja, potražite u članku [Uvod u okruženje za aplikaciju servisa][IntroToAppServiceEnvironment]

Detalje oko kontrola dolazni promet na vaše okruženje servisa aplikacija potražite u članku [kontroliranje dolazni promet prema okruženju servisa aplikacija][ControlInboundASE]

Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
