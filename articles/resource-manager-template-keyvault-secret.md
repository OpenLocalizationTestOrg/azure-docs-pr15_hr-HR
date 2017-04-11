<properties
   pageTitle="Voditelj resursa predložak za tajna u ključa sigurnog | Microsoft Azure"
   description="Prikazuje shemi resursima za implementaciju ključa sigurnog tajne pomoću predloška."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Shemu ključa sigurnog tajnu predloška

Stvara tajna koji je spremljen u ključa zbirke ključeva. Ove vrste resursa često implementirati kao podređeni resurs od [ključne zbirke ključeva](resource-manager-template-keyvault.md).

## <a name="schema-format"></a>Oblikovanje sheme

Da biste stvorili ključa sigurnog tajna, dodajte Sljedeća shema predloška. Na tajna može se definirati kao ili podređeni resursa od ključne sigurnog ili najviše razine resursa. Možete definirati ga kao podređeni resurs kada ključa sigurnog je uveden u isti predložak. Morat ćete definirati na tajna kao najviše razine resursa kada ključa sigurnog je uveden u isti predložak ili kada se morate stvoriti više tajne ponavljanje na vrsti resursa. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Vrijednosti

Tablice u nastavku opisuju vrijednosti morate postaviti u shemi.

| Ime | Vrijednost |
| ---- | ---- | 
| Vrsta | Redni broj<br />Obavezno<br />**tajne** (kad je uveden kao podređeni resursa od ključne sigurnog) ili<br /> **Microsoft.KeyVault/vaults/secrets** (kad je uveden kao najviše razine resursa)<br /><br />Vrsta resursa, da biste stvorili. |
| apiVersion | Redni broj<br />Obavezno<br />**2015-06-01** ili **2014. – 12-19 – pregled**<br /><br />Verziju API će se koristiti za stvaranje resursa. | 
| ime | Niz<br />Obavezno<br />Jedna riječ kada implementiran kao podređeni resurs ključa sigurnog ili u obliku **{ključ-sigurnog-name} / {tajna name}** kada je uveden kao najviše razine resursa za dodavanje postojeće ključne sigurnog.<br /><br />Naziv tajna da biste stvorili. |
| Svojstva | Objekt<br />Obavezno<br />[Svojstva objekta](#properties)<br /><br />Objekt koji određuje vrijednost tajna da biste stvorili. |
| dependsOn | Polja<br />Neobavezno<br />Popis odvojenih zarezom resursa imena ili resursa odredišnih stilova.<br /><br />Skup resursa ovisi o tu vezu. Ako je ključa zbirke ključeva za na tajna implementiran u isti predložak, naziv ključa sigurnog obuhvatiti taj element da biste bili sigurni da je najprije implementiran. |

<a id="properties" />
### <a name="properties-object"></a>Svojstva objekta

| Ime | Vrijednost |
| ---- | ---- | 
| vrijednost | Niz<br />Obavezno<br /><br />Tajno vrijednost za pohranu u ključa zbirke ključeva. Kada prosljeđivanje vrijednost za to svojstvo, koristite parametar vrsta **securestring**.  |

    
## <a name="examples"></a>Primjeri

Prvi primjer uvodi na tajna kao podređeni resurs od ključne zbirke ključeva.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Drugi primjer uvodi se tajna kao najviše razine resurs koji je spremljen u postojeće ključne sigurnog.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Daljnji koraci

- Opće informacije o ključa sefovi potražite u članku [Početak rada s sigurnog ključ Azure](./key-vault/key-vault-get-started.md).
- Primjer pozivanju ključa sigurnog tajna kada implementacija predložaka, potražite u članku [proći sigurne vrijednosti tijekom implementacije](resource-manager-keyvault-parameter.md).


