<properties
    pageTitle="Kako koristiti Azure Dijagnostika (.NET) uz servise u Oblaku | Microsoft Azure"
    description="Korištenje Azure Dijagnostika za prikupljanje podataka iz servisa Azure oblaka za ispravljanje pogrešaka, mjerenje performanse, nadzor, promet analizu i više."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Omogućivanje Azure Dijagnostika na servise u Oblaku za Azure

Pozadine na Azure Dijagnostika potražite u članku [Pregled Dijagnostika Azure](../azure-diagnostics.md) .


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Kako omogućiti i dijagnostiku u ulogu suradnika

Ovaj vodič kroz opisuje kako implementirati ulogu Azure tempiranja koji emits telemetrijskih podataka pomoću .NET EventSource predmete. Azure Dijagnostika koristi se za prikupljanje telemetrijskih podataka i spremite ga na račun za Azure prostora za pohranu. Prilikom stvaranja ulogu suradnika Visual Studio automatski omogućuje Dijagnostika 1.0 kao dio rješenja u SDK-ovi Azure za .NET 2.4 i starijim verzijama. Sljedeće upute opisuju postupka za stvaranje radnih uloge, onemogućivanje Dijagnostika 1.0 iz rješenja, i implementacija Dijagnostika 1.2 ili 1.3 vaša uloga suradnika.

### <a name="pre-requisites"></a>Prije requisites
U ovom se članku pretpostavlja da ste pretplatu na Azure i koristite Visual Studio 2013 s Azure SDK. Ako nemate pretplatu na Azure, možete se prijaviti za [Besplatnu probnu verziju][]. Provjerite jeste li [instalirati i konfigurirati Azure PowerShell verzije 0.8.7 ili novije verzije][].

### <a name="step-1-create-a-worker-role"></a>Korak 1: Stvaranje ulogu suradnika
1.  Pokrenite **Visual Studio 2013**.
2.  Stvaranje novog projekta **Azure u Oblaku** iz predloška **oblaka** ciljeve .NET Framework 4,5.  Dodjela naziva projekta "WadExample", a zatim kliknite u redu.
3.  Odaberite **Ulogu suradnika** , a zatim kliknite u redu. Stvorit će se projekt.
4.  U **Pregledniku rješenja**, dvokliknite datoteku **WorkerRole1** svojstva.
5.  U **konfiguraciji** tab poništite potvrdni okvir **Omogući dijagnostiku** da biste onemogućili Dijagnostika 1.0 (Azure SDK 2.4 i eariler).
6.  Sastavljanje rješenje da biste provjerili imate li bez pogrešaka.

### <a name="step-2-instrument-your-code"></a>Korak 2: Instrumenata kod
Zamijenite sadržaj WorkerRole.cs sljedeći kod. Klase SampleEventSourceWriter, nasljeđuju od [EventSource predmete][]implementira četiri načina zapisivanja: **SendEnums**, **MessageMethod**, **SetOther** i **HighFreq**. Prvi parametar metodu **WriteEvent** definira ID za odgovarajući događaj. Način pokretanja implementira beskonačno Ponavljaj koja poziva svaki od metoda zapisivanje implementirana u predmete **SampleEventSourceWriter** svakih 10 sekundi.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Korak 3: Implementacija vaša uloga suradnika
1.  Implementacija vaša uloga suradnika Azure iz programa Visual Studio tako da odaberete **WadExample** projekta u pregledniku rješenja zatim **Objavi** na izborniku **Sastavi** .
2.  Odaberite pretplatu.
3.  U dijaloškom okviru **Postavke objavljivanja za Microsoft Azure** odaberite **Stvori novo...**.
4.  U dijaloškom okviru **Stvaranje servis u Oblaku i račun za pohranu** unesite **naziv** (na primjer, "WadExample") i odaberite regija ili afinitet grupe.
5.  Postavite **okruženje** **pripremna**.
6.  Izmjena druge **Postavke** po potrebi, a zatim kliknite **Objavi**.
7.  Po dovršetku implementaciju provjerite je li na portalu za Azure klasični servis u oblaku u stanju **pokrenut** .

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Korak 4: Stvaranje Dijagnostika konfiguracijskoj datoteci i instalirajte proširenje
1.  Preuzmite definicija sheme datoteke javno konfiguracije izvršavanjem sljedeće naredbe ljuske PowerShell:
2.
        (Get-AzureServiceAvailableExtension - ExtensionName "PaaSDiagnostics" - ProviderNamespace "Microsoft.Azure.Diagnostics"). PublicConfigurationSchema | Više datoteka-kodiranje utf8 - FilePath 'WadConfig.xsd'

