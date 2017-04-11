<properties
    pageTitle="Dodavanje poveznik za Facebook u aplikacijama za logiku | Microsoft Azure"
    description="Pregled poveznik za Facebook s parametrima REST API-JA"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>Početak rada s poveznik za Facebook
Povezivanje sa servisom Facebook i objavite na vremensku crtu, pronađite stranicu sažetka sadržaja i više. 

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju.


Sa servisom Facebook, možete učiniti sljedeće:

- Sastavljanje vaše tvrtke tijek na temelju podataka zatražite od Facebook. 
- Koristite okidač primitku nove objave.
- Korištenje akcije koje se objavljuju na vremensku traku se na stranicu sažetak sadržaja i drugo. Ove se radnje dobiti odgovor, a zatim unesite dostupne za ostale akcije izlaz. Ako, na primjer, kada je novu objavu na vremenskoj traci, možete poduzeti taj unos i automatske na Twitter sažetka sadržaja. 



Da biste dodali operaciju u aplikacijama logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija
Poveznik za Facebook obuhvaća sljedeće okidača i akcije. 

| Okidača | Akcija|
| --- | --- |
| <ul><li>Kada postoje nove objave na moj vremenske trake</li></ul> |<ul><li>Početak sažetak sadržaja s Moje vremenske trake</li><li>Objavljivanje Moje vremenske trake</li><li>Kada postoje nove objave na moj vremenske trake</li><li>Početak stranice sažetka sadržaja</li><li>Početak korisnika vremenske trake</li><li>Objavljivanje stranica</li></ul>

Sve poveznike podržava podatke u JSON i XML formatima.

## <a name="create-a-connection-to-facebook"></a>Stvaranje veze sa servisom Facebook
Kada dodate ovaj poveznik aplikacija logike, morate Autorizirajte logike aplikacije za povezivanje s vašeg Facebook.

1. Prijavite se na račun za Facebook
2. Odaberite **autorizacija**i Dopusti logike aplikacije da bi povezao i koristiti vaš Facebook. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] U ostale aplikacije logike možete koristiti tu istu vezu servisa Facebook.

## <a name="swagger-rest-api-reference"></a>Referenca swagger REST API-JA
Odnosi se na verziju: 1.0.

### <a name="get-feed-from-my-timeline"></a>Početak sažetak sadržaja s Moje vremenske trake
Može vidjeti sažetke sadržaja s vremenske trake prijavljenog korisnika.  
```GET: /me/feed```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|polja|niz|ne|upit|Ništa |Navedite polja koje će se prikazati. Primjer (id, naziv, slika).|
|ograničenje|cijeli broj|ne|upit| Ništa|Maksimalni broj objava koje će se moguće dohvatiti|
|s|niz|ne|upit| Ništa|Ograničavanje popisa objava samo one s lokacijom priložene.|
|Filtar|niz|ne|upit| Ništa|Dohvaćanje samo objave koje odgovaraju određenom strujanje filtra.|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


### <a name="post-to-my-timeline"></a>Objavljivanje Moje vremenske trake
Poruka o stanju vremensku crtu prijavljenog korisnika objavu.  
```POST: /me/feed```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Objava|niz |Da|tijelo|Ništa |Nova poruka će se objaviti|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Kada postoje nove objave na moj vremenske trake
Pokreće novu tijek kada novu objavu na vremenskoj traci prijavljenog korisnika.  
```GET: /trigger/me/feed```

Nema parametara. 

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


### <a name="get-page-feed"></a>Početak stranice sažetka sadržaja
Objave zatražite od sažetak sadržaja na određenu stranicu.  
```GET: /{pageId}/feed```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|pageId|niz|Da|put| Ništa|ID stranice iz kojeg objave moraju biti dohvaćeni.|
|ograničenje|cijeli broj|ne|upit| Ništa|Maksimalni broj objava koje će se moguće dohvatiti|
|include_hidden|Booleove vrijednosti|ne|upit|Ništa |Želite li da biste obuhvatili sve objave koje su bili skriveni stranica ili ne|
|polja|niz|ne|upit|Ništa |Navedite polja koje će se prikazati. Primjer (id, naziv, slika).|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


### <a name="get-user-timeline"></a>Početak korisnika vremenske trake
Objave zatražite od korisnika vremenske crte.  
```GET: /{userId}/feed```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID korisnika|niz|Da|put|Ništa |ID korisnika čije vremenske trake moraju biti dohvaćeni.|
|ograničenje|cijeli broj|ne|upit|Ništa |Maksimalni broj objava koje će se moguće dohvatiti|
|s|niz|ne|upit|Ništa |Ograničavanje popisa objava samo one s lokacijom priložene.|
|Filtar|niz|ne|upit| Ništa|Dohvaćanje samo objave koje odgovaraju određenom strujanje filtra.|
|polja|niz|ne|upit| Ništa|Navedite polja koje će se prikazati. Primjer (id, naziv, slika).|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


### <a name="post-to-page"></a>Objavljivanje stranica
Objava poruke koja će se stranica servisa Facebook kao prijavljenog korisnika.  
```POST: /{pageId}/feed```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|pageId|niz|Da|put|Ništa |ID stranice na kojoj ćete objaviti.|
|Objava|Mnoge |Da|tijelo|Ništa |Nova poruka će se objaviti.|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="object-definitions"></a>Definicija objekta

#### <a name="getfeedresponse"></a>GetFeedResponse

