<properties
    pageTitle="Prilagodba unaprijed konfigurirane rješenja | Microsoft Azure"
    description="Sadrži smjernice o prilagodbi rješenja Azure IoT paket unaprijed konfigurirane."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Prilagodba konfiguriranog rješenja

Unaprijed konfiguriranom rješenja koji ste dobili uz paket IoT Azure demonstrirati servisa unutar paket zajednički rad izlaganje rješenje za kraj do kraja. Iz početnu točku postoje raznih mjesta proširiti i Prilagodba rješenje za nekim konkretnim scenarijima. U sljedećim odjeljcima opisuju te uobičajene prilagodbe točke.

## <a name="finding-the-source-code"></a>Pronalaženje izvornog koda

Izvorni kod za konfiguriranog rješenja dostupna je na GitHub u sljedećim spremištima:

- Udaljena nadzor: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Predvidljivu održavanje: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Izvorni kod za konfiguriranog rješenja navedeni su da bismo pokazali obrazaca i postupci za implementaciju funkciju završetka do kraja pomoću Azure IoT paketa rješenja za IoT. Možete pronaći dodatne informacije o tome kako izraditi i implementacija rješenja u spremištima GitHub.

## <a name="changing-the-preconfigured-rules"></a>Promjena konfiguriranog pravila

