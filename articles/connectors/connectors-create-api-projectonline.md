<properties
pageTitle="ProjectOnline | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. Project Online je Fleksibilno online rješenje za upravljanje PORTFELJEM projekta (PPM) i svakodnevni rad od Microsofta. Isporučeno putem sustava Office 365, Project Online omogućuje tvrtke ili ustanove da biste brzo započeli mogućnosti upravljanja snažna projekta za planiranje, prioritet i upravljanje projektima i ulaganjima u portfelje projekata – s gotovo bilo kojeg mjesta na gotovo bilo kojeg uređaja."
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

# <a name="get-started-with-the-projectonline-connector"></a>Početak rada s poveznik za ProjectOnline

Project Online je Fleksibilno online rješenje za upravljanje PORTFELJEM projekta (PPM) i svakodnevni rad od Microsofta. Isporučeno putem sustava Office 365, Project Online omogućuje tvrtke ili ustanove da biste brzo započeli mogućnosti upravljanja Napredna projekta za planiranje, prioritet i upravljanje projektima i ulaganjima u portfelje projekata – s gotovo bilo kojeg mjesta na gotovo bilo kojeg uređaja.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju. 

Mogli početi s radom sada stvaranjem logike aplikacije, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za ProjectOnline mogu se koristiti kao akciju; ima trigger(s). Sve poveznike podržava podatke u JSON i XML formatima. 

 Poveznik za ProjectOnline sadrži sljedeće akcije i/ili trigger(s) dostupne:

### <a name="projectonline-actions"></a>Akcije ProjectOnline
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Popisi projekata na project online web-mjestu|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Stvara novi projekt na project online web-mjestu|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Stvaranje novog zadatka u programu project vam|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Poslovni resursi stvara sustava project online web-mjestu|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Popisi objavljene zadataka u projektu|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Odjavljuje projekta s web-mjesta|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Prijava i objavljivanje i postojeći projekt na web-mjestu|
### <a name="projectonline-triggers"></a>ProjectOnline okidača
Možete poslušati za ove event(s):

|Pokretanje | Opis|
|--- | ---|
|Prilikom stvaranja novog projekta|Pokreće u tijeku prilikom svakog stvaranja novog projekta|
|Pri stvaranju novog resursa|Pokreće novu tijek stvaranja novog resursa|
|Pri stvaranju novog zadatka|Pokreće na tijek stvaranja novog zadatka|


## <a name="create-a-connection-to-projectonline"></a>Povezivanje s ProjectOnline
Da biste stvorili logike aplikacije ProjectOnline, najprije morate stvoriti **vezu** zatim unijeti detalje za sljedeća svojstva: 

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|Tokena|Da|Unesite vjerodajnice za ProjectOnline|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

## <a name="reference-for-projectonline"></a>Vodič za ProjectOnline
Odnosi se na verziju: 1.0

## <a name="onnewproject"></a>OnNewProject
Prilikom stvaranja novog projekta: pokreće u tijeku prilikom svakog stvaranja novog projekta 

```GET: /trigger/_api/ProjectData/Projects``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewresource"></a>OnNewResource
Pri stvaranju novog resursa: pokreće novi tijek stvaranja novog resursa 

```GET: /trigger/_api/ProjectData/Resources``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewtask"></a>OnNewTask
Kada se stvara novi zadatak: pokreće na tijek stvaranja novog zadatka 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="listprojects"></a>ListProjects
Popis projekata: popisa projekata na project online web-mjestu 

```GET: /_api/ProjectServer/Projects``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="createproject"></a>CreateProject
Stvara novi projekt: stvara novi projekt na project online web-mjestu 

```POST: /_api/ProjectServer/Projects``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|
|proj| |Da|tijelo|Ništa|Novi projekt da biste stvorili|

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


## <a name="createtask"></a>CreateTask
Stvara novi zadatak: Stvaranje novog zadatka u programu project vam 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|niz|Da|put|Ništa|Jedinstveni ID projekta da biste dodali zadatak|
|zadatak| |Da|tijelo|Ništa|Novi zadatak da biste dodali projekta|

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


## <a name="createresource"></a>CreateResource
Stvaranje novog resursa: stvara poslovni resursi sustava project online web-mjestu 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|
|resurs| |Da|tijelo|Ništa|Novi poslovni resurs da biste dodali projekta|

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


## <a name="listtasks"></a>ListTasks
Popisi zadataka: popisi objavljene zadataka u projektu 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|niz|Da|put|Ništa|Jedinstveni ID projekta za dohvaćanje zadataka|

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


## <a name="checkoutproject"></a>CheckoutProject
Odjava projekta: odjavljuje projekta s web-mjesta 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|niz|Da|put|Ništa|Jedinstveni ID projekta da biste dodali zadatak|

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


