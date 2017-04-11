<properties
 pageTitle="Azure IoT koncentrator skaliranje | Microsoft Azure"
 description="U članku se opisuje skaliranje Azure IoT koncentratora."
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
 ms.date="09/19/2016"
 ms.author="elioda"/>

# <a name="scaling-iot-hub"></a>Promjena veličine IoT koncentratora

Azure IoT koncentrator podržavaju do milijuna istodobno povezanih uređaja. Dodatne informacije potražite u članku [IoT koncentrator cijene][lnk-pricing]. Svaka jedinica IoT koncentrator omogućuje određeni broj dnevnih poruke.

Pravilno skalirali rješenje, razmislite o određenom korištenje IoT koncentratora. Posebno razmotriti propusnost Vršna potrebni za sljedeće kategorije operacije:

* Uređaj u oblak poruke
* Oblak uređaj poruke
* Operacija registra za identitet

Osim tih propusnost potražite u članku [IoT koncentrator kvotama i Reguliranje][] i dizajniranje rješenje sukladno tome.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Propusnost uređaj u oblak i oblaka uređaj poruke

Najbolji je način veličina rješenja programa IoT koncentrator je da biste procijenili promet na temelju po jedinici.

Uređaj u oblak poruke slijedite ove smjernice osigurale trajne propusnost.

| Razina | Osigurale trajne propusnost | Stopa osigurale trajne Pošalji |
| ---- | -------------------- | ------------------- |
| S1 | Najviše 1111 KB/minutni po jedinici<br/>(1,5 GB/dan/jedinice) | Aritmetička sredina 278 poruke/minutni po jedinici<br/>(400,000 poruke/dan po jedinici) |
| S2 | Do 16 MB/minute po jedinici<br/>(22.8 GB/dan/jedinice) | Prosjek 4167 poruke/minute po jedinici<br/>(6 milijuna poruke/dan po jedinici) |
| S3 | Najviše 814 MB/minute po jedinici<br/>(1144.4 GB/dan/jedinice) | Prosjek 208,333 poruke/minute po jedinici<br/>(300 milijuna poruke/dan po jedinici) |

## <a name="identity-registry-operation-throughput"></a>Operacija propusnost registra za identitet

Koncentrator IoT identiteta registra operacije ne bi biti izvođenje operacije, kao što je uglavnom odnose na uređaju dodjele resursa.

Brojevi performanse određene neprekidnog rada, potražite u članku [IoT koncentrator kvote i Reguliranje][].

## <a name="sharding"></a>Sharding

Dok je na jednom koncentrator IoT mogu mijenjati veličinu milijune uređaja, ponekad rješenje potreban je određeni performanse karakteristika jedan IoT koncentrator ne jamči. U tom slučaju preporučuje se u više IoT koncentratora particija uređajima. Više IoT koncentratora glačanje bursts promet i dobiti potrebne propusnost ili stope postupak koji su potrebni.

## <a name="next-steps"></a>Daljnji koraci

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[Koncentrator IoT kvotama i Reguliranje]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
