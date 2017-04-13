<properties
    pageTitle="Stvoriti i prenijeti na crveno je vaša Enterprise Linux VHD za korištenje u Azure"
    description="Saznajte kako stvoriti i prenijeti na Azure virtualne tvrdi disk (VHD) koji sadrži crveno je vaša Linux operacijski sustav."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Priprema virtualnog računala s crvenim razgovor za Azure

U ovom se članku će Saznajte kako pripremiti crveno je vaša Enterprise Linux (RHEL) virtualnog računala za korištenje u Azure. Verzije RHEL koje se primjenjuju u ovom članku su 6,7, 7.1 i 7,2. Hypervisors za pripremu koje se primjenjuju u ovom članku su Hyper-V, koji se temelji na otklanjanje virtualnog računala (KVM) i VMware. Dodatne informacije o uvjete potrebno ispuniti za sudjelovanje u programu pristupa putem oblaka je crveno vaša potražite u članku [je crveno vaša pristupa putem oblaka web-mjesta](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) i [Pokretanje RHEL na Azure](https://access.redhat.com/articles/1989673).

[Priprema RHEL 6,7 virtualnog računala od upravitelja Hyper-V](#rhel67hyperv)

[Priprema 7.1 7,2 RHEL virtualnog računala od upravitelja Hyper-V](#rhel7xhyperv)

[Priprema RHEL 6,7 virtualnog računala s KVM](#rhel67kvm)

[Priprema 7.1 7,2 RHEL virtualnog računala s KVM](#rhel7xkvm)

[Priprema RHEL 6,7 virtualnog računala s VMware](#rhel67vmware)

[Priprema 7.1 7,2 RHEL virtualnog računala s VMware](#rhel7xvmware)

[Priprema 7.1 7,2 RHEL virtualnog računala iz datoteke kickstart](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Priprema virtualnog računala s crvenim razgovor od upravitelja Hyper-V
### <a name="prerequisites"></a>Preduvjeti
U ovom se odjeljku pretpostavlja da ste već instalirali RHEL slike (iz ISO datoteke koju ste nabavili je crveno vaša web-mjesta) na virtualni tvrdi disk (VHD). Dodatne informacije o korištenju upravitelja Hyper-V tako da biste instalirali operacijski sustav slike potražite u članku [Instalacija Hyper-V uloge i konfiguracija virtualnog računala](http://technet.microsoft.com/library/hh846766.aspx).

**Napomene o instalaciji RHEL**

- Pogledajte i [Općenito Linux napomene o instalaciji](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) dodatne savjete za Azure Priprema Linux.

- U noviji oblik VHDX nije podržan u Azure. Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet ljuske PowerShell **Pretvori vhd** .

- VHDs moraju se stvoriti kao "fiksno" – dinamične VHDs nisu podržane.

- Kada instalirate sustav Linux, preporučujemo da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk s operacijskim Sustavom ikada potrebno priloženi drugi VM za otklanjanje poteškoća. LVM ili [RAID](virtual-machines-linux-configure-raid.md) može se koristiti na diskova podataka ako Preferirani.

- Konfiguriranje zamjena particije na disku OS. Možete konfigurirati agent Linux da biste stvorili datoteku zamjena na disku privremene resursa. Dodatne informacije o tome je dostupna u koracima u nastavku.

- Sve se VHDs mora imati veličine koje su višekratnike od 1 MB.

- Kada koristite **qemu img** da biste pretvorili slike na disku VHD oblik, primijetit ćete da je poznatih problema u verzijama qemu img 2.2.1 ili noviji. Ovu pogrešku rezultira nepravilno oblikovani VHD. Problem namijenjen je ispraviti u budućem izdanju programa qemu img. Zasad, preporučujemo da koristite qemu img verziju 2.2.0 ili neke starije verzije.

### <a id="rhel67hyperv"> </a>Priprema RHEL 6,7 virtualnog računala od upravitelja Hyper-V###


1.  U upravitelju Hyper-V odaberite virtualnog računala.

2.  Kliknite **Poveži** da biste otvorili prozor konzole za virtualnog računala.

3.  Deinstalacija NetworkManager ponovnim pokretanjem sljedeće naredbe:

        # sudo rpm -e --nodeps NetworkManager

    Napomena Ako je paket već nije instaliran, ta naredba neće uspjeti pojavljuje poruka o pogrešci. To je za očekivati.

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

6.  Premještanje (ili uklanjanje) udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet sučelja. Ta pravila uzrokovati probleme kada Kloniraj virtualnog računala web-mjesto Microsoft Azure ili u okvir za Hyper V:

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # sudo chkconfig network on

8.  Registrirajte se pretplate crveno razgovor da biste omogućili instalaciju paketa u spremištu RHEL ponovnim pokretanjem sljedeće naredbe:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Paket WALinuxAgent `WALinuxAgent-<version>` Završi dodataka spremište je vaša crvene boje. Omogući dodatke spremište ponovnim pokretanjem sljedeće naredbe:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite `/boot/grub/menu.lst` u uređivaču teksta i bili sigurni da otklanjanje zadani sadrži sljedećih parametara:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. To će onemogućiti NUMA zbog pogreške u verziji otklanjanje koji koriste RHEL 6.

    Osim gornje akciju, preporučujemo da uklonite sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.

    Mogućnost crashkernel može biti lijevo konfiguriran po želji, ali bilješke da taj parametar smanjite količinu memorije u VM po 128 MB ili više njih. To može biti problematičnog na manje VM.

11. Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja. To je obično zadano. Izmjena /etc/ssh/sshd_config da biste dodali sljedeći redak:

        ClientAliveInterval 180

12. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Imajte na umu da instalacije paketa WALinuxAgent uklonit će se NetworkManager i NetworkManager gnome paketa ako oni nisu već uklonjeni kao što je opisano u koraku 2.

13. Stvaranje zamjena prostora na disku OS.
Agent za Azure Linux automatski možete konfigurirati zamjena prostora putem lokalnog resursa disk koji je pridružen na VM kada je na VM dodjeli na Azure. Imajte na umu privremene diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon što instalirate Azure Linux Agent (pogledajte u prethodnom koraku), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Unregister pretplate (Ako je potrebno) tako da pokrenete sljedeću naredbu:

        # sudo subscription-manager unregister

15. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Kliknite **Akcije > isključi** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.
 

### <a id="rhel7xhyperv"> </a>Priprema 7.1 7,2 RHEL virtualnog računala od upravitelja Hyper-V###

1.  U upravitelju Hyper-V odaberite virtualnog računala.

2.  Kliknite **Poveži** da biste otvorili prozor konzole za virtualnog računala.

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

5.  Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # sudo chkconfig network on

6.  Registrirajte se pretplate crveno razgovor da biste omogućili instalaciju paketa u spremištu RHEL ponovnim pokretanjem sljedeće naredbe:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite `/etc/default/grub` u uređivaču teksta i uređivanje parametar **GRUB_CMDLINE_LINUX** . Ako, na primjer:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Osim gornje akciju, preporučujemo da uklonite sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.
    Mogućnost crashkernel može biti lijevo konfiguriran po želji, ali bilješke da taj parametar smanjite količinu memorije u VM po 128 MB ili više njih. To može biti problematičnog na manje VM.

8.  Nakon što završite uređivanje `/etc/default/grub`, pokrenite sljedeću naredbu ponovno stvaranje konfiguracije grub:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja. To je obično zadano. Izmjena `/etc/ssh/sshd_config` da biste dodali sljedeći redak:

        ClientAliveInterval 180

10. Paket WALinuxAgent `WALinuxAgent-<version>` Završi dodataka spremište je vaša crvene boje. Omogući dodatke spremište ponovnim pokretanjem sljedeće naredbe:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Stvorite zamjena prostora na disku za OS. Agent za Azure Linux automatski možete konfigurirati zamjena prostora putem lokalnog resursa disk koji je pridružen na VM kada je na VM dodjeli na Azure. Imajte na umu privremene diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon što instalirate Azure Linux Agent (potražite u prethodnom koraku) izmjena sljedećih parametara u `/etc/waagent.conf` komponenta:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Ako želite Odjava pretplatu, pokrenite sljedeću naredbu:

        # sudo subscription-manager unregister

14. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Kliknite **Akcije > isključi** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Priprema virtualnog računala s crvenim razgovor s KVM

### <a id="rhel67kvm"> </a>Priprema RHEL 6,7 virtualnog računala s KVM###


1.  Preuzimanje slika KVM RHEL 6,7 je crveno vaša web-mjestu.

2.  Postavljanje lozinke korijen.

    Generiranje šifriranu lozinku, a zatim kopirajte rezultatu naredbe:

        # openssl passwd -1 changeme

    Postavljanje lozinke korijenski s guestfish:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Promjena drugog polja korijenski korisnika iz "ste!!" Da biste šifriranu lozinku.

3.  Stvaranje virtualnog računala u KVM iz qcow2 slike, postaviti vrstu diska na **qcow2**i postavite model za uređaj sučelja virtualne mreže na **virtio**. Zatim pokrenite virtualnog računala i prijavite se kao korijen.

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

6.  Premještanje (ili uklanjanje) udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet sučelja. Ta pravila uzrokovati probleme kada Kloniraj virtualnog računala web-mjesto Microsoft Azure ili u okvir za Hyper V:

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # chkconfig network on

8.  Registrirajte se pretplate crveno razgovor da biste omogućili instalaciju paketa u spremištu RHEL ponovnim pokretanjem sljedeće naredbe:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite `/boot/grub/menu.lst` u uređivaču teksta i bili sigurni da otklanjanje zadani sadrži sljedećih parametara:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. To će onemogućiti NUMA zbog pogreške u verziji otklanjanje koji koriste RHEL 6.

    Osim gornje akciju, preporučujemo da uklonite sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.
    Mogućnost crashkernel može biti lijevo konfiguriran po želji, ali bilješke da taj parametar smanjite količinu memorije u VM po 128 MB ili više njih. To može biti problematičnog na manje VM.

10. Dodavanje modula Hyper-V u initramfs:  

    Uređivanje `/etc/dracut.conf` i dodavanje sadržaja: add_drivers += "hv_vmbus hv_netvsc hv_storvsc"

    Ponovno stvaranje initramfs: # dracut – f - v

11. Deinstalacija oblaka pokretanja:

        # yum remove cloud-init

12. Provjerite je li poslužitelj SSH instalirati i konfigurirati da biste pokrenuli prilikom pokretanja:

        # chkconfig sshd on

    Izmjena /etc/ssh/sshd_config da biste uključili sljedeće retke:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Ponovno pokrenite sshd:

        # service sshd restart

13. Paket WALinuxAgent `WALinuxAgent-<version>` Završi dodataka spremište je vaša crvene boje. Omogući dodatke spremište ponovnim pokretanjem sljedeće naredbe:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Agent za Azure Linux automatski možete konfigurirati zamjena prostora putem lokalnog resursa disk koji je pridružen na VM kada je na VM dodjeli na Azure. Imajte na umu privremene diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon što instalirate Azure Linux Agent (pogledajte u prethodnom koraku), komponenta izmjena sljedećih parametara u **/etc/waagent.conf** :

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Unregister pretplate (Ako je potrebno) tako da pokrenete sljedeću naredbu:

        # subscription-manager unregister

17. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Isključivanje VM u KVM.

19. Pretvaranje slika qcow2 VHD oblik.
    Najprije pretvorite slike u obliku neobrađenog:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Provjerite je li veličinu neobrađenog slike poravnat s 1 MB. U suprotnom zaokruživanje veličina da biste poravnali s 1 MB: # MB = $((1024*1024)) veličina # = $(qemu img informacije -f sirovim – izlaz json "rhel-6.7.raw" | \ gawk ' odgovaraju (0 kn, / "virtualne veličina": ([0 do 9] +), /, val) {ispis val[1]}') # rounded_size = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    Pretvaranje neobrađenog diska u VHD za nepromjenjive veličine:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Priprema 7.1 7,2 RHEL virtualnog računala s KVM###


1.  Preuzimanje slika KVM RHEL 7.1 (ili 7,2) s crvenim je vaša web-mjesta. Koristit ćemo RHEL 7.1 kao u primjeru u nastavku.

2.  Postavljanje lozinke korijen.

    Generiranje šifriranu lozinku, a zatim kopirajte rezultatu naredbe:

        # openssl passwd -1 changeme

    Postavljanje lozinke korijenski s guestfish.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Promjena drugo polje korijenski korisnika iz "ste!!" Da biste šifriranu lozinku.

3.  Stvaranje virtualnog računala u KVM iz qcow2 slike, postaviti vrstu diska na **qcow2**i postavite model za uređaj sučelja virtualne mreže na **virtio**. Zatim pokrenite virtualnog računala i prijavite se kao korijen.

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

6.  Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # chkconfig network on

7.  Registrirajte se pretplate crveno razgovor da biste omogućili instalaciju paketa u spremištu RHEL ponovnim pokretanjem sljedeće naredbe:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite `/etc/default/grub` u uređivaču teksta i uređivanje parametar **GRUB_CMDLINE_LINUX** . Ako, na primjer:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Osim gornje akciju, preporučujemo da uklonite sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.
    Mogućnost crashkernel može biti lijevo konfiguriran po želji, ali bilješke da taj parametar smanjite količinu memorije u VM po 128 MB ili više njih. To može biti problematičnog na manje VM.

9.  Nakon što završite uređivanje `/etc/default/grub`, pokrenite sljedeću naredbu ponovno stvaranje konfiguracije grub:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Dodajte Hyper-V modula u initramfs:

    Uređivanje `/etc/dracut.conf` i dodavanje sadržaja:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Ponovno stvaranje initramfs:

        # dracut –f -v

11. Deinstalacija oblaka pokretanja:

        # yum remove cloud-init

12. Provjerite je li poslužitelj SSH instalirati i konfigurirati da biste pokrenuli prilikom pokretanja:

        # systemctl enable sshd

    Izmjena /etc/ssh/sshd_config da biste uključili sljedeće retke:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Ponovno pokrenite sshd:

        systemctl restart sshd

13. Paket WALinuxAgent `WALinuxAgent-<version>` Završi dodataka spremište je vaša crvene boje. Omogući dodatke spremište ponovnim pokretanjem sljedeće naredbe:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # yum install WALinuxAgent

    Omogućite servis za waagent:

        # systemctl enable waagent.service

15. Stvaranje zamjena prostora na disku OS. Agent za Azure Linux automatski možete konfigurirati zamjena prostora putem lokalnog resursa disk koji je pridružen na VM kada je na VM dodjeli na Azure. Imajte na umu privremene diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon što instalirate Azure Linux Agent (potražite u prethodnom koraku) izmjena sljedećih parametara u `/etc/waagent.conf` komponenta:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Unregister pretplate (Ako je potrebno) tako da pokrenete sljedeću naredbu:

        # subscription-manager unregister

17. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Isključivanje virtualnog računala u KVM.

19. Pretvaranje slika qcow2 VHD oblik.

    Najprije pretvorite slike u obliku neobrađenog:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Provjerite je li veličinu neobrađenog slike poravnat s 1 MB. U suprotnom zaokruživanje veličina da biste poravnali s 1 MB:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    Pretvaranje neobrađenog diska u VHD za nepromjenjive veličine:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Priprema virtualnog računala s crvenim razgovor s VMware
### <a name="prerequisites"></a>Preduvjeti
U ovom se odjeljku pretpostavlja da ste već instalirali RHEL virtualnog računala u VMware. Dodatne informacije o tome kako instalirati operacijski sustav u VMware potražite u [Vodiču za instalaciju VMware goste operacijski sustav](http://partnerweb.vmware.com/GOSIG/home.html).

- Kada instalirate operacijski sustav Linux, preporučujemo da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk s operacijskim Sustavom ikada potrebno priloženi drugi VM za otklanjanje poteškoća. LVM ili RAID može se koristiti na diskova podataka ako Preferirani.

- Konfiguriranje zamjena particije na disku OS. Možete konfigurirati agent Linux da biste stvorili datoteku zamjena na disku privremene resursa. Dodatne informacije o tome možete pronaći u koracima u nastavku.

- Kada stvorite virtualne na tvrdom disku, odaberite **trgovine virtualne disk u jednu datoteku**.



### <a id="rhel67vmware"> </a>Priprema RHEL 6,7 virtualnog računala s VMware###

1.  Deinstalacija NetworkManager ponovnim pokretanjem sljedeće naredbe:

         # sudo rpm -e --nodeps NetworkManager

    Napomena Ako je paket već nije instaliran, ta naredba neće uspjeti pojavljuje poruka o pogrešci. To je za očekivati.

2.  Stvorite datoteku pod nazivom **mreže** u /etc/sysconfig/direktorij koji sadrži sljedeći tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Stvaranje datoteku pod nazivom **ifcfg eth0** u na /etc/sysconfig/network-scripts / direktorij koji sadrži sljedeći tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  Premještanje (ili uklanjanje) udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet sučelja. Ta pravila uzrokovati probleme kada Kloniraj virtualnog računala web-mjesto Microsoft Azure ili u okvir za Hyper V:

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # sudo chkconfig network on

6.  Registrirajte se pretplate crveno razgovor da biste omogućili instalaciju paketa u spremištu RHEL ponovnim pokretanjem sljedeće naredbe:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Paket WALinuxAgent `WALinuxAgent-<version>` Završi dodataka spremište je vaša crvene boje. Omogući dodatke spremište ponovnim pokretanjem sljedeće naredbe:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite "/ boot/grub/menu.lst" u uređivaču teksta i bili sigurni da otklanjanje zadani sadrži sljedećih parametara:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. To će onemogućiti NUMA zbog pogreške u verziji otklanjanje koji koriste RHEL 6.
    Osim gornje akciju, preporučujemo da uklonite sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.
    Mogućnost crashkernel može biti lijevo konfiguriran po želji, ali bilješke da taj parametar smanjite količinu memorije u VM po 128 MB ili više njih. To može biti problematičnog na manje VM.

9.  Dodavanje modula Hyper-V u initramfs:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja. To je obično zadano. Izmjena `/etc/ssh/sshd_config` da biste dodali sljedeći redak:

        ClientAliveInterval 180

11. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Stvaranje zamjena prostora na disku OS:

    Agent za Azure Linux automatski možete konfigurirati zamjena prostora putem lokalnog resursa disk koji je pridružen na VM kada je na VM dodjeli na Azure. Imajte na umu privremene diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon što instalirate Azure Linux Agent (potražite u prethodnom koraku) izmjena sljedećih parametara u `/etc/waagent.conf` komponenta:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Unregister pretplate (Ako je potrebno) tako da pokrenete sljedeću naredbu:

        # sudo subscription-manager unregister

14. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Isključite na VM i pretvarati datoteku VMDK .vhd datoteku.

    Najprije pretvorite slike u obliku neobrađenog:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Provjerite je li veličinu neobrađenog slike poravnat s 1 MB. U suprotnom zaokruživanje veličina da biste poravnali s 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    Pretvaranje neobrađenog diska u VHD za nepromjenjive veličine:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>Priprema 7.1 7,2 RHEL virtualnog računala s VMware###

1.  Stvorite datoteku pod nazivom **mreže** u /etc/sysconfig/direktorij koji sadrži sljedeći tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Stvaranje datoteku pod nazivom **ifcfg eth0** u na /etc/sysconfig/network-scripts / direktorij koji sadrži sljedeći tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Provjerite je li mrežni servis počet će prilikom pokretanja tako da pokrenete sljedeću naredbu:

        # sudo chkconfig network on

4.  Registrirajte se pretplate crveno razgovor da biste omogućili instalaciju paketa u spremištu RHEL ponovnim pokretanjem sljedeće naredbe:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite `/etc/default/grub` u uređivaču teksta i uređivanje parametar **GRUB_CMDLINE_LINUX** . Ako, na primjer:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Ovo će također osigurati da sve poruke konzole šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Osim gornje akciju, preporučujemo da uklonite sljedećih parametara:

        rhgb quiet crashkernel=auto

    Pokretanje grafički i Tihi nisu korisna u okruženje oblaka gdje želimo zapisnika slati serijskog priključka.
    Mogućnost crashkernel može biti lijevo konfiguriran po želji, ali bilješke da taj parametar smanjite količinu memorije u VM po 128 MB ili više njih. To može biti problematičnog na manje VM.

6.  Nakon što završite uređivanje `/etc/default/grub`, pokrenite sljedeću naredbu ponovno stvaranje konfiguracije grub:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Dodavanje modula Hyper-V u initramfs:

    Uređivanje `/etc/dracut.conf`, dodati sadržaj:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Ponovno stvaranje initramfs:

        # dracut –f -v

8.  Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja. To je obično zadano. Izmjena `/etc/ssh/sshd_config` da biste dodali sljedeći redak:

        ClientAliveInterval 180

9.  Paket WALinuxAgent `WALinuxAgent-<version>` Završi dodataka spremište je vaša crvene boje. Omogući dodatke spremište ponovnim pokretanjem sljedeće naredbe:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Instalirajte Azure Linux Agent ponovnim pokretanjem sljedeće naredbe:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Stvaranje zamjena prostora na disku OS. Agent za Azure Linux automatski možete konfigurirati zamjena prostora putem lokalnog resursa disk koji je pridružen na VM kada je na VM dodjeli na Azure. Imajte na umu privremene diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon što instalirate Azure Linux Agent (potražite u prethodnom koraku) izmjena sljedećih parametara u `/etc/waagent.conf` komponenta:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Ako želite Odjava pretplatu, pokrenite sljedeću naredbu:

        # sudo subscription-manager unregister

13. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Isključite na VM i pretvorite VMDK datoteku u obliku VHD.

    Najprije pretvorite slike u obliku neobrađenog:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Provjerite je li veličinu neobrađenog slike poravnat s 1 MB. U suprotnom zaokruživanje veličina da biste poravnali s 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    Pretvaranje neobrađenog diska u VHD za nepromjenjive veličine:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Priprema virtualnog računala s crvenim razgovor s ISO pomoću datoteke kickstart automatski


### <a id="rhel7xkickstart"> </a>Priprema 7.1 7,2 RHEL virtualnog računala iz datoteke kickstart###


1.  Stvaranje datoteke kickstart sa sadržajem u nastavku, a zatim spremite datoteku. Dodatne informacije o instalaciji kickstart potražite u [Vodiču za instalaciju Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Postavite kickstart datoteku na mjesto na kojem se može pristupiti iz instalacije sustava.

3.  U upravitelju Hyper-V, stvorite novi VM. Na stranici **povezivanje virtualne na tvrdom disku** , odaberite **Priloži virtualne tvrdi disk kasnije**, a dovršavanje čarobnjaka za novi virtualnog računala.

4.  Otvorite postavke VM:

    na.  Prilaganje novi virtualni tvrdi disk na VM. Provjerite je li da biste odabrali **Oblik VHD** i **Fixed veličinu**.

    b.  Instalacija ISO priložiti DVD pogon.

    c.  Postavljanje BIOS za pokretanje s CD-a.

5.  Započnite s VM. Kada se pojavi vodiču za instalaciju, pritisnite tipku **Tab** da biste konfigurirali mogućnosti pokretanja.

6.  ENTER `inst.ks=<the location of the kickstart file>` na kraju mogućnosti pokretanja i pritisnite **Enter**.

7.  Pričekajte da se instalacija završi. Kada završite, na VM će biti zatvoren automatski. Vaš Linux VHD je sada je moguće prenijeti Azure.

## <a name="known-issues"></a>Poznati problemi
Poznati su problemi prilikom korištenja RHEL 7.1 Hyper-V i Azure.

### <a name="disk-io-freeze"></a>Zamrzavanje/i na disku

Taj se problem može pojaviti tijekom često korištenih za pohranu na disku/i aktivnosti s RHEL 7.1 Hyper-V i Azure.   

Brzina REPRO:

Ovaj je problem povremen. Međutim, pojavljuje se više često tijekom čestih disk/i postupke u Hyper-V i Azure.   


[AZURE.NOTE] Poznati problem već odgovorili tako da je vaša crvene boje. Da biste instalirali pridružene rješenja, pokrenite sljedeću naredbu:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Upravljački program za Hyper-V nije moguće dodati u početne RAM disk prilikom korištenja koji nisu-Hyper-V hypervisor

U nekim slučajevima Linux instalaciju može uključiti upravljačke programe za Hyper-V u početne disk RAM-a (initrd ili initramfs) osim ako ih ne otkrije da se izvodi u okruženju Hyper-V.

Kada koristite drugi virtualizacije sustava (odnosno Virtualbox Xen, itd.) da biste pripremili Linux sliku, možda ćete morati ponovno stvaranje initrd osigurava da barem otklanjanje module hv_vmbus i hv_storvsc dostupne su na početni RAM disk. Ovo je poznat problem barem na sustavima koji se temelji na upstream raspodjele je vaša crvene boje.

Da biste riješili taj problem, morate dodati Hyper-V module u initramfs i ponovno ga stvoriti:

Uređivanje `/etc/dracut.conf` i dodavanje sadržaja:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Ponovno stvaranje initramfs:

        # dracut –f -v

Dodatne informacije potražite u članku informacije o [rasponu initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Daljnji koraci
Sada ste spremni za korištenje crveno je vaša Enterprise Linux virtualne tvrdi disk da biste stvorili novi virtualnim strojevima u Azure. Ako je ovo prvi put da prenosite datoteke .vhd za Azure, pogledajte korake 2 i 3 u [Stvaranje i prijenos virtualne tvrdi disk koji sadrži operacijski sustav Linux](virtual-machines-linux-classic-create-upload-vhd.md).

Dodatne informacije o hypervisors koji su certificirani za pokretanje crveno je vaša Enterprise Linux potražite u članku [crveno je vaša web-mjesta](https://access.redhat.com/certified-hypervisors).
