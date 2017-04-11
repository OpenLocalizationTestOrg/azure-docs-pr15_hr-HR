<properties
 pageTitle="Uvoz i izvoz koncentrator IoT uređaj identiteta | Microsoft Azure"
 description="Koncepti i .NET kod isječci za upravljanje skupno koncentrator IoT uređaj identiteta"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Skupno upravljanja IoT koncentrator uređaj identiteta

Svaki IoT koncentrator ima registra identiteta uređaj, možete koristiti za stvaranje po uređaj resursa u servisa, kao što je red čekanja koji sadrži in-flight poruke oblak na uređaj. Identitet registra uređaja omogućuje vam za kontrolu pristupa krajnje točke okrenutom uređaja. U ovom se članku opisuje kako uvoz i izvoz uređaj identiteta grupno i iz registra za identitet uređaja.

Uvoz i izvoz mjesto za preuzimanje operacije u kontekstu *poslove* koje omogućuju vam dopušta izvršavanje operacija servisa skupno u odnosu na središte IoT.

Klase **RegistryManager** sadrži **ExportDevicesAsync** i **ImportDevicesAsync** metode koje koriste framework **posao** . Načini omogućuju izvoz, uvoz i sinkronizacija cijelosti do registra za IoT koncentrator uređaja.

## <a name="what-are-jobs"></a>Što su zadacima?

Uređaj identiteta registra operacije koristiti sustav **posla** kada postupak:

*  Sadrži potencijalno dulje vrijeme izvođenja u usporedbi s standardne izvođenje operacije ili
*  Vraća veliku količinu podataka korisniku.

U tim slučajevima, umjesto jednog poziv API čekanje ili blokiranje na rezultat operacije, operacija asinkrono stvara **zadatak** za IoT koncentratora. Operacija pa odmah vraća **JobProperties** objekt.

Sljedeće C# koda prikazuje kako stvoriti zadatak programa izvoza:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Zatim možete koristiti klase **RegistryManager** upit stanje **posla** koji koristi vraćeni **JobProperties** metapodataka.

Sljedeće C# koda pokazuje kako provjeriti svakih pet sekundi da biste vidjeli ako je zadatak završio izvršavanja:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Izvoz uređaja

