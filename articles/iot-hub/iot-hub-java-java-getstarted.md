<properties
    pageTitle="Azure IoT koncentrator za Java Uvod | Microsoft Azure"
    description="Azure IoT koncentrator pomoću Java početak rada vodič. Pomoću Azure IoT koncentrator i Java sustava Microsoft Azure IoT SDK-ovi za implementaciju rješenja programa Internet stvari."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Početak rada s Azure IoT koncentrator za Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na kraju ovog praktičnog vodiča postoje tri Java konzola aplikacije:

* **Stvaranje-uređaj-identiteta**, čime se uređaju identitetu i pridruženi sigurnosni ključ povezati Simulirani uređaj.
* **čitanje d2c poruka**, koja se prikazuje telemetrijskih poslao Simulirani uređaj.
* **Simulirani uređaj**koji povezuje koncentratora za IoT s identitetom uređaj ste ranije stvorili i šalje poruku telemetrijskih svaki drugi pomoću protokola AMQP.

> [AZURE.NOTE] U članku [IoT koncentrator SDK-ovi] [ lnk-hub-sdks] pruža informacije o raznim SDK-ovi koje možete koristiti da biste sastavili obje aplikacije da biste pokrenuli na uređaje i pozadinskih vašeg rješenja.

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Java ODA 8. <br/> [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Java za ovog praktičnog vodiča u sustavu Windows ili Linux.

+ Maven 3.  <br/> [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Maven za ovog praktičnog vodiča u sustavu Windows ili Linux.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Kao završnom koraku zabilježite vrijednost **primarnog ključa** , a zatim **razmjena poruka**. Na plohu **porukama** zabilježite **kompatibilan s preglednikom događaja koncentrator naziv** i **kompatibilan s preglednikom događaja koncentrator krajnjoj točki**. Prilikom stvaranja aplikacije **čitanje d2c poruka** morate ove tri vrijednosti.

![Azure portala IoT koncentrator poruka plohu][6]

Sada ste stvorili koncentratora za IoT, a imate IoT koncentrator naziv glavnog računala, koncentrator IoT niz za povezivanje, IoT koncentrator primarni ključ, naziv događaja koncentrator kompatibilnog i krajnje kompatibilan s preglednikom događaja koncentratora koje morate poduzeti ovog praktičnog vodiča.

## <a name="create-a-device-identity"></a>Stvaranje uređaja identiteta

U ovom ćete odjeljku stvaranja aplikacije za Java konzola koji stvara novi uređaj identitet u registru identiteta koncentratora za IoT. Uređaj ne može povezati s IoT koncentrator osim ako ima stavku u registru identiteta uređaja. Pogledajte odjeljak **Uređaj identiteta registra** [IoT koncentrator za razvojne inženjere vodič] [ lnk-devguide-identity] dodatne informacije. Kada pokrenete ovu aplikaciju konzole, generira ID jedinstveni uređaja i ključ koji uređaju možete koristiti da biste odredili sam kada šalje poruke uređaj u oblak IoT koncentrator.

1. Stvorite novu praznu mapu pod nazivom iot-java-get-rada. U mapi iot-java-get-rada stvorite novi projekt Maven naziva **Stvaranje-uređaju-identitetu** pomoću sljedeće naredbe vaše naredbeni redak. Imajte na umu to je jedan, dugi naredba:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Naredbeni redak, idite u novu mapu stvaranje-uređaju-identitetu.

3. Uređivaču teksta otvorite datoteku pom.xml u mapu stvaranje uređaju identitetu i dodajte sljedeće ovisnost čvor **ovisnosti** . To vam omogućuje da pomoću značajke pakiranja iothub servisa sdk u aplikaciji:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Spremite i zatvorite datoteku pom.xml.

5. Korištenje uređivača teksta, otvorite datoteku u create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Dodajte sljedeće naredbe **Uvoz** datoteke:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Dodajte sljedeće varijable predmete razinom klasu **aplikacije** zamjene **{yourhubconnectionstring}** s vrijednošću vaše napomene ranije:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Izmijenite potpis **Glavna** načina da biste dodali iznimke na sljedeći način:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Dodajte sljedeći kod kao tijelo **Glavna** načina. Kod stvara se *javadevice* u registar identiteta IoT koncentrator Ako još ne postoji. Zatim prikazuje id uređaja i ključ koji vam zatreba:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Spremite i zatvorite datoteku App.java.

11. Da biste sastavili aplikacije za **Stvaranje uređaja identiteta** pomoću Maven, pokrenite sljedeću naredbu u naredbenom retku u mapi stvaranje uređaju identitetu:

    ```
    mvn clean package -DskipTests
    ```

12. Da biste pokrenuli aplikacije za **Stvaranje uređaja identiteta** pomoću Maven, pokrenite sljedeću naredbu u naredbenom retku u mapi stvaranje uređaju identitetu:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Zabilježite **id uređaja** i **ključ uređaja**. Trebat će vam ove kasnije prilikom stvaranja aplikacije koja se povezuje s IoT koncentrator kao uređaj.

> [AZURE.NOTE] Identitet registra IoT koncentrator samo pohranjuje identiteta uređaj za omogućivanje sigurnog pristupa središtu. Sprema uređaju ID-a i tipke da biste koristili kao sigurnosne vjerodajnice i omogućeno/onemogućeno Označi zastavicom koje možete koristiti da biste onemogućili pristup za pojedinačne uređaj. Ako aplikacija nije potrebno pohranjivati metapodatke druge specifične za uređaj, trebali biste koristiti u spremištu specifičnim aplikacijama. Pogledajte vodič za [razvojne inženjere koncentrator IoT] [ lnk-devguide-identity] dodatne informacije.

## <a name="receive-device-to-cloud-messages"></a>Primanje poruka uređaj u oblak

U ovom ćete odjeljku stvaranja aplikacije za Java konzola koji se čita uređaj u oblak poruke iz centra za IoT. Koncentratora za IoT izlaže [Koncentrator događaj][lnk-event-hubs-overview]-kompatibilne krajnju točku da biste mogli čitati poruke uređaj u oblak. Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča stvara čitača osnovni koja nije prikladna za implementaciju visoke propusnost. [Postupak uređaj u oblak poruke] [ lnk-process-d2c-tutorial] vodič prikazuje kako obraditi uređaju u oblak poruke na razini. [Početak rada s koncentratorima događaj] [ lnk-eventhubs-tutorial] Praktični vodič pruža dodatno informacije o porukama postupak iz koncentratora za događaj i se primjenjuju na krajnje točke kompatibilan s preglednikom događaja koncentrator IoT koncentratora.

> [AZURE.NOTE] Krajnje kompatibilan s preglednikom događaja koncentrator za čitanje poruka uređaj oblak uvijek koristi protokol AMQP.

1. U iot-java-get-rada mapu koju ste stvorili u odjeljku *Stvaranje uređaju identitetu* , stvorite novi projekt Maven naziva **čitanje d2c poruka** pomoću sljedeće naredbe na naredbeni redak. Imajte na umu to je jedan, dugi naredba:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Naredbeni redak, idite u novu mapu čitanje d2c poruka.

3. Uređivaču teksta otvorite datoteku pom.xml u mapu čitanje d2c poruka i dodajte sljedeće ovisnost čvor **ovisnosti** . To vam omogućuje da pomoću značajke pakiranja eventhubs klijenta u aplikaciji za čitanje iz krajnje kompatibilan s preglednikom događaja koncentratora:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Spremite i zatvorite datoteku pom.xml.

5. Korištenje uređivača teksta, otvorite datoteku u read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. Dodajte sljedeće naredbe **Uvoz** datoteke:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Dodajte sljedeće varijable klase razinom klase **aplikacije** . **{Youriothubkey}**, **{youreventhubcompatibleendpoint}**i **{youreventhubcompatiblename}** zamijenite vrijednosti koje ste prethodno zabilježili:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Dodajte sljedeće metode **receiveMessages** klase **aplikacije** . Ovaj postupak stvara instancu **EventHubClient** za povezivanje s krajnje kompatibilan s preglednikom događaja koncentratora i asinkrono stvara instancu **PartitionReceiver** za čitanje s particije koncentratora za događaj. Neprestano petlje i ispisuje Detalji poruke dok se ne završava aplikacije.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Ta metoda koristi filtar stvara primatelja tako da se primatelja prikazuje samo poruke poslane koncentrator IoT nakon pokretanja primatelja pokrenut. To je korisno u okruženju test da biste vidjeli trenutni skup poruke. U okruženju proizvodnje koda provjerite je li da obrađuje sve poruke – pogledajte [upute za obradu IoT koncentrator uređaj u oblak poruka] [ lnk-process-d2c-tutorial] vodiča za dodatne informacije.

9. Izmijenite potpis **Glavna** načina da biste uključili iznimku na sljedeći način:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Dodajte sljedeći kod na **glavnom** način u razredu **aplikacije** . Kod stvara dvije instance **EventHubClient** i **PartitionReceiver** i omogućuje vam da biste zatvorili aplikacija nakon što dovršite obrada poruka:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Kod pretpostavlja da ste stvorili koncentratora za IoT u sloju (besplatno) F1. Besplatni IoT koncentrator ima dvije particije pod nazivom "0" i "1".

11. Spremite i zatvorite datoteku App.java.

12. Da biste sastavili aplikacije **čitanje d2c poruka** pomoću Maven, pokrenite sljedeću naredbu u naredbenom retku u mapi čitanje d2c poruka:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Stvaranje aplikacije Simulirani uređaja

U ovom ćete odjeljku stvaranja aplikacije za Java konzola koji simulira uređaj koji šalje poruke uređaj u oblak koncentratora za IoT.

1. U iot-java-get-rada mapu koju ste stvorili u odjeljku *Stvaranje uređaju identitetu* , stvorite novi projekt Maven naziva **Simulirani uređaja** koji koristi sljedeću naredbu na naredbeni redak. Imajte na umu to je jedan, dugi naredba:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Naredbeni redak, idite u novu mapu Simulirani uređaja.

3. Uređivaču teksta otvorite datoteku pom.xml u mapu Simulirani uređaja i dodajte sljedeće ovisnosti čvor **ovisnosti** . To vam omogućuje da pomoću značajke pakiranja iothub java klijenta u aplikaciji za komunikaciju s koncentratora za IoT te serijalizirati Java objekata JSON:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Spremite i zatvorite datoteku pom.xml.

5. Korištenje uređivača teksta, otvorite datoteku u simulated-device\src\main\java\com\mycompany\app\App.java.

6. Dodajte sljedeće naredbe **Uvoz** datoteke:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Dodajte sljedeće varijable predmete razinom klasu **aplikacije** zamjena **{youriothubname}** IoT koncentrator ime i **{yourdevicekey}** vrijednošću ključa uređaj generira u sekciji *stvorite identitet uređaj* :

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    U ovom primjeru aplikacije koristi **protokol** varijabla kada je instancira **DeviceClient** objekt. Možete koristiti protokol HTTP ili AMQP možete komunicirati s IoT koncentratora.

8. Dodajte sljedeće ugniježđene funkcije **TelemetryDataPoint** predmete unutar klase **aplikacije** da biste odredili telemetrijskih podataka uređaju šalje koncentratora za IoT:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Dodajte sljedeće **EventCallback** klase ugniježđene unutar klase **aplikacije** da biste prikazali status potvrđivanje koji središtu IoT vraća kada obrađuje poruku s Simulirani uređaja. Ta metoda obavještava glavnom niti u aplikaciji kada poruka bude obrađen:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Dodajte sljedeće **MessageSender** predmete ugniježđene unutar klase **aplikacije** . Način **pokrenuti** u klase generira uzorak telemetrijskih podataka da biste poslali koncentratora za IoT i čeka se potvrđivanje prije slanja na sljedeću poruku:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Ta metoda šalje novu poruku uređaj u oblak jedne sekunde središtu IoT priznaje na prethodnu poruku. Poruka sadrži JSON serijalizirani objekt s na deviceId i slučajno generiranim brojem u programu Publisher senzora brzina vjetra.

11. Sljedeći kod koji stvara niti za slanje poruka uređaj u oblak koncentratora za IoT zamijenite **Glavna** načina:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Spremite i zatvorite datoteku App.java.

13. Da biste sastavili aplikacije **Simulirani uređaj** pomoću Maven, pokrenite sljedeću naredbu u naredbenom retku u mapi Simulirani uređaj:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog provodite pravila Ponovi (primjerice na eksponencijalnom backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara][lnk-transient-faults].

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U naredbenom retku u mapi čitanje d2c, pokrenite sljedeću naredbu da biste započeli nadzor prvi particije u koncentratora za IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Koncentrator IoT Java servisa klijentska aplikacija za praćenje poruka uređaj u oblak][7]

2. U naredbenom retku u mapi Simulirani uređaj, pokrenite sljedeću naredbu da biste započeli slanja telemetrijskih podataka koncentratora za IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Koncentrator IoT Java uređaj klijentske aplikacije za slanje poruka uređaj u oblak][8]

3. **Korištenje** pločice [Azure portal] [ lnk-portal] prikazuje se broj poruka poslana koncentrator:

    ![Azure portala korištenje pločica s prikazom broj poruke poslane IoT koncentratora][43]

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču konfiguriran novi koncentrator IoT na portalu za Azure, a zatim stvoriti uređaju identitetu u odjeljku središte identiteta registra. Koristi ovaj identitet uređaj da biste omogućili aplikaciju Simulirani uređaj da biste poslali poruke uređaj u oblak središtu. Koji ste stvorili i aplikaciju koja se prikazuje u poruke koje su stigle prema središtu. 

Da biste nastavili Uvod u koncentrator IoT i da biste istražili drugim situacijama IoT potražite u članku:

- [Povezivanje uređaja][lnk-connect-device]
- [Uvod u upravljanje uređajima][lnk-device-management]
- [Uvod u pristupnik SDK IoT][lnk-gateway-SDK]

Da biste saznali kako proširiti rješenje IoT i obraditi uređaju u oblak poruke na razini, prikazati [poruka uređaj u oblak postupak] [ lnk-process-d2c-tutorial] vodič.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/