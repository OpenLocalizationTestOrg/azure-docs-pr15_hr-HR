<properties 
   pageTitle="Prikaz dijagnostičkih zapisnika za Azure podataka Lake spremište | Microsoft Azure" 
   description="Razumijevanje upute za postavljanje i pristup dijagnostičke zapisnike Azure podataka Lake spremišta " 
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Pristup dijagnostičke zapisnike Azure podataka Lake spremišta

Informirajte se o uključite Zapisivanje dijagnostičkih podataka za vaš račun spremišta Lake podataka i zapisnicima koji se prikupljaju za vaš račun.

Tvrtke i ustanove mogu omogućiti zapisivanje dijagnostičkih podataka za svoj račun Lake spremišta podataka za Azure prikupljanje podataka programa access tragovi koji daje informacije kao što je popis korisnika pristup podacima, koliko se često pristupa podacima, koliko podaci se pohranjuju u račun, itd.

## <a name="prerequisites"></a>Preduvjeti

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Račun za azure podataka Lake trgovine**. Slijedite upute na [Početak rada s spremišta Lake podataka za Azure pomoću portala za Azure](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Uključite Zapisivanje dijagnostičkih podataka za vaš račun spremišta Lake podataka

1. Prijavite se novom [Portalu Azure](https://portal.azure.com).

2. Otvorite računa spremišta Lake podataka i vaše plohu računa spremišta Lake podataka kliknite **Postavke**, a zatim **Dijagnostičkih postavki**.

3. U **dijagnostičkih** plohu izvršite sljedeće promjene da biste konfigurirali Zapisivanje dijagnostičkih podataka.

    ![Uključite Zapisivanje dijagnostičkih podataka] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Omogućivanje dijagnostičkih zapisnika")

    * **Status** postavite **na** uključite Zapisivanje dijagnostičkih podataka.
    * Možete odabrati pohrana/postupak podatke na dva načina.
        * Odaberite željenu mogućnost da biste **izvezli događaj koncentrator** za strujanje se podaci iz zapisnika programa Azure koncentrator za događaj. Vjerojatno će koristiti ovu mogućnost ako imate kanal do obrade analize dolazne zapisnike u stvarnom vremenu. Ako odaberete tu mogućnost, morate unijeti detalje za središtu Azure događaj koji želite koristiti.
        * Odaberite željenu mogućnost da biste **izvezli u račun za pohranu** za spremanje zapisnika u račun za Azure pohranu. Tu mogućnost koristite ako želite arhivirati podatke koji će biti obradu obrađuju kasnije. Ako odaberete tu mogućnost, morate unijeti poslovni subjekt Azure prostora za pohranu za spremanje zapisnika u.
    * Odredite želite li da biste dobili zapisnike nadzora ili zapisnika zahtjev ili i jedno i drugo.
    * Navedite broj dana za koji moraju zadržati podatke.
    * Kliknite **Spremi**.

Nakon što ste omogućili dijagnostičkih postavki, možete gledati zapisnike na kartici **Dijagnostičke zapisnike** .

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Prikaz dijagnostičkih zapisnika za vaš račun spremišta Lake podataka

Da biste pogledali podatke za vaš račun spremišta Lake podataka na dva načina.

* Iz prikaza za postavke računa spremišta Lake podataka
* S računa za pohranu Azure pohrane podataka

### <a name="using-the-data-lake-store-settings-view"></a>Korištenje prikaza postavke pohrane Lake podataka

1. Plohu spremišta podataka Lake račun **Postavke** , kliknite **Dijagnostičke zapisnike**.

    ![Prikaz dijagnostičkih bilježenje u zapisnik] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Prikaz dijagnostičkih zapisnika") 

2. U plohu **Dijagnostičke zapisnike** trebali biste vidjeti zapisnike kategorizirani po **Zapisnike nadzora** i **Zatražite zapisnika**.
    * Zahtjev za zapisnike snimite svaki zahtjev API-JA na računu za spremište Lake podataka.
    * Zapisnike nadzora su slične zahtjev zapisnike, no nudi mnogo detaljnije analitički operacija se izvodi na računu za spremište Lake podataka. Ako, na primjer, pojedinačni prijenos poziva API-JA u zapisnicima zahtjev može rezultirati više operacija "Dodaj" u zapisnika nadzora.

3. Kliknite vezu za **Preuzimanje** protiv svaku stavku zapisnika da biste preuzeli zapisnike.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>S računa za Azure prostora za pohranu koji sadrži podatke iz zapisnika

1. Otvorite plohu račun za pohranu Azure pridružene spremišta Lake podataka za prijavu, a zatim kliknite blob-ova. **Servis Blob** plohu popis dva spremnika.

    ![Prikaz dijagnostičkih bilježenje u zapisnik] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Prikaz dijagnostičkih zapisnika")

    * Spremnik **uvida zapisnike nadzora** sadrži zapisnika nadzora.
    * Spremnik **uvida zapisnika zahtjeve** sadrži zapisnike zahtjev.

2. Unutar te spremnika zapisnike spremaju se u odjeljku sljedeću strukturu.

    ![Prikaz dijagnostičkih bilježenje u zapisnik] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Prikaz dijagnostičkih zapisnika")

    Na primjer, može biti potpuni put do zapisnik nadzora`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Similary, potpuni put do zapisnik zahtjev možda`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Razumijevanje strukturu podataka zapisnika

Zapisnike nadzora i zahtjev su JSON OSNOVNI oblik. U ovom ćete odjeljku smo pogledajte strukturu JSON za zahtjev i zapisnika nadzora.

### <a name="request-logs"></a>Zahtjev za zapisnika

Evo ogledni unos u zapisniku JSON oblikovani zahtjev. Svaki blob sadrži jedan Korijenski objekt koji se naziva **zapisa** koji sadrži polje zapisnika objekata.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Zahtjev za zapisnika sheme

| Ime            | Vrsta   | Opis                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| vrijeme            | Niz | Vremenska oznaka (po UTC-u) zapisnika                                              |
| resourceId      | Niz | ID resursa koji traje operacija postavite na                            |
| Kategorija        | Niz | Kategorija zapisnika. Na primjer, **Zahtjevi**.                                   |
| operationName   | Niz | Naziv operacije zapisane. Na primjer, getfilestatus.              |
| resultType      | Niz | Stanje postupka, na primjer, 200.                                 |
| callerIpAddress | Niz | IP adresa klijenta upućivanje zahtjeva                                |
| correlationId   | Niz | Id zapisnika koje možete koristiti za grupiranje skup zapisnika povezane stavke |
| identitet        | Objekt | Identitet koji je generirao u zapisnik                                            |
| Svojstva      | JSON   | Potražite u nastavku pojedinosti                                                          |

#### <a name="request-log-properties-schema"></a>Zahtjev za zapisnika svojstva sheme

| Ime                 | Vrsta   | Opis                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod           | Niz | HTTP metoda koristi za operaciju. Na primjer, DOBITI. |
| Put                 | Niz | Put operacija izvršena na                   |
| RequestContentLength | INT    | Duljina sadržaja zahtjev HTTP                    |
| ClientRequestId      | Niz | Id koji služi kao jedinstvena identifikacija zahtjev              |
| StartTime            | Niz | Vrijeme poslužitelj zaprimanja zahtjev         |
| EndTime              | Niz | Vrijeme u kojem poslužitelj je poslao odgovor              |

### <a name="audit-logs"></a>Zapisnika nadzora

Evo ogledni unos u zapisniku nadzora JSON oblikovani. Svaki blob sadrži jedan Korijenski objekt koji se naziva **zapisa** koji sadrži polje zapisnika objekata

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Shema zapisnika nadzora

| Ime            | Vrsta   | Opis                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| vrijeme            | Niz | Vremenska oznaka (po UTC-u) zapisnika                                              |
| resourceId      | Niz | ID resursa koji traje operacija postavite na                            |
| Kategorija        | Niz | Kategorija zapisnika. Na primjer, **nadzora**.                                      |
| operationName   | Niz | Naziv operacije zapisane. Na primjer, getfilestatus.              |
| resultType      | Niz | Stanje postupka, na primjer, 200.                                 |
| correlationId   | Niz | Id zapisnika koje možete koristiti za grupiranje skup zapisnika povezane stavke |
| identitet        | Objekt | Identitet koji je generirao u zapisnik                                            |
| Svojstva      | JSON   | Potražite u nastavku pojedinosti                                                          |

#### <a name="audit-log-properties-schema"></a>Shema za svojstva zapisnika nadzora

| Ime       | Vrsta   | Opis                              |
|------------|--------|------------------------------------------|
| StreamName | Niz | Put operacija izvršena na  |


## <a name="samples-to-process-the-log-data"></a>Uzorci za obradu podataka zapisnika

Spremište Lake podataka za Azure daju uzorka za obradu i analizirati podatke zapisnika. Uzorak možete pronaći na [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="see-also"></a>Vidi također

- [Pregled Lake spremišta podataka za Azure](data-lake-store-overview.md)
- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)

