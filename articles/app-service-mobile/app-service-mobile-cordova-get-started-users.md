<properties
    pageTitle="Dodavanje provjere autentičnosti na Apache Cordova s aplikacijama Mobile | Aplikacije servisa za Azure"
    description="Saznajte kako pomoću mobilne aplikacije u Azure aplikacije servisa za provjeru autentičnosti korisnika aplikacije Apache Cordova kroz mnoštvo davatelji identiteta, uključujući Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Dodavanje provjere autentičnosti Apache Cordova aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Sažetak

U ovom ćete praktičnom vodiču dodajte provjere autentičnosti todolist makronaredbi brzi početak rada na Cordova Apache pomoću davatelja identiteta podržane. Pomoću ovog praktičnog vodiča temelji se na vodič [Početak rada s aplikacijama Mobile] , koja se najprije morate dovršiti.

##<a name="register"></a>Registracija aplikacije za provjeru autentičnosti i konfiguriranje servisa za aplikaciju

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Pogledajte videozapis koji prikazuje slične korake](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Ograničavanje dozvola korisnicima čija je autentičnost provjerena

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Sada možete provjeriti anonimni pristup vašem pozadinskog je onemogućen. U Visual Studio otvorite projekt koji ste stvorili kada završi vodič [Početak rada s aplikacijama Mobile], zatim pokretanje aplikacije u **Google Android Emulator** i provjerite je li da se pojavila se neočekivana pogreška veze prikazuju nakon pokretanja aplikacije.

Nakon toga će se ažurirati aplikaciju za provjeru autentičnosti korisnika prije traži resursa iz pozadine za mobilnu aplikaciju.

##<a name="add-authentication"></a>Dodavanje provjere autentičnosti za aplikaciju

1. Otvaranje projekta u **Visual Studio**, a zatim otvorite na `www/index.html` datoteke za uređivanje.

2. Pronađite na `Content-Security-Policy` metaoznaku u odjeljku glavni.  Morat ćete dodati voditelju OAuth popis dopuštenih izvora.

  	| Davatelj usluga               | Naziv davatelja SDK | Glavno računalo OAuth                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | aad               | https://login.Windows.NET   |
  	| Facebook               | facebook          | https://www.facebook.com    |
  	| Google                 | Google            | https://Accounts.Google.com |
  	| Microsoft              | microsoftaccount  | https://login.Live.com      |
  	| Twitter                | twitter           | https://API.twitter.com     |

    Primjer sadržaja--pravila sigurnosti (implementirana za Azure Active Directory) je na sljedeći način:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Trebali biste zamijeniti `https://login.windows.net` s glavnim računalom OAuth iz gornje tablice.  Potražite u [dokumentaciji sadržaj sigurnosna pravila] za dodatne informacije o ovom metaoznaku.

    Imajte na umu da neki davatelji provjere autentičnosti ne zahtijeva sadržaj sigurnosna pravila promjene kada se koristi na odgovarajuće mobilnim uređajima.  Ako, na primjer, bez promjena sadržaja sigurnosna pravila su potrebne prilikom upotrebe Google provjere autentičnosti na uređaju sa sustavom Android.

3. Otvaranje u `www/js/index.js` datoteke za uređivanje, pronađite u `onDeviceReady()` metoda i u odjeljku Stvaranje klijent kod dodajte sljedeće:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Imajte na umu da kod zamijeniti postojeći kod koji stvara referenci tablice i osvježava korisničkog Sučelja.

    Login() metoda provjere autentičnosti počinje davatelja usluga. Način login() je funkciji asinkrone koji vraća obećanje JavaScript.  Ostatak inicijalizaciju smješten unutar odgovor obećanje tako da se izvršava dok se ne dovrši login() način.

4. Kod koji ste upravo dodali, zamijeniti `SDK_Provider_Name` pod nazivom vaš davatelj usluga za prijavu. Na primjer, da biste postigli Azure Active Directory, koristite `client.login('aad')`.

4. Pokrenite projekt.  Po završetku projekta pokretanje, aplikacija prikazat će na stranicu za prijavu OAuth za u odabranom davatelja usluge provjere autentičnosti.

##<a name="next-steps"></a>Daljnji koraci

* Saznajte više [O autentičnosti] sa servisom Azure aplikacije.
* Nastavite vodič dodavanjem [Automatske obavijesti] Apache Cordova aplikacije.

Saznajte kako koristiti u SDK-ovi.

* [Apache Cordova SDK]
* [Poslužitelj ASP.NET SDK]
* [Poslužitelj Node.js SDK]

<!-- URLs. -->
[Početak rada s aplikacijama Mobile]: app-service-mobile-cordova-get-started.md
[Dokumentacija sadržaj sigurnosna pravila]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Automatske obavijesti]: app-service-mobile-cordova-get-started-push.md
[O provjeri autentičnosti]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[Poslužitelj ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Poslužitelj Node.js SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
