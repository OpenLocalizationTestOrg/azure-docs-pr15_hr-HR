<properties
    pageTitle="Azure baze podataka SQL poslužitelja baze podataka – razini i vatrozid pravila pomoću T SQL | Microsoft Azure"
    description="Saznajte kako konfigurirati Vatrozid za IP adrese Azure SQL baze podataka programa access."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Konfiguriranje pravila poslužitelja baze podataka – razini i vatrozida za baze podataka SQL Azure pomoću T SQL


> [AZURE.SELECTOR]
- [Pregled](sql-database-firewall-configure.md)
- [Portal za Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-JA](sql-database-configure-firewall-settings-rest.md)


Baza podataka Microsoft Azure SQL koristi pravila vatrozida da dopušta veze na poslužiteljima i baza podataka. Možete odrediti postavke poslužitelja baze podataka – razini i vatrozida za matricu ili baze podataka za korisnika u poslužitelj baze podataka SQL Azure selektivno dopustili pristup bazi podataka.

> [AZURE.IMPORTANT] Da biste omogućili aplikacije iz Azure da biste se povezali s poslužiteljem baze podataka, mora biti omogućen Azure veze. Dodatne informacije o pravila vatrozida i omogućivanjem veze iz Azure potražite u članku [Vatrozid za baze podataka SQL Azure](sql-database-firewall-configure.md). Ako želite napraviti veze unutar granicu Azure oblaka, možda da biste otvorili neke dodatne TCP priključci. Dodatne informacije potražite u **V12 podataka za SQL: izvan Dodavanje veze za vanjskih unutar** odjeljak [priključci izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="server-level-firewall-rules"></a>Pravila vatrozida razini poslužitelja

Samo glavni prijava razini poslužitelja ili administrator Azure Active Directory možete stvoriti pravila vatrozida razini poslužitelja pomoću Transact-SQL.

1. Pokrenite prozor upit i povezati virtualne glavnom bazom podataka pomoću SQL Server Management Studio.
2. Pravila vatrozida razini poslužitelja mogu odabrana, stvoriti, ažurirati, ili izbrisati iz unutar prozora upita.
3. Za stvaranje ili ažuriranje pravila vatrozida razini poslužitelja, izvoditi na `sp_set_firewall_rule` pohranjena procedura. U sljedećem primjeru omogućuje raspon IP adresa na poslužitelju Contoso.<br/>Početak tako da prikazuje pravila koja već postoji.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Zatim dodajte pravilo vatrozida.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Da biste izbrisali pravilo razini poslužitelja Vatrozid za izvršavanje sp_delete_firewall_rule pohranjene procedure. U sljedećem primjeru briše pravilo pod nazivom ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Dodatne informacije o ove pohranjene procedure potražite u članku [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) i [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Pravila vatrozida razinom baze podataka

Samo korisnici baze podataka s dozvolom za **KONTROLU** na bazu podataka (kao što je vlasnik baze podataka) možete stvoriti pravila vatrozida razinom baze podataka.

1. Nakon stvaranja vatrozid razini poslužitelja za IP adrese, pokrenite upit prozor putem portala Classic ili SQL Server Management Studio.
2. Povezivanje s bazom podataka za koji želite da biste stvorili pravilo vatrozid razinom baze podataka.

    Da biste stvorili novi ili ažurirati postojeće pravilo vatrozid razinom baze podataka, izvoditi na `sp_set_database_firewall_rule` pohranjena procedura. Sljedeći primjer stvara novo pravilo vatrozid pod nazivom ContosoFirewallRule.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Da biste izbrisali postojeće pravilo vatrozid razinom baze podataka, izvoditi na `sp_delete_database_firewall_rule` pohranjena procedura. U sljedećem primjeru briše pravilo pod nazivom ContosoFirewallRule.
`
   Sp_delete_database_firewall_rule izvršavanja PRVE @name = N'ContosoFirewallRule "

Dodatne informacije o ove pohranjene procedure potražite u članku [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) i [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Daljnji koraci

Za kako na članke o stvaranju pravila vatrozida razini poslužitelja pomoću druge načine, potražite u članku: 

- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću portala za Azure](sql-database-configure-firewall-settings.md)
- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću komponente PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću REST API-JA](sql-database-configure-firewall-settings-rest.md)

Vodič za stvaranje baze podataka, potražite u članku [Stvaranje baze podataka SQL u minutama pomoću portala za Azure](sql-database-get-started.md).
Pomoć za povezivanje s bazom podataka Azure SQL s Otvori izvor ili aplikacije drugih proizvođača, potražite u članku [klijent brzi početak rada primjere koda s bazom podataka SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Da biste razumjeli kako se kretati s bazama podataka potražite u članku [Upravljanje sigurnošću baze podataka programa access i prijavite se](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Dodatni resursi

- [Zaštita baze podataka](sql-database-security.md)
- [Centar za sigurnost za modul za baze podataka SQL Server i baze podataka Azure SQL](https://msdn.microsoft.com/library/bb510589)
