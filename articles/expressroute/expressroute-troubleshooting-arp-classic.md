<properties
   pageTitle="Vodič za otklanjanje poteškoća ExpressRoute: početak ARP tablice | Microsoft Azure"
   description="Ova stranica sadrži upute za početak na ARP tablice za je elektronička ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>Vodič za otklanjanje poteškoća ExpressRoute: početak ARP tablice u modelu klasični implementacije

> [AZURE.SELECTOR]
[PowerShell – resursima](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell – klasični](expressroute-troubleshooting-arp-classic.md)

U ovom se članku vodit će vas kroz korake za pribavljanje tablice adresu razlučivost Protocol (ARP) za vaše elektronička Azure ExpressRoute.

>[AZURE.IMPORTANT] Ovaj dokument namijenjen za dijagnosticiranje i otklanjanje problema vezanih uz jednostavne. Nije namijenjen biti zamjena za Microsoftove službe za podršku. Ako se problem ne riješi pomoću sljedećih smjernica, otvorite zahtjev za podršku putem [Pomoć za Microsoft Azure + podrška](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresa razlučivost Protocol (ARP) i ARP tablice
ARP je sloja 2 protokol koji je definiran u [RFC 826](https://tools.ietf.org/html/rfc826). ARP koristi se za mapiranje adresu Ethernet (MAC adresa) IP adresa.

Tablica programa ARP nudi mapiranje IPv4 adresa i MAC adresu za određeni peering. U tablici ARP peering programa ExpressRoute elektronička nudi sljedeće informacije za svaki sučelja (primarnih i sekundarnih):

1. Mapiranje IP adrese lokalnog sučelja usmjerivač adresu za MAC
2. Mapiranje IP adresu ExpressRoute sučelja usmjerivač adresu za MAC
3. Starost mapiranja

Tablica ARP može pomoći s Provjera konfiguracije sloja 2 i otklanjanje problema s povezivanjem osnovni sloja 2.

Slijedi primjer ARP tablice:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Sljedeći odjeljak pruža informacije o pregledavanju ARP tablice koje možete vidjeti rub usmjerivača ExpressRoute.

## <a name="prerequisites-for-using-arp-tables"></a>Preduvjeti za korištenje ARP tablica

Provjerite je li prije nastavka imati sljedeće:

 - Valjani ExpressRoute elektronička koji je konfiguriran za korištenje barem jedan peering. Na elektronička mora biti potpuno konfiguriran davatelj povezivanje. Vi (ili vaš davatelj usluga za povezivanje) morate konfigurirati barem jedno od peerings (Azure privatne, Azure javno ili Microsoft) na ovom elektronička.

 - Rasponi IP adresa koji se koriste za konfiguriranje peerings (Azure privatne, Azure javno i Microsoft). Pregledajte Primjeri dodjele IP adresa [ExpressRoute usmjeravanje preduvjeti stranice](expressroute-routing.md) da biste dobili razumijevanja mapiranje IP adresa za sučelja na vašem aise i na strani ExpressRoute. Informacije o konfiguraciji peering možete dobiti tako da pročitate [ExpressRoute peering konfiguracije stranice](expressroute-howto-routing-classic.md).

 - Informacije davatelja umrežavanje tima ili povezivanje o adresama MAC sučelja koji se koriste sljedeće IP adrese.

 - Najnovije modul Windows PowerShell za Azure (verzija 1.50 ili noviji).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP tablice za vaše elektronička ExpressRoute
Ovo poglavlje sadrži upute o tome kako prikaza ARP tablica za svaku vrstu peering pomoću komponente PowerShell. Prije nego što nastavite, vi ili vaš davatelj usluga za povezivanje mora konfigurirati na peering. Svaki elektronička ima dva puta (primarnih i sekundarnih). Neovisno o možete provjeriti ARP tablice za svaki put.

### <a name="arp-tables-for-azure-private-peering"></a>ARP tablice za Azure privatne peering
Sljedeći cmdlet nudi na ARP tablice za Azure privatne peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Slijedi primjer izlaz za neki:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP tablice za Azure javno peering:
Sljedeći cmdlet nudi na ARP tablice za Azure javno peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Slijedi primjer izlaz za neki:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Slijedi primjer izlaz za neki:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP tablice za Microsoft peering
Sljedeći cmdlet nudi na ARP tablice za Microsoft peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Ogledna izlaz je prikazano u nastavku za neki:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Upute za korištenje ove informacije
Da biste provjerili valjanost konfiguracije sloja 2 i povezivanjem mogu se ARP tablicu s peering. Ovo poglavlje sadrži pregled načina na koji se ARP tablice izgleda u različitim scenarijima.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>ARP tablice kada je elektronička je u stanju radu (očekivani)

 - Tablica ARP ima unos za lokalni strani valjanu adresu IP i MAC i slično unosa za Microsoft strani.
 - Zadnji octet IP adrese lokalnog uvijek je neparan broj.
 - Zadnji octet Microsoft IP adrese uvijek je paran broj.
 - Pojavit će se s istom adresom za MAC na strani Microsoft za sva tri peerings (primarni/sekundarni).


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>ARP tablice kada je na lokalni ili kada na strani povezivanje davatelja ima poteškoća s

 U tablici ARP pojavit će se samo jednu stavku. Prikazuje mapiranja između MAC adresa i IP adresu koja se koristi Microsoft strane.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Ako naiđete na problem ovako, otvorite zahtjev za podršku kod davatelja usluge povezivanja da biste riješili problem.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>ARP tablice kada je Microsoft strana ima poteškoća

 - Nećete vidjeti tablice programa ARP prikazani u peering ako postoje problemi na strani Microsoft.
 -  Otvorite zahtjev za podršku putem [Pomoć za Microsoft Azure + podrška](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Navedite imate problema s povezivanjem sloja 2.

## <a name="next-steps"></a>Daljnji koraci

 - Provjerite valjanost konfiguracije Layer 3 za vaše elektronička ExpressRoute:
     - Pronađite rutu sažetka da biste odredili stanje BGP sesije.
     - Pronađite usmjeravanje tablice da biste utvrdili koji prefiksi su objavljeno preko ExpressRoute.
 - Provjerite valjanost prijenos podataka i smanjivati pregledom bajtova.
 - Ako i dalje imate problema, otvorite zahtjev za podršku putem [Pomoć za Microsoft Azure + podrška](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
