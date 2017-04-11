<properties
    pageTitle="Kako pomoću servisa Bus redova pomoću Ruby | Microsoft Azure"
    description="Saznajte kako pomoću servisa Bus redova u Azure. Primjere koda pisane Ruby."
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

# <a name="how-to-use-service-bus-queues"></a>Upute za korištenje servisa Bus redovi

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ovaj vodič opisuje način korištenja servisa Bus redova. Primjere zapisuju u Ruby i korištenje Azure gem. Scenariji u kojima je moguće uvrstiti **Stvaranje reda čekanja, slanje i primanje poruka**i **Brisanje reda čekanja**. Dodatne informacije o redovima Bus servisa u odjeljku [Sljedeće korake](#next-steps) .

## <a name="what-are-service-bus-queues"></a>Što su redova Bus servis?

Servis Bus redova podržava *brokered porukama* modela komunikacije. Sa redovima, komponente distribuirane aplikacije ne izravno međusobno komunicirati; Umjesto toga mogu razmjenjivati poruke putem red koji funkcionira kao posrednik. Proizvođač poruke (pošiljatelj) ruke isključivanje poruke u red i zatim nastavlja njegov obrada.
Asinkrono, potrošača poruke (tekstnog okvira) povlači poruka iz reda čekanja i obrađuje ga. Producentu nema ćete morati pričekati odgovor od korisnik da bi se i dalje obraditi i poslati daljnje poruke. Redovi nude **prvog u, prvi Out (FIFO)** Isporuka poruke jedan ili više konkurentnog njezinoj. To jest, poruke se obično prima i obrađuje primatelje redoslijedom u kojem su dodane u redu čekanja, a svakoj poruci je prima i obrađuje potrošača samo jedne poruke.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Servisa Bus redova su općenite namjene tehnologija koje je moguće koristiti za razna scenarije:

-   Komunikacija između web- a tempiranja ulogama u [više razina Azure aplikacije](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md).
-   Komunikacije između aplikacija za lokalni i Azure hostira aplikacije u [hibridnog rješenja](service-bus-dotnet-hybrid-app-using-service-bus-relay.md).
-   Komunikacija između komponenti aplikacije raspodijeljeno radi lokalnog u različitim tvrtke ili ustanove ili odjela tvrtke ili ustanove.

Pomoću redova možete omogućuju vam da biste skalirali out bolje aplikacija i omogućivanje više otpornost na vašem arhitektura.

## <a name="create-a-namespace"></a>Stvaranje prostor naziva

Da biste počeli koristiti servis Bus redova u Azure, prvo morate stvoriti prostor naziva. Prostor naziva nudi scoping spremnik adresiranja resursima servisa Bus unutar vaše aplikacije. Prostor naziva putem sučelja naredbenog retka potrebno je stvoriti jer Azure portal stvaranje naziva s vezom ACS.

Da biste stvorili prostor naziva:

1. Otvorite konzole za Azure Powershell.

2. Upišite sljedeću naredbu da biste stvorili prostor naziva Bus servisa. Navedite vrijednosti polja naziva i navedite isti regiju kao aplikacije.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Pribavljanje upravljanje vjerodajnica za prostor za naziv

Da bi se izvođenje operacija upravljanja, kao što su stvaranje reda u novi prostor za naziv, morate nabaviti upravljanje vjerodajnice za naziva.

Cmdlet ljuske PowerShell ste pokrenuli da biste stvorili naziva Azure service bus prikazat će se ključ možete koristiti za upravljanje naziva. Kopirajte vrijednost **DefaultKey** . U kodu će koristiti tu vrijednost u nastavku ovog praktičnog vodiča.

![Kopiranje ključ](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] Taj ključ možete pronaći i ako prijavite se na [portal za Azure](https://portal.azure.com/) i pronađite informacije o vezi za prostor za naziv Bus servisa.

## <a name="create-a-ruby-application"></a>Stvaranje Ruby aplikacije

Stvaranje Ruby aplikacije. Upute potražite u članku [Stvaranje Ruby aplikaciju na Azure](/develop/ruby/tutorials/web-app-with-linux-vm/).

## <a name="configure-your-application-to-use-service-bus"></a>Konfiguriranje aplikacija za korištenje servisa Bus

Da biste koristili Bus servisa Azure, preuzmite i pomoću značajke pakiranja Ruby Azure koje sadrži skup praktičnost biblioteke koje komunikaciju OSTALE servise za pohranu.

### <a name="use-rubygems-to-obtain-the-package"></a>Korištenje RubyGems da biste dobili pakiranje

1. Korištenje naredbenog retka sučelja kao što je **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Unix).

2. Upišite "gem Instaliraj azure" u prozoru naredbu da biste instalirali gem i ovisnosti.

### <a name="import-the-package"></a>Uvoz paketa

Korištenje omiljene uređivač, dodajte sljedeće na vrh Ruby datoteku koju namjeravate koristiti za pohranu:

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Postavljanje veze sustava Bus servisa Azure

Modul Azure čita varijable okruženja **AZURE\_SERVICEBUS\_prostor naziva** i **AZURE\_SERVICEBUS\_ACCESS_KEY** informacije potrebne za povezivanje s prostora za naziv Bus servisa. Ako te varijable okruženja su postavljena, morate navesti informacije prostor naziva prije korištenja **Azure::ServiceBusService** s sljedeći kod:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Prostor naziva vrijednost postavite na vrijednost koju ste stvorili, a ne cijeli URL. Na primjer, koristite **"yourexamplenamespace"**, ne "yourexamplenamespace.servicebus.windows.net".

## <a name="how-to-create-a-queue"></a>Stvaranje reda čekanja

Objekt **Azure::ServiceBusService** omogućuje rad sa redovima. Da biste stvorili reda, pomoću metode **create_queue()** . Sljedeći primjer stvara reda ili ispisuje sve pogreške.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Možete proslijediti i **Azure::ServiceBus::Queue** objekt s dodatnim mogućnostima, koji omogućuje vam da biste nadjačali zadane postavke reda čekanja, kao što su vrijeme poruke Live ili maksimalne reda čekanja veličina. Sljedeći primjer prikazuje način da biste postavili veličinu maksimalno reda čekanja 5GB vrijeme i Live 1 minute:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Upute za slanje poruka u redu čekanja

Da biste poslali poruku servisa Bus red aplikacije pozive u **Slanje\_reda čekanja\_message()** način **Azure::ServiceBusService** objekta. Poruka da biste (primljenim i iz) redova Bus servisa **Azure::ServiceBus::BrokeredMessage** objekti i skup standardna svojstva (kao što su **oznake** i **vrijeme\_da biste\_live**), rječnika koji koristi se za čuvanje konkretnih svojstava prilagođene aplikacije i u tijelu podataka proizvoljne aplikacije. Aplikaciju možete postaviti tijelo poruke prosljeđivanjem vrijednost niza kao poruka i potrebne standardna svojstva će popunjen zadane vrijednosti.

Sljedeći primjer pokazuje kako poslati probnu poruku red pod nazivom "test red" pomoću **Slanje\_reda čekanja\_message()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Servis Bus redova podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u redu čekanja, ali postoji u kapaciteta ukupnu veličinu poruke zaključale reda. Ovu veličinu reda čekanja definirana je u trenutku stvaranja s gornju granicu 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Kako se poruka iz reda čekanja

Poruke, isporučene su onemogućuje korištenje reda čekanja na **primanje\_reda čekanja\_message()** način **Azure::ServiceBusService** objekta. Prema zadanim postavkama, čitati i poruka zaključan ne brišu se iz reda. Međutim, možete izbrisati poruke iz reda kao čita tako da postavite na **: peek_lock** mogućnost **False**.

Zadano ponašanje omogućuje čitanje i brisanje dvije paralelne faze operacija koja također omogućuje podržava aplikacija koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da drugi korisnici ga prima sprječavanje i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete **Brisanje\_reda čekanja\_message()** način i omogućivanje poruka će se izbrisati kao parametar. Na **Brisanje\_reda čekanja\_message()** način će označavanje poruke kao se potrošena i uklanjanje iz reda.

Ako na **: uvid\_Zaključaj** je parametar postavljen na **false**, za čitanje i brisanje poruke postaje najjednostavniji modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Jer je servis Bus će označene poruke kao potrošena, kada aplikacija pokreće i započinje ponovno troše poruke, imate će ga Propušteni poruku koja je potrošena prije no što se srušiti.

Sljedeći primjer pokazuje kako dobiti i obrada poruka pomoću **primanje\_reda čekanja\_message()**. U primjeru najprije prima i briše poruke pomoću **: uvid\_Zaključaj** postavite na **false**, a zatim ga prima drugu poruku, a zatim briše poruku pomoću **Brisanje\_reda čekanja\_message()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kako rukovati ruši aplikacije i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako primatelj aplikacija ne može obraditi poruku zbog nekog razloga, a zatim ga da biste uputili poziv na **otključavanje\_reda čekanja\_message()** način **Azure::ServiceBusService** objekta. Zbog toga Bus servisa da biste otključali poruku u redu čekanja i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan u redu čekanja, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (na primjer, ako aplikacija ruši), servis Bus automatski ćete otključati poruku, a on postaje dostupan za ponovno primili.

Za slučaj da aplikacije ruši se nakon obrade poruku, ali prije na **Brisanje\_reda čekanja\_message()** zove se način, a zatim poruku će redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obradu**; to jest, svaku poruku obrađuje barem jednom, ali u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne njihovih aplikacija za rukovanje isporuka duplicirane poruke. To je često postići da pomoću na **poruke\_id** svojstvo poruka ostaje nepromijenjen preko pokušao.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove redova Bus servisa, slijedite ove veze da biste saznali više.

-   Pregled [redova, teme i pretplate](service-bus-queues-topics-subscriptions.md).
-   Posjetite [Azure SDK Ruby](https://github.com/Azure/azure-sdk-for-ruby) spremište na GitHub.

Usporedbe između redova Bus servisa Azure koji se spominju u ovom članku i Azure redova koji se spominju u članku [upute za korištenje reda čekanja za pohranu iz Ruby](../storage/storage-ruby-how-to-use-queue-storage.md) , potražite u članku [Azure redovima i Bus redovi servisa Azure – usporedbi i Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
