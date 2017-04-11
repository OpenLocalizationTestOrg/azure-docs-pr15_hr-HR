<properties
    pageTitle="Premještanje podataka iz sustava SQL Server na lokaciji SQL Azure s Azure podataka tvorničke | Azure"
    description="Postavljanje programa ADF kanal koji sastavlja svom dvije aktivnosti migraciju podataka koji zajedno premještanje podataka na dnevno s bazama podataka na lokaciji i u oblaku."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Premještanje podataka iz sustava SQL server na lokaciji SQL Azure s tvorničke Azure podataka

U ovoj se temi objašnjava premještanje podataka iz baze podataka na lokaciji SQL Server s bazom podataka SQL Azure putem spremište blobova platforme Azure pomoću Azure podataka tvorničke (ADF).

Sljedeće veze **izbornika** na temu koja opisuje kako ingest podatke u ciljnu okruženja mjesto spremanja i obrađuju tijekom postupka timu podataka znanstvenog podatke.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Uvod: Što je ADF i kada moraju se koristi za migraciju podataka?

Azure tvorničke podataka je potpuno upravljanih podataka u oblaku Integracija servis orchestrates i automatizira premještanja i transformacije podataka. Ključni pojam u modelu ADF je kanal. Na kanal je logički grupiranja aktivnosti, od kojih svaka definira akcije izvršiti na podataka koji se nalaze u skupove podataka. Povezani servisi koriste se za definiranje podatke koji su potrebni za tvorničke podataka da biste se povezali na izvor podataka.

S ADF, postojeće servise za obradu podataka možete sastavljeni u kanali podatke koji su izrazito dostupan je i upravljani u oblaku. Kanali tih podataka može se zakazati ingest, Priprema, pretvorba, analizirati i objavljivanje podataka, a ADF upravlja i orchestrates složenih podataka i obrada ovisnosti. Rješenja mogu brzo ugrađen i implementiran u oblaku, povezivanje sve veći broj lokalnih podataka i izvora podataka u oblaku.

Razmislite o korištenju ADF:

- Kada je podatke potrebno neprestano migrirati u scenarija hibridnog pristupa resursima na lokaciji i oblaka 
- Kada podatke je transakcije ili je potrebno izmijeniti ili imaju poslovne logike u njega dodali kada migrira. 

