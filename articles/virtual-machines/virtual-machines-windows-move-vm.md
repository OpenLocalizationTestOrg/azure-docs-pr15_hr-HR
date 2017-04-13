<properties
    pageTitle="Premještanje prozora VM | Microsoft Azure"
    description="Premještanje Windows VM drugu Azure pretplatu ili resursa grupu u modelu implementacije Voditelj resursa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Premještanje Windows VM drugu Azure grupu pretplatu ili resursa 

U ovom se članku vodit će vas kroz kako premjestiti Windows VM između grupa resursa ili pretplate. Premještanje jedne pretplate biti korisno ako je izvorno stvorio je VM osobne pretplate i želite premjestiti pretplatu tvrtke na nastaviti s radom.

> [AZURE.NOTE] Novi resurs ID-a stvorit će se kao dio premještanje. Kada u VM premještena, morat ćete ažuriranje alata i skripte za korištenje resursa novi ID-a. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Premještanje na VM pomoću komponente Powershell

Da biste premjestili virtualnog računala u drugu grupu resursa, morate svakako premjestite i sve zavisne resurse. Da biste koristili cmdlet Premjesti AzureRMResource, potrebno je imati naziv resursa i vrstu resursa. Možete doći s cmdlet traži AzureRMResource.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Da biste premjestili VM moramo da biste premjestili više resursa. Radimo samo je stvorite zaseban varijable za svaki resurs i zatim popisu. U ovom se primjeru sadrži većinu osnovni resursa u VM, ali možete dodati više prema potrebi.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Da biste premjestili u drugu pretplatu, uvrstite parametar **- DestinationSubscriptionId** . 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Će se zatražiti da biste potvrdili da želite premjestiti navedenih resursa. Upišite **Y** da biste potvrdili da želite premjestiti u člancima.

  
## <a name="next-steps"></a>Daljnji koraci

Različite vrste resursa možete premještati između grupa resursa i pretplate. Dodatne informacije potražite u članku [Premještanje resursa ili pretplatu za novu grupu resursa](../resource-group-move-resources.md).    