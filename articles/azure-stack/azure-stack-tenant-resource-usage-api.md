<properties
    pageTitle="Smjernice za korištenje resursa API | Microsoft Azure"
    description="Vodič za korištenje resursa API-JA, koji Dohvaćanje informacija o korištenju Azure stogu."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>Smjernice za korištenje resursa API-JA

Klijent možete koristiti API klijent za prikaz podataka o korištenju klijenta vlastite resursa. Taj API se dosljedna Azure korištenja API-JA (trenutno privatne pretpregled).

Cmdlet komponente Windows PowerShell **Get-UsageAggregates** možete koristiti da biste pristupili podataka o korištenju kao što su servisu Azure.

## <a name="api-call"></a>API poziva

### <a name="request"></a>Zahtjev

Zahtjev dohvaća potrošnje detalje za tražene pretplate i tražene vremenski okvir. Postoji nema tijelo zahtjev.

| **Način**  | **URI zahtjeva** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| POČETAK         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenti

| **Argument**             | **Opis** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure resursima krajnjoj točci okruženje za Azure stogu. |
| *subId*                   | ID korisnika koji je upućivanje poziva pomoću pretplate. Taj API samo u upit možete koristiti za korištenje jedne pretplate. Davatelji možete koristiti API za korištenje resursa davatelj usluga za korištenje upita za sve klijenata. |
| *reportedStartTime*       | Početak upita. Vrijednost *DateTime* mora biti u UTC-u i na početku sata, na primjer, 13:00. Za dnevni Zbrajanje tu vrijednost postavite na ponoći UTC-a. Oblik je *unijeti prespojni znak* ISO 8601, na primjer, 2015 06 16T18% 3a53% 3a11% 2b00% 3a00Z, gdje dvotočka je unijeti prespojni znak za % 3a i plus je unijeti prespojni znak za % 2b tako da bude URI neslužbeni. |
| *reportedEndTime*         | Vrijeme završetka upita. Ograničenja koja se odnose na *reportedStartTime* primijeniti ovaj argument. Vrijednost za *reportedEndTime* ne može biti u budućnosti. |
| *aggregationGranularity*  | Neobavezan parametar koji sadrži dvije vrijednosti pojedinačnog potencijalne: svakodnevno i zaračunava. Kao vrijednosti predlažu, jedan vraća podatke u dnevne preciznosti, a drugi je svaki sat rješenja. Mogućnost dnevnih je zadana postavka. |
| *subscriberId*            | ID pretplate. Da biste dobili Filtrirani podaci, potreban je ID pretplate Izravni klijenta davatelja usluga. Ako nema pretplate parametar ID-a nije naveden, poziv vraća podatke o korištenju za sve davatelja Izravni klijenata. |
| *API-verzija*             | Verzija protokol koji se koristi da bi taj zahtjev. Morate koristiti 2015-06-01 – pregled. |
| *continuationToken*       | Token dohvaćeni iz zadnjeg poziva davatelju usluge korištenja API-JA. To je potrebno kada odgovor veća od 1000 retke. Ovo je knjižne oznake za napredak. Ako ne postoji učitavanja podataka od početka dan ili sata, ovisno o na granularnosti proslijeđen. |

### <a name="response"></a>Odgovor

DOHVAĆANJE /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 i reportedEndTime = 2015 06 01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = dnevnu & api-verzija = 1.0

{

"vrijednost":\[

{

"id": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"naziv": "sub1-meterID1"

"Vrsta": "Microsoft.Commerce/UsageAggregate"

"Svojstva": {

"subscriptionId": "sub1"

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\" mjesto\\":\\" Aljasku\\",\\" oznake\\": null,\\" additionalInfo\\": null}}",

:2.4000000000 "Količina"

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Detalji odgovora

| **Argument**      | **Opis** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID-a*              | Jedinstveni ID za korištenje aggregate |
| *ime*            | Naziv korištenje aggregate |
| *Vrsta*            | Definicija resursa |
| *subscriptionId*  | Pretplata identifikator Azure korisnika |
| *usageStartTime*  | Vrijeme korištenja grupe kojoj pripada združeni korištenje početka UTC-a |
| *usageEndTime*    | Vrijeme završetka UTC-a za korištenje grupe kojoj pripada združeni korištenje |
| *instanceData*    | Ključ vrijednost parova pojedinosti instancu (u novom obliku):<br>  *resourceUri*: u potpunosti kvalificirana ID resursa, uključujući grupe resursa i naziv instance <br>  *lokacija*: područje u kojem se pokrenuti servis <br>  *oznaka*: oznake resursa koji navodi korisnika <br>  *additionalInfo*: više Detalji o resursa potrošena, na primjer, vrsta verzija ili slike s operacijskim Sustavom |
| *Količina*        | Iznos potrošnje resursa kojih je došlo u ovom vremenskog okvira |
| *meterId*         | Jedinstveni ID za resurs koji je potrošena (i pod nazivom *ResourceID*) |

## <a name="next-steps"></a>Daljnji koraci

[Najčešća pitanja vezana uz vezane uz korištenje](azure-stack-usage-related-faq.md)
