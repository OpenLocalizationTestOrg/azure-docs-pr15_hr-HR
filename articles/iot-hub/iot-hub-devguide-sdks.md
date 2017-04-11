<properties
 pageTitle="Vodič za razvojne inženjere - IoT koncentrator SDK-ovi | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – informacije o i veze na razne Azure IoT koncentrator uređaja i servisa SDK-ovi."
 services="iot-hub"
 documentationCenter=""
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

# <a name="iot-hub-sdks"></a>Koncentrator IoT SDK-ovi

## <a name="iot-hub-device-sdks"></a>Koncentrator IoT uređaj SDK-ovi

Uređaj Microsoft Azure IoT SDK-ovi sadrže kod koji olakšava sastavnih uređaji i aplikacije koje se povezati i upravlja servisa Azure IoT koncentratora.

Sljedeći uređaj IoT SDK-ovi su dostupni za preuzimanje iz GitHub:

- [Azure IoT uređaja SDK za C] [ lnk-c-device-sdk] pisane ANSI C (C99) za prenosivost i općenite platformu kompatibilnosti.
- [Azure IoT uređaja SDK za .NET][lnk-dotnet-device-sdk]
- [Azure IoT uređaja SDK za Java][lnk-java-device-sdk]
- [Azure IoT uređaja SDK za Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT uređaja SDK za Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] U odjeljku datoteke readme u spremištima GitHub informacije o korištenju jezika i upravitelji ovisne paketa da biste instalirali binarne datoteke i ovisnosti na vašem računalu razvoj.

## <a name="os-platforms-and-hardware-compatibility"></a>Kompatibilnost s operacijskim Sustavom platforme i hardvera

Dodatne informacije o SDK kompatibilnosti s određeni hardver uređajima potražite u članku [Azure Certificirano za katalog uređaj IoT][lnk-certified].

## <a name="iot-hub-service-sdks"></a>Koncentrator IoT servisa SDK-ovi

SDK-ovi servisa Microsoft Azure IoT sadržavati kod koji olakšava sastavnih aplikacije kompatibilne izravno s IoT koncentrator za upravljanje uređajima i sigurnost.

Servis sljedeće IoT SDK-ovi su dostupni za preuzimanje iz GitHub:

- [Azure IoT usluga SDK za .NET][lnk-dotnet-service-sdk]
- [Azure IoT usluga SDK za Node.js][lnk-node-service-sdk]
- [Azure IoT usluga SDK za Java][lnk-java-service-sdk]

> [AZURE.NOTE] U odjeljku datoteke readme u spremištima GitHub informacije o korištenju jezika i upravitelji ovisne paketa da biste instalirali binarne datoteke i ovisnosti na vašem računalu razvoj.

## <a name="azure-iot-gateway-sdk"></a>SDK Azure IoT pristupnika

U ovom SDK za Azure IoT pristupnika sadrži infrastrukturu i module za stvaranje IoT pristupnika rješenja. Možete proširiti SDK za stvaranje pristupnika prilagođene sve scenarij završetka do kraja.

Možete preuzeti [Azure IoT pristupnika SDK] [ lnk-gateway-sdk] iz GitHub.

## <a name="online-api-reference-documentation"></a>Mrežne dokumentacije referenca API-JA

Slijedi popis veza online API reference potražite u dokumentaciji za Azure IoT uređaj servisa te biblioteke pristupnika:

- [Internet stvari (IoT) .NET][lnk-dotnet-ref]
- [Koncentrator IoT OSTALE][lnk-rest-ref]
- [Azure IoT uređaja SDK za C][lnk-c-ref]
- [Azure IoT uređaja SDK za Java][lnk-java-ref]
- [Azure IoT usluga SDK za Java][lnk-java-service-ref]
- [Azure IoT uređaja SDK za Node.js][lnk-node-ref]
- [Azure IoT usluga SDK za Node.js][lnk-node-service-ref]
- [Azure IoT pristupnika SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Daljnji koraci

Druge teme referencu u ovom vodiču za razvojne inženjere IoT koncentrator obuhvaćaju sljedeće:

- [Koncentrator IoT krajnje točke][lnk-devguide-endpoints]
- [Jezik upita za twins, postupcima i zadacima][lnk-devguide-query]
- [Kvota i ograničavanje][lnk-devguide-quotas]
- [Podrška za MQTT IoT koncentratora][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md