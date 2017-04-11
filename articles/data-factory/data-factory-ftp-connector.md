<properties 
    pageTitle="Premještanje podataka iz FTP poslužitelj | Microsoft Azure" 
    description="Saznajte više o premještanje podataka s FTP poslužitelja pomoću tvorničke Azure podataka." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Premještanje podataka s FTP poslužitelja pomoću tvorničke Azure podataka
U ovom se članku opisuje kako koristiti aktivnosti Kopiraj u tvorničke Azure podataka da biste premjestili sadržaj s FTP poslužitelja podržani primatelj izvor podataka. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koji predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i na popisu podataka trgovine podržana kao izvora/primatelji. 

Tvorničke podataka trenutno podržava samo premještanje podataka s FTP poslužitelja u drugim spremišta podataka, ali ne premještanju podataka iz trgovine drugih podataka na FTP poslužitelj. Podržava oba lokalnih podataka i FTP poslužiteljima u oblaku. 

Ako premještate podatke iz **lokalnog** FTP poslužitelja u oblak izvor podataka (primjer: spremište blobova platforme Azure), instalaciju i korištenje pristupnik za upravljanje podacima. Pristupnik za upravljanje podacima je agent za klijenta koji je instaliran na vašem računalu lokalnog koji omogućuje servise u oblaku za povezivanje s lokalnim resursa. Detalje o pristupnika potražite u članku [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md) . Potražite u članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) detaljne upute na postavljanje pristupnika te ga koristiti. Pomoću pristupnika možete povezati s poslužiteljem FTP čak i ako je poslužitelj na Azure IaaS virtualnog računala (VM). 

Pristupnika možete instalirati na tom računalu lokalnog ili Azure IaaS VM kao FTP poslužitelj. No preporučujemo da instalirate pristupnika na zasebnom računalu ili zasebnom VM IaaS Azure da biste izbjegli Nadmetanje resursa i bolje performanse. Kada instalirate pristupnika na zasebnom računalo, na računalu trebali biste moći pristupiti FTP poslužitelj. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Najlakši način stvaranja kanala koja se kopira podatke iz FTP poslužitelj je za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

Sljedeći primjeri sadrže definicije JSON uzorka koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Primjer: Kopiranje podataka iz FTP poslužitelj blobova platforme Azure

Ovaj primjer pokazuje kako kopirati podatke iz FTP poslužitelj blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

- Povezane servis vrste [FtpServer](#ftp-linked-service-properties).
- Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Za unos [dataset](data-factory-create-datasets.md) vrste [FileShare](#fileshare-dataset-type-properties).
- Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [FileSystemSource](#ftp-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka s FTP poslužitelja blobova platforme Azure svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

**FTP povezana servisa** U ovom se primjeru koristi osnovna provjera autentičnosti pomoću korisničkog imena i lozinke u obliku običnog teksta. Također možete koristiti jednu od sljedećih načina: 

- Anonimna provjera autentičnosti 
- Osnovna provjera autentičnosti pomoću šifrirane vjerodajnica
- FTP putem SSL/TLS (FTPS)

Sekciji [FTP povezani servisa](#ftp-linked-service-properties) potražite u članku za različite vrste provjere autentičnosti možete koristiti. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
    }

**Azure servis za pohranu povezana**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Unos dataset FTP** Ovaj skup podataka koji se odnosi na FTP mape `mysharedfolder` i `test.csv`. Kanal kopira datoteku na odredište. 

Postavljanje "vanjski": "true" obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Blobova platforme Azure izlazni skup podataka**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Kanal s Kopiraj aktivnosti**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **FileSystemSource** , a **primatelj** je vrsta **BlobSink**. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>Svojstva FTP povezane servisa

Sljedeća tablica sadrži opis elemenata JSON specifične za FTP povezane servis.

| Svojstvo | Opis | Obavezno | Zadani |
| -------- | ----------- | -------- | ------- | 
| Vrsta | Svojstvo vrsta mora biti postavljeno na FtpServer | Da | &nbsp;
| glavno računalo | Naziv ili IP adresu na FTP poslužitelj | Da | &nbsp;
| authenticationType | Određivanje vrsta provjere autentičnosti | Da | Osnovni, anonimni |
| korisničko ime | Korisnik koji ima pristup FTP poslužitelj | ne | &nbsp;
| lozinke | Lozinku za korisnika (korisničko ime) | ne | &nbsp;
| encryptedCredential | Šifrirana vjerodajnica za pristup poslužitelju FTP | ne | &nbsp;
| gatewayName | Naziv pristupnika pristupnik za upravljanje podacima za povezivanje s lokalnim FTP poslužitelj | ne    | &nbsp;
| priključak | Priključak na kojem se priključuje FTP poslužitelj | ne | 21 |
| enableSsl | Odredite želite li koristiti FTP putem kanala SSL/TLS | ne | TRUE | 
| enableServerCertificateValidation | Odredite želite li omogućiti Provjera valjanosti SSL certifikat za poslužitelja pri korištenju FTP putem kanala SSL/TLS | ne | TRUE | 

### <a name="using-anonymous-authentication"></a>Korištenje anonimna provjera autentičnosti

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Pomoću korisničkog imena i lozinke u obliku običnog teksta za osnovnu provjeru autentičnosti
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Korištenje priključak, enableSsl, enableServerCertificateValidation

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Pomoću encryptedCredential za provjeru autentičnosti i pristupnika

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Detalje o postavljanju vjerodajnica za lokalni izvor podataka FTP u odjeljku [postavka vjerodajnice i sigurnost](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Svojstva vrste aktivnosti FTP Kopiraj

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, svojstva vrste razlikuju se ovisno o vrsti izvora i primatelji.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.

## <a name="next-steps"></a>Daljnji koraci
Potražite u sljedećim člancima: 

- [Kopiraj aktivnosti vodič](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) za detaljne upute za stvaranje na kanal s Kopiraj aktivnosti. 
