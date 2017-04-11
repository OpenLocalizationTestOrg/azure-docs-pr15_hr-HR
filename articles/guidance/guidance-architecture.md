
<properties
   pageTitle="Upute za Azure | Uzorci i prakse | Microsoft Azure"
   description="Arhitekturi Azure Reference"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Arhitekturi Azure Reference

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Sadržaj je u aktivni razvoj. Korisno je danas, pa ne možemo su čime dostupni za pretpregled. Cijenimo vaše povratne informacije._

Naš arhitekturi referenca raspoređena su scenariju s više povezanih arhitekturi grupirane.
Svaki pojedinačne arhitektura nudi preporučeni Načini rada i prescriptive korake, kao i izvršne komponente koje embodies preporuke.
Mnoge na arhitekturi su progresivno; stvara se pri vrhu prethodni arhitekturi koji imaju manje preduvjeti.

## <a name="designing-your-infrastructure-for-resiliency"></a>Dizajniranje preduvjete infrastrukture za otpornost

Ovaj niz počinje preporučene postupke za optimalne VM konfiguracije i culminates implementacije regije s pomoćnim.

- [Sa sustavom Windows VM na Azure](guidance-compute-single-vm.md)
- [Sustavom Linux VM Azure](guidance-compute-single-vm-linux.md)
- [Pokretanje više VMs skalabilnost i dostupnosti](guidance-compute-multi-vm.md)
- [Izvodi Windows VMs za Arhitektura programa N sloja](guidance-compute-n-tier-vm.md)
- [Pokrenut Linux VMs Arhitektura programa N sloja](guidance-compute-n-tier-vm-linux.md)
- [Izvodi Windows VMs u više područja za visoke dostupnosti](guidance-compute-multiple-datacenters.md)
- [Pokretanje Linux VMs u više područja za visoke dostupnosti](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Povezivanje lokalne mreže Azure

Ovaj niz pokreće po takvi načini povezivanja postojeće mreže Azure. Zatim ga proširuje da prekrije preduvjeti za dostupnost i sigurnost.

- [Implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN-a](guidance-hybrid-network-vpn.md)
- [Implementacijom arhitekturu hibridnog mreže s Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
- [Implementacijom arhitekturu iznimno dostupna hibridnog mreže](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Zaštita hibridnog mreže

Ovaj niz pokriva dokazana Savjeti o stvaranju DMZ u Azure sigurnu vezu koji dolaze iz vaše lokalne podatkovnom centru, a na Internetu.

- [Implementacijom DMZ između Azure i vaše lokalne podatkovnim centrom](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Implementacijom DMZ između Azure i Interneta](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Pružanja usluge za identitet

Ovaj niz pokreće po takvi kako pomoću servisa Azure Active Directory omogućuje provjeru autentičnosti korisnika u Azure. Zatim se proširuje da prekrije složene scenariji proširivanje infrastruktura za ZBRAJA za Azure i pomoću ADFS za delegiranje.

- [Implementacijom Azure Active Directory](./guidance-identity-aad.md)
- [Proširivanje usluge Active Directory (ZBRAJA) za Azure](./guidance-identity-adds-extend-domain.md)
- [Stvaranje skupa stabala resursa u Active Directory Directory Services (ZBRAJA) u Azure](./guidance-identity-adds-resource-forest.md)
- [Implementacijom Active Directory Federation Services (ADFS) u Azure](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Architecting skalabilni web-aplikacije pomoću Azure PaaS

Ovaj niz pokriva preporuke za izgradnje skalabilni i vrlo dostupnih web-aplikacije. 

- [Osnovni web-aplikacije](guidance-web-apps-basic.md)
- [Poboljšanje skalabilnost u web-aplikaciji](guidance-web-apps-scalability.md)
- [Web-aplikacije s visoke dostupnosti](guidance-web-apps-multi-region.md)
