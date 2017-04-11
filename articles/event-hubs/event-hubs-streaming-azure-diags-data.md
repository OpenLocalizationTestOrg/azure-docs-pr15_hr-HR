<properties
    pageTitle="Strujanje Azure Dijagnostika podataka u tipkovni put pomoću događaja koncentratora | Microsoft Azure"
    description="Prikazuje kako konfigurirati Dijagnostika Azure s koncentratorima događaj iz završetka na kraju, uključujući upute za uobičajeni scenariji."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Strujanje Azure Dijagnostika podataka u tipkovni put putem koncentratora događaja

Azure Dijagnostika načina fleksibilne mogli ste prikupljati metriku i zapisnike iz oblaka services virtualnim strojevima (VMs) i prijenos rezultate Azure prostora za pohranu. Pokretanje u vremenskom okviru ožujak 2016 (SDK 2.9), možete sita Dijagnostika s izvorima podataka potpuno prilagođene i prijenos podataka tipkovni put u sekundama pomoću [Azure događaj koncentratora](https://azure.microsoft.com/services/event-hubs/).

Podržane vrste podataka obuhvaćaju sljedeće:

- Događaji događaj praćenje za sustav Windows (ETW)
- Mjerača performansi
- Zapisnike događaja sustava Windows
- Zapisnici aplikacije
- Azure zapisnika infrastrukture za dijagnostiku

U ovom se članku objašnjava konfiguriranje Dijagnostika Azure s koncentratorima događaj iz end da biste završetka. Za sljedeće uobičajeni scenariji i daju upute:

- Kako prilagoditi zapisnicima i metriku koja se sinked s koncentratorima događaja
- Kako promijeniti konfiguracije u svakom okruženju
- Kako prikazati događaj koncentratora toka podataka
- Otklanjanje poteškoća s veza  

## <a name="prerequisites"></a>Preduvjeti

Događaj koncentratora sinking u Azure Dijagnostika podržava servise u Oblaku, VMs, skupovi skaliranje virtualnog računala i servisa tkanina početne u Azure SDK 2.9 i odgovarajuće Azure alate za Visual Studio.

- Proširenje Azure Dijagnostika 1.6 ([Azure SDK za .NET 2.9 ili noviji](https://azure.microsoft.com/downloads/) pronalaze to po zadanom)
- [Visual Studio 2013 ili noviji](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Postojeće konfiguracije Dijagnostika Azure u aplikaciji pomoću datoteke *.wadcfgx* i jedan od sljedećih načina:
    - Visual Studio: [Konfiguriranje Dijagnostika za servise u Oblaku Azure i virtualnih računala](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Komponente Windows PowerShell: [Omogućivanje Dijagnostika na servise u Oblaku Azure pomoću komponente PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)
- Prostor naziva koncentratora događaj dodjeli po u članku [Početak rada s koncentratorima događaja](./event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Povezivanje Azure Dijagnostika primatelj koncentratora događaja

Azure Dijagnostika uvijek primatelja zapisnika i metrike prema zadanim postavkama, s računom za Azure prostora za pohranu. Aplikacija možda dodatno sita s koncentratorima događaj tako da dodate nove sekcije **Primatelji** element **WadCfg** u odjeljku **PublicConfig** *.wadcfgx* datoteke. U Visual Studio na *.wadcfgx* je datoteka spremljena u sljedeći put: **Oblaka servisa Project** > **uloge** >  **(naziv uloge)** > **diagnostics.wadcfgx** datoteku.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

U ovom primjeru URL koncentratora događaj postavljena na potpuno kvalificiranih naziva središtu događaj: prostora za naziv događaja koncentratora + "/" + naziv događaja koncentratora.  

URL koncentratora događaja prikazuje se na [portal za Azure](http://go.microsoft.com/fwlink/?LinkID=213885) na nadzornoj ploči za događaj koncentratora.  

Naziv **sita** moguće je postaviti za bilo koji valjani niz pod uvjetom da dosljedno koristi iste vrijednosti u konfiguracijskoj datoteci.

> [AZURE.NOTE]  Mogu postojati i dodatni primatelji, kao što su *applicationInsights* konfiguriran u ovom odjeljku. Azure dijagnostika omogućuje jednu ili više primatelji se definirati ako svaki primatelj i deklarirani u odjeljku **PrivateConfig** .  

Primatelj događaja koncentratora morate deklarirati i definirano u odjeljku **PrivateConfig** *.wadcfgx* konfiguracijskoj datoteci.

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

Na `SharedAccessKeyName` vrijednost se mora podudarati ključ zajednički pristup potpis (SAS) i pravila koji sadrži definirani u prostor za naziv **Događaja koncentratora** . Idite na nadzornoj ploči događaj koncentratora [Azure portal](https://manage.windowsazure.com), kliknite karticu **Konfiguracija** i postaviti imenovani pravilo (na primjer, "SendRule") koja ima dozvole za *Slanje* . **StorageAccount** i prijavljenom u **PrivateConfig**. Nema potrebe da biste promijenili vrijednosti ovdje ako rade. U ovom primjeru smo ostavite vrijednosti prazno, koji se prijave da do resursa postavite vrijednosti. Ako, na primjer, *ServiceConfiguration.Cloud.cscfg* okruženje konfiguracijska datoteka postavlja okruženje odgovarajuće nazive i ključevi.  

> [AZURE.WARNING] Tipku događaj koncentratora SAS je pohranjena u obliku običnog teksta u datoteci *.wadcfgx* . Često ovog ključa prijavljene u kontrolu izvorišnog koda ili nije dostupan kao sredstvo na poslužitelj za sastavljanje tako da ga zaštititi po potrebi. Preporučujemo da koristite tipke SAS s dozvolama za *slanje samo* da je zlonamjeran korisnik mogli pisati u središtu događaj, ali ne preslušavanje ga ili upravljati.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Konfiguriranje Azure dijagnostičkog zapisnika i metriku za sita s koncentratorima događaja

Kao spomenuli, sve zadane i prilagođene Dijagnostika podatke, odnosno metriku i zapisnici se automatski sinked Azure prostora za pohranu konfigurirani vremenskim razmacima. S koncentratorima događaja i sve dodatne primatelj, možete odrediti jedan čvor korijen ili lista u hijerarhiji biti sinked s koncentratora za događaj. To obuhvaća ETW događaje, mjerača performansi, zapisnika događaja sustava Windows i aplikacije zapisnika.   

Imajte na umu kako puno podatkovnih točaka treba zapravo prenijeti s koncentratorima događaj je. Obično razvojnim inženjerima prenijeti najniža Latencija tipkovne put podataka koji se moraju biti potrošena i brzo interpretiraju. Sustavi za praćenje upozorenja ili pravila za automatsko skaliranje su primjeri. Razvojni inženjer možda konfiguriranje programa spremište zamjenski analizu i pretraživanje spremište –, na primjer, Azure strujanje analize, Elasticsearch, prilagođene nadzornim sustavom ili omiljene nadzornim sustavom od drugih korisnika.

Slijedi nekoliko primjera konfiguracije.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

U sljedećem primjeru primatelj primjenjuje se na nadređeni čvor **PerformanceCounters** u hijerarhiji, što znači da je sve podređene **PerformanceCounters** poslat će se događaj koncentratora.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

U prethodnom primjeru, primatelj primjenjuje se na samo tri mjerača: **Zahtjevi u redu čekanja**, **Odbijena zahtjeve**i **% procesor vrijeme**.  

Sljedeći primjer prikazuje način na koji možete razvojni inženjer ograničiti količinu podataka koji se šalju se ključnih metrike koji se koriste za taj servis stanja.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

U ovom primjeru primatelj primjenjuje se na zapisnika i filtriran samo na razini Prati pogreške.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Implementacija i ažuriranje servise u Oblaku aplikacije i dijagnostici config

Visual Studio nudi najjednostavniji put za implementaciju aplikacije i konfiguracija primatelj koncentratora za događaj. Prikaz i uređivanje datoteka, otvorite datoteku *.wadcfgx* u Visual Studio, uređivati i spremite je. Put je **Oblaka servisa Project** > **uloge** > **(naziv uloge)** > **diagnostics.wadcfgx**.  

Sada sve uvođenje i implementaciju ažuriranje akcije u Visual Studio, Visual Studio Team System, i sve naredbe ili skripte koje se temelje na MSBuild i njihovo korištenje u **/t: objavljivanje** cilj obuhvatiti *.wadcfgx* postupka pakiranja. Osim toga, implementacije i ažuriranja implementacija datoteku Azure pomoću odgovarajuće proširenje agent Dijagnostika Azure na vašem VMs.

Nakon implementacije aplikacije i konfiguraciji Azure Dijagnostika, vidjet ćete odmah aktivnosti na nadzornoj ploči od koncentratora za događaj. To znači da ste spremni prijeđite na prikaz podataka tipkovne put u klijentu ili analize alat ga slušatelj po izboru.  

Na sljedećoj slici na nadzornoj ploči koncentratora događaja prikazuje dobar slanja podataka Dijagnostika koncentrator događaj pokretanje tijekom nakon 11 Poslijepodne. Kada je aplikacija je uveden s datotekom ažurirane *.wadcfgx* i ispravno konfigurirano primatelj.

![][0]  

> [AZURE.NOTE] Kada unesete ažuriranja Azure Dijagnostika konfiguracijskoj datoteci (.wadcfgx), preporučuje se da vam automatske ažuriranja cijelu aplikaciju, kao i konfiguraciji pomoću Visual Studio objavljivanje ili skripte komponente Windows PowerShell.  

## <a name="view-hot-path-data"></a>Prikaz tipkovne put podataka

Kako je opisano prethodno, postoje mnogim slučajevima koristi za slušanje i obrada podataka koncentratora za događaj.

Jedan jednostavan pristup tako da stvorite program konzole small test preslušavanje koncentratora za događaj i ispišite strujanje Izlaz. Možete postaviti sljedeći kod koji je objašnjeno u detaljnije [Početak rada s koncentratorima događaj](./event-hubs-csharp-ephcs-getstarted.md)), u aplikaciji konzolu.  

Imajte na umu aplikacije konzole mora sadržavati [događaj procesor glavno računalo Nuget paketa](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Imajte na umu da biste zamijenili vrijednosti u uglate zagrade u funkciji **glavne** vrijednosti za resurse.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Otklanjanje poteškoća s primatelj koncentratora događaja

- Koncentrator događaj prikažite dolaznim ili odlaznim događaj aktivnosti prema očekivanjima.

    Provjerite je li koncentratora za događaj uspješno dodjeli. Sve informacije veze u odjeljku **PrivateConfig** *.wadcfgx* mora se podudarati s vrijednostima resursa kao što se vidi na portalu. Provjerite je li na portalu imaju SAS pravila definirani ("SendRule" u ovom primjeru) i da je dodijelio dozvolu za *Slanje* .  

- Nakon ažuriranja, središtu događaj više ne prikazuje aktivnosti dolaznim ili odlaznim događaj.

    Najprije provjerite je li informacije koncentratora za događaj i konfiguraciji točan kao što je prethodno opisano. Ponekad **PrivateConfig** je ponovno postaviti ažuriranjem za implementaciju. Preporučena popravak je promjene svih *.wadcfgx* u projektu i automatske dovršeno aplikacije ažuriranja. Ako to nije moguće, provjerite je li ažuriranje Dijagnostika ih gura dovršeno **PrivateConfig** koji uključuje tipku SAS.  

- Pokušao sam prijedloge, a središtu događaj još uvijek ne funkcionira.

    U prostor za pohranu Azure tablicu koja sadrži zapisnika i pogrešaka za dijagnostiku Azure sam: **WADDiagnosticInfrastructureLogsTable**. Jedan je pomoću alata kao što su [Explorer Azure prostora za pohranu](http://www.storageexplorer.com) za povezivanje s ovim računom za pohranu i prikaz u ovoj su tablici te dodavanje upita za vremenske oznake u zadnja 24 sata. Možete koristiti alat za izvoz .csv datoteke, a zatim ga otvorite u aplikaciji, kao što je Microsoft Excel. Excel olakšava traženje nizova Pozivna kartica, kao što su **EventHubs**, da biste vidjeli koje se pogreške prijavljuje.  

## <a name="next-steps"></a>Daljnji koraci

• [Dodatne informacije o događajima koncentratora](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Dodatak: Dovršavanje primjer datoteke (.wadcfgx) konfiguracije Dijagnostika Azure

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Komplementarna *ServiceConfiguration.Cloud.cscfg* da bi ovaj primjer izgleda ovako.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
