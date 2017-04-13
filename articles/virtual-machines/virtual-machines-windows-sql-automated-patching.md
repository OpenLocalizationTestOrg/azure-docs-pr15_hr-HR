<properties
    pageTitle="Automatski zakrpa za SQL Server VMs (Voditelj resursa) | Microsoft Azure"
    description="U članku se objašnjava značajka automatskog zakrpa za SQL Server virtualnim strojevima sa servisu Azure pomoću upravitelja resursa."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatski zakrpa za SQL Server na Azure virtualnim računalima sustava (Voditelj resursa)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-sql-automated-patching.md)
- [Klasični](virtual-machines-windows-classic-sql-automated-patching.md)

Automatsko zakrpa uspostavlja prozora Održavanje za programa Azure virtualne računalo sa sustavom SQL Server. Automatsko ažuriranje moguće je instalirati samo tijekom održavanja prozor. Za SQL Server, u ovom rescriction osigurava da se ažuriranja sustava i sve povezane ponovnog pokretanja pojaviti u trenutku najbolje moguće za bazu podataka. Automatsko zakrpa ovisi o [SQL Server IaaS Agent za nastavak](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model. Da biste pogledali klasičnu verziju ovog članka, potražite u članku [Automatski zakrpa za SQL Server u Azure virtualnim strojevima Classic](virtual-machines-windows-classic-sql-automated-patching.md).

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

- Ako planirate da biste konfigurirali automatski zakrpa PowerShell, [instalirajte najnovije naredbe ljuske PowerShell Azure](../powershell-install-configure.md) .

>[AZURE.NOTE] Automatsko zakrpa ovisi o SQL Server IaaS Agent za nastavak. Trenutni SQL virtualnog računala Galerija slika dodajte ovaj kućni broj prema zadanim postavkama. Dodatne informacije potražite u članku [SQL Server IaaS Agent za nastavak](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Postavke

U sljedećoj su tablici opisane mogućnosti koje je moguće je konfigurirati za automatski zakrpa. Navedeni koraci za konfiguraciju stvarni razlikuju se ovisno o tome koristite li Azure portal ili naredbe ljuske PowerShell sustava Windows Azure.

|Postavka|Moguće vrijednosti|Opis|
|---|---|---|
|**Automatski zakrpa**|Omogućiti ili onemogućiti (onemogućen)|Omogućuje ili onemogućuje automatski zakrpa za Azure virtualnog računala.|
|**Održavanje rasporeda**|Svaki dan, ponedjeljak, Utorak, srijeda, Četvrtak, Petak, subota, nedjelja|Raspored za preuzimanje i instaliranje ažuriranja sustava Windows, SQL Server i Microsoft virtualnog računala.|
|**Održavanje početka sat**|0-24|Lokalni početno vrijeme da biste ažurirali virtualnog računala.|
|**Održavanje prozoru trajanje**|30 180|Broj minuta dopušteno za preuzimanje i instalaciju ažuriranja.|
|**Kategorija zakrpa**|Važno|Kategorija ažuriranja da biste preuzeli i instalirali.|

## <a name="configuration-in-the-portal"></a>Konfiguriranje na portalu
Portal za Azure možete koristiti da biste konfigurirali automatski zakrpa tijekom dodjele resursa ili za postojeće VMs.

### <a name="new-vms"></a>Novi VMs
Pomoću portala za Azure konfigurirajte automatski zakrpa kada stvarate novo računalo virtualne SQL Server u modelu implementacije Voditelj resursa.

U plohu **SQL Server postavke** odaberite **Automated zakrpa**. Azure snimka za portala zaslona prikazuje plohu **Automatski zakrpa SQL** .

![SQL automatski zakrpa Azure portalu](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Kontekst, potražite u članku dovršeno temu na [Dodjeljivanje virtualnog računala sustava SQL Server u Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Postojeće VMs
Za postojeće SQL Server virtualnim strojevima, odaberite sustava SQL Server virtualnog računala. Zatim u odjeljku **konfiguracije SQL poslužitelja** plohu **Postavke** .

![SQL automatsko zakrpa za postojeće VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

U plohu **konfiguracije SQL poslužitelja** , kliknite gumb **Uređivanje** u Automated zakrpa sekcije.

![Konfiguriranje SQL automatski zakrpa za postojeće VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Kada završite, kliknite gumb **u redu** pri dnu plohu **konfiguracije SQL Server** da biste spremili promjene.

Ako su omogućivanje automatskog zakrpa prvi put, Azure konfigurira IaaS agenta za SQL Server u pozadini. Za to vrijeme Azure portal možda neće prikazati je automatski zakrpa konfiguriran. Pričekajte nekoliko minuta agent za instalirati, konfigurirati. Nakon toga Azure portal odražava nove postavke.

>[AZURE.NOTE] Možete konfigurirati i automatski zakrpa pomoću predloška. Dodatne informacije potražite u članku [Azure brzi početak rada predloška za automatski zakrpa](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).

## <a name="configuration-with-powershell"></a>Konfiguracija pomoću komponente PowerShell

Nakon dodjele resursa sustava SQL VM, koristite PowerShell za konfiguriranje automatski zakrpa.

U sljedećem primjeru PowerShell koristi se za konfiguriranje automatski zakrpa na postojeće VM SQL Server. Naredba **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** konfigurira novi prozor održavanje automatskog ažuriranja.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Ovisno o tome u ovom primjeru, u sljedećoj tablici opisane praktično efekt na odredištu Azure VM:

|Parametar|Efekt|
|---|---|
|**DayOfWeek**|Zakrpa instaliran svakog četvrtka.|
|**MaintenanceWindowStartingHour**|Početak obnavljaju 11:00 prijepodne.|
|**MaintenanceWindowsDuration**|Zakrpa mora biti instaliran unutar 120 minuta. Na temelju vremena početka, oni morate dovršiti 1:00 pm.|
|**PatchCategory**|Samo moguće postavku za taj parametar nije **Važno**.|

To može potrajati nekoliko minuta da biste instalirali i konfigurirali IaaS agenta za SQL Server.

Da biste onemogućili automatsko zakrpa, pokrenite isti skriptu bez na **– Omogućivanje** parametar **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Izostanak s **– Omogućivanje** parametar signale naredbu da biste onemogućili značajki.

## <a name="next-steps"></a>Daljnji koraci

Informacije o drugih dostupnih automatizaciju zadataka potražite u članku [SQL Server IaaS Agent za nastavak](virtual-machines-windows-sql-server-agent-extension.md).

Dodatne informacije o pokretanju sustava SQL Server na Azure VMs potražite u članku [SQL Server na virtualnim računalima sustava Azure pregled](virtual-machines-windows-sql-server-iaas-overview.md).
