<properties
    pageTitle="Podatkovne veze baze podataka: podatke strujanje unose iz programa toka događaj | Microsoft Azure"
    description="Saznajte više o postavljanju podatkovnu vezu strujanje analize pod nazivom "unosa". Unosi obuhvaćaju strujanja podataka iz događaja i se odnose na podatke."
    keywords="tijeku podataka, podatkovne veze baze podataka, a zatim strujanje događaja"
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

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Podatkovne veze baze podataka: dodatne informacije o podacima strujanje unose iz događaje strujanje Analytics

Podatkovna veza za strujanje analize je strujanja podataka događaje iz izvora podataka. To se naziva "ulaz". Strujanje analize je jednostavno prva liga Integracija s Azure strujanje izvora koncentrator događaj, IoT koncentratora i Blob pohrana podataka koji se mogu s istim ili različitim Azure pretplate kao posla analize.

## <a name="data-input-types-data-stream-and-reference-data"></a>Vrste za unos podataka: podataka strujanje i pregled podataka
Kao što je podataka pritisak na izvor podataka, je troše posao strujanje analize i obrađuju u stvarnom vremenu. Unosi su podijeljeni dvije različite vrste: podataka strujanje unosa i referencu unosa podataka.

### <a name="data-stream-inputs"></a>Unosi toka podataka
Tijeku podataka je neograničeno slijeda događajima u budućnosti tijekom vremena. Strujanje analize poslove mora sadržavati barem jedan podatkovni strujanje unos utrošiti i transformacije posla. Spremište blobova platforme, koncentratorima događaja i IoT koncentratorima podržane su kao strujanje unos izvore. Događaj koncentratorima se koriste za prikupljanje tokove događaja na više uređaja i servise, kao što je sažeci sadržaja aktivnosti društvene mreže, burzovni trgovinu podatke ili podatke iz senzori. IoT koncentratora su optimizirani za prikupljanje podataka iz povezanih uređaja u scenarijima Internet stvari (IoT).  Spremište blobova platforme moguće je koristiti kao izvor unosa za ingesting skupno podataka kao tok.  

