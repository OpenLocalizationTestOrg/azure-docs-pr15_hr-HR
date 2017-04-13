<properties
    pageTitle="Ponovno postavljanje lozinke ili konfiguracije udaljene radne površine na Windows VM | Microsoft Azure"
    description="Saznajte kako ponovno postaviti lozinku za račun i servise udaljene radne površine na Windows VM pomoću portala za Azure ili Azure PowerShell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Kako vratiti servisu udaljene radne površine ili njegov lozinka za prijavu u Windows VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Ako ne možete povezati s Windows virtualnog računala (VM), možete ponovno postaviti lozinku lokalnog administratora ili ponovno postavi konfiguracija servisa za udaljene radne površine. Portal za Azure ili proširenje VM Access možete koristiti u Azure PowerShell ponovno postavljanje lozinke. Ako koristite PowerShell, provjerite imate najnoviju modul ljuske PowerShell instaliran na računalo, a vi ste prijavljeni u pretplatu za Azure. Detaljne upute, pročitajte [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md).

> [AZURE.TIP] Možete provjeriti verziju PowerShell koju ste instalirali pomoću`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windows VMs u modelu implementacije Voditelj resursa

### <a name="azure-portal"></a>Portal za Azure
Odaberite vaše VM tako da kliknete **Pregled** > **virtualnim strojevima** > *sustava Windows virtualnog računala* > **sve postavke** > **ponovno postavljanje lozinke**. Prikazuje se plohu za ponovno postavljanje lozinke:

![Stranica za ponovno postavljanje lozinke](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Unesite korisničko ime i novu lozinku, a zatim kliknite **Spremi**. Pokušajte se ponovno povezati s vašeg VM.

### <a name="vmaccess-extension-and-powershell"></a>Proširenje VMAccess i komponente PowerShell

Provjerite imate Azure PowerShell 1.0 ili noviju verziju, a ste se prijavili u račun pomoću na `Login-AzureRmAccount` cmdlet.

#### <a name="reset-the-local-administrator-account-password"></a>**Ponovno postavite lozinku lokalnog administratorskog računa**

Pomoću naredbe ljuske PowerShell [Postavljanje AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) možete ponovno postaviti administrator lozinka ili korisničko ime.

Stvaranje administratoru lokalne vjerodajnice računa pomoću sljedeće naredbe:

    $cred=Get-Credential

Ako upišete različit od trenutnog računa naziva, sljedeću naredbu proširenje VMAccess preimenuje lokalnog administratorskog računa, dodjeljuje lozinku za taj račun i problemi u događaj odjave udaljene radne površine. Ako je onemogućen lokalni administratorski račun, proširenje VMAccess omogućuje ga.

Korištenje VM nastavak programa access da biste postavili novu vjerodajnice na sljedeći način:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Zamjena `myRG`, `myVM`, `myVMAccess`, i mjesto s vrijednostima za sustav.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Ponovno postavljanje konfiguracija servisa za udaljene radne površine**

Daljinski pristup možete ponovno postaviti da vaš VM pomoću [Skupa AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) ili [Skup AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx)na sljedeći način. (Zamjena na `myRG`, `myVM`, `myVMAccess` i mjesto s vlastitim vrijednostima.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Ili:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Obje naredbe agent dodali na novu imenovani VM pristup virtualnog računala. Bilo kojem trenutku u VM može sadržavati samo na jednom VM pristup agent. Da biste postavili pristup VM agent svojstva uspješno, uklonite agent za access već postavio pomoću `Remove-AzureRmVMAccessExtension` ili `Remove-AzureRmVMExtension`. Počevši od Azure PowerShell verzije 1.2.2, možete izbjeći ovaj korak pri korištenju `Set-AzureRmVMExtension` s na `-ForceRerun` mogućnost. Prilikom korištenja `-ForceRerun`, provjerite je li koristiti isti naziv za pristup agent VM postavljen tako da prethodne naredbe.

Ako i dalje ne možete povezati daljinski virtualnog računala, pročitajte članak dodatne korake da biste pokušati [Otklanjanje poteškoća s veze udaljene radne površine da biste je utemeljen na sustavu Windows Azure virtualnog računala](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="windows-vms-in-the-classic-deployment-model"></a>Windows VMs u modelu klasični implementacije

### <a name="azure-portal"></a>Portal za Azure

Za virtualnim strojevima stvoren pomoću model klasični implementacije, možete koristiti za [Azure portal](https://portal.azure.com) da biste ponovno postavili servisu udaljene radne površine. Kliknite: **Pregled** > **virtualnim strojevima (klasični)** > *sustava Windows virtualnog računala* > **Vrati udaljene...**. Pojavljuje se sljedeća stranica.

![Vrati stranicu konfiguracije RDP](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Možete pokušati i vraćanje ime i lozinku lokalnog administratorskog računa. Kliknite: **Pregled** > **virtualnim strojevima (klasični)** > *sustava Windows virtualnog računala* > **sve postavke** > **ponovno postavljanje lozinke**. Pojavljuje se sljedeća stranica.

![Stranica za ponovno postavljanje lozinke](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Nakon što unesete novo korisničko ime i lozinku, kliknite **Spremi**.

### <a name="vmaccess-extension-and-powershell"></a>Proširenje VMAccess i komponente PowerShell

Provjerite je li instaliran VM Agent na virtualnog računala. Proširenje VMAccess ne moraju biti instalirani mogli koristiti, pod uvjetom da VM Agent dostupna. Provjerite je li VM Agent već instaliran pomoću sljedeće naredbe. (Zamijeniti "myCloudService" i "myVM" Nazivi servis u oblaku i VM, odnosno. Nazivi možete saznati tako da pokrenete `Get-AzureVM` bez parametre.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Naredba za **Pisanje host** prikazuje **True**, VM Agent je li instaliran. Prikazuje **False**, pogledajte upute i veze na preuzimanje u [VM Agent i proširenja - 2 dio](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure bloga.

Ako ste stvorili virtualnog računala pomoću portala, provjerite hoće li `$vm.GetInstance().ProvisionGuestAgent` vraća **vrijednost True**. Ako nije, možete je postaviti pomoću ove naredbe:

    $vm.GetInstance().ProvisionGuestAgent = $true

Ta se naredba onemogućuje sljedeće pogreške ako pokrenete naredbu **Postavi AzureVMExtension** u sljedećim koracima: "Dodjela resursa za goste Agent mora biti omogućen na objektu VM prije postavljanja nastavak IaaS VM programa Access."

#### <a name="reset-the-local-administrator-account-password"></a>**Ponovno postavite lozinku lokalnog administratorskog računa**

Stvaranje vjerodajnica za prijavu s trenutnom naziv lokalnog administratorskog računa i novu lozinku, a zatim pokrenite na `Set-AzureVMAccessExtension` na sljedeći način.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Ako upišete različit od trenutnog računa naziva, proširenje VMAccess preimenuje lokalnog administratorskog računa, dodjeljuje lozinku za taj račun i problemi s udaljene radne površine sign-out. Ako je onemogućen lokalni administratorski račun, proširenje VMAccess omogućuje ga.

Te naredbe Vrati i konfiguracija servisa za udaljene radne površine.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Ponovno postavljanje konfiguracija servisa za udaljene radne površine**

Da biste ponovno konfiguracija servisa za udaljene radne površine, pokrenite sljedeću naredbu:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

Proširenje VMAccess pokreće dvije naredbe na virtualnog računala:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Ta se naredba omogućuje ugrađenoj grupi Vatrozid za Windows uz čiju dolazne promet udaljene radne površine koja koriste TCP priključak 3389.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Ta se naredba postavlja na fDenyTSConnections vrijednost registra 0, omogućivanje veze udaljene radne površine.


## <a name="next-steps"></a>Daljnji koraci

Ako access proširenje Azure VM ne reagira, a ne možete ponovno postavljanje lozinke, možete [promijeniti lokalni Windows lozinku izvanmrežno](virtual-machines-windows-reset-local-password-without-agent.md). Ovaj postupak je napredniji postupak i povezivanje virtualne tvrdog diska problematična VM s drugom VM zahtijeva. Koraka navedenih u ovom članku, a samo pokušajte ponovno postavljanje metodu Izvanmrežna lozinke ne uspije.

[Azure VM proširenja i značajke](virtual-machines-windows-extensions-features.md)

[Povezivanje s Azure virtualnog računala s RDP ili SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Otklanjanje poteškoća s vezama udaljene radne površine da biste je utemeljen na sustavu Windows Azure virtualnog računala](virtual-machines-windows-troubleshoot-rdp-connection.md)
