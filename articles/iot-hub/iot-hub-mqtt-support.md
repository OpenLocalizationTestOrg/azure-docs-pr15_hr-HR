<properties
 pageTitle="Podrška IoT koncentrator MQTT | Microsoft Azure"
 description="Opis MQTT podržava u koncentratoru razini IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>Podrška za MQTT IoT koncentratora

Koncentrator IoT omogućuje uređaja možete komunicirati s IoT koncentrator krajnje točke uređaj pomoću [MQTT v3.1.1] [ lnk-mqtt-org] protokol priključak 8883 ili MQTT v3.1.1 putem protokola WebSocket na priključak 443. Koncentrator IoT zahtijeva svu komunikaciju uređaj da biste zaštiti putem TLS/SSL-a (dakle, IoT koncentrator ne podržava nesigurne veze putem priključka 1883).

## <a name="connecting-to-iot-hub"></a>Povezivanje s IoT koncentratora

Na uređaju možete koristiti MQTT protokol za povezivanje s korištenjem biblioteke u [Programu Microsoft Azure IoT SDK-ovi] koncentratora za IoT[ lnk-device-sdks] ili pomoću na MQTT protokol izravno.

## <a name="using-the-device-client-sdks"></a>Pomoću klijentskog uređaja SDK-ovi

[Uređaj klijent SDK-ovi] [ lnk-device-sdks] koji podržavaju protokol MQTT su dostupne za Java, Node.js, C, C# i Python. Klijentskog uređaja SDK-ovi koriste standardne niz za povezivanje IoT koncentrator uspostaviti vezu na IoT koncentrator. Da biste koristili protokol MQTT, protokol parametar klijenta mora biti postavljeno na **MQTT**. Prema zadanim postavkama klijentskog uređaja SDK-ovi povezati koncentratora za IoT s **CleanSession** zastavicu **0** i koristiti **QoS 1** za razmjenu poruka s središtu IoT.

Kada je uređaj priključen programa IoT koncentrator, klijentskog uređaja SDK-ovi sadrže metode koje omogućuju uređaj da biste poruke za slanje i primanje poruka iz centra za IoT.

Sljedeća tablica sadrži veze na primjere koda za svaki jezik podržani i određuje parametar da biste koristili uspostaviti vezu IoT koncentrator pomoću protokola MQTT.

| Jezik                   | Protokol parametra        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure-iot-uređaja – mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migracija aplikaciju za uređaj s AMQP MQTT
Ako koristite [uređaj klijent SDK-ovi][lnk-device-sdks], prijelaz s korištenjem AMQP za MQTT zahtijeva promjena protokola parametra u klijent za inicijalizaciju prema uputama iznad.

Kada to učinite, svakako provjerite sljedeće stavke:

* AMQP vraća pogreške za više uvjeta dok MQTT prekida vezu. Kao rezultat vaše obrade logike iznimki možda će biti potrebno neke promjene.
* MQTT ne podržava operacije *odbacivanje* prilikom primanja [poruka C2D][lnk-messaging]. Ako vaš pozadinskih treba dobivate odgovor u aplikaciji uređaj, razmislite o korištenju [Izravni metode][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Izravno pomoću protokola MQTT

Ako je uređaj ne možete koristiti klijentskog uređaja SDK-ovi, i dalje možete povezati krajnje točke javni uređaj pomoću protokola MQTT. U paketu **za POVEZIVANJE** uređaja trebali biste koristiti sljedeće vrijednosti:

- Za svako polje **ClientId** pomoću **deviceId**. 
- Polje **Username** u koristiti `{iothubhostname}/{device_id}`, pri čemu je {iothubhostname} cijelog CName od središta IoT.

    Ako, na primjer, ako je naziv vaše središte IoT **contoso.azure devices.net** i ako je naziv uređaja **MyDevice01**puni **korisničko ime** polje mora sadržavati `contoso.azure-devices.net/MyDevice01`.

- Za svako polje **lozinke** pomoću tokena SAS. Oblik tokena SAS je isti kao protokoli HTTP i AMQP:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Dodatne informacije o tome da biste generirali SAS tokeni potražite u članku u odjeljku uređaj [pomoću IoT koncentrator sigurnosnih tokena][lnk-sas-tokens].
    
    Kada testiranje, možete koristiti za [Uređaj Explorer] [ lnk-device-explorer] alat za brzo stvaranje tokena SAS koji možete kopirati i zalijepiti u vlastiti kod:
    
    1. Idite na karticu **Upravljanje** u programu Explorer uređaja.
    2. Kliknite **SAS tokena** (gornjem desnom kutu).
    3. Na **SASTokenForm**, odaberite svoj uređaj **DeviceID** padajući popis. Postavite **TTL**.
    4. Kliknite **Generiraj** da biste stvorili vaš token.
    
    SAS tokena koje generira ima sljedeću strukturu:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Dio ovaj token da biste koristili kao što je polje za **lozinku** da biste se povezali pomoću MQTT:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

MQTT povezivanje i ukloniti pakete, IoT koncentrator problemi događaja na kanalu **Nadzor operacije** s dodatnim informacijama koje olakšavaju otklanjanje problema s povezivanjem.

### <a name="sending-messages-to-iot-hub"></a>Slanje poruka IoT koncentrator

Nakon toga uspjelo povezivanje uređaja poruke možete slati IoT koncentrator pomoću `devices/{device_id}/messages/events/` ili `devices/{device_id}/messages/events/{property_bag}` kao **Naziv za temu**. Na `{property_bag}` element omogućuje slanje poruke s dodatna svojstva u obliku kodiran url-om uređaja. Ako, na primjer:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] To `{property_bag}` element koristi iste kodiranje kao nizovi upita u HTTP protokola.

