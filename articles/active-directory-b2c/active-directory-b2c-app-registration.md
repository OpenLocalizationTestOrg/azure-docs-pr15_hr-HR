<properties
    pageTitle="Azure Active Directory B2C: Registracija programa | Microsoft Azure"
    description="Kako registrirati aplikacije s Azure Active Directory B2C"
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registracija aplikacije

## <a name="prerequisite"></a>Preduvjeta

Da biste sastavili aplikacija koja prihvaća korisničke prijave i prijavu, najprije morate registrirati aplikacije s klijentom sustava Azure Active Directory B2C. Dobiti vlastiti klijentu slijedeći korake navedene u [Stvori klijent za Azure AD B2C](active-directory-b2c-get-started.md). Kad prođete sve korake navedene u tom članku, imat ćete plohu značajke B2C prikvačiti na Startboard.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Dođite do značajke plohu B2C

Ako imate plohu značajke B2C prikvačiti na Startboard, prikazat će se plohu čim se prijaviti na [portal za Azure](https://portal.azure.com/) kao globalni Administrator B2C klijentu.

Na plohu možete pristupiti i klikom na **Pregled** , a zatim **Azure AD B2C** u lijevom navigacijskom oknu [Azure portal](https://portal.azure.com/).

> [AZURE.IMPORTANT] Morate biti globalni Administrator klijenta B2C da biste mogli pristupiti značajke plohu B2C. Globalni Administrator iz drugih klijenta ili korisnika iz bilo kojeg klijent ne može pristupiti.  Možete se prebacivati klijent sustava B2C koristeći alat za prebacivanje klijenta u gornjem desnom kutu Portal za Azure.

## <a name="register-an-application"></a>Registracija aplikacije

1. Na plohu značajke B2C portala za Azure, kliknite **aplikacije**.
2. Kliknite **Dodaj +** pri vrhu na plohu.
3. Unesite **naziv** za aplikaciju koja će se opisuju aplikacije njezinoj. Na primjer, nije moguće unesite "Contoso B2C aplikacije".
4. Ako pišete web-aplikacija, uključivanje ili isključivanje parametar **uključiti web-aplikacije / web-API-JA** na **da**. **Odgovori URL-ovi** su krajnje točke gdje Azure AD B2C vratit će sve tokena koje zahtjeva za aplikaciju. Na primjer, unesite `https://localhost:44321/`. Ako web-aplikacija također biti Pozivanje neke API osigurava Azure AD B2C na webu, ćete htjeti stvoriti **Aplikaciji tajna** kao i tako da kliknete gumb **Stvori ključ** .

    > [AZURE.NOTE] **Aplikaciji tajna** je vjerodajnicu za zaštitu i trebali biste pravilno zaštićen.

5. Ako pišete mobilnu aplikaciju, uključivanje ili isključivanje parametar **nativni klijent za uvrštavanje** na **da**. Kopirajte dolje zadani **Preusmjeravanje URI** koji će se automatski stvoriti.
6. Kliknite **Stvori** da biste registrirali aplikacije.
7. Kliknite aplikaciju koju ste upravo stvorili, a zatim kopirajte dolje globalno jedinstveni **ID klijenta za aplikaciju** koja će se koristiti u nastavku kod.

> [AZURE.IMPORTANT] Aplikacijama koje su stvorene u značajke plohu B2C morati upravlja na istom mjestu. Ako uređujete B2C aplikacije pomoću programa PowerShell ili drugog portala mogu postati nepodržane i vjerojatno neće funkcionirati ako Azure AD B2C.

## <a name="build-a-quick-start-application"></a>Stvaranje aplikacije za brzi početak rada

Sad kad ste aplikaciju registriran Azure AD B2C, možete Provedite jedan od naše vodiči za Brzi start započnete s radom. Evo nekoliko preporuka:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
