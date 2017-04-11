<properties
    pageTitle="Slanje poruka oblak na uređaj s IoT koncentrator | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste saznali kako slanje poruka oblaka uređaj koncentrator IoT Azure pomoću Java."
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
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Praktični vodič: Način slanja poruka oblak na uređaj s IoT koncentratora i Java

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Uvod

Azure IoT koncentrator je servis za potpuno upravljane koji olakšava omogućiti pouzdanog i sigurne dvosmjerni komunikaciju između milijune IoT uređaji i završavaju aplikaciju natrag. Praktični vodič za [Početak rada s IoT koncentrator] pokazuje kako stvoriti koncentratora za IoT, Dodjela uređaju identitetu u njoj i kod Simulirani uređaj koji šalje poruke uređaj u oblak.

Pomoću ovog praktičnog vodiča sastavlja na [Početak rada s IoT koncentratora]. Kako pokazuje da biste:

- S vašeg računala oblaka pozadinskih, slanje poruka oblaka uređaj jedan uređaj putem koncentratora IoT.
- Oblak uređaj poruka na uređaju.
- S vašeg računala oblaka sigurnosno završi, zahtjev potvrđivanje isporuke (*povratne informacije*) za poruke poslane na uređaj s IoT koncentratora.

Dodatne informacije o oblaka uređaj poruke možete pronaći u [Vodič za razvojne inženjere koncentrator IoT][IoT Hub Developer Guide - C2D].

Na kraju ovog praktičnog vodiča, pokrenite dvije Java konzola aplikacije:

* **Simulirani uređaj**, izmijenjenu verziju aplikacije stvorene u [Početak rada s IoT koncentrator], koja se povezuje s koncentratora za IoT i prima poruke oblak na uređaju.
* **slati c2d poruke**koje šalje poruku oblaka uređaj Simulirani uređaja putem koncentratora IoT i prima njegov potvrđivanje isporuke.

> [AZURE.NOTE] Koncentrator IoT ima podršku SDK za mnoge uređaj platforme i jezika (uključujući C, Java i Javascript) do Azure IoT uređaj SDK-ovi. Podrobne upute o povezivanju uređaja kod ovog praktičnog vodiča i obično Azure IoT koncentrator potražite u članku [Razvojni centar za Azure IoT].

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Java ODA 8. <br/> [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Java za ovog praktičnog vodiča u sustavu Windows ili Linux.

+ Maven 3.  <br/> [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Maven za ovog praktičnog vodiča u sustavu Windows ili Linux.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

## <a name="receive-messages-on-the-simulated-device"></a>Primanje poruka na uređaju Simulirani

U ovom ćete odjeljku izmjena Simulirani uređaj aplikaciju koju ste stvorili u oblak uređaj poruka iz koncentratora IoT [Početak rada s IoT koncentratora] .

1. Korištenje uređivača teksta, otvorite datoteku u simulated-device\src\main\java\com\mycompany\app\App.java.

2. Dodajte sljedeće klase **MessageCallback** kao ugniježđene predmete unutar klase **aplikacije** . Način **za izvršavanje** se poziva kada uređaj Prima poruke iz koncentratora IoT. U ovom primjeru uređaj uvijek obavještava središtu da je dovršena poruku:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Izmjena **Glavna** načina da biste stvorili **MessageCallback** instancu i pozvati metodu **setMessageCallback** prije otvaranja klijenta na sljedeći način:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Ako koristite HTTP umjesto MQTT ili AMQP kao prijenos, instancu **DeviceClient** provjerava za poruke s IoT koncentrator diskovni (manje od svakih 25 minuta). Dodatne informacije o razlikama između MQTT, AMQP i HTTP podršci i ograničavanje IoT koncentrator potražite u članku [Vodič za razvojne inženjere koncentrator IoT][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Slanje poruke oblaka uređaja

U ovom ćete odjeljku stvaranja aplikacije za Java konzole koja šalje poruke oblaka uređaj aplikaciju Simulirani uređaja. Potreban vam je uređaj Id uređaja koji ste dodali u ovom praktičnom vodiču za [Početak rada s IoT koncentratora] . Morate niz za povezivanje za vaše IoT središte koje možete pronaći na [portal za Azure].

1. Stvaranje projekta Maven naziva **Slanje c2d poruka** pomoću sljedeće naredbe na naredbeni redak. Imajte na umu to je jedan, dugi naredba:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Naredbeni redak, idite u novu mapu za slanje c2d poruka.

3. Uređivaču teksta otvorite datoteku pom.xml u mapi za slanje c2d poruka i dodajte sljedeće ovisnost čvor **ovisnosti** . Dodavanje ovisnosti omogućuje pomoću značajke pakiranja **iothub java-servis-klijenta** u aplikaciji za komunikaciju sa servisu koncentrator za IoT:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Spremite i zatvorite datoteku pom.xml.

5. Korištenje uređivača teksta, otvorite datoteku u send-c2d-messages\src\main\java\com\mycompany\app\App.java.

6. Dodajte sljedeće naredbe **Uvoz** datoteke:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Dodajte sljedeće varijable predmete razinom klasu **aplikacije** zamjena **{yourhubconnectionstring}** i **{yourdeviceid}** s vrijednostima na navedena ranije:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. Sljedeći kod koji se povezuje s koncentratora za IoT, šalje poruku na uređaj i čeka potvrdu uređaj primili i obrađuju poruku zamijenite **Glavna** načina:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Za sake jednostavnosti, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog trebali biste provesti pravila Ponovi (kao što su eksponencijalne backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara].

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U naredbenom retku u mapi Simulirani uređaj, pokrenite sljedeću naredbu za početak slanja telemetrijskih koncentratora za IoT i da biste preslušali za oblak uređaj poruke poslane s vaše središte:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Pokrenite aplikaciju Simulirani uređaja][img-simulated-device]

2. U naredbenom retku u mapi za slanje c2d poruka, pokrenite sljedeću naredbu da biste poslali poruku oblaka uređaja i pričekajte da potvrda za povratne informacije:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Pokrenite naredbu da biste poslali poruku oblaka uređaja][img-send-command]

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako slanje i primanje poruka oblak na uređaj. 

Da biste vidjeli primjera dovršeno završetka do kraja rješenja koje koriste IoT koncentrator, potražite u članku [Azure IoT paket].

Dodatne informacije o razvoju rješenja s IoT koncentrator potražite u članku [Vodič za razvojne inženjere koncentrator IoT].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Početak rada s IoT koncentratora]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Vodič za IoT koncentrator za razvojne inženjere]: iot-hub-devguide.md
[Razvojni centar za Azure IoT]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Rukovanje tranzitne kvara]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Portal za Azure]: https://portal.azure.com
[Azure IoT paketa]: https://azure.microsoft.com/documentation/suites/iot-suite/