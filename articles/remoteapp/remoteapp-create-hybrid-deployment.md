<properties
    pageTitle="Upute za stvaranje hibridnog zbirke za Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako stvoriti implementacije RemoteApp koja se povezuje s internoj mreži."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo"/>

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Upute za stvaranje hibridnog zbirke za Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Postoje dvije vrste zbirki Azure RemoteApp:

- Oblak: potpuno nalazi se u Azure. Možete odabrati da biste spremili sve podatke u oblaku (tako da samo oblak zbirke) ili za vaše zbirke povezati s VNET i spremanje podataka postoji.   
- Hibridno: obuhvaća virtualne mreže za pristup lokalnog – to zahtijeva korištenje Azure AD i okruženju lokalni Active Directory.

Ne znate koje je potrebno? Pogledajte [koje zbirke potrebne za Azure RemoteApp](remoteapp-collections.md).

Pomoću ovog praktičnog vodiča vodit će vas kroz postupak stvaranja hibridnog zbirke. Postoji osam korake:

1.  Odlučiti koju [sliku](remoteapp-imageoptions.md) želite koristiti za svoju zbirku. Možete stvoriti prilagođenu sliku ili koristite jedan dio vaše pretplate na Microsoft slika.
2. Postavljanje virtualne mreže. Pogledajte podatke o [planiranju VNET](remoteapp-planvnet.md) i [promjenu veličine](remoteapp-vnetsizing.md) .
2.  Stvaranje zbirke.
2.  Uključivanje zbirka lokalne domene.
3.  Dodajte sliku predložak u zbirku.
4.  Konfiguriranje sinkronizacije direktorija. Azure RemoteApp zahtijeva integrirati Azure Active Directory ili 1) konfiguriranje Azure Active Directory Sync s mogućnošću sinkronizaciju lozinke ili 2) konfiguriranje Azure Active Directory Sync bez mogućnost sinkroniziranje lozinkom, ali koristite domenu koja nije omogućila vanjski pristup za AD FS. Pogledajte [informacije o konfiguraciji za Active Directory s RemoteApp](remoteapp-ad.md).
5.  Objavljivanje aplikacija RemoteApp.
6.  Konfiguriranje korisničkog pristupa.

**Prije početka**

Trebali biste učiniti sljedeće prije stvaranja zbirke:

