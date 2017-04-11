<properties
   pageTitle="Rješavanje problema za kompatibilnost baze podataka sustava SQL Server prije migracije s bazom podataka SQL | Microsoft Azure"
   description="Web-mjesto Microsoft Azure SQL baze podataka, Migracija baze podataka, kompatibilnost, u okvir za SQL Azure Čarobnjak za migraciju, SSDT"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Migracija baze podataka SQL Server s bazom podataka Azure SQL pomoću SQL Server Data Tools za Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Savjetnik za nadogradnju](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

U ovom se članku Naučite prepoznavanje i rješavanje problema kompatibilnosti baze podataka za SQL Server pomoću SQL Server Data Tools za Visual Studio prije migracije s bazom podataka SQL Azure.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Pomoću SQL Server Data Tools za Visual Studio

Da biste uvezli shemu baze podataka u bazu podataka projekta za Visual Studio za analizu pomoću SQL Server Data Tools za Visual Studio ("SSDT"). Da biste analizirali, određivanje ciljne platforme za projekt kao V12 baze podataka SQL i stvaranje projekta. Ako je sastavljanje uspije, kompatibilan je baza podataka. Sastavljanje ne uspije, mogli biste riješiti pogreške u SSDT (ili neke druge alate spominju u ovoj temi). Kada uspješno sastavlja projekt, objavite ga ponovno kao kopiju izvorišne baze podataka. Značajka usporedbe podataka možete koristiti u SSDT za kopiranje podataka iz izvorišne baze podataka u bazu podataka za kompatibilne Azure SQL V12. Zatim možete migrirati ažurirane bazu podataka. Da biste koristili tu mogućnost, preuzmite [najnoviju verziju SSDT](https://msdn.microsoft.com/library/mt204009.aspx).

  ![Dijagram VSSSDT migracije](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Ako je potreban je samo za sheme migracije, shema moguće objavljivati izravno iz Visual Studio izravno s bazom podataka SQL Azure. Ovaj postupak koristite kada shemu baze podataka potreban je dodatne promjene ne može riješiti samo Čarobnjak za migraciju.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Otkrivanje problema s kompatibilnošću pomoću SQL Server Data Tools za Visual Studio
   
1.  Otvorite **objekt programa Explorer za SQL Server** u Visual Studio. Korištenje **Dodavanje SQL Server** za povezivanje s instancu sustava SQL Server koja sadrži migrira bazu podataka. Pronađite bazu podataka u programu Explorer objekta, desnom tipkom miša kliknite bazu podataka i odaberite **Stvori novi projekt...**     
    
    ![Novi projekt](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Konfiguriranje postavki uvoza da biste **uvezli aplikacije opsegu samo objekti**. Poništite potvrdni okvir mogućnosti uvoza na sljedeći način: poziva prijave, dozvole i postavki baze podataka.    

    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Kliknite **Start** da biste uvezli u bazu podataka i stvaranje projekta koji sadrži datoteku T SQL skripte za sve objekte u bazi podataka. Datoteka skripte su ugniježđene u mapama u projektu.    

    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  U programu Visual Studio rješenje Explorer desnom tipkom miša kliknite projekt baze podataka, a zatim odaberite Svojstva. Na stranici s **Postavkama projekta** konfiguriranje ciljne platforme za Microsoft Azure SQL baza podataka V12.    
    
    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Desnom tipkom miša kliknite projekt, a zatim odaberite **Sastavljanje** za sastavljanje projekta.    
    
    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  **Popis pogrešaka** prikazuje svaki nekompatibilnost programa. U ovom slučaju korisničko ime NT AUTHORITY\NETWORK SERVICE nije kompatibilna. Budući da je nekompatibilan, možete ga komentara ili ga ukloniti (i adresa utjecaju na ovom prijave i uloge uklanjanjem rješenja baze podataka).     
    
    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Rješavanje problema s kompatibilnošću pomoću SQL Server Data Tools za Visual Studio

1.  Dvokliknite prvi skriptu da biste otvorili skriptu u prozor upit i komentar izvan skriptu, a izvršavanje skriptu.     
    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Ponovite taj postupak za svaki skriptu koja sadrži nekompatibilnosti dok ostati bez pogreške.    
    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Kada se baza podataka nalazi besplatne pogrešaka, desnom tipkom miša kliknite projekt i odaberite **Objavi**. Kopiju izvorišne baze podataka u komponenti i objaviti (preporučljivo je da biste koristili kopiju, najmanje prethodno).     
 - Prije objavljivanja, ovisno o tome na izvor SQL Server (verziju stariju od verzije 2014 SQL Server), morate ponovno postaviti projekta ciljne platforme da biste omogućili implementacije.     
 - Ako premještate starije baze podataka SQL Server, ne predstavljanje sve značajke u projekt koje nisu podržane u izvoru SQL Server do migrirati bazu podataka na noviju verziju sustava SQL Server.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  U sustavu SQL Server objekt Exploreru desnom tipkom miša kliknite izvorišne baze podataka, a zatim kliknite **Usporedbe podataka**. Usporedba projekta s izvornom bazom podataka olakšava razumijevanje koje su promjene pomoću čarobnjaka. Odaberite svoju verziju Azure SQL V12 baze podataka, a zatim kliknite **Završi**.    
    
    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Pregledajte razlike otkriven, a zatim kliknite **Ažuriraj cilj** za migraciju podataka iz izvorišne baze podataka u bazu podataka Azure SQL V12.     
    
    ![Zamjenski tekst](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Odaberite način implementacije. U odjeljku [Migracija kompatibilne baze podataka SQL Server s bazom podataka SQL.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
