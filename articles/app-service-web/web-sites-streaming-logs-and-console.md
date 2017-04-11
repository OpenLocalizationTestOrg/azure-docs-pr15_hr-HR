<properties 
    pageTitle="Strujanje zapisnicima i konzole" 
    description="Strujanje zapisnicima i konzole za pregled" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Strujanje zapisnicima i konzole

## <a name="streaming-logs"></a>Strujanje zapisnika

**Portal za Azure** predstavlja programa integrirani strujanje zapisnika preglednik koji omogućuje prikaz praćenje događaja iz **Aplikacije servisa za** aplikacije u stvarnom vremenu.  

Postavljanje Ova značajka zahtijeva nekoliko jednostavnih koraka:

- Pisanje kašnjenja u kodu
- Omogućivanje aplikacije **dijagnostičke zapisnike** aplikacije
- Prikaz toka iz ugrađenih UI **Strujanje zapisnika** **Azure portal**.

### <a name="how-to-write-traces-in-your-code"></a>Kako napisati kašnjenja u kodu ###

Jednostavno je pisanju kašnjenja u kodu.  U C# je dovoljno pisanja koda za sljedeće:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Praćenje klase se nalaze u prostor za naziv System.Diagnostics.

U aplikaciji node.js možete napisati kod da biste postigli isti rezultat:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Kako omogućiti i prikaz na strujanje zapisnika
![][BrowseSitesScreenshot]Dijagnostika omogućena na temelju po aplikacije. Započnite pregledavanjem koju želite omogućiti tu značajku na web-mjesto.  
  
![][DiagnosticsLogs]S izbornika postavke, pomaknite se prema dolje do odjeljka **praćenja** i kliknite **(1) dijagnostičke zapisnike**. Zatim **(2) Omogući** **Aplikacije bilježenje (datotečnom sustavu)** ili **Aplikacije zapisivanje (blob)** mogućnost **razine** možete mijenjati razinu težinu kašnjenja za snimanje. Ako samo želite upoznati sa značajkama, postavite razinu na **tekstni** da biste bili sigurni svih svojih izjava praćenje prikuplja.

Kliknite **SPREMI** pri vrhu na plohu i spremni ste za prikaz zapisnika.

>[AZURE.NOTE] Na višu **razinu težinu** dodatni resursi potrošiti zapisnika i dodatne kašnjenja da je proizvedeno Provjerite je li **razinu težinu** konfiguriran za ispravan verbosity za proizvodnju ili velikog prometa web-mjesta. 

![][StreamingLogsScreenshot]Da biste pogledali na **strujanje zapisnika** iz unutar Azure portala, kliknite **(1) strujanje zapisnika** i u odjeljku **praćenje** izbornika postavke. Pisali aplikacije je aktivno izjave o praćenju, trebali biste ih vidjeti u na **(2) strujanje zapisnike korisničkog Sučelja** u najbliži stvarnom vremenu.

## <a name="console"></a>Konzola
**Portal za Azure** omogućuje pristup konzole za aplikacije. Možete Istraživanje datotečnom sustavu pokrenite aplikaciju i pokreću skripte komponente powershell/cmd. Povezane su tako da postavite kao pokrenute aplikacije kod prilikom izvršavanja naredbe konzole iste dozvole. Pristup zaštićeni direktorija ili pokretanje skripti koje je potrebno je blokirana povećane dozvole.  

![][ConsoleScreenshot]S izbornika postavke, pomaknite se prema dolje do odjeljak **Alati za razvoj** , a zatim kliknite **Konzolu (1)** i **(2) konzole** korisničkog Sučelja otvorit će se udesno.

Upoznati s **konzole**, pokušajte osnovni naredbama kao što su:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
