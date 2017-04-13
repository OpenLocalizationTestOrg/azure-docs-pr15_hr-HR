<properties 
   pageTitle="Određivanje postavki DNS-a u datoteci konfiguracije virtualne mreže | Microsoft Azure"
   description="Kako promijeniti postavke DNS poslužitelja u virtualne mreže pomoću datoteke Konfiguracija virtualne mreže u modelu klasični implementacije"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Određivanje postavki DNS-a u datoteci konfiguracije virtualne mreže

Konfiguracijska datoteka za mrežni ima dvije elemente koje možete koristiti da biste odredili postavke za upravljanje pravima na informacije (DNS-Domain Name System): **DnsServers** i **DnsServerRef**. Možete dodati popis DNS poslužitelji navođenjem njihove IP adrese i nazivi referenca na **DnsServers** element. **DnsServerRef** element možete koristiti da biste odredili koji DNS poslužitelja stavke iz DnsServers element koriste se za različite mrežne web-mjesta unutar virtualne mreže.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije.

Konfiguracijska datoteka mreži može sadržavati sljedeće elemente. Naslov svaki element je povezana s stranice koje pruža dodatne informacije o postavkama vrijednost elementa.

>[AZURE.IMPORTANT] Informacije o konfiguriranju mreže konfiguracijska datoteka potražite u članku [Konfiguriranje virtualne mreže pomoću mreže konfiguracijska datoteka](virtual-networks-using-network-configuration-file.md). Informacije o svaki element koji se nalaze u konfiguracijskoj datoteci mreže potražite u članku [Azure virtualne mreže konfiguracije shemu](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[Element DNS-a](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] Atribut **naziv** elementa **DnsServer** služi samo kao referenca **DnsServerRef** element. Ne predstavlja naziv glavnog računala za DNS poslužitelj. Svaki **DnsServer** vrijednost atributa mora biti jedinstvena preko cijele pretplate Microsoft Azure

[Element virtualne mreže web-mjesta](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Da biste odredili postavku za element virtualne mreže web-mjesta, on mora biti prethodno definirano u elementu DNS. DnsServerRef *naziv* elementa virtualne mreže web-mjesta mora se odnositi na naziv vrijednost koja je navedena u elementu DNS-a za DnsServer *naziv*.

## <a name="next-steps"></a>Daljnji koraci

- Razumijevanje [sheme konfiguracije Azure virtualne mreže](http://go.microsoft.com/fwlink/?LinkId=248093).
- Razumijevanje [Azure Service konfiguracije shemu](https://msdn.microsoft.com/library/windowsazure/ee758710).
- [Konfiguracija virtualne mreže pomoću mrežnih datoteka konfiguracije](virtual-networks-using-network-configuration-file.md).
