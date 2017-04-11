<properties
    pageTitle="Kako koristiti Azure Dijagnostika na virtualnim računalima sustava | Microsoft Azure"
    description="Pomoću Azure Dijagnostika za prikupljanje podataka iz Azure virtualnim strojevima za ispravljanje pogrešaka, mjerenje performanse, nadzor, promet analizu i više."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Omogućivanje Dijagnostika na Azure virtualnim računalima sustava

Pozadine na Azure Dijagnostika potražite u članku [Pregled Dijagnostika Azure](azure-diagnostics.md) .

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Kako omogućiti i dijagnostiku u virtualnog računala

Ovaj vodič kroz opisuje kako daljinski instalirati Dijagnostika Azure virtualnog računala s računala razvoj. I Saznajte kako implementirati aplikaciju koja se izvršava na Azure virtualnog računala i emits telemetrijskih podataka pomoću.NET [EventSource predmete][]. Azure Dijagnostika koristi se za prikupljanje za telemetriju i spremite ga na račun za Azure prostora za pohranu.

### <a name="pre-requisites"></a>Prije requisites
Ovaj vodič kroz pretpostavlja da ste pretplatu na Azure i koristite Visual Studio 2013 s Azure SDK. Ako nemate pretplatu na Azure, možete se prijaviti za [Besplatnu probnu verziju][]. Provjerite jeste li [instalirati i konfigurirati Azure PowerShell verzije 0.8.7 ili novije verzije][].

### <a name="step-1-create-a-virtual-machine"></a>Korak 1: Stvaranje virtualnog računala
1.  Na računalu razvoj pokrenuti Visual Studio 2013.
2.  U Visual Studio **Explorer poslužitelja** proširite **Azure**, desnom tipkom miša kliknite **virtualnim strojevima** , zatim odaberite **Stvaranje virtualnog računala**.
3.  U dijaloškom okviru **Odabir pretplate** odaberite pretplatu Azure, a zatim kliknite **Dalje**.
4.  Odaberite **Windows Server 2012 R2 podatkovnog centra, studenom 2014** u dijaloškom okviru **Odabir slike za virtualnog računala** , a zatim kliknite **Dalje**.
5.  U odjeljku **Virtualnog računala osnovne postavke**postavite naziv virtualnog računala da biste "wadexample". Postavite Administrator korisničko ime i lozinku, a zatim kliknite **Dalje**.
6.  U dijaloškom okviru **Postavke servisa u Oblaku** stvorite nove servise u oblaku pod nazivom "wadexampleVM". Stvaranje novog računa za pohranu pod nazivom "wadexample", a zatim kliknite **Dalje**.
7.  Kliknite **Stvori**.

### <a name="step-2-create-your-application"></a>Korak 2: Stvaranje aplikacije
1.  Na računalu razvoj pokrenuti Visual Studio 2013.
2.  Stvaranje nove vizualne C# konzole aplikacije namijenjen za .NET Framework 4,5. Dodijelite naziv projekta "WadExampleVM".
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  Zamijenite sadržaj Program.cs sljedeći kod. Klase **SampleEventSourceWriter** implementira četiri načina zapisivanja: **SendEnums**, **MessageMethod**, **SetOther** i **HighFreq**. Prvi parametar metodu WriteEvent definira ID za odgovarajući događaj. Način pokretanja implementira beskonačno Ponavljaj koja poziva svaki od metoda zapisivanje implementirana u predmete **SampleEventSourceWriter** svakih 10 sekundi.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
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

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

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
        }
        }


4.  Spremite datoteku i odaberite **Stvaranje rješenja** s izbornika **Sastavljanje** da biste sastavili kod.


### <a name="step-3-deploy-your-application"></a>Korak 3: Implementacija aplikacije
1.  Desnom tipkom miša kliknite na projektu **WadExampleVM** u **Pregledniku rješenja** , a zatim odaberite **Otvori mapu u Eksploreru za datoteke**.
2.  Dođite do mape u *bin\Debug* i kopirajte sve datoteke (WadExampleVM.*)
3.  U **Programu Explorer poslužitelja** desnom tipkom miša kliknite virtualnog računala, a zatim odaberite **Poveži putem udaljene radne površine**.
4.  Nakon uspostave na VM stvorite mapu pod nazivom WadExampleVM i lijepljenje aplikacije datoteke u mapu.
5.  Pokrenite aplikaciju WadExampleVM.exe. Prikazat će se prozor prazne konzole.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Korak 4: Stvaranje konfiguraciju Dijagnostika i instalirajte proširenje
1.  Preuzmite definicija sheme javno konfiguracije datoteke na računalo razvoj izvršavanjem sljedeće naredbe ljuske PowerShell:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Otvorite novu XML datoteku u Visual Studio u projektu već otvoren ili u Visual Studio instancu s nema otvorenih projekata. U Visual Studio, odaberite **Dodaj** -> **Nove stavke...**  ->  **Visual C# stavki** -> **podataka** -> **XML datoteku**. Dajte naziv datoteci "WadExample.xml"
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Korak 5: Daljinski instalirati Dijagnostika na vašem Azure virtualnog računala
PowerShell cmdleti za upravljanje Dijagnostika na na VM su: postavljanje AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension i uklanjanje AzureVMDiagnosticsExtension.

1.  Na računalu za razvojne inženjere otvorite Azure PowerShell.
2.  Izvršavanje skripta za daljinski instalirati Dijagnostika na VM (Zamijeni *StorageAccountKey* s ključ računa za pohranu za vaš račun za pohranu wadexamplevm):

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Korak 6: Pogledajte telemetrijskih podataka
U Visual Studio **Poslužitelja Explorer** dođite do wadexample račun za pohranu. Nakon što u VM pokrenuta pet minuta trebali biste vidjeti tablice **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** i **WADSetOtherTable**. Dvokliknite na jednu od tablica da biste pogledali telemetrijskih prikupljene.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Konfiguriranje datoteke sheme

Konfiguracijska datoteka Dijagnostika definira vrijednosti koje se koriste za inicijalizaciju dijagnostičkih konfiguracijske postavke prilikom pokretanja agent za dijagnostiku. Pogledajte [najnovije referenca sheme](https://msdn.microsoft.com/library/azure/mt634524.aspx) za valjane vrijednosti i primjeri.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Dodatne informacije potražite u članku [Otklanjanje poteškoća dijagnostike Azure](azure-diagnostics-troubleshooting.md) .


## <a name="next-steps"></a>Daljnji koraci
Da biste promijenili podatke prikupljate, [pogledajte popis virtualnog računala povezani članci Azure Dijagnostika](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) otklanjanje poteškoća ili dodatne informacije o Dijagnostika Općenito.


[EventSource klasa]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Besplatna probna verzija]: http://azure.microsoft.com/pricing/free-trial/
[Instaliranje i konfiguriranje Azure PowerShell verzije 0.8.7 ili novije verzije]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
