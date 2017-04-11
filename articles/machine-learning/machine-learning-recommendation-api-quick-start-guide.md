<properties 
    pageTitle="Vodič za brzi početak: strojnog učenja preporuke API | Microsoft Azure" 
    description="Azure strojnog učenja preporuke – vodič za brzi početak rada" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Vodič za brzi početak rada za API preporuke za učenje računala

>[AZURE.NOTE]Trebali biste početi koristiti Kognitivne servisa za preporuke API umjesto ovu verziju. Servis za preporuke za Kognitivne će zamjene taj servis, a sve nove značajke će razvio postoji. Ima nove mogućnosti kao što su grupnog slanja promjena podršku, bolje Explorer API-JA, izvlačenje, dosljedan prijava/naplata sučelje za čišćenje API, itd.
> Dodatne informacije o [migraciji nove Kognitivne usluge](http://aka.ms/recomigrate)


U ovom dokumentu opisuje kako na onboard servisa i aplikaciju koju želite koristiti Microsoft Azure strojnog učenja preporuke. Dodatne informacije možete pronaći na API preporuke u [galeriju](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Općenito pregled

Da biste koristili Azure strojnog učenja preporuke, morate poduzeti sljedeće korake:

* Stvaranje modela – model je spremnik podataka o korištenju, katalog podataka i preporuke modela.
* Uvoz kataloga podataka – katalog sadrži metapodatke o stavke. 
* Uvoz podataka o korištenju - podataka o korištenju možete prenijeti u jedan od načina 2 (ili oboje):
    * Prijenos datoteke koja sadrži podatke o korištenju.
    * Slanjem događaji acquisition podataka.
    Obično prijenos datoteke korištenje da biste mogli da biste stvorili modelu početne preporuke (Samopokretanje) i koristiti dok sustav prikuplja dovoljno podataka pomoću acquisition oblik podataka.
* Sastavljanje preporuke model – to je asinkrona operacija u kojem sustava preporuke uzima sve podatke o korištenju i stvara preporuke modela. Ovaj postupak može potrajati nekoliko minuta ili nekoliko sati, ovisno o veličini podatke i konfiguracije parametara Sastavi. Kada pokretanje sastavljanje, dobit ćete ID-a Sastavi. Koristite da biste provjerili kada je završen postupka Sastavi prije pokretanja za preporuke.
* Korištenje preporuke - Get preporuke za određenu stavku ili popis stavki.

Sve gore navedene korake gotovi kroz API preporuke za Azure strojnog učenja.  Možete preuzeti aplikaciju uzorka koji implementira svaki od ovih koraka iz na [galerije kao i.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Ograničenja

* Maksimalni broj modela po pretplati je 10.
* Maksimalan broj stavki koje mogu sadržavati katalog je 100,000.
* Najveći broj točaka korištenje koje se zadržavaju se ~ 5,000,000. Najstarija izbrisat će se ako nove će se prenijeti ili prijavio.
* Maksimalna veličina podataka koje se mogu poslati u OBJAVI (npr. Uvoz kataloga podataka, uvoz podataka o korištenju) je 200MB.
* Broj transakcije sekundi za sastavljanje za preporuke modela koji još nije aktivna ~ 2TPS. Sastavljanje za preporuke model koji je aktivan mogu sadržavati prema gore da biste 20TPS.

##<a name="integration"></a>Integracija

###<a name="authentication"></a>Provjera autentičnosti
Microsoftovu studiju Azure Marketplace podržava Basic ili OAuth način provjere autentičnosti. Tipke računa jednostavno možete pronaći tako da odete tipki na tržištu u odjeljku [Postavke računa](https://datamarket.azure.com/account/keys). 
####<a name="basic-authentication"></a>Osnovna provjera autentičnosti
Dodavanje zaglavlja autorizacije:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Pretvorite Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Pretvorite Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Servis URI-JA
Servis korijen URI za Azure strojnog učenja preporuke API-ji je [ovdje.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Servis za cijeli URI izražen je elemenata specifikacija OData.

###<a name="api-version"></a>API verzija
Svaki poziv API će imati, na kraju, parametra upita pod nazivom apiVersion koje je potrebno postaviti na "1.0".

###<a name="ids-are-case-sensitive"></a>ID-ovi su velika i mala slova
ID-a, bilo koji od API-ji, vratio razlikuju velika i mala slova, a želite koristiti kao takve prilikom proslijeđena kao parametara u naknadni pozivi API-JA. Ako, primjerice, model ID-a i ID-a kataloga su velika i mala slova.

###<a name="create-a-model"></a>Stvaranje modela
Stvaranje zahtjeva za "Stvaranje modela":

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelName   |   Samo slova (A-Z, a z), brojevi (0 do 9) rastavnice (--), a dopuštene donje crtice (_).<br>Maksimalna duljina: 20 |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |


**Odgovor**:

Šifra stanja HTTP: 200

- `feed/entry/content/properties/id`-Sadrži ID modela.
**Napomena**: ID modela je velika i mala slova.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Uvoz kataloga podataka

Ako prenesete nekoliko kataloga datoteka s istim modelom s nekoliko pozive, ne možemo će umetnuti samo nove stavke kataloga. Postojeće stavke će ostati s izvorne vrijednosti.

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator model (velika i mala slova)  |
| Naziv datoteke | Tekstno identifikator kataloga.<br>Samo slova (A-Z, a z), brojevi (0 do 9) rastavnice (--), a dopuštene donje crtice (_).<br>Maksimalna duljina: 50 |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | Kataloga podataka. Oblik:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Ime</th><th>Obavezna</th><th>Vrsta</th><th>Opis</th></tr><tr><td>Id stavke</td><td>Da</td><td>Duljina alfanumerički, Maks 50</td><td>Jedinstveni identifikator stavke</td></tr><tr><td>Naziv stavke</td><td>Da</td><td>Alfanumerički, Maks ograničenje od 255 znakova</td><td>Naziv stavke</td></tr><tr><td>Kategorija artikla</td><td>Da</td><td>Alfanumerički, Maks ograničenje od 255 znakova</td><td>Kategoriju kojoj pripada tu stavku (primjerice kuhanje knjige, izražajnost...)</td></tr><tr><td>Opis</td><td>ne</td><td>Duljina alfanumerički, Maks 4000</td><td>Opis tu stavku</td></tr></table><br>Maksimalna veličina datoteke je 200MB.<br><br>Primjer:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan adresara<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, sobe za trenutke: rezervirajte Fiction (Byzantium pojavit će se)<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, adresara<br>552a1940-21e4-4399-82bb-594b46d7ed54, Restraint Beasts, adresara</pre> |


**Odgovor**:

Šifra stanja HTTP: 200

- `Feed\entry\content\properties\LineCount`-Broj redaka koji su prihvatili.
- `Feed\entry\content\properties\ErrorCount`-Broj redaka koji su umetati zbog pogreške.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Uvoz podataka o korištenju

####<a name="uploading-a-file"></a>Prijenos datoteke
U ovom se odjeljku objašnjava prijenos podataka o korištenju pomoću datoteke. Možete nazvati taj API nekoliko puta pomoću podataka o korištenju. Sve podatke o korištenju spremit će se za sve pozive.

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator model (velika i mala slova) |
| Naziv datoteke | Tekstno identifikator kataloga.<br>Samo slova (A-Z, a z), brojevi (0 do 9) rastavnice (--), a dopuštene donje crtice (_).<br>Maksimalna duljina: 50 |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | Korištenje podataka. Oblik:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Ime</th><th>Obavezna</th><th>Vrsta</th><th>Opis</th></tr><tr><td>Id korisnika</td><td>Da</td><td>Alfanumerički</td><td>Jedinstveni identifikator korisnika</td></tr><tr><td>Id stavke</td><td>Da</td><td>Duljina alfanumerički, Maks 50</td><td>Jedinstveni identifikator stavke</td></tr><tr><td>Vrijeme</td><td>ne</td><td>Datum u obliku: gggg-MM/ddThh (npr. 2013/06/20T10:00:00)</td><td>Vrijeme podataka</td></tr><tr><td>Događaja</td><td>Ne Ako navedeni, a zatim morate staviti datuma</td><td>Nešto od sljedećeg:<br>• Kliknite<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Za kupnju</td><td></td></tr></table><br>Maksimalna veličina datoteke je 200MB.<br><br>Primjer:<br><pre>149452, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Odgovor**:

Šifra stanja HTTP: 200

- `Feed\entry\content\properties\LineCount`-Broj redaka koji su prihvatili.
- `Feed\entry\content\properties\ErrorCount`-Broj redaka koji su umetati zbog pogreške.
- `Feed\entry\content\properties\FileId`-Datoteka identifikator.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Korištenje podataka nabavu
U ovom se odjeljku objašnjava da biste poslali događaje u stvarnom vremenu Azure strojnog učenja preporuke, obično na web-mjestu.

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Zahtjev za tijelo| Unos podataka događaja za svaki događaj koji želite poslati. Pošaljite za istu sesiju korisnika ili pregledniku isti ID u polju ID sesije. (Pogledajte primjer tijela događaja dolje).|


- Primjer za događaj "Kliknite":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Primjer za događaj 'RecommendationClick':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Primjer za događaj 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Primjer za događaj 'RemoveShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Primjer za događaj "Kupnju":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Primjer slanja 2 događaja, 'Kliknite' i 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Odgovor**: Šifra stanja HTTP: 200

###<a name="build-a-recommendation-model"></a>Stvaranje modela preporuka

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator model (velika i mala slova)  |
| userDescription | Tekstno identifikator kataloga. Imajte na umu da ako koristite razmake koje morate kodiranja s % 20 umjesto toga. Pogledajte primjer iznad.<br>Maksimalna duljina: 50 |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Ovo je asinkronog API-JA. Dobit ćete Sastavi ID kao odgovor. Da biste saznali kada je završen sastavljanje, poziva "Dohvati sastavlja Status je modela" API-JA i pronađite taj ID Sastavi u odgovoru. Imajte na umu da se koja nužnih od minuta za sati ovisno o veličini skupa podataka.

Nije moguće zauzeti preporuke prije pomaka sastavljanje završava.

Valjani Sastavi status:

- Stvaranje – Model stvoren.
- U redu čekanja – modela Sastavi je pokrenut i je u redu.
- Ugrađena je u tijeku sastavnih – modela.
- Uspjeh – Sastavi završen uspješno.
- Pogreška – Sastavi završio s pogreške.
- Otkazano – Sastavi otkazan.
- Otkazivanje – otkaže Sastavi.


Imajte na umu da ID Sastavi pronaći ćete u odjeljku sljedeći put:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Dohvatite Sastavi status modela

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId         |   Jedinstveni identifikator model (velika i mala slova)    |
|   onlyLastBuild   |   Označava hoće li se vraćati sve povijest Sastavi modela ili samo statusa najnoviju verziju. |
|   apiVersion      |   1.0                                 |


**Odgovor**:

Šifra stanja HTTP: 200

Odgovor sadrži jednim zapisom po Sastavi. Svaka stavka ima sljedeće podatke:

- `feed/entry/content/properties/UserName`– Ime korisnika.
- `feed/entry/content/properties/ModelName`– Naziv modela.
- `feed/entry/content/properties/ModelId`– Jedinstveni identifikator modela.
- `feed/entry/content/properties/IsDeployed`– Hoće li je sastavljanje implementiran (poznatog aktivni Sastavi).
- `feed/entry/content/properties/BuildId`– Stvaranje Jedinstveni identifikator.
- `feed/entry/content/properties/BuildType`– Vrsta sastavljanje.
- `feed/entry/content/properties/Status`– Stvaranje stanja. Može biti nešto od sljedećeg: pogreška, sastavni, Queued, Cancelling, Cancelled, uspjeh
- `feed/entry/content/properties/StatusMessage`– Poruka o stanju detaljne (odnosi se samo na određenim država).
- `feed/entry/content/properties/Progress`– Stvaranje napretka (%).
- `feed/entry/content/properties/StartTime`– Vrijeme početka Sastavi.
- `feed/entry/content/properties/EndTime`– Stvaranje vrijeme završetka.
- `feed/entry/content/properties/ExecutionTime`– Stvaranje trajanje.
- `feed/entry/content/properties/ProgressStep`– Detalje o trenutnom faza u kojoj se nalazi Sastavi u tijeku.

Valjani Sastavi status:
- Stvorili – Sastavi zahtjev stavka stvorena.
- U redu čekanja – zahtjev za sastavljanje je pokrenut i je u redu.
- Sastavni – Sastavi je u tijeku.
- Uspjeh – Sastavi završen uspješno.
- Pogreška – Sastavi završio s pogreške.
- Otkazano – Sastavi otkazan.
- Otkazivanje – otkaže Sastavi.

Valjane vrijednosti za sastavljanje vrstu:
- Položaj – Rank izdanja. (RANK napraviti detalja, pogledajte dokument "Strojnog učenja preporuke API dokumentaciju".)
- Preporuka - preporuke Sastavi.
- Fbt - često kupili zajedno Sastavi.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Preporuke

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator model (velika i mala slova) |
| ItemId-ovi | Odvojenih zarezom popis stavki kojom se preporučuje za.<br>Maksimalna duljina: 1024 |
| numberOfResults | Broj potrebnih rezultata |
| includeMetatadata | Buduću upotrebu, uvijek false |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200

Odgovor sadrži jednu stavku po stavku preporučene. Svaka stavka ima sljedeće podatke:

- `Feed\entry\content\properties\Id`-ID preporučene stavke.
- `Feed\entry\content\properties\Name`-Naziv stavke.
- `Feed\entry\content\properties\Rating`-Ocjena preporuke; veći broj označava veći pouzdanosti.
- `Feed\entry\content\properties\Reasoning`-Preporuke zaključivanja (npr. preporuke objašnjenja).

OData XML

Primjer odgovor u nastavku sadrži 10 preporučene stavke:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Ažuriranje modela
Možete ažurirati opis modela ili aktivni Sastavi ID.
*Aktivni Sastavi ID* – svaki Sastavi za svaki model sadrži ID-a Sastavi. Identifikator aktivni Sastavi je prvi uspješno sastavljanje svaki novi modela. Kada imate aktivni Sastavi ID-om, a zatim učinite dodatne sastavlja s istim modelom, morate izričito postavite ga kao zadani ID Sastavi ako želite. Kada ste zauzeti preporuke, ako ne navedete Sastavi ID-a koji želite koristiti, zadana koristit će se automatski.

U ovom mehanizam omogućuje vam - nakon što dodate preporuke modela proizvodnje - omogućuje stvaranje nove modele i testirati prije nego što ih Promicanje radnog.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POSTAVLJANJE     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| ID-a | Jedinstveni identifikator model (velika i mala slova) |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Imajte na umu da su XML oznake opis i ActiveBuildId nije obavezno. Ako ne želite postaviti opis ili ActiveBuildId, uklonite cijeli oznaka. |

**Odgovor**:

Šifra stanja HTTP: 200

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Pravne
Dokument se isporučuje "kao-je". Informacije i prikaze izražena u ovom dokumentu, uključujući URL ADRESE i druge reference na Internet web-mjesta, može se promijeniti bez prethodne najave. Primjeri koji se spominju u ovom dokumentu samo ilustracija i izmišljeni su. Ne stvarne povezanosti ili veze ili je slučajna. Ovaj dokument nije vam dati pravo na bilo kojem intelektualnog vlasništva bilo kojeg Microsoftova proizvoda. Možete kopirati i koristiti ovaj dokument za internu upotrebu kao referencu. © 2014 Microsoft. Microsoft Corporation. 
 
