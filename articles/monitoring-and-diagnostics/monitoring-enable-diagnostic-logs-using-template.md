<properties
    pageTitle="Automatski omogućite Diagnostic postavke pomoću predloška resursima | Microsoft Azure"
    description="Saznajte kako koristiti predložak Voditelj resursa da biste stvorili dijagnostičkih postavke koje će vam omogućiti da strujanje sustava dijagnostičke zapisnike s koncentratorima događaj ili ih spremiti na računu za pohranu."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Automatski omogućiti dijagnostičkih postavki na stvaranje resursa pomoću predloška Voditelj resursa
U ovom članku Pokazat ćemo kako možete koristiti [predloška Azure Voditelj resursa](../resource-group-authoring-templates.md) za konfiguriranje dijagnostičkih postavki na resursu prilikom stvaranja. To vam omogućuje da se automatski pokrenuti strujanje dijagnostičke zapisnike i metriku za događaj koncentratora arhiviranja na računu za pohranu ili ih šalje prijava analitiku stvaranja resurs.

Način za omogućivanje dijagnostičke zapisnike pomoću predloška resursima ovisi o vrsti resursa.

- **Osobe koje nisu računalnim** resursi (Ako, na primjer, mreže sigurnosnih grupa, logike aplikacije, automatizacija) pomoću [dijagnostičkih postavki opisane u ovom članku](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Izračun** Resursi (WAD/LAD-poštu) koristiti [WAD/LAD konfiguracijska datoteka opisane u ovom članku](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

Sadržaj članka možemo opisuje način konfiguriranja Dijagnostika na bilo koji način.

Slijede Osnovni koraci:

1. Stvaranje predloška kao datoteku JSON koji opisuje kako stvoriti resursa i omogućiti i dijagnostiku.
2. [Uvođenje predloška pomoću bilo koji način implementacije](../resource-group-template-deploy.md).

U nastavku ne možemo dati primjera datoteku predloška JSON potrebno da biste generirali za koje nisu računalnim i računalnim resursi.

## <a name="non-compute-resource-template"></a>Predlošku koji nisu računalnim resursa
Za resurse koji nisu računalnim, morat ćete učiniti dvije stvari:

1. Dodajte parametre da blob parametara za naziv računa za pohranu, servis bus pravilo ID ili OMS prijava analitiku radni prostor ID (Omogućivanje arhiviranje dijagnostičkih zapisnika na računu za pohranu, strujanje zapisnika s koncentratorima događaja i/ili slanje zapisnika prijava analitiku).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. U polju Resursi resursa za koju želite omogućiti dijagnostičke zapisnike, dodajte resurs vrste `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Svojstva blob za postavku dijagnostičkih slijedi [oblik opisane u ovom članku](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Evo cijelog primjer u kojem se stvara sigurnosnu grupu mreže i uključuje strujanje na web-mjesto koncentratora za događaj i u okvir za pohranu na računu za pohranu.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Izračunavanje predlošku resursa
Da biste omogućili dijagnostika na računalnim resursa, primjerice virtualnog računala ili servisa tkanina klaster, morate:

1. Proširenje Azure Dijagnostika dodati definiciju VM resursa.
2. Navedite prostora za pohranu računa i/ili događaj koncentrator kao parametar.
3. Dodajte sadržaj WADCfg XML datoteke u svojstvo XMLCfg escaping sve znakove XML pravilno.

> [AZURE.WARNING] Ovaj je posljednji korak može biti Škakljivo da biste dobili desno. Primjer u kojem se konfiguracije sheme Dijagnostika dijeli varijabli koje su unijeti prespojni znak i ispravno oblikovana [potražite u ovom članku](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) .

Je cijeli postupak, uključujući uzorka, što je opisano [u ovom dokumentu](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Daljnji koraci
- [Saznajte više o Azure dijagnostičkih zapisnika](./monitoring-overview-of-diagnostic-logs.md)
- [Strujanje Azure dijagnostičke zapisnike s koncentratorima događaja](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
