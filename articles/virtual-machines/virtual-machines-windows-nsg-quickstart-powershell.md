<properties
   pageTitle="Otvorite priključke za VM pomoću komponente PowerShell | Microsoft Azure"
   description="Saznajte kako otvoriti priključak / stvaranje krajnje točke za vaše VM Windows Azure resursa Upravitelj implementacije način i Azure PowerShell"
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

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Otvaranje priključci i krajnje točke da biste na VM u Azure pomoću komponente PowerShell
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Brzi naredbe
Da biste stvorili mrežni sigurnosne grupe i ACL pravila morate [najnoviju verziju programa Azure PowerShell instaliran](../powershell-install-configure.md). Možete i [izveli taj postupak pomoću portala za Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Prijavite se na račun za Azure:

```powershell
Login-AzureRmAccount
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjer dio `myResourceGroup`, `myNetworkSecurityGroup`, i `myVnet`.

Stvorite pravilo. Sljedeći primjer stvara pravilo pod nazivom `myNetworkSecurityGroupRule` da dopušta promet TCP na priključak 80:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Nakon toga stvorite na mreži sigurnosne grupe i dodijelite pravilo HTTP-a koji ste upravo stvorili na sljedeći način. Sljedeći primjer stvara sigurnosnu grupu mreže pod nazivom `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Sada ćemo podmreži dodijeliti sigurnosne grupe na mreži. U sljedećem primjeru dodjeljuje postojeće virtualne mreže pod nazivom `myVnet` varijabli `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Sigurnosne grupe na mreži pridružiti vašoj podmreži. U sljedećem primjeru povezuje podmreži pod nazivom `mySubnet` s grupom sigurnost mreže:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Na kraju, ažurirajte virtualne mreže da bi promjene stupile na snagu:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Dodatne informacije o sigurnosnim grupama s mrežom
Brze naredbe ovdje omogućuje brz početak rada s promet slijedi vaše VM. Mrežni sigurnosne grupe sadrže mnoge sjajne značajke i preciznosti za pristupom resurse. Dodatne informacije o [stvaranju sigurnosne grupe mreže i ACL pravila ovdje](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Možete definirati mreže sigurnosne grupe i ACL pravila u sklopu Azure upravljanja resursima predložaka. Pročitajte više o [stvaranju mreže sigurnosne grupe s predlošcima](../virtual-network/virtual-networks-create-nsg-arm-template.md)članku.

Ako je potrebno koristiti priključak prosljeđivanje da biste mapirali jedinstveni vanjskog priključka Interni priključak na vašem VM, koristite raspoređivača opterećenja i pravila Prijevod mrežne adrese (NAT). Na primjer, trebali biste izlaganje TCP priključak 8080 vanjsko i imaju promet usmjereni na TCP priključak 80 na VM. Možete dodatne informacije o [stvaranju opterećenja na mjesto na Internetu](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Daljnji koraci
U ovom primjeru stvara jednostavno pravilo da dopušta promet HTTP-a. Možete pronaći informacije o stvaranju detaljnije okruženja u sljedećim člancima:

- [Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md)
- [Što je mreža grupe sigurnosti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure pregled resursima za učitavanje Balancers](../load-balancer/load-balancer-arm.md)