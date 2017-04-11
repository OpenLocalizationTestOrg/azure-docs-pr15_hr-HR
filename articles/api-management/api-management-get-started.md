<properties
    pageTitle="Upravljanje prvi API-JA u upravljanju API Azure | Microsoft Azure"
    description="Saznajte kako stvoriti API-ji, dodajte operacije i početak rada s upravljanjem API-JA."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Upravljanje prvi API-JA u upravljanju API Azure

## <a name="overview"> </a>Pregled

Ovaj vodič pokazuje kako brzo početi s radom u značajkom upravljanja API Azure i provjerite prvi API poziva.

## <a name="concepts"> </a>Što je upravljanje API Azure?

Upravljanje Azure API-JA možete koristiti da biste snimili sve pozadinskog sustava i pokretanje programa API full-fledged koji se temelje na njemu.

Uobičajeni scenariji obuhvaćaju sljedeće:

* **Zaštita mobilne infrastrukture** po gating programa access s tipkama API sprječava DOS napadima pomoću ograničavanje ili pomoću pravila dodatnom sigurnošću kao što su JWT tokena provjere valjanosti.
* **Omogućivanje Neovisni partnera ecosystems** koja nudi brzo partnera za uhodavanje putem portala za razvojne inženjere i stvara sustava façade API-JA za decouple iz internog implementacije koji nisu pruge za potrošnju partnera.
* **Pokretanje programa interne API** po koja nudi središnje mjesto za tvrtku ili ustanovu komunikaciju o dostupnosti i najnovije promjene u API-ji, gating pristupa na temelju računi tvrtke ili ustanove, sve na temelju sigurnog kanala između pristupnika API-JA i pozadinski.


Sustav sastoji se od sljedeće komponente:

* **Pristupnik za API** je krajnju točku koja:
  * Prihvaća API poziva te ih preusmjerava na pozadinski sustav.
  * Provjerava API tipke, JWT tokena, certifikata i ostalih vjerodajnica.
  * Primjenjuje kvota korištenje i ocjenjivanje ograničenja.
  * Pretvorbe API-JA u hodu bez kod izmjene.
  * Predmemorira pozadinskog odgovore gdje postavljanje.
  * Zapisnici poziva metapodataka u svrhu analize.

* **Portal za publisher** je administratora sučelje gdje postavite program za API-JA. Pomoću njega:
    * Definiranje ili uvoz sheme API-JA.
    * Paket API-ji u proizvodima.
    * Postavljanje pravila kao što su kvote ili transformacije na API-ji.
    * Uvid zatražite od analize.
    * Upravljanje korisnicima.

* **Portala za razvojne inženjere** služi kao glavni web prisutnosti za razvojne inženjere, gdje ih možete:
    * Dokumentacija za čitanje API-JA.
    * Isprobajte API putem interaktivne konzolu.
    * Stvaranje poslovnog subjekta i pretplata na početak tipke API-JA.
    * Pristup analize na vlastita korištenje.


## <a name="create-service-instance"> </a>Stvoriti instancu upravljanje API-JA

>[AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Ako nemate račun, možete stvoriti pomoću računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju][].

Prvi korak u radu s API upravljanje je da biste stvorili instanca servisa. Prijava na [Portal za klasični Azure][] i kliknite **Novo**, **Aplikacije servisa**, **Upravljanje API -JA**, a zatim **Stvori**.

![Upravljanje API nove instance][api-management-create-instance-menu]

**URL-a**, navedite naziv jedinstveni poddomenu za URL servisa.

Odaberite željeni **pretplate** i **regije** za vaše instanca servisa. Nakon toga odabira, kliknite gumb **Dalje** .

![Novi servisa za upravljanje API-JA][api-management-create-instance-step1]

Unesite **Contoso Ltd.** **Naziv tvrtke ili ustanove**, a zatim unesite adresu e-pošte u polju **Administrator e-pošte** .

>[AZURE.NOTE] Tu adresu e-pošte koristi se za obavijesti iz sustava upravljanja API-JA. Dodatne informacije potražite u članku [upute za konfiguriranje obavijesti i predlošci e-pošte u odjeljku Upravljanje Azure API -JA][].

![Novi servisa za upravljanje API-JA][api-management-create-instance-step2]

Upravljanje API instanci servisa dostupne u tri razine: za razvojne inženjere, standardna i Premium. Prema zadanim postavkama, nove instance servisa za upravljanje API stvaraju se u sloju za razvojne inženjere. Odaberite sloj standardni prikaz ili Premium, potvrdite okvir **Napredne postavke** i odaberite željeni sloju na sljedećem zaslonu.

