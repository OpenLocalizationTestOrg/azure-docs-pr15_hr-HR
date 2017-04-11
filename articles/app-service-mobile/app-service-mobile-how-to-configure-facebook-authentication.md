<properties
    pageTitle="Upute za konfiguriranje provjere autentičnosti za aplikaciju aplikacije servisa Facebook"
    description="Upute za konfiguriranje provjere autentičnosti za aplikaciju aplikacije servisa Facebook."
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

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje Facebook prijava

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

U ovoj se temi objašnjava konfiguriranje Azure aplikacije servisa za korištenje servisa Facebook kao davatelj usluge provjere autentičnosti.

Da biste dovršili postupak u ovoj temi, morate imati račun za Facebook provjereno e-pošte adresa i broj mobilnog telefona. Da biste stvorili novi račun za Facebook, idite na [facebook.com].

## <a name="register"> </a>Registrirati aplikacija sa servisom Facebook

1. Prijavite se na [portal za Azure], a zatim otvorite u aplikaciji. Kopirajte **URL-a**. To će koristiti da biste konfigurirali aplikaciju servisa Facebook.

2. U drugom prozoru preglednika, idite na web-mjesto [Facebook razvojnim inženjerima] i prijavite se pomoću vjerodajnice za račun servisa Facebook.

3. (Neobavezno) Ako nije već registrirana, kliknite **aplikacije** > **registrirati kao programer**, zatim prihvaćanje pravila i slijedite korake za registraciju.

4. Kliknite **Moje aplikacije** > **dodati novu aplikaciju** > **web-mjesto** > **preskočiti i stvorite ID aplikacije**. 

5. U **Zaslonsko ime**, upišite jedinstveni naziv aplikacije, upišite **E-pošte za kontakt**, odaberite **kategoriju** za aplikaciju programa, a zatim kliknite **Stvori ID aplikacije** i dovršiti provjeru sigurnost. To će vas odvesti na nadzornoj ploči za razvoj za novu aplikaciju servisa Facebook.

6. U odjeljku "Facebook prijava", kliknite **Kreni**. Dodajte svoje aplikacije **Preusmjeravanje URI** **valjani OAuth**preusmjeravanje ji, a zatim kliknite **Spremi promjene**. 

    > [AZURE.NOTE] Preusmjeravanje URI je URL-a aplikacije dodan putom, _/.auth/login/facebook/callback_. Na primjer, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Provjerite koristite li HTTPS shemu.

6. U lijevom navigacijskom oknu kliknite **Postavke**. U polju **Aplikaciji tajna** kliknite **Prikaz**, unesite lozinku ako ste tražili, a zatim zabilježite vrijednosti **ID aplikacije** i **Aplikaciji tajna**. Koristite ih kasnije da biste konfigurirali aplikaciju u Azure.

    > [AZURE.IMPORTANT] Tajna aplikacija je vjerodajnicu za zaštitu. Zajedničko korištenje ovog tajna sa svima ili distribucija unutar klijentske aplikacije.

7. Račun za Facebook koja se koristila za registraciju aplikacija je administrator aplikacije. U ovom trenutku samo administratori mogu se prijaviti u ovu aplikaciju. Za provjeru autentičnosti drugih računa servisa Facebook, kliknite **Pregled aplikacije** i omogućite **< vaše--naziv aplikacije > učini javno dostupnim** da biste omogućili Općenito javni pristup pomoću provjere autentičnosti Facebook.

## <a name="secrets"> </a>Facebook dodajte informacije u aplikaciji

1. Vratite se u [Azure portal]dođite do aplikacija. Kliknite **Postavke** > **provjere autentičnosti / autorizacije**, i provjerite je li **Provjera autentičnosti za aplikaciju servisa** **na**.

2. Kliknite **Facebook**, zalijepite u aplikacije i ID- a aplikaciji tajna vrijednosti koje ste prethodno, po želji Omogući sve opsega potrebne za svoju aplikaciju, a zatim kliknite **u redu**.

    ![][0]

    Prema zadanim postavkama aplikacije servisa za omogućuje provjeru autentičnosti, ali ograničiti ovlašteni pristup sadržaju web-mjesta i API-ji. Korisnici moraju Autorizirajte u kodu aplikacije.

3. (Neobavezno) Da biste ograničili pristup web-mjestu samo korisnici provjere autentičnosti putem servisa Facebook, postavite **poduzeti kada zahtjeva nije autentičnost** sa **servisom Facebook**. To su potrebne da sve zahtjeve za moguće provjeriti autentičnost, a sve zahtjeve za Neprovjereni vas preusmjerava na Facebooku za provjeru autentičnosti.

4. Po završetku konfiguriranje provjere autentičnosti, kliknite **Spremi**.

Sada ste spremni za korištenje servisa Facebook za provjeru autentičnosti u svojoj aplikaciji.

## <a name="related-content"> </a>Povezani sadržaj

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Razvojni inženjeri Facebook]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Portal za Azure]: https://portal.azure.com/
