<properties
   pageTitle="PowerShell cmdleti za Azure SQL Data Warehouse"
   description="Pronađite gornji PowerShell cmdleti za Azure SQL Data Warehouse uključujući kako zaustaviti i nastaviti baze podataka."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell cmdleti i REST API-ji za SQL Data Warehouse

Mnogih zadataka administracije SQL Data Warehouse njome se upravlja pomoću cmdleta ljuske PowerShell Azure ili REST API-ji.  U nastavku su neki primjeri kako pomoću naredbe ljuske PowerShell automatizirali uobičajene zadatke u SQL Data Warehouse.  OSTALE primjere dobar potražite u članku [Upravljanje skalabilnost s OSTATKOM][].

> [AZURE.NOTE]  Da biste koristili Azure PowerShell sa SQL Data Warehouse, morate Azure PowerShell verzije 1.0.3 ili noviji.  Možete provjeriti vašu verziju tako da pokrenete **modul za Get - ListAvailable-naziv Azure**.  Najnoviju verziju moguće je instalirati sa [Instalacijskog programa sustava Microsoft Web platforme][].  Dodatne informacije o instaliranju najnovije verziji, potražite u članku [kako instalirati i konfigurirati Azure PowerShell][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Početak rada s cmdleta ljuske PowerShell za Azure

1. Otvorite Windows PowerShell. 
2. Kada se zatraži PowerShell pokrenite te naredbe Prijava na upravitelj resursa Azure i odaberite svoju pretplatu.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Primjer skladištu za zaustavljanje SQL podataka

Zadržite pokazivač bazu pod nazivom "Database02" nalazi na poslužitelju pod nazivom "Server01."  Poslužitelj je u grupi Azure resursa pod nazivom "ResourceGroup1." 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Varijacije, u ovom se primjeru cijevi dohvaćene objekt [Suspend AzureRmSqlDatabase][].  Kao rezultat je zaustavljeno bazu podataka. Naredba konačan prikazuje rezultate.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Pokretanje primjer skladištu podataka za SQL

Nastavak operacija baze podataka pod nazivom "Database02" nalazi na poslužitelju pod nazivom "Server01." Poslužitelj je sadržan u grupu resursa pod nazivom "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Varijacije, u ovom se primjeru dohvaća podatke u bazu podataka pod nazivom "Database02" s poslužitelja pod nazivom "Server01", koji je sadržan u grupu resursa pod nazivom "ResourceGroup1." Ga cijevi dohvaćene objekt [Životopis AzureRmSqlDatabase][].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Imajte na umu da je poslužitelj foo.database.windows.net, koristite "odnožje" kao naziv poslužitelja - u cmdleta ljuske PowerShell.

## <a name="frequently-used-powershell-cmdlets"></a>Često koristi cmdleta ljuske PowerShell

Tih cmdleta ljuske PowerShell se često koriste s Azure SQL Data Warehouse.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Novi AzureRmSqlDatabase][]
- [Uklanjanje AzureRmSqlDatabase][]
- [Vraćanje AzureRmSqlDatabase][] 
- [Životopis AzureRmSqlDatabase][]
- [Odaberite AzureRmSubscription][]
- [Postavljanje AzureRmSqlDatabase][]
- [Privremeno obustavljanje AzureRmSqlDatabase][]

## <a name="next-steps"></a>Daljnji koraci
Još primjera PowerShell potražite u članku:

- [Stvaranje SQL Warehouse podataka pomoću komponente PowerShell][]
- [Vraćanje baze podataka][]

Popis svih zadataka koji možete automatski sa servisom PowerShell potražite u članku [Cmdleti za baze podataka SQL Azure][].  Popis zadataka koji možete automatski s OSTATKOM, potražite u članku [Postupci za baze podataka SQL Azure][].

<!--Image references-->

<!--Article references-->
[Kako instalirati i konfigurirati Azure PowerShell]: ./powershell-install-configure.md
[Stvaranje SQL Warehouse podataka pomoću komponente PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Vraćanje baze podataka]: ./sql-data-warehouse-restore-database-powershell.md
[Upravljanje skalabilnost s OSTATKOM]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Cmdleti za baze podataka Azure SQL]: https://msdn.microsoft.com/library/mt574084.aspx
[Postupci za baze podataka Azure SQL]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Novi AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Uklanjanje AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Vraćanje AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Životopis AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Odaberite AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Postavljanje AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Privremeno obustavljanje AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Instalacijski program platformu Microsoft Web]: https://aka.ms/webpi-azps
