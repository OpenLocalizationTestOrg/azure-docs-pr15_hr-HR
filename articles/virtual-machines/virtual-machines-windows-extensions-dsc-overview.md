<properties
   pageTitle="Želite stanja konfiguracija za Azure pregled | Microsoft Azure"
   description="Pregled za korištenje rukovatelj Proširenje Microsoft Azure za konfiguraciju stanje želji PowerShell. Obuhvaćaju preduvjete, arhitektura, a zatim cmdleta..."
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

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Uvod u proširenje rukovatelj konfiguracije stanje želji Azure #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure VM Agent i pridruženi proširenja su dio paketa Microsoft Azure infrastrukture Services. Proširenja VM su komponente softvera proširiti mogućnosti funkcije VM i pojednostavniti razne operacije upravljanja VM. Na primjer, proširenje VMAccess može se koristiti za ponovno postavljanje administratorske lozinke ili proširenje za prilagođene skripte mogu se izvršiti skripte na na VM.

U ovom se članku predstavlja proširenje PowerShell želji stanje konfiguracije (DSC) za Azure VMs kao dio Azure PowerShell SDK. Novi cmdleti za možete koristiti da biste prenijeli i Primjena PowerShell DSC konfiguracije na programa Azure VM omogućeno s nastavkom PowerShell DSC. Na PowerShell DSC nastavak poziva u PowerShell DSC enact primljene DSC konfiguracije na na VM. Ta je funkcija dostupna i putem portala za Azure.

## <a name="prerequisites"></a>Preduvjeti ##
**Lokalno računalo** Za interakciju s nastavkom Azure VM, morate koristiti portal za Azure ili Azure PowerShell SDK. 

**Agent za goste** Azure VM koji će se konfigurirati tako da konfiguracije DSC mora biti OS-a koji podržava ili Windows Management Framework (WMF) 4.0 ili 5.0. [Povijest verzija proširenje DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/)moguće je pronaći čitavog popisa podržanih verzija OS-a.

## <a name="terms-and-concepts"></a>Uvjeti i koncepti ##
Ovaj vodič presumes poznavanje s konceptima sljedeće:

Konfiguracija – DSC konfiguracije dokumenta. 

Čvor - odredišta DSC konfiguracije. U ovom dokumentu "čvor" uvijek se odnosi na VM Azure.

Konfiguracija podataka – na .psd1 datoteke koji sadrže podatke okolini za konfiguraciju

## <a name="architectural-overview"></a>Pregled arhitekture ##

Proširenje Azure DSC koristi framework Azure VM Agent za izlaganje, enact i stvaranje izvješća na DSC konfiguracije koji se izvode na Azure VMs. Proširenje DSC očekuje .zip datoteku s barem konfiguracije dokumenta i skup navedeni putem komponente PowerShell SDK Azure ili putem portala za Azure parametri.

Kada datotečni nastavak zove se prvi put, pokrenut će se proces instalacije. Ovaj postupak instalira verziju od na Windows Management Framework (WMF) pomoću sljedećih logika:

1. Ako je Azure VM OS Windows Server 2016, koristi se nijedna akcija. Windows Server 2016 već ima najnoviju verziju programa PowerShell instaliran.
2. Ako u `wmfVersion` svojstvo nije naveden, tu verziju na WMF je instaliran osim ako nije kompatibilan s na VM OS.
3. Ako nema `wmfVersion` svojstvo nije naveden, u najnoviji primjenjivo na WMF je instalirana verzija.

Instalacija sustava WMF zahtijeva ponovno pokretanje. Nakon ponovnog pokretanja, preuzimanja datotečni nastavak .zip datoteke navedene u na `modulesUrl` svojstvo. Ako je to mjesto u spremište blobova platforme Azure, SAS tokena bude navedena u na `sasToken` svojstva za pristup datoteci. Kada u .zip se preuzimaju i raspakirane, funkcija konfiguracije definirano u `configurationFunction` je pokrenite da biste generirali MOF datoteku. Proširenje zatim pokreće `Start-DscConfiguration -Force` generirani MOF datoteku. Proširenje snima izlazne i zapisivanje je poništiti Status kanal Azure. Sada na DSC LCM rukuje nadzor i ispravak normalno. 

## <a name="powershell-cmdlets"></a>Cmdleta ljuske PowerShell ##

PowerShell cmdleti mogu se s ARM ili ASM pakiranje, objavljivanje i praćenje DSC nastavak implementacije. Sljedeće Cmdlete navedene su moduli ASM, ali "Azure" mogu zamijeniti "AzureRm" tako da koristi model OKVIRA. Na primjer, `Publish-AzureVMDscConfiguration` koristi ASM, gdje `Publish-AzureRmVMDscConfiguration` koristi OKVIRA. 

`Publish-AzureVMDscConfiguration`Otvara u konfiguracijskoj datoteci, skenira za zavisne DSC resurse i stvara .zip datoteku koja sadrži konfiguraciju i DSC resursima koji su potrebni za enact konfiguraciju. Možete i stvoriti paket lokalno pomoću na `-ConfigurationArchivePath` parametar. U suprotnom je objavljuje .zip datoteke sa spremištem blobova platforme Azure i sigurne s SAS token.

