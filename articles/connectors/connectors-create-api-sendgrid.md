<properties
pageTitle="SendGrid | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. Davatelj veze SendGrid omogućuje vam slanje e-pošte i upravljanje popise primatelja."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Početak rada s SendGrid poveznika

Davatelj veze SendGrid omogućuje vam slanje e-pošte i upravljanje popise primatelja.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju. 

Mogli početi s radom sada stvaranjem logike aplikacije, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za SendGrid mogu se koristiti kao akciju; ima trigger(s). Sve poveznike podržava podatke u JSON i XML formatima. 

 Poveznik za SendGrid sadrži sljedeće radnje: Postoje bez okidača.

### <a name="sendgrid-actions"></a>SendGrid akcije
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|[PošaljiE-poštu](connectors-create-api-sendgrid.md#sendemail)|Šalje poruku e-pošte pomoću SendGrid API-JA (ograničeno na 10 000 primatelja)|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Dodavanje pojedinačnih primatelja na popis primatelja|


## <a name="create-a-connection-to-sendgrid"></a>Povezivanje s SendGrid
Da biste stvorili logike aplikacije SendGrid, najprije morate stvoriti **vezu** zatim unijeti detalje za sljedeća svojstva: 

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|ApiKey|Da|Navedite ključ za Api SendGrid|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

Kada stvorite vezu, možete je koristiti za izvođenje akcije i slušanje za okidača opisane u ovom članku.

## <a name="reference-for-sendgrid"></a>Vodič za SendGrid
Odnosi se na verziju: 1.0

## <a name="sendemail"></a>PošaljiE-poštu
Slanje e-pošte: šalje poruku e-pošte pomoću SendGrid API-JA (ograničeno na 10 000 primatelja) 

```POST: /api/mail.send.json``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|zahtjev| |Da|tijelo|Ništa|Slanje poruke e-pošte|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|429|Zahtjev za previše|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Dodavanje primatelja popis: pojedinačne primatelja dodali na popis primatelja 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a popisa|niz|Da|put|Ništa|Jedinstveni id popisa primatelja|
|recipientId|niz|Da|put|Ništa|Jedinstveni id primatelja|

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

### <a name="emailrequest"></a>EmailRequest


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|iz|niz|Da |
|fromname|niz|ne |
|Da biste|niz|Da |
|toname|niz|ne |
|Predmet|niz|Da |
|tijelo|niz|Da |
|ishtml|Booleove vrijednosti|ne |
|kopija|niz|ne |
|ccname|niz|ne |
|Skrivena kopija|niz|ne |
|bccname|niz|ne |
|OutboundSMTPServer|niz|ne |
|Datum|niz|ne |
|zaglavlja|niz|ne |
|datoteka|polja|ne |
|Nazivi datoteka|polja|ne |



### <a name="emailresponse"></a>EmailResponse


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|poruka|niz|ne |



### <a name="recipientlists"></a>RecipientLists


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Popisi|polja|ne |



### <a name="recipientlist"></a>RecipientList


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|ime|niz|ne |
|recipient_count|cijeli broj|ne |



### <a name="recipients"></a>Primatelji


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Primatelji|polja|ne |



### <a name="recipient"></a>Primatelj


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|e-pošte|niz|ne |
|Pre_zime|niz|ne |
|Osobno_ime|niz|ne |
|ID-a|niz|ne |


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)