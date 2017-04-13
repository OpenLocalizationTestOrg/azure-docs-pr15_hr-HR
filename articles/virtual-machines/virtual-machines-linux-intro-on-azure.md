<properties
    pageTitle="Uvod u Linux u Azure | Microsoft Azure"
    description="Dodatne informacije o korištenju Linux virtualnim strojevima na Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

#<a name="introduction-to-linux-on-azure"></a>Uvod u Linux na Azure

Ova tema sadrži pregled nekih aspektima korištenja Linux virtualnih računala u oblaku Azure. Implementacija Linux virtualnog računala je jasan postupak korištenja slike iz galerije.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Provjere autentičnosti: Korisnička imena, lozinke i SSH tipke

Prilikom stvaranja na Linux virtualnog računala pomoću portala za Azure klasični su zatražiti da navede korisničko ime, lozinka ili SSH javni ključ. Odabir korisničko ime za implementaciju Linux virtualnog računala na Azure podložni sljedeća ograničenja: imena Sistemski računi (UID < 100) već prikazivanju virtualnog računala nisu dozvoljene 'korijenski', primjerice.


 - Potražite u članku [Stvaranje virtualnog računala koja se izvodi Linux](virtual-machines-linux-quick-create-cli.md)
 - Informirajte [se o korištenju SSH s Linux na Azure](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Nabavljanje superkorisnik ovlasti pomoću`sudo`

Korisnički račun koji nije naveden tijekom implementacije instancu virtualnog računala na Azure je povlaštene račun. Taj račun konfiguriran je po Linux Agent Azure moći pretvoriti ovlasti za korištenje korijenske (superkorisnik račun) u `sudo` utility. Kada prijavljeni putem ovoga korisničkog računa, moći za izvođenje naredbe kao korijen pomoću Sintaksa naredbe

    # sudo <COMMAND>

Po želji možete dobiti korijenski ljuske koristeći **sudo s**.

- Potražite u članku [Korištenje korijenske ovlasti na virtualnim računalima sustava Linux servisu Azure](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Konfiguriranje vatrozida

Azure nudi filtar za dolazni paketa kojem se ograničava povezivanje priključke naveden na portalu za Azure klasični. Prema zadanome, samo dopuštene priključak je SSH. Pristup dodatnim priključke na vašem računalu virtualnim Linux mogu se otvoriti konfiguriranjem krajnje točke na portalu za Azure klasični:

 - Pročitajte sljedeće: [Postavljanje krajnje točke za virtualnog računala](virtual-machines-windows-classic-setup-endpoints.md)

Slika Linux u galeriji Azure omogućiti vatrozid *iptables* prema zadanim postavkama. Po želji vatrozida može se konfigurirati za pružanje dodatnih filtriranja.


## <a name="hostname-changes"></a>Naziv glavnog računala promjene

Ako pokrenete prethodno instance komponente Linux slike, ćete morati Navedite naziv glavnog računala virtualnog računala. Kada je pokrenut virtualnog računala, ovog glavnog računala Objavi DNS poslužitelji platformu tako da više virtualnim strojevima međusobno povezani izvršava IP adresa pretraživanja pomoću hostnames.

Po želji naziv glavnog računala promjene se nakon postavila virtualnog računala, koristite naredbu

    # sudo hostname <newname>

Agent za Azure Linux sadrži funkciju automatski prepoznati tu promjenu naziva i prikladno konfiguriranje virtualnog računala zadržava te promjene i objavite tu promjenu DNS poslužitelji platforme.

 - [Agent za Azure Linux korisničkom priručniku](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Oblak pokretanja
Slike **Ubuntu** i **CoreOS** koristi funkciju oblaka pokretanja na Azure, koji pruža dodatne mogućnosti za pokretački virtualnog računala.

 - [Kako ubaciti prilagođenih podataka](virtual-machines-windows-classic-inject-custom-data.md)
 - [Prilagođene podatke i oblaka pokretanja na Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Stvaranje particije Azure zamjena pomoću oblaka pokretanja](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Upute za korištenje CoreOS na Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Snimanje slika virtualnog računala

Azure nudi mogućnost snimanja stanja postojeće virtualnog računala u slike koje je moguće naknadno koristiti za implementaciju instance dodatne virtualnog računala. Agent za Azure Linux možda koristi za vraćanje neke prilagodbe koju je izvršila tijekom postupka dodjele resursa. Možda slijedite korake u nastavku da biste snimili virtualnog računala kao sliku:

1. Pokrenite **waagent-deprovision** da biste poništili dodjelu resursa prilagodbe. Ili **waagent-deprovision + korisnika** po želji, brisanje korisničkog računa navedena tijekom dodjele resursa i sve povezane podatke.

2. Isključivanje dolje/isključeno virtualnog računala.

3. Kliknite *snimiti* na portalu za Azure klasični ili koristite Powershell ili EŽA alata za snimanje virtualnog računala kao sliku.

 - Pročitajte sljedeće: [kako snimiti Linux virtualnog računala da biste koristili kao predloška](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Prilaganje diskova

Svaki virtualnog računala ima privremene, lokalni *disk resursa* priložene. Budući da se podaci na disku resursa možda neće biti durable preko ponovna, često koristi aplikacije i procesa koji se izvode u virtualnog računala za prolaznim i **privremene** pohrane podataka. Također se koristi za spremanje stranice ili Zamjena datoteke za operacijski sustav.

Na Linux, resursa disk je obično upravlja Azure Linux Agent i automatski postavljen na **/mnt/resource** (ili **/mnt** na slikama Ubuntu).


>[AZURE.NOTE] Imajte na umu da disk resursa **privremene** diska, možda biti izbrisane i preoblikovati kada je na VM sustava.

Na Linux podatkovni disk može biti pod nazivom Jezgra sustava kao `/dev/sdc`, i korisnici će morati particije, oblikovanje i postavljanje tog resursa. To je prekriveno korak po korak u ovom praktičnom vodiču: [kako priložiti podatkovni Disk na virtualnog računala](virtual-machines-linux-classic-attach-disk.md).

 - **Vidi također:** [Konfiguriranje softver RAID na Linux](virtual-machines-linux-configure-raid.md)  &  [Konfiguriranje LVM na Linux VM servisu Azure](virtual-machines-linux-configure-lvm.md)

