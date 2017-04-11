<properties 
    pageTitle="Kako dodati operacije API-JA u upravljanju API Azure | Microsoft Azure" 
    description="Saznajte kako dodati operacije API-JA u upravljanju Azure API-JA." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Kako dodati operacije API-JA u upravljanju API Azure

Prije korištenja API-JA u odjeljku Upravljanje API operacije mora biti dodan. Ovaj vodič prikazuje kako dodati i konfigurirati različite vrste operacije da biste API u odjeljku Upravljanje API-JA.

## <a name="add-operation"> </a>Dodajte operaciju

Operacije dodaju i konfiguriran za API na portalu za publisher. Da biste pristupili portala za publisher, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA.

![Portal za Publisher][api-management-management-console]

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

Odaberite željeni API na portalu za publisher, a zatim odaberite karticu **operacije** . 

![Operacije][api-management-operations]

Kliknite **Dodavanje postupak** da biste dodali novi postupak. Prikazat će se **Nova operacija** i po zadanom će biti odabrana kartica **potpis** .

![Postupak dodavanja][api-management-add-operation]

Navedite **HTTP glagolski** tako da odaberete na padajućem popisu.

![HTTP metoda][api-management-http-method]

<a name="url-template"></a>

Definiranje URL predloška upisivanjem u URL djelić koji se sastoji od jednog ili više segmenata put URL-a i nula ili više parametara niza upita. URL predloška, dodan osnovni URL API-JA, označava jedne HTTP operacije. Možda sadrži jednu ili više naziva varijable dijelove koji su otkrije vitičaste zagrade. Ove varijabilnih dijelova nazivaju parametri predložaka i dinamički dodijeljeni vrijednosti dobivenih iz URL-a na zahtjev zahtjev obrađuje platformu upravljanje API-JA.

![URL predloška][api-management-url-template]

<a name="rewrite-url-template"></a>

Po želji možete navesti **dopune URL predloška**. Omogućuje pomoću standardnih URL predloška za obradu zahtjevi za razgovore na pristupnom, pri pozivanju pozadinske putem pretvorene URL-a prema predlošku novog teksta. Parametri predložaka iz predloška URL će se koristiti u predlošku novog teksta. Sljedeći primjer pokazuje kako sadržaj kodira kao put segmenta u web-servisa iz prethodnog primjera se može pružati kao parametra upita u API objavljene putem upravljanja API platformu pomoću predložaka URL-a.

![Dopuna URL predloška][api-management-url-template-rewrite]

Pozivatelji operacije će koristiti oblik `/customers?customerid=ALFKI` i to će se mapirati `/Customers('ALFKI')` kada je servis pozadinske poziva.


**Zaslonsko** ime i **Opis** unesite opis postupka i koriste dokumentaciju kojom se programerima pomoću ovog API-JA na portalu za razvojne inženjere.

![Opis][api-management-description]

Opis postupka možete navesti kao običnog teksta ili HTML u tekstnom okviru **Opis** .

## <a name="operation-caching"> </a>Operacija predmemoriranja

Predmemoriranje odgovor smanjuje Latencija izgledati API korisnici, Spušta utrošak propusnosti i smanjuje opterećenje na webu HTTP servisa implementacije u API-JA. 

Jednostavno i brzo Omogućivanje predmemoriranja za operaciju, odaberite karticu **Međuspremanje** , a zatim potvrdite okvir **Omogući** .

![Predmemoriranje][api-management-caching-tab]

**Trajanje** određuje vremensko razdoblje tijekom kojeg odgovora operacija ostaje u predmemoriji. Zadana je vrijednost 3600 sekundi ili 1 sat.

