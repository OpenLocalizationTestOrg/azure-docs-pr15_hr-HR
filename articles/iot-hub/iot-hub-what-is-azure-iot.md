<properties
 pageTitle="Azure rješenja za Internet stvari | Microsoft Azure"
 description="Pregled IoT na Azure uključujući arhitekturu rješenja za uzorak i njegovom Azure IoT koncentrator, uređaj SDK-ovi i konfiguriranog rješenja"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Daljnji koraci

Azure IoT koncentrator je Azure servis koji omogućuje siguran i pouzdan dvosmjerni komunikaciju između aplikacija sigurnosno završetka i milijune uređaja. Omogućuje aplikacije pozadinskih da biste:

- Primanje telemetrijskih na razini s uređaja.
- Usmjeravanje podatke s uređaja procesorom strujanje događaja.
- Primanje prijenosima datoteka s uređaja.
- Pošaljite oblaka uređaj naredbe određene uređaje.

Možete koristiti IoT koncentrator za implementaciju vlastiti za rješenje pozadinskih. Koncentrator IoT obuhvaća i registra za identitet uređaj za dodjeljivanje uređajima, njihove sigurnosne vjerodajnice i njihovih prava za povezivanje s središtu. Da biste saznali više o IoT koncentrator, pogledajte [što je IoT koncentrator?] [lnk-iot-hub].

Da biste saznali kako Azure IoT koncentrator omogućuje utemeljenih na standardima upravljanje uređajima IoT za daljinsko upravljanje, konfiguriranje i ažuriranje uređaja, potražite u članku [Pregled programa Azure IoT koncentratora za upravljanje uređajima][lnk-device-management].

Možete implementirati klijentske aplikacije na velik broj platforme uređaja hardver i operacijskim sustavima, možete pomoću uređaja IoT SDK-ovi. Uređaj IoT SDK-ovi obuhvaćaju biblioteke koje olakšavaju slanje telemetriju koncentratora za IoT i primanje naredbe oblak na uređaj. Kada koristite na SDK-ovi, možete odabrati iz nekoliko mrežnih protokola za komunikaciju s IoT koncentratora. Dodatne informacije potražite u članku [informacije o uređaju SDK-ovi][lnk-device-sdks].

Za početak pisanja koda za neke i pokretanje nekih uzoraka potražite u članku [Početak rada s IoT koncentrator] [ lnk-getstarted] vodič.

Možda će vas zanimati i [Azure IoT paket][lnk-iot-suite], što je zbirka konfiguriranog rješenja. Paket IoT omogućuje vam brzo počnite s radom i Skaliraj uobičajene scenarije IoT – kao što su udaljene nadzor, upravljanje resursa i predvidljivu održavanje projekata IoT.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md