<properties
    pageTitle="Primjeri za uobičajene uzorke strujanje analize korištenja upita | Microsoft Azure"
    description="Uobičajene uzorke Azure strujanje analize upita "
    keywords="Primjeri upita"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Primjeri za uobičajene uzoraka korištenja strujanje Analytics za upite

## <a name="introduction"></a>Uvod

Upita u Azure strujanje analize izražena je na jeziku nalik SQL upit koji je opisan u vodiču za [Strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx) .  U ovom se članku navode rješenja za nekoliko uobičajenih upita uzoraka koji se temelji na scenarije stvarnih svijeta.  To je zadatak u tijeku, a bit će se ažurirati uzorcima novi pridonositi.

## <a name="query-example-data-type-conversions"></a>Primjer upita: pretvorbe vrsta podataka
**Opis**: definiranje vrste svojstava na ulaznog toka.
Ako, na primjer, uskoro Debljina automobila na ulaznog toka kao nizove i mora se pretvoriti u INT za izvođenje ZBROJI ga.

**Unos**:

| Provjerite | Vrijeme | Težina |
| --- | --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z | "1000" |
| Honda | 2015-01 – 01T00:00:02.0000000Z | "2000" |

**Izlaz**:

| Provjerite | Težina |
| --- | --- |
| Honda | 3000 |

