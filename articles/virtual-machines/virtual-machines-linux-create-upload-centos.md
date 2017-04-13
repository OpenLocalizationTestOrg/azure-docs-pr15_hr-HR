<properties
    pageTitle="Stvoriti i prenijeti Linux VHD utemeljen na CentOS u Azure"
    description="Saznajte kako stvoriti i prenijeti na Azure virtualne tvrdi disk (VHD) koji sadrži CentOS sustavom Linux operacijski sustav."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Priprema virtualnog računala CentOS za Azure

- [Priprema CentOS 6.x virtualni stroj za Azure](#centos-6x)
- [Priprema CentOS 7.0 + virtualnog računala za Azure](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Preduvjeti ##

U ovom se članku pretpostavlja da ste već instalirali na CentOS (ili sličan izvedenica) Linux operacijski sustav virtualne tvrdi disk. Postoji više alata za stvaranje .vhd datoteka, primjerice rješenje virtualizacije kao što su Hyper-V. Upute potražite u članku [Instalacija Hyper-V uloge i konfiguracija virtualnog računala](http://technet.microsoft.com/library/hh846766.aspx).


**Napomene o instalaciji centOS**

- Pogledajte i [Općenito Linux napomene o instalaciji](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) dodatne savjete za Azure Priprema Linux.

- Oblik VHDX nije podržan u Azure, samo **nepromjenjive VHD**.  Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet Pretvori vhd.

- Prilikom instalacije sustava Linux preporučuje se da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk OS ikada potrebno priloženi drugi VM za otklanjanje poteškoća.  [LVM](virtual-machines-linux-configure-lvm.md) ili [RAID](virtual-machines-linux-configure-raid.md) može se koristiti na diskova podataka ako Preferirani.

- NUMA nije podržana za veće VM zbog pogreške u verzijama otklanjanje Linux ispod 2.6.37. Taj se problem prvenstveno utječe distribucija pomoću na upstream otklanjanje crvena je vaša 2.6.32. Ručna instalacija agent za Azure Linux (waagent) automatski onemogućiti NUMA u konfiguraciji GRUB za otklanjanje Linux. Dodatne informacije o tome pronaći ćete u koracima u nastavku.

- Konfiguriranje zamjena particije na disku OS. Agent za Linux moguće je konfigurirati da biste stvorili datoteku zamjena na disku privremene resursa.  Dodatne informacije o tome pronaći ćete u koracima u nastavku.

- Sve se VHDs mora imati veličine koje su višekratnike od 1 MB.


## <a name="centos-6x"></a>CentOS 6.x ##

1. U upravitelju Hyper-V odaberite virtualnog računala.

2. Kliknite **Poveži** da biste otvorili prozor konzole za virtualnog računala.

3. Deinstalacija NetworkManager ponovnim pokretanjem sljedeće naredbe:

        # sudo rpm -e --nodeps NetworkManager

    **Bilješke:** Ako je paket već nije instaliran, ta naredba neće uspjeti pojavljuje poruka o pogrešci. To je za očekivati.

4.  Stvaranje datoteku pod nazivom **mreže** na `/etc/sysconfig/` direktorij koji sadrži sljedeći tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Stvaranje datoteku pod nazivom **ifcfg eth0** u na `/etc/sysconfig/network-scripts/` direktorij koji sadrži sljedeći tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Izmjena udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet interface(s). Ta pravila može uzrokovati probleme kada kloniranje virtualnog računala web-mjesto Microsoft Azure ili u okvir za Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules


7. Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # sudo chkconfig network on


8. **Samo centOS 6,3**: instalirajte upravljačke programe za na Linux integraciju servisa (LIS).

    **Važno: Koraka je samo vrijedi za CentOS 6,3 i noviji.**  *U CentOS 6,4 + servisima za integraciju Linux već dostupne*su u standardni otklanjanje.

    - Slijedite upute za instalaciju na [stranici za preuzimanje LIS](https://www.microsoft.com/en-us/download/details.aspx?id=46842) i instalirajte RPM na sliku.  


9. Da biste instalirali paket python pyasn1 ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install python-pyasn1

10. Ako želite koristiti kopije OpenLogic koji se nalaze unutar Azure podatkovnim centrima, zatim zamijenite /etc/yum.repos.d/CentOS-Base.repo datoteku sljedećim spremištima.  Ovo će dodati spremište **[openlogic]** koja obuhvaća paketa za Azure Linux agent:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Bilješke:** Do kraja ovog vodiča će da koristite najmanje repo [openlogic] koji će se koristiti da biste instalirali ispod agent za Azure Linux.


11. Dodajte sljedeći redak /etc/yum.conf:

        http_caching=packages

    I **na CentOS 6,3 samo** dodati sljedeći redak:

        exclude=kernel*

12. Onemogućivanje modul yum "fastestmirror" tako da uredite datoteku "/ etc/yum/pluginconf.d/fastestmirror.conf", a zatim u odjeljku `[main]`, upišite sljedeće:

        set enabled=0

13. Pokrenite sljedeću naredbu da biste očistili trenutne metapodatke yum:

        # yum clean all

14. **Na CentOS 6,3 samo**ažuriranje otklanjanje pomoću sljedeće naredbe:

        # sudo yum --disableexcludes=all install kernel

15. Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite "/ boot/grub/menu.lst" u uređivaču teksta i bili sigurni da otklanjanje zadani sadrži sljedećih parametara:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. To će onemogućiti NUMA zbog pogreške u verziji otklanjanje koristi CentOS 6.

    Osim navedenog, preporučuje se da biste *uklonili* sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.

    Na `crashkernel` mogućnost može biti lijevo konfiguriran po želji, ali bilješke da taj parametar se skraćuje memorije u VM po 128MB ili više njih, što može biti problematična na manje VM.


16. Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja.  To je obično zadano.

17. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install WALinuxAgent

    Imajte na umu da instalacije paketa WALinuxAgent uklonit će se NetworkManager i NetworkManager gnome paketa ako oni nisu već uklonjeni kao što je opisano u koraku 2.

18. Stvaranje zamjena prostora na disku OS.

    Agent za Azure Linux automatski možete konfigurirati zamjena prostor pomoću diska lokalnog resursa koja je priložena u VM nakon dodjele resursa na Azure. Imajte na umu *privremene* diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon instalacije Azure Linux Agent (pogledajte prethodni korak), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Kliknite **Akcije -> isključi prema dolje** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Promjene u CentOS 7 (i slično Izvedenice)**

Priprema CentOS 7 virtualnog računala za Azure vrlo je sličan CentOS 6, no postoji nekoliko važnih razlika vrijednosti sudjelovanje:

 - Paket NetworkManager više nisu u sukobu s agent za Azure Linux. Prema zadanim postavkama nije instaliran paket, a preporučujemo da se ne uklanja.
 - GRUB2 sada se koristi kao učitavač pokretanja zadani tako da je postupak za uređivanje parametara otklanjanje promijenio (potražite u nastavku).
 - XFS sada je zadani datotečni sustav. Datotečni sustav ext4 mogu i dalje se po želji.


**Navedeni koraci za konfiguraciju**

1. U upravitelju Hyper-V odaberite virtualnog računala.

2. Kliknite **Poveži** da biste otvorili prozor konzole za virtualnog računala.

3.  Stvaranje datoteku pod nazivom **mreže** na `/etc/sysconfig/` direktorij koji sadrži sljedeći tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Stvaranje datoteku pod nazivom **ifcfg eth0** u na `/etc/sysconfig/network-scripts/` direktorij koji sadrži sljedeći tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Izmjena udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet interface(s). Ta pravila može uzrokovati probleme kada kloniranje virtualnog računala web-mjesto Microsoft Azure ili u okvir za Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # sudo chkconfig network on

7. Da biste instalirali paket python pyasn1 ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install python-pyasn1

8. Ako želite koristiti kopije OpenLogic koji se nalaze unutar Azure podatkovnim centrima, zatim zamijenite /etc/yum.repos.d/CentOS-Base.repo datoteku sljedećim spremištima.  Ovo će dodati spremište **[openlogic]** koja obuhvaća paketa za Azure Linux agent:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Bilješke:** Do kraja ovog vodiča će da koristite najmanje repo [openlogic] koji će se koristiti da biste instalirali ispod agent za Azure Linux.

9.  Pokrenite sljedeću naredbu Očisti trenutne metapodatke yum i instalirati ažuriranja:

        # sudo yum clean all
        # sudo yum -y update

10. Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvaranje "/ zadani/itd/grub" u uređivaču teksta i uređivanje u `GRUB_CMDLINE_LINUX` parametar, primjerice:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Također isključuju se novi konvencije imenovanja CentOS 7 za NIC-ovi. Osim navedenog, preporučuje se da biste *uklonili* sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.

    Na `crashkernel` mogućnost može biti lijevo konfiguriran po želji, ali bilješke da taj parametar se skraćuje memorije u VM po 128MB ili više njih, što može biti problematična na manje VM.

11. Kad završite uređivanje "/ zadani/Dr/grub" po iznad, pokrenite sljedeću naredbu ponovno stvaranje konfiguracije grub:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Provjerite je li poslužitelj SSH instalirati i konfigurirati da biste pokrenuli prilikom pokretanja.  To je obično zadano.

13. **Samo ako je stvaranje sliku iz VMWare, VirtualBox ili KVM:** Dodavanje modula Hyper-V u initramfs:

    Uređivanje `/etc/dracut.conf`, dodati sadržaj:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Ponovno stvaranje na initramfs:

        # dracut –f -v

14. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Stvaranje zamjena prostora na disku OS.

    Agent za Azure Linux automatski možete konfigurirati zamjena prostor pomoću diska lokalnog resursa koja je priložena u VM nakon dodjele resursa na Azure. Imajte na umu *privremene* diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon instalacije Azure Linux Agent (pogledajte prethodni korak), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Kliknite **Akcije -> isključi prema dolje** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.

## <a name="next-steps"></a>Daljnji koraci
Sada ste spremni za korištenje CentOS Linux virtualne tvrdi disk da biste stvorili novi virtualnim strojevima u Azure. Ako je ovo prvi put da prenosite datoteke .vhd za Azure, pogledajte korake 2 i 3 u [Stvaranje i prijenos virtualne tvrdi disk koji sadrži operacijski sustav Linux](virtual-machines-linux-classic-create-upload-vhd.md).
