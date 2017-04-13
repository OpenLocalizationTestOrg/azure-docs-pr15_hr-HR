<properties
pageTitle="Priprema za Oracle Linux virtualnog računala za Azure | Microsoft Azure"
description="Korak po korak konfiguracije računala virtualne Oracle koja se izvodi Linux u Microsoft Azure."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Priprema za Oracle Linux virtualnog računala za Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Priprema za Oracle Linux 6,4 + virtualnog računala za Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Priprema za Oracle Linux 7.0 + virtualnog računala za Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da ste već instalirali operacijski sustav tvrtke Oracle Linux virtualne tvrdi disk. Postoji više alata za stvaranje .vhd datoteka, primjerice rješenja virtualizacije kao što su Hyper-V. Upute potražite u članku [Instalacija Hyper-V i stvaranje virtualnog računala](http://technet.microsoft.com/library/hh846766.aspx).

**Napomene o instalaciji Linux Oracle**

- Web-mjesto tvrtke Oracle, crveno je vaša kompatibilne Jezgra i u okvir za njegov UEK3 (Otklanjanje Unbreakable Enterprise) i podržani su na Hyper-V i Azure. Da biste postigli najbolje rezultate, ne zaboravite ažurirati na najnovije otklanjanje prilikom pripreme vaše VHD Oracle Linux.

- UEK2 Oracle korisnika nije podržan na Hyper-V i Azure kao što je uključiti potrebne upravljačke programe.

- U noviji oblik VHDX nije podržan u Azure. Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet Pretvori vhd.

- Kada instalirate sustav Linux, preporučujemo da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk OS ikada potrebno priloženi drugi VM za otklanjanje poteškoća. LVM ili [RAID](virtual-machines-linux-configure-raid.md) može se koristiti na diskova podataka ako Preferirani.

- NUMA nije podržana za veće VM zbog pogreške u verzijama otklanjanje Linux ispod 2.6.37. Taj se problem prvenstveno utječe distribucija koje koriste u upstream otklanjanje crvena je vaša 2.6.32. Ručna instalacija agent za Azure Linux (waagent) automatski onemogućiti NUMA u konfiguraciji GRUB za otklanjanje Linux. Dodatne informacije o tome pronaći ćete u koracima u nastavku.

- Konfiguriranje zamjena particije na disku OS. Agent za Linux moguće je konfigurirati da biste stvorili datoteku zamjena na disku privremene resursa. Dodatne informacije o tome pronaći ćete u koracima u nastavku.

- Sve se VHDs mora imati veličine koje su višekratnike od 1 MB.

- Provjerite je li u `Addons` spremište omogućena. Odaberite da biste uredili datoteku `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) ili `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) i taj redak promijenite `enabled=0` da biste `enabled=1` u odjeljku **[ol6_addons]** i **[ol7_addons]** u ovoj datoteci.


## <a name="oracle-linux-64"></a>Oracle Linux 6,4 +
Morate poduzeti korake za određenu konfiguraciju u operacijskom sustavu za virtualnog računala da biste pokrenuli u Azure.

1. U oknu centra za Upravitelj Hyper-V odaberite virtualnog računala.

2. Kliknite **Poveži** da biste otvorili prozor za virtualnog računala.

3. Deinstalacija NetworkManager ponovnim pokretanjem sljedeće naredbe:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Ako je paket već nije instaliran, ta naredba neće uspjeti pojavljuje poruka o pogrešci. To je za očekivati.

4. Stvorite datoteku pod nazivom **mreže** u /etc/sysconfig/direktorij koji sadrži sljedeći tekst:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Stvaranje datoteku pod nazivom **ifcfg eth0** u na /etc/sysconfig/network-scripts / direktorij koji sadrži sljedeći tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Premještanje (ili uklanjanje) udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet sučelja. Ta pravila uzrokovati probleme kada ste kloniranje virtualnog računala u Azure ili Hyper-V:

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # chkconfig network on

8.  Instalirajte python pyasn1 ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install python-pyasn1

9.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite "/ boot/grub/menu.lst" u uređivaču teksta i bili sigurni da otklanjanje zadani sadrži sljedećih parametara:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Pogreška u Oracle, crveno je vaša kompatibilne otklanjanje to će onemogućiti NUMA.

    Osim navedenog, preporučujemo da *uklonite* sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.

    Na `crashkernel` mogućnost može biti lijevo konfiguriran po želji, ali bilješke da taj parametar smanjite količinu memorije u VM po 128 MB ili više njih. To može biti problematična na manje VM.

