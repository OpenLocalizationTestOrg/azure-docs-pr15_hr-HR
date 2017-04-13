<properties
    pageTitle="Stvaranje SSH tipke Linux i Mac | Microsoft Azure"
    description="Stvaranje i korištenje SSH tipke na Linux i Mac Voditelj resursa i uvođenje klasičnog modelima na Azure."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Stvaranje SSH tipke na Linux i Mac za Linux VMs servisu Azure

Pomoću programa keypair SSH virtualnim strojevima možete stvoriti na Azure koji je prema zadanim postavkama pomoću tipki SSH za provjeru autentičnosti, bez potrebe za lozinke za prijavu.  Lozinke možete pogoditi i otvorite svoje VMs do pokušaja relentless brute prisilno pogoditi lozinku. VMs stvorena pomoću predložaka Azure ili `azure-cli` mogu sadržavati javni ključ SSH kao dio implementacije, uklanjanjem konfiguracije za implementaciju objave.  Ako se povezujete s Linux VM iz sustava Windows, pročitajte članak [ovaj dokument.](virtual-machines-linux-ssh-from-windows.md)

U članku zahtijeva:

- Azure račun ([dobiti besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure EŽA](../xplat-cli-install.md) prijavljeni`azure login`

- način Azure EŽA _mora biti u_ Voditelj resursa za Azure`azure config mode arm`

## <a name="quick-commands"></a>Brzi naredbe

U sljedeće naredbe zamijenite Primjeri vlastite mogućnosti.

SSH tipke su po zadanom znali što se `.ssh` direktorija.  

```bash
cd ~/.ssh/
```

Ako nemate na `~/.ssh` direktorija u `ssh-keygen` naredba će Stvori je za vas odgovarajuće dozvole.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Unesite naziv datoteke koja je spremljena u na `~/.ssh/` direktorija:

```bash
id_rsa
```

Da biste unijeli pristupni izraz za id_rsa:

```bash
correct horse battery staple
```

Sada je na `id_rsa` i `id_rsa.pub` SSH ključa par u na `~/.ssh` direktorija.

```bash
ls -al ~/.ssh
```

Dodavanje novostvoreni ključ `ssh-agent` na Linux i Mac (dodaju se spremišta lozinki OSX):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Kopirajte javni ključ SSH s poslužiteljem Linux:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Testiranje prijavu pomoću tipki umjesto lozinku:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Detaljni vodič

Korištenje SSH javni i privatni ključevi je najjednostavniji način da biste se prijavili poslužitelja Linux. [Javni ključ šifriranja](https://en.wikipedia.org/wiki/Public-key_cryptography) omogućuje mnogo sigurnije da biste se prijavili Linux ili BSD VM u Azure od lozinki, što može biti Grube prisilno daleko jednostavnije. Javni ključ možete se zajednički koristi sa svima; No samo vi (ili infrastruktura za lokalne sigurnosne) imaju privatni ključ.  Privatni ključ SSH mora imati [vrlo sigurne lozinke](https://www.xkcd.com/936/) (izvor:[xkcd.com](https://xkcd.com)) da biste zaštitili ga.  Ova lozinka je pristup privatni ključ SSH i **nije** lozinke korisničkog računa.  Kada dodate lozinke ključ SSH šifrira privatni ključ tako da je beskorisno bez lozinke za otključavanje privatni ključ.  Ako napadaču otuđeno privatni ključ, a te tipke ne sadrži lozinke, će moći koristiti taj privatni ključ za prijavu na sve poslužitelje koji imaju odgovarajuće javni ključ.  Ako je privatni ključ zaštićena je ne mogu koristiti u tom napadač, koja omogućuje dodatnu razinu zaštite za vaše infrastrukture na Azure.

U ovom se članku stvara *ssh-rsa* oblikovani ključa datotekama, koji se preporučuje za implementacije na Voditelj resursa.  tipke *ssh-rsa* potrebni su [portal](https://portal.azure.com) za klasični i resursima implementacije.

## <a name="create-the-ssh-keys"></a>Stvaranje SSH tipke

Azure zahtijeva barem 2048-bitni, ssh-rsa oblikujte javni i privatni ključevi. Da biste stvorili pomoću tipke `ssh-keygen`, čime se postavlja niz pitanja, a zatim piše privatni ključ i odgovarajući javni ključ. Prilikom stvaranja programa Azure VM se kopira javni ključ `~/.ssh/authorized_keys`.  Tipke SSH u `~/.ssh/authorized_keys` koriste se za izazov klijent tako da odgovara odgovarajući privatni ključ na vezu za prijavu na SSH.

## <a name="using-ssh-keygen"></a>Korištenje ssh-keygen

Ta se naredba stvara lozinkom zaštićenim (šifrirane) SSH Keypair pomoću RSA 2048-bitni i se komentar da biste lakše prepoznali.  

Započnite tako da promijenite direktorija, tako da sve svoje ssh tipke stvaraju se u tom direktoriju.

```bash
cd ~/.ssh
```

Ako nemate na `~/.ssh` direktorija u `ssh-keygen` naredba će Stvori je za vas odgovarajuću dozvolu.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Naredba objašnjenje_

`ssh-keygen`= program koji se koristi za stvaranje tipki

`-t rsa`= vrste ključ da biste stvorili koji se u [RSA oblik](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bitova ključa

`-C "myusername@myserver"`= komentar dodan na kraj javnu ključa datoteku da biste lakše prepoznali.  Obično poruke e-pošte koja se koristi kao komentar, ali možete koristiti neku drugu najbolje odgovara vašem infrastrukture.

### <a name="using-pem-keys"></a>Pomoću tipki PEM

Ako koristite na klasični implementacija model (klasični Portal Azure ili EŽA za upravljanje servisa Azure `asm`), možda ćete morati koristiti PEM oblikovani SSH tipke za pristup vašem VMs Linux.  Evo kako stvoriti ključ PEM iz postojeće SSH javni ključ i postojeće x509 certifikata.

Da biste stvorili na PEM oblikovani ključ iz postojeće SSH javni ključ:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Primjer ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Spremanje ključa datoteke:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Naziv ključ za ovog članka.  Imate ključa par pod nazivom **id_rsa** je zadana postavka, a neki alati biste možda pomislili naziv datoteka s privatnim ključem **id_rsa** da imate dobra je ideja. Imenik `~/.ssh/` je zadano mjesto za ključne parove SSH i SSH konfiguracijskoj datoteci.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Popis na `~/.ssh` direktorija. `ssh-keygen`stvara se `~/.ssh` direktorija ako ga nema, a postavlja točan načini vlasništvo i datoteke.

Ključni lozinku:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`upućuje na lozinke kao "pristupni."  To je *Toplo* preporučuje se da biste dodali lozinku vaše ključa uparuje. Bez lozinke zaštita ključ, svatko tko ima datoteka privatnog ključa možete ga koristiti za prijavu bilo kojem poslužitelju koji sadrži odgovarajući javni ključ. Dodavanje lozinke štiti više u slučaju da je korisnik može ostvariti pristup privatni ključ datoteci vam dao vremena da biste promijenili tipki koristiti za ovjeru.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Korištenje ssh agenta za pohranu privatni ključ lozinku

Da biste izbjegli pravopisne lozinku datoteka s privatnim ključem sa svake prijave SSH, možete koristiti `ssh-agent` u predmemoriju datoteka s privatnim ključem lozinku. Ako koristite Mac, spremišta lozinki OSX sigurno pohranjuje privatni ključ lozinke kada pozovete `ssh-agent`.

Najprije provjerite `ssh-agent` je pokrenut

```bash
eval "$(ssh-agent -s)"
```

Sada dodajte privatni ključ koji se `ssh-agent` pomoću naredbe `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Privatni ključ lozinku sada je pohranjena `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Stvaranje i konfiguriranje programa SSH konfiguracijska datoteka

Je preporučena preporučenim načinom rada za stvaranje i konfiguriranje programa `~/.ssh/config` datoteku da biste ubrzali prijave dodaci i za optimiziranje vaše ponašanje SSH klijenta.

Sljedeći primjer prikazuje standardnu konfiguracije.

### <a name="create-the-file"></a>Stvaranje datoteke

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Uređivanje datoteke da biste dodali novu konfiguraciju SSH:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Primjer `~/.ssh/config` datoteke:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Ovaj config SSH daje sekcije za svaki poslužitelj da biste omogućili svaki imati vlastitu namjenski par ključa. Zadane postavke (`Host *`) su za sve domaćini koji ne odgovaraju nijednom određene domaćini prema gore u konfiguracijskoj datoteci.


### <a name="config-file-explained"></a>Konfiguracijska datoteka objašnjenje

`Host`= Naziv glavnog računala koja se poziva na na terminal.  `ssh fedora22`nalaže `SSH` da biste koristili vrijednosti u bloku postavke koje su označene `Host fedora22` Napomena: to može biti bilo koju oznaku koja je logički za Upotreba i predstavlja stvarni naziv glavnog računala poslužitelja za sve.

`Hostname 102.160.203.241`= IP adresa ili naziv DNS poslužitelja pristupa.

`User myusername`= Udaljeni korisnički račun koji ćete koristiti prilikom prijave na poslužitelj.

`PubKeyAuthentication yes`= govori SSH koji želite koristiti ključa SSH da biste se prijavili.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= SSH privatni ključ i odgovarajući javni ključ za provjeru autentičnosti.


## <a name="ssh-into-linux-without-a-password"></a>SSH u Linux bez lozinke

Sad kad ste programa SSH ključa par i konfiguriran SSH konfiguracijskoj datoteci, vam se prijaviti na vašem VM Linux brzo i sigurno. Prvo se prijavite s poslužiteljem pomoću ključa SSH upite naredbe koje za pristupni izraz te ključne datoteke.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Naredba dodataka

Kada `ssh fedora22` se izvršava SSH najprije pronalazi i učitava sve postavke na `Host fedora22` blok, a zatim opterećenje sve preostale postavke iz zadnjeg bloka `Host *`.

## <a name="next-steps"></a>Daljnji koraci

Sljedeći gore je da biste stvorili Azure Linux VMs pomoću nove SSH javni ključ.  Azure VMs koje su stvorene pomoću SSH javni ključ kao prijavu su bolje sigurna od VMs stvorena pomoću zadani način prijave lozinke.  Azure VMs stvoren pomoću tipke SSH su po zadanom je konfiguriran pomoću lozinke onemogućen, izbjegavanje Grube prisilno guessing pokušaja.

- [Stvaranje sigurnog Linux VM pomoću predloška za Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Stvaranje sigurnog Linux VM pomoću portala za Azure](virtual-machines-linux-quick-create-portal.md)
- [Stvaranje sigurnog Linux VM pomoću EŽA Azure](virtual-machines-linux-quick-create-cli.md)
