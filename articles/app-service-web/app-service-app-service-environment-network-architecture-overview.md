<properties 
    pageTitle="Pregled arhitektura mreže okruženja aplikacije servisa" 
    description="Pregled arhitekture ofApp topologije mrežni servis okruženja." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Pregled arhitektura mreže okruženja aplikacije servisa

## <a name="introduction"></a>Uvod ##
Aplikacije servisa okruženja uvijek se stvaraju unutar podmreži [virtualne mreže] [ virtualnetwork] – aplikacije u okruženju aplikacije servisa mogli komunicirati sa privatne krajnje točke koja se nalazi unutar iste topologije virtualne mreže.  Budući da korisnici mogu zaključati dijelove njihove Infrastruktura virtualne mreže, važno je da biste shvatili vrste mreže komunikacije tokova koji se pojavljuju s okruženjem aplikacije servisa.

## <a name="general-network-flow"></a>Tijek Općenito mreže ##
 
Kada se aplikacija servisa okruženje (elika i mala slova) koristi javna virtualne IP adresa (VIP) za aplikacije, sav dolazni promet stigne na tom javno VIP.  To obuhvaća HTTP i HTTPS promet za aplikacije, kao i ostali promet za FTP, udaljene pogrešaka funkcije i operacija Azure upravljanja.  Potpuni popis određene priključke (i obaveznih i neobaveznih) koje su dostupne na javno VIP potražite u članku na [kontroliranje dolazni promet] [ controllinginboundtraffic] u okruženju aplikacije servisa. 

Okruženja aplikacije servisa za podršku i pokrenuti aplikacije koje su vezane samo na virtualne mreže interne adrese, koja se naziva i adrese ILB (interna opterećenja).  Na programa ILB omogućeno elika i mala slova, HTTP i HTTPS promet za aplikacije, kao i daljinski pozivi pogrešaka stižu na adresi ILB.  Najčešći konfiguracijama elika i mala ILB-slova, promet FTP/FTPS stizati i na adresi ILB.  No operacija Azure upravljanja će i dalje tijeka s priključcima 454/455 javno VIP od programa ILB omogućen elika i mala slova.

Dijagramu u nastavku prikazuje pregled različitih tokova ulazni i izlazni mreže za okruženju servisa aplikacija koje su povezane aplikacije javnu virtualne IP adresu:

![Općenito mreže tokova][GeneralNetworkFlows]

Okruženja aplikacije servisa mogu komunicirati s raznim krajnje točke za privatne korisnike.  Ako, na primjer, aplikacije koji se izvodi u okruženje za aplikaciju servisa možete povezati s poslužiteljima baza podataka radi na virtualnim računalima sustava IaaS u istom virtualne mrežna topologija.

>[AZURE.IMPORTANT] Pogled na mrežni dijagram, "Ostali izračunati resurse" su uveden u drugoj podmreži iz servisa okruženja aplikacije. Implementacija resursa u istoj podmreži s elika i u mala slova blokira povezivanja s elika i mala slova te resursima (osim usmjeravanje određene intra-elika i mala slova). Implementacija drugoj podmreži umjesto (u istoj VNET). Okruženje za aplikaciju servisa pa će se moći povezati. Potrebno je bez dodatna konfiguracija.

Okruženja aplikacije servisa za komunicirati s Sql DB i Azure prostora za pohranu resurse potrebne za Upravljanje projektom i okolini programa aplikacije servisa.  Neke od Sql i pohranu resursa okruženju aplikacije servisa komunicira s nalaze se na istom području kao okruženje za aplikaciju servisa dok drugi nalaze se u udaljene Azure područja.  Kao rezultat izlaznog povezani s Internetom uvijek potreban je za okruženje aplikacije servisa za pravilno funkcionirati. 

Budući da okruženju servisa aplikacija je uveden u podmreži, mreže sigurnosne grupe može koristiti da biste odredili dolazni promet na podmreži.  Pojedinosti o upravljaju dolazni promet na servis okruženju aplikacije potražite u sljedećem [članku][controllinginboundtraffic].

Pojedinosti o kako omogućiti izlaznog internetska veza s okruženjem servisa aplikacija potražite u sljedećem članku o radu s [Express usmjeravanje][ExpressRoute].  Odnosi se na isti način opisan u članku prilikom rada s povezivanjem na web-mjesto, a pomoću prisilno tuneliranje.

## <a name="outbound-network-addresses"></a>Izlazni mrežne adrese ##
Kada servis okruženju aplikacije izvrši odlazni pozivi, IP adresu uvijek povezan je s odlazni pozivi.  Određeni IP adresa koji se koristi ovisi o tome hoće li se nalazi unutar topologije virtualne mreže ili izvan topologije virtualne mreže krajnju točku koja se poziva.

