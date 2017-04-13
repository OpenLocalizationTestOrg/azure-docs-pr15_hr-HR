<properties
    pageTitle="Stvaranje kopije specijalizirane VM u Azure | Microsoft Azure"
    description="Saznajte kako stvoriti kopiju specijalizirane VM Windows radi u Azure, u modelu implementacije Voditelj resursa."
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
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Stvaranje kopije specijalizirane VM Windows radi u Azure 

U ovom se članku objašnjava korištenje alata AZCopy da biste stvorili kopiju u VHD iz specijalizirane VM Windows koja se izvodi u Azure. Kopiju sustava VHD možete koristiti da biste stvorili novi VM. 

- Ako želite kopirati generalizirano VM potražite u [članku Stvaranje VM slike iz postojeće generalizirano VM za Azure](virtual-machines-windows-capture-image.md).

- Ako želite prenijeti VHD iz lokalnog VM, kao što je stvoren pomoću značajke Hyper-V, pogledajte [Prijenos Windows VHD iz programa lokalnog VM za Azure](virtual-machines-windows-upload-image.md).


## <a name="before-you-begin"></a>Prije početka

Provjerite je li vam:

- Prikupite podatke o **izvorišne i odredišne račune za pohranu**. Za izvor VM morate imena pohranu računa i spremnik. Obično naziv spremnika bit će **vhds**. Morate imati račun za pohranu odredište. Ako ga već nemate, možete stvoriti pomoću na portalu (**Više usluge** > prostora za pohranu računi > Dodaj) ili pomoću cmdleta [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) . 

- Imate Azure [PowerShell 1.0](../powershell-install-configure.md) (ili noviji) instaliran.

- Ste preuzeli i instalirali [Alat za AzCopy](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>Deallocate na VM

Deallocate VM oslobađa VHD želite kopirati. 

- **Portal**: kliknite **virtualnim strojevima** > **myVM** > Zaustavi
- **PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` deallocates VM pod nazivom **myVM** u grupu resursa **myResourceGroup**.

**Status** VM na portalu za Azure mijenja iz **prekine** u **zaustavljen (deallocated)**.


## <a name="get-the-storage-account-urls"></a>Dohvaćanje URL-ovi za račun za pohranu

Potreban vam je URL-ovi za pohranu računa izvorišne i odredišne. URL-ovi izgledaju: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Ako već znate naziv računa i spremnik za pohranu, možete zamijeniti samo informacija između uglatim zagradama da biste stvorili URL. 

Da biste dobili URL možete koristiti portal za Azure ili Azure Powershell:

- **Portal**: kliknite **više usluga** > **račune za pohranu**  >  <storage account> **blob-ova** i izvornu datoteku VHD se vjerojatno nalazi u spremniku **vhds** . Kliknite **Svojstva** za spremnik i kopiranje teksta koje su označene **URL-a**. Morat ćete spremnika izvorišni i odredišni URL-ove. 

- **PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` dohvaća podatke za VM pod nazivom **myVM** u grupu resursa **myResourceGroup**. U rezultatima potražite u odjeljku **profil za pohranu** **Vhd Uri**. Prvi dio Uri je URL spremniku i u posljednjem dijelu je naziv OS VHD na VM.

## <a name="get-the-storage-access-keys"></a>Početak pristupnih tipki za pohranu

Pronađite pristupnih tipki za izvorišne i odredišne račune za pohranu. Dodatne informacije o pristupnih tipki potražite u članku [o Azure prostora za pohranu računa](../storage/storage-create-storage-account.md).

- **Portal**: kliknite **više usluga** > **račune za pohranu**  >  <storage account> **Sve postavke** > **pristupnih tipki**. Kopirajte ključ koji je označen kao **ključ1**.
- **PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` u grupu resursa **myResourceGroup**može vidjeti ključa za pohranu za račun za pohranu **mystorageaccount** . Kopirajte ključ koji je označen kao **ključ1**.


## <a name="copy-the-vhd"></a>Kopirajte na VHD 

Možete kopirati datoteke iz jedne račune za pohranu pomoću AzCopy. Za kontejner odredište Ako navedeni spremnik ne postoji, ona stvorit će se umjesto vas. 

Da biste koristili AzCopy, otvorite naredbeni redak na lokalnom računalu, a zatim dođite do mape u kojem je instaliran AzCopy. Bit će slično *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Da biste kopirali sve datoteke u spremniku, koristite **parametar** . To se može koristiti da biste kopirali OS VHD, a sve diskova podataka ako se ona nalazi u istom kontejner. U ovom se primjeru pokazuje kako da biste kopirali sve datoteke u spremniku **mysourcecontainer** u račun za pohranu **mysourcestorageaccount** spremnik **mydestinationcontainer** na računu za pohranu **mydestinationstorageaccount** . Zamijenite imena pohranu računi i spremnika vlastitu. Zamjena `<sourceStorageAccountKey1>` i `<destinationStorageAccountKey1>` vlastitim ključeve.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Ako samo želite kopirati određene VHD u kontejneru s više datoteka, možete odrediti i naziv datoteke pomoću parametar /Pattern. U ovom primjeru, kopirat će se samo datoteku pod nazivom **myFileName.vhd** .

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Kada završite, primit ćete poruku koja izgleda otprilike ovako:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Otklanjanje poteškoća

- Kada koristite AZCopy, ako se prikaže poruka "nije uspjela provjera zahtjev poslužitelj. Provjerite je li vrijednost zaglavlja autorizacije oblikovan pravilno uključujući potpis." i koristite ključ 2 ili tipku sekundarne prostora za pohranu, pokušajte pomoću ključa za pohranu primarnog ili 1.


## <a name="next-steps"></a>Daljnji koraci

- Možete stvoriti novu VM prema [priložite kopiju VHD da biste VM kao na disk OS](virtual-machines-windows-create-vm-specialized.md).












