<properties
    pageTitle="Automatsko sigurnosno kopiranje za SQL Server virtualnim strojevima (Voditelj resursa) | Microsoft Azure"
    description="U članku se objašnjava značajka automatskog sigurnosnog kopiranja za SQL Server pokrenut na virtualnim računalima sustava Azure pomoću upravitelja resursa. "
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
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatsko sigurnosno kopiranje za SQL Server na Azure virtualnim računalima sustava (Voditelj resursa)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-sql-automated-backup.md)
- [Klasični](virtual-machines-windows-classic-sql-automated-backup.md)

Automatske sigurnosnog kopiranja automatski konfigurira [Upravlja sigurnosnu kopiju za Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) za sve baze podataka postojeće i nove na programa Azure VM sa sustavom SQL Server 2014 Standard ili Enterprise. To vam omogućuje da konfigurirate pravilnim sigurnosne kopije baze podataka koje koriste durable blobova platforme Azure. Automatske sigurnosnog kopiranja ovisi o [SQL Server IaaS Agent za nastavak](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model. Da biste pogledali klasičnu verziju ovog članka, potražite u članku [Automatsko sigurnosno kopiranje za SQL Server u Azure virtualnim strojevima klasični](virtual-machines-windows-classic-sql-automated-backup.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste koristili automatskog sigurnosnog kopiranja, imajte na umu sljedeće preduvjete:

**Operacijski sustav**:

- Windows Server 2012.
- Windows Server 2012 R2

**Verzija izdanje sustava SQL Server**:

- Standardna SQL Server 2014.
- Enterprise SQL Server 2014.

**Konfiguriranje baze podataka**:

- Baza podataka za ciljnu morate koristiti potpuni oporavak modela

**Azure PowerShell**:

- Ako planirate konfiguriranje automatizirano sigurnosno kopiranje sa servisom PowerShell, [instalirajte najnovije naredbe ljuske PowerShell Azure](../powershell-install-configure.md) .

>[AZURE.NOTE] Automatske sigurnosnog kopiranja ovisi o SQL Server IaaS Agent za nastavak. Trenutni SQL virtualnog računala Galerija slika dodajte ovaj kućni broj prema zadanim postavkama. Dodatne informacije potražite u članku [SQL Server IaaS Agent za nastavak](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Postavke

U sljedećoj su tablici opisane mogućnosti koje je moguće je konfigurirati za automatsko sigurnosno kopiranje. Navedeni koraci za konfiguraciju stvarni razlikuju se ovisno o tome koristite li Azure portal ili naredbe ljuske PowerShell sustava Windows Azure.

|Postavka|Raspon (zadano)|Opis|
|---|---|---|
|**Automatske sigurnosnog kopiranja**|Omogućiti ili onemogućiti (onemogućen)|Omogućuje ili onemogućuje automatizirano sigurnosno kopiranje za programa Azure VM sa sustavom SQL Server 2014 Standard ili Enterprise.|
|**Razdoblje zadržavanja**|1 – 30 dana (30 dana)|Broj dana koliko će se zadržavati sigurnosnu kopiju.|
|**Račun za pohranu**|Azure prostora za pohranu računa (u prostor za pohranu za navedeni VM stvara)|Račun Azure prostora za pohranu za pohranu datoteka automatizirano sigurnosno kopiranje u spremište blobova platforme. Spremnik stvorit će se na tom mjestu pohraniti sve datoteke sigurnosne kopije. Konvencije imenovanja datoteka sigurnosne kopije sadrži datum, vrijeme i naziv računala.|
|**Šifriranje**|Omogućiti ili onemogućiti (onemogućen)|Omogućuje ili onemogućuje šifriranje. Kada je omogućen šifriranja, certifikate koji se koriste za vraćanje sigurnosne kopije nalaze se u račun za navedeni prostora za pohranu u željeni spremnik automaticbackup koristeći istu konvencija imenovanja. Ako se promijeni lozinku, novi certifikat koji je generirao lozinku, ali stari certifikat ostaje da biste vratili prethodnu sigurnosne kopije.|
|**Lozinke**|Lozinka tekst (ništa)|Lozinka za ključeva za šifriranje. To je samo potreban je li omogućena šifriranje. Da biste vratili šifrirane sigurnosnu kopiju, morate imati ispravnu lozinku i povezane certifikat koji je korišten u trenutku snimljena sigurnosnu kopiju.|

## <a name="configuration-in-the-portal"></a>Konfiguriranje na portalu
Portal za Azure možete koristiti da biste konfigurirali automatizirano sigurnosno kopiranje tijekom dodjele resursa ili za postojeće VMs.

### <a name="new-vms"></a>Novi VMs
Pomoću portala za Azure Konfigurirajte automatsko sigurnosno kopiranje kada stvarate novo računalo virtualne SQL Server 2014 u modelu implementacije Voditelj resursa.

U plohu **SQL Server postavke** odaberite **Automatsko sigurnosno kopiranje**. Azure snimka za portala zaslona prikazuje plohu **SQL automatizirano sigurnosno kopiranje** .

![Konfiguracija SQL automatizirano sigurnosno kopiranje Azure portalu](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Kontekst, potražite u članku dovršeno temu na [Dodjeljivanje virtualnog računala sustava SQL Server u Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Postojeće VMs
Za postojeće SQL Server virtualnim strojevima, odaberite sustava SQL Server virtualnog računala. Zatim u odjeljku **konfiguracije SQL poslužitelja** plohu **Postavke** .

![SQL automatizirano sigurnosno kopiranje za postojeće VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

U plohu **konfiguracije SQL poslužitelja** , kliknite gumb **Uredi** u odjeljku Automated sigurnosne kopije.

![Konfiguriranje SQL automatizirano sigurnosno kopiranje za postojeće VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Kada završite, kliknite gumb **u redu** pri dnu plohu **konfiguracije SQL Server** da biste spremili promjene.

Ako prvi put su omogućivanje automatskog sigurnosnog kopiranja, Azure konfigurira IaaS agenta za SQL Server u pozadini. Za to vrijeme Azure portal možda neće prikazati je konfiguriran automatizirano sigurnosno kopiranje. Pričekajte nekoliko minuta agent za instalirati, konfigurirati. Nakon toga Azure portal odražavaju nove postavke.

>[AZURE.NOTE] Možete konfigurirati i automatskog sigurnosnog kopiranja pomoću predloška. Dodatne informacije potražite u članku [predloška Azure brzi početak rada za automatsko sigurnosno kopiranje](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Konfiguracija pomoću komponente PowerShell

Nakon dodjele resursa sustava SQL VM, pomoću komponente PowerShell Konfigurirajte automatsko sigurnosno kopiranje.

U sljedećem primjeru PowerShell automatizirano sigurnosno kopiranje je konfiguriran za na postojeće VM 2014 SQL Server. Naredba **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** konfigurira automatizirano sigurnosno kopiranje postavki za spremanje sigurnosne kopije u račun za Azure prostora za pohranu povezan s virtualnog računala. Ove sigurnosne kopije će biti zadržani 10 dana. Naredba **Postavi AzureRmVMSqlServerExtension** ažurira navedeni VM Azure te postavke.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

To može potrajati nekoliko minuta da biste instalirali i konfigurirali IaaS agenta za SQL Server.

Da biste omogućili šifriranja, izmijenite prethodna skripta za prosljeđivanje parametar **EnableEncryption** uz lozinku (sigurne niz) za parametar **CertificatePassword** . Sljedeću skriptu omogućuje automatsko sigurnosno kopiranje postavki u prethodnom primjeru i dodaje šifriranje.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Da biste onemogućili automatsko sigurnosno kopiranje, pokrenite isti skriptu bez na **– Omogućivanje** parametar naredbu **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** . Izostanak s **– Omogućivanje** parametar signale naredbu da biste onemogućili značajki. Kao što je s instalacijom, može potrajati nekoliko minuta da biste onemogućili automatsko sigurnosno kopiranje.

>[AZURE.NOTE] Uklanjanje IaaS agenta za SQL Server uklanja prethodno konfigurirane postavke automatskog sigurnosnog kopiranja. Trebali biste onemogućiti automatizirano sigurnosno kopiranje prije onemogućivanja ili deinstalacija IaaS agenta za SQL Server.

## <a name="next-steps"></a>Daljnji koraci

Automatske sigurnosnog kopiranja konfigurira upravlja sigurnosne kopije Azure VMs. Pa važno je da biste [pregledali potražite u dokumentaciji upravlja sigurnosnu kopiju](https://msdn.microsoft.com/library/dn449496.aspx) da biste shvatili ponašanje i posljedice.

Možete pronaći dodatne sigurnosnog kopiranja i vraćanja smjernice za SQL Server na Azure VMs u temi: [sigurnosno kopiranje i vraćanje za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-backup-recovery.md).

Informacije o drugih dostupnih automatizaciju zadataka potražite u članku [SQL Server IaaS Agent za nastavak](virtual-machines-windows-sql-server-agent-extension.md).

Dodatne informacije o pokretanju sustava SQL Server na Azure VMs potražite u članku [SQL Server na virtualnim računalima sustava Azure pregled](virtual-machines-windows-sql-server-iaas-overview.md).
