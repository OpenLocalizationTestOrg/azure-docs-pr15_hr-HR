<properties
    pageTitle="Pregled: Upravljanje Alati za baze podataka SQL | Microsoft Azure"
    description="Uspoređuje alata i mogućnosti za upravljanje bazom podataka SQL Azure"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Pregled: Upravljanje Alati za baze podataka SQL

U ovoj se temi istražuje i uspoređuje alata i mogućnosti za upravljanje baze podataka Azure SQL pa možete odabrati desnom alat za posao, vaša tvrtka, a. Odabir desnog alat ovisi o koliko baze podataka upravljate, zadatka i koliko često se izvodi zadatak.

## <a name="azure-portal"></a>Portal za Azure

[Portal za Azure](https://portal.azure.com) je koji se temelji na web aplikacija gdje možete stvoriti, ažurirati, i brisanje baze podataka i logički poslužitelji i praćenje aktivnosti baze podataka. Ako ste tek ste počeli raditi s Azure, upravljanje nekoliko baze podataka ili morate učiniti nešto brzo sjajan je ovaj alat.

Dodatne informacije o korištenju portala potražite u članku [Upravljanje bazama podataka SQL pomoću portala za Azure](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio i SQL Server Data Tools u Visual Studio

SQL Server Management Studio (SSMS) i Alati podataka SQL Server (SSDT) su klijentskim alatima koji se izvode na vašem računalu za upravljanje i razvoj bazu podataka u oblaku. Ako ste je razvojni inženjer upoznati s Visual Studio ili druge integriranih razvojnih okruženja (IDEs), [isprobajte SSDT u Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). SSMS koje se mogu koristiti s bazama podataka Azure SQL poznajete mnogo administratore baze podataka. [Preuzmite najnoviju verziju SSMS](https://msdn.microsoft.com/library/mt238290) i uvijek koristiti i najnovije izdanje prilikom rada s bazom podataka SQL Azure. Dodatne informacije o upravljanju baze podataka SQL Azure s SSMS potražite u članku [Upravljanje bazama podataka SQL pomoću SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Uvijek koristite najnoviju verziju sustava [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) i [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) za ostati sinkronizirani s ažuriranja za Microsoft Azure i baze podataka SQL.


## <a name="powershell"></a>PowerShell

Koristite PowerShell za upravljanje bazama podataka i grupe elastic baze podataka, a da biste automatizirali implementacijama Azure resursa. Microsoft preporučuje ovaj alat za upravljanje velikim brojem baze podataka i automatske implementacije i promjene resursa u radnom okruženju.

Dodatne informacije potražite u članku [Upravljanje SQL baze podataka pomoću komponente PowerShell](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Elastic Alati baze podataka
Korištenje alata za elastic baze podataka za izvođenje akcija kao što su 

* Izvršavanja T-SQL skripte uspoređuje sa skupom baze podataka pomoću [elastic posla](sql-database-elastic-jobs-overview.md)
* Premještanje baze podataka više klijentu modela model jednog klijenta pomoću [alata za podjelu cirkularnog pisma](sql-database-elastic-scale-overview-split-and-merge.md)
* Upravljanje bazama podataka u model klijentu za jednu ili više klijentu modela pomoću [elastic mjerilo klijentska biblioteka](sql-database-elastic-database-client-library.md).
 

## <a name="additional-resources"></a>Dodatni resursi

- [Azure Voditelj resursa](https://azure.microsoft.com/features/resource-manager/)
- [Automatizacija Azure](https://azure.microsoft.com/documentation/services/automation/)
- [Azure rasporeda](https://azure.microsoft.com/documentation/services/scheduler/)