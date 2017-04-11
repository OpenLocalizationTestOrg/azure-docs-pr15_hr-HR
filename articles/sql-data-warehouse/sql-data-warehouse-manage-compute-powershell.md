<properties
   pageTitle="Upravljanje računalnim power u skladištu podataka SQL Azure (PowerShell) | Microsoft Azure"
   description="Zadaci ljuske PowerShell za upravljanje izračunati power. Promjena veličine izračunati resursi prilagodbom DWUs. Ili, zadržite pokazivač i nastaviti računalnim resursi troškova."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Upravljanje računalnim power u skladištu podataka SQL Azure (PowerShell)

> [AZURE.SELECTOR]
- [Pregled](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [OSTALE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaliranje Vremensko mjerilo uspješnosti izračunati resurse i memorije da bi odgovarao zahtjeva za promjenom od svoje radno opterećenje. Troškova skaliranja natrag resursima tijekom vremena koje nisu Vršna ili privremeno zaustavljanje računalnim potpuno. 

Zbirku zadataka koristi Azure portala:

- Promjena veličine računalnim
- Zaustavljanje računalnim
- Životopis računalnim

Dodatne informacije potražite u članku [Upravljanje izračunati pregled][].


## <a name="before-you-begin"></a>Prije početka

### <a name="install-the-latest-version-of-azure-powershell"></a>Instalirajte najnoviju verziju Azure PowerShell

> [AZURE.NOTE]  Da biste koristili Azure PowerShell sa SQL Data Warehouse, morate Azure PowerShell verzije 1.0.3 ili noviji.  Da biste provjerili trenutnu verziju pokrenite naredbu **modul za Get - ListAvailable-naziv Azure**. Najnoviju verziju možete instalirati iz [Microsoft Web platformu Installer][].  Dodatne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Početak rada s cmdleta ljuske PowerShell za Azure

Za početak rada:

1. Otvorite Azure PowerShell. 
2. Kada se zatraži PowerShell pokrenite te naredbe Prijava na upravitelj resursa Azure i odaberite svoju pretplatu.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Promjena veličine računalnim power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Da biste promijenili u DWUs, koristite cmdlet ljuske PowerShell za [Postavljanje AzureRmSqlDatabase][] . Sljedeći primjer postavlja cilj razini usluge DW1000 za bazu podataka MySQLDW koji se nalazi na poslužitelju MojPoslužitelj. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Zaustavljanje računalnim

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Pauziranje baze podataka, koristite cmdlet [Suspend AzureRmSqlDatabase][] . U sljedećem primjeru zaustavlja bazu pod nazivom Database02 nalazi na poslužitelju pod nazivom Server01. Poslužitelj je u grupi Azure resursa pod nazivom ResourceGroup1. 

> [AZURE.NOTE] Imajte na umu da je poslužitelj foo.database.windows.net, koristite "odnožje" kao naziv poslužitelja - u cmdleta ljuske PowerShell.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Varijacije, u ovom se primjeru sljedeći dohvaća podatke u bazu podataka u $database objekt. Zatim se cijevi objekt [Suspend AzureRmSqlDatabase][]. Rezultati se pohranjuju u resultDatabase objekt. Naredba konačan prikazuje rezultate.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Životopis računalnim

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Da biste započeli baze podataka, koristite cmdlet [Životopis AzureRmSqlDatabase][] . U sljedećem primjeru pokreće bazu pod nazivom Database02 nalazi na poslužitelju pod nazivom Server01. Poslužitelj je u grupi Azure resursa pod nazivom ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Varijacije, u ovom se primjeru sljedeći dohvaća podatke u bazu podataka u $database objekt. Zatim cijevi objekt [Životopis AzureRmSqlDatabase][] i sprema rezultate u $resultDatabase. Naredba konačan prikazuje rezultate.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Daljnji koraci

Druge zadatke upravljanja potražite u članku [Pregled upravljanja][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Pregled upravljanja]: ./sql-data-warehouse-overview-manage.md
[Kako instalirati i konfigurirati Azure PowerShell]: ./powershell-install-configure.md
[Upravljanje računalnim pregled]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Životopis AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Privremeno obustavljanje AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Postavljanje AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Instalacijski program platformu Microsoft Web]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
