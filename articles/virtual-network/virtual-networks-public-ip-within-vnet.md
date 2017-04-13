<properties 
   pageTitle="Kako koristiti javnu IP adresa u virtualne mreže"
   description="Saznajte kako konfigurirati virtualne mreže da biste koristili javnu IP adresa"
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
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Javnu IP adresa prostor u virtualne mreže (VNet)

Virtualne mreže (VNets) mogu sadržavati javne i privatne (RFC 1918 adresni blokovi) IP razmake adresu. Kada dodate javno rasponu IP adresa, će se tretirati kao dio prostora privatne VNet IP adresu koja je dostupna u VNet, međusobno povezanih VNets i iz lokalne lokacije.

Na slici u nastavku prikazuje VNet koji uključuje javne i privatne razmake IP adresa.

![Konceptualni javnu IP](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Kako dodati javno rasponu IP adresa?

Dodavanje javno rasponu IP adresa na isti način kao i dodajte privatne rasponu IP adresa; pomoću bilo *netcfg* datoteke ili dodavanjem konfiguraciju [Azure portal](http://portal.azure.com). Javni rasponu IP adresa možete dodati kada stvarate vaše VNet ili vratite i dodajte ga kasnije. U sljedećem primjeru pokazuje javne i privatne IP adresa razmake konfiguriran u istoj VNet.

![Javnu IP adresu na portalu](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Postoje li ograničenja?

Postoji nekoliko rasponi IP adresa koji nisu dopušteni:

- 224.0.0.0/4 (višesmjernog)

- 255.255.255.255/32 (emitiranje)

- 127.0.0.0/8 (povratna)

- 169.254.0.0/16 (link-local)

- 168.63.129.16/32 (interne DNS-a)

## <a name="next-steps"></a>Daljnji koraci

[Kako upravljati DNS poslužitelja koji se koriste u VNet](virtual-networks-manage-dns-in-vnet.md)