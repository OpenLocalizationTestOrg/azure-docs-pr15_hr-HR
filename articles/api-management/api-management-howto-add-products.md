<properties 
    pageTitle="Upute za stvaranje i objavljivanje proizvoda u upravljanju API Azure" 
    description="Upute za stvaranje i objavljivanje proizvoda u upravljanju Azure API-JA." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Upute za stvaranje i objavljivanje proizvoda u upravljanju API Azure

U Azure upravljanja API-JA, proizvod sadrži jednu ili više API-ji kao i kvota korištenja i uvjete korištenja. Kada je objavljen proizvoda, razvojni inženjeri možete pretplatu na proizvod i početi koristiti API-ji proizvoda. U temi sadrži vodič kroz stvaranje proizvoda, dodavanje API i objavljivanje za razvojne inženjere.

## <a name="create-product"> </a>Stvaranje proizvoda

Operacije dodaju i konfiguriran za API na portalu za publisher. Da biste pristupili portal za publisher, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA.

![Portal za Publisher][api-management-management-console]

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

Na izborniku na lijevoj strani da biste prikazali stranicu **proizvoda** kliknite **proizvoda** pa kliknite **Dodaj proizvod**.

![Proizvodi][api-management-products]

![Novi proizvod][api-management-add-new-product]

U polje **naziv** i opis proizvoda u polje **Opis** unesite opisni naziv proizvoda.

Proizvodi u odjeljku Upravljanje API može biti **otvorena** ili **zaštićenog**. Zaštićeni proizvoda morate imati pretplatu na prije korištenja, prilikom otvaranja proizvoda mogu se bez pretplatu. Potvrdite okvir da biste stvorili zaštićeni proizvod koji zahtijeva pretplatu **potrebna pretplata** . Ovo je zadana postavka.

Ako želite da se administrator za pregled i prihvaćanje ili odbacivanje pokušaja pretplata na ovaj proizvod, provjerite **potrebno odobrenje pretplate** . Ako okvir nije potvrđen, prilikom pokušaja pretplate bit će automatski odobrene. Dodatne informacije o pretplatama potražite u članku [Prikaz pretplatnike proizvoda][].

Da biste omogućili račune za razvojne inženjere da biste se pretplatili više puta proizvoda, odaberite potvrdni okvir **Dopusti višestruke pretplate** . Ako taj okvir nije potvrđen, svaki račun za razvojne inženjere možete se pretplatiti samo jednom vremenskom uz proizvod.

![Neograničeno višestruke pretplate][api-management-unlimited-multiple-subscriptions]

Da biste ograničili broj višestruke pretplate istodobno, potvrdite okvir **ograničenje broja istodobno pretplate na** , a zatim unesite ograničenje pretplate. U sljedećem primjeru istodobno pretplate ograničeni su na četiri po računu za razvojne inženjere.

![Četiri višestruke pretplate][api-management-four-multiple-subscriptions]

Kada niste konfigurirali nove mogućnosti proizvoda, kliknite **Spremi** da biste stvorili novi proizvod.

![Proizvodi][api-management-products-page]

>Prema zadanim postavkama novih proizvoda su objavu, a su vidljivi samo grupe **administratora** .

Da biste konfigurirali proizvod, kliknite naziv proizvoda na kartici **Proizvodi** .

## <a name="add-apis"> </a>Dodavanje API-ji proizvoda

**Proizvodi** stranica sadrži četiri veze za konfiguraciju: **Sažetak**, **Postavke**, **vidljivost**i **pretplatnika**. Na kartici **Sažetak** je gdje možete dodati API-ji i objavljivanje ili poništavanje objavljivanja proizvoda.

![Sažetak][api-management-new-product-summary]

Prije objavljivanja proizvoda morate dodati jedan ili više API-ji. Da biste to učinili, kliknite **Dodaj API -JA proizvoda**.

![Dodavanje API-ji][api-management-add-apis-to-product]

