<properties
   pageTitle="Kućni broj prilagođene skripte na Windows VM | Microsoft Azure"
   description="Automatiziranje zadataka konfiguracije Azure VM pomoću prilagođene skripte proširenje da pokreću skripte komponente PowerShell na udaljenom VM za Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Prilagođene skripte proširenja za virtualnim strojevima sa sustavom Windows

U ovom se članku daje pregled uputa za korištenje proširenje za prilagođene skripte na Windows VMs pomoću cmdleta ljuske PowerShell Azure upravljanja API-ji servisa Azure.

Proširenja virtualnog računala (VM) u komponenti Microsoft i Pouzdani izdavači treće strane da biste proširili funkcionalnost na VM. Da biste saznali nastavci VM potražite u članku [Azure VM proširenja i značajke](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Prilagođene skripte nastavak pregled

S nastavkom prilagođene skripte za Windows, možete pokrenuti skripte komponente PowerShell na udaljenom VM bez prijave na njega. Skripte možete pokrenuti nakon dodjele resursa u VM ili u bilo kojem trenutku tijekom životnim ciklusom na VM bez otvaranja sve dodatne priključke. Najčešći slučajeva koristi za pokretanje prilagođene skripte proširenje obuhvaćaju pokrenut, instaliranja i konfiguriranja dodatnog softvera na na VM nakon što su mu dodijeljeni resursi.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Preduvjeti za pokretanje prilagođene skripte datotečni nastavak

1. Instalacija <a href="http://azure.microsoft.com/downloads" target="_blank">Azure PowerShell cmdleti za</a> verziju 0.8.0 ili noviji.
2. Ako želite da se skripte za pokretanje na postojeće VM, provjerite je li VM Agent je omogućena na VM. Ako nije instaliran, slijedite ove [korake](virtual-machines-windows-classic-agents-and-extensions.md). Ako je na VM stvorili s portala za Azure, VM Agent je instaliran po zadanom.
3. Prenesite skripte koju želite pokrenuti na VM Azure za pohranu. Skripte mogu potjecati iz spremniku jedan ili više potpisu.
4. Skripta se treba autor tako da se skripte stavka koju je započeo datotečni nastavak, pokreće druge skripti.

## <a name="custom-script-extension-scenarios"></a>Scenariji za nastavak prilagođene skripte

### <a name="upload-files-to-the-default-container"></a>Prijenos datoteka na zadani spremnik

Sljedeći primjer prikazuje kako možete pokrenuti skripte na na VM ako su u spremniku za pohranu od zadanog računa za svoju pretplatu. Prenesite skripte naziv spremnika. Zadani prostor za pohranu račun možete provjeriti pomoću na **Get-AzureSubscription – zadani** naredbe.

Sljedeći primjer stvara na VM, ali možete pokrenuti isti scenarij na postojeće VM.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Prijenos datoteka na spremnik za pohranu koji nisu zadani

Ovaj scenarij pokazuje kako koristiti spremniku koji nisu zadani prostor za pohranu unutar iste pretplate ili u neku drugu pretplatu za prijenos skripte i datoteka. U ovom se primjeru prikazuju postojeće VM, ali isti operacije mogu izvršiti dok stvarate na VM.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Prijenos skripte na više spremnika preko prostora za pohranu za različite račune

  Ako datoteka skripte pohranjuju se preko više spremnika, morate unijeti URL puni pristup zajedničkoj potpisa (SAS) za datoteke da biste pokrenuli skripte.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Dodajte nastavak prilagođene skripte na portalu za Azure

Idite na VM <a href="https://portal.azure.com/ " target="_blank">Azure portal</a> i dodati proširenje određivanjem datoteka skripte za pokretanje.

  ![Odredite skriptna datoteka][5]


### <a name="uninstall-the-custom-script-extension"></a>Deinstalacija proširenja za prilagođene skripte

Proširenje za prilagođene skripte s na VM možete deinstalirati pomoću sljedeće naredbe.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Proširenje za prilagođene skripte pomoću predložaka

Da biste saznali kako koristiti prilagođene skripte proširenje s predlošcima Azure Voditelj resursa, pogledajte odjeljak [Korištenje proširenja za prilagođene skripte za Windows VMs s predlošcima Azure Voditelj resursa](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
