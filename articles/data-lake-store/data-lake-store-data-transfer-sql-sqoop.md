<properties 
   pageTitle="Kopiranje podataka između spremišta Lake podataka i baze podataka Azure SQL pomoću Sqoop | Microsoft Azure"
   description="Korištenje Sqoop da biste kopirali podatke između baze podataka SQL Azure i spremište Lake podataka" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopiranje podataka između spremišta Lake podataka i baze podataka Azure SQL pomoću Sqoop

Saznajte kako koristiti Apache Sqoop za uvoz i izvoz podataka između baze podataka SQL Azure i spremište Lake podataka.
 

## <a name="what-is-sqoop"></a>Što je Sqoop?

Velikih skupova podataka aplikacije su prirodnim izbor za obradu nestrukturirane i djelomično strukturirane podatke, kao što su Zapisnici i datoteke. Međutim, mogu postojati i potrebe za obradu strukturiranih podataka pohranjenih u relacijske baze podataka.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) je alat osmišljeno za prijenos podataka između relacijske baze podataka i spremište velikih skupova podataka, kao što je spremište Lake podataka. Možete je koristiti za uvoz podataka iz sustava za upravljanje relacijske baze podataka (RDBMS) kao što su baze podataka SQL Azure u spremištu Lake podataka. Možete, a zatim Pretvorba i analiziranje podataka pomoću radnih opterećenja velikih skupova podataka i zatim izvezite podatke natrag na RDBMS. U ovom ćete praktičnom vodiču pomoću baze podataka SQL Azure kao relacijske baze podataka uvoz/izvoz iz.
 

## <a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- **Omogućivanje pretplate Azure** spremišta podataka Lake javno pretpregled. Pročitajte [upute](data-lake-store-get-started-portal.md#signup). 
- **Klaster azure HDInsight** pomoću programa access s računom spremišta Lake podataka. Potražite u članku [Stvaranje programa HDInsight klaster s trgovinom Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md). U ovom se članku pretpostavlja da ste je klaster HDInsight Linux pomoću spremišta Lake podataka programa access.
- **Baze podataka azure SQL**. Upute o tome da biste je stvorili, potražite u članku [Stvaranje baze podataka Azure SQL](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Učite brzo s videozapisima?

[Pogledajte ovaj videozapis](https://mix.office.com/watch/1butcdjxmu114) o tome da biste kopirali podatke između blob polja Azure prostora za pohranu i spremište Lake podataka pomoću DistCp.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Stvaranje primjera tablica u bazi podataka SQL Azure

1. Za početak s stvorite dva primjera tablica u bazi podataka SQL Azure. Povezivanje s bazom podataka SQL Azure, a zatim pokrenite sljedeći upita pomoću [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) ili Visual Studio.

    **Stvaranje Tablica1**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Stvaranje tablica2**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. U **tablica1**, dodajte ogledne podatke. **Tablica2** ostavite praznim. Ne možemo će uvoz podataka iz **parametra Table1** u spremište Lake podataka. Zatim ćemo će izvoz podataka iz spremišta Lake podataka u **tablica2**. Pokrenite sljedeće isječka.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Korištenje Sqoop iz programa HDInsight klaster s pristupom spremište Lake podataka

Za HDInsight klaster već ima dostupne pakete Sqoop. Ako ste konfigurirali klaster HDInsight da koristi spremište Lake podataka kao dodatan prostor za pohranu, možete koristiti Sqoop (bez promjene konfiguracije) za uvoz/izvoz podataka između relacijske baze podataka (u ovom primjeru baze podataka SQL Azure) i spremišta podataka Lake račun. 

1. Za ovaj vodič pretpostavimo da ste stvorili Linux klaster da biste trebali biste koristiti SSH za povezivanje s klaster. Pogledajte članak [Povezivanje sa sustavom Linux HDInsight klaster](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Provjerite ima li pristup računa spremišta Lake podataka iz skupine. Iz upita SSH, pokrenite sljedeću naredbu:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    To mora sadržavati popis datoteka/mapa u spremištu Lake podataka računa.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Uvoz podataka iz baze podataka SQL Azure u spremištu Lake podataka

3. Dođite do direktorija koje su dostupne pakete Sqoop. Obično, dogodit će se na `/usr/hdp/<version>/sqoop/bin`. 

4. Uvoz podataka iz **tablica1** spremišta podataka Lake račun. Upotrijebite sljedeću sintaksu:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Imajte na umu taj **sql--naziv poslužitelja baze podataka –** rezervirano mjesto predstavlja naziv poslužitelja na kojem se pokreće baze podataka Azure SQL. rezervirano mjesto za **SQL baza podataka naziv** predstavlja stvarni baze podataka.

    Na primjer,

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Provjerite je li prenijeti da podatke računa spremišta Lake podataka. Pokrenite sljedeću naredbu:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Trebali biste vidjeti sljedeće izlaz.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Svaki **dio – m -** * datoteke odgovara redaka u izvornoj tablici * *tablica1**. Možete pregledati sadržaj dijela-m -* datoteke da biste potvrdili.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Izvoz podataka iz spremišta Lake podataka u bazu podataka SQL Azure

6. Izvoz podataka iz spremišta podataka Lake računa u praznu tablicu, **tablica2**, u bazi podataka SQL Azure. Pomoću sljedeće sintakse.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Na primjer,

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Provjerite je li tablica baze podataka SQL je prenijeti podatke. Pomoću [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) ili Visual Studio povezivanje s bazom podataka SQL Azure, a zatim pokrenite sljedeći upit.

        
        SELECT * FROM TABLE2

    To mora sadržavati sljedeće izlaz.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Vidi također

- [Kopirajte podatke iz blob polja za pohranu Azure u spremištu Lake podataka](data-lake-store-copy-data-azure-storage-blob.md)
- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
