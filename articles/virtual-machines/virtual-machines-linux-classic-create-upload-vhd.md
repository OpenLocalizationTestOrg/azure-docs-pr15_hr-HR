<properties
    pageTitle="Stvoriti i prenijeti Linux VHD | Microsoft Azure"
    description="Stvoriti i prenijeti na Azure virtualne tvrdi disk (VHD) s modelom uvođenje klasičnog koja sadrži operacijski sustav Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Stvaranje i prijenos virtualne tvrdi Disk koji sadrži Linux operacijski sustav

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Također možete [prenijeti sliku prilagođene disk pomoću upravitelja resursa Azure](virtual-machines-linux-upload-vhd.md).

U ovom se članku prikazuje kako stvoriti i prenijeti virtualne tvrdom disku (VHD) da biste ga mogli koristiti kao vlastite slike za stvaranje virtualnim strojevima Azure. Saznajte kako pripremiti operacijski sustav da biste mogli koristiti da biste stvorili više virtualnim strojevima koji se temelji na toj slici. 

>  [AZURE.NOTE] Ako imate nekoliko sekundi dok, Pridonesite poboljšanju dokumentaciju Azure Linux VM prihvaćanjem ovaj [Brzi upitnik](https://aka.ms/linuxdocsurvey) rad. Svaki odgovor pridonose pomoć zatražite posla.

## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da imate sljedeće stavke:

- **Linux operacijskih sustava u datoteci .vhd** - instalirali [Azure licencira Linux raspodjele](virtual-machines-linux-endorsed-distros.md) (ili vidjeti [podatke koji nisu licencira distribucija](virtual-machines-linux-create-upload-generic.md)) virtualne disk u obliku VHD. Postoji više alata za stvaranje VM i VHD:
    - Instaliranje i konfiguriranje [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) ili [KVM](http://www.linux-kvm.org/page/RunningKVM), pobrinite se da biste koristili VHD obliku slike. Ako je potrebno, možete ga [pretvoriti sliku](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomoću `qemu-img convert`.
    - Možete koristiti i Hyper-V [na Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) ili [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] U noviji oblik VHDX nije podržan u Azure. Kada stvorite na VM, odredite VHD kao oblik. Ako je potrebno, možete pretvoriti VHDX diskova VHD pomoću [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) ili [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) cmdlet ljuske PowerShell. Nadalje, Azure ne podržava prijenos dinamičke VHDs da morate pretvoriti takve diskova statične VHDs prije prijenosa. Možete koristiti alate kao što su [Azure VHD uslužni programi za OTVORITE](https://github.com/Microsoft/azure-vhd-utils-for-go) da biste pretvorili dinamičkih diskova tijekom postupka prenosi Azure.

- **Sučelje naredbenog retka za azure** – instalacija najnovije [Azure sučelje naredbenog retka](../virtual-machines-command-line-tools.md) da biste prenijeli na VHD.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Korak 1: Priprema slike je moguće prenijeti

Azure podržava razne Linux distribucija (pogledajte [Licencira distribucija](virtual-machines-linux-endorsed-distros.md)). U sljedećim člancima će vas voditi kroz kako pripremiti različite distribucija Linux koje su podržane u Azure. Kada dovršite korake iz sljedećeg vodilice, dolazak natrag nakon što dodate VHD datoteke koja je spremna za prijenos Azure:

- **[Distribucija utemeljen na centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Crvena je vaša Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Ostale - distribucije koje nisu licencira](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Azure platforme SLA primjenjuje se na virtualnim strojevima operacijski Linux samo kad je jedan od endorsed distribucija koristi s detaljima o konfiguraciji kao što je navedeno u odjeljku "podržani verzije' u [Linux na Azure-Endorsed distribucija](virtual-machines-linux-endorsed-distros.md). Sve Linux distribucija u galeriji Azure slike su licencira distribucija u konfiguraciji potrebna.

Pogledati i **[Napomene o instalaciji Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** Priprema slike Linux za Azure općenite savjete za.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Korak 2: Priprema veza Azure

Provjerite je li koristite EŽA Azure u modelu uvođenje klasičnog (`azure config mode asm`), zatim se prijavite na račun servisa:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Korak 3: Prijenos slike za Azure

Potreban račun za pohranu da biste prenijeli datoteke VHD. Možete odabrati postojećeg računa za pohranu ili [stvorite novi](../storage/storage-create-storage-account.md).

Koristite EŽA Azure prijenos slike pomoću sljedeće naredbe:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

U prethodnom primjeru:

- **BlobStorageURL** je URL za račun za pohranu namjeravate koristiti
- **YourImagesFolder** je spremnik unutar blobova mjesto na koje želite pohraniti slike
- **VHDName** je oznaku koja će se pojaviti na portalu za prepoznavanje virtualne na tvrdom disku.
- **PathToVHDFile** je cijeli put i naziv datoteke .vhd na vašem računalu.

Na sljedećoj je slici prikazan dovršeno primjer:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Korak 4: Stvaranje na VM iz slike
Stvaranje VM pomoću `azure vm create` na isti način kao obični VM. Navedite naziv dodijelili sliku u prethodnom koraku. U sljedećem primjeru se koristi naziv slike **UbuntuLTS** dan u prethodnom koraku:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Da biste stvorili vlastiti VMs, navedite vlastitu korisničko ime + lozinku, mjesto, naziv DNS-a i naziv slike.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u članku [Referenca za Azure EŽA za Azure uvođenje klasičnog model](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
