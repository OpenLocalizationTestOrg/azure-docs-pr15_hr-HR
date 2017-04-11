<properties
    pageTitle="Azure Monitor autoscaling uobičajenih metriku. | Microsoft Azure"
    description="Saznajte koje metriku se najčešće koriste za autoscaling servise u Oblaku, virtualnih računala i web-aplikacije."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor autoscaling uobičajenih mjerenja

Azure autoscaling monitora omogućuje da biste skalirali broj pokrenute instance prema gore ili prema dolje, na temelju telemetrijskih podataka (metriku). Ovaj dokument opisuje uobičajene metriku koja želite koristiti. Na portalu Azure za servise u Oblaku i farme poslužitelja možete odabrati metriku resursa za promjenu veličine tako da. Međutim, sve metriku možete odabrati iz različitih resursa za promjenu veličine tako da.

Evo pojedinosti o tome kako biste pronašli i popis metrike koju želite za promjenu veličine tako da. Sljedeće se primjenjuje kao i skaliranja skupove skaliranje virtualnog računala.

## <a name="compute-metrics"></a>Izračunavanje mjerenja
Prema zadanom Azure VM v2 dolazi s nastavkom Dijagnostika konfigurirana, a imaju sljedeće metriku uključena.

- [Goste metriku v2 VM za Windows](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Goste metriku v2 Linux VM](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Možete koristiti u `Get MetricDefinitions` API/PoSH/EŽA da biste pogledali metriku dostupna za vaš VMSS resursa. 

Ako koristite VM skaliranje skupova i ne vidite određeni metričkih podataka na popisu, pa je vjerojatno *onemogućene* u Dijagnostika pozivni broj.

Ako ne se određeni metriku uzorkovanja ili prenijeti na željenu učestalost, možete ažurirati Dijagnostika konfiguracije.

Ako vrijedi iznad svakom slučaju, pregledajte [Korištenje ljuske PowerShell za omogućivanje Azure Dijagnostika u virtualnog računala sa sustavom Windows](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) o ljuske PowerShell za konfiguriranje i ažuriranje Azure VM Dijagnostika pozivni broj da biste omogućili metriku. Članak sadrži i ogledne datoteke Dijagnostika konfiguracije.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Izračunavanje metriku Windows VM v2 kao gost OS

Kada stvorite novu VM (v2) u Azure, Dijagnostika je omogućena korištenjem dijagnostike nastavak.

Možete stvoriti popis metriku pomoću sljedeće naredbe u ljusci PowerShell.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Možete stvoriti upozorenje za sljedeće metriku.

|Naziv mjerenja|   Jedinica|
|---|---|
|\Processor(_Total)\% procesor vremena    |Posto|
|\Processor(_Total)\% ovlasti vremena   |Posto|
|\Processor(_Total)\% vrijeme korisnika |Posto|
|Učestalost \Processor za \Processor informacije (_Total) |Count|
|\System\Processes| Count|
|Count \Thread \Process (_Total)| Count|
|Count \Handle \Process (_Total)  |Count|
|\Memory\% izvršenom bajtova koristi   |Posto|
|\Memory\Available bajtova|   Bajtova|
|\Memory\Committed bajtova    |Bajtova|
|Ograničenje \Memory\Commit|  Bajtova|
|\Memory\Pool Straničeno bajtova|  Bajtova|
|\Memory\Pool nepaginiranih|   Bajtova|
|\PhysicalDisk(_Total)\% vrijeme na disku| Posto|
|\PhysicalDisk(_Total)\% čitanju diska vremena|    Posto|
|\PhysicalDisk(_Total)\% disk vrijeme pisanja|   Posto|
|Prijenosi/sec \Disk \PhysicalDisk (_Total)   |CountPerSecond|
|Čitanja/sec \Disk \PhysicalDisk (_Total)   |CountPerSecond|
|Zapisivanje/sec \Disk \PhysicalDisk (_Total)  |CountPerSecond|
|Bajtova/sec \Disk \PhysicalDisk (_Total)   |BytesPerSecond|
|\PhysicalDisk (_Total) \Disk čitati bajtova/sec| BytesPerSecond|
|\PhysicalDisk (_Total) bajtova u pisanje \Disk/sec |BytesPerSecond|
|\Avg \PhysicalDisk (_Total). Duljina reda čekanja na disku|  Count|
|\Avg \PhysicalDisk (_Total). Duljina reda čekanja za čitanje na disku| Count|
|\Avg \PhysicalDisk (_Total). Duljina reda čekanja za pisanje na disku |Count|
|\LogicalDisk(_Total)\% slobodnog prostora| Posto|
|Megabajta \Free \LogicalDisk (_Total)|   Count|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Izračunavanje metriku Linux VM v2 kao gost OS

Kada stvorite novu VM (v2) u Azure, Dijagnostika po zadanom je omogućena korištenjem dijagnostike nastavak.

Možete stvoriti popis metriku pomoću sljedeće naredbe u ljusci PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Možete stvoriti upozorenje za sljedeće metriku.

|Naziv mjerenja                            |Jedinica|
|---|---|
|\Memory\AvailableMemory                |Bajtova|
|\Memory\PercentAvailableMemory         |Posto|
|\Memory\UsedMemory                     |Bajtova|
|\Memory\PercentUsedMemory              |Posto|
|\Memory\PercentUsedByCache             |Posto|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Bajtova|
|\Memory\PercentAvailableSwap           |Posto|
|\Memory\UsedSwap                       |Bajtova|
|\Memory\PercentUsedSwap                |Posto|
|\Processor\PercentIdleTime             |Posto|
|\Processor\PercentUserTime             |Posto|
|\Processor\PercentNiceTime             |Posto|
|\Processor\PercentPrivilegedTime       |Posto|
|\Processor\PercentInterruptTime        |Posto|
|\Processor\PercentDPCTime              |Posto|
|\Processor\PercentProcessorTime        |Posto|
|\Processor\PercentIOWaitTime           |Posto|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Sekundi|
|\PhysicalDisk\AverageWriteTime         |Sekundi|
|\PhysicalDisk\AverageTransferTime      |Sekundi|
|\PhysicalDisk\AverageDiskQueueLength   |Count|
|\NetworkInterface\BytesTransmitted     |Bajtova|
|\NetworkInterface\BytesReceived        |Bajtova|
|\NetworkInterface\PacketsTransmitted   |Count|
|\NetworkInterface\PacketsReceived      |Count|
|\NetworkInterface\BytesTotal           |Bajtova|
|\NetworkInterface\TotalRxErrors        |Count|
|\NetworkInterface\TotalTxErrors        |Count|
|\NetworkInterface\TotalCollisions      |Count|




## <a name="commonly-used-web-server-farm-metrics"></a>Najčešće korištenih metriku Web (farme poslužitelja)

Možete izvesti i automatsko skaliranje na temelju uobičajenih metrike web server kao što je Duljina reda čekanja HTTP-a. Njegova metričkim naziv je **HttpQueueLength**.  Sljedeći odjeljak sadrži popis metriku dostupna poslužitelja farmi (Web Apps).

### <a name="web-apps-metrics"></a>Web-aplikacije mjerenja

Možete stvoriti popis metriku web-aplikacije pomoću sljedeće naredbe u ljusci PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Možete se upozorenje na ili promjena veličine tako da te metriku.

|Naziv mjerenja        |Jedinica|
|---                |---|
|CpuPercentage      |Posto|
|MemoryPercentage   |Posto|
|DiskQueueLength    |Count|
|HttpQueueLength    |Count|
|BytesReceived      |Bajtova|
|BytesSent          |Bajtova|


## <a name="commonly-used-storage-metrics"></a>Najčešće korištenih metrike pohrane
Mogu se mijenjati veličinu tako da Duljina reda čekanja za pohranu, što je broj poruka u redu čekanja za pohranu. Duljina reda čekanja za pohranu je posebno metriku i prag primjenjuje bit će se broj poruka po instance. To znači da ako postoje dvije instance, a ako je prag postavljeno na 100, će skaliranje kada ukupni broj poruka u redu čekanja 200. Na primjer, 100 poruke po instance.

Možete konfigurirati to je portal Azure u plohu **Postavke** . Za skupove skaliranje VM, možete ažurirati postavke automatsko skaliranje u predlošku OKVIRA da biste upotrijebili *metricName* *ApproximateMessageCount* i prosljeđivanje ID reda čekanja za pohranu kao *metricResourceUri*.

Ako, na primjer, s računom za pohranu klasični metricTrigger postavka automatsko skaliranje sadrži:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Za račun (koji nisu classic) prostora za pohranu, u metricTrigger sadrži:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Najčešće korištenih servisa Bus mjerenja

Mogu se mijenjati veličinu tako da Duljina reda čekanja Bus servisa koji se broj poruka u redu čekanja Bus servisa. Duljina reda čekanja Bus servis je posebno metriku i prag navedeni primjenjuje bit će se broj poruka po instance. To znači da ako postoje dvije instance, a ako je prag postavljeno na 100, će skaliranje kada ukupni broj poruka u redu čekanja 200. Na primjer, 100 poruke po instance.

Za skupove skaliranje VM, možete ažurirati postavke automatsko skaliranje u predlošku OKVIRA da biste upotrijebili *metricName* *ApproximateMessageCount* i prosljeđivanje ID reda čekanja za pohranu kao *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Za Bus servisa pojam grupu resursa ne postoji, ali Voditelj resursa Azure stvara zadanu grupu resursa po regijama. Grupa resursa obično je u obliku "Zadano - ServiceBus-[Regija]". Na primjer, 'Zadani-ServiceBus-EastUS', 'Zadani-ServiceBus-WestUS', 'zadani-ServiceBus-AustraliaEast"itd.
