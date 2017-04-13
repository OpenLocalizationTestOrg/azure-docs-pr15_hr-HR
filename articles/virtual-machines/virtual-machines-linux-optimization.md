<properties
    pageTitle="Optimiziranje sustava VM Linux na Azure | Microsoft Azure"
    description="Naučite nekoliko savjeta za optimizaciju da biste bili sigurni ste postavili na Linux VM postigli optimalne performanse na Azure"
    keywords="Linux virtualnog računala, linux virtualnog računala ubuntu virtualnog računala" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Optimiziranje sustava VM Linux na Azure

Stvaranje Linux virtualnog računala (VM) jednostavno je to iz naredbenog retka ili s portala sustava. Pomoću ovog praktičnog vodiča objašnjava kako biste bili sigurni ste postavili ga da biste optimizirali njegovih performansi na platformi Microsoft Azure. U ovoj se temi koristi programa VM Ubuntu poslužitelj, ali možete stvoriti i Linux virtualnog računala pomoću [svoje slike kao predlošci](virtual-machines-linux-create-upload-generic.md).  

## <a name="prerequisites"></a>Preduvjeti

U ovoj se temi pretpostavlja da već imate radu Azure pretplate ([besplatnu probnu prijava](https://azure.microsoft.com/pricing/free-trial/)), [instaliran EŽA Azure](../xplat-cli-install.md) i u pretplatu Azure već dodjeli na VM. Prije nego što to bilo što s Azure – imate provjere autentičnosti za svoju pretplatu. Da biste to učinili s EŽA Azure, ondje upišite `azure login` da pokrenete postupak interaktivne. 

## <a name="azure-os-disk"></a>Azure OS na disku

Kada stvorite Linux Vm u Azure, ima dvije diskova pridružen. /dev/sda na disku OS, /dev/sdb je privremeni disk.  Koristiti glavni disk OS (/ razvojni/sda) za sve osim operacijski sustav optimiziran je za brzog pokretanja VM i navedite dobre performanse za vaše radnih opterećenja. Koju želite priložiti jedan ili više diskova za vaše VM da biste dobili stalnog i optimizirana za pohranu za vaše podatke. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Dodavanje diskova za veličina i performanse ciljnih web-mjesta 

Ovisno o veličini VM, možete priložiti do 16 dodatnih diskova na za odgovora-niz, 32 diskova na niz D i 64 diskova na niz G na računalu – svakog do 1 TB po veličini. Dodajte dodatni diskova prema potrebi po prostor i IOps preduvjeti. Svaki disk ima ciljni performanse od 500 IOps za pohranu u standardni i najviše 5000 IOps po na disku za pohranu Premium.  Dodatne informacije o diskova Premium prostora za pohranu potražite [prostora za pohranu Premium: visokih performansi prostor za pohranu Azure VMs](../storage/storage-premium-storage.md)

Da biste postigli najvišu IOps na pohranu Premium diskova gdje se njihove postavke predmemorije postavljena na "Samo za čitanje" ili "Nema", "barriers" morate onemogućiti tijekom postavljanje datotečnog sustava u Linux. Ne morate barriers jer su durable te postavke predmemorije upisivanje Premium prostora za pohranu sigurnosno diskova.

- Ako koristite **reiserFS**, onemogućivanje barriers pomoću mogućnosti postavljanja "zapreka nijedno =" (za omogućivanje barriers, koristite "zapreka = pražnjenje")
- Ako koristite **ext3/ext4**, onemogućivanje barriers pomoću mogućnosti postavljanja "zapreka = 0" (za omogućivanje barriers, koristite "zapreka = 1")
- Ako koristite **XFS**, onemogućivanje barriers pomoću mogućnosti postavljanja "nobarrier" (za omogućivanje barriers koristiti mogućnost "zapreka")

## <a name="storage-account-considerations"></a>Pitanja vezana uz račun za pohranu

Kada stvarate vaše VM Linux u Azure, provjerite je li priložiti diskova s računa za pohranu residing u području isti kao vaše VM osigurati Zatvori Blizina i minimiziranja latenciju mreže.  Svaki račun standardne prostora za pohranu sastoji se od 20 najviše k IOps i kapacitet veličine 500 TB.  To funkcionira na približno 40 intenzivnog korištenih diskova uključujući OS disku i sve podatke diskova stvarate. Za račune za pohranu Premium nema IOps Maksimalna ograničenja, ali postoji ograničenje veličine 32 TB. 

Kada Postupanje s visokim IOps radnih opterećenja i odlučili standardne prostora za pohranu za vaše diskova, možda ćete morati podijeliti na diskova u više prostora za pohranu računa da biste bili sigurni da ne kliknete 20 000 IOps ograničenje za račune standardne prostora za pohranu. Vaš VM mogu sadržavati kombinacije diskova iz preko prostora za pohranu za različite račune i vrste računa za pohranu da biste postigli optimalne konfiguraciji. 

## <a name="your-vm-temporary-drive"></a>Privremeni VM pogon

Prema zadanim postavkama kada stvarate VM, Azure omogućuje disk OS (/ razvojni/sda) i privremene disk (/ razvojni/sdb).  Dodatnih diskova dodate prikazivati kao /dev/sdc, /dev/sdd, /dev/sde i tako dalje. Sve podatke na disku privremene (/ razvojni/sdb) nije durable, a može biti izgubljene ako VM promjene veličine, ponovno uvođenje, kao što su određene događaje ili održavanje navodi ponovno pokretanje sustava VM.  Veličina i vrsta privremene diska povezana je s veličina VM odaberete trenutku implementacije. Ako bilo koji od premium veličina VMs (DS, G i DS_V2 niz) privremene pogon je sigurnosno putem lokalnog SSD dodatne performanse do 48 k IOps. 

## <a name="linux-swap-file"></a>Zamjena Linux datoteka

Ako je vaš VM Azure Ubuntu ili CoreOS slike, CustomData možete koristiti da biste poslali cloud konfiguracijska oblaka pokretanja. Ako ste [prenijeli prilagođenu Linux sliku](virtual-machines-linux-upload-vhd.md) koja koristi oblaka pokretanja, i konfigurirate zamjena particije pomoću oblaka pokretanja.

Na Ubuntu slike oblaka, morate koristiti oblaka pokretanja da biste konfigurirali particija Zamijeni. Na wiki Ubuntu dodatne pojedinosti potražite u članku na sljedećoj stranici: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Slika bez podrške za oblak pokretanja VM slike implementiran iz trgovine Azure imati Agent za Linux VM integriran s OS-a. Ovaj agent omogućuje VM interakciju s različite servise za Azure. Pod pretpostavkom da ste implementiran standardnom slikom iz trgovine Azure, trebali biste biste pravilno konfigurirali postavke Linux zamijeni datoteku, učinite sljedeće:

Pronalaženje i izmjena dvije stavke u datoteci **/etc/waagent.conf** . Oni kontrolirati postojanje parametra namjenski Zamjena datoteke i veličinu datoteke Zamijeni. Parametri tražite da biste izmijenili su `ResourceDisk.EnableSwap=N` i`ResourceDisk.SwapSizeMB=0` 

Morate ih mijenjati ovako:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size u MB da bi odgovarao vašim potrebama} 

Nakon što ste napravili promjene, morat ćete ponovno pokrenite na waagent ili ponovno pokrenite vaše VM Linux u skladu s tim promjenama.  Promjene implementirana i zamijeni datoteka stvorena je kada koristite na `free` naredba za prikaz slobodnog prostora. U primjeru u nastavku je datoteka zamjena 512MB stvoren kao rezultat izmjena waagent.conf datoteku.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>/ I planiranje algoritam za pohranu Premium

S na 2.6.18 Linux otklanjanje, zadani/i planiranje algoritam promijenio iz krajnji rok u CFQ (potpuno sajma stavljanja algoritam). Da biste dobili izravnim pristupom/i uzorke, postoji negligible razlike u performansama razlike između CFQ i krajnji rok.  Za utemeljen na SSD diskova pri čemu je uzorak/i disk većinom uzastopnih, promjene algoritam NOOP ili rok možete postići bolje performanse/i.

### <a name="view-the-current-io-scheduler"></a>Prikaz trenutnog/i raspored

Koristite sljedeću naredbu:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Vidjet ćete pratiti izlaz koji pokazuje trenutni raspored.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Promjena trenutni uređaj (/ razvojni/sda) / i algoritam za planiranje rasporeda

Koristite sljedeće naredbe:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Postavljanje za /dev/sda samostalno nije korisno. Potrebno je moguće postaviti na sve podatke diskova gdje uzastopnih/i dominates uzorak/i.  

Trebali biste vidjeti sljedeće Izlaz, koji označava taj grub.cfg je uspješno izgraditi i ažurirati da alat za zakazivanje zadane NOOP.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Za raspodjelu obitelj Redhat potrebni su jedino sljedeću naredbu:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Koristiti softver RAID da biste postigli više li / Ops

Ako vaš radnih opterećenja zahtijeva više IOps nego što možete unijeti jedan disk, morate koristiti softver RAID konfiguracija više diskova. Budući da Azure već izvodi na disku otpornost sloju lokalne tkanina, postigli najvišu razinu performansi iz konfiguracije za Podjela RAID 0.  Morate Dodjela resursa za stvaranje novih diskova u okruženje za Azure i priložite vaše VM Linux prije no što particija, oblikovanje i postavljanje pogone.  Dodatne informacije o konfiguriranju na RAID instalaciju softvera na vašem VM Linux servisu azure pronaći ćete u dokumentu **[Konfiguriranje RAID softver na Linux](virtual-machines-linux-configure-raid.md)** .


## <a name="next-steps"></a>Daljnji koraci

Imajte na umu, uz sve rasprave optimizacije morate testove prije i nakon svake promjene za mjerenje utjecaj imat će se promjene.  Optimizacija je postupak korak po korak koji će imati različite rezultate na različitim računalima u svom okruženju.  Što radi jedne konfiguracije možda neće funkcionirati za druge korisnike.

Neke korisne veze na dodatne resurse: 

- [Prostor za pohranu Premium: Visokih performansi prostor za pohranu radnih opterećenja Azure virtualnog računala](../storage/storage-premium-storage.md)
- [Agent za Azure Linux korisničkom priručniku](virtual-machines-linux-agent-user-guide.md)
- [Optimiziranje performanse MySQL Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md)
- [Konfiguriranje softver RAID na Linux](virtual-machines-linux-configure-raid.md)
