<properties 
    pageTitle="Kako stvoriti oblaka skup Azure RemoteApp | Microsoft Azure" 
    description="Saznajte kako stvoriti implementacije Azure RemoteApp sprema podatke u oblaku Azure." 
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

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Kako stvoriti oblaka skup Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Postoje dvije vrste zbirki [Azure RemoteApp](remoteapp-collections.md): 

- Oblak: potpuno nalazi se u Azure. Možete odabrati da biste spremili sve podatke u oblaku (tako da samo oblak zbirke) ili za vaše zbirke povezati s VNET i spremanje podataka postoji.   
- Hibridno: obuhvaća virtualne mreže za pristup lokalnog – to zahtijeva korištenje Azure AD i okruženju lokalni Active Directory.

Pomoću ovog praktičnog vodiča vodit će vas kroz postupak stvaranja zbirke oblaka. Postoje četiri koraka: 

1.  Stvorite zbirku Azure RemoteApp.
2.  Ako želite konfigurirati Sinkroniziranje direktorija. Ako koristite Azure AD + servisa Active Directory, morate sinkronizirati korisnika, kontakata i lozinke iz vaše lokalne servisa Active Directory za vaš klijent Azure AD.
5.  Objavljivanje aplikacija.
6.  Konfiguriranje korisničkog pristupa.


**Prije početka**

Trebali biste učiniti sljedeće prije stvaranja zbirke:

