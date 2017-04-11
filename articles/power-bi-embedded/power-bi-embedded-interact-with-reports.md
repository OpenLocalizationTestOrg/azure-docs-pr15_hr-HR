<properties
   pageTitle="Interaktivno izvješća pomoću JavaScript API | Microsoft Azure"
   description="Power BI ugrađene, interakciju s izvješća pomoću JavaScript API-JA"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Interakcija s izvješćima servisa Power BI pomoću JavaScript API-JA

JavaScript API za Power BI omogućuje vam da jednostavno ugrađivanje izvješćima servisa Power BI aplikacija. S API-jem, aplikacija programski možete raditi s izvješća elemente kao što su stranice i filtri. U ovom interaktivnosti čini izvješćima servisa Power BI integrirano dio aplikacije.

Ugrađivanje izvješća servisa Power BI u aplikaciji pomoću iframe koja se nalazi u sklopu aplikacije. Iframe ponaša se kao granicu naslova između aplikacija i izvješća, kao što je vidljivo na sljedećoj slici. 

![Power BI ugrađene iframe bez Javascript API-JA](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Iframe olakšava postupka ugrađivanje puno, ali bez JavaScript API izvješća i aplikacije ne možete raditi s međusobno. Nemaju interakcije možete učiniti mislite da je izvješće ne zaista dio aplikacije. Aplikacije i izvješća nisu potrebni za komunikaciju i obrnuto, kao na sljedećoj slici.

![Power BI ugrađene iframe s Javascript API-jem](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

JavaScript API za Power BI omogućuje vam da biste napisali kod koji se mogu sigurno prolaziti kroz granicu iframe. Time se omogućuje aplikacije programski izvrši neku akciju u izvješću, a da biste preslušali za događaje iz akcije koje korisnici napravili unutar izvješća.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Što možete učiniti s JavaScript API za Power BI?
Pomoću JavaScript API-JA možete upravljati izvješća, dođite do stranice u izvješću, filtriranje izvješća i rukovati ugrađivanje događaja. Sljedeći dijagram prikazuje strukturu u API-JA.

![Power BI JavaScript API dijagrama](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Upravljanje izvješćima
Javascript API omogućuje upravljanje ponašanjem na razini izvješća i stranice:

- Sigurno ugrađivanje Konkretno izvješće dodatka Power BI u aplikaciji – pokušajte [ugrađivanje pokazni videozapis aplikacije](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Postavljanje pristupa tokena
- Konfiguriranje izvješća
  - Omogućivanje i onemogućivanje okno filtra i okno navigacije po stranici – pokušajte [ažurirati postavke pokazni videozapis aplikaciju](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Postavljanje zadanih vrijednosti za stranice i filtri – pokušajte [postaviti zadane postavke pokazni videozapis](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Unos, a zatim izađite iz prikaza preko cijelog zaslona

[Dodatne informacije o ugrađivanje izvješća](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Dođite do stranice u izvješću
Enbales JavaScript API da biste otkrili sve stranice u izvješću i da biste postavili na trenutnoj stranici. Isprobajte [aplikaciju navigacijskoj pokazni videozapis](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Dodatne informacije o Navigacija stranicom](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Filtriranje izvješća
JavaScript API nudi osnovni i napredni filtriranja za ugrađene izvješća i stranice izvješća. Pokušajte [filtriranja pokazni videozapis aplikacije](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)i pregledajte neke uvodni šifru.  


#### <a name="basic-filters"></a>Osnovni filtri
Osnovni filtar nalazi se na razini stupca ili hijerarhije i sadrži popis vrijednosti za uključivanje ili isključivanje.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Dodatni filtri
Dodatni filtri korištenje logičkih operatora i ili ili, i prihvatite uvjete jednom ili dvjema, svaka s vlastite operator i vrijednost. Su podržani uvjeti:

- Ništa
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Sadrži
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Je
- IsNot
- IsBlank
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Dodatne informacije o filtriranju](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Rukovanje događajima
Osim slanja informacija u iframe, aplikacija možete primati podatke na sljedeći događaji koji dolaze iz iframe:

- Ugrađivanje
  - učitavanja
  - Pogreška
- Izvješća
  - pageChanged
  - dataSelected (uskoro dostupno)

[Dodatne informacije o rukovanje događajima](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o JavaScript API za Power BI, pogledajte sljedeće veze:

- [Wiki JavaScript API-JA](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Referenca modela objekta](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Uzorci
  - [Kutna](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Uživo pokazni videozapis](https://microsoft.github.io/PowerBI-JavaScript/demo/)
