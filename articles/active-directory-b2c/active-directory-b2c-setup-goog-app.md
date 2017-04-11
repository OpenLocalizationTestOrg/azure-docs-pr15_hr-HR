<properties
    pageTitle="Azure Active Directory B2C: Google + konfiguracije | Microsoft Azure"
    description="Navedite prijave i prijavu njezinoj Google + računa u vašem aplikacijama koje su zaštićene po Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Navedite prijave i prijavu njezinoj Google + računa

## <a name="create-a-google-application"></a>Stvaranje aplikacije Google +

Da biste upotrijebili Google + davateljem identiteta u B2C Azure Active Directory (Azure AD), morate stvoriti aplikaciju Google + i navesti s desnog parametra. Potreban račun Google + da biste to učinili. Ako ga nemate, možete je doći pri [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Otvorite [Google konzole za razvojne inženjere](https://console.developers.google.com/) i prijavite se pomoću Google + vjerodajnice za račun.
2. Kliknite **Stvori projekt**, unesite **Naziv projekta**, a zatim kliknite **Stvori**.

    ![Google + – prvi koraci](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + – novi projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Kliknite **Upravitelj API -JA** , a zatim kliknite **vjerodajnice** u lijevom navigacijskom oknu.
4. Kliknite karticu **OAuth pristanak zaslona** pri vrhu.

    ![Google + - vjerodajnice](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Odaberite ili Navedite valjanu **adresu e-pošte**, navedite **naziv proizvoda**i kliknite **Spremi**.

    ![Google + - OAuth pristanak zaslona](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Kliknite **novu vjerodajnice** , a zatim odaberite **OAuth ID klijenta**.

    ![Google + - OAuth pristanak zaslona](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. U odjeljku **Vrsta aplikacije**, odaberite **web-aplikaciju**.

    ![Google + - OAuth pristanak zaslona](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Navedite **naziv** aplikacije, unesite `https://login.microsoftonline.com` u polju **ovlašteni JavaScript drugačijeg izvora** i `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` u polju **ovlaštene preusmjeravanje ji** . **{Klijentu}** zamijenite nazivom vaš klijent (na primjer, contosob2c.onmicrosoft.com). **{Klijentu}** vrijednost je velika i mala slova. Kliknite **Stvori**.

    ![Google + – stvaranje ID klijenta](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Kopirajte vrijednosti **ID klijenta** i **tajna klijenta**. Trebat će vam oba da biste konfigurirali Google + kao davateljem identiteta na klijentu. **Tajna klijent** je vjerodajnicu za zaštitu.

    ![Google + - tajna klijenta](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Konfiguriranje Google + kao davateljem identiteta na klijentu

1. Slijedite ove korake da biste [došli do značajke plohu B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portala za Azure.
2. Na značajke plohu B2C kliknite **davatelji identiteta**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. Navedite neslužbeni **naziv** za konfiguraciju davatelja identiteta. Na primjer, unesite "G +".
5. Kliknite **vrstu davatelja identiteta**, odaberite **Google**pa kliknite **u redu**.
6. Kliknite **Postavi ovo davatelja identiteta** i unesite ID klijenta i klijent tajna Google + aplikacije koje ste prethodno stvorili.
7. Kliknite **u redu** , a zatim kliknite **Stvori** da biste spremili konfiguraciju Google +.
