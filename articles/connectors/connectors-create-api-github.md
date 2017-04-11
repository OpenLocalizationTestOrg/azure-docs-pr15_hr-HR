<properties
pageTitle="GitHub | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. GitHub je utemeljen na webu brojka spremište hostiranje. Nudi sve raspodijeljeno revizije kontrola i izvorni kod management (IO) funkcije brojka kao i dodavanju vlastitu značajki."
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

# <a name="get-started-with-the-github-connector"></a>Početak rada s GitHub poveznika

GitHub utemeljenom na webu brojka spremište hostiranje je. Nudi sve raspodijeljeno revizije kontrola i izvorni kod management (IO) funkcije brojka kao i dodavanju vlastitu značajki.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju. 

Mogli početi s radom sada stvaranjem logike aplikacije, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za GitHub mogu se koristiti kao akciju; ima trigger(s). Sve poveznike podržava podatke u JSON i XML formatima. 

 Poveznik za GitHub sadrži sljedeće akcije i/ili trigger(s) dostupne:

### <a name="github-actions"></a>GitHub akcije
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Stvara problema|
### <a name="github-triggers"></a>GitHub okidača
Možete poslušati za ove event(s):

|Pokretanje | Opis|
|--- | ---|
|Ako je otvorena problema|Problem se otvori|
|Kad je zatvoreno problema|Problem je zatvoreno|
|Kada vam je dodijeljen problema|Problem se dodjeljuje|


## <a name="create-a-connection-to-github"></a>Povezivanje s GitHub
Da biste stvorili logike aplikacije GitHub, najprije morate stvoriti **vezu** zatim detalje za sljedeća svojstva: 

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|Tokena|Da|Unesite vjerodajnice GitHub|
Kada stvorite vezu, možete je koristiti za izvođenje akcije i slušanje za okidača opisane u ovom članku. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

## <a name="reference-for-github"></a>Vodič za GitHub
Odnosi se na verziju: 1.0

## <a name="createissue"></a>CreateIssue
Stvaranje problema: stvara problema 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|repositoryOwner|niz|Da|put|Ništa|Vlasnik spremište|
|repositoryName|niz|Da|put|Ništa|Naziv spremišta|
|issueBasicDetails| |Da|tijelo|Ništa|Detalji o temi|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="issueopened"></a>IssueOpened
Ako je otvorena problem: problem otvoren 

```GET: /trigger/issueOpened``` 

Nema parametara za ovaj poziv
#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="issueclosed"></a>IssueClosed
Kad je zatvoreno problem: problem je zatvoreno 

```GET: /trigger/issueClosed``` 

Nema parametara za ovaj poziv
#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="issueassigned"></a>IssueAssigned
Kada vam je dodijeljen problem: problem se dodjeljuje 

```GET: /trigger/issueAssigned``` 

Nema parametara za ovaj poziv
#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="object-definitions"></a>Definicija objekta 

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Naslov|niz|Da |
|tijelo|niz|Da |
|primatelja dodjele|niz|Da |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Naslov|niz|Da |
|tijelo|niz|Da |
|primatelja dodjele|niz|Da |
|broj|niz|ne |
|Stanje|niz|ne |
|created_at|niz|ne |
|repository_url|niz|ne |


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)