<properties 
    pageTitle="Azure višestruka provjera autentičnosti - načina funkcioniranja tijeka rada"
    description="Azure višestruka provjera autentičnosti olakšava zaštitu pristup s podacima i aplikacijama dok korisnik zahtjev za jednostavne postupak prijave za sastanak. Pruža dodatne sigurnosti uz obaveznu drugi obrazac provjere autentičnosti i nudi Jaka provjera autentičnosti putem raspon mogućnosti jednostavno provjere."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>Kako funkcionira Azure višestruka provjera autentičnosti

Sigurnost višestruka provjera autentičnosti leži u njegov Slojeviti pristup. Ugrozi više čimbenika provjere autentičnosti predstavlja vrlo test za attackers. Čak i ako napadaču upravlja da biste saznali korisničku lozinku, je beskorisno i bez ima pouzdani uređaj. Treba li korisnika izgubite uređaj, osoba koja je pronalazi nećete moći koristiti osim ako on i zna se korisnikova lozinka.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure višestruka provjera autentičnosti olakšava zaštitu pristup s podacima i aplikacijama dok korisnik zahtjev za jednostavne postupak prijave za sastanak.  Pruža dodatne sigurnosti uz obaveznu drugi obrazac provjere autentičnosti i nudi Jaka provjera autentičnosti putem raspon jednostavno Provjera mogućnosti:

- telefonski poziv
- tekstne poruke
- mobilne aplikacije obavijesti – dopuštanje korisnicima da biste odabrali način radije
- kod za potvrdu aplikacije za mobilne uređaje
- 3 tokena OATH za zabavu

Dodatne informacije o kako to funkcionira potražite u članku ovom videozapisu.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Dostupni načini za multi-factor authentication
Kada korisnik prijavi, dodatne provjere šalje se korisniku.  U nastavku slijedi popis metode koje se mogu koristiti za ovaj pokretanja zvuka.

Način provjere  | Opis
------------- | ------------- |
Telefonski poziv | Da biste korisničke Pametni telefon da biste zatražili da biste potvrdili da su se prijavljujete pritiskom na znak # uspostave poziva.  To će dovršiti postupak provjere valjanosti.  Ova mogućnost može se konfigurirati i moguće je promijeniti u kod koji navedete.
Tekstne poruke | Tekstne poruke poslat će se korisničke Pametni telefon kod 6 znamenke.  Unesite kod u da biste dovršili postupak provjere valjanosti.
Obavijest o mobilnoj aplikaciji | Zahtjev za potvrdu poslat će se korisničke Pametni telefon da biste zatražili dovršeno Provjera tako da odaberete potvrda iz mobilne aplikacije. To će se dogoditi ako ste odabrali obavijesti aplikacija kao način primarni provjere.  Ako se prikaže to kada se ne prijavljujete, možete odabrati da biste se prijavili kao prijevara.
Kod za potvrdu pomoću mobilne aplikacije | Kod za potvrdu poslat će se mobilne aplikacije koji se izvodi na korisničke Pametni telefon.  To će se dogoditi ako ste odabrali kod za potvrdu kao način primarni provjere.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Verzija Azure višestruka provjera autentičnosti
Azure višestruka provjera autentičnosti dostupan je u tri različite verzije.  U tablici u nastavku opisuju svaki od ovih detaljnije.

