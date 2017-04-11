<properties
 pageTitle="Vodič za predvidljivu održavanja | Microsoft Azure"
 description="Vodič predvidljivu održavanja unaprijed konfigurirane rješenja Azure IoT."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Vodič za predvidljivu održavanja unaprijed konfigurirane rješenja

## <a name="introduction"></a>Uvod

Rješenje predvidljivu održavanja unaprijed konfigurirane paket IoT je rješenje završetka do kraja za tvrtke scenarij u kojem se predviđa točke kada je vjerojatno će se pojaviti pogreška. Unaprijed konfigurirani rješenje doći koji koristite za aktivnosti kao što su optimiziranje održavanja. Rješenje kombinira ključa servisa Azure IoT paket, uključujući [Azure strojnog učenja] [ lnk_machine_learning] radnog prostora. Ovaj radni prostor sadrži eksperimenata, ovisno o skupu podataka javno uzorka za predviđanje u preostale korisni vijek (RUL) od mehanizam aircraft. Rješenje u potpunosti implementira scenarij IoT tvrtke kao početnu točku za planiranje i implementacija rješenja koje ispunjavaju određeni poslovni potrebama.

## <a name="logical-architecture"></a>Logička arhitekture

Na sljedećem su dijagramu navode logičke komponente konfiguriranog rješenja:

![][img-architecture]

Plavi stavke su Azure servise koji su dodjeli mjestu prilikom Dodjela konfiguriranog rješenja. Možete Dodjela konfiguriranog rješenja u regiji Istočni SAD-a, Sjeverna Europa ili istočnoazijski.

Nekoliko resursa nisu dostupne u područja gdje Dodjela konfiguriranog rješenja. Narančasta stavki u dijagramu predstavljaju servisa Azure dodjeli u najbliži dostupan regiji (Južnoafrička središnje NAM, zapadni Europe ili Jugoistočne Azije) dali odabranog područja.

Zelena stavka je Simulirani uređaj koji predstavlja mehanizam aircraft. Možete dodatne informacije o sljedećim Simulirani uređajima u sljedećem odjeljku.

