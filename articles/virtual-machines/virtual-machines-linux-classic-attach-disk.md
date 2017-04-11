<properties
    pageTitle="Na disku priložiti Linux VM | Microsoft Azure"
    description="Saznajte kako priložiti podatkovni disk Azure virtualnog računala radi Linux i pokrenuti tako da bude spreman za korištenje."
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
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Kako priložiti podatkovni Disk Linux virtualnog računala

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Pogledajte kako [priložiti na disku podataka pomoću modela implementacije Voditelj resursa](virtual-machines-linux-add-disk.md).

Možete priložiti prazan diskova i diskova koji sadrže podatke na vašem VMs Azure. Obje vrste diskova su .vhd datoteke koje se nalaze u račun za Azure prostora za pohranu. Kao s dodavanjem sve disk na računalo Linux nakon priložiti disk morate pokrenuti i oblikujte ga tako da bude spreman za korištenje. U ovom se članku Detalji o prilaganju prazan diskova i diskova već koji sadrži podatke za vaše VMs, kao i zatim pokrenuti i oblikovati novi disk.

> [AZURE.NOTE]Je najbolje koristiti odvojene diskova radi pohrane podataka virtualnog računala. Kada stvorite Azure virtualnog računala, ima na disk operacijski sustav i privremene disk. **Privremeni diska koristite za pohranu stalni podataka.** Kao što je naziv podrazumijeva, nudi privremenu pohranu. Nudi zalihosti ni sigurnosne kopije jer ga se ne nalaze u Azure prostora za pohranu.
> Privremeni disk je obično upravlja Azure Linux Agent i automatski postavljen na **/mnt/resource** (ili **/mnt** na slikama Ubuntu). S druge strane, na disku podataka može biti primjerice s nazivom Linux Jezgra sustava `/dev/sdc`, i trebali biste particije, oblikovanje i postavljanje tog resursa. Pogledajte [Vodič za korisnički Agent za Linux Azure] [ Agent] detalje.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Pokretanje nove podatke na disku u Linux

1. SSH za vaše VM. Dodatne pojedinosti potražite u članku [kako se prijaviti na virtualnog računala koja se izvodi Linux][Logon].

2. Dalje morate pronaći identifikator uređaja za podatkovni disk za pokretanje. Da biste to učinili na dva načina:

    a) Grep za uređaje SCSI u zapisnicima kao sljedeću naredbu:

            $sudo grep SCSI /var/log/messages

    Za nedavne Ubuntu distribucija, možda ćete morati koristiti `sudo grep SCSI /var/log/syslog` jer prijave `/var/log/messages` će se možda onemogućiti prema zadanim postavkama.

    Možete pronaći identifikator zadnji podatkovni disk koji je dodan u poruke koje se prikazuju.

    ![Se poruke na disku](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    OR

    b) korištenje na `lsscsi` naredbu da biste saznali id uređaja. `lsscsi`moguće je instalirati jedan od ovih `yum install lsscsi` (na razgovor crveno prema distribucija) ili `apt-get install lsscsi` (na Debian prema distribucija). Možete pronaći disk tražite njegov _lun_ ili **logičke jedinice broj**. Na primjer, _lun_ diskova ste priložili mogu lako vidjeti iz `azure vm disk list <virtual-machine>` kao:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Usporedba ove podatke s izlaz `lsscsi` za isti uzorak virtualnog računala:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    Zadnji broj n-torke u svakom retku je _lun_. U odjeljku `man lsscsi` dodatne informacije.

3. U naredbeni redak upišite sljedeću naredbu da biste stvorili uređaju:

        $sudo fdisk /dev/sdc


