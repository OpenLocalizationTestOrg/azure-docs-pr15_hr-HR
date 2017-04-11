<properties
pageTitle="Mapiranje polja u indexers Azure pretraživanja"
description="Konfiguriranje mapiranja polja za indeksiranje pretraživanja Azure na račun za razlike u nazive polja i prikazi podataka"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Mapiranje polja u indexers Azure pretraživanja

Prilikom korištenja indexers Azure pretraživanja u slučajevima gdje ulazne podatke ne prilično odgovara shemi indeksa cilj povremeno možete pronaći sami. U tom slučaju možete koristiti **mapiranja polja** za pretvaranje podataka u željeni oblik. 

Neke situacije u kojima su korisne mapiranja polja:
 
- Izvor podataka sadrži polje `_id`, ali Azure pretraživanje ne dopušta počevši od podvlaku nazive polja. Mapiranje polja omogućuje "Preimenovanje" polja. 
- Želite popuniti nekoliko polja u indeksu s istim podacima izvora podataka, na primjer jer koju želite primijeniti različite analyzers ta polja. Mapiranje polja omogućuju "fork" polje izvora podataka.
- Morate za Base64 kodiranje ili dekodiranje podataka. Mapiranje polja podržava nekoliko **Mapiranje funkcije**, uključujući funkcije za Base64 kodiranje i dekodiranje.   


> [AZURE.IMPORTANT] Trenutno polje mapiranja funkcionalnost je u pretpregledu. On je dostupan samo u REST API-JA pomoću verzija **2015-02-28-pretpregled**. Imajte na umu, pretpregled API-ji su namijenjene testiranje i procjenu, a ne treba koristiti u radnom okruženju.

## <a name="setting-up-field-mappings"></a>Postavljanje mapiranje polja

Mapiranje polja možete dodati prilikom stvaranja novog pomoću [Stvaranje indeksiranje](search-api-indexers-2015-02-28-preview.md#create-indexer) API-JA za indeksiranje. Mapiranje polja na indeksiranja indeksiranje pomoću [Komponente za indeksiranje ažuriranje](search-api-indexers-2015-02-28-preview.md#update-indexer) API-JA možete upravljati. 

Mapiranje polja sastoji se od 3 dijelova: 

1. A `sourceFieldName`, koji predstavlja polja u izvoru podataka. Potreban je to svojstvo. 

2. Na neobavezno `targetFieldName`, koji predstavlja polja u indeksu pretraživanja. Ako se ispusti, koristi se isti naziv kao izvor podataka. 

3. Na neobavezno `mappingFunction`, koji mogu transformirati podataka na jedan od nekoliko unaprijed definirane funkcije. Cijeli popis funkcija je [ispod](#mappingFunctions).

Mapiranje polja dodaju se u `fieldMappings` polja na definiciju indeksiranje. 

Na primjer, Evo kako bi odgovarao razlike u nazivima polja: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Indeksiranje može imati više mapiranja polja. Na primjer, Evo kako vam može "fork" polja:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure pretraživanje koristi velika i mala slova usporedbe za razrješavanje imena polja i funkcija u mapiranja polja. To je praktično (ne morate dobar sve kućište), ali to znači da izvor podataka ili indeks ne može imati polja koje se razlikuju samo po predmetu.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Funkcije mapiranje polja

Trenutno podržane su sljedeće funkcije: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Izvodi *URL sigurnih* Base64 kodiranje unos niza. Pretpostavlja da je unos UTF-8 kodiran. 

#### <a name="sample-use-case"></a>Primjer koristi slučaja 

Samo URL sigurnih znakova mogu se pojaviti u dokument ključa Azure pretraživanja (jer korisnici moraju imati mogućnost adresa dokumenta, primjerice pomoću pretraživanja API-JA). Ako vaši podaci sadrže URL nesigurnih znakova, a želite koristiti za popunjavanje ključa polja u indeksu pretraživanja, pomoću te funkcije.   

#### <a name="example"></a>Primjer 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Izvodi Base64 dekodiranje niz. Unos pretpostavlja se da je u *URL-a sigurno* Base64 kodirani niz. 

#### <a name="sample-use-case"></a>Primjer koristi predmet 

Blob prilagođene metapodatke mora biti ASCII kodiran. Kodiranje Base64 možete koristiti za predstavljanje proizvoljne Unicode nizove u blob prilagođene metapodacima. Međutim, da biste pretraživanje smisleni, ova funkcija možete koristiti da biste uključili kodirani natrag u nizove "obične" kada puniti indeksu pretraživanja.  

#### <a name="example"></a>Primjer 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Dijeli niza polja pomoću navedeni graničnik pa izdvajanja token na navedeni položaj u stvorenom Podijeli.

Na primjer, ako je unos `Jane Doe`, `delimiter` je `" "`(razmak) i `position` je 0, rezultat je `Jane`; Ako u `position` je 1, rezultat je `Doe`. Ako položaj upućuje na token koji ne postoji, će se vraća pogreška.

#### <a name="sample-use-case"></a>Primjer koristi slučaja 

Sadrži izvor podataka u `PersonName` polja, a želite indeksirati kao dva zasebna `FirstName` i `LastName` polja. Ovu funkciju možete koristiti za podjelu unos korištenje znaka razmaka kao graničnik.

#### <a name="parameters"></a>Parametri

- `delimiter`: niz da biste koristili kao razdjelnik prilikom podjele niz.
- `position`: cjelobrojna polje položaj tokena da biste odabrali nakon podjele niz.    

#### <a name="example"></a>Primjer

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Pretvara niz koji je oblikovan kao JSON polja nizova u nizu polja koje je moguće koristiti za popunjavanje na `Collection(Edm.String)` u indeks. 

Na primjer, ako je niz unosa `["red", "white", "blue"]`, zatim ciljno polje Vrsta `Collection(Edm.String)` će popunjen tri vrijednosti `red`, `white` i `blue`. Za unos vrijednosti koje se ne mogu raščlaniti kao polja niz JSON, se vraća pogreška. 

#### <a name="sample-use-case"></a>Primjer koristi slučaja

Azure SQL baze podataka ne sadrži vrstu podataka ugrađene prirodan karte `Collection(Edm.String)` polja u Azure pretraživanja. Popunjavanje niz zbirke polja, oblikujte izvorišnih podataka kao polje niza JSON i pomoću te funkcije. 

#### <a name="example"></a>Primjer 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Pridonesite poboljšati Azure pretraživanja

Ako imate zahtjeve za značajke ili ideje poboljšanja, provjerite stupili nam na našem [UserVoice web-mjesta](https://feedback.azure.com/forums/263029-azure-search/).