2.  Dodajte XML datoteke u projekt **WorkerRole1** klikom desnom tipkom miša na projektu **WorkerRole1** , a zatim odaberite **Dodaj** -> **Nove stavke...**  ->  **Visual C# stavki** -> **podataka** -> **XML datoteku**. Dajte naziv datoteci "WadExample.xml".

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  Na WadConfig.xsd pridružiti konfiguracijskoj datoteci. Provjerite je li prozor uređivača WadExample.xml aktivnog prozora. Pritisnite tipku **F4** da biste otvorili prozor **Svojstva** . Kliknite svojstvo **sheme** u prozoru **Svojstva** . Kliknite **...** u svojstvu **sheme** . Kliknite na **dodatnoj..** gumb i prijeđite na mjesto na koje ste spremili datoteku XSD i odaberite datoteku WadConfig.xsd. Kliknite **u redu**.
4.  Zamijenite sadržaj konfiguracijska datoteka WadExample.xml sljedeće XML, a zatim spremite datoteku. Datoteka konfiguracije definira nekoliko mjerača performansi da biste prikupili: jedan za procesora i jedan za korištenje memorije. Zatim konfiguraciju definira četiri događaji koje odgovaraju načinima u razredu SampleEventSourceWriter.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Korak 5: Instalacija Dijagnostika na vaša uloga suradnika
PowerShell cmdleti za upravljanje Dijagnostika na ulozi weba ili tempiranja su: postavljanje AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension i uklanjanje AzureServiceDiagnosticsExtension.

1.  Otvorite Azure PowerShell.
2.  Izvršavanje skriptu da biste instalirali Dijagnostika na vaša uloga suradnika (Zamijeni *StorageAccountKey* ključ računa za pohranu za vaš račun za pohranu wadexample):

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Korak 6: Pogledajte telemetrijskih podataka
U Visual Studio **Poslužitelja Explorer** dođite do wadexample račun za pohranu. Nakon što je oblak servisa pokrenuta oko pet minuta trebali biste vidjeti tablice **WADMessageTable** **WADEnumsTable**, **WADHighFreqTable**, **WADPerformanceCountersTable** i **WADSetOtherTable**. Dvokliknite na jednu od tablica da biste pogledali telemetrijskih prikupljene.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Konfiguriranje datoteke sheme

Konfiguracijska datoteka Dijagnostika definira vrijednosti koje se koriste za inicijalizaciju dijagnostičkih konfiguracijske postavke prilikom pokretanja agent za dijagnostiku. Pogledajte [najnovije referenca sheme](https://msdn.microsoft.com/library/azure/mt634524.aspx) za valjane vrijednosti i primjeri.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Ako imate poteškoća, potražite u članku [Otklanjanje poteškoća Azure dijagnostike](../azure-diagnostics-troubleshooting.md) pomoć za najčešće probleme.

## <a name="next-steps"></a>Daljnji koraci
Da biste promijenili podatke prikupljate, [pogledajte popis virtualnog računala povezani članci Azure Dijagnostika](azure-diagnostics.md#cloud-services) otklanjanje poteškoća ili dodatne informacije o Dijagnostika Općenito.


[EventSource klasa]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Besplatna probna verzija]: http://azure.microsoft.com/pricing/free-trial/
[Instaliranje i konfiguriranje Azure PowerShell verzije 0.8.7 ili novije verzije]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
