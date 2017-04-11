<properties
    pageTitle="Uvoz datoteke BACPAC stvaranje baze podataka Azure SQL pomoću komponente PowerShell | Microsoft Azure"
    description="Uvoz datoteke BACPAC stvaranje baze podataka Azure SQL pomoću komponente PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Uvoz datoteke BACPAC stvaranje baze podataka Azure SQL pomoću komponente PowerShell

**Jedan baze podataka**

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Ovaj članak sadrži upute za stvaranje baze podataka Azure SQL tako da uvezete datoteku [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) sa servisom PowerShell.

Baza podataka stvorena je iz datoteke BACPAC (.bacpac) uvezeni iz programa spremnik blobova platforme Azure prostora za pohranu. Ako nemate BACPAC datoteke u Azure prostora za pohranu, potražite u članku [arhiva baze podataka Azure SQL BACPAC datoteku pomoću komponente PowerShell](sql-database-export-powershell.md). Ako već imate BACPAC datoteku koja nije u Azure prostor za pohranu, [Korištenje AzCopy jednostavno prenijeti na račun za Azure prostora za pohranu](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Baze podataka SQL Azure automatski stvara i održava sigurnosne kopije za svakog korisnika bazu podataka možete vratiti. Detalje potražite u članku [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md).


Da biste uvezli SQL baze podataka, potrebno je sljedeće:

- Azure pretplate. Ako vam je potrebna pretplata na Azure jednostavno kliknite **Besplatnu probnu verziju** pri vrhu ove stranice, a zatim vratite do kraja ovog članka.
- BACPAC datoteke baze podataka koju želite uvesti. Na BACPAC mora biti u spremniku za blob na [račun za Azure prostora za pohranu](../storage/storage-create-storage-account.md) .



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>Postavljanje varijable okruženju sustava

Postoji nekoliko varijable koju morate zamijeniti primjere vrijednosti određene vrijednosti za bazu podataka i račun za pohranu.

Naziv poslužitelja mora biti na poslužitelju na kojem se trenutno postoji u pretplatu u prethodnom koraku odabrali. Mora biti server u kojoj želite stvoriti u baze podataka. Nije podržan uvoz baze podataka izravno u elastic grupe aplikacija. No možete uvesti u jednu bazu podataka, a zatim kretanje baze podataka u zajedničko područje.

Naziv baze podataka je naziv za novu bazu podataka.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Sljedeće varijable su s računa za pohranu gdje se nalazi vaš BACPAC. [Portal za Azure](https://portal.azure.com)potražite na račun servisa za pohranu da biste dobili te vrijednosti. Primarni tipkovni prečac možete pronaći tako da kliknete **sve postavke** , a zatim **tipke** s računa za pohranu plohu.

Naziv blob je naziv postojeće BACPAC datoteke koje želite stvoriti bazu podataka. Morate unijeti kućni broj .bacpac.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Pokretanje [Get-vjerodajnicu] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) cmdlet otvara prozor traži korisničko ime i lozinku. Unesite prijava za administratore i lozinku za baze podataka SQL server ($ServerName od prethodnog), a ne korisničko ime i lozinka za račun za Azure.

    $credential = Get-Credential


## <a name="import-the-database"></a>Uvoz u bazu podataka

Ta se naredba šalje zahtjev za uvoz baze podataka na servis. Ovisno o veličini baze podataka, operacije uvoza može potrajati neko vrijeme da biste dovršili.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Praćenje napretka operacije

Nakon izvođenja [novo AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), možete provjeriti status zahtjeva tako da pokrenete u [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>Skripta za uvoz PowerShell baze podataka SQL


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Daljnji koraci

- Za povezivanje i upit uvezene SQL baze podataka potražite u članku [za povezivanje s bazom podataka SQL s SQL Server Management Studio i obaviti primjer T SQL upita](sql-database-connect-query-ssms.md)
