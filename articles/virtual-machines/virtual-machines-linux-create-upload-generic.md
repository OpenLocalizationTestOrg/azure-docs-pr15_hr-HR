<properties
    pageTitle="Stvoriti i prenijeti Linux VHD servisu Azure"
    description="Saznajte kako stvoriti i prenijeti na Azure virtualne tvrdi disk (VHD) koji sadrži Linux operacijski sustav."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Informacije za koje nisu licencira distribucije #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Važno**: SLA platforme Azure u odnosi se na virtualnim strojevima operacijski Linux samo kad je jedan od [licencira distribucija](virtual-machines-linux-endorsed-distros.md) koristi. Sve distribucija Linux kojemu u galeriji Azure slika su licencira distribucija u konfiguraciji potrebna.

- [Linux na Azure - licencira distribucije](virtual-machines-linux-endorsed-distros.md)
- [Podrška za Linux slike u programu Microsoft Azure](https://support.microsoft.com/kb/2941892)

Sve distribucija sustavom Azure ćete morati zadovoljavaju nekoliko preduvjeta za su izgledi ispravno pokrenuti na platformi.  Ovaj je članak po nisu means potpun svaki raspodjele je drugi; i moguće je prilično da čak i ako zadovoljavaju sve kriterije ispod i dalje morat ćete znatno dotjerati sustav Linux da biste bili sigurni da je ispravno izvodi na platformi.

Je zbog toga preporučujemo da započnete s jednim od naše [Linux na distribucija licencira Azure](virtual-machines-linux-endorsed-distros.md) kada je to moguće. U sljedećim člancima će vas voditi kroz kako pripremiti na razne endorsed Linux distribucija koje su podržane u Azure:

- **[Distribucija utemeljen na centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Crvena je vaša Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Ostatak ovaj članak sadrži opće smjernice za pokretanje sustava raspodjele Linux na Azure.


## <a name="general-linux-installation-notes"></a>Napomene o instalaciji Općenito Linux ##

- Oblik VHDX nije podržan u Azure, samo **nepromjenjive VHD**.  Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet Pretvori vhd. Ako koristite VirtualBox to znači da odabirom **Fixed veličina** umjesto zadanog dinamički dodijeliti prilikom stvaranja disk.

- Prilikom instalacije sustava Linux je *preporučuje se* da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk OS ikada potrebno priloženi drugi identične VM za otklanjanje poteškoća. [LVM](virtual-machines-linux-configure-lvm.md) ili [RAID](virtual-machines-linux-configure-raid.md) može se koristiti na diskova podataka.

- Potreban je otklanjanje podrška za postavljanje UDF datotečnih. Prilikom prvog pokretanja sustava na Azure dodjele resursa konfiguracije se prenosi Linux VM putem UDF oblikovani medijskih sadržaja koji je pridružen za goste. Agent za Azure Linux moraju imati mogućnost postavljanja datotečni sustav UDF čitanje svoju konfiguraciju i dodjela resursa u VM.

- Linux otklanjanje verzije ispod 2.6.37 ne podržavaju NUMA na Hyper-V s veće VM. Ovaj problem prvenstveno impacts starije distribucija pomoću na upstream otklanjanje crvena je vaša 2.6.32 i riješen u RHEL 6.6 (Otklanjanje-2.6.32-504). Sustavi izvodi prilagođene jezgre starije od 2.6.37 ili utemeljen na RHEL jezgre starije od 2.6.32-504 morate postaviti parametar pokretanje `numa=off` na otklanjanje naredbenog retka u grub.conf. Dodatne informacije potražite u članku razgovor crveno [KB 436883](https://access.redhat.com/solutions/436883).

- Konfiguriranje zamjena particije na disku OS. Agent za Linux moguće je konfigurirati da biste stvorili datoteku zamjena na disku privremene resursa.  Dodatne informacije o tome pronaći ćete u koracima u nastavku.

- Sve se VHDs mora imati veličine koje su višekratnike od 1 MB.


### <a name="installing-linux-without-hyper-v"></a>Instaliranje Linux bez Hyper-V ###

U nekim slučajevima Linux instalaciju možda ne sadrže upravljačke programe za Hyper-V u početne ramdisk (initrd ili initramfs) osim ako ih ne otkrije da se izvodi u okruženju Hyper-V.  Kada koristite drugi virtualizacije sustava (odnosno Virtualbox KVM, itd.) da biste pripremili Linux sliku, možda ćete morati ponovno stvaranje initrd osigurava da barem `hv_vmbus` i `hv_storvsc` moduli otklanjanje dostupne su na početni ramdisk.  Ovo je poznat problem barem na sustavima koji se temelji na upstream raspodjele je vaša crvene boje.

Mehanizmi za ponovno stvaranje slika initrd ili initramfs razlikuje se ovisno o distribuciju. Vaše raspodjele dokumentaciju ili podrške za pravilan postupak.  Evo jednog primjera kako ponovno stvaranje pomoću initrd na `mkinitrd` utility:

Najprije sigurnosno kopiranje postojeću sliku initrd:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Nakon toga ponovno stvaranje initrd s na `hv_vmbus` i `hv_storvsc` otklanjanje modula:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Promjena veličine VHDs ###

Slika VHD na Azure mora imati virtualne veličina poravnati 1MB.  Obično VHDs stvoren pomoću značajke Hyper-V mora već biti poravnat pravilno.  Ako na VHD nije ispravno poravnat pa možete primiti poruku o pogrešci koja je slična kada pokušate stvoriti *sliku* iz vaše VHD:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Da biste to riješili možete promijeniti veličinu VM pomoću konzole za Upravitelj Hyper-V ili cmdleta ljuske Powershell za [Promjenu veličine VHD](http://technet.microsoft.com/library/hh848535.aspx) .  Ako ne koristite u okruženju sustava Windows pa preporučuje se korištenje qemu img da biste pretvorili (Ako je potrebno) i promjena veličine u VHD.

> [AZURE.NOTE] Postoji poznatih problema u verzijama qemu img > = 2.2.1 čiji je rezultat nepravilno oblikovani VHD. Problem riješen u QEMU 2,6. Preporučuje se da biste koristili qemu img 2.2.0 ili podređenijeg ili ažuriranje na 2,6 ili noviji. Referenca: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Promjena veličine VHD izravno alatima kao što su `qemu-img` ili `vbox-manage` može uzrokovati neće VHD.  Stoga preporučuje se prvi Pretvori VHD NEOBRAĐENOG disk sliku.  Ako slika VM je već stvoren obliku NEOBRAĐENOG disk slike (Zadana postavka za neke Hypervisors kao što je KVM) možda preskočite ovaj korak:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Izračunavanje potrebne veličine slika na disku da biste bili sigurni poravnan virtualni veličina 1MB.  Ovaj vam mogu pomoći sljedeće tulumu Jezgrena skripta.  Koristi skriptu "`qemu-img info`" da biste odredili virtualne veličinu slike na disku i zatim ponovno izračunava veličinu tako da se sljedeći 1MB:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Promjena veličine sirovim disk pomoću $rounded_size postavljen iznad skripte:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Sada NEOBRAĐENOG disku ponovno pretvoriti-veličina VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Preduvjeti za otklanjanje Linux ##

Linux servisima za integraciju (LIS) upravljačke programe za Hyper-V i Azure pridonio su izravno upstream otklanjanje Linux. Mnoge distribucija koji sadrže najnovije verzije Linux otklanjanje (odnosno 3.x) će već imam te upravljačke programe ili inače backported verzijama te upravljačke programe pružiti njihove jezgre.  Te upravljačke programe stalno se ažuriraju u upstream otklanjanje s nove popravke i značajkama, pa kada je to moguće preporučuje se da biste pokrenuli programa [licencira raspodjele](virtual-machines-linux-endorsed-distros.md) koji obuhvaća tih rješenja i ažuriranja.

Ako su pokrenuti varijantu crveno je vaša Enterprise Linux verzije **6.0 6,3**, zatim morat ćete instalirati najnovije upravljačke programe za LIS za Hyper-V. [Na tom mjestu](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409)možete pronaći upravljačke programe. Od RHEL **6,4 +** (i Izvedenice) upravljačke programe za LIS već isporučuje se uz Jezgra sustava i tako da nema dodatne instalaciju paketa potrebna za pokretanje te sustavima na Azure.

Ako je potrebno prilagođene otklanjanje, preporučuje se da biste koristili otklanjanje noviju verziju (odnosno **3.8 +**). Za te distribucija ili dobavljače koji održavate vlastite otklanjanje neke trud bit će morati redovito backport upravljačke programe za LIS iz upstream otklanjanje za svoje prilagođene otklanjanje.  Čak i ako već imate relativno najnovijom verzijom otklanjanje, preporučljivo je da biste pratili sve upstream rješenja u LIS upravljačke programe i backport one prema potrebi. Mjesto izvorne datoteke upravljački program LIS dostupna je u datoteci [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) u stablu Linux otklanjanje izvora:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Na vrlo minimalne, poznato Izostanak sljedećih zakrpa bi mogli uzrokovati poteškoće na Azure i tako da ih mora biti uključeno u Jezgra sustava. Ovaj je popis po nisu means iscrpan ili potpune za sve distribucija:

- [ata_piix: Odgodi diskova za upravljačke programe za Hyper-V prema zadanim postavkama](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: račun za tranzitne pakete na putu Vrati izvorne postavke](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: izbjegli korištenje WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: onemogućili PISANJE ISTI za RAID i upravljačke programe za prilagodnik virtualnog glavnog računala](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: NULL pokazivač dereference popravak](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: Nazovi međuspremnik neuspjeha može uzrokovati/i Zamrzavanje](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: zaštitili dvostruki izvođenja __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Agent za Azure Linux ##

[Agent za Azure Linux](virtual-machines-linux-agent-user-guide.md) (waagent) potreban je ispravno Dodjela Linux virtualnog računala u Azure. Mogli dobiti najnoviju verziju, datoteka problema ili Pošalji zahtjeve istaknuti pri [repo Linux Agent GitHub](https://github.com/Azure/WALinuxAgent).

- Agent za Linux je objavio licencom Apache 2.0. Mnoge distribucija već omogućavaju RPM ili deb paketa agenta i tako da u nekim slučajevima to može instalirati i ažurirati malo truda.

- Agent za Azure Linux zahtijeva Python v2.6 +.

- Agenta zahtijeva i python pyasn1 modul. Većina distribucija to vam kao zasebna paket koji se može instalirati.

- U nekim slučajevima Azure Linux Agent možda nije kompatibilan s NetworkManager. Mnoge paketa RPM/Deb nudi distribucija konfiguriranje NetworkManager kao sukoba waagent paket, a tako će deinstalirati NetworkManager pri instalaciji paketa agent Linux.


## <a name="general-linux-system-requirements"></a>Općenito Linux sistemske preduvjete ##

- Izmjena redak za pokretanje otklanjanje u GRUB ili GRUB2 da biste uključili sljedećih parametara. Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema.

    Osim navedenog, preporučuje se da biste *uklonili* sljedećih parametara ako postoje:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.

    Na `crashkernel` mogućnost može biti lijevo konfiguriran po želji, ali bilješke da taj parametar se skraćuje memorije u VM po 128MB ili više njih, što može biti problematična na manje VM.

- Instaliranje Azure Linux Agent

    Za dodjeljivanje Linux sliku na Azure potreban je agenta za Azure Linux.  Mnoge distribucija pružaju agenta kao paketa RPM ili Deb (paket se obično naziva 'WALinuxAgent' ili 'walinuxagent').  Agenta može biti instaliran i ručno tako da slijedite korake u [Vodič Agent Linux](virtual-machines-linux-agent-user-guide.md).

- Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja.  To je obično zadano.

- Stvaranje zamjena prostora na disku OS

    Agent za Azure Linux automatski možete konfigurirati zamjena prostor pomoću diska lokalnog resursa koja je priložena u VM nakon dodjele resursa na Azure. Imajte na umu *privremene* diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon instalacije Azure Linux Agent (pogledajte prethodni korak), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Kao konačna korak, pokrenite sljedeće naredbe deprovision virtualnog računala:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Na Virtualbox vidjet ćete sljedeće pogreške nakon pokretanja ' waagent – prisilno - deprovision': `[Errno 5] Input/output error`. Poruka o pogrešci nije od ključne važnosti i možete je zanemariti.

- Morat ćete pa isključite virtualnog računala i prenesite na VHD Azure.
