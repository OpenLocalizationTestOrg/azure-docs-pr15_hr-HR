<properties
    pageTitle="Pregled paketa programa Microsoft Azure IoT | Microsoft Azure"
    description="Pregled načina Azure IoT paket nudi internet stvari konfiguriranog rješenja za prikupljanje analizu i pohrane podataka, navedite vizualizacije i integrirati s drugim sustavima."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Što je paket IoT Azure?

Azure internetske servise stvari (IoT) nudi širok raspon mogućnosti. Ove usluge ocjene enterprise omogućuju vam:

- Prikupljanje podataka s uređaja
- Analiza podataka strujanja u-kretanja
- Pohrana i velikih skupova podataka za upite
- Vizualizacija podataka u stvarnom vremenu i povijesne
- Integracija s pozadinske sustave

Izlaganje ove mogućnosti, Azure IoT programski paketi zajedno većem broju servisa Azure s prilagođene datotečnim nastavcima kao *konfiguriranog rješenja*. Ova konfiguriranog rješenja su osnovni implementacije uobičajene uzorke rješenje IoT koji vam pomoći da biste smanjili vrijeme poduzeti prilikom izlaganja vaš IoT rješenja. Korištenje [IoT softver razvojni paketi][lnk-sdks], možete prilagoditi i proširivanja tih rješenja da bi odgovarao s vlastitim potrebama. Ova rješenja možete koristiti i kao primjeri ili predložaka kada su razvoja novih IoT rješenja.

U sljedećem videozapisu naći Uvod u paketu Azure IoT:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT services u paketu IoT Azure

Unaprijed konfigurirani rješenja obično koriste sljedeći servisi:

- Temeljni u paketu IoT Azure je [Azure IoT koncentrator] [ lnk-iot-hub] servisa. Taj servis pruža mogućnosti razmjene poruka uređaj u oblak i oblaka uređaja i ponaša se kao pristupnik u oblak i drugih ključa IoT paket servisa. Servis omogućuje primanje poruka na uređajima na razini i poslati naredbe uređajima.

- [Azure analize strujanje] [ lnk-asa] pruža analize podataka u kretanja. Paket IoT upravlja taj servis obraditi dolazne telemetrijskih, izvođenje zbrajanja i otkriti događaja. Unaprijed konfiguriranom rješenja i pomoću analize strujanje obradu Informativna poruka koje sadrže podatke kao što su metapodataka ili naredbe Odgovori s uređaja. Rješenja pomoću analize strujanje obradu poruka s uređaja i isporučiti te poruke na druge servise.

- [Prostor za pohranu za Azure] [ lnk-azure-storage] i [Azure DocumentDB] [ lnk-document-db] pružiti mogućnosti pohrane podataka. Unaprijed konfigurirani rješenja pomoću spremište blobova platforme za pohranu telemetrijskih i ga učiniti dostupnim za analizu. Rješenja pomoću DocumentDB pohranjivati metapodatke uređaja i omogućivanje mogućnosti upravljanja na uređaju rješenja.

- [Azure web-aplikacije] [ lnk-web-apps] i [Microsoft Power BI] [ lnk-power-bi] pružiti mogućnosti vizualizaciju podataka. Fleksibilnost Power BI omogućuje vam brzo stvoriti vlastiti interaktivne nadzorne ploče koje koriste paket IoT podatke.

Da biste saznali arhitektura uobičajeni IoT rješenje potražite u članku [Microsoft Azure i Internet od stvari (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Unaprijed konfigurirani rješenja

IoT paket sadrži unaprijed konfiguriranom rješenja koja omogućuju vam da biste brzo početak rada s, a da biste istražili uobičajeni scenariji IoT, kao što su *udaljene nadzor* i *predvidljivu održavanja*, koji omogućuje Azure IoT paket. Možete uvesti ova rješenja Azure pretplatu, a zatim pokrenite IoT scenarij dovršen, krajnji do kraja.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste pregled paketa IoT što možete učiniti i koji su njegov glavne komponente, dodatne informacije o prethodno rješenja u paketu IoT, pročitajte članak [koji su Azure IoT unaprijed konfigurirane rješenja?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
