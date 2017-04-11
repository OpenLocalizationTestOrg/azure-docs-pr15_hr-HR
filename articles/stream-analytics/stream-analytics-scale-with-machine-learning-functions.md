<properties
    pageTitle="Skaliranje posla strujanje analize pomoću funkcija Azure strojnog učenja | Microsoft Azure"
    description="Naučite ispravno skaliranje poslove strujanje analize (particija, SU količine i više) prilikom korištenja funkcije Azure strojnog učenja."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Promjena veličine posla strujanje analize pomoću funkcija Azure strojnog učenja

Često je vrlo lako postaviti za strujanje analize i pokretanje ogledne podatke kroz nju. Što treba smo? kada moramo isti Pokreni veće jedinice podataka Potrebna je nam da biste razumjeli kako konfigurirati posao Analitika strujanje tako da ga će promijeniti veličinu. U ovom dokumentu, ne možemo sadrži posebne aspekte skaliranje strujanje analize poslove s funkcijama strojnog učenja. Informacije o tome kako Općenito skaliranja strujanje Analitika poslova potražite u članku [Promjena veličine poslova](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Što je funkciji Azure strojnog učenja u analize strujanje?

Funkcija strojnog učenja u strujanje analize može se koristiti kao što su uobičajeni funkcija poziv u jezik upita strujanje analize. Međutim, iza scenu, funkcija pozive su zapravo zahtjeva za Azure strojnog učenja web-servisa. Web-servisi za strojno učenje podržava "grupnog slanja promjena" više redaka, koja se naziva i kliknite Spremi grupe u istom web servisa API poziv, da biste poboljšali cjelokupan propusnost. Potražite u sljedećim člancima dodatne informacije [Funkcija Azure strojnog učenja u strujanje analize](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) i [Azure strojnog učenja Web Services](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Konfiguriranje posao strujanje analize pomoću funkcija strojnog učenja

Prilikom konfiguriranja funkcija strojnog učenja za strujanje analize posla, postoje dva parametra treba uzeti u obzir veličine obradu funkcija pozive strojnog učenja i strujanje jedinice (SUs) dodjeli za posao strujanje analize. Da biste odredili odgovarajuće vrijednosti za to, najprije odluka mora izvršiti između Latencija i propusnost, odnosno Latencija posao strujanje analize i propusnost svaki SU. SUs uvijek može dodati zadatak da biste povećali propusnost i particioniranom strujanje analize upita, iako dodatne SUs povećava trošak pokretanja posla.

Stoga je važno da biste odredili *odstupanje* Latencija radi analize toka posla. Dodatni Latencija pokretanje zahtjevima za uslugu Azure strojnog učenja prirodan povećava se s veličinom grupe koji će zbirni Latencija posao strujanje analize. S druge strane, povećati veličinu serije omogućuje zadatak strujanje Analytics za obradu *više događaji s na *isti broj * zahtjeva za servis strojnog učenja web. Često povećava latencije za servis strojnog učenja web je podgrupama linearni povećava veličinu serije pa Imajte na umu trošak-najučinkovitiji veličinu serije za web-servis strojnog učenja u bilo kojem navedeni situaciji. Zadana veličina grupe za zahtjeve za uslugu web 1000 i može se mijenjati korištenjem [Strujanje analize REST API -JA](https://msdn.microsoft.com/library/mt653706.aspx "Strujanje analize REST API -JA") ili [PowerShell klijent za strujanje analize](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell klijent za strujanje analize").

Kada je određeno veličinu serije količinu strujanje jedinice (SUs) mogu se odrediti, ovisno o broju događaji koje je potrebno funkciju procesa sekundi. Dodatne informacije o strujanje jedinicama pogledajte u članku [Skaliranje strujanje Analitika poslova](stream-analytics-scale-jobs.md#configuring-streaming-units).

Općenito govoreći, postoje 20 Istodobni veze web-uslugu strojnog učenja za svaku 6 SUs, osim što 1 SU zadacima i 3 SU poslovi će dobiti 20 Istodobni veze i.  Na primjer, ako je stopa ulaznih podataka 200,000 događaje u sekundi i veličinu serije ostaje na zadane postavke 1000 rezultirajućem Latencija servisa web pomoću 1000 događaje minijaturni obrade je 200ms. To znači da svaku vezu možete unijeti 5 zahtjevi za web-servis strojnog učenja u drugi. S 20 vezama strujanje analize posao možete obraditi 20 000 događaji u 200ms i stoga 100 000 događaji u drugi. Radi obrade 200,000 događaja u sekundi, posao strujanje analize potrebno 40 Istodobni veze, koja se nalazi za 12 SUs. Dijagramu u nastavku prikazuje zahtjeve iz posla strujanje analize krajnja točka servisa web strojnog učenja – svakoj SUs 6 na max ima 20 Istodobni veze s web-servis strojnog učenja.

![Promjena veličine strujanje analize s primjer posao strojnog učenja funkcije 2] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Promjena veličine strujanje analize s primjer posao strojnog učenja funkcije 2")

Općenito govoreći, ***B*** za obradu veličinu, ***L*** Latencija servisa web obradu veličini B u milisekundama propusnost za strujanje analize zadatak s ***N*** SUs je:

![Promjena veličine analize strujanje s strojnog učenja formule funkcije] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Promjena veličine analize strujanje s strojnog učenja formule funkcije")

Dodatni razmotriti možda 'Maks Istodobni pozivi' na strani servis strojnog učenja web, preporučuje se postaviti za maksimalnu vrijednost (trenutno 200).

Dodatne informacije o Ova postavka Molimo pročitajte [članak skaliranje za stroj Upoznavanje web-usluge](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Primjer – šalju analizu

Sljedeći primjer uključuje strujanje analize zadatak s šalju analiza funkcije strojnog učenja kao što je opisano u [Vodič za integraciju strujanje analize strojnog učenja](stream-analytics-machine-learning-integration-tutorial.md).

Jednostavan upit u potpunosti particioniranom slijedi funkciju **šalju** se upit kao što je prikazano u nastavku:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Zamislite sljedeće; s propusnost od 10 000 tweets sekundi posao strujanje analize moraju se stvoriti za analizu šalju od tweets (događaja). Korištenje 1 SU nije posao strujanje analize moći promet? Pomoću zadane veličine obradu 1000 posao trebali biste moći pratiti unos. Daljnje funkciju dodane strojnog učenja treba generirati više od druge latencije koji je opće Latencija zadanom analize šalju strojnog učenja web-servisa (veličine zadane grupe 1000). Zadatak analize strujanje **cjelokupan** ili Završi do kraja Latencija obično bi nekoliko sekundi. Pogledajte detaljnije u strujanje analize posao, *osobito* strojnog učenja funkcija pozive. Veličinu serije pojavljuju kao 1000, propusnost 10 000 događaja će otprilike 10 zahtjevi za web-servisa. Čak i uz 1 SU nema dovoljno Istodobni veze da biste omogućili ovu unos promet.

Ali ako stopa ulazni događaj povećava 100 x i sada posao strujanje analize treba obraditi 1,000,000 tweets sekundi? Postoje dvije mogućnosti:

1.  Povećavanje veličine grupe ili
2.  Particija ulaznog toka radi obrade događaja paralelno

S mogućnošću prvi zadatak **Latencija** će se povećati.

S mogućnošću drugi više SUs morate biti dodjeli i stoga generiranje više istovremeni zahtjevi za učenje stroj web servisa. To znači da će se povećati posla **trošak** .


Pretpostavimo kašnjenje analize šalju strojnog učenja web-servisa je 200ms za serije 1000 događaj ili ispod, 250ms za 5000 događaja grupe i 300ms za serije 10 000 događaj ili 500ms za serije 25.000 događaj.

1. Pomoću prvu mogućnost, (**ne** dodjele resursa više SUs), veličinu serije može biti povećan je da bi **25.000**. To shodno omogućuje zadatak za obradu 1,000,000 događaje s 20 Istodobni vezama na web-servisa za strojno učenje (s Latencija 500ms po poziva). Pa dodatne Latencija posla strujanje analize zbog zahtjeva za funkciju šalju protiv zahtjeva za servis strojnog učenja web želite povećati iz **200ms** za **500ms**. No imajte na umu da skupna veličina **ne može** biti poboljšani neizmjerno web-servisi strojnog učenja potrebno veličina tereta zahtjeva biti 4 MB ili manje web-servisa zahtjeve vremenskog ograničenja nakon 100 sekundi operacije.
2. Koristite za drugu mogućnosti, veličinu serije lijevo na 1000 s 200ms web servisa Latencija, svakih 20 Istodobni veze na web-uslugu moći ćete obrade 1000 *20* 5 događaja = 100,000 sekundi. Tako da obradi 1,000,000 događaje u sekundi posao potrebni 60 SUs. U usporedbi s prvu mogućnost analize toka posla bi više web-servisa obrade zahtjeva, shodno generiranje povećana trošak.

U nastavku je tablica za propusnost posao strujanje Analytics za različite SUs i veličine grupe (u broj događaja u sekundi).

| veličinu serije (Latencija ML)  | 500 (200ms) | 1000 (200ms) | 5000 (250ms) | 10 000 (300ms) | 25.000 (500ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SU** | 2,500 | 5000 | 20 000 | 30.000 | 50.000 |
| **3 SUs** | 2,500 | 5000 | 20 000 | 30.000 | 50.000 |
| **6 SUs** | 2,500 | 5000 | 20 000 | 30.000 | 50.000 |
| **12 SUs** | 5000 | 10 000 | 40.000 | 60.000 | 100,000 |
| **18 SUs** | 7,500 | 15.000 | 60.000 | 90,000 | 150.000 |
| **24 SUs** | 10 000 | 20 000 | 80,000 | 120,000 | 200.000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25.000 | 50.000 | 200.000 | 300.000 | 500 000 |

Sada, već imat ćete dobar objašnjenje funkcioniranja funkcije strojnog učenja u strujanje analize. Vjerojatno i znate poslove strujanje analize "izvlačenje" podataka iz izvora podataka i svaki "istaknuti" vraća niz događaja za zadatak strujanje Analytics za obradu. Kako ovo utjecaj istaknuti modela strojnog učenja web zahtjevima za uslugu?

U normalnim veličinu serije ne možemo postaviti za funkcije strojnog učenja točno neće djeljiv broj događaja vratio svaki zadatak strujanje analize "istaknuti". Kada se to dogodi web-servisa za strojno učenje će se pozivati s serije "djelomično". To možete učiniti da biste ne plaćati dodatni posao indirektni kašnjenje u coalescing događaje iz povlačite da biste istaknuti.

## <a name="new-function-related-monitoring-metrics"></a>Nove funkcije vezane nadzora metrike

U području Monitor posao strujanje analize tri dodatne vezane uz funkcija metriku dodane. Su ZAHTJEVI za (funkcija), FUNKCIJA događaja i FUNKCIJA ZAHTJEVA, kao što je prikazano u nastavku grafici.

![Promjena veličine analize strujanje s strojnog učenja funkcije mjerenja] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Promjena veličine analize strujanje s strojnog učenja funkcije mjerenja")

Su definirana na sljedeći način:

**FUNKCIJA ZAHTJEVA**: broj zahtjeva (opis funkcije).

**FUNKCIJA događaja**: broj događaja u zahtjevima za funkciju.

**FUNKCIJA ZAHTJEVA**: broj zahtjeva nije uspjelo (opis funkcije).

## <a name="key-takeaways"></a>Ključni Takeaways  

Da biste skalirali programa posao strujanje analize pomoću funkcija strojnog učenja sažetak glavnih točaka, morate smatra sljedećih stavki:

1.  Stopa ulazni događaj
2.  Dopušteni Latencija za izvodi posao strujanje analize (a time i veličine obrade zahtjeva za servis strojnog učenja web)
3.  Dodjela resursa SUs analize strujanje i broj zahtjevima za uslugu strojnog učenja web (Dodatne vezane uz funkcija troškovi)

Potpuno particioniranom strujanje analize upit korišten kao primjer. Ako je potrebno složeniji upit [Azure strujanje analize forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) je odličan resurs za dodatnu pomoć tima za strujanje analize.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o strujanje analize, pogledajte:

- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
