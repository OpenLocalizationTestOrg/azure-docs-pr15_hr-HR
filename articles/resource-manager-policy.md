<properties
    pageTitle="Azure pravila upravljanja resursima | Microsoft Azure"
    description="U članku se opisuje kako pomoću pravila upravljanja resursima Azure da biste spriječili kršenja na različite opsege kao što su pretplate, grupa resursa ili pojedinačnih resursa."
    services="azure-resource-manager"
    documentationCenter="na"
    authors="ravbhatnagar"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="gauravbh;tomfitz"/>

# <a name="use-policy-to-manage-resources-and-control-access"></a>Pomoću pravila za upravljanje resursima i nadzor pristupa

Azure Voditelj resursa sada omogućuje kontrolu pristupa putem prilagođena pravila. S pravilima, možete spriječiti korisnika u tvrtki ili ustanovi iz prekidanje konvencije koji su potrebni za upravljanje resursima za vaše tvrtke ili ustanove. 

Stvaranje definicije pravila koja opisuje akcije ili resursa koji se izričito zabranjen. Dodijelite te definicije pravila u željenom dosegu, primjerice pretplatu, grupa resursa ili pojedinačnog resursa. 

U ovom se članku objašnjavaju smo osnovna struktura jezika za definiranje pravila koje možete koristiti za stvaranje pravila. Zatim ćemo opisuju načina primjene tih pravila na različite opsege.

## <a name="how-is-it-different-from-rbac"></a>Kako se razlikuje od RBAC?

Postoji nekoliko ključne razlike između pravila i kontrola pristupa na temelju uloga, ali je najprije da biste shvatili da pravila i RBAC zajedno. Da biste koristili pravila, mora se provjeriti preko RBAC. Za razliku od RBAC, pravila je zadani Dopusti i eksplicitne uskratiti sustava. 

RBAC usredotočuje se na Akcije koje **korisnik** može izvoditi na različite opsege. Ako, na primjer, određenom korisniku se dodaje ulogu suradnika za grupu resursa u željenom dosegu da bi korisnik mogao snimiti promjene u toj grupi resursa. 

Pravila usredotočuje se na **resursa** akcije na različite opsege. Ako, na primjer, putem pravila, možete odrediti vrste resursa koje možete dodjeli ili ograničavanje mjesta u kojima je moguće dodjeli resursa.

## <a name="common-scenarios"></a>Uobičajeni scenariji

Jedan scenarij uobičajeni je obavezan odjela oznake svrhu chargeback. Možda želite Dopusti operacije samo kada je povezan centru odgovarajuća cijena; tvrtke ili ustanove u suprotnom se odbiti zahtjev. Ovo pravilo štedi naplatiti odgovarajuća cijena centar za razne operacije obavljene.

Drugi uobičajeni scenarij je tvrtki ili ustanovi poželjeti kontrolu na mjestima na kojima se stvaraju resursi. Ili žele kontrolu pristupa resursima dopuštanjem samo određene vrste resursa koji će se resursi.

Isto tako, tvrtki ili ustanovi možete kontrolirati katalog servisa ili Nametni željeni konvencije imenovanja za resurse.

Pomoću pravila scenarija možete jednostavno se postići.

## <a name="policy-definition-structure"></a>Struktura definicija pravila

