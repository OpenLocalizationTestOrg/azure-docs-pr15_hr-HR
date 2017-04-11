<properties
   pageTitle="Logika aplikacije petlje, opsega i Debatching | Microsoft Azure"
   description="Logika aplikacije petlje, opseg i debatching koncepti"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logika aplikacije petlje, opsega i Debatching
  
>[AZURE.NOTE] Ovu verziju članka primjenjuje shemu logike 2016 aplikacije-04-01 – pregled i novijim verzijama.  Koncepti slični su za starije sheme, no dosezi samo su dostupne za ovu shemu i noviji.
  
## <a name="foreach-loop-and-arrays"></a>Petlja ForEach i polja
  
Logika aplikacija omogućuje Ponavljaj za skup podataka i izvođenje akcije za svaku stavku.  To je moguće putem na `foreach` akcija.  U dizajneru, možete odrediti da biste dodali na za svaki petlji.  Nakon odabira polja koje želite ponavljanje, možete početi s dodavanjem akcija.  Trenutno su ograničen na samo jednu akciju po foreach petlje, ali to ograničenje će biti lifted u vraćaju tjedana.  Jednom unutar petlje možete početi da biste odredili što se događa pri svakoj vrijednosti polja.

Ako koristite prikaz koda, možete odrediti na za svaki petlji kao što su ispod.  Ovo je primjer programa za svaki petlja koje šalje poruku e-pošte za svaku adresu e-pošte koja sadrži "microsoft.com":

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` akcija možete iteracija preko polja do 5000 redaka.  Iteracija možete izvršiti paralelno, pa možda će biti potrebno da biste dodali poruke reda potrebi tok.
  
## <a name="until-loop"></a>Do petlje
  
  Dok je uvjet zadovoljen možete izvoditi akcije ili niza akcija.  Najčešći scenarij za to je poziva krajnje dok ne dobijete odgovor koji tražite.  U dizajneru, možete odrediti da biste dodali programa do petlje.  Kad dodate akcije unutar petlje, možete postaviti izlazni uvjet, kao što se događa ograničenja.  Postoji 1 minute odgode između ciklusa petlje.
  
  Ako koristite prikaz koda, možete odrediti programa dok se ne sviđa petlja ispod.  Ovo je primjer pozivanje krajnje HTTP dok tijelo odgovor ima vrijednost "Dovršeno".  To će kada dovršite oba 
  
  * HTTP odgovor ima status "Dovršeno"
  * Je pokušala 1 h
  * To je looped 100 puta
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn i Debatching

Ponekad okidač primiti polja stavki koje želite debatch i pokrenuti tijek rada po stavku.  To je moguće napraviti putem na `spliton` naredbe.  Po zadanom, ako vaš swagger okidača određuje teret koji je polja, u `spliton` dodat će se i započnite Pokreni po stavku.  SplitOn mogu se dodati samo okidač.  To se može ručno konfigurirati ili nadjačati u kod prikaza definicija.  Trenutno možete debatch SplitOn polja do 5000 stavki.  Ne mogu imati u `spliton` i i implementirati uzorak syncronous odgovor.  Svaki tijek rada pod nazivom koji je na `response` akciju uz `spliton` će se izvoditi asyncronously i pošaljite neposrednu `202 Accepted` odgovor.  

SplitOn može biti naveden u prikazu koda kao u sljedećem primjeru.  Dobiva polja stavki i debatches za svaki redak.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Dosega

Moguće je da biste grupirali niza akcija Suradnja pomoću opseg.  To je posebno korisno za implementaciju rukovanje iznimke.  U dizajneru možete dodati novi raspon i početi s dodavanjem radnje unutar njega.  Možete definirati opsege u kod prikaza kao što je sljedeća:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
