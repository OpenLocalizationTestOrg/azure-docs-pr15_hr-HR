<properties
 pageTitle="Koncentrator IoT dijagnostičkih mjerenja"
 description="Pregled metriku Azure IoT koncentrator korisnicima omogućuje procijenite stanje njihove resursa"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Uvod u dijagnostičkih mjerenja

Dijagnostičke metriku dati bolje podataka o stanju Azure resursa za Azure pretplatu. Metriku omogućuju procijenite stanje servisa i uređaja povezanih s njom. Statistički podaci o korisniku dostupnog važna su jer oni omogućuju vidite što se događa s vašem IoT koncentratora i pomoć korijenskog uzroka problema s bez obzira na to obratite se podršci za Azure.

Možete omogućiti dijagnostičkih metriku s portala za Azure.

## <a name="how-to-enable-diagnostic-metrics"></a>Kako omogućiti dijagnostičkih mjerenja

1. Stvaranje koncentratora za IoT. Može pronaći upute o stvaranju koncentratora za IoT u [Početak] [ lnk-get-started] vodič.

2. Otvorite plohu koncentratora za IoT. Iz njega, kliknite **Dijagnostika**.

    ![][1]

3. Konfiguriranje vaše Dijagnostika postavljanja statusa na **na** , a zatim odaberite račun za pohranu Azure radi pohrane podataka Dijagnostika. Potvrdite **metriku**, a zatim pritisnite **Spremi**. Imajte na umu račun za Azure prostora za pohranu potrebno je stvoriti na vrijeme i da vam se naplatiti zasebno za pohranu. Možete odabrati i da biste poslali podatke Dijagnostika krajnje koncentratora za događaj.

    ![][2]

4. Nakon postavljanja Dijagnostika vratite se na središte plohu za **Pregled** IoT. U odjeljku **praćenja** u plohu se popunjava metriku informacije. Klikom na grafikonu otvorit će se okno metriku gdje možete pogledati sažetak informacija metriku koncentratora za IoT i uređivanje odabira metriku prikazane na grafikonu. Možete konfigurirati i upozorenja na temelju metrike vrijednosti.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Metriku i kako ih koristiti

Koncentrator IoT nudi nekoliko mjernih podataka da bi se dobilo pregled stanja vaše središte i ukupan broj uređaja povezanih s njom. Možete kombinirati podatke iz više metriku da biste crtali povećali sliku stanja središtu IoT. U sljedećoj tablici opisane metriku svaki koncentrator IoT prati i svaki metriku odnosa cjelokupan status središtu IoT.

| Mjerenja | Opis mjerenja | Što metriku koristi |
| ---- | ---- | ---- |
| d2c.telemetry.ingress.allProtocol | Broj poruke poslane na svim uređajima | Pregled podataka na šalje poruku |
| d2c.telemetry.ingress.Success | Brojanje svih poruka uspješno u središtu | Pregled ingress uspjelo poruku u središtu |
| c2d.Commands.egress.Complete.Success | Brojanje svih naredbi poruka dovršio primanju uređaj na svim uređajima | Zajedno s metriku na odustati od i Odbaci daje pregled ukupne C2D naredba uspješnosti |
| c2d.Commands.egress.abandon.Success | Brojanje svih poruka uspješno napuštenih primanju uređaj na svim uređajima | Ističe potencijalne probleme ako poruke su početak napuštenih češće nego je očekivano |
| c2d.Commands.egress.reject.Success | Brojanje svih poruka uspješno odbio primanju uređaj na svim uređajima | Ističe potencijalne probleme ako češće očekivan su početak odbijanju poruka |
| devices.totalDevices | Prosjek, minimum i max broj uređaja registrirana koncentrator IoT | Broj uređaja registrirati koncentratora |
| devices.connectedDevices.allProtocol | Prosjek, minimum i max broja istodobno povezanih uređaja | Pregled broja uređaja povezanih s koncentratora |

## <a name="next-steps"></a>Daljnji koraci

Sad kad vidjeli pregled dijagnostičkih metriku slijedite ovu vezu da biste saznali više o upravljanju Azure IoT koncentratora:

- [Operacije nadzora][lnk-monitor]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
