<properties
    pageTitle="Azure Active Directory 2.0 aplikacija za Android | Microsoft Azure"
    description="Kako stvoriti aplikaciju za Android koja se prijavi korisnika završavaju na osobni Microsoftov račun i rad ili školske računi i pozive API grafikonu pomoću drugih proizvođača biblioteke."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Dodavanje prijava u aplikaciju programa sa sustavom Android pomoću drugih proizvođača biblioteke grafikonu API-JA pomoću krajnju točku 2.0

Identitet platformu Microsoft koristi Otvori standarde kao što su OAuth2 i OpenID povezivanje. Razvojni inženjeri možete koristiti bilo koje biblioteke žele integrirati s naših usluga. Da biste lakše razvojnim inženjerima naš platformu pomoću drugih biblioteka, ne možemo ste napisali nekoliko vodiči poput ove da bismo pokazali kako konfigurirati biblioteke drugih proizvođača za povezivanje s identiteta platformu Microsoft. Većina biblioteka koje implementirati [Specifikacija RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) možete povezati s platformu Microsoft identitet.

Uz aplikaciju sustava stvara ovaj vodič, korisnici mogu prijavite se u tvrtki ili ustanovi i zatim traženje same u tvrtki ili ustanovi pomoću API grafikonu.

Ako ste novi korisnik OAuth2 ili povezivanje OpenID, približno tu konfiguraciju uzorka možda neće imati smisla vama. Preporučujemo da pročitate [2.0 protokoli - OAuth 2.0 autorizacije kod tijek](active-directory-v2-protocols-oauth-code.md) za pozadinu.

> [AZURE.NOTE] Neke značajke naš platformu kojima je potreban izraz standardima OAuth2 ili povezivanje OpenID, primjerice uvjetno pristup i upravljanje pravilima Intune, potrebno koristiti naše Otvori izvor Microsoft Azure identiteta biblioteke.

Krajnja točka 2.0 ne podržava sve scenariji Azure Active Directory i značajke.

