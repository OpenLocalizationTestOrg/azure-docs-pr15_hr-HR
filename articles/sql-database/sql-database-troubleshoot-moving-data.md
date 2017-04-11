<properties
    pageTitle="Premještanje baze podataka između poslužitelja, s pretplate, a i Azure."
    description="Brze korake da biste kopirali, premjestite i Migracija podataka i bazama podataka u bazi podataka SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Prebacivanje baze podataka na poslužitelje, s pretplate, a i Azure

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Da biste premjestili baze podataka na drugi poslužitelj u okviru iste pretplate
- [Portal za Azure](https://portal.azure.com)kliknite **baze podataka SQL**, na popisu odaberite bazu podataka i zatim kliknite **Kopiraj**. Pročitajte članak [kopiranje baze podataka Azure SQL](sql-database-copy.md) za više detalja.

## <a name="to-move-a-database-between-subscriptions"></a>Da biste se kretali baze podataka pretplate
- [Portal za Azure](https://portal.azure.com)kliknite **SQL poslužitelja** , a zatim odaberite poslužitelja koji hostira baze podataka s popisa. Kliknite **Premjesti**, a zatim odaberite resurse za premještanje i pretplatu za premještanje.

## <a name="to-migrate-a-sql-database-into-azure"></a>Da biste Migracija baze podataka SQL Azure
- Određivanje kompatibilnosti baze podataka, a zatim odaberite način desnom migracije ovisno o vašim potrebama. Slijedite upute i mogućnosti u [Migracija baze podataka SQL Server](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Da biste stvorili kopiju baze podataka za korištenje izvan Azure
- [Izvoz datoteke BACPAC.](sql-database-export.md)
