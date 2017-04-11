<properties
    pageTitle="Baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL | Microsoft Azure"
    description="Saznajte kako postaviti logičke poslužitelj baze podataka SQL, pravila vatrozida poslužitelj baze podataka SQL i ogledne podatke. Osim toga, upute za povezivanje s klijentskim alatima, konfiguriranje korisnika i postavljanje pravila vatrozida baze podataka."
    keywords="Vodič za SQL baze podataka, stvoriti bazu podataka za sql"
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
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>Baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL u minutama pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Pomoću ovog praktičnog vodiča saznat ćete kako koristiti Azure portala:

- Stvaranje baze podataka Azure SQL s oglednim podacima.
- Stvaranje pravila vatrozida razini poslužitelja za jednu IP adresa ili raspon IP adresa.

Ti isti zadaci možete izvršiti pomoću [C#](sql-database-get-started-csharp.md) ili [PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Stvaranje prve baze podataka Azure SQL 

1. Ako ste trenutno povezani, povezivanje s [portala za Azure](http://portal.azure.com).
2. Kliknite **Novo**, kliknite **podataka + prostor za pohranu**, a zatim pronađite **SQL baze podataka**.

    ![Nove baze podataka sql 1](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Kliknite **SQL baze podataka** da biste otvorili plohu SQL baze podataka. Sadržaj na ovom plohu razlikuje se ovisno o broju pretplate i svoje postojeće objekte (primjerice postojeći poslužitelji).

    ![Nove baze podataka sql 2](./media/sql-database-get-started/sql-database-new-database-2.png)

4. U tekstni okvir **Naziv baze podataka** Navedite naziv prve baze podataka – kao što su "Moje bazu podataka". Zelena kvačica upućuje na to koju ste naveli valjani naziv.

    ![Nove baze podataka sql 3](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Ako imate više pretplata, odaberite pretplatu.
6. **Grupa resursa**, kliknite **Stvori novi** i navedite naziv za prvu grupu resursa – kao što su "Moje resursa – grupni". Zelena kvačica upućuje na to koju ste naveli valjani naziv.

    ![Nove baze podataka sql 4](./media/sql-database-get-started/sql-database-new-database-4.png)

7. U odjeljku **Odaberite izvor**, kliknite **Uzorak** pa u odjeljku **odaberite uzorak** **AdventureWorksLT [V12]**.

    ![Nove baze podataka sql 5](./media/sql-database-get-started/sql-database-new-database-5.png)

8. U odjeljku **poslužitelj**, kliknite **Konfiguriraj potrebne postavke**.

    ![Nove baze podataka sql 6](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Na Elektronička ploča poslužitelja kliknite **Stvori novi poslužitelj**. Baze podataka Azure SQL stvara se unutar objekta poslužitelj, što može biti poslužiteljem novi ili postojeći poslužitelj.

    ![Nove baze podataka sql 7](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Pregledajte Elektronička ploča **Novi poslužitelja** da biste shvatili potrebne informacije za novi poslužitelj.

    ![Nove baze podataka sql 8](./media/sql-database-get-started/sql-database-new-database-8.png)

11. U tekstni okvir **naziv poslužitelja** Navedite naziv poslužitelja prvi – kao što su "Moje – novi--objekt poslužitelja". Zelena kvačica upućuje na to koju ste naveli valjani naziv.

    ![Nove baze podataka sql 9](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. U odjeljku **Prijava administrator poslužitelja**navedite korisničko ime za prijavu administratora za ovaj poslužitelj – kao što su "Moj administrator-računa". U ovom prijava naziva glavni prijava server. Zelena kvačica upućuje na to koju ste naveli valjani naziv.

    ![Nove baze podataka sql 10](./media/sql-database-get-started/sql-database-new-database-10.png)

13. U odjeljku **Lozinka** i **Potvrdi lozinku**, unos lozinke za poslužitelj glavni Prijava računa – kao što su "p@ssw0rd1". Zelena kvačica upućuje na to koju ste naveli valjanu lozinku.

    ![Nove baze podataka sql 11](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. U odjeljku **mjesto**, odaberite odgovarajuće mjesto – kao što su "Australija Istok" podatkovnog centra.

    ![Nove baze podataka sql 12](./media/sql-database-get-started/sql-database-new-database-12.png)

15. U odjeljku ** Stvaranje V12 server (najnovije ažuriranje), obratite pozornost na to da imate samo mogućnost stvaranja trenutnu verziju Azure SQL server.

    ![Nove baze podataka sql 13](./media/sql-database-get-started/sql-database-new-database-13.png)

16. Obratite pozornost na to da po zadanom potvrdni okvir **Dopusti azure servisa za pristup poslužitelju** potvrđen i ne može se mijenjati. Ovo je dodatne mogućnosti. Možete promijeniti ovu postavku u vatrozid postavke poslužitelja za taj objekt poslužitelja, iako većina scenarijima za to nije potrebno.

    ![Nove baze podataka sql 14](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Na novu Elektronička ploča poslužitelja provjerite svoj odabir, a zatim **Odaberite** da biste odabrali ovo novi poslužitelj za novu bazu podataka.

    ![Nove baze podataka sql 15](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Na plohu SQL baze podataka, u odjeljku **sloju određivanje cijena**kliknite **S2 Standard** , a zatim kliknite **Osnovni** da biste odabrali najmanje pozivnim sloju cijene za prve baze podataka. Cijene sloju uvijek možete kasnije promijeniti.

    ![Nove baze podataka sql 16](./media/sql-database-get-started/sql-database-new-database-16.png)

19. Na plohu SQL baze podataka provjerite svoj odabir, a zatim kliknite **Stvori** da biste stvorili prvu poslužitelj i bazu podataka. Vrijednosti koje ste naveli su provjeriti i implementaciju pokrenuti.

    ![Nove baze podataka sql 17](./media/sql-database-get-started/sql-database-new-database-17.png)

20. Na alatnoj traci portala kliknite stavke **obavijesti** da biste provjerili status implementaciju sustava.

    ![Nove baze podataka sql 18](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Kada se dovrši implementaciju, novi Azure SQL poslužitelj i bazu podataka stvaraju se u Azure. Nećete moći povezati s novom poslužitelju ili baze podataka pomoću alata za SQL Server dok ne stvorite pravila vatrozida server da biste otvorili Vatrozid za SQL baze podataka za veze iz izvan Azure.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Daljnji koraci
Nakon što dovršite ovaj Praktični vodič SQL baze podataka i stvorili bazu podataka sadrži ogledne podatke, spremni ste za istraživanje pomoću alata za omiljene.

- Ako ste upoznati s Transact-SQL i SQL Server Management Studio (SSMS), Saznajte kako [Povezivanje i upita baze podataka SQL pomoću SSMS](sql-database-connect-query-ssms.md).

- Ako znate Excel, Saznajte kako [Povezivanje s bazom podataka sustava SQL Azure pomoću programa Excel](sql-database-connect-excel.md).

- Ako ste spremni ste za početak kodiranje, odaberite programski jezik na [biblioteke veza za SQL baze podataka i SQL Server](sql-database-libraries.md).

- Ako želite da biste premjestili lokalne baze podataka SQL Server Azure, potražite u članku [Migracija baze podataka SQL baze podataka](sql-database-cloud-migrate.md) da biste saznali više.

- Ako želite da biste učitali neke podatke u novu tablicu iz CSV datoteke pomoću alata naredbenog retka BCP, potražite u članku [učitavanja podataka u SQL baze podataka iz CSV datoteke pomoću BCP](sql-database-load-from-csv-with-bcp.md).

- Ako želite započeti Istraživanje sigurnost baze podataka SQL Azure, potražite u članku [Uvod u sigurnost](sql-database-get-started-security.md)


## <a name="additional-resources"></a>Dodatni resursi

[Što je SQL baze podataka?](sql-database-technical-overview.md)
