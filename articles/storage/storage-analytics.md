<properties
    pageTitle="Prikupljanje zapisnika i metrike podataka pomoću analize prostora za pohranu | Microsoft Azure"
    description="Analytics za pohranu omogućuje vam da biste pratili metriku podataka za sve servise za pohranu i prikupljanje zapisnika za pohranu Blob, reda čekanja i tablice."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Prostor za pohranu Analytics

## <a name="overview"></a>Pregled

Azure Analytics za pohranu izvodi zapisivanje i njihovi metriku podataka računa za pohranu. Ove podatke možete koristiti praćenje zahtjeve, analizi trendova korištenje i dijagnosticiranje problema s računom za pohranu.

Da biste koristili Analytics za pohranu, morate omogućiti pojedinačno za svaku uslugu koju želite nadzirati. Možete je omogućiti s [Portala za Azure](https://portal.azure.com). Detalje potražite u članku [monitora na račun za pohranu na portalu za Azure](storage-monitor-storage-account.md). Možete omogućiti i analize prostora za pohranu programski putem REST API-JA ili u biblioteku klijenta. Kako se koriste operacije [Dobiti svojstva Blob servisa](https://msdn.microsoft.com/library/hh452239.aspx), [Dobiti svojstva reda čekanja servisa](https://msdn.microsoft.com/library/hh452243.aspx), [Dobiti svojstva tablice servisa](https://msdn.microsoft.com/library/hh452238.aspx)i [Dobiti svojstva datoteke servisa](https://msdn.microsoft.com/library/mt427369.aspx) da biste omogućili Analytics za pohranu za svaku uslugu

Zbrojeno podaci se pohranjuju u dobro poznatog blob (za zapisivanje) i dobro poznate tablice (za metriku), koji mogu pristupiti s Blob i servis tablice API-ji.

Prostor za pohranu analize ima ograničenje 20 TB iznosa pohranjene podatke koji ne ovisi o ukupno ograničenje za vaš račun za pohranu. Dodatne informacije o naplati i pravilnicima zadržavanja podataka potražite u članku [Analytics za pohranu i naplati](https://msdn.microsoft.com/library/hh360997.aspx). Dodatne informacije o ograničenjima prostora za pohranu računa potražite u članku [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md).

Detaljnije vodič o korištenju Analytics za pohranu i druge alate za prepoznavanje, dijagnosticiranje i otklanjanje poteškoća vezanih uz Azure prostora za pohranu, potražite u članku [Monitor, dijagnosticiranje, i otklanjanje poteškoća s spremište na platformi Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>O zapisivanje Analytics za pohranu

Prostor za pohranu Analitika prijavi detaljne informacije o uspješnih i neuspješnih zahtjeva za servis za pohranu. Ove informacije može se koristiti za praćenje pojedinačne zahtjeve i dijagnosticiranje problema sa servisom za pohranu. Zahtjevi za prijavljeni najbolje trud temelj.

Stavke evidencije stvaraju se samo ako je aktivnosti servisa za pohranu. Ako, na primjer, ako na račun za pohranu ima aktivnosti na njegovu servisu Blob, ali ne i u uslugama tablice ili red, samo zapisnika vezani uz servis Blob stvorit će se.

Zapisivanje Analytics za pohranu nije dostupna za servis za Azure datoteka.

### <a name="logging-authenticated-requests"></a>Zahtjevi za zapisivanje autentičnost

Prijavljeni ste sljedeće vrste čija je autentičnost provjerena zahtjeva za:

- Zahtjevi za uspješan.

- Nije uspjelo zahtjeve, uključujući vremensko ograničenje, ograničavanje, mreže, autorizacije i drugih pogrešaka.

- Zahtjevi za korištenje zajednički pristup potpis (SAS), uključujući zahtjeva nije uspjelo i uspješno.

- Zahtjevi za analize podataka.

Zahtjevi za koje je načinio prostora za pohranu analize odvojeno, kao što su zapisnika stvaranje ili brisanje, niste prijavljeni. Potpuni popis prijavljenih podataka navedenih u temama [prijavljeni operacije Analytics za pohranu i poruke o statusu](https://msdn.microsoft.com/library/hh343260.aspx) i [Oblik zapisnika analize](https://msdn.microsoft.com/library/hh343259.aspx) .

### <a name="logging-anonymous-requests"></a>Bilježenje u zapisnik anonimni zahtjeva
Prijavljeni ste sljedeće vrste anonimni zahtjeva za:

- Zahtjevi za uspješan.

- Pogreške na poslužitelju.

- Pogreške vremenskog ograničenja za klijenta i poslužitelja.

- Nije uspjelo zahtjevi za DOHVAĆANJE kod pogreške 304 (ne i mijenjati).

Sve ostale nije uspjelo anonimni zahtjeve niste prijavljeni. Potpuni popis prijavljenih podataka navedenih u teme [prijavljen operacije analize za pohranu i poruke o statusu](https://msdn.microsoft.com/library/hh343260.aspx) i [Oblik zapisnika analize](https://msdn.microsoft.com/library/hh343259.aspx) .

### <a name="how-logs-are-stored"></a>Kako se pohranjuju zapisnika
Sve zapisnike spremaju se u blok blob-ova u kontejner $logs koji se automatski stvara kada Analytics za pohranu je omogućen za račun za pohranu. Spremnik $logs nalazi se u blob naziva računa za pohranu, na primjer: `http://<accountname>.blob.core.windows.net/$logs`. Ovaj spremnik nije moguće izbrisati kada je omogućena Analytics za pohranu, iako moguće je izbrisati njegov sadržaj.

>[Azure.NOTE] Spremnik $logs ne prikazuje se kada spremnik stavku operacija izvođenja, kao što je način [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) . To morate pristupiti izravno. Ako, na primjer, možete koristiti metodu [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) da biste pristupili blob-ova u na `$logs` spremnik.
Kao što su prijavljeni zahtjeve, analitičkih podataka za pohranu prenesite Srednja rezultate kao blokove. Prostor za pohranu analize povremeno će izvršavanje te blokira i učinite ih dostupnima kao blob.

Dupliciranih zapisa mogu postojati za zapisnici stvoreni u istom sat. Možete odrediti ako zapisa je duplikat potvrđivanjem broj **RequestId** i **postupak** .

### <a name="log-naming-conventions"></a>Prijavite se konvencije imenovanja
Svaki zapisnika će biti zapisane u sljedećem obliku.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

U sljedećoj tablici opisane svaki atribut u nazivu zapisnika.

| Atribut         | Opis                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < naziv usluge >    | Naziv servis za pohranu. Na primjer: blob, tablicu ili red.                                                                                                                          |
| GGGG              | Četveroznamenkastom godinom zapisnika. Na primjer: 2011.                                                                                                                                           |
| MM                | Mjesec dvoznamenkasti za zapisnik. Na primjer: 07.                                                                                                                                             |
| DD                | Mjesec dvoznamenkasti za zapisnik. Na primjer: 07.                                                                                                                                             |
| HH                | Dvoznamenkasti sat koji pokazuje početnu sat zapisnici u 24-satnom obliku UTC-a. Na primjer: 18.                                                                                     |
| mm                | Dvoznamenkasti broj koji označava početni minute za zapisnike. Ta vrijednost je podržana u trenutnu verziju prostora za pohranu analize i njegovom vrijednošću uvijek biti 00.     |
| <counter>         | Polje brojač s šest znamenki koji označava broj zapisnika blob-ova za servis za pohranu generira u jedan sat vremensko razdoblje. Brojač počinje 000000. Na primjer: 000001.     |

Slijedi naziv zapisnika gotovi ogledni koji kombinira prijašnjih primjera.

    blob/2011/07/31/1800/000001.log

Slijedi uzorka URI koje je moguće koristiti za pristup prethodne zapisnika.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Kad se zahtjeva za pohranu, rezultirajući naziv zapisnika korelaciji sat dovršetka tražene operacije. Na primjer, ako je zahtjev za GetBlob dovršen na 6:30 poslije podne. na 7/31/2011 zapisnik će biti napisani s prefiksom sljedeće:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metapodataka zapisnika
Sve zapisnika blob-ova spremaju se s metapodacima koje je moguće koristiti za prepoznavanje zapisivanje podataka koje sadrži blob-om. U sljedećoj tablici opisane svaki atribut metapodataka.

| Atribut     | Opis                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | U članku se opisuje li zapisnik sadrži podatke koji su vezani uz za čitanje, pisanje ili brisanje operacije. Tu vrijednost možete uključiti jednu vrstu ili kombinaciju sva tri odvojenih točkama sa zarezom. Primjer 1: pisanje; Primjer 2: čitanje, pisanje; Primjer 3: čitanje, pisanje, brisanje.    |
| StartTime     | Najraniji vrijeme unosa u zapisniku u obliku gggg-MM-DDThh:mm:ssZ. Na primjer: 2011-07-31T18:21:46Z.                                                                                                                                             |
| EndTime       | Najkasnije unosa u zapisniku u obliku gggg-MM-DDThh:mm:ssZ. Na primjer: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | Verzija oblika zapisnika. Trenutno je jedini podržani vrijednost 1.0.                                                                                                                                                                                     |

Sljedeći popis prikazuje gotovi ogledni metapodataka pomoću prijašnjih primjera.

- LogType = pisanja

- StartTime = 2011-07 – 31T18:21:46Z

- EndTime = 2011-07 – 31T18:22:09Z

- LogVersion = 1.0

### <a name="accessing-logging-data"></a>Pristup podacima zapisivanje

Svi podaci u `$logs` spremnik mogu pristupiti putem servisa za Blob API-ji, uključujući .NET API-ji nudi na Azure upravlja biblioteke. Administrator računa za pohranu možete pročitati i izbrisati zapisnike, ali ne možete stvoriti ili ih ažurirati. U zapisniku metapodataka i naziv zapisnika može se koristiti kada slanje upita za zapisnik. Moguće je da navedeni sat zapisnika da se pojavi izvan redoslijeda, ali metapodatke uvijek određuje vremenski raspon zapisnika stavki u zapisnik. Zbog toga možete koristiti kombinaciju imena zapisnika i metapodacima prilikom traženja određeni zapisnika.

## <a name="about-storage-analytics-metrics"></a>O Analytics za pohranu metrika

Prostor za pohranu analize možete spremiti metriku koji sadrže Zbrojeno transakcije Statistika i kapacitet podatke o zahtjevima za servis za pohranu. Transakcije prijavljene i na razini API operacija, kao i na razini servisa za pohranu i kapacitet prijavljuje na razini servisa za pohranu. Metriku podataka može se koristiti za analizu korištenja servisa za pohranu dijagnosticiranje problema s zahtjevima za u odnosu na servis za pohranu i da biste poboljšali performanse aplikacije koje koriste servisa.

Da biste koristili Analytics za pohranu, morate omogućiti pojedinačno za svaku uslugu koju želite nadzirati. Možete je omogućiti s [Portala za Azure](https://portal.azure.com). Detalje potražite u članku [monitora na račun za pohranu na portalu za Azure](storage-monitor-storage-account.md). Možete omogućiti i analize prostora za pohranu programski putem REST API-JA ili u biblioteku klijenta. Pomoću **Svojstva servisa za početak** operacije da biste omogućili Analytics za pohranu za svaki servis.

### <a name="transaction-metrics"></a>Transakcije mjerenja

Robusni skup podataka naveden u vremenskim razmacima svaki sat ili minute za svaki servis za pohranu i zatražili API operacija, uključujući ingress/izlazne, dostupnost, pogreške, i kategorizirani postotaka zahtjev. Vidjet ćete popis svih pojedinosti transakcije u temi [Za pohranu analize metriku tablice sheme](https://msdn.microsoft.com/library/hh343264.aspx) .

Transakcije podataka naveden na dvije razine – razina usluge i razine operacija API-JA. Na razini usluge statistiku sažimanje svih zatražio API operacije zapisuju u tablici entitet svaki sat čak i ako se zahtjevi ne su izvršene u servisu. Operacija razini API Statistika samo zapisuju u entitet operacija zatražio je u tom sat.

Na primjer, ako izvođenje operacije **GetBlob** o vašoj usluzi Blob, metrike pohrane analize će zahtjev za prijavu i uključiti u skupne podatke i usluge Blob kao i **GetBlob** postupak. Međutim, ako se nijedna operacija **GetBlob** zatražili tijekom sat, entitet će nije moguće upisati u `$MetricsTransactionsBlob` za taj postupak.

Metriku transakcije se snimaju za zahtjeve za korisnika i zahtjevi za koje je načinio Analytics za pohranu sam. Na primjer, se bilježe zahtjeve za pohranu Analitika pisati zapisnicima i entiteti tablice. Dodatne informacije o kako se naplaćuju te zahtjeve, potražite u članku [Analytics za pohranu i naplati](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Kapaciteta mjerenja

>[AZURE.NOTE] Trenutno kapaciteta metriku dostupne su samo za servis Blob. Kapaciteta metriku u tablicu i servis reda čekanja bit će dostupni u budućim verzijama Analytics za pohranu.

Svakodnevno naveden kapaciteta podataka računa spremišta blobova platforme servisa i dvije tablice entiteti zapisuju. Jedan entitet pruža Statistika za podatke o korisniku, a drugi pruža statistiku o na `$logs` blob spremnik koristi Analytics za pohranu. Na `$MetricsCapacityBlob` tablica sadrži statistike sljedeće:

- **Kapacitet**: količinu prostora za pohranu koriste računa spremišta blobova platforme servisa u bajtova.

- **ContainerCount**: broj blob spremnika u servisu računa spremišta blobova platforme.

- **ObjectCount**: broj zajamčenoj i nepotvrđenim blok ili stranice blob-ova u servisu računa spremišta blobova platforme.

Dodatne informacije o metriku kapaciteta potražite u članku [Prostora za pohranu analize metriku tablice shemu](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Kako se pohranjuju mjerenja

Svi podaci metriku za sve servise za pohranu pohranjena u tri tablice rezervirana za tu uslugu: jednu tablicu transakcije informacije, jednu tablicu da biste saznali minute transakcije i kapacitet podatke u drugoj tablici. Transakcije i minute transakcije informacije sastoji se od podataka zahtjeva i odgovora, a informacije kapaciteta sastoji se od prostora za pohranu podataka o korištenju. Metriku sat, minuta metriku i kapacitet računa spremišta blobova platforme servisa možete pristupiti u tablicama na koje se nazivaju kao što je opisano u sljedećoj tablici.

| Razina mjerenja                         | Nazivi tablica                                                                                                                   | Podržane verzije                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Svaki sat metrike primarno mjesto      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | Verzije prije 2013-08-15 samo. Dok je i dalje podržane su sljedeće nazive, preporučuje se li prijeći na korištenje tablica navedena u nastavku.  |
| Svaki sat metrike primarno mjesto      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Sve verzije, uključujući 2013-08-15.                                                                                                               |
| Minute metrike primarno mjesto      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Sve verzije, uključujući 2013-08-15.                                                                                                               |
| Svaki sat metrike sekundarne mjesto    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Sve verzije, uključujući 2013-08-15. Mora biti omogućen pristup za čitanje zemlj suvišnih replikacije.                                                    |
| Minute metrike sekundarne mjesto    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Sve verzije, uključujući 2013-08-15. Mora biti omogućen pristup za čitanje zemlj suvišnih replikacije.                                                    |
| Kapacitet (samo u servis Blob)          | $MetricsCapacityBlob                                                                                                          | Sve verzije, uključujući 2013-08-15.                                                                                                               |


U ovim su tablicama stvaraju automatski kada Analytics za pohranu je omogućen za račun za pohranu. Im je moguće pristupiti putem prostora za naziv računa za pohranu, na primjer:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Pristup podacima mjerenja

Sve podatke u tablicama metriku mogu pristupiti putem servisa za tablicu API-ji, uključujući .NET API-ji nudi na Azure upravlja biblioteke. Administrator računa za pohranu možete čitati i brisanje entiteti tablice, ali ne možete stvoriti ili ih ažurirati.

## <a name="billing-for-storage-analytics"></a>Naplata za pohranu Analytics

Prostor za pohranu analize omogućena je prema vlasnika računa za pohranu; nije omogućeno po zadanom. Svi podaci metriku zapisuje u servisima za pohranu računa. Zbog toga svaki postupak pisanja obavlja Analytics za pohranu je naplatu. Uz to, količinu prostora za pohranu koriste metriku podataka je i naplatu.

Sljedeće radnje obavlja Analytics za pohranu su naplatu:

- Zahtjevi za stvaranje blob-ova za prijavu. 

- Zahtjevi za stvaranje entiteti tablice za određene parametre.

Ako ste konfigurirali pravilo zadržavanja podataka, koje se neće naplatiti za brisanje transakcije kada Analytics za pohranu izbriše stare zapisivanje i metrike podatke. Međutim, brisanje transakcije od klijenta su naplatu. Dodatne informacije o pravilnicima zadržavanja potražite u članku [Postavljanje pravila zadržavanja za pohranu analize podataka](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Objašnjenje zahtjeva za naplatu

Svaki zahtjev za servis za pohranu računa ili se produljuje naplatu koje nisu naplatu. Prostor za pohranu analize evidentira svake pojedine zahtjev na neki servis, uključujući poruka o stanju koji pokazuje kako je rukovanja zahtjeva. Isto tako, analitičkih podataka za pohranu pohranjuje metriku za uslugu te operacije API tog servisa, uključujući postotaka i ukupan broj određene poruke o statusu. Zajednički te značajke omogućuju analizirati zahtjeva za naplatu, poboljšati aplikacije i dijagnosticiranje problema sa zahtjevima za servisa. Dodatne informacije o naplati potražite u članku [Objašnjenje Azure prostora za pohranu za naplatu – propusnosti, transakcije, i kapacitet](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Kada pogledate prostora za pohranu analize podataka, pomoću tablice u temi [prijavljeni operacije Analytics za pohranu i poruke o statusu](https://msdn.microsoft.com/library/azure/hh343260.aspx) da bi se utvrdilo koji zahtjeva za naplatu. Zatim možete usporediti zapisnicima i metriku podataka poruke o statusu da biste vidjeli ako je naplaćeno određenog zahtjeva. Da biste istražili dostupnosti za servis za pohranu ili pojedinačne API postupak možete koristiti i tablice u na prethodnu temu.

## <a name="next-steps"></a>Daljnji koraci

### <a name="setting-up-storage-analytics"></a>Postavljanje Analytics za pohranu
- [Praćenje računa za pohranu na portalu za Azure](storage-monitor-storage-account.md)
- [Omogućavanje i konfiguriranje Analytics za pohranu](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Zapisivanje Analytics za pohranu  
- [O zapisivanje Analytics za pohranu](https://msdn.microsoft.com/library/hh343262.aspx)
- [Oblik zapisnika Analytics](https://msdn.microsoft.com/library/hh343259.aspx)
- [Prostor za pohranu analize prijavljeni operacije i poruke o statusu](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Metrike pohrane Analytics
- [O metrike pohrane Analytics](https://msdn.microsoft.com/library/hh343258.aspx)
- [Prostor za pohranu analize metriku tablice sheme](https://msdn.microsoft.com/library/hh343264.aspx)
- [Prostor za pohranu analize prijavljen operacije i poruke o statusu](https://msdn.microsoft.com/library/hh343260.aspx)  
