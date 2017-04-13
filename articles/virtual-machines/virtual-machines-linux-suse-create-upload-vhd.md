<properties
    pageTitle="Stvoriti i prenijeti VHD za Linux SUSE servisu Azure"
    description="Saznajte kako stvoriti i prenijeti na Azure virtualne tvrdi disk (VHD) koji sadrži SUSE Linux operacijski sustav."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Priprema SLES ili openSUSE virtualnog računala za Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Preduvjeti ##

U ovom se članku pretpostavlja da ste već instalirali SUSE ili openSUSE Linux operacijski sustav virtualne tvrdi disk. Postoji više alata za stvaranje .vhd datoteka, primjerice rješenje virtualizacije kao što su Hyper-V. Upute potražite u članku [Instalacija Hyper-V uloge i konfiguracija virtualnog računala](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE napomene o instalaciji

- Pogledajte i [Općenito Linux napomene o instalaciji](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) dodatne savjete za Azure Priprema Linux.

- Oblik VHDX nije podržan u Azure, samo **nepromjenjive VHD**.  Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili cmdlet Pretvori vhd.

- Prilikom instalacije sustava Linux preporučuje se da koristite standardne particije umjesto LVM (često zadano za mnoge instalacije). To izbjeći LVM naziv u sukobu s kloniranu VMs, osobito ako je na disk OS ikada potrebno priloženi drugi VM za otklanjanje poteškoća. [LVM](virtual-machines-linux-configure-lvm.md) ili [RAID](virtual-machines-linux-configure-raid.md) može se koristiti na diskova podataka ako Preferirani.

- Konfiguriranje zamjena particije na disku OS. Agent za Linux moguće je konfigurirati da biste stvorili datoteku zamjena na disku privremene resursa.  Dodatne informacije o tome pronaći ćete u koracima u nastavku.

- Sve se VHDs mora imati veličine koje su višekratnike od 1 MB.


## <a name="use-suse-studio"></a>Koristite SUSE Studio
[SUSE Studio](http://www.susestudio.com) možete jednostavno stvaranje i upravljanje SLES i openSUSE slike za Azure i Hyper-V. To je preporučeni način za prilagodbu vlastite slike SLES i openSUSE.

Kao zamjena za stvaranje vlastitog VHD, SUSE i objavljuje slike BYOS (Premjesti vaše vlastite pretplatu) za SLES pri [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Priprema SUSE Linux Enterprise Server 11 SP4 ##

1. U oknu centra za Upravitelj Hyper-V odaberite virtualnog računala.

2. Kliknite **Poveži** da biste otvorili prozor za virtualnog računala.

3. Registrirajte se dopustiti preuzimanje ažuriranja i instalaciju paketa sustava SUSE Linux Enterprise.

4. Ažuriranje sustava s najnovijim zakrpa:

        # sudo zypper update

5. Instalirajte Azure Linux Agent u spremištu SLES:

        # sudo zypper install WALinuxAgent

6. Prijava ako waagent je postavljeno na "u" chkconfig, a ako ne omogućite ga za automatsko pokrenite:
               
        # sudo chkconfig waagent on

7. Provjeri je li servis waagent je pokrenut i ako nije, započnite je: 

        # sudo service waagent start
                
8. Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste učinili ovaj Otvori "/ boot/grub/menu.lst" u uređivaču teksta i bili sigurni da otklanjanje zadani sadrži sljedećih parametara:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    To će osigurati sve konzole poruke šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema.

9. Provjerite da /boot/grub/menu.lst i /etc/fstab i referencu na disku pomoću UUID (prema-uuid) umjesto disk ID-a (po id). 

    Početak disk UUID
    
        # ls /dev/disk/by-uuid/

    Ako /dev/disk/by-id / se koristi, ažurirajte /boot/grub/menu.lst i/Dr/fstab proper po – uuid vrijednost

    Prije promjene
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Nakon svake promjene
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Izmjena udev pravila da bi se izbjeglo generiranje statične pravila za Ethernet interface(s). Ta pravila može uzrokovati probleme kada kloniranje virtualnog računala web-mjesto Microsoft Azure ili u okvir za Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Se preporučuje da biste uredili datoteku "/ itd/sysconfig/mreže/dhcp" i promjena na `DHCLIENT_SET_HOSTNAME` parametar na sljedeći način:

        DHCLIENT_SET_HOSTNAME="no"

12. "/ Itd/sudoers", komentar izgleda ili ukloniti sljedeće retke ako postoje:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja.  To je obično zadano.

14. Stvaranje zamjena prostora na disku OS.

    Agent za Azure Linux automatski možete konfigurirati zamjena prostor pomoću diska lokalnog resursa koja je priložena u VM nakon dodjele resursa na Azure. Imajte na umu *privremene* diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon instalacije Azure Linux Agent (pogledajte prethodni korak), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Kliknite **Akcije -> isključi prema dolje** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.


----------

## <a name="prepare-opensuse-131"></a>Priprema openSUSE 13,1 + ##

1. U oknu centra za Upravitelj Hyper-V odaberite virtualnog računala.

2. Kliknite **Poveži** da biste otvorili prozor za virtualnog računala.

3. Na ljuske, pokrenite naredbu "`zypper lr`". Ako ta naredba Vrati izlaz slično sljedećem, a zatim na spremištima konfigurirane prema očekivanjima – su potrebna bez prilagođavanja (Imajte na umu da se mogu razlikovati brojevi verzija):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Ako je naredba vraća "Nema spremištima definirani..." da biste dodali te repos koristiti sljedeće naredbe:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Zatim možete provjeriti tako da pokrenete naredbu su dodane u spremištima "`zypper lr`" ponovno. U slučaju da jedan od spremištima odgovarajući ažuriranje nije omogućena, omogućite pomoću sljedeće naredbe:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. Ažuriranje Jezgra na najnovije verzije sustava:

        # sudo zypper up kernel-default

    Ili da biste ažurirali sustav s najnovijim zakrpa:

        # sudo zypper update

5.  Instalirajte Azure Linux Agent.

        # sudo zypper install WALinuxAgent

6.  Izmjena redak za pokretanje otklanjanje u konfiguraciji grub da biste dodali dodatne otklanjanje parametara za Azure. Da biste to učinili, otvorite "/ boot/grub/menu.lst" u uređivaču teksta i bili sigurni da otklanjanje zadani sadrži sljedećih parametara:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    To će osigurati sve konzole poruke šalju prvi serijski priključak, koji vam mogu pomoći Azure podrška za ispravljanje pogrešaka problema. Osim toga, uklonite sljedećih parametara iz redak za pokretanje otklanjanje ako postoje:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Se preporučuje da biste uredili datoteku "/ itd/sysconfig/mreže/dhcp" i promjena na `DHCLIENT_SET_HOSTNAME` parametar na sljedeći način:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Važne:** "/ Itd/sudoers", komentar izgleda ili ukloniti sljedeće retke ako postoje:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Provjerite je li poslužitelj SSH instaliran i konfiguriran za pokretanje prilikom pokretanja.  To je obično zadano.

10. Stvaranje zamjena prostora na disku OS.

    Agent za Azure Linux automatski možete konfigurirati zamjena prostor pomoću diska lokalnog resursa koja je priložena u VM nakon dodjele resursa na Azure. Imajte na umu *privremene* diska lokalnog resursa disk, a možda ispraznili kada se VM se uklanjaju resursi. Nakon instalacije Azure Linux Agent (pogledajte prethodni korak), komponenta izmjena sljedećih parametara u /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Pokrenite sljedeće naredbe deprovision virtualnog računala i Priprema za dodjelu resursa na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Provjerite je li Azure Linux Agent izvodi prilikom pokretanja:

        # sudo systemctl enable waagent.service

13. Kliknite **Akcije -> isključi prema dolje** u upravitelju Hyper-V. Vaš Linux VHD je sada je moguće prenijeti Azure.

## <a name="next-steps"></a>Daljnji koraci
Sada ste spremni za korištenje SUSE Linux virtualne tvrdi disk da biste stvorili novi virtualnim strojevima u Azure. Ako je ovo prvi put da prenosite datoteke .vhd za Azure, pogledajte korake 2 i 3 u [Stvaranje i prijenos virtualne tvrdi disk koji sadrži operacijski sustav Linux](virtual-machines-linux-classic-create-upload-vhd.md).
