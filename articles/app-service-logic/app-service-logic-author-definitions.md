<properties 
    pageTitle="Autor aplikacije logike definicije | Microsoft Azure" 
    description="Saznajte kako napisati definiciju JSON za logike aplikacije" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>Autor aplikacije logike definicije
U ovoj se temi pokazuje kako koristiti definicije [Azure logike aplikacije](app-service-logic-what-are-logic-apps.md) koje je jednostavno, deklarativno JSON jezik. Ako niste učinili još, pogledajte [kako stvoriti novu aplikaciju logike](app-service-logic-create-a-logic-app.md) prvi put. Možete čitati i [materijala punu referencu definicija jezika na MSDN-u](http://aka.ms/logicappsdocs).

## <a name="several-steps-that-repeat-over-a-list"></a>Nekoliko koraka koje se ponavljaju iznad popisa

Na raspolaganju su [Vrsta foreach](app-service-logic-loops-and-scopes.md) da biste ponovili iznad polja do 10 k stavke i izvođenje akcija za svaki unos.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Korak za obradu pogreške ako nešto pošlo po redu

Najčešće želite omogućiti pisanje *olakšava korak* – neke logiku koja se izvršava, ako se **i samo ako je**, jedan ili više pozive nije uspjelo. U ovom primjeru smo su dohvaćanje podataka iz različitih mjesta, ali ne uspijete poziv, koje želim OBJAVITI poruku negdje da bi se može pronaći tu pogrešku kasnije:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Možete unijeti korištenje na `runAfter` svojstva da biste odredili na `postToErrorMessageQueue` izvoditi samo `readData` je **nije uspio**.  To može biti popis mogućih vrijednosti pa `runAfter` može biti `["Succeeded", "Failed"]`.

Na kraju, budući da su sada riješiti pogrešku, ne možemo više označiti Pokreni **nije uspjelo**. Kao što je ovdje možete vidjeti, ovoj instanci znači da **je uspjelo** čak i ako jedan korak nije uspjelo, korak za rukovanje to je napisao.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Dvije (ili više) koraka koji će se izvoditi paralelno

Da bi više izvođenja akcije paralelno, u `runAfter` svojstvo mora biti jednak tijekom rada. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Kao što je vidljivo u primjeru iznad, oba `branch1` i `branch2` postavljene na pokrenuti nakon `readData`. Zbog toga od ovih grana funkcionirat će u paralelno:

![Paralelno](./media/app-service-logic-author-definitions/parallel.png)

Vidjet ćete jednaka je vremenska oznaka oba grana. 

## <a name="join-two-parallel-branches"></a>Uključivanje dvije paralelne grana

Možete se uključiti dvije akcije koje su postavljeni za izvršavanje paralelno dodavanjem stavki na `runAfter` svojstvo slično iznad.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Paralelno](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Mapiranje stavki na popisu s nekoliko različitih konfiguracija

Sljedeći, recimo želimo da biste potpuno drugačije sadržaj ovisno o vrijednosti svojstva. Na karti vrijednosti odredišta možemo stvoriti kao parametar:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

U ovom slučaju da najprije se koristiti popis članaka, a zatim u drugom koraku pretražuje u kartu, koja se temelji na kategoriju koja je definirana kao parametar koji URL-a da biste sadržaj iz. 

Dvije stavke koje pozornost ovdje: u [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funkcija koristi da biste provjerili da biste vidjeli odgovara li kategoriju jednu od kategorija poznati definirani. Drugo, nakon što smo dobili kategoriju, ne možemo možete uvesti stavke karte pomoću uglatim zagradama: `parameters[...]`. 

## <a name="working-with-strings"></a>Rad s nizovima

Postoje različite funkcije koje se mogu koristiti za rukovanje niz. Pogledajmo primjer u kojem imamo niza koji želite proslijediti u sustavu, ali ne možemo nisu sigurni da kodiranje znakova će se rukovati pravilno. Jedan je base64 kodiranje niz. Međutim, da biste izbjegli escaping u URL-a ne možemo prelaska da biste zamijenili nekoliko znakova. 

Želimo podniz tog sustava u redoslijed naziv jer prvih 5 znakova koji se koriste.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Rad s unutrašnji izvan:

1. Početak u [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) u orderer naziva to vraća natrag ukupan broj znakova

2. Oduzimanje 5 (jer ćete želimo kraći niz)

3. Ustvari vode na [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Ne možemo Počni od indeksa `5` i otvorite ostatak niza.

4. Pretvaranje ovaj podniz da biste na [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) niza

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)sve se `+` znakova s`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)sve se `/` znakova s`_`

## <a name="working-with-date-times"></a>Rad s vremena

Vremena može biti korisno, osobito ako pokušate za izvlačenje podataka iz izvora podataka koji ne podržava prirodan **okidača**.  Da biste utvrdili izvodite koliko razne korake možete koristiti i vremena. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

U ovom primjeru smo izdvajanje u `startTime` od prethodnom koraku. Zatim ćemo početak trenutnog vremena i oduzimanja od jedne sekunde:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (druge vremenske jedinice nije moguće koristiti kao što su `minutes` ili `hours`). Na kraju, ne možemo možete usporediti te dvije vrijednosti. Ako prvi manja je od drugi, a zatim taj način više od jedne sekunde isteka jer je redoslijed u prvom smjestiti. 

I bilješke koje možete koristiti formatters niz oblikovanju datuma: u nizu upita koristiti [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) da biste na RFC1123. Sve datuma oblikovanja [navedenih na MSDN-u](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Pomoću parametara vrijeme uvođenja za različitim okruženjima

Da bi se životni ciklus implementacije na kojem imate razvojno okruženje, pripremna okruženja, a zatim radnog okruženja uobičajeno je. U sve te možda želite definiciju isti, ali koristite različite baze podataka, na primjer. Isto tako, trebali biste koristiti isti definition preko mnogo različitih područja za visoke dostupnosti, ali želite svaku instancu aplikacije logike razgovarati s bazom podataka to područje. 

Imajte na umu da se razlikuje od dovrši druge parametre prilikom *izvođenja*, trebali biste koristiti u `trigger()` funkcionirati kao što je istaknut iznad. 

Možete početi s definicijom vrlo simplistic poput ove:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Zatim u stvarni `PUT` zahtjeva za aplikaciju logike navedete parametar `uri`. Imajte na umu kao više ne postoji zadana vrijednost u opseg logike aplikacije potreban je taj parametar:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

U svakom okruženju pa možete unijeti neku drugu vrijednost za na `connection` parametar. 

Potražite u [dokumentaciji REST API -JA](https://msdn.microsoft.com/library/azure/mt643787.aspx) za sve mogućnosti za stvaranje i upravljanje logike aplikacije. 
