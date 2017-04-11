<properties
pageTitle="Azure Active Directory 2.0 .NET nativni aplikacija | Microsoft Azure"
description="Kako stvoriti .NET nativni aplikaciju koja se korisnicima pomoću oba Microsoftova Account i računi tvrtke ili obrazovne ustanove."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Dodavanje prijava u aplikaciju za radnu površinu sustava Windows

S na krajnjoj točki 2.0 možete brzo dodati provjere autentičnosti u aplikacijama za stolna računala s podrškom za oba Microsoftovi računi i računi tvrtke ili obrazovne ustanove.  Također omogućuje aplikacije za sigurno komunikaciju s pozadinskom web API-ja, kao i [Microsoft Graph](https://graph.microsoft.io) i nekoliko [API Sjedinjeno komuniciranje za Office 365](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Neke scenarije Azure Active Directory (AD) i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

[.NET nativni aplikacije koji se pokreću na uređaju](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD predstavlja Microsoft identiteta provjera autentičnosti biblioteke ili MSAL.  MSAL-isključivom svrhom u životu je da biste olakšali aplikacije da biste dobili tokeni za pozivanje web-servisa.  Da bismo pokazali samo kako je lako, ovdje bismo ćete stvoriti popis zadataka za .NET WPF aplikaciju koja:

- Prijave korisnika i dobije pristup tokeni [OAuth 2.0 protokol provjere autentičnosti](active-directory-v2-protocols.md#oauth2-authorization-code-flow).
- Sigurno poziva pozadinskog web-servisa popis zadataka koji je osigurani i tako da OAuth 2.0.
- Potpisuje korisnika.

## <a name="download-sample-code"></a>Preuzimanje uzorak koda

Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  Da biste pratili, možete [preuzeti aplikacije skeleton kao u .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) ili Kloniraj na skeleton:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

Aplikaciju dovršene navedeni su na kraju ovog vodiča.

## <a name="register-an-app"></a>Registracija aplikacije
Stvaranje nove aplikacije na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite ove [detaljne upute](active-directory-v2-app-registration.md).  Obavezno:

- Kopiraj dolje **Id aplikacije** dodijeljene aplikacije, morat ćete ga uskoro.
- Dodajte platformu **mobilne** aplikacije.

## <a name="install--configure-msal"></a>Instaliranje i konfiguriranje MSAL
Sad kad ste aplikacije registrirani s Microsoftom, možete instalirati MSAL i svoj identitet vezanih kod.  MSAL da biste mogli komunicirati krajnju točku 2.0, morate pružiti neke informacije o registraciji vaše aplikacije.

-   Započnite dodavanjem MSAL projekt TodoListClient korištenju konzole za Upravitelj paketa.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   U programu project TodoListClient otvorite `app.config`.  Zamjena vrijednosti elemenata u na `<appSettings>` sekciju u skladu s vizualnim vrijednosti unos u Registracija portal za aplikacije.  Kod pozivati te vrijednosti svaki put kada se koristi MSAL.
    -   U `ida:ClientId` je **Id aplikacije** aplikaciju koju ste kopirali iz portalu.

- U programu project TodoList servisa otvorite `web.config` u korijenskoj mapi projekta.  
    - Zamjena na `ida:Audience` vrijednost isti **Id aplikacije** na portalu.

## <a name="use-msal-to-get-tokens"></a>Koristite MSAL da biste dobili tokena
Osnovno načelo iza MSAL je da svaki put kada aplikacije potreban token za pristup, jednostavno zovete `app.AcquireToken(...)`, a MSAL ne ostale.  

-   U na `TodoListClient` projekt, otvara `MainWindow.xaml.cs` i pronađite u `OnInitialized(...)` način.  Prvi je korak za pokretanje aplikacije programa `PublicClientApplication` -MSAL-primarni predmete koji predstavlja izvornim aplikacijama.  To je mjesto proslijedite MSAL koordinate potrebne za komunikaciju s Azure AD i recite im kako predmemoriju tokena.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Prilikom pokretanja aplikacije, želimo da biste provjerili i potražite u članku ako se korisnik već prijavili na aplikaciju.  Međutim, ne možemo ne želite još samo pozvati Sučelja za prijavu – ćemo napraviti korisnika kliknite "Prijava" da biste to učinili.  Također u na `OnInitialized(...)` metoda:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Ako niste prijavljeni korisnik i oni kliknite gumb "Prijava", želimo pozivanje prijava korisničkog Sučelja i je korisnik unositi vjerodajnice.  Implementacija rukovatelj gumb za prijavu:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

- Ako korisnik uspješno znakove u MSAL će primiti i predmemoriju token umjesto vas te možete nastaviti da biste uputili poziv na `GetTodoList()` način pouzdano.  Sve što je preostalo da biste dobili zadaci korisničke je implementirati u `GetTodoList()` način.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Pokreni

Čestitamo! Sada imate aplikaciju za rad .NET WPF koja sadrži mogućnost provjeru autentičnosti korisnika i sigurno poziva Web API-ji pomoću OAuth 2.0.  Pokrenite oba projekte i prijavite se pomoću osobnog Microsoftova računa ili računa tvrtke ili obrazovne ustanove.  Dodavanje zadataka na popis zadataka za tog korisnika.  Odjava i ponovno prijavite i mjehuričaste grafikone da biste vidjeli svoj popis zadataka.  Zatvorite aplikaciju i ponovno ga pokrenuti.  Obratite pozornost na to kako ostaje netaknuta sesija – to je zato što aplikaciju predmemorira tokeni u lokalnu datoteku.

MSAL olakšava ugraditi uobičajene značajke identiteta u aplikaciju programa, korištenje osobnih i rade računa.  Ga brine sve poslovne dirty umjesto vas - upravljanje predmemorije, podrška OAuth protokola izlaganje korisnika s Prijava korisničko Sučelje, osvježavanje istekle tokeni i drugo.  Sve što trebate znati je jedan poziv API `app.AcquireTokenAsync(...)`.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) [prikazuje se kao .zip ovdje](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)ili pak mogu Kloniraj ga iz GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Daljnji koraci

Sada se može pomaknuti na dodatne teme.  Preporučujemo vam da biste isprobali:

- [Zaštita API Web TodoListService s krajnju točku 2.0](active-directory-v2-devquickstarts-dotnet-api.md)

Za dodatne resurse, pogledajte:  

- [Vodič za razvojne inženjere 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Oznake "msal" StackOverflow >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Sigurnosna ažuriranja za naši proizvodi

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću potražite na [toj stranici](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
