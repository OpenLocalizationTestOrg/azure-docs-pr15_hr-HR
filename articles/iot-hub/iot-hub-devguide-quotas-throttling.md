<properties
 pageTitle="Vodič za razvojne inženjere - kvotama i ograničavanje | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – opis kvote koji se odnose na središte IoT i regulacije očekivano"
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

# <a name="reference---quotas-and-throttling"></a>Referenca - kvotama i ograničavanje

## <a name="quotas-and-throttling"></a>Kvota i ograničavanje

Azure pretplate može sadržavati najviše 10 IoT koncentratora.

Svaki IoT koncentrator je tamo uz određeni broj jedinica u određenim SKU (Dodatne informacije potražite u članku [Cijene za Azure IoT koncentrator][lnk-pricing]). SKU i broj jedinica odredite maksimalni dnevni kvote poruka koje šaljete.

Se SKU određuje i regulacije ograničenja koja IoT koncentrator primjenjuje na sve operacije.

## <a name="operation-throttles"></a>Reguliranje operacije

Reguliranje operacija su stopa ograničenja koja se primjenjuju minute rasponi i namijenjeni da biste izbjegli zloupotrebu. Koncentrator IoT pokušava izbjegli vraćanje pogreške kad god je moguće, ali se pokreće vraćanje iznimke ako je Reguliranje bi predugo.

Slijedi popis nametnuti Reguliranje. Vrijednosti odnose se na pojedinačne koncentratora.

| Ograničenja | Vrijednost po koncentratora |
| -------- | ------------- |
| Identitet registra operacija (Stvaranje, dohvatiti, popisa, ažuriranje, brisanje) | 5000/min/jedinica (za S3) <br/> 100/min/jedinica (za S1 i S2). |
| Veza na uređaju | 6000/sec/jedinica (za S3), 120/sec/jedinicu (za S2), 12/sec/jedinicu (za S1). <br/>Najmanji od 100/sec. <br/> Na primjer, dva S1 jedinice su 2\*12 = 24/sec, ali ste barem 100/sec preko vaše jedinice. S devet S1 jedinice, imate 108/sec (9\*12) preko vaše jedinice. |
| Šalje uređaj u oblak | 6000/sec/jedinica (za S3), 120/sec/jedinicu (za S2), 12/sec/jedinicu (za S1). <br/>Najmanji od 100/sec. <br/> Na primjer, dva S1 jedinice su 2\*12 = 24/sec, ali ste barem 100/sec preko vaše jedinice. S devet S1 jedinice, imate 108/sec (9\*12) preko vaše jedinice. |
| Šalje oblaka uređaja | 5000/min/jedinica (za S3), 100/min/jedinica (za S1 i S2). |
| Oblak uređaj Prima | 50000/min/jedinica (za S3), 1000/min/jedinica (za S1 i S2). |
| Postupci za prijenos datoteka | 5000 datoteke prenesite obavijesti/min/jedinice (za S3), 100 datoteka prijenos obavijesti/min/jedinica (za S1 i S2). <br/> 10000 SAS ji može biti izgleda za račun za Azure pohranu odjednom.<br/> 10 SAS ji/uređaj može biti izvan odjednom. | 

Važno je da pojašnjavaju Reguliranje *veze uređaja* određuje je brzina kojom možete nove veze uređaja uspostaviti s koncentratora za IoT, a ne maksimalni broj istodobno povezanih uređaja. Reguliranje ovisi o broju jedinica koja vam je dodijeljena središtu.

Ako, na primjer, ako kupite jedna jedinica S1 dobit ograničenja 100 veza u sekundi. To znači da povezati 100 000 uređaje potrebno najmanje 1000 sekundi (približno 16 minuta). Međutim, imate proizvoljan broj istodobno povezanim uređajima kao što su uređaji registriran u registru za identitet vaš uređaj.

Detaljan od središta IoT ograničavanje ponašanje, potražite u članku na blogu [IoT koncentrator ograničavanje i][lnk-throttle-blog].

>[AZURE.NOTE] U bilo kojem trenutku, moguće je da biste povećali kvote ili ograničenja ograničenja povećavanjem broja dodijeljenu jedinice u koncentratora za IoT.

>[AZURE.IMPORTANT] Identitet registra operacije su namijenjene izvođenju upravljanje uređajima i scenariji za dodjelu resursa. Čitanje ili ažuriranje velik broj uređaja identiteta podržan je kroz [Uvoz i izvoz poslove][lnk-importexport].

## <a name="next-steps"></a>Daljnji koraci

Druge teme referencu u ovom vodiču za razvojne inženjere IoT koncentrator obuhvaćaju sljedeće:

- [Koncentrator IoT krajnje točke][lnk-devguide-endpoints]
- [Jezik upita za twins, postupcima i zadacima][lnk-devguide-query]
- [Podrška za MQTT IoT koncentratora][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md