Verzija  | Opis
------------- | ------------- |
Višestruka provjera autentičnosti za Office 365 | Ova verzija funkcionira isključivo s aplikacijama sustava Office 365 i upravlja se putem portala sustava Office 365. Da bi se sada omogućuju administratorima sigurne njihove resursima sustava Office 365 pomoću višestruke provjere autentičnosti.  Ova verzija u sklopu pretplate na Office 365.
Višestruka provjera autentičnosti za administratore za Azure | Isti podskup mogućnosti višestruke provjere autentičnosti za Office 365 će biti dostupna uz bez naknadu svim administratorima Azure. Svaki administrativnog računa Azure pretplate sada možete dobiti dodatnu zaštitu tako što ćete omogućiti ta je funkcija core višestruke provjere autentičnosti. Kako administrator želi da biste pristupili Azure portal da biste stvorili VM web-mjesta, Upravljanje prostorom za pohranu, mobilnih usluga ili drugog servisa Azure možete dodati višestruke provjere autentičnosti svoj administratorski račun.
Azure višestruka provjera autentičnosti | Azure višestruka provjera autentičnosti nudi richest skup mogućnosti. <br><br>Pruža dodatne konfiguracijske mogućnosti putem na portal za upravljanje Azure, dodatno izvješćivanje i podrška za raspon lokalnih podataka i aplikacije u oblaku. Azure višestruka provjera autentičnosti možete kupiti kao samostalni licence i je vezanoj instalaciji unutar Azure Active Directory Premium i paket mobilnost za tvrtke. <br><br>Ga mogu kupiti na temelju potrošnje stvaranjem davateljem Azure višestruke provjere autentičnosti u Azure pretplate.
##<a name="feature-comparison-of-versions"></a>Značajka usporedbe verzija
U sljedećoj su tablici ispod nalaze se popis značajke koje su dostupne u različitim verzijama Azure višestruke provjere autentičnosti.


Značajka  | Višestruka provjera autentičnosti za Office 365 (uvršteni su u Office 365 SKU-ove)|Višestruka provjera autentičnosti za administratore Azure (dio Azure pretplate na) | Azure višestruka provjera autentičnosti (u sklopu Azure AD Premium i paket mobilnost za tvrtke)
------------- | :-------------: |:-------------: |:-------------: |
Administratori mogu zaštititi računa s MFA| * | * (Dostupno samo za Azure administratorske račune)|*
Mobilne aplikacije kao drugi faktor|* | * | *
Telefonski poziv kao drugi faktor|* | * | *
SMS kao drugi faktor|* | * | *
Aplikacija lozinke za klijente koji ne podržavaju MFA|* | * | *
Administrator kontrolu nad metoda provjere autentičnosti| *| *| *
Način PIN-a| | | *
Upozorenje prijevara| | | *
MFA izvješća| | | *
Jednokratno zaobilazak| | | *
Prilagođeni čestitke za telefonske pozive| | | *
Prilagodba ID pozivatelja za telefonske pozive| | | *
Potvrda događaja| | | *
Pouzdani IP-ovi| | | *
Privremeno obustavljanje MFA zapamćenih uređaja (javno pretpregled)| | | *
MFA SDK| | | *
MFA za lokalni aplikacije pomoću poslužitelja za MFA| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Kako nabaviti Azure višestruka provjera autentičnosti

Želite li punu funkcionalnost telefona nudi Azure višestruka provjera autentičnosti umjesto samo one za korisnike sustava Office 365 i Azure administratori, postoji nekoliko mogućnosti da biste podesili:

1.  Kupite licence Azure višestruke provjere autentičnosti i dodijeliti korisnicima.
2.  Kupite licence koje imaju Azure višestruka provjera autentičnosti vezanoj instalaciji unutar njih kao što su Azure Active Directory Premium ili paket Enterprise mobilnost i dodijeliti korisnicima.
3.  Stvaranje Azure višestruke provjere autentičnosti davatelja usluge razmjene Azure pretplate. Ako već nemate Azure pretplatu, možete se prijaviti za probnu pretplatu na Azure. Probne pretplate morat će pretvoreni u pravilnim pretplate prije isteka probne.

Prilikom korištenja davatelja Azure višestruke provjere autentičnosti postoje dvije korištenje modela dostupna koji se naplaćuju putem pretplate Azure:


- **Po korisniku**. Općenito za velike tvrtke koje želite omogućiti višestruke provjere autentičnosti za fiksnim brojem zaposlenike koji redovito potrebna provjera autentičnosti.
- **Po provjere autentičnosti**. Općenito za velike tvrtke koje želite omogućiti višestruke provjere autentičnosti za veliku skupinu vanjski korisnici koji diskovni potrebna provjera autentičnosti.

Cijene detalje potražite u članku [cijene za Azure MFA.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Odaberite po licenci ili utemeljen na potrošnje modela koja najbolje odgovara vašoj tvrtki ili ustanovi.   Na početak rada potražite u članku [Početak rada](multi-factor-authentication-get-started.md)
