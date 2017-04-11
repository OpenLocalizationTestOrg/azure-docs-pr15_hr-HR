<properties
 pageTitle="Predvidljivu održavanja unaprijed konfigurirane rješenje | Microsoft Azure"
 description="Opis predvidljivu održavanja unaprijed konfigurirane rješenja Azure IoT."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
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

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Pregled predvidljivu održavanja unaprijed konfigurirane rješenja

Rješenje *predvidljivu održavanja* unaprijed konfigurirane jedan je od [unaprijed konfiguriranom rješenja] [ lnk_preconfigured_solutions] objavio kao dio sustava [Microsoft Azure IoT paket][lnk_iot_suite]. Rješenje se integrira u stvarnom vremenu uređaj telemetrijskih zbirke predvidljivu model stvoren pomoću [Azure strojnog učenja][lnk_machine_learning].


Azure IoT paket, korporacije možete brzo i jednostavno povezivanje i praćenje imovine i analizirati podatke u stvarnom vremenu. Rješenje predvidljivu održavanja unaprijed konfigurirane uzima podatke i koristi obogaćenog nadzornih ploča i vizualizacija omogućuju tvrtke novi obavještavanje koji mogu pogon učinkovitosti i poboljšati strujanja prihod.

## <a name="the-scenario"></a>Scenarij

Fabrikam je regionalne avioprijevoznici koje usredotočuje se na sučelje sjajno klijenta na konkurencije cijene. Uzroku kašnjenja letova je održavanja problema i održavanje modul aircraft je osobito zahtjevne. Modul pogreška tijekom leta mora se na sve costs izbjegavati da Fabrikam redovito pregledava njegov modula i pridržava Održavanje programa. Međutim, aircraft modula ne uvijek uloga isto. Neke nepotrebne održavanja se izvodi na module. Više važno je napomenuti da, dođe do problema koji ground programa aircraft dok se izvodi održavanje. To uzrokuje kašnjenja skup, osobito ako je aircraft je na mjestu gdje desnom mehaničara ili rezervnih dijelova nisu dostupne.

Modula Fabrikam, aircraft su instrumented s senzori koje praćenje modul uvjeta tijekom leta. Fabrikam pomoću Azure IoT paket da biste prikupili podatke senzor koji se prikupljaju tijekom na leta. Nakon accumulating godina modul radu i podataka nije uspjelo, fizičari Fabrikam, podaci su katalog modeliran način za predviđanje u preostale korisni vijek (RUL) od mehanizam aircraft. Što su otkrili je korelacije između podataka iz četiri senzori modul s obućom modul koji vodi usmjerenog nije uspjelo. Dok Fabrikam Nastavi da biste izvršili običnog provjerama da biste bili sigurni sigurnost, je sada možete koristiti u modelima za izračunavanje RUL za svaki modul nakon svakog leta pomoću za telemetriju prikupljenih u modula tijekom na leta. Fabrikam sada možete predviđanje buduće točke pogreške i plan za održavanje i popravak unaprijed.

> [AZURE.NOTE] Model rješenja koristi stvarni modul obućom podatke.

Po procjenjivanje točke prilikom održavanja je potrebno, Fabrikam možete optimizirati operacije da biste smanjili troškove. Održavanje coordinators raditi s planiranje Planiranje održavanja odgovarajući pomoću programa aircraft zaustavljanja na određeno mjesto i osiguravanje dovoljno vremena za aircraft tako da bude iz servisa bez uzrokuje prekidu raspored. Fabrikam mogu zakazivati tehničare sukladno tome; osiguravanje aircraft su servisiran učinkovito bez vrijeme čekanja. Upravitelji kontrola primiti tarife održavanja da bi mogli Optimiziraj njihove poručivanja postupak i rezervni zaliha dijelove. Sve to omogućuje Fabrikam da biste minimizirali vrijeme početka projektirane aircraft i da biste smanjili operacijski troškovi štede sigurnost passengers i ekipa.

Da biste razumjeli kako [Azure IoT paket] [ lnk_iot_suite] pruža mogućnosti kupci morati shvatili potencijalni predvidljivu održavanja, pregledajte [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Kako se temelji rješenje predvidljivu održavanja

Rješenje upravlja ovaj dostupan kao predložak da biste prikazali ove mogućnosti rad s uređaja telemetrijskih prikupljene putem servisa IoT paket model Azure strojnog učenja. Microsoft je stvorio [model regresije] [ lnk_regression_model] od mehanizam aircraft i objavili dovršeno predložak podataka<sup>\[1\]</sup>, i detaljne upute za korištenje modela.

Predvidljivu održavanja unaprijed konfigurirane rješenja Azure IoT koristi ogledni model regresije stvorene iz ovog predloška može se implementiran u pretplatu za Azure te vidljiva kroz automatski generirani API-JA. Rješenje obuhvaća podskup testiranja podataka koji predstavlja 4 (od 100 ukupni) modula i 4 (od ukupne 21) strujanja podataka senzor koji sadrže točan rezultat obučeni modela.

*\[1\] A. Saxena i Goebel K. (2008). "Turbofan modul smanjene performanse simulacije skup podataka", spremište podataka Prognostics za NASA Ames (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), centar za istraživanje Ames NASA, Moffett polja, CA*

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o kako Azure IoT omogućuje predvidljivu održavanja scenariji, pročitajte [snimiti vrijednost iz Internet stvari][lnk_capture_value].

Iskoristite [vodič] [ lnk-predictive-walkthrough] predvidljivu održavanja unaprijed konfigurirane rješenja.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Možete istraživati i druge značajke i mogućnosti IoT paket unaprijed konfigurirane rješenja:

- [Najčešća pitanja o IoT paketa][lnk-faq]
- [Sigurnost IoT od dna prema gore][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
