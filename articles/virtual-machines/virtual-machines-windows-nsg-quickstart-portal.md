<properties
   pageTitle="Otvorite priključke za VM pomoću portala za Azure | Microsoft Azure"
   description="Saznajte kako otvoriti priključak / stvaranje krajnje točke za vaše Windows VM korištenja resursa Upravitelj implementacije modela na portalu za Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Otvaranje priključaka da biste na VM u Azure pomoću portala za Azure
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Brzi naredbe
Možete i [izveli taj postupak pomoću komponente PowerShell Azure](virtual-machines-windows-nsg-quickstart-powershell.md).

Prvo, stvorite sigurnosne grupe na mreži. Odaberite grupu resursa na portalu, kliknite **Dodaj**, a zatim pronađite i odaberite "Mreža sigurnosnu grupu":

![Dodavanje sigurnosne grupe mreže](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Unesite naziv svoje mreže sigurnosne grupe, odaberite ili stvorite grupu resursa i odaberite mjesto. Kliknite **Stvori** završetku:

![Stvaranje sigurnosne grupe mreže](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Odaberite novi mrežni sigurnosne grupe. Odaberite "ulaznog sigurnosna pravila", a zatim kliknite gumb **Dodaj** da biste stvorili pravilo:

![Dodavanje ulaznog pravila](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Navedite naziv za novo pravilo. Priključak 80 već unesen prema zadanim postavkama. U ovom plohu je gdje želite promijeniti izvor, protokol i odredište prilikom dodavanja dodatna pravila za svoje mreže sigurnosne grupe. Kliknite **u redu** da biste stvorili pravilo:

![Stvaranje ulazna pravila](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Završnom koraku se želite pridružiti podmreži ili određene mrežno sučelje sigurnosne grupe na mreži. Recimo da sigurnosne grupe mreže pridružiti podmreži. Odaberite "Podmreže", a zatim kliknite **Povezivanje**:

![Sigurnosne grupe mreže pridružiti podmreži](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Odaberite virtualne mreže, a zatim odaberite odgovarajuće podmreže:

![Pridruživanje sigurnosne grupe mreže virtualne mreže](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Sada ste stvorili sigurnosnu grupu mreže, stvorili pravilo ulazne koje omogućuje promet na priključak 80 i povezane s podmreži. Bilo koji VMs povežete s tom podmreže su dostupna priključak 80.


## <a name="more-information-on-network-security-groups"></a>Dodatne informacije o sigurnosnim grupama s mrežom
Brze naredbe ovdje omogućuje brz početak rada s promet slijedi vaše VM. Mrežni sigurnosne grupe sadrže mnoge sjajne značajke i preciznosti za pristupom resurse. Dodatne informacije o [stvaranju sigurnosne grupe mreže i ACL pravila ovdje](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Možete definirati mreže sigurnosne grupe i ACL pravila u sklopu Azure upravljanja resursima predložaka. Pročitajte više o [stvaranju mreže sigurnosne grupe s predlošcima](../virtual-network/virtual-networks-create-nsg-arm-template.md)članku.

Ako je potrebno koristiti priključak prosljeđivanje da biste mapirali jedinstveni vanjskog priključka Interni priključak na vašem VM, koristite raspoređivača opterećenja i pravila Prijevod mrežne adrese (NAT). Na primjer, trebali biste izlaganje TCP priključak 8080 vanjsko i imaju promet usmjereni na TCP priključak 80 na VM. Možete dodatne informacije o [stvaranju opterećenja na mjesto na Internetu](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Daljnji koraci
U ovom primjeru stvara jednostavno pravilo da dopušta promet HTTP-a. Možete pronaći informacije o stvaranju detaljnije okruženja u sljedećim člancima:

- [Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md)
- [Što je mreža grupe sigurnosti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure pregled resursima za učitavanje Balancers](../load-balancer/load-balancer-arm.md)