4. Kada se to od vas zatraži, unesite **n** da biste stvorili novi particije.


    ![Stvaranje uređaja](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Kada se to od vas zatraži, upišite **p** da biste particija primarna particija. Unesite **1** da biste prvi particije, a zatim na koji se vrsta unesite da biste prihvatili zadane vrijednosti za na valjkasti. U nekim sustavima ga možete prikazati zadane vrijednosti prvi i zadnji sektora, umjesto u valjkasti. Možete odabrati da biste prihvatili zadane vrijednosti.


    ![Stvaranje particije](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Upišite **p** da biste vidjeli detalje o disk koji ima particije.


    ![Informacije o popisu disk](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Upišite **w** da biste napisali postavki disk.


    ![Pisanje promjene na disku](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Sada možete stvoriti u datotečni sustav na nju. Broj particije dodati Identifikator uređaja (u sljedećem primjeru `/dev/sdc1`). Sljedeći primjer stvara s ext4 particije na /dev/sdc1:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Stvaranje datotečnom sustavu](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] Sustavi za SuSE Linux Enterprise 11 podržavaju samo pristup samo za čitanje za ext4 datotečnih. Za te sustavima, preporučuje se da biste oblikovali ext3 umjesto ext4 novi datotečni sustav.


9. Provjerite direktorij postavljanja novog datotečnog sustava, na sljedeći način:

        # sudo mkdir /datadrive


10. Na kraju pogonu, možete postaviti na sljedeći način:

        # sudo mount /dev/sdc1 /datadrive

    Podatkovni disk je sada da biste koristili kao **/datadrive**.
    
    ![Stvaranje imenika i postavljanje disk](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Dodajte novi pogon /etc/fstab:

    Da biste bili sigurni pogon je ponovo postaviti automatski nakon ponovnog pokretanja se morate dodati/itd/fstab datoteku. Osim toga, preporučujemo da /etc/fstab koristiti UUID (univerzalno Jedinstveni identifikator) da biste se pozvali na pogon, a ne samo naziv uređaja (odnosno /dev/sdc1). Korištenje na UUID izbjegava pogrešan disk koji se postavljen na određenom mjestu ako OS-a otkrije pogreške na disku tijekom operacijskih sustava i sve preostale podataka diskova dodijeljene zatim te uređaju ID-a. Da biste pronašli UUID novi pogon, možete koristiti uslužni **blkid** :

        # sudo -i blkid

    Rezultat izgleda otprilike ovako:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] Pogrešno uređivanje datoteke **/etc/fstab** može rezultirati neće sustava. Ako je sigurni, pročitajte dokumentaciju za raspodjelu informacije o pravilno uredili datoteku. Također preporučuje se da se prije uređivanja stvara sigurnosnu kopiju datoteke /etc/fstab.

    Nakon toga otvorite **/etc/fstab** datoteku u uređivaču teksta:

        # sudo vi /etc/fstab

    U ovom primjeru koristimo vrijednost UUID na novi uređaj **/dev/sdc1** koja je stvorena u prethodnim koracima, a mountpoint **/datadrive**. Dodajte sljedeći redak na kraj **/etc/fstab** datoteke:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Ili u sustavima koji se temelji na SuSE Linux možda ćete morati koristiti malo drugačije oblikovanje:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] Na `nofail` mogućnost osigurava da počinje s VM čak i ako na datotečnom sustavu oštećen ili disk ne postoji prilikom pokretanja. Bez tu mogućnost, možda ćete naići ponašanje kao što je opisano u [Ne SSH da biste Linux VM zbog FSTAB pogrešaka](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Sada možete testirati datotečnom sustavu pravilno postavljen tako da unmounting i odnosno remounting datotečnom sustavu primjer točke postavljanja `/datadrive` stvorenih u starijim korake:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Ako u `mount` naredba daje pogrešku, potražite i druge objekte/fstab pravilne sintakse. Ako dodatni podaci pogona ili particije stvaraju, unesite ih u/itd/fstab zasebno kao i.

    Provjerite pogon na koji je moguće pisati pomoću ove naredbe:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Naknadno uklanjanje podatkovni disk bez uređivanja fstab može uzrokovati VM uvoza za pokretanje. Ako je zajednička pojavljivanje, većina distribucija pružaju ili u `nofail` i/ili `nobootwait` fstab mogućnosti koje omogućuju sustava za pokretanje čak i ako se na disku ne uspije postavljanja pri pokretanju vremena. U dokumentaciji sustava raspodjele dodatne informacije o parametara.

### <a name="trimunmap-support-for-linux-in-azure"></a>Funkcija TRIM/UNMAP podrška za Linux servisu Azure
Neke jezgre Linux podržava TRIM/UNMAP operacije da biste odbacili Neiskorišteni blokova na disku. Te operacije prvenstveno korisne su u standardni prostora za pohranu obavještavate Azure Izbrisane stranice više nisu valjane, a možete se odbacuju. Odbacivanje stranice možete spremiti trošak ako stvaranje velikih datoteka, a zatim ih izbrisati.

Postoje dva načina da biste omogućili TRIM podrška u vašem VM Linux. Uobičajeni, pogledajte svoje raspodjele preporučeni način:

- Korištenje na `discard` dostupnosti mogućnost `/etc/fstab`, na primjer:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Umjesto toga možete izvesti u `fstrim` naredba ručno iz naredbenog retka ili ga dodati na crontab redovito pokretanje:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Otklanjanje poteškoća
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]


## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o korištenju sustava VM Linux u sljedećim člancima:

- [Kako se prijaviti na virtualnog računala koja se izvodi Linux][Logon]

- [Upute za odvajanje disk s Linux virtualnog računala](virtual-machines-linux-classic-detach-disk.md)

- [EŽA Azure pomoću model implementacije Classic](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
