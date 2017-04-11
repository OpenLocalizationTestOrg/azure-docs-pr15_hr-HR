<properties
    pageTitle="Azure Active Directory B2C: Konfiguriranje Amazon | Microsoft Azure"
    description="Korisnici s računima Amazon u vaše aplikacije koje su zaštićene po Azure Active Directory B2C omogućuju prijave i prijavu."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Navedite prijave i prijavu njezinoj Amazon računa

## <a name="create-an-amazon-application"></a>Stvaranje aplikacije komponente Amazon

Da biste koristili Amazon davateljem identiteta u B2C Azure Active Directory (Azure AD), morate stvoriti aplikaciju Amazon i navesti s desnog parametra. Potreban vam je račun Amazon da biste to učinili. Ako ga nemate, možete je doći pri [http://www.amazon.com/](http://www.amazon.com/).

1. Idite u [Centar za razvojne inženjere Amazon](https://login.amazon.com/) i prijavite se pomoću vjerodajnica za račun Amazon.
2. Ako već niste učinili, kliknite **Potpiši**, slijedite korake za registraciju za razvojne inženjere i prihvatite pravila.
3. Kliknite **registrirati nove aplikacije**.

    ![Registracija nove aplikacije na web-mjestu Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Sadrže informacije o aplikaciji (**naziv**, **Opis**i **URL obavijest o zaštiti privatnosti**), a zatim kliknite **Spremi**.

    ![Pruža informacije o aplikaciji za registriranje novu aplikaciju na Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. U odjeljku **Web-postavke** kopirajte vrijednosti **ID klijenta** i **Tajna klijenta**. (Morate kliknuti gumb **Pokaži tajna** da biste vidjeli taj.) Potreban vam je oba da biste konfigurirali Amazon kao davateljem identiteta na klijentu. Kliknite **Uređivanje** pri dnu odjeljka. **Tajna klijent** je vjerodajnicu za zaštitu.

    ![ID klijenta i tajna klijenta za novu aplikaciju na Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Unesite `https://login.microsoftonline.com` u polju **Dopušteno drugačijeg izvora JavaScript** i `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` u polju **Dopušteno vratili URL-ova** . **{Klijentu}** zamijenite nazivom vaš klijent (na primjer, contoso.onmicrosoft.com). Kliknite **Spremi**. **{Klijentu}** vrijednost je velika i mala slova.

    ![Pružanje JavaScript drugačijeg izvora i povratna URL-ovi za novu aplikaciju na Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Konfiguriranje Amazon kao davateljem identiteta na klijentu

1. Slijedite ove korake da biste [došli do značajke plohu B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portala za Azure.
2. Na značajke plohu B2C kliknite **davatelji identiteta**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. Navedite neslužbeni **naziv** za konfiguraciju davatelja identiteta. Na primjer, unesite "Amzn".
5. Kliknite **vrstu davatelja identiteta**, odaberite **Amazon**pa kliknite **u redu**.
6. Kliknite **Postavi ovo davatelja identiteta** i unesite ID klijenta i klijent tajna Amazon aplikacije koje ste prethodno stvorili.
7. Kliknite **u redu** , a zatim kliknite **Stvori** da biste spremili konfiguraciju Amazon.
