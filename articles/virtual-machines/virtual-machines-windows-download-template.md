<properties
    pageTitle="Stvaranje VM slika iz programa Azure VM | Microsoft Azure"
    description="Saznajte kako stvoriti generalizirano VM sliku iz postojeće Azure VM stvorene u model implementacije Voditelj resursa"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Preuzimanje predloška za web VM

Kada stvorite na VM u Azure pomoću portala i PowerShell, predloška resursima automatski se stvara za vas. Ovaj predložak možete koristiti da biste brzo duplicirali implementacije. Predložak sadrži informacije o svim resursa u grupu resursa. Za virtualnog računala, to znači da spremnika predložak sve stvorenoj radi ispunjavanja VM u toj grupi resursa, uključujući mrežni resursi.

## <a name="download-the-template-using-the-portal"></a>Preuzimanje predloška pomoću portala

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Jedan izbornik koncentrator odaberite **virtualnih računala**.
3. S popisa odaberite virtualnog računala.
5. Odaberite **skripte za automatizaciju**.
6. Odaberite **Preuzimanje** i spremite .zip datoteke na lokalnom računalu.
7. Otvorite .zip datoteku, a zatim izdvojiti datoteke u mapu. Datoteka .zip će sadržavati:
    
    - deploy.ps1
    - deploy.SH 
    - deployer.rb
    - DeploymentHelper.cs
    - PARAMETERS.JSON
    - template.JSON

Datoteka .json je predložak.
    
## <a name="download-the-template-using-powershell"></a>Preuzimanje predloška pomoću komponente PowerShell

Možete preuzeti i datoteku predloška .json pomoću cmdleta [Izvoz AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) . Možete koristiti u `-path` parametar možete unijeti naziv datoteke i put datoteke .json. U ovom se primjeru pokazuje kako preuzeti predložak za grupu resursa pod nazivom **myResourceGroup** **C:\users\public\downloads** mapu na lokalnom računalu.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o implementaciji resurse pomoću predložaka potražite u članku [Vodič za upravljanje resursima predložak](../resource-manager-template-walkthrough.md).