<properties
    pageTitle="Promjena veličine klasični VM Windows | Microsoft Azure"
    description="Promjena veličine na Windows virtualnog računala stvorene u modelu klasični implementaciju, pomoću komponente Powershell Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Promjena veličine Windows VM stvorene u model klasični implementacije

U ovom se članku objašnjava kako biste promijenili veličinu Windows VM stvorene u modelu uvođenje klasičnog pomoću komponente Powershell Azure.

Kada se preporučuje se mogućnost da biste promijenili veličinu na VM, postoje dva pojma koje kontroliraju raspon veličine dostupne da biste promijenili veličinu virtualnog računala. Prvi pojam je područje u kojem je implementiran na VM. Popis VM veličine dostupne u regiji je na kartici servisa Azure područja web-stranice. Drugi pojam je fizički hardverski trenutno hostira vaš VM. Fizička poslužitelje na kojima se VMs su grupirane u klastere uobičajenih fizičke hardvera. Način za promjenu veličina VM razlikuje se ovisno o ako željenu novu veličinu VM podržava klaster hardver koji se trenutno nalaze u VM.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Možete i [promijeniti veličinu VM stvorene u model implementacije Voditelj resursa](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Dodavanje računa

Morate konfigurirati Azure ljuske PowerShell za rad s resursima za klasični Azure. Slijedite korake u nastavku da biste konfigurirali Azure ljuske PowerShell za upravljanje klasični resursi.

1. U odzivniku komponente PowerShell upišite `Add-AzureAccount` i pritisnite **Enter**. 
2. Unesite adresu e-pošte povezanog s pretplatom na Azure, a zatim kliknite **Nastavi**. 
3. Upišite u okvir Lozinka za račun. 
4. Kliknite **Prijava**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Promjena veličine u istom klaster hardvera

Da biste promijenili veličinu VM veličinu koja je dostupna u klaster hardver koji se nalaze na VM, poduzeti sljedeće korake.

1. Pokrenite sljedeću naredbu komponente PowerShell na popisu dostupni u skupini hardver hosting servisa u oblaku koja sadrži na VM veličina VM.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Pokrenite sljedeće naredbe za promjenu veličine na VM.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Promjena veličine na novi hardver klaster

Da biste promijenili veličinu VM na veličinu nije dostupna u skupini hardver hosting VM oblaka usluga i sve VMs u servis u oblaku morate moguće ponovno stvoriti. Svaki servis u oblaku nalazi se na jednom hardver klaster tako da sva VMs u servis u oblaku mora biti veličine podržana za hardver klaster. Sljedeći koraci opisuju će kako promijeniti veličinu na VM brisanju i vraćanju servisa u oblaku.

1. Pokrenite sljedeću naredbu komponente PowerShell popis veličina VM u regiji. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Zabilježite sve konfiguracijske postavke za svaki VM u oblaku koja sadrži VM da biste promijeniti veličinu. 
3. Izbrišite sve VMs u oblaku odabirom mogućnosti da biste zadržali na diskova za svaki VM.
4. Ponovno stvorite VM za moguće promijeniti pomoću do željene veličine VM.
5. Ponovno stvorite sve VMs koja su u oblaku pomoću veličina VM dostupne u skupini hardver sada hostira servisa u oblaku.

Ogledne skripte za brisanju i vraćanju neki servis u oblaku pomoću novu veličinu VM nalazi se [ovdje](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Daljnji koraci

- [Promjena veličine VM stvorene u model implementacije Voditelj resursa](virtual-machines-windows-resize-vm.md).