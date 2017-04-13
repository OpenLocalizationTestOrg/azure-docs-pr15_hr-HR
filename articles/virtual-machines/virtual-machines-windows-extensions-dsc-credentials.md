<properties
   pageTitle="Prosljeđivanje vjerodajnice na Azure pomoću DSC | Microsoft Azure"
   description="Pregled na sigurno prosljeđivanje vjerodajnice na virtualnim strojevima Azure pomoću komponente PowerShell želji stanje konfiguracije"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Prosljeđivanje vjerodajnice proširenje rukovatelj Azure DSC #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

U ovom se članku objašnjava želji stanje konfiguracije proširenje za Azure. Pregled ručicu za nastavak DSC možete pronaći na [Uvod u proširenje rukovatelj Azure želji stanje konfiguracije](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Prosljeđivanje vjerodajnice
U sklopu postupka konfiguracije, koje možda morate da postavite korisničke račune pristup servisima ili instalirajte program u kontekstu korisnika. Da biste učinite sljedeće, morate unijeti vjerodajnice. 

DSC omogućuje s parametrima konfiguracije u kojem ušli u konfiguraciji te sigurno pohranjuje datoteke MOF vjerodajnice. Proširenje rukovatelj Azure pojednostavnjuje upravljanje vjerodajnicama unosom automatsko upravljanje certifikata. 

Razmislite o sljedeću skriptu konfiguracije DSC koji stvara lokalni korisnički račun s određenu lozinku:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Važno je da *čvor localhost* kao dio uključiti konfiguracije. Ako nema izjavu o zaštiti sljedeće korake ne funkcionira kao rukovatelj proširenje posebno traži localhost izjava čvor. Također važno je da biste uključili typecast *[PsCredential]*kao određenu vrstu pokreće proširenja za šifriranje vjerodajnicu. 

Objavite ovu skriptu sa spremištem blobova:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Postavljanje proširenje Azure DSC i navedite vjerodajnicu:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Kako su zaštićene vjerodajnice
Pokretanje kod traži vjerodajnice. Kada nije naveden, sprema se u memoriji ukratko. Kada se objavljuje sa `Set-AzureVmDscExtension` cmdlet, ga prenosi putem HTTP da biste na VM, pri čemu je Azure pohranjuje šifrirane na disku, koristeći lokalne VM certifikata. Zatim se nakratko dešifrirati u memoriji i ponovno šifrirane za prosljeđivanje DSC.

Takvo ponašanje razlikuje se od [korištenja sigurne konfiguracije bez ručicu za nastavak](https://msdn.microsoft.com/powershell/dsc/securemof). Okruženje za Azure omogućuje način za prijenos podataka konfiguracije sigurno putem potvrde. Kada koristite ručicu za nastavak DSC, nema potrebe da navedete $CertificatePath ili na $CertificateID / $Thumbprint stavke u ConfigurationData.


## <a name="next-steps"></a>Daljnji koraci ##

Dodatne informacije o ručicu za nastavak Azure DSC potražite u članku [Uvod proširenje rukovatelj Azure želji stanje konfiguracije](virtual-machines-windows-extensions-dsc-overview.md). 

Pregledajte [predloška Azure Voditelj resursa za proširenje DSC](virtual-machines-windows-extensions-dsc-template.md).

Dodatne informacije o DSC PowerShell, [Posjetite centar za dokumentaciju PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Da biste pronašli dodatne funkcije možete upravljati pomoću komponente PowerShell DSC [Pregled Galerija PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) dodatne resurse DSC.
