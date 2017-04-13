<properties
   pageTitle="Omogućeno s datotečnim nastavcima Windows VM | Microsoft Azure"
   description="Dodatne informacije o omogućeno Voditelj resursa Azure s datotečnim nastavcima za VMs za Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Omogućeno s datotečnim nastavcima Windows VM Azure Voditelj resursa

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Iz Azure PowerShell pokrenite sljedeći cmdlet Azure PowerShell:

      Get-AzureVMAvailableExtension


Ovaj cmdlet vraća naziv izdavača, naziv proširenja i verziju na sljedeći način:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Ta tri svojstva mapirajte "publisher", "Vrsta" i "typeHandlerVersion" odnosno u iznad isječak predložak.

>[AZURE.NOTE]Uvijek preporučuje se najnoviju verziju kućni broj da biste pristupili slijedite funkciju najčešće ažurirani.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Označavanje sheme za nastavak konfiguracije parametre

Sljedeći korak s predloškom proširenja za izradu je prepoznavanje oblik za konfiguraciju parametara. Svaki nastavak podržava vlastiti skup parametara.

Da biste vidjeli konfiguracije uzorka za Windows extensions, potražite u članku [uzorka proširenja za Windows](virtual-machines-windows-extensions-configuration-samples.md).


Pogledajte sljedeće da biste u potpunosti dovršeno predložak s datotečnim nastavcima VM.

[Proširenje prilagođene skripte na Windows VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Nakon predložak za izradu, možete implementirati pomoću Azure PowerShell.
