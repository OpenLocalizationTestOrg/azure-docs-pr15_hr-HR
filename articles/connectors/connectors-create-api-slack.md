<properties
pageTitle=" Korištenje zalihe poveznika u aplikacijama za logiku | Microsoft Azure"
description="Početak rada s Prazan hod poveznika u aplikacije sustava Microsoft Azure aplikacije servisa logike"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-slack-connector"></a>Početak rada s zalihe poveznika

Prazan hod je alat za komunikaciju tima, koji objedinjuje sve komunikacije tima u jednom postavite, trenutačno pretraživanja i dostupna kad god otvorite.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju.

Poveznik za zalihe omogućuje sljedeće:

* Koristite da biste sastavili logike aplikacije

Da biste dodali operaciju u aplikacijama logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Ćemo objasniti što okidača i akcija

Poveznik za zalihe koje se mogu koristiti kao akciju; Postoje bez okidača. Sve poveznike podržava podatke u JSON i XML formatima. 

 Poveznik za zalihe sadrži sljedeće akcije i/ili trigger(s) dostupne:

### <a name="slack-actions"></a>Zalihe akcije
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|Funkcije postMessage|Slanje poruke na određeni kanal.|
## <a name="create-a-connection-to-slack"></a>Povezivanje s Prazan hod
Da biste koristili poveznik za zalihe, koje najprije stvorite **vezu** , a zatim unijeti detalje za tih svojstava: 

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|Tokena|Da|Unesite vjerodajnice za zalihe|

Slijedite ove korake da biste prijavite se u prazan hod i dovršili konfiguraciju zalihe **veze** u aplikaciji logika:

1. Odaberite **Ponavljanje**
2. Odaberite **Učestalost** i unesite **Interval**
3. Odaberite **Dodaj akciju**  
![Konfiguriranje Prazan hod][1]  
4. U okvir za pretraživanje unesite Prazan hod i pričekajte za pretraživanje da biste se vratili sve stavke s Prazan hod u nazivu
5. Odaberite **Prazan hod - objave poruke**
6. Odaberite **prijavite se na Slack**:  
![Konfiguriranje Prazan hod][2]
7. Navedite zalihe vjerodajnice za prijavu u da biste autorizirali aplikacije    
![Konfiguriranje Prazan hod][3]  
8. Bit ćete preusmjereni na vaše tvrtke ili ustanove prijava stranice. **Autorizirajte** Prazan hod interakciju s aplikacijom logika:      
![Konfiguriranje Prazan hod][5] 
9. Nakon dovršetka autorizacija bit ćete preusmjereni na logiku aplikaciju da biste dovršili konfiguriranjem odjeljku **Prazan hod - se sve poruke** . Dodajte druge okidača i akcije koje su vam potrebne.  
![Konfiguriranje Prazan hod][6]
10. Spremanje rezultata rada tako da odaberete **spremiti** na traci izbornika iznad.


>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

## <a name="slack-rest-api-reference"></a>Zalihe referenca REST API-JA
#### <a name="this-documentation-is-for-version-10"></a>Ove dokumentacije namijenjen je verzija: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Slanje poruke na određeni kanal.
**```POST: /chat.postMessage```** 



| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|kanal|niz|Da|upit|Ništa|Kanal, privatnoj grupi ili kanal za razmjenu izravnih poruka da biste poslali poruku. Može biti naziv (ex: #general) ili kodiranih ID-a|
|tekst|niz|Da|upit|Ništa|Tekst poruku koju želite poslati. Mogućnosti oblikovanja potražite u članku https://api.slack.com/docs/formatting.|
|korisničko ime|niz|ne|upit|Ništa|Naziv na robot|
|as_user|Booleove vrijednosti|ne|upit|Ništa|Prenesite true da biste objavili poruku kao korisnika čija je autentičnost provjerena umjesto kao na robot|
|Raščlanjivanje|niz|ne|upit|Ništa|Promijenite kako se postupa poruke. Detalje potražite u članku https://api.slack.com/docs/formatting.|
|link_names|cijeli broj|ne|upit|Ništa|Pronalaženje i povezivanje kanala imena i adresa.|
|unfurl_links|Booleove vrijednosti|ne|upit|Ništa|Prenesite TRUE Omogući unfurling prvenstveno temeljenom na tekstu sadržaja.|
|unfurl_media|Booleove vrijednosti|ne|upit|Ništa|Prenesite false da biste onemogućili unfurling od medijskog sadržaja.|
|icon_url|niz|ne|upit|Ništa|URL za sliku koji se koristi kao ikonu za ovu poruku|
|icon_emoji|niz|ne|upit|Ništa|Emoji za korištenje kao ikona za ovu poruku|


### <a name="here-are-the-possible-responses"></a>Evo nekoliko mogućih odgovora:

|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|408|Prekoračenje vremena zahtjeva|
|429|Previše zahtjeva|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|503|Prazan hod servis nije dostupan|
|504|Prekoračenje vremena za pristupnik|
|Zadani|Nije uspjelo.|
------



## <a name="object-definitions"></a>Definition(s) objekta: 

 **Poruka**: poruku na servisu Yammer

Obavezna svojstva za poruku:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|ID-a|cijeli broj|
|content_excerpt|niz|
|sender_id|cijeli broj|
|replied_to_id|cijeli broj|
|created_at|niz|
|network_id|cijeli broj|
|message_type|niz|
|sender_type|niz|
|URL-a|niz|
|web_url|niz|
|group_id|cijeli broj|
|tijelo|nije definirano|
|thread_id|cijeli broj|
|direct_message|Booleove vrijednosti|
|client_type|niz|
|client_url|niz|
|jezik|niz|
|notified_user_ids|polja|
|izjave o zaštiti privatnosti|niz|
|liked_by|nije definirano|
|system_message|Booleove vrijednosti|



 **PostOperationRequest**: predstavlja objavu zahtjev za Yammer poveznik da biste objavili na servisu yammer

Potrebna svojstva PostOperationRequest:

tijelo

**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|tijelo|niz|
|group_id|cijeli broj|
|replied_to_id|cijeli broj|
|direct_to_id|cijeli broj|
|emitiranje|Booleove vrijednosti|
|tema1|niz|
|tema2|niz|
|topic3|niz|
|topic4|niz|
|topic5|niz|
|topic6|niz|
|topic7|niz|
|topic8|niz|
|topic9|niz|
|topic10|niz|
|topic11|niz|
|topic12|niz|
|topic13|niz|
|topic14|niz|
|topic15|niz|
|topic16|niz|
|topic17|niz|
|topic18|niz|
|topic19|niz|
|topic20|niz|



 **MessageList**: popis poruka

Potrebna svojstva MessageList:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|poruka|polja|



 **MessageBody**: tijelo poruke

Potrebna svojstva MessageBody:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|raščlaniti|niz|
|običan|niz|
|Obogaćeni|niz|



 **LikedBy**: sviđa po

Potrebna svojstva LikedBy:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|Count|cijeli broj|
|Nazivi|polja|



 **YammmerEntity**: sviđa po

Potrebna svojstva YammmerEntity:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|Vrsta|niz|
|ID-a|cijeli broj|
|full_name|niz|


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Definition(s) objekta: 

 **WebResultModel**: rezultati pretraživanja web Bing

Potrebna svojstva WebResultModel:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|Naslov|niz|
|Opis|niz|
|DisplayUrl|niz|
|ID-a|niz|
|FullUrl|niz|



 **VideoResultModel**: Bing videozapisa rezultata

Potrebna svojstva VideoResultModel:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|Naslov|niz|
|DisplayUrl|niz|
|ID-a|niz|
|MediaUrl|niz|
|Prilikom izvođenja|cijeli broj|
|Minijatura|nije definirano|



 **ThumbnailModel**: minijaturu svojstva multimedijske elementa

Potrebna svojstva ThumbnailModel:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|MediaUrl|niz|
|ContentType|niz|
|Širina|cijeli broj|
|Visina|cijeli broj|
|FileSize|cijeli broj|



 **ImageResultModel**: rezultati pretraživanja slika putem tražilice Bing

Potrebna svojstva ImageResultModel:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|Naslov|niz|
|DisplayUrl|niz|
|ID-a|niz|
|MediaUrl|niz|
|SourceUrl|niz|
|Minijatura|nije definirano|



 **NewsResultModel**: rezultati pretraživanja za Bing novosti

Potrebna svojstva NewsResultModel:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|Naslov|niz|
|Opis|niz|
|DisplayUrl|niz|
|ID-a|niz|
|Izvor|niz|
|Datum|niz|



 **SpellResultModel**: Bing pravopis prijedloge rezultata

Potrebna svojstva SpellResultModel:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|ID-a|niz|
|Vrijednost|niz|



 **RelatedSearchResultModel**: Bing povezane rezultate pretraživanja

Potrebna svojstva RelatedSearchResultModel:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|Naslov|niz|
|ID-a|niz|
|BingUrl|niz|



 **CompositeSearchResultModel**: Bing složeni rezultata

Potrebna svojstva CompositeSearchResultModel:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|WebResultsTotal|cijeli broj|
|ImageResultsTotal|cijeli broj|
|VideoResultsTotal|cijeli broj|
|NewsResultsTotal|cijeli broj|
|SpellSuggestionsTotal|cijeli broj|
|WebResults|polja|
|ImageResults|polja|
|VideoResults|polja|
|NewsResults|polja|
|SpellSuggestionResults|polja|
|RelatedSearchResults|polja|


## <a name="object-definitions"></a>Definition(s) objekta: 

 **PostOperationResponse**: predstavlja odgovor objavu operacije Prazan hod poveznik za objavljivanje na prazan hod

Potrebna svojstva PostOperationResponse:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|ok|Booleove vrijednosti|
|kanal|niz|
|TS|niz|
|poruka|nije definirano|
|Pogreška|niz|



 **MessageItem**: kanala poruke.

Potrebna svojstva MessageItem:


Nijedna od svojstava nije potrebna. 


**Sva svojstva**: 


| Ime | Vrsta podataka |
|---|---|
|tekst|niz|
|ID-a|niz|
|korisnik|niz|
|stvorili|cijeli broj|
|is_user izbrisati|Booleove vrijednosti|


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
