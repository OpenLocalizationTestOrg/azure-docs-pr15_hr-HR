<properties
 pageTitle="Vodič za razvojne inženjere - prijenos datoteke | Microsoft Azure"
 description="Azure vodič za razvojne inženjere koncentrator IoT - prijenos datoteke s uređaja da biste središte IoT"
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

# <a name="upload-files-from-a-device"></a>Prijenos datoteka s uređaja

## <a name="overview"></a>Pregled

Detaljne, u [koncentratoru IoT krajnje točke] [ lnk-endpoints] članak, uređaja možete pokrenuti prijenos datoteke tako da pošaljete obavijest putem uređaja dostupnog krajnje (**/devices/ {deviceId} / datoteke**).  Kada uređaj obavještava IoT koncentratoru dovršeni prijenos, IoT koncentrator stvara datoteku prijenosima koji primate putem servisa dostupnog krajnje (**/messages/servicebound/filenotifications**) kao poruke.

Umjesto brokering poruka putem koncentratora IoT sam IoT koncentrator umjesto djeluje kao otpremnik pridruženi račun za Azure prostora za pohranu. Na uređaju zahtjeve za pohranu tokena iz koncentratora IoT koji se odnose na uređaj želi prenijeti datoteku. Uređaj koristi SAS URI da biste prenijeli datoteku za pohranu, a nakon dovršetka prijenos uređaj će poslati primatelju obavijest o završetku IoT koncentrator. Koncentrator IoT potvrđuje datoteku je učitana, a zatim dodaje obavijesti za prijenos datoteka na servis dostupnog datoteke obavijesti SMS krajnju točku.

Prije prijenosa datoteke IoT koncentrator s uređaja, morate konfigurirati vaše središte povezivanjem [Programa Azure prostora za pohranu] [ lnk-associate-storage] računa na njega.

Vaš uređaj pa možete [pokrenuti na prijenos] [ lnk-initialize] i [obavijesti koncentrator IoT] [ lnk-notify] kada se dovrši prijenos. Po želji, prilikom uređaj obavještava IoT koncentrator prijenos dovrši, servis možete stvoriti [obavijest][lnk-service-notification].

### <a name="when-to-use"></a>Kada se koristi

Koristite ovu značajku IoT koncentrator kada je potrebno za prijenos datoteke s uređaja u funkcioniranju servisa pozadinske umjesto slanja poruke putem koncentratora IoT.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Račun za Azure pohranu pridružiti IoT koncentratora

Da biste koristili funkciju prijenos datoteke, morate najprije povezivanje poslovnog subjekta za pohranu Azure središtu IoT. To možete učiniti nešto putem [portala za Azure][lnk-management-portal], ili programski putem [davatelja resursa IoT koncentrator REST API -ji][lnk-resource-provider-apis]. Nakon što ste povezali račun za pohranu Azure s koncentratora za IoT, servis vraća SAS URI na uređaj kada uređaj pokreće zahtjeva za prijenos datoteka.

> [AZURE.NOTE] [Azure IoT koncentrator SDK-ovi] [ lnk-sdks] automatski rukovati dohvaćanje SAS URI, prijenos datoteka i obavještavanje IoT koncentratoru dovršene prijenos.

## <a name="initialize-a-file-upload"></a>Pokrenuti prijenos datoteke

Koncentrator IoT je krajnje posebno za uređaje da biste zatražili SAS URI za pohranu za prijenos datoteke. Uređaj pokreće postupak za prijenos datoteka slanjem objavu koncentrator IoT pri `{iot hub}.azure-devices.net/devices/{deviceId}/files` s JSON tijelo sljedeće:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

Koncentrator IoT vraća sljedeće uređaj koristi da biste prenijeli datoteku:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Zastarjeli: pokrenuti prijenos datoteke s VIDJELI

> [AZURE.NOTE] U ovom se odjeljku opisuju zastarjeli funkcija načina primanja SAS URI iz centra za IoT. Koristite prethodno opisan način objavu.

