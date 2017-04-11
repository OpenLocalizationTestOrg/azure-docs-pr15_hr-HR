<properties
    pageTitle="Skaliranje strujanje analize zadataka da biste povećali propusnost | Microsoft Azure"
    description="Saznajte kako skaliranje strujanje analize poslove konfiguriranje unos particije, usklađivanje definicije upita i postavljanje posao strujanje jedinice."
    keywords="Podaci strujanje, strujanje obradu podataka ugađanje analytics"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Promjena veličine Azure strujanje Analitika poslova da biste povećali propusnost strujanje obrada podataka

Saznajte kako ugađanje analize zadacima i izračunati *strujanje jedinice* za strujanje analize, upute za promjenu veličine strujanje analize poslove konfiguriranje unos particije, usklađivanje definicije upita analize i postavljanje posao strujanje jedinice. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Što su dijelove posao analize strujanje?
Definiciju posla strujanje Analytics uključuje unosa, upita i izlaz. Unosa su na kojem piše posao toka podataka, upit se koristi za pretvaranje toka unos podataka, a izlaz je gdje posao šalje rezultata na posao.  

Posao potreban je barem jedan unos izvor podataka tok. Strujanje unos izvora podataka mogu se spremiti u događaj središte Azure servisa Bus ili u spremište blobova platforme Azure. Dodatne informacije potražite u članku [Uvod u Azure strujanje analize](stream-analytics-introduction.md) i [počnite s korištenjem Azure strujanje analize](stream-analytics-get-started.md).

## <a name="configuring-streaming-units"></a>Konfiguriranje strujanje jedinice
Strujanje jedinicama (SUs) predstavljaju resurse i računalnih power potrebne za izvršavanje programa Azure strujanje analize posla. SUs omogućuje opišite relativni događaj obrade kapaciteta stopljena mjera procesora, memorije, na temelju i čitanje i pisanje stope. Svaka strujanje jedinica odgovara otprilike 1MB/sekunda propusnost. 

