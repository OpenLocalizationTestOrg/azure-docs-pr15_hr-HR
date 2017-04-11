<properties
    pageTitle="Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću komponente PowerShell"
    description="Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću komponente PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)


U ovom članku navedene upute za arhiviranje baze podataka Azure SQL [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) datoteku (koji je spremljen u spremište blobova platforme Azure) pomoću komponente PowerShell.

Kada je potrebno da biste stvorili arhivu baze podataka Azure SQL, shemu baze podataka i podatke možete izvesti u datoteku BACPAC. Datoteka BACPAC se jednostavno ZIP datoteku s datotečnim nastavkom .bacpac. Datoteke BACPAC kasnije moguće pohraniti u spremište blobova platforme Azure ili lokalno spremište u lokalne lokacije. To se također mogu uvesti natrag u baze podataka SQL Azure ili je instalacija sustava SQL Server na lokalni.

**Razmatranja**

- Za arhiviranje da bi bio putem transakcije dosljedan, morate omogućiti da nema pisanje aktivnosti se pojavljuje prilikom izvoza ili koju izvozite iz [putem transakcije dosljedan kopiju](sql-database-copy.md) baze podataka Azure SQL.
- Maksimalna veličina datoteke BACPAC arhiviranih spremište blobova platforme Azure je 200 GB. Za arhiviranje veća BACPAC datoteka za lokalno spremište, koristite uslužni [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) naredbeni redak. Uslužni dolazi s Visual Studio i SQL Server. Možete [preuzeti](https://msdn.microsoft.com/library/mt204009.aspx) najnoviju verziju sustava SQL Server Data Tools da biste dobili uslužni.
- Arhiviranje u spremište Azure premium pomoću datoteke BACPAC nije podržana.
- Ako operaciju izvoza premašuje 20 sati, mogu se otkazati. Da biste poboljšali performanse tijekom izvoza, možete učiniti sljedeće:
 - Privremeno povećati razinu zaštite servisa.
 - Prestat sve za čitanje i pisanje aktivnosti prilikom izvoza.
 - Koristite [indeks](https://msdn.microsoft.com/library/ms190457.aspx) s vrijednostima koje nisu null na sve velike tablice. Izvoz možda bez grupirani indeksi neće uspjeti ako je potrebno više od 6 12 sati. To je jer servis za izvoz morate dovršiti pregled tablicu možete pokušati izvesti cijelu tablicu. Dobar način da biste odredili ako tablica su optimizirani za izvoz je da biste pokrenuli **DBCC SHOW_STATISTICS** i provjerite je li *RANGE_HI_KEY* nije null i njezina vrijednost je dobro raspodjele. Detalje potražite u članku [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs se ne treba koristiti za sigurnosno kopiranje i vraćanje operacije. Baze podataka SQL Azure automatski stvara sigurnosne kopije za svakog korisnika bazu podataka. Detalje potražite u članku [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md).

Da biste dovršili ovaj članak, potrebno je sljedeće:

- Azure pretplate.
- Baze podataka Azure SQL.
- [Račun za Azure standardne prostora za pohranu](../storage/storage-create-storage-account.md), s blob spremnik za pohranu na BACPAC u standardni prostora za pohranu.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Izvoz baze podataka

Na [New-AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) cmdlet šalje zahtjev za izvoz baze podataka na servis. Ovisno o veličini baze podataka, operaciju izvoza može potrajati neko vrijeme da biste dovršili.

> [AZURE.IMPORTANT] Da bi putem transakcije dosljedan BACPAC datoteke, trebali biste prvi [stvoriti kopiju baze podataka](sql-database-copy-powershell.md), a zatim izvezite kopiju baze podataka.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Praćenje napretka operaciju izvoza

Nakon izvođenja [novo AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), možete provjeriti status zahtjeva tako da pokrenete [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Pokretanje to odmah nakon zahtjev obično vraća **Status: u toku**. Kada se prikaže **Status: uspješno** na Izvoz je gotov.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Izvoz primjer baze podataka SQL

U sljedećem primjeru izvozi postojeću bazu podataka sustava SQL na BACPAC i pokazuje kako provjeriti stanje operacije izvoza.

Da biste pokrenuli u primjeru, postoji nekoliko varijable morate zamijeniti određene vrijednosti za vaš račun baze podataka i prostora za pohranu. [Portal za Azure](https://portal.azure.com)potražite na račun servisa za pohranu za naziv računa za spremište blobova platforme spremnik naziv, a vrijednost ključa. Možete pronaći ključ pritiskom na **tipke za pristup** na vaše plohu račun za pohranu.

Zamjena sljedeće `VARIABLE-VALUES` s vrijednostima za određene Azure resurse. Naziv baze podataka je postojeću bazu podataka koju želite izvesti.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali kako uvesti baze podataka Azure SQL pomoću komponente Powershell potražite u članku [Uvoz BACPAC pomoću komponente PowerShell](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Dodatni resursi

- [Novi AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)