<properties
   pageTitle="Automatizacija resursa u OMS rješenja | Microsoft Azure"
   description="Rješenja u OMS obično obuhvaćaju runbooks u automatizaciji Azure da biste automatizirali procese kao što su prikupljanje i obrada podataka nadzora.  U ovom se članku opisuje kako uključiti runbooks i njihovih povezanih resursa u rješenje."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automatizacija resursa u OMS rješenja (pretpregled)

>[AZURE.NOTE]Ovo je prva verzija dokumentacije za stvaranje rješenja za upravljanje OMS koje su trenutno u pretpregledu. Bilo koji sheme opisanim mogu se promijeniti.   

[Rješenja za upravljanje OMS](operations-management-suite-solutions.md) obično obuhvaćaju runbooks u automatizaciji Azure da biste automatizirali procese kao što su prikupljanje i obrada podataka nadzora.  Osim runbooks, računa za automatizaciju obuhvaća sadržaje kao što su varijabli i rasporede koji podržavaju runbooks koji se koriste u rješenje.  U ovom se članku opisuje kako uključiti runbooks i njihovih povezanih resursa u rješenje.

>[AZURE.NOTE]Uzorci u ovom članku pomoću parametara i varijabli koje su potrebne ili zajednički rješenja za upravljanje i što je opisano u [Stvaranje rješenja za upravljanje u paketu operacije upravljanja (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da ste već upoznati s postupkom da biste [stvorili rješenje za upravljanje](operations-management-suite-solutions-creating.md) i strukturu datoteke rješenja.

## <a name="samples"></a>Uzorci
Ogledni predlošci resursima za automatizaciju resursa možete pristupiti iz [predložaka za brzi početak rada u GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Automatizacija računa
Svi resursi u automatizaciji Azure nalaze se u [račun za automatizaciju](../automation/automation-security-overview.md#automation-account-overview).  Kao što je opisano u [radni prostor OMS i račun za automatizaciju](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) Automatizacija račun nije obuhvaćen rješenje za upravljanje, ali mora postojati prije instalacije rješenja.  Ako nije dostupna, zatim Instaliranje rješenja neće uspjeti.

Naziv računa Automatizacija je naziv svakog resursa za automatizaciju.  To možete učiniti u rješenje s parametrom **accountName** kao u sljedećem primjeru runbook resursa.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
Resursi za [automatizaciju Azure runbook](../automation/automation-runbook-types.md) vrstu **Microsoft.Automation/automationAccounts/runbooks** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| runbookType | Određuje vrsta na kompilacije. <br><br> Skripta - skriptu komponente PowerShell <br>PowerShell – PowerShell tijeka rada <br> GraphPowerShell - runbook skripte grafički PowerShell <br> GraphPowerShellWorkflow - runbook grafički PowerShell tijeka rada   |
| logProgress | Određuje hoće li se [tijeku zapisa](../automation/automation-runbook-output-and-messages.md) stvaraju za na runbook. |
| logVerbose  | Određuje mogu li se [opširno zapisa](../automation/automation-runbook-output-and-messages.md) generirati za na runbook. |
| Opis | Neobavezni opis u runbook. |
| publishContentLink | Određuje sadržaj na kompilacije. <br><br>URI - Uri za sadržaj na kompilacije.  To će biti .ps1 datoteke za runbooks PowerShell i skripte i izvezene grafički runbook datoteku za grafikon runbook.  <br> verzija - verzije kompilacije za vlastitu praćenje. |

Primjer resursa runbook je ispod.  U ovom slučaju ga dohvaća na runbook iz [Galerije PowerShell](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Pokretanje programa runbook
Da biste započeli s runbook u rješenje upravljanja na dva načina.

- Da biste započeli s runbook kada je instaliran rješenje, stvorite [zadatak resursa](#automation-jobs) prema uputama u nastavku.
- Da biste započeli s runbook prema rasporedu, stvoriti [raspored resursa](#schedules) prema uputama u nastavku. 


## <a name="automation-jobs"></a>Automatizacija zadataka
Da biste započeli s runbook kada je instaliran rješenje za upravljanje, stvorite **zadatak** resursa.  Resursi za posao vrstu **Microsoft.Automation/automationAccounts/jobs** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| runbook    | Jedno **ime** entitet s nazivom na kompilacije.  |
| Parametri | Entitet za svaki parametar potrebnih u runbook. |


Posao sadrži naziv runbook i sve vrijednosti parametra slati na runbook.  Posao mora biti [ovise o](operations-management-suite-solutions-creating.md#resources) runbook koji počinje od na runbook moraju se stvoriti prije posao.  Stvaranje zavisnosti na druge zadatke za runbooks koji se moraju dovršiti prije trenutnog.

Naziv resursa posao mora sadržavati GUID koje obično dodijelio parametra.  Dodatne informacije o GUID parametara u [Stvaranje rješenja u paketu operacije upravljanja (OMS)](operations-management-suite-solutions-creating.md#parameters).  

Slijedi primjer posla resursa koji će se pokrenuti na runbook kada je instaliran rješenje za upravljanje.  Neki drugi runbooks morate dovršiti prije nego što je to nešto pokrene, pa ima ovisnosti na zadacima za tu runbook.  Detalje za posao se odvija pomoću parametara i varijable umjesto izravno naveden.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Certifikati
[Automatizacija Azure certifikati](../automation/automation-certificates.md) vrstu **Microsoft.Automation/automationAccounts/certificates** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| base64Value   | Osnovni 64 vrijednost za potvrdu. |
| otisak prsta    | Otisak prsta certifikata. |

Primjer resursa certifikat je ispod.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Vjerodajnice
[Vjerodajnice za automatizaciju Azure](../automation/automation-credentials.md) vrstu **Microsoft.Automation/automationAccounts/credentials** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| korisničko ime   | Korisničko ime za vjerodajnicu. |
| lozinke   | Lozinka za vjerodajnicu. |

Primjer resursa vjerodajnica je ispod.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Rasporedi
[Automatizacija Azure rasporede](../automation/automation-schedules.md) vrstu **Microsoft.Automation/automationAccounts/schedules** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Opis | Neobavezan opis za raspored. |
| startTime   | Određuje vrijeme početka raspored kao objekt datuma i vremena. Niz se može pružati ako je moguće pretvoriti u valjanih datuma i vremena. |
| isEnabled   | Određuje je li omogućen u rasporedu. |
| Interval    | Vrsta interval za raspored.<br><br>dan<br>sat |
| FREQUENCY   | Učestalost raspored pokrenuti u broj dana ili sata. |

Primjer rasporeda resursa je ispod.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Varijable
[Automatizacija Azure varijable](../automation/automation-variables.md) vrste **Microsoft.Automation/automationAccounts/variables** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Opis | Neobavezan opis za varijablu. |
| isEncrypted | Određuje hoće li biti šifrirane varijablu. |
| Vrsta        | Vrsta podataka za varijablu. |
| vrijednost       | Vrijednost za varijablu. |

Primjer varijable resurs je ispod.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Moduli
Rješenje za upravljanje nije potrebno definirati [globalnih modula](../automation/automation-integration-modules.md) koristi vaš runbooks jer oni uvijek biti dostupni.  Morate unijeti resursa za druge modul koji se koriste na runbooks, a na runbook treba ovise o modul resursa da biste bili sigurni da je stvorena prije na runbook.

[Integracija moduli](../automation/automation-integration-modules.md) vrstu **Microsoft.Automation/automationAccounts/modules** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| contentLink | Određuje sadržaj modul. <br><br>URI - Uri za sadržaj na kompilacije.  To će biti .ps1 datoteke za runbooks PowerShell i skripte i izvezene grafički runbook datoteku za grafikon runbook.  <br> verzija - verzije kompilacije za vlastitu praćenje. |

Primjer modul resursa je ispod.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Ažuriranje moduli
Ako ažurirate rješenje upravljanja koja sadrži runbook koji koristi raspored i novu verziju rješenje sadrži novi modul koji koriste tu runbook, u runbook može koristiti staru verziju modula.  Trebali biste obuhvaćaju sljedeće runbooks u rješenje i stvaranje zadatka da biste pokrenuli ih prije drugih runbooks.  To će osigurati module ažuriraju kao obavezno prije učitavaju se u runbooks.

- [Ažuriranje ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) će sve moduli koristi runbooks u rješenje provjerite jesu li najnoviju verziju.  
- [ReRegisterAutomationSchedule-MS-upravljanje dokumentima](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) registrirati sve resurse za raspored da biste bili sigurni da u runbooks povezanim s najnovijim module.


Slijedi primjer obaveznih elemenata rješenje podržava ažuriranje modula.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje prikaza rješenje](operations-management-suite-solutions-resources-views.md) vizualizacija prikupljene podatke.