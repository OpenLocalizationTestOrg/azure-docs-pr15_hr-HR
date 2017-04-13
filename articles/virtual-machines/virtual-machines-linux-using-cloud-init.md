<properties
    pageTitle="Korištenje oblaka pokretanja da biste prilagodili Linux VM tijekom stvaranja | Microsoft Azure"
    description="Da biste prilagodili Linux VM tijekom stvaranja pomoću oblaka pokretanja."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Da biste prilagodili Linux VM tijekom stvaranja pomoću oblaka pokretanja

U ovom se članku objašnjava da biste oblaka pokretanja skriptu da biste postavili naziv glavnog računala, paketi ažuriranja instalacije i upravljati korisničkim računima.  Oblak pokretanja skripte nazivaju se tijekom stvaranja VM iz Azure EŽA.  U članku zahtijeva:

- Azure račun ([dobiti besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure EŽA](../xplat-cli-install.md) prijavili `azure login`.

- način za Azure Voditelj resursa Azure EŽA _mora biti u_ `azure config mode arm`.

## <a name="quick-commands"></a>Brzi naredbe

Stvaranje oblaka init.txt skriptu koja se postavlja glavnog računala ažurira sve pakete i dodaje korisnik sudo Linux.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Stvorite grupu resursa za pokretanje VMs u.

```bash
azure group create myResourceGroup westus
```

Stvaranje Linux VM pomoću oblaka pokretanja da biste konfigurirali prilikom pokretanja.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Detaljni vodič

### <a name="introduction"></a>Uvod

Kada pokrenete novi VM Linux, dobivate standardna Linux VM s ništa prilagođene ili jeste li spremni za vaše potrebe. [Oblak pokretanja](https://cloudinit.readthedocs.org) je standardni način ubaciti skripte ili konfiguracija postavki u tom VM Linux kao što je pokrenuo za prvi put.

Na Azure, postoje tri načina da biste unijeli promjene na Linux VM dok ga se implementiran ili pokrenuto.

- Ubaci skripti pomoću oblaka pokretanja.
- Ubaci skripti pomoću Azure [VMAccess nastavak](virtual-machines-linux-using-vmaccess-extension.md).
- Azure predložak pomoću oblaka pokretanja.
- Azure predložak pomoću [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Da biste u bilo kojem trenutku nakon pokretanja ubaciti skripte:

- SSH za izvođenje naredbe izravno
- Ubaci skripti pomoću Azure [VMAccess proširenje](virtual-machines-linux-using-vmaccess-extension.md)imperatively ili u predložak programa Azure
- Ansible soli, Chef i Puppet, kao što su Alati za upravljanje konfiguracije.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Oblak pokretanja dostupnost na Azure VM brzo stvaranje pseudonima slike:

| Pseudonim     | Publisher | Ponude        | SKU         | Verzija | Oblak pokretanja |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | Centos       | 7,2         | najnovije  | ne         |
| CoreOS    | CoreOS    | CoreOS       | Stabilan      | najnovije  | Da        |
| Debian    | credativ  | Debian       | 8           | najnovije  | ne         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | najnovije  | ne         |
| RHEL      | Redhat    | RHEL         | 7,2         | najnovije  | ne         |
| UbuntuLTS | Kanonski | UbuntuServer | 14.04.4-LTS | najnovije  | Da        |

Microsoft surađuje s našim partnerima da biste dobili oblaka pokretanja uključeni i rada na slike koje omogućuju Azure.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Dodavanje skripte oblaka pokretanja stvaranje VM pomoću EŽA Azure

Da biste pokrenuli skriptu oblaka pokretanja pri stvaranju na VM u Azure, navedite datoteku oblaka pokretanja pomoću EŽA Azure `--custom-data` prijelaz.

Stvorite grupu resursa za pokretanje VMs u.

```bash
azure group create myResourceGroup westus
```

Stvaranje Linux VM pomoću oblaka pokretanja da biste konfigurirali prilikom pokretanja.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Stvaranje oblaka pokretanja skriptu da biste postavili hostname Linux VM

Najjednostavniji i najvažnije postavke za sve VM Linux bi glavnog računala. Ne možemo jednostavno postaviti to oblaka pokretanja pomoću ovu skriptu.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Primjer oblaka pokretanja skripte pod nazivom `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Tijekom početne pokretanja na VM ovu skriptu oblaka pokretanja postavlja glavnog računala na `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Prijava i provjerite je li hostname novi VM.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Stvaranje oblaka pokretanja skriptu da biste ažurirali Linux

Sigurnost, želite vaše VM Ubuntu da biste ažurirali prilikom prvog pokretanja.  Korištenje oblaka pokretanja smo možete učiniti s skriptu prati, ovisno o distribuciji Linux koristite.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Primjer oblaka pokretanja skripte `cloud_config_apt_upgrade.txt` za Debian obitelj

```bash
#cloud-config
apt_upgrade: true
```

Kada je pokrenuto Linux, instalirani paketi ažuriraju putem `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Prijava i provjerite je li se ažurirati sve pakete.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Stvaranje oblaka pokretanja skripte za dodavanje korisnika u grupu Linux

Jedna od prvog zadaci na bilo kojem novi VM Linux jest dodavanje korisnika za sebe ili da biste izbjegli korištenje `root`. SSH tipke su preporučenim načinom rada za sigurnost i upotrebljivosti i ona se dodaju u `~/.ssh/authorized_keys` datoteku s ovu skriptu oblaka pokretanja.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Primjer oblaka pokretanja skripte `cloud_config_add_users.txt` za Debian obitelj

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Kada je pokrenuto Linux, navedenih korisnici su stvoreni i dodani u grupi sudo.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Prijava i provjerite je li novostvoreni korisnika.

```bash
ssh myVM
cat /etc/group
```

Izlaz

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Daljnji koraci

Oblak pokretanja postaje jedan standardni način da biste izmijenili vaše VM Linux na pokretanje. Azure ima proširenja VM koje omogućuju vam da biste izmijenili LinuxVM pokretanje ili dok se izvodi. Ako, na primjer, možete koristiti Azure VMAccessExtension da biste ponovno postavili SSH ili korisnik podatke dok se izvodi na VM. S oblaka pokretanja, morate ponovno pokrenite računalo ponovno postavljanje lozinke.

[O proširenja virtualnog računala i značajkama](virtual-machines-linux-extensions-features.md)

[Upravljanje korisnicima, SSH i potvrdite ili popravak diskova na Azure Linux VMs korištenje nastavka VMAccess](virtual-machines-linux-using-vmaccess-extension.md)
