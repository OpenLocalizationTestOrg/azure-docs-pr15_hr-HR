<properties
    pageTitle="Upute za konfiguriranje provjere autentičnosti za aplikaciju aplikacije servisa Twitter"
    description="Saznajte kako konfigurirati Twitter provjere autentičnosti za aplikaciju servisa aplikacija."
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

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje Twitter prijava

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

U ovoj se temi objašnjava konfiguriranje aplikacije servisa za Azure da biste upotrijebili Twitter davatelj usluge provjere autentičnosti.

Da biste dovršili postupak u ovoj temi, morate imati Twitter računa koji ima provjereno e-pošte adresa i telefonski broj. Da biste stvorili novi račun Twitter, idite na <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Morate registrirati aplikacije Twitter


1. Prijavite se na [portal za Azure], a zatim otvorite u aplikaciji. Kopirajte **URL-a**. To će koristiti da biste konfigurirali Twitter aplikacije.

2. Idite na web-mjesto [Servisa Twitter razvojnim inženjerima] , prijavite se pomoću računa vjerodajnice Twitter pa kliknite **Stvorite novu aplikaciju**.

3. Upišite **naziv** i **Opis** nove aplikacije. Zalijepite **URL** vaše aplikacije za vrijednost **web-mjesta** . Nakon toga **URL povratnog**zalijepiti **Povratnog URL** koji ste prethodno kopirali. Ovo je dodan putom, _/.auth/login/twitter/callback_pristupnik za mobilnu aplikaciju. Na primjer, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Provjerite koristite li HTTPS shemu.

3.  Pri dnu stranice, pročitajte i prihvatite uvjete. Zatim kliknite **Stvori aplikacija Twitter**. To registrira aplikacija prikazuje detalje aplikacije.

4. Kliknite karticu **Postavke** , odaberite **Dopusti ove aplikacije koja će se koristiti da biste se prijavili pomoću Twitter**, zatim kliknite **Postavke ažuriranja**.

5. Odaberite karticu **ključeva i tokeni programa Access** . Zabilježite vrijednosti **Potrošača ključ (ključ za API -JA)** i **potrošača tajna (tajna API -JA)**.

    > [AZURE.NOTE] Tajna potrošača je vjerodajnicu za zaštitu. Zajedničko korištenje ovog tajna sa svima ili distribucija s aplikacijom.


## <a name="secrets"> </a>Dodati na Twitteru informacije u aplikaciji

13. Vratite se u [Azure portal]dođite do aplikacija. Kliknite **Postavke**, a zatim **provjere autentičnosti / autorizacije**.

14. Ako provjera autentičnosti / autorizacije značajka nije omogućena, uključite prijelaz na **na**.

15. Kliknite **na Twitteru**. Zalijepite ID aplikacije i aplikaciji tajna vrijednosti koje ste prethodno nabavili. Zatim kliknite **u redu**.

    ![][1]

    Prema zadanim postavkama aplikacije servisa za omogućuje provjeru autentičnosti, ali ograničiti ovlašteni pristup sadržaju web-mjesta i API-ji. Korisnici moraju Autorizirajte u kodu aplikacije.

17. (Neobavezno) Da biste ograničili pristup web-mjestu samo korisnici autentičnost po Twitter, postavite **poduzeti kada zahtjeva nije autentičnost** na **Twitteru**. To su potrebne da sve zahtjeve za moguće provjeriti autentičnost, a sve zahtjeve za Neprovjereni vas preusmjerava u Twitter za provjeru autentičnosti.

17. Kliknite **Spremi**.

Sada ste spremni za korištenje Twitter za provjeru autentičnosti u svojoj aplikaciji.

## <a name="related-content"> </a>Povezani sadržaj

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Razvojni inženjeri twitter]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Portal za Azure]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
