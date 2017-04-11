<properties
    pageTitle="Praćenje tijeka u aplikaciji servisa oblaka s Azure Dijagnostika | Microsoft Azure"
    description="Praćenje poruka dodati aplikaciju Azure radi ispravljanja pogrešaka, mjerenje performanse, nadzor, promet analizu i više."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Praćenje tijeka servise u Oblaku aplikacijom Dijagnostika Azure

Praćenje je način za nadzor izvođenja aplikacije dok se izvodi. Klase [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)i [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) možete koristiti da biste snimili podatke o pogreškama i izvođenja aplikacije u zapisnicima, tekstne datoteke ili drugih uređaja za kasniju analizu. Dodatne informacije o praćenju potražite u članku [praćenje i Instrumenting aplikacije](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Korištenje naredbe prati i praćenje parametri

Implementacija praćenje u aplikaciji za servise u Oblaku dodavanjem [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) konfiguracije aplikacije i upućivanje poziva System.Diagnostics.Trace ili System.Diagnostics.Debug u kodu aplikacije. Korištenje datoteka konfiguracije *app.config* uloge suradnika i *web.config* web uloge. Prilikom stvaranja novog servis pomoću predloška za Visual Studio Azure Dijagnostika automatski dodaje u projekt, a na DiagnosticMonitorTraceListener dodaje se odgovarajući konfiguracijska datoteka za uloge koje dodate.

Informacije o stavljanje izvješća o praćenju, potražite u članku [Kako: dodavanje naredbe prati aplikacije kod](https://msdn.microsoft.com/library/zd83saa2.aspx).

Potvrđivanjem [Parametri praćenja](https://msdn.microsoft.com/library/3at424ac.aspx) u kodu možete kontrolirati pojavljuje li praćenje i kako proširenom je. Omogućuje praćenje stanja aplikacije u okruženju proizvodnje. To je osobito važno u poslovnoj aplikaciji koja koristi više komponenti koji se izvode na više računala. Dodatne informacije potražite u članku [Kako: Konfiguriranje praćenja parametri](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Konfiguriranje ga slušatelj praćenja u aplikaciji za Azure

Praćenje i ispravljanje pogrešaka i TraceSource, potrebno postaviti "slušače" mogli ste prikupljati i snimanje poruke koje se šalju. Slušače prikupljanje, pohranjivanje i usmjeravati praćenje poruke. Oni izravno izlaz praćenje u odgovarajuće cilja, kao što je zapisnika, prozor ili tekstne datoteke. Azure Dijagnostika koristi [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) predmete.

Prije nego što dovršite sljedeći postupak, morate pokrenuti Azure dijagnostičkih monitor. Da biste to učinili, potražite u članku [Omogućavanje dijagnostike u Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Imajte na umu da ako koristite predloške koje nudi Visual Studio, konfiguracije ga slušatelj se automatski dodaje umjesto vas.


### <a name="add-a-trace-listener"></a>Dodajte ga slušatelj za praćenje

1. Otvorite datoteku web.config ili app.config za vašu ulogu.
2. Dodajte sljedeći kod u datoteku. Promijeniti atribut verzije da biste koristili broj verzije u sklopu pozivate. Verzija skupa ne moraju nužno biti mijenja uz svako izdanje Azure SDK osim ako su ažuriranja za taj.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Provjerite je li referenca projekta u sklopu Microsoft.WindowsAzure.Diagnostics. Ažurirajte broj verzije u xml iznad tako da odgovara verziji referencirani Microsoft.WindowsAzure.Diagnostics skupa.

3. Spremite datoteku konfiguracije.

Dodatne informacije o slušače potražite u članku [Praćenje slušače](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Kada dovršite korake da biste dodali ga slušatelj, možete dodati naredbe prati kod.


### <a name="to-add-trace-statement-to-your-code"></a>Da biste dodali izjava praćenja u kodu

1. Otvorite izvornu datoteku za svoju aplikaciju. Ako, na primjer, u <RoleName>.cs datoteke za ulogu web ili ulogu suradnika.
2. Dodajte sljedeće pomoću izjave ako je već nisu dodani:
    ```
        using System.Diagnostics;
    ```
3. Dodavanje naredbi prati mjesto na koje želite snimiti podatke o stanju aplikacije. Da biste oblikovali izlaz naredbu prati možete koristiti na različite načine. Dodatne informacije potražite u članku [Kako: dodavanje naredbe prati aplikacije kod](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Spremi izvornu datoteku.
