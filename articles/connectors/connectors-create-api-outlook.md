<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. Poveznik za Outlook.com omogućuje da upravljaju vašom poštom, kalendare i kontakte. Izvršite različite radnje kao što je slanje pošte, zakazivanje sastanaka, dodavanje kontakata i itd."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Početak rada s poveznik za Outlook.com

Poveznik za Outlook.com omogućuje da upravljaju vašom poštom, kalendare i kontakte. Izvršite različite radnje kao što je slanje pošte, zakazivanje sastanaka, dodavanje kontakata i itd.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju. 

Mogli početi s radom sada stvaranjem logike aplikacije, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za Outlook.com mogu se koristiti kao akciju; ima trigger(s). Sve poveznike podržava podatke u JSON i XML formatima. 

 Poveznik za Outlook.com sadrži sljedeće akcije i/ili trigger(s) dostupne:

### <a name="outlookcom-actions"></a>Akcije Outlook.com
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|Dohvaća podatke u porukama e-pošte iz mape|
|[PošaljiE-poštu](connectors-create-api-outlook.md#SendEmail)|Šalje poruku e-pošte|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Briše poruke e-pošte tako da ID-a|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Označava poruku e-pošte pročitanim|
|[OutboundSMTPServer](connectors-create-api-outlook.md#ReplyTo)|Odgovori na poruke e-pošte|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Vraća za e-poštu privitak po ID-a|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Slanje poruke e-pošte s više mogućnosti i pričekajte da odgovor na poruke uz neku od mogućnosti primatelj|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Slanje poruke e-pošte za odobrenje i pričekajte da odgovor primatelja|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Dohvaća kalendara|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Dohvaća stavki iz kalendara|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Stvara novi događaj|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Dohvaća određene stavke iz kalendara|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Brisanje stavke kalendara|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Djelomično ažurira stavke kalendara|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Dohvaća podatke u mapama kontakata|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Dohvaća kontakte iz mape kontakata|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Stvara novi kontakt|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Dohvaća podatke u određenim kontaktom iz mape kontakata|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Brisanje kontakta|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Djelomično ažuriranja kontakta|
### <a name="outlookcom-triggers"></a>Outlook.com okidača
Možete poslušati za ove event(s):

|Pokretanje | Opis|
|--- | ---|
|Na događaj prije početka|Pokreće u tijeku pokretanja nadolazećih događaja|
|Na nove e-pošte|Pokreće u tijeku kada pristigne nova e-pošta|
|Na novim stavkama|Koji se prikazuje prilikom stvaranja nove stavke kalendara|
|Na ažuriranja stavki|Koji se prikazuje kada je izmijenjena stavke kalendara|


## <a name="create-a-connection-to-outlookcom"></a>Povezivanje s Outlook.com
Da biste stvorili logike aplikacije sa servisom Outlook.com, najprije morate stvoriti **vezu** zatim unijeti detalje za sljedeća svojstva: 

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|Tokena|Da|Unesite vjerodajnice za Outlook.com|
Kada stvorite vezu, možete je koristiti za izvođenje akcije i slušanje za okidača opisane u ovom članku.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.  

## <a name="reference-for-outlookcom"></a>Vodič za Outlook.com
Odnosi se na verziju: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
Na događaj prije pokretanja: pokreće u tijeku pokretanja nadolazećih događaja 

```GET: /Events/OnUpcomingEvents``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|upit|Ništa|Jedinstveni identifikator kalendara|
|lookAheadTimeInMinutes|cijeli broj|ne|upit|15|Da biste potražili predstojeće događaje na unaprijed vrijeme (u minutama)|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|202|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="getemails"></a>GetEmails
Početak poruke e-pošte: dohvaća podatke u porukama e-pošte iz mape 

```GET: /Mail``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|folderPath|niz|ne|upit|Ulazna pošta|Put do mape u radi dohvaćanja poruke e-pošte (zadani: 'Ulazna pošta')|
|vrh|cijeli broj|ne|upit|10|Broj e-pošte za dohvaćanje (zadani: 10)|
|fetchOnlyUnread|Booleove vrijednosti|ne|upit|TRUE|Dohvaćanje samo nepročitanih poruka e-pošte?|
|includeAttachments|Booleove vrijednosti|ne|upit|FALSE|Ako je postavljen na true, privici će biti dohvaćeni i zajedno s e-pošte|
|searchQuery|niz|ne|upit|Ništa|Upit za pretraživanje da biste filtrirali poruke e-pošte|
|Preskoči|cijeli broj|ne|upit|0|Broj e-pošte da biste preskočili (zadani: 0)|
|skipToken|niz|ne|upit|Ništa|Preskakanje token dohvaćanje nove stranice|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="sendemail"></a>PošaljiE-poštu
Slanje e-pošte: šalje poruku e-pošte 

```POST: /Mail``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|PorukaEPošte| |Da|tijelo|Ništa|E-pošte|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="deleteemail"></a>DeleteEmail
Brisanje e-pošte: briše poruke e-pošte tako da ID-a 

```DELETE: /Mail/{messageId}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|messageId|niz|Da|put|Ništa|ID e-pošte da biste izbrisali|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="markasread"></a>MarkAsRead
Označavanje kao pročitano: označava poruku e-pošte pročitanim 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|messageId|niz|Da|put|Ništa|ID e-pošte označiti kao čitanja|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="replyto"></a>OutboundSMTPServer
Odgovaranje na e-pošte: odgovora na poruku e-pošte 

```POST: /Mail/ReplyTo/{messageId}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|messageId|niz|Da|put|Ništa|ID e-pošte da biste odgovorili|
|komentar|niz|Da|upit|Ništa|Odgovor komentar|
|replyAll|Booleove vrijednosti|ne|upit|FALSE|Odgovaranje na svim primateljima|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="getattachment"></a>GetAttachment
Početak privitka: vraća za e-poštu privitak po ID-a 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|messageId|niz|Da|put|Ništa|ID e-pošte|
|attachmentId|niz|Da|put|Ništa|ID da biste preuzeli privitak|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="onnewemail"></a>OnNewEmail
Na nove e-pošte: pokreće u tijeku kada pristigne nova e-pošta 

```GET: /Mail/OnNewEmail``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|folderPath|niz|ne|upit|Ulazna pošta|Mape e-pošte za dohvaćanje (zadani: Ulazna pošta)|
|Da biste|niz|ne|upit|Ništa|Adrese primatelja e-pošte|
|iz|niz|ne|upit|Ništa|S adrese|
|važnost|niz|ne|upit|Normalno|Važnost e-pošte (najviša, normalno, najniža) (zadani: normalno)|
|fetchOnlyWithAttachment|Booleove vrijednosti|ne|upit|FALSE|Dohvaćanje samo poruke e-pošte s privitkom|
|includeAttachments|Booleove vrijednosti|ne|upit|FALSE|Sadrže privitke|
|subjectFilter|niz|ne|upit|Ništa|Niz za pretraživanje u predmetu|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|202|Prihvatili|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
Slanje e-pošte s mogućnostima: slanje e-pošte s više mogućnosti i pričekajte da odgovor na poruke uz neku od mogućnosti primatelj 

```POST: /mailwithoptions/$subscriptions``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Da|tijelo|Ništa|Zahtjev za pretplatu za mogućnosti e-pošte|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|201|Pretplata je stvorena|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Slanje e-pošte odobrenje: slanje poruke e-pošte za odobrenje i pričekajte da odgovor primatelja 

```POST: /approvalmail/$subscriptions``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Da|tijelo|Ništa|Zahtjev za pretplatu za e-poštu za odobrenje|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|201|Pretplata je stvorena|
|400|BadRequest|
|401|Neovlašteno|
|403|Zabranjen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="calendargettables"></a>CalendarGetTables
Početak kalendara: dohvaća kalendara 

```GET: /datasets/calendars/tables``` 

Nema parametara za ovaj poziv
#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="calendargetitems"></a>CalendarGetItems
Početak događaja: dohvaća stavki iz kalendara 

```GET: /datasets/calendars/tables/{table}/items``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator kalendara za dohvaćanje|
|$filter|niz|ne|upit|Ništa|Upit ODATA filtar da biste ograničili broj stavki|
|$orderby|niz|ne|upit|Ništa|Upit orderBy ODATA za određivanje redoslijeda stavki|
|$skip|cijeli broj|ne|upit|Ništa|Broj stavki da biste preskočili (zadani = 0)|
|$top|cijeli broj|ne|upit|Ništa|Maksimalan broj stavki za dohvaćanje (zadani = 256)|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="calendarpostitem"></a>CalendarPostItem
Stvaranje događaja: stvara novi događaj 

```POST: /datasets/calendars/tables/{table}/items``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator kalendara|
|stavke| |Da|tijelo|Ništa|Stavke kalendara da biste stvorili|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="calendargetitem"></a>CalendarGetItem
Početak događaja: dohvaća određene stavke iz kalendara 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator kalendara|
|ID-a|niz|Da|put|Ništa|Jedinstveni identifikator stavke kalendara za dohvaćanje|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Brisanje događaja: briše stavke kalendara 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator kalendara|
|ID-a|niz|Da|put|Ništa|Jedinstveni identifikator stavke kalendara da biste izbrisali|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Ažuriranje događaj: djelomično ažurira stavke kalendara 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator kalendara|
|ID-a|niz|Da|put|Ništa|Jedinstveni identifikator stavke kalendara za ažuriranje|
|stavke| |Da|tijelo|Ništa|Stavke kalendara za ažuriranje|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Na nove stavke: koji se prikazuje prilikom stvaranja nove stavke kalendara 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator kalendara|
|$filter|niz|ne|upit|Ništa|Upit ODATA filtar da biste ograničili broj stavki|
|$orderby|niz|ne|upit|Ništa|Upit orderBy ODATA za određivanje redoslijeda stavki|
|$skip|cijeli broj|ne|upit|Ništa|Broj stavki da biste preskočili (zadani = 0)|
|$top|cijeli broj|ne|upit|Ništa|Maksimalan broj stavki za dohvaćanje (zadani = 256)|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Na ažuriranje stavke: koji se prikazuje kada je izmijenjena stavke kalendara 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator kalendara|
|$filter|niz|ne|upit|Ništa|Upit ODATA filtar da biste ograničili broj stavki|
|$orderby|niz|ne|upit|Ništa|Upit orderBy ODATA za određivanje redoslijeda stavki|
|$skip|cijeli broj|ne|upit|Ništa|Broj stavki da biste preskočili (zadani = 0)|
|$top|cijeli broj|ne|upit|Ništa|Maksimalan broj stavki za dohvaćanje (zadani = 256)|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="contactgettables"></a>ContactGetTables
Dohvaćanje mape s kontaktima: vraća kontakte mape 

```GET: /datasets/contacts/tables``` 

Nema parametara za ovaj poziv
#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="contactgetitems"></a>ContactGetItems
Dohvaćanje kontakata: dohvaća kontakte iz mape kontakata 

```GET: /datasets/contacts/tables/{table}/items``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator mapu s kontaktima za dohvaćanje|
|$filter|niz|ne|upit|Ništa|Upit ODATA filtar da biste ograničili broj stavki|
|$orderby|niz|ne|upit|Ništa|Upit orderBy ODATA za određivanje redoslijeda stavki|
|$skip|cijeli broj|ne|upit|Ništa|Broj stavki da biste preskočili (zadani = 0)|
|$top|cijeli broj|ne|upit|Ništa|Maksimalan broj stavki za dohvaćanje (zadani = 256)|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="contactpostitem"></a>ContactPostItem
Stvaranje kontakta: stvara novi kontakt 

```POST: /datasets/contacts/tables/{table}/items``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator mape kontakata|
|stavke| |Da|tijelo|Ništa|Stvaranje kontakta|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="contactgetitem"></a>ContactGetItem
Početak kontakt: dohvaća podatke u određenim kontaktom iz mape kontakata 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator mape kontakata|
|ID-a|niz|Da|put|Ništa|Jedinstveni identifikator kontakta za dohvaćanje|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Brisanje kontakta: Brisanje kontakta 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator mape kontakata|
|ID-a|niz|Da|put|Ništa|Jedinstveni identifikator kontakta da biste izbrisali|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="contactpatchitem"></a>ContactPatchItem
Obnoviti kontakt: djelomično ažurira kontaktu 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Tablica|niz|Da|put|Ništa|Jedinstveni identifikator mape kontakata|
|ID-a|niz|Da|put|Ništa|Jedinstveni identifikator kontakta za ažuriranje|
|stavke| |Da|tijelo|Ništa|Stavci kontakta za ažuriranje|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="object-definitions"></a>Definicija objekta 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [niz, objekt]]


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="object"></a>Objekt


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|



### <a name="sendmessage"></a>Otprema


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Privici|polja|ne |
|Iz|niz|ne |
|Kopija|niz|ne |
|Skrivena kopija|niz|ne |
|Predmet|niz|Da |
|Tijelo|niz|Da |
|Važnost|niz|ne |
|IsHtml|Booleove vrijednosti|ne |
|Da biste|niz|Da |



### <a name="sendattachment"></a>SendAttachment


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|@odata.type|niz|ne |
|Ime|niz|Da |
|ContentBytes|niz|Da |



### <a name="receivemessage"></a>ReceiveMessage


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|ne |
|IsRead|Booleove vrijednosti|ne |
|HasAttachment|Booleove vrijednosti|ne |
|DateTimeReceived|niz|ne |
|Privici|polja|ne |
|Iz|niz|ne |
|Kopija|niz|ne |
|Skrivena kopija|niz|ne |
|Predmet|niz|Da |
|Tijelo|niz|Da |
|Važnost|niz|ne |
|IsHtml|Booleove vrijednosti|ne |
|Da biste|niz|Da |



### <a name="receiveattachment"></a>ReceiveAttachment


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|Da |
|ContentType|niz|Da |
|@odata.type|niz|ne |
|Ime|niz|Da |
|ContentBytes|niz|Da |



### <a name="digestmessage"></a>DigestMessage


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Predmet|niz|Da |
|Tijelo|niz|ne |
|Važnost|niz|ne |
|Sažetka|polja|Da |
|Privici|polja|ne |
|Da biste|niz|Da |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="datasetsmetadata"></a>DataSetsMetadata


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Tablični|nije definirano|ne |
|blob|nije definirano|ne |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Izvor|niz|ne |
|riješiti problem|niz|ne |
|urlEncoding|niz|ne |
|tableDisplayName|niz|ne |
|tablePluralName|niz|ne |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Izvor|niz|ne |
|riješiti problem|niz|ne |
|urlEncoding|niz|ne |



### <a name="tablemetadata"></a>TableMetadata


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ime|niz|ne |
|Naslov|niz|ne |
|x ms dozvola|niz|ne |
|x, ms i mogućnosti|nije definirano|ne |
|shema|nije definirano|ne |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|sortRestrictions|nije definirano|ne |
|filterRestrictions|nije definirano|ne |
|filterFunctions|polja|ne |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|koje je moguće sortirati|Booleove vrijednosti|ne |
|unsortableProperties|polja|ne |
|ascendingOnlyProperties|polja|ne |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|koje je moguće filtrirati|Booleove vrijednosti|ne |
|nonFilterableProperties|polja|ne |
|requiredProperties|polja|ne |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|NotificationUrl|niz|ne |
|Poruka|nije definirano|ne |



### <a name="messagewithoptions"></a>MessageWithOptions


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Predmet|niz|Da |
|Mogućnosti|niz|Da |
|Tijelo|niz|ne |
|Važnost|niz|ne |
|Privici|polja|ne |
|Da biste|niz|Da |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|ne |
|resurs|niz|ne |
|notificationType|niz|ne |
|notificationUrl|niz|ne |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|NotificationUrl|niz|ne |
|Poruka|nije definirano|ne |



### <a name="approvalmessage"></a>ApprovalMessage


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Predmet|niz|Da |
|Mogućnosti|niz|Da |
|Tijelo|niz|ne |
|Važnost|niz|ne |
|Privici|polja|ne |
|Da biste|niz|Da |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|SelectedOption|niz|ne |



### <a name="tableslist"></a>TablesList


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="table"></a>Tablica


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Ime|niz|ne |
|Riješiti problem|niz|ne |



### <a name="item"></a>Stavke


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ItemInternalId|niz|ne |



### <a name="calendaritemslist"></a>CalendarItemsList


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="calendaritem"></a>CalendarItem


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ItemInternalId|niz|ne |



### <a name="contactitemslist"></a>ContactItemsList


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="contactitem"></a>ContactItem


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ItemInternalId|niz|ne |



### <a name="datasetslist"></a>DataSetsList


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="dataset"></a>Skup podataka


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Ime|niz|ne |
|Riješiti problem|niz|ne |


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)