- [Registracija](https://azure.microsoft.com/services/remoteapp/) za Azure RemoteApp.
- Stvorite korisnički račun u servisu Active Directory da biste koristili kao račun servisa Azure RemoteApp. Ograničite dozvole za taj račun tako da se mogu samo pridružiti strojeva domeni.
- Prikupite informacije o lokalne mreže: IP adresa informacije i Detalji o uređaju VPN-a.
- Instalirajte modul [Azure PowerShell](../powershell-install-configure.md) .
- Prikupite informacije o korisnicima koji želite dopustiti pristup. Potrebno je Azure Active Directory korisnikovo Glavno ime (na primjer, name@contoso.com) za svakog korisnika. Provjerite odgovara li između Azure AD UPN-a li i servisa Active Directory.
- Odaberite sliku predložak. Slika predloška Azure RemoteApp sadrži aplikacije i programa koji želite objaviti za korisnike. Dodatne informacije potražite u članku [Mogućnosti za Azure RemoteApp slike](remoteapp-imageoptions.md) .
- Želite li koristiti Office 365 ProPlus? Pogledajte informacije [u nastavku](remoteapp-officesubscription.md).
- [Konfiguriranje u servisu Active Directory RemoteApp](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Korak 1: Postavljanje virtualne mreže
Možete implementirati hibridnog zbirke koja koristi postojeću Azure virtualne mrežu ili možete stvoriti novu virtualne mreže. Virtualne mreže omogućuje podatke iz programa access korisnika u lokalnoj mreži putem udaljene resursi RemoteApp. Korištenje Azure virtualne mreže omogućuje pristup mreži Izravni zbirke drugih servisa Azure i virtualnim strojevima implementiran na tom virtualne mreže.

Provjerite je li pregledajte podatke [VNET planiranja](remoteapp-planvnet.md) i [VNET veličinu](remoteapp-vnetsizing.md) prije stvaranja vaše VNET.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Stvaranje programa Azure VNET i pridruživanje za implementaciju sustava Active Directory

Najprije stvaranje [virtualne mreže](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). To možete učiniti na kartici **mreže** na portalu za Azure. Morate povezati virtualne mreže Implementacija servisa Active Directory koji se klijent sustava Azure Active Directory.

Dodatne informacije potražite [Stvaranje virtualne mreže pomoću portala za Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Provjerite je li virtualne mreže jeste li spremni za Azure RemoteApp
Prije stvaranja zbirke web recimo provjerite nalazi li se novi virtualne mreže spreman. To možete provjeriti tako da učinite sljedeće:

1. Stvaranje Azure virtualnog računala unutar podmreže virtualne mreže koju ste upravo stvorili za RemoteApp.
2. Korištenje udaljene radne površine za povezivanje s virtualnog računala. (Kliknite **Poveži**.)
3. Uključivanje u istu Implementacija servisa Active Directory koji želite koristiti za RemoteApp.

Jeste li to funkcionira? Virtualne mreže i podmreže spremni ste za Azure RemoteApp!

Možete pronaći dodatne informacije o stvaranju Azure virtualnim strojevima i povezati ih s udaljene radne površine [ovdje](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Korak 2: Stvorite zbirku Azure RemoteApp ##



1. [Portal za Azure](http://manage.windowsazure.com)idite na stranicu Azure RemoteApp.
2. Kliknite **Novo > Stvaranje s VNET**.
3. Unesite naziv za vašu zbirku.
4. Odaberite plan koji želite koristiti - standardno ili osnovni.
5. Odaberite vaše VNET s padajućeg popisa, a zatim vašoj podmreži.
6. Odaberite da biste se pridružili na vašu domenu.
5. Kliknite **Stvaranje RemoteApp zbirke**.

Nakon stvaranja zbirka Azure RemoteApp dvokliknite naziv zbirke. Koje će otvorite stranicu za **Brzi početak rada** – to je gdje ste dovršili konfiguraciju zbirke.

Jeste li nešto poći po zlu? Pogledajte [informacije o otklanjanju poteškoća zbirke za hibridno](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Korak 3: Povezati zbirka lokalne domene ##


1. Na stranici za **Brzo pokretanje** kliknite **Pridruži se lokalne domene**.
2. Dodajte račun servisa Azure RemoteApp lokalne domene servisa Active Directory. Trebat će se naziv domene, Organizacijska jedinica, servis račun korisničko ime i lozinku.

    Ovo je informacije prikupljene ako ste pratili korake u [Konfiguriranje u servisu Active Directory Azure RemoteApp](remoteapp-ad.md).


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Korak 4: Veza na sliku Azure RemoteApp ##

Slika predloška Azure RemoteApp sadrži programe koje želite zajednički koristiti s korisnicima. Možete stvoriti novu [sliku predložak](remoteapp-imageoptions.md) ili veze do postojeće slike (jedne već uvezeni ili prenijeli Azure RemoteApp). Također možete povezati s jednom od Azure RemoteApp [Slika predložaka](remoteapp-images.md) koje sadrže web-mjesto sustava Office 365 ili u okvir za programe sustava Office 2013 (za Probno korištenje).

Ako prenosite novu sliku, morate unijeti naziv, a zatim odaberite mjesto za sliku. Na sljedećoj stranici čarobnjaka ćete vidjeti skup cmdleta ljuske PowerShell – Kopiraj i pokretanje tih cmdleta s dodatnim komponente Windows PowerShell upit da biste prenijeli navedenu sliku.

Ako se povezujete postojeće slike predložak, jednostavno Navedite naziv slike, mjesto i pridruženi Azure pretplate.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Korak 5: Konfiguriranje sinkronizacije imenika Active Directory ##

Azure RemoteApp zahtijeva integrirati Azure Active Directory ili 1) konfiguriranje Azure Active Directory Sync s mogućnošću sinkronizaciju lozinke ili 2) konfiguriranje Azure Active Directory Sync bez mogućnost sinkroniziranje lozinkom, ali koristite domenu koja nije omogućila vanjski pristup za AD FS.

Pogledajte [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – ovaj članak sadrži prilikom postavljanja Integracija direktorija u koracima od 4.

Pogledajte [Vodič za sinkronizaciju direktorija](http://msdn.microsoft.com//library/azure/hh967642.aspx) za planiranje informacije i detaljne upute.

## <a name="step-6-publish-apps"></a>Korak 6: Objavljivanje aplikacija ##

Azure RemoteApp aplikacije je aplikacija ili program koji omogućuje korisnicima. Nalazi li se u sliku predložak ste prenijeli za zbirku. Kad korisnik pristupa aplikaciju, čini se da biste pokrenuli u svoje lokalnog okruženja, no zaista izvodi u Azure.

Prije nego što se korisnici mogu pristupati aplikacije, morate ih objaviti – omogućuje korisnici mogu pristupiti aplikacijama kroz Udaljena radna površina.

Više aplikacija možete objaviti u zbirku. Stranica za objavljivanje kliknite **Objavi** da biste dodali aplikaciju. Možete objaviti ili na izborniku **Start** predložak slike ili navođenjem put na slici predložak za aplikaciju. Ako odaberete da biste dodali s izbornika **Start** , odaberite program da biste dodali. Ako se odlučite za davanje put do aplikaciju, navedite naziv aplikacije i put do kojem je instaliran na slici predložak.

## <a name="step-7-configure-user-access"></a>7 korak: Konfiguriranje korisničkog pristupa ##

Sad kad ste stvorili vaše zbirke, morate dodati korisnike koje želite da biste mogli koristiti udaljenim resursima. Korisnici koji omogućuju pristup moraju postojati u klijentu za Active Directory povezan s pretplatom koristi za stvaranje zbirku Azure RemoteApp.

1.  Na stranici za brzo pokretanje kliknite **Konfiguriraj korisničkog pristupa**.
2.  Unesite poslovni račun (iz servisa Active Directory) ili Microsoftov račun koji želite dopustiti pristup.

    **Bilješke:**

    Provjerite je li koje koristite u “user@domain.com” oblik.

    Ako koristite Office 365 ProPlus u sklopu zbirke, morate koristiti identiteta servisa Active Directory za korisnike. Omogućuje provjeru licenciranje.


3.  Kada se potvrđuju korisnike, kliknite **Spremi**.


## <a name="next-steps"></a>Daljnji koraci ##
To je – uspješno stvorili i implementirati zbirka hibridnog Azure RemoteApp. Sljedeći je korak da bi korisnici preuzmite i instalirajte Udaljena radna površina. Možete pronaći URL-a za klijenta na stranici Azure RemoteApp brzi početak rada. Nakon toga ste Prijava korisnika na klijentu i pristup aplikacijama koje ste objavili.



### <a name="help-us-help-you"></a>Pomozite nam da vam pomoći
Jeste li znali da osim ocjena u ovom se članku i upućivanje komentare dolje ispod, koje možete unijeti promjene sam članak? Nešto nedostaje? Nešto nije u redu? Nije li moguće napisati nešto što je samo pregledniji? Pomicanje prema gore, a zatim kliknite **Uređivanje na GitHub** da biste unijeli promjene – one će dođite do nas za pregled, a zatim jednom smo odjaviti na njima, vidjet ćete promjene i poboljšanja ovdje.