### <a name="reference-data"></a>Pregled podataka
Analitički strujanje podržava druga vrsta unos naziva referentnih podataka. Ovo je pomoćna podataka koje je statički ili sporo mijenjaju tijekom vremena i obično koristi za izvođenje korelacije i look-ups. Azure blobova trenutno samo podržanih ulazni izvor za referentnih podataka. Pregled podataka izvora blob-ova ograničeni su na 100MB veličine.
Da biste saznali kako stvoriti referencu unosa podataka, potražite u članku [Korištenje referentnih podataka](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Stvaranje unosa za strujanje podataka s koncentratora za događaj

[Azure događaj koncentratora](https://azure.microsoft.com/services/event-hubs/) su izrazito podesiva objavljivanja-pretplate ingestor događaj. Ga možete prikupiti milijune događaje u sekundi, tako da mogu obraditi i analizirajte pretraživanje velikog količine podataka koje je stvorio povezanih uređaja i aplikacije. To je jedan od najčešće korištenih ulaza za strujanje analize. Koncentratorima događaja i analize strujanje zajedno nude kupci rješenje za kraj da biste završetka za analizu u stvarnom vremenu. Događaj koncentratora omogućuju klijente dolaziti događaje Azure u stvarnom vremenu i analize strujanje zadacima možete ih obrađivati u stvarnom vremenu. Na primjer, klijentima možete slati klikova web, senzor čitanja, online zapisnika događaja s koncentratorima događaja i stvaranje strujanje analize zadataka da biste upotrijebili događaj koncentratora strujanja ulaznih podataka za filtriranje u stvarnom vremenu, Zbrajanje i korelacije.

Važno Imajte na umu da vremenske oznake zadani događaja koji dolaze iz koncentratora događaja u strujanje analize vremenska oznaka koje će se događaj stigli u središte događaja koji je EventEnqueuedUtcTime je. Za obradu podataka kao tok pomoću vremenske oznake u opseg događaj, mora se koristiti [Vremenske oznake po](https://msdn.microsoft.com/library/azure/dn834998.aspx) ključnoj riječi.

### <a name="consumer-groups"></a>Grupe korisnika

Svaki unos strujanje analize događaj koncentrator mora biti konfigurirana tako da imati vlastitu grupu korisnika. Kada posao sadrži na koja se sama pridružite ili više unosa, povratnu može pročitati više čitač slijedu, koji utječe brojni čitači u grupu za jednog korisnika. Da biste izbjegli premašuju događaj koncentrator ograničenje od 5 čitatelji po grupi potrošača po particije, je najbolji način za određivanje korisničke grupe za svaki zadatak strujanje analize. Imajte na umu da postoji i ograničenje od 20 grupe korisnika po koncentratora za događaj. Dodatne informacije potražite u [Vodiču za programiranje događaja koncentratora](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-event-hub-as-an-input-data-stream"></a>Konfiguriranje događaja koncentrator kao programa strujanje ulaznih podataka

U tablici u nastavku objašnjeno svakog svojstva u događaja koncentrator za unos kartica s njezin opis:

| NAZIV SVOJSTVA | OPIS |
|------|------|
| Pseudonim za unos | Neslužbeni naziv koji će se koristiti u upitu posao referentni ovog unosa |
| Polje naziva Bus servisa | Prostor za naziv Bus servis je spremnik za skup poruka entiteti. Kada ste stvorili novi događaj koncentrator, i stvara prostor naziva Bus servisa. |
| Koncentrator za događaj | Naziv za unos koncentratora za događaj. |
| Naziv pravila koncentrator događaja | Pravilnik zajednički pristup, koji se može stvoriti na kartici događaj koncentrator konfiguriranje. Svaki pravila zajednički pristup će imati naziv dozvole koje ste postavili i pristupne ključeve. |
| Ključ pravila koncentrator za događaj | Zajednički se koristi tipkovnog provjerava autentičnost na temelju pristup prostora za naziv Bus servisa. |
| Događaj koncentrator korisničke grupe (nije obavezno) | Grupa potrošača za ingest podatke iz koncentratora za događaj. Ako nije naveden, zadacima strujanje analize će koristiti potrošača grupi zadani ingest podatke iz koncentratora za događaj. Preporučuje se da biste koristili različita korisnička grupa za svaku strujanje analize. |
| Događaj serijaliziranog oblika | Da biste bili sigurni upitima funkcionira kao što ste očekivali, strujanje analize treba znati koji oblik serijalizacije (JSON, CSV ili Avro) koristite za dolazne strujanja podataka. |
| Šifriranje | UTF-8 trenutno samo podržani oblik kodiranja. |

Ako podataka dolazi iz izvora koncentrator događaj, možete pristupiti na nekoliko polja metapodataka u upitu strujanje analize. U tablici u nastavku navedeni polja i njihov opis.

| SVOJSTVO | OPIS |
|------|------|
| EventProcessedUtcTime | Datum i vrijeme događaja je obradili strujanje analize. |
| EventEnqueuedUtcTime | Datum i vrijeme događaja je primio koncentratorima događaj. |
| PartitionId | ID polje particija za unos prilagodnika. |

Ako, na primjer, možda pišete upita kao što je sljedeća:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Stvaranje ulaz strujanja podataka IoT koncentratora

Azure Iot koncentrator je vrlo skalabilni objavljivanja-pretplate događaj ingestor optimizirana IoT scenarija.
Važno Imajte na umu da vremenske oznake zadani događaja koji dolaze iz koncentratora IoT u strujanje analize vremenske oznake koje događaj stigli u središte IoT koji je EventEnqueuedUtcTime je. Za obradu podataka kao tok pomoću vremenske oznake u opseg događaj, mora se koristiti [Vremenske oznake po](https://msdn.microsoft.com/library/azure/dn834998.aspx) ključnoj riječi.

> [AZURE.NOTE] Moguće je obraditi samo poruke poslane s svojstvo DeviceClient.

### <a name="consumer-groups"></a>Grupe korisnika

Svaki unos strujanje analize IoT koncentrator mora biti konfigurirana tako da imati vlastitu grupu korisnika. Kada posao sadrži na koja se sama pridružite ili više unosa, povratnu može pročitati više čitač slijedu, koji utječe brojni čitači u grupu za jednog korisnika. Da biste izbjegli premašuju koncentrator IoT ograničenje od 5 čitatelji po grupi potrošača po particije, je najbolji način za određivanje korisničke grupe za svaki zadatak strujanje analize.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>Konfiguriranje IoT koncentrator kao u tijeku podataka za unos

U dolje navedenoj tablici objašnjavaju svakog svojstva na kartici IoT koncentrator unos s njezin opis:

| NAZIV SVOJSTVA | OPIS |
|------|------|
| Pseudonim za unos | Neslužbeni naziv koji će se koristiti u upitu posao referentni taj unos. |
| IoT koncentratora | Koncentratora za IoT je spremnik za skup razmjenu entiteti. |
| Krajnja točka | Naziv krajnje točke na središte IoT. |
| Naziv pravila zajednički pristup | Zajednički pristup pravila da biste omogućili pristup središtu IoT. Svaki pravila zajednički pristup će imati naziv dozvolama koje ste postavili i pristupne ključeve. |
| Zajednički pristup pravila ključ | Zajednički se koristi tipkovnog provjerava autentičnost na temelju pristup središtu IoT. |
| Korisničke grupe (nije obavezno) | Grupa potrošača da biste podatke iz koncentratora IoT ingest. Ako nije naveden, zadacima strujanje analize će koristiti potrošača grupi zadani ingest podataka iz koncentratora IoT. Preporučuje se da biste koristili različita korisnička grupa za svaku strujanje analize. |
| Događaj serijaliziranog oblika | Da biste bili sigurni upitima funkcionira kao što ste očekivali, strujanje analize treba znati koji oblik serijalizacije (JSON, CSV ili Avro) koristite za dolazne strujanja podataka. |
| Šifriranje | UTF-8 trenutno samo podržani oblik kodiranja. |

Ako podataka dolazi iz izvora IoT koncentrator, možete pristupiti na nekoliko polja metapodataka u upitu strujanje analize. U tablici u nastavku navedeni polja i njihov opis.

| SVOJSTVO | OPIS |
|------|------|
| EventProcessedUtcTime | Datum i vrijeme koji nije obrađen događaj. |
| EventEnqueuedUtcTime | Datum i vrijeme događaja je primio središtu IoT. |
| PartitionId | ID polje particija za unos prilagodnika. |
| IoTHub.MessageId | Služi za povezivanje dvosmjerni komunikacije u središte IoT. |
| IoTHub.CorrelationId | Koristi za odgovore i povratnih informacija u IoT koncentratora. |
| IoTHub.ConnectionDeviceId | Čija je autentičnost provjerena id koji se koristi da biste poslali poruku, oznaku na porukama koje su servicebound po IoT koncentratora. |
| IoTHub.ConnectionDeviceGenerationId | GenerationId čija je autentičnost provjerena uređaj koji se koristi da biste poslali poruku, oznaku na porukama koje su servicebound po IoT koncentratora. |
| IoTHub.EnqueuedTime | Vrijeme kad je primio poruku IoT koncentratora. |
| IoTHub.StreamId | Svojstvo prilagođeni događaj dodao uređaj pošiljatelja. |

## <a name="create-a-blob-storage-data-stream-input"></a>Stvaranje strujanje unosa podataka spremište blobova platforme

Za scenarije s velikim količinama nestrukturirane podatke za pohranu u oblaku, blobova nudi učinkovit i skalabilni rješenja. Podatke u [spremište blobova platforme](https://azure.microsoft.com/services/storage/blobs/) obično se smatra brzina "ostale", ali je može obraditi kao tok podataka strujanje analize. Jedan uobičajeni scenarij Blob prostora za pohranu unose s strujanje analize je obrada zapisnika, gdje telemetrijskih se hvata iz sustava i mora biti raščlaniti i obrađuju za izdvajanje smisleni podataka.

Važno Imajte na umu da zadani vremenske oznake Blob prostora za pohranu događaja u strujanje analize vremenske oznake blob-om izmjene *isBlobLastModifiedUtcTime*je. Za obradu podataka kao tok pomoću vremenske oznake u opseg događaj, mora se koristiti [Vremenske oznake po](https://msdn.microsoft.com/library/azure/dn834998.aspx) ključnoj riječi.

Imajte na umu da CSV oblikovani unosa **obavezan** redak zaglavlja da biste definirali polja za skup podataka. Daljnje polja retka zaglavlja svi moraju biti **Jedinstveni**.

> [AZURE.NOTE] Analitički strujanje ne podržava dodavanje sadržaja u postojeće blob. Analitički strujanje samo pregledavati blob jednom i promjene gotovo kada ovaj čitanje nije obrađen. Najbolje je da biste prenijeli sve podatke na jednom i dodati sve dodatne događaje u spremište blobova platforme.

U dolje navedenoj tablici objašnjavaju svakog svojstva u Blob kartici pohrana za unos s njezin opis:

<table>
<tbody>
<tr>
<td>NAZIV SVOJSTVA</td>
<td>OPIS</td>
</tr>
<tr>
<td>Pseudonim za unos</td>
<td>Neslužbeni naziv koji će se koristiti u upitu posao referentni taj unos.</td>
</tr>
<tr>
<td>Račun za pohranu</td>
<td>Naziv računa spremišta gdje se nalaze datoteke blob.</td>
</tr>
<tr>
<td>Ključ za račun za pohranu</td>
<td>Tajna tipku povezanu s računom za pohranu.</td>
</tr>
<tr>
<td>Spremnik za pohranu
</td>
<td>Spremnika sadrže logičke grupiranja za pohranjene na servisu Microsoft Azure Blob blob-ova. Kada prenesete blob Blob servisa, morate navesti spremnik za taj blob.</td>
</tr>
<tr>
<td>Put prefiks uzorak [neobavezni]</td>
<td>Put datoteke primijenjenih radi pronalaženja blob-ova unutar navedeni kontejnera.
U parametru path, odaberete Navedite jednu ili više instanci sljedeće varijable 3:<BR>{date}, {vrijeme,}<BR>{particija}<BR>Primjer 1: cluster1/zapisnika / {date} / {vremena} / {particija}<BR>Primjer 2: cluster1/zapisnika / {date}<P>Imajte na umu da "*" nije dopušteno vrijednost za pathprefix. Nije dopušteno samo valjani <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">znakovi blobova platforme Azure</a> .</td>
</tr>
<tr>
<td>Oblik datuma [neobavezni]</td>
<td>Ako se u put prefiks koristi token datum, možete odabrati oblik datuma u kojima su organizirane datotekama. Primjer: Gggg/MM/DD</td>
</tr>
<tr>
<td>Oblik vremena [neobavezni]</td>
<td>Ako se u put prefiks koristi token vrijeme, navedite oblik vremena u kojem su organizirane datotekama. Trenutno je jedini podržani vrijednost HH.</td>
</tr>
<tr>
<td>Događaj serijaliziranog oblika</td>
<td>Da biste bili sigurni upitima funkcionira kao što ste očekivali, strujanje analize treba znati koji oblik serijalizacije (JSON, CSV ili Avro) koristite za dolazne strujanja podataka.</td>
</tr>
<tr>
<td>Šifriranje</td>
<td>CSV i JSON, UTF-8 trenutno samo podržani oblik kodiranja.</td>
</tr>
<tr>
<td>Graničnik</td>
<td>Analitički strujanje podržava nekoliko uobičajenih graničnike Serijalizacija podataka u obliku CSV. Podržani vrijednosti su zarezom sa zarezom, razmak, uvlaka i okomitu crtu.</td>
</tr>
</tbody>
</table>

Kada vaši podaci dolazi iz izvora spremište blobova platforme, možete pristupiti nekoliko polja metapodataka u upitu strujanje analize. U tablici u nastavku navedeni polja i njihov opis.

| SVOJSTVO | OPIS |
|------|------|
| BlobName | Naziv unosa blob isporučenu događaj iz. |
| EventProcessedUtcTime | Datum i vrijeme događaja je obradili strujanje analize. |
| BlobLastModifiedUtcTime | Datum i vrijeme zadnje izmjene blob-om. |
| PartitionId | ID polje particija za unos prilagodnika. |

Ako, na primjer, možda pišete upita kao što je sljedeća:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci
Mogućnosti podatkovnih veza u Azure ste naučili za strujanje Analitika poslova. Da biste saznali više o strujanje analize, pogledajte:

- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
