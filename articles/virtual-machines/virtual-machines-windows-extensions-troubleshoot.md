<properties
   pageTitle="Otklanjanje poteškoća s Windows VM kućni broj neuspjeha | Microsoft Azure"
   description="Dodatne informacije o otklanjanju pogreške proširenje Azure Windows VM"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Otklanjanje pogrešaka proširenje Azure Windows VM

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Prikaz stanja proširenja
Predlošci Azure Voditelj resursa možete izvršiti iz Azure PowerShell. Kada se izvršava predložak, status proširenje moguće je prikazati iz Azure resursa Explorer ili alata naredbenog retka.

Evo jednog primjera:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Evo ogledne izlaz:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Otklanjanje poteškoća s nastavkom neuspjeha

### <a name="re-running-the-extension-on-the-vm"></a>Ponovnim pokretanjem proširenje na na VM

Ako koristite skripte na na VM korištenje prilagođene skripte nastavka, ponekad možete naići pogrešku kojima VM uspješno je stvorena, ali nije uspjela skriptu. U odjeljku ove conditons preporučeni način za oporavak ta se pogreška je da biste uklonili proširenje i ponovno pokrenite predložak.
Napomena: U budućnosti, ta je funkcija će biti nadograđeni da biste uklonili potrebe za deinstalaciju datotečni nastavak.


#### <a name="remove-the-extension-from-azure-powershell"></a>Uklanjanje proširenje Azure PowerShell

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Kada datotečni nastavak uklonjena, predložak može biti ponovno izvedena pokrenuti skripte na VM.
