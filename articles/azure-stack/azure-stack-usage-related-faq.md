<properties
    pageTitle="Najčešća pitanja za vezane uz korištenje | Microsoft Azure"
    description="Popis metar stogu Azure, usporedbe Azure korištenja API-JA, korištenje vrijeme i potrebnom vrijeme šifre pogrešaka."
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

# <a name="azure-stack-usage-api-faqs"></a>Najčešća pitanja za korištenje API Azure stogu
U ovom se članku navedeni odgovori na najčešća pitanja o korištenju API-JA snop Azure.

## <a name="what-meter-ids-can-i-see"></a>Što metar ID-a mogu vidjeti?

Trenutno korištenje prijavljuje za mrežu, za pohranu i računalnim resursa davatelja.

| **Davatelja resursa** | **ID metar** |**Naziv metar** | **Jedinica** | **Dodatne informacije** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Mreža** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Korištenje javnu IP adresa | IP adresa |                    
| **Prostor za pohranu**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*sata | Ukupni kapacitet troše tablice |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*sata | Ukupni kapacitet troše blob-Ova stranica |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*sati  | Ukupni kapacitet troše reda čekanja |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*sata  | Ukupni kapacitet troše bloka blob-ova |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Zahtjev za brojanje u 10,000s   | Tablica zahtjevima za uslugu (u 10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Ingress podataka u GB | Tablica servisa podataka ingress u GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress u GB | Tablica servisa podataka izlazne u GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Zahtjevi za brojanje u 10,000s | Blob zahtjevima za uslugu (u 10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Ingress podataka u GB          | Blob servisa podataka ingress u GB 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress u GB              | Blob servisa podataka izlazne u GB 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Zahtjevi za brojanje u 10,000s   | Red čekanja zahtjevima za uslugu (u 10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Ingress podataka u GB         | Red čekanja servisa podataka ingress u GB 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress u GB  | Red čekanja servisa podataka izlazne u GB 
| **Izračun** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | VM veličina sata | Veličina virtualnog računala |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Po čemu se korištenja API-ji stogu Azure razlikuje [Azure korištenja API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (trenutno u javnom pretpregled)?

-   Korištenje API klijent je u skladu s API Azure, uz jednu iznimku: zastavice *showDetails* trenutno nije podržano u stogu Azure.

-   Davatelj usluge korištenja API-JA odnosi samo na hrpu Azure.

-   Trenutno [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) koja je dostupna u Azure nije dostupna u stogu Azure.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Koja je razlika između korištenje vremena i potrebnom vremena?

Izvješća o korištenju podataka postoje dvije vrijednosti glavni vremena:

-   **Prijavili vremena**. Vrijeme prilikom korištenja događaja unijeli korištenje sustava

-   **Korištenje vremena**. Vrijeme kada je potrošena Azure stogu resursa

Možda ćete vidjeti nepodudaranje vrijednosti za korištenje vremena i vremenu potrebnom za korištenje određenih događaj. Odgoda može biti iste duljine kao više vremena u bilo kojem okruženju.

Trenutno upita *samo pomoću prijavljenih vremena*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Što znače te šifre pogrešaka korištenja API-JA?

| **Šifra stanja HTTP** | **Kod pogreške** | **Opis** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400-Neispravan zahtjev        | *NoApiVersion*     | Nedostaje parametar upita *api-verzija* .
| 400-Neispravan zahtjev        | *InvalidProperty*  | Svojstvo nema ili ima vrijednost koja nije valjana. U poruci u kod pogreške u tijelu odgovor označava nedostaje svojstvo.
| 400-Neispravan zahtjev        | *RequestEndTimeIsInFuture*  | Vrijednost za *ReportedEndTime* je u budućnosti. Vrijednosti u budućnosti nije dopuštena za ovaj argument.
| 400-Neispravan zahtjev        | *SubscriberIdIsNotDirectTenant*    | Davatelj API poziva koristi ID-a za pretplatu koja nije valjana klijentu pozivatelja.
| 400-Neispravan zahtjev        | *SubscriptionIdMissingInRequest*   | Nedostaje pretplate ID pozivatelja.
| 400-Neispravan zahtjev        | *InvalidAggregationGranularity*   | Zatraženo je programa koji nisu valjani zbrajanja preciznosti. Valjane vrijednosti su dnevni i svaki sat.
| 503                    | *ServiceUnavailable*   | Budući da je servis zauzet ili poziv u tijeku ograničio vrijeme, pojavila se pogreška u retryable. |

## <a name="next-steps"></a>Daljnji koraci
[Klijenta za naplatu i chargeback u stogu Azure](azure-stack-billing-and-chargeback.md)

[Korištenje resursa davatelja API-JA](azure-stack-provider-resource-api.md)

[Smjernice za korištenje resursa API-JA](azure-stack-tenant-resource-usage-api.md)
