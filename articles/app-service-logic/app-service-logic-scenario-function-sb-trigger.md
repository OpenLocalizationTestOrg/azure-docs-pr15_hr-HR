<properties
   pageTitle="Scenarij aplikacije logika: Stvaranje okidača Azure funkcije servisa Bus | Microsoft Azure"
   description="Da biste stvorili servisa Bus okidača za aplikaciju logiku funkcije Azure"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Scenarij aplikacije logika: Stvaranje okidača Bus servisa Azure pomoću funkcije Azure

Azure funkcije možete koristiti da biste stvorili okidača za aplikaciju logike kada je potrebno za implementaciju u dugoročnih ga slušatelj ili zadatak. Na primjer, možete stvoriti funkciju koja će se svaki tjedan Budite prisutni na red i odmah pokrenuti aplikaciju logike kao okidač automatske.

## <a name="build-the-logic-app"></a>Stvaranje aplikacije logike

U ovom primjeru imate funkcija pokrenut za svaku aplikaciju logike koji mora biti pokrenut. Prvo, stvorite aplikaciju za logiku koja sadrži okidača HTTP zahtjev. Funkcija poziva te krajnjoj točki kad god se prima reda čekanja poruka.  

1. Stvorite novu aplikaciju logike; Odaberite okidača **ručno – kada an HTTP zahtjev je primljen** .  
   Po želji možete navesti JSON sheme za uporabu reda čekanja poruka pomoću alata kao što je [jsonschema.net](http://jsonschema.net). Zalijepite u shemi okidača. To pomaže u dizajneru razumijevanje oblik podataka i više jednostavno kreću se svojstva kroz tijek rada.
1. Dodavanje dodatnih koraka koji želite da se pokreće kada se prima reda čekanja poruka. Na primjer, pošaljite poruku e-pošte putem sustava Office 365.  
1. Spremanje aplikacije logike za generiranje URL povratnog okidača za aplikaciju logike. Na kartici okidača pojavit će se URL-a.

![Pojavljuje se povratnog URL-a na kartici okidača][1]

## <a name="build-the-function"></a>Sastavljanje funkcija

Ćete morati stvoriti funkcija koje će poslužiti kao okidača i preslušavanje reda.

1. [Portal za Azure funkcije](https://functions.azure.com/signin)odaberite **Novoj funkciji**, a zatim predložak **ServiceBusQueueTrigger - C#** .

    ![Portal za Azure funkcije][2]

2. Konfigurirati vezu čekanja za servis Bus (koji će koristiti Bus SDK-a servisa Azure `OnMessageReceive()` ga slušatelj).
3. Jednostavne funkcije da biste nazvali logike krajnja točka aplikacije (ste ranije stvorili) pomoću reda čekanja poruka kao okidača za pisanje. Evo primjera cijelog funkcije. U primjeru se koristi u `application/json` vrste sadržaja poruke, ali to možete promijeniti ako je potrebno.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Da biste testirali, dodajte poruku reda čekanja putem alata kao što je [Servis Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer). Potražite aplikaciju logike pokreću odmah nakon funkciju primi poruku.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
