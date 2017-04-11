<properties
pageTitle="RSS | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. Poveznik za RSS korisnicima omogućuje objavljivanje i dohvaćanje sažetaka sadržaja stavke. Omogućuje korisnicima omogućuje pokretanje operacije kada novu stavku Objavi sažetka sadržaja."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-rss-connector"></a>Uvod u RSS poveznika
RSS popularnih web syndication oblik se koristi da biste objavili često ažurirani sadržaj – kao što su unosa bloga i vijesti.  Mnoge sadržaja izdavača omogućuju RSS sažetke sadržaja da biste korisnicima omogućili pretplatite na njega.  Pomoću poveznika RSS dohvaćanja sažetka sadržaja podataka i pokretanje tokova nove stavke koje su objavljene na RSS sažetak sadržaja.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju. 

Mogli početi s radom sada stvaranjem logike aplikacije, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za RSS mogu se koristiti kao akciju; ima trigger(s). Sve poveznike podržava podatke u JSON i XML formatima. 

 Poveznik za RSS sadrži sljedeće akcije i/ili trigger(s) dostupne:

### <a name="rss-actions"></a>RSS akcije
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Pronađite sve RSS sažetka sadržaja stavke.|
### <a name="rss-triggers"></a>RSS okidača
Možete poslušati za ove event(s):

|Pokretanje | Opis|
|--- | ---|
|Kada se objavljuje nova stavka sažetka sadržaja|Pokreće tijek rada kad se objavljuje novi sažetak sadržaja|


## <a name="create-a-connection-to-rss"></a>Stvaranje veze na RSS

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

## <a name="reference-for-rss"></a>Vodič za RSS
Odnosi se na verziju: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Kada se objavljuje nova stavka sažetka sadržaja: pokrene tijek rada kad se objavljuje novi sažetak sadržaja 

```GET: /OnNewFeed``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|feedUrl|niz|Da|upit|Ništa|Url sažetka sadržaja|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|202|Prihvaćen|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="listfeeditems"></a>ListFeedItems
Popis svih RSS sažetka sadržaja stavki.: se sve RSS sažetka sadržaja stavke. 

```GET: /ListFeedItems``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|feedUrl|niz|Da|upit|Ništa|Url sažetka sadržaja|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|202|Prihvatili|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="object-definitions"></a>Definicija objekta 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="feeditem"></a>FeedItem


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|Da |
|Naslov|niz|Da |
|sadržaj|niz|Da |
|veze|polja|ne |
|updatedOn|niz|ne |


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)