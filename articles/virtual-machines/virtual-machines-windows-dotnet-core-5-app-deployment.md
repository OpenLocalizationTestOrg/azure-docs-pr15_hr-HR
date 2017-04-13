<properties
   pageTitle="Automatizacija implementaciju aplikacije s datotečnim nastavcima virtualnog računala | Microsoft Azure"
   description="Praktični vodič DotNet Core Azure virtualnog računala"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Implementacija aplikacije s predlošcima Voditelj resursa za Azure

Kada Azure infrastructural preduvjeti imate prepoznati i prevede uvođenje predloška, stvarni aplikacije implementacije mora biti adresirane. Implementacija aplikacije ovdje je upućuju na instalacije binarne datoteke iz stvarnog aplikacije na Azure resursi. Primjer spremište glazbu, .net Core i IIS moraju biti instalacije i konfiguracije na svakom virtualnog računala. Binarne datoteke glazbu trgovine moraju biti instalirani na virtualnog računala i baze podataka za glazbu spremišta prethodno stvorili.

Ovaj dokument detalje o kako proširenja virtualnog računala automatiziraju implementaciju aplikacije i konfiguracije Azure virtualnih računala. Istaknute su sve zavisnosti i jedinstvene konfiguracije. Za najbolje mogućnosti rada prije implementacije instance komponente rješenja Azure pretplate i rad uz predloška Azure Voditelj resursa. Dovršavanje predložak Ovdje možete pronaći – [Glazbu implementacije trgovine u sustavu Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows).

## <a name="configuration-script"></a>Skripte za konfiguraciju

Proširenja virtualnog računala su specijalizirane programi koji se izvoditi na temelju virtualnim strojevima omogućuje automatizaciju konfiguracije. Proširenja dostupni su za posebne namjene kao što je antivirusni, konfiguracije zapisivanja i Docker konfiguracije. Proširenje za prilagođene skripte mogu se pokrenuti kod skripte virtualnog računala. S uzorak glazbu spremište je do prilagođene skripte proširenje za konfiguriranje virtualnim računalima sustava Windows i instalirajte aplikaciju glazbu spremište.

Prije nego što dovršenog kako su deklarirane proširenja virtualnog računala u predlošku Azure Voditelj resursa, pregledajte skriptu koja se pokreće. Ova skripta konfigurira virtualnog računala za Windows za hostiranje glazbu trgovine aplikacija. Prilikom pokretanja, skripta instalira sve potrebne softver, instalirajte aplikaciju iz trgovine glazbu iz izvora kontrole i priprema baze podataka. 

> Ovaj primjer je u svrhu pokazni.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>Proširenje VM skripte

VM proširenja mogu se izvoditi na temelju virtualnog računala trenutku Sastavi uključivanjem proširenje resursa u predlošku Voditelj resursa Azure. Proširenje možete dodati pomoću čarobnjaka za Visual Studio dodavanje resursa ili tako da umetnete valjani JSON u predlošku. Proširenje skripte resursa ugniježđen je u resursa virtualnog računala; To se može vidjeti u sljedećem primjeru.

Slijedite vezu da biste vidjeli JSON uzorak u predlošku Voditelj resursa – [VM skripte proširenje](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Obratite pozornost na to u u nastavku JSON skriptu pohranjen u GitHub. Ova skripta također biti pohranjen u spremište blobova platforme Azure. Predlošci Voditelj resursa Azure omogućuju niz izvođenja skripte za konstruirana tako da se vrijednosti parametara predložak moguće je koristiti kao parametar za skripte izvršavanja. U ovom slučaju podaci navedeni kada implementacija predložaka, a te vrijednosti se zatim može koristiti prilikom izvršavanja skriptu.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Dodatne informacije o korištenju prilagođene skripte proširenje potražite u članku [prilagođene skripte proširenja s predlošcima Voditelj resursa](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Sljedeći korak

<hr>

[Istražite više Azure resursima predložaka](https://github.com/Azure/azure-quickstart-templates)
