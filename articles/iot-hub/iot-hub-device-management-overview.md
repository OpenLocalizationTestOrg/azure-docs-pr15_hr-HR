<properties
 pageTitle="Koncentrator IoT uređaj upravljanje pregled | Microsoft Azure"
 description="Ovaj članak sadrži pregled upravljanja uređajima Azure IoT koncentrator: životni ciklus enterprise uređaj, ponovno pokrenite računalo, tvorničke Vrati izvorne postavke, Ažuriranje opreme, konfiguracije, twins uređaj, upita, zadacima"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Pregled upravljanja uređajima Azure IoT koncentrator (pretpregled)

## <a name="introduction"></a>Uvod

Azure IoT koncentrator nudi značajke i modelu proširenja koja omogućuju uređaja i pozadinske razvojnim programerima izraditi robusne rješenja za upravljanje uređajima IoT. IoT uređaja u rasponu od ograničenog senzori i microcontrollers jednu svrhu do Napredna pristupnika koje usmjeravanje komunikacije za grupe uređaja.  Uz to, korištenje slučajeve i preduvjeti za IoT operatora razlikuju se znatno preko delatnosti.  Bez obzira na ovo varijacije koncentrator IoT Azure upravljanja uređajima omogućuje mogućnosti, uzoraka i kod biblioteke da biste cater raznih Postavljanje uređaja i krajnjim korisnicima.

Presudne dio stvaranja uspješno enterprise IoT rješenja omogućuje Strategije za kako rukovati operatori daljnje upravljanje njihove zbirke uređaja. Operatori IoT zahtijevaju jednostavne i pouzdane alate i aplikacije koje omogućuju da biste fokus na više strateški aspekte svoje zadatke. Ovaj članak sadrži:

- Kratak pregled pristup Azure IoT koncentrator za upravljanje uređajima.
- Opis uobičajenih načela upravljanja uređaja.
- Opis životni ciklus uređaja.
- Pregled uobičajene uzorke upravljanje uređaja.

## <a name="iot-device-management-principles"></a>IoT uređaj upravljanje načela

IoT objedinjuje s njim jedinstvenog skupa izazove upravljanja uređajima i svaki rješenje enterprise predmete morate adresa na sljedeći način:

![Azure IoT koncentrator uređaj upravljanje načela grafiku][img-dm_principles]

- **Promjena veličine i Automatizacija**: IoT rješenja obavezan jednostavni Alati koji mogu automatizirati redovno zadatke i omogućivanje osoblje razmjerno malo operacije da biste upravljali milijune uređaja. Svakodnevno, operatori očekujete daljinski obraditi uređaju operacije, masovno i samo biti obaviješteni kada dođe do problema koji zahtijevaju njihove izravno obratiti pozornost.

- **Openness i kompatibilnost**: zajednici uređaja u IoT je extraordinarily raznih. Alati za upravljanje morate prilagoditi kako bi odgovarao više klase uređaja, platforme i protokoli. Operatori moraju imati mogućnost podržava razne vrste uređaja iz na većinu ograničenog ugrađene jedne proces se, Napredna i potpuno funkcionalni računalima.

- **Kontekst Informiranost**: IoT okruženja su dinamički i ikad promjene. Servis pouzdanosti je paramount. Operacija upravljanja na uređaju morate uzeti u SLA održavanja sustava windows, mreža i power Državama, uvjeta u koristi i uređaj geolokacija-a da biste bili sigurni nedostupnost te održavanje ne utječe na operacije kritične poslovne ili stvorite opasan uvjeta.

- **Servis mnogih uloga**: podrška za jedinstveni tijekovi rada i procesa IoT operacije uloga je presudne. Osoblje operacije harmoniously morate raditi na navedeni ograničenja Interna odjelima.  I morate pronaći otkrili načini plošni Realno vrijeme informacije o operacije uređaju nadređeni i druge tvrtke troškova uloge.

## <a name="iot-device-lifecycle"></a>Životni ciklus IoT uređaja

Postoji skup faze upravljanje Općenito uređaja koji su zajednički projekt IoT. U Azure IoT postoji pet faze unutar životnom ciklusu IoT uređaj:

![Pet faze životni ciklus Azure IoT uređaj: planiranje, Dodjela resursa, konfiguriranje, nadzor, povući][img-device_lifecycle]

Svako od tih pet faza, postoji nekoliko uređaja operator preduvjete koje mora ispuniti možete unijeti gotovo rješenje:

- **Planiranje**: Omogućivanje operatori da biste stvorili shemu za uređaj metapodataka koja omogućuje da jednostavno i točno upit, a ciljne skupine uređaji za skupno upravljanje operacije. Možete koristiti twin uređaj za pohranu metapodataka za uređaj u obliku oznake i svojstva.

    *Daljnje za čitanje*: [Početak rada s uređajem twins][lnk-twins-getstarted], [objašnjenje uređaj twins][lnk-twins-devguide], [kako koristiti twin svojstva][lnk-twin-properties]

