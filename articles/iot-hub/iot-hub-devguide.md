<properties
 pageTitle="Teme vodič za razvojne inženjere za IoT koncentrator | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič koji uključuje IoT koncentrator krajnje točke, sigurnost, uređaj identiteta registra, upravljanje uređajima i poruka"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="azure-iot-hub-developer-guide"></a>Vodič za razvojne inženjere za Azure IoT koncentratora

Azure IoT koncentrator je servis za potpuno upravljane koji olakšava omogućiti pouzdanog i sigurne dvosmjerni komunikaciju između milijune IoT uređaji i završavaju aplikaciju natrag.

Azure IoT koncentrator omogućuje vam:

* Sigurne komunikacije pomoću po uređaj sigurnosne vjerodajnice i pristup kontrolu.
* Pouzdan uređaj u oblak i oblaka uređaj hyper skaliranje razmjene poruka.
* Povezivanje jednostavno uređaj s bibliotekama uređaj za najčešće korištene jezika i platformama.

Ovaj vodič za razvojne inženjere koncentrator IoT obuhvaća u sljedećim člancima:

- [Slanje i primanje poruka s koncentrator IoT] [ devguide-messaging] članku se opisuju razmjenu značajke (uređaj u oblak i oblaka uređaj) izlaže IoT koncentratora. Članak sadrži i informacije o temama kao što su oblika poruka i protokoli podržani komunikacije i brojeve priključaka koriste.
- [Prijenos datoteka s uređaja] [ devguide-upload] u članku se opisuje kako prenijeti datoteke s uređaja. Članak sadrži i informacije o temama kao što su obavijesti postupka uplaod možete poslati.
- [Upravljanja uređajima u središte IoT] [ devguide-identities] opisuje što informacija svaki IoT koncentrator uređaj identiteta registra pohranjuju te kako možete pristupiti i mijenjati.
- [Upravljanje pristupom koncentrator IoT] [ devguide-security] u članku se opisuje model sigurnosti koji se koriste za dopustiti pristup funkcija IoT koncentrator za oba uređaja i komponente u oblaku. Članak sadrži informacije o korištenju tokeni i X.509 certifikate i detalje o možete dodijeliti dozvole.
- [Korištenje twins uređaj da biste sinkronizirali stanje i konfiguracija] [ devguide-device-twins] opisuju koncept *twin uređaja* i funkcionalnosti izlaže kao što su sinkronizaciju uređaja s njegova twin. Članak sadrži informacije o podacima pohranjenima u uređaj twin.
- [Pozivanje metode izravno na uređaju] [ devguide-directmethods] u članku se opisuje životnim ciklusom Izravni metoda informacije o pozivanje metode na uređaju s aplikacijom pozadinske i obradu metodu izravno na uređaju.
- [Zakazivanje zadataka na više uređaja] [ devguide-jobs] u članku se opisuje kako mogu zakazivati zadatke na više uređaja. U članku se opisuje slanje zadataka koji se obavljati zadatke kao što su izvršavanje Izravni metode ažuriranje devcie pomoću devcie twin. Također se opisuje kako postaviti upit status zadatka.
- [Referenca - krajnje točke koncentrator IoT] [ devguide-endpoints] opisuju razne izlaže svaki IoT koncentrator za upravljanje i izvođenje operacija krajnje točke. Članak opisuje i kako možete koristiti pristupnik polja da biste omogućili neki uređaji za povezivanje s vašeg IoT koncentrator krajnje točke.
- [Referenca - jezika za upite za twins, postupcima i zadacima] [ devguide-query] u članku se opisuje tog jezika za upite koji omogućuje za dohvaćanje podataka iz vaše središte o uređaju twins i zadacima.
- [Referenca - kvotama i ograničavanje] [ devguide-quotas] navedene kvote u servis IoT koncentratora i regulacije ponašanje možete očekivati da biste vidjeli kada premašuju ograničenja.
- [Referenca - uređaja i servisa SDK-ovi] [ devguide-sdks] popise možete koristiti SDK-ovi razviti uređaja i servisa aplikacije kompatibilne sa koncentratora za IoT. Članak sadrži veze na mrežne dokumentacije API-JA.
- [Referenca - podršku IoT koncentrator MQTT] [ devguide-mqtt] pruža podrobne informacije o tome kako koncentrator IoT podržava protokol MQTT. Članak opisuje podrška za protokol MQTT ugrađeni u SDK-ovi i sadrži informacije o korištenju protokol MQTT izravno.
- [Pojmovnik] [ devguide-glossary] popis uobičajenih uvjete vezane uz IoT koncentratora.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

