<properties
 pageTitle="Vodič za razvojne inženjere - porukama | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – uređaj u oblak i oblaka uređaj poruka"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="send-and-receive-messages-with-iot-hub"></a>Slanje i primanje poruka s IoT koncentratora

## <a name="overview"></a>Pregled

Koncentrator IoT nudi sljedeće razmjenu primitives komunikaciju s uređajem:

- [Uređaj u oblak] [ lnk-d2c] s uređaja u aplikaciju sigurnosno Završi.
- [Oblak uređaj] [ lnk-c2d] iz aplikacije sigurnosno završetka (*servis* "ili" *oblaka*").

Svojstva Core razmjenu funkcionalnosti koncentrator IoT su pouzdanosti i rok trajanja poruke. Tih svojstava omogućiti resilience da biste Povremeni povezivanje na strani uređaja, a da biste učitali krivina u događaj obrade na strani oblaka. Koncentrator IoT implementira *barem jednom* isporuke jamstva uređaj u oblak i oblaka uređaj razmjene poruka.

Koncentrator IoT podržava više [uređaja dostupnog protokola] [ lnk-protocols] (kao što su MQTT, AMQP i HTTP-a). Da biste podržali objedinjenog interoperabilnost putem protokola, IoT koncentrator definira [uobičajeni oblik poruke] [ lnk-message-format] koji podržavaju sve protokole okrenutom uređaj.

Koncentrator IoT izlaže [krajnje kompatibilan s preglednikom događaja koncentrator] [ lnk-compatible-endpoint] da biste omogućili pozadinskih aplikacija za čitanje poruka uređaj u oblak primio središtu.

### <a name="when-to-use"></a>Kada se koristi

Poruka je mogućnost core IoT koncentratora. Upotrijebite je kada morate slanje poruka s uređaja na pozadini ili slati poruke iz vaše pozadinskih na uređaj.

Usporedbe IoT koncentratora i koncentratora događaja servisa, potražite u članku [koncentrator za usporedbu IoT i događaja koncentratora][lnk-compare].

## <a name="device-to-cloud-messages"></a>Uređaj u oblak poruke

Slanje poruka uređaja u oblaku putem uređaja dostupnog krajnje (**/devices/ {deviceId} / poruke/događaja**). Servis za pozadinsku Prima poruke uređaja u oblaku putem servisa dostupnog krajnje (**/messages/events**) koji je kompatibilan s [Koncentratorima događaj][lnk-event-hubs]. Zbog toga možete koristiti standardni [Integracija koncentratora za događaj i SDK-ovi] [ lnk-compatible-endpoint] primati poruke uređaj u oblak.

Koncentrator IoT implementira uređaj u oblak poruka u način na koji je slična [Koncentratora događaj][lnk-event-hubs]. Koncentrator IoT uređaja u oblak poruke su sličniji događaj koncentratora *događaje* od [Servisa Bus] [ lnk-servicebus] *poruke*.

Ova implementacija sadrži utjecaju na sljedeće:

* Isto tako na događaje događaj koncentratora uređaj u oblak poruke su durable i zadržava se u koncentratora za IoT za najviše sedam dana (potražite u članku [mogućnosti konfiguracije uređaja u oblak][lnk-d2c-configuration]).
* Uređaj u oblak poruke imaju preko fiksni skup particije koja je postavljena na vrijeme stvaranja (potražite u članku [mogućnosti konfiguracije uređaja u oblak][lnk-d2c-configuration]).
* Analogously s koncentratorima događaj klijenti čitanju poruka uređaj u oblak mora rukovati particije i checkpointing. Potražite u članku [Koncentratora događaj – troše događaji][lnk-event-hubs-consuming-events].
* Kao što su događaji događaj koncentratora uređaj u oblak poruke može biti najviše 256 KB i mogu grupirati u grupama za optimiziranje šalje. Serija može biti najviše 256 KB i najviše 500 poruka.

Razlikuju, no nekoliko važnih koncentrator IoT uređaja u oblak poruka i koncentratora događaja:

* Kao što je opisano u [kontrolu pristupa koncentrator IoT] [ lnk-devguide-security] u odjeljku središte IoT omogućuje provjeru autentičnosti i pristup kontrole po uređaja.
* Koncentrator IoT omogućuje milijune istodobno povezanih uređaja (potražite u članku [kvotama i ograničavanje][lnk-quotas]), dok događaj koncentratora ograničeno je na 5000 AMQP veze po prostor naziva.
* Koncentrator IoT ne dopušta proizvoljne particija pomoću **PartitionKey**. Uređaj u oblak poruke imaju na temelju njihove izvorišne **deviceId**.
* Promjena veličine IoT koncentrator je malo razlikovati od skaliranja koncentratora za događaj. Dodatne informacije potražite u odjeljku [Skaliranje koncentrator IoT][lnk-guidance-scale].

> [AZURE.NOTE] Ne možete zamijeniti IoT koncentrator za događaj koncentratora u svim situacijama. Na primjer, u computations za obradu neki događaj možda potrebno ponovno stvaranje particija događaje vezana uz različite svojstvo ili polje prije analiza strujanja podataka. U ovom scenariju, možete koristiti koncentratora za događaj za decouple dva dijelove strujanje obrade kanal. Dodatne informacije potražite u članku *particije* u [Azure događaj koncentratora pregled][lnk-eventhub-partitions].

Detalje o korištenju uređaja u oblak poruke, potražite u člancima [IoT koncentrator API -ji i SDK-ovi][lnk-sdks].

> [AZURE.NOTE] Prilikom korištenja HTTP-a za slanje poruka uređaj u oblak, svojstvo nazive i vrijednosti može sadržavati samo ASCII alfanumeričkih znakova, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Osobe koje nisu telemetrijskih promet

Često, osim točke podataka telemetrijskih uređaja slati poruke i zahtjeve za koje je potrebno izvođenja i rukovanja iz aplikacije poslovne logike sloj. Ako, na primjer, ključnih upozorenja koje morate pokrenuti određene akcije u pozadinskih ili uređaj odgovore na naredbe poslao pozadinska.

Dodatne informacije o najbolji način za obradu ove vrste poruka potražite u [Praktični vodič: način obrade koncentrator IoT uređaja u oblak poruke] [ lnk-d2c-tutorial] vodič.

### <a name="device-to-cloud-configuration-options"></a>Mogućnosti konfiguracije uređaja u oblak

Koncentratora za IoT izlaže sljedeća svojstva da biste omogućili kontrolu uređaja u oblak poruka.

* **Broj particije**. Ovo svojstvo možete postaviti na stvaranje da biste definirali broj particije za ingestion uređaja u oblak događaj.
* **Vrijeme zadržavanja**. Ovo svojstvo određuje koliko dugo poruka uređaj u oblak. Zadana vrijednost je jedan dan, ali ga možete povećati sedam dana.

Osim toga, analogously s koncentratorima događaj IoT koncentrator omogućuje vam upravljanje korisničke grupe na uređaju-na-oblaka primanje krajnjoj točki.

Možete izmijeniti sve tih svojstava, ili programski putem [davatelja resursa IoT koncentrator REST API -ji][lnk-resource-provider-apis], ili pomoću [portala za Azure][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Anti krivotvorenje svojstva

Da biste izbjegli uređaj krivotvorenje koncentrator IoT uređaja u oblak poruke žigovima svih poruka s sljedeća svojstva:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Prva dva sadrže **deviceId** i **generationId** izvorišne uređaja, po [Svojstva uređaja identiteta][lnk-device-properties].

Svojstvo **ConnectionAuthMethod** sadrži JSON serijalizirani objekt, uz sljedeća svojstva:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Oblak uređaj poruke

Slanje poruka oblaka uređaja putem servisa dostupnog krajnje (**/messages/devicebound**). Na uređaju prima ih putem krajnje specifične za uređaj (**/devices/ {deviceId} / poruke/devicebound**).

Svaku poruku oblaka uređaj je namijenjeno na jedan uređaj tako da postavite svojstvo **Da biste** na **/devices/ {deviceId} / poruke/devicebound**.

>[AZURE.IMPORTANT] Svaki uređaj red sadrži najviše 50 oblaka uređaj poruke. Pokušavate slanje više poruka na istom uređaju rezultira pogreškom.

> [AZURE.NOTE] Kad šaljete poruke oblaka uređaj, svojstvo nazive i vrijednosti može sadržavati samo ASCII alfanumeričkih znakova, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Životni ciklus poruke

Da bi barem jednom Isporuka poruke, IoT koncentrator nastavi oblaka uređaj poruke u redovima po uređaja. Uređaji morate izričito prihvatite *dovršetka* za koncentrator IoT da biste ih uklonili iz reda. Time će se jamčiti otpornost odnosu s povezivanjem i pogreške na uređaj.

Sljedeći dijagram prikazuje stanje grafikonu životni ciklus za poruku oblaka uređaja.

![Životni ciklus oblaka uređaj poruke][img-lifecycle]

Kada je servis šalje poruku, smatra *Enqueued*. Kada uređaj želi *dobiti* poruku, koncentrator IoT *zaključavanja* poruke (postavlja stanje **nevidljivi**) dopuštanja drugim niti na istom uređaju da biste pokrenuli daljnje poruke. Kada uređaj niti dovrši obrada poruke, obavještava IoT koncentrator s *dovršavanje* poruke.

Na uređaju, možete:

- *Odbacivanje* poruke uzrokuje IoT koncentrator da biste postavili **Deadlettered** stanje. Napomena: uređaje koji se povezuju s MQTT ne može odbaciti C2D poruke.
- *Ćete* poruku koja uzrokuje IoT koncentrator vratite poruke na red sa statusom postavite na **Enqueued**.

Niti nije uspjelo obraditi poruku bez obavještavanja IoT koncentratora. U ovom slučaju poruke automatski prijelaz s **nevidljivi** stanja u stanje **Enqueued** nakon *vremenskog ograničenja vidljivost (ili lock)*. Zadana vrijednost ovog vremenskog ograničenja je jedne minute.

Poruku možete prijelaz između **Enqueued** i **nevidljivi** stanja za najviše, broj naveden u svojstvu **isporuke maksimalni broj** na IoT koncentratora. Nakon taj broj prijelaza koncentrator IoT **Deadlettered**postavlja stanje poruke. Isto tako, IoT koncentrator postavlja stanje poruke da biste **Deadlettered** nakon njegova važenja (put [Live][lnk-ttl]).

Praktični vodič u oblak uređaj poruke, potražite u članku [Praktični vodič: slanju poruke oblak na uređaj s koncentrator IoT][lnk-c2d-tutorial]. Tema referenca na funkcioniranja različitih API-ji i SDK-ovi izložiti funkciju oblaka uređaj, potražite [IoT koncentrator API -ji i SDK-ovi][lnk-sdks].

> [AZURE.NOTE] Obično oblaka uređaj poruke dovršite kad god gubitak poruke ne utječe na logike aplikacije. Na primjer, sadržaj poruke je uspješno dosljedan u lokalno spremište ili operacija uspješno izvršen. Tranzitne informacije čije gubitka bi utjecati funkcionalnosti aplikacije također biti nosi poruku. Ponekad za zadatke dugoročnih mogu provoditi poruke oblaka uređaj nakon persisting opis zadatka u lokalno spremište. Zatim možete obavijestiti aplikacije pozadinskih s jednog ili više uređaja u oblak porukama u različitim fazama tijek zadatka.

### <a name="message-expiration-time-to-live"></a>Isteka poruke (vrijeme do live)

Svaku poruku oblaka uređaj ima neko vrijeme isteka. Ovaj put postavljen servis (u svojstvu **ExpiryTimeUtc** ) ili tako da IoT koncentrator pomoću zadano *vrijeme Live* naveden kao svojstvo programa IoT koncentratora. Potražite u članku [mogućnosti konfiguracije oblaka uređaj][lnk-c2d-configuration].

> [AZURE.NOTE] Slanje poruke nepovezane uređaje na uobičajeni način da biste iskoristili isteka poruke i izbjegli je da biste postavili kratko vrijeme Live vrijednosti. Taj se način postiže isti rezultat kao i održavanje stanje veze uređaja tijekom učinkovitiji. Kada zatražite poruke acknowledgements IoT koncentrator koja vas obavještava uređaja koji su primati poruke i uređaja koji nisu u mreži ili nije uspjela.

### <a name="message-feedback"></a>Povratne informacije za poruke

Kada pošaljete poruku oblaka uređaj, servis možete zatražiti isporuku po poruke povratne informacije o završno stanje poruke.

- Ako postavite svojstvo **Ack** na **pozitivne**, IoT koncentrator generira povratne informacije poruku ako i samo ako ste poruku oblaka uređaj dosegne stanje **Dovršeno** .
- Ako postavite svojstvo **Ack** na **negativne**, IoT koncentrator generira poruku povratne informacije, ako i samo ako se poruke oblaka uređaj je dostigne **Deadlettered** stanje.
- Ako postavite svojstvo **Ack** do **punog**, IoT koncentrator generira poruku povratnih informacija u oba slučaja.

> [AZURE.NOTE] Ako ** **Ack** pun**, a ne primite poruku povratnih informacija, to znači da istekle poruke povratne informacije. Servis nije moguće znali što se dogodilo s izvornu poruku. U praksi servisa potrebno je provjeriti da možete ga obraditi povratne informacije prije no što istekne. Vrijeme isteka Maksimalna je dva dana, koji omogućuje mnogo vremena da biste dobili servis ponovno pokrenuti ako dođe do pogreške.

Kao što je opisano u [krajnje točke][lnk-endpoints], IoT koncentrator nudi povratne informacije putem servisa dostupnog krajnje (**/messages/servicebound/feedback**) kao poruke. Semantiku za primanje povratnih informacija isti su kao oblak uređaj poruke, a imaju isti [životni ciklus poruke][lnk-lifecycle]. Kad god je to moguće, povratne informacije poruka je odbacivanja u jednu poruku, pomoću sljedećih oblika:

| Svojstvo | Opis |
| -------- | ----------- |
| EnqueuedTime | Vremenska oznaka koja označava stvaranja poruke. |
| ID korisnika | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

Tijelo je JSON serijalizirani polje zapisa, svaka s sljedeća svojstva:

| Svojstvo | Opis |
| -------- | ----------- |
| EnqueuedTimeUtc | Vremenska oznaka koja označava kada se dogodilo rezultat poruke. Ako, na primjer, uređaj dovršiti ili poruka istekla. |
| OriginalMessageId | **MessageId** oblaka uređaj poruku na koju se odnosi taj podatak povratne informacije. |
| Kôd statusa | Potreban cijeli broj. Koristi povratnu informaciju poruke koje generira IoT koncentratora. <br/> 0 = uspjeh <br/> 1 = poruka istječe <br/> 2 = Maksimalna isporuke broja skokova <br/> 3 = poruku odbio |
| Opis | Niz vrijednosti za **kôd statusa**. |
| DeviceId | **DeviceId** ciljni uređaj oblaka uređaj poruke na koju se odnosi ovaj dio povratne informacije. |
| DeviceGenerationId | **DeviceGenerationId** ciljni uređaj oblaka uređaj poruke na koju se odnosi ovaj dio povratne informacije. |


>[AZURE.IMPORTANT] Servis morate navesti **MessageId** poruke oblaka uređaj da biste mogli povezivanje njegov povratnih informacija s izvornu poruku.

Sljedeći primjer prikazuje tijela poruke za povratne informacije.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Mogućnosti konfiguracije oblaka uređaja

Svaki IoT koncentrator izlaže sljedeće mogućnosti konfiguracije za razmjenu poruka oblak na uređaj.

| Svojstvo | Opis | Raspon i zadane |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Zadani TTL za poruke u oblak na uređaj. | Interval ISO_8601 do 2D (minimalne 1 minute). Zadani: 1 sat. |
| maxDeliveryCount | Maksimalna isporuke broj redova oblaka uređaj po uređaji. | 1 do 100. Zadani: 10. |
| feedback.ttlAsIso8601 | Zadržavanja za povratne informacije vezane servisa poruke. | Interval ISO_8601 do 2D (minimalne 1 minute). Zadani: 1 sat. |
| feedback.maxDeliveryCount | Maksimalna isporuke broj red povratne informacije. | 1 do 100. Zadani: 100. |

Dodatne informacije potražite u članku [Stvaranje IoT koncentratora][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Čitanje poruka uređaj u oblak

Koncentrator IoT izlaže krajnje točke za usluge pozadinske čitanje poruka uređaj u oblak primio vaše središte. Krajnja točka je događaj koncentrator kompatibilan s preglednikom, koji omogućuje korištenje svih Mehanizmi koncentratora događaja servisa podržava prilikom čitanja poruke.

Kada koristite [Azure servisa Bus SDK za .NET] [ lnk-servicebus-sdk] ili [Događaj koncentratora - glavno računalo procesor događaj][lnk-eventprocessorhost], možete koristiti sve nizove koncentrator IoT veze s odgovarajuće dozvole. Koristiti **poruke/događaje** kao naziv događaja koncentratora.

Kada koristite SDK-ovi (ili integracije proizvoda) koje su svjesni od središta IoT, morate dohvatiti kompatibilan s preglednikom događaja koncentrator krajnjoj točki i naziv događaja koncentrator kompatibilnog s postavke IoT koncentrator za [Azure portal][lnk-management-portal]:

1. U koncentratoru plohu IoT kliknite **razmjena poruka**.
2. U odjeljku **Postavke uređaja u oblak** Traži sljedeće vrijednosti: **krajnje kompatibilan s preglednikom događaja koncentrator**, **naziv kompatibilan s preglednikom događaja koncentratora**i **particije**.

    ![Postavke uređaja u oblak][img-eventhubcompatible]

> [AZURE.NOTE] Ako SDK-a traži vrijednost **naziv glavnog računala** ili **prostor naziva** , uklanjanje **krajnje kompatibilan s preglednikom događaja koncentrator**shemu. Na primjer, ako je na krajnjoj točki kompatibilan s preglednikom događaja koncentrator **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, **naziv glavnog računala** bio **iothub-ns-myiothub-1234.servicebus.windows.net**i **polja naziva** bio **iothub-ns-myiothub-1234**.

Možete koristiti bilo koji zajednički pristup sigurnosna pravila koja ima **ServiceConnect** dozvole za povezivanje s navedenom koncentratora za događaj.

Ako vam je potrebna Izrada niz za povezivanje programa događaja koncentrator korištenjem prethodne informacije, koristite sljedeći uzorak:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Slijedi popis SDK-ovi i integracije koje možete koristiti s krajnje točke koncentrator kompatibilan događaja koji se izlaže koncentrator IoT:

* [Klijent Java događaj koncentratora](https://github.com/hdinsight/eventhubs-client)
* [Spout Apache oluja](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). [Spout izvora](https://github.com/apache/storm/tree/master/external/storm-eventhubs) možete vidjeti na GitHub.
* [Integracija Apache Spark](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Pregled tema:

U sljedećim temama reference sadrže dodatne informacije o razmjeni poruka s IoT koncentratora.

## <a name="message-format"></a>Oblik poruke

Koncentrator IoT poruke čine:

* Postavljanje *svojstava sustava*. Svojstva koja IoT koncentrator tumači ili postavlja. Postavljeno je unaprijed određenim.
* Postavljanje *svojstava aplikacije*. Rječnik niz svojstava koja možete definirati aplikacije i access, bez obzira na to ukloniti serijski broj u tijelu poruke. Koncentrator IoT nikad ne mijenja tih svojstava.
* Neprozirne binarni tijelo.

Dodatne informacije o kako poruka kodirana različite protokoli potražite u članku [IoT koncentrator API -ji i SDK-ovi][lnk-sdks].

U sljedećoj su tablici navedeni skup svojstva sustava u koncentratoru IoT porukama.

| Svojstvo | Opis |
| -------- | ----------- |
| MessageId | Korisnik moguće postaviti identifikator za poruku koristi za uzoraka zahtjev za odgovor. Oblik: Velika i mala slova niza (128 znakova) ASCII 7-bitnom alfanumeričke znakove + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Redni broj | Broj (jedinstveni po uređaj reda) dodijelio IoT koncentrator za svaku poruku oblak na uređaj. |
| Da biste | Odredište naveden u [Oblak uređaj] [ lnk-c2d] poruke. |
| ExpiryTimeUtc | Datum i vrijeme isteka poruke. |
| EnqueuedTime | Datum i vrijeme IoT koncentrator je primio poruku. |
| CorrelationId | Svojstva niza poruku odgovora koja obično sadrži MessageId zahtjev, u zahtjev za odgovor uzoraka. |
| ID korisnika | ID koji se koristi za određivanje polazište poruke. Kada poruka generira IoT koncentrator, je postavljen na `{iot hub name}`. |
| Ack | Generator poruke povratne informacije. Ovo svojstvo koristi u porukama oblaka uređaj da biste zatražili IoT koncentrator za generiranje povratne informacije poruke uslijed potrošnju poruke uređaj. Moguće vrijednosti: **ništa** (zadano): generira se ne prikazuje se poruka povratne informacije, **pozitivan**: primiti poruku povratne informacije, ako je poruka dovršena, **negativne**: primiti poruku povratne informacije ako istekla poruka (ili je dosegne isporuke maksimalni broj) bez dovršetak uređaja ili **cijelog**: pozitivne i negativne. Dodatne informacije potražite u članku [povratne informacije poruke][lnk-feedback]. |
| ConnectionDeviceId | ID odredio koncentrator IoT na uređaj u oblak poruke. Sadrži **deviceId** uređaja koji je poslao poruku. |
| ConnectionDeviceGenerationId | ID odredio koncentrator IoT na uređaj u oblak poruke. Sadrži **generationId** (po [Svojstva uređaja identiteta][lnk-device-properties]) uređaja koji je poslao poruku. |
| ConnectionAuthMethod | Način provjere autentičnosti odredio koncentrator IoT na uređaj u oblak poruke. Ovo svojstvo sadrži informacije o načina provjere autentičnosti provjerava autentičnost na temelju uređaj slanja poruke. Dodatne informacije potražite u članku [uređaj na cloud anti krivotvorenje][lnk-antispoofing].|

## <a name="communication-protocols"></a>Protokola za komunikaciju

Koncentrator IoT omogućuje uređaje za [MQTT][lnk-mqtt], MQTT putem WebSockets, [AMQP][lnk-amqp], AMQP nad WebSockets i HTTP protokola za komunikaciju strani uređaja. Sljedeća tablica sadrži više razine preporuke za izboru protokol:

| Protokol | Kada odaberete ovaj protokol |
| -------- | ------------------------------------ |
| MQTT <br> MQTT preko WebSocket     | Korištenje na svim uređajima koje je potrebno da biste se povezali više uređaja (svaka s vlastitom vjerodajnice po uređaj) putem iste veze za TLS. |
| AMQP <br> AMQP preko WebSocket    | Korištenje na polja i oblaka pristupnik da iskoristite prednost veze višestrukosti svim uređajima. |
| HTTP    | Koristi se za uređaje koji se ne može podržati ostalih protokola. |

Kada odaberete vaše protokol za komunikaciju uređaja strani Imajte na umu sljedeće:

* **Uzorak oblak na uređaj**. HTTP nema učinkovit način za implementaciju poslužitelja pribadače. Kao takve prilikom korištenja HTTP, uređaji ankete IoT koncentrator za oblak uređaj poruke. Ovaj pristup nije za uređaja i IoT koncentratora. U odjeljku trenutni smjernice za HTTP svakom uređaju treba ankete za poruke svakih 25 minuta ili više njih. S druge strane, MQTT i AMQP podržavaju automatske poslužitelja prilikom primanja poruka oblaka uređaj. Omogućuju odmah ih gura poruka iz koncentratora IoT na uređaju. Ako je isporuke Latencija složen, MQTT ili AMQP su najbolje protokoli za korištenje. Za rijetko povezanih uređaja HTTP kao i funkcionira.
* **Polje pristupnika**. Prilikom korištenja MQTT i HTTP, ne može povezati više uređaja (svaka s vlastitom vjerodajnice po uređaj) pomoću iste veze za TLS. [Dakle, polje pristupnika scenarijima]za[lnk-azure-gateway-guidance], te protokola su ili lošije jer trebaju nešto TLS veze između polja pristupnika i IoT koncentrator za svaki uređaj povezan s pristupnik za polje.
* **Niska resursa uređaja**. Biblioteke MQTT i HTTP imaju manje ostavlja manji trag pri od AMQP biblioteke. Kao takve, ako uređaj sadrži ograničen resursa (na primjer, manji od 1 MB RAM-a), tim protokolima mogu samo protokol implementaciju dostupna.
* **Prijelaz na mreži**. Standardni protokol AMQP koristi priključak 5671, dok MQTT očekuje podatke priključak 8883, koji može uzrokovati probleme u mrežama koje su zatvorene za-HTTP protokola. MQTT putem WebSockets AMQP nad WebSockets i HTTP dostupnih koja će se koristiti u ovom scenariju.
* **Veličina tereta**. MQTT i AMQP su binarni protokola koje rezultiraju više sažetom payloads od HTTP-a.

> [AZURE.NOTE] Prilikom korištenja HTTP svakom uređaju treba ankete za oblak uređaj poruke svakih 25 minuta ili više njih. Međutim, tijekom razvoj nije prihvatljiva ankete češće od svakih 25 minuta.

## <a name="port-numbers"></a>Brojevi priključka

Uređaje možete komunicirati s IoT koncentratora u Azure pomoću protokola za različite. Obično odabir protokol pokreću određenim potrebama rješenja. U sljedećoj su tablici navedeni izlazni priključci koji moraju biti otvoreni za uređaj da biste mogli koristiti određeni protokol:

| Protokol | Priključke |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT preko WebSockets | 443    |
| AMQP     | 5671    |
| AMQP preko WebSockets | 443    |
| HTTP     | 443     |
| LWM2M (Upravljanje uređajima) | 5684 |

Nakon stvaranja koncentratora za IoT u Azure regiji, središtu zadržava istu IP adresu za vijek koncentratora. Međutim, da biste zadržali kvaliteta servisa, ako Microsoft premješta središtu IoT jedinica drugoj veličini zatim što vam je dodijeljen novu IP adresu.

## <a name="notes-on-mqtt-support"></a>Bilješke o MQTT podrška

Koncentrator IoT implementira protokola MQTT v3.1.1 sljedeća ograničenja i određene ponašanje:

  * **QoS 2 nisu podržani**. Kada klijentsko uređaj objavljuje poruku s **QoS 2**, IoT koncentrator zatvara mrežnu vezu. Kada klijentsko uređaj pretplaćuje se na temu s **QoS 2**, koncentrator IoT daje Maksimalna QoS razine 1 u paketu **SUBACK** .
  * **Zadrži poruke zadržava**. Ako klijent uređaj objavljuje poruke zastavicom ZADRŽI postavite na 1, IoT koncentrator dodaje u **x-uključivanje-zadržati** svojstvo aplikacije na poruku. U ovom slučaju IoT koncentrator zadržava Zadrži poruke, ali umjesto prosljeđuje pozadinsku aplikaciju.

Dodatne informacije potražite u članku [IoT koncentrator MQTT podržava][lnk-devguide-mqtt].

Kao konačan važna pregledavate [Azure IoT protokol pristupnika] [ lnk-azure-protocol-gateway] koji omogućuje za implementaciju pristupnika visokih performansi prilagođene protokol koji sučelja izravno s IoT koncentratora. Protokol pristupnika Azure IoT omogućuje vam da biste prilagodili protokol uređaju kako bi odgovarao brownfield MQTT implementacijama ili drugi prilagođeni protokoli. Takvog potrebni, međutim, pokrenuti i raditi protokolom pristupnika.

## <a name="additional-reference-material"></a>Dodatne reference materijala

Druge teme referencu u vodiču za razvojne inženjere obuhvaćaju sljedeće:

- [Krajnje točke koncentrator IoT] [ lnk-endpoints] opisuju razne izlaže svaki IoT koncentrator za upravljanje i izvođenje operacija krajnje točke.
- [Ograničavanje i kvotama] [ lnk-quotas] u članku se opisuje kvote koji se odnose na servis IoT koncentratora i regulacije ponašanje očekivati prilikom korištenja servisa.
- [Koncentrator IoT uređaja i servisa SDK-ovi] [ lnk-sdks] popis različitih jezika SDK-ovi koje koristi u prilikom razvoja uređaja i servisa aplikacije kompatibilne s IoT koncentratora.
- [Upit jezik za twins, postupcima i zadacima] [ lnk-query] u članku se opisuje jezik upita možete koristiti za dohvaćanje informacija iz centra za IoT o uređaju twins, metode i zadacima.
- [Podrška IoT koncentrator MQTT] [ lnk-devguide-mqtt] navedene su dodatne informacije o podršci za IoT koncentrator za protokol MQTT.

## <a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako slanje i primanje poruka s IoT koncentrator, možda je u sljedećim temama vodič za razvojne inženjere:

- [Prijenos datoteka s uređaja][lnk-devguide-upload]
- [Upravljanja uređajima u središte IoT][lnk-devguide-identities]
- [Upravljanje pristupom koncentrator IoT][lnk-devguide-security]
- [Korištenje twins uređaj da biste sinkronizirali stanje i konfiguracija][lnk-devguide-device-twins]
- [Pozivanje metode izravno na uređaju][lnk-devguide-directmethods]
- [Zakazivanje zadataka na više uređaja][lnk-devguide-jobs]

Ako želite isprobati neke koncepta opisane u ovom članku, možda ćete zanimati i sljedeći vodiči koncentrator IoT:

- [Početak rada s Azure IoT koncentratora][lnk-getstarted-tutorial]
- [Upute za slanje poruka oblak na uređaj s IoT koncentratora][lnk-c2d-tutorial]
- [Način obrade koncentrator IoT uređaja u oblak poruke][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
