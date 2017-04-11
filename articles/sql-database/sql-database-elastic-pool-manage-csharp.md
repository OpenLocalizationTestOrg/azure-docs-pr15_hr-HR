<properties
    pageTitle="Nadzor i upravljanje elastic baze podataka grupe aplikacija uz C# | Microsoft Azure"
    description="Upravljanje grupe aplikacija programa za baze podataka SQL Azure elastic baze podataka pomoću C# baze podataka razvojnih tehnika."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Nadzor i upravljanje elastic baze podataka grupe aplikacija C & #x 23; 

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Saznajte kako upravljati na [skup elastic baze podataka](sql-database-elastic-pool.md) pomoću C i #x 23;. 

>[AZURE.NOTE] Mnoge nove značajke baze podataka SQL podržani su samo ako koristite [model implementacije Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md), tako da uvijek koristite najnoviji **biblioteke za upravljanje Azure SQL baze podataka za .NET ([Dokumenti](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet paketa](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Stariji [biblioteke utemeljene na klasični implementacije](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) podržani su za kompatibilnost s prijašnjim verzijama samo, stoga preporučujemo da koristite noviju biblioteke Voditelj resursa koji se temelje.

Da biste dovršili korake u ovom članku, potrebne su vam sljedeće stavke:

- Na elastic skup (grupu kojom želite upravljati). Da biste stvorili zajedničko područje, potražite u članku [Stvaranje baze podataka za elastic grupe aplikacija uz C#](sql-database-elastic-pool-create-csharp.md).
- Visual Studio. Besplatni kopiju Visual Studio, potražite u članku stranici [Visual Studio preuzimanja](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="move-a-database-into-an-elastic-pool"></a>Premještanje baze podataka u elastic grupe aplikacija

Samostalno baze podataka možete premjestiti i smanjivanje zajedničko područje.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Baza podataka popisa u elastic grupe aplikacija

Da biste dohvatili sve baze podataka u zajedničko područje, nazovite metodu [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) .

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Promjena postavki performansi zajedničko područje

Dohvaćanje postojeći skup svojstava. Izmijenite vrijednosti i izvršavanje metodu CreateOrUpdate.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Latencija elastic skup operacije

- Promjena eDTUs min po bazi podataka ili Maks eDTUs po bazi podataka obično dovršava pet minuta ili manje.
- Vrijeme da biste promijenili veličinu skup (eDTUs) ovisi o kombiniranu veličinu sve baze podataka u. Promjena Prosječna 90 minuta ili manje po 100 GB. Na primjer, ako je ukupni prostor sve baze podataka u 200 GB, zatim očekivani Latencija za mijenjanje eDTU skup po skup je 3 sata ili manje.




## <a name="additional-resources"></a>Dodatni resursi

- [Šifre pogreške SQL baze podataka SQL klijentske aplikacije: baze podataka pogreška veze i ostalih problema](sql-database-develop-error-messages.md).
- [SQL baze podataka](https://azure.microsoft.com/documentation/services/sql-database/)
- [API-ji za upravljanje Azure resursa](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Stvaranje nove grupe aplikacija elastic baze podataka pomoću C#](sql-database-elastic-pool-create-csharp.md)
- [Kada se skup elastic baze podataka želite koristiti?](sql-database-elastic-pool-guidance.md)
- U odjeljku [Skaliranje odgovor s bazom podataka SQL Azure](sql-database-elastic-scale-introduction.md): pomoću Alati baze podataka za elastic mjerilo Izlaz, premještanje podataka, upit ili stvorite transakcije.

