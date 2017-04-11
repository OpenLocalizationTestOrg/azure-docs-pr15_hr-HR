<properties
pageTitle="Korištenje poveznik za Office 365 Video u aplikacijama za logiku | Microsoft Azure"
description="Početak rada s poveznik za Office 365 Video u aplikacijama za aplikaciju Microsoft Azure servisa logike"
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

# <a name="get-started-with-the-office365-video-connector"></a>Početak rada s poveznik za Office 365 Video
Povezivanje sustava Office 365 Video da biste dobili informacije o sustava Office 365 video, dobit ćete popis videozapisa i drugo. Poveznik za Office 365 Video koristiti putem:

- Logika aplikacije 

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju. Ovaj poveznik nije podržana na prethodne verzije sheme.

Uz Office 365 Video, možete učiniti sljedeće:

- Sastavljanje vaše tvrtke tijek na temelju podataka zatražite od sustava Office 365 Video. 
- Korištenje akcija koje Provjera stanja portala videozapisa, dobit ćete popis svih videozapisa u kanalu i još mnogo. Ove se radnje dobiti odgovor, a zatim unesite dostupne za ostale akcije izlaz. Ako, na primjer, konektora tražilice Bing za pretraživanje videozapisi za Office 365, a zatim konektora Office 365 video radi dobivanja informacija o taj videozapis. Videozapis koji zadovoljavaju vaše zahtjeve, ovaj videozapis možete objaviti na Facebooku. 

Da biste dodali operaciju u aplikacijama logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

Poveznik za Office 365 Video sadrži sljedeće radnje: Postoje bez okidača.

| Okidača | Akcija|
| --- | --- |
| Ništa | <ul><li>Provjera stanja videozapisa portala</li><li>Dohvati sve vidljivi kanale</li><li>Početak reprodukcije url servisa Azure Media Services manifesta videozapisa</li><li>Početak token nošenja da biste pristupili dešifrirati videozapisa</li><li>Dohvaća podatke o određenom Office 365 video</li><li>Popisi Svi videozapisi za Office 365 izlagati u kanalu</li></ul>

Sve poveznike podržava podatke u JSON i XML formatima. 

## <a name="create-a-connection-to-office365-video-connector"></a>Povezivanje s poveznik za Office 365 Video
Kada dodate ovaj poveznik aplikacija logike, morate prijaviti na račun za Office 365 Video i Dopusti logike aplikacije da biste se povezali s računom.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Kada stvorite vezu, unesite Office 365 video svojstva, kao što su ime klijenta ili kanal ID-a. **Referenca REST API -JA** u ovoj se temi opisuju tih svojstava.

>[AZURE.TIP] U ostale aplikacije logike možete koristiti ovaj iste veze za Office 365 Video.

## <a name="swagger-rest-api-reference"></a>Referenca swagger REST API-JA
Odnosi se na verziju: 1.0.

### <a name="checks-video-portal-status"></a>Provjera stanja videozapisa portala 
Provjerava video portala status da biste pogledali videozapis services omogućena.  
```GET: /{tenant}/IsEnabled``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|klijent|niz|Da|put|Ništa|Naziv klijenta za korisnika u imeniku je dio|


#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|



### <a name="get-all-viewable-channels"></a>Dohvati sve vidljivi kanale 
Dohvaća sve kanale korisnik ima pristup za prikaz.  
```GET: /{tenant}/Channels``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|klijent|niz|Da|put|Ništa|Naziv klijenta za korisnika u imeniku je dio|


#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Popisi Svi videozapisi za Office 365 izlagati u kanalu 
Popis svih prezentacija u kanalu videozapisa Office 365.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|klijent|niz|Da|put|Ništa|Naziv klijenta za korisnika u imeniku je dio|
|channelId|niz|Da|put|Ništa|Id kanal iz kojeg videozapisi morati moguće dohvatiti|


#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|




### <a name="gets-information-about-a-particular-office365-video"></a>Dohvaća podatke o određenom Office 365 video 
Dohvaća podatke o određenom Office 365 video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|klijent|niz|Da|put|Ništa|Naziv klijenta za korisnika u imeniku je dio|
|channelId|niz|Da|put|Ništa|Id kanala|
|videoId|niz|Da|put|Ništa|Identifikator videozapisa|


#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Početak reprodukcije url servisa Azure Media Services manifesta videozapisa 
Videozapis se reprodukcija url manifesta servisa Azure Media Services.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|klijent|niz|Da|put|Ništa|Naziv klijenta za korisnika u imeniku je dio|
|channelId|niz|Da|put|Ništa|Id kanala|
|videoId|niz|Da|put|Ništa|Identifikator videozapisa|
|streamingFormatType|niz|Da|upit|Ništa|Vrsta strujanje oblika. 1 – prelazili putem glatke strujanje ili MPEG-crtica. 0 - HLS strujanje|


#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Početak token nošenja da biste pristupili dešifrirati videozapisa 
Pronađite token nošenja da biste pristupili dešifrirati videozapis.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|klijent|niz|Da|put|Ništa|Naziv klijenta za korisnika u imeniku je dio|
|channelId|niz|Da|put|Ništa|Id kanala|
|videoId|niz|Da|put|Ništa|Identifikator videozapisa|


#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|Postupak je uspio|
|400|BadRequest|
|401|Neovlašteno|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja|
|Zadani|Nije uspjelo.|


## <a name="object-definitions"></a>Definicija objekta

#### <a name="channel-channel-class"></a>Kanala: Klase kanala

| Ime | Vrsta podataka | Obavezno|
|---|---|---|
|ID-a|niz|ne|
|Naslov|niz|ne|
|Opis|niz|ne|


#### <a name="video"></a>Videozapis 

| Ime | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|ne|
|Naslov|niz|ne|
|Opis|niz|ne|
|CreationDate|niz|ne|
|Vlasnik|niz|ne|
|ThumbnailUrl|niz|ne|
|VideoUrl|niz|ne|
|VideoDuration|cijeli broj|ne|
|VideoProcessingStatus|cijeli broj|ne|
|ViewCount|cijeli broj|ne|


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).
