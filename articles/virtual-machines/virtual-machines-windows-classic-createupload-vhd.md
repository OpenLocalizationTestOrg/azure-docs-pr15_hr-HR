<properties
    pageTitle="Stvoriti i prenijeti sliku VM pomoću komponente Powershell | Microsoft Azure"
    description="Saznajte kako stvoriti i prenijeti generalizirano Windows Server sliku (VHD) pomoću model klasični implementacije i Azure Powershell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Stvaranje i prijenos VHD za poslužitelj sustava Windows Azure

U ovom se članku prikazuje kako prenijeti vlastite generalizirano VM slike kao virtualne tvrdi disk (VHD) da biste mogli koristiti da biste stvorili virtualnih računala. Dodatne informacije o i VHDs u Microsoft Azure potražite u članku [o i VHDs za virtualnih računala](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Također možete [prenijeti](virtual-machines-windows-upload-image.md) virtualnog računala pomoću modela Voditelj resursa. 

## <a name="prerequisites"></a>Preduvjeti

U ovom se članku pretpostavlja da imate:

- **Pretplata programa Azure** – ako ga nemate, možete ga [besplatno otvorite račun za Azure](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** – imate Microsoft Azure modul ljuske PowerShell sustava instalacije i konfiguracije da biste koristili svoju pretplatu. 

- **A . Datoteka VHD** - podržani Windows operacijskom sustavu spremaju u datoteku .vhd i priložiti virtualnog računala. Provjerite je li uloge poslužitelja koji se izvode na na VHD podržava Sysprep. Dodatne informacije potražite u članku [Sysprep podrška za uloge poslužitelja](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.IMPORTANT] Oblik VHDX nije podržan u Microsoft Azure. Disk možete pretvoriti u oblik VHD pomoću upravitelja Hyper-V ili [cmdlet Pretvori VHD](http://technet.microsoft.com/library/hh848454.aspx). Detalje potražite u članku [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Korak 1: Priprema na VHD 

Prije prenesete na VHD Azure, on mora GRG generalizirano korištenjem alata Sysprep. To Priprema VHD će se koristiti kao sliku. Detalje o Sysprep potražite u članku [upute za korištenje Sysprep: programa Uvod](http://technet.microsoft.com/library/bb457073.aspx). Sigurnosno kopirajte na VM prije pokretanja Sysprep.

Iz virtualnog računala koje je instaliran operacijski sustav da biste dovršili postupak u nastavku:

1. Prijavite se u operacijskom sustavu.

2. Otvorite prozor naredbenog retka kao administrator. Promijeniti direktorij **%windir%\system32\sysprep**, a zatim pokrenite `sysprep.exe`.

    ![Otvorite prozor naredbenog retka](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  Pojavit će se dijaloški okvir **Alat za pripremu sustava** .

    ![Pokrenite Sysprep](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  U **Alat za pripremu sustava**odaberite **Unesite sustava izvan okvira sučelje (OOBE)** , a zatim provjerite je li isključena **konf** .

5.  U odjeljku **Mogućnosti isključivanja**odaberite **isključivanje**.

6.  Kliknite **u redu**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Korak 2: Stvaranje računa za pohranu i spremnik

Da biste imali mjesto da biste prenijeli datoteku .vhd morate s računom za pohranu u Azure. Ovaj korak pokazuje kako stvoriti račun ili pak zatražite informacije potrebno s postojećeg računa. Zamjena varijable u &lsaquo; zagrade &rsaquo; s vlastitim podacima.

1. Prijava

        Add-AzureAccount

1. Postavite Azure pretplatu.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Stvaranje novog računa za pohranu. Naziv računa spremišta mora biti jedinstvena, 3 24 znakova. Naziv može biti bilo koju kombinaciju slova i brojeva. Morate navesti mjesto kao što su "Istok NAM"
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Postavljanje novog računa za pohranu kao zadani.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Stvorite novi spremnik.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Korak 3: Prijenos datoteke .vhd

[Dodavanje AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) možete koristiti da biste prenijeli na VHD.

U prozoru Azure PowerShell ste koristili u prethodnom koraku, upišite sljedeću naredbu i zamijeni varijable u &lsaquo; zagrade &rsaquo; s vlastitim podacima.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Korak 4: Dodavanje slike na popis prilagođene slika

Pomoću cmdleta [Dodaj AzureVMImage](https://msdn.microsoft.com/library/mt589167.aspx) da biste dodali sliku popis prilagođene slika.

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Daljnji koraci

Možete sada [stvoriti prilagođene VM](virtual-machines-windows-classic-createportal.md) pomoću sliku koju ste prenijeli.

