<properties 
    pageTitle="Kako pomoću servisa Bus redova pomoću Python | Microsoft Azure" 
    description="Saznajte kako koristiti Bus servisa Azure redova iz Python." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Upute za korištenje servisa Bus redovi

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

U ovom se članku opisuje način korištenja servisa Bus redova. Primjere zapisuju u Python i pomoću [Python Azure servisa Bus paketa][]. Scenariji u kojima je moguće uvrstiti **Stvaranje reda čekanja, slanje i primanje poruka**i **Brisanje reda čekanja**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Da biste instalirali Python ili [Python Azure servisa Bus paketa][], potražite u [Vodiču za instalaciju Python](../python-how-to-install.md).

## <a name="create-a-queue"></a>Stvaranje reda čekanja

Objekt **ServiceBusService** omogućuje rad sa redovima. Dodajte sljedeći kôd pri vrhu bilo koje datoteke Python u kojoj želite li programatski pristup Bus servisa:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

Sljedeći kod stvara **ServiceBusService** objekt. Zamjena `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` s prostor naziva, naziv ključa za potpis (SAS) zajednički pristup i vrijednost.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Vrijednosti za naziv ključa SAS i vrijednost možete pronaći podatke o vezi [Azure klasični portal][] ili u oknu Visual Studio **Svojstva** prilikom odabira naziva Bus servisa u programu Explorer Server (kao što je prikazano u prethodnom odjeljku).

```
bus_service.create_queue('taskqueue')
```

**create_queue** podržava i dodatne mogućnosti koje omogućuju vam da biste nadjačali zadane postavke reda čekanja kao što su vrijeme poruke Live (TTL) ili maksimalne reda čekanja veličina. Sljedeći primjer postavlja veličinu Maksimalna reda čekanja 5GB i TTL vrijednost 1 minutu:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Slanje poruke u red

Da biste poslali poruku servisa Bus red aplikacije pozive u **Slanje\_reda čekanja\_poruke** način **ServiceBusService** objekta.

Sljedeći primjer pokazuje kako poslati probnu poruku red pod nazivom *taskqueue pomoću* **Slanje\_reda čekanja\_poruke**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Servis Bus redova podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u redu čekanja, ali postoji u kapaciteta ukupnu veličinu poruke zaključale reda. Ovu veličinu reda čekanja definirana je u trenutku stvaranja s gornju granicu 5 GB. Dodatne informacije o kvotama potražite u članku [kvota Bus servisa][].

## <a name="receive-messages-from-a-queue"></a>Primanje poruka iz reda čekanja

Poruke, isporučene su onemogućuje korištenje reda čekanja na **primanje\_reda čekanja\_poruke** način **ServiceBusService** objekta:

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Poruke se brišu iz reda kao kada čita parametar **uvid\_Zaključaj** postavljen na **False**. Možete pročitati (uvid) i zaključavanje poruke bez brisanja iz reda čekanja postavljanjem parametar **uvid\_Zaključaj** na **True**.

Ponašanje čitanja i brisanje poruke u sklopu postupka primanje je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Budući da Bus servisa će ste označili poruku kao potrošena, a zatim kada aplikacija pokreće i započinje ponovno troše poruke, ga će imati Propušteni poruku koja je potrošena prije no što se srušiti.

Ako na **uvid\_Zaključaj** je parametar postavljen na **True**, na primanje postaje dvije faze postupak koji omogućuje za podršku aplikacije koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da drugi korisnici ga prima sprječavanje i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete način za **Brisanje** **poruke** objekta. Način za **Brisanje** će označavanje poruke kao što je u tijeku potrošena i uklanjanje iz reda.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kako rukovati ruši aplikacije i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako je primatelj aplikacija nije moguće obraditi poruku zbog nekog razloga, pa je možete nazvati metodu **otključavanje** na objektu **poruke** . Time će Bus servisa da biste otključali poruku u redu čekanja i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan u redu čekanja, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (npr., ako ruši se aplikacija), a zatim servis Bus će automatski otključati poruku i učiniti je dostupnom koji se dobiva ponovno.

Za slučaj da aplikacije zatvara se nakon obrade poruku, ali prije nego što se zove se način za **Brisanje** , zatim poruku će biti redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obrade**, odnosno svake poruke obradit će se barem jednom no u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne njihovih aplikacija za rukovanje isporuka duplicirane poruke. To je često postići pomoću **MessageId** svojstva poruke, koju će ostaju iste preko pokušao.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove redova Bus servisa, slijedite ove veze da biste saznali više.

-   Potražite u članku [redova, teme i pretplate][].

[Azure klasični portal]: https://manage.windowsazure.com
[Python Azure servisa Bus paketa]: https://pypi.python.org/pypi/azure-servicebus  
[Redova, teme i pretplate]: service-bus-queues-topics-subscriptions.md
[Servis Bus kvote]: service-bus-quotas.md
 
