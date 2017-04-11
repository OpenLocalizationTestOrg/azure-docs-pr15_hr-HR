<properties
    pageTitle="Postavljanje uređaja Windows 10 s Azure AD na stranici Postavke | Microsoft Azure"
    description="U članku se objašnjava kako korisnici mogu pridružiti za Azure AD putem izbornika postavke."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Postavljanje uređaja Windows 10 s Azure AD na stranici Postavke
Ako već koristite Windows 7 ili Windows 8 i računalu ili uređaju nadogradnje na Windows 10, možete se uključiti Azure Active Directory (Azure AD) putem izbornika postavke.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Da biste se uključili u Azure AD na izborniku postavke


1. Na izborniku **Start** kliknite gumbić **Postavke** .
2. Na stranici **Postavke**odaberite **sustav**->**o**->**Uključivanje Azure AD**.
<center>
![Uključivanje Azure AD na izborniku postavke](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Kliknite **Nastavi** u prozoru poruke Azure AD uključiti.
<center>
![Prozor poruke spoj Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Unesite vjerodajnice za prijavu. Ovaj sučelje za prijavu neće sadržavati sve korake koji su potrebni za dovršavanje provjere autentičnosti. Ako su dio pridruženim klijent, administrator će vam pružiti sučelje za vanjski pristup koje se hostira vaša tvrtka ili ustanova.
<center>
![Unesite vjerodajnice za prijavu](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Ako je vaša organizacija konfigurirala Azure višestruke provjere autentičnosti za uključivanje Azure AD, navedite drugi faktor prije nastavka.
6. Kliknite **Prihvati** na zaslonu **Dopusti uređaju da se upravlja** .
7. Trebali biste vidjeti poruku "uređaj je sada se uključili u svojoj tvrtki ili ustanovi Azure AD".


## <a name="additional-information"></a>Dodatne informacije
* [Saznajte više o korištenju scenariji za uključivanje za Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
* [Provjera autentičnosti identiteta bez lozinke kroz Microsoft Passport](active-directory-azureadjoin-passport.md)
