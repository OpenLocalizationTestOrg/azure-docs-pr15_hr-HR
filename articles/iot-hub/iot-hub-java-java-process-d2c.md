<properties
    pageTitle="Obradu IoT koncentrator uređaj u oblak poruka (Java) | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič Java da biste saznali korisne uzorke da obradi koncentrator IoT uređaja u oblak poruke."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/01/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-java"></a>Praktični vodič: Način obrade koncentrator IoT uređaja u oblak poruke pomoću jezika Java

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Uvod

Azure IoT koncentrator je potpuno upravljana servis koji omogućuje pouzdanog i sigurne dvosmjerni komunikaciju između milijune IoT uređaji i aplikacije sigurnosno završetka. Druge vodiče ([Početak rada s IoT koncentratora] i [slanje poruka oblak na uređaj s koncentrator IoT][lnk-c2d]) pokazalo kako se koristi osnovni uređaj u oblak i oblaka uređaj razmjenu funkcionalnost IoT koncentratora.

Pomoću ovog praktičnog vodiča sastavlja na kôda prikazanog u ovom praktičnom vodiču za [Početak rada s IoT koncentrator] , a prikazuju se dvije skalabilni uzorke koje možete koristiti za obradu uređaj u oblak poruke:

- Pouzdan prostora za pohranu uređaj u oblak poruke u [spremište blobova platforme Azure]. Uobičajeni scenarij je analize *Hladna put* pohraniti telemetrijskih podataka u BLOB polja da biste koristili kao unos u postupke analize. Ti procesi mogu biti pokreću Alati kao što su [Tvorničke Azure podataka] ili stog [HDInsight (Hadoop)] .

- Pouzdan obrada *interaktivne* uređaj u oblak poruke. Uređaj u oblak poruke interaktivni su kada su odmah okidača za skup akcija u pozadinska aplikacija. Ako, na primjer, na uređaju možda poslati poruku alarma koji pokreće Umetanje s karata u sustavu CRM. Za razliku od toga poruke *točke podataka* jednostavno dolaziti analitički modul. Na primjer, temperatura telemetrijskih s uređaja koji će se spremiti za kasniju analizu je poruka točke podataka.

Jer IoT koncentrator izlaže [Koncentrator događaj][lnk-event-hubs]-kompatibilne krajnjoj točki Prima poruke uređaj u oblak, ovog vodiča koristi [EventProcessorHost] instance. Ova instanca:

* Pouzdano sprema poruke *točke podataka* u spremište blobova platforme Azure.
* Prosljeđuje *interaktivne* uređaj u oblak poruke programa Azure [Bus servis reda čekanja] za obradu odmah.

Bus servisa osigurava pouzdanog obrada interaktivne poruka, kao što je nudi checkpoints po poruke i vrijeme utemeljen na prozor de-dupliciranje.

> [AZURE.NOTE] **EventProcessorHost** instance je samo jedan način obrade interaktivne poruke. Druge mogućnosti obuhvaćaju [Tkanina servisa Azure] [ lnk-service-fabric] i [Analitiku strujanje Azure][lnk-stream-analytics].

Na kraju ovog praktičnog vodiča, pokrenite tri Java konzola aplikacije:

* **Simulirani uređaj**, izmijenjenu verziju aplikacije stvorene u vodič za [Početak rada s IoT koncentrator] šalje poruke uređaj u oblak točke podataka svaki drugi i interaktivnih uređaj u oblak poruke svakih 10 sekundi. Ta aplikacija koristi protokol AMQP možete komunicirati s IoT koncentratora.
* **postupak d2c poruke** koristi klase [EventProcessorHost] za dohvaćanje poruka iz krajnje kompatibilan s preglednikom događaja koncentratora. Datoteka, a zatim pouzdano pohranjivati poruke točke podataka u spremište blobova platforme Azure i prosljeđuje interaktivne poruke servisa Bus red.
* **postupak-interaktivne-poruka** poništite redovi interaktivne poruke iz servisa Bus reda.

