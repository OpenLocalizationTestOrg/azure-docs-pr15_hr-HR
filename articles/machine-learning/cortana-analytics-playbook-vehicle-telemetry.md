<properties 
    pageTitle="Vozila telemetrijskih analize rješenje playbook | Microsoft Azure" 
    description="Korištenje mogućnosti programa obavještavanje Cortana da bi se dobio u stvarnom vremenu i predvidljivu uvida na vozila stanja i vožnju navike." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Vozila telemetriju analize rješenje playbook

U ovom veze prema **izborniku poglavlja u ovom playbook** . 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Pregled
Super računala prijeđete iz na Laboratorija i su Grupirani stupčasti grafikon u našem garaže! Ove najnovije automobiles sadrže myriad senzora, daje im omogućuje praćenje i praćenje milijune događaje u sekundi. Ne možemo očekivati da po 2020, većinu tih automobilima će imati su povezani s Internetom. Zamislite dodirom u ovom mnoštvo podataka možete unijeti najbolje u predmete sigurnost, pouzdanosti i sučelje za vožnju! Microsoft učinio to si stvarnosti s obavještavanje Cortana.

Microsoftovi Cortana je potpuno upravljanih velikih skupova podataka i naprednom analitikom paket koji omogućuje pretvaranje podataka u Inteligentna akcija. Želimo vam predstavljanje Cortana obavještavanje vozila Telemetrijskih analize rješenje predložak. Ovo je rješenje pokazuje kako dealerships automobila, kupnju proizvođači i osiguranja tvrtke mogu koristiti mogućnosti obavještavanje Cortana da bi se dobio u stvarnom vremenu i predvidljivu uvida na vozila stanja i vožnju navike. 

Rješenje se implementira kao [lambda arhitektura uzorak](https://en.wikipedia.org/wiki/Lambda_architecture) prikazuje potpunu potencijalne od Cortana obavještavanje platformu za u stvarnom vremenu i skupna obrada. Rješenje: 

- omogućuje Telematics vozila simulator
- upravlja događaj koncentratora za ingesting milijune Simulirani vozila telemetrijskih događaje u Azure 
- koristi strujanje analize da bi se dobio uvida u stvarnom vremenu na stanje vozila
-  Nastavi podatke u Dugoročne prostora za pohranu za bogatiji obrade analize. 
- uzima prednost strojnog učenja za značajkom prepoznavanja u stvarnom vremenu i skupna obrada da bi se dobio predvidljivu uvide.
- upravlja HDInsight za pretvaranje podataka na razini i tvorničke podataka za rukovanje djelovanje, Planiranje, Upravljanje resursima i nadzor kanala skupna obrada 
- daje rješenja obogaćenog nadzorne ploče za podatke u stvarnom vremenu i vizualizacija predvidljivu analize pomoću dodatka Power BI

## <a name="architecture"></a>Arhitektura

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Slika 1 – arhitekturu rješenja za vozila Telemetrijskih Analytics*

Ovo rješenje sadrži sljedeće **komponente obavještavanje Cortana** i showcases njihove Integracija end da biste završetka


- **Događaj koncentratora** za ingesting milijune vozila telemetrijskih događaje u Azure.
- **Strujanje analize** pokušavaju uvida u stvarnom vremenu na stanje vozila i nastavi tih podataka u Dugoročne prostora za pohranu za bogatiji obrade analize.
- **Strojnog učenja** za otkrivanje značajkom u stvarnom vremenu i skupna obrada da bi se dobio predvidljivu uvide.
- **HDInsight** je leveraged za pretvaranje podataka na razini
- **Tvorničke podataka** rukuje djelovanje, Izrada rasporeda, Upravljanje resursima te praćenje obrade kanala.
- **Power BI** daje rješenja obogaćenog nadzorne ploče za podatke u stvarnom vremenu i vizualizacije predvidljivu analize.

Ovo je rješenje pristupa dvije različite **izvore podataka**: 

- **Simulirani signale vozila i dijagnostici**: simulator za telematics vozila emits dijagnostičke informacije i signale koji odgovaraju stanje vozila i vožnju uzorak u određenom trenutku u vremenu. 
- **Vozila kataloga**: dataset reference koje sadrže VIN za mapiranje modela.