>[AZURE.NOTE] Razina za razvojne inženjere je razvoj, testiranje i pilot-programa API, pri čemu visoke dostupnosti nije problem. U razine standardnu i Premium mogu mijenjati veličinu vaš broj rezervirane jedinica za rukovanje više promet. Standardna i Premium razine omogućuju servis za upravljanje API-JA s najviše obrada power i performanse. Pomoću ovog praktičnog vodiča mogu provoditi pomoću bilo kojeg sloju. Dodatne informacije o API upravljanje razine potražite u članku [Upravljanje API cijene][].

Kliknite potvrdni okvir da biste stvorili vaše instanca servisa.

![Novi servisa za upravljanje API-JA][api-management-instance-created]

Nakon stvaranja instanca servisa, sljedeći je korak da biste stvorili ili uvoz API.

## <a name="create-api"> </a>Uvoz API

API sastoji se od skupa operacije koje možete pozvati iz klijentske aplikacije. Operacije API su koji na postojeće web-servise.

Moguće je stvoriti API-ji (i operacije može dodati) ručno ili možete uvesti. U ovom ćete praktičnom vodiču smo će uvesti API za uzorak Kalkulator web-servisa omogućuje tvrtka Microsoft i hostirane na Azure.

>[AZURE.NOTE] Upute o stvaranju API i ručno dodavanje operacije potražite u članku [Stvaranje API-ji](api-management-howto-create-apis.md) i [Dodavanje operacije da biste API](api-management-howto-add-operations.md).

API-ji konfigurirane s portala za publisher u kojoj se pristupa putem klasične portala za Azure. Dosegne portal za publisher, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA.

![Portal za Publisher][api-management-management-console]

Da biste uvezli Kalkulator API-JA, kliknite **API-ji** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim **Uvoza API -JA**.

![Gumb Uvezi API-JA][api-management-import-api]

Poduzmite sljedeće korake da biste konfigurirali Kalkulator API-JA:

1. Kliknite **Iz URL-a**, unesite **http://calcapi.cloudapp.net/calcapi.json** u tekstni okvir **URL dokumenta specifikacija** pa kliknite izborni gumb **Swagger** .
2. U **web-URL API sufiks** tekstni okvir upišite **Izračunaj** .
3. Kliknite u okvir **proizvoda (nije obavezno)** i odaberite **Starter**.
4. Kliknite **Spremi** da biste uvezli u API-JA.

![Dodavanje novog API-JA][api-management-import-new-api]