Definicija pravila stvara se JSON. Što se sastoji od jednog ili više uvjeta logički operatori koje definiraju Akcije, a efekta objašnjava što se događa kada su ispunjeni uvjete. Shema je objavljeno na [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

Zapravo, pravila sadrži sljedeće elemente:

**Operatora uvjet/logika:** skup uvjeta koja se može kroz skup logički operatori.

**Efekt:** što se događa kada se zadovolje uvjet – ili uskratiti ili nadzora. Efekt nadzora emits upozorenje događaj servisa. Na primjer, administrator može stvoriti pravila koja uzrokuje događaja nadzora ako svi stvara velike VM. Administrator možete kasnije pregledati zapisnike.

    {
      "if" : {
          <condition> | <logical operator>
      },
      "then" : {
          "effect" : "deny | audit | append"
      }
    }
    
## <a name="policy-evaluation"></a>Procjena pravila

Pravila se procjenjuju stvaranja resursi. U slučaju uvođenje predloška pravila vrednuju se tijekom stvaranja svakog resursa u predlošku. 

> [AZURE.NOTE] Trenutno pravila rezultirati vrste resursa koji ne podržavaju oznake, vrsta i mjesto, kao što su vrsta Microsoft.Resources/deployments resursa. Tu podršku dodat će se u budućim vrijeme. Da biste izbjegli probleme s kompatibilnost sa starijim verzijama, trebali biste izričito navesti vrsta kada pravilnike za stvaranje. Na primjer, oznake pravila koja odredite vrste primjenjuje se za sve vrste. U tom slučaju implementacije predložak možda neće funkcionirati ako postoji ugniježđene resurs koji ne podržava oznake, a vrsta resursa za implementaciju dodana procjene pravila. 

## <a name="logical-operators"></a>Logički operatori

Podržani logički operatori uz vidjet ćete da sintaksa su:

| Operator naziv     | Sintaksa         |
| :------------- | :------------- |
| Ne            | "ne": {&lt;uvjeta ili operator &gt;}             |
| I           | "allOf": [{&lt;uvjeta ili operator &gt;}, {&lt;uvjeta ili operator &gt;}] |
| Ili                         | "anyOf": [{&lt;uvjeta ili operator &gt;}, {&lt;uvjeta ili operator &gt;}] |

Voditelj resursa omogućuju vam da odredite složene logike u pravilo kroz ugniježđene operatore. Na primjer, možete ukinuti stvaranje resursa na određenom mjestu za vrstu navedenog resursa. Primjer ugniježđene operatora prikazano u nastavku.

## <a name="conditions"></a>Uvjeta

Uvjet vrednuje kao li **polje** ili **izvor** zadovoljava određene kriterije. Podržani uvjet imena i sintaksa su:

| Naziv uvjet | Sintaksa                |
| :------------- | :------------- |
| Jednako             | "jednako je": "&lt;vrijednost&gt;"               |
| Kao što su                  | "sviđa mi se": "&lt;vrijednost&gt;"                   |
| Sadrži          | "sadrži": "&lt;vrijednost&gt;"|
| U                        | "u": ["&lt;vrijednost1&gt;","&lt;vrijednost2&gt;"]|
| ContainsKey    | "containsKey": "&lt;keyName&gt;" |
| Postoji     | "postoji": "&lt;booleovom&gt;" |

### <a name="fields"></a>Polja

Uvjeti su oblikovane pomoću polja i izvora. Polje predstavlja svojstava u opseg resursa zahtjev koji se koristi za opisuju stanje resursa. Izvor predstavlja karakteristike zahtjeva za samu. 

Podržani su sljedeći polja i izvora:

Polja: **naziv**, **vrstu**, **Vrsta**, **mjesto**, **oznake** **oznake.** *, and * *Svojstvo Pseudonim**. 

### <a name="property-aliases"></a>Svojstvo pseudonima 
Svojstvo Pseudonim je naziv koji se može koristiti u definiciju pravila da biste pristupili resursa svojstava određene vrste, kao što su postavke i SKU-ove. Radi preko svih API verzija tamo gdje postoje svojstvo. Pseudonima mogu biti dohvaćeni pomoću REST API-JA prikazano u nastavku (Powershell podršku dodat će se u budućnosti):

    GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
    
Definicija pseudonima prikazano u nastavku. Kao što vidite, pseudonima definira putova u različitim verzijama API-JA, čak i ako nema promjena za naziv svojstva. 

    "aliases": [
        {
          "name": "Microsoft.Storage/storageAccounts/sku.name",
          "paths": [
            {
              "path": "properties.accountType",
              "apiVersions": [
                "2015-06-15",
                "2015-05-01-preview"
              ]
            },
            {
              "path": "sku.name",
              "apiVersions": [
                "2016-01-01"
              ]
            }
          ]
        }
    ]

Trenutno su podržane pseudonima:

| Pseudonim | Opis |
| ---------- | ----------- |
| {resourceType}/sku.name | Vrste resursa podržani su: Microsoft.Compute/virtualMachines,<br />Microsoft.Storage/storageAccounts,<br />Microsoft.Web/serverFarms,<br /> Microsoft.Scheduler/jobcollections,<br />Microsoft.DocumentDB/databaseAccounts,<br />Microsoft.Cache/Redis,<br />Microsoft.CDN/profiles |
| {resourceType}/sku.family | Podržani resursa vrsta je Microsoft.Cache/Redis |
| {resourceType}/sku.capacity | Podržani resursa vrsta je Microsoft.Cache/Redis |
| Microsoft.Compute/virtualMachines/imagePublisher |  |
| Microsoft.Compute/virtualMachines/imageOffer  |  |
| Microsoft.Compute/virtualMachines/imageSku  |  |
| Microsoft.Compute/virtualMachines/imageVersion  |  |
| Microsoft.Cache/Redis/enableNonSslPort |  |
| Microsoft.Cache/Redis/shardCount |  |
| Microsoft.SQL/servers/version |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveId |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveName |  |
| Microsoft.SQL/servers/databases/edition |  |
| Microsoft.SQL/servers/databases/elasticPoolName |  |
| Microsoft.SQL/servers/elasticPools/dtu |  |
| Microsoft.SQL/servers/elasticPools/edition |  |

Trenutno pravila funkcionira samo na zahtjeve za STAVI. 

## <a name="effect"></a>Efekt
Pravila podržava tri vrste efekt – **uskratiti** **nadzora**i **Dodavanje**. 

- Nemoj dopustiti generira događaja u zapisniku nadzora, a ne uspije zahtjev
- Nadzora generira događaja u zapisniku nadzora, ali neće uspjeti zahtjev
- Dodavanje dodaje definirani skup polja na zahtjev 

Za **Dodavanje**, navedite sljedeće detalje:

    ....
    "effect": "append",
    "details": [
      {
        "field": "field name",
        "value": "value of the field"
      }
    ]

Vrijednost može biti niz ili JSON oblikovanje objekta. 

## <a name="policy-definition-examples"></a>Primjeri definicija pravila

Sada Pogledajmo kako ćemo definirati pravila da biste postigli prethodnih slučajeva.

### <a name="chargeback-require-departmental-tags"></a>Chargeback: Zahtijevaju odjela oznake

Sljedeća pravila onemogućava zahtjevi za koje nemate oznakom koja sadrži ključ "costCenter".

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

Sljedeća pravila dodaje costCenter oznaka s unaprijed definiranih vrijednosti kad postoje nema oznaka. 

    {
      "if": {
        "field": "tags",
        "exists": "false"
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags",
            "value": {"costCenter":"myDepartment" }
          }
        ]
      }
    }
    