- [Registracija](https://azure.microsoft.com/services/remoteapp/) za Azure RemoteApp. 
- Prikupite informacije o korisnicima koji želite dopustiti pristup. To se može biti podatke o Microsoftovu računu ili servisa Active Directory podatke o računu tvrtke za korisnike.
- Ovaj postupak pretpostavlja da ste ili namjeravate koristiti jedan od slika predložaka navedene kao dio pretplate ili da ste već prenijeli slika predložak koji želite koristiti. Ako je potrebno prenijeti sliku drugi predložak, to možete učiniti na stranici slika predložaka. Jednostavno kliknite **Prenesi sliku predloška** i slijedite korake u čarobnjaku. 
- Želite li koristiti Office 365 ProPlus? Pogledajte informacije [u nastavku](remoteapp-officesubscription.md).
- Želite li navesti prilagođene aplikacije ili LOB programa? Stvorite novu [sliku](remoteapp-imageoptions.md) i koristiti u sklopu zbirke oblaka.
- Znate li vam je potrebno da biste se povezali s VNET. Ako odaberete da biste se povezali s VNET, provjerite zadovoljava [smjernice za promjenu veličine](remoteapp-vnetsizing.md) i da [se možete povezati RemoteApp](remoteapp-vnet.md). Pogledajte [članak Planiranje VNET ](remoteapp-planvnet.md)dodatne informacije.
- Ako koristite na VNET, odlučite želite li uključiti lokalne domene servisa Active Directory.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Korak 1: Stvaranje zbirke oblaka - sa ili bez na VNET##


Slijedite ove korake da biste **stvorili zbirku koja je samo za oblaku**:

1. Na portalu za upravljanje idite na stranicu RemoteApp.
2. Kliknite **Novo > brzo stvaranje**.
3. Unesite naziv za vašu zbirku pa odaberite regiju.
4. Odaberite plan koji želite koristiti - standardno ili osnovni.
5. Odaberite predložak će se koristiti za tu zbirku. 

    **Savjet:** U sklopu pretplate na RemoteApp [Slika predložaka](remoteapp-images.md) koje sadrže Office 365 ili Office 2013 (za Probno korištenje) programa, neke objavljene (kao što su Word) i drugim korisnicima želite objaviti. Možete stvoriti novu [sliku](remoteapp-imageoptions.md) i koristiti u sklopu zbirke oblaka.


1. Kliknite **Stvaranje RemoteApp zbirke**.
    
    **Važne:** Može potrajati do 30 minuta Dodjela vaše zbirke.

Nakon stvaranja zbirka Azure RemoteApp dvokliknite naziv zbirke. Koje će otvorite stranicu za **Brzi početak rada** – to je gdje ste dovršili konfiguraciju zbirke.

Da biste stvorili u **oblak + VNET zbirke**, poduzmite sljedeće korake:

1. Na portalu za upravljanje idite na stranicu Azure RemoteApp.
2. Kliknite **Novi** > **Stvaranje s VNET**.
3. Unesite naziv za vašu zbirku.
4. Odaberite plan koji želite koristiti - standardno ili osnovni.
5. Odaberite VNET ste već stvorili. Ne znate kako to učiniti? Zasad, koraci su u temi [hibridnog](remoteapp-create-hybrid-deployment.md) .
6. Odlučite želite li uključiti vaše zbirke na vašu domenu. Ako je da, morat ćete integraciju Azure AD pomoću AD Connect i okruženja sustava Active Directory. Koji je obuhvaćeno u nastavku **Korak 2**.
6. Kliknite **Stvaranje RemoteApp zbirke**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Korak 2: Konfiguriranje sinkronizacije imenika Active Directory (nije obavezno) ##

Ako želite koristiti Active Directory Azure RemoteApp zahtijeva sinkronizacije direktorija između servisa Azure Active Directory i lokalni Active Directory za sinkronizaciju korisnika, kontakata i lozinke za vaš klijent Azure Active Directory. Potražite u članku [Konfiguriranje u servisu Active Directory Azure RemoteApp](remoteapp-ad.md) za planiranje informacije. Možete posjetiti i izravno u [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) informacije.

## <a name="step-3-publish-apps"></a>Korak 3: Objavljivanje aplikacija ##

Azure RemoteApp aplikacije je aplikacija ili program koji omogućuje korisnicima. Nalazi li se u sliku predložak ste prenijeli za zbirku. Kad korisnik pristupa aplikaciju, pojavit će se aplikaciju da biste pokrenuli u svoje lokalnog okruženja, ali zaista izvodi u virtualnog računala u Azure. 

Prije nego što se korisnici mogu pristupati aplikacije, morate ih objaviti – objavljivanje aplikacija omogućuje korisnici mogu pristupiti aplikacijama kroz Udaljena radna površina.
 
Više aplikacija možete objaviti u zbirku Azure RemoteApp. Stranica za objavljivanje kliknite **Objavi** da biste dodali programa. Možete objaviti ili na izborniku **Start** predložak slike ili navođenjem put na slici predložak za aplikaciju. Ako odaberete da biste dodali s izbornika **Start** , odaberite aplikaciju za objavljivanje. Ako se odlučite za davanje put do aplikaciju, navedite naziv aplikacije i put do kojem je instaliran na slici predložak.

## <a name="step-4-configure-user-access"></a>Korak 4: Konfiguriranje korisničkog pristupa ##

Sad kad ste stvorili vaše zbirke, morate dodati korisnike koje želite da biste mogli koristiti udaljenim resursima. Ako koristite Active Directory, korisnici koji omogućuju pristup moraju postojati u klijentu za Active Directory povezan s pretplatom ste koristili za stvaranje zbirke.

1.  Na stranici za brzo pokretanje kliknite **Konfiguriraj korisničkog pristupa**. 
2.  Unesite poslovni račun (iz servisa Active Directory) ili Microsoftov račun koji želite dopustiti pristup.

    **Bilješke:** 

    Provjerite je li koje koristite u “user@domain.com” oblik.

    Ako koristite Office 365 ProPlus u sklopu zbirke, morate koristiti identiteta servisa Active Directory za korisnike. Omogućuje provjeru licenciranje. 

3.  Kada se potvrđuju korisnike, kliknite **Spremi**.


## <a name="next-steps"></a>Daljnji koraci ##

To je – uspješno stvorili i implementirati zbirka oblaka Azure RemoteApp. Sljedeći je korak da bi korisnici preuzmite i instalirajte Udaljena radna površina. Možete pronaći URL-a za klijenta na stranici Azure RemoteApp brzi početak rada. Nakon toga ste Prijava korisnika na klijentu i pristup aplikacijama koje ste objavili.

### <a name="help-us-help-you"></a>Pomozite nam da vam pomoći 
Jeste li znali da osim ocjena u ovom se članku i upućivanje komentare dolje ispod, koje možete unijeti promjene sam članak? Nešto nedostaje? Nešto nije u redu? Nije li moguće napisati nešto što je samo pregledniji? Pomicanje prema gore, a zatim kliknite **Uređivanje na GitHub** da biste unijeli promjene – one će dođite do nas za pregled, a zatim jednom smo odjaviti na njima, vidjet ćete promjene i poboljšanja ovdje.