.Zip datoteku stvorenu u ovaj cmdlet ima .ps1 skripte za konfiguraciju u korijenu arhivsku mapu. Resursi imaju mapu modul smješta u mapu za arhiviranje. 

`Set-AzureVMDscExtension`umetati ga postavke potrebne nastavka PowerShell DSC u konfiguraciji objekt VM, koji se zatim može primijeniti na VM Azure s `Update-AzureVM`.

`Get-AzureVMDscExtension`dohvaća status proširenje DSC određeni VM. 

`Get-AzureVMDscExtensionStatus`dohvaća stanje konfiguracije DSC enacted putem rukovatelja DSC kućni broj. Ovu akciju mogu izvršiti na jednom VM ili grupu VMs.

`Remove-AzureVMDscExtension`Uklanja rukovatelj proširenje iz navedene virtualnog računala. Ovaj cmdlet ne **uklonili konfiguraciju,** deinstalirajte u WMF ili promijenite postavke primijenjene na virtualnog računala. Uklanja se samo ručicu za nastavak. 

**Ključne razlike u cmdleti za ASM i ARM**

- Cmdleti za ARM su sinkronizirano. Cmdleti za ASM su asinkronog.
- ResourceGroupName, VMName, ArchiveStorageAccountName, verzije i mjesto su nove potrebne parametre.
- ArchiveResourceGroupName je novi neobavezan parametar OKVIRA. Ovaj parametar možete odrediti kada vaš račun za pohranu pripada grupu resursa za različite od onog kojem je stvorena virtualnog računala.
- ConfigurationArchive ArchiveBlobName pod nazivom u OBLAK
- Naziv spremnika ArchiveContainerName pod nazivom u OBLAK
- StorageEndpointSuffix ArchiveStorageEndpointSuffix pod nazivom u OBLAK
- Parametar onemogućite dodana OKVIRA da biste omogućili automatsko ažuriranje rukovatelj nastavak na najnoviju verziju kao i kada je dostupna. Nnote taj parametar sadrži potencijal čega se izvršen na na VM kada je nova verzija u WMF. 


## <a name="azure-portal-functionality"></a>Azure portala funkcija ##
Pronađite klasični VM. U odjeljku postavke -> Općenito kliknite "Proširenja." Stvorit će se nova okna. Kliknite "Dodaj", a zatim odaberite PowerShell DSC.

Portal mora unos.
**Konfiguriranje modula ni skriptu**: to polje nije obavezno. Potreban je .ps1 datoteku koja sadrži skripte za konfiguraciju ili .zip datoteke s skripte za konfiguraciju .ps1 korijenske mape i svih zavisne resursa u modulu mape unutar u .zip. Mogu se kreirati pomoću na `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet obuhvaćeno Azure PowerShell SDK. U spremištu blobova korisnika osigurava SAS token se prenese datoteka .zip. 

**Konfiguracija podataka PSD1 datoteke**: to polje nije obavezno. Ako vašoj konfiguraciji zahtijeva konfiguracije podatkovne datoteke u .psd1, to polje koristite da biste ga odabrali, a zatim prenese na vaše spremište blobova platforme korisnika gdje je osigurani po SAS token. 
 
**Module-Qualified naziv konfiguracije**: .ps1 datoteke mogu imati više funkcija konfiguracije. Unesite naziv slijedi .ps1 skripte za konfiguraciju na '\' i naziv funkcije konfiguracije. Na primjer, ako skriptu .ps1 ima naziv "configuration.ps1", a konfiguracija je "IisInstall", unesite:`configuration.ps1\IisInstall`

**Konfiguracija argumenti**: Ako funkciju konfiguracije traje argumente, unesite ih u njega u obliku `argumentName1=value1,argumentName2=value2`. Imajte na umu ovaj oblik neki drugi oblik od kako konfiguracije argumente prihvaćaju pomoću cmdleta ljuske PowerShell ili Voditelj resursa predložaka. 

## <a name="getting-started"></a>Početak rada ##

Proširenje Azure DSC uzima u dokumentima konfiguracije DSC i enacts ih na Azure VMs. Slijedi jednostavnog primjera konfiguraciju. Spremiti lokalno kao "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Na sljedeći način postavite IisInstall.ps1 skripte na navedeni VM, izvršavanje konfiguraciju i ponovno izvješća o statusu.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Bilježenje u zapisnik ##

Zapisnici spremaju se u:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[broj verzije]

## <a name="next-steps"></a>Daljnji koraci ##

Dodatne informacije o DSC PowerShell, [Posjetite centar za dokumentaciju PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Pregledajte [predloška Azure Voditelj resursa za proširenje DSC](virtual-machines-windows-extensions-dsc-template.md
). 

Da biste pronašli dodatne funkcije možete upravljati pomoću komponente PowerShell DSC [Pregled Galerija PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) dodatne resurse DSC.

Detalje o prosljeđivanje osjetljive parametara u konfiguracijama, potražite u članku [Upravljanje vjerodajnice sigurno s nastavkom rukovatelj DSC](virtual-machines-windows-extensions-dsc-credentials.md).
