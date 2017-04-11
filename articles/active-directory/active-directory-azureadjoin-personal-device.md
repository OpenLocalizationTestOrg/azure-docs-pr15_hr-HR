

<properties
    pageTitle="Uključivanje osobnim uređaja u vašoj tvrtki ili ustanovi | Microsoft Azure"
    description="U članku se objašnjava kako korisnici mogu registrirati osobnih uređaja Windows 10 njihove korporacijskom mrežom, a navedeni koraci za implementaciju za scenarij BYOD."
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

# <a name="join-a-personal-device-to-your-organization"></a>Uključivanje osobnim uređaja u vašoj tvrtki ili ustanovi

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Da biste se pridružili uređaj sa sustavom Windows 10 u vašoj tvrtki ili ustanovi

1.  S izbornika **Start** odaberite **Postavke**.
2.  Odaberite **računi**, a zatim kliknite **svoj račun**.
3.  Kliknite **Dodavanje rad ili Školskog računa**, a zatim upišite u računa tvrtke ili ustanove.
4.  Na stranici prijava za tvrtku ili ustanovu unesite korisničko ime i lozinku, a zatim kliknite **u redu**.
5.  Zatražit će se za pitanja i odgovora višestruke provjere autentičnosti. (Ovaj test je konfigurirati tako da IT administrator).
6.  Azure Active Directory (Azure AD) provjerava je li uređaj zahtijeva registraciju za upravljanje mobilnim uređajima.
7.  Windows registrira uređaj u imenik tvrtke ili ustanove Azure AD i vas se upisuje u odjeljku upravljanje mobilnim uređajima, ako je to potrebno.
8.  Ako ste korisnik upravljanih, Windows vodi na radnu površinu kroz automatsko prijavu.
9.  Ako ste pridruženi korisnik, bit ćete usmjereni da biste u sustavu Windows zaslon za prijavu u da biste unijeli vjerodajnice.

## <a name="additional-information"></a>Dodatne informacije
* [Windows 10 za tvrtke: načina korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblak na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Provjera autentičnosti identiteta bez lozinke kroz Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Saznajte više o korištenju scenariji za uključivanje za Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
