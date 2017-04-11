<properties
    pageTitle="Upravljanje bazama podataka SQL Azure pomoću Azure Automatizacija | Microsoft Azure"
    description="Saznajte kako se servisa Azure Automatizacija može koristiti za upravljanje bazama podataka Azure SQL na razini."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Upravljanje bazama podataka SQL Azure pomoću automatizacije Azure

Ovaj vodič predstavljanje će vam na Azure Automation Services i kako ga možete koristiti da biste pojednostavnili upravljanje bazama podataka za Azure SQL.


## <a name="what-is-azure-automation"></a>Što je Automatizacija Azure?

[Automatizacija Azure](https://azure.microsoft.com/services/automation/) je Azure usluga za pojednostavljivanje oblaka upravljanje kroz Automatizacija procesa. Korištenje Azure Automatizacija, dugoročnih, ručno, podložni pogreške i često koji se ponavljaju zadaci mogu se automatizirati da biste povećali pouzdanosti, učinkovitost i vremena za vrijednost za tvrtku ili ustanovu.

Azure Automatizacija nudi modul izvođenja vrlo pouzdane i vrlo dostupnih tijeka rada koji se mijenja veličinu da biste kao rastom vašoj tvrtki ili ustanovi ne odgovara vašim potrebama. U automatizaciji Azure procesa može biti kicked ručno, sustavi 3 proizvođača ili u određenim intervalima tako da se zadaci dogoditi kada je potrebno.

Smanjite radu indirektni i oslobodili IT / DevOps osoblje obavljanje posla koji dodaje tvrtke vrijednost pomicanjem zadataka upravljanja oblaka će se automatski pokrenuti po Azure automatizaciju.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Kako Azure automatizaciju omogućuju upravljanje bazama podataka Azure SQL?

Azure SQL baze podataka može upravljati u Automatizacija Azure pomoću [cmdleta ljuske PowerShell za baze podataka SQL Azure](https://msdn.microsoft.com/library/dn546723.aspx) koji su dostupni u odjeljku [Alati za Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Automatizacija je tih cmdleta ljuske PowerShell za baze podataka SQL Azure dostupna izvan okvira za da bi se izvršava svih zadataka upravljanja SQL DB unutar servisa. Možete uparivanje i tih cmdleta u automatizaciji Azure pomoću cmdleta za druge servise za Azure, da biste automatizirali zadatke složene preko servisa Azure i 3 sustavi proizvođača.

Azure Automatizacija ima mogućnost komunikacije s poslužiteljima SQL izravno slanjem SQL naredbi pomoću komponente PowerShell.

[Automatizacija Azure runbook Galerija](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) sadrži raznih runbooks tim i zajednice proizvoda za početak automatske upravljanje baze podataka SQL Azure, drugih servisa Azure i 3 sustavi proizvođača. Galerija runbooks obuhvaćaju sljedeće:

 * [Pokretanje SQL upita u bazi podataka sustava SQL Server](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Okomito mjerilo (prema gore ili dolje) baze podataka SQL Azure prema rasporedu](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Izrežite tablice SQL Ako svoju bazu podataka izvršenja maksimalnu veličinu](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Indeksiranje tablica u bazi podataka SQL Azure ako iznimno fragmentirane](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove Automatizacija Azure i kako ga možete koristiti za upravljanje baze podataka Azure SQL, slijedite ove veze da biste saznali više o Azure automatizaciju.

- [Pregled Azure automatizacije](../automation/automation-intro.md)
- [Moje prvi runbook](../automation/automation-first-runbook-graphical.md)
- [Karta Azure Automatizacija učenje](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure Automatizacija: Svoje agente SQL u Oblaku](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 
