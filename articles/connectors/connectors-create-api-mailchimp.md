<properties
pageTitle="MailChimp | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. MailChimp je SaaS servis koji omogućuje tvrtke za upravljanje i automatizirati marketinške aktivnosti e-pošte, uključujući i slanje marketinške poruke e-pošte, automatiziranog poruke i ciljano kampanja."
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

# <a name="get-started-with-the-mailchimp-connector"></a>Početak rada s MailChimp poveznika

MailChimp je SaaS servis koji omogućuje tvrtke za upravljanje i automatizirati marketinške aktivnosti e-pošte, uključujući i slanje marketinške poruke e-pošte, automatiziranog poruke i ciljano kampanja.


>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju.

Mogli početi s radom sada stvaranjem logike aplikacije, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za MailChimp mogu se koristiti kao akciju; ima trigger(s). Sve poveznike podržava podatke u JSON i XML formatima.

 Poveznik za MailChimp sadrži sljedeće akcije i/ili trigger(s) dostupne:

### <a name="mailchimp-actions"></a>MailChimp akcije
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Stvaranje nove kampanje ovisno o vrsti kampanje, popis primatelja i kampanje postavke (predmet, naslov, from_name i reply_to)|
|[newlist](connectors-create-api-mailchimp.md#newlist)|Stvaranje novog popisa na vašem računu MailChimp|
|[addmember](connectors-create-api-mailchimp.md#addmember)|Dodavanje ili ažuriranje popisa članova|
|[removemember](connectors-create-api-mailchimp.md#removemember)|Da biste izbrisali člana s popisa.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Ažurirajte podatke o određenom popisu člana|
### <a name="mailchimp-triggers"></a>MailChimp okidača
Možete poslušati za ove event(s):

|Pokretanje | Opis|
|--- | ---|
|Kada se člana je dodana na popis|Pokreće tijek rada kad novog člana dodan na popis|
|Pri stvaranju novog popisa|Pokreće tijek rada prilikom stvaranja novog popisa|


## <a name="create-a-connection-to-mailchimp"></a>Povezivanje s MailChimp
Da biste stvorili logike aplikacije MailChimp, najprije morate stvoriti **vezu** zatim unijeti detalje za sljedeća svojstva:

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|Tokena|Da|Unesite vjerodajnice MailChimp|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

## <a name="reference-for-mailchimp"></a>Vodič za MailChimp
Odnosi se na verziju: 1.0

## <a name="newcampaign"></a>newcampaign
Nove kampanje: Stvaranje nove kampanje ovisno o vrsti kampanje, popis primatelja i kampanje postavke (predmet, naslov, from_name i reply_to)

```POST: /campaigns```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|newCampaignRequest| |Da|tijelo|Ništa|JSON objekt koji želite poslati u tijelu s novi zahtjev za parametre kampanje|

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


## <a name="newlist"></a>newlist
Novi popis: Stvaranje novog popisa na vašem računu MailChimp

```POST: /lists```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|newListRequest| |Da|tijelo|Ništa|JSON objekt koji želite poslati u tijelu s novi zahtjev za parametre kampanje|

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


## <a name="addmember"></a>addmember
Dodavanje člana popis: Dodavanje i ažuriranje popisa članova

```POST: /lists/{list_id}/members```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|niz|Da|put|Ništa|Jedinstveni id za popis|
|newMemberInList| |Da|tijelo|Ništa|JSON objekt koji želite poslati u tijelu novim podacima člana|

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


## <a name="removemember"></a>removemember
Uklanjanje člana iz popisa: Brisanje člana s popisa.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|niz|Da|put|Ništa|Jedinstveni id za popis|
|member_email|niz|Da|put|Ništa|Adresu e-pošte da biste izbrisali člana|

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


## <a name="updatemember"></a>updatemember
Ažuriranje podataka o člana: Ažurirajte podatke o određenom popisu člana

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|niz|Da|put|Ništa|Jedinstveni id za popis|
|member_email|niz|Da|put|Ništa|Na jedinstveni adresu e-pošte svakog člana za ažuriranje|
|updateMemberInListRequest| |Da|tijelo|Ništa|JSON objekt koji želite poslati u tijelu podacima ažurirane člana|

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


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Kada se člana je dodana na popis: pokrene tijek rada kad novog člana dodan na popis

```GET: /trigger/lists/{list_id}/members```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|list_id|niz|Da|put|Ništa|Jedinstveni id za popis|

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


## <a name="oncreatelist"></a>OnCreateList
Pri stvaranju novog popisa: pokrene tijek rada prilikom stvaranja novog popisa

```GET: /trigger/lists```

Nema parametara za ovaj poziv
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


## <a name="object-definitions"></a>Definicija objekta

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Vrsta|niz|Da |
|Primatelji|nije definirano|Da |
|postavke|nije definirano|Da |
|variate_settings|nije definirano|ne |
|Praćenje|nije definirano|ne |
|rss_opts|nije definirano|ne |
|social_card|nije definirano|ne |



### <a name="recipient"></a>Primatelj


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|list_id|niz|Da |
|segment_opts|nije definirano|ne |



### <a name="settings"></a>Postavke


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|subject_line|niz|Da |
|Naslov|niz|ne |
|from_name|niz|Da |
|reply_to|niz|Da |
|use_conversation|Booleove vrijednosti|ne |
|to_name|niz|ne |
|folder_id|cijeli broj|ne |
|autentičnost|Booleove vrijednosti|ne |
|auto_footer|Booleove vrijednosti|ne |
|inline_css|Booleove vrijednosti|ne |
|auto_tweet|Booleove vrijednosti|ne |
|auto_fb_post|polja|ne |
|fb_comments|Booleove vrijednosti|ne |



### <a name="variatesettings"></a>Variate_Settings


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|winner_criteria|niz|ne |
|wait_time|cijeli broj|ne |
|test_size|cijeli broj|ne |
|subject_lines|polja|ne |
|send_times|polja|ne |
|from_names|polja|ne |
|reply_to_addresses|polja|ne |



### <a name="tracking"></a>Praćenje


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Otvorit će se|Booleove vrijednosti|ne |
|html_clicks|Booleove vrijednosti|ne |
|text_clicks|Booleove vrijednosti|ne |
|goal_tracking|Booleove vrijednosti|ne |
|ecomm360|Booleove vrijednosti|ne |
|google_analytics|niz|ne |
|clicktale|niz|ne |
|Salesforce|nije definirano|ne |
|highrise|nije definirano|ne |
|capsule|nije definirano|ne |



### <a name="rssopts"></a>RSS_Opts


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|feed_url|niz|ne |
|FREQUENCY|niz|ne |
|constrain_rss_img|niz|ne |
|Raspored|nije definirano|ne |



### <a name="socialcard"></a>Social_Card


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|image_url|niz|ne |
|Opis|niz|ne |
|Naslov|niz|ne |



### <a name="segmentopts"></a>Segment_Opts


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|saved_segment_id|cijeli broj|ne |
|MATCH|niz|ne |



### <a name="salesforce"></a>Salesforce


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|kampanje|Booleove vrijednosti|ne |
|bilješke|Booleove vrijednosti|ne |



### <a name="highrise"></a>Highrise


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|kampanje|Booleove vrijednosti|ne |
|bilješke|Booleove vrijednosti|ne |



### <a name="capsule"></a>Capsule


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|bilješke|Booleove vrijednosti|ne |



### <a name="schedule"></a>Raspored


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|sat|cijeli broj|ne |
|daily_send|nije definirano|ne |
|weekly_send_day|niz|ne |
|monthly_send_date|broj|ne |



### <a name="dailysend"></a>Daily_Send


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|nedjelja|Booleove vrijednosti|ne |
|ponedjeljak|Booleove vrijednosti|ne |
|utorak|Booleove vrijednosti|ne |
|srijeda|Booleove vrijednosti|ne |
|Četvrtak|Booleove vrijednosti|ne |
|petak|Booleove vrijednosti|ne |
|subota|Booleove vrijednosti|ne |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|ne |
|Vrsta|niz|ne |
|create_time|niz|ne |
|archive_url|niz|ne |
|status|niz|ne |
|emails_sent|cijeli broj|ne |
|send_time|niz|ne |
|CONTENT_TYPE|niz|ne |
|Primatelj|polja|ne |
|postavke|nije definirano|ne |
|variate_settings|nije definirano|ne |
|Praćenje|nije definirano|ne |
|rss_opts|nije definirano|ne |
|ab_split_opts|nije definirano|ne |
|social_card|nije definirano|ne |
|report_summary|nije definirano|ne |
|delivery_status|nije definirano|ne |
|_links|polja|ne |



### <a name="absplitopts"></a>AB_Split_Opts


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|split_test|niz|ne |
|pick_winner|niz|ne |
|wait_units|niz|ne |
|wait_time|cijeli broj|ne |
|split_size|cijeli broj|ne |
|from_name_a|niz|ne |
|from_name_b|niz|ne |
|reply_email_a|niz|ne |
|reply_email_b|niz|ne |
|subject_a|niz|ne |
|subject_b|niz|ne |
|send_time_a|niz|ne |
|send_time_b|niz|ne |
|send_time_winner|niz|ne |



### <a name="reportsummary"></a>Report_Summary


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Otvorit će se|cijeli broj|ne |
|unique_opens|cijeli broj|ne |
|open_rate|broj|ne |
|klikova|cijeli broj|ne |
|subscriber_clicks|broj|ne |
|click_rate|broj|ne |



### <a name="deliverystatus"></a>Delivery_Status


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|omogućeno|Booleove vrijednosti|ne |
|can_cancel|Booleove vrijednosti|ne |
|status|niz|ne |
|emails_sent|cijeli broj|ne |
|emails_canceled|cijeli broj|ne |



### <a name="link"></a>Veza


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|rel|niz|ne |
|href|niz|ne |
|način|niz|ne |
|targetSchema|niz|ne |
|shema|niz|ne |



### <a name="newlistrequest"></a>NewListRequest


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ime|niz|Da |
|kontaktu|nije definirano|Da |
|permission_reminder|niz|Da |
|use_archive_bar|Booleove vrijednosti|ne |
|campaign_defaults|nije definirano|Da |
|notify_on_subscribe|niz|ne |
|notify_on_unsubscribe|niz|ne |
|email_type_option|Booleove vrijednosti|Da |
|Vidljivost|niz|ne |



### <a name="contact"></a>Kontakt


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|tvrtke|niz|Da |
|Adresa1|niz|Da |
|Adresa2|niz|ne |
|Grad|niz|Da |
|Stanje|niz|Da |
|Poštanski broj|niz|Da |
|države|niz|Da |
|telefon|niz|Da |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|from_name|niz|Da |
|from_email|niz|Da |
|Predmet|niz|ne |
|jezik|niz|Da |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|Da |
|ime|niz|Da |
|kontaktu|nije definirano|Da |
|permission_reminder|niz|Da |
|use_archive_bar|Booleove vrijednosti|ne |
|campaign_defaults|nije definirano|Da |
|notify_on_subscribe|niz|ne |
|notify_on_unsubscribe|niz|ne |
|date_created|niz|ne |
|list_rating|cijeli broj|ne |
|email_type_option|Booleove vrijednosti|Da |
|subscribe_url_short|niz|ne |
|subscribe_url_long|niz|ne |
|beamer_address|niz|ne |
|Vidljivost|niz|ne |
|moduli|polja|ne |
|Statistika|nije definirano|ne |
|_links|polja|ne |



### <a name="stats"></a>Statistika


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|member_count|cijeli broj|ne |
|unsubscribe_count|cijeli broj|ne |
|cleaned_count|cijeli broj|ne |
|member_count_since_send|cijeli broj|ne |
|unsubscribe_count_since_send|cijeli broj|ne |
|cleaned_count_since_send|cijeli broj|ne |
|campaign_count|cijeli broj|ne |
|campaign_last_sent|cijeli broj|ne |
|merge_field_count|cijeli broj|ne |
|avg_sub_rate|broj|ne |
|avg_unsub_rate|broj|ne |
|target_sub_rate|broj|ne |
|open_rate|broj|ne |
|click_rate|broj|ne |
|last_sub_date|niz|ne |
|last_unsub_date|niz|ne |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Popisi|polja|ne |
|total_items|cijeli broj|ne |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|email_type|niz|ne |
|status|niz|Da |
|merge_fields|nije definirano|ne |
|interese|niz|ne |
|jezik|niz|ne |
|VIP|Booleove vrijednosti|ne |
|mjesto|nije definirano|ne |
|Adresa_Epošte|niz|Da |



### <a name="firstandlastname"></a>FirstAndLastName


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|FNAME|niz|ne |
|LNAME|niz|ne |



### <a name="location"></a>Mjesto


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|zemljopisnu širinu|broj|ne |
|Dužina|broj|ne |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|ne |
|Adresa_Epošte|niz|ne |
|unique_email_id|niz|ne |
|email_type|niz|ne |
|status|niz|ne |
|merge_fields|nije definirano|ne |
|interese|niz|ne |
|Statistika|nije definirano|ne |
|ip_signup|niz|ne |
|timestamp_signup|niz|ne |
|ip_opt|niz|ne |
|timestamp_opt|niz|ne |
|member_rating|cijeli broj|ne |
|last_changed|niz|ne |
|jezik|niz|ne |
|VIP|Booleove vrijednosti|ne |
|email_client|niz|ne |
|mjesto|nije definirano|ne |
|last_note|nije definirano|ne |
|list_id|niz|ne |
|_links|polja|ne |



### <a name="lastnote"></a>Last_Note


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|note_id|cijeli broj|ne |
|created_at|niz|ne |
|created_by|niz|ne |
|Napomena|niz|ne |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Članovi|polja|ne |
|list_id|niz|ne |
|total_items|cijeli broj|ne |



### <a name="object"></a>Objekt






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Adresa_Epošte|niz|ne |
|email_type|niz|ne |
|status|niz|Da |
|merge_fields|nije definirano|ne |
|interese|niz|ne |
|jezik|niz|ne |
|VIP|Booleove vrijednosti|ne |
|mjesto|nije definirano|ne |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|Članovi|polja|ne |
|list_id|niz|ne |
|total_items|cijeli broj|ne |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|Da |
|Adresa_Epošte|niz|Da |
|unique_email_id|niz|ne |
|email_type|niz|ne |
|status|niz|ne |
|merge_fields|nije definirano|Da |
|interese|niz|ne |
|Statistika|nije definirano|ne |
|ip_signup|niz|ne |
|timestamp_signup|niz|ne |
|ip_opt|niz|ne |
|timestamp_opt|niz|ne |
|member_rating|cijeli broj|ne |
|last_changed|niz|ne |
|jezik|niz|ne |
|VIP|Booleove vrijednosti|ne |
|email_client|niz|ne |
|mjesto|nije definirano|ne |
|last_note|nije definirano|ne |
|list_id|niz|ne |
|_links|polja|ne |


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)
