<properties
    pageTitle="Azure AD Xamarin Uvod | Microsoft Azure"
    description="Sastavljanje Xamarin aplikacije koja integrira Azure AD za prijavu i poziva Azure AD zaštićen API-ji pomoću OAuth."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Integriranje Azure AD u Xamarin aplikacije

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin omogućuje pisanje mobilne aplikacije u C# koji se može izvoditi na iOS, Android i Windows (mobilnih uređaja i računala). Ako ste izgradnja aplikacije pomoću Xamarin, Azure AD olakšava jednostavne i jasan za provjeru autentičnosti korisnika s računa za Active Directory. Omogućuje i aplikacije da biste sigurno trošiti sve API zaštićen Azure AD, kao što su API-ji sa sustavom Office 365 ili Azure API na webu.

Xamarin aplikacije za koje moraju imati pristup zaštićeni resursa, Azure AD omogućuje provjeru autentičnosti biblioteke imenika Active Directory ili ADAL. ADAL-isključivom svrhom u životu je da biste olakšali aplikacije da biste dobili pristup tokena. Da bismo pokazali samo kako je lako, ovdje bismo ćete stvoriti "Direktorija Searcher" aplikaciju koja:

-   Izvodi u sustavima iOS, Android, radnu površinu sustava Windows, Windows Phone i iz Windows trgovine.
- Koristi jedan prijenosni klasu biblioteke (PCL) za provjeru autentičnosti korisnika i dobili tokeni za Azure AD grafikonu API-JA
-   Pretražuje direktorij za korisnike s određenom UPN.

Da biste sastavili dovršeno aplikacije raditi, morat ćete:

2. Postavljanje okruženja za razvoj Xamarin
2. Registracija aplikacije s Azure AD.
3. Instaliranje i konfiguriranje ADAL.
5. Da biste dobili tokena iz Azure AD pomoću ADAL.

