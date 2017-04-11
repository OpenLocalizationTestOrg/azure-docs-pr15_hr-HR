<properties
    pageTitle="Izlaz JSON strujanje analitičkih | Microsoft Azure"
    description="Saznajte kako strujanje analize ciljnu Azure DocumentDB za izlaz JSON za arhiviranje podataka i niska Latencija upita na nestrukturirane podatke JSON."
    keywords="JSON Izlaz"
    documentationCenter=""
    services="stream-analytics,documentdb"
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

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>Ciljnu Azure DocumentDB za JSON izlaza iz strujanje Analytics

Analitički strujanje usmjeriti na [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) za JSON Izlaz, omogućivanje podataka arhiviranja i niska Latencija upita nestrukturirane podatke JSON. Saznajte kako najbolje implementirati Ta integracija.

Za osobe koje nisu poznati DocumentDB, pogledajte [DocumentDB, tečaj](https://azure.microsoft.com/documentation/learning-paths/documentdb/) za početak rada.

## <a name="basics-of-documentdb-as-an-output-target"></a>Osnove DocumentDB kao u ciljni Izlaz
Izlaz Azure DocumentDB u strujanje Analytics omogućuje pisanje vaše strujanje obradu rezultata kao rezultat JSON u vašem one DocumentDB. Analitički strujanje nije moguće stvoriti zbirke u bazi podataka, umjesto toga potrebno stvarati ih upfront. Ovo je tako da budu naplatu troškova DocumentDB zbirki prozirne vama i tako da možete precizno podesite performanse, dosljednost i kapacitet zbirke izravno pomoću [DocumentDB API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx). Preporučujemo korištenje jedne baze podataka DocumentDB po strujanje poslu za logično razdvajanje zbirke za strujanje posao.

Neke mogućnosti zbirke DocumentDB detaljne ispod.

## <a name="tune-consistency-availability-and-latency"></a>Precizno podesite dosljednost, dostupnost i latenciji

U skladu s potrebama aplikacije, DocumentDB omogućuje precizno podešavanje baze podataka i zbirki i provjerite gubitke između dosljednost, dostupnost i Latencija. Ovisno o tome koje su razine čitanje dosljednost vašim potrebama scenarij u odnosu na čitanje i pisanje Latencija, možete odabrati razinu dosljednost na računu za bazu podataka. Prema zadanim postavkama DocumentDB omogućuje sinkrono indeksiranja na svakom operaciju u zbirku. Ovo je druga mogućnost korisna za kontrolu pisanja/pročitano performanse u DocumentDB. Dodatne informacije o ovoj temi, pogledajte članak [Promjena baze podataka i upita razina dosljednost](../documentdb/documentdb-consistency-levels.md) .

## <a name="choose-a-performance-level"></a>Odaberite razinu performansi

DocumentDB zbirke mogu se kreirati pri 3 performanse različite razine (S1, S2 ili S3), koji određuju propusnost koje su dostupne za CRUDs za tu zbirku. Uz to, performanse utječe indeksiranja/dosljednost razine u zbirci web. Pogledajte [članak](../documentdb/documentdb-performance-levels.md) razine te performanse detaljno.

## <a name="upserts-from-stream-analytics"></a>Upserts iz strujanje Analytics

Strujanje analize Integracija s DocumentDB omogućuje umetanje ili ažuriranje zapisa u zbirci web DocumentDB na temelju danog stupca ID-a dokumenta. To se naziva *Upsert*.

Analitički strujanje koristi se optimistična Procjena Upsert pristup, gdje ažuriranja samo gotovi kada Umetanje neće uspjeti zbog sukoba ID-a dokumenta. To ažuriranje je izvršavati strujanje analize kao ZAKRPA, tako da omogućuje djelomičnog ažuriranja u dokument, odnosno dodavanja nova svojstva ili Zamjena postojećeg Svojstva ćelije slijedom izvršiti. Imajte na umu da se promjene u vrijednostima svojstava polja u dokument rezultat JSON u cijeli polju Početak prebrisati, odnosno polja nisu spojene.

## <a name="data-partitioning-in-documentdb"></a>Podaci koji se particija u DocumentDB

DocumentDB zbirke omogućuju particija podataka koji se temelje na upit uzoraka i performanse potrebama vaše aplikacije. Svaku zbirku može sadržavati do 10GB podataka (maksimalno), a trenutno ne postoji način radi skaliranja prema gore (i prelijevanje) zbirku. Za skaliranje odgovor, strujanje Analytics omogućuje pisati u više zbirki s određenom prefiks (pogledajte informacije o korištenju ispod). Strujanje analize koristi dosljedan strategije [Razrješavanje raspršivanje particije](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) na temelju naveo stupac PartitionKey korisnik za particija svojim zapisima izlaz. Broj zbirki s prefiksom navedeni prilikom pokretanja strujanje posla se koristi kao particija count Izlaz u kojem posao zapisuje paralelno (zbirke DocumentDB = particije Izlaz). Za jednu S3 zbirke s drži indeksiranja na taj način samo umeće, o 0,4 MB/s pisanje propusnost možete očekivati. Korištenje više zbirki možete dopustiti da biste postigli veću propusnost i povećanu kapaciteta.

Ako namjeravate u budućnosti povećati broj particija možda morati Zaustavi vašeg posla, ponovno stvaranje particija podatke iz postojeće zbirke u nove zbirke i pa ponovno pokrenite posao strujanje analize. Dodatne informacije o korištenju PartitionResolver i ponovno particija uz ogledni kod uvrstiti u objavu uputa za daljnji rad. U članku [particija u DocumentDB](../articles/documentdb-partition-data.md#developing-a-partitioned-application) navedeni i Detalji o tome.

## <a name="documentdb-settings-for-json-output"></a>Postavke DocumentDB JSON Izlaz

Stvaranje DocumentDB kao na Izlaz u strujanje analize generira upita za podatke kao što se vidi ispod. U ovom se odjeljku nalaze objašnjenje definiciju svojstva.

![documentdb strujanje analize izlaz zaslona](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Pseudonim za izlaz** – pseudonim za upućivanje ovog Izlaz u upitu ASA  
-   **Naziv računa** – ime ili krajnjoj točki URI DocumentDB računa.  
-   **Ključ računa** – zajednički pristup ključ za račun DocumentDB.  
-   **Baza podataka** – naziv baze podataka u DocumentDB.  
-   **Uzorak naziv zbirke** – uzorak naziv zbirke za zbirke koja će se koristiti. Oblik naziva zbirke možete konstruirana pomoću token neobavezno {particija} gdje će se particije Počni od 0. Slijede uzorka valjanih unosa:  
   1\) MyCollection – jednu zbirku pod nazivom "MyCollection" mora postojati.  
   2\) – takve zbirke mora postojati – MyCollection {particija} "MyCollection0", "MyCollection1", "MyCollection2" i tako dalje.  
-   **Ključ particija** – naziv polja u stavkama događaja izlaz koristi se za određivanje ključ za particija izlazna preko zbirke. Za izlazni jedinstvenu zbirku može biti bilo koji stupac proizvoljne izlaz koristi npr PartitionId.  
-   **ID-a dokumenta** – nije obavezno. Naziv polja u izlaz događaja koji se koristi za određivanje primarni ključ koji umetanje ili ažuriranje operacije temelje.  