ADF omogućuje za planiranje i praćenje zadataka pomoću jednostavne JSON skripte koji upravljaju premještanje podataka na temelju periodički. ADF ima i druge mogućnosti kao što je podrška za složene operacije. Dodatne informacije o ADF potražite u dokumentaciji na [Tvorničke Azure podataka (ADF)](https://azure.microsoft.com/services/data-factory/).


## <a name="scenario"></a>Scenarij

Ne možemo postaviti programa ADF kanal koji sastavlja svom dvije aktivnosti za migraciju podataka. Zajednički ih prebacivanje podataka po danu na lokaciji SQL baze podataka i baze podataka SQL Azure u oblaku. Dvije aktivnosti su:

* Kopiranje podataka iz baze podataka sustava SQL Server na lokaciji na račun za spremište blobova platforme Azure
* Kopiranje podataka s računa za spremište blobova platforme Azure s bazom podataka SQL Azure.

>[AZURE.NOTE] Koraci prikazani ovdje su prilagoditi iz detaljnije vodič dodijelio tim za ADF: [Premještanje podataka između lokalnih izvora i u oblaku s pristupnik za upravljanje podacima](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) reference odjeljke te teme postoje ako je to prikladno.


## <a name="prereqs"></a>Preduvjeti
Pomoću ovog praktičnog vodiča podrazumijeva da imate:

* Za **Azure pretplate**. Ako nemate pretplatu, možete se prijaviti za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
* **Račun za Azure prostora za pohranu**. Koristite račun za Azure prostora za pohranu za spremanje podataka pomoću ovog praktičnog vodiča. Ako nemate račun za Azure prostora za pohranu, u članku [Stvaranje računa za pohranu](storage-create-storage-account.md#create-a-storage-account) . Kada stvorite račun za pohranu, morate nabaviti ključ za račun za pristup prostora za pohranu. Potražite u članku [Prikaz, Kopiraj i regenerate prostora za pohranu pristupnih tipki](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Pristup s **bazom podataka Azure SQL**. Ako morate postaviti baze podataka SQL Azure, tpoic [Uvod u rad s bazom podataka sustava Microsoft Azure SQL](../sql-database/sql-database-get-started.md) pruža informacije o tome kako Dodjela nove instance programa baze podataka SQL Azure.
* Instalirana i konfiguriran **Azure PowerShell** lokalno. Upute potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

> [AZURE.NOTE] Ovaj postupak koristi [Azure portal](https://portal.azure.com/).


##<a name="upload-data"></a>Prijenos podataka sustava SQL Server na lokaciji

Koristimo [NEW taksi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) da bismo pokazali postupak migracije. Skup podataka taksi SEBI je dostupna, kao što je naznačeno u tom objavu na Azure bloba prostora za pohranu [Podataka taksi SEBI](http://www.andresmh.com/nyctaxitrips/). Podaci ima dvije datoteke, datoteke trip_data.csv koji sadrži detalje putovanja, i datoteku trip_far.csv koja sadrži detalje o prijevoz platiti za svaki put. Uzorak i opis te datoteke nalaze se u [NEW taksi Trips Dataset opis](machine-learning-data-science-process-sql-walkthrough.md#dataset).


Možete prilagoditi postupak navedene skup vlastitim podacima ili slijedite korake opisan NEW taksi skupu podataka. Da biste prenijeli NEW taksi skupu podataka u bazu podataka sustava SQL Server na lokaciji, slijedite postupak za navedene u [Skupno uvoz podataka u bazu podataka sustava SQL Server](machine-learning-data-science-process-sql-walkthrough.md#dbload). Ove su upute su za SQL Server na programa Azure virtualnog računala, ali isti je postupak za prijenos sustava SQL Server na lokaciji.


##<a name="create-adf"></a>Stvaranje na tvorničke Azure podataka

Upute za stvaranje novog tvorničke podataka Azure i grupu resursa za [Azure portal](https://portal.azure.com/) postoje [Stvaranje na tvorničke Azure podataka](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory). Naziv nove instance ADF *adfdsp* i grupu stvorili resursa *adfdsprg*.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Instaliranje i konfiguriranje gore pristupnik za upravljanje podacima

Da biste omogućili vaše kanali na tvorničke programa Azure podataka za rad sa sustava SQL Server na lokaciji, morate ga dodati kao povezane servis tvorničke podataka. Da biste stvorili povezane servisa za sustava SQL Server na lokaciji, morate:

- Preuzmite i instalirajte pristupnik za upravljanje podacima na lokalno računalo. 
- Konfiguriranje servisa za povezane za lokalni izvor podataka da biste koristili pristupnika. 

Pristupnik za upravljanje podacima serializes i deserializes izvor i primatelj podataka na računalu na kojem je smještena.

Upute za postavljanje međuverzije i detalje o pristupnik za upravljanje podacima potražite u članku [Premještanje podataka između lokalnih izvora i u oblaku s pristupnik za upravljanje podacima](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)


## <a name="adflinkedservices"></a>Stvaranje povezanih servisa za povezivanje na izvor podataka

Servis za povezane definira podatke koji su potrebni za Azure tvorničke podatke za povezivanje s podacima resursa. Detaljni opis postupka za stvaranje povezani servisi navedeni su u [Stvaranje povezani servisi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services).

Imamo tri resursa u ovom scenariju za koje su vam potrebne povezani servisi.

1. [Povezane usluga za SQL Server na lokaciji](#adf-linked-service-onprem-sql)
2. [Povezane služba za spremište blobova platforme Azure](#adf-linked-service-blob-store)
3. [Povezane služba za baze podataka Azure SQL](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Povezane služba za baze podataka SQL Server na lokaciji

Da biste stvorili povezane servisa SQL Server na lokaciji:

- Kliknite **Spremište podataka** odredišna stranica za ADF klasični portala za Azure 
- Odaberite **SQL** i unesite vjerodajnice za *korisničko ime* i *lozinku* za SQL Server na lokaciji. Morate unijeti nazivu poslužitelja kao na **potpuno kvalificirani naziv instance naziv poslužitelja obrnutu kosu crtu (servername\instancename)**. Naziv povezane servis *adfonpremsql*.

###<a name="adf-linked-service-blob-store"></a>Povezane služba za blobova platforme

Da biste stvorili povezane služba za spremište blobova platforme Azure računa:

- Kliknite **Spremište podataka** odredišna stranica za ADF klasični portala za Azure
- Odaberite **Račun za pohranu za Azure** 
- Unesite spremište blobova platforme Azure ključ i spremnik naziv računa. Naziv povezane servis *adfds*.

###<a name="adf-linked-service-azure-sql"></a>Povezane služba za baze podataka Azure SQL

Da biste stvorili povezane servisa Azure SQL baze podataka:

- Kliknite **Spremište podataka** odredišna stranica za ADF klasični portala za Azure
- Odaberite **Azure SQL** i unesite vjerodajnice za *korisničko ime* i *lozinku* za baze podataka SQL Azure. *Korisničko ime* mora biti naveden kao *user@servername*.   


##<a name="adf-tables"></a>Definiranje i stvaranje tablica da biste odredili kako pristupiti u skupove podataka

Stvorite tablice koje navode strukturu, mjesto i dostupnost na skupova podataka pomoću sljedećih postupaka utemeljen na skripte. Datoteke JSON koriste se za definiranje tablice. Dodatne informacije o strukturi te datoteke potražite u članku [skupova podataka](../data-factory/data-factory-create-datasets.md).

> [AZURE.NOTE]  Moraju izvršiti na `Add-AzureAccount` cmdlet prije izvršavanja cmdleta [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) da biste potvrdili desno Azure pretplate uključen za izvršavanje naredbe. Dokumentaciju o ovaj cmdlet potražite u članku [Dodavanje AzureAccount](https://msdn.microsoft.com/library/azure/dn790372.aspx).

Definicija utemeljen na JSON u tablicama pomoću sljedećih naziva:

* **naziv tablice** u sustavu SQL server na lokaciji je *nyctaxi_data*
* **naziv spremnika** u spremište blobova platforme Azure računa je *naziv spremnika*  

Tri tablice definicije su vam potrebne za ovu ADF kanala:

1. [Tablica sustava SQL na lokaciji](#adf-table-onprem-sql)
2. [Blob tablice](#adf-table-blob-store)
3. [SQL Azure tablice](#adf-table-azure-sql)

> [AZURE.NOTE]  Ove postupke pomoću komponente PowerShell Azure definiranje i stvaranje ADF aktivnosti. No zadaci možete i napraviti pomoću portala za Azure. Detalje potražite u članku [Stvaranje ulazni i izlazni skupova podataka](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets).

###<a name="adf-table-onprem-sql"></a>Tablica sustava SQL na lokaciji

Definiciju tablice za SQL Server na lokaciji je naveden u sljedeća JSON datoteka:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

Nazivi stupaca nisu uvršteni u nastavku. Pod možete odabrati na nazive stupaca tako da ih ovdje uvrstite (za pojedinosti Provjera tema [ADF dokumentacije](../data-factory/data-factory-data-movement-activities.md ) .

Kopiranje JSON definiciju tablice u datoteku pod nazivom *onpremtabledef.json* datoteka i spremite ga na poznati mjesto (ovdje pretpostavlja se da *C:\temp\onpremtabledef.json*). Stvaranje tablice u ADF s sljedeći cmdlet Azure PowerShell:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>Blob tablice
Definiciju tablice za mjesto blob izlaz je u nastavku (karata ingested podatke iz lokalnog u blobova platforme Azure):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Kopiranje JSON definiciju tablice u datoteku pod nazivom *bloboutputtabledef.json* datoteka i spremite ga na poznati mjesto (ovdje pretpostavlja se da *C:\temp\bloboutputtabledef.json*). Stvaranje tablice u ADF s sljedeći cmdlet Azure PowerShell:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure tablice
Definiciju tablice SQL Azure izlaz je u nastavku (ovu shemu karata podatke koji dolaze iz blob-om):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Kopiranje JSON definiciju tablice u datoteku pod nazivom *AzureSqlTable.json* datoteka i spremite ga na poznati mjesto (ovdje pretpostavlja se da *C:\temp\AzureSqlTable.json*). Stvaranje tablice u ADF s sljedeći cmdlet Azure PowerShell:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Definiranje i stvorite kanal

Navedite aktivnosti koje pripadaju kanal i stvorite kanal sa sljedećih postupaka utemeljen na skriptu. Datoteke JSON koristi se za definiranje svojstva kanal.

* Skripta pretpostavlja da **kanal naziv** je *AMLDSProcessPipeline*.
* Imajte na umu i ne možemo postaviti periodicity kanal može izvršiti na dnevno i koristiti zadano vrijeme izvođenja za posao (12 am UTC).

> [AZURE.NOTE]Sljedećih postupaka pomoću komponente PowerShell Azure definiranje i stvaranje ADF kanal. No ovaj zadatak možete i napraviti pomoću portala za Azure. Detalje potražite u članku [Stvaranje i pokretanje na kanal](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline).

Korištenje definicije tablice koje ste prethodno naveli definiciju kanal za ADF navedena je na sljedeći način:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Kopirajte ovu definiciju JSON kanal u datoteku pod nazivom *pipelinedef.json* datoteka i spremite ga na poznati mjesto (ovdje pretpostavlja se da *C:\temp\pipelinedef.json*). Stvoriti kanal ADF s sljedeći cmdlet Azure PowerShell:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Potvrda možete vidjeti kanal na ADF na portalu klasični Azure prikazuju kao sljedeće (kada je kliknete dijagram)

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>Pokretanje kanal
Kanal mogu se izvoditi sada pomoću sljedeće naredbe:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

Vrijednosti parametara *DatumPočetka* i *završnog datuma* moraju biti zamijenjene stvarni datumi između kojih želite kanal za pokretanje.

Kada se izvršava kanal, ćete moći vidjeti podatke koji se prikazuju se u spremniku za blob-om, jedne datoteke dnevno odabrana.

Imajte na umu da ne možemo ste leveraged funkcionalnost nudi ADF kanala podataka postupno. Dodatne informacije o tome kako to i druge mogućnosti koje ste dobili od ADF potražite u [dokumentaciji ADF](https://azure.microsoft.com/services/data-factory/).
