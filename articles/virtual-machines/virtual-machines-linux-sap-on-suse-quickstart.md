<properties
   pageTitle="Testiranje NetWeaver SAP-a na Microsoft Azure SUSE Linux VMs | Microsoft Azure"
   description="Testiranje NetWeaver SAP-a na Microsoft Azure SUSE Linux VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Tekući NetWeaver SAP-a na Microsoft Azure SUSE Linux VMs


U ovom se članku opisuju razne stvari o kojima treba voditi računa prilikom koristite NetWeaver SAP-a u programu Microsoft Azure SUSE Linux virtualnim strojevima (VMs). Od 19 2016 može SAP NetWeaver službeno podržana je na SUSE Linux VMs na Azure. Sve pojedinosti o Linux verzijama, verzije otklanjanje SAP i tako dalje pronaći ćete u SAP bilješke 1928533 "aplikacija SAP na Azure: podržani proizvodi i Azure VM vrste".
Daljnje dokumentaciju o SAP-a na Linux VMs nalazi se ovdje: [Korištenje SAP-a na Linux virtualnim strojevima (VMs)](virtual-machines-linux-sap-get-started.md).


Sljedeće informacije pridonose izbjegli neke potencijalne probleme.

## <a name="suse-images-on-azure-for-running-sap"></a>Slika SUSE na Azure radi SAP-a

