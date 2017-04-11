<properties
   pageTitle="Zavisnosti u predlošcima resursima | Microsoft Azure"
   description="U članku se opisuje postavljen jedan resurs ovisi o drugi resurs tijekom implementacije da biste bili sigurni da su u točno određenim redoslijedom implementirana resursi."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Definiranje ovisnosti u predlošcima Voditelj resursa za Azure

Za dani resurs može ostale resurse koji mora postojati prije nego je implementiran resurs. Ako, na primjer, SQL server mora postojati prije implementacije bazom podataka SQL. Definirate odnos tako da označite jednu resurse kao ovisi o neki drugi resurs. Obično definirate ovisnost elementom **dependsOn** , ali možete ga definirati i pomoću funkcije **reference** . 

Voditelj resursa procjenjuje međuzavisnosti resurse i uvodi ih zavisne redoslijedom. Kada su resursi ovise o međusobno povezani, resursima ih uvodi paralelno.

## <a name="dependson"></a>dependsOn

U predlošku dependsOn element omogućuje vam da biste definirali jedan resurs kao zavisne na jedan ili više resursa. Vrijednost može biti popis odvojenih zarezom nazive resursa. 

Sljedeći primjer prikazuje skup skaliranje virtualnog računala koja ovisi o raspoređivača opterećenja, virtualne mreže i petlje stvara više prostora za pohranu računa. Ostali resursi nisu prikazani u sljedećem primjeru, ali su potrebni za postoji na nekom drugom mjestu u predlošku.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Da biste definirali ovisnosti između resursa i resursa koje su stvorene pomoću petlje Kopiraj, postavite dependsOn element naziv petlje. Na primjer, potražite u članku [Stvaranje više instanci resursa u Azure Voditelj resursa](resource-group-create-multiple.md).

Dok je možda inclined da biste koristili dependsOn da biste mapirali odnose između resursa, važno je da biste shvatili zašto je radite nešto jer to može utjecati na performanse uvođenja. Ako, na primjer, da biste dokument kako međusobno resursa, dependsOn nije desnom pristup. Ne možete poslati upit resursa koji su definirani u elementu dependsOn nakon implementacije. Pomoću dependsOn koji potencijalno utjecati na vrijeme uvođenja jer resursima implementacije u paralelni dva resursa koji imaju ovisnosti. Da biste dokument odnose između resursi, umjesto toga koristite [Povezivanje resursa](resource-group-link-resources.md).

## <a name="child-resources"></a>Podređeni resursi

Svojstvo resursa omogućuje vam da navedete podređeni resursi vezani uz resursa koji se definiraju. Podređeni resursa može biti definirani pet razina dubine. Važno Imajte na umu ovisnost implicitno nije stvoren između podređeni resursa i resursa nadređenog je. Ako vam je potrebna resursa podređeni uvesti nakon nadređenog resursa, morate izričito stanja taj ovisnosti s svojstvo dependsOn. 

Svaki resurs nadređenog prihvaća samo određene vrste resursa kao podređeni resurse. Vrste prihvaćenom resursa su navedeni u [shemu predloška](https://github.com/Azure/azure-resource-manager-schemas) nadređenog resursa. Naziv podređene vrste resursa obuhvaća naziv nadređene vrste resursa, kao što su **Microsoft.Web/sites/config** i **Microsoft.Web/sites/extensions** su oba podređeni resurse **Microsoft.Web/sites**.

Sljedeći primjer prikazuje SQL server i SQL baze podataka. Obratite pozornost na to izričito ovisnost definiran između SQL baze podataka i SQL server čak i ako se baza podataka nalazi podređeni poslužitelja.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>Referenca (opis funkcije)

[Referenca (opis funkcije)](resource-group-template-functions.md#reference) omogućuje izraz za dobivanje glavne njegovom vrijednošću s drugim nazivom JSON i parove vrijednosti ili runtime resursi. Pregled izraza implicitno deklarirati da jedan resurs ovisi o drugi. 

    reference('resourceName').propertyPath

Taj element ili dependsOn element možete koristiti da biste odredili ovisnosti, ali ne trebate koristiti oba istoj zavisnih resursa. Kad god je to moguće, koristite implicitno referenca da biste izbjegli slučajno dodavanje nepotrebne ovisnosti.

Dodatne informacije potražite u članku [Referenca funkcije](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o stvaranju predložaka Azure Voditelj resursa, potražite u odjeljku [Predlošci na dokumentima](resource-group-authoring-templates.md). 
- Popis dostupnih funkcija u predložak, potražite u članku [funkcije predložak](resource-group-template-functions.md).

