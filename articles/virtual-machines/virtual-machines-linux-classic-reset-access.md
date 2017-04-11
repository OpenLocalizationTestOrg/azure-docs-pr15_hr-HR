<properties
        pageTitle="Ponovno postavljanje lozinke Linux VM i SSH ključ iz na EŽA | Microsoft Azure"
        description="Kako koristiti proširenje VMAccess iz na Azure sučelje naredbenog retka (EŽA) za ponovno postavljanje lozinke Linux VM ili SSH ključ, rješavanje SSH konfiguraciju i provjera dosljednosti disk"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Kako ponovno postaviti lozinku Linux VM ili SSH ključ, rješavanje SSH konfiguraciju i provjera dosljednosti disk korištenje nastavka VMAccess


Ako ne uspijete povezati s Linux virtualnog računala na Azure zbog zaboravljene lozinke ključa netočan sigurne ljuske (SSH) ili problem s konfiguracijom SSH pomoću proširenje VMAccessForLinux EŽA Azure ponovno postavljanje lozinke ili SSH ključ, rješavanje SSH konfiguraciju i provjera dosljednosti disk. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

S EŽA Azure naredbom **azure vm proširenje postavljanje** iz naredbenog retka sučelja (tulumu, Terminal, naredbeni redak) za pristup naredbama. Pokrenite **azure pomoć vm proširenje postavljanje** za korištenje detaljne nastavak.

EŽA Azure možete učiniti sljedeće zadatke:

