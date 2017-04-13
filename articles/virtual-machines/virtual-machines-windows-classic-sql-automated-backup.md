<properties
    pageTitle="Automatsko sigurnosno kopiranje za SQL Server virtualnim strojevima (klasični) | Microsoft Azure"
    description="U članku se objašnjava značajka automatskog sigurnosnog kopiranja za SQL Server pokrenut na virtualnim računalima sustava Azure pomoću upravitelja resursa. "
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

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatske sigurnosnog kopiranja za SQL Server na virtualnim Azure računalima (Classic)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-sql-automated-backup.md)
- [Klasični](virtual-machines-windows-classic-sql-automated-backup.md)

Automatske sigurnosnog kopiranja automatski konfigurira [Upravlja sigurnosnu kopiju za Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) za sve baze podataka postojeće i nove na programa Azure VM sa sustavom SQL Server 2014 Standard ili Enterprise. Omogućuje konfiguriranje običnog sigurnosne kopije baze podataka koje koriste durable blobova platforme Azure. Automatske sigurnosnog kopiranja ovisi o [SQL Server IaaS Agent za nastavak](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Voditelj resursa verziju ovog članka potražite u članku [Automatsko sigurnosno kopiranje za SQL Server u Voditelj resursa virtualnim strojevima Azure](virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste koristili automatskog sigurnosnog kopiranja, imajte na umu sljedeće preduvjete:

**Operacijski sustav**:

- Windows Server 2012.
- Windows Server 2012 R2

**Verzija izdanje sustava SQL Server**:

- Standardna SQL Server 2014.
- Enterprise SQL Server 2014.

>[AZURE.NOTE] SQL Server 2016 još nije podržana za automatsko sigurnosno kopiranje.

**Konfiguriranje baze podataka**:

- Baza podataka za ciljnu morate koristiti model potpuni oporavak.

**Azure PowerShell**:

- [Instalacija najnovije naredbe Azure PowerShell](../powershell-install-configure.md).

**SQL Server IaaS nastavak**:

- [Instaliranje IaaS proširenja za SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Postavke

U sljedećoj su tablici opisane mogućnosti koje je moguće je konfigurirati za automatsko sigurnosno kopiranje. Za klasični VMs, morate koristiti PowerShell te postavke konfigurirajte.

|Postavka|Raspon (zadano)|Opis|
|---|---|---|
|**Automatske sigurnosnog kopiranja**|Omogućiti ili onemogućiti (onemogućen)|Omogućuje ili onemogućuje automatizirano sigurnosno kopiranje za programa Azure VM sa sustavom SQL Server 2014 Standard ili Enterprise.|
|**Razdoblje zadržavanja**|1 – 30 dana (30 dana)|Broj dana koliko će se zadržavati sigurnosnu kopiju.|
|**Račun za pohranu**|Azure prostora za pohranu računa (u prostor za pohranu za navedeni VM stvara)|Račun Azure prostora za pohranu za pohranu datoteka automatizirano sigurnosno kopiranje u spremište blobova platforme. Spremnik stvorit će se na tom mjestu pohraniti sve datoteke sigurnosne kopije. Konvencije imenovanja datoteka sigurnosne kopije sadrži datum, vrijeme i naziv računala.|
|**Šifriranje**|Omogućiti ili onemogućiti (onemogućen)|Omogućuje ili onemogućuje šifriranje. Kada je omogućen šifriranja, certifikate koji se koriste za vraćanje sigurnosne kopije nalaze se u račun za navedeni prostora za pohranu u željeni spremnik automaticbackup koristeći istu konvencija imenovanja. Ako se promijeni lozinku, novi certifikat koji je generirao lozinku, ali stari certifikat ostaje da biste vratili prethodnu sigurnosne kopije.|
|**Lozinke**|Lozinka tekst (ništa)|Lozinka za ključeva za šifriranje. To je samo potreban je li omogućena šifriranje. Da biste vratili šifrirane sigurnosnu kopiju, morate imati ispravnu lozinku i povezane certifikat koji je korišten u trenutku snimljena sigurnosnu kopiju.|

## <a name="configuration-with-powershell"></a>Konfiguracija pomoću komponente PowerShell

U sljedećem primjeru PowerShell automatizirano sigurnosno kopiranje je konfiguriran za na postojeće VM 2014 SQL Server. Naredba **Nova AzureVMSqlServerAutoBackupConfig** konfigurira automatizirano sigurnosno kopiranje postavki za spremanje sigurnosne kopije u račun za Azure prostora za pohranu određen varijabla $storageaccount. Ove sigurnosne kopije će biti zadržani 10 dana. Naredba **Postavi AzureVMSqlServerExtension** ažurira navedeni VM Azure te postavke.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

To može potrajati nekoliko minuta da biste instalirali i konfigurirali IaaS agenta za SQL Server.

Da biste omogućili šifriranja, izmijenite prethodna skripta za prosljeđivanje parametar EnableEncryption uz lozinku (sigurne niz) za parametar CertificatePassword. Sljedeću skriptu omogućuje automatsko sigurnosno kopiranje postavki u prethodnom primjeru i dodaje šifriranje.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Da biste onemogućili automatsko sigurnosno kopiranje, pokrenite isti skriptu bez na **– Omogućivanje** parametar **Novo AzureVMSqlServerAutoBackupConfig**. Kao što je s instalacijom, može potrajati nekoliko minuta da biste onemogućili automatsko sigurnosno kopiranje.

>[AZURE.NOTE] Onemogućivanje i deinstalaciji IaaS agenta za SQL Server uklonite prethodno konfigurirane postavke upravlja sigurnosnu kopiju. Trebali biste onemogućiti automatizirano sigurnosno kopiranje prije onemogućivanja ili deinstalacija IaaS agenta za SQL Server.

## <a name="next-steps"></a>Daljnji koraci

Automatske sigurnosnog kopiranja konfigurira upravlja sigurnosne kopije Azure VMs. Pa važno je da biste [pregledali potražite u dokumentaciji upravlja sigurnosnu kopiju](https://msdn.microsoft.com/library/dn449496.aspx) da biste shvatili ponašanje i posljedice.

Možete pronaći dodatne sigurnosnog kopiranja i vraćanja smjernice za SQL Server na Azure VMs u temi: [sigurnosno kopiranje i vraćanje za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-backup-recovery.md).

Informacije o drugih dostupnih automatizaciju zadataka potražite u članku [SQL Server IaaS Agent za nastavak](virtual-machines-windows-classic-sql-server-agent-extension.md).

Dodatne informacije o pokretanju sustava SQL Server na Azure VMs potražite u članku [SQL Server na virtualnim računalima sustava Azure pregled](virtual-machines-windows-sql-server-iaas-overview.md).
