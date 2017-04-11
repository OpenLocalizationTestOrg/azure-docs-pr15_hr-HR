<properties
    pageTitle="Praktični vodič SQL baze podataka: Uvod u sigurnost"
    description="Saznajte kako stvoriti korisničke račune za pristup i upravljanje bazom podataka."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>Baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL korisničke račune za pristup i upravljanje bazom podataka


> [AZURE.SELECTOR]
- [Početak rada vodiča](sql-database-get-started-security.md)
- [Dopuštanje pristupa](sql-database-manage-logins.md)

U ovom ćete praktičnom vodiču saznati kako pomoću SQL Studio Server Management (SSMS) da biste:

- Prijavite se SQL baze podataka pomoću upravitelja prijave razini poslužitelja.
- Stvaranje baze podataka SQL korisnički račun.
- Dodjela baze podataka SQL korisničkih [dozvola db_owner](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0).
- Povezivanje s bazom podataka SQL korisničkih računa koji nije glavni razini poslužitelja.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Daljnji koraci
Sad kad ste dovršiti ovog praktičnog vodiča SQL baze podataka i stvorili korisnički račun i korisnik dodijelila dozvole vlasnika baze podataka na račun, spremni ste za dodatne informacije o [sigurnosti SQL baze podataka](sql-database-manage-logins.md).


