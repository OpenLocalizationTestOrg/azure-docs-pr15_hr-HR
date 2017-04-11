<properties
    pageTitle="Povezivanje Azure virtualnim strojevima prijava analitiku | Microsoft Azure"
    description="Za Windows i Linux virtualnim strojevima sa servisu Azure, preporučeni način prikupljene zapisnika i metriku je instalacijom prijava analitiku Azure VM datotečni nastavak. Da biste instalirali prijava analitiku proširenje virtualnog računala na Azure VMs možete koristiti portal za Azure ili PowerShell."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Povezivanje Azure virtualnim strojevima zapisnika Analytics

Za računala sa sustavom Windows i Linux, preporučeni način za prikupljanje zapisnika i metrike je instalacijom agent zapisnika analize.

Da biste instalirali agent prijava analitiku na Azure virtualnim strojevima najjednostavnije putem proširenja za VM zapisnika analize.  Korištenje proširenje pojednostavljuje postupak instalacije i automatski konfigurira agent za slanje podataka u radnom prostoru prijava analitiku koji navedete. Agenta i nadograđuje automatski jamči da imate najnovije značajke i rješenja.

Za Windows virtualnim strojevima omogućiti proširenja za *Microsoft Agent nadzor* virtualnog računala.
Za Linux virtualnim strojevima omogućiti *Linux OMS Agent za* proširenje virtualnog računala.