Da biste izvezli cijelosti do registra za IoT koncentrator uređaj spremniku blobova platforme [Azure pohrane](https://azure.microsoft.com/documentation/services/storage/) koja se [Zajednički pristup potpis](https://msdn.microsoft.com/library/ee395415.aspx), upotrijebite metodu **ExportDevicesAsync** .

Ovaj postupak omogućuje vam stvaranje pouzdanog sigurnosne kopije vaših podataka uređaj u spremniku blob koji omogućuje upravljanje.

Način **ExportDevicesAsync** traži dva parametra:

*  *Niz* koji sadrži URI spremnika blob. U ovom URI mora sadržavati SAS token koja omogućuje pristup pisanje spremnik. Posao stvara blob bloka u ovom spremniku radi pohrane podataka serijaliziranog izvoz uređaja. SAS token mora sadržavati sljedeće dozvole:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  *Booleova* koji označava ako želite izostaviti tipke za provjeru autentičnosti iz izvoza podataka. Ako je **false**, provjere autentičnosti tipke nalaze se u odjeljku Izvezi Izlaz; u suprotnom tipke izvoze se kao **vrijednost null**.

Sljedeće C# koda pokazuje kako da biste započeli zadatak za izvoza koja sadrži tipke za provjeru autentičnosti uređaja u izvoza podataka, a zatim ankete za dovršetak:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Posao sprema rezultat u spremniku blob uneseni kao blob blok s nazivom **devices.txt**. Za izlazne podatke sastoji se od JSON serijalizirani uređaj podatke, s jednim uređajem po retku.

Sljedeći primjer prikazuje podatke:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Ako vam je potreban pristup da biste te podatke u kodu, koji možete jednostavno ukloniti serijski broj ove podatke pomoću klase **ExportImportDevice** . Sljedeće C# koda prikazuje kako čitati informacije o uređaju koji je prethodno izvezena blob bloka:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  Način **GetDevicesAsync** klase **RegistryManager** možete koristiti i da bi dohvatili popis uređaja. No takvog ima teško kapaciteta 1000 broj objekata uređaja koje se vraćaju. Kutije očekivani koristi za metodu **GetDevicesAsync** je za razvoj scenariji da biste olakšali ispravljanje pogrešaka i ne preporučuje se za proizvodnju radnih opterećenja.

## <a name="import-devices"></a>Uvoz uređaja

Metoda **ImportDevicesAsync** u predmete **RegistryManager** omogućuje izvođenje masovne uvoz i sinkronizacija operacije u do registra za IoT koncentrator uređaja. Kao što je način **ExportDevicesAsync** metodu **ImportDevicesAsync** koristi framework **posao** .

Pobrinuti metodom **ImportDevicesAsync** jer uz dodjeljivanje novim uređajima u registar identiteta uređaj, možete i ažuriranje i brisanje postojećih uređaja.

> [AZURE.WARNING]  Operacije uvoza ne može poništiti. Obavezno sigurnosno kopirajte postojeće podatke pomoću metode **ExportDevicesAsync** u drugi spremnik blob prije promjene skupno u registar identiteta uređaja.

Način **ImportDevicesAsync** uzima dva parametra:

*  *Niz* koji sadrži URI programa spremnika blobova platforme [Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/) da biste koristili kao *unos* za posao. U ovom URI mora sadržavati SAS token koja omogućuje pristup čitanju spremnik. Ovaj spremnik mora sadržavati blob s naziv **devices.txt** koja sadrži podatke serijaliziranog uređaj da biste uvezli u registar identiteta uređaja. Uvoz podataka mora sadržavati informacije o uređaju u istom JSON OSNOVNI oblik koji posao **ExportImportDevice** koristi kada stvara **devices.txt** blob. SAS token mora sadržavati sljedeće dozvole:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  *Niz* koji sadrži URI programa spremnika [Azure spremište](https://azure.microsoft.com/documentation/services/storage/) blobova platforme da biste koristili kao *Izlaz* iz posao. Posao stvara blob bloka u ovom spremniku pohraniti sve informacije o pogrešci s Dovršeni uvoz **posao**. SAS token mora sadržavati sljedeće dozvole:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  Isti spremniku blob postavite pokazivač miša dva parametra. Zasebnom parametre jednostavno omogućuju veću kontrolu nad podataka kao što je spremnik izlaz potrebne dodatne dozvole.

Sljedeće C# koda pokazuje kako da biste započeli posao uvoza:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Uvoz ponašanje

Upotrijebite metodu **ImportDevicesAsync** izvesti sljedeće radnje skupno u registar identiteta uređaj:

-   Registracija za skupno novih uređaja
-   Skupno brisanja postojećih uređaja
-   Skupno promjenu statusa (Omogućivanje i onemogućivanje uređaji)
-   Skupno Dodjela tipkovnih novi uređaj provjere autentičnosti
-   Skupno automatski regeneration ključeva za provjeru autentičnosti uređaja

Možete izvršiti bilo koju kombinaciju prethodne operacije u jednom **ImportDevicesAsync** poziv. Na primjer, možete registrirati novim uređajima i izbrisati ili ažuriranje postojećih uređaja u isto vrijeme. Kada se koristi uz način **ExportDevicesAsync** , potpuno možete migrirati svim svojim uređajima iz jednog koncentrator IoT na drugi.

Pomoću svojstva neobavezno **importMode** pod stavkom Uvezi podatke serijalizacije svakog upravljajte postupak po – uređaj za uvoz. Svojstvo **importMode** sastoji se od sljedećih mogućnosti:

| importMode |  Opis |
| -------- | ----------- |
| **createOrUpdate** | Ako je uređaj ne postoji navedeni **id**, upravo registriran je. <br/>Ako je uređaj već postoji, postojeće podatke prebrisat će navedeni ulaznih podataka bez obzira na **jedna** vrijednost. |
| **Stvaranje** | Ako je uređaj ne postoji navedeni **id**, upravo registriran je. <br/>Ako je uređaj već postoji, pogreška zapisuje datoteke zapisnika. |
| **Ažuriranje** | Ako je uređaj već postoji navedeni **id**, postojeće podatke prebrisat će navedeni ulaznih podataka bez obzira na **jedna** vrijednost. <br/>Ako je uređaj ne postoji, pogreška zapisuje datoteke zapisnika. |
| **updateIfMatchETag** | Ako je uređaj već postoji navedeni **id**, postojeće podatke prebrisat će navedeni ulaznih podataka samo ako je pronađeno podudaranje **e-oznake** . <br/>Ako je uređaj ne postoji, pogreška zapisuje datoteke zapisnika. <br/>Ako se ne podudaraju se **e-oznake** , pogreška zapisuje datoteke zapisnika. |
| **createOrUpdateIfMatchETag** | Ako je uređaj ne postoji navedeni **id**, upravo registriran je. <br/>Ako je uređaj već postoji, postojeće podatke prebrisat će navedeni ulaznih podataka samo ako je pronađeno podudaranje **e-oznake** . <br/>Ako se ne podudaraju se **e-oznake** , pogreška zapisuje datoteke zapisnika. |
| **Brisanje** | Ako je uređaj već postoji navedeni **id**, ona će se izbrisati bez obzira na **jedna** vrijednost. <br/>Ako je uređaj ne postoji, pogreška zapisuje datoteke zapisnika. |
| **deleteIfMatchETag** | Ako je uređaj već postoji navedeni **id**, ona se briše samo ako je pronađeno podudaranje **e-oznake** . Ako je uređaj ne postoji, pogreška zapisuje datoteke zapisnika. <br/>Ako se ne podudaraju se e-oznake, pogreška zapisuje datoteke zapisnika. |

> [AZURE.NOTE] Ako podatke serijalizacije definirate izričito programa **importMode** zastavica za uređaj, po zadanom je odabrana **createOrUpdate** tijekom operacije uvoza.

## <a name="import-devices-example--bulk-device-provisioning"></a>Uvoz primjera uređaji – skupno uređaj dodjele resursa 

Sljedeće C# kod uzorka ilustrira da biste generirali više identiteta uređaja koji:

- Uključite tipki za provjeru autentičnosti.
- Blob Blok za pisanje te informacije o uređaju.
- Uvoz uređaje u registar identiteta uređaja.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Uvoz uređaji primjer – masovnog brisanja

Sljedećim primjerom koda prikazuje kako izbrisati uređaje koje ste dodali u prethodnom primjeru koda pomoću:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Početak spremnik SAS URI


Sljedećim primjerom koda pokazuje kako da biste generirali [SAS URI](../storage/storage-dotnet-shared-access-signature-part-2.md) čitanje, pisanje i Izbriši dozvole za spremniku blob:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku naučili kako izvoditi masovne operacije protiv identiteta registra uređaja u koncentratora za IoT. Slijedite ove veze da biste saznali više o upravljanju Azure IoT koncentratora:

- [Korištenje mjerenja][lnk-metrics]
- [Operacije nadzora][lnk-monitor]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md