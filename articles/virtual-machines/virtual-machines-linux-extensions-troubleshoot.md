<properties
   pageTitle="Otklanjanje poteškoća s Linux VM kućni broj neuspjeha | Microsoft Azure"
   description="Informirajte se o otklanjanju poteškoća s Azure Linux VM kućni broj neuspjeha"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Otklanjanje poteškoća s Azure Linux VM kućni broj neuspjeha

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Prikaz stanja proširenja
Azure resursima predloške možete izvršiti iz EŽA Azure. Kada se izvršava predložak, status proširenje moguće je prikazati iz Azure resursa Explorer ili alata naredbenog retka.

Evo jednog primjera:

Azure EŽA:

      azure vm get-instance-view


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

## <a name="troubleshooting-extenson-failures"></a>Otklanjanje poteškoća Extenson pogreške:

### <a name="re-running-the-extension-on-the-vm"></a>Ponovnim pokretanjem proširenje na na VM

Ako koristite skripte na na VM korištenje prilagođene skripte nastavka, ponekad možete naići pogrešku kojima VM uspješno je stvorena, ali nije uspjela skriptu. U odjeljku ove conditons preporučeni način za oporavak ta se pogreška je da biste uklonili proširenje i ponovno pokrenite predložak.
Napomena: ubuduće, ta je funkcija će biti nadograđeni da biste uklonili potrebe za deinstalaciju datotečni nastavak.

#### <a name="remove-the-extension-from-azure-cli"></a>Uklanjanje proširenje EŽA Azure

      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Gdje "publsher naziv" odgovara vrsti proširenje iz izlaza "azure vm get-instanci – prikaz" i je naziv resursa proširenje iz predloška

Kada datotečni nastavak uklonjena, predložak može biti ponovno izvedena pokrenuti skripte na VM.