Sivi stavke predstavljaju komponente koje implementirali mogućnosti *Administracija uređaja* . Trenutnom izdanju sustava rješenje predvidljivu održavanja unaprijed konfigurirane Dodjela ove resurse. Da biste saznali više o administraciji uređaj, pogledajte na [udaljenom unaprijed konfigurirana rješenja za nadzor][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simulirani uređaja

U unaprijed konfiguriranom rješenje Simulirani uređaj predstavlja mehanizam aircraft. Rješenje se nudi dva modula za mapiranje jedan aircraft. Svaki modul emits četiri vrste telemetrijskih: senzor 9, 11 senzor, 14 senzor, i 15 senzor omogućuju podatke potrebne za model strojnog učenja za izračun na ostale korisne vijek (RUL) za modul. Svaki Simulirani uređaj šalje sljedeće poruke telemetrijskih IoT koncentrator:

*Count ciklus*. Krugu predstavlja dovršene leta varijable duljine između 2-10 sati u kojima se telemetrijskih podataka hvata svakih pola sata tijekom na leta.

*Telemetrijskih*. Postoje četiri senzore koji predstavljaju atribute modul. Senzori označavaju generically senzor 9, 11 senzor, 14 senzor i senzor 15. Ove 4 senzori predstavljaju telemetrijskih potrebne da biste dobili korisne rezultate iz modela strojnog učenja za RUL. Ovaj model stvara se iz javne skupa podataka koji sadrži podatke o senzor realni modul. Dodatne informacije o načinu stvaranja modela iz izvorne skupa podataka potražite u odjeljku [Cortana Galerija predvidljivu održavanja predložak obavještavanje][lnk-cortana-analytics].

Simulirani uređaja možete učiniti sljedeće naredbe poslane iz koncentratora za IoT:

| Naredba | Opis |
|---------|-------------|
| StartTelemetry | Određuje stanje simulaciju.<br/>Pokreće uređaj slanje telemetrijskih     |
| StopTelemetry  | Određuje stanje simulaciju.<br/>Zaustavlja uređaj slanje telemetrijskih |

Koncentrator IoT nudi potvrdu naredba uređaja.

## <a name="azure-stream-analytics-job"></a>Azure analize toka posla

**Posao: Telemetrijskih** primjenjuju na dolazne strujanje telemetrijskih uređaj pomoću dva izraza. Prvi odabire sve telemetrijskih iz uređaja i šalje podatke u bloba pohranu gdje vizualizirati u web-aplikaciji. Druga naredba formula izračunava prosječnu senzor vrijednosti putem dvije minute klizača prozor i šalje te podatke u koncentratoru događaj **procesor događaj**.

## <a name="event-processor"></a>Procesor događaja

**Procesor događaj** uzima vrijednosti average senzor za dovršene ciklus. Je put uvježbavan u fazama te se vrijednosti za API-JA koji se izlaže strojnog učenja model da biste izračunali RUL za mehanizam.

## <a name="azure-machine-learning"></a>Azure strojnog učenja

Dodatne informacije o načinu stvaranja modela iz izvorne skupa podataka potražite u odjeljku [Cortana Galerija predvidljivu održavanja predložak obavještavanje][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Započnimo objašnjenje

U ovom se odjeljku vodit će vas kroz komponente rješenja, u članku se opisuje namjena slova i navode primjeri.

### <a name="predictive-maintenance-dashboard"></a>Nadzorna ploča predvidljivu održavanja

Ova stranica u web-aplikaciji koristi PowerBI JavaScript kontrole (potražite u članku [PowerBI signale spremište][lnk-powerbi]) za prikaz:

- Za izlazne podatke iz poslova strujanje analize u spremište blobova platforme.
- Brojanje RUL i ciklus po aircraft modul.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Opažanja ponašanje rješenje oblaka

Na portalu Azure pronađite grupu resursa pod nazivom rješenje ste odlučili da biste pogledali dodijeljenu resurse.

![][img-resource-group]

Kada dodijelite konfiguriranog rješenja, primit ćete poruku e-pošte s vezom na radnom prostoru strojnog učenja. Možete se kretati u radni prostor strojnog učenja iz [azureiotsuite.com] [ lnk-azureiotsuite] stranice za rješenje dodijeljenu kada je u stanju **spremno** .

![][img-machine-learning]

Na portalu za rješenja vidjet ćete da uzorka nudi četiri Simulirani uređaja za predstavljanje dva aircraft s dva modula po aircraft, svaka s četiri senzori. Kada prvi put otvorite centar za rješenja, simulacije se zaustaviti.

![][img-simulation-stopped]

Kliknite **Start simulacije** da biste započeli simulacije u kojem vidite povijest senzor RUL, ciklusa, i povijest RUL popunjavanje na nadzornoj ploči.

![][img-simulation-running]

Kada je RUL manje od 160 (do proizvoljne praga odabrali svrhe pokazni), portalu rješenje prikazuje upozorenje simbol uz prikaz RUL i ističe modul aircraft u žuta. Obratite pozornost na to kako su vrijednosti RUL Općenito prema dolje trend cjelokupan, ali obično uskoči prema gore i dolje. Takvo ponašanje rezultata s različitim ciklusa duljine i točnost modela.

![][img-simulation-warning]

Potpuna simulacije traje oko 35 minuta da biste dovršili 148 ciklusa. Praga 160 RUL ispunjen prvo na oko pet minuta i oba modula kliknite praga pri oko 8 minuta.

Simulacije traje kroz cijeli skup podataka 148 ciklusa i settles na konačne RUL i ciklus vrijednosti.

Simulacije bilo kojem trenutku možete prekinuti, ali kliknete **Start simulacije** replays simulacije od početka skupu podataka.

## <a name="next-steps"></a>Daljnji koraci

Sada se već pokretali rješenje predvidljivu održavanja unaprijed konfigurirane trebali biste ga izmijenite, potražite u članku [smjernice za prilagodbu konfiguriranog rješenja][lnk-customize].

Blogu [Paket IoT - u odjeljku u Napredne postavke - predvidljivu održavanja](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet pruža dodatne pojedinosti o predvidljivu održavanja unaprijed konfigurirane rješenja.

Možete istraživati i druge značajke i mogućnosti IoT paket unaprijed konfigurirane rješenja:

- [Najčešća pitanja o IoT paketa][lnk-faq]
- [Sigurnost IoT od dna prema gore][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
