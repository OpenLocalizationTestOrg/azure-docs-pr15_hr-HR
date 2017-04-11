<properties
pageTitle="Wunderlist | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. Wunderlist navedite popis obveza popisa i zadatka Upravitelj radi lakšeg njihove obaviti posao.  Hoće li se popis namirnica zajednički koristite s dragih prijatelja, rad na projektu ili planirate godišnjih odmora, Wunderlist olakšava zabilježite, zajedničko korištenje i dovršili vaše to¬dos. Wunderlist sinkronizira trenutačno između telefona, tableta i računala, da biste mogli pristupati svim vašim zadacima s bilo kojeg mjesta."
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

# <a name="get-started-with-the-wunderlist-connector"></a>Početak rada s Wunderlist poveznika

Wunderlist navedite popis obveza popisa i zadatka Upravitelj radi lakšeg njihove obaviti posao.  Hoće li se popis namirnica zajednički koristite s dragih prijatelja, rad na projektu ili planirate godišnjih odmora, Wunderlist olakšava zabilježite, zajedničko korištenje i dovršili vaše to¬dos. Wunderlist sinkronizira trenutačno između telefona, tableta i računala, da biste mogli pristupati svim vašim zadacima s bilo kojeg mjesta.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju. 

Mogli početi s radom sada stvaranjem logike aplikacije, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za Wunderlist mogu se koristiti kao akciju; ima trigger(s). Sve poveznike podržava podatke u JSON i XML formatima. 

 Poveznik za Wunderlist sadrži sljedeće akcije i/ili trigger(s) dostupne:

### <a name="wunderlist-actions"></a>Wunderlist akcije
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|Dohvaćanje popise povezane s vašim računom.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Stvaranje popisa.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Dohvaćanje zadataka s određenog popisa.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Stvaranje zadatka|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Dohvaćanje podzadataka s određenog popisa ili iz određenog zadatka.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Stvaranje subtask unutar određenog zadatka|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Dohvaćanje bilješki za određenog popisa ili određenog zadatka.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Dodajte bilješku određenog zadatka|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Dohvaćanje zadataka komentari za određeni popis ili određenog zadatka.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Dodavanje komentara određenog zadatka|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Dohvaćanje podsjetnike za određeni popis ili određenog zadatka.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Postavljanje podsjetnika.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Dohvaćanje datoteka za određeni popis ili određenog zadatka.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Dohvaća podatke u određenom popisu|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Brisanje popisa|
|[UpdateList](connectors-create-api-wunderlist.md#updatelist)|Ažuriranje određenih popisa|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Dohvaća određenog zadatka|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Ažurira određenog zadatka|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Briše određenog zadatka|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Dohvaća podatke u određenim subtask|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Ažurira određene subtask|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Briše određene subtask|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Dohvaćanje određenu bilješku|
|[UpdateNote](connectors-create-api-wunderlist.md#updatenote)|Ažuriranje određenu bilješku|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Brisanje određene bilješke|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Dohvaćanje komentar određenog zadatka|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Želite li ažurirati određene podsjetnik|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Brisanje određenih podsjetnik|
### <a name="wunderlist-triggers"></a>Wunderlist okidača
Možete poslušati za ove event(s):

|Pokretanje | Opis|
|--- | ---|
|Kada je zadatak|Pokreće novu tijek kada dospije zadatka na popisu|
|Pri stvaranju novog zadatka|Pokreće novu tijek stvaranja novog zadatka na popisu|
|Kada se pojavljuje podsjetnik|Pokreće novu tijek kada se pojavi podsjetnik|


## <a name="create-a-connection-to-wunderlist"></a>Povezivanje s Wunderlist
Da biste stvorili logike aplikacije Wunderlist, najprije morate stvoriti **vezu** zatim unijeti detalje za sljedeća svojstva: 

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|Tokena|Da|Unesite vjerodajnice Wunderlist|
Kada stvorite vezu, možete je koristiti za izvođenje akcije i slušanje za okidača opisane u ovom članku. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

## <a name="reference-for-wunderlist"></a>Vodič za Wunderlist
Odnosi se na verziju: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Kada je zadatak: pokreće novi tijek kada dospije zadatka na popisu 

```GET: /trigger/tasksdue``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|


## <a name="triggertasknew"></a>TriggerTaskNew
Kada se stvara novi zadatak: pokreće novi tijek stvaranja novog zadatka na popisu 

```GET: /trigger/tasksnew``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|


## <a name="triggerreminder"></a>TriggerReminder
Kada se pojavljuje podsjetnik: pokreće novi tijek kada se pojavi podsjetnik 

```GET: /trigger/reminders``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|task_id|cijeli broj|ne|upit|Ništa|ID zadatka|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|


## <a name="retrievelists"></a>RetrieveLists
Dohvaćanje popisa: dohvaćanje popise povezane s vašim računom. 

```GET: /lists``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="createlist"></a>CreateList
Stvaranje popisa: Stvaranje popisa. 

```POST: /lists``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Objava| |Da|tijelo|Ništa|Novi popis će biti stvoren|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|Zadani|Nije uspjelo.|


## <a name="listtasks"></a>ListTasks
Dohvaćanje zadataka: dohvaćanje zadataka s određenog popisa. 

```GET: /tasks``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|Dovršeno|Booleove vrijednosti|ne|upit|Ništa|Dovršeno|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="createtask"></a>CreateTask
Stvaranje zadatka: Stvaranje zadatka 

```POST: /tasks``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Objava| |Da|tijelo|Ništa|Novi zadatak će biti stvoren|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|201|Stvorili|


## <a name="listsubtasks"></a>ListSubTasks
Početak podzadataka: dohvaćanje podzadataka s određenog popisa ili iz određenog zadatka. 

```GET: /subtasks``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|task_id|cijeli broj|ne|upit|Ništa|ID zadatka|
|Dovršeno|Booleove vrijednosti|ne|upit|Ništa|Dovršeno|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="createsubtask"></a>CreateSubTask
Stvaranje na subtask: Stvaranje subtask unutar određenog zadatka 

```POST: /subtasks``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Objava| |Da|tijelo|Ništa|Novi subtask će biti stvoren|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|201|Stvorili|


## <a name="listnotes"></a>ListNotes
Primati bilješke: dohvaćanje bilješki za određenog popisa ili određenog zadatka. 

```GET: /notes``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|task_id|cijeli broj|ne|upit|Ništa|ID zadatka|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="createnote"></a>CreateNote
Stvaranje bilješke: Dodavanje bilješke određenog zadatka 

```POST: /notes``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Objava| |Da|tijelo|Ništa|Nova bilješka će biti stvoren|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|201|Stvorili|


## <a name="listcomments"></a>ListComments
Dohvaćanje zadataka komentari: dohvaćanje zadataka komentari za određeni popis ili određenog zadatka. 

```GET: /task_comments``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|task_id|cijeli broj|ne|upit|Ništa|ID zadatka|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="createcomment"></a>CreateComment
Dodavanje komentara uz zadatak: dodavanje komentara određenog zadatka 

```POST: /task_comments``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Objava| |Da|tijelo|Ništa|Novi zadatak komentar će biti stvoren|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|201|Stvorili|


## <a name="retrievereminders"></a>RetrieveReminders
Poslužite se podsjetnicima: dohvaćanje podsjetnike za određeni popis ili određenog zadatka. 

```GET: /reminders``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|task_id|cijeli broj|ne|upit|Ništa|ID zadatka|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="createreminder"></a>CreateReminder
Postavljanje podsjetnika: postavite podsjetnik. 

```POST: /reminders``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Objava| |Da|tijelo|Ništa|Novi podsjetnik će biti stvoren|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|Zadani|Nije uspjelo.|


## <a name="retrievefiles"></a>RetrieveFiles
Dohvaćanje datoteka: dohvaćanje datoteka za određeni popis ili određenog zadatka. 

```GET: /files``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|task_id|cijeli broj|ne|upit|Ništa|ID zadatka|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak uspješan|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="getlist"></a>GetList
Pronalaženje popisa: dohvaća podatke u određenom popisu 

```GET: /lists/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|ID popisa|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="deletelist"></a>DeleteList
Brisanje popisa: brisanje popisa 

```DELETE: /lists/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|cijeli broj|Da|put|Ništa|ID popisa|
|revizije|cijeli broj|Da|upit|Ništa|Revizije|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|204|Nema sadržaja|


## <a name="updatelist"></a>UpdateList
Ažuriranje popisa: ažuriranje određenih popisa 

```PATCH: /lists/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|cijeli broj|Da|put|Ništa|ID popisa|
|Objava| |Da|tijelo|Ništa|Detalji popisa|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="gettask"></a>GetTask
Početak zadatka: dohvaća određenog zadatka 

```GET: /tasks/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|ID-a|cijeli broj|Da|put|Ništa|ID zadatka|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="updatetask"></a>UpdateTask
Ažuriranje zadatka: ažurira određenog zadatka 

```PATCH: /tasks/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|ID-a|cijeli broj|Da|put|Ništa|ID zadatka|
|Objava| |Da|tijelo|Ništa|Detalji o zadatku|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="deletetask"></a>DeleteTask
Brisanje zadatka: briše određenog zadatka 

```DELETE: /tasks/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|cijeli broj|Da|upit|Ništa|ID popisa|
|ID-a|cijeli broj|Da|put|Ništa|ID zadatka|
|revizije|cijeli broj|Da|upit|Ništa|Revizije|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|204|Nema sadržaja|


## <a name="getsubtask"></a>GetSubTask
Početak subtask: dohvaća podatke u određenim subtask 

```GET: /subtasks/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|Subtask ID-a|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="updatesubtask"></a>UpdateSubTask
Ažuriranje na subtask: ažurira određene subtask 

```PATCH: /subtasks/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|cijeli broj|Da|put|Ništa|Subtask ID-a|
|Objava| |Da|tijelo|Ništa|Detalji o subtask|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="deletesubtask"></a>DeleteSubTask
Brisanje na subtask: briše određene subtask 

```DELETE: /subtasks/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|cijeli broj|Da|put|Ništa|Subtask ID-a|
|revizije|cijeli broj|Da|upit|Ništa|Revizije|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|204|Nema sadržaja|


## <a name="getnote"></a>GetNote
Početak bilješke: dohvaćanje određenu bilješku 

```GET: /notes/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|Napomena ID-a|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="updatenote"></a>UpdateNote
Ažuriranje bilješke: ažuriranje određenu bilješku 

```PATCH: /notes/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|cijeli broj|Da|put|Ništa|Napomena ID-a|
|Objava| |Da|tijelo|Ništa|Napomena pojedinosti|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="deletenote"></a>DeleteNote
Brisanje bilješke: brisanje određene bilješke 

```DELETE: /notes/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|cijeli broj|Da|put|Ništa|Napomena ID-a|
|revizije|cijeli broj|Da|upit|Ništa|Revizije|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|204|Nema sadržaja|


## <a name="getcomment"></a>GetComment
Početak zadatka komentar: dohvaćanje komentar određenog zadatka 

```GET: /task_comments/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|ID komentara|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="updatereminder"></a>UpdateReminder
Ažuriranje podsjetnika: ažuriranje određene podsjetnik 

```PATCH: /reminders/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|cijeli broj|Da|put|Ništa|ID podsjetnik|
|Objava| |Da|tijelo|Ništa|Detalji o podsjetnik|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|


## <a name="deletereminder"></a>DeleteReminder
Brisanje podsjetnika: brisanje određenih podsjetnik 

```DELETE: /reminders/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|cijeli broj|Da|put|Ništa|ID podsjetnik.|
|revizije|cijeli broj|Da|upit|Ništa|Revizije|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|204|Nema sadržaja|


## <a name="object-definitions"></a>Definicija objekta 

### <a name="list"></a>Popis


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|created_at|niz|ne |
|Naslov|niz|ne |
|list_type|niz|ne |
|Vrsta|niz|ne |
|revizije|cijeli broj|ne |



### <a name="createdlist"></a>CreatedList


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|created_at|niz|ne |
|Naslov|niz|ne |
|revizije|cijeli broj|ne |
|Vrsta|niz|ne |



### <a name="task"></a>Zadatak


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|assignee_id|cijeli broj|ne |
|assigner_id|cijeli broj|ne |
|created_at|niz|ne |
|created_by_id|cijeli broj|ne |
|due_date|niz|ne |
|list_id|cijeli broj|ne |
|revizije|cijeli broj|ne |
|starred|Booleove vrijednosti|ne |
|Naslov|niz|ne |



### <a name="subtask"></a>Subtask


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|task_id|cijeli broj|ne |
|created_at|niz|ne |
|created_by_id|cijeli broj|ne |
|revizije|niz|ne |
|Naslov|niz|ne |



### <a name="note"></a>Napomena


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|task_id|cijeli broj|ne |
|sadržaj|niz|ne |
|created_at|niz|ne |
|updated_at|niz|ne |
|revizije|cijeli broj|ne |



### <a name="comment"></a>Komentar


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|task_id|cijeli broj|ne |
|revizije|cijeli broj|ne |
|tekst|niz|ne |
|Vrsta|niz|ne |
|created_at|niz|ne |



### <a name="reminder"></a>Podsjetnik


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|Datum|niz|ne |
|task_id|cijeli broj|ne |
|revizije|cijeli broj|ne |
|Vrsta|niz|ne |
|created_at|niz|ne |
|updated_at|niz|ne |



### <a name="createdreminder"></a>CreatedReminder


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|Datum|niz|ne |
|task_id|cijeli broj|ne |
|revizije|cijeli broj|ne |
|created_at|niz|ne |
|updated_at|niz|ne |



### <a name="file"></a>Datoteka


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|cijeli broj|ne |
|URL-a|niz|ne |
|task_id|cijeli broj|ne |
|list_id|cijeli broj|ne |
|user_id|cijeli broj|ne |
|put|niz|ne |
|CONTENT_TYPE|niz|ne |
|file_size|cijeli broj|ne |
|local_created_at|niz|ne |
|created_at|niz|ne |
|updated_at|niz|ne |
|Vrsta|niz|ne |
|revizije|cijeli broj|ne |



### <a name="newtask"></a>NewTask


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|list_id|cijeli broj|Da |
|Naslov|niz|Da |
|assignee_id|cijeli broj|ne |
|Dovršeno|Booleove vrijednosti|ne |
|recurrence_type|niz|ne |
|recurrence_count|cijeli broj|ne |
|due_date|niz|ne |
|starred|Booleove vrijednosti|ne |



### <a name="newlist"></a>NewList


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Naslov|niz|Da |



### <a name="newsubtask"></a>NewSubtask


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|list_id|cijeli broj|Da |
|task_id|cijeli broj|Da |
|Naslov|niz|Da |
|Dovršeno|Booleove vrijednosti|ne |



### <a name="newnote"></a>NewNote


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|list_id|cijeli broj|Da |
|task_id|cijeli broj|Da |
|sadržaj|niz|Da |



### <a name="newcomment"></a>NewComment


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|list_id|cijeli broj|Da |
|task_id|cijeli broj|Da |
|tekst|niz|Da |



### <a name="newreminder"></a>NewReminder


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|list_id|cijeli broj|Da |
|task_id|cijeli broj|Da |
|Datum|niz|Da |



### <a name="updatetask"></a>UpdateTask


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|revizije|cijeli broj|ne |
|Naslov|niz|ne |
|assignee_id|cijeli broj|ne |
|Dovršeno|Booleove vrijednosti|ne |
|recurrence_type|niz|ne |
|recurrence_count|cijeli broj|ne |
|due_date|niz|ne |
|starred|Booleove vrijednosti|ne |



### <a name="updatelist"></a>UpdateList


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|revizije|cijeli broj|ne |
|Naslov|niz|ne |



### <a name="updatesubtask"></a>UpdateSubtask


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|revizije|cijeli broj|ne |
|Naslov|niz|ne |
|Dovršeno|Booleove vrijednosti|ne |



### <a name="updatenote"></a>UpdateNote


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|revizije|cijeli broj|ne |
|sadržaj|niz|ne |



### <a name="updatereminder"></a>UpdateReminder


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Datum|niz|ne |
|revizije|cijeli broj|ne |


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)