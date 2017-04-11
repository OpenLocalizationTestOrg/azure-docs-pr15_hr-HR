<properties
    pageTitle="Okvir poveznika dodati logiku aplikacija | Microsoft Azure"
    description="Pregled poveznik za okvir s parametrima REST API-JA"
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

# <a name="get-started-with-the-box-connector"></a>Početak rada s okvir poveznika
Povežite se okvir i stvaranje datoteka, Izbriši datoteke i drugo. 

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2015-08-01 – pregled shema verziju.

Okvir, možete učiniti sljedeće:

- Sastavljanje vaše tvrtke tijek na temelju podataka zatražite od okvira. 
- Uporaba okidača kada Stvori ili ažurira datoteku.
- Pomoću akcije koje kopirati datoteku, izbrišite datoteke i drugo. Ove se radnje dobiti odgovor, a zatim unesite dostupne za ostale akcije izlaz. Na primjer, kada datoteku mijenja se u okviru, možete poduzeti te datoteke i e-poštu ga pomoću sustava Office 365.

Da biste dodali operaciju u aplikacijama logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija
Okvir obuhvaća sljedeće okidača i akcije.

| Okidača | Akcija|
| --- | --- |
|<ul><li>Kada se stvaraju datoteke</li><li>Izmjene datoteke</li></ul> | <ul><li>Stvaranje datoteke</li><li>Kada se stvaraju datoteke</li><li>Kopiranje datoteke</li><li>Brisanje datoteke</li><li>Izdvajanje Arhivske mape</li><li>Sadržaj datoteke pomoću ID-a</li><li>Sadržaj datoteke pomoću puta</li><li>Dohvaćanje datoteka metapodataka pomoću ID-a</li><li>Dohvaćanje datoteka metapodataka pomoću puta</li><li>Ažuriranje datoteka</li><li>Izmjene datoteke</li></ul>

Sve poveznike podržava podatke u JSON i XML formatima.

## <a name="create-a-connection-to-box"></a>Povezivanje s okvir
Kada dodate ovaj poveznik aplikacija logike, morate Autorizirajte logike aplikacije za povezivanje s vašim signala.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Kada stvorite vezu, unesite u okvir svojstva. **Referenca REST API -JA** u ovoj se temi opisuju tih svojstava.

>[AZURE.TIP] Isti okvir povezivanje možete koristiti u drugim aplikacijama logike.

## <a name="swagger-rest-api-reference"></a>Referenca swagger REST API-JA
Odnosi se na verziju: 1.0.

### <a name="create-file"></a>Stvaranje datoteke
Prenosite datoteke u okvir.  
```POST: /datasets/default/files```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|folderPath|niz|Da|upit|Ništa |Put do mape da biste prenijeli datoteke na okvir|
|ime|niz|Da|upit|Ništa |Naziv datoteke da biste stvorili u okviru|
|tijelo|String(Binary) |Da|tijelo|Ništa |Sadržaj datoteke da biste prenijeli na okvir|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="when-a-file-is-created"></a>Kada se stvaraju datoteke
Prilikom stvaranja nove datoteke u mapi okvir, pokreće se u tijeku.  
```GET: /datasets/default/triggers/onnewfile```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|IDmape|niz|Da|upit|Ništa |Jedinstveni identifikator mape u okvir|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="copy-file"></a>Kopiranje datoteke
Kopira datoteke u okvir.  
```POST: /datasets/default/copyFile```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Izvor|niz|Da|upit|Ništa |URL-a izvornu datoteku|
|odredište|niz|Da|upit| Ništa|Put datoteke odredište u okviru, uključujući ciljnog naziva datoteke|
|Želite li izbrisati|Booleove vrijednosti|ne|upit| Ništa|Ako se brišu odredišnu datoteku postavljeno na "true"|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="delete-file"></a>Brisanje datoteke
Brisanje datoteke iz okvira.  
```DELETE: /datasets/default/files/{id}```


| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa |Jedinstveni identifikator datoteke da biste izbrisali iz okvira|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="extract-archive-to-folder"></a>Izdvajanje Arhivske mape
Izdvaja arhivsku datoteku u mapu u okvir (primjer: .zip).  
```POST: /datasets/default/extractFolderV2```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|Izvor|niz|Da|upit| |Put do datoteke arhive|
|odredište|niz|Da|upit| |Put u okvir za izdvajanje sadržaja arhive|
|Želite li izbrisati|Booleove vrijednosti|ne|upit| |Ako se brišu odredišna datoteka postavljeno na "true"|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="get-file-content-using-id"></a>Sadržaj datoteke pomoću ID-a
Dohvaća sadržaj datoteke iz okvira pomoću ID-a.  
```GET: /datasets/default/files/{id}/content```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put|Ništa |Jedinstveni identifikator datoteku u okviru|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="get-file-content-using-path"></a>Sadržaj datoteke pomoću puta
Dohvaća sadržaj datoteke iz okvira pomoću puta.  
```GET: /datasets/default/GetFileContentByPath```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|put|niz|Da|upit|Ništa |Jedinstveni put do datoteke u okviru|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="get-file-metadata-using-id"></a>Dohvaćanje datoteka metapodataka pomoću ID-a
Dohvaća datoteke metapodataka iz okvira pomoću ID-a datoteke.  
```GET: /datasets/default/files/{id}```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put| Ništa|Jedinstveni identifikator datoteku u okviru|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="get-file-metadata-using-path"></a>Dohvaćanje datoteka metapodataka pomoću puta
Dohvaća datoteke metapodataka iz okvira pomoću puta.  
```GET: /datasets/default/GetFileByPath```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|put|niz|Da|upit|Ništa |Jedinstveni put do datoteke u okviru|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="update-file"></a>Ažuriranje datoteka
Ažurira datoteku u okviru.  
```PUT: /datasets/default/files/{id}```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|ID-a|niz|Da|put| Ništa|Jedinstveni identifikator datoteke da biste ažurirali u okviru|
|tijelo|String(Binary) |Da|tijelo|Ništa |Sadržaj datoteke da biste ažurirali u okviru|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="when-a-file-is-modified"></a>Izmjene datoteke
Kada datoteke je izmijenjena u okviru mapi, pokreće se u tijeku.  
```GET: /datasets/default/triggers/onupdatedfile```

| Ime|Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|IDmape|niz|Da|upit|Ništa |Jedinstveni identifikator mape u okvir|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


## <a name="object-definitions"></a>Definicija objekta

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Naziv svojstva | Vrsta podataka | Obavezno|
|---|---|---|
|Tablični|nije definirano|ne|
|blob|nije definirano|ne|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|Izvor|niz|ne|
|riješiti problem|niz|ne|
|urlEncoding|niz|ne|
|tableDisplayName|niz|ne|
|tablePluralName|niz|ne|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|Izvor|niz|ne|
|riješiti problem|niz|ne|
|urlEncoding|niz|ne|

#### <a name="blobmetadata"></a>BlobMetadata

|Naziv svojstva | Vrsta podataka |Obavezno|
|---|---|---|
|ID-a|niz|ne|
|Ime|niz|ne|
|Riješiti problem|niz|ne|
|Put|niz|ne|
|LastModified|niz|ne|
|Veličina|cijeli broj|ne|
|Vrste medija|niz|ne|
|IsFolder|Booleove vrijednosti|ne|
|E-oznake|niz|ne|
|FileLocator|niz|ne|

## <a name="next-steps"></a>Daljnji koraci

[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).
