<properties 
   pageTitle="Određivanje postavki DNS-a u datoteci konfiguracije servisa | Microsoft Azure"
   description="Određivanje prilagođene postavke DNS-a pomoću datoteke konfiguracija servisa za virtualne mreže"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Određivanje postavki DNS-a u datoteci konfiguracije servisa

## <a name="dns-elements"></a>Elementi DNS-a

Konfiguracijska datoteka za servis može sadržavati element DnsServers s popis IPv4 adrese servisa za upravljanje pravima na informacije (DNS-Domain Name System) poslužitelji koji će koristiti usluge. Postavke u konfiguracijskoj datoteci servisa nadjačavaju postavke u konfiguracijskoj datoteci mreže. Dodatne informacije potražite u članku [Azure servisa konfiguracije shema (.cscfg datoteka)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Atribut **naziv** elementa **DnsServer** koristi se samo naziv reference. Ne predstavlja naziv glavnog računala za DNS poslužitelj. Svaki **DnsServer** vrijednost atributa mora biti jedinstvena preko cijele Microsoft Azure pretplate.

## <a name="see-also"></a>Vidi također

[Shema konfiguracije Azure Service (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Shema konfiguracije Azure virtualne mreže](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfiguriranje virtualne mreže pomoću mrežnih konfiguracije datoteka](http://go.microsoft.com/fwlink/?LinkId=248094)

[O postavkama virtualne mreže na portalu za upravljanje](http://go.microsoft.com/fwlink/?LinkId=248092)

