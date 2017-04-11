<properties
    pageTitle="Korištenje resursa davatelja API | Microsoft Azure"
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

# <a name="provider-resource-usage-api"></a>Korištenje resursa davatelja API-JA

Davatelj termina primjenjuje za administratora servisa te neki ovlaštenih usluga. Administratori servisa i ovlaštenog davatelji možete koristiti davatelja usluge korištenja API-JA da biste pogledali korištenje njihove Izravni klijenata. Ako, na primjer, P0 da biste uputili poziv API davatelja da biste dobili informacije o korištenju P1 korisnika i mjesta P2 izravno korištenje, a za informacije o korištenju P3 i P4 možete nazvati P1.

![Konceptualni modela hijerarhije davatelja usluga](media/azure-stack-provider-resource-api/image1.png)


## <a name="api-call-reference"></a>Referenca za poziv API-JA

### <a name="request"></a>Zahtjev

Zahtjev dohvaća potrošnje detalje za traženi pretplate i traženi vremenski okvir. Postoji nema tijelo zahtjev.

Korištenje API je davatelj API-jem tako Pozivatelj mora biti dodijeljena ulogu vlasnika, suradnik ili čitač pretplate na svojeg davatelja usluga.

| **Način**  | **URI zahtjeva** |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|  POČETAK        | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenti

| **Argument**              | **Opis** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *armendpoint*             | Azure resursima krajnjoj točci okruženje za Azure stogu. Konvencije Azure stoga je naziv krajnje točke ARM mora biti u obliku https://api. {Naziv domene} ". Ako, na primjer, ako naziv domene je azurestack.local, zatim krajnju točku ARM bit će https://api.azurestack.local. |
| *subId*                   | ID korisnika koji je upućivanje poziva pomoću pretplate. |
| *reportedStartTime*       | Početak upita. Vrijednost *DateTime* mora biti u UTC-u i na početku sata, na primjer, 13:00. Za dnevni Zbrajanje tu vrijednost postavite na ponoći UTC-a. Oblik je *unijeti prespojni znak* ISO 8601, na primjer, 2015 06 16T18% 3a53% 3a11% 2b00% 3a00Z, gdje dvotočka je unijeti prespojni znak za % 3a i plus je unijeti prespojni znak za % 2b tako da bude URI neslužbeni. |
| *reportedEndTime*         | Vrijeme završetka upita. Ograničenja koja se odnose na *reportedStartTime* primijeniti ovaj argument. Vrijednost za *reportedEndTime* ne može biti u budućnosti. |
| *aggregationGranularity*  | Neobavezan parametar koji sadrži dvije vrijednosti pojedinačnog potencijalne: svakodnevno i zaračunava. Kao vrijednosti predlažu, jedan vraća podatke u dnevne preciznosti, a drugi je svaki sat rješenja. Mogućnost dnevnih je zadana postavka. |
| *subscriberId*            | ID pretplate. Da biste dobili Filtrirani podaci, potreban je ID pretplate Izravni klijenta davatelja usluga. Ako nema pretplate parametar ID-a nije naveden, poziv vraća podatke o korištenju za sve davatelja Izravni klijenata. |
| *API-verzija*             | Verzija protokol koji se koristi da bi taj zahtjev. Morate koristiti 2015-06-01 – pregled. |
| *continuationToken*       | Token dohvaćeni iz zadnjeg poziva davatelju usluge korištenja API-JA. To je potrebno kada odgovor veća od 1000 retke. Ovo je knjižne oznake za napredak. Ako ne postoji učitavanja podataka od početka dan ili sata, ovisno o na preciznosti proslijeđen. |



### <a name="response"></a>Odgovor

DOHVAĆANJE /subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 i reportedEndTime = 2015 06 01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = dnevnu & subscriberId = sub1.1 & api-verzija = 1.0

{

"vrijednost":\[

{

"id": "/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1 ",

"naziv": "sub1.1-meterID1"

"Vrsta": "Microsoft.Commerce/UsageAggregate"

"Svojstva": {

"subscriptionId": "sub1.1"

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\"mjesto\\

":\\" Aljasku\\",\\" oznake\\": null,\\" additionalInfo\\": null}}",

:2.4000000000 "Količina"

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Detalji odgovora


| **Argument**       | **Opis**
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID-a*               | Jedinstveni ID za korištenje aggregate
| *ime*             | Naziv korištenje aggregate
| *Vrsta*             | Definicija resursa
| *subscriptionId*   | Identifikator korisnika u stogu Azure pretplate
| *usageStartTime*   | Vrijeme korištenja grupe kojoj pripada združeni korištenje početka UTC-a
| *usageEndTime*     | Vrijeme završetka UTC-a za korištenje grupe kojoj pripada združeni korištenje
| *instanceData*     | Ključ vrijednost parova pojedinosti instancu (u novom obliku):<br> *resourceUri*: u potpunosti kvalificirana ID resursa koji uključuje grupe resursa i naziv instance <br> *lokacija*: područje u kojem se pokrenuti servis <br> *oznaka*: resursa oznake koje su navedene korisnik <br> *additionalInfo*: više Detalji o resursa potrošena, na primjer, vrsta verzija ili slike s operacijskim Sustavom |
| *Količina*         | Iznos potrošnje resursa kojih je došlo u ovom vremenskog okvira |
| *meterId*          | Jedinstveni ID za resurs koji je potrošena (i pod nazivom *ResourceID*) |

## <a name="next-steps"></a>Daljnji koraci

[Korištenje resursa klijentu API reference](azure-stack-tenant-resource-usage-api.md)

[Najčešća pitanja vezana uz vezane uz korištenje](azure-stack-usage-related-faq.md)
