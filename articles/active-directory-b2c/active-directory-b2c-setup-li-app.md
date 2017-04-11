<properties
    pageTitle="Azure Active Directory B2C: Konfiguracija servisa LinkedIn | Microsoft Azure"
    description="Navedite prijave i prijavu njezinoj s računima LinkedIn u vaše aplikacije koje su zaštićene po Azure Active Directory B2C"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Navedite prijave i prijavu njezinoj s računom servisa LinkedIn

## <a name="create-a-linkedin-application"></a>Stvaranje aplikacije servisa LinkedIn

Da biste koristili LinkedIn davateljem identiteta u B2C Azure Active Directory (Azure AD), morate stvoriti aplikaciju LinkedIn i navesti s desnog parametra. Potreban račun servisa LinkedIn da biste to učinili. Ako ga nemate, možete je doći pri [https://www.linkedin.com/](https://www.linkedin.com/).

1. Idite na sustava [razvojnim inženjerima LinkedIn web-mjesto](https://www.developer.linkedin.com/) i prijavite se pomoću vjerodajnica za račun servisa LinkedIn.
2. Kliknite **Moje aplikacije** na traci izbornika na vrhu, a zatim kliknite **Stvori aplikacija**.

    ![LinkedIn - nove aplikacije](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. U obrazac za **Stvaranje nove aplikacije** unesite odgovarajuće podatke (**Naziv tvrtke**, **naziv**, **Opis**, **URL za logotip aplikacije**, **Koristite aplikaciju**, **URL web-mjesta**, **Poslovnu e-poštu** i **Službeni telefon**).
4. Pristajete da **LinkedIn API uvjete korištenja** , a zatim kliknite **Pošalji**.

    ![LinkedIn - Register aplikacije](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Kopirajte vrijednosti **ID klijenta** i **Tajna klijenta**. (Ćete ih pronaći u odjeljku **Provjera autentičnosti tipke**.) Trebat će vam oba Konfiguriranje servisa LinkedIn kao davateljem identiteta na klijentu.

    >[AZURE.NOTE] **Tajna klijent** je vjerodajnicu za zaštitu.

6. Unesite `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` u polju **Ovlašteni preusmjeravanje URL-ovi** (u odjeljku **OAuth 2.0**). **{Klijentu}** zamijenite nazivom vaš klijent (na primjer, contoso.onmicrosoft.com). Kliknite **Dodaj**, a zatim kliknite **Ažuriraj**. **{Klijentu}** vrijednost je velika i mala slova.

    ![LinkedIn – postavljanje aplikacije](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Konfiguriranje servisa LinkedIn kao davateljem identiteta na klijentu

1. Slijedite ove korake da biste [došli do značajke plohu B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portala za Azure.
2. Na značajke plohu B2C kliknite **davatelji identiteta**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. Navedite neslužbeni **naziv** za konfiguraciju davatelja identiteta. Na primjer, unesite "Ćelije".
5. Kliknite **vrstu davatelja identiteta**, odaberite **LinkedIn**pa kliknite **u redu**.
6. Kliknite **Postavi ovo davatelja identiteta** i unesite ID klijenta i klijent tajna aplikacije LinkedIn koju ste prethodno stvorili.
7. Kliknite **u redu** , a zatim kliknite **Stvori** da biste spremili konfiguraciju LinkedIn.
