<properties
    pageTitle="Premještanje Linux VM | Microsoft Azure"
    description="Premještanje Linux VM drugu Azure pretplatu ili resursa grupu u modelu implementacije Voditelj resursa."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Premještanje Linux VM drugu grupu pretplatu ili resursa

U ovom se članku vodit će vas kroz kako premjestiti Linux VM između grupa resursa ili pretplate. Premještanje na VM jedne pretplate biti korisno ako ste stvorili u VM osobne pretplate i želite premjestiti pretplate vaše tvrtke.

> [AZURE.NOTE] Novi resurs ID-a stvaraju se kao dio premještanje. Kada u VM premještena, morate ažurirati alata i skripte za korištenje resursa novi ID-a. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Premještanje na VM pomoću EŽA Azure 

Da biste uspješno premjestili na VM, morate premjestiti na VM i njegovih podrške resursa. Pomoću naredbe **azure grupa Prikaži** popis resursa u grupu resursa i ID-ova. Lakše pipe izlaz tu naredbu u datoteku tako da možete kopirati i zalijepiti ID-ove u noviji naredbe.

    azure group show <resourceGroupName>

Da biste premjestili na VM i njegovih resursa u drugu grupu resursa, koristite EŽA naredbu **Premjesti azure resursa** . Sljedeći primjer prikazuje kako možete premjestiti na VM i najčešće resursi bit će potrebno. Ne možemo koristiti parametar **-i** i prenesite na popisu odvojenih zarezom (bez razmaka) ID-ova za resurse programa da biste premjestili.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Ako želite da biste premjestili na VM i njegovih resursa neku drugu pretplatu, dodajte na **– odredište subscriptionId & #60; destinationSubscriptionID & #62;** parametar da biste odredili pretplate odredište.

Ako radite iz naredbenog retka na računalu sa sustavom Windows, morate dodati na **$** ispred nazivima varijabli kada ih deklariranje. To nije potrebno Linux.

Se od vas zatraži da biste potvrdili da želite premjestiti navedenog resursa. Upišite **Y** da biste potvrdili da želite premjestiti u člancima.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Daljnji koraci

Različite vrste resursa možete premještati između grupa resursa i pretplate. Dodatne informacije potražite u članku [Premještanje resursa ili pretplatu za novu grupu resursa](../resource-group-move-resources.md).    