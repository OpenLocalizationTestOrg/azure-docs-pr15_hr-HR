<properties 
    pageTitle="Uvod API aplikacije | Microsoft Azure" 
    description="Saznajte kako aplikacije servisa za Azure pomaže vam pri razvoju glavno računalo i zauzeti RESTful API-ji." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>Pregled aplikacija API-JA

API aplikacije u aplikacije servisa za Azure nude značajke koje olakšavaju razviti glavno računalo i zauzeti API-ji u oblak i lokalne. S aplikacijama API preuzeti enterprise ocjene sigurnost, kontrola jednostavnog pristupa, hibridnog povezivanje, automatsko generiranje SDK i objedinjenog Integracija s [Logike aplikacije](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md) je potpuno upravljanih platformu za web-mjesto mobilnim, i scenarija integracije. API aplikacije jedan je od četiri vrste aplikacije koje nudi [Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md).

![Vrste aplikaciju u aplikacije servisa za Azure](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Zašto koristiti aplikacije API-JA?

Slijede ključne značajke API aplikacija:

- **Prijenos postojeće API-JA kao-je** – ne morate promijeniti neke od kod u postojeće API-ji za iskoristite prednost API aplikacije--samo kod implementirati aplikaciju API-JA. Vaš API-JA možete koristiti bilo koji jezik ili framework podržava aplikacije servisa, uključujući ASP.NET i C#, Java, PHP, Node.js i Python.

- **Jednostavno potrošnje** - integrirani podrška za [metapodatke Swagger API](http://swagger.io/) čini vaš API-ji jednostavno potrošni niz klijenata.  Automatsko generiranje koda za klijenta za API-ji u različitim jezicima uključujući C#, Java i Javascript. Jednostavno konfigurirati [CORS](app-service-api-cors-consume-javascript.md) bez promjene kod. Dodatne informacije potražite u članku [aplikacija API servisa metapodataka za otkrivanje i kod generacije API-JA](app-service-api-metadata.md) i [utrošak API aplikacije iz JavaScript pomoću CORS](app-service-api-cors-consume-javascript.md). 

- **Kontrola jednostavnog pristupa** – zaštita API aplikacije iz Neprovjereni Accessa bez ikakvih promjena kod. Servise za ugrađenu provjeru autentičnosti sigurne API-ji za pristup tako da druge servise ili klijentima koji predstavlja korisnika. Davatelji identiteta podržani obuhvaćaju Azure Active Directory, Facebook, Twitter, Google i Microsoftov Account. Klijenti mogu koristiti Active Directory provjera autentičnosti biblioteke (ADAL) ili SDK za mobilne aplikacije. Dodatne informacije potražite u članku [provjere autentičnosti i autorizacije API aplikacije u aplikacije servisa za Azure](app-service-api-authentication.md).

- **Integracija s programom visual Studio** - namjenski Alati u Visual Studio pojednostavniti rad stvaranja implementacija, troše, ispravljanje pogrešaka i upravljanje aplikacijama API-JA. Dodatne informacije potražite u članku [Objavljivanje SDK Azure 2.8.1 za .NET](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Integracija s aplikacijama logike** - API aplikacije koje ste stvorili možete troše [Logike aplikacija servisa](../app-service-logic/app-service-logic-what-are-logic-apps.md).  Dodatne informacije potražite u članku [Korištenje prilagođenog API-JA hostirane na aplikacije servisa s aplikacijama logiku](../app-service-logic/app-service-logic-custom-hosted-api.md) i [nove sheme verzija 2015-08-01 – pretpregled](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Aplikaciju API može potrajati i značajke koje nudi [Web-aplikacije](../app-service-web/app-service-web-overview.md) i [Mobilne aplikacije](../app-service-mobile/app-service-mobile-value-prop.md). Obrnuto vrijedi i: Ako koristite web-aplikacijama ili mobilne aplikacije za hostiranje API, može potrajati API aplikacije značajke kao što su Swagger metapodataka za klijentski generiranje koda i CORS za pristup putem preglednika domenama. Jedina razlika između vrste tri aplikacije (API-JA, web-mjesto mobilne) je naziv i ikonu koji se koriste za ih na portalu za Azure.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Koja je razlika između aplikacija za API-JA i upravljanje API Azure?

Aplikacija za API-JA i [Upravljanje API Azure](../api-management/api-management-key-concepts.md) su komplementarnu servisi:

* Upravljanje API je o upravljanju API-ji. Stavljate sučelje za upravljanje API-JA na API monitoru i ograničenja korištenja, rukovati ulazni i izlazni, nekoliko API-ji konsolidirati u jednu krajnjoj točki i tako dalje. API-ji upravlja mogu nalaziti bilo kojeg mjesta.
* API aplikacija je o hosting API-ji. Servis sadrži značajke koje olakšavaju razvijanje i dosta API, ali ga nema ugrađenu vrste nadzor, ograničavanje, rukovanje ili konsolidacije te upravljanje API-JA ne. Ako ne trebate značajke upravljanja API-JA, možete hostirati API-ji u aplikacijama za API bez korištenja upravljanja API-JA.

Ovo je dijagram koji ilustrira upravljanje API-JA za API-ji hostirane u aplikacijama za API-JA i drugdje.

![Upravljanje Azure API-JA i aplikacije API-JA](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Neke značajke upravljanja API-JA i aplikacije API imaju slične funkcije.  Na primjer, oba možete automatizirati CORS podršku. Kada zajednički koristite dva servisa, koristit ćete upravljanje API-JA za CORS Budući da funkcionira kao sučelje za API aplikacija. 

## <a name="getting-started"></a>Početak rada

Početak rada s aplikacijama API uvođenjem uzorak koda u jednu potražite u članku vodič za bez obzira framework radije:

* [PLATFORME ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Postavljanje pitanja o aplikacijama API-JA, pokrenite niti u na [forumu API aplikacije](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 
