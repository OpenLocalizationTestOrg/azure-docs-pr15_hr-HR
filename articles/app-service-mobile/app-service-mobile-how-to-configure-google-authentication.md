<properties
    pageTitle="Upute za konfiguriranje provjere autentičnosti za aplikaciju aplikacije servisa Google"
    description="Saznajte kako konfigurirati Google provjere autentičnosti za aplikaciju servisa aplikacija."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje Google prijava

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

U ovoj se temi objašnjava konfiguriranje aplikacije servisa za Azure da biste upotrijebili Google davatelj usluge provjere autentičnosti.

Da biste dovršili postupak u ovoj temi, morate imati račun za Google koja je navedena adresa e-pošte potvrđeni. Da biste stvorili novi Google račun, idite na [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Morate registrirati aplikaciju Google

1. Prijavite se na [portal za Azure], a zatim otvorite u aplikaciji. Kopirajte **URL-a**, koji je kasnije koristili za konfiguriranje aplikacije Google.

2. Dođite do web-mjesta [Google API-ji](http://go.microsoft.com/fwlink/p/?LinkId=268303) , prijavite se pomoću vjerodajnica Google račun, kliknite **Stvori projekt**, navedite **Naziv projekta**, a zatim kliknite **Stvori**.

3. U odjeljku **Društvenih API -ji** kliknite **Google + API -JA** , a zatim **Omogući**.

4. U lijevom navigacijskom oknu, **vjerodajnice** > **OAuth pristanak zaslona**, zatim odaberite svoju **adresu e-pošte**, unesite **Naziv proizvoda**i kliknite **Spremi**.

5. Na kartici **vjerodajnice** kliknite **Stvori vjerodajnice** > **OAuth ID klijenta**, a zatim odaberite **web-aplikacija**.

6. Zalijepite aplikacije servisa za **URL** koji ste prethodno kopirali u **Ovlašteni drugačijeg izvora JavaScript**, a zatim zalijepite preusmjeravanje URI **Ovlašteni URI za preusmjeravanje**. Preusmjeravanje URI je URL-a aplikacije dodan putom, _/.auth/login/google/callback_. Na primjer, `https://contoso.azurewebsites.net/.auth/login/google/callback`. Provjerite koristite li HTTPS shemu. Zatim kliknite **Stvori**.

7. Na sljedećem zaslonu zabilježite vrijednosti ID klijenta i tajna klijenta.


    > [AZURE.IMPORTANT]
    Tajna klijent je vjerodajnicu za zaštitu. Zajedničko korištenje ovog tajna sa svima ili distribucija unutar klijentske aplikacije.


## <a name="secrets"> </a>Google dodajte informacije u aplikaciji

8. Vratite se u [Azure portal]dođite do aplikacija. Kliknite **Postavke**, a zatim **provjere autentičnosti / autorizacije**.

9. Ako provjera autentičnosti / autorizacije značajka nije omogućena, uključite prijelaz na **na**.

10. Kliknite **Google**. Zalijepite aplikacije i ID- a aplikaciji tajna vrijednosti koje ste prethodno, a ako želite omogućiti sve opsega potrebama u aplikaciji. Zatim kliknite **u redu**.

    ![][1]

    Prema zadanim postavkama aplikacije servisa za omogućuje provjeru autentičnosti, ali ograničiti ovlašteni pristup sadržaju web-mjesta i API-ji. Korisnici moraju Autorizirajte u kodu aplikacije.

17. (Neobavezno) Da biste ograničili pristup web-mjestu samo korisnici autentičnost po Google, postavite **poduzeti kada zahtjeva nije autentičnost** na **Google**. To su potrebne da sve zahtjeve za moguće provjeriti autentičnost, a sve zahtjeve za Neprovjereni vas preusmjerava u Google za provjeru autentičnosti.

12. Kliknite **Spremi**.

Sada ste spremni za korištenje Google za provjeru autentičnosti u svojoj aplikaciji.

## <a name="related-content"> </a>Povezani sadržaj

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Portal za Azure]: https://portal.azure.com/

