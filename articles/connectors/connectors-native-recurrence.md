<properties
    pageTitle="Dodavanje ponavljanja okidača u aplikacijama logike | Microsoft Azure"
    description="Pregled okidača ponavljanje i kako je koristiti aplikaciju Azure logike."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Početak rada s okidača ponavljanja

Pomoću okidača ponavljanja možete stvoriti Napredna tijekova rada u oblaku.

Na primjer, možete:

- Zakazivanje tijeka rada za izvođenje SQL pohranjene procedure svaki dan.
- E-pošte sažetak svih tweets prošli tjedan o određenim oznaku.

Početak korištenja okidača ponavljanje u aplikaciji logike potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Koristite okidač ponavljanja

Okidač je događaja koji se mogu koristiti za pokretanje tijeka rada koji je definiran u aplikaciji logike. [Dodatne informacije o okidača](connectors-overview.md).

Evo primjera slijeda kako postaviti ponavljanje okidača u aplikaciji logika:

1. Dodavanje **ponavljanja** okidača u prvom koraku u aplikaciji logike.
2. Unesite parametre za intervala ponavljanja.

Aplikaciju logike pokreće se odmah Pokreni nakon svakog vremensko razdoblje.

![Pokretanje HTTP](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Detalji o okidača

Ponavljanje okidača ima sljedeća svojstva koja možete konfigurirati.

Nakon što u određenom vremenskom intervalu ga aktivira se logike aplikacije.
A * znači da je obavezno polje.

|Zaslonsko ime|Naziv svojstva|Opis|
|---|---|---|
|Učestalost *|FREQUENCY|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Interval *|Interval|Interval navedeni učestalosti za ponavljanje.|
|Vremenska zona|Vremenska zona|Ako je vrijeme početka bez UTC pomak, koristit će se ovaj vremensku zonu.|
|Vrijeme početka|startTime|Vrijeme početka u [obliku ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## <a name="next-steps"></a>Daljnji koraci

Sada, isprobajte platforme i [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md). Druge dostupne poveznika u aplikacijama logike možete istražiti tako da pogledate našem [popisu API-ji](apis-list.md).