Sljedeća pravila dodaje costCenter oznaka s unaprijed definiranih vrijednosti kada nema oznaka costCenter, ali postoje druge oznake. 

    {
      "if": {
        "allOf": [
          {
            "field": "tags",
            "exists": "true"
          },
          {
            "field": "tags.costCenter",
            "exists": "false"
          }
        ]
    
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags.costCenter",
            "value": "myDepartment"
          }
        ]
      }
    }


### <a name="geo-compliance-ensure-resource-locations"></a>Usklađenost zemlj.: Provjerite lokacije resursa

Sljedeći primjer prikazuje pravila koja onemogućava zahtjeve gdje mjesto nije Sjeverna Europa ili Zapad Europe.

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="service-curation-select-the-service-catalog"></a>Servis Curation: Odaberite katalog servisa

Sljedeći primjer prikazuje pravila koja omogućuje akcije samo na servisi vrste Microsoft.Resources/\*, Microsoft.Compute/\*, Microsoft.Storage/\*, Microsoft.Network/\* dopušteno. Ništa nije dopušten.

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "field" : "type",
              "like" : "Microsoft.Resources/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Compute/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Storage/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="use-approved-skus"></a>Korištenje odobrene SKU-ove

Sljedeći primjer prikazuje korištenje svojstvo pseudonim za ograničavanje SKU-ove. U ovom primjeru Standard_LRS i Standard_GRS odobrenja za račune za pohranu.

    {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          },
          {
            "not": {
              "allof": [
                {
                  "field": "Microsoft.Storage/storageAccounts/sku.name",
                  "in": ["Standard_LRS", "Standard_GRS"]
                }
              ]
            }
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
    

### <a name="naming-convention"></a>Konvencije imenovanja

Sljedeći primjer prikazuje korištenje zamjenskih znakova koje podržavaju uvjet "kao što su". Uvjet koji navodi Ako naziv odgovara spomenuta uzorak (namePrefix\*nameSuffix) odbiti zahtjev.

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }
    
