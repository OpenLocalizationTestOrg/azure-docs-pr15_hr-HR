<properties
    pageTitle="SQL Server Agent proširenja za SQL Server VMs (Voditelj resursa) | Microsoft Azure"
    description="U ovoj se temi opisuje kako upravljati agent proširenja sustava SQL Server koji automatizira određene administrativnih zadataka za SQL Server. To obuhvaća automatskog sigurnosnog kopiranja, automatski zakrpa i Azure ključ sigurnog integracije. U ovoj se temi koristi način implementacije Voditelj resursa."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>SQL Server Agent proširenja za SQL Server VMs (Voditelj resursa)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-sql-server-agent-extension.md)
- [Klasični](virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS Agent za nastavak (SQLIaaSExtension) pokreće Azure virtualnim strojevima za automatizaciju zadataka administracije. Ova tema sadrži pregled usluge podržava kućni broj, kao i upute za instalaciju, status i uklanjanje.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model. Da biste pogledali klasičnu verziju ovog članka, potražite u članku [SQL Server Agent proširenja za SQL Server VMs klasični](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Podržane usluge

Proširenje Agent za SQL Server IaaS podržava sljedeće administrativnih zadataka:

| Značajka za administraciju | Opis |
|---------------------|-------------------------------|
| **SQL automatskog sigurnosnog kopiranja** | Automatizira planiranje sigurnosnih kopija za sve baze podataka za zadane instance sustava SQL Server u na VM. Dodatne informacije potražite u članku [Automated sigurnosne kopije za SQL Server na virtualnim računalima sustava Azure (Voditelj resursa)](virtual-machines-windows-sql-automated-backup.md).|
| **SQL automatski zakrpa** | Konfigurira održavanja prozor tijekom kojeg ažuriranjima sustava VM može obaviti, pa možete izbjeći ažurira tijekom vremena Vršna za svoje radno opterećenje. Dodatne informacije potražite u članku [Automated zakrpa za SQL Server na virtualnim računalima sustava Azure (Voditelj resursa)](virtual-machines-windows-sql-automated-patching.md).|
| **Integracija Azure sigurnog ključa** | Omogućuje vam automatski instalirate i konfigurirate Azure ključ sigurnog na vašem VM SQL Server. Dodatne informacije potražite u članku [Konfiguriranje Azure ključ sigurnog Integracija za SQL Server na Azure VMs (Voditelj resursa)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Preduvjeti

Preduvjeti za korištenje proširenje Agent za SQL Server IaaS na vašem VM:

**Operacijski sustav**:

- Windows Server 2012.
- Windows Server 2012 R2

**Verzija sustava SQL Server**:

- SQL Server 2012.
- SQL Server 2014.
- SQL Server 2016

**Azure PowerShell**:

- [Preuzimanje i konfiguriranje najnovije naredbe ljuske PowerShell za Azure](../powershell-install-configure.md)

## <a name="installation"></a>Instalacija

Proširenje Agent za SQL Server IaaS automatski se instalira prilikom dodjele resursa nešto Galerija slika za SQL Server virtualnog računala.

Ako stvorite sustava Windows Server samo za OS virtualnog računala, proširenje možete instalirati ručno pomoću cmdleta komponente PowerShell **Skup AzureVMSqlServerExtension** . Na primjer, sljedeću naredbu instalira proširenje na je samo za s operacijskim Sustavom Windows Server VM i pod nazivom "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Ako ažurirate na najnoviju verziju sustava SQL IaaS Agent proširenja, morate ponovno pokrenuti virtualnog računala nakon ažuriranja datotečni nastavak.

>[AZURE.NOTE] Ako instalirate proširenje Agent za SQL Server IaaS ručno na VM za poslužitelj sustava Windows, morate koristiti i upravljanje značajkama pomoću naredbe ljuske PowerShell. Portala sučelje nije dostupna samo za SQL Server Galerija slika.

## <a name="status"></a>Status

Jedan da biste provjerili je li instaliran proširenje tako da biste vidjeli stanje agent na portalu za Azure. Odaberite **sve postavke** plohu virtualnog računala, a zatim kliknite na **proširenja**. Trebali biste vidjeti proširenje **SQLIaaSExtension** na popisu.

![SQL Server IaaS Agent proširenje Azure portalu](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Možete koristiti i Azure Powershell cmdlet **Get-AzureVMSqlServerExtension** .

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Naredba prethodne potvrđuje agenta instalirana i daje informacije o statusu Općenito. Možete dobiti i određene podatke o stanju automatskog sigurnosnog kopiranja i zakrpa pomoću sljedeće naredbe.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Uklanjanje   

Na portalu za Azure deinstalirate proširenje klikom na tri točke na plohu **proširenja** svojstava virtualnog računala. Zatim kliknite **Izbriši**.

![Deinstalacija i nastavak IaaS Agent za SQL Server Azure portalu](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Možete koristiti i cmdleta ljuske Powershell **Ukloni AzureRmVMSqlServerExtension** .

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Daljnji koraci

Započnite pomoću nekog od servisa podržava datotečni nastavak. Dodatne pojedinosti potražite u temama u odjeljku [podržani services](#supported-services) ovog članka.

Dodatne informacije o pokretanju sustava SQL Server na virtualnim strojevima sa sustavom Azure potražite u članku [SQL Server na virtualnim računalima sustava Azure pregled](virtual-machines-windows-sql-server-iaas-overview.md).