Radi NetWeaver SAP-a na Azure, koristite samo SUSE Linux Enterprise Server SLES 12 (SPx) – pogledajte i SAP bilješke 1928533. Posebne SUSE slika je u trgovine Windows Azure ("SLES 11 SP3 za SAP Pokrećite"), no to nije namijenjen općenitu upotrebu. Koristite ovu sliku jer ga je rezervirana za rješenje [SAP oblaka potražite biblioteku](https://cal.sap.com/) .  

Voditelj resursa Azure trebate koristiti za sve nove testira i instalacije na Azure. Da biste potražili slike SUSE SLES i verzije pomoću Azure PowerShell ili Azure sučelja naredbenog retka (EŽA), koristite sljedeće naredbe. Zatim koristite Izlaz, na primjer, da biste definirali OS slike u JSON predložak za implementaciju novi VM Linux SUSE.
Te naredbe ljuske PowerShell su valjani za Azure PowerShell verzije 1.0.1 i noviji.

* Potražite postojeće izdavača, uključujući SUSE:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Potražite postojeće ponude iz SUSE:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Potražite SUSE SLES ponude:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Potražite određenu verziju programa SLES SKU:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Instaliranje WALinuxAgent u SUSE VM

Agent naziva WALinuxAgent je dio slike SLES iz trgovine Windows Azure. Informacije o instaliranju ručno (na primjer, kada prijenos SLES OS virtualne tvrdog diska (VHD) iz lokalnog) potražite u članku:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (virtualne-računalima-linux-licencira-distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP "poboljšane nadzor"

SAP "Poboljšana nadzor" je obavezna preduvjeta za izvođenje SAP-a na Azure. Provjerite je li pojedinosti u SAP Imajte na umu 2191498 "SAP-a na Linux s Azure: Poboljšana nadzor".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Prilaganje diskova Azure podataka programa VM Linux Azure

Trebali biste nikad postavljanja Azure diskova podataka za programa VM Linux Azure pomoću uređaja ID. Umjesto toga koristite univerzalno Jedinstveni identifikator (UUID). Kada koristite grafičke alate za Azure postavljanja diskova podataka, na primjer, budite oprezni. Ponovno stavke u /etc/fstab.

Problem s ID uređaja je da ga može promijeniti, a zatim Azure VM možda "smrzavanje" u postupku pokretanja. Da biste smanjili problem, možete dodati parametar nofail u /etc/fstab. Ali Budite oprezni s nofail jer aplikacije mogu koristiti točku postavljanja kao prije i napisati u datotečnom sustavu korijenski u slučaju da na vanjskom Azure disk nije postavljen vrijeme pokretanje.

Samo iznimku ugrađivanjem putem UUID je prilaganja OS na disku za otklanjanje poteškoća s svrhe, kao što je opisano u sljedećoj sekciji.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Otklanjanje poteškoća s SUSE VM koji više nije dostupan

Možda postoje situacije u kojima se SUSE VM na Azure blokira u postupku pokretanja (na primjer, s pogreške vezane uz postavljanje diskova). Taj se problem možete provjeriti pomoću značajke pokretanje Dijagnostika za Azure virtualnim strojevima v2 na portalu za Azure. Dodatne informacije potražite u članku [Pokretanje dijagnostike] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Da biste riješili taj problem je da biste priložili OS disk iz oštećene VM drugi VM SUSE na Azure. Zatim unesite odgovarajuće promjene kao što su /etc/fstab za uređivanje ili uklanjanje pravila udev mreža, kao što je opisano u sljedećem odjeljku.

Postoji jedan važno treba uzeti u obzir. Implementacija nekoliko VMs SUSE iz iste trgovine Windows Azure slike (na primjer, SLES 11 SP4) uzrokuje OS disk da biste uvijek biti postavljena tako da isti UUID. Zbog toga priložite na disk OS s različitim VM koja je postavila pomoću iste slike trgovine Windows Azure pomoću na UUID rezultirat će dvije identične UUIDs. Time se uzrokuje probleme i znači da će zapravo VM namijenjena za otklanjanje poteškoća s pokretanje iz OS disk priloženom i oštećene umjesto izvorne jedan.

Za izbjegavanje ovog na dva načina:

* Korištenje nekom drugom slikom trgovine Windows Azure za otklanjanje poteškoća VM (na primjer, SLES 11 SPx umjesto SLES 12).
* Ne priložite disk oštećen OS s drugom VM pomoću UUID – korištenje nešto drugo.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Prijenos SUSE VM lokalnim Azure

Opis koraka da biste prenijeli SUSE VM lokalnim Azure, potražite u članku [Priprema SLES ili openSUSE virtualnog računala za Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Ako želite da biste prenijeli VM bez deprovision koraka na kraju (na primjer, da biste Zadrži postojeće instalacije SAP-a, kao i naziv glavnog računala), provjerite sljedeće stavke:

* Provjerite je li na disku OS postavljen pomoću UUID, a ne na uređaju ID-a. Promjena UUID samo u /etc/fstab nije dovoljno za OS disk. Osim toga, nemojte zaboraviti prilagodite učitavanje pokretanja putem YaST ili uređivanjem /boot/grub/menu.lst.
* Ako koristite oblik VHDX za SUSE OS disk i pretvorite ga u VHD za prijenos u Azure, vrlo je vjerojatno da mrežni uređaj promijenit će se iz eth0 u eth1. Da biste izbjegli probleme s kada ste pokretanje na Azure kasnije, promijeniti vratiti eth0 kao što je opisano u [popravljanje eth0 kloniranu VMware SLES 11] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Osim što je opisano u članku, preporučujemo da uklonite ovo:

   /lib/udev/Rules.d/75-persistent-NET-Generator.Rules

Možete instalirati i Linux Agent Azure (waagent) kojih ćete izbjeći potencijalne probleme, pod uvjetom da nema više NIC-ovi.

## <a name="deploying-a-suse-vm-on-azure"></a>Implementacija VM SUSE na Azure

Novi SUSE VMs trebali biste stvoriti i pomoću datoteke predložaka JSON u novi model upravljanja resursima Azure. Nakon stvaranja datoteke predloška JSON možete implementirati na VM pomoću sljedeće naredbe EŽA kao zamjena za PowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Dodatne pojedinosti o JSON datoteke predložaka potražite u članku [Voditelj resursa za Azure za izradu predložaka] (resursa-grupe – stvaranje-templates.md) i [Azure brzi početak rada predlošci] (https://azure.microsoft.com/documentation/templates/).

Dodatne informacije o EŽA i upravljanja resursima Azure potražite u članku [korištenje EŽA Azure za Mac i Linux Windows s Azure Voditelj resursa] (xplat-eža-azure-resursa-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP licence i hardver ključ

Za službeni ustanova SAP Azure novi mehanizam uvedena je da biste izračunali ključ hardver SAP-a koji se koristi za SAP licencu. Otklanjanje SAP morali ih je prilagoditi da biste pomoću ovog. Prijašnji verzije otklanjanje SAP-a za Linux niste uključili tu promjenu kod. Dakle, u određenim situacijama (, na primjer, Azure VM promjenu veličine), ključ hardver SAP promijenjen i SAP licenca je više ne moraju biti valjane. To je riješiti u najnovije jezgre SAP Linux. Pojedinosti potražite SAP bilješke 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>Paket sapconf SUSE / počeo gledati Admin

SUSE nudi paket pod nazivom "sapconf", koja upravlja postavljanje postavki specifične za SAP-a. Dodatne informacije o čemu služi paketa i kako instalirati i koristiti ga potražite u članku [pomoću sapconf da biste pripremili SUSE Linux Enterprise Server da biste pokrenuli sustavima SAP] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) i [što je sapconf ili kako pripremiti SUSE Linux Enterprise poslužitelju za izvođenje sustavima SAP?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

U aplikacijom postoji novi alat koji je zamijenio sapconf - počeo gledati adm. Jedan možete pronaći dodatne informacije o alatu pratiti dvije veze u nastavku.

Moguće je pronaći SLES dokumentaciju o počeo gledati Admin profila sap-hana [ovdje](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Moguće je pronaći ugađanje sustavi za SAP radnih opterećenja s počeo gledati-Admin - [Ovo](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) poglavlje 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>Zajedničko korištenje NFS u raspodijeljeno instalacijama SAP-a

Ako imate raspodijeljeno instalaciju – na primjer, mjesto na koje želite instalirati baza podataka i poslužitelji aplikacija SAP-a u zasebnom VMs – možete zajednički koristiti /sapmnt directory putem mreže datoteka sustava (NFS). Ako imate problema s korake za instalaciju nakon što stvorite zajedničko korištenje NFS za /sapmnt, provjerite je li "no_root_squash" postavljeno za zajedničko korištenje.

## <a name="logical-volumes"></a>Logička jedinicama

U prošlosti prema potrebi nešto velik logičke na više diskova Azure podataka (na primjer, za SAP baze podataka), je preporučljivo za korištenje mdadm lvm nije potpuno provjeriti valjanost još na Azure. Da biste saznali kako postaviti Linux RAID na Azure pomoću mdadm, potražite u odjeljku [Konfiguriranje softvera RAID na Linux](virtual-machines-linux-configure-raid.md). U aplikacijom na početku 2016 može lvm potpuno podržane na Azure i može se koristiti kao alternative mdadm. Dodatne informacije o lvm na Azure potražite u članku [Konfiguriranje LVM na Linux VM u Azure](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure SUSE spremište

Ako imate problema s programom access u standardni Azure SUSE spremište, poslužite se jednostavne naredbe je ponovno postaviti. To se može dogoditi ako stvorili privatne OS sliku u Azure regiji, a zatim kopirajte sliku na drugo područje na mjesto na koje želite uvesti nove VMs koji se temelje na privatne sliku s operacijskim Sustavom. Samo pokrenite sljedeću naredbu unutar na VM:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome radne površine

Ako želite koristiti radnu površinu Gnome da biste instalirali cjelokupnog sustava SAP pokazni videozapis unutar jedne VM – uključujući GUI programa SAP preglednik i SAP konzola – koristite ovaj podsjetnik da biste ga instalirali na slikama Azure SLES:

   Za SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Za SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>Podrška za SAP-a za Oracle na Linux u oblaku

Nema ograničenja podršku iz Oracle na Linux u virtualiziranom okruženju. Iako to nije u temi specifične za Azure, važno je znati. SAP-a ne podržava Oracle na SUSE ili crveno razgovor u javnoj popisa kao što su Azure. O ovoj se temi izravno obratiti Oracle.
