<properties
    pageTitle="Upute za korištenje servisa Bus teme (Ruby) | Microsoft Azure"
    description="Saznajte kako pomoću servisa Bus teme i pretplata u Azure. Primjere koda zapisuju Ruby aplikacijama."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Upute za korištenje servisa Bus teme/pretplate

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

U ovom se članku opisuje način korištenja servisa Bus teme i pretplate iz Ruby aplikacija. Scenariji u kojima je moguće uvrstiti **Stvaranje teme i pretplate, stvaranje filtre za pretplatu, slanje poruka** na temu, **dobivate poruku iz pretplate**i **Brisanje teme i pretplate**. Dodatne informacije o temama i pretplate potražite u odjeljku [Sljedeće korake](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Servis Bus teme i pretplate

Servis Bus teme i pretplata podržava na *objavljivanja/pretplate* poruka komunikacije modela. Prilikom korištenja teme i pretplate, komponente distribuirane aplikacije komunikaciju izravno međusobno; Umjesto toga ih sustava exchange poruka putem teme, koja služi kao posrednik.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Za razliku od servisa Bus redove, gdje svaki poruku obrađuje jedan korisnik, teme i pretplate nude obrazac **jedan-prema-više** komunikacije, pomoću uzorak Objavi/pretplata. Nije moguće registrirati višestruke pretplate na temu. Prilikom slanja poruke na temu ga pa postane dostupna za svaku pretplatu za obradu zasebno.

Pretplata na temu nalikuje virtualne reda čekanja koja prima kopije poruka koje su poslane na temu. Po želji možete registrirati filtar pravila za temu na temelju po pretplatu, što vam omogućuje da filtar/ograničiti primiti koje se poruke na temu koja tema pretplate.

Servis Bus teme i pretplata omogućuju mjerilo za obradu velik broj poruka preko velikom broju korisnika i aplikacije.

## <a name="create-a-namespace"></a>Stvaranje prostor naziva

Da biste počeli koristiti servis Bus redova u Azure, prvo morate stvoriti prostor naziva. Prostor naziva nudi scoping spremnik adresiranja resursima servisa Bus unutar vaše aplikacije. Prostor naziva putem sučelja naredbenog retka potrebno je stvoriti jer [Azure portal][] stvaranje naziva s vezom ACS.

Da biste stvorili prostor naziva:

1. Otvorite prozor konzole programa Azure Powershell.

2. Upišite sljedeću naredbu da biste stvorili prostor naziva. Navedite vrijednosti polja naziva i navedite isti regiju kao aplikacije.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Stvaranje prostor naziva](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Nabavite zadani upravljanje vjerodajnice za naziva

Da bi se izvođenje operacija upravljanja, kao što su stvaranje reda u novi prostor za naziv, morate nabaviti upravljanje vjerodajnice za naziva.

Cmdlet ljuske PowerShell ste pokrenuli da biste stvorili prostor za naziv servisa Bus prikazat će se ključ možete koristiti za upravljanje naziva. Kopirajte vrijednost **DefaultKey** . U kodu će koristiti tu vrijednost u nastavku ovog praktičnog vodiča.

![Kopiranje ključ](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> Taj ključ možete pronaći i ako prijavite se na [portal za Azure][] i pronađite informacije o vezi za vaš prostor naziva.

## <a name="create-a-ruby-application"></a>Stvaranje Ruby aplikacije

Upute potražite u članku [Stvaranje Ruby aplikaciju na Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Konfiguriranje aplikacija za korištenje servisa Bus

Da biste koristili Bus servisa, preuzmite i pomoću značajke pakiranja Ruby Azure koje sadrži skup praktičnost biblioteke koje komunikaciju OSTALE servise za pohranu.

### <a name="use-rubygems-to-obtain-the-package"></a>Korištenje RubyGems da biste dobili pakiranje

1. Korištenje naredbenog retka sučelja kao što je **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Unix).

2. Upišite "gem Instaliraj azure" u prozoru naredbu da biste instalirali gem i ovisnosti.

### <a name="import-the-package"></a>Uvoz paketa

Korištenje omiljene uređivač, dodajte sljedeće na vrh Ruby datoteku u kojoj namjeravate koristiti za pohranu:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Postavljanje servisa Bus veze

Modul Azure čita varijable okruženja **AZURE\_SERVICEBUS\_prostor naziva** i **AZURE\_SERVICEBUS\_PRISTUP\_KLJUČ** informacije potrebne za povezivanje s vašeg prostora za naziv. Ako te varijable okruženja su postavljena, morate navesti informacije prostor naziva prije korištenja **Azure::ServiceBusService** s sljedeći kod:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Prostor naziva vrijednost postavite na vrijednost koju ste stvorili umjesto cijelog URL-a. Na primjer, koristite **"yourexamplenamespace"**, ne "yourexamplenamespace.servicebus.windows.net".

## <a name="create-a-topic"></a>Stvaranje teme

Objekt **Azure::ServiceBusService** omogućuje vam da biste radili s temama. Sljedeći kod stvara **Azure::ServiceBusService** objekt. Da biste stvorili temu, koristite na **Stvaranje\_topic()** način. Sljedeći primjer stvara temu ili ispisuje pogreške ako ih ima.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Možete proslijediti i **Azure::ServiceBus::Topic** objekt s dodatnim mogućnostima koje omogućuju vam da biste nadjačali zadane postavke tema kao što su vrijeme poruku ako Live ili maksimalne reda čekanja veličina. Sljedeći primjer prikazuje Postavljanje veličine Maksimalna reda čekanja 5GB vrijeme i Live 1 minute:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Stvaranje pretplate

Tema pretplate i stvaraju se s objektom **Azure::ServiceBusService** . Pretplate se nazivaju i može imati neobavezno filtar koji ograničava skup poruka isporučena virtualne red svoje pretplate.

Pretplate se trajni i će i dalje postoji do ili ih ili temi su povezani s, brišu se. Ako aplikacija sadrži logiku da biste stvorili pretplatu, je najprije provjerite postoji li već pretplatu pomoću metode amortizacije getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Stvaranje pretplate s filtrom zadani (MatchAll)

Filtar **MatchAll** je zadani filtar koji se koristi ako nema filtra nije naveden stvaranja novu pretplatu. Kada se koristi filtar **MatchAll** , sve poruke koje se objavljuju u temi spremaju se u svoje pretplate virtualne red. Sljedeći primjer stvara pretplatu pod nazivom "sve poruke" i koristi zadani **MatchAll** filtar.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Stvaranje pretplate s filtrima

Možete definirati i filtre koje vam omogućuje da odredite treba poruke poslane na temu prikazuju se unutar određenog pretplate.

Većina fleksibilne vrstu filtra podržava pretplate je u **Azure::ServiceBus::SqlFilter**, koji implementira podskup SQL92. Filtri SQL rade na svojstva poruke koje su objavljene na temu. Dodatne informacije o izrazima koje je moguće koristiti s filtrom SQL pregledajte sintaksa [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) .

Možete dodati i filtre za pretplatu pomoću na **Stvaranje\_rule()** metode objekta **Azure::ServiceBusService** . Ovaj postupak omogućuje vam da biste dodali nove filtre na postojeću pretplatu.

Budući da zadani filtar automatski će se primijeniti na sve nove pretplate, najprije morate ukloniti zadani filtar ili **MatchAll** nadjačat će sve filtre možete navesti. Zadano pravilo možete ukloniti pomoću na **Brisanje\_rule()** način **Azure::ServiceBusService** objekta.

Sljedeći primjer stvara pretplatu pod nazivom "najviša-poruka" s **Azure::ServiceBus::SqlFilter** koji samo odabire poruke koje sadrže prilagođene **poruke\_broj** svojstvo veći od 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Isto tako, sljedeći primjer stvara pretplatu pod nazivom "najniža-poruka" s **Azure::ServiceBus::SqlFilter** koji samo odabire poruke koje sadrže **message_number** svojstvo manje od ili jednako 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Prilikom slanja poruke da bi se sada "test temu" je uvijek biti isporučeno za primatelje pretplaćeni na pretplatu "sve poruke" tema, a selektivno isporučuju primatelje pretplaćeni na "najviša-poruka" i "nisko poruka" tema pretplate (ovisno o tome sadržaj poruke).

## <a name="send-messages-to-a-topic"></a>Slanje poruka teme

Da biste poslali poruku teme Bus servisa, morate koristiti aplikaciju na **Slanje\_temu\_message()** način **Azure::ServiceBusService** objekta. Poruke poslane na servis Bus teme su pojavljivanja **Azure::ServiceBus::BrokeredMessage** objekte. Objekti **Azure::ServiceBus::BrokeredMessage** sadrže skup standardna svojstva (kao što su **oznake** i **vrijeme\_da biste\_live**), rječnika koji koristi se za čuvanje konkretnih svojstava prilagođene aplikacije i u tijelu niza podataka. Aplikaciju možete postaviti tijelo poruke prosljeđivanjem vrijednost niza da biste na **Slanje\_temu\_message()** način i bilo koji su potrebni standardna svojstva će biti popunjena zadane vrijednosti.

Sljedeći primjer pokazuje kako poslati pet testiranje poruka "test tema". Imajte na umu da vrijednost prilagođeno svojstvo **message_number** svake poruke mijenja se na iteracije petlje (Time se određuje pretplati prima ga):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Servis Bus teme podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u temi, ali postoji u kapaciteta ukupnu veličinu poruke zaključale temu. Ovu temu veličinu definirana je u trenutku stvaranja s gornju granicu 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Primanje poruka iz pretplate

Poruke, isporučene su iz pretplata pomoću na **primanje\_pretplate\_message()** način **Azure::ServiceBusService** objekta. Prema zadanim postavkama, read(peak) i poruka zaključan bez brisanja iz pretplate. Mogu čitati i izbrisali poruku iz pretplate tako da postavite na **uvid\_Zaključaj** mogućnost **False**.

Zadano ponašanje omogućuje čitanje i brisanje dvije paralelne faze operacija koja također omogućuje podržava aplikacija koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da drugi korisnici ga prima sprječavanje i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete **Brisanje\_pretplate\_message()** način i omogućivanje poruka će se izbrisati kao parametar. Na **Brisanje\_pretplate\_message()** način će označavanje poruke kao se potrošena i ukloniti iz pretplate.

Ako na **: uvid\_Zaključaj** je parametar postavljen na **false**, za čitanje i brisanje poruke postaje najjednostavniji modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Budući da Bus servisa će ste označili poruku kao potrošena, a zatim kada aplikacija pokreće i započinje ponovno troše poruke, ga će imati Propušteni poruku koja je potrošena prije no što se srušiti.

Sljedeći primjer pokazuje kako se mogu primati poruke i obrađeni pomoću **primanje\_pretplate\_message()**. U primjeru najprije prima i briše poruke iz pretplate "nisko poruka" pomoću **: uvid\_Zaključaj** postavite na **false**, a zatim ga prima drugu poruku od "najviša-poruka", a zatim briše poruku pomoću **Brisanje\_pretplate\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Držač za aplikaciju ruši i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako primatelj aplikacija ne može obraditi poruku zbog nekog razloga, a zatim ga da biste uputili poziv na **otključavanje\_pretplate\_message()** način **Azure::ServiceBusService** objekta. Zbog toga Bus servisa da biste otključali poruku unutar pretplate i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan unutar pretplate, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (na primjer, ako ruši se aplikacija), a zatim servis Bus će automatski otključati poruku i učiniti je dostupnom koji se dobiva ponovno.

Za slučaj da aplikacije ruši se nakon obrade poruku, ali prije na **Brisanje\_pretplate\_message()** zove se način, a zatim poruku redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obradu**; to jest, svaku poruku obradit će se najmanje jedanput, ali u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne njihovih aplikacija za rukovanje isporuka duplicirane poruke. Ovaj logike je često postići pomoću na **poruke\_id** svojstva poruke, koju će ostaju iste preko pokušao.

## <a name="delete-topics-and-subscriptions"></a>Brisanje teme i pretplate

Teme i pretplate stalni i izričito daljnja putem [portala za Azure][] ili programski. Primjeru pokazuje kako izbrisati temi pod nazivom "test tema".

```
azure_service_bus_service.delete_topic("test-topic")
```

Brisanje teme briše i sve pretplate registrirane s temom. Pretplate se brišu i zasebno. Sljedeći kod pokazuje kako izbrisati pretplate pod nazivom "najviša-poruka" s "test tema" tema:

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Bus teme, slijedite ove veze da biste saznali više.

- Potražite u članku [redova, teme i pretplate](service-bus-queues-topics-subscriptions.md).
- Pregled API-JA za [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx).
- Posjetite [Azure SDK Ruby](https://github.com/Azure/azure-sdk-for-ruby) spremište na GitHub.
 
[Portal za Azure]: https://portal.azure.com
