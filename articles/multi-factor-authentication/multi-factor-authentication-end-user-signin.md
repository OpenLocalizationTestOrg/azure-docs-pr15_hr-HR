<properties
    pageTitle="Azure MFA prijava doživljaj Azure višestruka provjera autentičnosti"
    description="Ova stranica će vam upute gdje se da biste vidjeli različite načine prijava dostupno u sklopu Azure MFA."
    keywords="Provjera autentičnosti korisnika, prijavu doživljaj, prijavite se pomoću mobilnog telefona, prijavite se pomoću telefona"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Prijava doživljaj Azure višestruka provjera autentičnosti
> [AZURE.NOTE]  Sljedeće dokumentaciji na ovoj stranici prikazuje na uobičajeni sučelje za prijavu.  Pomoć s prijavom potražite u članku [imate li poteškoća s Azure višestruka provjera autentičnosti](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Što će sučelje za prijavu?
Ovisno o tome kako prijaviti u i koristiti višestruke provjere autentičnosti, iskustva razlikovat će se.  U ovom odjeljku ne možemo pronaći ćete informacije o tome što mogu očekivati kada se prijavite.  Odaberite onaj koji najbolje opisuje postupke:


Šta radiš?|Opis
:------------- | :------------- |
[Prijava pomoću telefonski broj za pozive iz mobilne mreže ili sustava office](#signing-in-with-mobile-or-office-phone) | Ovo je što možete očekivati od prijave pomoću telefona broj za pozive iz mobilne mreže ili sustava office.
[Prijava pomoću aplikacije Microsoft Authenticator pomoću obavijesti](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Ovo je što možete očekivati pomoću aplikacije Microsoft Authenticator s obavijesti.
[Prijava pomoću aplikacije Microsoft Authenticator pomoću koda za potvrdu](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Ovo je što možete očekivati thapp Microsoft Authenticator pomoću koda za potvrdu.
[Prijava pomoću zamjenski način](#signing-in-with-an-alternate-method)|Time ćete prikazati što mogu očekivati ako želite koristiti zamjenske metode.

## <a name="signing-in-with-mobile-or-office-phone"></a>Prijava pomoću telefonski broj za pozive iz mobilne mreže ili sustava office

Podaci navedeni u nastavku opisane su pisanju višestruka provjera autentičnosti pomoću telefona broj za pozive iz mobilne mreže ili office.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Da biste se prijavili pomoću poziva office ili mobilni telefon

- Prijavite se u aplikaciju ili servis kao što je Office 365 pomoću korisničkog imena i lozinke.
- Microsoft će uspostavi poziv sa mnom.

![Microsoft poziva](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Odgovorite na telefon, a zatim pritisnete tipku #.

![Odgovor](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Trebali biste sada prijavili u.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Prijava pomoću aplikacije Microsoft Authenticator pomoću obavijesti

Podaci navedeni u nastavku opisane su doživljaj prilikom korištenja višestruka provjera autentičnosti pomoću aplikacije Microsoft Authenticator kada se šalju obavijesti.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Prijavite se pomoću obavijest poslali aplikaciju Microsoft Authenticator

- Prijavite se u aplikaciju ili servis kao što je Office 365 pomoću korisničkog imena i lozinke.
- Microsoft će poslati primatelju obavijest.

![Microsoft će poslati obavijest](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Odgovorite na telefon, a zatim pritisnete tipku potvrdi.  Ako je vaša tvrtka potreban PIN-a koji će se tražiti je ovdje.

![Provjerite je li](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Postavljanje](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Trebali biste sada prijavili u.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Prijava pomoću aplikacije Microsoft Authenticator pomoću koda za potvrdu

Podaci navedeni u nastavku opisane su doživljaj prilikom korištenja višestruka provjera autentičnosti pomoću aplikacije Microsoft Authenticator kada ga koristite s kod za potvrdu.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Da biste se prijavili u kod za potvrdu pomoću aplikacije Microsoft Authenticator

- Prijavite se u aplikaciju ili servis kao što je Office 365 pomoću korisničkog imena i lozinke.
- Microsoft će vas tražiti kod za potvrdu.

![Unesite kod za potvrdu](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Otvorite aplikaciju Microsoft Authenticator na telefonu i unesite kod u okvir mjesto koje se prijavljujete.

![Dobiti kod](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Trebali biste sada prijavili u.


## <a name="signing-in-with-an-alternate-method"></a>Prijava pomoću zamjenski način


U sljedećoj sekciji vidjet ćete kako se prijaviti pomoću zamjenski način načina primarni možda neće biti dostupne.

### <a name="to-sign-in-with-an-alternate-method"></a>Da biste se prijavili pomoću zamjenski način

- Prijavite se u aplikaciju ili servis kao što je Office 365 pomoću korisničkog imena i lozinke.
- Odaberite koristite drugi potvrdu mogućnost.  Postojati s izbor različite mogućnosti. Broj prikazat će se temeljiti na koliko ste postavili Dodatno.

![Korištenje zamjenske metode](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Odaberite zamjenski način i prijavite se.
