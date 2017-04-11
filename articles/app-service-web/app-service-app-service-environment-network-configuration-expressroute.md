<properties 
    pageTitle="Detalji o konfiguracija mreže za rad s usmjeravanje Express" 
    description="Mrežni konfiguracije detalje za pokretanje aplikacije servisa okruženja u virtualne mreže povezani je elektronička ExpressRoute." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Mrežni konfiguracije detalje aplikacije servisa za okruženjima ExpressRoute 

## <a name="overview"></a>Pregled ##
Korisnici mogu povezati [Azure ExpressRoute] [ ExpressRoute] sklopovske njihove virtualne mrežne infrastrukture tako da biste Azure proširivanje njihove lokalne mreže.  Servis okruženju aplikacije mogu se kreirati podmreži [virtualne mreže] [ virtualnetwork] infrastrukture.  Aplikacije koji se izvode na servis okruženja aplikacije pa možete uspostaviti sigurnu vezu pozadinske resursima dostupno samo putem veze ExpressRoute.  

Moguće je stvoriti okruženju aplikacije servisa u **svakom** Voditelj resursa Azure virtualne mreže, **ili** klasični implementaciju modela virtualne mreže.  S nedavnih promjena u lipnja 2016, ASEs sada također može uvesti u virtualne mreže koje koriste rasponi javno adresa ili razmake RFC1918 adresa (odnosno Privatna adresa). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Potrebne mrežne veze ##
Postoje preduvjete za povezivanje s mreže za aplikaciju servisa okruženja koje možda nije moguće prethodno poštovati virtualne mreže s programa ExpressRoute.  Okruženja aplikacije servisa za sve od sljedećeg potrebne da bi se ispravno funkcionirati:


