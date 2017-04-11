<properties 
    pageTitle="Najčešća pitanja o događaj koncentratora | Microsoft Azure"
    description="Najčešća pitanja vezana uz koncentratora događaj."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Događaj koncentratora najčešća Pitanja

Događaj koncentratora sadrži veliki intake postojanost i obrada podataka događaje iz izvora podataka visokom propusnošću i/ili milijune uređaja. Kada u paru s servisa Bus redovima i teme koncentratora događaj omogućuje trajni implementacijama naredba i kontrola za scenarije [Internet stvari (IoT)](https://azure.microsoft.com/services/iot-hub/) .

U ovom se članku govori o podatke o cijenama i odgovori na najčešća pitanja o koncentratora događaja:

## <a name="pricing-information"></a>Informacije o cijenama

Potpune informacije o događajima koncentratora cijene, potražite u članku [informacije o cijenama koncentratora za događaj](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Način izračuna koncentratora događaj ingress događaja?

Svaki događaj šalje koncentratora za događaj broji kao naplatu poruku. *Događaj ingress* se definira kao jedinica podataka koja je manja od ili jednako 64 KB. Bilo koji događaj koja je manja od 64 KB veličine smatra se jedan naplatu događaj. Ako je veći od 64 KB događaj, broj naplatu događaja se izračunava skladu s veličinom događaja u višestrukim grafikonima od 64 KB. Ako, na primjer, događaj 8 KB poslane koncentrator događaj naplate kao jedan događaj, no 96 KB poruku poslao koncentrator događaj naplate kao dva događaja.

Događaji potrošena iz centra za događaj, kao i postupci upravljanja i kontrolirati pozive kao što su checkpoints, ne računaju se kao naplatu ingress događaji, ali ako skupi najviše jedinica odbitak propusnost.

## <a name="what-are-event-hubs-throughput-units"></a>Što su jedinice propusnost koncentratora događaja?

Izričito odabrati propusnost jedinice koncentratora događaj, portala za Azure ili predlošci upravitelja resursa koncentratora za događaj. Jedinice propusnost Primijeni na sve koncentratora događaja u do naziva događaja koncentratora i daje svaku jedinicu propusnost pravo naziva sljedeće mogućnosti:

- Prema gore na 1 MB sekundi ingress događaja (događaji poslana u koncentratora za događaj), ali ne više od 1000 ingress događaje, operacija upravljanja ili kontrole API poziva sekundi.

- Do 2 MB sekundi izlazne događaja (događaje potrošena iz centra za događaj).

- Do 84 GB prostora za pohranu događaja (dovoljni za zadano razdoblje zadržavanja 24-satni).

Jedinice propusnost koncentratora za događaj se naplatiti zaračunava, ovisno o maksimalni broj jedinica odabrane tijekom navedene sat.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Kako se događaj koncentratora propusnost jedinica ograničenja primjenjuju?

Ako propusnost ukupni ingress ili događaj stopa ukupni ingress preko svih događaja koncentratora u prostor naziva premašuje dostupan jedinica zbrajanja propusnost, pošiljatelja će ograničio vrijeme i primanje pogreške koja označava da je premašena kvota ingress.

Ako propusnost ukupni izlazne ili stopu izlazne ukupni događaj preko svih događaja koncentratora u prostor naziva premašuje dostupan jedinica aggregate propusnost, primatelje ograničio se vrijeme i primanje pogreške koja označava da je premašena kvota izlazne. Ingress i izlazne kvote provode se zasebno, tako da nema pošiljatelj može uzrokovati događaj potrošnju usporiti ni u onemogućuje događaja koja se šalje u koncentratora za događaj.

Imajte na umu da odabir jedinica propusnost ovisi o broju razdjeljivanja koncentratora za događaj. Dok se svaki particija nudi Maksimalna propusnost 1 MB po drugi ingress (s najviše 1000 događaje u sekundi) i 2 MB po drugi izlazne, postoji bez fixed naknade za particije sami. Naplatiti je jedinica Zbrojeno propusnost na sve koncentratora događaja u do naziva koncentratora za događaj. Ovaj uzorak omogućuje stvaranje dovoljno particije za podršku za predviđenu Maksimalno opterećenje u sustavima, bez povećavanja bilo kakvih troškova jedinica propusnost dok učitavanja događaja u sustavu zapravo zahtijeva veću propusnost brojeva i povećava se bez promjene strukture i arhitektura vašeg sustava kao mogućnost Učitaj u sustavu.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Postoji li ograničenje broja jedinice propusnost koje je moguće odabrati?

Nema ograničenja zadani 20 jedinica propusnost po prostor naziva. Možete zatražiti veća ograničenja propusnost jedinica po arhiviranja zahtjev za podršku možete. Izvan ograničenja za jedinica za 20 propusnost objedinjuje dostupne su u 20 do 100 jedinica propusnost. Imajte na umu da pomoću više od 20 jedinica propusnost uklanja mogućnost da biste promijenili broj jedinica propusnost bez spremanja zahtjev za podršku možete.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Je li naknadu za zadržavanje događaje događaj koncentratora za više od 24 sata?

Događaj koncentratora standardne sloju omogućuju poruke zadržavanja razdoblja dulje od 24 sata, za najviše 30 dana. Ako je veličina ukupan broj pohranjene događaje premašuje odbitak za pohranu za broj jedinica odabranog propusnost (84 GB po jedinici propusnost), veličina veći od popust se naplaćuje objavljene brzinom spremište blobova platforme Azure. Odbitak za pohranu u svaku jedinicu propusnost pokriva sve troškove prostora za pohranu za razdoblja zadržavanja od 24 sata (zadano) čak i ako se za odbitak Maksimalna ingress koristi propusnost jedinice prema gore.

## <a name="what-is-the-maximum-retention-period"></a>Što je razdoblje zadržavanja Maksimalna?

Standardni koncentratora sloju događaj trenutno podržava razdoblje zadržavanja Maksimalna od sedam dana. Imajte na umu da koncentratora događaj nije namijenjen kao spremište trajna podataka. Scenariji u kojima je praktičan ponavljanje programa strujanje događaja u istom sustavima; su namijenjene razdoblja zadržavanja veći od 24 sata na primjer, obuka i provjerite je li novi model učenje računala na postojećim podacima.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Kako se veličina prostora za pohranu događaj koncentratora izračunava i naplatiti?

Ukupnu veličinu svih događaja spremljene, uključujući interne indirektni za događaj zaglavlja ili na disku za pohranu strukture u svih događaja koncentratora mjeri se tijekom dana. Na kraju dan izračunava se veličina prostora za pohranu Vršna. Dnevni odbitak za pohranu izračunato na temelju najmanji broj jedinica propusnost koje ste odabrali tijekom dana (svaku jedinicu propusnost nudi programa odbitak 84 GB). Ako ukupnu veličinu premašuje izračunati dnevnih odbitak za pohranu, suvišno za pohranu naplate za korištenje stope spremište blobova platforme Azure (brzinom **Lokalno suvišnih prostora za pohranu** ).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Mogu li koristiti jednu AMQP vezu za slanje i primanje iz koncentratora za događaj i redova teme Bus servis?

Da, pod uvjetom da sve koncentratora događaj, redovima i teme su u isti prostor naziva. Kao takve, možete implementirati dvosmjerni, brokered veza s brojnim uređajima s subsecond latencies učinkovit i vrlo skalabilni način.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Nemojte brokered veze naplaćuje s koncentratorima događaja?

Za pošiljatelja, veze naplaćuje samo kada se koristi protokol AMQP. Postoje bez naknade veze za slanje događaja putem HTTP, bez obzira na to koliko slanje sustavi ili uređaji. Ako namjeravate koristiti AMQP (na primjer, da biste postigli učinkovitiji strujanje događaj ili da biste omogućili komunikacije piše Dvosmjerno u IoT naredba i kontrola scenarijima) pogledajte [informacije o cijenama servisa Bus](https://azure.microsoft.com/pricing/details/service-bus/) stranice informacije o što čine brokered veze i kako ih se s ograničenim prometom.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Koja je razlika između događaj koncentratora osnovne i standardne razine?

Događaj koncentratora standardne sloju sadrži više od onog što je dostupno u događaj koncentratora osnovni, a u nekim konkurencije sustavima. Te značajke uključuju razdoblja zadržavanja dulje od 24 sata i mogućnost da biste koristili jednu AMQP vezu da biste poslali naredbe velikog broja uređaja s subsecond latencies, kao i za slanje telemetrijskih iz te uređaje u događaj koncentratora. Na popisu značajki potražite u članku [informacije o cijenama koncentratora za događaj](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="geographic-availability"></a>Geografske dostupnosti

Događaj koncentratora dostupna je u sljedećim regijama:

|Zemlj.|Područja|
|---|---|
|Sjedinjene Države|Središnje SAD-a, Istočni SAD-a, Istočni sad 2, Južna središnje SAD-a, Zapad SAD-a|
|Europa|Sjeverna Europa, Zapad Europe|
|Države Pacifičke|Istočnoazijski, Jugoistočne Azije|
|Japan|Japanska ožujak Japan Zapad|
|Brazil|Južna Brazil|
|Australija|Australija istočnoazijske, južnoazijske Australija|

## <a name="support-and-sla"></a>Podrška i SLA

Tehnička podrška za događaj koncentratora je dostupan putem [forumi zajednice korisnika](https://social.msdn.microsoft.com/forums/azure/home). Podrška za upravljanje naplatom i pretplatom navedeni su na nema trošak.

Dodatne informacije o oglednim SLA potražite u članku stranici [Ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/) .

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o događajima koncentratora, potražite u sljedećim člancima:

- [Pregled događaja koncentratora][].
- U Dovršeno [Ogledni program koji koristi događaj koncentratora][].

[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
