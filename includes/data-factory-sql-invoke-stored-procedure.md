## <a name="invoking-stored-procedure-for-sql-sink"></a>Pozivanje pohranjene procedure za SQL sita

Kada kopirate podatke u SQL Server ili Azure SQL/SQL Server baza podataka, naveden korisnik pohranjene procedure nije konfiguriran i s dodatne parametre. 

Pohranjena procedura može biti leveraged kada ugrađeni Kopiraj Mehanizmi služe namjenu. To je obično leveraged prilikom vrlo obrade (spajanje stupca, a zatim traženje dodatnih vrijednosti, umetanja u većem broju tablica...) treba riješiti prije konačne umetanja izvorišnih podataka u odredišnoj tablici. 

Možda pozivanje pohranjena procedura po izboru. Sljedeći primjer pokazuje kako koristiti pohranjena procedura jednostavne unosa u tablicu u bazi podataka. 

**Izlazni skup podataka**

U ovom primjeru je vrsta: SqlServerTable. Postavite na AzureSqlTable će se koristiti za baze podataka Azure SQL. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Definirajte sekciji SqlSink u kopiju aktivnosti JSON na sljedeći način. Da biste nazvali pohranjena procedura prilikom umetanja podataka, potrebne su svojstva SqlWriterStoredProcedureName i SqlWriterTableType.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

U bazi podataka, definirajte pohranjena procedura s istim nazivom kao SqlWriterStoredProcedureName. Ulazne podatke iz navedeni izvor i umetanje obrađuje u izlaznoj tablici. Obratite pozornost na to da naziv parametra pohranjena procedura mora biti jednak tableName definirana u datoteci JSON tablice.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

U bazi podataka, definirajte tablice s istim nazivom kao SqlWriterTableType. Obratite pozornost na shemi vrste tablica mora biti isti kao shemi vratilo ulaznih podataka.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

Značajka pohranjena procedura vodi prednost [Table-Valued parametara](https://msdn.microsoft.com/library/bb675163.aspx).