> [AZURE.NOTE] Koncentrator IoT ima SDK podršku za više uređaja platforme i jezika, uključujući C, Java i JavaScript. Upute kako zamijeniti Simulirani uređaj pomoću ovog praktičnog vodiča fizički uređaj i kako povezati uređaji koncentratora za IoT, potražite u članku [Razvojni centar za Azure IoT].

Pomoću ovog praktičnog vodiča se izravno primjenjuju na druge načine za događaj koncentrator kompatibilan s poruke, kao što su projekti [HDInsight (Hadoop)] . Dodatne informacije potražite u članku [Vodič za razvojne inženjere Azure IoT koncentrator - uređaja na cloud].

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Potpune radne verzije vodiča za [Početak rada s IoT koncentratora] .

+ Java ODA 8. <br/> [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Java za ovog praktičnog vodiča u sustavu Windows ili Linux.

+ Maven 3.  <br/> [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Maven za ovog praktičnog vodiča u sustavu Windows ili Linux.

+ Aktivni Azure račun. <br/>Ako nemate račun, možete stvoriti [pomoću računa](https://azure.microsoft.com/free/) u samo nekoliko minuta.

Imat ćete neke osnovne znanja [Azure prostora za pohranu] i [Bus servisa Azure].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Slanje interaktivne poruka s Simulirani uređaja

U ovom ćete odjeljku izmjena Simulirani uređaj aplikaciju koju ste stvorili u vodiču [Početak rada s IoT koncentrator] za slanje poruka interaktivne uređaj u oblak središtu IoT.

1. Da biste otvorili datoteku simulated-device\src\main\java\com\mycompany\app\App.java koristite uređivač teksta. Ova datoteka sadrži kod za aplikaciju **Simulirani uređaj** koji ste stvorili u ovom praktičnom vodiču za [Početak rada s IoT koncentratora] .

2. Dodajte sljedeće ugniježđene predmet predmet **aplikacije** :

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    Klase je slična **MessageSender** predmete u programu project **Simulirani uređaja** . Samo razlike među njima da sada svojstvo **MessageId** sustav, a zatim prilagođeno svojstvo naziva **messageType**.
    Kod dodjeljuje univerzalno Jedinstveni identifikator (UUID) svojstvo **MessageId** . Bus servisa možete koristiti ovaj identifikator za uklanjanje duplikata poruke primi. Uzorak koristi to svojstvo **messageType** za razlikovanje interaktivne iz poruka točke podataka. Aplikacija prosljeđuje te podatke u svojstva poruke, umjesto u tijelu poruke tako da se događaj procesor ne morate ukloniti serijski broj poruku da biste izvršili usmjeravanje poruka.

    > [AZURE.NOTE] Nije važno da biste stvorili **MessageId** koristi da biste deaktivirali duplicirali interaktivne poruke u kodu uređaja. Povremeni mrežnu komunikaciju ili druge neuspješna može rezultirati više ponovno slanje iste poruke s uređaja. Možete koristiti i ID semantičkog poruke, kao što su raspršivanje podatkovnih polja odgovarajuće poruke, umjesto UUID.

3. Izmjena **glavni** način slanje oba interaktivne poruka i podataka točaka poruke kao što je prikazano u sljedećim koda:

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Spremite i zatvorite datoteku simulated-device\src\main\java\com\mycompany\app\App.java.

    > [AZURE.NOTE] Radi jednostavnosti, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog trebali biste provesti pravila Ponovi kao što su eksponencijalne backoff, kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara].

5. Da biste sastavili aplikacije **Simulirani uređaj** pomoću Maven, pokrenite sljedeću naredbu u naredbenom retku u mapi Simulirani uređaj:

    ```
    mvn clean package -DskipTests
    ```

## <a name="process-device-to-cloud-messages"></a>Obrada poruke uređaj u oblak

U ovom ćete odjeljku stvaranja aplikacije za Java konzola koji obrađuje uređaj u oblak poruke iz centra za IoT. Koncentrator Iot izlaže programa [koncentrator događaja]-kompatibilne krajnja točka za omogućivanje aplikacije za čitanje poruka uređaj u oblak. Pomoću ovog praktičnog vodiča koristi klase [EventProcessorHost] za obradu tih poruka u aplikaciji konzolu. Dodatne informacije o porukama postupak iz koncentratora događaja potražite u članku vodič za [Početak rada s koncentratorima događaj] .

Glavni test kada implementirati pouzdanog prostora za pohranu točke podataka poruke ili prosljeđivanje interaktivne poruka je obrada događaja ovisi potrošača poruka omogućuje checkpoints za tijek. Nadalje, da biste postigli visoke propusnost, kada čitate iz koncentratora događaj mora sadržavati checkpoints u velikim grupama. Taj se način stvara nastanka duplikata obrade za velik broj poruka, ako postoji pogreška, a zatim se vratite na prethodni kontrolne točke. U ovom ćete praktičnom vodiču pogledajte kako sinkronizirati zapisivanja Azure prostora za pohranu i windows de-dupliciranje Bus servisa s **EventProcessorHost** checkpoints.

Da biste pouzdano pisanje poruka sa spremištem Azure uzorka koristi značajka potvrdi pojedinačne bloka [bloka blob-ova][Azure Block Blobs]. Procesor događaj akumulira poruke u memoriji dok je vrijeme možete unijeti na kontrolne točke. Na primjer, nakon skupljena međuspremnik poruka je dostigne maksimalno bloka veličinu 4 MB ili pak nakon Bus servisa de-dupliciranje minutama će se doba dana. Nakon toga prije kontrolne točke, kod se obvezuje novi Blok za blob-om.

Procesor događaj koristi događaj koncentratora pomiče poruku kao blok ID-a. U ovom mehanizam omogućuje procesor događaja da biste izvršili provjeru de-dupliciranje prije nego što je pamti novi Blok za pohranu, pobrinite moguće neočekivanog između potvrđivanja blok i Kontrolna točka.

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi jedan račun za Azure prostora za pohranu za pisanje svih poruka koje dohvaća iz koncentratora IoT. Da biste odlučili ako trebate koristiti više prostora za pohranu Azure računa u rješenje, potražite u članku [Azure prostora za pohranu skalabilnost smjernice].

Aplikacija koristi značajku de-dupliciranje Bus servisa da biste izbjegli duplikate kada obrađuje interaktivne poruke. Simulirani uređaj oznaku svaku interaktivne poruku s jedinstvenim **MessageId**. U ovom id Bus servisa da biste bili sigurni da u prozoru za vrijeme navedeno de-dupliciranje bez dva poruke s istom **MessageId** isporučuju u primatelja. U ovom de-dupliciranje, zajedno s semantiku dovršetka po poruke koje ste dobili od redova Bus servisa olakšava implementirati pouzdanog obrada interaktivne poruka.

Da biste bili sigurni da ne prikazuje se poruka je ponovo poslati izvan prozora de-dupliciranje, kod sinkronizira Kontrolna točka mehanizam **EventProcessorHost** s prozorom de-dupliciranje reda čekanja Bus servisa. Ova sinkronizacije obavlja tvrtka prisilno na Kontrolna točka barem jednom svaki put kada se u prozor de-dupliciranje minutama (u ovom ćete praktičnom vodiču prozora je jedan sat).

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi jedan particioniranom servisa Bus čekanja za proces interaktivne poruke dohvaćeni iz koncentratora IoT. Dodatne informacije o korištenju servisa Bus redova da biste zadovoljava preduvjete skalabilnost vašeg rješenja potražite u dokumentaciji [Bus servisa Azure] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Dodjela resursa za račun za Azure prostora za pohranu i servisa Bus reda čekanja

Da biste koristili klase [EventProcessorHost] , imate račun za pohranu Azure da biste omogućili **EventProcessorHost** za bilježenje kontrolne točke podataka. Možete koristiti postojeći račun za Azure pohranu ili slijedite upute [O Azure prostor za pohranu] da biste stvorili novi. Zabilježite niz za povezivanje računa Azure prostora za pohranu.

> [AZURE.NOTE] Kada kopirate i zalijepite niz za povezivanje računa Azure prostora za pohranu, provjerite je li nema razmaka uključena.

Morate reda Bus servisa da biste omogućili pouzdanog obrada interaktivne poruka. Možete stvoriti reda programski s prozorom de-dupliciranje jedan sat, kao što je opisano u [kako pomoću servisa Bus redova][čekanja Bus servisa]. Osim toga, možete koristiti [Azure klasični portal][lnk-classic-portal], slijedeći ove korake:

1. U donjem lijevom kutu kliknite **Novo** . Kliknite **Aplikaciju servisa** > **Bus servisa** > **reda čekanja** > **Stvoriti prilagođene**. Unesite naziv **d2ctutorial**, odaberite područje, a pomoću postojećih polja naziva ili stvorite novi. Zabilježite polja naziva, morate ga kasnije u ovom ćete praktičnom vodiču. Na sljedećoj stranici odaberite **Omogući duplikata**pa postavite **dupliciranje doba dana povijest otkrivanje** na jedan sat. Kliknite kvačicu u donjem desnom kutu da biste spremili konfiguraciju red.

    ![Stvaranje reda u portal za Azure][30]

2. Na popisu servisa Bus redova kliknite **d2ctutorial**pa kliknite **Konfiguriraj**. Stvorite dva pravila zajednički pristup, jedan pod nazivom **Slanje** s dozvolama za **Slanje** i jednom pod nazivom **preslušali** s dozvolama za **Slušanje** . Zabilježite vrijednosti **primarnog ključa** za oba pravilnika, morate ih kasnije u ovom ćete praktičnom vodiču. Kada završite, kliknite **Spremi** pri dnu.

    ![Konfiguriranje reda Azure portalu][31]

### <a name="create-the-event-processor"></a>Stvaranje događaja procesor

U ovom ćete odjeljku stvoriti aplikaciju Java postupak porukama iz krajnje kompatibilan s preglednikom događaja koncentrator.

Prvi zadatak tako da dodate projekta Maven naziva **procesa, d2c i poruke** koje prima poruke uređaj u oblak iz krajnje kompatibilan s preglednikom događaja koncentrator IoT koncentratora i usmjerava te poruke na druge servise pozadinske.

1. U iot-java-get-rada mapu koju ste stvorili u ovom praktičnom vodiču za [Početak rada s IoT koncentrator] , stvaranje projekta Maven naziva **postupak d2c poruke** pomoću sljedeće naredbe na naredbeni redak. Imajte na umu to je jedan, dugi naredba:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Naredbeni redak, idite u novu mapu postupak d2c poruke.

3. Uređivaču teksta otvorite datoteku pom.xml u mapu postupak d2c poruka i dodajte sljedeće ovisnosti čvor **ovisnosti** . Te ovisnosti omogućuju korištenje azure eventhubs, azure eventhubs eph i azure servicebus paketa u aplikaciji za interakciju s IoT koncentratora i reda čekanja Bus servisa:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Spremite i zatvorite datoteku pom.xml.

Sljedeći zadatak je da biste dodali programa klase **ErrorNotificationHandler** u projekt.

1. Da biste stvorili datoteku process-d2c-messages\src\main\java\com\mycompany\app\ErrorNotificationHandler.java koristite uređivač teksta. Dodajte sljedeći kod prikaz poruke o pogreškama iz **EventProcesssorHost** instance datoteke:

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Spremite i zatvorite datoteku ErrorNotificationHandler.java.

Sada možete dodati predmete koji implementira sučelje **IEventProcessor** . Klase **EventProcessorHost** poziva klase obraditi uređaju u oblak poruke koje su stigle iz centra za IoT. Kod u klase implementira logike pouzdano spremanje poruka u spremniku blob i prosljeđivanje poruka interaktivne red Bus servisa.

Način **onEvents** postavlja **latestEventData** varijabla koja prati offset i slijed broj najnoviju poruku pročitati procesor ovaj događaj. Imajte na umu da je svaki procesor dužni jedna particija. Način **onEvents** zatim prima skupine poruke iz koncentratora IoT i obrađuje ih na sljedeći način: šalje interaktivne poruke red Bus servisa i dodaje porukama točke podataka u **toAppend** memorijski međuspremnik. Ako memorijski međuspremnik dosegne 4 MB ograničenja blok ili vrijeme windows de-dupliciranje minutama (jedan sat nakon zadnjeg kontrolne točke u ovom ćete praktičnom vodiču), način pokreće na kontrolne točke.

Način **AppendAndCheckPoint** najprije generira **blockId** bloka za dodavanje blob-om. Azure prostora za pohranu potreban je sve blokirali ID-a da imam metodu pads pomak s početnim nulama iste duljine. Zatim, ako blok s tim ID-om je već blob-om, metodu prebrisati je s trenutnog sadržaja međuspremnika.

> [AZURE.NOTE] Da biste pojednostavnili kod ovog praktičnog vodiča koristi jedan blob po particija ćete spremati poruke. Realni rješenja želite implementirati datoteke vodoravnim stvaranjem dodatnih datoteka nakon određenog vremena ili kada dođu određene veličine. Imajte na umu da je blobova platforme Azure bloka može sadržavati najviše 195 GB podataka.

Sljedeći zadatak je implementirati **IEventProcessor** sučelja:

1. Da biste stvorili datoteku process-d2c-messages\src\main\java\com\mycompany\app\EventProcessor.java koristite uređivač teksta.

2. Dodajte sljedeće uvozi i definicija klase EventProcessor.java datoteku. Klase **EventProcessor** implementira sučelja **IEventProcessor** koji definira ponašanje klijent koncentratora događaja:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Dodajte na sljedeće načine klase **EventProcessor** implementaciju **IEventProcessor** sučelja:

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. Dodajte sljedeće varijable predmete razinom **EventProcessor** klasa:

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Dodajte način **AppendAndCheckPoint** potpisom sljedeće **EventProcessor** klasa:

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Dodajte sljedeći kod metodu **AppendAndCheckPoint** za dohvaćanje trenutne poruke offset i slijed broj u particije:

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. U načinu **AppendAndCheckPoint** pomoću trenutnu vrijednost pomaka stvoriti instancu **BlockEntry** sljedećeg bloka za spremanje blob-om:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. U način **AppendAndCheckPoint** prijenos skup najnovija poruka na blob blok i dohvatiti trenutni popis blokova:

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. U način **AppendAndCheckPoint** stvaranje početne bloka u novi blob ili bloka dodati postojeće blob:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. Na kraju u način **AppendAndCheckPoint** stvaranje na kontrolne točke na i Priprema za spremanje sljedećeg bloka poruke:

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. U način **onEvents** dodati sljedeći kod primati poruke od koncentratora IoT krajnjoj točki i prosljeđivanje poruka interaktivne red Bus servisa. Zatim pozvati metodu **AppendAndCheckPoint** kada bloka pun ili istječe vremenskog ograničenja:

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. Na kraju u način **onEvents** dodajte inače ako uvjet WHERE da biste uputili poziv **AppendAndCheckPoint** ako vremensko ograničenje istekne dok nema poruka koji dolaze iz koncentratora IoT:

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Spremiti promjene na EventProcessor.java datoteku.

Konačni zadataka u projektu **postupak d2c poruke** tako da dodate kod **glavni** način koji instancira **EventProcessorHost** instance.

1. Da biste otvorili datoteku process-d2c-messages\src\main\java\com\mycompany\app\App.java koristite uređivač teksta.

2. Dodajte sljedeće naredbe **Uvoz** datoteke:

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Dodajte sljedeće varijabla predmete razinom predmete **aplikacije** . **{Yourstorageaccountconnectionstring}** zamijenite Azure prostora za pohranu računa niza za povezivanje ste bilješku o prethodno u odjeljku [Dodjela račun za Azure prostora za pohranu i reda Bus servisa](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. Dodajte sljedeće varijable predmete razinom predmete **aplikacije** i zamijenite **{yourservicebusnamespace}** prostora za naziv Bus servisa i **{yourservicebussendkey}** s ključem **Slanje** vašeg reda. Prethodno unijeli bilješke prostor naziva i **Slušanje** ključa vrijednosti u odjeljku [Dodjela račun za Azure prostora za pohranu i reda Bus servisa](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Dodajte sljedeće varijable klase razinom klase **aplikacije** . **{Youreventhubcompatibleendpoint}** zamijenite vrijednost kompatibilan s preglednikom događaja koncentrator krajnjoj točki. Vrijednost krajnjoj točki izgleda **njegov... prostor naziva** da bi se trebali biste ukloniti na **sb: / /** prefiks i sufiks **.servicebus.windows.net/** . **{Youreventhubcompatiblename}** zamijenite nazivom kompatibilan s preglednikom događaja koncentratora. **{Youriothubkey}** zamijenite tipku **iothubowner** . Unijeli bilješke te vrijednosti u [Stvaranje koncentratora za IoT] [ lnk-create-an-iot-hub] odjeljak praktičnog vodiča za *Početak rada s Azure IoT koncentrator za Java* :

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Izmijenite potpis **Glavna** načina na sljedeći način:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. Dodajte sljedeći kod **glavni** način upućivanje blob spremniku u kojoj su pohranjeni poruke:

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. Na **glavnom** način da biste dobili reference na servis za servis Bus dodati sljedeći kod:

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. U **glavnom** metoda konfigurirati i stvoriti instancu **EventProcessorHost** . Mogućnost **setInvokeProcessorAfterReceiveTimeout** osigurava da instancu **EventProcessorHost** poziva metodu **onEvents** u sučelju **IEventProcessor** čak i kada nema poruka za obradu. Način **onEvents** pa uvijek poziva metodu **AppendAndCheckPoint** kada istekne vremensko ograničenje.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. U **glavnom** način registrirali **IEventProcessor** implementaciju s instancom **EventProcessorHost** :

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Na kraju, dodavanje logike u **glavni** način da biste isključili instancu **EventProcessorHost** :

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Spremite i zatvorite datoteku process-d2c-messages\src\main\java\com\mycompany\app\App.java.

13. Da biste sastavili aplikacije **postupak d2c poruke** pomoću Maven, pokrenite sljedeću naredbu u naredbenom retku u mapi postupak d2c poruke:

    ```
    mvn clean package -DskipTests
    ```

## <a name="receive-interactive-messages"></a>Interaktivni poruka

U ovom ćete odjeljku pišete Java konzola aplikacija na prima interaktivne poruke iz servisa Bus reda.

Prvi zadatak tako da dodate projekta Maven naziva **postupak-interaktivne-poruka** koja prima poruke poslane na red Bus servisa s **EventProcessor** instance.

1. U iot-java-get-rada mapu koju ste stvorili u ovom praktičnom vodiču za [Početak rada s IoT koncentrator] , stvaranje projekta Maven naziva **postupak-interaktivne-poruka** pomoću sljedeće naredbe vaše naredbeni redak. Imajte na umu to je jedan, dugi naredba:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Naredbeni redak, idite u novu mapu postupak – interaktivne-poruke.

3. Uređivaču teksta otvorite datoteku pom.xml u mapu postupak-interaktivne-poruka i dodajte sljedeće ovisnost čvor **ovisnosti** . U ovom ovisnost omogućuje pomoću značajke pakiranja azure servicebus u aplikaciji za interakciju s vašeg reda Bus servisa:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Spremite i zatvorite datoteku pom.xml.

Sljedeći zadatak je za dodavanje koda za dohvaćanje poruka iz servisa Bus reda.

1. Da biste otvorili datoteku process-interactive-messages\src\main\java\com\mycompany\app\App.java koristite uređivač teksta.

2. Dodajte sljedeće `import` izjave datoteku:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. Dodajte sljedeće varijable predmete razinom predmete **aplikacije** i zamijenite **{yourservicebusnamespace}** prostora za naziv Bus servisa i **{yourservicebuslistenkey}** s ključem **preslušali** vašeg reda. Prethodno unijeli bilješke prostor naziva i **Slušanje** ključa vrijednosti u odjeljku [Dodjela račun za Azure prostora za pohranu i reda Bus servisa](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. Dodajte sljedeće ugniježđene klasa klase **aplikacije** primaju poruke iz reda čekanja:

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Izmijenite potpis **Glavna** načina na sljedeći način:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. U **glavnom** način dodati sljedeći kod da biste pokrenuli slušanje za nove poruke:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Spremite i zatvorite datoteku process-interactive-messages\src\main\java\com\mycompany\app\App.java.

8. Da biste sastavili aplikacije **postupak-interaktivne-poruka** pomoću Maven, pokrenite sljedeću naredbu u naredbenom retku u mapi postupak-interaktivne-poruka:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni pokrenuti tri aplikacije.

1.  Da biste pokrenuli aplikaciju **postupak-interaktivne-poruka** , u naredbeni redak ili ljuske dođite do mape u proces-interaktivne-poruka i pokrenite sljedeću naredbu:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Pokreni proces-interaktivne-poruka][processinteractive]

2.  Da biste pokrenuli aplikaciju **postupak d2c poruke** , u naredbeni redak ili ljuske dođite do mape u postupku d2c poruke i pokrenite sljedeću naredbu:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Pokreni proces d2c poruke][processd2c]

3.  Da biste pokrenuli aplikaciju **Simulirani uređaj** , u naredbeni redak ili ljuske dođite do mape u Simulirani uređaja i pokrenite sljedeću naredbu:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Pokretanje Simulirani uređaja][simulateddevice]

> [AZURE.NOTE] Da biste vidjeli ažuriranja u vašem blob, morate smanjiti konstanta **MAX_BLOCK_SIZE** u predmete **StoreEventProcessor** manje vrijednosti, kao što je **1024**. Budući da je potrebno neko vrijeme da se dosegne ograničenje veličine bloka s podataka koji se šalju Simulirani uređaj, korisno je tu promjenu. S manjom veličinom blok ne moraju čekati toliko dugo da biste vidjeli blob stvorili i ažurirati. Međutim, pomoću veću veličinu bloka postavlja aplikaciju više skalabilni.

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako pouzdano obraditi točke podataka i interaktivnih poruke uređaj u oblak pomoću klase [EventProcessorHost] .

[Upute za slanje poruka oblak na uređaj s IoT koncentrator] [ lnk-c2d] objašnjava slanje poruka s vašeg pozadinskih na uređajima.

Primjera dovršeno završetka do kraja rješenja koje koriste IoT koncentrator potražite [Azure IoT paket][lnk-suite].

Dodatne informacije o razvoju rješenja s IoT koncentrator potražite u članku [Vodič za razvojne inženjere koncentrator IoT].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

<!-- Links -->

[Spremište blobova platforme Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[Tvorničke Azure podataka]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Red čekanja Bus servisa]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT koncentrator za razvojne inženjere vodič – uređaj da biste u oblaku]: iot-hub-devguide-messaging.md

[Azure prostora za pohranu]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Vodič za IoT koncentrator za razvojne inženjere]: iot-hub-devguide.md
[Početak rada s IoT koncentratora]: iot-hub-java-java-getstarted.md
[Razvojni centar za Azure IoT]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Rukovanje tranzitne kvara]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[O Azure prostora za pohranu]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Početak rada s koncentratorima događaja]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Azure pohranu skalabilnost smjernice]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Rukovanje tranzitne kvara]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub