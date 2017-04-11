<properties
    pageTitle="Praćenje, dijagnosticiranje i otklanjanje poteškoća s prostora za pohranu | Microsoft Azure"
    description="Pomoću značajki kao što su analytics za pohranu, zapisivanje na klijentskoj strani i drugih alata drugih proizvođača da biste odredili, dijagnosticiranje i otklanjanje poteškoća vezanih uz Azure prostora za pohranu."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Praćenje, dijagnosticiranje i otklanjanje poteškoća s spremište na platformi Microsoft Azure

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Pregled

Dijagnosticiranje i otklanjanje poteškoća u distribuirane aplikacije smješten u okruženje oblaka može biti složenije od u klasične okruženja. Aplikacija se može uvesti u na PaaS ili IaaS Infrastruktura lokalno, na mobilnom uređaju ili kombinaciju od njih. Obično vaše aplikacije mrežni promet možda prolaziti javne i privatne mreže i aplikacije mogu koristiti više prostora za pohranu tehnologije kao što je Microsoft Azure spremišta tablica, blob-ova, redove ili datoteke uz ostale podatke kao što su pohranjeni kao relacijski i dokumentiranje baze podataka.

Da biste upravljali takve aplikacije uspješno praćenje ih doći i znati kako dijagnosticiranje i otklanjanje poteškoća sa svim aspektima ih i njihovih zavisnih tehnologije. Kao korisnik servise za pohranu Azure, trebali biste kontinuirano praćenje servise za pohranu aplikacija koristi za promjene neočekivano ponašanje (kao što su manja od vremena uobičajeni odgovor) i zapisivanje podataka možete koristiti i da biste prikupili detaljnije podatke, a da biste analizirali problem detaljniju. Dijagnostika informacije s i nadzora i bilježenja će vam pomoći da biste odredili uzrok program naišao na problem. Zatim otklanjanje poteškoća i određivanje odgovarajuće korake koje možete poduzeti da biste ga remediate. Azure prostora za pohranu je core servisa Azure i važan dio Većina rješenja koja kupci implementacija Azure infrastrukture za obrasce. Azure prostora za pohranu sadrži mogućnosti da biste pojednostavnili nadzor, dijagnosticiranje i otklanjanje problema s prostora za pohranu u oblaku aplikacija.

> [AZURE.NOTE] Prostor za pohranu računi s vrstom replikacije od zona suvišne prostora za pohranu (ZRS) nemaju metriku ili mogućnost bilježenje omogućeno trenutno. Osim toga, datoteka servisa Azure ne podržava zapisivanje trenutno.

Stjecanja vodič za otklanjanje poteškoća s kraja do kraja u aplikacijama za pohranu Azure, potražite u članku [Završetka do kraja rješavanje problema pomoću Azure metrike pohrane i zapisivanje, AzCopy, i alata za analizu poruke](storage-e2e-troubleshooting.md).

+ [Uvod]
    + [Način organiziranja ovaj vodič]
+ [Servis za pohranu za nadzor]
    + [Praćenje stanja servisa]
    + [Nadzor kapaciteta]
    + [Nadzor dostupnosti]
    + [Praćenje performansi]
+ [Dijagnosticiranje problema prostora za pohranu]
    + [Servis stanja problema]
    + [Problemi s performansama]
    + [Dijagnosticiranje pogreške]
    + [Prostor za pohranu emulator problema]
    + [Alati za zapisivanje za pohranu]
    + [Pomoću alata za zapisivanje mreže]
+ [Praćenje završetka do kraja]
    + [Correlating podaci iz zapisnika]
    + [ID klijenta zahtjev]
    + [ID zahtjev poslužitelja]
    + [Vremenske oznake]
+ [Upute za otklanjanje poteškoća]
    + [Prikaži metriku visoke AverageE2ELatency i niska AverageServerLatency]
    + [Metriku prikaz malo AverageE2ELatency i niska AverageServerLatency, ali klijent se pojavili visokom latencijom]
    + [Metriku prikaz visoke AverageServerLatency]
    + [Nailazite na neočekivano kašnjenja u isporuke poruke na u redu čekanja]
    + [Metriku prikaz povećava u PercentThrottlingError]
    + [Metriku prikaz povećava u PercentTimeoutError]
    + [Metriku prikaz povećava u PercentNetworkError]
    + [Klijent prima poruke 403 HTTP (zabranjeno)]
    + [Klijent prima poruke 404 HTTP (nema)]
    + [Klijent prima poruke 409 HTTP (Sukob)]
    + [Metriku prikaz malo PercentSuccess ili analize zapisnika stavke imaju operacije sa statusom transakcije ClientOtherErrors]
    + [Kapacitet metriku prikaz neočekivane povećava u korištenja kapacitet pohrane]
    + [Nailazite na neočekivano ponovna virtualnim strojevima koji imaju velik broj priložene VHDs]
    + [Problem nastaje pomoću emulator prostora za pohranu za razvoj ili test]
    + [Se pojavili problema s instalacijom Azure SDK za .NET]
    + [Imate različite problem sa servisom za pohranu]
    + [Otklanjanje problema s Azure datoteke sa sustavom Windows i Linux](storage-troubleshoot-file-connection-problems.md)
+ [Appendices]
    + [Dodatak 1: Korištenje Fiddler da biste snimili HTTP i HTTPS promet]
    + [Dodatak 2: Korištenje Wireshark da biste snimili mrežni promet]
    + [Dodatak 3: Pomoću alata za analizu Microsoft poruku da biste snimili mrežni promet]
    + [Dodatak 4: Pomoću programa Excel da biste pogledali metriku i zapisivanje podataka]
    + [Dodatak 5: Nadzor s uvide aplikacije za Visual Studio Team Services]

## <a name="introduction"></a>Uvod

Ovaj vodič objašnjava značajke kao što su Azure Analytics za pohranu, klijentsko zapisivanje u biblioteku Azure prostora za pohranu klijenta i drugih alata drugih proizvođača za prepoznavanje dijagnosticiranje i otklanjanje poteškoća s Azure prostora za pohranu probleme vezane uz.

![][1]

*Slika 1 nadzor Dijagnostika, i otklanjanje poteškoća*

Ovaj vodič namijenjen može pročitati prvenstveno programerima internetske servise koje koriste web-mjesto servisa Azure prostora za pohranu i u okvir za IT profesionalce odgovorni za upravljanje takve internetske servise. Ciljevi ovaj vodič su:

- Da biste lakše održavanje stanja i performanse svog računa Azure prostora za pohranu.
- Da vam pruži potrebne postupke i alati da biste lakše odlučili ako poteškoću ili problem u aplikaciji odnosi Azure prostora za pohranu.
- Kako bi pružio s akcijama upute za otklanjanje problema vezanih uz Azure prostora za pohranu.

### <a name="how-this-guide-is-organized"></a>Način organiziranja ovaj vodič

U odjeljku "[servisima za pohranu za nadzor]" opisuje praćenje stanja i performanse servisa za pohranu Azure pomoću metrike analize Azure pohrane (metrike pohrane).

U odjeljku "[Dijagnosticiranje prostora za pohranu problemi]" opisuje dijagnosticiranje problema pomoću Azure prostora za pohranu analize zapisivanje (prostora za pohranu prijava). Također se opisuje kako omogućiti zapisivanje na klijentskoj strani pomoću funkcije na jedan od klijenta biblioteke kao što je biblioteka za pohranu klijenta za .NET ili Azure SDK Java.

U odjeljku "[praćenje završetka do kraja]" opisuje kako povezivanje informacija koje se nalaze u raznim datoteke zapisnika i metriku podataka.

U odjeljku "[upute za otklanjanje poteškoća]" sadrži otklanjanje poteškoća smjernice za neke od uobičajenih vezanih uz pohranu problema koje možete naići.

Na "[Appendices]" obuhvatiti informacije o korištenju drugih alata kao što su Wireshark i Netmon za analizu Fiddler za analizu HTTP/HTTPS poruke i Microsoftov analizator poruke za correlating podaci iz zapisnika mrežnih paketa podataka.


## <a name="monitoring-your-storage-service"></a>Servis za pohranu za nadzor

Ako ste upoznati s praćenje performansi sustava Windows, shvatite metrike pohrane kao Azure prostora za pohranu ekvivalent mjerača nadzor performansi sustava Windows. U metrike pohrane ćete pronaći opsežan skup metriku (mjerača u Windows Performance Monitor terminologija) kao što su dostupnost servisa, ukupan broj zahtjeva za uslugu ili postotak uspješno zahtjeva za uslugu. Potpuni popis dostupnih metriku potražite u članku [Prostora za pohranu analize metriku tablice shemu](http://msdn.microsoft.com/library/azure/hh343264.aspx). Možete odrediti želite li servis za pohranu za prikupljanje i aggregate metriku svaki sat ili svake minute. Dodatne informacije o tome kako omogućiti metriku i praćenje računa za pohranu potražite u članku [Omogućivanje metrike pohrane i prikaz podataka metriku](http://go.microsoft.com/fwlink/?LinkId=510865).

Možete odabrati e-pošte koje svaki sat metrike koju želite prikazati na [Portal za Azure](https://portal.azure.com) i konfiguriranje pravila koja će obavijestiti administratori tako da svaki put kada se svaki sat metriku premaši određeni prag. Dodatne informacije potražite u članku [Primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). 

Servis za pohranu prikuplja metriku pomoću najbolje trud, no možda snimiti svaki operaciju spremanja.

Na portalu za Azure možete pogledati metriku kao što su dostupnost, zahtjevi za Ukupno i prosječne latencije brojeva na račun za pohranu. Pravilo obavijesti i postavljen da bi i administrator ako dostupnost ispod određenu razinu. Iz prikaza te podatke, jednog moguće područja za istrage je postotak uspjeh servisa tablice koja se ispod 100% (Dodatne informacije potražite u odjeljku "[metriku prikaz malo PercentSuccess ili analize zapisnika stavke imaju operacije sa statusom transakcije ClientOtherErrors]").

Trebali biste kontinuirano praćenje Azure aplikacije da biste bili sigurni da su dobar i predstavljanja očekivani po:

- Uspostavljanje neke osnovne metriku za aplikaciju koja će vam omogućiti da usporedili trenutne podatke i izdvojili većih promjena ponašanje Azure prostora za pohranu i aplikacije. Vrijednosti na metriku osnovne, u mnogim slučajevima bit će aplikacije određene te ih Uspostava u kada su performanse testiranje aplikacije.
- Snimanje minute metriku i njihova korištenja praćenje aktivno neočekivane pogreške i anomalies kao što su krivina pogreškom broji ili zatražite stope.
- Snimanje svaki sat metriku i njihovo korištenje kao što su praćenje prosječne vrijednosti izračunati prosjek broji pogreške i zatražite stope.
- Istražuje potencijalnih problema pomoću alata za dijagnostiku kako je opisano kasnije u odjeljku "[problemi prostora za pohranu za dijagnosticiranje]."

Grafikoni u 3 slici u nastavku prikazuju kako za izračunavanje prosjeka koji se pojavljuje za svaki sat metriku možete sakriti krivina u aktivnosti. Svaki sat metriku prikazuju se da biste prikazali stižu rata zahtjeve, tijekom minute metriku otkrivanje prilagođavati razlikama koje su doista izvršava.

![][3]

Ostatak u ovom se odjeljku opisuju metrike koje treba praćenje i zašto.

### <a name="monitoring-service-health"></a>Praćenje stanja servisa

[Portal za Azure](https://portal.azure.com) možete koristiti da biste pogledali stanju servis za pohranu (i drugih servisa za Azure) u svim Azure regijama diljem svijeta. To vam omogućuje da vidite odmah ako problem izvan kontrole utjecaja servis za pohranu u regiji koristite za svoju aplikaciju.

[Portal za Azure](https://portal.azure.com) mogu nuditi obavijesti o kupljenih koje utječu na različite servise za Azure.
Napomena: Taj podatak je prije bile dostupno, zajedno s povijesnim podataka na [Nadzornoj ploči servisa Azure](http://status.azure.com).

Dok [Azure Portal](https://portal.azure.com) prikuplja informacije stanja iz unutar Azure podatkovnim centrima (unutar iz nadzor), preporučuje se i usvojio je pristup izvan u da biste generirali stilova sintetičkih transakcija koje su se povremeno pristup Azure hostira web-aplikacije s više mjesta. Primjeri takvog u vanjskoj su servise koje nudi [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) i uvide aplikacije za Visual Studio Team Services. Dodatne informacije o aplikaciji uvida za Visual Studio Team Services potražite u članku dodatak "[dodatak 5: nadzor s uvida aplikacije za Visual Studio Team Services](#appendix-5)."

### <a name="monitoring-capacity"></a>Nadzor kapaciteta

Metrike pohrane samo pohranjuje kapaciteta metriku za servis blob jer blob-ova obično računa za najveću proporcije pohranjene podatke (u vrijeme pisanja nije moguće koristiti za pohranu metrika praćenje kapacitet tablica i redova). Ove podatke možete pronaći u tablici **$MetricsCapacityBlob** ako ste omogućili nadzor za servis Blob. Metrike pohrane zapisa tih podataka jednom dnevno, a vrijednost **RowKey** možete koristiti da biste odredili hoće li se redak sadrži entitet koji se odnosi na korisnika (vrijednost **podataka**) ili analize podataka (vrijednost **analize**). Svaki pohranjene entitet sadrži informacije o prostora za pohranu koriste (**kapaciteta** mjeri bajtova) i trenutni broj spremnika (**ContainerCount**) i blob polja (**ObjectCount**) u račun za pohranu. Dodatne informacije o metriku kapaciteta pohranjenu u tablici **$MetricsCapacityBlob** potražite u članku [Prostora za pohranu analize metriku tablice shemu](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [AZURE.NOTE] Trebali biste praćenje te vrijednosti za Prijevremeni upozorenje o da se ograničenju na kapacitet vašeg računa za pohranu. Na portalu za Azure možete dodati upozorenja pravila koja vas obavještava ako koristite zbrajanja za pohranu premašuje ili pada ispod pragovi koji navedete.

Procjena veličine različitih prostora za pohranu objekata kao što su blob-ova pomoć potražite u članku na blogu [Razumijevanje Azure prostora za pohranu za naplatu – propusnosti, transakcije, i kapacitet](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Nadzor dostupnosti

Trebali biste Praćenje Dostupnost servise za pohranu u vašem računu za pohranu prateći vrijednost u stupcu **dostupnost** u tablicama svaki sat ili minute metriku — **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. **Dostupnost** stupac sadrži vrijednost postotka koji označava dostupnost usluge ili operacija API predstavljena retka ( **RowKey** prikazuje ako redak sadrži metriku za uslugu cijela ili iz određene operacije API-JA).

Sve vrijednosti manje od 100% označava da su neke zahtjeva za pohranu neuspješnih. Vidjet ćete Zašto su neuspješnih tako da Provjera drugih stupaca metriku podatke koje prikazivali brojevi zahtjeva s vrstama različite pogreške kao što su **ServerTimeoutError**. Možete očekivati da biste vidjeli **dostupnost** pasti privremeno ispod 100% razloga kao što su vremensko ograničenje tranzitne poslužitelja dok servis premješta particije bolje zahtjev za učitavanje saldo; Pokušaj logike u klijentskoj aplikaciji tretirati takve Povremeni uvjeta. U članku [prijavljeni operacija Analytics za pohranu i poruke o statusu](http://msdn.microsoft.com/library/azure/hh343260.aspx) navedene vrste transakcije koja sadrži metrike pohrane u svoj izračun **dostupnost** .

[Portal za Azure](https://portal.azure.com)možete dodati upozorenja pravila koja vas obavještava ako **dostupnost** usluge pada ispod praga koji navedete.

U odjeljku "[upute za otklanjanje poteškoća]" u ovom vodiču opisuju neke uobičajene prostora za pohranu servisa problema vezanih uz dostupnost.

### <a name="monitoring-performance"></a>Praćenje performansi

Praćenje performansi servise za pohranu, možete koristiti sljedeće mjernih podataka iz tablica metriku svaki sat i minute.

- Vrijednosti u stupcima **AverageE2ELatency** i **AverageServerLatency** prikaz Prosječno vrijeme servis za pohranu ili vrsti operacije API traje obradi zahtjeve za. **AverageE2ELatency** je mjera širine Latencija završetka do kraja koji sadrži i vrijeme za zahtjev za čitanje i slanje odgovora osim vrijeme potrebno za obradu zahtjeva (stoga obuhvaća latenciju mreže kada zahtjev je dostigne servis za pohranu); **AverageServerLatency** je mjera širine samo vrijeme obrade te stoga isključuje sve latenciju mreže povezane s komunikaciju s klijent. U odjeljku "[metriku prikaz visoke AverageE2ELatency i niska AverageServerLatency]" kasnije u ovom vodiču za raspravu programa Zašto može biti vrlo razliku između te dvije vrijednosti.
- Vrijednosti u stupcima **TotalIngress** i **TotalEgress** prikazivanje ukupnu količinu podataka, u bajta, dolaze na, a zatim otvorite iz servisima za pohranu ili putem određenu vrstu operacije API-JA.
- Vrijednosti u stupcu **TotalRequests** Pokaži ukupni broj zahtjeva za servis za pohranu API operacije Prima. **TotalRequests** je ukupan broj zahtjeva koja prima servis za pohranu.

Obično ćete nadzirati neočekivane promjene u bilo kojem od ovih vrijednosti kao pokazatelj imate li problema koje je potrebno istrage.

[Portal za Azure](https://portal.azure.com)možete dodati pravila za upozorenja koja vas obavještava Ako bilo koji od mjerenja performansi za ovaj servis jesen ispod ili Premašili prag koji navedete.

U odjeljku "[upute za otklanjanje poteškoća]" u ovom vodiču opisuju neke uobičajene prostora za pohranu servisa problema vezanih uz performanse.


## <a name="diagnosing-storage-issues"></a>Dijagnosticiranje problema prostora za pohranu

Postoji nekoliko načina na koji vam može postati svjesni problem ili problem u aplikaciji, uključuju:

- Glavna pogreška koja uzrokuje aplikacije rušiti ili mogu prestati funkcionirati.
- Bitne promjene iz osnovne vrijednosti u metriku koji nadzirete opisan u prethodnom odjeljku "[servisima za pohranu za nadzor]".
- Izvješća s korisnicima aplikacije niste dovršili neke određena operacija očekivani ili neka značajka ne funkcionira.
- Pogreške generirana unutar vaše aplikacije koje se pojavljuju u datoteke zapisnika ili neke druge metode obavijesti.

Obično problema vezanih uz servise za pohranu Azure nalaziti u jednu od četiri široke kategorije:

- Aplikacija je do problema s performansama, dojavi korisnika ili površina prikazati tako da promjene u metriku performanse.
- Došlo je do problema s Azure prostora za pohranu infrastrukturu u jednu ili više područja.
- Došlo je do pogreške, dojavi korisnika ili površina prikazati tako da povećava na jedan od metriku broj pogreške koje praćenje aplikacije.
- Tijekom razvoja i testiranja možda koristite emulator lokalno spremište; Možda ćete naići neki problemi koje se odnose na posebno korištenje emulator prostora za pohranu.

U sljedećim se odjeljcima strukture morate slijediti korake za dijagnosticiranje i otklanjanje poteškoća u svakoj od tih četiri kategorija. U odjeljku "[upute za otklanjanje poteškoća]" u nastavku ovaj vodič sadrži više detalja za neke uobičajene probleme koji se mogu pojaviti.

### <a name="service-health-issues"></a>Servis stanja problema

Problemi sa servisom stanja obično su izvan kontrole. [Portal za Azure](https://portal.azure.com) daje informacije o tijeku probleme s Azure servisa uključujući servise za pohranu. Odlučili za pristup za čitanje zemlj suvišnih pohranu prilikom stvaranja računa za pohranu, zatim slučaju podataka koji se nije dostupan u primarnom mjestu aplikacija nije privremeno prebacivanje kopija samo za čitanje na sekundarnom mjestu. Da biste to učinili, aplikacija morate moći prelaziti pomoću mjesta primarnih i sekundarnih za pohranu i moći raditi u načinu rada sa smanjenom funkcionalnošću podacima samo za čitanje. Biblioteka Azure prostora za pohranu klijenta omogućuju definiranje pravila Ponovi koji može čitati sekundarne prostora za pohranu u slučaju da ne uspije čitanje iz primarnog prostora za pohranu. Aplikacije i treba imati na umu da je podatke u sekundarni mjesto naposljetku dosljedan. Dodatne informacije potražite u članku na blogu [zalihosti mogućnosti za Azure pohrane i suvišnih prostora za pohranu čitanje zemlj.](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Problemi s performansama

Performanse aplikacije može biti subjektivna, osobito iz perspektive korisnika. Stoga je važno je imati osnovne metriku dostupna da biste lakše prepoznali gdje je možda je došlo do problema s performansama. Mnogo je čimbenika može utjecati na performanse Azure servisa za pohranu servisa perspektive klijentskog računala. Sljedećih čimbenika može raditi u servis za pohranu, klijent ili mrežne infrastrukture; Stoga je važno je imati Strategije za označavanje origin problem s performansama.

Nakon što ste identificirali vjerojatno mjesto uzrokuju problem s performansama iz metriku, datoteke zapisnika pa možete koristiti da biste pronašli detaljne podatke za dijagnosticiranje i riješili ovaj problem.

U odjeljku "[upute za otklanjanje poteškoća]" u nastavku ovog vodiča pruža dodatne informacije o neke uobičajene performanse povezane problemi koji se mogu pojaviti.

### <a name="diagnosing-errors"></a>Dijagnosticiranje pogreške

Korisnici aplikacije možda će vas obavijestiti o pogreškama dojavi klijentske aplikacije. Metrike pohrane zapisa i brojanja pogreške različite vrste s servise za pohranu kao što su **NetworkError**, **ClientTimeoutError**ili **AuthorizationError**. Dok metrike pohrane samo zapisi brojanja vrste različite pogreške, možete dobiti dodatne detalje o pojedinačne zahtjeve tako da Provjera poslužiteljsko, klijentsko i zapisnika mreže. Šifra stanja HTTP vratio servis za pohranu će ponuditi naznaku Zašto zahtjeva nije uspjelo.

> [AZURE.NOTE] Imajte na umu da možete očekivati da biste vidjeli neke Povremeni pogreške:, na primjer, pogreške zbog tranzitne mrežni uvjeti ili pogreške aplikacije.

U sljedećim resursima su korisne za razumijevanje vezanih uz pohranu statusa i pogreške šifre:

- [Šifre uobičajene pogreške REST API-JA](http://msdn.microsoft.com/library/azure/dd179357.aspx)
- [Kodovi pogrešaka servisa blobova platforme](http://msdn.microsoft.com/library/azure/dd179439.aspx)
- [Kodovi pogrešaka servis reda čekanja](http://msdn.microsoft.com/library/azure/dd179446.aspx)
- [Kodovi pogrešaka servisa tablice](http://msdn.microsoft.com/library/azure/dd179438.aspx)
- [Kodovi pogrešaka servisa datoteka](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Prostor za pohranu emulator problema

Azure SDK sadrži prostor za pohranu emulator mogu se izvoditi na radne stanice razvoj. Ovaj emulator simulira Većina ponašanja servisa Azure prostora za pohranu i koristan tijekom razvoj i testiranje, što vam omogućuje da pokretanje aplikacije koje koriste servise za pohranu Azure bez potrebe za Azure pretplate i račun za Azure prostora za pohranu.

U odjeljku "[upute za otklanjanje poteškoća]" u ovom vodiču opisuju neke uobičajene probleme s naišao na korištenje emulator prostora za pohranu.

### <a name="storage-logging-tools"></a>Alati za zapisivanje za pohranu

Spremanje zapisnika omogućuje poslužiteljsko zapisivanje zahtjeva za pohranu u vašem računu Azure prostora za pohranu. Dodatne informacije o tome kako omogućiti poslužiteljsko zapisivanje i pristup podacima zapisnika potražite u članku [Omogućivanje prijave za pohranu i pristup podaci iz zapisnika](http://go.microsoft.com/fwlink/?LinkId=510867).

Klijent biblioteka za pohranu za .NET omogućuje prikupljanje klijentsko podaci iz zapisnika koji se odnosi na postupci za pohranu obavlja aplikacije. Dodatne informacije potražite u članku [zapisivanja na klijentskoj strani s bibliotekom .NET prostora za pohranu klijenta](http://go.microsoft.com/fwlink/?LinkId=510868).

> [AZURE.NOTE] U nekim okolnostima (kao što su pogreške autorizacije SAS) korisnik može izvješća pogreške za koje možete pronaći nema podataka o zahtjev u zapisnicima poslužiteljsko prostora za pohranu. Možete koristiti mogućnosti zapisivanja u biblioteku za pohranu klijenta da biste istražili ako je uzrok problema na klijentskom računalu ili pomoću alata za nadzor mreže da biste istražili s mrežom.

### <a name="using-network-logging-tools"></a>Pomoću alata za zapisivanje mreže

Možete snimiti promet između postupak klijentske i poslužiteljske pružaju podrobne informacije o postupak klijentske i poslužiteljske razmjena podataka i temeljni uvjeti na mreži. Korisni mrežnih zapisivanje alata obuhvaćaju sljedeće:

- [Fiddler](http://www.telerik.com/fiddler) je besplatno web proxy koji omogućuje vam da biste pregledali zaglavlja i tereta podataka HTTP i HTTPS zahtjeva i odgovora poruka za ispravljanje pogrešaka. Dodatne informacije potražite u članku [dodatak 1: korištenje Fiddler da biste snimili HTTP i HTTPS promet](#appendix-1).
- [Microsoft Network Monitor (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) i [Wireshark](http://www.wireshark.org/) su besplatni mrežni protokol analyzers koji omogućuju prikaz paketa detaljne informacije o širokog raspona mrežne protokole. Dodatne informacije o alatu Wireshark potražite u odjeljku "[dodatak 2: korištenje Wireshark da biste snimili mrežni promet](#appendix-2)".
- Microsoftov analizator poruka je alat od Microsofta koji nadjačava Netmon i koji se uz dohvaćanje mrežnih podataka paketa, omogućuje vam prikaz i analiza podataka zapisnika zabilježene iz drugih alata. Dodatne informacije potražite u odjeljku "[dodatak 3: pomoću programa Microsoft poruke Analyzer da biste snimili mrežni promet](#appendix-3)".
- Ako želite da biste izvršili osnovne test da biste provjerili je li klijentskom računalu možete se povezati sa servisom Azure prostora za pohranu putem mreže, to nije moguće pomoću alata za standardne **ping** na klijentskom računalu. Međutim, možete koristiti [Alat za **tcping** ](http://www.elifulkerson.com/projects/tcping.php) da biste provjerili povezivanje.

U mnogim slučajevima, podaci zapisnika prijave za pohranu i klijentska biblioteka za pohranu bit će potrebne za dijagnosticiranje problema, ali u nekim slučajevima morat ćete detaljnije informacije koje možete unijeti te alate za zapisivanje mreže. Ako, na primjer, pomoću Fiddler da biste vidjeli poruke HTTP i HTTPS vam omogućuje da vidite zaglavlje i tereta podataka koji se šalju i iz servise za pohranu koji će vam omogućuju da provjeriti kako će se klijentska aplikacija ponovnih pokušaja operacije prostora za pohranu. Protokol analyzers kao što su Wireshark raditi na razini paketa omogućujući vam da biste vidjeli TCP podatke koji bi vam omogućuju da rješavanje problema s izgubiti pakete i problema s povezivanjem. Alat za analizu poruke rade HTTP- a TCP slojeva.

## <a name="end-to-end-tracing"></a>Praćenje završetka do kraja

Praćenje završetka do kraja pomoću raznih datoteke zapisnika je korisno tehnika istražuje potencijalne probleme. Informacije o datumu/vremenu iz podataka metriku možete koristiti kao naznaku ponavljajućoj u datoteke zapisnika informacija koje će vam pomoći da riješite problem.

### <a name="correlating-log-data"></a>Correlating podaci iz zapisnika

Prilikom pregleda zapisnika s klijentskim aplikacijama, prati mreže i zahtjeve za pohranu poslužiteljsko zapisivanje je važnosti da biste mogli povezivanje preko različitih zapisničke datoteke. Datoteke zapisnika obuhvaća broj različitih polja korisni kao identifikatori korelacije. Zahtjev za id klijenta je najkorisnije polje za povezivanje stavki u različitim zapisnika. No katkad može biti korisna id zahtjev poslužitelja ili vremenske oznake. Sljedeći odjeljci sadrže dodatne informacije o tim mogućnostima.

### <a name="client-request-id"></a>ID klijenta zahtjev

Klijentska biblioteka za pohranu automatski generira id za zahtjev za jedinstveni klijenta za svaki zahtjev.

- U zapisniku klijentsko koji stvara klijentska biblioteka za pohranu, u polju **ID klijenta zahtjev za** svaku stavku zapisnika koji se odnose na zahtjev za pojavit će se zahtjev za id klijenta.
- U mrežnog prometa kao što su nešto zabilježene po Fiddler, zahtjev za id klijenta vidljiv je u zahtjev za poruke kao vrijednost **x-ms--zahtjev-id klijenta** HTTP zaglavlje.
- Poslužiteljsko zapisivanje prostora za pohranu zapisnika, zahtjev za id klijenta će se pojaviti u stupcu ID zahtjev za klijenta.

> [AZURE.NOTE]Moguće je više zahtjeva za zajedničko korištenje isti id klijenta zahtjev jer klijent možete dodijeliti tu vrijednost (iako klijentska biblioteka za pohranu automatski dodjeljuje nove vrijednosti). Slučaju ponovne pokušaje putem klijentskog programa, sve pokušaje zajednički koristiti isti zahtjev id klijenta. U slučaju grupe koji se šalju putem klijentskog programa, skupine ima id zahtjev jednog klijenta.


### <a name="server-request-id"></a>ID zahtjev poslužitelja

Servis za pohranu automatski generira poslužitelj zahtjeva ID-a.

- Poslužiteljsko zapisivanje prostora za pohranu zapisnik, id zahtjeva za poslužitelj će se pojaviti **ID zahtjeva zaglavlja** stupca.
- U praćenju mrežom kao što su nešto zabilježene po Fiddler id zahtjev poslužitelja pojavit će se u porukama za odgovor kao vrijednost **x ms-zahtjev-id** HTTP zaglavlje.
- U zapisniku klijentsko koji stvara klijentska biblioteka za pohranu id zahtjev poslužitelja prikazuje se u stupcu **Tekst postupak** za stavku evidencije prikazuje detalje o odgovor poslužitelja.

> [AZURE.NOTE] Servis za pohranu uvijek dodjeljuje poslužiteljem jedinstveni id zahtjeva svaki zahtjev za Prima, tako da svaki pokušaj pokušaj putem klijentskog programa i svaki operacija obuhvaćeno seriji ima id zahtjev jedinstveni poslužitelja.

Ako klijentska biblioteka za pohranu throws **StorageException** u klijentu, svojstvo **RequestInformation** sadrži **RequestResult** objekt koji sadrži svojstvo **ServiceRequestID** . **RequestResult** objekta možete pristupiti i putem **OperationContext** instance.

Uzorak koda ispod pokazuje kako postaviti prilagođene vrijednosti **ClientRequestId** prema priložite objekta **OperationContext** zahtjev za servis za pohranu. Također se prikazuje kako dohvatiti **ServerRequestId** vrijednost iz poruka s odgovorom.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Vremenske oznake

Da biste pronašli stavke povezane evidencije, ali Budite oprezni od bilo kojeg skew sat između klijenta i poslužitelja koje možda postoje možete koristiti i vremenske oznake. Pretražujte plus ili minus 15 minuta za koji se podudaraju poslužiteljsko unose koji se temelje na vremensku oznaku na klijentskom računalu. Imajte na umu da blob metapodataka za blob-ova koji sadrže određene parametre označava vremenski raspon za metriku pohranjene u blob; To je korisno ako imate mnogo metriku blob-ova za tu minuta ili sat.

## <a name="troubleshooting-guidance"></a>Upute za otklanjanje poteškoća

U ovom se odjeljku će vam pomoći s na Dijagnostika i otklanjanje poteškoća s nekih uobičajenih problema aplikacije naići prilikom korištenja servisa Azure prostora za pohranu. Koristite na popisu u nastavku da biste pronašli informacije koje su važne za konkretan problem.

**Otklanjanje poteškoća s stabla odluke**

----------

Ne poteškoću odnose se na performanse jedne od servise za pohranu?

- [Prikaži metriku visoke AverageE2ELatency i niska AverageServerLatency]
- [Metriku prikaz malo AverageE2ELatency i niska AverageServerLatency, ali klijent se pojavili visokom latencijom]
- [Metriku prikaz visoke AverageServerLatency]
- [Nailazite na neočekivano kašnjenja u isporuke poruke na u redu čekanja]

----------

Ne poteškoću odnose se na dostupnost jedne od servise za pohranu?

- [Metriku prikaz povećava u PercentThrottlingError]
- [Metriku prikaz povećava u PercentTimeoutError]
- [Metriku prikaz povećava u PercentNetworkError]

----------

Klijentska aplikacija prima je odgovor HTTP 4XX (kao što su 404) iz servisa za pohranu?

- [Klijent prima poruke 403 HTTP (zabranjeno)]
- [Klijent prima poruke 404 HTTP (nema)]
- [Klijent prima poruke 409 HTTP (Sukob)]

----------

[Metriku prikaz malo PercentSuccess ili analize zapisnika stavke imaju operacije sa statusom transakcije ClientOtherErrors]

----------

[Kapacitet metriku prikaz neočekivane povećava u korištenja kapacitet pohrane]

----------

[Nailazite na neočekivano ponovna virtualnim strojevima koji imaju velik broj priložene VHDs]

----------

[Problem nastaje pomoću emulator prostora za pohranu za razvoj ili test]

----------

[Se pojavili problema s instalacijom Azure SDK za .NET]

----------

[Imate različite problem sa servisom za pohranu]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Prikaži metriku visoke AverageE2ELatency i niska AverageServerLatency

Na slici ispod s [Portala za Azure](https://portal.azure.com) alat za nadzor prikazuje primjer gdje je znatno veću od **AverageServerLatency** **AverageE2ELatency** .

![][4]

Imajte na umu da servis za pohranu samo izračunava metriku **AverageE2ELatency** uspješno zahtjeva za i, za razliku od **AverageServerLatency**sadrži i vrijeme klijent uzima podatke za slanje i primanje potvrđivanje iz servisa za pohranu. Stoga razlika između **AverageE2ELatency** i **AverageServerLatency** može biti zbog klijentska aplikacija se sporo odgovoriti ili zbog uvjeta na mreži.

> [AZURE.NOTE] Možete pogledati i **E2ELatency** i **ServerLatency** za pojedinačne prostora za pohranu operacije u podaci zapisnika prijave za pohranu.

#### <a name="investigating-client-performance-issues"></a>Istražuje probleme s performansama klijenta

Mogućih razloga za klijentski program sporo reagirati uključuju pojavljuju ograničen broj dostupnih veze ili niti se Nedostatak resursa kao što je propusnost procesora, memorije ili mreži. Možda ćete moći riješiti problem s izmjenom kod klijenta koji će biti učinkovitije (na primjer korištenjem asinkronog pozive na servis za pohranu) ili pomoću naredbe veće virtualnog računala (pomoću više jezgri i dodatnu memoriju).

Za servise tablice i reda čekanja algoritam Nagle također može uzrokovati visoke **AverageE2ELatency** to **AverageServerLatency**: dodatne informacije potražite u članku objave [Nagle na algoritam je ne neslužbeni pri Small zahtjeva](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). Algoritam Nagle u kodu možete onemogućiti pomoću klase **ServicePointManager** u **System.Net** naziva. To biste trebali učiniti prije upućivanje poziva u tablici ili otvorite red services u aplikaciji jer to ne utječe na veze koje se već nalaze. U sljedećem primjeru dolazi iz metoda **Application_Start** u ulogu suradnika.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Provjerite klijentski se zapisnici da biste vidjeli koliko zahtjeve za razgovore klijentska aplikacija je slanje i provjeri Općenito .NET povezani performanse grla u klijentu kao što je procesora, .NET smeća, Upotreba mreže ili memorije. Kao početnu točku za .NET klijentske aplikacije za otklanjanje poteškoća potražite u članku [značajka ispravljanja pogrešaka, praćenje, a Profiliranje](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Problemi s istražuje Latencija mrežama

Obično je visoke završetka do kraja Latencija uzrokovanih mreže zbog tranzitne uvjeta. Možete Istražite oba tranzitne i stalni mreže probleme kao što su izostavljanje pakete pomoću alata kao što su Wireshark ili Microsoftov analizator poruke.

Dodatne informacije o korištenju Wireshark otklanjanje poteškoća s mrežom, u odjeljku "[dodatak 2: korištenje Wireshark da biste snimili mrežni promet]."

Dodatne informacije o korištenju alata za analizu poruka Microsoft otklanjanje poteškoća s mrežom, u odjeljku "[dodatak 3: korištenje alata za analizu Microsoft Message da biste snimili mrežni promet]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Metriku prikaz malo AverageE2ELatency i niska AverageServerLatency, ali klijent se pojavili visokom latencijom

U ovom scenariju, najvjerojatnije je kašnjenje u zahtjeva za pohranu da servis za pohranu. Treba istražiti Zašto zahtjeva putem klijentskog programa se ne želite ih putem servisa blob.

Jedan od mogućih razloga za klijentski program što slanja zahtjeva za su koje je ograničen broj dostupnih veze ili niti.

Trebali biste provjerite i li klijent obavlja više ponovne pokušaje i istražiti uzrok ako je to slučaj. Da biste odredili hoće li klijent obavlja više ponovne pokušaje, možete učiniti sljedeće:

- Pregledajte zapisnike Analytics za pohranu. Ako se pojavljivati više ponovne pokušaje, vidjet ćete više operacija isti ID zahtjev za klijenta, ali sa zahtjevom za drugi poslužitelj ID-a.
- Pregledajte zapisnike klijenta. Opširno zapisivanje označava da je pokušaj došlo je do.
- Ispravljanje pogrešaka u kodu i provjerite svojstva objekta **OperationContext** pridružene zahtjev. Ako je postupak ponoviti, svojstvo **RequestResults** neće sadržavati više jedinstvenih poslužitelj zahtjeva ID-a. Možete provjeriti i vremena početka i završetka za svaki zahtjev za potvrdu. Dodatne informacije potražite u članku uzorak koda u odjeljku [ID zahtjev poslužitelja].

Ako ne postoje problemi u klijentu, treba istražiti potencijalne probleme mreža kao što su gubitka paketa. Možete koristiti alate kao što su Wireshark ili Microsoftov analizator poruku da biste istražili problema s mrežom.

Dodatne informacije o korištenju Wireshark otklanjanje poteškoća s mrežom, u odjeljku "[dodatak 2: korištenje Wireshark da biste snimili mrežni promet]."

Dodatne informacije o korištenju alata za analizu poruka Microsoft otklanjanje poteškoća s mrežom, u odjeljku "[dodatak 3: korištenje alata za analizu Microsoft Message da biste snimili mrežni promet]."

### <a name="metrics-show-high-AverageServerLatency"></a>Metriku prikaz visoke AverageServerLatency

U slučaju visoke **AverageServerLatency** blob preuzimanje zahtjeva za, koristite zapisnike prijave za pohranu da biste vidjeli postoje li koji se ponavljaju zahtjeva za istu blob (ili skup blob-ova). Blob prijenos zahtjeve, treba istražiti koje bloka veličina klijent koristi (na primjer, blokira manja od 64K veličine može uzrokovati folije osim ako su za čitanje i u manja od 64K blokova), i ako je više klijenata prenosite blokova u istom blob paralelno. Trebali biste provjeriti i metriku-minutni krivina u broj zahtjeva koje rezultiraju premašuju na po drugi skalabilnost ciljnih web-mjesta: vidjeti "[metriku prikaz povećava PercentTimeoutError]."

Ako vam se prikazuje visoke **AverageServerLatency** za blob preuzimanje zahtjeve kad postoje koji se ponavljaju zahtjeva za istu blob ili skup blob-ova, zatim razmotrite predmemoriranje te blob-ova pomoću predmemorije Azure ili Azure mrežom sadržaja isporuke (CDN). Za prijenos zahtjeve, možete poboljšati propusnost pomoću veću veličinu bloka. Da bi upiti s tablicama i moguće je implementirati klijentsko predmemoriranje klijenti koje izvođenje operacije isti upita i gdje se podaci ne mijenjaju često.

Visoke vrijednosti **AverageServerLatency** može biti simptom loše dizajnirane tablice ili upite rezultat operacije pregleda ili koji pratite dodavanjem/prepend protiv uzorak. Dodatne informacije potražite "[metriku prikaz povećava PercentThrottlingError]".

> [AZURE.NOTE] Možete pronaći sveobuhvatan popis za provjeru uspješnosti kontrolnog popisa ovdje: [Microsoft Azure prostora za pohranu performanse i skalabilnost kontrolnog popisa](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Nailazite na neočekivano kašnjenja u isporuke poruke na u redu čekanja

Ako ste naišli na razmak između vremena aplikacije dodaje poruku u red i vrijeme postane dostupna za čitanje iz reda čekanja, trebali biste provedite sljedeće korake za dijagnosticiranje problema:

- Provjerite je li aplikacija uspješno dodavanje poruke u redu čekanja. Provjerite da aplikacija se nije ponovni metodu **AddMessage** nekoliko puta prije uspješnu. Biblioteka za pohranu klijenta zapisnike prikazivat će se neki koji se ponavljaju ponovne pokušaje operacija prostora za pohranu.
- Provjerite je li je definiran skew između radnih uloge dodaje poruku reda čekanja i ulogu suradnika koji čita poruku iz reda čekanja koji ga čini se kao da je odgodu obrada.
- Provjerite je li ulogu suradnika koji se čita poruke iz reda čekanja neuspješnih. Ako klijent red poziva metodu **GetMessage** , ali ne reagira pomoću programa potvrđivanje, poruka će ostati nevidljivi na u redu čekanja do isteka razdoblja **invisibilityTimeout** . Sada poruku postaje dostupan za obradu ponovno.
- Potvrdite okvir ako je Duljina reda čekanja rast tijekom vremena. To se može dogoditi ako nemate dovoljno zaposlenici zaduženi za dostupne za obradu sve poruke koje su drugi zaposlenici zaduženi za postavljanje na red. Trebali biste provjeriti i metriku da biste vidjeli ako brisanje zahtjeva za neuspjeh i broj dequeue na poruke koje se mogu upućivati ponavljaju nije uspjela prilikom pokušaja da biste izbrisali poruku.
- Pregledajte zapisnike prijave za pohranu za sve operacije reda čekanja koje nemaju veća od očekivanih **E2ELatency** i **ServerLatency** vrijednosti na dulje vremensko razdoblje nego inače.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Metriku prikaz povećava u PercentThrottlingError

Regulacije pogreške pojaviti ako premašite odredišta skalabilnost servis za pohranu. Servis za pohranu ne da biste bili sigurni da nema jednog klijenta ili klijent može koristiti servis štetu drugim korisnicima. Dodatne informacije potražite u članku [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md) pojedinosti o skalabilnost ciljevi za račune za pohranu i ciljeve particije unutar računa za pohranu.

Ako metrika **PercentThrottlingError** prikazali povećava postotak zahtjeva koji se ne uspijeva uz regulacije poruku o pogrešci, morate istražiti jedan od dva scenarija:

- [Tranzitne povećava PercentThrottlingError]
- [Trajno povećava PercentThrottlingError pogreške]

Povećava **PercentThrottlingError** pojavljivanja i istovremeno kao povećava se broj zahtjeva za pohranu ili kada ste prethodno učitavanje testiranje aplikacije. To može također manifesta sam u klijentu kao "503 poslužitelj zauzet" ili "500 operacija vremenskog ograničenja" HTTP poruke o statusu operacija prostora za pohranu.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Tranzitne povećava PercentThrottlingError

Ako vam se prikazuje krivina vrijednost **PercentThrottlingError** koje se podudaraju s razdoblja visoke aktivnosti za aplikaciju, trebali biste implementirati eksponencijalne (ne linearni) natrag isključiti strategiju za ponovne pokušaje u klijentu: to će smanjiti odmah opterećenje particija i pomoć za aplikacije za glačanje izvan krivina u promet. Dodatne informacije o implementaciji pravila Ponovi pomoću klijentska biblioteka za pohranu potražite u članku [Microsoft.WindowsAzure.Storage.RetryPolicies prostor naziva](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [AZURE.NOTE] Možda ćete vidjeti krivina vrijednost **PercentThrottlingError** koje se podudaraju s razdoblja visoke aktivnosti za aplikaciju: najvjerojatnije ovdje je servis za pohranu premještanje particije da biste poboljšali opterećenja.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Trajno povećava PercentThrottlingError pogreške

Ako vam se prikazuje dosljedno visoka vrijednost za sljedeće **PercentThrottlingError** trajna povećanje u vašim jedinicama transakcije ili kada izvršavate vaša početna učitavanja testira na aplikaciju, a zatim morate procjeni kako aplikacija koristi particije prostora za pohranu i hoće li se bliži skalabilnost ciljevi za račun za pohranu. Ako, na primjer, ako vam se prikazuje ograničavanje pogrešaka na čekanja (čime se broji kao jedna particija), zatim razmotrite korištenje dodatne redove možete proširiti transakcije preko više particija. Ako vam se prikazuje ograničavanje pogrešaka na tablice, morate razmislite o korištenju različitih shema particioniranja možete proširiti svoje transakcije preko više particija pomoću širem krugu particija ključa vrijednosti. Jedan uobičajenih uzroka problem se prepend/pridruživanje protiv uzorak gdje odaberite datum kao ključ particija, a zatim sve podatke na određeni dan zapisuje se jedna particija: opterećenju, to može rezultirati usko grlo za pisanje. Trebali biste razmislite o različitim stvaranje particija dizajna ili procijeniti je li pomoću blobova možda bolje rješenje. Trebali biste provjeri je li za ograničavanje se pojavljuje uslijed krivina u prometa i istražiti načina izglađivanje vaše uzorak zahtjeva.

Ako svoje transakcije raspodijelite više particija, morate i dalje biti svjesni ograničenja skalabilnost postaviti za račun za pohranu. Na primjer, ako ste koristili deset redovi svaki obrade maksimalno 2000 poruke 1KB sekundi, će biti pri ukupno ograničenje od 20 000 poruka u sekundi za račun za pohranu. Ako vam je potrebna za obradu više od 20 000 entiteti sekundi, razmislite o pomoću više računa za pohranu. Trebali biste nose i na umu da veličine zahtjeve i entiteti ima utjecaj na kada je servis za pohranu regulira klijentima: Ako imate veće zahtjeve i entiteti, koji mogu biti ograničio vrijeme ranije.

Dizajn upita Neučinkovit također mogu prouzročiti da pogoditi iz prvog skalabilnost ograničenja za tablicu particije. Ako, na primjer, upit s filtrom koji samo odabire jedan postotku entiteti u particije, ali koja skenira svi entiteti particije morat ćete pristupiti svaki entitet. Svaki entitet čitati će se računati pri ukupan broj transakcija u particije; Dakle, možete jednostavno dođete do skalabilnost ciljeve.

> [AZURE.NOTE] Performanse testiranje trebali biste otkrili sve Neučinkovit podaci za dizajne upita u aplikaciji.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Metriku prikaz povećava u PercentTimeoutError

Vaš metriku Pokaži povećava **PercentTimeoutError** zbog nekog od servise za pohranu. U isto vrijeme klijent prima veliku količinu poruke o statusu HTTP "500 operacija vremensko ograničenje" operacija prostora za pohranu.

> [AZURE.NOTE] Možda ćete vidjeti pogreške vremenskog ograničenja privremeno kao servis za pohranu Učitavanje zahtjeva za salda pomicanjem particije novi poslužitelj.

Metriku **PercentTimeoutError** je prikupljene sljedeće metriku: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**i **SASServerTimeoutError**.

Vremensko ograničenje poslužitelja uzrokuju pogrešku na poslužitelju. Vremenska ograničenja klijent dogoditi jer je operaciju na poslužitelju vremenskog ograničenja određen klijenta; Ako, na primjer, pomoću klijentska biblioteka za pohranu klijenta možete postaviti vremenskog ograničenja za operaciju korištenjem svojstva **ServerTimeout** klase **QueueRequestOptions** .

Vremensko ograničenje poslužitelja upućivati na problem sa servisom za pohranu koji zahtijeva dodatno istrage. Metriku možete koristiti da biste vidjeli ako su odlazak skalabilnost ograničenja za uslugu i da biste odredili sve krivina u promet koji uzrokuju problem. Ako je problem povremen, možda će biti zbog ujednačavanje opterećenja aktivnosti u servisu. Ako se problem je stalni i je uzrokovano aplikacije odlazak skalabilnost ograničenja servisa, trebali biste podići podršku problem. Vremenska ograničenja za klijenta, morate odlučiti ako vremensko ograničenje je postavljeno na odgovarajuću vrijednost u klijentu i ili promijeni vrijednost vremenskog ograničenja u klijentu ili istražiti kako možete poboljšati performanse operacije u servis za pohranu, primjerice optimiziranje upita za tablicu ili smanjivanje veličine poruka.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Metriku prikaz povećava u PercentNetworkError

Vaš metriku Pokaži povećava **PercentNetworkError** zbog nekog od servise za pohranu. Metriku **PercentNetworkError** je prikupljene sljedeće metriku: **NetworkError**, **AnonymousNetworkError**i **SASNetworkError**. Ove se dogoditi kada servis za pohranu otkriva mrežne pogreške kada klijent čini zahtjeva za pohranu.

Najčešći uzrok te pogreške je klijent prekida veze prije isteka vremenskog ograničenja u servisima za pohranu. Treba istražiti kod u klijent za razumijevanje Zašto i kada klijent prekida vezu sa servisom za pohranu. Možete koristiti i Wireshark, Microsoftov analizator poruke ili Tcping da biste istražili problemi s mrežom putem klijentskog programa. Ti Alati opisana su u [Appendices].

### <a name="the-client-is-receiving-403-messages"></a>Klijent prima poruke 403 HTTP (zabranjeno)

Ako klijentska aplikacija je prijavi pogrešaka 403 HTTP (zabranjeno), vjerojatno uzrok je klijent je pomoću programa istekle zajednički pristup potpis (SAS) kada šalje zahtjeva za pohranu (iako druge mogući uzroci uključuju tipki sat skew, koji nisu valjani i prazan zaglavlja). Ako je ključa istekle SAS uzrok, nećete vidjeti sve stavke u podataka za zapisnik poslužiteljsko zapisivanje prostora za pohranu. Sljedeća tablica prikazuje uzorak iz zapisnika klijentsko generira klijentska biblioteka za pohranu koji ilustrira pojavljivanja problema:

Izvor|Verbosity|Verbosity|Id klijenta zahtjev|Operacija teksta
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Informacije o|3|85d077ab-...|Mjesto primarni po načinu mjesto PrimaryOnly počevši od operacije.
Microsoft.WindowsAzure.Storage|Informacije o|3|85d077ab-...|Pokretanje sinkrono zahtjev za https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;Promijeni = mypolicy&amp;potpisa = OFnd4Rd7z01fIvh 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3D&amp;api-verzija = 2014. 02 14.
Microsoft.WindowsAzure.Storage|Informacije o|3|85d077ab-...|Čeka se odgovor.
Microsoft.WindowsAzure.Storage|Upozorenje|2|85d077ab-...|Izbačena tijekom čeka se odgovor: udaljeni poslužitelj vraća pogrešku: (403) zabranjeno...
Microsoft.WindowsAzure.Storage|Informacije o|3|85d077ab-...|Odgovora. Šifra stanja = 403, zatražite ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, sadržaj MD5 = e-oznake =.
Microsoft.WindowsAzure.Storage|Upozorenje|2|85d077ab-...|Izbačena tijekom operacije: udaljeni poslužitelj vraća pogrešku: (403) zabranjeno...
Microsoft.WindowsAzure.Storage|Informacije o|3 |85d077ab-...|Provjera je li se ponoviti postupak. Ponovite count = 0, Šifra stanja HTTP = 403, iznimka = udaljeni poslužitelj vraća pogrešku: (403) zabranjeno...
Microsoft.WindowsAzure.Storage|Informacije o|3|85d077ab-...|Na sljedeću lokaciju je postavljen primarni, ovisno o načinu mjesto.
Microsoft.WindowsAzure.Storage|Pogreška|1|85d077ab-...|Pravila Ponovi jeste li dopušta na pokušaj. Neuspjeh s udaljeni poslužitelj vraća pogrešku: (403) zabranjeno.

U ovom scenariju treba istražiti Zašto SAS token istječe prije nego što klijent šalje token na poslužitelj:

- Obično ne postavite početno vrijeme prilikom stvaranja SAS za klijentski ako želite odmah koristiti. Ako postoje razlike malu glavnom računalu za generiranje SAS pomoću trenutnog vremena i servis za pohranu, a zatim je servis za pohranu prima SAS koji još nije valjana.
- Na SAS ne postavite vrijeme kratkog isteka. Ponovno malu razlike između glavnom računalu za generiranje na SAS i servis za pohranu može dovesti do SAS su starije od očekivanu istječe.
- Ne parametar verzija u ključu SAS (na primjer **SZ = 2015 04 05**) odgovara verziji klijentska biblioteka za pohranu koristite? Preporučujemo da uvijek koristite najnoviju verziju [Biblioteka za pohranu klijenta](https://www.nuget.org/packages/WindowsAzure.Storage/).
- Ako Obnovi tipke za pristup za pohranu, to možete poništiti valjanost bilo koje postojeće SAS tokena. Ako generiranje SAS tokeni s dugim isteka vremena za klijentske aplikacije u predmemoriju Razlog može biti problem.

Ako koristite biblioteku za pohranu klijenta za generiranje SAS tokena, zatim jednostavno je da biste sastavili valjani token. Međutim, ako koristite za pohranu REST API-JA i ručno izgradnje tokeni SAS pažljivo pročitajte u temi [Prenošenjem pristup potpisom zajednički pristup](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>Klijent prima poruke 404 HTTP (nema)
Ako klijentska aplikacija prima poruku 404 HTTP (nema) s poslužitelja, podrazumijeva da objekt koji se klijent pokušavao koriste (kao što je entitet, tablice, blob, spremnik ili čekanja) ne postoji u servis za pohranu. Postoji nekoliko mogućih razloga za to, kao što su:

- [Klijent ili drugi proces prethodno izbrisali objekt]
- [Problem za autorizaciju zajednički pristup potpis (SAS)]
- [Klijentsko JavaScript kod imaju dozvolu pristupa objekta]
- [Pogreška s mrežom]

#### <a name="client-previously-deleted-the-object"></a>Klijent ili drugi proces prethodno izbrisali objekt
U slučajevima gdje klijent pokušava čitanje, ažuriranje i brisanje podataka u servis za pohranu je obično lakše u zapisnicima poslužiteljsko prethodne operacije koje dotičnog objekt izbrisali iz servis za pohranu. Često, podaci zapisnika prikazuju se da drugi korisnik ili postupak izbrisali objekt. U poslužiteljsko zapisivanje prostora za pohranu zapisnika, vrsti operacije i zatražili objekta – ključa stupaca prikazati kada klijentsko izbrisana objekta.

U scenariju gdje klijent pokušava Umetanje objekta je možda neće biti odmah očite Zašto rezultat je odgovor 404 HTTP (nema) given da klijent stvara novi objekt. Međutim, ako je klijent stvara blob mora biti može pronaći kontejner s blob Ako klijent stvara poruku moraju imati mogućnost da biste pronašli reda, a klijent dodaje redak je moraju imati mogućnost da biste pronašli tablicu.

Zapisnik klijentsko iz biblioteke za pohranu klijenta možete koristiti da bi se dobio na detaljnije razumjeli kada klijent šalje određenim zahtjevima za servis za pohranu.

Zapisnik klijentsko sljedeće generira biblioteka za pohranu klijenta ilustrira problem kada klijent nije moguće pronaći spremnik za blob ga stvara. Ovaj zapisnik sadrži detalje za sljedeće postupke za pohranu:

ID zahtjeva|Postupak
---|---
07b26a5d-...|Način **DeleteIfExists** da biste izbrisali kontejner s blob. Imajte na umu da ovaj postupak sadrži zahtjev za **ZAGLAVLJE** da biste provjerili postojanje spremnika.
e2d06d78...|Način **CreateIfNotExists** stvaranje kontejnera za blob. Imajte na umu da ovaj postupak sadrži zahtjev za **LAKŠI** koji provjerava postojanje parametra spremnik. **LAKŠI** vraća 404 poruku, ali i dalje.
de8b1c3c-...|Način **UploadFromStream** da biste stvorili blob-om. Zahtjev za **STAVITI** ne uspije 404 poruke

Stavke zapisnika:

ID zahtjeva |  Operacija teksta
---|---
07b26a5d-...|Početni sinkrono zahtjev za https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 2014. lipnja 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Čeka se odgovor.
07b26a5d-... | Odgovora. Šifra stanja = 200, zatražite ID = eeead849-... Sadržaj MD5 = e-oznake = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Odgovor zaglavlja uspješno su obrađene, nastavite s ostatkom postupak.
07b26a5d-... | Preuzimanje tijelo odgovor.
07b26a5d-... | Operacija je uspješno dovršena.
07b26a5d-... | Početni sinkrono zahtjev za https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 2014. lipnja 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Čeka se odgovor.
07b26a5d-... | Odgovora. Šifra stanja = 202, zatražite ID = 6ab2a4cf-..., sadržaj MD5 = e-oznake =.
07b26a5d-... | Odgovor zaglavlja uspješno su obrađene, nastavite s ostatkom postupak.
07b26a5d-... | Preuzimanje tijelo odgovor.
07b26a5d-... | Operacija je uspješno dovršena.
e2d06d78-... | Početni asinkronog zahtjev za https://domemaildist.blob.core.windows.net/azuremmblobcontainer.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 2014. lipnja 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Čeka se odgovor.
de8b1c3c-... | Početni sinkrono zahtjev za https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... |  StringToSign = STAVI... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-blob-Type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-date:Tue, 03 2014. lipnja 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Priprema za pisanje zahtjev podataka.
e2d06d78-... | Izbačena tijekom čeka se odgovor: udaljeni poslužitelj vraća pogrešku: (404) nije pronađen...
e2d06d78-... | Odgovora. Šifra stanja = 404, zatražite ID = 353ae3bc-..., sadržaj MD5 = e-oznake =.
e2d06d78-... | Odgovor zaglavlja uspješno su obrađene, nastavite s ostatkom operacija.
e2d06d78-... | Preuzimanje tijelo odgovor.
e2d06d78-... | Operacija je uspješno dovršena.
e2d06d78-... | Početni asinkronog zahtjev za https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
e2d06d78-...|StringToSign = STAVI... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-date:Tue, 03 2014. lipnja 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Čeka se odgovor.
de8b1c3c-... | Podaci zahtjeva za pisanje.
de8b1c3c-... | Čeka se odgovor.
e2d06d78-... | Iznimka tijekom čeka se odgovor: udaljeni poslužitelj vraća pogrešku: (409) sukoba...
e2d06d78-... | Odgovora. Šifra stanja = 409, zatražite ID = c27da20e-..., sadržaj MD5 = e-oznake =.
e2d06d78-... | Preuzimanje tijelo odgovor pogreške.
de8b1c3c-... | Izbačena tijekom čeka se odgovor: udaljeni poslužitelj vraća pogrešku: (404) nije pronađen...
de8b1c3c-... | Odgovora. Šifra stanja = 404, zatražite ID = 0eaeab3e-..., sadržaj MD5 = e-oznake =.
de8b1c3c-...| Iznimka tijekom operacije: udaljeni poslužitelj vraća pogrešku: (404) nije pronađen...
de8b1c3c-... | Pravila Ponovi jeste li dopušta na pokušaj. Neuspjeh s udaljeni poslužitelj vraća pogrešku: (404) nije pronađen...
e2d06d78-... | Pravila Ponovi jeste li dopušta na pokušaj. Neuspjeh s udaljeni poslužitelj vraća pogrešku: (409) sukoba...

U ovom primjeru zapisnik prikazuje da klijent je interleaving zahtjeve iz metode **CreateIfNotExists** (zahtjev id e2d06d78...) zahtjevima iz metode **UploadFromStream** (de8b1c3c-...); To se događa jer klijentska aplikacija je asinkrono pozivanje ove metode. Izmijenite asinkronog kod u klijentu da biste bili sigurni da stvara spremnik prije da biste prenijeli sve podatke na blob u njih. Najbolje sve spremnike potrebno stvoriti unaprijed.

#### <a name="SAS-authorization-issue"></a>Problem za autorizaciju zajednički pristup potpis (SAS)

Ako klijentska aplikacija pokušava koristiti SAS ključ koji sadrže potrebne dozvole za operaciju, servis za pohranu vraća poruku 404 HTTP (nema) klijentu. Istovremeno, vidjet ćete i vrijednost različita od nule za **SASAuthorizationError** u metriku.

Sljedeća tablica prikazuje ogledne poruke zapisnika poslužiteljsko zapisivanje prostora za pohranu datoteci zapisnika programa:

<table>
  <tr>
    <td>Zahtjev za vrijeme početka</td>
    <td>2014.-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Vrsti operacije</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Stanje zahtjeva</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>Šifra stanja HTTP</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Vrsta provjere autentičnosti</td>
    <td>SAS</td>
  </tr>
  <tr>
    <td>Vrsta servisa</td>
    <td>Blob</td>
  </tr>
  <tr>
    <td>Zahtjev za URL-a</td>
    <td>
    https://domemaildist.blob.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp; sr = c&amp;amp; Promijeni = mypolicy&amp;amp; potpisa = XXXXX&amp;amp; api-verzija = 2014. 02 14&amp;amp;</td>
  </tr>
  <tr>
    <td>Zahtjev za zaglavlja ID-a</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>ID klijenta zahtjev</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

Zašto se klijentska aplikacija pokušava za izvođenje operacije je nije dobio dozvole za treba istražiti.

#### <a name="JavaScript-code-does-not-have-permission"></a>Klijentsko JavaScript kod imaju dozvolu pristupa objekta

Ako koristite JavaScript klijenta i servis za pohranu je povratkom HTTP 404 poruke, potražite sljedeće pogreške JavaScript u pregledniku:

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Možete koristiti alate za razvojne inženjere F12 u pregledniku Internet Explorer za praćenje poruka razmjenjivati između za preglednik i servis za pohranu kada klijentsko JavaScript problema.

Te se pogreške događaju jer web-pregledniku implementira sigurnosnog ograničenja [ista pravila polazište](http://www.w3.org/Security/wiki/Same_Origin_Policy) koji sprječava pozivom API na drugu domenu s domene na stranici dolazi iz web-stranice.

Da biste riješili problem JavaScript, možete konfigurirati unakrsne polazište resursa zajedničko korištenje (CORS) za servis za pohranu klijenta pristupa. Dodatne informacije potražite u članku [zajedničko korištenje više polazište resursa (CORS) podrška za Azure servise za pohranu](http://msdn.microsoft.com/library/azure/dn535601.aspx).

Sljedećim primjerom koda objašnjava konfiguriranje servisa za blob da biste omogućili JavaScript pokrenut u domeni Contoso da biste pristupili blob na servisu za spremište blobova platforme:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Pogreška s mrežom

U nekim okolnostima izgubiti mrežnih paketa može dovesti do servis za pohranu povratka HTTP 404 poruke klijent. Ako, na primjer, kada klijentska aplikacija je entitet brisanje iz tablice servisa vidjet vratiti izvješća na iznimku prostora za pohranu klijenta programa "HTTP 404 (nema)" poruka o stanju servisa tablice. Kada istražili tablice u servis za pohranu tablice, vidjet ćete da jeste li servis izbrisati entitet kao što ste tražili.

Detalji iznimke u klijentu uključuju id zahtjeva (7e84f12d...) dodijelio servisa tablice zahtjev: te podatke olakšati pronalaženje detalji zahtjeva u zapisnicima poslužiteljsko prostora za pohranu pretraživanjem **zahtjev id zaglavlja** stupca u datoteke zapisnika. Da biste odredili kada pogreške kao što su ova pojaviti i pretraživanje datoteke zapisnika na temelju vremena metriku snimljena ta se pogreška može koristiti i metriku. Ta stavka zapisnika prikazuje da status porukom "HTTP (404) klijent druge pogreške" nije uspjelo brisanje. Istu stavku zapisnika obuhvaća i id zahtjeva generira klijent u stupcu **klijent, zahtjev i id** (813ea74f...).

Zapisnik poslužiteljsko obuhvaća i neke druge stavke s istom vrijednošću **klijent, zahtjev i id** (813ea74f...) za postupak brisanja uspješno odnosi na isti entitet, a zatim iz iste klijenta. Ovaj postupak uspio brisanja došlo je do vrlo uskoro prije nije uspio zahtjeva.

Vjerojatno uzrok scenarij je klijent prima brisanje zahtjeva za entitet servis za tablicu, koji je uspjela, ali nije primio je potvrđivanje s poslužitelja (možda zbog problema s privremeni problem s mrežom). Klijent zatim automatski ponoviti postupak (pomoću isti **klijent, zahtjev i id**), a u ovom pokušaj nije uspjelo jer je entitet već izbrisano.

Ako se problem pojavljuje se često, treba istražiti Zašto se klijent neuspješnih prima acknowledgements iz tablice servisa. Ako je problem povremen, trebali biste prekriva pogreške "HTTP (404) nije pronađen" i prijava u klijentu ali Dopusti klijentu da biste nastavili.

### <a name="the-client-is-receiving-409-messages"></a>Klijent prima poruke 409 HTTP (Sukob)

Sljedeća tablica prikazuje se izdvojiti iz zapisnika poslužiteljsko za dva operacije na klijentu: **DeleteIfExists** odmah slijedi **CreateIfNotExists** dajte isti naziv spremnik blob. Imajte na umu da svaki klijent operacija rezultira dva zahtjeva poslane na poslužitelj, najprije **GetContainerProperties** zahtjev za smanjenja spremnik, nakon čega slijedi zahtjev za **DeleteContainer** ili **CreateContainer** .

Vremenska oznaka|Postupak|Rezultat|Naziv spremnika|Id klijenta zahtjev
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

Kod u klijentskoj aplikaciji briše i odmah obnavlja spremniku blob dajte isti naziv: način **CreateIfNotExists** (klijent zatražite ID bc881924-...) ipak ne uspijeva uz poruku o pogrešci 409 HTTP (Sukob). Kada klijentsko briše blob spremnika, tablice ili redova postoji kratak razdoblja prije naziva ponovo postane dostupna.

Klijentska aplikacija trebali biste koristiti spremnik jedinstvene nazive kad god se stvara novi spremnika ako nije zajednička uzorak Izbriši/ponovo Stvori.

### <a name="metrics-show-low-percent-success"></a>Metriku prikaz malo PercentSuccess ili analize zapisnika stavke imaju operacije sa statusom transakcije ClientOtherErrors

Metriku **PercentSuccess** snima postotak operacije koje su uspješno na temelju HTTP kod stanja. Operacija pomoću kodova stanja 2XX Brojanje kao uspjelo dok operacija pomoću kodova stanja 3XX, 4XX i 5XX rasponi broje se kao nije uspjelo i donje **PercentSucess** metričkim vrijednost. Na popisu datoteke zapisnika poslužiteljsko prostora za pohranu te operacije bilježe se sa statusom transakcije **ClientOtherErrors**.

Nije važno Imajte na umu da te operacije je uspješno dovršeno i zbog toga ne utječu na druge metriku kao što su dostupnost. Primjeri operacije koje uspješno izvršavanje, ali koji može uzrokovati neuspješnih HTTP kodovima stanja obuhvaćaju sljedeće:
- **ResourceNotFound** (Nije moguće pronaći 404), primjerice iz zahtjevom GET u blob koji ne postoji.
- **ResouceAlreadyExists** (Sukob 409), primjerice iz operacije **CreateIfNotExist** gdje se resursa već postoji.
- **ConditionNotMet** (Ne i mijenjati 304), primjerice s uvjetnim operacija kao što su kada klijent pošalje **jedna** vrijednost i HTTP **If-ništa-Match** zaglavlje da bi se zatražila sliku samo ako je ažurirana od posljednje operacije.

Možete pronaći popis uobičajenih REST API-JA šifre pogrešaka koje vraćaju servise za pohranu na stranici [Uobičajenih OSTALE šifre pogrešaka API -JA](http://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Kapacitet metriku prikaz neočekivane povećava u korištenja kapacitet pohrane


Ako vidite iznenadno neočekivane promjene u kapaciteta na vašem računu za pohranu, možete ispitati razloga po prvi pogled na određene parametre dostupnost; Ako, na primjer, povećava se broj nije uspjelo brisanje zahtjeva za mogu dovesti do povećava mogu blobova koje koristite kao operacije određene čišćenja aplikacije koje možda imaju očekujete da se osloboditi prostor možda ne radi prema očekivanjima (na primjer, jer tokeni SAS koji se koristi za oslobađanje prostora istekao).

### <a name="you-are-experiencing-unexpected-reboots"></a>Kada se pojave neočekivana ponovna od virtualnim računalima sustava Azure s velikim brojem priloženom VHDs

Ako je virtualnog računala Azure (VM) ima velik broj priložene VHDs koje se nalaze u istim računom za pohranu, ne može prelaziti skalabilnost ciljevi za račun pojedinačne prostora za pohranu uzrokuju VM uvoza. Trebali biste provjeriti minute metriku za račun za pohranu (**TotalRequests**/**TotalIngress**/**TotalEgress**) za krivina koje premašuju skalabilnost ciljevi za račun za pohranu. Pogledajte odjeljak "[metriku prikaz povećava PercentThrottlingError]" za pomoć prilikom određivanja ako ograničavanje pojavila na računu za pohranu.

Općenito govoreći, svaki pojedinačnih unosa ili izlazna operacija na VHD iz virtualnog računala prevodi se **Stranice nabavite** ili **Postavljanje stranice** operacije u podlozi blob stranice. Zbog toga procijenjeno IOPS okruženju sustava možete koristiti za ugađanje koliko VHDs možete imati jednu za pohranu računa temelju određene ponašanje aplikacije. Ne preporučujemo da imate više od 40 diskova u prostor za pohranu za jedan račun. Potražite u članku [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md) pojedinosti o trenutnom skalabilnost ciljevi za račune za pohranu, posebice zahtjev za Ukupno stopu i ukupne propusnosti za vrstu računa za pohranu koristite.
Ako su premašuju skalabilnost ciljevi za vaš račun za pohranu, morate potvrditi svoje VHDs više prostora za pohranu za različite račune da biste smanjili aktivnosti u svaki pojedini račun.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Problem nastaje pomoću emulator prostora za pohranu za razvoj ili test

Obično korištenje prostora za pohranu emulator tijekom razvoja i testirati da biste izbjegli preduvjeta za račun za Azure prostora za pohranu. Uobičajeni problemi koji se mogu pojaviti kada koristite za pohranu emulator su:

- [Značajka "X" ne funkcioniraju u emulator prostora za pohranu]
- [Pogreška "vrijednost zbog nekog od zaglavlja HTTP nije u ispravnom obliku" prilikom korištenja emulator prostora za pohranu]
- [Pokretanje emulator prostora za pohranu zahtijeva administratorske ovlasti]

#### <a name="feature-X-is-not-working"></a>Značajka "X" ne funkcioniraju u emulator prostora za pohranu

Prostor za pohranu emulator ne podržava sve značajke servisa Azure prostora za pohranu kao što je servis datoteka. Dodatne informacije potražite u članku [korištenje prostora za pohranu Emulator Azure za razvoj i testiranje](storage-use-emulator.md).

Za značajke koje ne podržava emulator prostora za pohranu, pomoću servisa Azure prostora za pohranu u oblaku.

#### <a name="error-HTTP-header-not-correct-format"></a>Pogreška "vrijednost zbog nekog od zaglavlja HTTP nije u ispravnom obliku" prilikom korištenja emulator prostora za pohranu

Testirate aplikacije koje koriste klijentska biblioteka za pohranu protiv lokalne pohrane emulator i metoda poziva kao što su **CreateIfNotExists** neće funkcionirati uz poruku o pogrešci "vrijednost zbog nekog od zaglavlja HTTP nije u ispravnom obliku." To označava verziju emulator prostora za pohranu koristite podržava verziju klijentska biblioteka za pohranu koji koristite. Klijentska biblioteka za pohranu dodaje zaglavlja **x ms-verzija** za sve zahtjeve za će se. Ako za pohranu emulator prepoznaje vrijednost u zaglavlju **x ms verzija** , odbacuje zahtjev.

Zapisnike prostora za pohranu klijenta za biblioteke možete koristiti da biste vidjeli vrijednost **x ms verzija zaglavlja** se šalje. Vrijednost **x ms verzija zaglavlja** možete vidjeti i ako koristite Fiddler za praćenje zahtjeve iz klijentske aplikacije.

Ovaj scenarij obično se pojavljuje ako ste instalirali i koristite najnoviju verziju klijentska biblioteka za pohranu bez ažuriranja emulator prostora za pohranu. Trebali biste instalirali najnoviju verziju emulator prostora za pohranu ili koristite za pohranu u oblaku umjesto u emulator za razvoj i testiranje.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Pokretanje emulator prostora za pohranu zahtijeva administratorske ovlasti

Zatraži administratorske vjerodajnice prilikom pokretanja emulator prostora za pohranu. To pojavljuje samo kada su Inicijalizacija emulator prostora za pohranu po prvi put. Nakon što ste pokrenuti emulator prostora za pohranu, ne morate administratorske ovlasti da biste ponovno pokrenuli.

Dodatne informacije potražite u članku [korištenje prostora za pohranu Emulator Azure za razvoj i testiranje](storage-use-emulator.md). Imajte na umu da možete i pokrenuti emulator prostora za pohranu u Visual Studio, koji će potrebne administratorske ovlasti.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Se pojavili problema s instalacijom Azure SDK za .NET

Prilikom pokušaja instalacije SDK ne uspijeva pokušavate instalirati emulator prostora za pohranu na lokalnom računalu. Zapisnik o instalaciji sadrži jednu od sljedećih poruka:

- CAQuietExec: Pogreška: nije moguće pristupiti instancom SQL-a
- CAQuietExec: Pogreška: nije moguće stvoriti bazu podataka

Razlog može biti problem s instalacijom postojeće LocalDB. Emulator prostora za pohranu po zadanom koristi LocalDB održati podataka kada je simulira servisa Azure prostora za pohranu. Možete vratiti na instancu LocalDB pokretanjem sljedeće naredbe u prozor naredbenog retka prije no što pokušate instalirati SDK-a.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

Naredba **Izbriši** uklanja sve stare datoteke baze podataka iz prethodnih instalacija emulator prostora za pohranu.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Imate različite problem sa servisom za pohranu

Ako ranijim odlomcima za otklanjanje poteškoća sadrži problem s servis za pohranu, trebali biste prihvaćaju sljedeće pristup za dijagnosticiranje i otklanjanje problema.

- Provjerite svoje metriku da biste vidjeli postoji li bilo kakve promjene iz očekivano osnovnog retka. Iz metrika, možda ćete moći određivanje je li se problem ne tranzitne ili trajna te operacijama prostora za pohranu problem se utjecaja.
- Informacije o metriku možete koristiti da biste lakše traženje podataka poslužiteljsko zapisnik detaljnije informacije o pogreškama koje se pojavljuje. Ove informacije može vam pomoći pronalaženje i otklanjanje problema.
- Ako informacije u zapisnicima poslužiteljsko nije dovoljno uspješno otklanjanje poteškoća, klijentsko zapisnike biblioteka za pohranu klijenta možete koristiti da biste istražili ponašanje klijentska aplikacija te alate kao što su Fiddler, Wireshark i Microsoftov analizator poruku da biste istražili mrežom.

Dodatne informacije o korištenju Fiddler u odjeljku "[dodatak 1: korištenje Fiddler da biste snimili HTTP i HTTPS promet]."

Dodatne informacije o korištenju Wireshark potražite u članku "[dodatak 2: korištenje Wireshark da biste snimili mrežni promet]."

Dodatne informacije o korištenju alata za analizu poruka Microsoft potražite u članku "[dodatak 3: korištenje alata za analizu Microsoft Message da biste snimili mrežni promet]."

## <a name="appendices"></a>Appendices

Na appendices opišite nekoliko alata koji će vam korisne kada su dijagnosticiranje i otklanjanje problema s Azure prostora za pohranu (i drugim servisima). Ti Alati nisu dio Azure prostora za pohranu i neke su proizvode treće. Kao takve, alatima koji se spominju u te appendices su prekriveni sve ugovor podršku možda imaju Microsoft Azure ili Azure prostora za pohranu i stoga u sklopu postupka procjenu treba provjeriti licenciranje i podršku mogućnosti dostupne u davatelja od tih alata.

### <a name="appendix-1"></a>Dodatak 1: Korištenje Fiddler da biste snimili HTTP i HTTPS promet

[Fiddler](http://www.telerik.com/fiddler) je koristan alat za analizu promet HTTP i HTTPS između klijentska aplikacija i servisa Azure prostora za pohranu koji koristite.

> [AZURE.NOTE] Fiddler možete dešifrirati promet HTTPS; pročitajte dokumentaciju Fiddler pažljivo da biste razumjeli kako to čini, a da biste shvatili sigurnosne posljedice.

Ovo je dodatak sadrži kratak vodič kako konfigurirati Fiddler da biste snimili promet između lokalnom stroju koji ste instalirali Fiddler i servise za Azure pohranu.

Nakon pokretanja Fiddler ga će početi Dohvaćanje HTTP i HTTPS promet na lokalnom računalu. Ovo su neke korisne naredbe za kontrolu Fiddler:

- Zaustavljanje i počnite hvatanje promet. Na glavni izbornik idite na karticu **datoteka** , a zatim **Snimite promet** da biste se prebacivali hvatanje uključiti i isključiti.
- Spremanje podataka snimljenu promet. Glavni izbornik, otvorite **datoteku**, kliknite **Spremi**, a zatim kliknite **Sve sesije**: to vam omogućuje da spremite promet u sesiju arhivsku datoteku. Ponovno učitajte sesiju arhiva kasnije za analizu ili Pošalji ako ste tražili Microsoftovoj podršci.

Da biste ograničili količinu prometa koji opisuje Fiddler, koristite filtre koja konfigurirati na kartici **filtara** . Sljedeće snimka zaslona prikazuje filtar koji opisuje samo promet poslane na krajnjoj točki za pohranu **contosoemaildist.table.core.windows.net** :

![][5]

### <a name="appendix-2"></a>Dodatak 2: Korištenje Wireshark da biste snimili mrežni promet

[Wireshark](http://www.wireshark.org/) je alat za analizu protokol mreže koji vam omogućuje da vidite paketa detaljne informacije o širokog raspona mrežne protokole.

Sljedeći postupak objašnjava da biste snimili paketa detaljne informacije o promet s lokalnog računala koje ste instalirali Wireshark na servis za tablice na vašem računu Azure prostora za pohranu.

1.  Pokrenite Wireshark na lokalnom računalu.
2.  U odjeljku **Početak** odaberite sučelja lokalne mreže ili sučelja koji su povezani s Internetom.
3.  Kliknite **Mogućnosti snimanja**.
4.  Dodajte filtra u tekstni okvir **Filtar za snimanje** . Ako, na primjer, **glavno računalo contosoemaildist.table.core.windows.net** će konfigurirati Wireshark da biste snimili samo pakete šalje u ili iz tablice krajnja točka servisa na računu za pohranu **contosoemaildist** . Pogledajte [popis svih filtara za snimanje](http://wiki.wireshark.org/CaptureFilters).

    ![][6]

5.  Kliknite **Start**. Wireshark sada snimite sve slati pakete ili iz krajnja točka servisa tablice prilikom korištenja klijentska aplikacija na lokalnom računalu.
6.  Kada završite, na glavni izbornik kliknite **snimanje** , a zatim **Zaustavi**.
7.  Da biste spremili snimljenu podataka u programu Wireshark snimiti datoteke, na glavni izbornik kliknite **datoteka** , a zatim **Spremi**.

WireShark će istaknuti sve pogreške koje postoji u prozoru **packetlist** . Možete koristiti i prozor **Stručnjaka Info** (kliknite **Analiza**, a zatim **Stručnjaka informacije**) da biste pogledali sažetak pogrešaka i upozorenja.

![][7]

Možete odabrati i da se TCP podaci kao što je aplikacijskom sloju vidi desnom tipkom miša na podataka TCP i odabirom **slijedite TCP strujanje**. To je posebno korisno ako zabilježene na ispis bez filtra snimke. Dodatne informacije potražite u članku [pratiti TCP strujanja] (http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [AZURE.NOTE] Dodatne informacije o korištenju Wireshark, potražite u članku [Vodič za korisnike Wireshark](http://www.wireshark.org/docs/wsug_html_chunked).

### <a name="appendix-3"></a>Dodatak 3: Pomoću alata za analizu Microsoft poruku da biste snimili mrežni promet

Koristite Microsoftov analizator poruku da biste snimili HTTP i HTTPS promet na sličan način da biste Fiddler i snimite mrežni promet na isti način u programu Wireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Konfiguriranje sesije web praćenje pomoću alata za analizu Microsoft poruke

Da biste konfigurirali sesije praćenje web za HTTP i HTTPS promet pomoću alata za analizu Microsoft poruke, pokrenite aplikaciju Microsoftov analizator poruku, a zatim u na izborniku **datoteka** kliknite **Prikupljanje i praćenje**. Na popisu dostupni praćenje scenariji odaberite **Web Proxy**. Zatim u oknu **Konfiguracije scenarij praćenja** u tekstni okvir **HostnameFilter** dodajte imena pohranu krajnje točke (možete potražiti nazivi [Portal za Azure](https://portal.azure.com)). Na primjer, ako je naziv vašeg računa Azure prostora za pohranu **contosodata**, trebali biste dodajte sljedeće tekstni okvir **HostnameFilter** :

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Znak razmaka odvaja u hostnames.

Kada ste spremni za početak prikupljanja podataka za praćenje, kliknite gumb **Start s** .

Dodatne informacije o praćenju Microsoftov analizator poruke **Web Proxy** , potražite u članku [Microsoft PEF WebProxy davatelja](http://technet.microsoft.com/library/jj674814.aspx).

Ugrađeni **Web Proxy** praćenja u Microsoftov analizator poruke temelji se na Fiddler; ga možete snimiti klijentsko HTTPS promet i prikaz šifrirane poruke HTTPS. Praćenje **Web Proxy** funkcionira konfiguriranjem lokalne proxy poslužitelja za sve HTTP i HTTPS promet koji je omogućuje pristup šifrirane poruke.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Dijagnosticiranje problema s mrežom pomoću alata za analizu Microsoft poruke

Uz praćenje Microsoftov analizator poruke **Web Proxy** za snimanje pojedinosti prometa HTTP/HTTPs između klijentske aplikacije i servis za pohranu možete koristiti i ugrađeno praćenje **Lokalnu vezu sloja** za snimanje informacija paketa mreže. Ovo omogućuje spremanje podataka slično onome koje možete snimiti s Wireshark i dijagnosticiranje mreže probleme kao što su prekida pakete.

Sljedeće snimku zaslona prikazuje primjer praćenje **Lokalnu vezu sloj** s **nekim pružaju** u stupcu **DiagnosisTypes** . Klikom na ikonu u stupcu **DiagnosisTypes** prikazuje detalje o poruku. U ovom primjeru poslužitelj preusmjeriti kojim poruka #305 jer nije primio je potvrđivanje putem klijentskog programa:

![][9]

Kada stvorite sesije praćenja u Microsoftov analizator poruke, možete odrediti filtre da biste smanjili količinu suvišnih u rezultatu praćenja. Na **snimiti / praćenje** stranici gdje definirate praćenja kliknite **Konfiguriraj** vezu pokraj odjeljka **Microsoft-Windows-NDIS-PacketCapture**. Sljedeća slika prikazuje konfiguracije koji filtrira TCP promet za IP adrese od tri servise za pohranu:

![][10]

Dodatne informacije o Microsoft poruke alata za analizu lokalnu vezu sloja praćenja potražite u članku [Microsoft-PEF-NDIS-PacketCapture davatelja usluga](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Dodatak 4: Pomoću programa Excel da biste pogledali metriku i zapisivanje podataka

Alati za mnoge omogućuju preuzimanje metrike pohrane podataka iz spremišta Azure tablica u obliku razdvojeni koja olakšava učitali podatke u Excel radi prikaza i analize. Prostor za pohranu zapisivanje podataka iz spremišta blobova platforme Azure je već razdvojeni oblik koji može učitati u programu Excel. Međutim, morate dodati naslove odgovarajući stupac u informacije na web-mjesto [Za pohranu analize zapisnika oblik](http://msdn.microsoft.com/library/azure/hh343259.aspx) i u okvir za [Prostora za pohranu analize metriku tablice sheme](http://msdn.microsoft.com/library/azure/hh343264.aspx).

Da biste uvezli podatke za pohranu zapisivanje u programu Excel nakon preuzmite ga iz spremišta blobova:

- Na izborniku **Podaci** kliknite **Iz teksta**.
- Dođite do datoteke zapisnika koje želite pregledati, a zatim kliknite **Uvezi**.
- U koraku 1 **Čarobnjaka za uvoz teksta**odaberite **Razgraničeno**.

Na korak 1 **Čarobnjak za uvoz teksta**, odaberite **sa zarezom** kao samo graničnik, a zatim dvaput ponudu kao **kvalifikatorom teksta**. Zatim kliknite **Završi** i odaberite gdje želite smjestiti podatke u radnoj knjizi.

### <a name="appendix-5"></a>Dodatak 5: Nadzor s uvide aplikacije za Visual Studio Team Services

Možete koristiti i značajku uvida aplikacije za Visual Studio Team Services kao dio performanse i nadzor dostupnosti. Ovaj alat učiniti sljedeće:

- Provjerite je li web-servisa dostupan je i odredište. Hoće li je aplikacija na web-mjestu ili web-aplikacijom uređaja koji koristi web-servisa, to možete testirajte URL svakih nekoliko minuta s mjesta diljem svijeta i obavijesti ako je došlo do problema.
- Brzo dijagnosticiranje probleme s performansama ni iznimke u web-servisa. Saznajte ako su u tijeku Rastegnuto procesora ili drugih resursa, stoga kašnjenja zatražite od iznimke i jednostavno pretražiti kašnjenja zapisnika. Ako u aplikaciji performanse ispod prihvatljiva ograničenja, ne možemo može poslati poruku e-pošte. Možete nadzirati .NET i Java web-servisi.

Možete pronaći dodatne informacije na [što je aplikacija uvida?](../application-insights/app-insights-overview.md).

<!--Anchors-->
[Uvod]: #introduction
[Način organiziranja ovaj vodič]: #how-this-guide-is-organized

[Servis za pohranu za nadzor]: #monitoring-your-storage-service
[Praćenje stanja servisa]: #monitoring-service-health
[Nadzor kapaciteta]: #monitoring-capacity
[Nadzor dostupnosti]: #monitoring-availability
[Praćenje performansi]: #monitoring-performance

[Dijagnosticiranje problema prostora za pohranu]: #diagnosing-storage-issues
[Servis stanja problema]: #service-health-issues
[Problemi s performansama]: #performance-issues
[Dijagnosticiranje pogreške]: #diagnosing-errors
[Prostor za pohranu emulator problema]: #storage-emulator-issues
[Alati za zapisivanje za pohranu]: #storage-logging-tools
[Pomoću alata za zapisivanje mreže]: #using-network-logging-tools

[Praćenje završetka do kraja]: #end-to-end-tracing
[Correlating podaci iz zapisnika]: #correlating-log-data
[ID klijenta zahtjev]: #client-request-id
[ID zahtjev poslužitelja]: #server-request-id
[Vremenske oznake]: #timestamps

[Upute za otklanjanje poteškoća]: #troubleshooting-guidance
[Prikaži metriku visoke AverageE2ELatency i niska AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Metriku prikaz malo AverageE2ELatency i niska AverageServerLatency, ali klijent se pojavili visokom latencijom]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Metriku prikaz visoke AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Nailazite na neočekivano kašnjenja u isporuke poruke na u redu čekanja]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Metriku prikaz povećava u PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Tranzitne povećava PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Trajno povećava PercentThrottlingError pogreške]: #permanent-increase-in-PercentThrottlingError
[Metriku prikaz povećava u PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Metriku prikaz povećava u PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[Klijent prima poruke 403 HTTP (zabranjeno)]: #the-client-is-receiving-403-messages
[Klijent prima poruke 404 HTTP (nema)]: #the-client-is-receiving-404-messages
[Klijent ili drugi proces prethodno izbrisali objekt]: #client-previously-deleted-the-object
[Problem za autorizaciju zajednički pristup potpis (SAS)]: #SAS-authorization-issue
[Klijentsko JavaScript kod imaju dozvolu pristupa objekta]: #JavaScript-code-does-not-have-permission
[Pogreška s mrežom]: #network-failure
[Klijent prima poruke 409 HTTP (Sukob)]: #the-client-is-receiving-409-messages

[Metriku prikaz malo PercentSuccess ili analize zapisnika stavke imaju operacije sa statusom transakcije ClientOtherErrors]: #metrics-show-low-percent-success
[Kapacitet metriku prikaz neočekivane povećava u korištenja kapacitet pohrane]: #capacity-metrics-show-an-unexpected-increase
[Nailazite na neočekivano ponovna virtualnim strojevima koji imaju velik broj priložene VHDs]: #you-are-experiencing-unexpected-reboots
[Problem nastaje pomoću emulator prostora za pohranu za razvoj ili test]: #your-issue-arises-from-using-the-storage-emulator
[Značajka "X" ne funkcioniraju u emulator prostora za pohranu]: #feature-X-is-not-working
[Pogreška "vrijednost zbog nekog od zaglavlja HTTP nije u ispravnom obliku" prilikom korištenja emulator prostora za pohranu]: #error-HTTP-header-not-correct-format
[Pokretanje emulator prostora za pohranu zahtijeva administratorske ovlasti]: #storage-emulator-requires-administrative-privileges
[Se pojavili problema s instalacijom Azure SDK za .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Imate različite problem sa servisom za pohranu]: #you-have-a-different-issue-with-a-storage-service

[Appendices]: #appendices
[Dodatak 1: Korištenje Fiddler da biste snimili HTTP i HTTPS promet]: #appendix-1
[Dodatak 2: Korištenje Wireshark da biste snimili mrežni promet]: #appendix-2
[Dodatak 3: Pomoću alata za analizu Microsoft poruku da biste snimili mrežni promet]: #appendix-3
[Dodatak 4: Pomoću programa Excel da biste pogledali metriku i zapisivanje podataka]: #appendix-4
[Dodatak 5: Nadzor s uvide aplikacije za Visual Studio Team Services]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