Dodatne informacije o [proširenja Azure virtualnog računala](../virtual-machines/virtual-machines-windows-extensions-features.md) i [Linux agent] (.. / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Kada koristite zbirke utemeljen na agent za podaci iz zapisnika, morate konfigurirati [izvore podataka u zapisniku analize](log-analytics-data-sources.md) da biste naveli zapisnika i metrike koje želite prikupiti.

>[AZURE.IMPORTANT] Ako konfiguriranje prijava analitiku podaci iz zapisnika indeks korištenjem [dijagnostike Azure](log-analytics-azure-storage.md)i konfiguriranje agent za prikupljanje isti zapisnika, zatim zapisnike prikuplja dvaput. Vam se naplatiti za oba izvora podataka. Ako imate agenta instaliran, a zatim treba prikupljanje zapisnika podataka korištenjem agent samostalno – ne konfiguriranje zapisnika analize da biste prikupili podatke iz zapisnika iz Azure Dijagnostika.

Postoje tri jednostavna načina da biste omogućili proširenje virtualnog računala analize zapisnika:

+ Pomoću portala za Azure
+ Pomoću ljuske PowerShell za Azure
+ Pomoću predloška programa Azure Voditelj resursa

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Omogućivanje proširenje VM na portalu za Azure

Možete instalirati agent za analizu zapisnika i povezati Azure virtualnog računala koja se izvršava na pomoću [portala za Azure](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Da biste instalirali agent zapisnika analize i povezivanje virtualnog računala u radni prostor zapisnika Analytics

1.  Prijavite se na [portal za Azure](http://portal.azure.com).
2.  Odaberite **Pregledaj** na lijevoj strani portalu i idite na **Prijava analitiku (OMS)** i odaberite ga.
3.  Na popisu prijava analitiku radnih prostora, odaberite onaj koji želite koristiti s Azure VM.  
    ![Radni prostori OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  U odjeljku **Upravljanje zapisnikom analize**odaberite **virtualnih računala**.  
    ![Virtualnim strojevima](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  Na popisu **virtualnim strojevima**odaberite virtualnog računala na kojem želite instalirati agenta. **OMS stanje veze** na VM označava da je **nije povezano**.  
    ![VM povezani s Internetom](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  U odjeljku detalja za virtualnog računala odaberite **Poveži**. Agenta je automatski instalacije i konfiguracije prijava analitiku radnog prostora. Ovaj postupak traje nekoliko minuta, koje vrijeme stanje veze OMS je *povezivanje...*  
    ![Povezivanje VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Kada instalirate i povezivanje agenta stanje **veze OMS** će se ažurirati da bi se prikazala **radni prostor**.  
    ![Povezani](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>Omogućivanje proširenje VM pomoću komponente PowerShell

Postoje različite naredbe za Azure klasični virtualnim računalima i resursima virtualnih računala. Slijede primjeri za klasični i resursima virtualnog računala.

Da biste postigli klasični virtualnim strojevima koristite u sljedećem primjeru PowerShell:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Da biste postigli resursima virtualnim strojevima koristite u sljedećem primjeru PowerShell:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Kada konfigurirate virtualnog računala pomoću komponente PowerShell, morate unijeti **ID radnog prostora** i **Primarni ključ**. Na stranici s **postavkama** portala OMS ili pomoću komponente PowerShell kao što je prikazano u prethodnom primjeru možete pronaći Id i ključ.

![Radni prostor ID i primarnog ključa](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Implementacija proširenje VM pomoću predloška

Pomoću upravitelja Azure resursa možete stvoriti jednostavnog predloška (u JSON OSNOVNI oblik) koji definira implementaciji i konfiguraciji aplikacije. Ovaj predložak je poznato kao predloška Voditelj resursa i njihovi deklarativno definirati implementacije. Pomoću predloška možete više puta implementacija aplikacije životni ciklus aplikacije te kako preuzeti pouzdanosti da su uvodi resursa u dosljedan stanju.

Uključivanjem agent prijava analitiku u sklopu predloška Voditelj resursa možete omogućiti da je unaprijed konfigurirana za radni prostor prijava analitiku svaki virtualnog računala.

Dodatne informacije o predlošcima resursima potražite u članku [Upravitelj resursa za Azure za izradu predložaka](../resource-group-authoring-templates.md).

Slijedi primjer Voditelj resursa predlošku koji se koristi za implementaciju virtualnog računala sa sustavom Windows s nastavkom Microsoft Agent za nadzor koji je instaliran. Ovaj predložak je predložak tipičnog virtualnog računala s sljedeće dodatke:

+ Parametri workspaceId i workspaceName
+ Odjeljak za nastavak Microsoft.EnterpriseCloud.Monitoring resursa
+ Izlaze da biste potražili workspaceId i workspaceSharedKey


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Predložak možete uvesti pomoću sljedeće naredbe ljuske PowerShell:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Otklanjanje poteškoća s virtualnim strojevima sa sustavom Windows

Ako agent proširenja za *Microsoft Agent nadzor* VM je instalacije ili izvješćivanja možete poduzeti sljedeće korake za otklanjanje poteškoća.

1. Provjerite je li instaliran agent za Azure VM i funkcionira ispravno pomoću koraka u [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
  + Možete pregledati i datoteka zapisnika VM agent`C:\WindowsAzure\logs\WaAppAgent.log`
  + Ako ne postoji u zapisniku, VM agent nije instaliran.
    - [Kliknite pločicu Azure VM Agent klasični VMs](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Potvrdite Microsoft Agent nadzor kućni broj intervala sustava zadatak je pokrenut na sljedeći način:
  + Prijavite se u sustav virtualnog računala
  + Otvorite raspored zadataka pa pronađite u `update_azureoperationalinsight_agent_heartbeat` zadatka
  + Potvrda zadatak je omogućen i sustavom svaki jedne minute
  + Prijava datoteke zapisnika intervala sustava`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Pregledajte Microsoft VM Agent za nadzor nastavak datoteke zapisnika sustava u`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Provjerite je li virtualnog računala možete pokrenuti skripte komponente PowerShell
4. Provjerite je li dozvole za C:\Windows\temp nisu mijenjani
5. Prikaz statusa Microsoft Agent nadzor upisivanjem sljedeće u dodatnim PowerShell prozora na virtualnog računala`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Pregledajte datoteke zapisnika instalacijskog programa Microsoft Agent za nadzor u`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Dodatne informacije potražite [Windows proširenja za otklanjanje poteškoća](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md).

## <a name="troubleshooting-linux-virtual-machines"></a>Otklanjanje poteškoća s Linux virtualnim strojevima

Ako agent za nastavak *OMS Agent za Linux* VM je instalacije ili izvješćivanja možete poduzeti sljedeće korake za otklanjanje poteškoća.

1. Ako je status proširenje *Nepoznato* provjerite je li instaliran agent za Azure VM i funkcionira ispravno po pregleda zapisnika VM agent`/var/log/waagent.log`
  + Ako ne postoji u zapisniku, VM agent nije instaliran.
  - [Kliknite pločicu Agent za Azure VM Linux VMs](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Za ostale dobro statusi pregledajte OMS Agent za nastavak Linux VM zapisnike datoteke u `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` i`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Ako je status proširenje dobar, ali podaci se prenosi pregledajte OMS Agent za datoteke zapisnika Linux u`/var/opt/microsoft/omsagent/log/omsagent.log`

Dodatne informacije potražite [Linux proširenja za otklanjanje poteškoća](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md).


## <a name="next-steps"></a>Daljnji koraci

+ Konfiguriranje [izvora podataka u zapisniku analize](log-analytics-data-sources.md) da biste naveli zapisnicima i metriku za prikupljanje.
+ Da biste prikupili podatke iz virtualne strojeva [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).
+ [Prikupljanje podataka korištenjem dijagnostike Azure](log-analytics-azure-storage.md) za ostale resurse koji se koriste u Azure.

Za računala koja se ne nalaze u Azure, možete instalirati agent prijava analitiku pomoću metode opisane u sljedećim člancima:

+ [S računala sa sustavom Windows zapisnika Analytics](log-analytics-windows-agents.md)
+ [Povežite računala Linux zapisnika Analytics](log-analytics-linux-agents.md)
