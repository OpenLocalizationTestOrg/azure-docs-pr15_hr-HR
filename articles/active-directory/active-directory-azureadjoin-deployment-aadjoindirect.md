<properties
    pageTitle="Scenariji za korištenje i implementaciju zahtjevi za Azure AD uključivanje | Microsoft Azure"
    description="U članku se objašnjava kako administratori postaviti Azure AD uključivanje njihove krajnjim korisnicima (zaposlenika, studenti, drugi korisnici). Opisano scenarija stvarnog života za korištenje Azure AD uključiti."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< oznake ms.service= "active directory" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Scenariji za korištenje i implementaciju zahtjevi za pridruživanje za Azure AD

## <a name="usage-scenarios-for-azure-ad-join"></a>Korištenje scenariji za pridruživanje za Azure AD
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Scenarij 1: Tvrtke uglavnom u oblaku

Azure Active Directory uključivanje (Azure AD pridružiti) vam može koristiti ako trenutno raditi i upravljanja za svoju tvrtku u oblaku ili premještate u oblak uskoro. Pomoću računa koji ste stvorili u Azure AD za prijavu u Windows 10. Kroz [postupak prvog pokretanja sučelje (FRX)](active-directory-azureadjoin-user-frx.md)ili spajanjem Azure AD [na](active-directory-azureadjoin-user-upgrade.md)izborniku postavke vaši korisnici možete pristupiti svojim računalima Azure AD.  Korisnici i mogli uživati u jednom prijava (SSO) pristup oblaka resursima kao što je Office 365 u preglednicima ili u aplikacijama sustava Office.

### <a name="scenario-2-educational-institutions"></a>Scenarij 2: Obrazovne ustanove

Obrazovna ustanova obično su dvije vrste korisnika: Nastavnici i studente. Nastavnici članovi smatraju longer-term članova tvrtke ili ustanove. Stvaranje lokalnih računa za njih je poželjno. No učenici su shorter-term članova tvrtke ili ustanove i njihovi računi može upravljati u Azure AD. To znači da se skaliranje direktorija možete pomiču u oblak umjesto se pohranjuju lokalno. Također znači učenici će se moći prijavite se u sustav Windows pomoću računa za Azure AD i dobiti pristup resursima sustava Office 365 u aplikacijama sustava Office.

### <a name="scenario-3-retail-businesses"></a>Scenarij 3: Maloprodaja tvrtke

Maloprodajne tvrtke imaju zaposlenici zaduženi za sezonski i Dugoročne Zaposlenici. Obično stvaranje lokalnih računa i korištenje domene združeni strojeva longer-term zaposlenike. No zaposlenici zaduženi za sezonski su shorter-term članova tvrtke ili ustanove, a je poželjno da biste upravljali njihove račune kojima korisničke licence možete jednostavnije premjestiti oko. Prilikom stvaranja korisničkih računa u oblaku licenci za Office 365, ti korisnici se prednostima prijave Windows i Office aplikacije s računom Azure AD dok održavanje veću fleksibilnost njihove licenci nakon što napuste.

### <a name="scenario-4-additional-scenarios"></a>Scenarij 4: Dodatni scenariji

Uz pogodnosti spomenuli, prednosti onemogućite korisnike Pridružite svoje uređaje Azure AD zbog i pojednostavnjeni spojno sučelje, upravljanje učinkovitog uređajima, registraciju za upravljanje automatsko mobilnog uređaja i jedinstvenu prijavu za Azure AD i lokalnog resursa.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Uvođenje zahtjevi za pridruživanje za Azure AD

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Omogućivanje korisnicima da biste se pridružili uređaj vlasništvu tvrtke izravno u Azure AD


Korporacije unijeti samo oblak računi partneri i tvrtke ili ustanove. Ovi partneri zatim jednostavno možete pristupiti aplikacijama tvrtke i resursi jedinstvenu prijavu. Taj se scenarij odnosi se na korisnike pristup resursima prvenstveno u oblaku, kao što je Office 365 ili SaaS aplikacije koje ovise o Azure AD za provjeru autentičnosti.

### <a name="prerequisites"></a>Preduvjeti
**Na razini tvrtke (administratora)**

*   Azure pretplatu pomoću servisa Azure Active Directory  

**Na razini korisnika**

*   Windows 10 (izdanja Professional i velike)

### <a name="administrator-tasks"></a>Administratorski zadaci
* [Postavljanje Registracija uređaja](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Zadaci korisnika
* [Postavljanje novog uređaja Windows 10 s Azure AD tijekom instalacije](active-directory-azureadjoin-user-frx.md)
* [Postavljanje uređaja Windows 10 s Azure AD na izborniku postavke](active-directory-azureadjoin-user-upgrade.md)
* [Uključivanje osobnim uređajem Windows 10 u vašoj tvrtki ili ustanovi](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>Omogućivanje BYOD u tvrtki ili ustanovi za Windows 10
Možete postaviti korisnika i zaposlenika za korištenje njihove osobne uređaje sa sustavom Windows (BYOD) da biste pristupili tvrtke aplikacija i resursa. Korisnike možete dodati osobnim uređajem Windows pristupa resursima servisa sigurne i usklađen način njihove Azure AD račune (računi tvrtke ili obrazovne ustanove).

### <a name="prerequisites"></a>Preduvjeti
**Na razini tvrtke (administratora)**

*   Azure AD pretplate

**Na razini korisnika**

*   Windows 10 (izdanja Professional i velike)


### <a name="administrator-tasks"></a>Administratorski zadaci

* [Postavljanje Registracija uređaja](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Zadaci korisnika
* [Uključivanje osobnim uređajem Windows 10 u vašoj tvrtki ili ustanovi](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Dodatne informacije
* [Windows 10 za tvrtke: načina korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblak na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Provjera autentičnosti identiteta bez lozinke kroz Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Saznajte više o korištenju scenariji za uključivanje za Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
