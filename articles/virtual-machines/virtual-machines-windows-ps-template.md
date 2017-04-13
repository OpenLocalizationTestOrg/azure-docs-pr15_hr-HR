<properties
    pageTitle="Stvaranje na VM s predloškom resursima | Microsoft Azure"
    description="Pomoću predloška Voditelj resursa i PowerShell jednostavno stvaranje nove Windows virtualnog računala."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Stvaranje virtualnog računala za Windows s predloškom Voditelj resursa

Ovaj članak predstavlja predložak programa Azure Voditelj resursa i pokazuje kako uvesti pomoću komponente PowerShell. Predložak uvodi jedan virtualnog računala sa sustavom Windows Server u novi virtualne mreže s jednom podmreže.

Trebali biste trajati otprilike 20 minuta pomoću koraka u ovom članku.

> [AZURE.IMPORTANT] Ako želite da se vaša VM kao dio skupa dostupnost, dodajte skup prilikom stvaranja na VM. Trenutno ne postoji način da biste dodali na VM raspoloživost postavljanje kada je stvorena.

## <a name="step-1-create-the-template-file"></a>Korak 1: Stvaranje predloška datoteke

Možete stvoriti vlastiti predložak pomoću informacija koje se nalaze u [predlošcima za izradu Voditelj resursa Azure](../resource-group-authoring-templates.md). Možete i implementirati predloške koje su stvorene za iz [Azure predlošci za početak rada](https://azure.microsoft.com/documentation/templates/).

1. Otvorite uređivač omiljene tekst i dodajte potrebne elementa i element potrebna contentVersion:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parametri](../resource-group-authoring-templates.md#parameters) uvijek nisu potrebni, no oni omogućuju unos vrijednosti kad je implementiran u predložak. Dodajte parametre element i njegovih podređenih elemenata nakon contentVersion element:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Varijable](../resource-group-authoring-templates.md#variables) može se koristiti u predlošku da biste odredili vrijednosti koje se često mijenjaju ili vrijednosti koje je potrebno stvoriti od kombinacije vrijednosti parametara. Dodati varijable element nakon odjeljka parametara:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Resursi](../resource-group-authoring-templates.md#resources) kao što su virtualnog računala, virtualne mreže i račun za pohranu sljedeće su definirani u predlošku. Dodavanje sekcije resursi nakon odjeljka varijable:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] U ovom se članku stvara virtualnog računala na kojem je instalirana verzija operacijskog sustava Windows Server. Dodatne informacije o odabiru druge slike potražite u članku [Kretanje i slika odaberite Azure virtualnog računala pomoću komponente Windows PowerShell i EŽA Azure](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Spremite datoteku predloška kao *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Korak 2: Stvaranje datoteke parametre

Da biste odredili vrijednosti za parametre resursa koji su definirani u predlošku, stvorite parametara datoteku koja sadrži vrijednosti koje se koriste kada je implementiran u predložak.

1. U uređivaču teksta, kopirajte sadržaj JSON na novu datoteku pod nazivom *Parameters.json*:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Potražite dodatne informacije o [preduvjetima za korisničko ime i lozinku](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Spremite datoteku parametara.

## <a name="step-3-install-azure-powershell"></a>Korak 3: Instalacija Azure komponente PowerShell

Informacije o instalacija najnovije verzije sustava Azure PowerShell, Odabir pretplate i prijave na račun potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="step-4-create-a-resource-group"></a>Korak 4: Stvaranje grupa resursa

Svi resursi moraju biti implementirano u [grupu resursa](../azure-resource-manager/resource-group-overview.md).

1. Umetnite popis dostupnih mjesta na kojem je moguće stvoriti resursi.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Zamijenite vrijednost **$locName** na mjesto na popisu, na primjer **Središnje SAD -a**. Stvorite varijablu.

        $locName = "location name"
        
3. Zamijenite vrijednost **$rgName** naziv novu grupu resursa. Stvorite varijablu i grupu resursa.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Trebali biste vidjeti nešto kao u ovom primjeru:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Korak 5: Stvaranje resursa s predloška i parametre

Zamijenite vrijednost **$templateFile** put i naziv datoteke predloška. Zamijenite vrijednost **$parameterFile** put i naziv datoteke parametara. Stvaranje varijabli i uvođenje predloška. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Možete i implementirati predloške i parametara s računa za Azure prostora za pohranu. Dodatne informacije potražite u članku [Korištenje Azure PowerShell s Azure prostora za pohranu](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Daljnji koraci

- Ako postoje problemi s implementaciju, sljedeći korak u kojoj se pogledajte [Otklanjanje poteškoća resursa grupe implementacijama pomoću portala za Azure](../resource-manager-troubleshoot-deployments-portal.md)
- Informirajte se o upravljanju virtualnog računala koju ste stvorili tako da pročitate [Upravljanje virtualnim strojevima pomoću upravitelja resursa Azure i PowerShell](virtual-machines-windows-ps-manage.md).
