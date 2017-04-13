<properties
   pageTitle="Stvaranje Linux VM na Azure pomoću na EŽA | Microsoft Azure"
   description="Stvaranje Linux VM na Azure pomoću na EŽA."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Stvaranje Linux VM na Azure pomoću na EŽA

U ovom se članku prikazuje kako brzo uvesti Linux virtualnog računala (VM) na Azure pomoću na `azure vm quick-create` naredbe u Azure sučelja naredbenog retka (EŽA). Na `quick-create` naredba uvodi VM unutar osnovne, sigurne infrastrukture koristiti za predložak ili hitro testiranje na pojam. U članku zahtijeva:

- Azure račun ([dobiti besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure EŽA](../xplat-cli-install.md) prijavili `azure login`.

- način za Azure Voditelj resursa Azure EŽA _mora biti u_ `azure config mode arm`.

Brzo možete implementirati Linux VM pomoću [portala za Azure](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-commands"></a>Brzi naredbe

Sljedeći primjer prikazuje način implementacije CoreOS VM i priložite ključ sigurne ljuske (SSH) (svoje argumente mogu se razlikovati):

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Detaljni vodič

Sljedeći vodič sadrži programa VM UbuntuLTS se distribuiranih, korak po korak, s objašnjenja što je način svakog koraka.

## <a name="vm-quick-create-aliases"></a>VM brzo stvaranje pseudonima

Brz način da biste odabrali raspodjele je za korištenje pseudonima Azure EŽA mapirati Najčešći distribucija OS. U sljedećoj su tablici navedeni pseudonima (na Azure EŽA verziju 0.10). Sve implementacije koje koriste `quick-create` zadane postavke i VMs koje se sigurnosno po solid-state pogon (SSD) prostor za pohranu, koji nudi pristup disku brže dodjele resursa i visokih performansi. (Te aliase predstavlja maleni dio dostupna Distribucija na Azure. Pronađite još slika u trgovine Windows Azure [Traženje sliku u ljusci PowerShell](virtual-machines-linux-cli-ps-findimage.md), [na webu](https://azure.microsoft.com/marketplace/virtual-machines/)ili [Prijenos vlastitu prilagođenu sliku](virtual-machines-linux-create-upload-generic.md).)

| Pseudonim     | Publisher | Ponude        | SKU         | Verzija |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7,2         | najnovije  |
| CoreOS    | CoreOS    | CoreOS       | Stabilan      | najnovije  |
| Debian    | credativ  | Debian       | 8           | najnovije  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | najnovije  |
| RHEL      | Crvena razgovor    | RHEL         | 7,2         | najnovije  |
| UbuntuLTS | Kanonski | Ubuntu poslužitelja | 14.04.4-LTS | najnovije  |

Sljedeće sekcije koristi u `UbuntuLTS` pseudonim za mogućnost **ImageURN** (`-Q`) za implementaciju poslužitelj za Ubuntu 14.04.4 LTS.

Prethodna `quick-create` primjer samo istaknut na `-M` zastavicu za prepoznavanje SSH javni ključ da biste prenijeli tijekom onemogućivanje lozinkama SSH tako da se Traži sljedeće argumente:

- Naziv grupe resursa (bilo koji niz je obično precizno za prvu grupu resursa za Azure)
- Naziv VM
- mjesto (`westus` ili `westeurope` su dobar zadane postavke)
- Linux (da biste omogućili Azure znati koje OS koju želite)
- korisničko ime

U sljedećem primjeru navodi sve vrijednosti tako da nema dodatno pitanja je obavezan. Dok god imate programa `~/.ssh/id_rsa.pub` kao ssh-rsa oblik javni ključ datoteku, funkcionira kao što je:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

Izlaz trebao izgledati kao sljedećeg bloka izlaz:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Prijavite se u novi VM

Prijavite se vaše VM pomoću javnu IP adresu na popisu izlaz. Možete koristiti i na potpuno kvalificirani naziv domene (FQDN) koja se nalazi:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Postupak prijave bi morao izgledati slično sljedećeg bloka izlaz:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Daljnji koraci

Na `azure vm quick-create` naredba je način za brzo uvesti u VM da biste mogli prijaviti na tulumu ljuske i početak rada. Međutim, pomoću `vm quick-create` ne dobivate proširenom kontrolu kao niti ga omogućuju vam da biste stvorili složeniji okruženju.  Da biste implementirali Linux VM koja je prilagođena za vaše infrastrukture, slijedite neki od ovih članaka:

- [Stvaranje određene implementacije pomoću predloška Azure Voditelj resursa](virtual-machines-linux-cli-deploy-templates.md)
- [Stvaranje vlastite prilagođene okruženje za Linux VM izravno pomoću naredbi EŽA Azure](virtual-machines-linux-create-cli-complete.md)
- [Stvaranje programa SSH osigurani Linux VM na Azure pomoću predložaka](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Možete i [koristite na `docker-machine` Azure upravljački program s različite naredbe da biste brzo stvorili Linux VM kao glavno računalo docker](virtual-machines-linux-docker-machine.md).
