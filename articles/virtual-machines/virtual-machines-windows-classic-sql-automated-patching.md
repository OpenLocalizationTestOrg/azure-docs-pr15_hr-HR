<properties
    pageTitle="Automatski zakrpa za SQL Server VMs (klasični) | Microsoft Azure"
    description="U članku se objašnjava značajka automatskog zakrpa za SQL Server virtualnim strojevima izvodi u Azure korištenje uvođenje klasičnog načina."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatski zakrpa za SQL Server na virtualnim Azure računalima (Classic)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-sql-automated-patching.md)
- [Klasični](virtual-machines-windows-classic-sql-automated-patching.md)

Automatsko zakrpa uspostavlja prozora Održavanje za programa Azure virtualne računalo sa sustavom SQL Server. Automatsko ažuriranje moguće je instalirati samo tijekom održavanja prozor. Za SQL Server na taj način da se ažuriranja sustava i sve povezane ponovnog pokretanja pojaviti u trenutku najbolje moguće za bazu podataka. Automatsko zakrpa ovisi o [SQL Server IaaS Agent za nastavak](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Voditelj resursa verziju ovog članka potražite u članku [Automatski zakrpa za SQL Server u Voditelj resursa virtualnim strojevima Azure](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste koristili automatski zakrpa, imajte na umu sljedeće preduvjete:

**Operacijski sustav**:

- Windows Server 2012.
- Windows Server 2012 R2

**Verzija programa SQL Server**:

- SQL Server 2012.
- SQL Server 2014.
- SQL Server 2016

**Azure PowerShell**:

- [Instalacija najnovije naredbe Azure PowerShell](../powershell-install-configure.md).

**SQL Server IaaS nastavak**:

- [Instaliranje IaaS proširenja za SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Postavke

U sljedećoj su tablici opisane mogućnosti koje je moguće je konfigurirati za automatski zakrpa. Za klasični VMs, morate koristiti PowerShell te postavke konfigurirajte.

|Postavka|Moguće vrijednosti|Opis|
|---|---|---|
|**Automatski zakrpa**|Omogućiti ili onemogućiti (onemogućen)|Omogućuje ili onemogućuje automatski zakrpa za Azure virtualnog računala.|
|**Održavanje rasporeda**|Svaki dan, ponedjeljak, Utorak, srijeda, Četvrtak, Petak, subota, nedjelja|Raspored za preuzimanje i instaliranje ažuriranja sustava Windows, SQL Server i Microsoft virtualnog računala.|
|**Održavanje početka sat**|0-24|Lokalni početno vrijeme da biste ažurirali virtualnog računala.|
|**Održavanje prozoru trajanje**|30 180|Broj minuta dopušteno za preuzimanje i instalaciju ažuriranja.|
|**Kategorija zakrpa**|Važno|Kategorija ažuriranja da biste preuzeli i instalirali.|

## <a name="configuration-with-powershell"></a>Konfiguracija pomoću komponente PowerShell

U sljedećem primjeru PowerShell koristi se za konfiguriranje automatski zakrpa na postojeće VM SQL Server. Naredba **Nova AzureVMSqlServerAutoPatchingConfig** konfigurira novi prozor održavanje automatskog ažuriranja.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Ovisno o tome u ovom primjeru, u sljedećoj tablici opisane praktično efekt na odredištu Azure VM:

|Parametar|Efekt|
|---|---|
|**DayOfWeek**|Zakrpa instaliran svakog četvrtka.|
|**MaintenanceWindowStartingHour**|Početak obnavljaju 11:00 prijepodne.|
|**MaintenanceWindowsDuration**|Zakrpa mora biti instaliran unutar 120 minuta. Na temelju vremena početka, oni morate dovršiti 1:00 pm.|
|**PatchCategory**|Samo moguće postavku za taj parametar nije "Važno".|

To može potrajati nekoliko minuta da biste instalirali i konfigurirali IaaS agenta za SQL Server.

Da biste onemogućili automatsko zakrpa, pokrenite isti skripte bez-parametar Omogući AzureVMSqlServerAutoPatchingConfig novo. Kao što je s instalacijom, može potrajati nekoliko minuta da biste onemogućili automatsko zakrpa.

## <a name="next-steps"></a>Daljnji koraci

Informacije o drugih dostupnih automatizaciju zadataka potražite u članku [SQL Server IaaS Agent za nastavak](virtual-machines-windows-classic-sql-server-agent-extension.md).

Dodatne informacije o pokretanju sustava SQL Server na Azure VMs potražite u članku [SQL Server na virtualnim računalima sustava Azure pregled](virtual-machines-windows-sql-server-iaas-overview.md).
