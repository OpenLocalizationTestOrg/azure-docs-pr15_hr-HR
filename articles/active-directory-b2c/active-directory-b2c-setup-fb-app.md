<properties
    pageTitle="Azure Active Directory B2C: Konfiguracija servisa Facebook | Microsoft Azure"
    description="Korisnici s računom servisa Facebook u vašem aplikacijama koje su zaštićene po Azure Active Directory B2C omogućuju prijave i prijavu."
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Navedite prijave i prijavu njezinoj s računima za Facebook

## <a name="create-a-facebook-application"></a>Stvaranje aplikacije servisa Facebook

Da biste koristili Facebook davateljem identiteta u B2C Azure Active Directory (Azure AD), morate stvoriti aplikaciju za Facebook i navesti s desnog parametra. Potreban račun servisa Facebook da biste to učinili. Ako ga nemate, možete je doći pri [https://www.facebook.com/](https://www.facebook.com/).

1. Idite na web-mjesto [Facebook za razvojne inženjere](https://developers.facebook.com/) i prijavite se pomoću vjerodajnica za račun servisa Facebook.
2. Ako već niste učinili, morate registrirati kao Facebook za razvojne inženjere. Da biste to učinili, kliknite **registrirati** (u gornjem desnom kutu stranice), prihvatite pravilnike Facebook, i dovršite korake za registraciju.
3. Kliknite **Moje aplikacije** , a zatim kliknite **Dodaj novu aplikaciju**. Odaberite **web-mjesto** kao platforme, a zatim kliknite **Preskoči i stvorite ID aplikacije**.

    ![Facebook – dodavanje nove aplikacije](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebook – dodavanje nove aplikacije – web-mjesta](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebook – stvaranje ID-a aplikacije](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. Na obrascu, navedite **Zaslonsko ime**, valjani **E-pošte za kontakt**, a zatim odgovarajuću **kategoriju**pa kliknite **Stvaranje ID aplikacije**. Morate prihvatiti pravila platformu Facebook i dovršili programa online sigurnosne provjere.

    ![Facebook - stvorite novi ID aplikacije](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. Na lijevom navigacijskom oknu kliknite **Postavke** .
6. Kliknite **+ Dodaj platforme** , a zatim odaberite **web-mjesta**.

    ![Facebook – postavke](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![Web-mjesto Facebook – postavke-](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. Unesite [https://login.microsoftonline.com/](https://login.microsoftonline.com/) u polje **URL web-mjesta** , a zatim kliknite **Spremi promjene**.

    ![Facebook - URL web-mjesta](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Kopirajte vrijednost ID-a **Aplikacije**. Kliknite **Prikaz** , a zatim kopirajte vrijednost **Aplikaciji tajna**. Trebat će vam oba Konfiguriranje servisa Facebook kao davateljem identiteta na klijentu. **Tajna aplikacija** je vjerodajnicu za zaštitu.

    ![Facebook - ID aplikacije i aplikaciji tajna](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Kliknite **+ Dodaj proizvoda** na lijevom navigacijskom oknu, a zatim gumb **Početak** uz **Facebook prijava**.

    ![Facebook – prijava za Facebook](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Unesite `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` u polju **valjani OAuth preusmjeravanje ji** u odjeljku **Postavke OAuth klijenta** . **{Klijentu}** zamijenite nazivom vaš klijent (na primjer, contosob2c.onmicrosoft.com). Kliknite **Spremi promjene** pri dnu stranice.

    ![Facebook - preusmjeravanje OAuth URI-JA](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. Da bi se može koristiti Azure AD B2C aplikaciju servisa Facebook, morate učiniti dostupnom javnosti. To možete učiniti tako da kliknete **Pregled aplikacije** na lijevom navigacijskom oknu i isključivanjem parametar pri vrhu stranice na **da** , a zatim kliknite **Potvrdi**.

    ![Facebook – javno aplikacije](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfiguriranje servisa Facebook kao davateljem identiteta na klijentu

1. Slijedite ove korake da biste [došli do značajke plohu B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portala za Azure.
2. Na značajke plohu B2C kliknite **davatelji identiteta**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. Navedite neslužbeni **naziv** za konfiguraciju davatelja identiteta. Na primjer, unesite "FB".
5. Kliknite **vrstu davatelja identiteta**, odaberite **Facebook**i kliknite **u redu**.
6. Kliknite **Postavi ovo davatelja identiteta** , a zatim unesite ID aplikacije i aplikaciji tajna (od Facebook aplikaciju koju ste ranije stvorili) u **ID klijenta** i **klijent tajna** polja odnosno.
7. Kliknite **u redu**, a zatim kliknite **Stvori** da biste spremili konfiguraciju servisa Facebook.
