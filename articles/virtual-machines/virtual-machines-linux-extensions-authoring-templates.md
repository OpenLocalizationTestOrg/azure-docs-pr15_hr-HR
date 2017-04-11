<properties
   pageTitle="Omogućeno s datotečnim nastavcima Linux VM | Microsoft Azure"
   description="Dodatne informacije o omogućeno Voditelj resursa Azure s datotečnim nastavcima za Linux VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Voditelj resursa Azure omogućeno s datotečnim nastavcima Linux VM

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Iz EŽA Azure, pokrenite sljedeće commnad:

      Azure VM extension list

Ta naredba Vrati publisher ime, naziv proširenja i verzije kao sljedeće:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Ta tri svojstva mapirajte "publisher", "Vrsta" i "typeHandlerVersion" odnosno u iznad isječak predložak.

>[AZURE.NOTE]Uvijek preporučuje se najnoviju verziju kućni broj da biste pristupili slijedite funkciju najčešće ažurirani.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Označavanje sheme za nastavak konfiguracije parametre

Sljedeći korak s predloškom proširenja za izradu je prepoznavanje oblik za konfiguraciju parametara. Svaki nastavak podržava vlastiti skup parametara.

Da biste vidjeli konfiguracije uzorka za Linux proširenja, kliknite u dokumentaciji pogledajte [primjere eExtensions Linux](virtual-machines-linux-extensions-configuration-samples.md).

Pogledajte sljedeće da biste u potpunosti dovršeno predložak s datotečnim nastavcima VM.

[Proširenje prilagođene skripte na Linux VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Nakon predložak za izradu, možete implementirati pomoću EŽA Azure.
