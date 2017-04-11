<properties
   pageTitle="Voditelj resursa predložak funkcije | Microsoft Azure"
   description="U članku se opisuje funkcija koristiti predložak Azure Voditelj resursa za dohvaćanje vrijednosti, rad s nizovima i numeričke vrijednosti i Dohvaćanje informacija za implementaciju."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-template-functions"></a>Azure funkcije predložak Voditelj resursa

U ovoj se temi opisuju sve funkcije možete koristiti u predlošku Azure Voditelj resursa.

Predložak funkcijama i njihovim parametrima su velika i mala slova. Na primjer, Voditelj resursa razrješava **variables('var1')** i **VARIABLES('VAR1')** kao isti. Kada se vrednuju, osim ako funkciju to izričito mijenja slučaj (primjerice toUpper ili toLower), funkcija zadržava slučaj. Za određene vrste resursa možda tretiraju slučaja neovisno o tome kako se procjenjuju funkcije.

## <a name="numeric-functions"></a>Numeričke funkcije

Voditelj resursa nudi sljedeće funkcije za rad s cijelim brojevima:

- [Dodavanje](#add)
- [copyIndex](#copyindex)
- [DIV](#div)
- [INT](#int)
- [Mod](#mod)
- [mul](#mul)
- [Sub](#sub)


<a id="add" />
### <a name="add"></a>Dodavanje

**Dodavanje (operand1 operand2)**

Vraća zbroj dva navedenih cijelih brojeva.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Da    | Da biste dodali cijeli prvi broj.
| operand2                           |   Da    | Da biste dodali cijeli drugi broj.

Sljedeći primjer dodaje dva parametra.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to add"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to add"
        }
      }
    },
    ...
    "outputs": {
      "addResult": {
        "type": "int",
        "value": "[add(parameters('first'), parameters('second'))]"
      }
    }

<a id="copyindex" />
### <a name="copyindex"></a>copyIndex

**copyIndex(offset)**

Vraća trenutni indeksa u petlji iteracije. 

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| OFFSET                           |   ne    | Iznos da biste dodali trenutnu vrijednost iteracije.

Ova funkcija uvijek koristi s objektom **Kopiraj** . Opis načina koristiti **copyIndex**potražite u članku [Stvaranje više instanci resursa u Azure Voditelj resursa](resource-group-create-multiple.md).

Sljedeći primjer prikazuje petlje kopiju i vrijednost indeksa uključeni u nazivu. 

    "resources": [ 
      { 
        "name": "[concat('examplecopy-', copyIndex())]", 
        "type": "Microsoft.Web/sites", 
        "copy": { 
          "name": "websitescopy", 
          "count": "[parameters('count')]" 
        }, 
        ...
      }
    ]


<a id="div" />
### <a name="div"></a>DIV

**DIV (operand1, operand2)**

Vraća cijeli broj s dva navedenih cijelih brojeva.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Da    | Cijeli broj koji se dijeli.
| operand2                           |   Da    | Cijeli broj koji se koristi za dijeljenje. Ne mogu biti 0.

U sljedećem primjeru dijeli jedan parametar drugi parametrom.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "divResult": {
        "type": "int",
        "value": "[div(parameters('first'), parameters('second'))]"
      }
    }

<a id="int" />
### <a name="int"></a>INT

**INT(valueToConvert)**

Navedena vrijednost pretvara u cijeli broj.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Da    | Vrijednost za pretvorbu cijeli broj. Vrsta vrijednosti može samo niz ili cijeli broj.

U sljedećem primjeru pretvara vrijednost parametra navedene za korisnika u cijeli broj.

    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    }


<a id="mod" />
### <a name="mod"></a>Mod

**MOD (operand1, operand2)**

Vraća ostatak dijeljenja cijeli broj pomoću dva navedenih cijelih brojeva.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Da    | Cijeli broj koji se dijeli.
| operand2                           |   Da    | Cijeli broj koji se koristi da biste dijelili, ne može se razlikovati od 0.

Sljedeći primjer Vraća ostatak dijeljenja jedan parametar drugi parametrom.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "modResult": {
        "type": "int",
        "value": "[mod(parameters('first'), parameters('second'))]"
      }
    }

<a id="mul" />
### <a name="mul"></a>mul

**mul (operand1, operand2)**

Vraća množenja dva navedenih cijelih brojeva.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Da    | Da biste pomnožili cijeli prvi broj.
| operand2                           |   Da    | Da biste pomnožili cijeli drugi broj.

U sljedećem primjeru množi jedan parametar drugi parametrom.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to multiply"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to multiply"
        }
      }
    },
    ...
    "outputs": {
      "mulResult": {
        "type": "int",
        "value": "[mul(parameters('first'), parameters('second'))]"
      }
    }

