<properties
    pageTitle="Uvjetno pristup uređaj pravila za servise sustava Office 365 | Microsoft Azure"
    description="Detalje o kako uređaj uvjeti utemeljeni na kontrolu pristupa servisima sustava Office 365. Premda zaposlenici zaduženi za podatke (IWs) za pristup sustavu Office 365 servise kao što su sustava Exchange i SharePoint Online na poslu ili u školi s njihovim osobnim uređaja, njihove administrator želi pristup biti secure.IT administratori mogu Dodjela uvjetno pristup uređaj pravila zaštita tvrtke resursa, dok je u isto vrijeme dopuštanja IWs na uređaji za pristup servisima."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Uvjetno pristup uređaj pravila za servise sustava Office 365

Izraz, "uvjetno pristup" sadrži više uvjeta pridružen kao što su korisnika čija je autentičnost provjerena višestruke provjere, autentičnost uređaja, usklađen uređaj itd. U ovoj se temi prvenstveno focusses na Uvjeti utemeljeni na uređaj za kontrolu pristupa servisima sustava Office 365. Premda zaposlenici zaduženi za podatke (IWs) za pristup sustavu Office 365 servise kao što su sustava Exchange i SharePoint Online na poslu ili u školi s njihovim osobnim uređaja, njihove administrator želi biti siguran pristup. IT administratorima možete Dodjela uvjetno pristup uređaj pravila zaštita tvrtke resursa, dok je u isto vrijeme dopuštanja IWs na uređaji za pristup servisima. Uvjetno pristup pravila u Office 365 može konfigurirati s portala Microsoft Intune uvjetno pristup.

Azure Active Directory nameće pravila uvjetnog pristup siguran pristup servisima sustava Office 365. Administraciju klijenta možete stvoriti pravila za uvjetno access blokira korisnika na uređaju koje nisu usklađene sa blokirali pristup na servis O365. Korisnik mora biti usklađen sa pravilniku uređaj tvrtke prije no što se može dodijeliti pristup servisu. Umjesto toga administrator možete stvoriti pravilo koje korisnici moraju samo registrirati svoje uređaje da biste pristupili na servis O365. Pravila možda primjenjuje na sve korisnike tvrtke ili ustanove, ili ograničen na nekoliko ciljne grupe i poboljšane tijekom vremena da biste dodali dodatne ciljne grupe.

Preduvjeta za nametanje pravila uređaj je za korisnike koji žele registrirati svoje uređaje sa servisom Azure Active Directory Registracija uređaja. Možete odabrati želite li omogućiti višestruke provjere autentičnosti (MFA) za Registracija uređaja sa servisom Azure Active Directory Registracija uređaja. MFA preporučuje servisa Azure Active Directory Registracija uređaja. Kada je omogućen MFA, korisnici Registracija svoje uređaje sa servisom Azure Active Directory Registracija uređaja su teško pokretnim za drugi provjere autentičnosti.

##<a name="how-does-conditional-access-policy-work"></a>Kako funkcionira pravila uvjetnog pristup?

Kada zahtjeva pristupa servis O365 iz platforma podržanom uređaju, Azure Active Directory potvrđuje korisnika i uređaj s koje korisnik pokreće zahtjev; i daje pristup servisu tek kada korisnik u skladu pravila za servis. Korisnici koji imaju svoje uređaje upisani ponudit će remedial upute o tome kako registrirati, a biti usklađen da biste pristupili tvrtke servise sustava O365. Korisnici na web-mjesto iOS i u okvir za uređaje sa sustavom Android će se morati registrirati svoje uređaje pomoću aplikacije Company Portal. Kada vas korisnik upisuje na svojem uređaju, uređaj je registriran Azure Active Directory i registrirate za upravljanje uređajima i usklađenost. Korisnici morate koristiti servisa Azure Active Directory Registracija uređaja u kombinaciji s Microsoft Intune da biste omogućili upravljanje mobilnim uređajima za Office 365. Registracija uređaja je stara obavezna za korisnike koji žele servisa za pristup sustavu Office 365 kada se provode pravila uređaja.

Kada korisnik vas upisuje svojem uređaju uspješno, uređaj postaje pouzdan. Azure Active Directory sadrži-jedinstvene prijave aplikacijama za access tvrtke i nameće pravila uvjetnog pristup da biste dodijelili pristup servisa ne samo prve jedan korisnik zatraži pristup, ali svaki put korisnik zatraži da biste obnovili programa access. Korisnik će biti zabranjen pristup servisima kada mijenjaju vjerodajnice za prijavu, uređaj prekinula/krađe ili pravila ne ispunjava kriterij u trenutku zahtjeva za obnovu.

## <a name="deployment-considerations"></a>Razmatranja implementacije:
Morate koristiti servisa Azure Active Directory Registracija uređaja za registraciju uređaja.

Kada korisnici su moguće provjeriti autentičnost lokalno, potreban je Active Directory Federation Services (AD FS) (1.0 i noviji). Višestruka provjera autentičnosti za uključivanje radnom mjestu neće uspjeti kada davatelja identiteta nije instaliranog višestruke provjere autentičnosti. Ako, na primjer, AD FS 2.0 nije višestruke provjere autentičnosti koje je moguće povezati. Administrator klijenta mora Pobrinite se da na lokalni AD FS je MFA koje je moguće povezati i valjani MFA način je omogućen, prije omogućivanja MFA na servisu Azure Active Directory Registracija uređaja. Ako, na primjer, AD FS na Windows Server 2012 R2 sadrži mogućnosti za MFA. Za dodatne valjani načina provjere autentičnosti (MFA) na poslužitelju za AD FS morate omogućiti i prije omogućivanja MFA na servisu Azure Active Directory Registracija uređaja. Dodatne informacije o podržanim MFA načine u AD FS potražite u članku konfiguriranje dodatne načine provjere autentičnosti za AD FS.

## <a name="frequently-asked-questions-faq"></a>Najčešća pitanja

P: što je zadani pravilnik izuzetaka za platforme nepodržane uređaja?

A: u vrijeme za izlaganje pravila uvjetnog pristup selektivno se primjenjuju na korisnike na web-mjesto iOS i u okvir za uređaje sa sustavom Android. Aplikacije za druge platforme uređaja su po zadanom i ne utječe pravila uvjetnog pristup za iOS i uređaje sa sustavom Android. Administraciju klijenta, no možete nadjačati globalni pravila da bi se onemogućilo pristup korisnika nije podržan platforme.
Je na vodič da biste proširili pravila uvjetnog pristup korisnicima na drugim platformama, uključujući Windows.

P: kada će pravila uvjetnog pristup servisima sustava Office 365 se proširiti preglednika koji se temelje aplikacije (na primjer, OWA, SharePoint utemeljenima na pregledniku).

A: u vrijeme za izlaganje uvjetno pristup servisima za Office 365 ograničeno je na obogaćene aplikacije na uređaju. Je na vodič da biste proširili pravila uvjetnog pristup korisnicima pristup servisima iz preglednika.