Odaberite željeni API-ji, a zatim kliknite **Spremi**.

## <a name="add-description"> </a>Dodajte opisne podatke proizvoda

Kartica **Postavke** omogućuje pružaju podrobne informacije o proizvodu kao što su njezinu svrhu, API-ji ga omogućuje pristup i druge korisne informacije. Sadržaj je namijenjeno pri razvojni inženjeri koji će se poziva na API-JA možete biti pisane običnim tekstom ili HTML oznake.

![Postavke proizvoda][api-management-product-settings]

Provjerite da biste stvorili zaštićeni proizvod koji zahtijeva pretplatu koja će se koristiti **potrebna pretplata** ili poništite potvrdni okvir da biste stvorili Otvori proizvoda koje se pozivati bez pretplatu.

Ako želite ručno odobravanje sve zahtjeve za pretplatu proizvoda, odaberite **potrebno odobrenje pretplate** . Po zadanom sve pretplate proizvoda automatski se dodjeljuju.

Da biste omogućili račune za razvojne inženjere da biste se pretplatili više puta uz proizvod, potvrdite okvir **Dopusti višestruke pretplate** , a zatim po želji navedite ograničenje. Ako taj okvir nije potvrđen, svaki račun za razvojne inženjere možete se pretplatiti samo jednom vremenskom uz proizvod.

Po želji ispunite polja **Uvjeti korištenja** koji opisuje uvjete korištenja za proizvod koji pretplatnike morate prihvatiti da biste mogli koristiti proizvod.

## <a name="publish-product"> </a>Objavljivanje proizvoda

Prije nego što možete pozvati API-ji uz proizvod, proizvod moraju se objaviti. Na kartici **Sažetak** proizvoda kliknite **Objavi**, a zatim kliknite da biste potvrdili **Da, objavite ga** . Da biste prethodno objavljeni proizvoda učinili privatnima, kliknite **Poništi objavu**.

![Objavljivanje proizvoda][api-management-publish-product]

## <a name="make-visible"> </a>Proizvoda neka bude vidljivo razvojni inženjeri

Kartica **vidljivost** omogućuje vam da odaberete koje uloge će moći vidjeli proizvoda portala za razvojne inženjere i pretplata na proizvod.

![Vidljivost proizvoda][api-management-product-visiblity]

Da biste omogućili ili onemogućili vidljivost proizvoda za razvojne inženjere u grupi, potvrdite ili poništite potvrdni okvir uz grupu, a zatim kliknite **Spremi**.

>Dodatne informacije potražite [u][]članku Stvaranje i korištenje grupa za upravljanje računima za razvojne inženjere u upravljanju Azure API -JA.

## <a name="view-subscribers"> </a>Prikaz pretplatnike proizvoda

Na kartici **pretplatnike** navedeni programerima koji ste pretplaćeni na proizvod. Detalji i postavke za svaki za razvojne inženjere moguće je prikazati tako da kliknete naziv za razvojne inženjere. U ovom primjeru bez razvojnim inženjerima još ste pretplaćeni na proizvod.

![Razvojni inženjeri][api-management-developer-list]

## <a name="next-steps"> </a>Sljedeće korake

Nakon dodat će se željeni API-ji i objave proizvoda razvojnim inženjerima možete pretplatu na proizvod i počnete da biste uputili poziv API-ji. Praktični vodič koji pokazuje te stavke kao i konfiguracija naprednih proizvoda potražite u članku [kako stvoriti i konfiguriranje postavki napredne proizvoda u upravljanju Azure API -JA][].

Dodatne informacije o radu s proizvodima potražite u ovom videozapisu.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Prikaz pretplatnike proizvoda]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Kako stvoriti i koristiti grupe za upravljanje računima za razvojne inženjere u upravljanju API Azure]: api-management-howto-create-groups.md
[Kako stvaranje i konfiguriranje postavki napredne proizvoda u upravljanju API Azure]: api-management-howto-product-with-rules.md 