Koncentrator IoT ima dvije OSTALE krajnje točke za podršku prijenosa datoteka jedan da biste dobili SAS URI za pohranu, a drugi će obavijestiti koncentratoru IoT dovršene prijenos. Uređaj pokreće postupak za prijenos datoteka slanjem VIDJELI koncentrator IoT na `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Koncentrator vraća SAS URI određene datoteku da biste je moguće prenijeti i ID korelacije koji će se koristiti kada se dovrši prijenos.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Obavijesti IoT koncentratoru prijenosa dovršene datoteka

Uređaj je zadužen za prijenos datoteka u prostor za pohranu pomoću SDK-ovi za pohranu Azure. Kada prijenos završi, uređaj šalje objavu koncentrator IoT pri `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` s JSON tijelo sljedeće:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Vrijednost `isSuccess` je Booleova predstavlja hoće li uspješno prenijeti datoteku. Šifra stanja za `statusCode` stanje prijenosa datoteka za pohranu, a `statusDescription` odgovara na `statusCode`.

## <a name="reference-topics"></a>Pregled tema:

U sljedećim temama reference sadrže dodatne informacije o prijenosu datoteke s uređaja.

## <a name="file-upload-notifications"></a>Datoteka prijenosima

Kada uređaj prenosi datoteke i obavještava IoT koncentrator dovršenog prijenos, servis po želji stvara poruku koja sadrži naziv i pohranu mjesto datoteke.

Kao što je opisano u [krajnje točke][lnk-endpoints], IoT koncentrator nudi datoteke prijenosima putem servisa dostupnog krajnje (**/messages/servicebound/fileuploadnotifications**) kao poruke. Semantiku primanje obavijesti za prijenos datoteka isti su kao poruke oblaka uređaja i imaju isti [životni ciklus poruke][lnk-lifecycle]. Svaku poruku dohvaćeni iz krajnjoj točki obavijesti za prijenos datoteka je zapis JSON s sljedeća svojstva:

| Svojstvo | Opis |
| -------- | ----------- |
| EnqueuedTimeUtc | Vremenska oznaka koja označava stvaranja obavijesti. |
| DeviceId | **DeviceId** uređaj koji prenijeti datoteku. |
| BlobUri | URI prenesenih datoteka. |
| BlobName | Naziv prenesene datoteke. |
| LastUpdatedTime | Vremenska oznaka koja označava kada datoteku zadnjeg ažuriranja. |
| BlobSizeInBytes | Veličina prenesenih datoteka. |

**Primjer**. Ovo je primjer tijelu poruke s obavijesti Prenesi datoteku.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Mogućnosti za konfiguraciju obavijesti prijenos datoteka

Svaki IoT koncentrator izlaže sljedeće mogućnosti konfiguracije prijenosima datoteka:

| Svojstvo | Opis | Raspon i zadane |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Određuje hoće li datoteka prijenosima zapisuju obavijesti o krajnjoj datoteke. | Booleovom. Zadani: True. |
| **fileNotifications.ttlAsIso8601** | Zadani TTL prijenosima datoteka. | ISO_8601 interval do 48 H (minimalne 1 minute). Zadani: 1 sat. |
| **fileNotifications.lockDuration** | Zaključavanje trajanje reda čekanja obavijesti za prijenos datoteka. | 5 do 300 sekundi (minimalne 5 sekundi). Zadani: 60 sekundi. |
| **fileNotifications.maxDeliveryCount** | Maksimalna isporuke count datoteke prenesite reda čekanja obavijesti. | 1 do 100. Zadani: 100. |

## <a name="additional-reference-material"></a>Dodatne reference materijala

Druge teme referencu u vodiču za razvojne inženjere obuhvaćaju sljedeće:

- [Krajnje točke koncentrator IoT] [ lnk-endpoints] opisuju razne izlaže svaki IoT koncentrator za upravljanje i izvođenje operacija krajnje točke.
- [Ograničavanje i kvotama] [ lnk-quotas] u članku se opisuje kvote koji se odnose na servis IoT koncentratora i regulacije ponašanje očekivati prilikom korištenja servisa.
- [Koncentrator IoT uređaja i servisa SDK-ovi] [ lnk-sdks] popis različitih jezika SDK-ovi koje koristi u prilikom razvoja uređaja i servisa aplikacije kompatibilne s IoT koncentratora.
- [Upit jezik za twins, postupcima i zadacima] [ lnk-query] u članku se opisuje jezik upita možete koristiti za dohvaćanje informacija iz centra za IoT o uređaju twins, metode i zadacima.
- [Podrška IoT koncentrator MQTT] [ lnk-devguide-mqtt] navedene su dodatne informacije o podršci za IoT koncentrator za protokol MQTT.

## <a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako prenijeti datoteke s uređaja pomoću IoT koncentrator, možda je u sljedećim temama vodič za razvojne inženjere:

- [Upravljanja uređajima u središte IoT][lnk-devguide-identities]
- [Upravljanje pristupom koncentrator IoT][lnk-devguide-security]
- [Korištenje twins uređaj da biste sinkronizirali stanje i konfiguracija][lnk-devguide-device-twins]
- [Pozivanje metode izravno na uređaju][lnk-devguide-directmethods]
- [Zakazivanje zadataka na više uređaja][lnk-devguide-jobs]

Ako želite isprobati neke koncepta opisane u ovom članku, možda ćete zanimati na središte IoT Praktični vodič:

- [Upute za prijenos datoteka s uređaja s oblakom pomoću IoT koncentratora][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
