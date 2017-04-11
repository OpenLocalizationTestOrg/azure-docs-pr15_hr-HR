<properties 
   pageTitle="Strujanje analize ograničenja tablice"
   description="U članku se opisuje ograničenja sustava i preporučene veličine za strujanje analize komponente i veze."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Ograničenje identifikator | Ograničenje       | Komentari |
|----------------- | ------------|--------- |
| Maksimalan broj jedinica strujanje po pretplati po regijama | 50 | Zahtjev za povećavanje strujanje jedinice za vašu pretplatu izvan 50 mogu se postaviti tako da se obratite se [Microsoftovoj službi za podršku](https://support.microsoft.com/en-us). |
| Maksimalna propusnost strujanje jedinica | 1MB / s * | Maksimalna propusnost po SU ovisi o scenarija. Stvarni propusnost može biti manji i ovisi o složenost upita i particija. Dodatne pojedinosti pronaći ćete u članku [Skaliranje Azure strujanje analize zadataka da biste povećali propusnost](../articles/stream-analytics/stream-analytics-scale-jobs.md) . |
| Maksimalni broj ulaza po poslu | 60 | Postoji teško ograničenje od 60 unosa po poslu strujanje analize. |
| Maksimalni broj izlaze po poslu | 60 | Postoji teško ograničenje od 60 izlaze po poslu strujanje analize. |
| Maksimalni broj funkcije po poslu | 60 | Postoji teško ograničenje od 60 funkcija po poslu strujanje analize. |
| Maksimalni broj zadataka po regijama | 1500 | Pretplate može imati najviše 1500 zadataka po zemljopisnim regijama. |