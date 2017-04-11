<properties
    pageTitle="Azure MFA i AD FS | Microsoft Azure"
    description="To je stranica Azure višestruke provjere autentičnosti koji opisuje način za početak rada s Azure MFA i AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Prvi koraci s Azure višestruke provjere autentičnosti i Active Directory Federation Services



<center>![Oblak](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Ako vaša tvrtka ili ustanova sadrži pridružene vašeg lokalnog servisa Active Directory s Azure Active Directory pomoću servisa AD FS, postoje dvije mogućnosti za korištenje Azure višestruke provjere autentičnosti.

- Zaštita oblaka resursa pomoću Azure višestruke provjere autentičnosti ili Active Directory Federation Services
- Zaštita oblaka i lokalnog resursa pomoću Azure višestruke provjere autentičnosti poslužitelja

U sljedećoj su tablici navedene sučelje za potvrdu između zaštita resursi Azure višestruke provjere autentičnosti i AD FS

|Sučelje za potvrdu – aplikacije utemeljene na pregled|Sučelje za potvrdu - aplikacije koje nisu-utemeljenima na pregledniku
:------------- | :------------- | :------------- |
Zaštita resurse Azure AD pomoću Azure višestruka provjera autentičnosti |<li>U prvom koraku potvrdu provodi lokalnog pomoću servisa AD FS.</li> <li>Drugi korak je metoda utemeljenu telefonu provesti pomoću provjere autentičnosti u oblaku.</li>|Krajnji korisnici možete koristiti aplikaciju lozinke za prijavu.
Zaštita resurse Azure AD pomoću Active Directory Federation Services |<li>U prvom koraku potvrdu provodi lokalnog pomoću servisa AD FS.</li><li>Drugi korak je izvesti lokalnog honoring zahtjev.</li>|Krajnji korisnici možete koristiti aplikaciju lozinke za prijavu.

Upozorenja lozinkama aplikacije za vanjske korisnike:

- Aplikacija lozinke potvrđuju pomoću provjere autentičnosti u oblaku, tako da ih zaobići vanjski pristup. Vanjski pristup samo aktivno koristi prilikom postavljanja programa aplikacije lozinku.
- Postavke za kontrolu pristupa klijenta lokalnog ne su na snazi lozinkama aplikacije.
- Izgubit ćete lokalnog mogućnost zapisivanje za provjeru autentičnosti za aplikaciju lozinke.
- Onemogući brisanja računa može proći do tri sata za sinkronizaciju direktorija, što Onemogući/brisanje aplikacije lozinke u oblak identitet.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o postavljanju Azure višestruke provjere autentičnosti ili poslužitelja za provjeru autentičnosti višestruku Azure AD fs potražite u sljedećim člancima:

- [Zaštita oblaka resursa pomoću Azure višestruke provjere autentičnosti i AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
- [Zaštita oblaka i lokalnog resursa pomoću poslužitelja za Azure višestruke provjere autentičnosti sustava Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Zaštita oblaka i lokalnog resursa pomoću Azure višestruke provjere autentičnosti poslužitelja za AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
