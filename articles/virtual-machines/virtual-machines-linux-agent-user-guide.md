<properties 
    pageTitle="Agent za Linux korisničkom priručniku | Microsoft Azure" 
    description="Saznajte kako instalirati i konfigurirati Linux Agent (waagent) da biste upravljali virtualnog računala interakciju s kontroler tkanina Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Agent za Azure Linux korisničkom priručniku

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Uvod

Microsoft Azure Linux Agent (waagent) upravlja Linux & FreeBSD dodjele resursa i VM interakcije s kontrolerom tkanina Azure. Nudi sljedeće funkcije za Linux i FreeBSD IaaS implementacije:

> [AZURE.NOTE] Agent za Azure Linux [datoteka pročitajme za](https://github.com/Azure/WALinuxAgent/blob/master/README.md) dodatne informacije potražite u članku.

* **Dodjela resursa za slike**
  - Stvaranje korisničkog računa
  - Konfiguriranje vrste provjere autentičnosti SSH
  - Uvođenje SSH javni ključ i ključne parove
  - Postavljanje naziv glavnog računala
  - Objavljivanje naziv glavnog računala na platformi DNS-a
  - Izvješćivanje o pogreškama ključni otisak prsta glavno računalo SSH platformi
  - Upravljanje resursima na disku
  - Oblikovanje i postavljanje disk resursa
  - Konfiguriranje zamjena prostora

* **Povezivanje s mrežom**
  - Upravlja usmjerava za poboljšanje kompatibilnosti s poslužiteljima DHCP platforme
  - Osigurava stabilnosti sučelja naziv mreže

* **Otklanjanje**
  - Konfigurira NUMA virtualne (onemogućite za otklanjanje < 2.6.37)
  - Troši Hyper-V entropy za /dev/random
  - Konfigurira SCSI vremenska ograničenja korijenski uređaj (što može biti udaljene)

* **Dijagnostika**
  - Konzola za preusmjeravanje serijski priključak

* **SCVMM implementacije**
    - Otkriva i bootstraps VMM agent za Linux kada se pokrene u okruženju sustava centra virtualnog računala Upravitelj 2012 R2

* **Proširenje VM**
    - Ubaci komponente autor Microsoft i partneri u Linux VM (IaaS) da biste omogućili softver i konfiguracija automatizacije
    - Proširenje VM implementaciju referenca na [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)


## <a name="communication"></a>Komunikacija
Dogodit će se Protok informacija iz platforme za agenta putem dva kanala:

* Vrijeme pokretanja priložiti DVD-a za IaaS implementacije. Ovaj DVD sadrži OVF usklađen konfiguracijska datoteka koja sadrži sve podatke za dodjelu resursa umjesto stvarnih keypairs SSH.
* TCP krajnjoj točki će REST API-JA koristi za postizanje implementacije i konfiguracija topologije.


## <a name="requirements"></a>Preduvjeti
Sljedeći sustavi testirate i poznato je da biste radili s Azure Linux Agent:

> [AZURE.NOTE] Ovaj popis mogu se razlikovati od službeni popis podržani sustavi na platformi Microsoft Azure, kao što je opisano u nastavku: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6,3 +
* Crvena je vaša Enterprise Linux 6,7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6,4 +

Drugi podržani sustavi:

* FreeBSD 10 + (Azure Linux Agent v2.0.10 +)

Agent za Linux ovisi o nekim paketa sustava da bi se ispravno funkcionirati:

* Python 2,6 +
* OpenSSL 1.0 +
* OpenSSH 5,3 +
* Uslužni programi datotečnom sustavu: sfdisk fdisk, mkfs, parted
* Alati za lozinku: chpasswd, sudo
* Tekst obrade Alati: sed, grep
* Mrežni Alati: ip usmjeravanje
* Otklanjanje podrške za postavljanje UDF filesystems.

## <a name="installation"></a>Instalacija
Instalacije pomoću programa RPM ili DEB paket spremištu paket sustava raspodjele je željeni način instaliranje i nadogradnja Agent za Azure Linux. Sve na [licencira raspodjele davatelji](virtual-machines-linux-endorsed-distros.md) integrirati agent paketa Azure Linux u svoje slike i spremišta.

Pogledajte dokumentacije [Azure Linux Agent repo na Github](https://github.com/Azure/WALinuxAgent) za instalaciju Napredne mogućnosti, primjerice instalacije iz izvora ili na web-mjesto prilagođene mjesta ili u okvir za prefiksi.


## <a name="command-line-options"></a>Mogućnosti naredbenog retka

### <a name="flags"></a>Zastavice

- opširno: povećati verbosity navedena naredba
- Prisilna Primjena: preskočiti interaktivne potvrdu za neke naredbe

### <a name="commands"></a>Naredbe

- Pomoć: navedeni podržani naredbe i zastavice.

- deprovision: pokušati očistiti sustav i provjerite prikladna za ponovno dodjeljivanje. Ovaj postupak izbrisati sljedeće:
 * Sve tipke SSH glavnog računala (Ako je Provisioning.RegenerateSshHostKeyPair y u konfiguracijskoj datoteci)
 * Konfiguracija poslužitelja naziva u /etc/resolv.conf
 * Lozinka korijenski iz /etc/shadow (Ako je Provisioning.DeleteRootPassword y u konfiguracijskoj datoteci)
 * Predmemorirani zakupa DHCP klijent poslužitelja
 * Naziv glavnog računala na izvorne vrijednosti primjenjuje na localhost.localdomain


> [AZURE.WARNING] Deprovisioning ne jamči da je slika očišćenog sve povjerljivih podataka i prikladan radi ponovne distribucije.


- deprovision + korisnika: izvodi sve pod - deprovision (prethodno opisano) i briše posljednje dodijeljenu korisnički račun (dobivenog od /var/lib/waagent) i povezane podatke. Ovaj parametar je kada Poništi dodjelu resursa u sliku koju ste prethodno dodjele resursa na Azure tako da mogu biti zabilježene i ponovno koristiti.

- verzija: prikazuje verziju waagent

- serialconsole: konfigurira GRUB da biste označili ttyS0 (prvi serijski priključak) kao konzole za pokretanje. Time se osigurava da otklanjanje bootup zapisnika šalju serijski priključak i učinili dostupnima za ispravljanje pogrešaka.

- daemon: pokretanje waagent kao daemon da biste upravljali interakciju s platforme. Da biste waagent u skripti init waagent navedena je argument.

- pokretanje: pokretanje waagent kao postupka u pozadini


## <a name="configuration"></a>Konfiguracija

Konfiguracijska datoteka (/ etc/waagent.conf) određuje akcije waagent. Ogledna datoteka konfiguracije prikazano u nastavku:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Različite mogućnosti konfiguracije su detaljno opisane u nastavku. Konfiguracija mogućnosti su tri vrste; Booleove vrijednosti, niz ili cijeli broj. Mogućnosti Booleova konfiguracije možete navesti kao "d" ili "n". Posebne ključna riječ "Nema" može se koristiti za neke niz vrsta konfiguracije stavke kao detaljne ispod.

**Provisioning.Enabled:**  
Vrsta: Booleova  
Zadani: y

Time se omogućuje korisniku omogućiti ili onemogućiti dodjele resursa funkcionalnost agenta. Valjane su vrijednosti "d" ili "n". Ako je onemogućen dodjele resursa, zadržavaju SSH tipke glavno računalo i korisnika na slici, a sve konfiguracija navedena u Azure dodjeljivanje API se zanemaruje.

> [AZURE.NOTE] Na `Provisioning.Enabled` parametar po zadanom je "n" na Ubuntu oblaka slike koje koriste oblaka pokretanja za dodjelu resursa.

  
**Provisioning.DeleteRootPassword:**  
Vrsta: Booleov  
Zadani: n

Ako skup, korijenski lozinku u datoteci/Dr/sjene gube se tijekom postupka dodjele resursa.


**Provisioning.RegenerateSshHostKeyPair:**  
Vrsta: Booleova  
Zadani: y

Ako skup, svi SSH glavno računalo ključa parovi (ecdsa, dsa i rsa) brišu se tijekom postupka za dodjelu resursa iz /etc/ssh /. Te se generira jedan par Osvježi ključa.

Vrsta šifriranja za Osvježi ključa par je konfigurirati stavkom Provisioning.SshHostKeyPairType. Napominjemo da neke distribucija ponovno stvoriti SSH ključa parove za bilo koju vrstu nedostaju šifriranja prilikom ponovnog pokretanja SSH daemon (na primjer, nakon ponovnog pokretanja).


**Provisioning.SshHostKeyPairType:**  
Vrsta: niza  
Zadani: rsa

To možete postaviti vrstu algoritam za šifriranje koji podržavaju daemon SSH na virtualnog računala. Obično podržanih vrijednosti su "rsa", "dsa" i "ecdsa". Imajte na umu da "putty.exe" u sustavu Windows ne podržava "ecdsa". Tako, ako namjeravate koristiti putty.exe u sustavu Windows da biste se povezali s Linux implementacijom, koristite "rsa" ili "dsa".


**Provisioning.MonitorHostName:**  
Vrsta: Booleova  
Zadani: y

Ako postavite, waagent će virtualnog računala Linux promjene naziv glavnog računala (kao što je vratio naredbu "naziv glavnog računala") i praćenje automatski ažurirati mrežna konfiguracija slici da bi se promjena primijenila. Da bi se na DNS poslužitelji automatske promjena naziva, povezivanje s mrežom će se ponovno pokrenuti u virtualnog računala. Posljedica će u brief gubitak s Internetom.


**Provisioning.DecodeCustomData**  
Vrsta: Booleov  
Zadani: n

Ako postavite, waagent će dekodiranje CustomData iz Base64.


**Provisioning.ExecuteCustomData**  
Vrsta: Booleov  
Zadani: n

Ako postavite, waagent će izvršiti CustomData nakon dodjele resursa.


**Provisioning.PasswordCryptId**  
Vrsta: niza  
Zadani: 6

Algoritam koji se koristi crypt pri stvaranju raspršivanje lozinku.  
 1 – MD5  
 2a - Blowfish  
 5 – SHA-256  
 6 – SHA-512  


**Provisioning.PasswordCryptSaltLength**  
Vrsta: niza  
Zadani: 10

Duljina slučajni soli koristiti pri stvaranju raspršivanje lozinku.


**ResourceDisk.Format:**  
Vrsta: Booleova  
Zadani: y

Ako postavite, disk resursa koje navede platforma bit će oblikovani i postavljen tako da waagent ako je vrsta datotečnom sustavu zatražio korisnik u "ResourceDisk.Filesystem" bilo što drugo osim "ntfs". Jedna particija vrste Linux (83) bit će dostupna na disku. Imajte na umu da particija ne oblikuje ako ga može biti uspješno postavljen.


**ResourceDisk.Filesystem:**  
Vrsta: niza  
Zadani: ext4

Određuje vrstu datotečnom sustavu za disk resursa. Podržani vrijednosti razlikuju se prema raspodjele Linux. Ako je niz X, zatim mkfs. X mora sadržavati na slici Linux. Slika SLES 11 obično trebali biste koristiti "ext3". Slika FreeBSD ovdje trebali biste koristiti 'ufs2'.


**ResourceDisk.MountPoint:**  
Vrsta: niza  
Zadani: / mnt/resursa 

Određuje put na kojoj je postavljen na disku resursa. Imajte na umu *privremene* diska resursa disk, a možda ispraznili kada se VM se uklanjaju resursi.


**ResourceDisk.MountOptions**  
Vrsta: niza  
Zadani: ništa

Određuje mogućnosti postavljanja diska će se proslijediti naredbu -o postavljanja. Ovo je popis vrijednosti odvojenih zarezom. "nodev, nosuid". Potražite u članku mount(8) detalje.


**ResourceDisk.EnableSwap:**  
Vrsta: Booleova  
Zadani: n

Ako postavite Zamjena datoteke (/ swapfile) je na disku resursa i dodati prostor zamjena sustava.


**ResourceDisk.SwapSizeMB:**  
Vrsta: cijeli broj  
Zadani: 0

Veličina datoteke Zamijeni u megabajtima.


**Logs.Verbose:**  
Vrsta: Booleova  
Zadani: n

Ako je boosted skup, verbosity zapisnika. Waagent prijavi /var/log/waagent.log i upravlja logrotate funkcionalnost sustava da biste zakrenuli zapisnika.


**OS. EnableRDMA**  
Vrsta: Booleova  
Zadani: n

Ako postavite, agenta će pokušati instaliranje i učitavanje upravljačkog programa za RDMA otklanjanje koja odgovara verziji firmver podlozi hardvera.


**OS. RootDeviceScsiTimeout:**  
Vrsta: cijeli broj  
Zadani: 300

To konfigurira SCSI vremensko ograničenje u sekundama na OS pogonima na disku i podatke. Ako ne postavite sustav koji se koriste zadane postavke.


**OS. OpensslPath:**  
Vrsta: niza  
Zadani: ništa

To se može koristiti da biste odredili zamjenski put za openssl binarni namijenjen šifriranja operacije.


**HttpProxy.Host, HttpProxy.Port**  
Vrsta: niza  
Zadani: ništa

Ako postavite, agenta će koristiti ovu proxy poslužitelj za pristup Internetu. 


## <a name="ubuntu-cloud-images"></a>Slike oblaka Ubuntu

Imajte na umu slika oblaka Ubuntu koristi funkciju [oblaka pokretanja](https://launchpad.net/ubuntu/+source/cloud-init) za izvršavanje mnogih zadataka konfiguracije koje bi inače upravlja Agent za Azure Linux.  Uzmite u obzir sljedeće razlike:


- **Provisioning.Enabled** po zadanom odabrana "n" na Ubuntu oblaka slike koje koriste oblaka pokretanja za obavljanje zadataka za dodjelu resursa.

- Sljedećih parametara konfiguracije imati utjecaja na Ubuntu oblaka slike koje koriste oblaka pokretanja da biste upravljali disk resursa i zamijeni prostor:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Pogledajte sljedeće resurse za konfiguriranje točku postavljanja na disku resursa i zamijeni prostor na slike oblaka Ubuntu prilikom dodjele resursa:

 - [Ubuntu Wiki: Konfiguriranje particije zamjena](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Injecting prilagođenih podataka u Azure virtualnog računala](virtual-machines-windows-classic-inject-custom-data.md)

