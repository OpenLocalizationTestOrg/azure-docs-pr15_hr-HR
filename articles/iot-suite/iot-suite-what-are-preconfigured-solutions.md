<properties
 pageTitle="Azure IoT unaprijed konfigurirane rješenja | Microsoft Azure"
 description="Opis Azure IoT unaprijed konfigurirane rješenja i njihovih arhitektura s veze na dodatne resurse."
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

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Što su rješenja Azure IoT paket unaprijed konfigurirane?

Rješenja Azure IoT paket unaprijed konfigurirane su implementacije uobičajene uzorke IoT rješenje koje možete implementirati za Azure pomoću pretplate. Možete koristiti konfiguriranog rješenja:

- Kao početnu točku za vlastitu IoT rješenja.
- Dodatne informacije o uobičajene uzorke IoT rješenja dizajna i razvoj.

Svakog konfiguriranog rješenja je dovršena, krajnji do kraja implementacija koji koristi Simulirani uređaji za generiranje telemetrijskih.

Osim implementacija i radi rješenja u Azure, možete preuzeti šifru dovršeno izvora i zatim Prilagodba i proširivanje rješenje vašim potrebama IoT.

> [AZURE.NOTE] Da biste implementirali konfiguriranog rješenja, posjetite [Microsoft Azure IoT paket][lnk-azureiotsuite]. U članku [Početak rada s rješenja IoT unaprijed konfigurirane] [ lnk-getstarted-preconfigured] navedene su dodatne informacije o tome kako uvesti i pokretanje nekog od rješenja.

Sljedeća tablica prikazuje kako mapiranje određene značajke IoT rješenja:

| Rješenja | Ingestion podataka | Identitet uređaja | Naredba i kontrola | Pravila i akcije | Predvidljivu Analytics |
|------------------------|-----|-----|-----|-----|-----|
| [Udaljena nadzora][lnk-getstarted-preconfigured] | Da | Da | Da | Da | -   |
| [Predvidljivu održavanja][lnk-predictive-maintenance] | Da | Da | Da | Da | Da |

- *Ingestion podataka*: Ingress podataka na razini u oblak.
- *Identitet uređaj*: upravljanja jedinstveni svaki povezani uređaja.
- *Naredba i kontrola*: slanje poruka na uređaju iz oblaka uzrokuju uređaj da biste preuzeli neke akcije.
- *Pravila i akcije*: rješenje pozadinskih koristi pravila funkcioniralo na konkretne podatke s uređaja u oblak.
- *Predvidljivu analize*: primjenjuje rješenje pozadinskih analiziraju podaci uređaja u oblaku za predviđanje prilikom određenih akcija treba odvija. Na primjer, analiza aircraft modul telemetrijskih da biste odredili kada je održavanja modul.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Udaljena pregled praćenja unaprijed konfigurirane rješenja

Ne možemo odabrali ste raspravljati udaljene nadzora konfiguriranog rješenja u ovom članku jer je ilustrira mnoge uobičajene elemente dizajna koje dijele druga rješenja.

Sljedeći dijagram prikazuje ključni elementi udaljenom rješenja za nadzor. U nastavku se odjeljcima navode dodatne informacije o tih elemenata.

![Nadzor udaljenom unaprijed konfigurirane arhitekturu rješenja][img-remote-monitoring-arch]

## <a name="devices"></a>Uređaji

Ako pokrenete udaljene nadzora konfiguriranog rješenja, unaprijed Dodjela resursa u rješenje koje nikakvo uređaja kao zamjenu za su četiri Simulirani uređaji. Ti Simulirani uređaji imaju ugrađene temperature i humidity modelu koji emits telemetrijskih. Te Simulirani uređaje obuhvaćaju predočiti završetka do kraja protok podataka putem rješenje i omogućavaju praktičan izvor telemetrijskih i cilj naredbe ako ste pozadinske programer pomoću rješenja kao početnu točku za prilagođene implementacije.

Kada uređaj najprije povezuje IoT koncentrator udaljene nadzora konfiguriranog rješenje, na uređaju informacije poruke poslane središtu IoT zbraja popis naredbi koje možete odgovoriti na uređaju. Na udaljenom nadzora konfiguriranog rješenje naredbe su: 

- *Ping uređaj*: uređaj odgovori na ta se naredba pomoću programa potvrđivanje. To je korisno za provjeru uređaj je i dalje aktivne i listening.
- *Pokretanje Telemetrijskih*: upućuje uređaj da biste Početak slanja telemetrijskih.
- *Zaustavljanje Telemetrijskih*: upućuje uređaj prestanu slati telemetrijskih.
- *Promjena Postavi točku temperatura*: određuje vrijednosti telemetrijskih Simulirani temperatura šalje uređaj. To je korisno za testiranje pozadinsku logiku.
- *Dijagnostičke Telemetrijskih*: određuje ako uređaj treba slati vanjskim temperatura kao telemetrijskih.
- *Promjena uređaja stanje*.: postavlja metapodataka svojstvo Stanje uređaja koji uređaj izvješća. To je korisno za testiranje pozadinsku logiku.