## <a name="publishproject"></a>PublishProject
Prijava i objavljivanje projekta: prijava i objavljivanje i postojeći projekt na web-mjestu 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|siteUrl|niz|Da|upit|Ništa|Korijenski url web-mjesta web-mjesta projekta (primjer: https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|niz|Da|put|Ništa|Jedinstveni ID za projekt prijava|

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

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="triggerproject"></a>TriggerProject


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ProjectStartDate|niz|ne |
|ProjectFinishDate|niz|ne |
|ProjectCreatedDate|niz|ne |
|ProjectId|niz|ne |
|ProjectModifiedDate|niz|ne |
|ProjectType|cijeli broj|ne |
|Nazivprojekta|niz|ne |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="triggerresource"></a>TriggerResource


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ResourceId|niz|ne |
|ResourceBaseCalendar|niz|ne |
|ResourceBookingType|cijeli broj|ne |
|ResourceCanLevel|Booleove vrijednosti|ne |
|ResourceCostPerUse|broj|ne |
|ResourceCreatedDate|niz|ne |
|ResourceEarliestAvailableFrom|niz|ne |
|ResourceEmail|niz|ne |
|ResourceInitials|niz|ne |
|ResourceIsActive|Booleove vrijednosti|ne |
|ResourceIsGeneric|Booleove vrijednosti|ne |
|ResourceLatestAvailableTo|niz|ne |
|ResourceModifiedDate|niz|ne |
|ResourceName|niz|ne |
|ResourceStatsuName|niz|ne |
|ResourceType|cijeli broj|ne |
|TypeDescription|niz|ne |
|TypeName|niz|ne |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="triggertask"></a>TriggerTask


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ProjectId|niz|ne |
|TaskId|niz|ne |
|Nazivprojekta|niz|ne |
|TaskName|niz|ne |
|TaskCreatedDate|niz|ne |
|TaskModifieddate|niz|ne |
|TaskStartDate|niz|ne |
|TaskFinishDate|niz|ne |
|TaskPriority|cijeli broj|ne |
|TaskIsActive|Booleove vrijednosti|ne |



### <a name="newproject"></a>NewProject


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Ime|niz|Da |
|Opis|niz|ne |
|Pokretanje|niz|ne |



### <a name="newreource"></a>NewReource


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Ime|niz|Da |
|IsBudget|Booleove vrijednosti|ne |
|IsGeneric|Booleove vrijednosti|ne |
|IsInactive|Booleove vrijednosti|ne |



### <a name="project"></a>Projekt


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ApprovedStart|niz|ne |
|ApprovedEnd|niz|ne |
|CheckedOutDate|niz|ne |
|CheckOutDescription|niz|ne |
|CheckOutId|niz|ne |
|DatumStvaranja|niz|ne |
|ID-a|niz|ne |
|IsCheckedOut|Booleove vrijednosti|ne |
|LastPublishedDate|niz|ne |
|LastSavedDate|niz|ne |
|OptimizerDecision|cijeli broj|ne |
|PlannerDecision|cijeli broj|ne |
|ProjectType|cijeli broj|ne |
|Ime|niz|ne |
|WinprojVersion|niz|ne |



### <a name="projectswrapper"></a>ProjectsWrapper


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="newtask"></a>NewTask


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Parametri|nije definirano|Da |



### <a name="taskparameters"></a>TaskParameters


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Ime|niz|Da |
|Bilješke|niz|ne |
|Pokretanje|niz|ne |
|Trajanje|niz|ne |



### <a name="enterpriseresource"></a>EnterpriseResource


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|CanLevel|Booleove vrijednosti|ne |
|Kod|niz|ne |
|CostAccrual|cijeli broj|ne |
|CostCenter|niz|ne |
|Stvorili|niz|ne |
|DefaultBookingType|cijeli broj|ne |
|E-pošte|niz|ne |
|ExternalId|niz|ne |
|Grupa|niz|ne |
|HireDate|niz|ne |
|ID-a|niz|ne |
|Inicijali|niz|ne |
|IsActive|Booleove vrijednosti|ne |
|IsBudget|Booleove vrijednosti|ne |
|IsCheckedOut|Booleove vrijednosti|ne |
|IsGeneric|Booleove vrijednosti|ne |
|IsTeam|Booleove vrijednosti|ne |
|MaterialLabel|niz|ne |
|Izmijenio|niz|ne |
|Ime|niz|ne |
|Phonetics|niz|ne |
|ResourceType|cijeli broj|ne |
|TerminationDate|niz|ne |



### <a name="taskswrapper"></a>TasksWrapper


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|vrijednost|polja|ne |



### <a name="task"></a>Zadatak


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Stvorili|niz|ne |
|Izmijenio|niz|ne |
|Pokretanje|niz|ne |
|Završi|niz|ne |
|Ime|niz|ne |
|ID-a|niz|ne |
|Prioritet|cijeli broj|ne |
|PostotakDovršenosti|cijeli broj|ne |
|Bilješke|niz|ne |
|Kontaktu|niz|ne |


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)