Klijentska aplikacija uređaja možete koristiti `devices/{device_id}/messages/events/{property_bag}` kao na **će naziv tema** da biste definirali proslijediti kao poruku telemetrijskih *će poruke* .

Koncentrator IoT ne podržava poruke QoS 2. Ako klijent uređaj objavljuje poruku s **QoS 2**, IoT koncentrator zatvara mrežnu vezu.
Koncentrator IoT zadržava Zadrži poruke. Ako je uređaj šalje poruku zastavicom **ZADRŽI** postavite na 1, IoT koncentrator dodaje u **x-uključivanje-zadržati** svojstvo aplikacije na poruku. U ovom slučaju, umjesto persisting poruka Zadrži IoT koncentrator prosljeđuje je pozadinska aplikacija.

### <a name="receiving-messages"></a>Primanje poruka

Da biste primili poruke iz centra za IoT, trebali biste uređaj pretplata pomoću `devices/{device_id}/messages/devicebound/#` kao **Filtar temu**. Višerazinske zamjenskih **#** u filtru tema koristi se samo za Dopusti da biste primili dodatna svojstva u nazivu temu. Koncentrator IoT ne dopušta korištenje na **#** ili **?** Zamjenski znakovi za filtriranje podređene teme. Budući da IoT koncentratora nije opće namjene razmjenu broker pub sub, ga podržava samo nazivi dokumentirane tema i filtri temu.

Provjerite bilješke, a zatim da uređaj nećete primati poruke iz centra za IoT, prije nego što je uspješno pretplaćeni na njegov uređaj određene krajnje točke, predstavljeni u `devices/{device_id}/messages/devicebound/#` temu filtar. Kada je uspostavljena je uspješno pretplatu, uređaj će se pokrenuti primati samo oblak uređaja koje su poslane na njega nakon vrijeme pretplate. Ako je uređaj povezuje se s **CleanSession** zastavicu **0**, pretplata će se ista i preko različitih sesija. U ovom slučaju, kada se sljedeći put povezuje s **CleanSession 0** uređaj primit će preostala poruke koje su poslane na nju dok je prekinuta. Ako uređaj koristi **CleanSession** zastavice postavite na **1** , premda je nećete primati poruke iz centra za IoT dok pretplaćuje se na njegov uređaj krajnje.

Koncentrator IoT nudi poruke čiji je **Naziv za temu** `devices/{device_id}/messages/devicebound/`, ili `devices/{device_id}/messages/devicebound/{property_bag}` ako postoje neki svojstva poruke. `{property_bag}`sadrži kodiran url-om ključ/vrijednost parova svojstva poruke. Svojstva i svojstva moguće postaviti korisnika sustava (primjerice **messageId** ili **correlationId**) uključeni su u spremniku svojstava. Sustav svojstvo nazivi imaju prefiks **$**, svojstva aplikacije pomoću izvorni naziv svojstva bez prefiksa.

Kada klijentsko uređaj pretplaćuje se na temu s **QoS 2**, koncentrator IoT daje Maksimalna QoS razine 1 u paketu **SUBACK** . Nakon toga koncentrator IoT će isporuka poruka uređaja koji koristi QoS 1.

### <a name="additional-considerations"></a>Dodatne napomene

Kao konačan važna, po potrebi prilagodite ponašanje protokol MQTT na strani oblaka trebali biste provjeriti [Azure IoT protokol pristupnika] [ lnk-azure-protocol-gateway] koji omogućuje za implementaciju pristupnika visokih performansi prilagođene protokol koji sučelja izravno s IoT koncentratora. Protokol pristupnika Azure IoT omogućuje vam da biste prilagodili protokol uređaju kako bi odgovarao brownfield MQTT implementacijama ili drugi prilagođeni protokoli. Takvog potrebni, međutim, pokrenuti i raditi protokolom pristupnika.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u članku [bilješki na MQTT podržava] [ lnk-mqtt-devguide] u vodiču za Azure IoT koncentrator za razvojne inženjere.

Dodatne informacije o protokolu MQTT potražite u [dokumentaciji MQTT][lnk-mqtt-docs].

Da biste saznali više o planiranju uvođenja IoT koncentrator, pogledajte:

- [Azure potvrđenih za katalog IoT uređaja][lnk-devices]
- [Dodatni protokoli za podršku][lnk-protocols]
- [Usporedba s koncentratorima događaja][lnk-compare]
- [Promjena veličine, HA i DR][lnk-scaling]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