10.  Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja. To je obično zadano.

11.  Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # <a name="sudo-yum-install-walinuxagent"></a>Instalacija yum sudo WALinuxAgent

    Imajte na umu da instalacije paketa WALinuxAgent uklonit će se NetworkManager i NetworkManager gnome paketa ako oni nisu već uklonjeni kao što je opisano u koraku 2.

12.  Stvaranje zamjena prostora na disku OS.

    Agent za Azure Linux automatski možete konfigurirati zamjena prostora putem lokalnog resursa disk koji je pridružen na VM nakon dodjele resursa na Azure. Imajte na umu da lokalnog resursa disk je *privremeni* disk, a je možda ispraznili kada se VM se uklanjaju resursi. Nakon što instalirate Azure Linux Agent (pogledajte prethodni korak), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Napomena: to postavljen na sve što vam je potrebna da bi bio.

13.  Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent – prisilno - deprovision
        # <a name="export-histsize0"></a>Izvoz HISTSIZE = 0
        # <a name="logout"></a>Odjavite

14.  Kliknite **Akcija -\> isključi** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.

## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Promjene u Oracle Linux 7**

Priprema za Oracle Linux 7 virtualnog računala za Azure vrlo je sličan je postupak za Oracle Linux 6. Međutim, postoji nekoliko važnih razlika vrijednosti sudjelovanje:

-   Otklanjanje kompatibilan je vaša crvene boje i UEK3 Oracle, podržani su u Azure. Preporučujemo da se otklanjanje UEK3.

-   Paket NetworkManager više nisu u sukobu s agent za Azure Linux. Prema zadanim postavkama nije instaliran paket pa preporučujemo da se ne uklanja.

-   GRUB2 sada se koristi kao učitavač pokretanja zadani tako da je postupak za uređivanje parametara otklanjanje promijenio (potražite u nastavku).

-   XFS sada je zadani datotečni sustav. Datotečni sustav ext4 mogu i dalje se po želji.

**Navedeni koraci za konfiguraciju**

1.  U upravitelju Hyper-V odaberite virtualnog računala.

2.  Kliknite **Poveži** da biste otvorili prozor konzole za virtualnog računala.

3.  Stvorite datoteku pod nazivom **mreže** u /etc/sysconfig/direktorij koji sadrži sljedeći tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Stvaranje datoteku pod nazivom **ifcfg eth0** u na /etc/sysconfig/network-scripts / direktorij koji sadrži sljedeći tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Premještanje (ili uklanjanje) udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet sučelja. Ta pravila uzrokovati probleme kada ste kloniranje virtualnog računala web-mjesto Microsoft Azure ili u okvir za Hyper-V.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # sudo chkconfig network on

7.  Da biste instalirali paket python pyasn1 ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install python-pyasn1

8.  Pokrenite sljedeću naredbu Očisti trenutne metapodatke yum i instalirati ažuriranja:

        # sudo yum clean all
        # sudo yum -y update

9.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvaranje "/ zadani/itd/grub" u uređivaču teksta i uređivanje u GRUB\_CMDLINE\_LINUX parametar, primjerice:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Osim navedenog, preporučujemo da *uklonite* sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.

    Na `crashkernel` mogućnost može biti lijevo konfiguriran po želji, ali bilješke da taj parametar smanjite količinu memorije u VM po 128 MB ili više njih. To može biti problematična na manje VM.

10.  Kad završite uređivanje "/ zadani/itd/grub", pokrenite sljedeću naredbu ponovno stvaranje konfiguracije grub:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2 mkconfig - o /boot/grub2/grub.cfg

11.  Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja. To je obično zadano.

12.  Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # <a name="sudo-yum-install-walinuxagent"></a>Instalacija yum sudo WALinuxAgent

13.  Stvaranje zamjena prostora na disku OS.

    Agent za Azure Linux automatski možete konfigurirati zamjena prostora putem lokalnog resursa disk koji je pridružen na VM nakon dodjele resursa na Azure. Imajte na umu da lokalnog resursa disk je *privremeni* disk, a je možda ispraznili kada se VM se uklanjaju resursi. Nakon što instalirate Azure Linux Agent (pogledajte prethodni korak), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Napomena: to postavljen na sve što vam je potrebna da bi bio.

14.  Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent – prisilno - deprovision
        # <a name="export-histsize0"></a>Izvoz HISTSIZE = 0
        # <a name="logout"></a>Odjavite

15.  Kliknite **Akcija -\> isključi** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.
