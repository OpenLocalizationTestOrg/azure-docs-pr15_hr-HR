<properties
    pageTitle="Praktični savjeti i otklanjanje poteškoća vodilice za čvor aplikacije na web-aplikacije Azure"
    description="Saznajte najbolje prakse te korake za otklanjanje poteškoća za čvor aplikacije na web-aplikacije Azure."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Praktični savjeti i otklanjanje poteškoća vodilice za čvor aplikacije na web-aplikacije Azure

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

U ovom se članku opisano Praktični savjeti i upute za otklanjanje poteškoća za [čvor aplikacije](app-service-web-nodejs-get-started.md) koje rade na Azure Webapps (s [iisnode](https://github.com/azure/iisnode)).

>[AZURE.WARNING] Budite oprezni prilikom korištenja korake za otklanjanje poteškoća na web-mjestu radnog. Preporuka je da biste otkrili aplikacija u sustavu instalacijski program nije radnog, primjerice svoje pripremna vremensko razdoblje i kada se problem ne riješi, zamjena pripremna vremensko razdoblje s vašeg radnog vremensko razdoblje.

## <a name="iisnode-configuration"></a>Konfiguriranje IISNODE

[Datoteke shema](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) prikazuje sve postavke koje je moguće je konfigurirati za iisnode. Neke postavke koje će biti korisno za svoju aplikaciju su:

* nodeProcessCountPerApplication

    Tom se postavkom određuje broj čvor procesa koji su pokrenuti po IIS aplikacije. Zadana je vrijednost 1. Postavljanjem to možete pokrenuti proizvoljan broj node.exe kao vaš VM osnovni broj 0. Preporučena je vrijednost 0 za većinu aplikaciju da bi se sve na jezgri mogu koristiti na vašem računalu. Node.exe je jedan niti da jedan node.exe zauzeti će najviše 1 core i da biste dobili maksimalne performanse iz aplikacije čvor koje ćete koristiti sve jezgri.

* nodeProcessCommandLine

    Tom se postavkom određuje put do na node.exe. Postavite tu vrijednost tako da postavite pokazivač u node.exe verziju.

* maxConcurrentRequestsPerProcess

    Tom se postavkom određuje najveći je broj istovremeni zahtjevi za svaku node.exe je poslao iisnode. Na azure webapps zadane vrijednosti za to je beskonačno. Će ne morate brinuti o toj postavci. Izvan azure webapps zadana vrijednost je 1024. Možda ćete morati konfigurirali ovaj ovisno o tome koliko zahtjeva za aplikaciju dobiva i brzinu aplikacije obrađuje svaki zahtjev.

* maxNamedPipeConnectionRetry

    Tom se postavkom određuje maksimalni broj puta iisnode vratit će unosom veze na imenovanih kanala da biste poslali zahtjev putem node.exe. Ova postavka u kombinaciji s namedPipeConnectionRetryDelay određuje Ukupno vrijeme čekanja svakog zahtjeva unutar iisnode. Zadana vrijednost je 200 na Azure Webapps. Ukupno vrijeme čekanja u sekundama = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

* namedPipeConnectionRetryDelay

    Ova postavka kontrole količinu vremena (u milisekundama) iisnode će Pričekajte između svakog pokušajte ponovno pošaljite zahtjev za node.exe putem imenovanih kanala. Zadana je vrijednost 250ms.
    Ukupno vrijeme čekanja u sekundama = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

    Po zadanom je ukupna vremenskog ograničenja u iisnode na azure webapps 200 \* 250ms = 50 sekundi.

* logDirectory

    Tom se postavkom određuje direktorija gdje iisnode zapisuje stdout/stderr. Zadana vrijednost je iisnode koji se odnosi direktorija glavni skripte (imenik gdje se nalazi prezentacija glavnom server.js)

* debuggerExtensionDll

    Tom se postavkom određuje koju verziju sustava iisnode čvor kontrola će koristiti kada čvor aplikacija za ispravljanje pogrešaka. Trenutno iisnode kontrola 0.7.3.dll i iisnode inspector.dll su samo 2 valjane vrijednosti za tu postavku. Zadana je vrijednost iisnode kontrola 0.7.3.dll. verzija iisnode kontrola 0.7.3.dll koristi čvor kontrola 0.7.3 i koristi websockets, pa ćete ga morati omogućiti websockets na vašem azure webapp da biste koristili ovu verziju. Pogledajte <http://www.ranjithr.com/?p=98> više pojedinosti o konfiguriranje iisnode koristiti novi provjeru čvor.

* flushResponse

    Zadano ponašanje IIS je da ga međuspremnika podatke o odzivu prema gore za 4MB prije nikakvu ili do kraja odgovor, ovisno o tome što dolaze prvi. iisnode nudi postavke konfiguracije da biste nadjačali takvo ponašanje: da biste pražnjenje fragment tijela entitet odgovor čim iisnode prima iz node.exe, prvo morate postaviti na iisnode/@flushResponse atribut u web.config na "true":
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Omogućivanje nikakvu svaki dio entitet tijelo odgovor dodaje indirektni performanse koji smanjuje propusnost sustava ~ 5% (od v0.1.13), pa je najbolje opsega tu postavku samo za krajnje točke koje je potrebno odgovor strujanje (npr. pomoću na <location> element u datoteka web.config)

    Osim toga, za strujanje aplikacije, morat ćete postaviti responseBufferLimit vaše rukovatelja iisnode 0.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    To je točka-zarez odvojene popis datoteka koje će biti nadzirane promjene. Promjena u datoteku uzrokuje aplikacije u koš za. Svaka stavka sastoji se od naziv neobavezno direktorija uz naziv potrebne datoteke koje su odnosu direktorija u kojem se nalazi točka unosa glavnog računala. Kartica koji su dopuštene u u datoteku samo dio naziva. Zadana je vrijednost "\*. js;web.config"

* recycleSignalEnabled

    Zadana je vrijednost false. Ako je omogućeno, čvor aplikacija možete povezati s imenovanih kanala (varijabla okruženja IISNODE\_KONTROLA\_kanala) i poslali poruku "koš za smeće". Time će w3wp u koš za obavljanje.

* idlePageOutTimePeriod

    Zadana je vrijednost 0, što znači ova značajka je onemogućen. Kada postavite neki vrijednost veću od 0, iisnode će stranica izgleda njezinih podređenih procesa svaki 'idlePageOutTimePeriod' milisekundi. Da biste shvatili koje stranica izgleda srednje vrijednosti, pogledajte ove [dokumentacije](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Ta postavka će biti korisno za aplikacije koji zauzimaju mnogo memorije i želite pageout memorije disk povremeno da biste oslobodili neke RAM-a.

>[AZURE.WARNING] Budite oprezni prilikom omogućivanja sljedeće postavke konfiguracije radnog aplikacije. Preporuka tako da ne omogućite uživo radnog aplikacije.

* debugHeaderEnabled

    Zadana je vrijednost false. Ako je postavljen na true, iisnode dodat će se HTTP odgovor zaglavlje iisnode-ispravljanje pogrešaka u svaki HTTP odgovor šalje vrijednost zaglavlja iisnode pogrešaka je URL-a. Pojedinačni dijelovi dijagnostičke informacije koje se mogu prikupe tako da pogledate URL djelić, ali je mnogo bolje vizualizacije postići tako da otvorite URL-a u pregledniku.

* loggingEnabled

    Tom se postavkom određuje zapisivanje stdout i stderr po iisnode. Iisnode će snimite stdout/stderr iz čvor procesa koji se pokreće i pisati u direktoriju naveden u postavci 'logDirectory'. Kada se omogući, aplikacija će biti pisanje zapisnike u datotečni sustav i ovisno o količinu zapisivanje gotovo aplikacija, mogući su posljedice performansi.

* devErrorsEnabled

    Zadana je vrijednost false. Kada postavljen na true, iisnode će se prikazivati HTTP kod stanja i kod pogreške Win32 u pregledniku. Šifra win32 bit će vam pomoći u ispravljanje pogrešaka određene vrste problema.
    
* debuggingEnabled (nemojte omogućiti na web-mjestu radnog uživo)

    Tom se postavkom određuje značajke za ispravljanje pogrešaka. Iisnode integriran s čvor kontrola. Omogućivanjem tu postavku omogućite ispravljanje pogrešaka aplikacije čvor. Kada je omogućen tu postavku, iisnode će izgleda potrebne čvor Provjera datoteka u direktoriju 'debuggerVirtualDir' na prvi zahtjev za ispravljanje pogrešaka u aplikaciji čvor. Slanjem zahtjeva http://yoursite/server.js/debug možete učitati provjeru čvor. Možete kontrolirati segment URL-a za ispravljanje pogrešaka s postavkom 'debuggerPathSegment'. Prema zadanom debuggerPathSegment = "za ispravljanje pogrešaka". Možete postaviti to GUID, primjerice tako da je teže i ne može otkriti drugim korisnicima.

    Provjerite [vezu](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) za dodatne informacije o ispravljanje pogrešaka.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenariji i preporuke i otklanjanje poteškoća

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Moje aplikacije čvor čini previše odlazni pozivi.

Mnoge aplikacije ćete raditi izlazne veze kao dio njihove obične operacije. Ako, na primjer, kada zahtjev, čvor aplikacije biste htjeli obratite na REST API-JA negdje drugdje i dobiti neke informacije za obradu zahtjeva. Želite koji želite koristiti Zadrži agent za aktivnosti kada http ili https poziva. Na primjer, nije moguće koristiti modul agentkeepalive kao agent aktivnosti vaše Zadrži pri stvaranju te odlazne pozive. To jamči da bi se na sockets ponovno na azure webapp VM i smanjuje mogućnost indirektnog stvaranja novog sockets za svaki zahtjev za izlazni. Osim toga, to jamči da koristite manji broj sockets mnogo izlaznog zahtjeva i zbog toga ne prekoračuje maxSockets koji su dodijeliti po VM. Preporuka na Azure Webapps bi da biste postavili vrijednost maxSockets agentKeepAlive ukupnom zbroju 160 sockets po VM. To znači da imate 4 node.exe sustavom u VM, bi želite postaviti agentKeepAlive maxSockets do 40 po node.exe koji je ukupna po VM 160.

Konfiguracija agentKeepALive primjer:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

U ovom se primjeru pretpostavlja da ste 4 node.exe izvode na vašem VM. Ako imaju različit broj node.exe sustavom u VM, morat ćete izmijeniti maxSockets postavljanje sukladno tome.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Moje aplikacije čvor koristi previše procesora.

Vjerojatno se preporuku iz Azure Webapps na portal o visoke potrošnju procesora. Možete postaviti i monitora da biste pogledali za određene [metrike](web-sites-monitor.md). Prilikom provjere procesora na [Nadzornoj ploči Portal Azure](../application-insights/app-insights-web-monitor-performance.md), provjerite vrijednosti MAX CPU-a da ne bi promakli Vršna vrijednosti.
U slučajevima gdje mislite aplikacija koristi previše procesora i ne može se objašnjava zašto, morat ćete profila čvor aplikacije.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profiliranje čvor aplikacija na azure webapps s V8 Profiler

Na primjer, omogućuje izgovorite aplikacija svijeta pozdrav koji želite profil kao što je prikazano u nastavku:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Idite na vašem https://yoursite.scm.azurewebsites.net/DebugConsole IO web-mjesta

Vidjet ćete naredbenog retka kao što je prikazano u nastavku. Idite u imeniku web-mjesta/wwwroot

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Pokrenite naredbu "npm instalirati v8 profiler"

To instalirajte v8 profiler u odjeljku čvor\_directory module i sve njezine ovisnosti.
Sada uredite vaše server.js da biste profil aplikacije.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Iznad promjene će profila funkciju WriteConsoleLog i pisati izlaz profila 'profile.cpuprofile' datoteku pod wwwroot vašeg web-mjesta. Slanje zahtjeva za aplikaciju. Vidjet ćete 'profile.cpuprofile' datoteke stvorene u odjeljku wwwroot vašeg web-mjesta.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Preuzmite ovu datoteku i morat ćete otvoriti datoteku pomoću alata za Chrome F12. Pritisnite F12 na chrome, a zatim kliknite na "Karticu profili". Kliknite gumb "Učitavanja". Odaberite datoteku profile.cpuprofile koji ste upravo preuzeli. Kliknite na profilu samo učitati.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Vidjet ćete da 95% vremena je troše WriteConsoleLog funkcije kao što je prikazano u nastavku. To također prikazuje brojeve redaka točno i izvorne datoteke koje uzrokuju problem.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Moje aplikacije čvor koristi previše memorije.

Vjerojatno se preporuku iz Azure Webapps na portal o potrošnje opterećenje memorije. Možete postaviti i monitora da biste pogledali za određene [metrike](web-sites-monitor.md). Prilikom provjere memorije na [Nadzornoj ploči Portal Azure](../application-insights/app-insights-web-monitor-performance.md), provjerite vrijednosti MAX memorije da ne bi promakli Vršna vrijednosti.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Otkrivanje osipanja i skupova Diffing za node.js 

[Čvor memwatch](https://github.com/lloyd/node-memwatch) možete koristiti da biste lakše prepoznali osipanjem memorije.
Možete instalirati memwatch baš kao i v8 profiler i uređivati kod da biste hvatanje i razlika curenje skupova koje prepoznavanje memorije u aplikaciji.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Moj node.exe su početak drugom procesu slučajno 

Postoji nekoliko mogućih razloga zašto to se događa:

1.  Aplikacija je prijavi je neuhvaćenu iznimke – Molimo potvrdite d:\\kućni\\LogFiles\\aplikacije\\datoteke zapisnika errors.txt za detalje na izbačena iznimka. Datoteka sadrži Praćenje stoga da biste mogli ispraviti aplikacije ovisno o tome.

2.  Aplikacija koristi previše memorije koji je utjecaja na druge procese početak rada. Ako je ukupna memorije VM blizu 100%, vaš node.exe može biti drugom procesu Upravitelj postupak da biste omogućili drugi procesi se mogućnost da biste učinili nešto. Da biste riješili taj problem, provjerite je li vaša aplikacija je propušta memorije ili ako ste aplikaciju zaista treba koristiti mnogo memorije, provjerite proširenjem veće VM s puno više RAM-a.

### <a name="my-node-application-does-not-start"></a>Moje aplikacije čvor ne želite pokrenuti

Ako aplikacija je uputio 500 pogreške pri pokretanju, može biti nekoliko razloga:

1.  Node.exe ne nalazi na odgovarajuće mjesto. Provjerite postavke nodeProcessCommandLine.

2.  Glavni skriptna datoteka nema na odgovarajuće mjesto. Provjerite web.config i provjerite odgovara li naziv datoteke glavni skripte u odjeljku rukovatelja onome na glavnom skriptna datoteka.

3.  Konfiguracija web.config nije ispravna – provjerite postavke imena/vrijednosti.

4.  Hladna Start – aplikacije traje predugo za pokretanje. Ako aplikacija traje dulje nego (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 sekundi iisnode će vratiti 500 Pogreška. Povećanje vrijednosti te postavke tako da odgovaraju vremena početka aplikacije da biste spriječili iisnode iz isteklo i povratkom 500 Pogreška.

### <a name="my-node-application-crashed"></a>Moje aplikacije čvor ruši

Aplikacija je prijavi je neuhvaćenu iznimke – Molimo potvrdite d:\\kućni\\LogFiles\\aplikacije\\datoteke zapisnika errors.txt za detalje na izbačena iznimka. Datoteka sadrži Praćenje stoga da biste mogli ispraviti aplikacije ovisno o tome.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Moje aplikacije čvor traje previše za pokretanje (Hladna pokretanje)

Najčešći Razlog je koje aplikacije sadrži mnogo datoteka u čvor\_moduli i aplikacija pokušava učitati većinu tih datoteka prilikom pokretanja. Prema zadanim postavkama, jer datotekama nalaze na zajedničkog mrežnog resursa na Azure Webapps učitavanja toliko datoteke može potrajati.
Neka rješenja da bi sustav učinila brže su:

1.  Provjerite je li paušalni ovisnost strukture i bez duplikata ovisnosti pomoću npm3 da biste instalirali module.

2.  Pokušaj drži učitavanje vaše čvora\_moduli i učitati sve module prilikom pokretanja. To znači da se poziv require('module') potrebno izvršiti kada su vam uistinu potrebni unutar funkcije pokušate koristiti modul.

3.  Azure Webapps nudi značajku nazvanu lokalne predmemorije. Ta značajka kopira sadržaj iz zajedničkog mrežnog resursa na lokalni disk na VM. Budući da su datoteke lokalni, vrijeme učitavanja čvora\_moduli je mnogo brže. -Ove [dokumentacija](../app-service/app-service-local-cache.md) objašnjava kako koristiti lokalne predmemorije u više detalja.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http stanje i substatus

[Izvorišna datoteka](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) nalaze se sve kombinacije iisnode moguće status/substatus možete se vratiti u slučaju pogreške.

Omogućivanje FREB aplikacije da biste se prikazuje kod pogreške win32 (Provjerite omogućite FREB samo na web-mjestima koje nisu radnog radi boljih performansi).

| HTTP stanje | HTTP SubStatus | Od mogućih razloga?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1000           | Došlo je neki problem i isporuka zahtjev za IISNODE – potvrdite ako je pokrenuti node.exe. Node.exe može imati ruši prilikom pokretanja. Provjerite konfiguraciju web.config pogrešaka.                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0x2 – aplikacija ne reagira URL-a. Provjerite URL dopuna pravila ili ima li eksplicitnih aplikacije točan usmjerava definirani. -Win32Error 0x6d – imenovanih kanala zauzet – Node.exe je prihvaćanje zahtjeva za jer je zauzet kanala. Provjerite visoke procesora. -Druge pogreške – potvrdite ako node.exe ruši.
| 500         | 1002           | Node.exe ruši – Provjera d:\\kućni\\LogFiles\\zapisivanje errors.txt za praćenje stoga.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Okomita konfiguracije problem – to nikad trebali biste vidjeti, ali ako to učinite, konfiguracije imenovanih kanala nije valjana.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004-1018      | Pojavila se neke pogreška tijekom slanja zahtjeva ili obradu odgovora iz node.exe. Provjerite je li node.exe ruši. Provjera d:\\kućni\\LogFiles\\zapisivanje errors.txt za praćenje stoga.                                                                                                                                                                                                                                                                                    |
| 503         | 1000           | Nema dovoljno memorije za dodjelu više imenovanih kanala veze. Provjera Zašto aplikacije koristi memorije. Provjerite maxConcurrentRequestsPerProcess postavka vrijednost. Ako njegov ne beskonačno, a imate mnogo zahtjeve, povećajte tu vrijednost da biste spriječili ta se pogreška.                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | Šalje zahtjev za node.exe neće se jer je recikliranje aplikacije. Kada je reciklira aplikacije, zahtjevi za treba obično posluženo.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Kod pogreške win32 potvrdite razloga stvarni – zahtjev nije ne šalje se na node.exe.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Imenovani kanal je zauzet – provjerite je li čvor koristi mnogo CPU-a                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Postoji postavka unutar NODE.exe naziva ČVOR\_NERIJEŠENO\_kanala\_INSTANCE. Prema zadanim postavkama izvan azure webapps tu vrijednost je 4. To znači da taj node.exe prihvaća samo zahtjeva za 4 na imenovani kanal. Na Azure Webapps je tu vrijednost postavite na 5000 i ta vrijednost mora biti dovoljno dobar za većinu aplikacija čvor koji se izvode na azure webapps. Trebali biste neće vidjeti 503.1003 na azure webapps jer imamo visoke vrijednosti za ČVOR\_NERIJEŠENO\_kanala\_INSTANCE.  |

## <a name="more-resources"></a>Dodatni resursi

Slijedite ove veze da biste saznali više o node.js aplikacije na aplikacije servisa za Azure.

* [Početak rada s Node.js web-aplikacije u aplikacije servisa za Azure](app-service-web-nodejs-get-started.md)
* [Upute za ispravljanje pogrešaka u web-aplikacijama Node.js u aplikacije servisa za Azure](web-sites-nodejs-debug.md)
* [Pomoću Node.js modula Azure aplikacije](../nodejs-use-node-modules-azure-apps.md)
* [Aplikacije servisa za Azure web-aplikacije: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Razvojni centar za Node.js](../nodejs-use-node-modules-azure-apps.md)
* [Istraživanje Super tajnu Kudu konzole za ispravljanje pogrešaka](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)