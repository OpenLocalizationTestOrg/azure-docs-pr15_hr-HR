<properties 
    pageTitle="Upravljanje API koncepti" 
    description="Saznajte više o API-ji, proizvodima, uloge, grupe i koncepata druge upravljanje API-JA." 
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

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Upute za uvoz definicije API-JA s operacije u upravljanju API Azure

U odjeljku Upravljanje API-JA, moguće je stvoriti novu API-ji i operacije dodati ručno ili na API moguće je uvesti te operacije u jednom koraku.

API-ji i njihove operacije može uvesti pomoću sljedećih oblika.

-   WADL
-   Swagger

Ovaj vodič prikazuje kako stvoriti novi API-JA i uvoz operacije u jednom koraku. Informacije o ručno stvaranje API i dodavanje operacije, potražite u članku [Stvaranje API-ji][] i [Dodavanje operacije da biste API][].

## <a name="import-api"> </a>Uvoz API

API-ji stvara i konfiguriran na portalu za publisher. Da biste pristupili portal za publisher, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

![Portal za Publisher][api-management-management-console]

Kliknite **API-ji** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim kliknite **Uvezi API**.

![Uvoz API-JA][api-management-import-apis]

Prozor **Uvoz API** ima tri kartice koje odgovaraju na tri načina za davanje specifikacija API-JA.

-   **Iz međuspremnika** omogućuje vam da biste zalijepili specifikacija API-JA u određenoj tekstni okvir.
-   **Iz datoteke** omogućuje pronađite i odaberite datoteku koja sadrži specifikacija API-JA.
-   **URL iz** omogućuje Navedite URL u navedenom u API-JA.

![Oblik za uvoz API-JA][api-management-import-api-clipboard]

Nakon unošenja specifikacija API-JA, koristite izborne gumbe na desnoj strani da biste naznačili specifikacija oblika. Podržani su sljedeći oblici.

-   WADL
-   Swagger

Nakon toga unesite **sufiks web-URL API -JA**. Time se dodaje osnovni URL servisa za upravljanje API-JA. Baza URL uobičajeno je sve API-ji hostirane na svaku instancu programa servisa za upravljanje API-JA. Upravljanje API razlikuje API-ji prema svojim sufiks i stoga sufiks mora biti jedinstvena za svaki API-JA u konkretnu instancu API upravljanje servisa.

Kada se unose sve vrijednosti, kliknite **Spremi** da biste stvorili na API-JA i povezane operacije. 

>[AZURE.NOTE] Praktični vodič uvoza osnovni Kalkulator API-JA u obliku Swagger, potražite u članku [Upravljanje prvi API-JA u upravljanju Azure API -JA](api-management-get-started.md).

## <a name="export-api"></a> Izvoz API

Osim novog API-ji za uvoz, možete izvesti definicije vaše API-ji s portala za publisher. Da biste to učinili, kliknite **Izvoz API** iz **Kartica Sažetak** **API-JA**.

![Izvoz API-JA][api-management-export-api]

API-ji moguće je izvesti pomoću WADL ili Swagger. Odaberite željeni oblik, kliknite **Spremi**pa odaberite mjesto na koje želite spremiti datoteku.

![API oblik za izvoz][api-management-export-api-format]

## <a name="next-steps"> </a>Sljedeće korake

Nakon stvaranja API i operacije uvoza, možete pregledavati i konfigurirati dodatne postavke, dodajte na API-JA proizvoda i objavite je da bi bio dostupan za razvojne inženjere. Dodatne informacije potražite u članku sljedeće vodilice.

-   [Konfiguriranje postavki API-JA][]
-   [Upute za stvaranje i objavljivanje proizvoda][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance

[Kako dodati operacije API]: api-management-howto-add-operations.md
[Upute za stvaranje i objavljivanje proizvoda]: api-management-howto-add-products.md
[Kako stvoriti API-ji]: api-management-howto-create-apis.md
[Konfiguriranje postavki API-JA]: api-management-howto-create-apis.md#configure-api-settings