### <a name="tag-requirement-just-for-storage-resources"></a>Oznaka obavezu samo resursa za pohranu

Sljedeći primjer pokazuje kako se ugnijezditi logički operatori obavezne oznaku aplikacije samo resursa za pohranu.

    {
        "if": {
            "allOf": [
              {
                "not": {
                  "field": "tags",
                  "containsKey": "application"
                }
              },
              {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
              }
            ]
        },
        "then": {
            "effect": "audit"
        }
    }

## <a name="policy-assignment"></a>Dodjela pravilnika

Pravila primjenjuju se na različite opsege kao što su pretplate, grupa resursa i pojedinačne resurse. Pravila nasljeđuju tako da sve podređene resurse. Tako, ako pravilo primjenjuje se na grupu resursa, nije primjenjivo na sve resurse u toj grupi resursa.

## <a name="creating-a-policy"></a>Stvaranje pravila

Ovo poglavlje sadrži detalje na kako pravilo moguće stvoriti pomoću REST API-JA.

### <a name="create-policy-definition-with-rest-api"></a>Stvaranje pravila definicije s REST API-JA

Možete stvoriti pravila s [REST API -JA za definicije pravila](https://msdn.microsoft.com/library/azure/mt588471.aspx). REST API-JA omogućuje vam stvaranje i brisanje pravila definicije te informacije o postojećih definicija.

Da biste stvorili pravilo, pokrenite:

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

S zahtjev tijelo slično kao u sljedećem primjeru:

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "name":"testdefinition"
    }


