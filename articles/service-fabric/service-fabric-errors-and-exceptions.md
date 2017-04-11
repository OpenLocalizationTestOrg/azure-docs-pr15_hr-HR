<properties
   pageTitle="Uobičajeni FabricClient iznimke Izbačena | Microsoft Azure"
   description="U članku se opisuje uobičajene iznimke i pogreške koje se mogu FabricClient API-ji izbačena prilikom izvođenja aplikacije i postupci upravljanja klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Najčešći iznimke i pogreške prilikom rada s API-ji FabricClient
API-ji [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) omogućiti klaster i aplikacije administratori za obavljanje zadataka administracije na servis tkanina aplikaciju, servis ili klaster. Ako, na primjer, implementaciju aplikacije, Nadogradnja i uklanjanja Provjera stanja klaster ili na servis za testiranje. Razvojnim inženjerima i klaster administratori mogu koristiti FabricClient API-ji za razvoj alata za upravljanje servisa tkanina klaster te aplikacije.

Postoje različite vrste operacije koje možete izvršiti pomoću FabricClient.  Svaki način mogu vratiti iznimke za pogreške zbog pogrešan unos, pogreške pri izvođenju ili tranzitne infrastrukture problema.  Potražite u dokumentaciji API referenca da biste pronašli koje iznimke su izbačena određene metodom. Postoje neke iznimke, međutim, koje možete je izbacio mnogo različitih [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) API-ji. U sljedećoj su tablici navedeni iznimke koji su uobičajeni preko FabricClient API-ji.

|Iznimke| Kada Izbačena|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Objekt [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) je u zatvorenom stanju. Rashoda objekta [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) koriste i stvoriti novi [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objekt. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Postupak isteklo. Kada više MaxOperationTimeout da biste dovršili postupak vodi, vraća se [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) .|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Provjera pristupa za operaciju nije uspjelo. Vraća se E_ACCESSDENIED.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Izvođenje operacije došlo je do pogreške prilikom izvođenja. Bilo koji od načina FabricClient potencijalno mogu vratiti [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx), svojstvo [KodPogreške](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) označava točan uzrok iznimku. Kodovi pogrešaka se definiraju [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) numeriranje.|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Operacija nije uspjela zbog tranzitne pogreškom neke vrste. Na primjer, postupak možda neće uspjeti jer kvorum replike privremeno nije dostupan. Nije uspjelo operacije koje možete pokušati odgovaraju tranzitne iznimke.|

Neke uobičajene [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) pogreške koje se mogu vratiti u [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx):

|Pogreška| Uvjet|
|---------|:-----------|
|CommunicationError|Zbog pogreške za komunikaciju postupak neće funkcionirati, ponovite postupak.|
|InvalidCredentialType|Vrsta vjerodajnica nije valjana.|
|InvalidX509FindType|U X509FindType nije valjana.|
|InvalidX509StoreLocation|Na X509 mjesto spremišta nije valjana.|
|InvalidX509StoreName|Na X509 partneri prodavači nije valjana.|
|InvalidX509Thumbprint|Na X509 niz otisak prsta certifikat nije valjan.|
|InvalidProtectionLevel|Razine zaštite nije valjan.|
|InvalidX509Store|X509 spremište certifikata nije moguće otvoriti.|
|InvalidSubjectName|Predmetni naziv nije valjan.|
|InvalidAllowedCommonNameList|Oblik niza uobičajeni naziv popisa nije valjan. Mora biti popis odvojenih zarezom.|
