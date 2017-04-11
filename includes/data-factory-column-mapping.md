## <a name="column-mapping-with-translator-rules"></a>Mapiranje stupca s prevoditelj pravila
Mapiranje stupca se može koristiti da biste odredili kako stupaca naveden u "strukturi" karte tablice izvora u stupce naveden u "struktura" Primatelj tablice. Svojstvo **mapiranje stupca** dostupan je u odjeljku **typeProperties** aktivnosti Kopiraj.

Mapiranje stupca podržava sljedeće scenarije:

- Sve stupce u izvornoj tablici "struktura" mapiraju se na sve stupce u tablici primatelj "struktura".
- Podskup stupaca u izvornoj tablici "struktura" mapiraju se na sve stupce u tablici primatelj "struktura".

Sljedeće su stanja pogreške i rezultirat će iznimku:

- Manje stupce ili više stupaca u "strukturu" Primatelj tablice od navedenog u mapiranje.
- Dupliciranje mapiranje.
- Rezultat upita SQL imati naziv stupca koji je naveden u mapiranje.

## <a name="column-mapping-samples"></a>Uzorci mapiranje stupca
> [AZURE.NOTE] Primjere u nastavku su za Azure SQL i blobova platforme Azure, ali se primjenjuju na bilo kojem spremišta podataka koje podržava pravokutni skupova podataka. Morat ćete prilagoditi skup podataka i definicija povezane servisa u primjerima ispod tako da pokazuje na podatke u izvoru relevantne podatke. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Primjer 1 – stupaca mapiranje iz Azure SQL s blobova platforme Azure
U ovom primjeru struktura je unos tablice i pokazuje na SQL tablice u bazi podataka Azure SQL.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

U ovom primjeru struktura je tablice izlazne i pokazuje na blob u Azure blobova.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

JSON aktivnosti prikazano u nastavku. Stupaca iz izvora mapirani u stupce u primatelj (**columnMappings**) pomoću **prevoditelja** svojstva.

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Tijek mapiranja stupaca:**

![Tijek mapiranje stupca](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Primjer 2 – stupca mapiranje pomoću SQL upit iz Azure SQL blobova platforme Azure
U ovom primjeru SQL upit koristi se za izdvajanje podataka iz Azure SQL umjesto jednostavno koji određuje naziv tablice i nazivi stupaca u odjeljku "struktura". 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

U ovom slučaju rezultata upita najprije mapiraju se stupaca navedenih u "struktura" izvora. Nakon toga stupaca iz izvora "struktura" mapiraju se stupci primatelj "struktura" s pravilima naveden u columnMappings.  Pretpostavimo da upit vraća 5 stupaca, dva dodatnih stupaca, a zatim onih koji su navedeni u "struktura" izvora.

**Tijek mapiranje stupca**

![Mapiranje stupca tijek-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







