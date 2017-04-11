<properties
   pageTitle="Upravljanje računalnim power u Azure SQL podataka skladištu (REST) | Microsoft Azure"
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Upravljanje računalnim power u Azure SQL podataka skladištu (REST)

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

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Promjena veličine računalnim power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Da biste promijenili u DWUs, koristite [Stvaranje ili ažuriranje baze podataka][] REST API-JA. Sljedeći primjer postavlja cilj razini usluge DW1000 za bazu podataka MySQLDW koji se nalazi na poslužitelju MojPoslužitelj. Poslužitelj je u grupi Azure resursa pod nazivom ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Zaustavljanje računalnim

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Pauziranje baze podataka, koristite [Zadržite pokazivač baze podataka][] REST API-JA. U sljedećem primjeru zaustavlja bazu pod nazivom Database02 nalazi na poslužitelju pod nazivom Server01. Poslužitelj je u grupi Azure resursa pod nazivom ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Životopis računalnim

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Da biste započeli baze podataka, koristite [Bazu podataka za životopis][] REST API-JA. U sljedećem primjeru pokreće bazu pod nazivom Database02 nalazi na poslužitelju pod nazivom Server01. Poslužitelj je u grupi Azure resursa pod nazivom ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Daljnji koraci

Druge zadatke upravljanja potražite u članku [Pregled upravljanja][].

<!--Image references-->

<!--Article references-->
[Pregled upravljanja]: ./sql-data-warehouse-overview-manage.md
[Upravljanje računalnim pregled]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Zaustavljanje baze podataka]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Životopis baze podataka]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Stvaranje ili ažuriranje baze podataka]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
