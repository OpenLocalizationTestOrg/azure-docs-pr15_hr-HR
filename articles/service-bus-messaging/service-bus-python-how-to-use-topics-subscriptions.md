<properties 
    pageTitle="Kako pomoću servisa Bus teme pomoću Python | Microsoft Azure" 
    description="Saznajte kako koristiti Bus servisa Azure teme i pretplate s Python." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Upute za korištenje servisa Bus teme i pretplate

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

U ovom se članku opisuje način korištenja servisa Bus teme i pretplate. Primjere zapisuju u Python i pomoću [Python Azure paketa][]. Scenariji u kojima je moguće uvrstiti **Stvaranje teme i pretplate**, **Stvaranje filtara za pretplatu**, **slanje poruka na temu**, **dobivate poruku iz pretplate**i **Brisanje teme i pretplate**. Dodatne informacije o temama i pretplate u odjeljku [Sljedeće korake](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Bilješke:** Ako je potrebno instalirati Python ili [Python Azure paketa][]potražite u [Vodiču za instalaciju Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>Stvaranje teme

Objekt **ServiceBusService** omogućuje vam da biste radili s temama. Dodajte sljedeće pri vrhu bilo koju datoteku Python u kojoj želite li programatski pristup Bus servisa:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Sljedeći kod stvara **ServiceBusService** objekt. Zamjena `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` s vašeg stvarni prostor naziva zajednički pristup potpis (SAS) ključa naziv i ključne vrijednost.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Vrijednosti za naziv ključa SAS i vrijednost možete dobiti od [Azure portal][].

```
bus_service.create_topic('mytopic')
```

**Stvaranje\_temu** podržava i dodatne mogućnosti koje omogućuju vam da biste nadjačali zadane postavke tema kao što su vrijeme poruke Live ili veličine maksimalno temu. Sljedeći primjer postavlja veličinu maksimalno tema 5 GB i jedan Live (TTL) vrijednost 1 minute:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Stvaranje pretplate

Pretplata na temu također se stvaraju s objektom **ServiceBusService** . Pretplate se nazivaju i može imati neobavezno filtar koji ograničava skup poruka isporučena virtualne red svoje pretplate.

> [AZURE.NOTE] Pretplate se trajni i će i dalje postoji dok ih ili temu na koji su pretplaćeni, brišu se.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Stvaranje pretplate s filtrom zadani (MatchAll)

Filtar **MatchAll** je zadani filtar koji se koristi ako nema filtra nije naveden stvaranja novu pretplatu. Kada se koristi filtar **MatchAll** , sve poruke koje se objavljuju u temi spremaju se u svoje pretplate virtualne red. Sljedeći primjer stvara pretplatu pod nazivom "AllMessages" i koristi zadani **MatchAll** filtar.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Stvaranje pretplate s filtrima

Možete definirati i filtre koje vam omogućuje da odredite treba poruke poslane na temu prikazuju se u određenoj temi pretplate.

Većina fleksibilne vrstu filtra podržava pretplate je u **SqlFilter**, koji implementira podskup SQL92. Filtri SQL rade na svojstva poruke koje su objavljene na temu. Dodatne informacije o izrazima koje je moguće koristiti s filtrom SQL potražite u članku sintaksa [SqlFilter.SqlExpression][] .

Možete dodati i filtre za pretplatu pomoću na **Stvaranje\_pravilo** metode objekta **ServiceBusService** . Ovaj postupak omogućuje dodavanje nove filtre na postojeću pretplatu.

> [AZURE.NOTE] Budući da zadani filtar automatski će se primijeniti na sve nove pretplate, najprije morate ukloniti zadani filtar ili **MatchAll** nadjačat će sve filtre možete navesti. Zadano pravilo možete ukloniti pomoću na **Brisanje\_pravilo** metode objekta **ServiceBusService** .

Sljedeći primjer stvara pretplatu pod nazivom `HighMessages` s **SqlFilter** koji samo odabire poruke koje sadrže prilagođene **messagenumber** svojstvo veći od 3:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Isto tako, sljedeći primjer stvara pretplatu pod nazivom `LowMessages` s **SqlFilter** koji samo odabire poruke koje sadrže **messagenumber** svojstvo manje od ili jednako 3:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Sada, kada poruka je poslana `mytopic` uvijek isporučuju primatelje pretplaćeni na temu pretplate **AllMessages** i selektivno isporučuju primatelje pretplaćeni na temu pretplatama **HighMessages** i **LowMessages** (ovisno o sadržaj poruke).

## <a name="send-messages-to-a-topic"></a>Slanje poruka teme

Da biste poslali poruku teme Bus servisa, morate koristiti aplikaciju na **Slanje\_temu\_poruke** metode objekta **ServiceBusService** .

Sljedeći primjer pokazuje kako poslati pet testiranje poruke `mytopic`. Imajte na umu da se mijenja se svojstva vrijednost **messagenumber** svake poruke na iteracije petlje (Time se određuje koje dobio):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Servis Bus teme podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u temi, ali postoji u kapaciteta ukupnu veličinu poruke zaključale temu. Ovu temu veličinu definirana je u trenutku stvaranja s gornju granicu 5 GB. Dodatne informacije o kvotama potražite u članku [kvota Bus servisa][].

## <a name="receive-messages-from-a-subscription"></a>Primanje poruka iz pretplate

Poruke, isporučene su iz pretplata pomoću na **primanje\_pretplate\_poruke** način **ServiceBusService** objekta:

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Poruke se brišu iz pretplate kao kada čita parametar **uvid\_Zaključaj** postavljen na **False**. Možete pročitati (uvid) i zaključavanje poruke bez brisanja iz reda čekanja postavljanjem parametar **uvid\_Zaključaj** na **True**.

Ponašanje čitanja i brisanje poruke u sklopu postupka primanje je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Budući da Bus servisa će ste označili poruku kao potrošena, a zatim kada aplikacija pokreće i započinje ponovno troše poruke, ga će imati Propušteni poruku koja je potrošena prije no što se srušiti.

Ako na **uvid\_Zaključaj** je parametar postavljen na **True**, na primanje postaje dvije faze postupak koji omogućuje za podršku aplikacije koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da drugi korisnici ga prima sprječavanje i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete način za **Brisanje** **poruke** objekta. Način za **Brisanje** označava da je poruka kao što je u tijeku potrošena i uklanja iz pretplate.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kako rukovati ruši aplikacije i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako je primatelj aplikacija nije moguće obraditi poruku zbog nekog razloga, pa je možete nazvati metodu **otključavanje** na objektu **poruke** . Time će Bus servisa da biste otključali poruku unutar pretplate i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan unutar pretplate, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (na primjer, ako aplikacija ruši), servis Bus automatski ćete otključati poruku, a on postaje dostupan za ponovno primili.

Za slučaj da aplikacije zatvara se nakon obrade poruku, ali prije nego što se zove se način za **Brisanje** , zatim poruku će biti redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obrade**, odnosno svake poruke obradit će se barem jednom no u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne njihovih aplikacija za rukovanje isporuka duplicirane poruke. To je često postići pomoću **MessageId** svojstva poruke, koju će ostaju iste preko pokušao.

## <a name="delete-topics-and-subscriptions"></a>Brisanje teme i pretplate

Teme i pretplate stalni i izričito daljnja putem [portala za Azure][] ili programski. Sljedeći primjer prikazuje način da biste izbrisali temi pod nazivom `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Brisanje teme briše i sve pretplate registrirane s temom. Pretplate se brišu i zasebno. Sljedeći kod prikazuje kako izbrisati pretplatu pod nazivom `HighMessages` iz na `mytopic` tema:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Bus teme, slijedite ove veze da biste saznali više.

-   Potražite u članku [redova, teme i pretplate][].
-   Vodič za [SqlFilter.SqlExpression][].

[Portal za Azure]: https://portal.azure.com
[Paket Python Azure]: https://pypi.python.org/pypi/azure  
[Redova, teme i pretplate]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Servis Bus kvote]: service-bus-quotas.md 
