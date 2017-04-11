<properties
     pageTitle="Konfiguriranje prijenosa datoteka pomoću portala za Azure | Microsoft Azure"
     description="Pregled uputa za konfiguriranje prijenosa datoteka pomoću portala za Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Konfiguriranje prijenosima datoteka pomoću portala za Azure

## <a name="file-upload"></a>Prijenos datoteke

Da biste koristili [datoteke prenesite funkcionalnost u središte IoT][lnk-upload], najprije morate povezati račun za Azure pohranu sa vaše središte. Odaberite postavke **prijenos datoteka** da bi se prikazao popis svojstava za prijenos datoteka za središtu IoT koji se mijenjaju.

![][13]

**Spremnik za pohranu**: pomoću portala za Azure kontejner blob u račun za Azure pohranu u trenutne pretplate Azure želite pridružiti koncentratora za IoT. Ako je potrebno, možete stvoriti račun za Azure prostora za pohranu na plohu i blob spremnik **račune za pohranu** na plohu **spremnika** . Koncentrator IoT automatski generira SAS ji s dozvolama za pisanje u ovom spremniku blobova platforme za uređaje da biste koristili prilikom prijenosa datoteka.

![][14]

**Primanje obavijesti o prenesenih datoteka**: Omogućivanje i onemogućivanje prijenosima datoteka putem klizač stavke.

**SAS TTL**: Ta je postavka u vrijeme važenja od ji SAS vratio koncentrator IoT na uređaju. Postavite na jedan sat po zadanom, no može se prilagoditi radi druge vrijednosti pomoću klizača.

**Obavijest o datoteke postavke zadani TTL**: U vrijeme važenja datoteke prenijeti obavijest da bi je istekao rok. Postavite na jedan dan po zadanom, no može se prilagoditi radi druge vrijednosti pomoću klizača.

**Broj za maksimalni isporuke obavijesti datoteka**: koliko je puta središtu IoT pokušava izlaganje datoteke prenesite obavijesti. Po zadanom postavljena na 10, no može se prilagoditi radi druge vrijednosti pomoću klizača.

![][15]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o mogućnostima prijenosa datoteka IoT koncentrator potražite u članku [Prijenos datoteke s uređaja] [ lnk-upload] u vodiču za razvojne inženjere.

Slijedite ove veze da biste saznali više o upravljanju Azure IoT koncentratora:

- [Skupno upravljanje uređajima IoT][lnk-bulk]
- [Korištenje mjerenja][lnk-metrics]
- [Operacije nadzora][lnk-monitor]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]
- [Sigurne rješenje IoT na prema gore][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md