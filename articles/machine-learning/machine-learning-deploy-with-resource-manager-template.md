<properties
    pageTitle="Implementacija strojnog učenja radni prostor putem predloška Azure resursima | Microsoft Azure"
    description="Kako implementirati radni prostor za Azure strojnog učenja pomoću predloška Azure Voditelj resursa"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Implementacija strojnog učenja radni prostor putem Azure Voditelj resursa

## <a name="introduction"></a>Uvod
Pomoću upravitelja resursa Azure koja se uvođenje predloška štedi vrijeme tako da date skalabilni način implementacije međusobno povezanih komponenti s Provjera valjanosti web-mjesta i pokušajte ponovno mehanizam. Da biste postavili radne prostore za učenje strojno Azure, na primjer, morate najprije konfigurirati račun Azure prostora za pohranu i implementacija radnog prostora. Zamislite ručno na taj način za stotine radnih prostora. Alternative lakše je koristiti predložak Azure Voditelj resursa za implementaciju sustava radnog prostora Azure strojno učenje i sve njezine ovisnosti. U ovom se članku vodi vas kroz postupak korak po korak. Sjajan pregled Azure upravljanja resursima potražite u članku [Pregled upravljanja resursima Azure](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Korak po korak: stvaranje radnog prostora za učenje računala
Ne možemo će Stvaranje grupe sustava Azure resursa, a zatim implementacija novi račun za Azure prostora za pohranu i novi Azure strojnog učenja radni prostor pomoću predloška Voditelj resursa. Nakon dovršetka implementaciju, ne možemo ispisat će važne informacije o radne prostore stvorene (primarni ključ, u workspaceID i URL-a u radni prostor).

### <a name="create-an-azure-resource-manager-template"></a>Stvaranje predloška Azure Voditelj resursa
Radni prostor za učenje računala potreban je račun za Azure prostor za pohranu za pohranu skup podataka povezan s njom.
Sljedeći predložak koristi naziv grupe resursa za generiranje naziv računa za pohranu i naziv radnog prostora.  Također koristi naziv računa spremišta kao svojstvo prilikom stvaranja radnog prostora.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Spremite ovaj predložak kao mlworkspace.json datoteku pod c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Implementacija grupi resursa koji se temelji na predlošku
* Otvaranje komponente PowerShell
* Instaliranje modula Azure Voditelj resursa i upravljanje servisom Azure  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Ove korake preuzeli i instalirali module potrebne da biste dovršili preostale korake. To samo je potrebno učiniti jednom u okruženju u kojem se izvodi naredbe ljuske PowerShell.   

* Autentičnost Azure  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Ovaj korak nije treba ponoviti za svaku sesiju. Kada se provjere autentičnosti informacija o pretplati mora biti prikazana.

![Račun za Azure][1]

Imamo pristup Azure, možemo stvoriti grupu resursa.

* Stvaranje grupa resursa

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Provjerite je li grupa resursa u pravilno dodjeli. **ProvisioningState** mora biti "je uspio."
Naziv grupe resursa koristi predložak za generiranje naziv računa za pohranu. Naziv računa spremišta mora biti između 3 i 24 znakova i koristiti brojeva i mala slova.

![Grupa resursa][2]

* Korištenje implementaciju grupiranje resursa, implementacije novog strojnog učenja radnog prostora.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Nakon dovršetka implementacijskih je jednostavne pristup svojstava radnog prostora koji je implementiran. Na primjer, možete pristupiti na primarni ključ tokena.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Drugi način za dohvaćanje tokeni postojeće radnog prostora je da biste koristili naredbu pozovite AzureRmResourceAction. Na primjer, navedete tokeni primarnih i sekundarnih svih radnih prostora.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Kada je radni prostor dodjeli, možete automatizirati i mnoge Azure strojnog učenja Studio zadatke [Modul ljuske PowerShell za Azure strojnog učenja](http://aka.ms/amlps).

## <a name="next-steps"></a>Daljnji koraci 
* Dodatne informacije o [Predlošcima resursima za izradu Azure](../resource-group-authoring-templates.md). 
* Morate pogledati [Spremište predložaka za Azure brzi početak rada](https://github.com/Azure/azure-quickstart-templates). 
* Pogledajte ovaj videozapis o [Azure Voditelj resursa](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
