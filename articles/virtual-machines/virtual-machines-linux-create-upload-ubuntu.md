<properties
    pageTitle="Stvoriti i prenijeti na Ubuntu Linux VHD servisu Azure"
    description="Saznajte kako stvoriti i prenijeti na Azure virtualne tvrdi disk (VHD) koji sadrži operacijski sustav Ubuntu Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Priprema za Ubuntu virtualnog računala za Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Slike oblaka za službeni Ubuntu
Ubuntu sada objavljuje službeni Azure VHDs preuzeti na adresi [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Ako je potrebno stvoriti vlastite slike za specijalizirane Ubuntu za Azure radije nego postupkom ručno ispod preporučuje se da biste pokrenuli s te poznati radi VHDs i prilagoditi ga prema potrebi. Najnovija izdanja slike uvijek možete pronaći na sljedećim mjestima:

 - Ubuntu 12.04/precizno: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04 Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Preduvjeti

U ovom se članku pretpostavlja da ste već instalirali operacijski sustav Ubuntu Linux virtualne tvrdi disk. Postoji više alata za stvaranje .vhd datoteka, primjerice rješenja virtualizacije kao što su Hyper-V. Upute potražite u članku [Instalacija Hyper-V uloge i konfiguracija virtualnog računala](http://technet.microsoft.com/library/hh846766.aspx).

**Napomene o instalaciji Ubuntu**

- Pogledajte i [Općenito Linux napomene o instalaciji](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) dodatne savjete za Azure Priprema Linux.
- Oblik VHDX nije podržan u Azure, samo **Fiksno VHD**.  Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet Pretvori vhd.
- Prilikom instalacije sustava Linux preporučuje se da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk OS ikada potrebno priloženi drugi VM za otklanjanje poteškoća. [LVM](virtual-machines-linux-configure-lvm.md) ili [RAID](virtual-machines-linux-configure-raid.md) može se koristiti na diskova podataka ako Preferirani.
- Konfiguriranje zamjena particije na disku OS. Agent za Linux moguće je konfigurirati da biste stvorili datoteku zamjena na disku privremene resursa.  Dodatne informacije o tome pronaći ćete u koracima u nastavku.
- Sve se VHDs mora imati veličine koje su višekratnike od 1 MB.


## <a name="manual-steps"></a>Ručni koraci

> [AZURE.NOTE] Prije stvaranja vlastitu prilagođenu sliku Ubuntu za Azure, razmotrite umjesto toga koristite slika iz [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) .


1. U oknu centra za Upravitelj Hyper-V odaberite virtualnog računala.

2. Kliknite **Poveži** da biste otvorili prozor za virtualnog računala.

3.  Zamijenite trenutni spremištima slike da biste koristili Azure repos Ubuntu korisnika. Koraci malo razlikovati ovisno o verziji Ubuntu.

    Prije uređivanja /etc/apt/sources.list, preporučuje se da biste ih sigurnosno kopirali:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. Slika Ubuntu Azure sada pratite otklanjanje *omogućenja hardver* (HWE). Ažuriranje operacijskog sustava za najnovije otklanjanje ponovnim pokretanjem sljedeće naredbe:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Izmjena redak za pokretanje otklanjanje za Grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili Otvori "/ zadani/itd/grub" u uređivaču teksta, pronađite varijabla pod nazivom `GRUB_CMDLINE_LINUX_DEFAULT` (ili dodavanje ako je potrebno) i uredite ga da biste uključili sljedećih parametara:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Spremite i zatvorite datoteku, a zatim pokrenite "`sudo update-grub`". To će osigurati sve konzole poruke šalju prvi serijski priključak, koji vam mogu pomoći Azure tehnička podrška s ispravljanje pogrešaka problema.

6.  Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja.  To je obično zadano.

7.  Instalirajte Azure Linux Agent:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Imajte na umu te instalacije na `walinuxagent` će uklonili paket na `NetworkManager` i `NetworkManager-gnome` paketi, ako su instalirani.

8.  Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Kliknite **Akcije -> isključi prema dolje** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.


## <a name="next-steps"></a>Daljnji koraci
Sada ste spremni za korištenje Ubuntu Linux virtualne tvrdi disk da biste stvorili novi virtualnim strojevima u Azure. Ako je ovo prvi put da prenosite datoteke .vhd za Azure, pogledajte korake 2 i 3 u [Stvaranje i prijenos virtualne tvrdi disk koji sadrži operacijski sustav Linux](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Reference ##

Ubuntu hardver omogućenja (HWE) otklanjanje:

- [http://blog.utlemming.org/2015/01/ubuntu-1404-Azure-Images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-Azure-Cloud-Images-Now-using-hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
