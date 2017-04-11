## <a name="send-messages-to-event-hubs"></a>Slanje poruke s koncentratorima događaja

U biblioteci Java klijenta za događaj koncentratora dostupna je za korištenje u Maven projektima [Maven središnjem spremniku](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22), a možete se pozivati pomoću sljedećih deklariranje ovisnost unutar Maven datoteku projekta:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Za različite vrste Sastavi okruženja, možete izričito nabaviti najnoviji objavljenu POSUDU datoteke u [Središnjem spremniku Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) ili od [točke raspodjele izdanje na GitHub](https://github.com/Azure/azure-event-hubs/releases).  

Za publisher na jednostavne događaj *com.microsoft.azure.eventhubs* paket za klijent klase koncentratora za događaj i uvesti *com.microsoft.azure.servicebus* paket za utility klase kao što su uobičajeni iznimke koji se zajednički koriste s klijentom razmjenu Bus servisa Azure. 

Sljedeći primjer prvo stvorite novi projekt Maven konzole/ljuske aplikacije u omiljene Java razvojno okruženje. Klasa će se pozivati ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Prostor naziva i nazivi događaj koncentrator zamijenite na vrijednosti koje se koriste prilikom stvaranja koncentratora za događaj.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Nakon toga stvorili jedinstven događaj pretvaranja niza u njegov UTF-8 bajtni šifriranje. Zatim ćemo stvoriti novu instancu klijent koncentratora događaj iz niza za povezivanje i pošaljite poruku.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
