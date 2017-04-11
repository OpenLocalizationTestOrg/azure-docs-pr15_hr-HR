<properties
    pageTitle="Implementacija predložaka sa servisom PowerShell u stogu Azure | Microsoft Azure"
    description="Saznajte kako implementirati virtualnog računala pomoću predloška Voditelj resursa i PowerShell."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Implementacija predložaka u stogu Azure pomoću komponente PowerShell

Pomoću komponente PowerShell implementirati predloške Azure upravljanja resursima u stogu PNA Azure.  Voditelj resursa predložaka uvođenje i dodjela resursa za sve resurse za aplikaciju u jednom, usklađenih postupak.

## <a name="run-azurerm-powershell-cmdlets"></a>Pokrenite Cmdlete ljuske AzureRM PowerShell

U ovom primjeru pokrenete skriptu za implementaciju virtualnog računala da biste PNA snop Azure pomoću predloška Voditelj resursa.  Prije nego što nastavite, provjerite je li imate [instalacije i konfiguracije PowerShell](azure-stack-connect-powershell.md)  

VHD koji se koristi u ovom primjeru predlošku je zadanu sliku trgovine (WindowsServer-2012 – R2-podatkovnog centra).

1.  Idite na <http://aka.ms/AzureStackGitHub>, potražite predložak **101 – jednostavno – windows – vm** i spremite je na sljedećem mjestu: c:\\predlošci\\azuredeploy-101 – jednostavno-windows-vm.json.

2.  U komponente PowerShell pokrenite sljedeću skriptu implementacije. Zamijenite *korisničkog imena* i *lozinke* korisničkog imena i lozinke. Na sljedećim koristi povećali vrijednost za parametar *$myNum* da biste onemogućili pisanje preko implementaciju sustava.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Otvorite portal stogu Azure, kliknite **Pregledaj**, kliknite **virtualnim strojevima**i potražite novog virtualnog računala (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Primjer videozapis: hibridna implementacija virtualnog računala

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Daljnji koraci

[Implementirati predloške pomoću Visual Studio](azure-stack-deploy-template-visual-studio.md)