Da biste postigli api-verzija koristite *2016-04-01*. Primjeri i dodatne pojedinosti potražite u članku [REST API -JA za definicije pravila](https://msdn.microsoft.com/library/azure/mt588471.aspx).

### <a name="create-policy-definition-using-powershell"></a>Stvaranje pravila definicije pomoću komponente PowerShell

Možete stvoriti pravila definicije pomoću cmdleta New-AzureRmPolicyDefinition. Sljedeći primjer stvara pravila za dopuštanje resursi samo u Sjevernoj Europe i Zapad Europe.

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy '{  
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'          

Rezultat izvođenja pohranjena u $policy objekt, a mogu se kasnije prilikom dodjele pravila. Za parametar pravila put do .json datoteku koja sadrži pravila također se može pružati umjesto navodeći umetnute pravila.

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain    regions" -Policy "path-to-policy-json-on-disk"

### <a name="create-policy-definition-using-azure-cli"></a>Stvaranje pravila definicije pomoću EŽA Azure

Možete stvoriti pravila definition azure EŽA pomoću naredbe definicija pravila kao što je prikazano u nastavku. Sljedeći primjer stvara pravila za dopuštanje resursi samo u Sjevernoj Europe i Zapad Europe.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy-string '{   
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'    
    

Nije moguće navesti put do .json datoteku koja sadrži pravila umjesto navodeći umetnute pravila kao što je prikazano u nastavku.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy "path-to-policy-json-on-disk"


## <a name="applying-a-policy"></a>Primjena pravila

### <a name="policy-assignment-with-rest-api"></a>Dodjela pravilnika s REST API-JA

Možete primijeniti definiciju pravila u željenom dosegu kroz [REST API -JA za dodjelu pravila](https://msdn.microsoft.com/library/azure/mt588466.aspx). REST API-JA omogućuje vam stvaranje i brisanje pravila dodjele te informacije o postojeće dodjele.

Da biste stvorili pravilo dodjele, pokrenite:

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

{Pravila-dodjeljivanjem} je naziv dodjele pravila. Da biste postigli api-verzija koristite *2016-04-01*. 

S zahtjev tijelo slično kao u sljedećem primjeru:

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "name":"VMPolicyAssignment"
    }

Primjeri i dodatne pojedinosti potražite u članku [REST API -JA za dodjelu pravila](https://msdn.microsoft.com/library/azure/mt588466.aspx).

### <a name="policy-assignment-using-powershell"></a>Dodjela pravilnika pomoću komponente PowerShell

Možete primijeniti pravilo stvorena iznad putem komponente PowerShell željeni doseg pomoću cmdleta New-AzureRmPolicyAssignment:

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
$Policy Evo objekt pravila koja je vraćena kao rezultat izvođenja cmdlet novo AzureRmPolicyDefinition kao što je prikazano gore. Opseg ovdje je naziv grupe resursa koji navedete.

Ako želite da biste uklonili iznad Dodjela pravilnika, možete ga učiniti na sljedeći način:

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Možete dobiti, promijeniti ili ukloniti definicije pravila putem Get-AzureRmPolicyDefinition, skup AzureRmPolicyDefinition i uklanjanje AzureRmPolicyDefinition cmdleta odnosno.

Isto tako, možete dobiti, promijeniti ili ukloniti pravila dodjele kroz Get-AzureRmPolicyAssignment, skup AzureRmPolicyAssignment i uklanjanje AzureRmPolicyAssignment cmdleta odnosno.

### <a name="policy-assignment-using-azure-cli"></a>Dodjela pravilnika pomoću EŽA Azure

Možete primijeniti pravilo stvorena iznad kroz Azure EŽA željeni doseg pomoću naredbe za dodjelu pravila:

    azure policy assignment create --name regionPolicyAssignment --policy-definition-id /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/<policy-name> --scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Opseg ovdje je naziv grupe resursa koji navedete. Ako je vrijednost parametra pravila-definicija – ID-a nije poznat, moguće je da biste dobili putem EŽA Azure kao što je prikazano u nastavku: 

    azure policy definition show <policy-name>

Ako želite da biste uklonili iznad Dodjela pravilnika, možete ga učiniti na sljedeći način:

    azure policy assignment delete --name regionPolicyAssignment --scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Početak, promjena ili uklanjanje pravila definicije putem pravila definition prikaz, postavite, i brisanje naredbe odnosno.

Isto tako, možete dobiti, promijeniti ili ukloniti pravila dodjele putem pravila dodjele prikaz i brisanje naredbe odnosno.

##<a name="policy-audit-events"></a>Pravila nadzora događaja

Nakon što primijenite pravilnika, počet ćete da biste pogledali događaje povezane s pravila. Možete ili idite na portal PowerShell ili EŽA Azure da biste pristupili slijedite ove podatke. 

### <a name="policy-audit-events-using-powershell"></a>Pravila nadzora događaja pomoću komponente PowerShell

Da biste prikazali svi događaji vezani uz odbijanje efekt, koristite sljedeću naredbu komponente PowerShell:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/deny/action"} 

Da biste pogledali sve događaje vezane uz nadzor efekt, koristite sljedeću naredbu:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/audit/action"} 

### <a name="policy-audit-events-using-azure-cli"></a>Pravila nadzora događaja pomoću EŽA Azure

Da biste pogledali sve grupe događaje iz resursa koju vezane uz odbijanje efekt, koristite sljedeću naredbu EŽA:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/deny/action\")"

Da biste pogledali sve događaje vezane uz nadzor efekt, koristite sljedeću naredbu EŽA:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/audit/action\")"
