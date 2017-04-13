<properties
   pageTitle="Stvaranje Linux VM s više NIC-ovi | Microsoft Azure"
   description="Saznajte kako stvoriti Linux VM s više NIC-ovi povezan s predlošcima Azure EŽA ili upravitelj resursa."
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
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Stvaranje Linux VM s više NIC-ovi
Možete stvoriti virtualnog računala (VM) u Azure koja ima više virtualne mreže sučelja (NIC-ovi) na pridružen. Uobičajeni scenarij bi da bi drugi podmreže za povezivanje sučelja i stražnje ili mreže za nadzor ili sigurnosne kopije rješenje. Ovaj članak sadrži brze naredbe za stvaranje na VM s više NIC-ovi pridružen. Detaljne informacije, uključujući kako stvoriti više NIC-ovi unutar vlastitih skripti tulumu potražite dodatne informacije o [implementaciji VMs više SLIKA](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Različitim [veličinama VM](virtual-machines-linux-sizes.md) podržavaju različit broj NIC-ovi, pa veličina vaše VM sukladno tome.

>[AZURE.WARNING] NIC-ovi više morate pridružiti kada stvarate na VM – ne možete dodati NIC-ovi postojeće VM. Možete [stvoriti VM temelju izvorne virtualne diskove](virtual-machines-linux-copy-vm.md) i stvaranje više NIC-ovi, kao što je implementirati u VM.

## <a name="quick-commands"></a>Brzi naredbe
Provjerite jeste li povezani s [Azure EŽA](../xplat-cli-install.md) prijavljen i koristite način upravljanja resursima:

```bash
azure config mode arm
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjer dio `myResourceGroup`, `mystorageaccount`, i `myVM`.

Prvo, stvorite grupu resursa. Sljedeći primjer stvara grupu resursa pod nazivom `myResourceGroup` u na `WestUS` mjesto:

```bash
azure group create myResourceGroup -l WestUS
```

Stvorite račun za pohranu za vaše VMs. Sljedeći primjer stvara za pohranu račun pod nazivom `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Stvaranje virtualne mreže da biste se povezali vaše VMs za. Sljedeći primjer stvara virtualne mreže pod nazivom `myVnet` s prefiksom na adresu `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Stvaranje dva podmreže virtualne mreže – jedan za sučelja promet i jedan za pozadinsku promet. Sljedeći primjer stvara dva podmreže pod nazivom `mySubnetFrontEnd` i `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Stvorite dva NIC-ovi, jedan NIC u sučelja podmreži i jedan NIC priložite podmreži pozadinsku. Sljedeći primjer stvara dva NIC-ovi, pod nazivom `myNic1` i `myNic2`, a pridružuje vaše podmreže:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Na kraju, stvorite VM, pridruživanje na dva NIC-ovi koji ste prethodno stvorili. Sljedeći primjer stvara VM pod nazivom `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Stvaranje više NIC-ovi pomoću EŽA Azure
Ako ste prethodno stvorili VM pomoću EŽA Azure, morate biti upoznati brze naredbe. Postupak je jednako da biste stvorili NIC jedan ili više NIC-ovi. Saznajte više pojedinosti o [implementaciji više NIC-ovi pomoću EŽA Azure](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), uključujući skriptiranje postupak ponavljanje kroz stvaranje sve mrežne kartice.

Sljedeći primjer stvara dva NIC-ovi, pod nazivom `myNic1` i `myNic2`, s jednog NIC povezati svaki podmreže:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Obično se mogu stvoriti na [Mreži sigurnosne grupe](../virtual-network/virtual-networks-nsg.md) ili [Učitavanje opterećenja](../load-balancer/load-balancer-overview.md) olakšavaju upravljanje i raspodijelite promet na vaše VMs. Ponovno naredbe isti su prilikom rada s više NIC-ovi. Sljedeći primjer stvara sigurnosnu grupu mreže pod nazivom `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Povezati pomoću mreže sigurnosne grupe sustava NIC-ovi `azure network nic set`. U sljedećem primjeru povezuje `myNic1` i `myNic2` s `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Prilikom stvaranja na VM sada navesti više NIC-ovi. Umjesto toga pomoću `--nic-name` možete unijeti jednu NIC, umjesto toga koristite `--nic-names` i navedite popis NIC-ovi odvojenih zarezom. Morate izvršiti kad odaberete veličina VM. Nema ograničenja za ukupan broj NIC-ovi koje možete dodati na VM. Dodatne informacije o [veličinama Linux VM](virtual-machines-linux-sizes.md). Sljedeći primjer pokazuje kako odrediti više NIC-ovi, a zatim veličina VM koji podržava više NIC-ovi (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Stvaranje više NIC-ovi s predlošcima Voditelj resursa
Azure resursima predložaka koristite deklarativno JSON datoteke da biste definirali vaše okruženje. [Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md)možete čitati. Voditelj resursa predložaka omogućuje stvaranje više instanci resursa tijekom uvođenja, kao što su stvaranje više NIC-ovi. Koristite *kopiju* da biste odredili koliko je instanci da biste stvorili:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Saznajte više o [stvaranju više instanci *kopiji*](../resource-group-create-multiple.md). 

Možete koristiti na `copyIndex()` da biste dodali broj naziv resursa koji omogućuje stvaranje `myNic1`, `myNic2`, itd. Na sljedećoj je slici prikazan primjer dodavanje vrijednost indeksa:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Dovršavanje primjera [Stvaranje više NIC-ovi s predlošcima resursima](../virtual-network/virtual-network-deploy-multinic-arm-template.md)može čitati.

## <a name="next-steps"></a>Daljnji koraci
Provjerite je li za pregled [veličine Linux VM](virtual-machines-linux-sizes.md) prilikom stvaranja na VM s više NIC-ovi. Obratite pozornost na najveći broj NIC-ovi svaki veličina VM podržava. 

Imajte na umu da ne možete dodati dodatne NIC-ovi postojeće VM prilikom implementacije sustava VM morate stvoriti sve mrežne kartice. Pobrinuti se prilikom planiranja vaše implementacije da biste bili sigurni da imate sve potrebne mrežne veze iz na outset.