- **Dodjela resursa**: sigurno Dodjela novim uređajima koncentrator IoT i omogućivanje operatori da biste odmah otkrili mogućnosti uređaja.  Koristiti registar koncentrator IoT uređaj da biste stvorili fleksibilne uređaj identiteta i vjerodajnice i obaviti ovaj postupak masovno pomoću posao. Stvaranje uređaja da biste prijavili njihove mogućnosti i uvjeta putem svojstva uređaja u twin uređaja.

    *Daljnje za čitanje*: [identiteta uređaj za upravljanje][lnk-identity-registry], [Upravljanje skupno identiteta uređaj][lnk-bulk-identity], [kako koristiti twin svojstva][lnk-twin-properties]

- **Konfiguriranje**: olakšali promjene konfiguracije skupno i ažuriranja opreme za uređaje uz zadržavanje stanja i sigurnost. Masovno izvršiti ove postupke za upravljanje uređajima pomoću željenog svojstva ili s Izravni metode i emitiranje zadacima.

    *Daljnje za čitanje*: [Izravni načina][lnk-c2d-methods], [pozovite Izravni metode na uređaju][lnk-methods-devguide], [kako koristiti svojstva twin][lnk-twin-properties], [Raspored i emitiranje poslove][lnk-jobs], [Raspored zadataka na više uređaja][lnk-jobs-devguide]

- **Monitor**: i praćenje cjelokupan uređaj stanja zbirke, status svakodnevnog, operatori problemima koji se možda će biti potrebno obratiti pažnju se upozorenje.  Primjena twin uređaj da biste omogućili uređaji izvješća Realno vrijeme operacijski uvjete i status operacija ažuriranja. Sastavljanje Napredna nadzorne ploče izvješća koja ponuditi najvažnija problema s korištenjem uređaja twin upita.

    *Daljnje za čitanje*: [kako koristiti svojstva twin][lnk-twin-properties], [jezika za upite za twins i zadacima][lnk-query-language]

- **Retire**: zamjena ili isključiti uređaja nakon neuspješne, Nadogradnja ciklusa ili na kraju servisa života.  Korištenje twin uređaj da biste zadržali informacije uređaja ako je uređaj fizički se zamjenjuje ili arhiviraju ako ne želite ponavljanje. Koristiti registar uređaj IoT koncentrator za sigurno opozivanje uređaj identiteta i vjerodajnice.

    *Daljnje za čitanje*: [kako koristiti svojstva twin][lnk-twin-properties], [Upravljanje uređaj identiteta][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>Koncentrator IoT uređaj upravljanje uzoraka

Koncentrator IoT omogućuje sljedeće skup uzoraka upravljanje uređaja.  [Vodiči za upravljanje uređajima] [ lnk-get-started] pokazuju detaljno produljivanje te uzorke da stane točno scenariju i kako dizajnirati novi uzorci na temelju tih core predložaka.

- **Ponovno** - pozadinskoj aplikacije obavještava uređaj Izravni metodom ga je pokrenuo ponovno pokretanje.  Uređaj koristi uređaj twin prijavljenih svojstva da biste ažurirali status nakon ponovnog pokretanja uređaja.

    ![Upravljanje uređajima Azure IoT koncentrator ponovno uzorak grafiku][img-reboot_pattern]

- **Vraćanje na tvorničke** - pozadinskoj aplikacije obavještava uređaj Izravni metodom ga je pokrenuo tvorničke Vrati izvorne postavke.  Uređaj koristi twin uređaj prijavljenim svojstva da biste ažurirali na tvorničke ponovno postavi status uređaja.

    ![Azure IoT koncentrator uređaj upravljanje tvorničke ponovno postavi grafiku uzorak][img-facreset_pattern]

- **Konfiguracija** – pozadinsku aplikaciju koristi twin želji svojstva uređaja da biste konfigurirali softver koji se izvodi na uređaju.  Uređaj koristi uređaj twin prijavljenih svojstva da biste ažurirali konfiguracije status uređaja.

    ![Azure IoT koncentrator uređaj upravljanje konfiguracije uzorak grafiku][img-config_pattern]

- **Ažuriranje opreme** - pozadinskoj aplikaciju obavještava uređaj Izravni metodom ga je pokrenuo ažuriranje opreme.  Uređaj pokreće multistep postupak preuzmite paket firmver, primijeniti paket firmver i na kraju se ponovno povezali sa servisom IoT koncentratora.  Tijekom postupka mult korak uređaj koristi uređaj twin prijavljenih svojstva da biste ažurirali tijek i status uređaja.

    ![Azure IoT koncentrator opremom upravljanje uređaja ažuriranje uzorak grafiku][img-fwupdate_pattern]

- **Izvješćivanje o tijeku i status** – aplikacije pozadinske pokreće upita twin uređaj, u skupu rezultata uređaja, da biste prijavili na stanje i napredak od akcija koji se izvode na uređaje.

    ![Azure upravljanja uređajima IoT koncentrator izvješćivanja napredak i stanje uzorak grafiku][img-report_progress_pattern]

## <a name="next-steps"></a>Daljnji koraci

Poslužite se mogućnostima, uzoraka i kod biblioteke koja omogućuje upravljanje uređajima Azure IoT koncentrator, da biste stvorili IoT aplikacije koje ispunjavaju preduvjeti za operator enterprise IoT unutar u svakoj fazi životnog ciklusa uređaja.

Daljnje informacije o značajki za upravljanje uređajima Azure IoT koncentrator, potražite u članku [Početak rada s upravljanja uređajima Azure IoT koncentrator] [ lnk-get-started] vodič.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md