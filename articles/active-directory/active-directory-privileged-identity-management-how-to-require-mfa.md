<properties
   pageTitle="Kako zahtijevaju višestruka provjera autentičnosti | Microsoft Azure"
   description="Saznajte kako zahtijevaju višestruke provjere autentičnosti (MFA) za povlaštene identitete s nastavkom Azure Active Directory povlaštene upravljanje identitetom."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Kako zahtijevaju MFA u Azure AD povlaštene upravljanje identitetom

Preporučujemo da obavezan višestruke provjere autentičnosti (MFA) za sve svoje administratori. Time se smanjuje rizik napada zbog compromised lozinku.

Možete zatražiti da korisnici popunjavaju u test MFA kada se prijaviti. Za blog [MFA za Office 365 i MFA za Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) uspoređuju što sve obuhvaća Office i Azure pretplate sa značajkama koje se nalaze u nuditi Microsoft Azure višestruke provjere autentičnosti.

Možete zatražiti da korisnici popunjavaju programa test MFA kada ih aktivirati uloga u Azure AD PIM. Na taj način, ako korisnik niste dovršili programa test MFA kada se prijavite, bit će mu ponuđena da biste to učinili po PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Zahtijevanje MFA u Azure AD povlaštene upravljanje identitetom

Prilikom upravljanja identitetima u PIM kao povlaštene ulogu administratora, možda ćete vidjeti upozorenja koja se preporučuje MFA za povlaštene račune. Kliknite sigurnosno upozorenje na nadzornoj ploči PIM, a novi plohu će se otvoriti s popisom administratorske račune koje zahtijevaju MFA.  Tražite MFA tako da odaberete više uloge i klikom na gumb **Popravi** ili možete kliknite tri točke pokraj pojedinačne uloge, a zatim kliknite gumb **Popravi** .

> [AZURE.IMPORTANT] Desnom tipkom miša now, radi samo Azure MFA pomoću tvrtke ili školske računi ne Microsoftova računa (obično osobnog računa koji se koristi za prijavu na Microsoftove servise kao što su Skype Xbox, Outlook.com, itd.). Zbog toga, svi se pomoću Microsoftova računa ne može biti administrator uvjete jer ne možete koristiti MFA da biste aktivirali njihove uloge. Ako tim korisnicima potrebna da biste nastavili upravljati radnih opterećenja pomoću Microsoftova računa, pretvoriti ih trajna administratori za sada.

Osim toga, možete promijeniti MFA obavezu posebnu ulogu tako da kliknete na njemu u odjeljku uloge PIM nadzorne ploče. Zatim kliknite **Postavke** u plohu uloge, a zatim u odjeljku višestruka provjera autentičnosti odaberete **Omogući** .

## <a name="how-azure-ad-pim-validates-mfa"></a>Kako će se Azure AD PIM Provjeri valjanost MFA

Postoje dvije mogućnosti za provjeru valjanosti MFA kada korisnik aktivira uloge.

Mogućnost najjednostavnije je ovise o Azure MFA za korisnike koji su aktiviranje povlaštene uloga. Da biste to učinili, najprije provjerite tim korisnicima licencu, ako je potrebno, a registriranih za Azure MFA. Dodatne informacije o tome kako to učiniti se [Uvod u rad s Azure višestruke provjere autentičnosti u oblaku](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Preporučuje, se, ali nije potrebna, možete konfigurirati Azure AD da biste nametnuli MFA za tim korisnicima kada se prijaviti. To je zato provjere MFA bit će po Azure AD PIM sam.

Osim toga, ako lokalnog provjere autentičnosti korisnika možete postaviti davatelja identiteta smatrati ODGOVORNIM ni za MFA. Na primjer, ako ste konfigurirali servisa Active Directory Federation Services da zahtijeva provjeru autentičnosti utemeljenu na pametne kartice prije pristupanja Azure AD, [Zaštita oblaka resursi Azure višestruke provjere autentičnosti i AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) sadrži upute za konfiguriranje AD FS da biste poslali zahtjevima za Azure AD. Kada se korisnik pokuša aktivirali ulogu, Azure AD PIM prihvaća da MFA je već provjerena za korisnika kad primi odgovarajuće zahtjevima.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
