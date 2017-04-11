<properties
    pageTitle="Azure pravila vatrozida razini poslužitelja SQL baze podataka pomoću REST API-JA | Microsoft Azure"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću REST API-JA


> [AZURE.SELECTOR]
- [Pregled](sql-database-firewall-configure.md)
- [Portal za Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-JA](sql-database-configure-firewall-settings-rest.md)


Baza podataka Microsoft Azure SQL koristi pravila vatrozida da dopušta veze na poslužiteljima i baza podataka. Možete odrediti postavke poslužitelja baze podataka – razini i vatrozida za matricu ili baze podataka za korisnika u poslužitelj baze podataka SQL Azure selektivno dopustili pristup bazi podataka.

> [AZURE.IMPORTANT] Da biste omogućili aplikacije iz Azure da biste se povezali s poslužiteljem baze podataka, mora biti omogućen Azure veze. Dodatne informacije o pravila vatrozida i omogućivanjem veze iz Azure potražite u članku [Vatrozid za baze podataka SQL Azure](sql-database-firewall-configure.md). Ako želite napraviti veze unutar granicu Azure oblaka, možda da biste otvorili neke dodatne TCP priključci. Dodatne informacije potražite u **V12 podataka za SQL: izvan Dodavanje veze za vanjskih unutar** odjeljak [priključci izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Upravljanje pravilima vatrozid razini poslužitelja putem REST API-JA
1. Upravljanje pravilima vatrozid kroz REST API-JA mora proći. Informacije potražite u članku [Vodič za programere za autorizaciju s resursima API Azure](../resource-manager-api-authentication.md).
2. Pravila razini poslužitelja se može stvoriti ažuriranim ili izbrisane pomoću REST API-JA

    Za stvaranje ili ažuriranje pravila vatrozida razini poslužitelja, izvršavanje metodu STAVI pomoću sljedeće:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Zahtjev za tijelo

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Da biste uklonili postojeće pravilo razini poslužitelja vatrozid, izvršavanje metode brisanja pomoću sljedeće:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Upravljanje pravilima vatrozid pomoću REST API-JA

* [Stvaranje ili ažuriranje pravila vatrozida](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Brisanje pravila vatrozida](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Početak pravila vatrozida](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Popis svih pravila vatrozida](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Daljnji koraci

Kako članak o korištenju Transact-SQL da biste stvorili pravilo poslužitelj baze podataka – razini i vatrozida potražite u članku [Konfiguriranje Azure SQL bazu podataka poslužitelja baze podataka – razini i vatrozid pravila pomoću T SQL](sql-database-configure-firewall-settings-tsql.md). 

Za kako na članke o stvaranju pravila vatrozida razini poslužitelja pomoću druge načine, potražite u članku: 

- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću portala za Azure](sql-database-configure-firewall-settings.md)
- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću komponente PowerShell](sql-database-configure-firewall-settings-powershell.md)

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

 
