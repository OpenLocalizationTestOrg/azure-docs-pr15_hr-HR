<properties
    pageTitle="Stvaranje programa VM Azure pomoću komponente PowerShell | Microsoft Azure"
    description="Korištenje ljuske PowerShell Azure i upravljanja resursima Azure jednostavno stvaranje nove VM sa sustavom Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Stvaranje Windows VM pomoću upravitelja resursa i komponente PowerShell

U ovom se članku objašnjava da biste brzo stvorili strojno virtualne Azure koja se izvodi Windows Server i resursima potrebne pomoću [Upravitelja resursa](../azure-resource-manager/resource-group-overview.md) i PowerShell. 

Koraci u ovom članku su potrebne za stvaranje virtualnog računala i traje 30 minuta da biste pomoću koraka. Primjer vrijednosti parametara u okviru naredbe zamijenite nazive smislene okruženju sustava.

## <a name="step-1-install-azure-powershell"></a>Korak 1: Instalacija Azure komponente PowerShell

Informacije o instalacija najnovije verzije sustava Azure PowerShell, Odabir pretplate i prijave na račun potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .
        
## <a name="step-2-create-a-resource-group"></a>Korak 2: Stvaranje grupa resursa

Svi resursi moraju se nalaziti u grupu resursa, tako da omogućuje da prvo stvorite.  

1. Umetnite popis dostupnih mjesta na kojem je moguće stvoriti resursi.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Postavljanje mjesta za resurse. Ta se naredba postavlja na mjesto na **centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Stvorite grupu resursa. Ta naredba stvara grupu resursa pod nazivom **myResourceGroup** na mjestu koje ste postavili.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Korak 3: Stvaranje računa za pohranu

Za pohranu virtualne tvrdi disk koji se koristi virtualnog računala koje ste stvorili potreban je [račun za pohranu](../storage/storage-introduction.md) . Imena pohranu račun mora biti između 3 i 24 znakova i možda sadrži brojeve i samo mala slova.

1. Testirajte naziv računa za pohranu za jedinstvenosti. Ta se naredba testira naziv **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Ako ta naredba Vrati **True**, predloženog naziva je jedinstveno unutar Azure. 
    
2. Stvorite račun za pohranu.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Korak 4: Stvaranje virtualne mreže

Sve virtualnim strojevima su [virtualne mreže](../virtual-network/virtual-networks-overview.md).

1. Stvaranje podmreže za virtualne mreže. Ta naredba stvara podmreže pod nazivom **mySubnet** s prefiks 10.0.0.0/24 na adresu.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Stvorite virtualne mreže. Ta naredba stvara virtualne mreže pod nazivom **myVnet** pomoću podmreže koju ste stvorili i na adresi prefiks **10.0.0.0/16**.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Korak 5: Stvaranje na javnu IP adresa i mrežne sučelja

Da biste omogućili komunikaciju s virtualnog računala u virtualne mreže, morate na [javnu IP adresa](../virtual-network/virtual-network-ip-addresses-overview-arm.md) i mrežno sučelje.

1. Stvorite na javnu IP adresu. Ta naredba stvara javnu IP adresu pod nazivom **myPublicIp** dodijeljeni koristite način od **dinamičke**.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Stvaranje mrežnog sučelja. Ta naredba stvara mrežno sučelje pod nazivom **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Korak 6: Stvaranje virtualnog računala

Sad kad ste sve na mjestu, vrijeme je za stvaranje virtualnog računala.

1. Pokrenite sljedeću naredbu da biste postavili naziv administratorskog računa i lozinku za virtualnog računala.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Lozinka mora biti na 12 123 znakova i imati barem jedan malim slovima znak, jedan znak pisani velikim slovima, jedan broj i jedan posebnih znakova. 
        
2. Stvaranje objekta konfiguracije za virtualnog računala. Ta se naredba stvara objekt konfiguracije pod nazivom **myVmConfig** koji određuje naziv u VM i veličine u VM.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Popis dostupnih veličine za virtualnog računala potražite u članku [veličine za virtualnim strojevima u Azure](virtual-machines-windows-sizes.md) .
    
3. Konfiguriranje postavki operacijski sustav na VM. Ta se naredba na VM postavlja naziv računala, vrsta operacijski sustav i vjerodajnice za račun.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Definiranje sliku da biste koristili Dodjela na VM. Ta se naredba definira sliku Windows Server da biste koristili u VM. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Dodatne informacije o odabiru slike da biste koristili potražite u članku [Kretanje i odaberite slika virtualnog računala za Windows Azure pomoću programa PowerShell ili na EŽA](virtual-machines-windows-cli-ps-findimage.md).
        
5. Dodavanje mrežnog sučelja koji ste stvorili konfiguracije.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Definirajte naziv i mjesto VM tvrdi disk. Datoteka za virtualne tvrdi disk je pohranjena u spremniku. Ta se naredba stvara disk u kontejner **vhds/WindowsVMosDisk.vhd** na računu za pohranu koji ste stvorili.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Dodajte podatke na disku operacijski sustav VM konfiguracije. Zamijenite vrijednost **$diskName** naziv diska operacijski sustav. Stvaranje varijablu, a zatim dodajte podatke na disku konfiguracije.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Na kraju, stvorite virtualnog računala.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Daljnji koraci

- Ako postoje problemi s implementaciju, sljedeći korak u kojoj se pogledajte [Otklanjanje poteškoća resursa grupe implementacijama pomoću portala za Azure](../resource-manager-troubleshoot-deployments-portal.md)
- Informirajte se o upravljanju virtualnog računala koju ste stvorili tako da pročitate [Upravljanje virtualnim strojevima pomoću upravitelja resursa Azure i PowerShell](virtual-machines-windows-ps-manage.md).
- Iskoristite prednost pomoću predloška za stvaranje virtualnog računala pomoću informacija u [Stvaranje virtualnog računala za Windows s predloškom Voditelj resursa](virtual-machines-windows-ps-template.md)