Odabir koliko SUs su potrebni za određeni posao ovisi o konfiguraciji particije ulaza i upit definiran za posao. Možete odabrati do kvotu u streaming pomoću portala za klasični Azure jedinice za posao. Azure pretplate prema zadanim postavkama ima ograničenja od najviše 50 strujanje jedinica za sve zadatke analize u određenoj regiji. Da biste povećali strujanje jedinice za pretplate, obratite se [Microsoftovoj podršci](http://support.microsoft.com).

Broj strujanje jedinica koje se mogu koristiti za posao ovisi o konfiguraciji particije ulaza i upit definiran za posao. Imajte na umu i valjana vrijednost za strujanje jedinice moraju se koristiti. Valjane vrijednosti Počni od 1, 3, 6, a zatim prema gore u koracima od 6, kao što je prikazano u nastavku.

![Azure strujanje analize strujanje jedinice skala][img.stream.analytics.streaming.units.scale]

U ovom se članku će vam pokazati kako izračun i ugađanje upita da biste povećali propusnost za poslova analize.

## <a name="embarrassingly-parallel-job"></a>Embarrassingly paralelno posla
Embarrassingly paralelno zadatak je najčešće skalabilni scenarij imamo Azure strujanje analize. Jedna particija ulaza u jednu instancu upit se povezuje jedna particija izlaz. Postizanje ovaj parallelism zahtijeva nekoliko stvari:

1.  Ako logiku upita ovisi o istim ključem obrađuje u istoj instanci upita, zatim morate osigurati događaje na istom particija unos. Slučaju koncentratorima događaj, to znači da podaci o događaju mora imati **PartitionKey** postavljanje ili koristite particioniranom pošiljatelja. Za Blob, to znači da događaje šalju se u istu mapu particije. Logiku upita moraju biti iste ključ obradili istoj instanci upita, možete zanemariti taj zahtjev. Primjer to bi jednostavnog upita odaberite / / filtar projekta.  
2.  Kada podatke podsjeća kao što je potrebno na strani unos, moramo da biste bili sigurni da upit particije. Potreban je koristi **Particije** u sve korake. Dopušteno više korake, ali svi oni morate particije iste tipke. Drugi napomenuti kako je da trenutno tipku za stvaranje particija mora biti postavljena na **PartitionId** da bi se potpuno paralelno posao.  
3.  Samo koncentratora za događaj i Blob podržavamo particioniranom izlaz. Za izlaz koncentratorima događaj, morate konfigurirati polje **PartitionKey** bude **PartitionId**. Za Blob, ne morate ništa učiniti.  
4.  Druge stvari koje treba Imajte na umu broj unosa particija mora biti jednak broj particije izlaz. Izlaz blob trenutno ne podržava particije, no to nije važno jer se ona će naslijediti aktiviranja sheme upstream upita. Primjeri particija vrijednosti koje želite dopustiti potpuno paralelno posao:  
    1.  8 događaj koncentratora za unos particije i 8 događaj koncentratora izlaz particije
    2.  8 događaj koncentratora za unos particije i Blob Izlaz  
    3.  8 blob unos particije i Blob Izlaz  
    4.  8 blob unos particije i 8 događaj koncentratorima izlaz particije  

Evo nekih primjera scenarija koji su embarrassingly paralelno.

### <a name="simple-query"></a>Jednostavan upit
Unos – događaj koncentratora s 8 particije izlaz – događaj koncentrator s 8 particije

**Upit:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Ovaj upit je Jednostavni filtar i kao takve, smo ne morate brinuti o particija unos mi šaljemo s koncentratorima događaja. Primijetit ćete da se upit ima **Particije tako da** od **PartitionId**pa ćemo ispunjavanje preduvjeta #2 od prethodnog. U rezultatu, moramo Konfiguriranje izlazne događaj koncentratorima u posao tako da ima polje **PartitionKey** postavljeno na **PartitionId**. Jedan posljednje provjere, unos particije == particije izlaz. U ovom Topologija nije embarrassingly paralelno.

### <a name="query-with-grouping-key"></a>Upit s ključem grupiranja
Unos – događaj koncentratora s 8 particije izlaz – Blob

**Upit:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Ovaj upit sadrži ključ grupiranja i kao takve, isti ključa mora biti obradili istoj instanci upita. To znači da moramo poslati naš događaje događaje koncentratorima particioniranom način. Tipku ne možemo zanimaju? **PartitionId** pojam za logiku posla, tipku realni smo zanimaju je **TollBoothId**. To znači da ne možemo postavite **PartitionKey** podataka događaja mi šaljemo s koncentratorima događaja biti **TollBoothId** događaja. Upit sadrži **Particije tako da** od **PartitionId**, pa ne možemo postoje dobro. U rezultatu, budući da je Blob, smo ne morate brinuti o konfiguriranju **PartitionKey**. Preduvjet #4, ponovno je to Blob, tako da mi ne morate brinuti o tome. U ovom Topologija nije embarrassingly paralelno.

### <a name="multi-step-query-with-grouping-key"></a>Višestruki korak upita pomoću ključa grupiranja ###
Unos – događaj koncentrator pomoću 8 particije izlaz – događaj koncentrator s 8 particije

**Upit:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Ovaj upit sadrži ključ grupiranja i kao takve, isti ključa mora biti obradili istoj instanci upita. Možete koristiti isti strategije kao prethodne upita. Upit sadrži više korake. Svaki korak mora li **Particija tako da** od **PartitionId**? Da, pa ne možemo su dobar. U rezultatu, moramo postavite **PartitionKey** **PartitionId** kao što su navedene iznad i smo također možete vidjeti ima isti broj particije kao unos. U ovom Topologija nije embarrassingly paralelno.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Ogledni Scenariji koji nisu embarrassingly paralelno

### <a name="mismatched-partition-count"></a>Count nepodudaran Partition ###
Unos – događaj koncentratora s 8 particije izlaz – događaj koncentrator s 32 particije

Nije važno što se upit u tom slučaju jer unos particija count! = count particija izlaz.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Ne koristite koncentratora događaj ili blob polja kao rezultat
Unos – događaj koncentratora s 8 particije izlaz – PowerBI

Izlaz PowerBI trenutno ne podržava particija.

### <a name="multi-step-query-with-different-partition-by-values"></a>Višestruki korak upita imaju različite vrijednosti za particija po
Unos – događaj koncentrator s 8 particije izlaz – događaj koncentrator s 8 particije

**Upit:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Kao što vidite, u drugom koraku koristi **TollBoothId** kao stvaranje particija ključ. To nije u prvom koraku i stoga ćete morati nam da biste učinili u Miješanje. 

Ovo su neki primjeri i counterexamples strujanje analize zadataka koji će moći postigli embarrassingly paralelno topologije i s njom mogućnost maksimalno skaliranje. Za zadatke koje ne pristaju ni jednu od ove profile buduća ažuriranja dovršenog kako maximally skaliranje su druge Kanonski strujanje analize slučajevi bit će.

Za neka sada korištenje opće smjernice ispod:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Izračun najviše strujanje jedinice posla
Ukupan broj strujanje jedinica koje je moguće koristiti strujanje analize posla ovisi o broju koraka u upit definiran za posao i broj particije svakog koraka.

### <a name="steps-in-a-query"></a>Koraci u upitu
Upit može imati jedan ili više korake. Svaki korak je podupit definirane pomoću ključnih riječi **s** . Samo upit koji je izvan ključnih riječi **s** i broje se kao korak, na primjer, **Odaberite** naredbu u sljedeći upit:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Prethodni upit sadrži dva koraka.

> [AZURE.NOTE] Ovaj primjer upita bit će objašnjena u nastavku članka.

### <a name="partition-a-step"></a>Particija korak

Particija korak potrebno ispuniti sljedeće uvjete:

- Ulazni izvor mora biti particije. Dodatne informacije potražite u [Vodiču za programiranje događaja koncentratora](../event-hubs/event-hubs-programming-guide.md).
- Naredbe **SELECT** upita moraju pročitati iz particioniranom ulazni izvor.
- Upit u koraku mora imati **Particija po** ključnoj riječi

Kada je upit particije, unos događaje neće biti obrađeni i zbroja u zasebnom particija grupe i događaje izlaze generiraju za svaku od grupa. Ako je poželjno kombinirane zbrajanja, morate stvoriti drugi korak koji nisu particije zbrajati.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Izračun max strujanje jedinice za posao

Svi koraci koje nisu particije zajedno mogu mijenjati veličinu do šest strujanje jedinice za posao strujanje analize. Da biste dodali dodatne strujanje jedinice, korak mora biti particije. Svaki particija može imati šest strujanje jedinice.

<table border="1">
<tr><th>Upit za posao</th><th>Max strujanje jedinice za posao</th></td>

<tr><td>
<ul>
<li>Upit sadrži jedan korak.</li>
<li>Korak ne particije.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Strujanje ulaznih podataka ima particije s 3.</li>
<li>Upit sadrži jedan korak.</li>
<li>U koraku ima particije.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Upit sadrži dva koraka.</li>
<li>Niti jedno od koraka particije.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Unos strujanja podataka ima particije s 3.</li>
<li>Upit sadrži dva koraka. Unos korak je particije i drugi korak nije.</li>
<li>Naredba SELECT čita iz particioniranom unos.</li>
</ul>
</td>
<td>24 (18 particioniranom upute) + 6 korake koje nisu particije</td></tr>
</table>

### <a name="examples-of-scale"></a>Primjeri skala
Sljedeći upit izračunava broj automobilima prolaze kroz stanice broj s tri tollbooths unutar prozora tri minute. Ovaj upit mogu biti skalirana najviše šest strujanje jedinicama.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Da biste koristili više strujanje jedinice za upit, oba podatke strujanje unos, a zatim upit mora biti particije. Given da particija strujanja podataka postavljena na 3, sljedeći upit Izmijenjeno je možete skalirana do 18 strujanje jedinice:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Kada je upit particije, unos događaja se obrađuju i pridružuje u zasebnom particija grupe. Izlaz događaja i generiraju se za sve grupe. Particija može uzrokovati nekoliko neočekivane rezultate kada je polje **Group by** nije ključ particije u strujanje unosa podataka. Ako, na primjer, polje **TollBoothId** u prethodni primjer upita nije particija ključ od Input1. Podaci iz TollBooth #1 moguće je šire u više particija.

Svaki od particije Input1 obradit će se zasebno po strujanje analize, a stvorit će se više zapisa broj automobila-računanja-kroz za istu tollbooth u istom prozoru tumbling. Ako tipku unos particija nije moguća, problem moguće popraviti dodavanjem koji nisu particija korake, na primjer:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Ovaj upit mogu biti skalirana na 24 strujanje jedinice.

>[AZURE.NOTE] Ako se pridružuju dva strujanja, provjerite je li strujanja imaju tipke particija stupca učinite spojeva i imati isti broj razdjeljivanja u oba.


## <a name="configure-stream-analytics-job-partition"></a>Konfiguriranje particije posao strujanje analize

**Da biste prilagodili strujanje jedinica za posao**

1. Prijavite se na [portal za upravljanje](https://manage.windowsazure.com).
2. U lijevom oknu kliknite **Analitika strujanje** .
3. Pritisnite strujanje analize zadatak koji želite skaliranja.
4. Kliknite **MJERILO** pri vrhu stranice.

![Azure strujanje analize strujanje jedinice skala][img.stream.analytics.streaming.units.scale]

Na portalu za Azure postavke mjerila možete pristupiti u odjeljku postavke:

![Azure konfiguracije Portal strujanje Analytics][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Performanse posao monitora

Pomoću portala za upravljanje, možete pratiti propusnost posao u događaje/sekundi:

![Azure strujanje analize praćenje zadataka][img.stream.analytics.monitor.job]

Izračunati očekivani propusnost radno opterećenje u događaje/sekundi. Ako je propusnost manja od očekivanog, ugađanje unos particija, ugađanje upita i dodavanje dodatnih strujanje jedinica za vaš posao.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Strujanje analize propusnost na razini - Raspberry Pi scenarija


Da biste razumjeli kako strujanje analize poslove promijenite veličinu u uobičajeni scenarij pomoću obrada propusnost preko više strujanje jedinica, Evo pokusa koji šalje senzor podatke (klijenti) u događaj koncentrator, obrađuje je i drugi događaj koncentrator kao na Izlaz šalje upozorenja ili statističke podatke.

Klijent šalje sintetizirani senzor podataka s koncentratorima događaja u obliku JSON strujanje analize i izlaz podataka ujedno JSON OSNOVNI oblik.  Evo kako bi izgledao oglednih podataka poput –  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Upit: stavke "slanje upozorenja kada je isključen osnovnu"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Mjerenje propusnost: Propusnost u ovom kontekstu je iznos ulaznih podataka obradili strujanje analize u određenu količinu vremena (10 minuta). Da biste postigli najbolje obrada propusnost za unos podataka, obje podatke strujanje unos, a zatim upit mora biti particije. Također **COUNT()**uvrštava u upit za mjerenje su obrađeni koliko unos događaja. Da biste osigurali posao nije jednostavno čekanja za unos događaja koji se isporučuju, svaki particija središtu unos događaj je unaprijed učitani s dovoljno ulaznih podataka (oko 300MB).  

U nastavku su rezultati povećanim broj jedinica strujeće i odgovarajuće particije broji u događaj koncentratorima.  

<table border="1">
<tr><th>Unos particije</th><th>Izlaz particije</th><th>Strujanje jedinice</th><th>Osigurale trajne propusnost
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB s</td>
</tr>
</table>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Zatražite pomoć
Za dodatnu pomoć, pokušajte našem [forumu Azure strujanje analize](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
