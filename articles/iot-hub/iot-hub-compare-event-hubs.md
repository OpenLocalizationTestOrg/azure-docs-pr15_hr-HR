<properties
 pageTitle="Usporedba Azure IoT koncentrator s koncentratorima Azure događaj | Microsoft Azure"
 description="Usporedba servisa Azure IoT koncentratora i Azure događaj koncentratora isticanje funkcionalne razlike i korištenje slučajeva."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Usporedba Azure IoT koncentratora i koncentratora Azure događaja

Jedna od slučajevi glavni koristi za IoT koncentrator je prikupite telemetrijskih s uređaja. Zbog toga IoT koncentrator često uspoređuje s [Koncentratorima Azure događaj][]. Kao što je IoT središte koncentratora događaj je događaj obrade servis koji omogućuje događaj i telemetrijskih ingress u oblak na pretraživanje velikog skala s niske latencije i visoke pouzdanosti.

Međutim, servise imaju brojne razlike koje detaljno opisane u tablici u nastavku.

| Područje | IoT koncentratora | Događaj koncentratora |
| ---- | ------- | ---------- |
| Komunikacijskom | Omogućuje uređaj u oblak i oblaka uređaj razmjene poruka. | Omogućuje samo ingress događaja (obično smatra scenarije uređaja u oblaku). |
| Podrška za protokol za uređaje | Podržava MQTT, AMQP, AMQP nad WebSockets i HTTP-a. Uz to, IoT koncentrator surađuje [Azure IoT protokol pristupnika][lnk-azure-protocol-gateway], implementacije pristupnika u prilagodljive protokol za podršku prilagođene protokola. | Podržava AMQP, AMQP nad WebSockets i HTTP-a. |
| Sigurnost | Nudi po uređaju identitetu i kontrola revocable pristupa. Potražite u [odjeljku Sigurnost vodič IoT koncentrator za razvojne inženjere]. | Pruža događaj koncentratora razini [pravila zajednički pristup][Event Hubs - security], s ograničenim opoziva podržava putem [pravila programa publisher][Event Hubs publisher policies]. IoT rješenja često moraju implementirati Prilagođena rješenja za podršku po uređaj vjerodajnice i anti krivotvorenje mjere. |
| Operacije nadzora | Omogućuje IoT rješenja da biste se pretplatili bogatog skupa uređaj identiteta upravljanja i povezivanjem događaje kao što su pogreške provjere autentičnosti pojedinačni uređaj, ograničavanje i iznimke u ispravnom obliku. Ti događaji omogućuju brzo prepoznavanje problema s povezivanjem na razini pojedinačnih uređaja. | Izlaže zbrajanja metriku. |
| Promjena veličine | Optimizirana je za podršku milijune istodobno povezanih uređaja. | Podržava više ograničen broj istodobno veze – najviše 5000 AMQP veze, po [Bus servisa Azure kvote][]. S druge strane, događaj koncentratora omogućuje vam da biste odredili particije za svaku poruku poslali. |
| Uređaj SDK-ovi | Omogućuje [uređaj SDK-ovi] [ Azure IoT Hub SDKs] velike različite platforme i jezika. | Podržana za .NET i C. Omogućuje AMQP i HTTP slanje sučelja. |
| Prijenos datoteke | Omogućuje IoT rješenja za prijenos datoteka s uređaja s oblakom. Obuhvaća u datoteku obavijesti krajnja točka za Integracija tijeka rada i programa operacije nadzor kategorija za podršku za ispravljanje pogrešaka. | Koristi uzorak potvrdite zahtjeva za ručno zahtjev datoteke s uređaja i uređaje pružiti ključa za pohranu za transakcije. |

Ukratko, čak i ako je samo slova koristi ingress uređaja u oblak telemetrijskih IoT koncentrator omogućuje uslugu namijenjen isključivo za povezivanje IoT uređaja. On će i dalje da biste proširili propositions vrijednost za scenarija pomoću značajke specifične za IoT. Koncentratora događaj namijenjen je ingress događaj na skalu pretraživanje velikog i u kontekstu scenariji suč podatkovnog centra i intra Standard.

Nije neuobičajeno da biste koristili IoT koncentratora i koncentratora događaj u istom rješenju – gdje IoT koncentrator rukuje komunikacije uređaj u oblak i događaja koncentratora rukuje ingress kasnije faze događaja u stvarnom vremenu obrada modula.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o planiranju uvođenja IoT koncentrator, pročitajte članak [Promjena veličine, HA i DR][lnk-scaling].

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

[Azure događaj koncentratora]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Odjeljak sigurnost vodiča IoT koncentrator za razvojne inženjere]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure servisa Bus kvote]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
