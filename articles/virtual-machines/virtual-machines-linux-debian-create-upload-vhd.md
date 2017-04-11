<properties
    pageTitle="Priprema Debian Linux VHD | Microsoft Azure"
    description="Saznajte kako stvoriti Debian 7 i 8 VHD datoteka za implementaciju u Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Priprema Debian VHD za Azure

## <a name="prerequisites"></a>Preduvjeti
U ovom se odjeljku pretpostavlja da ste već instalirali Debian Linux operacijski sustav iz .iso datoteke preuzete iz sustava [Debian web-mjesto](https://www.debian.org/distrib/) virtualne tvrdi disk. Postoji više alata za stvaranje datoteka .vhd; Hyper-V je samo jedan primjer. Upute za korištenje Hyper-V potražite u članku [Instalacija Hyper-V uloge i konfiguracija virtualnog računala](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Napomene o instalaciji

- Pogledajte i [Općenito Linux napomene o instalaciji](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) dodatne savjete za Azure Priprema Linux.
- U noviji oblik VHDX nije podržan u Azure. Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet **Pretvori vhd** .
- Prilikom instalacije sustava Linux preporučuje se da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk OS ikada potrebno priloženi drugi VM za otklanjanje poteškoća. [LVM](virtual-machines-linux-configure-lvm.md) ili [RAID](virtual-machines-linux-configure-raid.md) može se koristiti na diskova podataka ako Preferirani.
- Konfiguriranje zamjena particije na disku OS. Agent za Azure Linux moguće je konfigurirati da biste stvorili datoteku zamjena na disku privremene resursa. Dodatne informacije o tome pronaći ćete u koracima u nastavku.
- Sve se VHDs mora imati veličine koje su višekratnike od 1 MB.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Koristite Azure upravljanje da biste stvorili Debian VHDs

Nema dostupnih za generiranje Debian VHDs za Azure, kao što su Alati u [azure-upravljanje](https://github.com/credativ/azure-manage) skripti [credativ](http://www.credativ.com/). To je preporučeni način nasuprot stvaranje sliku ispočetka. Na primjer, da biste stvorili Debian 8 VHD izvršite sljedeće naredbe da biste preuzeli azure upravljati (i ovisnosti) i pokrenuti skriptu azure_build_image:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Ručno Priprema Debian VHD

1. U upravitelju Hyper-V odaberite virtualnog računala.

2. Kliknite **Poveži** da biste otvorili prozor konzole za virtualnog računala.

3. Komentar izvan redak za **deb cdrom** u `/etc/apt/source.list` ako postavili VM protiv ISO datoteke.

4. Uređivanje u `/etc/default/grub` datoteku i Izmjena parametra **GRUB_CMDLINE_LINUX** na sljedeći način da biste dodali dodatne otklanjanje parametara za Azure.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Ponovno stvaranje na grub i pokretanje:

        # sudo update-grub

6. Dodavanje Azure spremištima Debian na /etc/apt/sources.list za Debian 7 ili 8:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Instalirajte Azure Linux Agent:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 7 je potrebna za pokretanje otklanjanje utemeljen na 3.16 wheezy backports spremištu. Najprije stvoriti datoteku s nazivom /etc/apt/preferences.d/linux.pref sadržajem sljedeće:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Zatim Pokreni "sudo Zemaljska get Instaliraj linux-slika-amd64" da biste instalirali novi otklanjanje.

8. Deprovision virtualnog računala i Priprema za dodjelu resursa na Azure i pokretanje:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Kliknite **Akcija** -> isključi prema dolje u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.


## <a name="next-steps"></a>Daljnji koraci

Sada ste spremni za korištenje Debian virtualne tvrdi disk da biste stvorili novi virtualnim strojevima u Azure. Ako je ovo prvi put da prenosite datoteke .vhd za Azure, pogledajte korake 2 i 3 u [Stvaranje i prijenos virtualne tvrdi disk koji sadrži operacijski sustav Linux](virtual-machines-linux-classic-create-upload-vhd.md).
