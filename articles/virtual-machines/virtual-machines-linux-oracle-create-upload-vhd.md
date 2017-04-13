<properties
    pageTitle="Stvaranje i prijenos programa Oracle Linux VHD | Microsoft Azure"
    description="Saznajte kako stvoriti i prenijeti na Azure virtualne tvrdi disk (VHD) koji sadrži operacijski sustav tvrtke Oracle Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Priprema za Oracle Linux virtualnog računala za Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Preduvjeti ##

U ovom se članku pretpostavlja da ste već instalirali operacijski sustav tvrtke Oracle Linux virtualne tvrdi disk. Postoji više alata za stvaranje .vhd datoteka, primjerice rješenja virtualizacije kao što su Hyper-V. Upute potražite u članku [Instalacija Hyper-V uloge i konfiguracija virtualnog računala](http://technet.microsoft.com/library/hh846766.aspx).


### <a name="oracle-linux-installation-notes"></a>Napomene o instalaciji Linux Oracle

- Pogledajte i [Općenito Linux napomene o instalaciji](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) dodatne savjete za Azure Priprema Linux.

- Web-mjesto Oracle, crveno je vaša kompatibilne Jezgra i u okvir za svoje UEK3 (Otklanjanje Unbreakable Enterprise) i podržani su na Hyper-V i Azure. Da biste postigli najbolje rezultate, svakako prilikom pripreme vašeg Oracle Linux VHD može ažurirati na najnovije otklanjanje.

- UEK2 Oracle korisnika nije podržan na Hyper-V i Azure kao što je uključiti potrebne upravljačke programe.

- Oblik VHDX nije podržan u Azure, samo **Fiksno VHD**.  Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet Pretvori vhd.

- Prilikom instalacije sustava Linux preporučuje se da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk OS ikada potrebno priloženi drugi VM za otklanjanje poteškoća. [LVM](virtual-machines-linux-configure-lvm.md) ili [RAID](virtual-machines-linux-configure-raid.md) može se koristiti na diskova podataka ako Preferirani.

- NUMA nije podržana za veće VM zbog pogreške u verzijama otklanjanje Linux ispod 2.6.37. Taj se problem prvenstveno utječe distribucija pomoću na upstream otklanjanje crvena je vaša 2.6.32. Ručna instalacija agent za Azure Linux (waagent) automatski onemogućiti NUMA u konfiguraciji GRUB za otklanjanje Linux. Dodatne informacije o tome pronaći ćete u koracima u nastavku.

- Konfiguriranje zamjena particije na disku OS. Agent za Linux moguće je konfigurirati da biste stvorili datoteku zamjena na disku privremene resursa.  Dodatne informacije o tome pronaći ćete u koracima u nastavku.

- Sve se VHDs mora imati veličine koje su višekratnike od 1 MB.

- Provjerite je li u `Addons` spremište omogućena. Uređivanje datoteke `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) ili `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) i taj redak promijenite `enabled=0` da biste `enabled=1` u odjeljku **[ol6_addons]** i **[ol7_addons]** u ovoj datoteci.


## <a name="oracle-linux-64"></a>Oracle Linux 6,4 + ##

Morate poduzeti korake za određenu konfiguraciju u operacijskom sustavu za virtualnog računala da biste pokrenuli u Azure.

1. U oknu centra za Upravitelj Hyper-V odaberite virtualnog računala.

2. Kliknite **Poveži** da biste otvorili prozor za virtualnog računala.

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

        # chkconfig network on

8. Instalirajte python pyasn1 ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install python-pyasn1

9.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste učinili ovaj Otvori "/ boot/grub/menu.lst" u uređivaču teksta i bili sigurni da otklanjanje zadani sadrži sljedećih parametara:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Pogreška u Oracle, crveno je vaša kompatibilne otklanjanje to će onemogućiti NUMA.

    Osim navedenog, preporučuje se da biste *uklonili* sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.

    Na `crashkernel` mogućnost može biti lijevo konfiguriran po želji, ali bilješke da taj parametar se skraćuje memorije u VM po 128MB ili više njih, što može biti problematična na manje VM.


10. Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja.  To je obično zadano.

11. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe. Najnovija verzija je 2.0.15.

        # sudo yum install WALinuxAgent

    Imajte na umu da instalacije paketa WALinuxAgent uklonit će se NetworkManager i NetworkManager gnome paketa ako oni nisu već uklonjeni kao što je opisano u koraku 2.

12. Stvaranje zamjena prostora na disku OS.

    Agent za Azure Linux automatski možete konfigurirati zamjena prostor pomoću diska lokalnog resursa koja je priložena u VM nakon dodjele resursa na Azure. Imajte na umu *privremene* diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon instalacije Azure Linux Agent (pogledajte prethodni korak), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Kliknite **Akcije -> isključi prema dolje** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.


----------


## <a name="oracle-linux-70"></a>Oracle Linux 7.0 + ##

**Promjene u Oracle Linux 7**

Priprema za Oracle Linux 7 virtualnog računala za Azure vrlo je sličan 6 Linux Oracle, no postoji nekoliko važnih razlika vrijednosti sudjelovanje:

 - Otklanjanje kompatibilan je vaša crvene boje i UEK3 Oracle, podržani su u Azure.  Preporučuje se otklanjanje UEK3.
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

5.  Izmjena udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet interface(s). Ta pravila može uzrokovati probleme kada kloniranje virtualnog računala web-mjesto Microsoft Azure ili u okvir za Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # sudo chkconfig network on

7. Da biste instalirali paket python pyasn1 ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install python-pyasn1

8.  Pokrenite sljedeću naredbu Očisti trenutne metapodatke yum i instalirati ažuriranja:

        # sudo yum clean all
        # sudo yum -y update

9.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili Otvori "/ zadani/itd/grub" u uređivaču teksta i uređivanje u `GRUB_CMDLINE_LINUX` parametar, primjerice:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Također isključuju se novi konvencije imenovanja OEL 7 za NIC-ovi. Osim navedenog, preporučuje se da biste *uklonili* sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.

    Na `crashkernel` mogućnost može biti lijevo konfiguriran po želji, ali bilješke da taj parametar se skraćuje memorije u VM po 128MB ili više njih, što može biti problematična na manje VM.


10. Kad završite uređivanje "/ zadani/itd/grub" po iznad, pokrenite sljedeću naredbu ponovno stvaranje konfiguracije grub:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja.  To je obično zadano.

12. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Stvaranje zamjena prostora na disku OS.

    Agent za Azure Linux automatski možete konfigurirati zamjena prostor pomoću diska lokalnog resursa koja je priložena u VM nakon dodjele resursa na Azure. Imajte na umu *privremeni* diska lokalnog resursa disk, a možda Isprazni prilikom na VM se uklanjaju resursi. Nakon instalacije Azure Linux Agent (pogledajte u prethodnom koraku), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Kliknite **Akcije -> isključi prema dolje** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.


## <a name="next-steps"></a>Daljnji koraci
Sada ste spremni za korištenje vašeg Oracle Linux .vhd da biste stvorili novi virtualnim strojevima u Azure. Ako je ovo prvi put da prenosite datoteke .vhd za Azure, pogledajte korake 2 i 3 u [Stvaranje i prijenos virtualne tvrdi disk koji sadrži operacijski sustav Linux](virtual-machines-linux-classic-create-upload-vhd.md).
