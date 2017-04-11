<properties
   pageTitle="Logika aplikacije primjere i scenariji | Microsoft Azure"
   description="Primjeri uobičajenih logike aplikacije i Saznajte kako implementirati uobičajeni scenariji"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>Primjeri logike aplikacije i uobičajeni scenariji

U ovom dokumentu pojedinosti uobičajeni scenariji i primjeri koji će vam pomoći da razumijete neki od načina na koje možete koristiti logike aplikacije za automatizacija poslovnih procesa. 

## <a name="custom-triggers-and-actions"></a>Prilagođeni okidača i akcija

Možete pokrenuti logike aplikacije iz drugih aplikacija na nekoliko načina. Evo nekoliko uobičajenih primjera:

- [Stvaranje prilagođene okidača ili akciju](app-service-logic-create-api-app.md)
- [Dugoročnih akcije](app-service-logic-create-api-app.md)
- [HTTP zahtjev okidača (OBJAVA)](app-service-logic-http-endpoint.md)
- [Webhook okidača i akcija](app-service-logic-create-api-app.md)
- [Ankete okidača](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Scenariji

- [Slika odgovor na zahtjev](app-service-logic-http-endpoint.md)
- [Odgovor na zahtjev s SMS PORUKE](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Rukovanje pogreškama i zapisivanje

- [Iznimke i pogreškama](app-service-logic-exception-handling.md)
- [Konfiguriranje upozorenja Azure i dijagnostici](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Scenariji

- [Slučaj koristi: Pogreške i iznimke rukovanje](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Uvođenje i upravljanje njima

- [Stvaranje automatskog implementacije](app-service-logic-create-deploy-template.md)
- [Stvorite i implementirajte logike aplikacije Visual Studio](app-service-logic-deploy-from-vs.md)
- [Praćenje aplikacija logike](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Vrste sadržaja, pretvorbe i transformacije

Logika aplikacije [jezika za definiranje tijeka rada](http://aka.ms/logicappsdocs) sadrži mnoge funkcije omogućuje vam da biste pretvorili i rad s različitim vrstama sadržaja.  Osim toga neće imati modul sve može da biste sačuvali vrste sadržaja kao tokova podataka kroz tijek rada.

- [Rukovanje vrste sadržaja](app-service-logic-content-type.md) kao što su aplikacije/json, xml-a/aplikacije i običnog teksta
- [Stvaranje definicije tijeka rada](app-service-logic-author-definitions.md)
- [Referenca za jezik definiciju tijeka rada](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Serije i ponavljanje

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Do](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Integracija s funkcijama Azure

- [Integracija Azure funkcije](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Scenariji

- [Azure funkcija kao okidač Bus servisa](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP te OSTALE SOAP

 - [Pozivanje SOAP](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Ne možemo će zadržati dodavanje primjere i scenariji ovaj dokument. Pomoću odjeljak Komentari nam primjere ili scenarija koji biste željeli vidjeti ovdje.