>[AZURE.NOTE] **Upravljanje API** trenutno podržava 1.2 i 2.0 verziju dokumenta Swagger za uvoz. Provjerite je li, čak i ako [Swagger 2.0 specifikacija](http://swagger.io/specification) deklarira koji `host`, `basePath`, i `schemes` svojstava nije obavezno, Swagger 2.0 dokument **mora** sadržavati tih svojstava u suprotnom se ne uvoze. 

Nakon uvoza s API prikazat će se sažetak stranica za API Sučelja na portalu za publisher.

![Sažetak API-JA][api-management-imported-api-summary]

U odjeljku API ima nekoliko kartica. Na kartici **Sažetak** prikazuje osnovni metriku i informacije o na API-JA. Kartica [Postavke](api-management-howto-create-apis.md#configure-api-settings) koristi se za prikaz i uređivanje konfiguracije API. Na kartici [operacije](api-management-howto-add-operations.md) služi za upravljanje operacije u API-JA. Konfiguriranje provjere autentičnosti pristupnik za pozadinskog poslužitelja pomoću Osnovna provjera autentičnosti ili [Provjera autentičnosti međusobna potvrda](api-management-howto-mutual-certificates.md)te da biste konfigurirali [autorizacija korisnika pomoću OAuth 2.0](api-management-howto-oauth2.md)možete koristiti karticu **Sigurnost** .  Na kartici **problema** koristi se za prikaz problema dojavi razvojni inženjeri koji koriste vaš API-ji. Na kartici **Proizvodi** koristi se za konfiguraciju proizvodi koje sadrže taj API.

Prema zadanim postavkama, svaku instancu API upravljanje dolazi s dva uzorka proizvoda:

-   **Starter**
-   **Neograničeno**

U ovom ćete praktičnom vodiču osnovni API Kalkulator dodan proizvoda Starter kad je uvezena u API-JA.

Da bi se upućivanje poziva na API razvojni inženjeri morate najprije se morate pretplatiti proizvoda koji ih omogućuje pristup na njega. Razvojni inženjeri možete se pretplatiti na proizvode iz portala za razvojne inženjere ili administratori možete se pretplatiti inženjerima omogućuje proizvoda na portalu za publisher. Administrator su Budući da ste stvorili instancu upravljanje API-JA u prethodnim koracima u ovom praktičnom vodiču da ste već pretplaćeni na svaki proizvod prema zadanim postavkama.

## <a name="call-operation"> </a>Pozvati operaciju s portala za razvojne inženjere

Operacije naziva se izravno na portalu za razvojne inženjere, koji pruža jednostavan način za prikaz i testiranje operacije API. U ovom koraku vodiča podići operacija na osnovni Kalkulator API- **Dodavanje dvije cijele brojeve** . Kliknite **portala za razvojne inženjere** na izborniku u gornjem desnom kutu portal za publisher.

![Portala za razvojne inženjere][api-management-developer-portal-menu]

Kliknite **API-ji** iz gornji izbornik, a zatim **Osnovni Kalkulator** da biste vidjeli dostupne operacije.

![Portala za razvojne inženjere][api-management-developer-portal-calc-api]

Imajte na umu uzorka opise i parametrima koji su uvezeni zajedno s API-jem i operacije, koja omogućuje dokumentacija za razvojne inženjere koji će koristiti ovaj postupak. Ovi opisi mogu se dodati operacije je netko dodao ručno.

Da biste nazvali postupka **Dodaj dvije cijele brojeve** , kliknite **isprobajte**.

![Isprobajte][api-management-developer-portal-calc-api-console]

Možete unijeti neke vrijednosti za parametre ili zadržati na zadane postavke, a zatim **Pošalji**.

![HTTP Get][api-management-invoke-get]

Kada se postupak poziva, portala za razvojne inženjere prikazuje **status odgovora**, **odgovor zaglavlja**i bilo koji **sadržaj odgovor**.

![Odgovor][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Prikaz analize

Za prikaz analitičkih podataka za osnovni kalkulator, prebacite se natrag na portalu za publisher tako da odaberete **Upravljanje** s izbornika na vrhu desno od portala za razvojne inženjere.

![Upravljanje][api-management-manage-menu]

Zadani prikaz za portal za publisher je na **nadzornu ploču**, koji sadrži pregled instance upravljanje API-JA.

![Nadzorna ploča][api-management-dashboard]

Zadržite pokazivač miša iznad grafikona za **Osnovni Kalkulator** da biste vidjeli određene metriku za korištenje na API-JA za određenog vremenskog razdoblja.

>[AZURE.NOTE] Ako ne vidite sve retke na grafikonu, vratite se na portal za razvojne inženjere i neke pozive u na API, pričekajte nekoliko sekundi dok se, a zatim vratite se na nadzornoj ploči.

Kliknite **Prikaz detalja** da biste pogledali sažetak stranice za API-JA, uključujući veću verziju te prikazati metriku.

![Analytics][api-management-mouse-over]

![Sažetak][api-management-api-summary-metrics]

Detaljne metriku i izvješća, na izborniku **Upravljanje API -JA** na lijevoj strani kliknite **Analitika** .

![Pregled][api-management-analytics-overview]

U odjeljku **analize** sadrži sljedeće četiri kartice:

-   **Na prvi pogled** omogućuje cjelokupan korištenju i stanju sustava metrika, kao i gornje razvojnim inženjerima, Najbolji proizvodi, gornji API-ji i gornje operacije.
-   **Korištenje** sadrži detaljne susret API poziva i propusnost, uključujući zemljopisnim prikaz.
-   **Stanje** usredotoči na kodovi stanja predmemoriju postocima uspjeha, vrijeme odaziva i API-JA i usluge puta odgovor.
-   **Aktivnosti** nudi izvješća koja kroz razine naniže na određeni aktivnosti za razvojne inženjere, proizvoda, API te operacije.

## <a name="next-steps"> </a>Sljedeće korake

- Saznajte kako se [zaštiti API-JA s ograničenjima stopa](api-management-howto-product-with-rules.md).

[Azure besplatnu probnu verziju]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Upute za konfiguriranje obavijesti i predlošci e-pošte u odjeljku Upravljanje Azure API-JA]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Upravljanje API cijene]: http://azure.microsoft.com/pricing/details/api-management/

[Azure klasični Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