+ [Ponovno postavljanje lozinke](#pwresetcli)
+ [Ponovno postavljanje SSH ključa](#sshkeyresetcli)
+ [Ponovno postavljanje lozinke i tipku SSH](#resetbothcli)
+ [Stvaranje novog korisničkog računa sudo](#createnewsudocli)
+ [Ponovno postavljanje konfiguracije SSH](#sshconfigresetcli)
+ [Brisanje korisnika](#deletecli)
+ [Prikaz stanja VMAccess proširenja](#statuscli)
+ [Provjera dosljednosti dodane diskova](#checkdisk)
+ [Popravak diskova dodani na vaš VM Linux](#repairdisk)


## <a name="prerequisites"></a>Preduvjeti

Morat ćete učiniti sljedeće:

- Morat ćete [instalirati EŽA Azure](../xplat-cli-install.md) i [povezati pretplate](../xplat-cli-connect.md) da biste koristili Azure resursi koji su povezani s vašim računom.
- Postavite ispravan način rada za uvođenje klasičnog model upisivanjem sljedeće u naredbeni redak:
        
        azure config mode asm
        
- Imati novu lozinku ili skup SSH tipki, ako želite da vam ponovno postavi bilo. Ne morate ih ako želite da vam ponovno postavi SSH konfiguracije.


## <a name="pwresetcli"></a>Ponovno postavljanje lozinke

1. Stvorite datoteku pod nazivom PrivateConf.json s te retke. Zamjena uglatim zagradama i & #60; rezervirano mjesto & #62; vrijednosti s vlastitim podacima.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Pokrenite sljedeću naredbu, zamjenjujući naziv virtualnog računala za & #60; vm naziv & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Ponovno postavljanje SSH ključa

1. Stvorite datoteku pod nazivom PrivateConf.json te sadržajem. Zamjena uglatim zagradama i & #60; rezervirano mjesto & #62; vrijednosti s vlastitim podacima.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Pokrenite sljedeću naredbu, zamjenjujući naziv virtualnog računala za & #60; vm naziv & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Ponovno postavljanje lozinke i tipku SSH

1. Stvorite datoteku pod nazivom PrivateConf.json te sadržajem. Zamjena uglatim zagradama i & #60; rezervirano mjesto & #62; vrijednosti s vlastitim podacima.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Pokrenite sljedeću naredbu, zamjenjujući naziv virtualnog računala za & #60; vm naziv & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Stvaranje novog korisničkog računa sudo

Ako ste zaboravili svoje korisničko ime, VMAccess možete koristiti da biste stvorili novu s sudo za izdavanje certifikata. U ovom slučaju postojeće korisničko ime i lozinku neće mijenjati.

Da biste stvorili novog korisnika sudo pristup putem lozinke, koristite skriptu u [ponovno postavljanje lozinke](#pwresetcli) i navedite novo korisničko ime.

Da biste stvorili novog korisnika sudo SSH tipki pristupa, koristite skriptu u [Ponovno postavi tipku SSH](#sshkeyresetcli) i navedite novo korisničko ime.

[Ponovno postavljanje lozinke i tipku SSH](#resetbothcli) možete koristiti i da biste stvorili novog korisnika s lozinku i SSH tipki pristupa.

## <a name="sshconfigresetcli"></a>Ponovno postavljanje konfiguracije SSH

Ako je SSH konfiguraciju u stanju neželjenih, mogu izgubiti i pristup na VM. Proširenje VMAccess možete koristiti da biste vratili konfiguraciju u zadano stanje. Da biste to učinili, samo morate postaviti ključ "reset_ssh" "TRUE". Proširenje će pokrenite SSH poslužitelj, otvorite priključak SSH na vašem VM i konfiguracija SSH vratiti na zadane vrijednosti. Korisnički račun (ime, lozinka ili tipke SSH) neće se promijeniti.

> [AZURE.NOTE] Konfiguracijska datoteka SSH koja će vratiti nalazi se na /etc/ssh/sshd_config.

1. Stvorite datoteku pod nazivom PrivateConf.json s sadržaja.

        {
        "reset_ssh":"True"
        }

2. Pokrenite sljedeću naredbu, zamjenjujući naziv virtualnog računala za & #60; vm naziv & #62;. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Brisanje korisnika

Ako želite da biste izbrisali korisnički račun bez prijave da biste na VM izravno, možete koristiti ovu skriptu.

1. Stvorite datoteku pod nazivom PrivateConf.json sa sadržajem, zamjenjujući korisničko ime da biste uklonili za & #60; usernametoremove & #62;. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Pokrenite sljedeću naredbu, zamjenjujući naziv virtualnog računala za & #60; vm naziv & #62;. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>Prikaz stanja VMAccess proširenja

Da biste prikazali status proširenje VMAccess, pokrenite sljedeću naredbu.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< naziv = 'checkdisk' <</a>Provjera dosljednosti dodane diskova

Da biste pokrenuli fsck na sve diskova na računalu virtualnim Linux, morat ćete učiniti sljedeće:

1. Stvorite datoteku pod nazivom PublicConf.json s sadržaja. Provjerite na disku traje Booleove vrijednosti za provjeru diskova priložiti virtualnog računala ili ne. 

        {   
        "check_disk": "true"
        }

2. Pokrenite sljedeću naredbu za izvršavanje, zamjenjujući naziv virtualnog računala za & #60; vm naziv & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Popravak diskova 

Da biste popravili diskova koje su postavljanje ili su pogrešaka u konfiguraciji postavljanja, ponovno postaviti pomoću VMAccess nastavak postavljanja konfiguracije na vašem računalu virtualne Linux. Zamjena naziv diska za & #60; yourdisk & #62;.

1. Stvorite datoteku pod nazivom PublicConf.json s sadržaja. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Pokrenite sljedeću naredbu za izvršavanje, zamjenjujući naziv virtualnog računala za & #60; vm naziv & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Daljnji koraci

* Da biste ponovno postavljanje lozinke ili SSH ključ pomoću cmdleta ljuske PowerShell Azure ili Voditelj resursa Azure predložaka, rješavanje konfiguracije SSH i provjera dosljednosti disk, potražite u [dokumentaciji proširenje VMAccess na GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* [Azure portal](https://portal.azure.com) možete koristiti i ponovno postavljanje lozinke ili SSH ključ Linux VM implementiran u modelu klasični implementacije. Trenutno ne možete koristiti portala učinite ovom za Linux VM implementiran u modelu implementacije Voditelj resursa.

* Dodatne informacije o korištenju VM proširenja za Azure virtualnih računala, pogledajte [članak proširenja virtualnog računala i značajke](virtual-machines-linux-extensions-features.md) .
