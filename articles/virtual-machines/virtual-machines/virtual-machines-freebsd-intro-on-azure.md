<properties
   pageTitle="Uvod u FreeBSD na Azure | Microsoft Azure"
   description="Dodatne informacije o korištenju FreeBSD virtualnim strojevima na Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>Uvod u FreeBSD na Azure
Ova tema sadrži pregled radi FreeBSD virtualnog računala u Azure.

## <a name="overview"></a>Pregled
FreeBSD za Microsoft Azure je operacijski sustav napredne računalo koristi za power Moderna poslužitelji stolnih računala, a ugrađene platforme. Slika FreeBSD 10.3 omogućuje tvrtka Microsoft Corporation, a nalazi se u Azure. Se temelji na izdanje FreeBSD 10.3 i Azure VM goste [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) je instaliran Agent. Odgovoran za komunikaciju između FreeBSD VM i Azure tkanina za operacije, kao što su dodjeljivanje VM prilikom prvog korištenja (korisničko ime, lozinku, naziv glavnog računala, itd.) i omogućivanje funkcije za selektivno VM proširenja je agenta.
Kao budućim verzijama sustava FreeBSD na strategije je ostati u toku i dostupnim najnovija izdanja uskoro nakon objavljivanja po FreeBSD izdanje tim. Budućem izdanju je [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>Implementacija FreeBSD virtualnog računala
Implementacija FreeBSD virtualnog računala je jasan postupak korištenja slike iz [Trgovine Windows Azure](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/).

## <a name="vm-extensions-for-freebsd"></a>VM proširenja za FreeBSD
Slijede podržani VM proširenja za FreeBSD.

### <a name="vmaccess"></a>VMAccess

Proširenje [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) učiniti sljedeće:

- Ponovno postavljanje lozinke izvorne sudo korisnika.
- Stvaranje novog korisnika sudo zbog lozinke koja je navedena.
- Postavljanje ključa javno glavno računalo s ključem dali.
- Ponovno postavljanje javnog glavno računalo ključ tijekom VM dodjeljivanja ako tipku glavno računalo nije naveden.
- Otvorite priključak SSH (22) i vraćanje u sshd_config ako je reset_ssh postavljen na true.
- Uklanjanje postojećeg korisnika.
- Provjerite diskova.
- Popravite dodane na disku.

### <a name="customscript"></a>CustomScript

Proširenje [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) učiniti sljedeće:

- Ako je navedena preuzmite prilagođene skripte iz spremišta Azure ili vanjski javno prostor za pohranu (na primjer, GitHub).
- Pokrenuti skriptu točke unosa.
- Podržava naredbe u istoj razini.
- Automatski pretvoriti čuva Windows stil u ljusci i Python skripti.
- Uklanjanje Sastavnice u ljusci i skripte Python automatski.
- Zaštita povjerljive podatke u CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Provjere autentičnosti: korisnička imena, lozinke i SSH tipke
Kada stvarate FreeBSD virtualnog računala pomoću portala za Azure, navedite korisničko ime, lozinka ili SSH javni ključ.
Korisnička imena za implementaciju FreeBSD virtualnog računala na Azure mora odgovarati imena Sistemski računi (UID < 100) već postoje u virtualnog računala ("korijenskog," na primjer).
Trenutno je podržano samo RSA SSH ključ. SSH ključa s više redaka moraju započinjati "---početak SSH2 JAVNI KLJUČ---" i završavaju s "---ZAVRŠETKA SSH2 JAVNI KLJUČ---".

## <a name="obtaining-superuser-privileges"></a>Nabavljanje superkorisnik ovlasti
Korisnički račun koji nije naveden tijekom implementacije instancu virtualnog računala na Azure je povlaštene račun. Paket sudo je bio instaliran objavljene FreeBSD slici.
Kada ste prijavljeni putem ovaj korisnički račun, možete pokrenuti naredbe kao korijen pomoću Sintaksa naredbe.

    # sudo <COMMAND>

Po želji možete dobiti korijenski ljuske pomoću sudo -s.

## <a name="next-steps"></a>Daljnji koraci
- Idite na [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) da biste stvorili FreeBSD VM.
- Ako želite stavljanje vlastitog FreeBSD Azure, pročitajte [Stvaranje i prijenos FreeBSD VHD za Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).
