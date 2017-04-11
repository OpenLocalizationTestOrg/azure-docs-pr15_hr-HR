<properties
    pageTitle="Postavljanje novog uređaja s Azure AD tijekom postavljanja | Microsoft Azure"
    description="Tema na kojem se objašnjava kako korisnici mogu postavljati Azure AD uključivanje tijekom prvog pokretanja iskustva."
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Postavljanje novog uređaja s Azure AD tijekom instalacije

U sustavu Windows 10, korisnici mogu pridružiti svoje uređaje Azure Active Directory (Azure AD) u prvog pokretanja sustava (FRX). Time se omogućuje tvrtke i ustanove moraju distribucija shrink-wrapped uređaja za svoje zaposlenike i učenici ili obavijestite tu osobu odaberite vlastite uređaje (CYOD).
Ako je na uređaju instaliran izdanja sustava Windows 10 Professional i Windows 10 Enterprise, sučelje po zadanom odabrana postupka za postavljanje za uređaje tvrtke vlasništvu.

## <a name="to-join-a-device-to-azure-ad"></a>Da biste se pridružili uređaj za Azure AD


1. Kada uključite novi uređaj i pokretanje postupka za postavljanje, trebali biste vidjeti **Početak spreman** poruke. Slijedite upute za postavljanje uređaja.
2. Pokrenite prilagodbom regija i jezik. Zatim prihvatite licencne odredbe za Microsoftov softver.
![Prilagodba za vašu regiju](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Odabir mreže koju želite koristiti za povezivanje s Internetom.
4. Odaberite želite li koristite osobnim uređajem ili uređaj tvrtke vlasništvu. Ako je vlasništvu tvrtke, kliknite **ovaj uređaj pripada moje tvrtke ili ustanove**. Time ćete sučelje za Azure AD uključiti. Slijedi zaslona da ćete vidjeti ako koristite Windows 10 Professional.
<center>
![Tko je vlasnik ovaj zaslon računala](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Unesite vjerodajnice koje ste dobili od tvrtke ili ustanove.
<center>
![Zaslon za prijavu](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Nakon što unesete korisničko ime, odgovarajući klijent nalazi se u Azure AD. Ako ste u pridruženoj domeni, koje će biti preusmjereni na poslužitelj sigurne tokena servisa (STS) lokalnog –, na primjer, Active Directory Federation Services (AD FS).
7. Ako ste korisnik iz domene koje nisu pridružene, unesite vjerodajnice izravno na stranicu za Azure AD hostira. Ako je konfiguriran tvrtke brendiranje, će potražite u članku logotipa tvrtke ili ustanove i podržava teksta.
8.  Zatražit će se za pitanja i odgovora višestruke provjere autentičnosti. Ovaj test je konfigurirati tako da IT administrator.
9.  Azure AD provjerava je li korisnik/uređaj zahtijeva registraciju u upravljanje mobilnim uređajima.
10. Windows registrira uređaj u imenik tvrtke ili ustanove Azure AD i vas se upisuje u odjeljku upravljanje mobilnim uređajima, ako je to potrebno.
11. Ako ste korisnik upravljanih, Windows vodi vas na radnoj površini kroz postupak automatske prijave.
12. Ako ste pridruženi korisnik, bit ćete preusmjereni na zaslon za prijavu u Windows da biste unijeli vjerodajnice.

> [AZURE.NOTE] Pridruživanje lokalne domene Windows Server Active Directory u sučelje za Windows – okvir nije podržana. Stoga Ako planirate uključivanje računala s domenom, umjesto toga treba odaberite vezu **postavljanja sustava Windows s lokalnog računa** . Zatim možete uključiti domenu od postavki na računalu kao što ste ih dodali prije.

## <a name="additional-information"></a>Dodatne informacije
* [Windows 10 za tvrtke: načina korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblak na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Provjera autentičnosti identiteta bez lozinke kroz Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Saznajte više o korištenju scenariji za uključivanje za Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
