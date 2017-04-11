<properties
    pageTitle="Konfiguriranje pravilo vatrozid razini poslužitelja baze podataka SQL | Microsoft Azure"
    description="Saznajte kako konfigurirati Vatrozid za IP adrese pristup Azure SQL server."
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
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Konfiguriranje pravilo vatrozid razini poslužitelja baze podataka SQL Azure pomoću portala za Azure


> [AZURE.SELECTOR]
- [Pregled](sql-database-firewall-configure.md)
- [Portal za Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-JA](sql-database-configure-firewall-settings-rest.md)

Azure SQL server koristi pravila vatrozida da dopušta veze na poslužiteljima i baza podataka. Možete odrediti postavke poslužitelja baze podataka – razini i vatrozida za matricu ili baze podataka za korisnika u Azure SQL server logičke poslužitelj za selektivno dopustili pristup bazi podataka. U ovoj se temi opisuje pravila vatrozida razini poslužitelja.

> [AZURE.IMPORTANT] Da biste omogućili aplikacije iz Azure da biste se povezali s poslužiteljem Azure SQL, mora biti omogućen Azure veze. Da biste shvatili kako pravila vatrozida rada, pogledajte [upute za konfiguriranje programa Azure SQL server vatrozid \- pregled](sql-database-firewall-configure.md). Ako želite napraviti veze unutar granicu Azure oblaka, možda da biste otvorili neke dodatne TCP priključci. Dodatne informacije potražite u **V12 podataka za SQL: izvan Dodavanje veze za vanjskih unutar** odjeljak [priključci izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL](sql-database-develop-direct-route-ports-adonet-v12.md)

**Preporuka:** Korištenje pravila vatrozida razini poslužitelja za administratore i kada imate mnogo baze podataka koje imaju isti zahtjeve za pristup i ne želite trošiti vrijeme pojedinačno konfiguriranje svaku bazu podataka. Microsoft preporučuje korištenje pravila vatrozida baze podataka razinom kad god je to moguće, za poboljšanje sigurnosti i pripreme bazu podataka.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Upravljanje postojeće pravila vatrozida razini poslužitelja putem portala za Azure

Ponovite korake da biste upravljali pravilima vatrozid razini poslužitelja.

- Da biste dodali trenutno računalo, kliknite Dodaj klijent IP.
- Da biste dodali dodatne IP adrese, upišite naziv pravila, pokrenite IP adresa i kraj IP adresa.
- Da biste izmijenili postojeće pravilo, kliknite bilo koje polje u pravilu i izmijenite.
- Da biste izbrisali postojeće pravilo, postavite pokazivač miša pravilo dok se ne pojavi X pri kraju retka. Kliknite X da biste uklonili pravilo.

Kliknite **Spremi** da biste spremili promjene.

## <a name="next-steps"></a>Daljnji koraci

Kako članak o korištenju Transact-SQL da biste stvorili pravilo poslužitelj baze podataka – razini i vatrozida potražite u članku [Konfiguriranje Azure SQL bazu podataka poslužitelja baze podataka – razini i vatrozid pravila pomoću T SQL](sql-database-configure-firewall-settings-tsql.md). 

Za kako na članke o stvaranju pravila vatrozida razini poslužitelja pomoću druge načine, potražite u članku: 

- [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću komponente PowerShell](sql-database-configure-firewall-settings-powershell.md)
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

 
