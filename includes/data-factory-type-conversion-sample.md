### <a name="type-conversion-sample"></a>Ogledna pretvorbe vrsta
Sljedeća Ogledna je za kopiranje podataka s Blob Azure SQL s pretvorbe vrsta.

Pretpostavimo da Blob skupu podataka u obliku CSV sadrži 3 stupca. Jedan od njih je stupac datuma i vremena u obliku prilagođenog datetime korištenje skraćeni francuski naziva dana u tjednu.

Će definirati skup podataka Blob izvora na sljedeći način uz definicije vrste stupaca.

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
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
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Dani u SQL vrste za .NET vrsta mapiranje gornje tablice koje želite definirati tablice Azure SQL s Sljedeća shema.

| Naziv stupca | SQL vrsta |
| ----------- | -------- |
| ID korisnika | bigint |
| ime | tekst |
| lastlogindate | Datum i vrijeme |

Sljedeća će se definiraju na skupu podataka Azure SQL na sljedeći način. Napomena: Nije potrebno da biste odredili "struktura" odjeljak s informacijama vrste Budući da je navedeno kao vrsta već naveden u podlozi spremišta podataka.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

U ovom slučaju tvorničke podataka automatski učinite vrstu pretvorbe uključujući polja datuma i vremena s datumom i vremenom prilagođene oblikovanje pomoću kulture fr fr kada premještanje podataka s blobova platforme Azure SQL.


