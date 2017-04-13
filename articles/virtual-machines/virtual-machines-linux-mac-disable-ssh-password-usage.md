<properties
    pageTitle="Onemogućivanje SSH lozinke na vašem VM Linux konfiguriranjem SSHD | Microsoft Azure"
    description="Sigurne vaše VM Linux na Azure onemogućivanjem prijave lozinku za SSH."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Onemogućivanje SSH lozinke na vašem VM Linux konfiguriranjem SSHD

U ovom se članku usredotočuje se na kako zaključati prijava sigurnosti vaše VM Linux.  Čim priključak SSH 22 otvara se na početak robotima svijeta pokušaja prijave pogađanjem lozinke.  Što ćemo napraviti u ovom članku je onemogućivanje prijave lozinku putem SSH.  Potpuno uklanjanjem mogućnost korištenja lozinki smo zaštiti Linux VM iz ove vrste brute prisilno napada.  Dodane dodatni je smo će konfigurirati Linux SSHD da biste omogućili samo prijave putem SSH javni i privatni ključevi, uz znatno najsigurnija način za prijavu Linux.  Moguće kombinacije to je potrebna pogoditi privatni ključ je immense i stoga discourages robotima iz even bothering da biste pokušali brute prisilno SSH tipke.


## <a name="goals"></a>Ciljevi

- Konfiguriranje SSHD da bi se onemogućilo:
  - Lozinka prijave
  - Korijenska korisničko ime za prijavu
  - Provjera autentičnosti odgovora
- Konfiguriranje SSHD da biste omogućili:
  - samo SSH ključa prijave
- Ponovno pokrenite SSHD dok ste prijavljeni i dalje
- Testiranje Nova konfiguracija SSHD

## <a name="introduction"></a>Uvod

[SSH definirani](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD je SSH poslužitelja koja se izvršava na Linux VM.  SSH je klijent koji se izvodi iz u ljusci na vaše radne stanice uređaja MacBook ili Linux.  SSH je i protokol koji se koristi za zaštitu i šifriranje komunikaciju između vaše radne stanice i Linux VM.

U ovom se članku vrlo je važno da biste zadržali jedan prijavite se na vaše VM Linux otvaranje za cijelu vodič kroz.  Zbog toga ne možemo otvorit će dva WBT i SSH Linux VM iz oba.  Koristit ćemo prvi terminal unesite željene promjene SSHDs konfiguracijska datoteka i ponovno pokrenite servis SSHD.  Koristit ćemo drugi terminal da biste testirali te promjene nakon ponovnog pokretanja servisa.  Jer smo se onemogućivanjem SSH lozinke i potrebe za oslanjanjem isključivo na SSH tipke, ako ključeva SSH nisu ispravni, a zatim zatvorite veza na VM, na VM bit će trajno zaključana, a nitko će moći prijava da biste je potrebno da bi se briše i ponovno stvoriti.

## <a name="prerequisites"></a>Preduvjeti

- [Stvaranje SSH tipke na Linux i Mac za Linux VMs servisu Azure](virtual-machines-linux-mac-create-ssh-keys.md)
- Račun za Azure
  - [Besplatna probna prijava](https://azure.microsoft.com/pricing/free-trial/)
  - [Portal za Azure](http://portal.azure.com)
- Linux VM sustavom azure
- SSH javnim i privatnim ključ par u`~/.ssh/`
- Javni ključ SSH u `~/.ssh/authorized_keys` na Linux VM
- Sudo prava na na VM
- Priključak 22 Otvori

## <a name="quick-commands"></a>Brzi naredbe

_Seasoned Linux administratori koji želite samo verzije TLDR, počnite ovdje.  Za sve druge želi Detaljno objašnjenje i vodič kroz, preskočite ovaj odjeljak._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Detaljni vodič kroz

Prijavite se na VM Linux na terminal 1 (T1).  Prijavite se na VM Linux terminal 2 (T2).

Na T2 ćemo prolaze da biste uredili konfiguracijska datoteka SSHD.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Na tom mjestu ne možemo će urediti samo postavke SSH ključa prijave za omogućivanje i onemogućivanje lozinke.  Nema više postavki u datoteku koja se treba istražiti i promjena da biste zaštitili kad su vam potrebne Linux & SSH.

#### <a name="disable-password-logins"></a>Onemogućivanje prijave lozinku

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Omogućivanje javni ključ za provjeru autentičnosti

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Onemogućivanje prijave korijenski

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Onemogućite provjeru autentičnosti odgovora

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Ponovno pokrenite SSHD

Iz ljuske T1 provjerite da i dalje prijavljen na.  To je važnosti tako da se neće dobiti zaključane izvan vaše VM ako ključeva SSH nisu ispravni jer sada onemogućuju lozinke.  Ako je bilo koje postavke nisu ispravni na vaše VM Linux T1 možete koristiti da biste riješili problem sshd_config kao koju će i dalje biti prijavljeni i SSH sačuvat će vezu aktivnosti tijekom servis SSHD ponovno pokrenite.

Iz T2 pokretanje:

##### <a name="on-the-debian-family"></a>Na Debian obitelj

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Na RedHat obitelj

```
username@macbook$ sudo service sshd restart
```

Na vaše VM zaštiti od pokušaja prijave lozinku brute prisilno sada onemogućena lozinke.  Ključeve samo SSH dopušteno bit ćete moći prijava brže i mnogo sigurnija.
