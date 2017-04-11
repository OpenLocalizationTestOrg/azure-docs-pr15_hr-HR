<properties
    pageTitle="Omogućivanje daljinskog pristupa za Azure implementacije u Eclipse"
    description="Upute za Omogućivanje daljinskog pristupa za Azure implementacije korištenje kompleta alata za Azure za Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Omogućivanje daljinskog pristupa za Azure implementacije u Eclipse

Da biste otklonili vaše implementacije, možete omogućiti i koristiti daljinski pristup za povezivanje s virtualnog računala hosting implementaciju sustava. Funkcija daljinski pristup oslanja na na Remote Desktop Protocol (RDP). Nakon što ste objavili na Azure ili ako koristite Eclipse s operacijskim sustavom Windows, možete konfigurirati daljinski pristup prije nego što objavite Azure, za implementaciju sustava možete konfigurirati daljinski pristup. Imajte na umu da ćete udaljene radne površine klijenta koji je kompatibilan s operacijskim sustavom radi povezivanja sa implementaciju sustava virtualnog računala u Azure.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Omogućivanje daljinskog pristupa prije implementacije Azure

> [AZURE.NOTE] Da biste omogućili daljinski pristup prije implementacije aplikaciju Azure, morate imati Eclipse u sustavu Windows.

Sljedeća slika prikazuje dijaloški okvir za Omogućivanje daljinskog pristupa svojstva **Daljinski pristup** .

![][ic719494]

Da biste prikazali dijaloški okvir svojstva za **Daljinski pristup** na dva načina:

* Kliknite vezu **Dodatno** u odjeljku **Daljinski pristup** dijaloški okvir **Objavi u Azure** .
* Otvorite dijaloški okvir **Svojstva** Azure projekta.

Prilikom stvaranja novog projekta Azure implementacije projekta neće imati daljinski pristup po zadanom omogućena. Međutim, daljinski pristup možete jednostavno omogućiti navođenjem korisničko ime i lozinku u dijaloški okvir **Objavi u Azure** . Lozinku za daljinski pristup je šifriran pomoću X.509 certifikate. Ako ne koristite unesite vlastiti certifikat šifriranja ovisi o samopotpisanog certifikata koji se isporučuju uz dodatak za Azure za Eclipse. U ovom samopotpisani certifikat je u mapi **certifikata** Azure projekta, spremljena kao datoteka javno certifikata (SampleRemoteAccessPublic.cer) i kao datoteka certifikata osobne podatke sustava Exchange (PFX) (SampleRemoteAccessPrivate.pfx). Drugu mogućnost sadrži privatni ključ za potvrdu, a ima zadani lozinku, **Password1**. Međutim, budući da ova lozinka je javno znanja, zadanu potvrdu trebali biste koristiti samo za učenju, ne za implementaciju radnog. Stoga osim za učenju, kada želite omogućiti udaljene sesije za implementacije, kliknite vezu **Dodatno** u dijaloškom okviru **Objavi na Azure** da biste odredili vlastiti certifikat. Imajte na umu da ćete morati prijenos PFX verziju certifikata na glavnom računalu usluge unutar portala za upravljanje Azure da taj Azure možete dešifrirati korisničku lozinku.

Ostatak vodič objašnjava Omogućivanje daljinskog pristupa za projekt programa Azure implementaciju koji je prethodno stvoren daljinski pristup onemogućene. Za potrebe ovog praktičnog vodiča, ćemo ćete stvoriti novi samopotpisani certifikat, a njegova .pfx datoteka će imati lozinke po izboru. Imate mogućnost korištenja certifikata koji je izdala ustanova za izdavanje certifikata.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Omogućivanje daljinskog pristupa nakon što ste implementiran na Azure

Da biste omogućili daljinski pristup nakon što ste implementiran na Azure, poduzmite sljedeće korake:

1. Prijavite se na portal za upravljanje Azure pomoću računa za Azure
1. Na popisu **Servise u Oblaku**, odaberite distribuiranih oblaku
1. U web-stranici servisa oblaka kliknite vezu **Konfiguriranje**
1. Na dnu stranice konfiguraciju, kliknite vezu **Remote**
1. Kada skočni se pojavi dijaloški okvir:
    * Odredite ulogu za koju želite da omogućite daljinski pristup
    * Klikom odaberite potvrdni okvir **Omogući udaljene radne površine**
    * Navedite korisničko ime i lozinku koju želite koristiti za daljinski pristup
    * Odaberite certifikat za korištenje
1. Kliknite **u redu** 

Prikazat će se poruka da je u tijeku, što može potrajati nekoliko minuta da biste dovršili je promjene konfiguracije. Po dovršetku konfiguracije promjena slijedite korake iz odjeljka **Da biste se prijavili daljinski** u nastavku ovog članka.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Omogućivanje daljinskog pristupa u vašem paketu

