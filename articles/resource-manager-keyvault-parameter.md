<properties
   pageTitle="Tajna sigurnog ključ s predloškom resursima | Microsoft Azure"
   description="U članku se objašnjava prenesite na tajna iz ključa sigurnog kao parametar tijekom implementacije."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Prenesite sigurne vrijednosti tijekom implementacije

Kada se morate proći sigurne vrijednost (kao što je lozinka) kao parametar tijekom implementacije, možete spremiti tu vrijednost kao tajna u [Azure ključ sigurnog](./key-vault/key-vault-whatis.md) i referencirati vrijednosti u drugim predlošcima Voditelj resursa. Samo referenca na tajna uključiti u predložak tako da nikada ne prikazuje na tajna i ne morate ručno unesite vrijednost za na tajna svaki put implementacija resurse. Odredite koji korisnici ili upravitelji servisa možete pristupiti na tajna.  

## <a name="deploy-a-key-vault-and-secret"></a>Implementacija ključa sigurnog i tajna

Da biste stvorili ključa zbirke ključeva možete se pozivati iz drugih predložaka Voditelj resursa, morate postaviti svojstvo **enabledForTemplateDeployment** na **true**, a da biste korisnika ili Upravitelj servisa koji će se izvoditi implementaciju koji se odnosi na tajna mora odobriti pristup.

Da biste saznali više o implementaciji ključa sigurnog i tajna, potražite u članku [ključ sigurnog sheme](resource-manager-template-keyvault.md) i [ključ sigurnog tajnu shemu](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Referenca tajna statične ID

Referenca tajna iz unutar datoteke parametre koje prosljeđuje vrijednosti u predložak. Referenca na tajna prosljeđivanjem identifikator resursa ključa sigurnog i naziv u tajna. U ovom primjeru tajna ključa sigurnog mora postojati i koristite statična vrijednost za id resursa.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Datotečni cijelu parametar izgleda ovako:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Parametar prihvaća na tajna mora biti **securestring**. Sljedeći primjer prikazuje odjeljke predložak koji implementira SQL server koji zahtijeva administratorsku lozinku.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Referenca tajna dinamički ID

U prethodnom odjeljku prikazivao kako proći id statične resursa za ključne sigurnog tajna. No u nekim slučajevima ćete morati upućuju na ključa sigurnog tajna koji se razlikuje ovisno o trenutnom implementacije. U tom slučaju možete teško koda identifikator resursa u datoteci parametara. Nažalost, ne možete dinamički generirati identifikator resursa u datoteci s parametrima jer predložak izraza nije dopušteno u datoteci parametara.

Za dinamičko stvaranje id resursa za ključne sigurnog tajna, premještate resursa koji je potrebno u tajna u ugniježđene predložak. U predlošku osnovne dodavanje ugniježđene predloška i prenesite parametar koji sadrži id dinamički generirani resursa.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Daljnji koraci

- Opće informacije o ključa sefovi potražite u članku [Početak rada s sigurnog ključ Azure](./key-vault/key-vault-get-started.md).
- Informacije o korištenju tipki sigurnog s virtualnog računala potražite u članku [Sigurnosna pitanja vezana uz za Azure Voditelj resursa](best-practices-resource-manager-security.md).
- Dovršavanje Primjeri odnose ključa tajne potražite u članku [primjeri sigurnog ključ](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

