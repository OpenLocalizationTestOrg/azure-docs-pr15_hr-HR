<properties 
    pageTitle="Kako stvoriti API-ji u upravljanju API Azure" 
    description="Saznajte kako stvoriti i konfigurirati API-ji u upravljanju Azure API-JA." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-apis-in-azure-api-management"></a>Kako stvoriti API-ji u upravljanju API Azure

API-JA u odjeljku Upravljanje API predstavlja skup operacije koje možete pozvati tako da klijentske aplikacije. Novi API-ji stvaraju se na portalu za publisher, a zatim željeni operacije dodat će se. Kada dodate operacije u API dodaje se proizvoda i mogu objavljivati. Nakon objavljivanja API možete se pretplatili i koriste razvojni inženjeri.

Ovaj vodič prikazuje prvog koraka u postupku: Stvaranje i konfiguriranje nove API-JA u odjeljku Upravljanje API-JA. Dodatne informacije o dodavanju operacije i proizvoda za objavljivanje, potražite u članku [Dodavanje operacije da biste API][] i [upute za stvaranje i objavljivanje proizvoda][].

## <a name="create-new-api"> </a>Stvaranje nove API-JA

API-ji stvara i konfiguriran na portalu za publisher. Da biste pristupili portala za publisher, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA.

![Portal za Publisher][api-management-management-console]

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

Kliknite **API-ji** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim **dodajte API-JA**.

![Stvaranje API-JA][api-management-create-api]

Pomoću prozora **Dodaj novi API-JA** možete konfigurirati nove API.

![Dodavanje novog API-JA][api-management-add-new-api]

Sljedeća polja koriste se za konfiguriranje nove API-JA.

-   **Naziv web API** nudi jedinstven i opisan naziv s API-JA. Prikazuje se u portala za razvojne inženjere i publisher.
-   **URL web-servisa** odnosi se na servis HTTP implementacije u API-JA. Upravljanje API prosljeđuje zahtjevi za tu adresu.
-   Osnovni URL za API servisa za upravljanje dodaje **sufiks web-URL API -JA** . Baza URL uobičajeno je sve API-ji hostira tvrtka instanca servisa upravljanje API-JA. Upravljanje API razlikuje API-ji prema svojim sufiks i stoga sufiks mora biti jedinstvena za svaki API-JA za dani publisher.
-   **Web-URL API shemu** određuje koje protokole koje će se koristiti za pristup s API-JA. HTTPs navedena je prema zadanim postavkama.
-   Da biste dodali po želji taj novi API proizvoda, kliknite **proizvoda (neobavezno)** padajućeg izbornika i odaberite proizvoda. Ovako možete mogu ponoviti više puta da biste dodali u API više proizvoda.

Kada niste konfigurirali željene vrijednosti, kliknite **Spremi**. Nakon stvaranja novog API-JA, prikazat će se sažetak stranica za na API-JA na portalu za publisher.

![Sažetak API-JA][api-management-api-summary]

## <a name="configure-api-settings"> </a>Postavki konfiguriranje API-JA

Kartica **Postavke** možete koristiti za provjeru i uređivanje konfiguracije API. **URL web-servisa**, **naziv web API**i **web-URL API sufiks** prethodno postavljene kada na API se stvara i može se mijenjati ovdje. **Opis** pruža neobavezan opis, a **web-URL API shemu** određuje koje protokole koje će se koristiti za pristup s API-JA.

![Postavke API-JA][api-management-api-settings]

Konfiguriranje provjere autentičnosti pristupnika implementacije u API servisa za pozadinski, odaberite karticu **Sigurnost** . Konfiguriranje provjere autentičnosti **HTTP osnovni** ili **klijentskih potvrda** može se koristiti **s vjerodajnicama** padajući popis. Da biste koristili HTTP osnovnu provjeru autentičnosti, unesite željene vjerodajnice. Informacije o korištenju provjera autentičnosti klijentskih potvrda, potražite [u][]članku sigurnost pozadinskih pomoću provjere autentičnosti certifikat klijenta u upravljanju Azure API -JA.

Karticu **Sigurnost** može se koristiti i da biste konfigurirali **autorizacija korisnika** pomoću OAuth 2.0. Dodatne informacije potražite u članku [Da biste autorizirali račune za razvojne inženjere pomoću OAuth 2.0 u upravljanju Azure API -JA][].

![Osnovna provjera autentičnosti postavke][api-management-api-settings-credentials]

Kliknite **Spremi** da biste spremili promjene postavki API-JA.

## <a name="next-steps"> </a>Sljedeće korake

Kada se stvara API i konfigurirali, sljedeći koraci su postavke da biste dodali operacije s API-JA, dodajte na API-JA proizvoda, a ga objavite tako da je dostupan za razvojne inženjere. Dodatne informacije potražite u sljedećim člancima.

-   [Kako dodati operacije API][]
-   [Upute za stvaranje i objavljivanje proizvoda][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Kako dodati operacije API]: api-management-howto-add-operations.md
[Upute za stvaranje i objavljivanje proizvoda]: api-management-howto-add-products.md

[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance
[Upute za sigurnost pozadinske klijenti provjera autentičnosti potvrda u upravljanju API Azure]: api-management-howto-mutual-certificates.md
[Kako da biste autorizirali račune za razvojne inženjere pomoću OAuth 2.0 u upravljanju API Azure]: api-management-howto-oauth2.md