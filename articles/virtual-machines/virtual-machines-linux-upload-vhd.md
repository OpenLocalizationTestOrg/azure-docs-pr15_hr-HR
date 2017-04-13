<properties
    pageTitle="Stvoriti i prenijeti prilagođenu sliku Linux | Microsoft Azure"
    description="Stvoriti i prenijeti virtualne tvrdom disku (VHD) za Azure slikom prilagođene Linux pomoću modela implementacije Voditelj resursa."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Prijenos i stvaranje Linux VM iz prilagođene disk slike

U ovom se članku objašnjava da biste prenijeli virtualne tvrdom disku (VHD) za Azure pomoću modela za implementaciju upravljanja resursima i stvaranje Linux VMs iz prilagođene sliku. To vam omogućuje da biste instalirali i konfigurirali Linux distro da biste svojim potrebama i zatim koristiti taj VHD da biste brzo stvorili Azure virtualnim strojevima (VMs).

## <a name="quick-commands"></a>Brzi naredbe
Ako morate brzo obaviti zadatak, sljedeće sekcije logaritma naredbe da biste prenijeli na VM Azure. Detaljnije informacije i kontekst svakog koraka možete pronaći ostatku dokumenta, [počevši ovdje](#requirements).

Provjerite jeste li povezani s [EŽA Azure](../xplat-cli-install.md) prijavljen i koristite način upravljanja resursima:

```bash
azure config mode arm
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjer dio `myResourceGroup`, `mystorageaccount`, i `myimages`.

Prvo, stvorite grupu resursa. Sljedeći primjer stvara grupu resursa pod nazivom `myResourceGroup` u na `WestUs` mjesto:

```bash
azure group create myResourceGroup --location "WestUS"
```

Stvorite račun za pohranu za vaše virtualne diskova. Sljedeći primjer stvara za pohranu račun pod nazivom `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Navedeni tipkovni prečaci za vaš račun za pohranu. Zabilježite `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Stvaranje spremnika unutar s računom za pohranu pomoću ključa za pohranu ste nabavili. Sljedeći primjer stvara spremnik pod nazivom `myimages` pomoću vrijednost ključa za pohranu iz `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Na kraju, prenesite na VHD spremniku koji ste stvorili. Navedite lokalni put do vaše VHD u odjeljku `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Sad možete stvarati u VM vaše prenesene virtualne disk [pomoću predloška Voditelj resursa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Možete koristiti u EŽA navođenjem URI na disk (`--image-urn`). Sljedeći primjer stvara VM pod nazivom `myVM` koje ste prethodno prenijeli pomoću virtualne diska:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Račun za pohranu odredište mora biti jednak koju ste prenijeli virtualne disku. Morate navesti ili odgovora pita za dodatne parametre potrebnih u `azure vm create` naredbe kao što su virtualne mreže, javnu IP adresu, korisničko ime i SSH tipke. Dodatne informacije o [dostupnih Voditelj resursa EŽA parametara](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Preduvjeti
Da biste dovršili sljedeće korake, potrebno je:

- **Linux operacijski sustav instaliran u datoteci .vhd** – instalacija [Azure licencira Linux raspodjele](virtual-machines-linux-endorsed-distros.md) (ili vidjeti [podatke koji nisu licencira distribucija](virtual-machines-linux-create-upload-generic.md)) virtualne disk u obliku VHD. Postoji više alata za stvaranje VM i VHD:
    - Instaliranje i konfiguriranje [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) ili [KVM](http://www.linux-kvm.org/page/RunningKVM), pobrinite se da biste koristili VHD obliku slike. Ako je potrebno, možete ga [pretvoriti sliku](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomoću `qemu-img convert`.
    - Možete koristiti i Hyper-V [na Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) ili [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] U noviji oblik VHDX nije podržan u Azure. Kada stvorite na VM, odredite VHD kao oblik. Ako je potrebno, možete pretvoriti VHDX diskova VHD pomoću [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) ili [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) cmdlet ljuske PowerShell. Nadalje, Azure ne podržava prijenos dinamičke VHDs da morate pretvoriti takve diskova statične VHDs prije prijenosa. Možete koristiti alate kao što su [Azure VHD uslužni programi za OTVORITE](https://github.com/Microsoft/azure-vhd-utils-for-go) da biste pretvorili dinamičkih diskova tijekom postupka prenosi Azure.

- VMs stvorene iz prilagođenu sliku moraju se nalaziti u isti prostor za pohranu račun kao samu sliku
    - Stvaranje računa za pohranu i spremnik za vaše prilagođenu sliku i stvorili VMs
    - Nakon stvaranja sve VMs sigurno možete izbrisati sliku

Provjerite jeste li povezani s [EŽA Azure](../xplat-cli-install.md) prijavljen i koristite način upravljanja resursima:

```bash
azure config mode arm
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjer dio `myResourceGroup`, `mystorageaccount`, i `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Priprema slike je moguće prenijeti

Azure podržava razne Linux distribucija (pogledajte [Licencira distribucija](virtual-machines-linux-endorsed-distros.md)). U sljedećim člancima će vas voditi kroz kako pripremiti različite distribucija Linux koje su podržane u Azure:

- **[Distribucija utemeljen na centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Crvena je vaša Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Ostale - distribucije koje nisu licencira](virtual-machines-linux-create-upload-generic.md)**

Pogledati i **[Napomene o instalaciji Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** Priprema slike Linux za Azure općenite savjete za.

> [AZURE.NOTE] [Azure platforme SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) primjenjuje se na VMs izvodi Linux samo kad je jedan od endorsed distribucija koristi s detaljima o konfiguraciji kao što je navedeno u odjeljku "podržani verzije' u [Linux na Azure-Endorsed distribucija](virtual-machines-linux-endorsed-distros.md).


## <a name="create-a-resource-group"></a>Stvaranje grupa resursa
Grupa resursa logično objediniti Azure resurse za podršku virtualnih računala, kao što su virtualne mreže i prostora za pohranu. Dodatne informacije o [grupama Azure resursa u nastavku](../azure-resource-manager/resource-group-overview.md). Prije nego što prenijeti sliku prilagođene disku i stvaranje VMs, najprije morate stvoriti grupu resursa. 

Sljedeći primjer stvara grupu resursa pod nazivom `myResourceGroup` u na `WestUS` mjesto:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu
VMs spremaju se kao BLOB-Ova stranica za pohranu subjekta. Dodatne informacije o [blobova platforme Azure ovdje](../storage/storage-introduction.md#blob-storage). Stvaranje računa za pohranu za prilagođene disk slike i VMs. Bilo koji VMs koji stvorite prilagođeni disk sliku moraju biti isti prostor za pohranu račun kao toj slici.

Sljedeći primjer stvara za pohranu račun pod nazivom `mystorageaccount` u grupi resursa koje ste prethodno stvorili:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Popis za pohranu računa tipke
Azure stvara dva 512-bitni tipkovni prečaci za svaki račun za pohranu. Ove tipke za pristup se koriste prilikom provjere autentičnosti s računom za pohranu, kao što su za izvođenje operacije pisanja. Dodatne informacije potražite u o [upravljanju pristup ovdje prostora za pohranu](../storage/storage-create-storage-account.md#manage-your-storage-account). Možete pogledati pristupnih tipki s na `azure storage account keys list` naredbe.

Prikaz pristupnih tipki za pohranu račun koji ste stvorili:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Izlaz slično je:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Zabilježite `key1` jer će vam omogućuje interakciju s računa za pohranu u sljedećim koracima.

## <a name="create-a-storage-container"></a>Stvaranje spremnika za pohranu
Na isti način stvoriti različite direktorija da biste logično organizirali lokalni datotečni sustav, stvorite spremnika za pohranu subjekta da biste organizirali virtualne diskova i slike. Prostor za pohranu račun može sadržavati bilo koji broj spremnika. 

Sljedeći primjer stvara spremnik pod nazivom `myimages`, pri određivanju tipka za pristup nabavili u prethodnom koraku (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Prijenos VHD
Sada možete prenijeti zapravo sliku prilagođene disk. Kao sa svih virtualne disketa koristi VMs, prenesete i pohraniti prilagođeni disk sliku kao blob stranice.

Odredite ključ access, spremnik koji ste stvorili u prethodnom koraku, a zatim put slike prilagođene disk na lokalnom računalu:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Stvaranje VM iz prilagođenu sliku
Kada stvorite VMs galerije prilagođene disk, navedite URI sliku na disku. Pobrinite se da podudaranja račun za pohranu odredište pohranjuju sliku prilagođene disk. Možete stvoriti na VM pomoću predloška Azure EŽA ili JSON Voditelj resursa.


### <a name="create-a-vm-using-the-azure-cli"></a>Stvaranje VM pomoću EŽA Azure
Odredite na `--image-urn` parametar s na `azure vm create` naredbu pokažite na sliku prilagođene disk. Pobrinite se da `--storage-account-name` odgovara račun za pohranu pohranjuju sliku prilagođene disk. Ne morate koristite spremnik isti kao sliku prilagođene na disku za pohranu na VMs. Provjerite jeste li stvorili sve dodatne spremnika na isti način kao starijim korake prije prijenosa slike prilagođene disk.

Sljedeći primjer stvara VM pod nazivom `myVM` iz prilagođene disk sliku:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

I dalje morate navesti ili odgovora pita za dodatne parametre potrebnih u `azure vm create` naredbe kao što su virtualne mreže, javnu IP adresu, korisničko ime i SSH tipke. Dodatne informacije o [dostupnih Voditelj resursa EŽA parametara](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Stvaranje VM pomoću predloška JSON
Azure resursima Predlošci su JavaScript objekt notaciju (JSON) datoteke koje definiraju okruženje za koju želite stvoriti. Predlošci razvrstan različitim resursa usluga kao što su računalnim ili mrežu. Možete koristiti postojeće predloške ili napišite vlastiti. Saznajte više o [korištenju upravitelja resursa i predloške](../azure-resource-manager/resource-group-overview.md).

Unutar na `Microsoft.Compute/virtualMachines` davatelja predloška, imate u `storageProfile` čvor koji sadrži detalje za konfiguraciju za vaše VM. Dva glavna parametara da biste uredili u `image` i `vhd` ji koji vode do prilagođene disk sliku i novi VM virtualne disk. U sljedećem odjeljku prikazuju primjera JSON za korištenje prilagođenih disk slike:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Možete koristiti [ovaj postojećeg predloška za stvaranje VM iz prilagođenu sliku](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) ili Saznajte više o [stvaranju predložaka Azure Voditelj resursa](../resource-group-authoring-templates.md). 

Nakon što dodate konfiguriran predložak, stvarate vaše VMs pomoću na `azure group deployment create` naredbe. Odredite URI JSON predloška s na `--template-uri` parametar:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Ako imate datoteke JSON lokalno pohranjene na računalu, možete koristiti u `--template-file` parametar umjesto:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Daljnji koraci
Nakon što pripremili i prenijeli prilagođene virtualne na disku, možete potražite dodatne informacije o [korištenju upravitelja resursa i predloške](../azure-resource-manager/resource-group-overview.md). Možda i želite da biste [dodali podatkovni disk](virtual-machines-linux-add-disk.md) na novu VMs. Ako imate aplikacije koji se izvode na vašem VMs koje su vam potrebne da biste pristupili, provjerite da biste [otvorili priključci i krajnje točke](virtual-machines-linux-nsg-quickstart.md).