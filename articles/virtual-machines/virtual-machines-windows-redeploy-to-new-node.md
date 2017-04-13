<properties 
    pageTitle="Ponovno implementirate virtualnim strojevima sa sustavom Windows | Microsoft Azure" 
    description="U članku se opisuje kako implementirati virtualnim računalima sustava Windows u smanjenju problema s povezivanjem RDP." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Ponovno implementirate virtualnog računala da biste novi Azure čvor

Ako ste je okrenuta prema poteškoća udaljene radne površine (RDP) za otklanjanje poteškoća veze ili aplikacije pristup utemeljen na sustavu Windows Azure virtualnog računala (VM) redeploying na VM može pomoći. Kada ponovno implementirate u VM prelazi na VM novi čvor unutar Azure Infrastruktura i powers ga ponovno uključite, zadržava sve mogućnosti konfiguracije i pridružene resurse. U ovom se članku objašnjava implementirati VM pomoću Azure PowerShell ili Azure portal.

> [AZURE.NOTE] Kada ponovno implementirate u VM gubi se privremene diska i ažuriraju dinamičku IP adresu povezanu sa sučeljem virtualne mreže. 

## <a name="using-azure-powershell"></a>Pomoću Azure komponente PowerShell

Pripazite da imate najnovija Azure PowerShell 1.x instaliran na vašem računalu. Dodatne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

Pomoću ove naredbe ljuske PowerShell Azure implementirati virtualnog računala:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Daljnji koraci
Ako imate poteškoća povezati svoje VM, možete pronaći određene pomoć za [Otklanjanje poteškoća s vezama RDP](virtual-machines-windows-troubleshoot-rdp-connection.md) ili [detaljne RDP korake za otklanjanje poteškoća](virtual-machines-windows-detailed-troubleshoot-rdp.md). Ako ne možete pristupiti aplikacije koje se izvode na vašem VM, možete čitati i [Otklanjanje poteškoća aplikacije](virtual-machines-windows-troubleshoot-app-connection.md).