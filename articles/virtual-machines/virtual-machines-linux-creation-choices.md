<properties
    pageTitle="Drugi načini stvaranja Linux VM | Microsoft Azure"
    description="Informacije o različitim načinima za stvaranje Linux virtualnog računala na Azure, uključujući veze na alate i vodiči za svaki način."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Drugi načini stvaranja Linux virtualnog računala u Azure

Imate fleksibilnost u Azure da biste stvorili Linux virtualni stroj (VM) koristiti alate i tijekove rada upoznati s vama. U ovom se članku navedene su ove razlike i primjeri za stvaranje vaše VMs Linux.


## <a name="azure-cli"></a>Azure EŽA 

Azure EŽA dostupna na raznim platformama putem paketa npm, pod uvjetom distro paketa ili Docker spremnik. Dodatne informacije o [načinu instaliranja i konfiguriranja EŽA Azure](../xplat-cli-install.md). Sljedeći vodiči za sadrže primjere korištenjem EŽA Azure. Članak svaki više pojedinosti o naredbe Brzi start EŽA prikazan:

- [Stvaranje Linux VM iz EŽA Azure razvojni i test](virtual-machines-linux-quick-create-cli.md)
    - Sljedeći primjer stvara CoreOS VM javnim ključem pod nazivom `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Stvaranje sigurnog Linux VM pomoću predloška za Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Sljedeći primjer stvara VM pomoću predloška koji je pohranjen na GitHub:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Stvaranje okruženja Linux dovršeno pomoću EŽA Azure](virtual-machines-linux-create-cli-complete.md)
    - Obuhvaća stvaranje raspoređivača opterećenja i više VMs u skupu dostupnost.

- [Dodavanje na disku Linux VM](virtual-machines-linux-add-disk.md)
    - Sljedeći primjer dodaje 5Gb diska postojeće VM koji se naziva `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Portal za Azure

[Portal za Azure](https://portal.azure.com) omogućuje vam da biste brzo stvorili na VM jer nema ništa da biste instalirali na računalu. Da biste stvorili na VM pomoću portala za Azure:

- [Stvaranje Linux VM pomoću portala za Azure](virtual-machines-linux-quick-create-portal.md) 
- [Prilaganje na disku pomoću portala za Azure](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Operacijski sustav i mogućnosti slika
Kada stvarate na VM, odaberite sliku ovisno o operacijskom sustavu koji želite pokrenuti. Azure i njezinih partnera nude mnoge slike, koji obuhvaćaju aplikacije i alate unaprijed instalirano. Ili je prenesite nešto vlastite slike [(u sljedećem](#use-your-own-image)odjeljku).

### <a name="azure-images"></a>Azure slike
Korištenje na `azure vm image` EŽA naredbe da biste vidjeli što je dostupan publisher, izdanje distro i izdanja.

Popis dostupnih izdavači na sljedeći način:

```bash
azure vm image list-publishers --location WestUS
```

Popis dostupnih proizvoda (ponuda) za dani publisher na sljedeći način:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Popis dostupnih SKU-ove (distro izdanja) navedeni ponude na sljedeći način:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Popis svih dostupnih slika za dani izdanje slijedi:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Još primjera pregledavanja i pomoću dostupnih slika potražite u članku [Kretanje i slika odaberite Azure virtualnog računala pomoću EŽA Azure](virtual-machines-linux-cli-ps-findimage.md).

Na `azure vm quick-create` i `azure vm create` naredbe imaju pseudonime možete koristiti da biste brzo pristupili uobičajene distros i njihovih najnovija izdanja. Korištenje pseudonima često je brže nego navodeći publisher, ponuda, SKU i verziju svaki put kada stvorite na VM:

| Pseudonim     | Publisher | Ponude        | SKU         | Verzija |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7,2         | najnovije  |
| CoreOS    | CoreOS    | CoreOS       | Stabilan      | najnovije  |
| Debian    | credativ  | Debian       | 8           | najnovije  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | najnovije  |
| RHEL      | Redhat    | RHEL         | 7,2         | najnovije  |
| SLES      | SLES      | SLES         | 12 SP1      | najnovije  |
| UbuntuLTS | Kanonski | UbuntuServer | 14.04.4-LTS | najnovije  |

### <a name="use-your-own-image"></a>Korištenje vlastite slike

Ako tražite specifične prilagodbe, možete koristiti sliku koji se temelje na postojeće VM za Azure *hvatanje* te VM. Također možete prenijeti sliku lokalnog. Dodatne informacije o podržanim distros i kako koristiti svoje slike potražite u sljedećim člancima:

- [Azure licencira distribucije](virtual-machines-linux-endorsed-distros.md)

- [Informacije za koje nisu licencira distribucije](virtual-machines-linux-create-upload-generic.md)

- [Kako snimiti Linux virtualnog računala kao predloška Voditelj resursa](virtual-machines-linux-capture-image.md).
    - Brzi početak primjer naredbe da biste snimili postojeće VM:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Daljnji koraci

- Stvaranje Linux VM, na [portalu](virtual-machines-linux-quick-create-portal.md)s [EŽA](virtual-machines-linux-quick-create-cli.md)ili pomoću [predloška Azure Voditelj resursa](virtual-machines-linux-cli-deploy-templates.md).

- Nakon stvaranja Linux VM, [dodajte podatkovni disk](virtual-machines-linux-add-disk.md).

- Brze korake da biste [ponovno postavljanje lozinke ili više njih SSH i upravljanje korisnicima](virtual-machines-linux-using-vmaccess-extension.md)
