<properties
   pageTitle="Voditelj resursa predložak za ključne sigurnog | Microsoft Azure"
   description="Prikazuje shemi resursima za implementaciju ključa sefovi pomoću predloška."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Ključni sigurnog shemu predloška

Stvara ključa zbirke ključeva.

## <a name="schema-format"></a>Oblikovanje sheme

Da biste stvorili ključa sigurnog, dodajte Sljedeća shema do odjeljka resursi predloška.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Vrijednosti

Tablice u nastavku opisuju vrijednosti morate postaviti u shemi.

| Ime | Vrijednost |
| ---- | ---- | 
| Vrsta | Redni broj<br />Obavezno<br />**Microsoft.KeyVault/vaults**<br /><br />Vrsta resursa, da biste stvorili. |
| apiVersion | Redni broj<br />Obavezno<br />**2015-06-01** ili **2014. – 12-19 – pregled**<br /><br />Verziju API će se koristiti za stvaranje resursa. | 
| ime | Niz<br />Obavezno<br />Naziv koja je jedinstvena preko Azure.<br /><br />Naziv ključa sigurnog da biste stvorili. Preporučujemo da koristite funkciju [uniqueString](resource-group-template-functions.md#uniquestring) s vašeg konvencija imenovanja da biste stvorili jedinstveni naziv, kao što je prikazano u primjeru u nastavku. |
| mjesto | Niz<br />Obavezno<br />Valjani regija ključa sefovi. Da biste utvrdili valjani područja, potražite u članku [podržane područja](resource-manager-supported-services.md#supported-regions).<br /><br />Područje za hostiranje ključa zbirke ključeva. |
| Svojstva | Objekt<br />Obavezno<br />[Svojstva objekta](#properties)<br /><br />Objekt koji navodi vrstu ključa sigurnog da biste stvorili. |
| Resursi | Polja<br />Neobavezno<br />Dopuštene vrijednosti: [ključ sigurnog tajnu resursi](resource-manager-template-keyvault-secret.md)<br /><br />Podređeni resursi za ključne zbirke ključeva. |

<a id="properties" />
### <a name="properties-object"></a>Svojstva objekta

| Ime | Vrijednost |
| ---- | ---- | 
| enabledForDeployment | Booleove vrijednosti<br />Neobavezno<br />**True** ili **false**<br /><br />Određuje na sigurnog omogućena virtualnog računala ili servisa tkanina implementacije. |
| enabledForTemplateDeployment | Booleove vrijednosti<br />Neobavezno<br />**True** ili **false**<br /><br />Određuje na sigurnog omogućena za korištenje u implementacijama predložak Voditelj resursa. Dodatne informacije potražite u odjeljku [Prosljeđivanje sigurne vrijednosti tijekom implementacije](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Booleove vrijednosti<br />Neobavezno<br />**True** ili **false**<br /><br />Određuje na sigurnog omogućena za šifriranje jedinice. |
| tenantId | Niz<br />Obavezno<br />**Globalno jedinstveni identifikator**<br /><br />Identifikator klijenta za pretplatu. Cmdlet ljuske PowerShell [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) ili **račun za azure prikaz** naredbe Azure EŽA možete dohvatiti ga. |
| accessPolicies | Polja<br />Obavezno<br />[objekt accessPolicies](#accesspolicies)<br /><br />Polja do 16 objekata koji odredite dozvole za korisnika ili usluge. |
| SKU | Objekt<br />Obavezno<br />[objekt SKU](#sku)<br /><br />SKU ključa zbirke ključeva. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>objekt properties.accessPolicies

| Ime | Vrijednost |
| ---- | ---- | 
| tenantId | Niz<br />Obavezno<br />**Globalno jedinstveni identifikator**<br /><br />Klijent identifikator Azure Active Directory klijentu koji sadrži **ID objekta** u ovim pravilima programa access |
| ID objekta | Niz<br />Obavezno<br />**Globalno jedinstveni identifikator**<br /><br />Identifikator objekta Azure Active Directory korisnika ili glavnicu servisa koji će imati pristup u zbirke ključeva. [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) ili Cmdlete ljuske PowerShell [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) ili EŽA Azure naredbe **azure ad korisnik** ili **azure ad sp** možete dohvatiti vrijednost. |
| dozvole | Objekt<br />Obavezno<br />[dozvole za objekte](#permissions)<br /><br />Pravo na ovom sigurnog objekt servisa Active Directory. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>objekt properties.accessPolicies.permissions

| Ime | Vrijednost |
| ---- | ---- | 
| tipke | Polja<br />Obavezno<br />**sve**, **sigurnosne kopije**, **Stvaranje**, **dešifrirati**, **Brisanje**, **šifriranje**, **se**, **Uvoz**, **popisa**, **Vraćanje**, **znak**, **unwrapkey**, **Ažuriranje**, **Provjerite je li**, **wrapkey**<br /><br />Pravo na tipke u ovom sigurnog u taj objekt servisa Active Directory. Ta vrijednost mora biti naveden kao polje s jednog ili više dopušteno vrijednostima. |
| tajne | Polja<br />Obavezno<br />**sve**, **Brisanje**, **se**, **popisa**, **postavite**<br /><br />Pravo na tajne u ovom sigurnog u taj objekt servisa Active Directory. Ta vrijednost mora biti naveden kao polje s jednog ili više dopušteno vrijednostima. |

<a id="sku" />
### <a name="propertiessku-object"></a>objekt Properties.SKU

| Ime | Vrijednost |
| ---- | ---- | 
| ime | Redni broj<br />Obavezno<br />**Standardno**ili **premium** <br /><br />Servis sloj KeyVault da biste koristili.  Standardna podržava tajne i softver zaštićeni tipke.  Premium dodaje podršku za HSM zaštićene tipke. |
| Obitelj | Redni broj<br />Obavezno<br />**ODGOVORA** <br /><br />Obitelj SKU-om za korištenje. |
 
    
## <a name="examples"></a>Primjeri

U sljedećem primjeru uvodi ključa sigurnog i tajna.

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

## <a name="quickstart-templates"></a>Predlošci za brzi početak rada

Sljedeći predložak za brzi početak rada uvodi ključa zbirke ključeva.

- [Stvaranje sigurnog ključa](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Daljnji koraci

- Opće informacije o ključa sefovi potražite u članku [Početak rada s sigurnog ključ Azure](./key-vault/key-vault-get-started.md).
- Primjer pozivanju ključa sigurnog tajna kada implementacija predložaka, potražite u članku [proći sigurne vrijednosti tijekom implementacije](resource-manager-keyvault-parameter.md).

