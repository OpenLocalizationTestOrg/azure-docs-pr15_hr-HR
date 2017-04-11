<properties
    pageTitle="Povijest verzija stanja za Azure AD Connect"
    description="Ovaj dokument opisane u izdanjima za Azure AD povežite zdravlje i što obuhvaća iz tih izdanja."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect stanja: Povijest verzija izdanja

Tim za Azure Active Directory redovito obnavlja povežite zdravlje Azure AD pomoću nove značajke i funkcije. U ovom se članku navode verzije i značajkama koje su objavljene.

## <a name="october-2016"></a>Listopad 2016
**Agent za ažuriranje:**
- Azure AD povežite zdravlje agente za AD FS \(verziju 2.6.408.0\)
    1. Poboljšanja otkrivanje IP adrese klijenta u zahtjevima za provjeru autentičnosti
    2. Popravci programskih pogrešaka vezanih uz upozorenja
- Azure AD povežite zdravlje agent za AD DS (verzija 2.6.408.0)
    1. Popravci programskih pogrešaka vezanih uz upozorenja.
- Azure AD povežite zdravlje agent za sinkronizaciju (verzija 2.6.353.0) objaviti u sklopu Azure AD Connect verzija 1.1.281.0
    1. Navedite potrebne podatke za izvješća o pogrešci sinkronizacije
    2. Popravci programskih pogrešaka vezanih uz upozorenja

**Nove značajke pretpregleda:**
- Povezivanje izvješća o pogrešci sinkronizacije za Azure AD

**Nove značajke:**
- Azure AD povežite zdravlje za AD FS - polja IP adresa dostupna je u izvješću o vodećih 50 korisnika s neispravni korisničkog imena i lozinke.

## <a name="july-2016"></a>Srpanj 2016

**Nove značajke pretpregleda:**

- [Azure AD Connect stanja za AD DS](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Siječanj 2016


**Agent za ažuriranje:**

- Azure AD povežite zdravlje agent za AD FS (verzija 2.6.91.1512)


**Nove značajke:**

- [Alat za povezivanje test za Azure AD povezivanje agenata stanja](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>Studenom 2015.


**Nove značajke:**

- Podrška za [kontrolu pristupa na temelju uloga](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)


**Nove značajke pretpregleda:**

- [Azure AD povezivanje stanja sinkronizacije](active-directory-aadconnect-health-sync.md).

**FIXED problemi:**

- Popravci programskih pogrešaka pogrešaka vidjeti prilikom registracije agent.

## <a name="september-2015"></a>Rujan 2015.

**Nove značajke:**

- Pogrešan izvješća lozinke korisničkog imena za AD FS
- Podrška za konfiguriranje Neprovjereni HTTP Proxy
- Podrška za konfiguriranje agent na Server core
- Poboljšanja upozorenja za AD FS
- Prenesite poboljšanja Azure AD povezivanje Health Agent za AD FS za povezivanje i podataka.


**FIXED problemi:**

- Popravci programskih pogrešaka u korištenje uvida za AD FS pogreške vrste.


## <a name="june-2015"></a>Lipnja 2015.

**Početna izdanje povežite zdravlje Azure AD za AD FS i Proxy za AD FS.**

**Nove značajke:**

- Upozorenja za nadzor AD FS i Proxy za AD FS poslužitelje s obavijesti e-poštom.
- Jednostavan pristup za AD FS Topologija i uzorke mjerača performansi za AD FS.
- Trend u uspješnih tokena zahtjeva na poslužiteljima AD FS grupirane aplikacije, metoda provjere autentičnosti itd zahtjev mreže mjesto.
- Trendova u zahtjeva na poslužiteljima AD FS grupirane aplikacije, pogreške vrste itd.
- Jednostavnije Agent implementacija pomoću vjerodajnica za Azure AD globalnog administratora.  




## <a name="next-steps"></a>Daljnji koraci
Saznajte više o [Monitor lokalnih identiteta infrastrukturu i sinkronizacije servisa u oblaku](active-directory-aadconnect-health.md).