> [AZURE.NOTE] Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>Preuzmite kod iz GitHub
Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  Da biste pratili, možete [preuzeti aplikacije skeleton kao u .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) ili Kloniraj na skeleton:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Možete preuzeti samo uzorak i započeti odmah:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Registracija aplikacije
Stvaranje nove aplikacije na [portal za registraciju aplikacije](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite detaljne upute pri [kako registrirati aplikacije s krajnju točku 2.0](active-directory-v2-app-registration.md).  Obavezno:

- Kopirajte **Id aplikacije** koji vam je dodijeljen aplikacije jer će vam je potrebna uskoro.
- Dodajte platformu **mobilne** aplikacije.

> Napomena: Registracija portal za aplikacije nudi **Preusmjeravanje URI** vrijednost. Međutim, u ovom primjeru morate koristiti zadanu vrijednost od `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>Preuzimanje biblioteke proizvođača NXOAuth2 i stvaranje radnog prostora

Za ovaj vodič će koristiti OIDCAndroidLib iz GitHub, što je u biblioteku OAuth2 na temelju kod OpenID povezivanje Google. Implementira matičnoj aplikaciji profila i podržava krajnja točka za provjeru autentičnosti korisnika. To su svi stvari koje morate integrirati s identiteta platformu Microsoft.

Kloniraj repo OIDCAndroidLib s računalom.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Postavljanje okruženja za Android Studio

1. Stvaranje novog projekta Android Studio i prihvatite zadane postavke u čarobnjaku.

    ![Stvaranje novog projekta u Android Studio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Uređaje sa sustavom Android cilj](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Dodavanje aktivnosti mobile](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Da biste postavili module projekta, premještanje kloniranu repo mjesto projekta. Možete i stvoriti projekta i Kloniraj ga izravno na mjesto projekta.

    ![Moduli projekta](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Otvorite postavke module za projekt pomoću kontekstnog izbornika ili pomoću prečaca Ctrl + Alt + lavne + S.

    ![Postavke moduli projekta](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Uklanjanje modul zadane aplikacije jer želite samo postavke spremnik projekta.

    ![Modul za zadane aplikacije](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Uvezite module iz kloniranu repo u trenutni projekt.

    ![Uvezi gradle projekt](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![stvaranje nove stranice modul](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Ponovite te korake za na `oidlib-sample` modul.

7. Provjera ovisnosti oidclib na `oidlib-sample` modul.

    ![oidclib međuzavisnosti modul oidlib uzorka](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Kliknite **u redu** i pričekajte da gradle sinkronizaciju.

    Vaše settings.gradle trebao bi izgledati kao što su:

    ![Snimka zaslona settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Sastavljanje aplikaciju uzorka li uzorak ispravno funkcioniranje.

    Nećete moći još koristi se s Azure Active Directory. Ne možemo morat ćete najprije konfigurirati neke krajnje točke. To je da biste bili sigurni da nemate problema sa sustavom Android Studio prije ne možemo pokrenuti prilagodbu aplikaciju uzorka.

10. Stvaranje i pokretanje `oidlib-sample` kao cilj u Studio sa sustavom Android.

    ![Tijek Sastavi oidlib uzorka](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Brisanje na `app ` direktorija koji je ostavljeno kada modul ukloniti iz projekta jer sa sustavom Android Studio ne briše za sigurnost.

    ![Struktura datoteke koja sadrži aplikaciju direktorija](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Otvaranje izbornika **Uređivanje konfiguracije** da biste uklonili izvođenja konfiguracija koja također lijevo, kada modula uklonili projekta.

    ![Konfiguracija izbornik uređivanje](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![pokretanje konfiguraciji aplikacije](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>Konfiguriranje krajnje točke uzorka

Sad kad ste na `oidlib-sample` uspješno pokrenuti, uredimo neke krajnje točke da biste dobili ovo radu s Azure Active Directory.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Konfiguriranje klijenta uređivanjem oidc_clientconf.xml datoteka

1. Budući da koristite tokova OAuth2 samo na početak token i nazovite Graph API, postavite klijent samo učiniti OAuth2. OIDC prosljeđivala u noviji primjeru.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Konfiguriranje svoj ID klijenta koji ste dobili na portalu za registraciju.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Konfigurirajte preusmjeravanje URI onu ispod.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Konfiguriranje vaše dosega koje su vam potrebne da biste pristupili API grafikonu.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

Na `User.Read` vrijednosti u `oidc_scopes` omogućuje vam da biste pročitali osnovni profila na traje u korisnika.
Dodatne informacije o sve dostupne opsege u [opsega dozvola Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).

Želite li objašnjenja o `openid` ili `offline_access` kao opsege u OpenID povezivanje potražite u članku [2.0 protokoli - OAuth 2.0 autorizacije kod tijeka](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Konfiguriranje vaš klijent krajnje točke uređivanjem oidc_endpoints.xml datoteka

- Otvaranje u `oidc_endpoints.xml` datoteku i izvršite sljedeće promjene:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Te krajnje točke nikad ne promijenite ako koristite OAuth2 kao vaše protokol.

> [AZURE.NOTE]
Krajnje točke za `userInfoEndpoint` i `revocationEndpoint` trenutno ne podržava Azure Active Directory. Ako to sa zadanom vrijednošću example.com ostavite, koji će Podsjeti koje nisu dostupne u uzorku :-)


## <a name="configure-a-graph-api-call"></a>Konfiguriranje grafikonu API poziva

- Otvaranje u `HomeActivity.java` datoteku i izvršite sljedeće promjene:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Ovdje jednostavne grafikonu API poziva vraća naš informacije.

To su sve što trebate napraviti promjene. Pokretanje u `oidlib-sample` aplikacija, a kliknite **Prijava**.

Nakon što ste uspješno autentičnost, odaberite gumb **Zatražite zaštićeni resursa** da biste testirali poziva API grafikonu.

## <a name="get-security-updates-for-our-product"></a>Sigurnosna ažuriranja za naše proizvoda

Pozivamo vas da primanje obavijesti o sigurnošću posjeta [TechCenter za sigurnost](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
