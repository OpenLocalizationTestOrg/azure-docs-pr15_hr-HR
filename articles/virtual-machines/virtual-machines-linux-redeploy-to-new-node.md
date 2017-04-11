<properties 
    pageTitle="Ponovno implementirate Linux virtualnim strojevima | Microsoft Azure" 
    description="U članku se opisuje kako implementirati Linux virtualnih računala da biste smanjili problema s povezivanjem SSH." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Ponovno implementirate virtualnog računala da biste novi Azure čvor

Ako je okrenuta prema poteškoća koje se mogu pomoći pri otklanjanju poteškoća SSH ili aplikacije programa access za Azure virtualnog računala (VM), redeploying na VM. Kada ponovno implementirate u VM prelazi na VM novi čvor unutar Azure Infrastruktura i powers ga ponovno uključite, zadržava sve mogućnosti konfiguracije i pridružene resurse. U ovom se članku objašnjava implementirati VM pomoću Azure EŽA ili Azure portal.

> [AZURE.NOTE] Kada ponovno implementirate u VM gubi se privremene diska i ažuriraju dinamičku IP adresu povezanu sa sučeljem virtualne mreže. 


## <a name="using-azure-cli"></a>Korištenje Azure EŽA

Provjerite imate [Najnoviju Azure EŽA instaliran](../xplat-cli-install.md) na vašem računalu, a radite u načinu rada Voditelj resursa (`azure config mode arm`).

Koristite sljedeću naredbu Azure EŽA implementirati virtualnog računala:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Možete vidjeti status promijeni VM kao ona vodi kroz postupak redeploy. Na `PowerState` od na VM odlazak 'Pokretanje' "ažuriranje',"Počinjanje"i na kraju 'pokretanjem' vodi kroz postupak redeploying glavno računalo za nove. Provjera stanja VMs unutar grupe resursa:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Daljnji koraci
Ako imate poteškoća povezati svoje VM, možete pronaći određene pomoć za [Otklanjanje poteškoća s vezama SSH](virtual-machines-linux-troubleshoot-ssh-connection.md) ili [detaljne SSH korake za otklanjanje poteškoća](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md). Ako ne možete pristupiti aplikacije koje se izvode na vašem VM, možete čitati i [Otklanjanje poteškoća aplikacije](virtual-machines-linux-troubleshoot-app-connection.md).