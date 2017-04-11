<properties
    pageTitle="Konfigurirati pravila vatrozida razini poslužitelja baze podataka SQL Azure pomoću komponente PowerShell | Microsoft Azure"
    description="Saznajte kako konfigurirati Vatrozid za IP adrese Azure SQL baze podataka programa access."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Konfigurirati pravila vatrozida razini poslužitelja baze podataka SQL Azure pomoću komponente PowerShell


> [AZURE.SELECTOR]
- [Pregled](sql-database-firewall-configure.md)
- [Portal za Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-JA](sql-database-configure-firewall-settings-rest.md)


Baze podataka SQL Azure koristi pravila vatrozida da dopušta veze na poslužiteljima i baza podataka. Možete odrediti postavke poslužitelja baze podataka – razini i vatrozida za glavnom bazom podataka ili baze podataka za korisnika u vaš poslužitelj baze podataka SQL selektivno dopustili pristup bazi podataka.

> [AZURE.IMPORTANT] Da biste omogućili aplikacije iz Azure da biste se povezali s poslužiteljem baze podataka, mora biti omogućen Azure veze. Dodatne informacije o pravila vatrozida i omogućivanjem veze iz Azure potražite u članku [Vatrozid za baze podataka SQL Azure](sql-database-firewall-configure.md). Ako želite napraviti veze unutar granicu Azure oblaka, možda da biste otvorili neke dodatne TCP priključci. Dodatne informacije potražite u "V12 SQL baze podataka: izvan Dodavanje veze za vanjskih unutar" odjeljak [priključci izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Stvaranje pravila vatrozida poslužitelja

Vatrozid razini poslužitelja moguće je stvoriti pravila, ažurirati i izbrisati pomoću Azure PowerShell.

Da biste stvorili novo pravilo razini poslužitelja vatrozid, izvoditi na [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) cmdlet. U sljedećem primjeru omogućuje raspon IP adresa na poslužitelju Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Da biste izmijenili postojeće pravilo razini poslužitelja vatrozid, izvoditi na [skup-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx) cmdlet. Sljedeći primjer mijenja raspon prihvatljiva IP adresa za pravilo pod nazivom ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Da biste izbrisali postojeće pravilo razini poslužitelja vatrozid, izvoditi na [Ukloni-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx) cmdlet. U sljedećem primjeru briše pravilo pod nazivom ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Upravljanje pravilima vatrozid pomoću komponente PowerShell

Da biste upravljali pravilima vatrozid možete koristiti i PowerShell. Dodatne informacije potražite u sljedećim temama:

* [Novi AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Ukloni AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Skup AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Daljnji koraci

Informacije o načinu korištenja Transact-SQL za stvaranje pravila vatrozida za poslužitelj baze podataka – razini i potražite u članku [Konfiguriranje Azure SQL bazu podataka poslužitelja baze podataka – razini i vatrozida pravila pomoću T-SQL](sql-database-configure-firewall-settings-tsql.md).

Informacije o stvaranju pravila vatrozida razini poslužitelja pomoću druge načine, potražite u članku:

- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću portala za Azure](sql-database-configure-firewall-settings.md)
- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću REST API-JA](sql-database-configure-firewall-settings-rest.md)

Vodič za stvaranje baze podataka, potražite u članku [Stvaranje baze podataka SQL u minutama pomoću portala za Azure](sql-database-get-started.md).
Pomoć za povezivanje s bazom podataka Azure SQL s Otvori izvor ili aplikacije drugih proizvođača, potražite u članku [klijent brzi početak rada primjere koda s bazom podataka SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Da biste razumjeli kako se kretati s bazama podataka potražite u članku [Upravljanje sigurnošću baze podataka programa access i prijavite se](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Dodatni resursi

- [Zaštita baze podataka](sql-database-security.md)
- [Centar za sigurnost za modul za baze podataka SQL Server i baze podataka Azure SQL](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->
