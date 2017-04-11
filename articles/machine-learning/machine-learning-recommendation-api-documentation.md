<properties 
    pageTitle="Učenje dokumentaciju za preporuke API-JA na računalu | Microsoft Azure" 
    description="Azure strojnog učenja preporuke API potražite u dokumentaciji za preporuke tražilica u dostupna u web-servisa Microsoft Azure Marketplace." 
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
    ms.author="LuisCa"/>

#<a name="azure-machine-learning-recommendations-api-documentation"></a>Azure strojnog učenja dokumentaciju za preporuke API-JA

>[AZURE.NOTE]Trebali biste početi koristiti Kognitivne servisa za preporuke API umjesto ovu verziju. Servis za preporuke za Kognitivne će zamjene taj servis, a sve nove značajke će razvio postoji. Ima nove mogućnosti kao što su grupnog slanja promjena podršku, bolje Explorer API-JA, izvlačenje, dosljedan prijava/naplata sučelje za čišćenje API, itd.
> Dodatne informacije o [migraciji nove Kognitivne usluge](http://aka.ms/recomigrate)

Ovaj dokument prikazuje Microsoft Azure strojnog učenja preporuke API-ji.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. Općenito pregled
Ovaj je dokument API reference. Bi trebao počinjati s "Azure strojnog učenja preporuke – brzi početak rada" dokument.

API preporuke za učenje Azure računala može se podijeliti u sljedeće logičke grupe:

- <ins>Ograničenja</ins> - preporuke API ograničenja.
- <ins>Opće informacije</ins> – podatke o autentičnosti, servis URI i rad s verzijama.
- <ins>Osnovni model</ins> - API-ji koji omogućuju osnovne operacije na model (npr. Stvaranje, ažuriranje i brisanje model).
- <ins>Model napredne</ins> - API-ji koji omogućuju dobivanje dodatnih podataka uvida modela.
- <ins>Model poslovna pravila</ins> - API-ji koji omogućuju vam da biste upravljali pravilima tvrtke na stranici rezultata preporuke modela.
- <ins>Katalog</ins> - API-ji koji omogućuju osnovne operacije na model kataloga. Katalog sadrži metapodatke o stavke podataka o korištenju.
- <ins>Značajka</ins> - API-ji koji omogućuju da biste dobili uvid stavke u katalogu i kako se te podatke koristiti za stvaranje boljih preporuka.
- <ins>Podaci o korištenju</ins> – API-ji koji omogućuju osnovne operacije na model podataka o korištenju. Korištenje podataka u obrascu osnovni sastoji se od retke koji uključuju parove & #60; korisnički ID & #62; & #60 ";" ID stavke "&" #62;.
- <ins>Sastavljanje</ins> - API-ji koji vam omogućuje pokretanje modela Sastavi i učinite osnovne operacije koje se odnose na ovu međuverziju. Model Sastavi možete pokrenuti nakon što dodate podatke o korištenju koristan.
- <ins>Preporuka</ins> - API-ji koji omogućuju zauzeti preporuke kada istekne sastavljanje modela.
- <ins>Korisničke podatke</ins> – API-ji koji omogućuju Dohvaćanje informacija na korištenje korisničkih podataka.
- <ins>Obavijesti o</ins> - API-ji koji omogućuju vam da biste primali obavijesti na probleme vezane uz vaše operacije API-JA. (Na primjer, prijavljujete podataka o korištenju putem Acquisition podataka i većina događaja obrada se ne uspijeva. Obavijest o pogrešci se zaokružuje.)

##<a name="2-limitations"></a>2. ograničenja

- Maksimalni broj modela po pretplati je 10.
- Maksimalan broj izgradi po modelu iznosi 20.
- Maksimalan broj stavki koje mogu sadržavati katalog je 100,000.
- Najveći broj točaka korištenje koje se zadržavaju se ~ 5,000,000. Najstarija izbrisat će se ako nove će se prenijeti ili prijavio.
- Maksimalna veličina podataka koje se mogu poslati u OBJAVI (npr. Uvoz kataloga podataka, uvoz podataka o korištenju) je 200MB.
- Maksimalan broj stavki koje možete unijeti tijekom dohvaćanja preporuke je 150.

##<a name="3-apis---general-information"></a>3. API - opće informacije

###<a name="31-authentication"></a>3.1. Provjera autentičnosti
Slijedite smjernice za Microsoft Azure Marketplace vezane uz provjeru autentičnosti. Na tržištu podržava Basic ili OAuth način provjere autentičnosti.

###<a name="32-service-uri"></a>3,2. Servis URI-JA
Servis korijen URI za Azure strojnog učenja preporuke API-ji je [ovdje.](https://api.datamarket.azure.com/amla/recommendations/v3/)

Servis za cijeli URI izražen je elemenata specifikacija OData.  

###<a name="33-api-version"></a>3,3. API verzija
Svaki poziv API će imati, na kraju, pod nazivom apiVersion koje je potrebno postaviti na 1.0 parametra upita.

###<a name="34-ids-are-case-sensitive"></a>3.4. ID-ovi su velika i mala slova
ID-a, bilo koji od API-ji, vratio razlikuju velika i mala slova, a želite koristiti kao takve prilikom proslijeđena kao parametara u naknadni pozivi API-JA. Ako, primjerice, model ID-a i ID-a kataloga su velika i mala slova.

##<a name="4-recommendations-quality-and-cold-items"></a>4 preporuke kvalitete i Hladna stavki

###<a name="41-recommendation-quality"></a>4.1. Preporuka kvalitete

Stvaranje modela preporuke obično je dovoljno da biste omogućili sustav za preporuke. Ipak, preporuke kvalitete razlikuje se ovisno o korištenju obrađuju i opseg kataloga. Na primjer ako imate mnogo Hladna stavke (stavke bez značajan korištenje), sustav imaju poteškoća pruža preporuke za pretraživanje kao stavku ili korištenje takvih stavke kao preporučene jedan. Da bi se u bi se prevladala problem Hladna stavke, sustav omogućuje korištenje metapodataka stavki da biste poboljšali preporuke. U ovom metapodataka se nazivaju se značajke. Standardne značajke koje su u adresar autora ili glumca u film. Značajke su pružati putem kataloga u obrascu nizova ključa vrijednosti. Za cijeli oblik datoteke kataloga, pogledajte [odjeljak kataloga uvoza](#81-import-catalog-data). 

###<a name="42-rank-build"></a>4.2. RANK međuverzije

Značajke možete poboljšati modela preporuka, no da biste to učinili zahtijeva korištenje smisleni značajke. U tu svrhu uvedena je novi Sastavi – rank Sastavi. U ovom Sastavi će se kretati upotrebljivost značajke. Smislen značajka je značajka s rezultatom položaj broja 2 i prema gore.
Nakon objašnjenje koje značajke su smisleni, pokretanje preporuke Sastavi s popisom (ili potpopis) smisleni značajki. Moguće je za korištenje ove značajke za poboljšanje Toplo stavke i Hladna stavke. Da biste ih koristiti za Toplo stavke u `UseFeatureInModel` Sastavi parametar trebali postaviti. Da biste mogli koristiti značajke za Hladna stavke u `AllowColdItemPlacement` Sastavi parametar trebala bi biti omogućena.
Napomena: Nije moguće da biste omogućili `AllowColdItemPlacement` bez omogućivanja `UseFeatureInModel`.

###<a name="43-recommendation-reasoning"></a>4,3. Preporuka zaključivanja

Preporuka zaključivanja je aspekt korištenje značajke. Uistinu, modul Azure strojnog učenja preporuke možete koristiti značajke za preporuke objašnjenja (poznatog zaključivanja), oni mogu dovesti do više pouzdanosti preporučene stavke iz korisničke preporuke.
Da biste omogućili zaključivanja, u `AllowFeatureCorrelation` i `ReasoningFeatureList` parametara mora biti instalaciju prije no što zahtijeva preporuke Sastavi.


##<a name="5-model-basic"></a>5. osnovni model

###<a name="51-create-model"></a>5.1. Stvaranje modela
Stvaranje zahtjeva za "Stvaranje modela".

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

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

###<a name="52-get-model"></a>5,2. Početak modela
Stvara zahtjeva "Dohvati modela".

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Primjer:<br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   ID-a  |   Jedinstveni identifikator model (velika i mala slova) |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Model podataka možete pronaći u odjeljku sljedeće elemente:

- `feed/entry/content/properties/Id`-Jedinstveni ID modela
- `feed/entry/content/properties/Name`-Naziv modela.
- `feed/entry/content/properties/Date`-Datum stvaranja modela.
- `feed/entry/content/properties/Status`-Stanje modela. Nešto od sljedećeg:
    - Stvorili - Model stvara i ne sadrži katalog i korištenje.
    - ReadyForBuild - Model stvara se i sadrži katalog i korištenje.
- `feed/entry/content/properties/HasActiveBuild`-Označava ako model uspješno ugrađen.
- `feed/entry/content/properties/BuildId`-ID modela aktivni Sastavi.
- `feed/entry/content/properties/Mpr`-Model rangiranja srednje percentil (MPR – dodatne informacije potražite u članku ModelInsight).
- `feed/entry/content/properties/UserName`– Naziv internog korisnika modela.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

###<a name="53-get-all-models"></a>5,3. Dohvati sve modela
Dohvaća sve modelima trenutnog korisnika.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br>Primjer:<br>`<rootURI>/GetAllModels?apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

- `feed/entry/content/properties/Id`-Jedinstveni ID modela
- `feed/entry/content/properties/Name`-Naziv modela.
- `feed/entry/content/properties/Date`-Datum stvaranja modela.
- `feed/entry/content/properties/Status`-Stanje modela. Nešto od sljedećeg:
  - Stvorili - Model stvara i ne sadrži katalog i korištenje.
  - ReadyForBuild - Model stvara se i sadrži katalog i korištenje.
- `feed/entry/content/properties/HasActiveBuild`-Označava ako model uspješno ugrađen.
- `feed/entry/content/properties/BuildId`-ID modela aktivni Sastavi.
- `feed/entry/content/properties/Mpr`-Modela MPR (pogledajte ModelInsight dodatne informacije).
- `feed/entry/content/properties/UserName`– Naziv internog korisnika modela.
- `feed/entry/content/properties/UsageFileNames`-Popis datoteka za korištenje modela odvojenih zarezom.
- `feed/entry/content/properties/CatalogId`-ID modela kataloga.
- `feed/entry/content/properties/Description`-Opis modela.
- `feed/entry/content/properties/CatalogFileName`-Naziv datoteke kataloga modela.

OData XML


    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

###<a name="54-update-model"></a>5.4. Ažuriranje modela

Možete ažurirati opis modela ili aktivni Sastavi ID.<br>
<ins>Aktivni Sastavi ID</ins> – svaki Sastavi za svaki model sadrži ID-a Sastavi. Identifikator aktivni Sastavi je prvi uspješno sastavljanje svaki novi modela. Kada imate aktivni Sastavi ID-om, a zatim učinite dodatne sastavlja s istim modelom, morate izričito postavite ga kao zadani ID Sastavi ako želite. Kada ste zauzeti preporuke, ako ne navedete Sastavi ID-a koji želite koristiti, zadana koristit će se automatski.<br>
U ovom mehanizam omogućuje vam - nakon što dodate preporuke modela proizvodnje - omogućuje stvaranje nove modele i testirati prije nego što ih Promicanje radnog.


| HTTP metoda | URI-JA |
|:--------|:--------|
|POSTAVLJANJE     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br>Primjer:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   ID-a      | Jedinstveni identifikator model (velika i mala slova)  |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br>Imajte na umu da su XML oznake opis i ActiveBuildId nije obavezno. Ako ne želite postaviti opis ili ActiveBuildId, uklonite cijeli oznaka.|

**Odgovor**:

Šifra stanja HTTP: 200

###<a name="55-delete-model"></a>5,5. Brisanje modela
Briše postojećem modelu tako da ID-a.

| HTTP metoda | URI-JA |
|:--------|:--------|
|BRISANJE     |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Primjer:<br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   ID-a  |   Jedinstveni identifikator model (velika i mala slova) |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

##<a name="6-model-advanced"></a>6. napredne modela

###<a name="61-model-data-insight"></a>6.1. Model podataka uvida
Vraća statističke podatke na podataka o korištenju koji je ovaj model načinjene pomoću.

Dostupna je samo za preporuke Sastavi.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Primjer:<br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Podaci se vraćaju kao skup svojstava.

- `feed/entry/id/content/properties/key`-Sadrži naziv svojstva.
- `feed/entry/id/content/properties/value`-Sadrži vrijednosti svojstva.

Sljedeća tablica prikazuje vrijednost koja predstavlja svaki ključ.

|Ključ|Opis|
|:-----|:----|
| AvgItemLength | Prosječan broj različitih korisnika po stavku. |
| AvgUserLength | Prosječan broj stavki po korisniku. |
| DensificationNumberOfItems | Broj stavki nakon čišćenje stavke koje nije moguće modelled. |
| DensificationNumberOfUsers | Broj točaka korištenje nakon čišćenje korisnika i stavke koje nije moguće modelled. |
| DensificationNumberOfRecords | Broj točaka korištenje nakon čišćenje korisnika i stavke koje nije moguće modelled. |
| MaxItemLength | Broj različitih korisnika najpopularnije stavke. |
| MaxUserLength | Maximal broj stavki za korisnika. |
| MinItemLength | Maximal broj različitih korisnika za stavku. |
| MinUserLength | Minimalni broj stavki za korisnika. |
| RawNumberOfItems | Broj stavki na popisu korištenje datoteke. |
| RawNumberOfUsers | Broj točaka korištenje prije sve čišćenje. |
| RawNumberOfRecords | Broj točaka korištenje prije sve čišćenje. |
| SamplingNumberOfItems | N/D |
| SamplingNumberOfRecords | N/D |
| SamplingNumberOfUsers | N/D |

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="62-model-insight"></a>6.2. Model uvida
Vraća modela uvida na aktivni sastavljanje ili (Ako je dao) na određene Sastavi.

Dostupna je samo za preporuke Sastavi.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |ID aktivnog Sastavi:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>S određenim Sastavi ID-om:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela |
|   buildId |   Neobavezno - broj koji označava uspješno Sastavi. |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Podaci se vraćaju kao skup svojstava.

- `feed/entry/id/content/properties/key`
- `feed/entry/id/content/properties/value`


Sljedeća tablica prikazuje vrijednost koja predstavlja svaki ključ.

| Ključ | Opis |
|:---- |:----|
| CatalogCoverage | Koji dio kataloga može biti modelled s uzoraka korištenja. Ostale stavke će vam značajke utemeljene na sadržaj. |
| Mpr | Srednja vrijednost percentila rang modela. Donja je bolje. |
| NumberOfDimensions | Broj dimenzije koristi algoritam factorization matrice. |


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="63-get-model-sample"></a>6,3. Početak ogledni Model
Dohvaća uzorka modela preporuke.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Primjer:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>S određenim Sastavi ID-om:<br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br>Primjer:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

OData XML

Odgovor se vraća u obliku neobrađenog teksta:

<pre>
Razine 1---655fc 955-a5a3-4a26-9723-3090859cb27b, Prey: U Novel 655fc 955-a5a3-4a26-9723-3090859cb27b, Prey: Novel ocjena: 0.5215 3f471802-f84f-44a0 – 99c 8-6d2e7418eec1, crni Hawk dolje: priče Moderna War ocjena: 0.5151 07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon ocjena: 0.5148 6afc18e4-8c2a - 43d 1-9021-57543d6b11d8, Imajica ocjena: 0.5146 e4cc5e69-3567-43ab-b00f-f0d8d0506870, kliknite popis ocjena: 0.514 56b61441-0eed-46cc-a8f6-112775b81892, živost i Death u Shanghai 56b61441-0eed-46cc-a8f6-112775b81892, živost i Death u Shanghai ocjena: 0.5218 53156702-cc0c-443d-b718-6fb74b2491d3, sin od \ ocjena: 0.5212 fb8cf7a6-8719-46ee - 97d 4-92f931d77a3a, dim i kopije: kratki Fictions i iluzije ocjena : 0.5188 8f5fe006-79e4-4679-816b-950989d1db4b, A mjesto se nikad ocjena (Moderni American Fiction): 0.5156 d8db4583-cc0f-49ce-bc95-b7fa3491623f, sreća: Novel ocjena: 0.5156 50471eec-9aeb-4900-84 d 7 – 21567ab18546, ako u Buddha Datirana: priručnik za pronalaženje ljubav na na Duhovna put cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine tajne u Ya-Ya Sisterhood: Novel ocjena: 0.5266 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, Poisonwood Bible: Novel ocjena: 0.5252 973f8cbd-0846-4f6b - 9d 28-4dd0d7dc3a19, Pigs u Heaven ocjena: 0.5244 e2cbf7ad-0636-4117-8b30-298da6df7077, životinja snove ocjena : 0.5227 6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions Ugly Stepsister: Novel ocjena: 0.5222 5e97148f-defb - 4d 74-af2d-80f4763bf531, precizno kraj na prezentacija (Oprah, Book kluba) 5e97148f-defb - 4d 74-af2d-80f4763bf531, precizno kraj ocjena prezentacija (Oprah, Book kluba): 0.537 5dcbac37-2946-4f2a-a0b3-bbe710f9409a, gore Otok: Novel ocjena: 0.5277 bc5b69db-733b-4346-adde-3927544258f7, Downtown ocjena: 0.5275 31fe5c63-3e5a - 48d 0-802b-d3b0f989a634, bolje dan: Tale krvnog i Sweatsocks ocjena: 0.5252 0adf981a-b65b - 4c 11-b36b-78aca2f948a2, savršen oluja : True priče od muškarci na temelju ocjena moru: 0.5238 68f97068-ae1a-4163-9e94-396b800b743d, Modoc: True priče maksimalnog Slona koji ikad mogli živjeti 68f97068-ae1a-4163-9e94-396b800b743d, Modoc: True priče maksimalnog Slona koji ikad mogli živjeti ocjena: 0.5379 6724862e-e4e7-4022-9614-1468d8b902ff, malo kuća na ocjena Prairie: 0.5345 cdedb837-1620-496d - 94c 4-6ccfed888320, malo kuće u širu ocjena šumu: 0.5325 382164ba-406b-4187-b726-d7a54b9d790d, dodirnuti od Pooh ocjena: 0.5309 6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5 , Na banaka boja šljive Creek ocjena: 0.5285 37ef8e74-e348-44e5-aabc-1d7f9efcb25b, su muškarci sa Ožu žene su iz Venus: praktičnih vodiča za poboljšanje komunikacije i ne uspijevate što u svoje odnose 37ef8e74-e348-44e5-aabc-1d7f9efcb25b, muškarci su iz Ožu, žene iz Venus: praktičnih vodiča za poboljšanje komunikacije i ne uspijevate što u svoje odnose ocjena: 0.5397 f2be16d4-5faf - 4d 32-ab83-7ba74d29261e, Politically ispravne Bedtime priče : Moderna Tales za naše život i radno vrijeme ocjena: 0.5207 ef732c5c-334b-4d6b-ab82-7255eb7286d0, čast među lopovima ocjena: 0.5195 0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, koji omogućuje stabla ocjena: 0.5194 883b360f-8b42-407f-b977-2f44ad840877, strašan priča želite reći u na tamno: prikupljenih ocjena American Folklore (strašan priča): 0.5184 ff51b67e-fa8e-4c5e-8f4d-02a928de735d, muškarci na poslu: rukotvorina Bejzbol d008dae9-c73a-40a1-9a9b-96d5cf546f36, Archipelago Gulag 1918 1956: pokusa u ocjena književnost istraživanja I-II: 0.5416 ff51b67e-fa8e-4c5e-8f4d-02a928de735d, muškarci na poslu : Rukotvorina Bejzbol ocjena: 0.5403 49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland ocjena: 0.5394 cc7964fd-d30f-478e-a425-93ddbdf094ed, poseban na prikupljanje: Arena Vol. 1 ocjena: 0.5379 8a1e9f36-97af-4614-bed9-24e3940a05f3, više Sniglets: bilo koju riječ koja ne prikazuje u rječniku, ali potrebno ocjena: 0.5377 12a6d988-be21-4a09-8143-9d5f4261ba16, san od Eagles 07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon ocjena: 0.5417 e4cc5e69-3567-43ab-b00f-f0d8d0506870, kliknite popis ocjena: 0.5416 1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, obitelji ocjena: 0.5371 56daeffe - 7d 48-43cd-8ef8-7dffd0c103d3, Kilo klase ocjena: 0.5366 b2fe511e-5cb9-4a56-b823-2801e63e6a96, pravne Tender ocjena : Fosil traženje 0.5366 df87525b-e435-4bd6-8701-4e60ad344e28, 56d 33036-dfda-46b9-8e2a-76cb03921bb0, X datoteke: ocjena dna nula: 0.5417 0780cde8-6529-4e1d-b6c6-082c1b80e596, dvanaest crveno Herrings ocjena: 0.5416 df87525b-e435-4bd6-8701-4e60ad344e28, pronalaženje Fosil ocjena: 0.5408 400fe331 - 2c 35-490c-adbc-b28b4b73d56c, moraju smo reći na zadužen? Ocjena: 0.5383 f86ad7d0 - 5c 03-42b3-aebf-13d44aec8b30, nijanse mirovanja ocjena: 0.5358 de1f62a4-89e6 - 44d 2-aaee-992a4bf093f1, kartu koja promijenili svijeta: William Novak i rođenja Moderna Geology de1f62a4-89e6 - 44d 2-aaee-992a4bf093f1, kartu koja promijenili svijeta: William Novak i rođenja Moderna Geology ocjena: 0.5422 b303538f-e2c6-4a2c-b425-8d21e684fc3e, moj organski uzgojene Oswald ocjena: 0.5385 34b84627-48af-4a4c - 96c 4-b26fb3863f56, ponoći Vrt dobro i Evil ocjena: 0.5379 306cbaa7-b1a8-4142-9 d 55-e11b5018a7a8, ulica pravnik ocjena : 0.5376 e53b4baa - 8c c 09 45 4 95c 0-b6a26b98770b, Smillas neuspješnih pronalaženja objekata osjećaj za snijeg ocjena: 0.5367

<a name="level-2"></a>Razine 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, u Sinister Svinja (Hillerman Tony) 352aaea1-6b12-454d-a3d5-46379d9e4eb2, Sinister Svinja (Hillerman Tony) ocjena: 0.5425 74c 49398-bc10-4af5-a658-a996a1201254, podređenih oluja (Peters Elizabeth) ocjena: 0.5387 9ba80080-196e-43fd-8025-391d963f77e7, u pluta djevojčice ocjena: 0.5372 e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Pohvala (Scottoline Lisa) ocjena: 0.5353 b2fe511e-5cb9-4a56-b823-2801e63e6a96, pravni Tender ocjena: 0.5332 c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon dana 0adf981a-b65b - 4c 11-b36b-78aca2f948a2, u savršen oluja: A True priče od muškarci odnosu u Sea ocjena: 0.5433 c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon dana ocjena : 0.543 a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: bilo koju riječ koji se ne pojavljuje u rječniku, ali moraju) ocjena: 0.5327 6f6e192e - 0d 64-49ca-9b63-f09413ea1ee6, Politically točan praznika priča: za Enlightened božićne godišnjim dobom ocjena: 0.5307 798051a8 147d - 4d 46-b0dc-e836325029e6, STAROST ocjena INNOCENCE (film TIE-IN): 0.5301 73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth Century Classics) cba8163f-6536-436b-8130-47b4a43c827f, pouzdanost nitko (službeni vodič za X datoteke Vol. 2) ocjena: 0.5434 5708e4cb 2492 49 c 0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) ocjena: 0.5406 73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth Century Classics) Ocjena: 0.5403 d885b0bd-ae4b-452d-bdf2-faa90197dbc9, boja uz ocjena: 0.539 b133a9c4-4784-4db3-b100-d0d6dffb94d2, istine se slaže ocjena tamo (službeni vodič za X datoteke Vol. 1): 0.5367 271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: ili znam zašto Winged Kit Sings 271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: ili znam zašto Winged Kit Sings ocjena: 0.5445 2de1c354-90ff - 47c 5-a0db-1bad7d88ef94, ocjena žena (podređenih nasilja nizova) na Salaryman: 0.5329 d279416e - 19c 0-43f8-9ec9-a585947879ca, Zen stav ocjena : 0.5316 c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge djevojčice Cootie: ocjenu Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)): 0.5305 8ef4751c-7074-409e-a3ac-d49b222fc864, gdje su ocjena prijave stvari: 0.5289 9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, njihove očiju ste gledali Božjom 9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, njihove očiju ste gledali Božjom ocjena: 0.5446 da45c4d5-aba1-413b-a9bd-50df98b1e1d2, stabala Bean ocjena: 0.5389 65ecbdd1 - 131c - 40c 3-a3d6-d86ca281377a, Božjom Small stvari ocjena: 0.5387 c78743bf-7947-4a0c-8db7-8a3bfe69ba70, kronike kamen ocjena: 0.5355 973f8cbd-0846-4f6b - 9d 28-4dd0d7dc3a19, Pigs u Heaven ocjena : 0.5344 5f17d90a-2604-4fe8-8977-1a280b9098b1, jedan za na novac (boja šljive Stephanie Novels (Paperback)) 5f17d90a-2604-4fe8-8977-1a280b9098b1, jedan za ocjena novac (boja šljive Stephanie Novels (Paperback)): 0.5446 57169b2b-9a8a-486b-9aac-1ed98ce57168, konačni privlačnosti ocjena: 0.5332 efcb1bc4-7278-4a8f-b491-befde02070d6, momenta istine ocjena: 0.5329 1efa91a2-993b - 4c 43-9f5c-3454fc12612d, snimiti faktor ocjena: 0.5309 24c 59962-458a-4ec8-b95d-d694e861919c, kod kuće u Mitford (Mitford godina) ocjena: 0.5303 4fd48c46-1a20 - 4c 57-bc7f-a02ef123dc52, kao prirode unijeli ga: dječaka tko je pokrenuto kao djevojčice 4fd48c46-1a20-4c57-bc7f-a02ef123dc52 , Kao što je prirode unijeli ga: na dječaka tko je potenciju kao na djevojčice ocjena: 0.5449 cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs u Heaven ocjena: 0.5329 19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, ljubav u ocjena vrijeme Cholera (Penguin sjajno knjige 20th Century): 0.5267 15689d 09-c711-4844-84 d 8-130a90237b26, Bel Canto ocjena: 0.5245 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, u Poisonwood Bible: A Novel ocjena: 0.5235 98df28ec-41e7-4fca-b77f-8b0d3109085d, zvijezde Trek o f874b5a3 - 5d 40-4436-94ff-0fa1c090ddf5, u Ned i dolazi (A Scribner classic) ocjena : 0.5451 98df28ec-41e7-4fca-b77f-8b0d3109085d, zvijezde Trek o ocjeni: 0.5442 0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Nepročitanih ocjena: 0.5421 15316ca6-1e38-425f-893d-691944a47000, više strašan priča želite reći u tamno ocjena: 0.5409 329d 5682-3dc3-4206-8aa2-eef4b1032258, slova ocjene zemlji: 0.54 5b9445d5-c072 - 419c - 8d 49-6f669bb1b0a9, kćeri Fortune: U Novel (Oprah-adresara kluba (tvrdi uvez)) 5b9445d5-c072 - 419c - 8d 49-6f669bb1b0a9, kćeri Fortune: Novel (Oprah-adresara kluba (tvrdi uvez)) ocjena: 0.5462 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, Poisonwood Bible: Novel ocjena: 0.5372 604eb3bd-6026-4f51-bffd-9fb54f180400, obitelji slike: Novel ocjena: 0.5341 8d06d01d – 31cd-4678-b6b1-140a67987ce9, pjesama ocjena svakodnevne vrijeme (Oprah-adresara kluba (Paperback)) : 0.5334 da45c4d5-aba1-413b-a9bd-50df98b1e1d2, stabla na Bean ocjena: 0.5319 d5358189-d70f-4e35-8add-34b83b4942b3, Pigs u Heaven d5358189-d70f-4e35-8add-34b83b4942b3, Pigs u Heaven ocjena: 0.5491 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, Poisonwood Bible: Novel ocjena: 0.5401 c78743bf-7947-4a0c-8db7-8a3bfe69ba70, kronike kamen ocjena: 0.5393 8d06d01d – 31cd-4678-b6b1-140a67987ce9, pjesama ocjena svakodnevne vrijeme (Oprah-adresara kluba (Paperback)): 0.5382 973f8cbd-0846-4f6b - 9d 28-4dd0d7dc3a19, Pigs u Heaven ocjena: 0.5367

</pre>


##<a name="7-model-business-rules"></a>7. pravila tvrtke modela

Ovo su vrste pravila za podržane:
- <strong>Popisu blokiranih</strong> - popisu blokiranih omogućava popis stavki koje ne želite da biste se vratili u rezultatima preporuke. 

- <strong>FeatureBlockList</strong> - popisu blokiranih značajka omogućuje vam da biste blokirali stavki koje se temelje na vrijednostima njegovim značajkama.

*Slanje više od 1000 stavki u jednom popisu blokiranih pravila ili poziva može vremenskog ograničenja. Ako vam je potrebna da biste blokirali više od 1000 stavki, možete upućivati pozive s nekoliko popisu blokiranih.*

- <strong>Upsale</strong> - Upsale omogućuje vam da biste nametnuli stavke da biste se vratili u rezultatima preporuke.

- <strong>WhiteList</strong> - bijeli popis omogućuje vam da samo prijedlog preporuke s popisa stavki.

- <strong>FeatureWhiteList</strong> - bijeli popis značajka omogućuje vam da samo preporučujemo da stavke koje sadrže vrijednosti određene značajke.

- <strong>PerSeedBlockList</strong> – po Početni broj popisa blokova omogućava po stavci popisa stavki koje nije moguće vratiti kao rezultat preporuke.




###<a name="71-get-model-rules"></a>7.1. Dohvatite modela pravila

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Primjer:<br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

- `feed/entry/content/properties/Id`-Jedinstveni identifikator ovo pravilo.
- `feed/entry/content/properties/Type`– Vrsta pravila.
- `feed/entry/content/properties/Parameter`-Parametar pravila.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="72-add-rule"></a>7,2. Dodavanje pravila

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/AddRule?apiVersion=%271.0%27`|
|ZAGLAVLJE   |`"Content-Type", "text/xml"`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | 
<ins>Kad god koja omogućuje ID-a stavke za pravila tvrtke, provjerite je li koristiti vanjski Id stavke (isti Id u koje ste koristili u datoteka kataloga)</ins><br>
<ins>Da biste dodali pravilo popisu blokiranih:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><ins>
<ins>Da biste dodali pravilo FeatureBlockList:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><ins>Da biste dodali pravilo Upsale:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br>
<ins>Da biste dodali pravilo WhiteList:</ins><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><ins>
<ins>Da biste dodali pravilo FeatureWhiteList:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><ins>Da biste dodali pravilo PerSeedBlockList:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|


**Odgovor**:

Šifra stanja HTTP: 200

U API vraća novostvorenu pravilo detalja. Svojstvo pravila može biti dohvaćeni iz putove za sljedeće:

- `feed/entry/content/properties/Id`-Jedinstveni identifikator ovo pravilo.
- `feed/entry/content/properties/Type`-Vrstu pravila: popisu blokiranih ili Upsale.
- `feed/entry/content/properties/Parameter`-Parametar pravila.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="73-delete-rule"></a>7,3. Brisanje pravila

| HTTP metoda | URI-JA |
|:--------|:--------|
|BRISANJE     |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela |
|   filterId    |   Jedinstveni identifikator filtra |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

###<a name="74-delete-all-rules"></a>7,4. Brisanje svih pravila

| HTTP metoda | URI-JA |
|:--------|:--------|
|BRISANJE     |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

##<a name="8-catalog"></a>8. katalog

###<a name="81-import-catalog-data"></a>8.1. Uvoz kataloga podataka

Ako prenesete nekoliko kataloga datoteka s istim modelom s nekoliko pozive, ne možemo će umetnuti samo nove stavke kataloga. Postojeće stavke će ostati s izvorne vrijednosti. Ako koristite taj način ne možete ažurirati kataloga podataka.

Katalog podataka morate slijediti sljedećem obliku:

- Bez značajki-`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Pomoću značajki-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

Napomena: Maksimalna veličina datoteka je 200MB.

**Detalji o obliku**

| Ime | Obavezna | Vrsta |  Opis |
|:---|:---|:---|:---|
| Id stavke |Da | [A-z], [a-z], [0-9], [_] & #40; Donja crta & #41; [–] & #40; crtice & #41;<br> Maksimalna duljina: 50 | Jedinstveni identifikator stavke. |
| Naziv stavke | Da | Bilo koje alfanumeričke znakove<br> Maksimalna duljina: 255 | Naziv stavke. | 
| Kategorija artikla | Da | Bilo koje alfanumeričke znakove <br> Maksimalna duljina: 255 | Kategorije koje ta stavka pripada (npr. kuhanje knjige, izražajnost...); može biti prazna. |
| Opis | Ne, osim ako su značajke prikaz (ali može biti prazna) | Bilo koje alfanumeričke znakove <br> Maksimalna duljina: 4000 | Opis tu stavku. |
| Popis značajki | ne | Bilo koje alfanumeričke znakove <br> Maksimalna duljina: 4000; Maksimalan broj značajke: 20 | Popis naziva značajke odvojenih zarezom = vrijednost značajke koje je moguće koristiti za poboljšanje modela preporuke; Pogledajte odjeljak [Dodatne teme](#2-advanced-topics) . |


| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|
|ZAGLAVLJE   |`"Content-Type", "text/xml"`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela  |
| Naziv datoteke | Tekstno identifikator kataloga.<br>Samo slova (A-Z, a z), brojevi (0 do 9) rastavnice (--), a dopuštene donje crtice (_).<br>Maksimalna duljina: 50 |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | Primjer (značajke):<br/>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, pojavit će se opis adresara autor = Wright Tihomir publisher = Harper Flamingo Kanadi, godine = 2001.<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, u sobi za trenutke: A Fiction (Byzantium knjiga), adresara, autor = Bantock Nadimak publisher = Harpercollins, godine = 1997<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, adresara, autor = Timothy Findley publisher = HarperFlamingo Kanadi, godine = 2001.<br>552a1940-21e4-4399-82bb-594b46d7ed54, Restraint Beasts, pojavit će se opis adresara autor = Magnus Mills publisher = Arcade objavljivanje godine = 1998</pre> |


**Odgovor**:

Šifra stanja HTTP: 200

U API vraća izvješća uvoza.
- `feed\entry\content\properties\LineCount`-Broj redaka koji su prihvatili.
- `feed\entry\content\properties\ErrorCount`-Broj redaka koji su umetati zbog pogreške.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="82-get-catalog"></a>8,2. Početak kataloga
Dohvaća sve stavke kataloga.
Katalog će biti dohvaćeni jednu stranicu po. Ako želite da stavke u određenom indeks, možete koristiti parametar odata $skip. Na primjer ako želite da biste dobili stavke počevši od položaja 100, dodajte parametar $skip = 100 na zahtjev.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Odgovor sadrži jednu stavku po stavku kataloga. Svaka stavka ima sljedeće podatke:

- `feed/entry/content/properties/ExternalId`-Kataloga vanjski ID stavke, Ponuđeni koje je korisnik odabrao.
- `feed/entry/content/properties/InternalId`-Kataloga Interna ID stavke, koji je generirao Azure strojnog učenja preporuke.
- `feed/entry/content/properties/Name`-Naziv stavke kataloga.
- `feed/entry/content/properties/Category`-Kategorija artikla kataloga.
- `feed/entry/content/properties/Description`-Kataloga opis stavke.
- `feed/entry/content/properties/Metadata`-Kataloga metapodataka stavke.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">The Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="83-get-catalog-items-by-token"></a>8,3. Katalog stavki po tokena

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela |
|   tokena   |   Tokena naziva stavke kataloga. Mora sadržavati najmanje 3 znamenke. |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Odgovor sadrži jednu stavku po stavku kataloga. Svaka stavka ima sljedeće podatke:

- `feed/entry/content/properties/InternalId`-Kataloga Interna ID stavke, koji je generirao Azure strojnog učenja preporuke.
- `feed/entry/content/properties/Name`-Naziv stavke kataloga.
- `feed/entry/content/properties/Rating`-(za buduću upotrebu)
- `feed/entry/content/properties/Reasoning`-(za buduću upotrebu)
- `feed/entry/content/properties/Metadata`-(za buduću upotrebu)
- `feed/entry/content/properties/FormattedRating`-(za buduću upotrebu)

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                </m:properties>
            </content>
        </entry>
    </feed>

##<a name="9-usage-data"></a>9. podaci o korištenju
###<a name="91-import-usage-data"></a>9,1. Uvoz podataka o korištenju
####<a name="911-uploading-file"></a>9.1.1. Prijenos datoteka
U ovom se odjeljku objašnjava prijenos podataka o korištenju pomoću datoteke. Možete nazvati taj API nekoliko puta pomoću podataka o korištenju. Sve podatke o korištenju spremit će se za sve pozive.

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId |   Jedinstveni identifikator modela  |
| Naziv datoteke | Tekstno identifikator kataloga.<br>Samo slova (A-Z, a z), brojevi (0 do 9) rastavnice (--), a dopuštene donje crtice (_).<br>Maksimalna duljina: 50 |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | Korištenje podataka. Oblik:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Ime</th><th>Obavezna</th><th>Vrsta</th><th>Opis</th></tr><tr><td>Id korisnika</td><td>Da</td><td>[A-z], [a-z], [0-9], [_] & #40; Donja crta & #41; [–] & #40; crtice & #41;<br> Maksimalna duljina: 255 </td><td>Jedinstveni identifikator korisnika.</td></tr><tr><td>Id stavke</td><td>Da</td><td>[A-z], [a-z], [0-9], [& #95;] & #40; Donja crta & #41; [–] & #40; crtice & #41;<br> Maksimalna duljina: 50</td><td>Jedinstveni identifikator stavke.</td></tr><tr><td>Vrijeme</td><td>ne</td><td>Datum u obliku: gggg-MM/ddThh (npr. 2013/06/20T10:00:00)</td><td>Vrijeme podataka.</td></tr><tr><td>Događaja</td><td>Ne; Ako navedeni morate staviti datuma</td><td>Nešto od sljedećeg:<br>• Kliknite<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Za kupnju</td><td></td></tr></table><br>Maksimalna veličina datoteke: 200MB<br><br>Primjer:<br><pre>149452, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Odgovor**:

Šifra stanja HTTP: 200

- `Feed\entry\content\properties\LineCount`-Broj redaka koji su prihvatili.
- `Feed\entry\content\properties\ErrorCount`-Broj redaka koji su umetati zbog pogreške.
- `Feed\entry\content\properties\FileId`-Datoteka identifikator.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="912-using-data-acquisition"></a>9.1.2. Korištenje podataka nabavu
U ovom se odjeljku objašnjava da biste poslali događaje u stvarnom vremenu Azure strojnog učenja preporuke, obično na web-mjestu.

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27`|
|ZAGLAVLJE   |`"Content-Type", "text/xml"`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
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
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
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

###<a name="92-list-model-usage-files"></a>9.2. Popis modela korištenje datoteka
Dohvaća metapodataka svih datoteka za korištenje modela.
Korištenje datoteke će biti dohvaćeni jednu stranicu po. Stavke containes 100 svake stranice. Ako želite da stavke u određenom indeks, možete koristiti parametar odata $skip. Na primjer ako želite da biste dobili stavke počevši od položaja 100, dodajte parametar $skip = 100 na zahtjev.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   forModelId  |   Jedinstveni identifikator modela |
|   apiVersion      | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Odgovor sadrži jednu stavku po datoteci korištenje. Svaka stavka ima sljedeće podatke:

- `feed\entry\content\properties\Id`-ID korištenje datoteka.
- `feed\entry\content\properties\Length`– Duljina korištenje datoteke u MB.
- `feed\entry\content\properties\DateModified`-Datum stvaranja korištenje datoteka.
- `feed\entry\content\properties\UseInModel`– Hoće li se koristi datoteka za korištenje u modelu.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
            </m:properties>
        </content>
    </entry>
</feed>

###<a name="93-get-usage-statistics"></a>9,3. Početak Statistički podaci o korištenju
Dohvaća Statistički podaci o korištenju.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela  |
| DatumPočetka] |   Datum početka. Oblik: gggg/MM/ddThh |
| završnog datuma | Datum završetka. Oblik: gggg/MM/ddThh |
| eventTypes |  Odvojenih zarezom niz vrsta događaja ili null se svi događaji  |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Zbirka elemenata ključa vrijednosti. Svaki od njih sadrži zbroj događaje za vrstu određene događaj grupirano po satu.

- `feed\entry[i]\content\properties\Key`-Sadrži vrijeme (grupirano po satu) i vrsta događaja.
- `feed\entry[i]\content\properties\Value`– Count Ukupna događaj.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="94-get-usage-file-sample"></a>9,4. Početak korištenja datoteke uzorka
Dohvaća prvi 2KB korištenje sadržaja datoteke.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela  |
| fileId |  Jedinstveni identifikator korištenje datoteku modela  |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Odgovor se vraća u obliku neobrađenog teksta:
<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15 , True, 1 235588, 21BF8088-B6C0-4509-870C-E1C7AC78304A, 2014. / 11/02T13:40:15, True, 1 158254, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014. / 11/02T13:40:15, True, 1 271195, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014. / 11/02T13:40:15, True, 1 141157, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014. / 11/02T13:40:15, True, 1 171118, 3BB5CB44-D143-4BDD-A55C-443964BF4B23, 2014. / 11/02T13:40:15, True, 1 225087, 3BB5CB44-D143-4BDD-A55C-443964BF4B23, 2014. / 11/02T13:40:15, True, 1
</pre>


###<a name="95-get-model-usage-file"></a>9,5. Dohvaćanje datoteka za korištenje modela
Dohvaća cijeli sadržaj datoteke za korištenje.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| funkcija MID | Jedinstveni identifikator modela  |
| FID | Jedinstveni identifikator korištenje datoteku modela |
| Preuzimanje | 1 |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

Odgovor se vraća u obliku neobrađenog teksta:
<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15 ,True,1 235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15 ,True,1 260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15 , True, 1 260965, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014. / 11/02T13:40:15, True, 1 102758, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014. / 11/02T13:40:15, True, 1 112602, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014. / 11/02T13:40:15, True, 1 163925, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014. / 11/02T13:40:15, True, 1 262998, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014. / 11/02T13:40:15, True, 1 144717, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014. / 11/02T13:40:15, True, 1
</pre>

###<a name="96-delete-usage-file"></a>9.6. Izbrišite datoteku za korištenje
Brisanje datoteka za korištenje navedeni modela.

| HTTP metoda | URI-JA |
|:--------|:--------|
|BRISANJE     |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27`|

| Naziv parametra    |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela  |
| fileId | Jedinstveni identifikator datoteke će se izbrisati |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200


###<a name="97-delete-all-usage-files"></a>9.7. Izbrišite sve datoteke za korištenje
Brisanje svih datoteka za korištenje modela.

| HTTP metoda | URI-JA |
|:--------|:--------|
|BRISANJE     |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27`|

| Naziv parametra    |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela  |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

##<a name="10-features"></a>10. značajke
U ovom se odjeljku objašnjava dohvatiti informacije o značajkama, kao što su uvezeni značajke i njihove vrijednosti, njihov položaj i kada je dodijeliti ovaj položaj. Značajke uvoze se kao dio kataloga podataka, a zatim njihov položaj povezan nakon dovršetka ranga Sastavi.
Značajka položaj možete promijeniti prema uzorku podataka o korištenju i vrste stavki. No za dosljedno korištenje/stavki, rang mora sadržavati samo small prilagođavati razlikama.
Položaj broja značajke je koji nije negativan broj. Broj 0 označava je li značajka ne rangiranih (u slučaju pozivanje taj API prije dovršetka prvog rank sastavljanje). Datum na kojoj je atribut rang zove svježina rezultat.

###<a name="101-get-features-info-for-last-rank-build"></a>10,1. Pristup značajke Info (za posljednje Rank Sastavi)
Dohvaća podatke značajke, uključujući rang posljednjeg uspješna rank Sastavi.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK      |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27`

| Naziv parametra    |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela  |
|samplingSize| Broj vrijednosti da biste uključili za svaku značajku prema koje su prisutne u katalogu podataka. <br/>Moguće vrijednosti su:<br> -1 - sve uzorke. <br>0 - bez uzorkovanje. <br>N - vratite N uzorka za svaki naziv značajke.|
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |


**Odgovor**:

Šifra stanja HTTP: 200

Odgovor sadrži popis stavki informacija značajke. Svaka stavka sadrži:

- `feed/entry/content/m:properties/d:Name`-Naziv značajke.
- `feed/entry/content/m:properties/d:RankUpdateDate`-Datuma na kojoj se rang je dodijeljen ovu značajku, poznatog Značajka svježina rezultat. Početni datum ('0001-01-01T00:00:00') označava da nema rank Sastavi izvršena.
- `feed/entry/content/m:properties/d:Rank`-Položaj značajke (prikaz). Položaj 2.0 i gore smatra dobro značajke.
- `feed/entry/content/m:properties/d:SampleValues`-Odvojenih zarezom popis vrijednosti do veličine uzorkovanje ste tražili.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get the features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>


###<a name="102-get-features-info-for-specific-rank-build"></a>10,2. Dobivanje značajke informacija (za određene Rank Sastavi)

Dohvaća podatke značajke, uključujući rangiranje za određene rank Sastavi.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK      |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27`

| Naziv parametra    |   Valjane vrijednosti    |
|:--------          |:--------          |
| modelId | Jedinstveni identifikator modela  |
|samplingSize| Broj vrijednosti da biste uključili za svaku značajku prema koje su prisutne u katalogu podataka.<br/> Moguće vrijednosti su:<br> -1 - sve uzorke. <br>0 - bez uzorkovanje. <br>N - vratite N uzorka za svaki naziv značajke.|
|rankBuildId| Jedinstveni identifikator rank sastavljanje ili -1 za sastavljanje rank posljednji|
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |


**Odgovor**:

Šifra stanja HTTP: 200

Odgovor sadrži popis stavki informacija značajke. Svaka stavka sadrži:

- `feed/entry/content/m:properties/d:Name`-Naziv značajke.
- `feed/entry/content/m:properties/d:RankUpdateDate`-Datuma na kojoj se rang je dodijeljen ovu značajku, poznatog Značajka svježina rezultat. Početni datum ('0001-01-01T00:00:00') označava da nema rank Sastavi izvršena.
- `feed/entry/content/m:properties/d:Rank`-Položaj značajke (prikaz). Položaj 2.0 i gore smatra dobro značajke.
- `feed/entry/content/m:properties/d:SampleValues`-Odvojenih zarezom popis vrijednosti do veličine uzorkovanje ste tražili.

OData

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get the features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


##<a name="11-build"></a>11. sastavljanje

  U ovom se odjeljku objašnjava različite API-ji vezane uz izdanja. Postoje 3 vrste izdanja: preporuke Sastavi, rank Sastavi i na Sastavi FBT (često kupili zajedno).

Sastavljanje preporuke Svrha je da biste generirali model preporuke za predviđanja. Predviđanja (za ovu vrstu Sastavi) dolaze u dva flavors:
* I2I - poznatog Preporuke za artikl - daje stavke ili popis stavki, ona će predviđanje popis stavki koje su vjerojatno biti zanimljive visoke.
* U2I - poznatog Korisnik preporuke stavke – navedeni korisničkog ID-ja (i po želji popis stavki) ovu mogućnost će predviđanje popis stavki koje su vjerojatno biti zanimljive visoke za dani korisnika (i njezin dodatni odabir stavki). Preporuke za U2I temelje se na povijest stavke koje su interesa za korisnika do vremena gradnje modela.

Rank Sastavi je tehnička Sastavi koja omogućuje dodatne informacije o upotrebljivost značajke. Obično da biste dobili najboljih rezultata za preporuke modela koje su potrebne značajke, morate poduzeti sljedeće korake:
- Pokretanje rank Sastavi (osim ako je rezultat značajke stabilan) i pričekajte dok se značajka rezultat.
- Dohvaćanje rang značajke tako da nazovete [Options značajke](#101-get-features-info-for-last-rank-build) API-JA.
- Konfiguriranje preporuke Sastavi sa sljedećih parametara:
    - `useFeatureInModel`-Postavljen na True.
    - `ModelingFeatureList`-Postavite na popis značajki s rezultatom 2.0 ili više (prema sortira koju ste vratili u prethodnom koraku) odvojenih zarezom.
    - `AllowColdItemPlacement`-Postavljen na True.
    - Po želji možete postaviti `EnableFeatureCorrelation` na True i `ReasoningFeatureList` popis značajki koje želite koristiti za objašnjenja (obično se isti popis značajke koje se koriste u modeliranja ili potpopis).
- Pokretanje sastavljanje preporuke s konfiguriranog parametra.

Napomena: Ako ste konfigurirali parametre (npr. sastavljanje preporuke bez parametara za pozivanje) ili nije izričito onemogućite korištenje značajke (npr. `UseFeatureInModel` postavite na False), sustav postavit će parametara vezane uz značajku za gornje vrijednosti explained u slučaju da postoji rank Sastavi.

Postoji bez ograničenja o pokretanju rank Sastavi i preporuke Sastavi istovremeno za isti model. Ipak, dva izdanja iste vrste ne može se izvoditi na isti model paralelno.

Za sastavljanje FBT (često kupili zajedno) još drugi preporuke algoritam naziva ponekad "običan" recommender koji je dobar za katalozi koji nisu istovrstan u prirode (istovrstan: knjige, filmovi, neke Hrana način; koje nisu istovrstan: računala i uređaja, a zatim domenama, Visoko raznih).

Napomena: Ako korištenje datoteke koje ste prenijeli sadrži neobavezno polje "događaj" pa za FBT modeliranja samo događaje "Za kupnju" će se koristiti. Ako nijedna vrsta događaja navedeni su svi događaji će se smatrati za kupnju.


####<a name="111-build-parameters"></a>11.1 sastavljanje parametara

Svaka vrsta Sastavi se može konfigurirati putem skupa parametara (opisane u nastavku). Ako ne konfigurirate parametre, sustav automatski atributa vrijednosti za parametre prema podacima postoji u vrijeme pokretanje na Sastavi.

#####<a name="1111-usage-condenser"></a>11.1.1. Korištenje condenser
Korisnike ili stavke s nekoliko točke korištenje može sadržavati više Šum od informacije. Sustav pokušava predviđanje Minimalni broj točaka korištenje po korisniku/stavku koja će se koristiti u modelu. Broj će se unutar raspona definiranog ItemCutoffLowerBound i ItemCutoffUpperBound parametara za stavke te raspona definiranog parametri UserCutOffLowerBound i UserCutoffUpperBound za korisnike. Postavljanjem barem jedno od odgovarajuće granica na nulu moguće minimizirati condenser učinak na stavke ili korisnicima.

#####<a name="1112-rank-build-parameters"></a>11.1.2. Položaj sastavljanje parametara

Sljedeća tablica prikazuje parametri Sastavi rank Sastavi.

|Ključ|Opis|Vrsta|Valjana vrijednost|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Broj iteracija izvodi modela prikazuje se ukupno vrijeme računalnim i točnost modela. Na viši broj, bolje točnost prikazat će vam se, ali će trajati dulje vrijeme računalnim.| Cijeli broj | 10 50 |
| NumberOfModelDimensions | Broj dimenzija odnosi se na broj 'značajke' modela će pokušati pronaći podataka. Povećanje broja dimenzije da bi bolje prilagodbom rezultata u manjim klastere. Međutim, previše dimenzija onemogućuje model pronalaženje korelacija između stavki. | Cijeli broj | 10 40 |
|ItemCutOffLowerBound| Definira donja granica stavke za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|ItemCutOffUpperBound| Definira gornja granica stavke za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|UserCutOffLowerBound| Definira donja granica korisnika za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|UserCutOffUpperBound| Definira gornja granica korisnika za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |

#####<a name="1113-recommendation-build-parameters"></a>11.1.3. Preporuka Sastavi parametara
Sljedeća tablica prikazuje Sastavi parametara za preporuke Sastavi.

|Ključ|Opis|Vrsta|Valjana vrijednost|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Broj iteracija izvodi modela prikazuje se ukupno vrijeme računalnim i točnost modela. Na viši broj, bolje točnost prikazat će vam se, ali će trajati dulje vrijeme računalnim.| Cijeli broj | 10 50 |
| NumberOfModelDimensions | Broj dimenzija odnosi se na broj 'značajke' modela će pokušati pronaći podataka. Povećanje broja dimenzije da bi bolje prilagodbom rezultata u manjim klastere. Međutim, previše dimenzija onemogućuje model pronalaženje korelacija između stavki. | Cijeli broj | 10 40 |
|ItemCutOffLowerBound| Definira donja granica stavke za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|ItemCutOffUpperBound| Definira gornja granica stavke za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|UserCutOffLowerBound| Definira donja granica korisnika za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|UserCutOffUpperBound| Definira gornja granica korisnika za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
| Opis | Sastavljanje opis. | Niz | Sav tekst, maksimalna 512 znakova |
| EnableModelingInsights | Omogućuje metriku na model preporuke za izračun. | Booleove vrijednosti | TRUE i False |
| UseFeaturesInModel | Upućuje na to ako se značajke mogu koristiti da biste poboljšali modela preporuke. | Booleove vrijednosti | TRUE i False |
| ModelingFeatureList | Popis odvojenih zarezom značajka imena koja želite koristiti u sastavljanje preporuke da biste poboljšali za preporuke. | Niz | Značajka imena do 512 znakova |
| AllowColdItemPlacement | Upućuje na to ako se preporuke treba i automatske Hladna stavki putem značajke sličnosti. | Booleove vrijednosti | TRUE i False |
| EnableFeatureCorrelation | Upućuje na to ako se značajke mogu koristiti u zaključivanja. | Booleove vrijednosti | TRUE i False |
| ReasoningFeatureList | Odvojenih zarezom popis naziva značajka koja će se koristiti za zaključivanja rečenice (primjerice preporuke objašnjenja).  | Niz | Značajka imena do 512 znakova |
| EnableU2I | Dopusti poznatog personalizirane preporuka U2I (korisnik za preporuke stavke). | Booleove vrijednosti | TRUE i False (true zadani) |

#####<a name="1114-fbt-build-parameters"></a>11.1.4. FBT Sastavi parametara
Sljedeća tablica prikazuje Sastavi parametara za preporuke Sastavi.

|Ključ|Opis|Vrsta|Valjana vrijednost (zadano)|
|:-----|:----|:----|:---|
|FbtSupportThreshold | Kako običan je model. Broj pojavljivanja dodatnih stavki koje će se smatrati Modeliranje.| Cijeli broj | 3 – 50 (6) |
|FbtMaxItemSetSize | Bounds broj stavki u skupu Česti.| Cijeli broj | 2-3 (2) |
|FbtMinimalScore | Minimalna rezultat koji često korištenih skup mora imati da bi bile obuhvaćene opseg vraćenih rezultata. Veći bolje.| Dvaput | 0 ili je iznad (0) |
|FbtSimilarityFunction | Definira funkciju sličnosti koriste sastavljanje. Dizalica bolje serendipity, suautorstvo pojavu bolje predvidljivost i Jaccard je bolje narušena pouzdanost između dva. | Niz | cooccurrence Dizalica, jaccard (Dizalica) |


###<a name="112-trigger-a-recommendation-build"></a>11.2. Pokretanje preporuke međuverzije

  Prema zadanim postavkama taj API će pokrenuti Sastavi za model preporuke. Da biste pokrenuli rank Sastavi (da bi se rezultat značajke) varijanta API Sastavi Sastavi vrsta parametra želite koristiti.


| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|
|ZAGLAVLJE   |`"Content-Type", "text/xml"`(Ako pošaljete zahtjev tijelo)|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela  |
| userDescription | Tekstno identifikator kataloga. Imajte na umu da ako koristite razmake koje morate kodiranja s % 20 umjesto toga. Pogledajte primjer iznad.<br>Maksimalna duljina: 50 |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | Ako je prazno sastavljanje će izvršiti s zadani parametri Sastavi.<br><br>Ako želite da biste postavili parametre Sastavi, pošaljite parametre kao XML u tijelo kao sljedeći primjer. (Pogledajte objašnjenje parametara u odjeljku "Stvaranje parametara").`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>` |

**Odgovor**:

Šifra stanja HTTP: 200

Ovo je asinkronog API-JA. Dobit ćete Sastavi ID kao odgovor. Da biste saznali kada je završen sastavljanje, poziva "Dohvati sastavlja Status je modela" API-JA i pronađite taj ID Sastavi u odgovoru. Imajte na umu da se koja nužnih od minuta za sati ovisno o veličini skupa podataka.

Nije moguće zauzeti preporuke prije pomaka sastavljanje završava.

Valjani Sastavi status:

- Stvaranje – zahtjev za sastavljanje stvorena.
- U redu čekanja - Sastavi zahtjev poslan i on je u redu.
- U tijeku je sastavni - Sastavi.
- Uspjeh - Sastavi završen uspješno.
- Pogreška – Sastavi završio s pogreške.
- Otkazano - Sastavi je otkazan.
- Otkazivanje - Odustani zahtjev za sastavljanje poslan.


Imajte na umu da ID Sastavi pronaći ćete u odjeljku sljedeći put:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

###<a name="113-trigger-build-recommendation-rank-or-fbt"></a>11.3. Pokretanje Sastavi (preporuke, rang ili FBT)

| HTTP metoda | URI-JA |
|:--------|:--------|
|OBJAVA     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27`|
|ZAGLAVLJE   |`"Content-Type", "text/xml"`(Ako pošaljete zahtjev tijelo)|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela  |
| userDescription | Tekstno identifikator kataloga. Imajte na umu da ako koristite razmake koje morate kodiranja s % 20 umjesto toga. Pogledajte primjer iznad.<br>Maksimalna duljina: 50 |
| buildType | Vrsta sastavljanje pozvati: <br/> -Preporuke za sastavljanje preporuka <br> -'Rangiranje' rank međuverzije <br/> -Fbt za sastavljanje FBT
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | Ako je prazno sastavljanje će izvršiti s zadani parametri Sastavi.<br><br>Ako želite postaviti parametre Sastavi, pošaljite im kao XML u tijelo kao što su u sljedeće ogledne. (Pogledajte odjeljak "Sastavljanje parametara" za objašnjenje i cijeli popis parametara).`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>` |

**Odgovor**:

Šifra stanja HTTP: 200

Ovo je asinkronog API-JA. Dobit ćete Sastavi ID kao odgovor. Da biste saznali kada je završen sastavljanje, poziva "Dohvati sastavlja Status je modela" API-JA i pronađite taj ID Sastavi u odgovoru. Imajte na umu da se koja nužnih od minuta za sati ovisno o veličini skupa podataka.

Nije moguće zauzeti preporuke prije pomaka sastavljanje završava.

Valjani Sastavi status:

- Stvaranje - Model stvoren.
- U redu čekanja - modela Sastavi je pokrenut i je u redu.
- Ugrađena je u tijeku sastavnih - modela.
- Uspjeh - Sastavi završen uspješno.
- Pogreška – Sastavi završio s pogreške.
- Otkazano - Sastavi je otkazan.
- Otkazivanje - otkaže Sastavi.

Imajte na umu da ID Sastavi pronaći ćete u odjeljku sljedeći put:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




###<a name="114-get-builds-status-of-a-model"></a>11,4. Dohvatite izgradi Status modela
Dohvaća izgradi i njihovo stanje za navedeni model.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|


|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   modelId         |   Jedinstveni identifikator modela  |
|   onlyLastBuild   |   Označava hoće li se vraćati sve povijest Sastavi modela ili samo statusa najnoviju verziju  |
|   apiVersion      |   1.0                                 |


**Odgovor**:

Šifra stanja HTTP: 200

Odgovor sadrži jednim zapisom po Sastavi. Svaka stavka ima sljedeće podatke:

- `feed/entry/content/properties/UserName`-Ime korisnika.
- `feed/entry/content/properties/ModelName`-Naziv modela.
- `feed/entry/content/properties/ModelId`-Jedinstveni identifikator modela.
- `feed/entry/content/properties/IsDeployed`– Hoće li je sastavljanje implementiran (poznatog aktivni Sastavi).
- `feed/entry/content/properties/BuildId`-Sastavljanje Jedinstveni identifikator.
- `feed/entry/content/properties/BuildType`– Vrsta sastavljanje.
- `feed/entry/content/properties/Status`– Stvaranje stanja. Može biti nešto od sljedećeg: pogreška, sastavni, Queued, Cancelling, Cancelled, uspjeh.
- `feed/entry/content/properties/StatusMessage`-Poruka detaljno stanje (odnosi se samo na određenim država).
- `feed/entry/content/properties/Progress`-Sastavljanje napretka (%).
- `feed/entry/content/properties/StartTime`-Vrijeme početka Sastavi.
- `feed/entry/content/properties/EndTime`-Stvaranja i vrijeme završetka.
- `feed/entry/content/properties/ExecutionTime`-Sastavljanje trajanje.
- `feed/entry/content/properties/ProgressStep`-Detalje o trenutnog stanja Sastavi u tijeku.

Valjani Sastavi status:
- Stvorili - Sastavi zahtjev stavka stvorena.
- U redu čekanja – zahtjev za sastavljanje je pokrenut i je u redu.
- Sastavni - Sastavi je u tijeku.
- Uspjeh - Sastavi završen uspješno.
- Pogreška – Sastavi završio s pogreške.
- Otkazano - Sastavi je otkazan.
- Otkazivanje - otkaže Sastavi.

Valjane vrijednosti za sastavljanje vrstu:
- Položaj – Rank izdanja.
- Preporuka - preporuke Sastavi.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


###<a name="115-get-builds-status"></a>11,5. Dohvaćanje sastavlja stanja
Vraća sastavljanje statusi sve modela korisnika.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27`|


|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
|   onlyLastBuild   |   Označava hoće li se vraćati sve povijest Sastavi modela ili samo statusa najnoviju verziju. |
|   apiVersion      |   1.0                                 |


**Odgovor**:

Šifra stanja HTTP: 200

Odgovor sadrži jednim zapisom po Sastavi. Svaka stavka ima sljedeće podatke:

- `feed/entry/content/properties/UserName`-Ime korisnika.
- `feed/entry/content/properties/ModelName`-Naziv modela.
- `feed/entry/content/properties/ModelId`-Jedinstveni identifikator modela.
- `feed/entry/content/properties/IsDeployed`– Hoće li je implementirana sastavljanje.
- `feed/entry/content/properties/BuildId`-Sastavljanje Jedinstveni identifikator.
- `feed/entry/content/properties/BuildType`– Vrsta sastavljanje.
- `feed/entry/content/properties/Status`– Stvaranje stanja. Može biti nešto od sljedećeg: pogreška, sastavni, Queued, Cancelled, Cancelling, uspjeh.
- `feed/entry/content/properties/StatusMessage`-Poruka detaljno stanje (odnosi se samo na određenim država).
- `feed/entry/content/properties/Progress`-Sastavljanje napretka (%).
- `feed/entry/content/properties/StartTime`-Vrijeme početka Sastavi.
- `feed/entry/content/properties/EndTime`-Stvaranja i vrijeme završetka.
- `feed/entry/content/properties/ExecutionTime`-Sastavljanje trajanje.
- `feed/entry/content/properties/ProgressStep`-Detalje o trenutnog stanja Sastavi u tijeku.

Valjani Sastavi status:
- Stvorili - Sastavi zahtjev stavka stvorena.
- U redu čekanja – zahtjev za sastavljanje je pokrenut i je u redu.
- Sastavni - Sastavi je u tijeku.
- Uspjeh - Sastavi završen uspješno.
- Pogreška – Sastavi završio s pogreške.
- Otkazano - Sastavi je otkazan.
- Otkazivanje - otkaže Sastavi.


Valjane vrijednosti za sastavljanje vrstu:
- Položaj – Rank izdanja.
- Preporuka - preporuke Sastavi.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


###<a name="116-delete-build"></a>11.6. Brisanje međuverzije
Brisanje na Sastavi.

NAPOMENA: <br>Nije moguće izbrisati aktivni Sastavi. Model mora se ažurirati različite aktivni Sastavi prije no što ga izbrišete.<br>Nije moguće izbrisati je u tijeku Sastavi. Sastavljanje morate najprije otkazati tako da nazovete <strong>Otkazati sastavljanje</strong>.

| HTTP metoda | URI-JA |
|:--------|:--------|
|BRISANJE     |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| buildId | Jedinstveni identifikator sastavljanje. |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200

###<a name="117-cancel-build"></a>11,7. Otkazivanje međuverzije
Otkazuje Sastavi koja je u stanju.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POSTAVLJANJE     |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| buildId | Jedinstveni identifikator sastavljanje. |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200

###<a name="118-get-build-parameters"></a>11,8. Početak Sastavi parametara
Vraća sastavljanje parametara.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| buildId | Jedinstveni identifikator sastavljanje. |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200

U ovom API vraća zbirka elemenata ključa vrijednosti. Svaki element predstavlja parametar i njezina vrijednost:
- `feed/entry/content/properties/Key`-Sastavljanje naziv parametra.
- `feed/entry/content/properties/Value`-Sastavljanje vrijednosti parametra.

Sljedeća tablica prikazuje vrijednost koja predstavlja svaki ključ.

|Ključ|Opis|Vrsta|Valjana vrijednost|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Broj iteracija izvodi modela prikazuje se ukupno vrijeme računalnim i točnost modela. Na viši broj, bolje točnost prikazat će vam se, ali će trajati dulje vrijeme računalnim.| Cijeli broj | 10 50 |
| NumberOfModelDimensions | Broj dimenzija odnosi se na broj 'značajke' modela će pokušati pronaći podataka. Povećanje broja dimenzije da bi bolje prilagodbom rezultata u manjim klastere. Međutim, previše dimenzija onemogućuje model pronalaženje korelacija između stavki. | Cijeli broj | 10 40 |
|ItemCutOffLowerBound| Definira donja granica stavke za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|ItemCutOffUpperBound| Definira gornja granica stavke za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|UserCutOffLowerBound| Definira donja granica korisnika za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
|UserCutOffUpperBound| Definira gornja granica korisnika za na condenser. Potražite u članku korištenje condenser iznad. | Cijeli broj | 2 ili više (0 onemogućite condenser) |
| Opis | Sastavljanje opis. | Niz | Sav tekst, maksimalna 512 znakova |
| EnableModelingInsights | Omogućuje metriku na model preporuke za izračun. | Booleove vrijednosti | TRUE i False |
| UseFeaturesInModel | Upućuje na to ako se značajke mogu koristiti da biste poboljšali modela preporuke. | Booleove vrijednosti | TRUE i False |
| ModelingFeatureList | Popis odvojenih zarezom značajka imena koja želite koristiti u sastavljanje preporuke da biste poboljšali za preporuke. | Niz | Značajka imena do 512 znakova |
| AllowColdItemPlacement | Upućuje na to ako se preporuke treba i automatske Hladna stavki putem značajke sličnosti. | Booleove vrijednosti | TRUE i False |
| EnableFeatureCorrelation | Upućuje na to ako se značajke mogu koristiti u zaključivanja. | Booleove vrijednosti | TRUE i False |
| ReasoningFeatureList | Odvojenih zarezom popis naziva značajka koja će se koristiti za zaključivanja rečenice (primjerice preporuke objašnjenja).  | Niz | Značajka imena do 512 znakova |


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

##<a name="12-recommendation"></a>12. preporuka
###<a name="121-get-item-recommendations-for-active-build"></a>12.1. Preporuke stavke (za aktivni Sastavi)

Preporuke od aktivnog sastavljanje vrste "Preporuke" ili "Fbt" na temelju popisa stavki sjemenke (ulazni).

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| ItemId-ovi | Odvojenih zarezom popis stavki kojom se preporučuje za. <br>Ako je aktivan sastavljanje vrsta FBT možete poslati samo jednu stavku. <br>Maksimalna duljina: 1024 |
| numberOfResults | Broj potrebnih rezultata <br> Max: 150 |
| includeMetatadata | Buduću upotrebu, uvijek false |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200


Odgovor sadrži jednu stavku po stavku preporučene. Svaka stavka ima sljedeće podatke:
- `Feed\entry\content\properties\Id`-ID preporučene stavke.
- `Feed\entry\content\properties\Name`-Naziv stavke.
- `Feed\entry\content\properties\Rating`-Ocjena preporuke; veći broj označava veći pouzdanosti.
- `Feed\entry\content\properties\Reasoning`-Preporuke zaključivanja (npr. preporuke objašnjenja).

Primjer odgovor u nastavku sadrži 10 preporučene stavke.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

###<a name="122-get-item-recommendations-of-a-specific-build"></a>12,2. Preporuke stavke (od određene Sastavi)

Preporuke određene Sastavi vrste "Preporuke" ili "Fbt".

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| ItemId-ovi | Odvojenih zarezom popis stavki kojom se preporučuje za. <br>Ako je aktivan sastavljanje vrsta FBT možete poslati samo jednu stavku. <br>Maksimalna duljina: 1024 |
| numberOfResults | Broj potrebnih rezultata <br> Max: 150  |
| includeMetatadata | Buduću upotrebu, uvijek false
| buildId | Sastavi id koji će se koristiti za zahtjev za preporuke |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200


Odgovor sadrži jednu stavku po stavku preporučene. Svaka stavka ima sljedeće podatke:
- `Feed\entry\content\properties\Id`-ID preporučene stavke.
- `Feed\entry\content\properties\Name`-Naziv stavke.
- `Feed\entry\content\properties\Rating`-Ocjena preporuke; veći broj označava veći pouzdanosti.
- `Feed\entry\content\properties\Reasoning`-Preporuke zaključivanja (npr. preporuke objašnjenja).

Pogledajte primjer odgovor u 12.1

###<a name="123-get-fbt-recommendations-for-active-build"></a>12.3. Preporuke FBT (za aktivni Sastavi)

Preporuke aktivni sastavljanje vrste "Fbt" na temelju stavke Početni broj (input).

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| ID stavke | Preporučujemo da za stavku. <br>Maksimalna duljina: 1024 |
| numberOfResults | Broj potrebnih rezultata <br>Max: 150 |
| minimalScore | Minimalna rezultat koji često korištenih skup mora imati da bi bile obuhvaćene opseg vraćenih rezultata |
| includeMetatadata | Buduću upotrebu, uvijek false |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200


Odgovor sadrži jednu stavku po skupu preporučene stavke (skup stavki koje obično kupili zajedno s Početni broj/unos stavke). Svaka stavka ima sljedeće podatke:
- `Feed\entry\content\properties\Id1`-ID preporučene stavke.
- `Feed\entry\content\properties\Name1`-Naziv stavke.
- `Feed\entry\content\properties\Id2`ID - 2nd preporučene stavke (neobavezno).
- `Feed\entry\content\properties\Name2`-Naziv 2 stavke (neobavezno).
- `Feed\entry\content\properties\Rating`-Ocjena preporuke; veći broj označava veći pouzdanosti.
- `Feed\entry\content\properties\Reasoning`-Preporuke zaključivanja (npr. preporuke objašnjenja).

Primjer odgovor u nastavku sadrži skupove 3 preporučene stavke.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="124-get-fbt-recommendations-of-a-specific-build"></a>12.4. Preporuke FBT (od određene Sastavi)

Preporuke određene Sastavi vrste "Fbt".

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| ID stavke | Preporučujemo da za stavku. <br>Maksimalna duljina: 1024 |
| numberOfResults | Broj potrebnih rezultata <br>Max: 150 |
| minimalScore | Minimalna rezultat koji često korištenih skup mora imati da bi bile obuhvaćene opseg vraćenih rezultata |
| includeMetatadata | Buduću upotrebu, uvijek false |
| buildId | Sastavi id koji će se koristiti za zahtjev za preporuke |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200


Odgovor sadrži jednu stavku po skupu preporučene stavke (skup stavki koje obično kupili zajedno s Početni broj/unos stavke). Svaka stavka ima sljedeće podatke:
- `Feed\entry\content\properties\Id1`-ID preporučene stavke.
- `Feed\entry\content\properties\Name1`-Naziv stavke.
- `Feed\entry\content\properties\Id2`ID - 2nd preporučene stavke (neobavezno).
- `Feed\entry\content\properties\Name2`-Naziv 2 stavke (neobavezno).
- `Feed\entry\content\properties\Rating`-Ocjena preporuke; veći broj označava veći pouzdanosti.
- `Feed\entry\content\properties\Reasoning`-Preporuke zaključivanja (npr. preporuke objašnjenja).

Pogledajte primjer odgovor u 12.3

###<a name="125-get-user-recommendations-for-active-build"></a>kao 12.5. Preporuke korisnika (za aktivni Sastavi)

Korisnik preporuke od Sastavi vrste "Preporuke" označena kao aktivna Sastavi.

U API će vratiti popis predviđene stavke prema povijest korištenja korisnika.

Bilješke: 
 1. Postoji nema korisnika preporuku za sastavljanje FBT.
 2. Ako je aktivan sastavljanje FBT će ovu metodu vraća pogrešku.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| ID korisnika  | Jedinstveni identifikator korisnika |
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

Pogledajte primjer odgovor u 12.1

###<a name="126-get-user-recommendations-with-item-list-for-active-build"></a>12.6. Preporuke korisnika s popisa stavki (za aktivni Sastavi)

Preporuke korisnika od Sastavi vrste "Preporuke" označena kao aktivna Sastavi s dodatnim popis stavki

U API će vratiti popis predviđene stavke prema povijest korištenja korisnika i dodatne navedene stavke.

Bilješke: 
 1. Postoji nema korisnika preporuku za sastavljanje FBT.
 2. Ako je aktivan sastavljanje FBT će ovu metodu vraća pogrešku.


| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| ID korisnika  | Jedinstveni identifikator korisnika |
| itemsIds | Odvojenih zarezom popis stavki kojom se preporučuje za. Maksimalna duljina: 1024 |
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

Pogledajte primjer odgovor u 12.1

###<a name="127-get-user-recommendations--of-a-specific-build"></a>12,7. Preporuke korisnika (od određene Sastavi)

Korisnik preporuke od određene Sastavi vrste "Preporuke".

U API će vratiti popis predviđene stavke prema povijest korištenja korisnika (koja se koristi u određenim sastavljanje).

Napomena: Postoji nema korisnika preporuku za sastavljanje FBT.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27`|


|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| ID korisnika  | Jedinstveni identifikator korisnika |
| numberOfResults | Broj potrebnih rezultata |
| includeMetatadata | Buduću upotrebu, uvijek false |
| buildId | Sastavi id koji će se koristiti za zahtjev za preporuke |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200


Odgovor sadrži jednu stavku po stavku preporučene. Svaka stavka ima sljedeće podatke:
- `Feed\entry\content\properties\Id`-ID preporučene stavke.
- `Feed\entry\content\properties\Name`-Naziv stavke.
- `Feed\entry\content\properties\Rating`-Ocjena preporuke; veći broj označava veći pouzdanosti.
- `Feed\entry\content\properties\Reasoning`-Preporuke zaključivanja (npr. preporuke objašnjenja).

Pogledajte primjer odgovor u 12.1


###<a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a>12,8. Preporuke korisnika s stavku popisa (određene Sastavi)

Zatražite preporuke Korisnički određene Sastavi vrste "Preporuke" i druge stavke na popisu.

U API će vratiti popis predviđene stavke prema povijest korištenja korisnika i popis dodatnih stavki.

Napomena: Tthere je nema korisnika preporuku za sastavljanje FBT.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27`|



|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| ID korisnika  | Jedinstveni identifikator korisnika |
| ItemId-ovi | Odvojenih zarezom popis stavki kojom se preporučuje za. Maksimalna duljina: 1024 |
| numberOfResults | Broj potrebnih rezultata |
| includeMetatadata | Buduću upotrebu, uvijek false |
| buildId | Sastavi id koji će se koristiti za zahtjev za preporuke |
| apiVersion | 1.0 |

**Odgovor:**

Šifra stanja HTTP: 200


Odgovor sadrži jednu stavku po stavku preporučene. Svaka stavka ima sljedeće podatke:
- `Feed\entry\content\properties\Id`-ID preporučene stavke.
- `Feed\entry\content\properties\Name`-Naziv stavke.
- `Feed\entry\content\properties\Rating`-Ocjena preporuke; veći broj označava veći pouzdanosti.
- `Feed\entry\content\properties\Reasoning`-Preporuke zaključivanja (npr. preporuke objašnjenja).

Pogledajte primjer odgovor u 12.1

##<a name="13-user-usage-history"></a>13. povijest korištenja korisnika
Kada se model preporuke gradnje sustav će dopustiti dohvatiti povijest korisnika (stavke povezane određenog korisnika) koristi za sastavljanje.
Taj API omogućuju dohvaćanje povijest korisnika

Napomena: povijest korisnik trenutno je dostupna samo za preporuke izdanja.

###<a name="131-retrieve-user-history"></a>13,1 dohvatiti povijest korisnika
Dohvaćanje popisa stavki koje se koriste u aktivni sastavljanje ili navedeni sastavljanje za dani korisničkog id-a.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     | Otkrijte prošlost korisnika za aktivni sastavljanje.<br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/>Otkrijte prošlost korisnika za dani sastavljanje`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`<br/><br/>Primjer:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`|


|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela.|
| ID korisnika | Jedinstveni identifikator korisnika.
| buildId | Neobavezan parametar Dopusti da biste naznačili iz koje međuverzije povijest korisnik mora biti dohvaćanje
| apiVersion | 1.0 |


**Odgovor:**

Šifra stanja HTTP: 200

Odgovor sadrži jednu stavku po stavku preporučene. Svaka stavka ima sljedeće podatke:
- `Feed\entry\content\properties\Id`-ID preporučene stavke.
- `Feed\entry\content\properties\Name`-Naziv stavke.
- `Feed\entry\content\properties\Rating`-N/D.
- `Feed\entry\content\properties\Reasoning`-N/D.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

##<a name="14-notifications"></a>14. obavijesti
Preporuke za učenje Azure strojno stvara obavijesti kada se dogodi trajnog pogrešaka u sustavu. Postoje 3 vrste obavijesti:
1.  Nije uspjelo sastavljanje - Ova obavijest aktivira se za svaki Sastavi.
2.  Nabava podataka obradu pogreške - Ova obavijest aktivira se kada imamo više od 100 pogrešaka u zadnji pet minuta obrada događaja korištenja po modelu.
3.  Preporuka potrošnje pogreška – Ova obavijest aktivira se kada imamo više od 100 pogrešaka u zadnji pet minuta obrade zahtjeva za preporuke u modelu.


###<a name="141-get-notifications"></a>14.1. Primanje obavijesti
Dohvaća sve obavijesti za sve modela ili jedan model.

| HTTP metoda | URI-JA |
|:--------|:--------|
|POČETAK     |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Preuzimanje svih obavijesti za sve modela:<br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br>Primjer za početak obavijesti za konkretan model:<br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277`|


|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Neobavezan parametar. Kad je ispušten, dobit ćete sve obavijesti za sve modele. <br>Valjana vrijednost: Jedinstveni identifikator modela.|
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor:**

Šifra stanja HTTP: 200

OData XML

    The response includes one entry per notification. Each entry has the following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

###<a name="142-delete-model-notifications"></a>14.2. Brisanje obavijesti modela
Briše sve dodatne obavijesti u modelu.

| HTTP metoda | URI-JA |
|:--------|:--------|
|BRISANJE     |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br>Primjer:<br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27`|


|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| modelId | Jedinstveni identifikator modela |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200

###<a name="143-delete-user-notifications"></a>14,3. Brisanje korisnika obavijesti
Briše sve obavijesti za sve modele.

| HTTP metoda | URI-JA |
|:--------|:--------|
|BRISANJE     |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27`|


|   Naziv parametra  |   Valjane vrijednosti                        |
|:--------          |:--------                              |
| apiVersion | 1.0 |
|||
| Zahtjev za tijelo | NIŠTA |

**Odgovor**:

Šifra stanja HTTP: 200




##<a name="15-legal"></a>15. pravne
Dokument se isporučuje "kao-je". Informacije i prikaze izražena u ovom dokumentu, uključujući URL ADRESE i druge reference na Internet Web-mjesta, može se promijeniti bez prethodne najave.<br><br>
Primjeri koji se spominju u ovom dokumentu samo ilustracija i izmišljeni su. Ne stvarne povezanosti ili veze ili je slučajna.<br><br>
Ovaj dokument nije vam dati pravo na bilo kojem intelektualnog vlasništva bilo kojeg Microsoftova proizvoda. Možete kopirati i koristiti ovaj dokument za internu upotrebu kao referencu.<br><br>
© 2015 Microsoft. Microsoft Corporation.
 