1. Project Explorer okna Eclipse, desnom tipkom miša kliknite Azure projekta, a zatim kliknite **Svojstva**.

1. U dijaloškom okviru **Svojstva** proširite **Azure** u lijevom oknu, a zatim kliknite **Daljinski pristup**.

1. U dijaloškom okviru **Daljinski pristup** provjerite je li potvrđen **Omogućivanje svih uloga da biste prihvatili udaljene radne površine veze s te vjerodajnice za prijavu** .

1. Navedite korisničko ime za povezivanje udaljene radne površine.

1. Odredite i potvrdite lozinku za korisnika. Korisničko ime i lozinku vrijednosti u ovom dijaloškom okviru koristit će se pri upućivanju veze udaljene radne površine. (Imajte na umu da je lozinka lozinku PFX.)

1. Navedite datum isteka za korisnički račun.

1. Kliknite **Novo** da biste stvorili novi samopotpisani certifikat. (Umjesto toga nije odnosno odaberite certifikat iz radnog prostora ili datoteke sustava putem gumba **radni prostor** ili **datotečnom sustavu** , ali ako za potrebe ovog praktičnog vodiča ćemo ćete stvoriti novi certifikat.)

    * U dijaloškom okviru **Novi certifikat** odredite i potvrdite lozinku koji će se koristiti za PFX datoteku.

    * Prihvatite vrijednost navedena za **Naziv (CN)**ili koristiti prilagođeni naziv.

    * Navedite put i naziv spremanje novi certifikat u obliku .cer. Za ovaj korak i na sljedeći korak, možete koristiti mapu **certifikata** Azure projekta, ali koje možete slobodno odaberite neko drugo mjesto. Za potrebe ovog praktičnog vodiča, ćemo koristiti **c:\mycert\mycert.cer**. (Stvorite mapu **c:\mycert** prije nastavka ili koristite postojeću mapu, po želji.)

    * Navedite put i naziv gdje novi certifikat i njegov privatni ključ, u obliku .pfx spremit će se. Za potrebe ovog praktičnog vodiča, ćemo koristiti **c:\mycert\mycert.pfx**. **Novi certifikat** dijalog trebao bi izgledati otprilike ovako (Ažuriraj putevima mapa ako niste koristili **c:\mycert**):

        ![][ic712275]

    * Kliknite **u redu** da biste zatvorili dijaloški okvir **Novi certifikat** .

1. Dijalog **Daljinski pristup** trebao bi izgledati otprilike ovako:</p>

    ![][ic719495]

1. Kliknite **u redu** da biste zatvorili dijaloški okvir **Daljinski pristup** .
    
Ponovno stvaranje aplikacije, s sastavljanje postavljanje radi implementacije na cloud.

## <a name="to-log-in-remotely"></a>Da biste se prijavili daljinski

Kada je vaša uloga instance spreman, možete daljinski prijave virtualnog računala na kojem se nalazi aplikacija.

* Ako koristite Eclipse na Windows, a odabrali mogućnost **Početak udaljene radne površine na implementacija** tijekom implementacije Azure, primit ćete s zaslon za prijavu udaljene radne površine kada se pokrene implementaciju sustava. Kada se pojavi upit za korisničko ime i lozinku, unesite vrijednosti koje ste naveli za udaljeni korisnik i moći ćete se prijaviti.

* Drugi način za daljinsko prijavite se putem <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Portal za upravljanje Azure</a>:

    * U prikazu na **Servise u Oblaku** portala za upravljanje Azure kliknite servis u oblaku, kliknite **instance**, kliknite konkretnu instancu, a zatim gumb **Poveži** . Gumb **Poveži** pojavit će se kao sljedeće na naredbenoj traci:

        ![][ic659273]

    * Nakon klika na gumb **za povezivanje** , zatražit će se da biste otvorili datoteku RDP. Otvorite datoteku, a zatim slijedite upute. (Nije moguće i spremite tu datoteku s vašim lokalnim računalom, a zatim pokrenite datoteku tako da dvokliknete udaljene prijava virtualnog računala bez obzira na to najprije morate otvoriti portal za upravljanje.)

    * Kada se pojavi upit za korisničko ime i lozinku, unesite vrijednosti koje ste naveli za udaljeni korisnik i moći ćete se prijaviti.

> [AZURE.NOTE] Ako ste u operacijskom sustavu koji nisu u sustavu Windows, morate koristiti udaljene radne površine klijent kompatibilan s operacijskim sustavom i slijedite korake da biste konfigurirali tog klijenta s postavkama u RDP datoteku koju ste preuzeli.

## <a name="see-also"></a>Vidi također

[Azure komplet alata za Eclipse][]

[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse][]

[Instalacija alata za Azure za Eclipse][] 

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java][].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png
