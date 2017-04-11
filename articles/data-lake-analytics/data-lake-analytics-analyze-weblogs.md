<properties
   pageTitle="Analiza zapisnika web-mjesta pomoću Lake analize podataka za Azure | Azure"
   description="Saznajte kako analize pomoću analize podataka Lake zapisnika za web-mjesta. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Praktični vodič: Analiza zapisnika web-mjesta pomoću Lake analize podataka za Azure

Saznajte kako da biste analizirali zapisnika web-mjesta pomoću analize Lake podataka, posebice na doznati koje preporučitelji pokrenuli na pogreške kada ste na web-mjestu.

>[AZURE.NOTE] Ako samo želite vidjeti aplikacije rad, sprema se vrijeme prođite kroz [interaktivni vodiči za korištenje Azure podataka Lake analize](data-lake-analytics-use-interactive-tutorials.md). Pomoću ovog praktičnog vodiča temelji se na istom scenarij i istu šifru. Svrha ovog praktičnog vodiča je da bi se dobilo razvojni inženjeri sučelje za stvaranje i pokretanje analize podataka Lake aplikacije iz end da biste završetka.

## <a name="prerequisites"></a>Preduvjeti:

- **Visual Studio 2015, Visual Studio 2013 ažurirati 4, ili Visual Studio 2012 s Visual C++ instaliran**.
- **Microsoft Azure SDK za .NET verzije 2.5 ili noviji**.  Instalirajte ga pomoću [installer platformu za Web](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Lake podataka Tools za Visual Studio](http://aka.ms/adltoolsvs)**.

    Kad instalirate Data Lake Tools za Visual Studio, prikazat će se izbornik **Lake podataka** u Visual Studio:

    ![Izbornik U SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Osnovno znanje analize podataka Lake i Lake Alati podataka za Visual Studio**. Da bismo započeli, pogledajte:

    - [Početak rada s analize Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-get-started-portal.md).
    - [Razvoj U – SQL skripta pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

- **Analize podataka Lake račun.**  Potražite u članku [Stvaranje računa Azure podataka Lake analize](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Lake Alati podataka ne podržava stvaranje analize podataka Lake računi.  Tako ćete morati stvoriti pomoću portala za Azure, Azure PowerShell, .NET SDK ili EŽA Azure.
- **Prenesite oglednih podataka na račun analize Lake podataka.** Potražite u članku [Prijenos SearchLog.tsv zadani račun za pohranu Lake podataka](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Da biste pokrenuli analize podataka Lake posla, trebat će vam neke podatke. Iako Lake Alati podataka podržava prijenos podataka, na portalu će koristiti da biste prenijeli ogledne podatke da biste olakšali praćenje ovog praktičnog vodiča.

## <a name="connect-to-azure"></a>Povezivanje s Azure

Prije nego što možete izraditi i testirati sve U SQL skripte, najprije morate povezati Azure.

**Povezivanje s podacima Lake analize**

1. Otvorite Visual Studio.
2. Na izborniku **Lake podataka** , kliknite **Postavke i mogućnosti**.
4. Ako netko prijavio, kliknite **Sign In**ili **Promjena korisnika** , a zatim slijedite upute.
5. Kliknite **u redu** da biste zatvorili dijaloški okvir mogućnosti i postavki.

**Da biste pregledali analize podataka Lake računi**

1. Iz Visual Studio, otvorite **Eksplorer za poslužitelj** , pritisnite **CTRL + ALT + S**.
2. Iz programa **Explorer poslužitelja**proširite **Azure**, a zatim **Analitika Lake podataka**. Prikazat će popis računa analize podataka Lake ako ih ima. Ne možete stvoriti račune analize podataka Lake iz na studio. Da biste stvorili račun, pročitajte članak [Početak rada s analize Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-get-started-portal.md) ili [Početak rada s Azure podataka Lake analize pomoću komponente PowerShell Azure](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Razvoj aplikacija U SQL

U SQL aplikacija je uglavnom skriptu U SQL. Dodatne informacije o U SQL potražite u članku [Uvod U SQL](data-lake-analytics-u-sql-get-started.md).

Korisnički definirane operatore zbrajanja možete dodati aplikaciju.  Dodatne informacije potražite u članku [U razvoju-SQL korisnički definirane operatori za analize podataka Lake zadatke](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Stvaranje i slanje analize podataka Lake posla**

1. Na izborniku **datoteka** kliknite **Novo**, a zatim **projekta**.
2. Odaberite vrstu projekta U SQL.

    ![novi projekt U SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Kliknite **u redu**. Visual studio stvara rješenje pomoću Script.usql datoteke.
4. Unesite sljedeću skriptu u datoteku Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    Da biste shvatili U SQL, potražite u članku [Početak rada s jezikom podataka U u analize Lake-SQL](data-lake-analytics-u-sql-get-started.md).    

5. Dodavanje nove skripte U SQL u projekt, a zatim unesite sljedeće:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Prijeđite natrag u prvu U SQL skripte i uz gumb **Pošalji** , navedite račun analize.
7. U **Pregledniku rješenja**, desnom tipkom miša kliknite **Script.usql**, a zatim **Stvoriti skriptu**. Provjerite je li rezultate u oknu Izlaz.
8. U **Pregledniku rješenja**, desnom tipkom miša kliknite **Script.usql**, a zatim **Pošalji skriptu**.
9. Provjerite je li **Analize račun** onu koju želite pokrenuti posla, a zatim kliknite **Pošalji**. Slanje rezultata i veze za posao dostupni su u alatima za Visual Studio uzrokuje prozor za Lake podataka po dovršetku slanja.
10. Pričekajte dok se ne posao je uspješno dovršena.  Ako nije uspjela posla, najvjerojatnije nedostaje izvornu datoteku.  Potražite sekciji pripremni ovog praktičnog vodiča. Dodatne informacije o otklanjanju poteškoća, potražite u članku [monitora i otklanjanje poteškoća sa zadacima Lake analize podataka za Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Nakon dovršetka posla bit će se pojaviti zaslon za sljedeće:

    ![analize podataka lake analizirati zapisnika weblogovi web-mjesta](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Sada ponovite korake od 7 do 10 za **Script1.usql**.

>[AZURE.NOTE]Nije moguće čitati iz ili U SQL tablicu koja sadrži stvorene ili izmijenjene u istom skripte za pisanje.  Zašto je korištenje koristite dva skripte u ovom primjeru.

**Da biste vidjeli izlaz posla**

1. Iz programa **Explorer poslužitelja**proširite **Azure**, proširite **Analize podataka Lake**, proširite računa analize podataka Lake, proširite **Račune za pohranu**, desnom tipkom miša kliknite zadani račun za pohranu Lake podataka, a zatim **Explorer**.
2.  Dvokliknite **uzorke** da biste otvorili mapu, a zatim **izlaza**.
3.  Dvokliknite **UnsuccessfulResponsees.log**.
4.  Izlazna datoteka unutar prikaz na grafikonu posao možete dvokliknuti i da bi se izravno prijeći u izlaz.

## <a name="see-also"></a>Vidi također

Početak rada s podacima Lake analize pomoću raznih alata, potražite u članku:

- [Početak rada s podacima Lake analize pomoću portala za Azure](data-lake-analytics-get-started-portal.md)
- [Početak rada s podacima Lake analize pomoću komponente PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Početak rada s podacima Lake analize pomoću .NET SDK](data-lake-analytics-get-started-net-sdk.md)

Da biste vidjeli dodatne teme razvoj:

- [Razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md)
- [Razvoj U SQL korisnički definirane operatori za analize podataka Lake poslove](data-lake-analytics-u-sql-develop-user-defined-operators.md)
