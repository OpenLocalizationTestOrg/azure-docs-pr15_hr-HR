<properties
    pageTitle="Arhiviranje koncentratora Azure događaj | Microsoft Azure"
    description="Pregled značajku Azure događaj koncentratora arhiva."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Arhiviranje koncentratora Azure događaja

Arhiviranje koncentratora za Azure događaj omogućuje automatski isporuku strujanja podataka u vaš događaj koncentratora s računom spremište blobova platforme po izboru s dodatnu fleksibilnost da biste postavili vrijeme ili veličine interval vašem odabiru. Postavljanje arhiviranje je brzi, postoje bez administrativne troškove pokrenuti i mijenja automatski veličinu s koncentratorima događaj [propusnost jedinice](event-hubs-overview.md#capacity-and-security). Arhiviranje koncentratora događaj je najjednostavniji način za strujanje podatke učitali u Azure i omogućuje fokusiranje na obradu podataka umjesto prikupljanje podataka.

Azure arhiva koncentratora događaja omogućuje vam da obradi kanali u stvarnom vremenu i grupe na isti toka. To vam omogućuje sastavljanje rješenja koja se može rasti s vašim potrebama tijekom vremena. Hoće li se Stvaranje grupe za sustave danas s oko pri buduće u stvarnom vremenu obrada ili želite dodati učinkovitog Hladna put za postojeće rješenje u stvarnom vremenu, arhiviranje koncentratora događaj omogućuje rad s strujanja podataka.

## <a name="how-event-hubs-archive-works"></a>Kako funkcionira arhiva koncentratora događaja

Događaj koncentratora je međuspremnik durable vrijeme zadržavanja za telemetriju ingress, slično raspodijeljeno zapisnika. Ključ za promjenu veličine u događaj koncentratora je [particioniranom potrošača modela](event-hubs-overview.md#partition-key). Svaki particija je neovisno segmenta podataka i neovisno potrošena. Tijekom vremena te podatke stari isključeno, ovisno o razdoblje zadržavanja konfigurirati. Kao rezultat navedeni koncentratora za događaj nikad ne može vidjeti "previše puni."

Arhiviranje koncentratora događaj omogućuju vam da odredite vlastite spremište blobova platforme Azure računa i spremnik koji će se koristiti za spremanje arhivirane podataka. Taj račun može biti u području isti kao koncentratora za događaj ili u drugoj regiji, dodavanje fleksibilnost značajku arhiva koncentratora za događaj.

Arhivirane podaci se upisuju u obliku [Apache Avro][] ; Sažmi brzo, binarni oblik koji omogućuje strukture podataka obogaćenog umetnutu shemu. Ovaj oblik često se koristi u zajednici Hadoop, kao i strujanje analize i tvorničke Azure podataka. Dodatne informacije o radu s Avro dostupna je u nastavku ovog članka.

### <a name="archive-windowing"></a>Prikaz u prozorima arhive

Arhiviranje koncentratora događaj omogućuje postavljanje prozora da biste odredili arhiviranje. Prozor je Minimalna veličina i vrijeme konfiguraciju s "prvi ima prednost pravilima," što znači uzrokovat će prvi okidača naišao na operaciju arhiva. Ako ste u prozor arhiva petnaest-minutni/100 MB i pošaljite 1 MB/s, veličine prozora će pokrenuti prije prozoru vremena. Svaki particija arhivira neovisno i piše blob dovršene bloka u trenutku arhiviranje, pod nazivom put kada je došlo do interval arhiva. Konvencije imenovanja je na sljedeći način:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Promjena veličine propusnost jedinice

Događaj koncentratora promet upravlja [propusnost jedinice](event-hubs-overview.md#capacity-and-security). Jedinica jedan propusnost omogućuje 1000 događaje u sekundi ingress i dvaput tu količinu izlazne i 1 MB/s. Standardni koncentratora događaj može konfigurirati 1-20 propusnost jedinice pa više moguće kupiti putem kvote povećava [podršku zatražite][]. Korištenje izvan kupljeni propusnost jedinice je ograničio vrijeme. Arhiviranje koncentratora događaj kopira podatke izravno iz Interna događaj koncentratora prostora za pohranu, zaobilaženje propusnost jedinica izlazne kvota i spremanje na izlazne za druge čitače obrada kao što su strujanje analize ili Spark.

Kada je konfiguriran, arhiviranje koncentratora za događaj automatski se pokreće čim šaljete prvi događaj. Pokretanje nastavi cijelo vrijeme. Da biste se lakše za vaše do obradu znati radi li postupak, događaj koncentratora piše prazne datoteke kada nema podataka. To omogućuje predvidljivi cadence i oznake koje možete sažetak sadržaja na obradu procesora.

## <a name="setting-up-event-hubs-archive"></a>Postavljanje događaja koncentratora arhive

Vrijeme stvaranja koncentrator događaja putem portala sustava i upravljanja resursima Azure moguće je konfigurirati arhiva koncentratora za događaj. Klikom na gumb **na** jednostavno omogućiti arhiva. Konfiguriranje računa za pohranu i spremnik tako da kliknete odjeljka **spremnika** u plohu. Budući da događaj koncentratora arhiva koristi provjeru autentičnosti servis za servis s prostora za pohranu, ne morate navesti niza za povezivanje za pohranu. Alat za odabir resursa automatski odabir resursa URI za vaš račun za pohranu. Ako koristite upravitelj resursa Azure, ova URI morate navesti izričito kao niz.

Prozor zadani vrijeme je pet minuta. Minimalna vrijednost je 1, maksimalna 15. Prozor **Veličina** ima raspon 10 500 MB.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Dodavanje arhiva postojeće koncentrator za događaj

Arhive moguće je konfigurirati na postojeći događaj koncentratora koje su u do naziva koncentratora za događaj. Značajka nije dostupna na starije porukama ili Miješano vrsta prostora naziva. Da biste omogućili arhiviranje na postojeće koncentratora za događaj ili promjena postavki arhiviranje, kliknite na prostor naziva za učitavanje plohu **Osnove** , a zatim kliknite u središtu događaja za koji želite omogućiti ili promijenite postavku arhiva. Kliknite **Svojstva** dio Otvori plohu kao što je prikazano na sljedećoj slici.

![][2]

Arhiviranje koncentratora događaja možete konfigurirati i putem Voditelj resursa Azure predložaka. Dodatne informacije potražite u [ovom članku](event-hubs-resource-manager-namespace-event-hub-enable-archive.md).

## <a name="exploring-the-archive-and-working-with-avro"></a>Istraživanje u arhivu i radu s Avro

Kada je konfiguriran, arhiviranje koncentratora događaja stvara datoteke u račun za Azure prostora za pohranu i spremnik naveden u prozoru konfiguriranog vremena. Možete pogledati te datoteke u bilo kojem alatu kao što su [Explorer Azure prostora za pohranu][]. Možete preuzeti datoteke lokalno raditi na njima.

Datoteke koje je stvorio događaj koncentratora arhiva imaju Sljedeća shema Avro:

![][3]

Jednostavan je način za istraživanje Avro datoteka je pomoću [Alata Avro][] posudu iz Apache. Nakon preuzimanja ovaj posudu, vidjet ćete sheme određenom datotekom Avro ponovnim pokretanjem sljedeće naredbe:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

Ta naredba Vrati

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Alati za Avro možete koristiti i da biste pretvorili datoteku JSON OSNOVNI oblik i izvedite druge obrada.

Da biste izvršili naprednije obrada, preuzmite i instalirajte Avro za odabir platforme. Prilikom pisanja ovog dostupno implementacije C, C++, C\#, jezika Java, NodeJS, Perl, PHP, Python i Ruby.

Apache Avro je dovršena vodiči za početak rada za [Java][] i [Python][]. Možete čitati i u članku [Uvod u arhivu koncentratora za događaj](event-hubs-archive-python.md) .

## <a name="how-event-hubs-archive-is-charged"></a>Kako se naplaćuje arhiva koncentratora događaja

Arhiviranje koncentratora događaj je s ograničenim prometom na sličan način jedinice propusnost kao trošak svaki sat. Naplatiti izravno je proporcionalna broj jedinica propusnost kupili za prostor za naziv. Kao što je propusnost jedinice povećati i smanjiti, arhiviranje koncentratora događaj povećava i smanjuje možete unijeti odgovarajuće performansi. Na metar dogoditi u tandemu. Iznos za arhiviranje koncentratora događaj je $0.10 po satu po jedinici propusnost koje se nude na 50% popusta tijekom razdoblja za pretpregled.

Arhiviranje koncentratora događaj doista je najjednostavniji način za dohvaćanje podataka u Azure. Korištenje Lake Azure podataka, tvorničke Azure podataka i Azure HDInsight, na raspolaganju vam obrade i druge analize vašem odabiru pomoću poznatih alata i platformama na bilo kojem razini vam je potrebna.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o događajima koncentratora potražite na sljedećim vezama:

- U Dovršeno [Ogledni program koji koristi događaj koncentratora][].
- Primjer [Skaliranje out događaj obrade s koncentratorima događaj][] .
- [Pregled događaja koncentratora][]

[Apache Avro]: http://avro.apache.org/
[zahtjev za podršku]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Explorer Azure prostora za pohranu]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Alati za Avro]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Promjena veličine izgleda događaj obrade s koncentratorima događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3