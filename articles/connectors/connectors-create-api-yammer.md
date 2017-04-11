<properties
pageTitle="Dodavanje poveznik servisa Yammer u aplikacijama za logiku | Microsoft Azure"
description="Pregled poveznik servisa Yammer s parametrima REST API-JA"
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

# <a name="get-started-with-the-yammer-connector"></a>Početak rada s poveznik za Yammer

Povezivanje sa servisom Yammer pristup razgovore u mrežu svoje tvrtke.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju.

Pomoću servisa Yammer, možete učiniti sljedeće:

- Sastavljanje vaše tvrtke tijek na temelju podataka zatražite od servisa Yammer. 
- Koristi se pokreće za kada postoje nove poruke u grupu ili sažetak sadržaja na sljedeći način.
- Korištenje akcija da biste poslali poruku, sve poruke i više. Ove se radnje dobiti odgovor, a zatim unesite dostupne za ostale akcije izlaz. Ako, na primjer, kada se pojavi novu poruku, možete poslati poruku e-pošte pomoću sustava Office 365.

Da biste dodali operaciju u aplikacijama logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija
Yammer obuhvaća sljedeće okidača i akcija. 

Pokretanje | Akcija
--- | ---
<ul><li>Kada je nova poruka u grupi</li><li>Kada postoje nove poruke u moje sljedeće sažetka sadržaja</li></ul>| <ul><li>Se sve poruke</li><li>Uzima poruke u grupu</li><li>Uzima poruke iz moje sljedeće sažetka sadržaja</li><li>Objava poruke</li><li>Kada je nova poruka u grupi</li><li>Kada postoje nove poruke u moje sljedeće sažetka sadržaja</li></ul>

Sve poveznike podržava podatke u JSON i XML formatima. 

## <a name="create-a-connection-to-yammer"></a>Stvaranje veze na servisu Yammer
Da biste koristili poveznik za Yammer, koje najprije stvorite **vezu** , a zatim unijeti detalje za tih svojstava: 

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|Tokena|Da|Unesite vjerodajnice za Yammer|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

## <a name="yammer-rest-api-reference"></a>Referenca REST API-JA na servisu Yammer
Ove dokumentacije namijenjen je verzija: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Se sve poruke javno prijavljenog korisnika mreža za Yammer
Odgovara "Svi" razgovori u web-sučelja servisa Yammer.  
```GET: /messages.json```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|older_then|cijeli broj|ne|upit|Ništa|Vraća poruke starije od ID poruke koja je navedena kao numeričkom nizu. To je korisno za paginating poruke. Na primjer, ako koji trenutno pregledavate 20 poruke, a najstarijeg broj 2912, nije moguće dodati "? older_than = 2912″ na vaš zahtjev za se 20 poruke prije no što oni se prikazuju.|
|newer_then|cijeli broj|ne|upit|Ništa|Vraća novija od ID poruke koja je navedena kao numeričkom nizu poruke. To se mora koristiti prilikom provjere za nove poruke. Ako gledate poruke, a na najnoviju poruku da funkcija Vrati 3516, možete poslati zahtjev s parametrom "? newer_than = 3516″ da biste bili sigurni da ne dobivate duplicirane kopije poruka već na stranici.|
|ograničenje|cijeli broj|ne|upit|Ništa|Prikazali samo određeni broj poruka.|
|stranica|cijeli broj|ne|upit|Ništa|Pronađite stranicu navedenu. Ako podatke veća od ograničenja, to polje možete koristiti da biste pristupili sljedeće stranice|


### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|400|Neispravan zahtjev|
|408|Prekoračenje vremena zahtjeva|
|429|Previše zahtjeva|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|503|Servis nije dostupan za Yammer|
|504|Prekoračenje vremena za pristupnik|
|Zadani|Nije uspjelo.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Slanje poruke u grupu ili cijela tvrtka sažetka sadržaja
Ako je navedena ID grupe poruka će biti objavljena navedena grupa još će će se objaviti u svim sažetka sadržaja tvrtke.    
```POST: /messages.json``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|unos| |Da|tijelo|Ništa|Zahtjev za porukom objavu|


### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|201|Stvorili|



## <a name="object-definitions"></a>Definicija objekta

#### <a name="message-yammer-message"></a>Poruka: Poruka servisa Yammer

| Ime | Vrsta podataka | Obavezno |
|---|---| --- | 
|ID-a|cijeli broj|ne|
|content_excerpt|niz|ne|
|sender_id|cijeli broj|ne|
|replied_to_id|cijeli broj|ne|
|created_at|niz|ne|
|network_id|cijeli broj|ne|
|message_type|niz|ne|
|sender_type|niz|ne|
|URL-a|niz|ne|
|web_url|niz|ne|
|group_id|cijeli broj|ne|
|tijelo|nije definirano|ne|
|thread_id|cijeli broj|ne|
|direct_message|Booleove vrijednosti|ne|
|client_type|niz|ne|
|client_url|niz|ne|
|jezik|niz|ne|
|notified_user_ids|polja|ne|
|izjave o zaštiti privatnosti|niz|ne|
|liked_by|nije definirano|ne|
|system_message|Booleove vrijednosti|ne|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Predstavlja objavu zahtjev za Yammer poveznik da biste objavili na servisu yammer

| Ime | Vrsta podataka | Obavezno |
|---|---| --- | 
|tijelo|niz|Da|
|group_id|cijeli broj|ne|
|replied_to_id|cijeli broj|ne|
|direct_to_id|cijeli broj|ne|
|emitiranje|Booleove vrijednosti|ne|
|tema1|niz|ne|
|tema2|niz|ne|
|topic3|niz|ne|
|topic4|niz|ne|
|topic5|niz|ne|
|topic6|niz|ne|
|topic7|niz|ne|
|topic8|niz|ne|
|topic9|niz|ne|
|topic10|niz|ne|
|topic11|niz|ne|
|topic12|niz|ne|
|topic13|niz|ne|
|topic14|niz|ne|
|topic15|niz|ne|
|topic16|niz|ne|
|topic17|niz|ne|
|topic18|niz|ne|
|topic19|niz|ne|
|topic20|niz|ne|

#### <a name="messagelist-list-of-messages"></a>MessageList: Popis poruka

| Ime | Vrsta podataka | Obavezno |
|---|---| --- | 
|poruka|polja|ne|


#### <a name="messagebody-message-body"></a>MessageBody: Tijelo poruke

| Ime | Vrsta podataka | Obavezno |
|---|---| --- | 
|raščlaniti|niz|ne|
|običan|niz|ne|
|Obogaćeni|niz|ne|

#### <a name="likedby-liked-by"></a>LikedBy: Sviđa po

| Ime | Vrsta podataka | Obavezno |
|---|---| --- | 
|Count|cijeli broj|ne|
|Nazivi|polja|ne|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Sviđa po

| Ime | Vrsta podataka | Obavezno |
|---|---| --- | 
|Vrsta|niz|ne|
|ID-a|cijeli broj|ne|
|full_name|niz|ne|


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
