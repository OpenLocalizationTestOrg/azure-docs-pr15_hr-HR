<properties
   pageTitle="Otvorite priključci i krajnje točke za Linux VM | Microsoft Azure"
   description="Saznajte kako otvoriti priključak / stvaranje krajnje točke za vaše VM Linux pomoću model za uvođenje Upravitelj Azure resursa i EŽA Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Otvaranje priključci i krajnje točke za Linux VM u Azure
Otvorite priključak ili stvaranje krajnje virtualne stroj (VM) u Azure stvaranjem filtra mreže na podmreži ili VM mrežno sučelje. Ti filtri koje kontroliraju ulazni i izlazni promet, postavite na mreži sigurnosne grupe priložiti resurs koji prima promet. Pogledajmo pomoću uobičajenih primjera web promet na priključak 80.

## <a name="quick-commands"></a>Brzi naredbe
Da biste stvorili sigurnosnu grupu mreže i pravila koja vam je potrebna [EŽA Azure](../xplat-cli-install.md) te koristite način Voditelj resursa:

```bash
azure config mode arm
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjer dio `myResourceGroup`, `myNetworkSecurityGroup`, i `myVnet`.

Stvaranje grupe mrežom sigurnost, komponenta unesete vlastite nazive i mjesto. Sljedeći primjer stvara sigurnosnu grupu mrežom pod nazivom `myNetworkSecurityGroup` u na `WestUS` mjesto:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Dodavanje pravila da biste dopustili HTTP promet na vaše webserver (ili Prilagodba vlastite scenariju, kao što su SSH povezivanja programa access ili bazu podataka). Sljedeći primjer stvara pravilo pod nazivom `myNetworkSecurityGroupRule` da dopušta promet TCP na priključak 80:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Sigurnosna grupa mreže pridružiti vaše VM mrežnog sučelja (NIC). U sljedećem primjeru povezuje postojeće NIC koji se naziva `myNic` s mreže sigurnosne grupe s nazivom `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Osim toga, možete povezati svoje mreže sigurnosne grupe s podmreži virtualne mreže, a ne samo na mrežnom sučelju na jednom VM. U sljedećem primjeru povezuje postojeće podmreže koji se naziva `mySubnet` u na `myVnet` virtualne mreže s mreže sigurnosne grupe s nazivom `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Dodatne informacije o sigurnosnim grupama s mrežom
Brze naredbe ovdje omogućuje brz početak rada s promet slijedi vaše VM. Mrežni sigurnosne grupe sadrže mnoge sjajne značajke i preciznosti za pristupom resurse. Dodatne informacije o [stvaranju sigurnosne grupe mreže i ACL pravila ovdje](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Možete definirati mreže sigurnosne grupe i ACL pravila u sklopu Azure upravljanja resursima predložaka. Pročitajte više o [stvaranju mreže sigurnosne grupe s predlošcima](../virtual-network/virtual-networks-create-nsg-arm-template.md)članku.

Ako je potrebno koristiti priključak prosljeđivanje da biste mapirali jedinstveni vanjskog priključka Interni priključak na vašem VM, koristite raspoređivača opterećenja i pravila Prijevod mrežne adrese (NAT). Na primjer, trebali biste izlaganje TCP priključak 8080 vanjsko i imaju promet usmjereni na TCP priključak 80 na VM. Možete dodatne informacije o [stvaranju opterećenja na mjesto na Internetu](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Daljnji koraci
U ovom primjeru stvara jednostavno pravilo da dopušta promet HTTP-a. Možete pronaći informacije o stvaranju detaljnije okruženja u sljedećim člancima:

- [Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md)
- [Što je mreža grupe sigurnosti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure pregled resursima za učitavanje Balancers](../load-balancer2    /load-balancer-arm.md)