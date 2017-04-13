<properties 
    pageTitle="Korištenje korijenske ovlasti na virtualnim strojevima Linux | Microsoft Azure" 
    description="Saznajte kako koristiti korijenski ovlasti na računalu virtualnim Linux u Azure." 
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


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Korištenje korijenski ovlasti na virtualnim strojevima Linux servisu Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Po zadanom se `root` korisnik onemogućena Linux virtualnim strojevima u Azure. Korisnicima možete pokrenuti naredbi s dodatnim ovlastima pomoću na `sudo` naredbe. Međutim, sučelje mogu se razlikovati ovisno o tome kako je dodijeljena sustav.

1. **Web-mjesto SSH ključ i u okvir za lozinku ili lozinku samo** – virtualnog računala je dodijeljena s potvrdom (`.CER` datoteke) ili SSH ključ kao i lozinku, ili samo korisničko ime i lozinku. U ovom slučaju `sudo` će se korisnikova lozinka traže prije izvršavanja naredbu.

2. **Samo s ključem SSH** - virtualnog računala je dodijeljena pomoću certifikata (`.cer`, `.pem`, ili `.pub` datoteke) ili SSH ključa, ali bez lozinke.  U ovom slučaju `sudo` **neće** Upitaj koji se korisnikova lozinka prije izvršavanja naredbu.


## <a name="ssh-key-and-password-or-password-only"></a>SSH ključ i lozinka ili samo lozinke

Prijavite se u Linux virtualnog računala pomoću provjere autentičnosti ključ ili lozinka SSH, a zatim pokrenite naredbi pomoću `sudo`, na primjer:

    # sudo <command>
    [sudo] password for azureuser:

U ovom slučaju korisnika će se tražiti lozinku. Kada unesete lozinku `sudo` će pokrenuti naredbu s `root` ovlasti.

Passwordless sudo možete omogućiti tako da uredite u `/etc/sudoers.d/waagent` datoteke, na primjer:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Ta promjena će omogućuju passwordless sudo korisnik "azureuser".

## <a name="ssh-key-only"></a>SSH ključa samo

Prijavite se u Linux virtualnog računala pomoću ključ za provjeru autentičnosti SSH, a zatim pokrenite naredbi pomoću `sudo`, na primjer:

    # sudo <command>

U ovom slučaju će korisnik **ne** može zatražiti unos lozinke. Nakon pritisak `<enter>`, `sudo` će pokrenuti naredbu s `root` ovlasti.

 