Da biste počeli, [Preuzmite skeleton projekt](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) ili [preuzeti dovršene uzorka](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Svaka je rješenje za Visual Studio 2013. Trebat ćete klijent za Azure AD Dodavanje korisnika i Registracija aplikacije. Ako još nemate klijent, [Saznajte kako da biste ga nabavili](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. postavili okruženje za razvoj Xamarin*
Budući da ovog praktičnog vodiča obuhvaća projekata za iOS, Android i Windows, morat ćete Visual Studio i Xamarin zajedno. Da biste stvorili potrebne okruženja, slijedite potpune upute na [instalaciju i instalirajte za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) na MSDN-u. Te upute obuhvaćaju materijal na raspolaganju su vam dodatne informacije o Xamarin dok čekate za instalaciju da biste dovršili. 

Kada dovršite potrebno postavljanje, otvorite rješenje u Visual Studio za početak rada. Vidjet ćete šest projekata: pet ovisne projekata i jednoj biblioteci prijenosni predmete koje će biti zajedničke za sve platforme`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. registrirati Searcher aplikacije direktorija*
Da biste omogućili aplikacije da biste dobili tokena, ćete najprije morate registrirati u klijentu za Azure AD i dati dozvolu za pristup grafikonu Azure AD API-JA:

-   Prijavite se na [Portal za upravljanje Azure](https://manage.windowsazure.com)
-   U navigacijskom oknu lijevoj strani kliknite **Servisa Active Directory**
-   Odaberite klijenta u kojoj se registrirati aplikaciju.
-   Kliknite karticu **aplikacije** pa kliknite **Dodaj** u donjem ladica.
-   Slijedite upite i stvorite novu **Nativni klijentska aplikacija**.
    -   **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima
    -   Kombinacija sheme i niz Azure AD pomoću kojega će vratiti tokena odgovore je **Preusmjeravanje Uri** . Unesite vrijednost, npr. `http://DirectorySearcher`.
-   Nakon što ste dovršili registraciju, AAD će dodijeliti aplikacije klijent Jedinstveni identifikator. Potreban vam je tu vrijednost u sljedećim odjeljcima tako da ga kopirali na kartici **Konfiguriraj** .
- I na kartici **Konfiguriraj** pronađite odjeljak "Dozvole za ostale aplikacije". Aplikacija "Azure Active Directory" dodajte dozvolu **pristupa vaše imenik tvrtke ili ustanove** u odjeljku **Dodijeliti dozvole**. To će omogućiti aplikacije da biste upit grafikonu API-JA za korisnike.

## <a name="2-install--configure-adal"></a>*2. instalirati i konfigurirati ADAL*
Sad kad ste aplikacije u Azure AD, možete instalirati ADAL i svoj identitet vezanih kod. ADAL da biste mogli komunicirati s Azure AD, morate pružiti neke informacije o registraciji vaše aplikacije.
-   Započnite dodavanjem ADAL u svim projektima u korištenju konzole za Upravitelj paketa rješenja.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Trebali biste primijetiti dvjema referencama biblioteke dodaju za svaki projekt – PCL dio ADAL i ovisne dijela.

-   U programu project DirectorySearcherLib otvorite `DirectorySearcher.cs`. Promijenite vrijednosti u predmete člana u skladu s vizualnim vrijednosti unos u Azure Portal. Kod pozivati te vrijednosti svaki put kada se koristi ADAL.
    -   U `tenant` je domena Azure AD klijent, npr. contoso.onmicrosoft.com
    -   U `clientId` je clientId aplikaciju koju ste kopirali iz portalu.
    - U `returnUri` je redirectUri – primjerice unijeli na portalu `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL da biste pristupili slijedite tokena iz AAD*
*Almost* sve logike provjere autentičnosti za aplikaciju leži u `DirectorySearcher.SearchByAlias(...)`. Sve što je potrebno u projektima ovisne jest proslijediti kontekstne parametar u `DirectorySearcher` PCL.

- Najprije otvorite `DirectorySearcher.cs` i dodajte nove parametar u `SearchByAlias(...)` način. `IPlatformParameters`je Kontekstni parametar koji encapsulates ovisne objekte koje ADAL potrebne za izvođenje provjere autentičnosti.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Nakon toga pokrenuti na `AuthenticationContext` -ADAL je primarni klasu. Ovo je gdje proći ADAL koordinate potrebne za komunikaciju s Azure AD. Zatim poziva `AcquireTokenAsync(...)`, koji prihvaća na `IPlatformParameters` objekta te će pozivanje potrebne da biste se vratili token aplikaciju tijek provjere autentičnosti.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`najprije će pokušati da biste se vratili token za traženi resurs (na grafikonu API u ovom slučaju) bez postavljanja upita korisniku unesite svoje vjerodajnice (putem predmemorije ili osvježavanje stare tokene). Samo ako je potrebno, ona će prikazati korisniku znak Azure AD na stranici prije pri dohvaćanju tražene tokena.


- Zatim možete priložiti token za pristup grafikonu API zahtjeva u zaglavlju autorizacije:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

To je sve za na `DirectorySearcher` PCL i aplikacije korisnika identitet vezanih kod.  Sve što je ostaje da biste uputili poziv na `SearchByAlias(...)` način u prikazima svaki platforme i gdje je potrebno dodati kod za pravilno rukovanje životni ciklus korisničkog Sučelja.

####<a name="android"></a>Android:
- U `MainActivity.cs`, dodajte poziv `SearchByAlias(...)` gumba kliknite rukovatelj:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Morate promijeniti u `OnActivityResult` životnog ciklusa način proslijediti sve provjere autentičnosti preusmjerava natrag na odgovarajući način.  ADAL nudi metode Pomoćnik za to u Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Radnu površinu sustava Windows:
- U `MainWindow.xaml.cs`, jednostavno upućivanje poziva na `SearchByAlias(...)` prosljeđivanje u `WindowInteropHelper` na radnoj površini `PlatformParameters` objekta:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- U `DirSearchClient_iOSViewController.cs`, u iOS `PlatformParameters` objekt jednostavno vodi reference s kontrolerom prikaza:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Univerzalni sustava Windows:
- U sustavu Windows univerzalni otvorite `MainPage.xaml.cs` i implementacija u `Search` metodu koja se koristi Pomoćnik za metodu u zajedničkoj projekta za ažuriranje korisničkog Sučelja prema potrebi.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Čestitamo! Sada imate rad Xamarin aplikacije koja sadrži mogućnost provjeru autentičnosti korisnika i sigurno poziva Web API-ji pomoću OAuth 2.0 preko pet različitih platformama. Ako to već niste učinili, sada je vrijeme za popunjavanje vaš klijent s nekim korisnicima. Pokrenite aplikaciju DirectorySearcher i prijavite pomoću nekog od tih korisnika. Traženje drugih korisnika na temelju njihove UPN-a.

ADAL olakšava ugraditi uobičajene značajke identiteta u aplikaciju. Ga brine sve poslovne dirty umjesto vas - upravljanje predmemorije, podrška OAuth protokola izlaganje korisnika s Prijava korisničko Sučelje, osvježavanje istekle tokeni i drugo. Sve što trebate znati je jedan poziv API `authContext.AcquireToken*(…)`.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) navedeni su [u nastavku](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Sada pomicati po na scenarije dodatne identitet. Preporučujemo vam da biste isprobali:

[Sigurne .NET Web API-JA s Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
