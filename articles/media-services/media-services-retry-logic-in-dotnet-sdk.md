<properties
    pageTitle="Ponovite logike u Media Services SDK za .NET | Microsoft Azure"
    description="U temi daje pregled pokušaj logike u Media Services SDK za .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Ponovite logike u Media Services SDK za .NET

Kada radite pomoću servisa Microsoft Azure, tranzitne kvarove može se pojaviti. Ako tranzitne kvara pojavljuje u većini slučajeva, nakon nekoliko ponovne pokušaje postupak uspijeva. Media Services SDK za .NET implementira pokušaj logike za rukovanje tranzitne kvarove vezanih uz iznimke i pogreške koje je uzrokovano web-zahtjeva izvršavanja upita, spremite promjene i postupci za pohranu.  Prema zadanim postavkama Media Services SDK za .NET izvršava četiri ponovne pokušaje prije ponovno prijavi iznimke u aplikaciji. Kod u aplikaciji morate zatim pravilno rukovati Ova se iznimka.  
  
 Slijedi kratak vodič zahtjev za Web, za pohranu, upita i SaveChanges pravila:  
  
-   Pravila za pohranu koristi se za operacije spremišta blobova platforme (prijenos ili preuzimanje datoteka resursa).  
  
-   Pravilnik za zahtjev za Web se koristi za zahtjeve Generički web (na primjer, za početak token za provjeru autentičnosti i rješavanje klaster krajnje korisnike).  
  
-   Pravila upita koja se koristi za ispitivanje entiteti s OSTATKOM (na primjer, mediaContext.Assets.Where(...)).  
  
-   Pravila SaveChanges koristi se za ikakve koja se mijenja podataka unutar servisa (na primjer, stvaranje entitet ažuriranje entitet, pozivanja funkcije servisa za operaciju).  
  
 Ova tema sadrži popis vrsta iznimke i šifre pogrešaka koje rješava Media Services SDK za .NET ponovnog pokušaja.  
  
## <a name="exception-types"></a>Vrsta iznimke  

U sljedećoj tablici opisane Media Services SDK za .NET rukuje ili postupak za neki postupci koji mogu uzrokovati kvarove tranzitne iznimke.  
  
Iznimke|Zahtjev za web|Prostor za pohranu|Upit|SaveChanges
----|------|----|---|---
WebException<br/>Dodatne informacije potražite u odjeljku [kodovi stanja WebException](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Da|Da|Da|Da  
DataServiceClientException<br/> Dodatne informacije potražite u članku [HTTP kodovima stanja pogreške](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|ne|Da|Da|Da
DataServiceQueryException<br/> Dodatne informacije potražite u članku [HTTP kodovima stanja pogreške](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|ne|Da|Da|Da  
DataServiceRequestException<br/> Dodatne informacije potražite u članku [HTTP kodovima stanja pogreške](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|ne|Da|Da|Da  
DataServiceTransportException|ne|ne|Da|Da
TimeoutException|Da|Da|Da|ne
SocketException|Da|Da|Da|Da  
StorageException|ne|Da|ne|ne 
IOException|ne|Da|ne|ne
  
###  <a name="WebExceptionStatus"></a>Kodovi stanja WebException  

Sljedeća tablica prikazuje za šifre pogrešaka koje WebException je implementirana logike pokušajte ponovno. Numeriranje [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) definira kodovi stanja.  
  
Status|Zahtjev za web|Prostor za pohranu|Upit|SaveChanges  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|Da|Da|Da|Da
NameResolutionFailure|Da|Da|Da|Da  
ProxyNameResolutionFailure|Da|Da|Da|Da  
SendFailure|Da|Da|Da|Da
PipelineFailure|Da|Da|Da|ne  
ConnectionClosed|Da|Da|Da|ne  
KeepAliveFailure|Da|Da|Da|ne  
UnknownError|Da|Da|Da|ne 
ReceiveFailure|Da|Da|Da|ne  
RequestCanceled|Da|Da|Da|ne  
Prekoračenje vremena|Da|Da|Da|ne
ProtocolError <br/>Pokušajte ponovno na ProtocolError upravlja obrade Šifra stanja HTTP. Dodatne informacije potražite u članku [HTTP kodovima stanja pogreške](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Da|Da|Da|Da|  
  
###  <a name="HTTPStatusCode"></a>HTTP kodovima stanja pogreške  

Kada se upit i SaveChanges operacija vratiti DataServiceClientException, DataServiceQueryException ili DataServiceQueryException, HTTP kod stanja pogreške, vraća se u svojstvu kôd statusa.  Sljedeća tablica prikazuje za koje šifre pogreške je implementirana logike pokušajte ponovno.  
  
 
Status|Zahtjev za web|Prostor za pohranu|Upit|SaveChanges 
---|----|----|----|----
401|ne|Da|ne|ne
403|ne|Da<br/>Rukovanje ponovne pokušaje s više čekanje.|ne|ne  
408|Da|Da|Da|Da
429|Da|Da|Da|Da  
500|Da|Da|Da|ne  
502|Da|Da|Da|ne  
503|Da|Da|Da|Da  
504|Da|Da|Da|ne  
  
Ako želite da preuzima susret stvarni implementaciju Media Services SDK za .NET ponovnog pokušaja potražite u članku [azure-sdk-za-media-servisima](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
