<properties
   pageTitle="Upravljanje virtualnim strojevima pomoću ljuske PowerShell Azure | Microsoft Azure"
   description="Dodatne naredbe koje možete koristiti da biste automatizirali zadatke u upravljanju virtualnim računalima."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Upravljanje virtualnim strojevima pomoću ljuske PowerShell za Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Mnoge zadatke učinite svakog dana da biste upravljali vaše VMs moguće je automatizirati pomoću cmdleta ljuske PowerShell Azure. U ovom se članku vam primjer naredbe za jednostavniji zadatke i veze na članke koji se prikazuju naredbe za složenije zadatke.

>[AZURE.NOTE] Ako još niste instalirali i konfigurirali Azure PowerShell još, možete dobiti upute u članku [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-use-the-example-commands"></a>Kako pomoću naredbe za primjer
Morate zamijeniti neki dio teksta u okviru naredbe s tekstom koji odgovara vašem okruženju. Na < i > simboli označavanje teksta morate zamijeniti. Kad zamijenite tekst, uklonite simbole, ali ostavite ponudu oznake na mjestu.

## <a name="get-a-vm"></a>Početak u VM
To je osnovni zadatak koji će se koristiti često. Pomoću njega možete pronaći informacije o na VM, izvršavanje zadataka na na VM ili Dohvati izlaz za pohranu u varijablu.

Radi dobivanja informacija o na VM, pokrenite sljedeću naredbu, sve unutar navodnika, uključujući zamjene u < i > znakova:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Da biste spremili Izlaz u varijablu $vm, pokrenite:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Prijavite se u VM sa sustavom Windows

Pokrenite sljedeće naredbe:

>[AZURE.NOTE] Naziv usluge virtualnog računala i oblaka možete pristupiti iz prikaz naredbe **Get-AzureVM** .
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Zaustavljanje na VM

Pokrenite sljedeću naredbu:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Taj parametar koristite da biste zadržali na virtualne IP (VIP) od servisa u oblaku u slučaju da je zadnji VM u taj servis u oblaku. <br><br> Ako koristite parametar StayProvisioned, od koje će se i dalje naplatiti za na VM.

## <a name="start-a-vm"></a>Pokretanje programa VM

Pokrenite sljedeću naredbu:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Prilaganje podatkovni disk
Ovaj zadatak zahtijeva nekoliko koraka. Prvo koristite na *** Dodaj-AzureDataDisk *** cmdlet za dodavanje disk $vm objektu. Nakon toga pomoću cmdleta **Ažuriranje AzureVM** da biste ažurirali konfiguraciju u VM.

Ćete morati odlučite želite li priložiti novi disk ili onu koja sadrži podatke. Novi disk, naredba stvara datoteku .vhd i pridružuje je.

Da biste priložili novi disk, pokrenite sljedeću naredbu:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Da biste priložili postojećih podataka na disku, pokrenite sljedeću naredbu:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Da biste priložili diskova podataka iz postojeće datoteke .vhd u spremište blobova platforme, pokrenite sljedeću naredbu:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Stvaranje VM sa sustavom Windows

Da biste stvorili novi utemeljen na sustavu Windows virtualni stroj u Azure, slijedite upute u [Koristite Azure PowerShell možete stvarati i prethodno konfigurirati virtualnim strojevima utemeljen na sustavu Windows](virtual-machines-windows-classic-create-powershell.md). U uputama tema kroz stvaranje Azure komponente PowerShell postavljena naredbe koje stvara utemeljen na sustavu Windows VM koje možete unaprijed konfigurirane:

- Pomoću servisa Active Directory članstvo u domeni.
- Pomoću dodatnih diskova.
- Kao član postojećeg uravnoteženja postavite.
- Pomoću statičke IP adrese.