|Naziv svojstva | Vrsta podataka | Obavezno|
|---|---|---|
|podataka|polja|ne|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|podataka|polja|ne|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Jednu stavku u profilu je sažetak sadržaja
Profil može biti korisnika, stranica, aplikacije ili grupu. 

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|ne|
|admin_creator|polja|ne|
|Opis|niz|ne|
|created_time|niz|ne|
|Opis|niz|ne|
|feed_targeting|nije definirano|ne|
|iz|nije definirano|ne|
|ikona|niz|ne|
|is_hidden|Booleove vrijednosti|ne|
|is_published|Booleove vrijednosti|ne|
|veza|niz|ne|
|poruka|niz|ne|
|ime|niz|ne|
|object_id|niz|ne|
|Slika|niz|ne|
|postavite|nije definirano|ne|
|izjave o zaštiti privatnosti|nije definirano|ne|
|Svojstva|polja|ne|
|Izvor|niz|ne|
|status_type|niz|ne|
|priče|niz|ne|
|Određivanje|nije definirano|ne|
|Da biste|polja|ne|
|Vrsta|niz|ne|
|updated_time|niz|ne|
|with_tags|nije definirano|ne|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Jednu stavku u profilu je sažetak sadržaja
Profil može biti korisnika, stranica, aplikacije ili grupu.

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|ne|
|created_time|niz|ne|
|iz|nije definirano|ne|
|poruka|niz|ne|
|Vrsta|niz|ne|

#### <a name="adminitem"></a>AdminItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|ne|
|veza|niz|ne|

#### <a name="propertyitem"></a>PropertyItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ime|niz|ne|
|tekst|niz|ne|
|href|niz|ne|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|poruka|niz|Da|
|veza|niz|ne|
|Slika|niz|ne|
|ime|niz|ne|
|Opis|niz|ne|
|Opis|niz|ne|
|postavite|niz|ne|
|oznaka|niz|ne|
|izjave o zaštiti privatnosti|nije definirano|ne|
|object_attachment|niz|ne|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|poruka|niz|Da|
|veza|niz|ne|
|Slika|niz|ne|
|ime|niz|ne|
|Opis|niz|ne|
|Opis|niz|ne|
|Akcija|polja|ne|
|postavite|niz|ne|
|oznaka|niz|ne|
|object_attachment|niz|ne|
|Određivanje|nije definirano|ne|
|feed_targeting|nije definirano|ne|
|objavljivanja|Booleove vrijednosti|ne|
|scheduled_publish_time|niz|ne|
|backdated_time|niz|ne|
|backdated_time_granularity|niz|ne|
|child_attachments|polja|ne|
|multi_share_end_card|Booleove vrijednosti|ne|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|ne|

#### <a name="profilecollection"></a>ProfileCollection

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|podataka|polja|ne|

#### <a name="useritem"></a>UserItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|ne|
|Osobno_ime|niz|ne|
|Pre_zime|niz|ne|
|ime|niz|ne|
|spol|niz|ne|
|o|niz|ne|

#### <a name="actionitem"></a>ActionItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ime|niz|ne|
|veza|niz|ne|

#### <a name="targetitem"></a>TargetItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|države|polja|ne|
|regionalne sheme|polja|ne|
|područja|polja|ne|
|gradova|polja|ne|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Objekt koji određuje novosti sažetka sadržaja ciljanja za ovaj članak
Svi sudionici te grupe, veća je vjerojatnost da biste vidjeli objave, drugima manje vjerojatno. Odnosi se na stranice samo.

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|države|polja|ne|
|područja|polja|ne|
|gradova|polja|ne|
|age_min|cijeli broj|ne|
|age_max|cijeli broj|ne|
|genders|polja|ne|
|relationship_statuses|polja|ne|
|interested_in|polja|ne|
|college_years|polja|ne|
|interese|polja|ne|
|relevant_until|cijeli broj|ne|
|education_statuses|polja|ne|
|regionalne sheme|polja|ne|

#### <a name="placeitem"></a>PlaceItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|ne|
|ime|niz|ne|
|overall_rating|broj|ne|
|mjesto|nije definirano|ne|

#### <a name="locationitem"></a>LocationItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|Grad|niz|ne|
|države|niz|ne|
|zemljopisnu širinu|broj|ne|
|located_in|niz|ne|
|Dužina|broj|ne|
|ime|niz|ne|
|regija|niz|ne|
|Stanje|niz|ne|
|Ulica|niz|ne|
|Poštanski broj|niz|ne|

#### <a name="privacyitem"></a>PrivacyItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|Opis|niz|ne|
|vrijednost|niz|Da|
|Dopusti|niz|ne|
|Nemoj dopustiti|niz|ne|
|prijateljima|niz|ne|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|veza|niz|ne|
|Slika|niz|ne|
|image_hash|niz|ne|
|ime|niz|ne|
|Opis|niz|ne|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|URL-a|niz|Da|
|Opis|niz|ne|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|Da|
|post_id|niz|Da|

#### <a name="postvideorequest"></a>PostVideoRequest

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|videoData|niz|Da|
|Opis|niz|Da|
|Naslov|niz|Da|
|uploadedVideoName|niz|ne|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|podataka|nije definirano|Da|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|URL-a|niz|Da|
|is_silhouette|Booleove vrijednosti|Da|
|Visina|niz|ne|
|Širina|niz|ne|

#### <a name="geteventresponse"></a>GetEventResponse

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|podataka|polja|Da|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|Da|
|ime|niz|Da|
|start_time|niz|ne|
|end_time|niz|ne|
|Vremenska zona|niz|ne|
|mjesto|niz|ne|
|Opis|niz|ne|
|ticket_uri|niz|ne|
|rsvp_status|niz|Da|


## <a name="next-steps"></a>Daljnji koraci

[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).