Ako je krajnju točku koja se poziva **izvan** topologije virtualne mreže, izlaznog adresa (ili izlazni NAT adresa) koja se koristi je javno VIP okruženja za aplikaciju servisa.  Tu adresu pronaći ćete u portala korisničkom sučelju okruženju servisa aplikacija u plohu svojstva.
 
![Izlaznih IP adresa][OutboundIPAddress]

Tu adresu mogu se odrediti za ASEs koji imaju samo javne VIP stvaranjem aplikacije u okruženju servisa aplikacija i zatim izvođenje *nslookup* na adresi aplikacije. Konačni IP adresa je i javne VIP, kao i okruženje servisa aplikacija izlazni NAT adresu.

Ako je krajnju točku koja se poziva **unutar** topologije virtualne mreže, izlaznog adresu pokrenuti aplikaciju bit će interne IP adresa resursa pojedinačne računalnim pokrenuti aplikaciju.  No nema mapiranja stalni virtualne mreže interne IP adrese za aplikacije.  Aplikacije možete se pomicati kroz resurse za različite računalnim i skup dostupnih računalnim resursa u okruženju servisa aplikacija možete promijeniti zbog skaliranje operacije.

Međutim, budući da je servis okruženju aplikacije uvijek nalazi unutar podmreži, su zajamčiti da interne IP adresa resursa računalnim pokrenuti aplikaciju uvijek će biti unutar raspona CIDR podmreži.  Zbog toga kada preciznije ACL-a ili mreže sigurnosne grupe koriste se za siguran pristup za drugi krajnje točke unutar virtualne mreže, podmreže raspon koji sadrži okruženje za aplikaciju servisa mora odobriti pristup.

Sljedeći dijagram prikazuje koncepte detaljno:

![Izlazni mrežne adrese][OutboundNetworkAddresses]

U gornjem dijagram:

- Budući da javne VIP okruženja za aplikaciju servisa je 192.23.1.2, to je izlaznih IP adresa koristi pri upućivanju poziva za krajnje točke "Internet".
- CIDR raspon koji sadrže podmreže okruženju servisa aplikacija je 10.0.1.0/26.  Ostale krajnje točke unutar iste Infrastruktura virtualne mreže prikazat će se pozive iz aplikacija kao potječu negdje u dosegu adresu.

## <a name="calls-between-app-service-environments"></a>Uspostavljanje poziva između aplikacije servisa za okruženja ##
Složenije scenarij može dogoditi ako implementacija više okruženja aplikacije servisa u istom virtualne mreže i odlazne pozive iz jedne aplikacije servisa okruženja druge aplikacije servisa okruženje.  Ove vrste unakrsne aplikacije servisa okruženje pozive također se neće smatrati "Internetske" pozive.

Na sljedećem su dijagramu prikazani Primjeri Slojeviti arhitektura s aplikacijama u jednu aplikaciju servisa okruženju (npr. "Prednju odaberite" web apps) pozivanje aplikacije u drugom okruženju aplikacije servisa (npr. Interna pozadinske API aplikacija nije namijenjen bude dostupno putem Interneta). 

![Uspostavljanje poziva između aplikacije servisa za okruženja][CallsBetweenAppServiceEnvironments] 

U primjeru iznad okruženje za servis aplikacije "Elika i mala slova jedan" ima izlaznih IP adresu 192.23.1.2.  Ako aplikaciju na to aplikacije servisa okruženje čini Izlazni poziv aplikacije na drugu aplikaciju servisa okruženju ("elika i mala slova dva") koja se nalazi u istom virtualne mreže, Izlazni poziv će se smatrati poziv "Internet".  Kao rezultat mrežni promet stiže na drugu aplikaciju servisa okruženje će prikazati kao potječu 192.23.1.2 (odnosno ne podmreže adresu raspon prvi servis okruženje za aplikaciju).

Iako uspostavljanje poziva između različitim okruženjima aplikacije servisa smatraju "Internetske" pozive, kad oba okruženja aplikacije servisa nalaze u istom području Azure mrežni promet će ostati u regionalnim Azure mreži i ne će se kreću fizički putem javnog Interneta.  Kao rezultat možete koristiti sigurnosne grupe mreže na podmreži drugom okruženju servisa aplikacije da biste omogućili samo dolazni pozivi iz prvog aplikacije servisa okruženje (čiji izlaznih IP adresa je 192.23.1.2), stoga osiguravanje sigurnu komunikaciju između servisa okruženja aplikacije.

## <a name="additional-links-and-information"></a>Dodatne veze i informacije ##
Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Detalje o dolazni priključke koristi okruženja aplikacije servisa i putem mreže sigurnosne grupe nadzire dolazni promet dostupan [ovdje][controllinginboundtraffic].

Informacije o korištenju korisnički definirana usmjerava da biste dodijelili izlaznog pristup Internetu okruženja aplikacije servisa je dostupan u ovom [se članku][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

