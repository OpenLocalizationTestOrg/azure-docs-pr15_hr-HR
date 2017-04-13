<properties
    pageTitle="Stvaranje na VM na portalu klasični | Microsoft Azure"
    description="Stvaranje virtualnog računala za Windows Azure klasični portalu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Stvaranje virtualnog računala sa sustavom Windows Azure klasični portalu

> [AZURE.SELECTOR]
- [Azure klasični portal](virtual-machines-windows-classic-tutorial.md)
- [PowerShell: Uvođenje klasični](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako da biste [izveli taj postupak korištenja modela implementaciju upravljanja resursima](virtual-machines-windows-hero-tutorial.md) pomoću **Novi Azure portal**. 

Pomoću ovog praktičnog vodiča prikazuje kako stvoriti programa Azure virtualnog računala (VM) sa sustavom Windows Azure klasični portalu. Ćemo pomoću Windows Server slika, primjerice, ali to je samo jedan od više slika nudi Azure. Imajte na umu da ste odabrali sliku ovisi o pretplate. Na primjer, slika za radnu površinu sustava Windows možda dostupna za pretplatnike na MSDN.

U ovom se odjeljku objašnjava korištenje mogućnosti **Iz galerije** na portalu za Azure klasični da biste stvorili virtualnog računala. Ta mogućnost nudi dodatne mogućnosti konfiguracije od mogućnost **Brzo stvaranje** . Ako, na primjer, ako se želite pridružiti virtualnog računala virtualne mreže, morate koristiti mogućnost **Iz galerije** .

Možete stvoriti i VMs pomoću [svoje slike](virtual-machines-windows-classic-createupload-vhd.md). Dodatne informacije o ovoj i druge načine, potražite u članku [različitih načina stvaranja virtualnog računala za Windows](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Vodič za videozapis

Ovo je vodič ovog praktičnog vodiča.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Stvaranje virtualnog računala

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [stvoriti VM pomoću modela implementaciju upravljanja resursima](virtual-machines-windows-hero-tutorial.md) na novom portalu Azure. 

- Prijavite se na virtualnog računala. Upute potražite u članku [prijavite se na virtualnog računala sa sustavom Windows Server](virtual-machines-windows-classic-connect-logon.md).

- Priloži na disku radi pohrane podataka. Možete priložiti prazan diskova i diskova koji sadrže podatke. Upute potražite u članku [prilaganje podatkovni disk na računalo virtualnim Windows stvorene pomoću model klasični implementacije](virtual-machines-windows-classic-attach-disk.md).
