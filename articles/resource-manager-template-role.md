<properties
   pageTitle="Voditelj resursa predloška radi dodjele uloga | Microsoft Azure"
   description="Prikazuje shemi resursima za implementaciju Dodjela uloga pomoću predloška."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Shemu predloška dodjele uloga

Uloga u navedenom dosegu dodjeljuje korisniku, grupi ili usluge.

## <a name="resource-format"></a>Oblikovanje resursa

Da biste stvorili Dodjela uloge, dodajte Sljedeća shema do odjeljka resursi predloška.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Vrijednosti

Tablice u nastavku opisuju vrijednosti morate postaviti u shemi.

| Ime | Obavezno | Opis |
| ---- | -------- | ----------- |
| Vrsta | Da    | Vrsta resursa, da biste stvorili.<br /><br /> Za grupu resursa:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Resursa:<br />**{davatelja-prostor naziva} / {vrsta resursa} / davatelji/roleAssignments** |
| apiVersion |Da | Verziju API će se koristiti za stvaranje resursa.<br /><br /> Koristite **2015-07-01**. | 
| ime | Da | Globalno jedinstveni identifikator za Dodjela nove uloge. |
| dependsOn | ne | Polja odvojenih zarezom resursa imena ili resursa odredišnih stilova.<br /><br />Skup resursa ovisi o ovom Dodjela uloge. Ako dodjeljujete uloge koje iz djelokruga resursa i resursa je uveden u isti predložak, uključite taj naziv resursa u taj element da biste bili sigurni resursa najprije implementiran. |
| Svojstva | Da | Svojstva objekta koja služi za identifikaciju definicije uloge, glavni i opseg. |

### <a name="properties-object"></a>Svojstva objekta

| Ime | Obavezno | Opis |
| ---- | -------- | ----------- |
| roleDefinitionId | Da |  Identifikator postojeću definiciju uloga koja će se koristiti u Dodjela uloge.<br /><br /> Pomoću sljedećih oblika:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Da | Globalno jedinstveni identifikator za postojeće glavnicu. Ova vrijednost karte ID u direktoriju i postavite pokazivač miša korisnika, usluge ili sigurnosne grupe. |
| opseg | ne | Opseg na kojoj ova Dodjela uloge odnosi se na.<br /><br />Da biste postigli grupe resursa, koristite:<br />**/resourceGroups/ /subscriptions/ {id pretplate} {-naziv grupe resursa –}**  <br /><br />Da biste postigli resursa, koristite:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Upute za korištenje resursa za dodjelu uloga

Dodjela uloge dodati predlošku kada je potrebno da biste dodali korisniku, grupi ili servis glavni uloge tijekom implementacije. Dodjele uloga nasljeđuju od više razine opsega, pa ako ste već dodali glavni ulogu na razini pretplate, morate ponovno za grupu resursa ili resurs.

Nema više vrijednosti identifikatora morate navesti prilikom rada s dodjele uloga. Možete dohvatiti vrijednosti pomoću programa PowerShell ili Azure EŽA.

### <a name="powershell"></a>PowerShell

Naziv Dodjela uloge zahtijeva globalno jedinstveni identifikator. Možete stvoriti novi identifikator za **naziv** s:

    $name = [System.Guid]::NewGuid().toString()

Možete dohvatiti identifikator za definicije uloge sa:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Možete dohvatiti identifikator za glavnicu s jednim od sljedećih naredbi.

Za grupu pod nazivom **revizore**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Korisnik s nazivom **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Za uslugu glavnicu pod nazivom **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure EŽA

Možete dohvatiti identifikator za definicije uloge sa:

    azure role show Reader --json | jq .[].Id -r

Možete dohvatiti identifikator za glavnicu s jednim od sljedećih naredbi.

Za grupu pod nazivom **revizore**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Korisnik s nazivom **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Za uslugu glavnicu pod nazivom **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Primjeri

Sljedeći predložak prima identifikator za uloge i identifikatora za korisnika, grupa ili usluge. Dodjeljuje uloga na razini grupu resursa.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Sljedeći predložak stvara račun za pohranu i dodjeljuje ulozi čitač račun za pohranu. Identifikatore dvije grupe i ulozi čitač su obuhvaćene predložak da biste pojednostavnili implementacije. Te se vrijednosti nije moguće dohvatiti trenutku implementaciju putem u skripti i kao parametara.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Predlošci za brzi početak rada

Sljedeći predlošci pokazalo kako se koristi dodjele resursa uloga:

- [Dodjela ugrađene uloge grupa resursa](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Dodijeli ulogu ugrađene postojeću VM](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Dodijeli ulogu ugrađene većem broju postojeću VMs](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Daljnji koraci

- Informacije o strukturi predloška potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md).
- Dodatne informacije o kontrola pristupa na temelju uloga potražite u članku [Utemeljen na Azure Active Directory uloga kontrola pristupa](./active-directory/role-based-access-control-configure.md).
