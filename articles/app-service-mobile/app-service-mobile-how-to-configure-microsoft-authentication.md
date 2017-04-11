<properties
    pageTitle="Konfiguriranje provjere autentičnosti Microsoftov Account aplikacije servisa za aplikaciju"
    description="Saznajte kako konfigurirati Microsoftov Account provjere autentičnosti za svoju aplikaciju servisa aplikacija."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje Microsoftova Account prijava

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

U ovoj se temi objašnjava konfiguriranje aplikacije servisa za Azure da biste koristili Microsoftov Account kao davatelj usluge provjere autentičnosti. 

## <a name="register-microsoft-account"> </a>Registrirati aplikaciju pomoću Microsoftova računa

1. Prijavite se na [portal za Azure], a zatim otvorite u aplikaciji. Kopirajte **URL-a**, koji se kasnije koristite da biste konfigurirali aplikaciju pomoću Microsoftova Account.

2. Idite na stranicu [Moje aplikacije] u Microsoft Account Developer Center i prijavite se pomoću Microsoftova računa, po potrebi.

3. Kliknite **Dodaj aplikaciju**, a zatim upišite naziv aplikacije pa kliknite **Stvori aplikacija**.

4. Zabilježite **ID aplikacije**, kao što je jer će vam kasnije. 

5. U odjeljku "Platforme", kliknite **Dodaj platforme** i odaberite "Web".

6. U odjeljku "Preusmjeravanje ji" navesti krajnja točka za aplikaciju, a zatim kliknite **Spremi**. 
 
    >[AZURE.NOTE]Preusmjeravanje URI je URL-a aplikacije dodan putom, _/.auth/login/microsoftaccount/callback_. Na primjer, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Provjerite koristite li HTTPS shemu.

7. U odjeljku "Aplikacije tajne", kliknite **Generiraj novu lozinku**. Zabilježite vrijednost koja će se prikazati. Kada napustite stranicu, on neće prikazat će se ponovno.


    > [AZURE.IMPORTANT] Lozinka je vjerodajnicu za zaštitu. Lozinka za zajedničko korištenje sa svima ili distribucija unutar klijentske aplikacije.

## <a name="secrets"> </a>Informacije dodavanje Microsoftova računa u aplikaciji servisnu aplikaciju

1. Ponovno [Azure portal]otvorite aplikaciju, kliknite **Postavke** > **provjere autentičnosti / autorizacije**.

2. Ako provjera autentičnosti / autorizacije značajka nije omogućena, prijeđite **na**.

3. Kliknite **Microsoftov račun**. Lijepljenje vrijednosti aplikacije ID i lozinku koju ste prethodno i po želji omogućite sve opsega potrebama u aplikaciji. Zatim kliknite **u redu**.

    ![][1]

    Prema zadanim postavkama aplikacije servisa za omogućuje provjeru autentičnosti, ali ograničiti ovlašteni pristup sadržaju web-mjesta i API-ji. Korisnici moraju Autorizirajte u kodu aplikacije.

4. (Neobavezno) Da biste ograničili pristup web-mjestu samo korisnici provjere autentičnosti putem Microsoftova računa, postavite **poduzeti kada zahtjeva nije autentičnost** na **Microsoftov račun**. To su potrebne da sve zahtjeve za moguće provjeriti autentičnost, a sve zahtjeve za Neprovjereni vas preusmjerava na Microsoftov račun za provjeru autentičnosti.

5. Kliknite **Spremi**.

Sada ste spremni za korištenje Microsoftova Account za provjeru autentičnosti u aplikaciji.

## <a name="related-content"> </a>Povezani sadržaj

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Moje aplikacije]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Portal za Azure]: https://portal.azure.com/
