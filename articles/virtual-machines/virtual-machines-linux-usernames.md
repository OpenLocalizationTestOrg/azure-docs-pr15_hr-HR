<properties 
    pageTitle="Odaberite imena korisnika za Linux | Microsoft Azure" 
    description="Saznajte kako u Azure odaberite imena korisnika za Linux virtualnog računala." 
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



#<a name="selecting-user-names-for-linux-on-azure"></a>Odaberite imena korisnika za Linux na Azure#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Prilikom dodjele resursa Linux virtualnog računala na Azure morate navesti ime korisnika koji nisu korijenski koje možete koristiti kasnije prijaviti na VM. Možete odabrati naziv novog korisnika ili dodjeljivanje putem portala za Azure klasični odgovarat zadani naziv "azureuser".

U većini slučajeva taj korisnik ne postoji osnovni sliku i stvorit će se tijekom postupka dodjele resursa. Ako korisnik postoji osnovni VM sliku, zatim agent za Azure Linux jednostavno konfigurira lozinku i/ili SSH ključ za tog korisnika na temelju informacija koje ste naveli prilikom stvaranja na VM.

**Međutim**, Linux definira skup korisnička imena koja će se koristiti. Postupak za dodjelu resursa će **neće uspjeti** ako pokušate Dodjela Linux VM pomoću postojećem korisniku sustava, koja je definirana kao korisnik s UID 0 99. Uobičajeni primjer je u `root` korisnika koji ima UID 0.

 - Vidi također: [rasponi Linux Standard baze - korisničkog ID-a](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Slijedi popis uobičajenih korisnika ugrađene sustava za CentOS i Ubuntu da izbjegavajte upotrebu prilikom dodjele resursa Linux virtualnog računala na Azure. Ovaj je popis samo primjer, potražite u dokumentaciji sustava raspodjele da biste bili sigurni da korisničko ime koje ste odabrali nisu u sukobu s postojećeg korisnika sustava.


## <a name="centos"></a>CentOS
- abrt
- Admin
- zvuk
- smeće
- CDROM
- cgred
- daemon
- dbus
- dialout
- dip
- na disku
- Meki
- FTP
- FTP
- igre
- gopher
- haldaemon
- Zastoj
- kmem
- Zaključavanje
- LP
- pošta
- Muškarac
- memorije
- nfsnobody
- nitko
- NTP
- operator
- oprofile
- postdrop
- postfix
- qpidd
- Korijenska
- RPC
- rpcuser
- saslauth
- isključivanje
- slocate
- sshd
- stapdev
- stapusr
- Sinkronizacija
- sistemskim
- Vrpca
- Test
- tcpdump
- TTY
- korisnici
- utempter
- utmp
- uucp
- vcsa
- videozapis
- kotačić


## <a name="ubuntu"></a>Ubuntu
- Admin
- administrator
- zvuk
- sigurnosno kopiranje
- smeće
- CDROM
- crontab
- daemon
- dialout
- dip
- na disku
- faksa
- Meki
- osigurač
- igre
- gnats
- IRC
- kmem
- Vodoravno
- libuuid
- Popis
- LP
- pošta
- Muškarac
- messagebus
- mlocate
- netdev
- Novosti
- nitko
- nogroup
- operator
- plugdev
- proxy poslužitelj
- Korijenska
- sasl
- sjene
- src
- ssh
- sshd
- osoblje
- sudo
- Sinkronizacija
- sistemskim
- syslog
- Vrpca
- TTY
- korisnici
- utmp
- uucp
- videozapis
- glas
- whoopsie
- "www" podataka

 