Predmemoriju tipke koriste se za razlikovanje odgovora tako da se odgovor na svaki drugi predmemorije ključ koji odgovaraju će dobiti vlastitu zasebnom predmemorirani vrijednost. Ako želite, unesite parametrima niza određene upita i/ili HTTP zaglavlja koja će se koristiti u odnosno računalstvu predmemorije ključa vrijednosti u tekstne okvire **Razlikovanje po parametrima niza upita** i **Razlikovanje po zaglavlja** . Kada nijedna zahtjev za navedeni, cijeli URL i sljedeće vrijednosti HTTP zaglavlja se koriste u generiranje ključeva predmemorije: **Prihvati** i **Prihvati-skup znakova**.

>Dodatne informacije o predmemoriranje i predmemoriranja pravila potražite u članku [kako se rezultati operacija predmemorije u upravljanju API Azure][].


## <a name="request-parameters"> </a>Zahtjev parametara

Operacija parametara upravlja se na karticu parametri. Predložak obrasca u **URL predloška** na kartici **potpis** automatski se dodaju i može se promijeniti samo tako da uredite URL predloška. Dodatni Parametri se može unijeti ručno.

Da biste dodali novi parametra upita, kliknite **Dodaj parametra upita** , a zatim unesite sljedeće podatke:

-   **Ime** - naziv parametra.
-   **Opis** - kratak opis parametra (neobavezno).
-   **Vrsta** – vrsta parametra odaberete na padajućem izborniku prema dolje.
-   **Vrijednosti** - vrijednosti koje se mogu dodijeliti taj parametar. Jedna od vrijednosti može biti označen kao zadano (neobavezno).
-   **Obavezno** - parametar učinili obaveznim tako da poništite potvrdni okvir. 

![Zahtjev za parametre][api-management-request-parameters]

## <a name="request-body"> </a>Zahtjev tijelo

Ako je postupak omogućuje (npr. STAVI, objavu) i zahtijeva tijelo pružati primjera u sve podržane predstavljanje oblike (primjerice json, XML). 

>Zahtjev za tijelo koristi se samo u svrhu dokumentacija, a ne provjerava.

Da biste unijeli u tijelu zahtjeva, prijeđite na karticu **tijelo** .

Kliknite **Dodavanje predstavljanje**, počnite upisivati ime željenu vrstu sadržaja (npr. aplikacije/json), odaberite je na padajućem izborniku, i zalijepite tijelo primjer zahtjev za željenu u odabranom obliku u tekstni okvir. 

![Zahtjev za tijelo][api-management-request-body]

U dodatne za prikaze, možete i navesti opis programa neobavezni tekst u tekstnom okviru **Opis** .

## <a name="responses"> </a>Odgovore

Nije dobro sadrže primjere odgovore za sve šifre status operacije može dati. Svaka Šifra stanja može imati više od jednog odgovora tijelo primjeru, jedan za svaki od podržanih vrsta sadržaja. 

Da biste dodali odgovor, kliknite **Dodaj** , a zatim počnite pisati kod željeni status. U ovom primjeru je Šifra stanja **200 u redu**. Kada kod prikazana u padajućem popisu, odaberite ga, a kod odgovor je stvoriti i dodati postupak.

![Šifra odaziva][api-management-response-code]

Kliknite **Dodavanje predstavljanje**, počnite upisivati ime željenu vrstu sadržaja (npr. aplikacije/json), a zatim je odaberite na padajućem izborniku prema dolje.

![Vrste sadržaja u tijelu][api-management-response-body-content-type]

Primjer tijelo odgovor u odabranom obliku zalijepite u tekstni okvir. 

![Tijelo odgovora][api-management-response-body]

Po želji, po želji dodajte opis u tekstnom okviru **Opis** .

Kada je konfiguriran operacija, kliknite **Spremi**.


## <a name="next-steps"> </a>Sljedeće korake

Kada se operacije dodaju API, sljedeći je korak u API pridružiti proizvoda i objaviti tako da je razvojni inženjeri da biste uputili poziv operacije.

-   [Upute za stvaranje i objavljivanje proizvoda][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Upute za stvaranje i objavljivanje proizvoda]: api-management-howto-add-products.md
[Kako se rezultati operacija predmemorije u upravljanju API Azure]: api-management-howto-cache.md