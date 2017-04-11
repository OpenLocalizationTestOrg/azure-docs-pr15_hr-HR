<properties
pageTitle="OneDrive za tvrtke | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. Povezivanje na OneDrive za tvrtke da biste upravljali datotekama. Izvršite različite radnje kao što su prijenos, ažuriranje, dobiti i brisanje datoteka."
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

# <a name="get-started-with-the-onedrive-for-business-connector"></a>Početak rada sa servisom OneDrive za tvrtke poveznika

Povezivanje na OneDrive za tvrtke da biste upravljali datotekama. Izvršite različite radnje kao što su prijenos, ažuriranje, dobiti i brisanje datoteka.

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju. 

Mogli početi s radom sada stvaranjem logike aplikacije, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija

OneDrive za tvrtke poveznik mogu se koristiti kao akciju; ima trigger(s). Sve poveznike podržava podatke u JSON i XML formatima. 

 OneDrive za tvrtke poveznik sadrži sljedeće akcije i/ili trigger(s) dostupne:

### <a name="onedrive-for-business-actions"></a>OneDrive za tvrtke akcije
Možete učiniti sljedeće akcije:

|Akcija|Opis|
|--- | ---|
|[GetFileMetadata](connectors-create-api-onedriveforbusiness.md#getfilemetadata)|Dohvaća metapodataka datoteke na servisu OneDrive za tvrtke pomoću ID-a|
|[UpdateFile](connectors-create-api-onedriveforbusiness.md#updatefile)|Ažurira datoteke na servisu OneDrive za tvrtke|
|[DeleteFile](connectors-create-api-onedriveforbusiness.md#deletefile)|Brisanje datoteke sa servisa OneDrive za tvrtke|
|[GetFileMetadataByPath](connectors-create-api-onedriveforbusiness.md#getfilemetadatabypath)|Dohvaća metapodataka datoteke na servisu OneDrive za tvrtke pomoću puta|
|[GetFileContentByPath](connectors-create-api-onedriveforbusiness.md#getfilecontentbypath)|Dohvaća sadržaj datoteke na servisu OneDrive za tvrtke pomoću puta|
|[GetFileContent](connectors-create-api-onedriveforbusiness.md#getfilecontent)|Dohvaća sadržaj datoteke na servisu OneDrive za tvrtke pomoću ID-a|
|[CreateFile](connectors-create-api-onedriveforbusiness.md#createfile)|Prenosite datoteke na OneDrive za tvrtke|
|[CopyFile](connectors-create-api-onedriveforbusiness.md#copyfile)|Kopira datoteke na OneDrive za tvrtke|
|[ListFolder](connectors-create-api-onedriveforbusiness.md#listfolder)|Popis datoteka na na servisu OneDrive za tvrtke u mapi|
|[ListRootFolder](connectors-create-api-onedriveforbusiness.md#listrootfolder)|Popis datoteka na OneDrive za tvrtke korijenske mape|
|[ExtractFolderV2](connectors-create-api-onedriveforbusiness.md#extractfolderv2)|Izdvaja mapa na OneDrive za tvrtke|
### <a name="onedrive-for-business-triggers"></a>Pokreće OneDrive za tvrtke
Možete poslušati za ove event(s):

|Pokretanje | Opis|
|--- | ---|
|Kada se stvaraju datoteke|Pokreće u tijeku prilikom stvaranja nove datoteke na servisu OneDrive za tvrtke u mapi|
|Izmjene datoteke|Pokreće u tijeku prilikom izmjene datoteke na servisu OneDrive za tvrtke u mapi|


## <a name="create-a-connection-to-onedrive-for-business"></a>Stvaranje veze na OneDrive za tvrtke
Da biste stvorili logike aplikacije onedrive za tvrtke, najprije morate stvoriti **vezu** zatim unijeti detalje za sljedeća svojstva: 

|Svojstvo| Obavezno|Opis|
| ---|---|---|
|Tokena|Da|Navedite OneDrive za tvrtke vjerodajnice|
Kada stvorite vezu, možete je koristiti za izvođenje akcije i slušanje za okidača opisane u ovom članku. 

>[AZURE.INCLUDE [Steps to create a connection to OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]

>[AZURE.TIP] Tu vezu možete koristiti u drugim aplikacijama logike.

## <a name="reference-for-onedrive-for-business"></a>Referenca za OneDrive za tvrtke
Odnosi se na verziju: 1.0

## <a name="getfilemetadata"></a>GetFileMetadata
Dohvaćanje datoteka metapodataka pomoću ID-a: dohvaća metapodataka datoteke na servisu OneDrive za tvrtke pomoću ID-a 

```GET: /datasets/default/files/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|Navedite datoteku|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="updatefile"></a>UpdateFile
O ažuriranju: ažuriranje datoteke na servisu OneDrive za tvrtke 

```PUT: /datasets/default/files/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|Navedite datoteku da biste ažurirali|
|tijelo| |Da|tijelo|Ništa|Sadržaj datoteke da biste ažurirali na servisu OneDrive za tvrtke|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="deletefile"></a>DeleteFile
Brisanje datoteke: briše datoteke sa servisa OneDrive za tvrtke 

```DELETE: /datasets/default/files/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|Navedite datoteku da biste izbrisali|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="getfilemetadatabypath"></a>GetFileMetadataByPath
Dohvaćanje datoteka metapodataka pomoću puta: dohvaća metapodataka datoteke na servisu OneDrive za tvrtke pomoću puta 

```GET: /datasets/default/GetFileByPath``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|put|niz|Da|upit|Ništa|Jedinstveni put do datoteke na servisu OneDrive za tvrtke|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="getfilecontentbypath"></a>GetFileContentByPath
Sadržaj datoteke pomoću puta: dohvaća sadržaj datoteke na servisu OneDrive za tvrtke pomoću puta 

```GET: /datasets/default/GetFileContentByPath``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|put|niz|Da|upit|Ništa|Jedinstveni put do datoteke na servisu OneDrive za tvrtke|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="getfilecontent"></a>GetFileContent
Sadržaj datoteke pomoću ID-a: dohvaća sadržaj datoteke na servisu OneDrive za tvrtke pomoću ID-a 

```GET: /datasets/default/files/{id}/content``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|Navedite datoteku|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="createfile"></a>CreateFile
Stvaranje datoteke: prenosite datoteke na OneDrive za tvrtke 

```POST: /datasets/default/files``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|folderPath|niz|Da|upit|Ništa|Put do mape da biste prenijeli datoteke na OneDrive za tvrtke|
|ime|niz|Da|upit|Ništa|Naziv datoteke da biste stvorili na servisu OneDrive za tvrtke|
|tijelo| |Da|tijelo|Ništa|Sadržaj datoteke za prijenos na OneDrive za tvrtke|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="copyfile"></a>CopyFile
Kopiranje datoteke: kopira datoteke na OneDrive za tvrtke 

```POST: /datasets/default/copyFile``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Izvor|niz|Da|upit|Ništa|URL-a izvornu datoteku|
|odredište|niz|Da|upit|Ništa|Odredište put datoteke na servisu OneDrive za tvrtke, uključujući ciljnog naziva datoteke|
|Želite li izbrisati|Booleove vrijednosti|ne|upit|FALSE|Ako se brišu odredišnu datoteku postavljeno na "true"|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="onnewfile"></a>OnNewFile
Kada se stvaraju datoteke: pokreće u tijeku prilikom stvaranja nove datoteke na servisu OneDrive za tvrtke u mapi 

```GET: /datasets/default/triggers/onnewfile``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|IDmape|niz|Da|upit|Ništa|Navedite mapu|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="onupdatedfile"></a>OnUpdatedFile
Izmjene datoteke: pokreće u tijeku prilikom izmjene datoteke na servisu OneDrive za tvrtke u mapi 

```GET: /datasets/default/triggers/onupdatedfile``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|IDmape|niz|Da|upit|Ništa|Navedite mapu|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="listfolder"></a>ListFolder
Popis datoteka u mapi: popisa datoteka na na servisu OneDrive za tvrtke u mapi 

```GET: /datasets/default/folders/{id}``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa|Navedite mapu|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="listrootfolder"></a>ListRootFolder
Popis korijenske mape: popisa datoteka na OneDrive za tvrtke korijenske mape 

```GET: /datasets/default/folders``` 

Nema parametara za ovaj poziv
#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="extractfolderv2"></a>ExtractFolderV2
Izdvajanje mape: izdvaja mapa na OneDrive za tvrtke 

```POST: /datasets/default/extractFolderV2``` 

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Izvor|niz|Da|upit|Ništa|Put do datoteke arhive|
|odredište|niz|Da|upit|Ništa|Put na servisu OneDrive za tvrtke za izdvajanje sadržaja arhive|
|Želite li izbrisati|Booleove vrijednosti|ne|upit|FALSE|Ako se brišu odredišna datoteka postavljeno na "true"|

#### <a name="response"></a>Odgovor

|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="object-definitions"></a>Definicija objekta 

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



### <a name="blobmetadata"></a>BlobMetadata


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|
|ID-a|niz|ne |
|Ime|niz|ne |
|Riješiti problem|niz|ne |
|Put|niz|ne |
|LastModified|niz|ne |
|Veličina|cijeli broj|ne |
|Vrste medija|niz|ne |
|IsFolder|Booleove vrijednosti|ne |
|E-oznake|niz|ne |
|FileLocator|niz|ne |



### <a name="object"></a>Objekt


| Naziv svojstva | Vrsta podataka | Obavezno |
|---|---|---|


## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)