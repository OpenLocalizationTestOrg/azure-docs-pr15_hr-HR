<properties
    pageTitle="Azure Active Directory B2C: Konfiguracija računa Microsoft | Microsoft Azure"
    description="Navedite prijave i prijavu njezinoj s Microsoftova računa u vaše aplikacije koje su zaštićene po Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Navedite prijave i prijavu njezinoj pomoću Microsoftova računa

## <a name="create-a-microsoft-account-application"></a>Stvaranje aplikacije Microsoftova računa

Da biste koristili Microsoftov račun kao davateljem identiteta u B2C Azure Active Directory (Azure AD), morate stvoriti račun za aplikaciju Microsoft i navesti s desnog parametra. Potreban Microsoftov račun da biste to učinili. Ako ga nemate, možete je doći pri [https://www.live.com/](https://www.live.com/).

1. Idite na [Portal za registraciju aplikacije Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) i prijavite se pomoću vjerodajnicama za Microsoftov račun.
2. Kliknite **Dodaj aplikaciju**.

    ![Microsoftova računa – dodavanje nove aplikacije](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Navedite **naziv** aplikacije, a zatim kliknite **Stvori aplikacija**.

    ![Microsoftova računa – naziv aplikacije](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Kopirajte vrijednost **Id aplikacije**. Potrebno je konfigurirati Microsoftov račun kao davateljem identiteta na klijentu.

    ![Microsoftova računa – Id aplikacije](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. Kliknite **Dodaj platforme** i odaberite **Web**.

    ![Microsoftova računa – dodavanje platforme](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Microsoftova računa – Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Unesite `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` u polju **Ji preusmjeravanje** . **{Klijentu}** zamijenite nazivom vaš klijent (na primjer, contosob2c.onmicrosoft.com).

    ![Microsoftova računa – preusmjeravanje URL-a](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. Kliknite **Generiraj novu lozinku** u odjeljku **Tajne aplikacije** . Kopirajte novu lozinku na zaslonu. Potrebno je konfigurirati Microsoftov račun kao davateljem identiteta na klijentu. Ova lozinka je vjerodajnicu za zaštitu.

    ![Microsoftova računa – stvaranje nove lozinke](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Microsoftova računa – novu lozinku](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. Potvrdite okvir koja vas obavještava da **podržava Live SDK** u odjeljku **Napredne mogućnosti** . Kliknite **Spremi**.

    ![Microsoftova računa – podrška Live SDK](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Konfiguriranje Microsoftov račun kao davateljem identiteta na klijentu

1. Slijedite ove korake da biste [došli do značajke plohu B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portala za Azure.
2. Na značajke plohu B2C kliknite **davatelji identiteta**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. Navedite neslužbeni **naziv** za konfiguraciju davatelja identiteta. Na primjer, unesite "MSA".
5. Kliknite **vrstu davatelja identiteta**, odaberite **Microsoftov račun**i kliknite **u redu**.
6. Kliknite **Postavljanje davatelj identiteta** i unesite aplikacije Id i lozinku računa aplikacije Microsoft koju ste ranije stvorili.
7. Kliknite **u redu** , a zatim kliknite **Stvori** da biste spremili svoje konfiguracija računa Microsoft.
