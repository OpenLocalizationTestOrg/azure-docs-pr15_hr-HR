<properties
   pageTitle="Korištenje želji stanja konfiguracije sa skupovima skaliranje virtualnog računala | Microsoft Azure"
   description="Pomoću skale virtualnog računala postavlja s datotečnim nastavkom Azure DSC"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Pomoću skale virtualnog računala postavlja s datotečnim nastavkom Azure DSC

[Virtualni stroj skaliranje skupove (VMSS)](virtual-machine-scale-sets-overview.md) može se koristiti s nastavkom rukovatelj [Azure želji stanje konfiguracije (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) . VMSS omogućuje upravljanje velikim brojem virtualnim strojevima i, a možete elastically promjena veličine i smanjivati kao odgovor na učitati. DSC koristi se za konfiguraciju na VMs čim prijavi tako da su pokrenuti softver proizvodnje.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Razlike između implementacija VM i VMSS

Strukturu u podlozi predložak za VMSS malo razlikuje se od jednog VM. Konkretno, jedan VM uvodi proširenja u odjeljku čvor "virtualMachines". Postoji stavka vrste "proširenja" gdje se DSC dodaje u predložak

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

VMSS čvor ima sekciju "Svojstva" u "VirtualMachineProfile", "extensionProfile" atribut. DSC dodaje se u odjeljku "proširenja"

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>Ponašanje VMSS

Ponašanje VMSS jednak ponašanje jedan VM. Prilikom stvaranja novog VM automatski dodjeli s nastavkom DSC. Novija verzija na WMF eventualno nastavka u VM izvršen prije uskoro online. Kada je na mreži, preuzimanja konfiguracije .zip DSC i dodjela na na VM. Dodatne informacije pronaći ćete u [Pregled za nastavak DSC Azure](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Daljnji koraci ##
Pregledajte [predloška Azure Voditelj resursa za proširenje DSC](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Saznajte kako s [nastavkom DSC sigurno rukuje vjerodajnice](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Dodatne informacije o događajima proširenje Azure DSC potražite u članku [Uvod u proširenje rukovatelj Azure želji stanje konfiguracije](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

Dodatne informacije o DSC PowerShell, [Posjetite centar za dokumentaciju PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 