<a id="sub" />
### <a name="sub"></a>Sub

**Sub (operand1, operand2)**

Vraća operator oduzimanja dva navedenih cijelih brojeva.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Da    | Cijeli broj oduzima se od.
| operand2                           |   Da    | Cijeli broj koji je oduzeti.

U sljedećem primjeru oduzima se jedan parametar iz nekog drugog parametra.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer subtracted from"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer to subtract"
        }
      }
    },
    ...
    "outputs": {
      "subResult": {
        "type": "int",
        "value": "[sub(parameters('first'), parameters('second'))]"
      }
    }

## <a name="string-functions"></a>Funkcije niza

Voditelj resursa nudi sljedeće funkcije za rad s nizovima:

- [base64](#base64)
- [Slika](#concat)
- [Duljina](#lengthstring)
- [padLeft](#padleft)
- [Zamjena](#replace)
- [Preskoči](#skipstring)
- [Podjela](#split)
- [niz](#string)
- [podniz](#substring)
- [Preuzimanje](#takestring)
- [toLower](#tolower)
- [toUpper](#toupper)
- [Obrezivanje](#trim)
- [uniqueString](#uniquestring)
- [URI-ja](#uri)


<a id="base64" />
### <a name="base64"></a>base64

**base64 (inputString)**

Vraća predstavljanje base64 niz.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| inputString                        |   Da    | Vrijednost niza da biste se vratili kao base64 prikaz.

Sljedeći primjer pokazuje kako koristiti funkciju base64.

    "variables": {
      "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
      "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

<a id="concat" />
### <a name="concat---string"></a>Slika - niza

**Slika (string1 string2, string3,...)**

Kombinira višestruke vrijednosti niza i vraća ulančani niz. 

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| Niz1                        |   Da    | Vrijednost niza za povezivanje.
| Dodatni nizova             |   ne     | Niz vrijednosti za spajanje.

Ova funkcija može potrajati bilo koji broj argumenta, a možete prihvatiti nizovi ili polja za parametre. Primjer Slaganje polja, potražite u članku [Slika - polja](#concatarray).

Sljedeći primjer prikazuje način za kombiniranje više vrijednosti niza da biste se vratili ulančani niz.

    "outputs": {
        "siteUri": {
          "type": "string",
          "value": "[concat('http://', reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
        }
    }


<a id="lengthstring" />
### <a name="length---string"></a>Duljina - niza

**Length(string)**

Vraća broj znakova u nizu.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| niz                        |   Da    | Vrijednost niza koji želite koristiti za početak broj znakova.

Primjer duljine pomoću polja, potražite u članku [duljine - polja](#length).

Sljedeći primjer vraća broj znakova u nizu. 

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "nameLength": "[length(parameters('appName'))]"
    }
        

<a id="padleft" />
### <a name="padleft"></a>padLeft

**padLeft (valueToPad, totalLength, paddingCharacter)**

Vraća niz desnog dodavanjem znakova s lijeve strane do Dostizanje ukupni zadane duljine.
  
| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| valueToPad                         |   Da    | Niz ili int desno poravnanje.
| totalLength                        |   Da    | Ukupan broj znakova u vraćenom nizu.
| paddingCharacter                   |   ne     | Znak koji želite koristiti za lijevo – udaljenosti od ruba dok se dosegne ukupne duljine. Zadana vrijednost je prostor.

Sljedeći primjer pokazuje kako tipkovnicu vrijednost parametra navedene za korisnika tako da dodate na znak za nulu dok niz dođe do 10 znakova. Ako je izvorni vrijednost parametra dulje od 10 znakova, dodaju se bez znakova.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "paddedAppName": "[padLeft(parameters('appName'),10,'0')]"
    }

<a id="replace" />
### <a name="replace"></a>Zamjena

**Zamijeni (originalString, oldCharacter, newCharacter)**

Vraća novi niz s sve instance jednog znaka u navedenim nizom zamjenjuje neki drugi znak.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| originalString                     |   Da    | Niz koji sadrži sve instance jednog znaka zamjenjuje neki drugi znak.
| oldCharacter                       |   Da    | Znak biti uklonjen iz izvornog niza.
| newCharacter                       |   Da    | Znak za dodavanje umjesto uklonjene znak.

Sljedeći primjer prikazuje način da biste uklonili sve crtice iz niza navedene za korisnika.

    "parameters": {
        "identifier": { "type": "string" }
    },
    "variables": { 
        "newidentifier": "[replace(parameters('identifier'),'-','')]"
    }

<a id="skipstring" />
### <a name="skip---string"></a>Preskoči - niza
**Preskoči (originalValue, numberToSkip)**

Vraća niz s sve znakove nakon navedenog broja u nizu.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| originalValue                      |   Da    | Niz koji se koristi za preskakanje.
| numberToSkip                       |   Da    | Broj znakova da biste preskočili. Ako je vrijednost 0 ili manje, vraćaju se svi znakovi u nizu. Ako je veći od duljine niza, vraća se prazan niz. 

Primjer Preskoči pomoću polja, potražite u članku [preskočiti - polja](#skip).

U sljedećem primjeru preskače na određeni broj znakova u nizu.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for skipping"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[skip(parameters('first'),parameters('second'))]"
      }
    }


<a id="split" />
### <a name="split"></a>Podjela

**Podjela (inputString, delimiterString)**

**Podjela (inputString, delimiterArray)**

Vraća polje nizova koja sadrži podnizovi unos niza koji su zarezima prema navedenom graničnike.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| inputString                        |   Da    | Niz za podjelu.
| Graničnik                          |   Da    | Graničnik da biste koristili, može biti jednog niza ili polja nizova.

U sljedećem primjeru dijeli niz sa zarezom.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "stringPieces": "[split(parameters('inputString'), ',')]"
    }

U sljedećem primjeru dijeli niz zarezom ili točkom sa zarezom.

    "variables": {
      "stringToSplit": "test1,test2;test3",
      "delimiters": [ ",", ";" ]
    },
    "resources": [ ],
    "outputs": {
      "exampleOutput": {
        "value": "[split(variables('stringToSplit'), variables('delimiters'))]",
        "type": "array"
      }
    }

<a id="string" />
### <a name="string"></a>niz

**String(valueToConvert)**

Pretvara navedena vrijednost niza.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Da    | Vrijednost za pretvaranje niz. Svaka vrsta vrijednosti se može pretvoriti, uključujući objekata i polja.

U sljedećem primjeru pretvara vrijednosti parametra navedene za korisnika u nizove.

    "parameters": {
      "jsonObject": {
        "type": "object",
        "defaultValue": {
          "valueA": 10,
          "valueB": "Example Text"
        }
      },
      "jsonArray": {
        "type": "array",
        "defaultValue": [ "a", "b", "c" ]
      },
      "jsonInt": {
        "type": "int",
        "defaultValue": 5
      }
    },
    "variables": { 
      "objectString": "[string(parameters('jsonObject'))]",
      "arrayString": "[string(parameters('jsonArray'))]",
      "intString": "[string(parameters('jsonInt'))]"
    }

<a id="substring" />
### <a name="substring"></a>podniz

**podniz (stringToParse, startIndex, Duljina)**

Vraća podniz koji počinje položaj znaka navedeni i sadrži određenog broja znakova.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| stringToParse                     |   Da    | Izvorni niz iz kojeg je dobivenih podniz.
| startIndex                         | ne      | Polje početni položaj znaka za podniz.
| Duljina                             | ne      | Broj znakova za podniz.

U sljedećem primjeru izdvaja prva tri znaka iz parametra.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "prefix": "[substring(parameters('inputString'), 0, 3)]"
    }

<a id="takestring" />
### <a name="take---string"></a>Preuzimanje - niza
**preuzimanje (originalValue, numberToTake)**

Vraća niz s određenog broja znakova s početka niza.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| originalValue                      |   Da    | Niz da bi znakova iz.
| numberToTake                       |   Da    | Broj znakova za preuzimanje. Ako je vrijednost 0 ili manje, vraća se prazan niz. Ako je veći od duljine niza za dani, vraćaju se svi znakovi u nizu.

Primjer preuzimanje pomoću polja, potražite u članku [Preuzimanje - polja](#take).

U sljedećem primjeru vodi na određeni broj znakova iz niza.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for taking"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[take(parameters('first'), parameters('second'))]"
      }
    }

<a id="tolower" />
### <a name="tolower"></a>toLower

**toLower(stringToChange)**

Navedenim nizom pretvara u mala slova.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Da    | Niz koji se pretvoriti u mala slova.

U sljedećem primjeru pretvara vrijednost parametra navedene za korisnika u mala slova.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "lowerCaseAppName": "[toLower(parameters('appName'))]"
    }

<a id="toupper" />
### <a name="toupper"></a>toUpper

**toUpper(stringToChange)**

Pretvara navedenim nizom pisani velikim slovima.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Da    | Niz koji se pretvoriti u velika slova.

U sljedećem primjeru pretvara vrijednost parametra navedene za korisnika u velika slova.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "upperCaseAppName": "[toUpper(parameters('appName'))]"
    }

<a id="trim" />
### <a name="trim"></a>Obrezivanje

**Funkcija TRIM (stringToTrim)**

Uklanja sve znakove razmaka početku i kraju s navedenim nizom.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| stringToTrim                       |   Da    | Niz koji se Obreži.

U sljedećem primjeru obrezuje znakove razmaka iz vrijednost parametra navedene za korisnika.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "trimAppName": "[trim(parameters('appName'))]"
    }

<a id="uniquestring" />
### <a name="uniquestring"></a>uniqueString

**uniqueString (baseString,...)**

Stvara deterministic raspršivanje niza na temelju vrijednosti navedene kao parametara. 

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| baseString      |   Da    | Niz koji se koristi u funkciji raspršivanje da biste stvorili jedinstveni niz.
| dodatni parametri potrebi    | ne       | Možete dodati proizvoljan broj nizove po potrebi da biste stvorili vrijednost koja određuje razinu jedinstvenosti.

Ova funkcija je korisno kada je potrebno da biste stvorili jedinstveni naziv resursa. Sadrže vrijednosti parametara za ograničavanje opsega jedinstvenosti rezultata. Možete odrediti je li naziv jedinstven prema dolje do pretplate, grupu resursa ili implementacije. 

Vraćena vrijednost nije slučajni niza, ali umjesto rezultat funkcije raspršivanje. Vraćena vrijednost je 13 znakova. Nije globalno jedinstveni. Preporučujemo vam da biste spojili vrijednosti s prefiksom iz vaše konvencija imenovanja da biste stvorili smisleni naziv. Sljedeći primjer prikazuje oblika vraćene vrijednosti. Naravno, stvarna vrijednost razlikovat će se po navedene parametrima.

    tcvhiyu5h2o5o

Sljedeći primjeri pokazuju kako koristiti uniqueString za stvaranje jedinstvenih vrijednosti za najčešće korištenih razine.

Jedinstveni iz djelokruga pretplate

    "[uniqueString(subscription().subscriptionId)]"

Jedinstveni iz djelokruga grupa resursa

    "[uniqueString(resourceGroup().id)]"

Jedinstveni iz djelokruga implementacije za grupu resursa

    "[uniqueString(resourceGroup().id, deployment().name)]"
    
Sljedeći primjer prikazuje način da biste stvorili jedinstveni naziv računa za pohranu koji se temelji na grupu resursa (u ovu grupu resursa naziv nije jedinstveni if konstruirana na isti način kao).

    "resources": [{ 
        "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
        "type": "Microsoft.Storage/storageAccounts", 
        ...



<a id="uri" />
### <a name="uri"></a>URI-ja

**URI (baseUri, relativeUri)**

Stvara apsolutne URI kombiniranjem na baseUri i relativeUri niz.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| baseUri                            |   Da    | Niz osnovni uri.
| relativeUri                        |   Da    | Niz relativni uri da biste dodali osnovni uri niz.

Vrijednost za parametar **baseUri** možete obuhvatiti određenom datotekom, no osnovni put se koristi tijekom izgradnje URI. Na primjer, prosljeđivanje **http://contoso.com/resources/azuredeploy.json** kao u baseUri osnovni URI **http://contoso.com/resources/**u rezultira parametar.

Sljedeći primjer prikazuje način za sastavljanje vezu ugniježđene predložak koji se temelji na vrijednost nadređene predloška.

    "templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"

## <a name="array-functions"></a>Funkcije polja

Voditelj resursa nudi nekoliko funkcija za rad s vrijednostima polja.

- [Slika](#concatarray)
- [Duljina](#length)
- [Preskoči](#skip)
- [Preuzimanje](#take)

Polje s vrijednostima niz razgraničeno prema vrijednosti, pročitajte članak [podijeliti](#split).

<a id="concatarray" />
### <a name="concat---array"></a>Slika - polja

**Slika (Polje1, Polje2; polje3,...)**

Kombinira više polja i vraća povezanih polja. 

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| polje1                        |   Da    | Polja za spajanje.
| dodatna polja             |   ne     | Polja za spajanje.

Ova funkcija može potrajati bilo koji broj argumenta, a možete prihvatiti nizovi ili polja za parametre. Primjer Ulančavanje vrijednosti niza, potražite u članku [Slika - niz](#concat).

Sljedeći primjer prikazuje način da biste spojili dva polja.

    "parameters": {
        "firstarray": {
            type: "array"
        }
        "secondarray": {
            type: "array"
        }
     },
     "variables": {
         "combinedarray": "[concat(parameters('firstarray'), parameters('secondarray'))]
     }
        

<a id="length" />
### <a name="length---array"></a>Duljina - polja

**Length(array)**

Vraća broj elemenata u polju.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| polja                        |   Da    | Polja za početak broj elemenata.

Koristite ovu funkciju s polja da biste naveli broj iteracija prilikom stvaranja resursi. U sljedećem primjeru parametar **siteNames** želite uputiti polja imena koja želite koristiti za stvaranje web-mjesta.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

Dodatne informacije o korištenju ove funkcije s polja potražite u članku [Stvaranje više instanci resursa u Azure Voditelj resursa](resource-group-create-multiple.md). 

Primjer korištenja duljina niza vrijednošću, potražite u članku [duljine - niz](#lengthstring).

<a id="skip" />
### <a name="skip---array"></a>Preskakanje - polja
**Preskoči (originalValue, numberToSkip)**

Vraća polje sa svih elemenata nakon navedenog broja u polju.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| originalValue                      |   Da    | Polja za preskakanje.
| numberToSkip                       |   Da    | Broj elemenata da biste preskočili. Ako je vrijednost 0 ili manje, vraćaju se svi elementi u polju. Ako je veći od duljine polja, vraća se prazan polja. 

Primjer korištenja Preskoči nizom, potražite u članku [preskočiti - niz](#skipstring).

U sljedećem primjeru preskače na određeni broj elementi u polju.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for skipping"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[skip(parameters('first'), parameters('second'))]"
      }
    }

<a id="take" />
### <a name="take---array"></a>iskoristite - polja
**preuzimanje (originalValue, numberToTake)**

Vraća polje s navedeni broj elemenata od početka polja.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| originalValue                      |   Da    | Polja da biste preuzeli elementi iz.
| numberToTake                       |   Da    | Broj elemenata da biste preuzeli. Ako je vrijednost 0 ili manje, vraća se prazan polja. Ako je veći od duljine navedene polja, vraćaju se svi elementi u polju.

Primjer korištenja Upoznavanje s nizom, potražite u članku [Preuzimanje - niz](#takestring).

U sljedećem primjeru vodi na određeni broj elemenata iz polja.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for taking"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[take(parameters('first'),parameters('second'))]"
      }
    }

## <a name="deployment-value-functions"></a>Uvođenje vrijednost funkcije

Voditelj resursa nudi sljedeće funkcije za dohvaćanje vrijednosti iz sekcije predloška i vrijednosti koje su vezane uz implementaciju:

- [uvođenje](#deployment)
- [Parametri](#parameters)
- [varijable](#variables)

Vrijednosti iz resursa, grupa resursa ili pretplate, pročitajte članak [funkcije resursa](#resource-functions).

<a id="deployment" />
### <a name="deployment"></a>uvođenje

**Deployment()**

Vraća informacije o trenutni postupak implementacije.

Ova funkcija vraća objekt koji se prenosi tijekom implementacije. Svojstva vraćenog objekta razlikuju se ovisno o li proslijeđena implementacije objekt kao veze ili kao objekt u retku. 

Kada se objekt implementacije se prenosi u retku, kao što su kada koristite parametar **- TemplateFile** u Azure PowerShell tako da pokazuje na lokalne datoteke, vraćenog objekta s oblikovanjem sljedeće:

    {
        "name": "",
        "properties": {
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [
                ],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Kada objekt prosljeđuje kao vezu, kao što su kada koristite parametar **- TemplateUri** da upućuju na udaljenom objekta, objekt se vraća u sljedećem obliku. 

    {
        "name": "",
        "properties": {
            "templateLink": {
                "uri": ""
            },
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Sljedeći primjer pokazuje kako koristiti deployment() da biste se povezali URI nadređenog predloška na temelju predloška.

    "variables": {  
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
    }  

<a id="parameters" />
### <a name="parameters"></a>Parametri

**Parametri (parameterName)**

Vraća vrijednost parametra. Naziv Navedeni parametar mora biti definirano u odjeljku parametara predloška.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| parameterName                      |   Da    | Naziv parametra da biste se vratili.

Sljedeći primjer prikazuje pojednostavljeni Upotreba funkcije parametara.

    "parameters": { 
      "siteName": {
          "type": "string"
      }
    },
    "resources": [
       {
          "apiVersion": "2014-06-01",
          "name": "[parameters('siteName')]",
          "type": "Microsoft.Web/Sites",
          ...
       }
    ]

<a id="variables" />
### <a name="variables"></a>varijable

**varijable (variableName)**

Vraća vrijednost varijable. U odjeljku varijable predloška, potrebno je definirati varijable čiji je naziv naveden.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| Naziv varijable                      |   Da    | Naziv varijable da biste se vratili.

Sljedeći primjer koristi varijable vrijednost.

    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
      }
    ],

## <a name="resource-functions"></a>Funkcija resursa

Voditelj resursa nudi sljedeće funkcije za dohvaćanje vrijednosti resursa:

- [listKeys i popis {Value}](#listkeys)
- [Davatelji usluga](#providers)
- [Referenca](#reference)
- [resourceGroup](#resourcegroup)
- [resourceId](#resourceid)
- [pretplate](#subscription)

Da biste dobili vrijednosti parametara, varijable ili trenutni implementaciju, potražite u odjeljku [implementacije vrijednost funkcije](#deployment-value-functions).

<a id="listkeys" />
<a id="list" />
### <a name="listkeys-and-listvalue"></a>listKeys i popis {Value}

**listKeys (resourceName ili resourceIdentifier, apiVersion)**

**Popis {Value} (resourceName ili resourceIdentifier, apiVersion)**

Vraća vrijednosti za svaku vrstu resursa koji podržava postupka popisa. Najčešći način korištenja je **listKeys**. 
  
| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| resourceName ili resourceIdentifier |   Da    | Jedinstveni identifikator resursa.
| apiVersion                         |   Da    | Verzija API stanja runtime resursa.

Sve operacije koje može biti počinje s **popisom** koristiti funkciju u predložak. Dostupne operacije obuhvaćaju **listKeys**, ali i operacija kao što je **popis**, **listAdminKeys**i **listStatus**. Da biste utvrdili koje vrste resursa imaju operacije na popisu, koristite sljedeću naredbu komponente PowerShell.

    Get-AzureRmProviderOperation -OperationSearchString *  | where {$_.Operation -like "*list*"} | FT Operation

Ili dohvatiti popis s EŽA Azure. U sljedećem primjeru dohvaća sve operacije **apiapps**i koristi utility JSON [jq](http://stedolan.github.io/jq/download/) da biste filtrirali samo operacije popisa.

    azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"

Na resourceId možete navesti pomoću [funkcije resourceId](#resourceid) ili koristeći oblik **{providerNamespace} / {resourceType} / {resourceName}**.

Sljedeći primjer prikazuje način da biste se vratili primarnih i sekundarnih ključeva s računa za pohranu u odjeljku izlaza.

    "outputs": { 
      "listKeysOutput": { 
        "value": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2016-01-01')]", 
        "type" : "object" 
      } 
    } 

Vraćenog objekta iz listKeys sastoji se od sljedećih oblika:

    {
      "keys": [
        {
          "keyName": "key1",
          "permissions": "Full",
          "value": "{value}"
        },
        {
          "keyName": "key2",
          "permissions": "Full",
          "value": "{value}"
        }
      ]
    }

<a id="providers" />
### <a name="providers"></a>Davatelji usluga

**Davatelji (providerNamespace, [resourceType])**

Vraća informacije o davatelja resursa i njegove vrste podržanih resursa. Ako ne navedete vrsta resursa, funkcija vraća podržane vrste za davatelja resursa.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| providerNamespace                  |   Da    | Prostor za naziv davatelja
| resourceType                       |   ne     | Vrsta resursa u navedenom prostoru naziva.

U sljedećem obliku, vraća se svaki podržana vrsta. Redoslijed polja ne zajamčiti.

    {
        "resourceType": "",
        "locations": [ ],
        "apiVersions": [ ]
    }

Sljedeći primjer pokazuje kako koristiti funkciju davatelja usluga:

    "outputs": {
        "exampleOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }

<a id="reference" />
### <a name="reference"></a>Referenca

**Referenca (resourceName ili resourceIdentifier, [apiVersion])**

Vraća objekt koji predstavlja stanje runtime drugi resurs.

| Parametar                          | Obavezno | Opis
| :--------------------------------: | :------: | :----------
| resourceName ili resourceIdentifier |   Da    | Naziv ili Jedinstveni identifikator resursa.
| apiVersion                         |   ne     | API verzija navedenih resursa. Ovaj parametar obuhvaćaju kada resurs nisu dodijeljeni resursi unutar istog predloška.

Funkcija **Referenca** izvedena njegovom vrijednošću iz stanja izvođenja i zbog toga ne mogu se koristiti u odjeljku varijable. Može se koristiti u odjeljku izlaze predloška.

Pomoću funkcije referencu koju implicitno deklarirati da jedan resurs ovisi o drugi resurs ako je referentni resource dodjeli unutar istog predloška. Ne morate koristiti svojstvo **dependsOn** . Funkcija se vrednuje dok referentni resource dovrši implementacije.

Sljedeći primjer odnosi se na račun za pohranu koji je implementiran u isti predložak.

    "outputs": {
        "NewStorage": {
            "value": "[reference(parameters('storageAccountName'))]",
            "type" : "object"
        }
    }

Sljedeći primjer odnosi se na račun prostora za pohranu koji je implementiran u ovaj predložak, ali postoji unutar iste grupe resursa kao resurse uvodi.

    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }

Možete dohvatiti određenu vrijednost iz vraćenog objekta, kao što su blob krajnjoj točki URI, kao što je prikazano u sljedećem primjeru.

    "outputs": {
        "BlobUri": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Sljedeći primjer odnosi se na račun za pohranu u grupu različitih resursa.

    "outputs": {
        "BlobUri": {
            "value": "[reference(resourceId(parameters('relatedGroup'), 'Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Svojstva objekta vraća **referencu** funkcija razlikuju se po vrsti resursa. Da biste pogledali svojstva nazive i vrijednosti za vrstu resursa, stvaranje jednostavne predloška koji vraća objekt u odjeljku **Proizvodi** . Ako imate postojeću resursa te vrste, predložak samo vraća objekt bez implementacija sve nove resurse. Ako nemate postojeći resurs te vrste, predložak uvodi tu vrstu i vraća objekt. Zatim dodajte tih svojstava za ostale predloške koje su potrebne za dinamično dohvaćanje vrijednosti tijekom implementacije. 

<a id="resourcegroup" />
### <a name="resourcegroup"></a>resourceGroup

**resourceGroup()**

Vraća objekt koji predstavlja trenutnu grupu resursa. 

Vraćeni je u sljedećem obliku:

    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
      "name": "{resourceGroupName}",
      "location": "{resourceGroupLocation}",
      "tags": {
      },
      "properties": {
        "provisioningState": "{status}"
      }
    }

Sljedeći primjer koristi lokacija grupu resursa za dodjelu mjesto za web-mjesta.

    "resources": [
       {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          ...
       }
    ]

<a id="resourceid" />
### <a name="resourceid"></a>resourceId

**resourceId ([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2] …)**

Vraća Jedinstveni identifikator resursa. 
      
| Parametar         | Obavezno | Opis
| :---------------: | :------: | :----------
| subscriptionId    |   ne     | Zadana je vrijednost trenutne pretplate. Kada je potrebno dohvatiti resursa u drugu pretplatu, navedite tu vrijednost.
| resourceGroupName |   ne     | Zadana je vrijednost trenutnu grupu resursa. Navedite tu vrijednost kada je potrebno dohvatiti resursa u drugu grupu resursa.
| resourceType      |   Da    | Vrsta resursa obuhvaća prostora za naziv resursa za davatelja usluga.
| resourceName1     |   Da    | Naziv resursa.
| resourceName2     |   ne     | Sljedeći resursa naziv segmenta ako je ugniježđeno resursa.

Ovu funkciju koristite kada je naziv resursa dvosmislene ili ne dodijeljenu unutar istog predloška. Identifikator se vraća u sljedećem obliku:

    /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/{resourceProviderNamespace}/{resourceType}/{resourceName}

Sljedeći primjer prikazuje način za dohvaćanje ID-ovi resursa za web-mjesta i baze podataka. Na web-mjestu postoji u grupu resursa pod nazivom **myWebsitesGroup** i baza podataka postoji u trenutnoj grupi resursa za ovaj predložak.

    [resourceId('myWebsitesGroup', 'Microsoft.Web/sites', parameters('siteName'))]
    [resourceId('Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]
    
Često morate ovu funkciju koristite kada putem računa za pohranu ili virtualne mreže u grupi zamjenski resursa. Račun za pohranu ili virtualne mreže može se koristiti u više grupa resursa; Zbog toga ne želite ih izbrisati prilikom brisanja grupa jedan resursa. Sljedeći primjer pokazuje kako resursa iz grupe vanjski resurs jednostavno može koristiti:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "virtualNetworkName": {
              "type": "string"
          },
          "virtualNetworkResourceGroup": {
              "type": "string"
          },
          "subnet1Name": {
              "type": "string"
          },
          "nicName": {
              "type": "string"
          }
      },
      "variables": {
          "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
      },
      "resources": [
      {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[parameters('nicName')]",
          "location": "[parameters('location')]",
          "properties": {
              "ipConfigurations": [{
                  "name": "ipconfig1",
                  "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "subnet": {
                          "id": "[variables('subnet1Ref')]"
                      }
                  }
              }]
           }
      }]
    }

<a id="subscription" />
### <a name="subscription"></a>pretplate

**Subscription()**

Vraća Detalji o pretplati u sljedećem obliku.

    {
        "id": "/subscriptions/#####",
        "subscriptionId": "#####",
        "tenantId": "#####"
    }

Sljedeći primjer prikazuje funkciju pretplate naziva u odjeljku izlaza. 

    "outputs": { 
      "exampleOutput": { 
          "value": "[subscription()]", 
          "type" : "object" 
      } 
    } 


## <a name="next-steps"></a>Daljnji koraci
- Opis sekcije u predlošku Azure upravljanja resursima potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md)
- Da biste spojili većeg broja predložaka, potražite u članku [Rad s predlošcima povezane s Azure Voditelj resursa](resource-group-linked-templates.md)
- Iteracija određeni broj puta kada stvarate vrstu resursa, potražite u članku [Stvaranje više instanci resursa u Azure Voditelj resursa](resource-group-create-multiple.md)
- Da biste vidjeli kako implementirati predložak koji ste stvorili, potražite u članku [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md)

