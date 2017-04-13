<properties
    pageTitle="SQL Server Agent proširenja za SQL Server VMs (klasični) | Microsoft Azure"
    description="U ovoj se temi opisuje kako upravljati agent proširenja sustava SQL Server koji automatizira određene administrativnih zadataka za SQL Server. To obuhvaća automatskog sigurnosnog kopiranja, automatski zakrpa i Azure ključ sigurnog integracije. U ovoj se temi koristi uvođenje klasičnog načina rada."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>SQL Server Agent proširenja za SQL Server VMs (Classic)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-sql-server-agent-extension.md)
- [Klasični](virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS Agent za nastavak (SQLIaaSAgent) pokreće Azure virtualnim strojevima za automatizaciju zadataka administracije. Ova tema sadrži pregled usluge podržava kućni broj, kao i upute za instalaciju, status i uklanjanje.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Voditelj resursa verziju ovog članka potražite u članku [Agent kućni broj za SQL Server VMs Voditelj resursa za SQL Server](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Podržane usluge

Proširenje Agent za SQL Server IaaS podržava sljedeće administrativnih zadataka:

| Značajka za administraciju | Opis |
|---------------------|-------------------------------|
| **SQL automatskog sigurnosnog kopiranja** | Automatizira planiranje sigurnosnih kopija za sve baze podataka za zadane instance sustava SQL Server u na VM. Dodatne informacije potražite u članku [Automatsko sigurnosno kopiranje za SQL Server na virtualnim računalima sustava Azure (klasični)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL automatski zakrpa** | Konfigurira održavanja prozor tijekom kojeg ažuriranjima sustava VM može obaviti, pa možete izbjeći ažurira tijekom vremena Vršna za svoje radno opterećenje. Dodatne informacije potražite u članku [automatski zakrpa za SQL Server na virtualnim računalima sustava Azure (klasični)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Integracija Azure sigurnog ključa** | Omogućuje vam automatski instalirate i konfigurirate Azure ključ sigurnog na vašem VM SQL Server. Dodatne informacije potražite u članku [Konfiguriranje Azure ključ sigurnog Integracija za SQL Server na Azure VMs (klasični)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Preduvjeti

Preduvjeti za korištenje proširenje Agent za SQL Server IaaS na vašem VM:

### <a name="operating-system"></a>Operacijski sustav:

- Windows Server 2012.
- Windows Server 2012 R2

### <a name="sql-server-versions"></a>Verzija sustava SQL Server:

- SQL Server 2012.
- SQL Server 2014.
- SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:

[Preuzimanje i konfiguriranje najnovije naredbe Azure PowerShell](../powershell-install-configure.md).

Pokrenite Windows PowerShell te je povežite s pretplate Azure pomoću naredbe **Dodaj AzureAccount** .

    Add-AzureAccount

Ako imate više pretplata, **Odaberite AzureSubscription** koristite da biste odabrali pretplatu koja sadrži vaš cilj klasični VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Sada možete dobiti popis klasični virtualnim strojevima i njihova imena pridružene usluge naredbom **Get-AzureVM** .

    Get-AzureVM

## <a name="installation"></a>Instalacija

Za klasični VMs, morate koristiti PowerShell možete instalirati SQL Server IaaS Agent proširenje i konfigurirati njegove servise za povezane. Pomoću cmdleta ljuske PowerShell **Skup AzureVMSqlServerExtension** da biste instalirali datotečni nastavak. Na primjer, sljedeću naredbu instalira proširenje na Windows Server VM (klasični) i pod nazivom "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Ako ažurirate na najnoviju verziju sustava SQL IaaS Agent proširenja, morate ponovno pokrenuti virtualnog računala nakon ažuriranja datotečni nastavak.

>[AZURE.NOTE] Klasični virtualnim računalima imati mogućnost instaliranja i konfiguriranja SQL IaaS Agent datotečni nastavak putem portala sustava.

## <a name="status"></a>Status

Jedan da biste provjerili je li instaliran proširenje tako da biste vidjeli stanje agent na portalu za Azure. Odaberite **sve postavke** plohu virtualnog računala, a zatim kliknite na **proširenja**. Trebali biste vidjeti proširenje **SQLIaaSAgent** na popisu.

![SQL Server IaaS Agent proširenje Azure portalu](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Možete koristiti i Azure Powershell cmdlet **Get-AzureVMSqlServerExtension** .

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Uklanjanje   

Na portalu za Azure deinstalirate proširenje klikom na tri točke na plohu **proširenja** svojstava virtualnog računala. Zatim kliknite **Izbriši**.

![Deinstalacija i nastavak IaaS Agent za SQL Server Azure portalu](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Možete koristiti i cmdleta ljuske Powershell **Ukloni AzureVMSqlServerExtension** .

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Daljnji koraci

Započnite pomoću nekog od servisa podržava datotečni nastavak. Dodatne pojedinosti potražite u temama u odjeljku [podržani services](#supported-services) ovog članka.

Dodatne informacije o pokretanju sustava SQL Server na virtualnim strojevima sa sustavom Azure potražite u članku [SQL Server na virtualnim računalima sustava Azure pregled](virtual-machines-windows-sql-server-iaas-overview.md).
