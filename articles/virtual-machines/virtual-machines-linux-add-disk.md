<properties
    pageTitle="Dodavanje na disku Linux VM | Microsoft Azure"
    description="Doznajte kako dodati trajni disk na Linux VM"
    keywords="Linux virtualnog računala dodali na disku resursa"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Dodavanje na disku Linux VM

U ovom se članku objašnjava da biste priložili stalni disk na VM tako da možete sačuvati podataka – čak i ako se vaš VM se brišu zbog održavanja ili promjenu veličine. Da biste dodali na disku, morate [EŽA Azure](../xplat-cli-install.md) konfiguriran u načinu Voditelj resursa (`azure config mode arm`).  

## <a name="quick-commands"></a>Brzi naredbe

U sljedećim primjerima naredba zamjena vrijednosti između &lt; i &gt; s vrijednostima iz vlastitu okruženju.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Prilaganje na disku

Prilaganje novi disk je brzi. Vrsta `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` da biste stvorili i priložili novi disk GB za vaše VM. Ako ste izričito prepoznati na račun za pohranu, bilo kojem disku stvorite se smješta u isti račun za pohranu gdje se nalazi na disku OS.  Trebao bi izgledati otprilike ovako:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Izlaz

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Povezivanje s VM Linux postavljanja novog diska

> [AZURE.NOTE] U ovoj se temi povezuje VM pomoću korisnička imena i lozinke. Da biste koristili javnim i privatnim ključ parove možete komunicirati s vašeg VM, potražite [u](virtual-machines-linux-mac-create-ssh-keys.md)članku korištenje SSH s Linux na Azure. Možete izmijeniti povezivost **SSH** VMs stvorena pomoću na `azure vm quick-create` pomoću naredbe na `azure vm reset-access` naredbu za ponovno postavljanje pristupa **SSH** potpuno, dodavanje i uklanjanje korisnika ili dodati javni ključ datoteke siguran pristup.

Kasnije zatrebate za SSH u svoje Azure VM za particija oblikovanje i postavljanje novog diska da ga mogli koristiti svoje VM Linux. Ako niste upoznati s povezivanjem s **ssh**, naredba uzima obrazac `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, i izgleda ovako:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Izlaz

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Sad kad ste povezani s vašeg VM, spremni ste za prilaganje na disku.  Najprije pronađite diska, pomoću `dmesg | grep SCSI` (metodu koristite da biste otkrili novi disk mogu se razlikovati). U ovom slučaju izgleda otprilike ovako:

```bash
dmesg | grep SCSI
```

Izlaz

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

i u slučaju ovoj se temi na `sdc` disk je onaj koji želimo. Sada particije na disku sa `sudo fdisk /dev/sdc` --uz pretpostavku da se u vašem slučaju disk je `sdc`, i učinite je primarni disk na particija 1 i prihvatite ostale zadane vrijednosti.

```bash
sudo fdisk /dev/sdc
```

Izlaz

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Stvaranje particije tako da upišete `p` naredbenom:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

I pisati u datotečnom sustavu particija pomoću naredbe **mkfs** navodeći svoju datotečnom sustavu vrstu i naziv uređaja. U ovoj se temi priznanje `ext4` i `/dev/sdc1` odozgo:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Izlaz

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Sada ćemo stvoriti direktorij postavljanja pomoću datoteke sustava `mkdir`:

```bash
sudo mkdir /datadrive
```

I postavljanje directory pomoću `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Podatkovni disk je sada da biste koristili kao `/datadrive`.

```bash
ls
```

Izlaz

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Da biste bili sigurni pogon je ponovo postaviti automatski nakon ponovnog pokretanja se morate dodati/itd/fstab datoteku. Osim toga, preporučujemo da/Dr/fstab koristiti UUID (univerzalno Jedinstveni identifikator) da biste se pozvali na pogon, a ne samo naziv uređaja (kao što su `/dev/sdc1`). Ako OS-a otkrije pogreške na disku prilikom pokretanja, pomoću na UUID izbjegava pogrešan disk koji se postavljen na određenom mjestu. Preostalo diskova podataka želite zatim dodijeliti te istom uređaju ID-a. Da biste pronašli UUID novi pogon, koristite uslužni **blkid** :

```bash
sudo -i blkid
```

Rezultat izgleda otprilike ovako:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Pogrešno uređivanje datoteke **/etc/fstab** može rezultirati neće sustava. Ako je sigurni, pročitajte dokumentaciju za raspodjelu informacije o pravilno uredili datoteku. Također preporučuje se da se prije uređivanja stvara sigurnosnu kopiju datoteke /etc/fstab.

Nakon toga otvorite **/etc/fstab** datoteku u uređivaču teksta:

```bash
sudo vi /etc/fstab
```

U ovom primjeru koristimo vrijednost UUID na novi uređaj **/dev/sdc1** koja je stvorena u prethodnim koracima, a mountpoint **/datadrive**. Dodajte sljedeći redak na kraj **/etc/fstab** datoteke:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Kasnije uklanjanje podatkovni disk bez uređivanja fstab može uzrokovati VM uvoza za pokretanje. Većina distribucija pružaju ili u `nofail` i/ili `nobootwait` fstab mogućnosti. Ove mogućnosti omogućuju sustava za pokretanje čak i ako se na disku ne uspije postavljanja prilikom pokretanja. U dokumentaciji sustava raspodjele dodatne informacije o parametara.

>[AZURE.NOTE] Mogućnost **nofail** osigurava da počinje s VM čak i ako na datotečnom sustavu oštećen ili disk ne postoji prilikom pokretanja. Bez tu mogućnost, koji se mogu pojaviti ponašanje kao što je opisano u [Ne SSH da biste Linux VM zbog pogreške FSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>Funkcija TRIM/UNMAP podrška za Linux servisu Azure
Neke jezgre Linux podržava TRIM/UNMAP operacije da biste odbacili Neiskorišteni blokova na disku. To je prvenstveno korisno u standardni prostora za pohranu obavještavate Azure Izbrisane stranice više nisu valjane, a možete se odbacuju. To možete spremiti trošak ako stvaranje velikih datoteka, a zatim ih izbrisati.

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

- Imajte na umu da novi disk nije dostupan u VM ako izvršen osim ako pišete te podatke u datoteku [fstab](http://en.wikipedia.org/wiki/Fstab) .
- Da biste osigurali ispravno konfigurirano vaše VM Linux, pregledajte preporuke za [optimiziranje performanse računala Linux](virtual-machines-linux-optimization.md) .
- Proširite kapacitet za pohranu dodavanjem dodatnih diskova i [Konfiguriranje RAID](virtual-machines-linux-configure-raid.md) dodatne performanse.
