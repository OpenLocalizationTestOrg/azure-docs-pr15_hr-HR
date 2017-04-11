<properties
   pageTitle="Implementacija više instanci resursa | Microsoft Azure"
   description="Korištenje kopiranja i polja u predlošku Voditelj resursa Azure iteracija više puta pri implementaciji resursi."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Stvaranje više instanci resursa u Azure Voditelj resursa

U ovoj se temi objašnjava iteracija u predlošku Voditelj resursa Azure da biste stvorili više instanci resursa.

## <a name="copy-copyindex-and-length"></a>kopiranje, copyIndex i duljine

Unutar resursa da biste stvorili više puta, možete definirati **Kopiraj** objekt koji određuje koliko je puta iteracija. Kopiraj uzima sljedećem obliku:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Trenutna vrijednost iteracije s funkcijom **copyIndex()** možete pristupiti kao što je prikazano u nastavku unutar funkcije slika.

    [concat('examplecopy-', copyIndex())]

Pri stvaranju višestrukih resursa neki od brojnih vrijednosti, koristite funkciju **duljine** da biste naveli broj. Navedite polja kao parametar funkciji duljine.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Korištenje vrijednosti indeksa u nazivu

Možete koristiti kopiju operacija stvaranje više instanci resurs koji jedinstveno naziva na temelju incrementing indeksa. Ako, na primjer, možda ćete morati dodati jedinstvenog broja na kraj svake naziv resursa koja je postavila. Za implementaciju tri web-mjesta pod nazivom:

- examplecopy 0
- examplecopy-1
- examplecopy-2.

Korištenje predloška za sljedeće:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Pomak indeksa

Primijetit ćete da je u prethodnom primjeru koja se proteže vrijednost indeksa od nule 2. Da biste pomak indeks možete proslijediti vrijednosti u funkciji **copyIndex()** , kao što su **copyIndex(1)**. Broj iteracija da biste izvršili i dalje naveden u element Kopiraj, ali je vrijednost argumenta copyIndex pomaka određenu vrijednost. Tako, koristeći isti predložak kao u prethodnom primjeru, no navodeći **copyIndex(1)** bi implementacija tri web-mjesta pod nazivom:

- examplecopy-1
- examplecopy-2
- examplecopy-3

## <a name="use-copy-with-array"></a>Kopiraj pomoću polja
   
Kopiranje je osobito korisni prilikom rada s poljima jer možete iteracija kroz svaki element u polju. Za implementaciju tri web-mjesta pod nazivom:

- examplecopy Contoso
- examplecopy Fabrikam
- examplecopy Coho

Korištenje predloška za sljedeće:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Naravno, postavite broj kopija na vrijednost različita od duljine polja. Ako, na primjer, nije moguće stvaranje polja s više vrijednosti, a zatim prenesite parametar vrijednost koja određuje koliko elemenata polja za implementaciju. U tom slučaju postavljate count kopiju kao što je prikazano u primjeru u prvom. 

## <a name="depending-on-resources-in-a-loop"></a>Ovisno o resursima u petlji

Možete odrediti da se resurs implementirati nakon drugog resursa pomoću **dependsOn** element. Kada vam je potrebna za implementaciju resurs koji ovisi o skup resursa u petlji, možete koristiti Davanje naziva Ponavljaj Kopiraj **dependsOn** element. Sljedeći primjer pokazuje kako implementirati 3 račune za pohranu prije no što implementirate virtualnog računala. Potpunu definiciju virtualnog računala ne prikazuje. Obratite pozornost na to da element Kopiraj ima **naziv** postavljena na **storagecopy** i element **dependsOn** za virtualnim strojevima i postavite na **storagecopy**.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Ponavljanje na ugniježđene resursa

Nije moguće koristiti petlje Kopiraj ugniježđene resursa. Ako vam je potrebna za stvaranje više instanci resursa koje obično definirate kao ugniježđene unutar drugog resursa, morate umjesto stvaranje resursa kao resurs najviše razine i definirati odnos s resursom nadređenog kroz **vrstu** i **naziv** svojstva.

Na primjer, pretpostavimo kao ugniježđene resurs unutar podataka tvorničke obično definirati skup podataka.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Da biste stvorili više instanci skupova podataka, trebali biste promijenite predložak kao što je prikazano u nastavku. Obavijest o potpuno kvalificiran vrstu i naziv sadrži naziv tvorničke podataka.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Stvaranje više instanci prilikom kopiranja neće funkcionirati

**Kopiraj** možete koristiti samo na vrste resursa, a ne svojstava unutar vrsta resursa. To može stvoriti probleme kada želite stvoriti više instanci nečega koja je dio resursa. Uobičajeni scenarij je da biste stvorili više diskova podataka za virtualnog računala. **Kopiranje** ne možete koristiti s diskova podataka jer **dataDisks** svojstvo je na virtualnog računala ne vlastitu vrstu resursa. Umjesto toga, stvaranje polja s proizvoljan broj diskova podataka kao što je će potrebna i prenesite stvarni broj diskova podataka da biste stvorili. U definiciji virtualnog računala, koristite funkciju **poduzeti** da biste dobili broj elemenata koji zapravo želite iz polja.

Potpuna primjera ovaj uzorak je prikaz u predlošku [Stvaranje VM s odabirom dinamičkih diskova podataka](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) .

Odjeljke uvođenje predloška možete prikazano u nastavku. Mnogo predložak je uklonjen da biste istaknuli u odjeljcima uvrštene u dinamičko stvaranje broj diskova podataka. Obratite pozornost na to parametar **numDataDisks** koji omogućuje prenesite broj diskova da biste stvorili. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Daljnji koraci
- Ako želite dodatne informacije o sekcijama predložak, potražite u članku [Za izradu predložaka Azure Voditelj resursa](./resource-group-authoring-templates.md).
- Sve funkcije možete koristiti u predlošku potražite u odjeljku [Azure resursima predložak funkcije](./resource-group-template-functions.md).
- Da biste saznali kako uvesti predložak, potražite u članku [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md).