Uređaji za Simulirani možete dodati rješenje koje šalji iste telemetrijskih poruka i odgovaranje na iste naredbe. 

## <a name="iot-hub"></a>IoT koncentratora

U unaprijed konfiguriranom rješenja, instancu IoT koncentrator odgovara *Oblaka pristupnika* u standardne [arhitekturu rješenja IoT][lnk-what-is-azure-iot].

Koncentratora za IoT prima telemetrijskih s uređaja na jednom krajnjoj točki. Koncentratora za IoT održava i uređaja određene krajnje točke gdje svaki uređaja možete dohvatiti naredbama koje se šalju na njega.

Koncentrator IoT primljene telemetrijskih postaje dostupan putem telemetrijskih davatelj usluge čitati krajnje točke.

## <a name="azure-stream-analytics"></a>Azure strujanje Analytics

Unaprijed konfiguriranom rješenje koristi tri [Azure strujanje analize] [ lnk-asa] poslovi (ASA) da biste filtrirali strujanje telemetrijskih iz uređaja:


- *Zadatak DeviceInfo* - proizvodi podataka koncentratora za događaj koji usmjerava uređaj Registracija određene poruke, šalje prilikom povezivanja s uređaja najprije ili kao odgovor na naredbu **Promjena uređaja stanja** u registru rješenje uređaj (DocumentDB baze podataka). 
- *Zadatak za telemetriju* - šalje sve neobrađenog telemetrijskih blobova platforme Azure za pohranu Hladno i izračunava telemetrijskih agregacije koje se prikazuju na nadzornoj ploči rješenja.
- *Pravila posla* - filtrira strujanje telemetrijskih vrijednosti koje premašuju bilo koje pravilo pragovi i podatke za koncentratora za događaj. Kada aktivira pravilo, prikaz portala nadzorne ploče rješenja prikazuje ovaj događaj kao novi redak u tablici povijesti alarma i pokreće akciju, ovisno o postavkama definirani prikazi pravila i akcije na portalu rješenja.

U unaprijed konfiguriranom rješenja, zadacima ASA obrasca dio **IoT rješenje sigurnosno završetka** u standardne [arhitekturu rješenja IoT][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Procesor događaja

U unaprijed konfiguriranom rješenja, procesor događaja obrazaca dio **IoT rješenje sigurnosno završetka** u standardne [arhitekturu rješenja IoT][lnk-what-is-azure-iot].

Poslovi **DeviceInfo** i ASA **pravila** poslati svoje izlaz događaj koncentratora za isporuku druge servise pozadinskih. Rješenje koristi [EventPocessorHost] [ lnk-event-processor] instancu koji se izvodi u [WebJob][lnk-web-job], čitati poruke iz koncentratora te događaj. **EventProcessorHost** koristi podatke **DeviceInfo** da biste ažurirali podatke uređaj u bazi podataka DocumentDB i koristi podatke **pravila** za pozivanje logike app i ažuriranje upozorenja prikazati na portalu rješenja.

## <a name="device-identity-registry-and-documentdb"></a>Uređaj identiteta registra i DocumentDB

Svaki koncentrator IoT obuhvaća [uređaj identiteta registra] [ lnk-identity-registry] koji pohranjuje ključeve za uređaj. Koncentrator IoT koristi taj podatak autentičnost uređaji – uređaj mora biti registriran, a imate valjani ključ prije nego što se može povezati središtu.

Ovo je rješenje sprema dodatne informacije o uređajima kao što su njihovo stanje, naredbe koje oni podržavaju i drugih metapodataka. Rješenje koristi DocumentDB baze podataka radi pohrane podataka ovaj uređaj specifične za rješenja i rješenje portal dohvaća podatke iz ove DocumentDB baze podataka za prikaz i uređivanje.

Rješenje morate zadržati podatke u registru identiteta uređaj sinkronizirati sa sadržajem DocumentDB baze podataka. **EventProcessorHost** koristi podatke iz **DeviceInfo** strujanje analize posla da biste upravljali sinkronizacije.

## <a name="solution-portal"></a>Portal za rješenja

![Rješenje nadzorne ploče][img-dashboard]

Portal za rješenja je koji se temelji na web Sučelja koji je implementiran u oblak kao dio konfiguriranog rješenja. Omogućuje vam:

- Prikaz povijesti telemetrijskih i alarma na nadzornoj ploči.
- Dodjela nove uređaje.
- Upravljanje i nadzirati uređaja.
- Pošaljite naredbe određene uređaje.
- Upravljanje pravilima i akcije.

U unaprijed konfiguriranom rješenja, portalu rješenje oblika dio **IoT rješenje sigurnosno završetka** i dio **obrade i povezivanje s poslovnim** uobičajeni [arhitekturu rješenja IoT][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o arhitekturi rješenje IoT potražite u članku [servisa Microsoft Azure IoT: referenca arhitektura][lnk-refarch].

Sada znate koje konfiguriranog rješenje je, možete dobiti započeo implementacija rješenja *udaljene nadzor* unaprijed konfigurirane: [Početak rada s konfiguriranog rješenja][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md