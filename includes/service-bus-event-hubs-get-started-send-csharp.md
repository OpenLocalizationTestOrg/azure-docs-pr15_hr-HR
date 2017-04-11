## <a name="send-messages-to-event-hubs"></a>Slanje poruke s koncentratorima događaja

U ovom odjeljku ćete pisati aplikacije konzole za Windows koja se šalje događaje koncentratora za događaj.

1. U Visual Studio, stvorite novi projekt Visual C# radnu površinu aplikacije pomoću ovog predloška **Aplikacije konzole za** projekt. Naziv projekta **pošiljatelja**.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. U pregledniku rješenja, desnom tipkom miša kliknite rješenje, a zatim **Upravljanje NuGet paketa rješenja**. 

3. Kliknite karticu **Pregled** , a zatim potražite `Microsoft Azure Service Bus`. Provjerite je li navedena naziv projekta (**pošiljatelj**) u okviru **verzije** . Kliknite **Instaliraj**i prihvatite uvjete korištenja. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio preuzimanja, instalira i dodaje referencu [Bus servisa Azure biblioteke NuGet paketa](https://www.nuget.org/packages/WindowsAzure.ServiceBus).

4. Dodajte sljedeće `using` naredbe pri vrhu datoteke **Program.cs** :

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Dodajte sljedeća polja klasa **Program** , zamjenjujući vrijednosti rezervirano mjesto s nazivom središtu događaj koji ste stvorili u prethodnom odjeljku i niz za povezivanje prostor naziva razinom koje ste prethodno spremili.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Dodajte na sljedeći način predmete **Program** :

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Ova metoda neprestano šalje događaja koncentratora za događaj s odgodu 200 ms.

7. Na kraju, dodajte sljedeće retke **Glavna** načina:

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
