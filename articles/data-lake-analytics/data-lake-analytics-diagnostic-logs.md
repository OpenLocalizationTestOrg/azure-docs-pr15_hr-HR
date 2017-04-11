<properties 
   pageTitle="Prikaz dijagnostičkih zapisnika Lake analitičkih podataka za Azure | Microsoft Azure" 
   description="Razumijevanje upute za postavljanje i pristup dijagnostičke zapisnike analitičkih Lake Azure podataka " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Pristup dijagnostičke zapisnike Lake analitičkih podataka za Azure

Informirajte se o uključite Zapisivanje dijagnostičkih podataka za vaš račun analize podataka Lake i zapisnicima koji se prikupljaju za vaš račun.

Tvrtke i ustanove možete omogućiti za svoj račun za Azure podataka Lake Analytics za prikupljanje podataka programa access tragovi Zapisivanje dijagnostičkih podataka. Ti zapisnici Navedite podatke kao što su:

* Popis korisnika kojima se pristupa podacima.
* Koliko se često pristupa podacima.
* Koliko se podaci se pohranjuju u računa.

## <a name="prerequisites"></a>Preduvjeti

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- **Omogućivanje pretplate Azure** pretpregled javno analize podataka Lake. Pročitajte [upute](data-lake-analytics-get-started-portal.md#signup).
- **Račun za azure podataka Lake analize**. Slijedite upute na [Početak rada s analize Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>Omogućite zapisivanje

1. Prijavite se novi [Azure portal](https://portal.azure.com).

2. Otvorite račun analize podataka Lake i vaš račun plohu analize podataka Lake, kliknite **Postavke**, a zatim **Dijagnostičkih postavki**.

3. U **dijagnostičkih** plohu izvršite sljedeće promjene da biste konfigurirali Zapisivanje dijagnostičkih podataka.

    ![Uključite Zapisivanje dijagnostičkih podataka] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Omogućivanje dijagnostičkih zapisnika")

    * **Status** postavite **na** uključite Zapisivanje dijagnostičkih podataka.
    * Možete odabrati pohrana/postupak podatke na dva načina.
        * Odaberite **Izvoz događaja koncentrator** za strujanje se podaci iz zapisnika programa Azure koncentrator za događaj. Koristite ovu mogućnost ako imate do obrada kanala da biste analizirali dolazne zapisnika u stvarnom vremenu. Ako odaberete tu mogućnost, morate unijeti detalje za središtu Azure događaj koji želite koristiti.
        * Odaberite **Izvoz u račun za pohranu** za spremanje zapisnika u račun za Azure pohranu. Tu mogućnost koristite ako želite arhivirati podatke. Ako odaberete tu mogućnost, morate navesti račun za Azure pohranu da biste spremili zapisnike da biste.
    * Odredite želite li da biste dobili zapisnike nadzora ili zapisnika zahtjev ili i jedno i drugo.
    * Navedite broj dana za koji moraju zadržati podatke.
    * Kliknite **Spremi**.

Nakon što ste omogućili dijagnostičkih postavki, možete gledati zapisnike na kartici **Dijagnostičke zapisnike** .

## <a name="view-logs"></a>Prikaz zapisnika

Da biste pogledali podatke za vaš račun analize podataka Lake na dva načina.

* Iz analize podataka Lake postavke računa
* S računa za pohranu Azure pohrane podataka

### <a name="using-the-data-lake-analytics-settings-view"></a>Korištenje podataka Lake analize postavke prikaza

1. Plohu analize podataka Lake račun **Postavke** , kliknite **Dijagnostičke zapisnike**.

    ![Prikaz dijagnostičkih bilježenje u zapisnik] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Prikaz dijagnostičkih zapisnika") 

2. U plohu **Dijagnostičke zapisnike** trebali biste vidjeti zapisnike kategorizirani po **Zapisnike nadzora** i **Zatražite zapisnika**.
    * Zahtjev za zapisnike snimite svaki zahtjev API-JA na računu analize podataka Lake.
    * Slične zahtjev zapisnike, no nudi mnogo detaljnije razrada svega postupaka koji se izvode na računu analize podataka Lake su zapisnika nadzora. Ako, na primjer, pojedinačni prijenos poziva API-JA u zapisnicima zahtjev može rezultirati više operacija "Dodaj" u zapisnika nadzora.

3. Kliknite vezu za **Preuzimanje** za stavku evidencije da biste preuzeli zapisnike.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>S računa za Azure prostora za pohranu koji sadrži podatke iz zapisnika

1. Otvorite plohu račun za pohranu Azure povezane s analize podataka Lake za zapisivanje, a zatim kliknite blob-ova. **Servis Blob** plohu popis dva spremnika.

    ![Prikaz dijagnostičkih bilježenje u zapisnik] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Prikaz dijagnostičkih zapisnika")

    * Spremnik **uvida zapisnike nadzora** sadrži zapisnika nadzora.
    * Spremnik **uvida zapisnika zahtjeve** sadrži zapisnike zahtjev.

2. Unutar te spremnike zapisnike spremaju se u odjeljku sljedeću strukturu.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] Na `##` stavke na putu sadrže godinu, mjesec, dan i sat u kojem je stvorena u zapisnik. Analize podataka Lake stvara jedne datoteke svaki sat pa `m=` uvijek sadrži vrijednost `00`.

    Na primjer, potpuni put do zapisnik nadzora može biti:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Isto tako, može biti potpuni put do zapisnik zahtjev:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Struktura zapisnika

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
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
| operationName   | Niz | Naziv operacije zapisane. Na primjer, GetAggregatedJobHistory.              |
| resultType      | Niz | Stanje postupka, na primjer, 200.                                 |
| callerIpAddress | Niz | IP adresa klijenta upućivanje zahtjeva                                |
| correlationId   | Niz | Id zapisnika. Ta vrijednost može se koristiti za grupiranje zapisnika povezane stavke |
| identitet        | Objekt | Identitet koji je generirao u zapisnik                                            |
| Svojstva      | JSON   | U sljedećem odjeljku (zahtjev zapisnika svojstva sheme) dodatne informacije |

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
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
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
| operationName   | Niz | Naziv operacije zapisane. Na primjer, JobSubmitted.              |
| resultType | Niz | Substatus stanja zadatka (operationName). |
| resultSignature | Niz | Dodatne informacije o statusu zadatka (operationName). |
| identitet      | Niz | Korisnik koji ste tražili postupak. Na primjer, susan@contoso.com.                                 |
| Svojstva      | JSON   | U sljedećem odjeljku (sheme svojstva zapisnika nadzora) dodatne informacije |

> [AZURE.NOTE] __resultType__ __resultSignature__ pružaju informacije o rezultat operacije i samo ako je dovršena postupak sadrže vrijednost. Na primjer, koje sadrže vrijednost kada __operationName__ sadrži vrijednost __JobStarted__ ili __JobEnded__.

#### <a name="audit-log-properties-schema"></a>Shema za svojstva zapisnika nadzora

| Ime       | Vrsta   | Opis                              |
|------------|--------|------------------------------------------|
| JobId | Niz | ID koji je dodijeljen zadatak  |
| JobName | Niz | Naziv koji ste dobili za posao |
| JobRunTime | Niz | Izvođenje koji se koristi za obradu posao |
| SubmitTime | Niz | Vrijeme (po UTC-u) koja je poslana posao |
| StartTime | Niz | Vrijeme posao pokrenut nakon slanja (po UTC-u). |
| EndTime | Niz | Vrijeme je završen posao. |
| Parallelism | Niz | Broj jedinica analize Lake podatke potrebne za ovaj zadatak tijekom slanja. |

> [AZURE.NOTE] __SubmitTime__, __StartTime__, __EndTime__ __Parallelism__ pružaju informacije o operaciju i samo ako je postupak rada ili dovršiti sadrže vrijednost. Na primjer, __SubmitTime__ sadrži vrijednost nakon __operationName__ označava __JobSubmitted__.

## <a name="process-the-log-data"></a>Obrada podataka zapisnika

Azure analize podataka Lake daju uzorka za obradu i analizirati podatke zapisnika. Uzorak možete pronaći na [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="next-steps"></a>Daljnji koraci

- [Pregled analize Lake Azure podataka](data-lake-analytics-overview.md)