Udaljena nadzora rješenje sadrži tri poslove [Azure strujanje Analytics](https://azure.microsoft.com/services/stream-analytics/) za implementaciju informacije, telemetrijskih i pravila uređaja logike koje su prikazane rješenja.

Tri strujanje poslove analize i njihovih sintaksa je opisano u dubinu na [udaljenom konfiguriranog rješenje vodič za nadzor](iot-suite-remote-monitoring-sample-walkthrough.md). 

Možete urediti te poslovi izravno za izmjenu logičku ili dodavanje logike određene scenariju. Strujanje analize zadacima možete pronaći na sljedeći način:
 
1. Idite na [portal za Azure](https://portal.azure.com).
2. Idite u grupu resursa s istim nazivom kao rješenje IoT. 
3. Odaberite posao Azure strujanje analize koju želite izmijeniti. 
4. Zaustavite posao odabirom mogućnosti **Zaustavi**skupa naredbi. 
5. Uređivanje unosa, upita i izlaza.

    Jednostavnom izmjenom sastoji se od promjene upita za posao **pravila** za korištenje na **"<"** umjesto na **">"**. Portal za rješenja će i dalje prikazuju **">"** kada uredite pravilo, ali Primijetit ćete da se ponašanje zrcaliti zbog promjena u podlozi posla.

6. Pokretanje zadatka

> [AZURE.NOTE] Remote nadzora nadzorne ploče ovisi o konkretne podatke pa mijenjanja poslove mogu prouzročiti na nadzornoj ploči uvoza.

## <a name="adding-your-own-rules"></a>Dodavanje vlastite pravila

Uz mijenjanje konfiguriranog poslovi Azure strujanje analize, možete koristiti portal za Azure da biste dodali nove zadatke ili dodajte nove upite postojeće poslove.

## <a name="customizing-devices"></a>Prilagodbom uređaja

Jedan od najčešćih proširenje aktivnosti radi s uređajima specifična za vašu situaciju. Za rad s uređajima na nekoliko načina. Ovih metoda obuhvaćaju mijenjanja Simulirani uređaj tako da odgovara vašem scenariju, ili pomoću [SDK IoT uređaj][] fizički uređaj s rješenje.

Postupni vodič s dodavanjem uređaja udaljene nadzora konfiguriranog rješenje potražite u članku [Povezivanje uređaja paket za Iot](iot-suite-connecting-devices.md) i [Udaljene nadzora uzorka SDK C](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) je kompatibilan s udaljenog nadzora konfiguriranog rješenja.

### <a name="creating-your-own-simulated-device"></a>Stvaranje vlastite Simulirani uređaja

Obuhvaćeno na udaljenom nadzora rješenja izvorni kod (referencirani iznad), je .NET simulator. Ovaj simulator je dodjeli kao dio rješenja i moguće izmijeniti da biste poslali različite metapodaci, telemetrijskih ili odgovaranje na različite naredbe.

Simulator konfiguriranog udaljene nadzora konfiguriranog rješenje je hladnijom uređaj koji emits temperature i humidity telemetrijskih simulator u programu project [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) možete izmijeniti kada ste forked GitHub spremište.

### <a name="available-locations-for-simulated-devices"></a>Mjesta dostupnih za Simulirani uređaje

Postavljanje zadanog mjesta se Seattle/Redmond, Washington, Sjedinjene Američke Države. Možete promijeniti ovim mjestima u [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Stvaranje i korištenje (izračunata) uređaja

[Azure IoT SDK-ovi](https://github.com/Azure/azure-iot-sdks) sadrže biblioteke za povezivanje razne vrste uređaja (jezika i operacijski sustavi) u IoT rješenja.

## <a name="modifying-dashboard-limits"></a>Izmjena ograničenja nadzorne ploče

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Broj uređaja koje se prikazuju u padajućem nadzorne ploče

Zadano je 200. Možete promijeniti taj broj u [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Broj PIN-ove za prikaz u kontroli Bing karte

Zadano je 200. Možete promijeniti taj broj u [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Vremensko razdoblje telemetrijskih grafikonu

Zadana je vrijednost 10 minuta. To možete promijeniti u [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Ručno postavljanje aplikacije uloge

U nastavku opisuje kako dodati aplikacije uloge **administrator** i **samo za čitanje** u unaprijed konfiguriranom rješenje. Imajte na umu da konfiguriranog rješenja već dodjeli s web-mjesta azureiotsuite.com obuhvaća uloge **administrator** i **samo za čitanje** .

Članovi uloge **samo za čitanje** možete vidjeti na nadzornoj ploči i na popisu uređaja, ali nije dopušteno dodavanje uređaja, promijenite atribute uređaja ili slanje naredbi.  Članovi u **administratorskoj** ulozi imali puni pristup funkcionalnost u rješenje.

1. Idite na [portal za Azure klasični][lnk-classic-portal].

2. Odaberite **servisa Active Directory**.

3. Kliknite naziv AAD klijentu koji ste koristili kada dodjeli rješenje.

4. Kliknite **aplikacije**.

5. Kliknite naziv aplikacije koja odgovara naziv konfiguriranog rješenja. Ako ne vidite aplikacije na popisu, odaberite **aplikacije koje je vlasnik Moja tvrtka** u **Prikaz** padajući popis i kliknite kvačicu.

6.  Pri dnu stranice kliknite **Upravljanje manifesta** , a zatim **Preuzmite manifesta**.

7. Time će se preuzeti datoteku .json na lokalno računalo.  Otvorite datoteku za uređivanje u uređivaču teksta po izboru.

8. U retku treći .json datoteke, vidjet ćete:

  ```
  "appRoles" : [],
  ```
  Zamijenite to sljedeće:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Spremite datoteku ažurirane .json (možete Prebriši postojeće datoteke).

10.  Na portalu za upravljanje Azure, pri dnu stranice odaberite **Upravljanje manifesta** , a zatim **Prijenos manifesta** prenijeti datoteku .json koje ste spremili u prethodnom koraku.

11. Sada ste dodali uloge **administrator** i **samo za čitanje** u aplikaciji.

12. Da biste dodijelili jednu od tih uloga korisnika u imeniku, potražite u članku [dozvola na web-mjestu azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Povratne informacije

Imate li Prilagodba kojima želite da biste vidjeli obuhvaćene u ovom dokumentu? Dodajte prijedloge za značajku [Govorne korisnika](https://feedback.azure.com/forums/321918-azure-iot)ili komentar u nastavku ovog članka. 

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o mogućnostima za prilagodbu konfiguriranog rješenja, pogledajte:

- [Povezivanje logike aplikacije rješenje Azure IoT paket udaljene nadzor unaprijed konfigurirane][lnk-logicapp]
- [Korištenje dinamičke telemetrijskih s udaljenog nadzor unaprijed konfigurirane rješenja][lnk-dynamic]
- [Uređaj informacije metapodataka u udaljene nadzor unaprijed konfigurirane rješenja][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[Uređaj IoT SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
