<properties
   pageTitle="Upit Azure SQL Data Warehouse (Visual Studio) | Microsoft Azure"
   description="Upit SQL Data Warehouse s Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Upit Azure SQL Data Warehouse (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure strojnog učenja](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Pomoću programa Visual Studio upit Azure SQL Data Warehouse u samo nekoliko minuta. Ova metoda koristi datotečni nastavak Alati podataka SQL Server (SSDT) u Visual Studio. 

## <a name="prerequisites"></a>Preduvjeti

Da biste koristili ovog praktičnog vodiča, potrebno je:

+ Za postojeće skladištu podataka SQL. Da biste je stvorili, potražite u članku [Stvaranje SQL Data Warehouse][].
+ SSDT za Visual Studio. Ako imate Visual Studio, vjerojatno je već imate to. Upute za instalaciju i mogućnosti potražite u članku [Instaliranje Visual Studio i SSDT][].
+ Potpuno kvalificiran naziv SQL poslužitelja. Da biste pronašli to, potražite u članku [Povezivanje sa SQL Data Warehouse][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. povezati SQL Data Warehouse

1. Otvorite Visual Studio 2013 ili 2015.
2. Otvorite Eksplorer za SQL Server objekt. Da biste to učinili, odaberite **Prikaz** > **Explorer objekt za SQL Server**.

    ![SQL Server objekt Explorer][1]

3. Kliknite ikonu **Dodaj SQL Server** .

    ![Dodavanje SQL Server][2]

4. Ispunite polja u prozoru poslužitelj za povezivanje.

    ![Povezivanje s poslužiteljem][3]

    - **Naziv poslužitelja**. Unesite **naziv poslužitelja** na prethodno prepoznala.
    - **Provjera autentičnosti**. Odaberite **SQL Server provjeru autentičnosti** ili **servisa Active Directory integriranu provjeru autentičnosti**.
    - **Korisničko ime** i **lozinku**. Ako je provjera autentičnosti SQL poslužitelja odabrana iznad, unesite korisničko ime i lozinku.
    - Kliknite **Poveži**.

5. Da biste istražili, proširite poslužitelj za Azure SQL. Možete pogledati baza podataka povezanih s poslužiteljem. Proširite AdventureWorksDW da biste vidjeli tablice u oglednu bazu podataka.

    ![Istražite AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. izvođenje upita uzorka

Sad kad sadrži je uspostaviti vezu s bazom podataka, recimo pisanje upita.

1. Desnom tipkom miša kliknite bazu podataka u programu Explorer sustava SQL Server objekt.

2. Odaberite **Novi upit**. Otvorit će se novi prozor upit.

    ![Novi upit][5]

3. Kopirajte ovu TSQL upita u prozoru upita:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Pokrenite upit. Da biste to učinili, kliknite zelenu strelicu, ili upotrijebite sljedeće prečac: `CTRL` + `SHIFT` + `E`.

    ![Izvođenje upita][6]

5. Pogledajte rezultate upita. U ovom primjeru FactInternetSales tablica ima 60398 redaka.

    ![Rezultati upita][7]

## <a name="next-steps"></a>Daljnji koraci

Sad kad se možete povezati i upit, pokušajte [vizualizacija podataka pomoću PowerBI][].

Konfiguriranje okruženja za provjeru autentičnosti Azure Active Directory potražite u članku [provjere autentičnosti za SQL Data Warehouse][].

<!--Arcticles-->
[Povezivanje s SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Stvaranje SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Prilikom instalacije Visual Studio i SSDT]: sql-data-warehouse-install-visual-studio.md
[Provjeru autentičnosti SQL Data Warehouse]: sql-data-warehouse-authentication.md
[vizualizacija podataka pomoću PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
