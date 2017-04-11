<properties 
    pageTitle="Konfiguriranje LVM na virtualnog računala koja se izvodi Linux | Microsoft Azure" 
    description="Saznajte kako konfigurirati LVM na Linux u Azure." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Konfiguriranje LVM na Linux VM servisu Azure

Ovaj dokument će govori o konfiguriranju logičke glasnoću Manager (LVM) u Azure virtualnog računala. Dok je izvedivo za konfiguriranje LVM na bilo kojem disku priložiti virtualnog računala, po zadanom Većina slika oblaka neće imati LVM konfiguriran na disku OS. To je da biste spriječili probleme s grupama duplicirane glasnoću ako OS disk ikad pridružen drugi VM raspodjele i vrsta, odnosno tijekom scenarij oporavak. Stoga preporučuje se samo za korištenje LVM na diskova podataka.


## <a name="linear-vs-striped-logical-volumes"></a>Linearni nasuprot prugastim logičke jedinice

LVM može se koristiti za kombiniranje broj fizičkih diskova u jedan za pohranu glasnoću. Prema zadanim postavkama LVM će obično stvoriti linearni logičke količine, što znači da se spajaju li fizičke prostora za pohranu zajedno. U ovom slučaju postupci za čitanje/pisanje obično samo poslat će se jedan disk. Nasuprot tome, ne možemo možete stvoriti i prugastim logičke količine gdje distribuira čitanja i pisanja za više diskova koje se nalaze u grupi jedinica (odnosno slično RAID0). Radi boljih performansi vjerojatno želite stripe logičkih diskova tako da se čitanja i pisanja koristiti sve priložene podataka diskova.

Ovaj dokument će opisuju kako spojiti nekoliko diskova podataka u jednu grupu, a zatim stvorite prugastim logičke glasnoću. Da biste radili s većinom distribucija su GRG generalizirano Pomalo korake u nastavku. U većini slučajeva uslužni programi i tijekova rada za upravljanje LVM na Azure nisu bitno razlikuje se od drugih okruženja. Uobičajen i potražite prodavaču Linux dokumentaciju i Praktični savjeti za korištenje LVM s na određeni razdiobom.


## <a name="attaching-data-disks"></a>Prilaganje diskova podataka
Nešto obično preporučujemo da biste započeli s dva ili više diskova prazan podataka prilikom korištenja LVM. Ovisno o vašim potrebama IO, možete odabrati da biste priložili diskova pohranjene u našem standardne prostor za pohranu, s do 500 IO/ps po disk ili naših prostora za pohranu Premium s do 5000 IO/ps po disk. U ovom se članku ne će odlaze pojedinosti o Dodjela resursa i diskova podataka priložite Linux virtualnog računala. Pogledajte na Microsoft Azure članku [prilaganje na disku](virtual-machines-linux-add-disk.md) detaljne upute o tome kako priložiti na disk prazan podataka Linux virtualnog računala na Azure.

## <a name="install-the-lvm-utilities"></a>Instalacija uslužni programi LVM

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS i Oracle Linux**

        # sudo yum install lvm2

- **SLES 12 i openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    Na SLES11 morate i uređivanje /etc/sysconfig/lvm i postavite `LVM_ACTIVATED_ON_DISCOVERED` omogućiti "":

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>Konfiguriranje LVM
U ovom vodiču će pretpostavimo da ste joj pridružili tri diskova podataka koje ćete nazivamo `/dev/sdc`, `/dev/sdd` i `/dev/sde`. Imajte na umu da ih možda neće uvijek biti iste nazive put u vašem VM. Možete pokrenuti "`sudo fdisk -l`" ili slično naredbu da biste dobili popis vaše dostupno diskova.

1. Priprema fizičke količine:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Stvaranje grupe jedinica. Ne možemo zovete u glasnoću grupu "podataka-vg01" u ovom primjeru:

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Stvaranje logičke volume(s). Naredba ispod smo će stvoriti jedna jedinica logičke pod nazivom "podataka-lv01" obuhvaćaju grupi čitave jedinice, ali imajte na umu da je izvedivo da biste stvorili više logičke jedinice u grupi jedinica.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Logička formatirali

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] Pomoću SLES11 "-t ext3" umjesto ext4. SLES11 podržava samo pristup ext4 filesystems samo za čitanje.


## <a name="add-the-new-file-system-to-etcfstab"></a>Dodavanje novog datotečnog sustava /etc/fstab

**Upozorenje:** Pogrešno uređivanje datoteke /etc/fstab može rezultirati neće sustava. Ako je sigurni, pogledajte u dokumentaciji za raspodjelu informacije o tome kako pravilno uredili datoteku. Također preporučuje se da se prije uređivanja stvara sigurnosnu kopiju datoteke /etc/fstab.

1. Stvaranje točku željeni postavljanja za novi datotečni sustav, primjerice:

        # sudo mkdir /data


2. Pronađite put logičke glasnoće

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Otvorite /etc/fstab u uređivaču teksta i dodavanje unosa za novi datotečni sustav, na primjer:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Nakon toga spremite i zatvorite /etc/fstab.


4. Testiranje točni i druge objekte/fstab stavku:

        # sudo mount -a

    Ako ta naredba rezultira poruka o pogrešci Provjerite sintaksu u datoteci i druge objekte/fstab.

    Potom pokrenite na `mount` naredbe da bi sustav datoteka:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Neobavezno) Failsafe pokretanje parametara u /etc/fstab

    Mnoge distribucija uključiti ili u `nobootwait` ili `nofail` dostupnosti parametre koje se može dodati i druge objekte/fstab datoteku. Parametara Dopusti za pogreške prilikom postavljanje određenog datotečni sustav i Dopusti sustavu Linux da biste nastavili s pokretanje čak i ako se ne može ispravno postavljanja RAID datotečnom sustavu. Pogledajte svoje raspodjelu potražite u dokumentaciji za dodatne informacije o parametara.

    Primjer (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
