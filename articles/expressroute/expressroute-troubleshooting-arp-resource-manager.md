<properties 
   pageTitle="Vodič za otklanjanje poteškoća ExpressRoute - početak ARP tablice | Microsoft Azure"
   description="Ova stranica sadrži upute za postizanje na ARP tablice za je elektronička ExpressRoute"
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

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>ExpressRoute za otklanjanje poteškoća vodič – početak ARP tablice u modelu implementacije Voditelj resursa

> [AZURE.SELECTOR]
[PowerShell – resursima](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell – klasični](expressroute-troubleshooting-arp-classic.md)

U ovom se članku vodit će vas kroz korake da biste saznali ARP tablice za vaše elektronička ExpressRoute. 

>[AZURE.IMPORTANT] Ovaj dokument namijenjen za dijagnosticiranje i otklanjanje problema vezanih uz jednostavne. Nije namijenjen biti zamjena za Microsoftove službe za podršku. Ako ne uspijete riješiti problem pomoću smjernice opisanim, morate otvoriti zahtjev za podršku možete s [Microsoftovoj podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresa razlučivost Protocol (ARP) i ARP tablice
Adresa razlučivost Protocol (ARP) je definirano u [RFC 826](https://tools.ietf.org/html/rfc826)protokol sloja 2. ARP koristi se za mapiranje adresu Ethernet (MAC adresa) s ip adresom.

U tablici ARP navedeni mapiranje ipv4 adresa i MAC adresu za određeni peering. U tablici ARP peering programa ExpressRoute elektronička nudi sljedeće informacije za svaki sučelja (primarnih i sekundarnih)

1. Mapiranje lokalnog usmjerivača sučelja ip adresa za MAC adresa
2. Mapiranje ExpressRoute usmjerivač sučelja ip adresa za MAC adresa
3. Starost mapiranja

ARP tablice omogućuju provjerili valjanost konfiguracije sloja 2 i otklanjanje poteškoća osnovni slojeva 2 problema s povezivanjem. 

Primjer ARP tablice: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Sljedeći odjeljak opisi prikaza tablice ARP vidjeti rub usmjerivača ExpressRoute. 

## <a name="prerequisites-for-learning-arp-tables"></a>Preduvjeti za učenje ARP tablice

Provjerite možete li se sljedeće prije dodatno tijek

 - Valjani ExpressRoute elektronička koji se s najmanje jedan peering. Na elektronička mora biti potpuno konfiguriran davatelj povezivanje. Vi (ili vaš davatelj usluga za povezivanje) mora konfigurirati barem jedno od peerings (Azure privatne, Azure javan i Microsoft) na ovom elektronička.
 - Rasponi IP adresa koristiti za konfiguriranje peerings (Azure privatne, Azure javan i Microsoft). Pregledajte Primjeri dodjele ip adresa [ExpressRoute usmjeravanje preduvjeti stranice](expressroute-routing.md) da biste dobili razumijevanja mapiranje ip adresa za sučelja s vaše strane i na strani ExpressRoute. Informacije o konfiguraciji peering možete dobiti tako da pročitate [ExpressRoute peering konfiguracije stranice](expressroute-howto-routing-arm.md).
 - Podatke s timom za umrežavanje / davatelja usluge povezivanja na MAC adrese sučelja koristi s ove IP adrese.
 - Mora imati najnoviju modul ljuske PowerShell za Azure (verzija 1.50 ili novija verzija).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Uvod u ARP tablice za vaše elektronička ExpressRoute
U ovom se odjeljku daju upute na prikaza tablica ARP po peering pomoću komponente PowerShell. Vi ili vaš davatelj usluga za povezivanje mora konfigurirati peering prije razvijaju Dodatno. Svaki elektronička ima dva puta (primarnih i sekundarnih). Neovisno o možete provjeriti ARP tablice za svaki put.

### <a name="arp-tables-for-azure-private-peering"></a>ARP tablice za Azure privatne peering
Sljedeći cmdlet nudi na ARP tablice za Azure privatne peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Ogledna izlaz je prikazano u nastavku za neki

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP tablice za Azure javno peering
Sljedeći cmdlet nudi na ARP tablice za Azure javno peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Ogledna izlaz je prikazano u nastavku za neki

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP tablice za Microsoft peering
Sljedeći cmdlet nudi na ARP tablice za Microsoft peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Ogledna izlaz je prikazano u nastavku za neki

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Upute za korištenje ove informacije
Na peering ARP tablice koje će se koristiti za utvrđivanje provjerili valjanost konfiguracije sloja 2 i povezivanjem. Ovo poglavlje sadrži pregled kako će izgledati ARP tablice pod različitim scenarijima.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP tablice kada je elektronička je u radno stanje (očekivani stanja)

 - Tablica ARP će imati unos za lokalni strani valjana IP adresa i MAC adresa i slično unosa za Microsoft strani. 
 - Zadnji octet ip adrese lokalnog uvijek će iznositi neparan broj.
 - Zadnji octet ip adrese Microsoft uvijek će biti paran broj.
 - Istu adresu MAC pojavit će se na strani Microsoft za sve 3 peerings (primarni / sekundarni). 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP tablice kada lokalni / povezivanje davatelja strana ima poteškoća

 - U tablici ARP pojavit će se samo jednu stavku. Prikazat će mapiranja između MAC adresa i IP adresa koji se koristi sa strane Microsoft. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Kod davatelja usluge povezivanja za ispravljanje pogrešaka takve probleme, otvorite zahtjev za podršku. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP tablice kada je Microsoft strana ima poteškoća

 - Nećete vidjeti tablice programa ARP prikazani u peering ako postoje problemi na strani Microsoft. 
 -  Otvorite zahtjev za podršku možete s [Microsoftovoj podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Navedite imate problema s povezivanjem sloja 2. 

## <a name="next-steps"></a>Daljnji koraci

 - Provjerite valjanost konfiguracije Layer 3 za vaše elektronička ExpressRoute
     - Početak usmjeravanje sažetka da biste odredili stanje sesije BGP 
     - Početak usmjeravanje tablice možete utvrditi koji prefiksi su objavljeno preko ExpressRoute
 - Provjerite valjanost prijenos podataka tako da pročitate bajtova / izgleda
 - Ako i dalje imate problema, otvorite zahtjev za podršku možete s [Microsoftovoj podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