**Rješenje**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Objašnjenje**: upotrijebite naredbu GLUMCIMA na polju Debljina da biste odredili vrstu (pogledajte popis podržane vrste podataka [u nastavku](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Primjer upita: korištenje sviđa ili nije li uzorak koji se podudaraju
**Opis**: Provjerite podudaraju li se vrijednost polja na događaj određene uzorak, primjerice, povrata ploče licencu koji počinju slovom A i završavaju 9

**Unos**:

| Provjerite | LicensePlate | Vrijeme |
| --- | --- | --- |
| Honda | ABC 123 | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | AAA 999 | 2015-01 – 01T00:00:02.0000000Z |
| Nissan | ABC 369 | 2015-01 – 01T00:00:03.0000000Z |

**Izlaz**:

| Provjerite | LicensePlate | Vrijeme |
| --- | --- | --- |
| Toyota | AAA 999 | 2015-01 – 01T00:00:02.0000000Z |
| Nissan | ABC 369 | 2015-01 – 01T00:00:03.0000000Z |

**Rješenje**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Objašnjenje**: koristite izjavu LIKE da biste provjerili da vrijednost polja LicensePlate počinje slovom A zatim je bilo koji niz znakova nula ili više i završava 9. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Primjer upita: odredite logike za različite slučajeva/vrijednosti (SLUČAJA naredbe)
**Opis**: Navedite drugi izračuni za polje na temelju nekog kriterija.
Ako, na primjer, unesite opis niz za koliko automobilima proslijeđena od iste stvaranjem s posebno slučaja 1.

**Unos**:

| Provjerite | Vrijeme |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:03.0000000Z |

**Izlaz**:

| CarsPassed | Vrijeme |
| --- | --- | --- |
| 1 Honda | 2015-01 – 01T00:00:10.0000000Z |
| 2 Toyotas | 2015-01 – 01T00:00:10.0000000Z |

**Rješenje**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Objašnjenje**: uvjet u slučaju zatražit omogućuju drugi izračuni na temelju nekog kriterija (u našem slučaju broj s automobilima u prozoru zbrajanja).

## <a name="query-example-send-data-to-multiple-outputs"></a>Primjer upita: slanje podataka u više izlaza
**Opis**: slanje podataka u većem broju izlaz ciljnih web-mjesta iz jednog posla.
Ako, na primjer, analize podataka za upozorenje o utemeljen na prag i arhiviranje svih događaja sa spremištem blobova

**Unos**:

| Provjerite | Vrijeme |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Honda | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:03.0000000Z |

**Output1**:

| Provjerite | Vrijeme |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Honda | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:03.0000000Z |

**Output2**:

| Provjerite | Vrijeme | Count |
| --- | --- | --- |
| Toyota | 2015-01 – 01T00:00:10.0000000Z | 3 |

**Rješenje**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Objašnjenje**: U u uvjetu govori strujanje analize koje izlaze za zapisivanje podataka iz izjavu o zaštiti.
Prvi upit je prolazni podataka smo primili na Izlaz pod nazivom ArchiveOutput.
Drugi upit ne nekoliko jednostavnih zbrajanja i filtriranje i šalje rezultate do alerting sustav.
*Napomena*: rezultate CTEs (odnosno POMOĆU naredbe) možete koristiti i u višestruke izjave o izlaz – to je dodana prednost otvaranja manje čitateljima ulazni izvor.
Na primjer, 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Primjer upita: Brojanje jedinstvenih vrijednosti
**Opis**: Brojanje jedinstvenih vrijednosti polja koje se pojavljuju u toka unutar prozora vremena.
Na primjer, koliko jedinstvenih stvaranjem s automobilima prošli kroz štandu naknada u 2 drugi prozor?

**Unos**:

| Provjerite | Vrijeme |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Honda | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:03.0000000Z |

**Izlaz:**

| Count | Vrijeme |
| --- | --- |
| 2 | 2015-01 – 01T00:00:02.000Z |
| 1 | 2015-01 – 01T00:00:04.000Z |

**Rješenje:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Objašnjenje:** Moramo početne skupljanja da biste dobili jedinstveni čini s njihovim count iznad prozora.
Zatim ćemo učinite prikupljene koliko čini smo imate – navedene sve jedinstvene vrijednosti u prozoru dobiti isti vremenske oznake, a zatim drugi prozor zbrajanja treba minimalni ne aggregate 2 windows iz prvog koraka.

## <a name="query-example-determine-if-a-value-has-changed"></a>Primjer upita: Određivanje je li vrijednost promijenio#
**Opis**: Pogledajte prethodne vrijednosti da biste odredili ako se razlikuje od trenutnu vrijednost, na primjer, prethodni automobila na putu naknada isti neka bude trenutni automobila?

**Unos**:

| Provjerite | Vrijeme |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |

**Izlaz**:

| Provjerite | Vrijeme |
| --- | --- |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |

**Rješenje**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Objašnjenje**: korištenje KAŠNJENJA da biste nakratko u unos strujanje jedan događaj natrag i dobiti neka vrijednost. Zatim Usporedi s stvaranjem na trenutnom događaju i izlaz događaj ako se razlikuju.

## <a name="query-example-find-first-event-in-a-window"></a>Primjer upita: traži prvi događaja u prozoru
**Opis**: traži prvi automobila u svakom intervalu 10 minuta?

**Unos**:

| LicensePlate | Provjerite | Vrijeme |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07 – 27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07 – 27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07 – 27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07 – 27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07 – 27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07 – 27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07 – 27T00:13:45.0000000Z |

**Izlaz**:

| LicensePlate | Provjerite | Vrijeme |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07 – 27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07 – 27T00:12:02.0000000Z |

**Rješenje**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Sad ćemo provjerite promjena problem i pronaći prvi automobil od posebice u svakom intervalu 10 minuta.

| LicensePlate | Provjerite | Vrijeme |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07 – 27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07 – 27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07 – 27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07 – 27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07 – 27T00:13:45.0000000Z |

**Rješenje**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Primjer upita: pronalaženje posljednje događaja u prozoru
**Opis**: pronalaženje posljednje automobila u svakom intervalu 10 minuta.

**Unos**:

| LicensePlate | Provjerite | Vrijeme |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07 – 27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07 – 27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07 – 27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07 – 27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07 – 27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07 – 27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07 – 27T00:13:45.0000000Z |

**Izlaz**:

| LicensePlate | Provjerite | Vrijeme |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07 – 27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07 – 27T00:13:45.0000000Z |

**Rješenje**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Objašnjenje**: postoje dva koraka u upitu – prvoga pronalazi najnovije vremenske oznake u sustavu windows 10 minuta. Drugi korak spaja rezultate prvog upita s izvornog strujanje da biste pronašli događaja koji se podudaraju posljednje vremenske oznake u svakom prozoru. 

## <a name="query-example-detect-the-absence-of-events"></a>Primjer upita: otkriti Izostanak događaja
**Opis**: Provjerite ima li strujanje nijedna vrijednost koja odgovara određenim kriterijima.
Ako, na primjer, 2 uzastopnih automobilima iz iste stvaranjem uneseno cesta naknada 90 sekundi?

**Unos**:

| Provjerite | LicensePlate | Vrijeme |
| --- | --- | --- |
| Honda | ABC 123 | 2015-01 – 01T00:00:01.0000000Z |
| Honda | AAA 999 | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | ZADANI 987 | 2015-01 – 01T00:00:03.0000000Z |
| Honda | GHI 345 | 2015-01 – 01T00:00:04.0000000Z |

**Izlaz**:

| Provjerite | Vrijeme | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01 – 01T00:00:02.0000000Z | AAA 999 | ABC 123 | 2015-01 – 01T00:00:01.0000000Z |

**Rješenje**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Objašnjenje**: korištenje KAŠNJENJA da biste nakratko u unos strujanje jedan događaj natrag i dobiti neka vrijednost. Zatim Usporedi s stvaranjem na trenutnom događaju i izlaz događaj ako su jednaki i koristiti KAŠNJENJA za dohvaćanje podataka o prethodne automobila.

## <a name="query-example-detect-duration-between-events"></a>Primjer upita: otkriti trajanje između događaja
**Opis**: pronalaženje trajanje danu događaj. Na primjer, navedeni web clickstream odredite vrijeme utrošeno na značajku.

**Unos**:  
  
| Korisnik | Značajka | Događaja | Vrijeme |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Pokretanje | 2015-01 – 01T00:00:01.0000000Z |
| user@location.com | RightMenu | END | 2015-01 – 01T00:00:08.0000000Z |
  
**Izlaz**:  
  
| Korisnik | Značajka | Trajanje |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Rješenja**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Objašnjenje**: korištenje ZADNJE funkcija za dohvaćanje posljednja vrijednost vremena kada je vrsta događaja "Start". Imajte na umu da posljednje funkcija koristi PARTICIJE [korisnik] da biste naznačili rezultat će se izračunati po jedinstveni korisnički.  Upit sadrži 1 sat maksimalni prag za vrijeme razlika između 'Početka' i 'Stani' događaja, no može se konfigurirati kao potrebne (ograničenje DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Primjer upita: otkriti trajanja uvjeta
**Opis**: pronalaženje izlaz koliko došlo je do uvjeta za.
Na primjer, pretpostavimo koji pogrešku koja je rezultirala sve automobilima imate pogrešan Debljina (veće od 20 000 funtama) – rado bismo izračunati trajanje pogreške.

**Unos**:

| Provjerite | Vrijeme | Težina |
| --- | --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01 – 01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01 – 01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01 – 01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01 – 01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01 – 01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01 – 01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01 – 01T00:00:08.0000000Z | 2000 |

**Izlaz**:

| StartFault | EndFault |
| --- | --- |
| 2015-01 – 01T00:00:02.000Z | 2015-01 – 01T00:00:07.000Z |

**Rješenje**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Objašnjenje**: korištenje KAŠNJENJA prikaz ulaznog toka 24 sata i potražite instance gdje rastezati StartFault i StopFault po weight < 20000.

## <a name="query-example-fill-missing-values"></a>Primjer upita: Unesite vrijednosti koje nedostaju
**Opis**: za strujanje događaja koje sadrže vrijednosti koje nedostaju proizvesti strujanje događaja s pravilnim vremenskim razmacima.
Na primjer, generirati događaj svakih 5 sekundi koje će se prijaviti nedavno vide točke podataka.

**Unos**:

| t | vrijednost |
|--------------------------|-------|
| "2014.-01-01T06:01:00" | 1 |
| "2014.-01-01T06:01:05" | 2 |
| "2014.-01-01T06:01:10" | 3 |
| "2014.-01-01T06:01:15" | 4 |
| "2014.-01-01T06:01:30" | 5 |
| "2014.-01-01T06:01:35" | 6 |

**Izlaz (prvih 10 redaka)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014.-01 – 01T14:01:00.000Z | 2014.-01 – 01T14:01:00.000Z | 1 |
| 2014.-01 – 01T14:01:05.000Z | 2014.-01 – 01T14:01:05.000Z | 2 |
| 2014.-01 – 01T14:01:10.000Z | 2014.-01 – 01T14:01:10.000Z | 3 |
| 2014.-01 – 01T14:01:15.000Z | 2014.-01 – 01T14:01:15.000Z | 4 |
| 2014.-01 – 01T14:01:20.000Z | 2014.-01 – 01T14:01:15.000Z | 4 |
| 2014.-01 – 01T14:01:25.000Z | 2014.-01 – 01T14:01:15.000Z | 4 |
| 2014.-01 – 01T14:01:30.000Z | 2014.-01 – 01T14:01:30.000Z | 5 |
| 2014.-01 – 01T14:01:35.000Z | 2014.-01 – 01T14:01:35.000Z | 6 |
| 2014.-01 – 01T14:01:40.000Z | 2014.-01 – 01T14:01:35.000Z | 6 |
| 2014.-01 – 01T14:01:45.000Z | 2014.-01 – 01T14:01:35.000Z | 6 |

    
**Rješenje**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Objašnjenje**: ovaj upit će Generiraj događaje 5 sekundi i će izlaz zadnji događaj primljenog prije. [Skakutanje prozor] Trajanje (https://msdn.microsoft.com/library/dn835041.aspx "Skakutanje prozor – Azure strujanje analize") određuje do natrag upit će izgledati da biste pronašli najnovije događaja (300 sekundi u ovom primjeru).


## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
