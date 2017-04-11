<properties
    pageTitle="Azure metriku Monitor - podržani metriku po vrsti resursa | Microsoft Azure"
    description="Popis metriku dostupne za svaku vrstu resursa s Azure Monitor."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Podržani metriku s monitora Azure

Azure monitora na nekoliko načina za interakciju s metrika, uključujući stvaranje grafikona na portalu, pristupanje njima putem REST API-JA ili ih upita pomoću programa PowerShell ili EŽA. Ispod popis svih sva mjerenja trenutno dostupan je sa Azure Monitor metričkim kanal.

> [AZURE.NOTE] Ostale metriku možda biti dostupne na portalu ili pomoću naslijeđene API-ji. Ovaj popis sadrži samo javne pretpregled metriku dostupna pomoću javnog pretpregleda Konsolidirani metričkim kanala Azure Monitor.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|CoreCount|Osnovni broj|Count|Ukupni zbroj|Ukupan broj jezgri na računu za obradu|
|TotalNodeCount|Count čvor|Count|Ukupni zbroj|Ukupan broj čvorove u račun za obradu|
|CreatingNodeCount|Stvaranje čvor Count|Count|Ukupni zbroj|Broj čvorove stvoren|
|StartingNodeCount|Početni broj čvor|Count|Ukupni zbroj|Broj čvorove pokretanja|
|WaitingForStartTaskNodeCount|Čekanje za početak zadatka čvor Count|Count|Ukupni zbroj|Broj čvorove čeka se pokrenuti dovršenje zadatka|
|StartTaskFailedNodeCount|Početak zadatka nije uspjela čvor Count|Count|Ukupni zbroj|Broj čvorove gdje nije uspjela pokretanje zadatka|
|IdleNodeCount|Neaktivne čvor Count|Count|Ukupni zbroj|Broj neaktivnosti čvorove|
|OfflineNodeCount|Izvanmrežna čvor Count|Count|Ukupni zbroj|Broj čvorove izvan mreže|
|RebootingNodeCount|Ponovnog pokretanja čvor Count|Count|Ukupni zbroj|Broj ponovnog pokretanja čvorove|
|ReimagingNodeCount|Reimaging čvor Count|Count|Ukupni zbroj|Broj reimaging čvorove|
|RunningNodeCount|Brojanje čvor|Count|Ukupni zbroj|Broj aktivnih čvorove|
|LeavingPoolNodeCount|Napuštanje grupe aplikacija čvor Count|Count|Ukupni zbroj|Broj čvorove ostavite na resurse|
|UnusableNodeCount|Neupotrebljiv čvor Count|Count|Ukupni zbroj|Broj nestabilan čvorove|
|TaskStartEvent|Zadatak početka događaja|Count|Ukupni zbroj|Ukupan broj zadatke koje ste pokrenuli|
|TaskCompleteEvent|Zadatak dovršen događaja|Count|Ukupni zbroj|Ukupan broj zadatke koje ste dovršili|
|TaskFailEvent|Zadatak nije uspjelo događaja|Count|Ukupni zbroj|Ukupan broj zadatke koje ste završili rad u stanju nije uspio|
|PoolCreateEvent|Skup stvaranje događaja|Count|Ukupni zbroj|Ukupan broj grupe koje su stvorene|
|PoolResizeStartEvent|Skup promjene veličine početka događaja|Count|Ukupni zbroj|Ukupan broj mijenja veličinu skup koji ste započeli|
|PoolResizeCompleteEvent|Skup promjene veličine dovršeno događaja|Count|Ukupni zbroj|Ukupan broj mijenja veličinu grupe koje ste dovršili|
|PoolDeleteStartEvent|Skup Izbriši početka događaja|Count|Ukupni zbroj|Ukupan broj briše grupu koji ste započeli|
|PoolDeleteCompleteEvent|Skup Izbriši dovršeno događaja|Count|Ukupni zbroj|Ukupan broj briše grupu koju ste završili rad|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|connectedclients|Povezani klijenti|Count|Najviše||
|totalcommandsprocessed|Ukupna operacije|Count|Ukupni zbroj||
|cachehits|Pronalaženje objekata u predmemoriji|Count|Ukupni zbroj||
|cachemisses|Neuspjele akcije u predmemoriji|Count|Ukupni zbroj||
|getcommands|Dobiti|Count|Ukupni zbroj||
|setcommands|Skupovi|Count|Ukupni zbroj||
|evictedkeys|Izbaciti tipke|Count|Ukupni zbroj||
|totalkeys|Ukupna tipke|Count|Najviše||
|expiredkeys|Istekle tipke|Count|Ukupni zbroj||
|usedmemory|Korištene memorije|Bajtova|Najviše||
|usedmemoryRss|Korištene memorije RSS|Bajtova|Najviše||
|serverLoad|Poslužitelja|Posto|Najviše||
|cacheWrite|Pisanje predmemorije|BytesPerSecond|Najviše||
|cacheRead|Čitanje predmemorije|BytesPerSecond|Najviše||
|percentProcessorTime|PROCESOR|Posto|Najviše||
|connectedclients0|Povezani klijenti (Shard 0)|Count|Najviše||
|totalcommandsprocessed0|Ukupna operacije (Shard 0)|Count|Ukupni zbroj||
|cachehits0|Pronalaženje objekata u predmemoriji (Shard 0)|Count|Ukupni zbroj||
|cachemisses0|Neuspjele akcije u predmemoriji (Shard 0)|Count|Ukupni zbroj||
|getcommands0|Dohvaća (Shard 0)|Count|Ukupni zbroj||
|setcommands0|Skupovi (Shard 0)|Count|Ukupni zbroj||
|evictedkeys0|Strelicama izbaciti (Shard 0)|Count|Ukupni zbroj||
|totalkeys0|Ukupna tipke (čvor 0)|Count|Najviše||
|expiredkeys0|Istekle tipke (Shard 0)|Count|Ukupni zbroj||
|usedmemory0|Korištene memorije (Shard 0)|Bajtova|Najviše||
|usedmemoryRss0|Korištene memorije RSS (Shard 0)|Bajtova|Najviše||
|serverLoad0|Učitavanje Server (Shard 0)|Posto|Najviše||
|cacheWrite0|Pisanje predmemorije (Shard 0)|BytesPerSecond|Najviše||
|cacheRead0|Čitanje predmemorije (Shard 0)|BytesPerSecond|Najviše||
|percentProcessorTime0|Procesor (Shard 0)|Posto|Najviše||
|connectedclients1|Povezani klijenti (Shard 1)|Count|Najviše||
|totalcommandsprocessed1|Ukupna operacije (Shard 1)|Count|Ukupni zbroj||
|cachehits1|Pronalaženje objekata u predmemoriji (Shard 1)|Count|Ukupni zbroj||
|cachemisses1|Neuspjele akcije u predmemoriji (Shard 1)|Count|Ukupni zbroj||
|getcommands1|Dohvaća (Shard 1)|Count|Ukupni zbroj||
|setcommands1|Skupovi (Shard 1)|Count|Ukupni zbroj||
|evictedkeys1|Strelicama izbaciti (Shard 1)|Count|Ukupni zbroj||
|totalkeys1|Ukupna tipke (čvor 1)|Count|Najviše||
|expiredkeys1|Istekle tipke (Shard 1)|Count|Ukupni zbroj||
|usedmemory1|Korištene memorije (Shard 1)|Bajtova|Najviše||
|usedmemoryRss1|Korištene memorije RSS (Shard 1)|Bajtova|Najviše||
|serverLoad1|Učitavanje Server (Shard 1)|Posto|Najviše||
|cacheWrite1|Pisanje predmemorije (Shard 1)|BytesPerSecond|Najviše||
|cacheRead1|Čitanje predmemorije (Shard 1)|BytesPerSecond|Najviše||
|percentProcessorTime1|Procesor (Shard 1)|Posto|Najviše||
|connectedclients2|Povezani klijenti (Shard 2)|Count|Najviše||
|totalcommandsprocessed2|Ukupna operacije (Shard 2)|Count|Ukupni zbroj||
|cachehits2|Pronalaženje objekata u predmemoriji (Shard 2)|Count|Ukupni zbroj||
|cachemisses2|Neuspjele akcije u predmemoriji (Shard 2)|Count|Ukupni zbroj||
|getcommands2|Dohvaća (Shard 2)|Count|Ukupni zbroj||
|setcommands2|Skupovi (Shard 2)|Count|Ukupni zbroj||
|evictedkeys2|Strelicama izbaciti (Shard 2)|Count|Ukupni zbroj||
|totalkeys2|Ukupna tipke (čvor 2)|Count|Najviše||
|expiredkeys2|Istekle tipke (Shard 2)|Count|Ukupni zbroj||
|usedmemory2|Korištene memorije (Shard 2)|Bajtova|Najviše||
|usedmemoryRss2|Korištene memorije RSS (Shard 2)|Bajtova|Najviše||
|serverLoad2|Učitavanje Server (Shard 2)|Posto|Najviše||
|cacheWrite2|Pisanje predmemorije (Shard 2)|BytesPerSecond|Najviše||
|cacheRead2|Čitanje predmemorije (Shard 2)|BytesPerSecond|Najviše||
|percentProcessorTime2|Procesor (Shard 2)|Posto|Najviše||
|connectedclients3|Povezani klijenti (Shard 3)|Count|Najviše||
|totalcommandsprocessed3|Ukupna operacije (Shard 3)|Count|Ukupni zbroj||
|cachehits3|Pronalaženje objekata u predmemoriji (Shard 3)|Count|Ukupni zbroj||
|cachemisses3|Neuspjele akcije u predmemoriji (Shard 3)|Count|Ukupni zbroj||
|getcommands3|Dohvaća (Shard 3)|Count|Ukupni zbroj||
|setcommands3|Skupovi (Shard 3)|Count|Ukupni zbroj||
|evictedkeys3|Strelicama izbaciti (Shard 3)|Count|Ukupni zbroj||
|totalkeys3|Ukupna tipke (čvor 3)|Count|Najviše||
|expiredkeys3|Istekle tipke (Shard 3)|Count|Ukupni zbroj||
|usedmemory3|Korištene memorije (Shard 3)|Bajtova|Najviše||
|usedmemoryRss3|Korištene memorije RSS (Shard 3)|Bajtova|Najviše||
|serverLoad3|Učitavanje Server (Shard 3)|Posto|Najviše||
|cacheWrite3|Pisanje predmemorije (Shard 3)|BytesPerSecond|Najviše||
|cacheRead3|Čitanje predmemorije (Shard 3)|BytesPerSecond|Najviše||
|percentProcessorTime3|Procesor (Shard 3)|Posto|Najviše||
|connectedclients4|Povezani klijenti (Shard 4)|Count|Najviše||
|totalcommandsprocessed4|Ukupna operacije (Shard 4)|Count|Ukupni zbroj||
|cachehits4|Pronalaženje objekata u predmemoriji (Shard 4)|Count|Ukupni zbroj||
|cachemisses4|Neuspjele akcije u predmemoriji (Shard 4)|Count|Ukupni zbroj||
|getcommands4|Dohvaća (Shard 4)|Count|Ukupni zbroj||
|setcommands4|Skupovi (Shard 4)|Count|Ukupni zbroj||
|evictedkeys4|Strelicama izbaciti (Shard 4)|Count|Ukupni zbroj||
|totalkeys4|Ukupna tipke (čvor 4)|Count|Najviše||
|expiredkeys4|Istekle tipke (Shard 4)|Count|Ukupni zbroj||
|usedmemory4|Korištene memorije (Shard 4)|Bajtova|Najviše||
|usedmemoryRss4|Korištene memorije RSS (Shard 4)|Bajtova|Najviše||
|serverLoad4|Učitavanje Server (Shard 4)|Posto|Najviše||
|cacheWrite4|Pisanje predmemorije (Shard 4)|BytesPerSecond|Najviše||
|cacheRead4|Čitanje predmemorije (Shard 4)|BytesPerSecond|Najviše||
|percentProcessorTime4|Procesor (Shard 4)|Posto|Najviše||
|connectedclients5|Povezani klijenti (Shard 5)|Count|Najviše||
|totalcommandsprocessed5|Ukupna operacije (Shard 5)|Count|Ukupni zbroj||
|cachehits5|Pronalaženje objekata u predmemoriji (Shard 5)|Count|Ukupni zbroj||
|cachemisses5|Neuspjele akcije u predmemoriji (Shard 5)|Count|Ukupni zbroj||
|getcommands5|Dohvaća (Shard 5)|Count|Ukupni zbroj||
|setcommands5|Skupovi (Shard 5)|Count|Ukupni zbroj||
|evictedkeys5|Strelicama izbaciti (Shard 5)|Count|Ukupni zbroj||
|totalkeys5|Ukupna tipke (čvor 5)|Count|Najviše||
|expiredkeys5|Istekle tipke (Shard 5)|Count|Ukupni zbroj||
|usedmemory5|Korištene memorije (Shard 5)|Bajtova|Najviše||
|usedmemoryRss5|Korištene memorije RSS (Shard 5)|Bajtova|Najviše||
|serverLoad5|Učitavanje Server (Shard 5)|Posto|Najviše||
|cacheWrite5|Pisanje predmemorije (Shard 5)|BytesPerSecond|Najviše||
|cacheRead5|Čitanje predmemorije (Shard 5)|BytesPerSecond|Najviše||
|percentProcessorTime5|Procesor (Shard 5)|Posto|Najviše||
|connectedclients6|Povezani klijenti (Shard 6)|Count|Najviše||
|totalcommandsprocessed6|Ukupna operacije (Shard 6)|Count|Ukupni zbroj||
|cachehits6|Pronalaženje objekata u predmemoriji (Shard 6)|Count|Ukupni zbroj||
|cachemisses6|Neuspjele akcije u predmemoriji (Shard 6)|Count|Ukupni zbroj||
|getcommands6|Dohvaća (Shard 6)|Count|Ukupni zbroj||
|setcommands6|Skupovi (Shard 6)|Count|Ukupni zbroj||
|evictedkeys6|Strelicama izbaciti (Shard 6)|Count|Ukupni zbroj||
|totalkeys6|Ukupna tipke (čvor 6)|Count|Najviše||
|expiredkeys6|Istekle tipke (Shard 6)|Count|Ukupni zbroj||
|usedmemory6|Korištene memorije (Shard 6)|Bajtova|Najviše||
|usedmemoryRss6|Korištene memorije RSS (Shard 6)|Bajtova|Najviše||
|serverLoad6|Učitavanje Server (Shard 6)|Posto|Najviše||
|cacheWrite6|Pisanje predmemorije (Shard 6)|BytesPerSecond|Najviše||
|cacheRead6|Čitanje predmemorije (Shard 6)|BytesPerSecond|Najviše||
|percentProcessorTime6|Procesor (Shard 6)|Posto|Najviše||
|connectedclients7|Povezani klijenti (Shard 7)|Count|Najviše||
|totalcommandsprocessed7|Ukupna operacije (Shard 7)|Count|Ukupni zbroj||
|cachehits7|Pronalaženje objekata u predmemoriji (Shard 7)|Count|Ukupni zbroj||
|cachemisses7|Neuspjele akcije u predmemoriji (Shard 7)|Count|Ukupni zbroj||
|getcommands7|Dohvaća (Shard 7)|Count|Ukupni zbroj||
|setcommands7|Skupovi (Shard 7)|Count|Ukupni zbroj||
|evictedkeys7|Strelicama izbaciti (Shard 7)|Count|Ukupni zbroj||
|totalkeys7|Ukupna tipke (čvor 7)|Count|Najviše||
|expiredkeys7|Istekle tipke (Shard 7)|Count|Ukupni zbroj||
|usedmemory7|Korištene memorije (Shard 7)|Bajtova|Najviše||
|usedmemoryRss7|Korištene memorije RSS (Shard 7)|Bajtova|Najviše||
|serverLoad7|Učitavanje Server (Shard 7)|Posto|Najviše||
|cacheWrite7|Pisanje predmemorije (Shard 7)|BytesPerSecond|Najviše||
|cacheRead7|Čitanje predmemorije (Shard 7)|BytesPerSecond|Najviše||
|percentProcessorTime7|Procesor (Shard 7)|Posto|Najviše||
|connectedclients8|Povezani klijenti (Shard 8)|Count|Najviše||
|totalcommandsprocessed8|Ukupna operacije (Shard 8)|Count|Ukupni zbroj||
|cachehits8|Pronalaženje objekata u predmemoriji (Shard 8)|Count|Ukupni zbroj||
|cachemisses8|Neuspjele akcije u predmemoriji (Shard 8)|Count|Ukupni zbroj||
|getcommands8|Dohvaća (Shard 8)|Count|Ukupni zbroj||
|setcommands8|Skupovi (Shard 8)|Count|Ukupni zbroj||
|evictedkeys8|Strelicama izbaciti (Shard 8)|Count|Ukupni zbroj||
|totalkeys8|Ukupna tipke (čvor 8)|Count|Najviše||
|expiredkeys8|Istekle tipke (Shard 8)|Count|Ukupni zbroj||
|usedmemory8|Korištene memorije (Shard 8)|Bajtova|Najviše||
|usedmemoryRss8|Korištene memorije RSS (Shard 8)|Bajtova|Najviše||
|serverLoad8|Učitavanje Server (Shard 8)|Posto|Najviše||
|cacheWrite8|Pisanje predmemorije (Shard 8)|BytesPerSecond|Najviše||
|cacheRead8|Čitanje predmemorije (Shard 8)|BytesPerSecond|Najviše||
|percentProcessorTime8|Procesor (Shard 8)|Posto|Najviše||
|connectedclients9|Povezani klijenti (Shard 9)|Count|Najviše||
|totalcommandsprocessed9|Ukupna operacije (Shard 9)|Count|Ukupni zbroj||
|cachehits9|Pronalaženje objekata u predmemoriji (Shard 9)|Count|Ukupni zbroj||
|cachemisses9|Neuspjele akcije u predmemoriji (Shard 9)|Count|Ukupni zbroj||
|getcommands9|Dohvaća (Shard 9)|Count|Ukupni zbroj||
|setcommands9|Skupovi (Shard 9)|Count|Ukupni zbroj||
|evictedkeys9|Strelicama izbaciti (Shard 9)|Count|Ukupni zbroj||
|totalkeys9|Ukupna tipke (čvor 9)|Count|Najviše||
|expiredkeys9|Istekle tipke (Shard 9)|Count|Ukupni zbroj||
|usedmemory9|Korištene memorije (Shard 9)|Bajtova|Najviše||
|usedmemoryRss9|Korištene memorije RSS (Shard 9)|Bajtova|Najviše||
|serverLoad9|Učitavanje Server (Shard 9)|Posto|Najviše||
|cacheWrite9|Pisanje predmemorije (Shard 9)|BytesPerSecond|Najviše||
|cacheRead9|Čitanje predmemorije (Shard 9)|BytesPerSecond|Najviše||
|percentProcessorTime9|Procesor (Shard 9)|Posto|Najviše||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|NumberOfCalls|Ukupan broj API poziva|Count|Ukupni zbroj|Ukupan broj API poziva.|
|NumberOfFailedCalls|Ukupan broj neuspjelih API poziva|Count|Ukupni zbroj|Ukupan broj neuspjelih API poziva.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|Postotak CPU-a|Postotak CPU-a|Posto|Prosjek|Postotak dodijeljene računalnim jedinice koje su trenutno koristi na Virtual Machine(s)|
|Mrežni u|Mrežni u|Bajtova|Ukupni zbroj|Broja bajtova na svim mreže primio Virtual Machine(s) (promet)|
|Izvan mreže|Izvan mreže|Bajtova|Ukupni zbroj|Broja bajtova smanjivati na svim mrežu tako da Virtual Machine(s) (odlazni promet)|
|Na disku čitanje bajtova|Na disku čitanje bajtova|Bajtova|Ukupni zbroj|Ukupno bajtova tijekom razdoblja za nadzor čitanje s diska|
|Na disku pisanje bajtova|Na disku pisanje bajtova|Bajtova|Ukupni zbroj|Ukupno bajtova napisan na disk tijekom razdoblja za nadzor|
|Disk Čitaj operacije/Sec|Disk Čitaj operacije/Sec|CountPerSecond|Prosjek|Čitanje disk IOPS|
|Operacije/Sec pisanje na disku|Operacije/Sec pisanje na disku|CountPerSecond|Prosjek|Pisanje na disku IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|Postotak CPU-a|Postotak CPU-a|Posto|Prosjek|Postotak dodijeljene računalnim jedinice koje su trenutno koristi na Virtual Machine(s)|
|Mrežni u|Mrežni u|Bajtova|Ukupni zbroj|Broja bajtova na svim mreže primio Virtual Machine(s) (promet)|
|Izvan mreže|Izvan mreže|Bajtova|Ukupni zbroj|Broja bajtova smanjivati na svim mrežu tako da Virtual Machine(s) (odlazni promet)|
|Na disku čitanje bajtova|Na disku čitanje bajtova|Bajtova|Ukupni zbroj|Ukupno bajtova tijekom razdoblja za nadzor čitanje s diska|
|Na disku pisanje bajtova|Na disku pisanje bajtova|Bajtova|Ukupni zbroj|Ukupno bajtova napisan na disk tijekom razdoblja za nadzor|
|Disk Čitaj operacije/Sec|Disk Čitaj operacije/Sec|CountPerSecond|Prosjek|Čitanje disk IOPS|
|Operacije/Sec pisanje na disku|Operacije/Sec pisanje na disku|CountPerSecond|Prosjek|Pisanje na disku IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|Postotak CPU-a|Postotak CPU-a|Posto|Prosjek|Postotak dodijeljene računalnim jedinice koje su trenutno koristi na Virtual Machine(s)|
|Mrežni u|Mrežni u|Bajtova|Ukupni zbroj|Broja bajtova na svim mreže primio Virtual Machine(s) (promet)|
|Izvan mreže|Izvan mreže|Bajtova|Ukupni zbroj|Broja bajtova smanjivati na svim mrežu tako da Virtual Machine(s) (odlazni promet)|
|Na disku čitanje bajtova|Na disku čitanje bajtova|Bajtova|Ukupni zbroj|Ukupno bajtova tijekom razdoblja za nadzor čitanje s diska|
|Na disku pisanje bajtova|Na disku pisanje bajtova|Bajtova|Ukupni zbroj|Ukupno bajtova napisan na disk tijekom razdoblja za nadzor|
|Disk Čitaj operacije/Sec|Disk Čitaj operacije/Sec|CountPerSecond|Prosjek|Čitanje disk IOPS|
|Operacije/Sec pisanje na disku|Operacije/Sec pisanje na disku|CountPerSecond|Prosjek|Pisanje na disku IOPS|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Prilikom pokušaja slanja poruka za telemetriju|Count|Ukupni zbroj|Pokušaj broj uređaja na Cloud telemetrijskih poruke slati koncentratora za IoT|
|d2c.telemetry.ingress.Success|Telemetrijskih poruke poslane|Count|Ukupni zbroj|Broj uređaja na Oblaku telemetrijskih poruke poslane uspješno koncentratora za IoT|
|c2d.Commands.egress.Complete.Success|Naredbe dovršeno|Count|Ukupni zbroj|Broj oblaka naredbama uređaj uspješno dovršena uređaj|
|c2d.Commands.egress.abandon.Success|Naredbe prekinuto|Count|Ukupni zbroj|Broj oblaka uređaj naredbama napuštenih uređaj|
|c2d.Commands.egress.reject.Success|Naredbe odbijena|Count|Ukupni zbroj|Broj oblaka naredbama uređaja odbio uređaj|
|devices.totalDevices|Ukupna uređaja|Count|Ukupni zbroj|Broj uređaja registrirana koncentratora za IoT|
|devices.connectedDevices.allProtocol|Povezani uređaji|Count|Ukupni zbroj|Broj uređaja povezanih s koncentratora za IoT|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|INREQS|Zahtjevi za razgovore|Count|Ukupni zbroj|Događaj koncentrator dolazne poruke propusnost za prostor naziva|
|SUCCREQ|Uspješnih zahtjeva|Count|Ukupni zbroj|Ukupna uspješnih zahtjeva za prostor naziva|
|FAILREQ|Nije uspjelo zahtjeva|Count|Ukupni zbroj|Ukupno neuspjelih zahtjeva za prostor naziva|
|SVRBSY|Zauzet pogreške na poslužitelju|Count|Ukupni zbroj|Ukupna poslužitelj zauzet pogreške za prostor naziva|
|INTERR|Pogreške internom poslužitelju|Count|Ukupni zbroj|Ukupna internom poslužitelju pogreške za prostor naziva|
|MISCERR|Druge pogreške|Count|Ukupni zbroj|Ukupno neuspjelih zahtjeva za prostor naziva|
|INMSGS|Dolazne poruke|Count|Ukupni zbroj|Ukupna dolaznih poruka radi prostor naziva|
|OUTMSGS|Odlaznih poruka|Count|Ukupni zbroj|Ukupno odlazne poruke za prostor naziva|
|EHINMBS|Dolazne bajtova u sekundi|BytesPerSecond|Ukupni zbroj|Događaj koncentrator dolazne poruke propusnost za prostor naziva|
|EHOUTMBS|Izlazni bajtova u sekundi|BytesPerSecond|Ukupni zbroj|Ukupno odlazne poruke za prostor naziva|
|EHABL|Arhiviranje zaostale poruka|Count|Ukupni zbroj|Arhiviranje poruka koncentrator događaja u zaostale za prostor naziva|
|EHAMSGS|Arhiviranje poruka|Count|Ukupni zbroj|Koncentrator događaj arhiviraju poruke u prostor naziva|
|EHAMBS|Arhiviranje poruka propusnost|BytesPerSecond|Ukupni zbroj|Koncentrator događaj arhiviraju propusnost poruku u prostor naziva|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|RunsStarted|Pokreće rada|Count|Ukupni zbroj|Broj tijek rada pokreće rada.|
|RunsCompleted|Pokreće dovršeno|Count|Ukupni zbroj|Broj tijek rada pokreće dovršeno.|
|RunsSucceeded|Pokreće uspjela|Count|Ukupni zbroj|Broj tijek rada pokreće je uspjela.|
|RunsFailed|Pokreće nije uspjela|Count|Ukupni zbroj|Broj tijek rada pokreće nije uspjelo.|
|RunsCancelled|Pokreće otkazana|Count|Ukupni zbroj|Broj tijek rada pokreće otkazani.|
|RunLatency|Pokretanje Latencija|Sekundi|Prosjek|Latencija dovršenom tijek rada izvodi.|
|RunSuccessLatency|Pokretanje Latencija uspjeh|Sekundi|Prosjek|Kašnjenje je uspjela tijek rada izvodi.|
|RunThrottledEvents|Pokretanje ograničeni događaja|Count|Ukupni zbroj|Broj akcija tijeka rada ili okidača ograničio vrijeme događaja.|
|RunFailurePercentage|Pokretanje postotka pogreške|Posto|Ukupni zbroj|Postotak tijek rada pokreće nije uspjelo.|
|ActionsStarted|Akcije pokrenut |Count|Ukupni zbroj|Broj akcije tijeka rada.|
|ActionsCompleted|Akcije dovršeno |Count|Ukupni zbroj|Broj akcija tijeka rada je dovršen.|
|ActionsSucceeded|Akcije uspjela |Count|Ukupni zbroj|Broj akcija tijeka rada je uspio.|
|ActionsFailed|Akcija nije uspjela|Count|Ukupni zbroj|Broj akcija tijeka rada nije uspjelo.|
|ActionsSkipped|Akcije preskočeno |Count|Ukupni zbroj|Broj akcija tijeka rada preskočiti.|
|ActionLatency|Latencija akcija |Sekundi|Prosjek|Latencija dovršene radnje.|
|ActionSuccessLatency|Latencija uspjeh akcija |Sekundi|Prosjek|Kašnjenje je uspjela radnje.|
|ActionThrottledEvents|Akcija ograničio vrijeme događaja|Count|Ukupni zbroj|Broj akcija tijeka rada ograničio vrijeme događaja...|
|TriggersStarted|Okidača rada |Count|Ukupni zbroj|Broj okidača tijeka rada.|
|TriggersCompleted|Okidača dovršeno |Count|Ukupni zbroj|Broj tijeka rada okidača dovršeno.|
|TriggersSucceeded|Okidača uspjela |Count|Ukupni zbroj|Broj okidača tijek rada je uspio.|
|TriggersFailed|Okidača nije uspjela |Count|Ukupni zbroj|Broj okidača tijeka rada nije uspjelo.|
|TriggersSkipped|Okidača preskočeno|Count|Ukupni zbroj|Broj tijeka rada okidača preskočiti.|
|TriggersFired|Okidača pokrenuta |Count|Ukupni zbroj|Broj okidača tijeka rada pokrenuta.|
|TriggerLatency|Latencija okidača |Sekundi|Prosjek|Latencija okidača dovršene tijeka rada.|
|TriggerFireLatency|Latencija Fire okidača |Sekundi|Prosjek|Latencija okidača pokrenuta tijeka rada.|
|TriggerSuccessLatency|Latencija uspjeh okidača |Sekundi|Prosjek|Latencija uspjela okidača tijeka rada.|
|TriggerThrottledEvents|Pokretanje ograničeni događaja|Count|Ukupni zbroj|Broj pokretanje tijeka rada ograničio vrijeme događaja.|
|BillableActionExecutions|Izvršavanja naplatu akcija|Count|Ukupni zbroj|Broj početak naplatiti je izvršavanja akcija tijeka rada.|
|BillableTriggerExecutions|Izvršavanja naplatu okidača|Count|Ukupni zbroj|Broj izvršavanja za pokretanje tijeka rada početak naplatiti.|
|TotalBillableExecutions|Ukupna izvršavanja za naplatu|Count|Ukupni zbroj|Broj izvršavanja tijeka rada za početak naplatiti.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|Propusnost|Propusnost|BytesPerSecond|Prosjek||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|SearchLatency|Latencija pretraživanja|Sekundi|Prosjek|Prosječna pretraživanje Latencija servisa za pretraživanje|
|SearchQueriesPerSecond|Upita za pretraživanje u sekundi|CountPerSecond|Prosjek|Upita za pretraživanje u sekundi servisa za pretraživanje|
|ThrottledSearchQueriesPercentage|Postotak upita ograničeni pretraživanja|Posto|Prosjek|Postotak upita za pretraživanje koji su ograničio vrijeme servisa za pretraživanje|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|CPUXNS|Korištenje središnjih procesora po prostor naziva|Posto|Najviše|Servis bus premium prostor naziva procesora korištenje mjerenja|
|WSXNS|Veličina memorije po prostor naziva|Posto|Najviše|Servis bus premium prostor naziva memorije korištenje mjerenja|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|cpu_percent|Postotak CPU-a|Posto|Prosjek|Postotak CPU-a|
|physical_data_read_percent|Postotak IO podataka|Posto|Prosjek|Postotak IO podataka|
|log_write_percent|Postotak IO zapisnika|Posto|Prosjek|Postotak IO zapisnika|
|dtu_consumption_percent|Postotak DTU|Posto|Prosjek|Postotak DTU|
|prostor za pohranu|Ukupnu veličinu baze podataka|Bajtova|Najviše|Ukupnu veličinu baze podataka|
|connection_successful|Uspješan veze|Count|Ukupni zbroj|Uspješan veze|
|connection_failed|Nije uspjelo veze|Count|Ukupni zbroj|Nije uspjelo veze|
|blocked_by_firewall|Vatrozid blokira|Count|Ukupni zbroj|Vatrozid blokira|
|zastoja zbog|Deadlocks|Count|Ukupni zbroj|Deadlocks|
|storage_percent|Postotak veličine baze podataka|Posto|Najviše|Postotak veličine baze podataka|
|xtp_storage_percent|Percent(Preview) OLTP u memoriji za pohranu|Posto|Prosjek|Percent(Preview) OLTP u memoriji za pohranu|
|workers_percent|Zaposlenici zaduženi za postotak|Posto|Prosjek|Zaposlenici zaduženi za posto|
|sessions_percent|Sesije posto|Posto|Prosjek|Sesije posto|
|dtu_limit|Ograničenje DTU|Count|Prosjek|Ograničenje DTU|
|dtu_used|DTU koristi|Count|Prosjek|DTU koristi|
|service_level_objective|Servis razine cilj baze podataka|Count|Ukupni zbroj|Servis razine cilj baze podataka|
|dwu_limit|ograničenje dwu|Count|Najviše|ograničenje dwu|
|dwu_consumption_percent|Postotak DWU|Posto|Prosjek|Postotak DWU|
|dwu_used|DWU koristi|Count|Prosjek|DWU koristi|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|cpu_percent|Postotak CPU-a|Posto|Prosjek|Postotak CPU-a|
|physical_data_read_percent|Postotak IO podataka|Posto|Prosjek|Postotak IO podataka|
|log_write_percent|Postotak IO zapisnika|Posto|Prosjek|Postotak IO zapisnika|
|dtu_consumption_percent|Postotak DTU|Posto|Prosjek|Postotak DTU|
|storage_percent|Postotak prostora za pohranu|Posto|Prosjek|Postotak prostora za pohranu|
|workers_percent|Zaposlenici zaduženi za posto|Posto|Prosjek|Zaposlenici zaduženi za posto|
|sessions_percent|Sesije posto|Posto|Prosjek|Sesije posto|
|eDTU_limit|ograničenje eDTU|Count|Prosjek|ograničenje eDTU|
|storage_limit|Ograničenje prostora za pohranu|Bajtova|Prosjek|Ograničenje prostora za pohranu|
|eDTU_used|eDTU koristi|Count|Prosjek|eDTU koristi|
|storage_used|Prostor za pohranu koriste|Bajtova|Prosjek|Prostor za pohranu koriste|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|ResourceUtilization|Upotreba SU %|Posto|Najviše|Upotreba SU %|
|InputEvents|Unos događaja|Count|Ukupni zbroj|Unos događaja|
|InputEventBytes|Unos bajtova događaja|Bajtova|Ukupni zbroj|Unos bajtova događaja|
|LateInputEvents|Najnovije unos događaja|Count|Ukupni zbroj|Najnovije unos događaja|
|OutputEvents|Izlaz događaja|Count|Ukupni zbroj|Izlaz događaja|
|ConversionErrors|Pogreške pri pretvaranju podataka|Count|Ukupni zbroj|Pogreške pri pretvaranju podataka|
|Pogreške|Pogreške prilikom izvođenja|Count|Ukupni zbroj|Pogreške prilikom izvođenja|
|DroppedOrAdjustedEvents|Izvan redoslijeda događaja|Count|Ukupni zbroj|Izvan redoslijeda događaja|
|AMLCalloutRequests|Zahtjevi za (funkcija)|Count|Ukupni zbroj|Zahtjevi za (funkcija)|
|AMLCalloutFailedRequests|Zahtjeva nije uspjelo (funkcija)|Count|Ukupni zbroj|Zahtjeva nije uspjelo (funkcija)|
|AMLCalloutInputEvents|Funkcija događaja|Count|Ukupni zbroj|Funkcija događaja|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|CpuPercentage|Postotak CPU-a|Posto|Prosjek|Postotak CPU-a|
|MemoryPercentage|Postotak memorije|Posto|Prosjek|Postotak memorije|
|DiskQueueLength|Duljina reda čekanja na disku|Count|Ukupni zbroj|Duljina reda čekanja na disku|
|HttpQueueLength|Duljina reda čekanja http|Count|Ukupni zbroj|Duljina reda čekanja http|
|BytesReceived|Podaci u|Bajtova|Ukupni zbroj|Podaci u|
|BytesSent|Podataka|Bajtova|Ukupni zbroj|Podataka|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|CpuTime|Vrijeme procesora|Sekundi|Ukupni zbroj|Vrijeme procesora|
|Zahtjevi za|Zahtjevi za|Count|Ukupni zbroj|Zahtjevi za|
|BytesReceived|Podaci u|Bajtova|Ukupni zbroj|Podaci u|
|BytesSent|Podataka|Bajtova|Ukupni zbroj|Podataka|
|Http2xx|HTTP 2xx|Count|Ukupni zbroj|HTTP 2xx|
|Http3xx|HTTP 3xx|Count|Ukupni zbroj|HTTP 3xx|
|Http401|HTTP 401|Count|Ukupni zbroj|HTTP 401|
|Http403|HTTP 403|Count|Ukupni zbroj|HTTP 403|
|Http404|HTTP 404|Count|Ukupni zbroj|HTTP 404|
|Http406|HTTP 406|Count|Ukupni zbroj|HTTP 406|
|Http4xx|HTTP 4xx|Count|Ukupni zbroj|HTTP 4xx|
|Http5xx|HTTP poslužitelj pogreške|Count|Ukupni zbroj|HTTP poslužitelj pogreške|
|MemoryWorkingSet|Radni skup memorije|Bajtova|Ukupni zbroj|Radni skup memorije|
|AverageMemoryWorkingSet|Prosječna memorija radni skup|Bajtova|Prosjek|Prosječna memorija radni skup|
|AverageResponseTime|Prosječno vrijeme odaziva|Sekundi|Prosjek|Prosječno vrijeme odaziva|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Mjerenja|Metričkim zaslonsko ime|Jedinica|Vrsta prikupljanja|Opis|
|---|---|---|---|---|
|CpuTime|Vrijeme procesora|Sekundi|Ukupni zbroj|Vrijeme procesora|
|Zahtjevi za|Zahtjevi za|Count|Ukupni zbroj|Zahtjevi za|
|BytesReceived|Podaci u|Bajtova|Ukupni zbroj|Podaci u|
|BytesSent|Podataka|Bajtova|Ukupni zbroj|Podataka|
|Http2xx|HTTP 2xx|Count|Ukupni zbroj|HTTP 2xx|
|Http3xx|HTTP 3xx|Count|Ukupni zbroj|HTTP 3xx|
|Http401|HTTP 401|Count|Ukupni zbroj|HTTP 401|
|Http403|HTTP 403|Count|Ukupni zbroj|HTTP 403|
|Http404|HTTP 404|Count|Ukupni zbroj|HTTP 404|
|Http406|HTTP 406|Count|Ukupni zbroj|HTTP 406|
|Http4xx|HTTP 4xx|Count|Ukupni zbroj|HTTP 4xx|
|Http5xx|HTTP poslužitelj pogreške|Count|Ukupni zbroj|HTTP poslužitelj pogreške|
|MemoryWorkingSet|Radni skup memorije|Bajtova|Ukupni zbroj|Radni skup memorije|
|AverageMemoryWorkingSet|Prosječna memorija radni skup|Bajtova|Prosjek|Prosječna memorija radni skup|
|AverageResponseTime|Prosječno vrijeme odaziva|Sekundi|Prosjek|Prosječno vrijeme odaziva|

## <a name="next-steps"></a>Daljnji koraci

- [Saznajte više o mjerenja monitoru za Azure](./monitoring-overview.md#monitoring-sources)
- [Stvaranje upozorenja na mjerenja](./insights-receive-alert-notifications.md)
- [Izvoz metriku prostora za pohranu, koncentrator događaj ili analize zapisnika](./monitoring-overview-of-diagnostic-logs.md)