-  Izlazni mrežne veze radi pohrane Azure krajnje točke diljem svijeta na oba priključke 80 i 443.  To obuhvaća krajnje točke koja se nalazi u istom području kao okruženje aplikacije servisa, kao i za pohranu krajnje točke nalaze u **drugim** Azure područja.  Razrješavanje Azure krajnje točke za pohranu u odjeljku sljedeće DNS domene: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* i *file.core.windows.net*.  
-  Izlazni mrežne veze sa servisom Azure datoteke na priključak 445.
-  Izlazni mrežne veze radi krajnje točke Sql baze podataka koja se nalazi u istom području kao okruženje za aplikaciju servisa.  Razrješavanje SQL DB krajnje točke u odjeljku sljedeće domene: *database.windows.net*.  Za tu radnju otvaranja pristup priključcima 1433, 11000 11999 i 14000 14999.  Dodatne informacije potražite u [članku o korištenju priključak V12 Sql baze podataka](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Izlazni mrežne veze radi Azure upravljanja ravnini krajnje točke (ASM i ARM krajnje točke).  To obuhvaća izlaznog povezivanje *management.core.windows.net* i *management.azure.com*. 
-  Izlazni mrežne veze radi *ocsp.msocsp.com*, *mscrl.microsoft.com* i *crl.microsoft.com*.  To je potrebno da podržava funkciju SSL.
-  Konfiguracija DNS-a za virtualne mreže mora imati sposobnost Razrješavanje sve krajnje točke i domene koje se spominju u ranije točke.  Ako ne može riješiti te krajnje točke, pokušaje stvaranje aplikacije servisa okruženje neće uspjeti te postojeće aplikacije servisa okruženja u kojima će se označiti kao dobro.
-  Izlazni pristup priključak 53 potreban je za komunikaciju s DNS poslužitelji.
-  Ako postoji prilagođeni DNS poslužitelj dijametralno pristupnika za VPN, DNS poslužitelja mora biti dostupan iz podmreže koja sadrži okruženje za aplikaciju servisa. 
-  Izlazni mrežni put ne putovanja putem interne tvrtke poslužitelji niti može biti prisilno tunneled na lokalni.  Na taj način mijenja učinkovitih NAT adresu izlazni mrežni promet iz servisa okruženja aplikacije.  Promjena adrese NAT izlazni mrežni promet programa aplikacije servisa okruženje će uzrokovati pogreške povezivanje mnogim krajnje točke naveden.  Rezultat nije uspjelo pokušaje stvaranja okruženja aplikacije servisa, kao i prethodno dobar aplikacije servisa okruženja u kojima se označiti kao dobro.  
-  Dolazni mrežni pristup tražene priključke za okruženja aplikacije servisa mora imati dopuštenje kao što je opisano u ovom [članku][requiredports].

Preduvjeti za DNS-a možete se podudarati zajamčiti valjani DNS Infrastruktura je konfiguriran i održava za virtualne mreže.  Ako iz bilo kojeg razloga DNS konfiguraciju promijeniti nakon stvaranja okruženja aplikacije servisa, razvojni inženjeri možete pokrenuti aplikaciju servisa okruženje za obraditi Nova konfiguracija DNS-a.  Pokretanje rolling ponovno okruženje pomoću ikone "Ponovno pokrenite" koja se nalazi pri vrhu plohu upravljanje okruženja aplikacije servisa [Azure portal] [ NewPortal] uzrokovat će okruženje za obraditi Nova konfiguracija DNS-a.

Dolazni mreže access koje će biti zadovoljen konfiguriranjem [mreže sigurnosne grupe] [ NetworkSecurityGroups] na podmreži okruženje servisa aplikacija dopustili potreban pristup kao što je opisano u ovom [članku][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Omogućivanje izlaznog mrežne veze za okruženju aplikacije servisa##
Prema zadanim postavkama novostvorenu elektronička ExpressRoute oglašava zadani smjer koji omogućuje izlaznog s Internetom.  S ovom konfiguracijom okruženju aplikacije servisa bit će moći povezati s drugim Azure krajnje točke.

No uobičajenih konfiguracije klijenta je za definiranje vlastitih zadani smjer (0.0.0.0/0) koje navodi izlaznog internetski promet na umjesto tijek lokalnog.  Ovaj tijek prometa invariably prijelomi okruženja aplikacije servisa jer odlazni promet ili blokirani lokalnog ili NAT bi one neprepoznatljivog skup adrese koje više ne funkcioniraju s različitim Azure krajnje točke.

Rješenje je da biste definirali (neke) korisnički definirane usmjerava (UDRs) na podmreži koja sadrži okruženje za aplikaciju servisa.  Na UDR definira usmjerava specifičnih za podmreže koji će se na snazi umjesto zadani smjer.

Ako je to moguće, preporučuje se da biste koristili konfiguraciju sljedeće:

- Konfiguriranje ExpressRoute oglašava 0.0.0.0/0, a zatim po zadanom prisilno tunnels sve odlazni promet na lokalni.
- UDR primjenjuje se na podmreži koja sadrži okruženje za aplikaciju servisa definira 0.0.0.0/0 sljedeći put vrstom Internet (primjer to nije ono prema dolje u ovom članku).

Kombinirani efekt od ovih koraka je da razinu podmreže UDR će prednost pred ExpressRoute prisilno tuneliranje, stoga osiguravanje izlaznog pristup Internetu iz servisa okruženja aplikacije.

> [AZURE.IMPORTANT] Usmjerava definirano u na UDR **mora** biti dovoljno specifična za prednost pred bilo koji usmjerava objavio ExpressRoute konfiguracije.  Primjeru u nastavku koristi 0.0.0.0/0 širok raspon adresu, a kao potencijalno mogu nehotice nadjačati usmjeravanje oglasa pomoću konkretne rasponi adresa.
>
>Aplikacije servisa okruženja nisu podržani s ExpressRoute konfiguracije tog **Unakrsno Oglasite usmjerava iz javne peering put do privatne peering put**.  Konfiguracija ExpressRoute koji imaju javno peering konfiguriran, primit će smjera od Microsofta za veliki skup rasponi Microsoft Azure IP adresa.  Ako su te rasponi adresa unakrsno objavljeno privatne peering puta, rezultat je da sve izlazne mrežnih paketa iz podmreže okruženje servisa aplikacija će biti tunneled prisilno klijenta lokalnog mrežne infrastrukture.  Ovaj tijek mreže trenutno nije podržano s okruženjima aplikacije servisa.  Jedan rješenja tog problema je da biste prestali usmjerava unakrsno oglašavanje iz javne peering put privatne peering put.

Osnovne informacije na korisnički definirane usmjerava dostupna je u [Pregled][UDROverview].  

Detalje o stvaranju i konfiguriranje korisnički definirane usmjerava dostupna je u [Način za vodič][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Primjer UDR konfiguracije okruženja aplikacije servisa ##

**Prije requisites**

1. Instaliranje modula Azure Powershell sa [stranice s preuzimanjima Azure] [ AzureDownloads] (datirana lipnja 2015 ili noviji).  U odjeljku "Alati naredbenog retka" postoji putem veze "Instalacija" u odjeljku "Komponente Windows Powershell" koji će se instalirati najnoviju cmdleta ljuske Powershell.

2. Preporučuje se da jedinstveni podmreže je za isključivo korištenje koji je stvorio okruženju aplikacije servisa.  Time se osigurava da UDRs primjenjuje se na podmreži samo otvorit će odlazni promet za aplikaciju servisa okruženje.
3. **Važno**: implementacija aplikacije servisa okruženje tek **nakon što** se sljedeći koraci za konfiguraciju.  Na taj način da izlaznog mrežne veze dostupna je prije implementacije okruženju aplikacije servisa.

**Korak 1: Stvaranje tablice imenovani usmjeravanje**

Sljedeći isječak stvara usmjeravanje tablicu pod nazivom "DirectInternetRouteTable" u regiji Zapad NAM Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Korak 2: Stvorite jedan ili više usmjerava u tablici usmjeravanje**

Morat ćete dodati jedan ili više usmjerava tablicu usmjeravanje da biste omogućili izlaznog pristup Internetu.  

Preporučeni način za konfiguriranje izlazne pristup Internetu je da biste odredili smjer 0.0.0.0/0 kao što je prikazano u nastavku.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Imajte na umu te 0.0.0.0/0 je adresa širok raspon i kao će biti nadjačati konkretne rasponi adresa objavio u ExpressRoute.  Ponovno iteracija starijim preporuke, UDR s 0.0.0.0/0 usmjeravanje treba koristiti u službi veznika konfiguraciji ExressRoute koje samo oglašava kao i 0.0.0.0/0. 

Umjesto toga, možete preuzeti potpun i ažurirani popis raspona CIDR koristi Azure.  Xml datoteku koja sadrži sve željene raspone Azure IP adresa dostupna iz [Microsoftova centra za preuzimanje][DownloadCenterAddressRanges].  

Imajte na umu kroz da te raspone mijenjaju tijekom vremena, stoga necessitating periodičku ručnim ažuriranjima usmjerava korisnički definirane za sinkroniziranosti.  Također, budući da je zadani gornju granicu od 100 usmjerava u jednom UDR, morat ćete "sažetak" rasponi Azure IP adresa tako da stane unutar ograničenja za 100 usmjeravanje Imajte na umu da UDR definirani usmjerava potrebno određeno smjerovima objavio vaše ExpressRoute.  


**Korak 3: Tablici usmjeravanje podmreže koja sadrži okruženje za aplikacije servisa za povezivanje**

Posljednji korak konfiguriranje je povezivanje tablice usmjeravanje podmreži gdje će biti implementirano u okruženju aplikacije servisa.  Sljedeća naredba povezuje "DirectInternetRouteTable" da biste na "ASESubnet" koja će sadržavati naposljetku okruženju aplikacije servisa.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Korak 4: Konačnih koraka**

Kada se tablica usmjeravanje povezana podmreži, preporučuje se da najprije testirajte i potvrdite svrhu efekt.  Na primjer, uvođenje virtualnog računala u podmreži i provjerite:


- Izlazni promet za krajnje točke Azure i koje nisu Azure ranije spomenutih u ovom članku je **ne** slijedi dolje elektronička ExpressRoute.  Vrlo je važno da biste provjerili takvo ponašanje jer ako odlazni promet od podmreži je i dalje se prisilno tunneled informacije o lokalnom aplikacije servisa okruženje stvaranja uvijek neće uspjeti. 
- Funkcije pretraživanja DNS-a za krajnjih točaka ranije spomenutih su sve rješavanje pravilno. 

Kada se uvjerite gore navedene korake, morate izbrisati virtualnog računala jer podmreži mora biti "prazan" prilikom stvaranja okruženja aplikacije servisa.
 
Zatim nastavite sa stvaranjem okruženju aplikacije servisa!

## <a name="getting-started"></a>Početak rada
Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Da biste započeli aplikacije servisa okruženja, potražite u članku [Uvod u okruženje za aplikaciju servisa][IntroToAppServiceEnvironment]

Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
