<properties 
    pageTitle="Servis Bus brokered Praktični vodič za razmjenu .NET | Microsoft Azure"
    description="Brokered razmjenu .NET Praktični vodič."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Praktični vodič za .NET razmjenu poruka servisa Bus brokered

Azure Bus servis nudi dva potpun rješenja – onaj koji se putem servisa središnje "preusmjeravanja" izvodi u oblak koji podržava raznih protokola za različite prijenosa i web-usluge standarde, uključujući SOAP, WS-*, a OSTALE. Klijent nije potrebno izravne veze sa servisom lokalne ni ne ga morate znati gdje se nalazi servis, a lokalnog servisa nije potrebna nikakve ulaznog priključke otvorena na vatrozida.

Drugi razmjenu rješenja omogućuje "brokered" Mogućnosti razmjene poruka. To možete mislili od kao asinkronog ili samostalne razmjenu značajke koje podršku objavljivanja-pretplate, vremenski decoupling i pomoću servisa Bus infrastruktura za razmjenu za ujednačavanje opterećenja. Samostalne komunikacije ima mnoge prednosti; Ako, na primjer, klijenata i poslužitelja možete povezati po potrebi i izvođenje njihove operacija asinkronog način.

Pomoću ovog praktičnog vodiča namijenjen pregled i stjecanja iskustva sa redovima, neke komponente core Bus servisa brokered poruka. Kada se kroz slijed teme pomoću ovog praktičnog vodiča, imat ćete aplikaciju koja popunjava popis poruka, stvara red i šalje poruke te red. Na kraju, aplikacija prima i prikazuje poruke iz reda čekanja, a zatim čisti njegov resurse i izlaz iz. Odgovarajući vodič koji opisuje način stvaranja aplikacija koja koristi Bus preusmjeravanja servisa, potražite u članku [servis Bus povezivati razmjenu vodič](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Uvod i preduvjeti

Redovi nude prvog u odjeljku Isporuka poruke prvi Out (FIFO) jedan ili više konkurentnog njezinoj. FIFO znači da poruka obično se očekuje da biste dobili i obradili primatelje vremenski redoslijedom u kojem su enqueued i svake poruke bit će primiti i obradili potrošača samo jednu poruku. Ključne prednosti korištenja redova je da biste postigli *vremenski decoupling* komponenti aplikacije: drugim riječima, proizvođača i korisnici ne moraju biti poruke slati i primati u isto vrijeme jer poruke pohranjuju durably u redu čekanja. Srodni prednost je *Učitavanje ujednačavanje*, čime se omogućuje proizvođača i korisnici za slanje i primanje poruka prema različitim stopama..

Slijede neki administratora i pripremni korake morate slijediti prije početka vodič. Prvi je da biste stvorili polje naziva servisa, a da biste dobili ključa za potpis (SAS) zajednički pristup. Prostor naziva nudi programa granicu aplikacije za svaku aplikaciju vidljiva kroz Bus servisa. Ključ SAS automatski se generira sustav stvaranja polje naziva servisa. Polje naziva servisa i SAS ključ nudi vjerodajnica kojima servis Bus potvrđuje pristup aplikaciji.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Stvorite polje naziva servisa i dobiti ključ SAS

U prvi je korak da biste stvorili polje naziva servisa, a da biste dobili ključa za [Potpis za zajedničko korištenje programa Access](service-bus-sas-overview.md) (SAS). Prostor naziva nudi programa granicu aplikacije za svaku aplikaciju vidljiva kroz Bus servisa. Ključ SAS automatski se generira sustav stvaranja polje naziva servisa. Polje naziva servisa i SAS ključ nudi vjerodajnica za Bus servis za provjeru autentičnosti pristup aplikaciji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Sljedeći je korak stvaranje projekta za Visual Studio i pisanje dvije funkcije Pomoćnik za učitavanje razdvojen zarezom popis poruka u objekt .NET [popis](https://msdn.microsoft.com/library/6sh2ey19.aspx) svakako upisali [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .

### <a name="create-a-visual-studio-project"></a>Stvaranje projekta za Visual Studio

1. Otvorite Visual Studio kao administrator klikom desne tipke miša na izborniku Start, a zatim kliknite **Pokreni kao administrator**.

1. Stvaranje novog projekta konzole za aplikacije. Kliknite izbornik **datoteka** i odaberite **Novo**, a zatim kliknite **projekt**. U dijaloškom okviru **Novi projekt** kliknite **Visual C#** (Ako **Visual C#** ne prikazuje, pogledajte u odjeljku **Drugih jezika**), kliknite predložak **Aplikacije konzole** i nazovite ih **QueueSample**. Koristi zadano **mjesto**. Kliknite **u redu** da biste stvorili projekta.

1. Pomoću upravitelja paketa NuGet za dodavanje biblioteka servisa Bus projekta:
    1. U pregledniku rješenja, desnom tipkom miša kliknite **QueueSample** projekta, a zatim kliknite **Upravljanje NuGet paketa**.
    2. U dijaloškom okviru **Upravljanje paketa Nuget** kliknite karticu **Pregled** i traženje **Bus servisa Azure**, a zatim kliknite **Instaliraj**.
<br />
1. U pregledniku rješenja, dvokliknite Program.cs datoteku da biste ga otvorili u uređivaču za Visual Studio. Promjena polja naziva iz njegova zadani naziv `QueueSample` da biste `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Izmjena na `using` naredbe kao što je prikazano u sljedećem kodu.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Stvoriti tekstnu datoteku pod nazivom Data.csv i kopiju u sljedeći tekst razdvojen zarezom.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Spremite i zatvorite datoteku Data.csv i Imajte na umu mjesto u kojem ste je spremili.

1. U pregledniku rješenja, desnom tipkom miša kliknite naziv Projektna (u ovom primjeru **QueueSample**), kliknite **Dodaj**, a zatim kliknite **Postojeću stavku**.

1. Dođite do datoteke Data.csv koji ste stvorili u koraku 6. Kliknite datoteku, a zatim kliknite **Dodaj**. Pobrinite se da * *sve datoteke (*.*) ** odabran na popisu vrsta datoteka.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Stvaranje onaj koji raščlanjuje popis poruka

1. U na `Program` klase prije na `Main()` metoda deklarirati dvije varijable: jednu vrstu **tablica podataka**, tako da sadrže popis poruka u Data.csv. Drugi mora biti vrste popis objekata, svakako ste upisali [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Drugu mogućnost je popis brokered poruke koje će koristiti sljedeće korake u ovom praktičnom vodiču.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Izvan `Main()`, definirajte na `ParseCSV()` način koji raščlanjuje na popisu poruka u Data.csv učita poruke u tablicu [tablica podataka](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) , kao što je prikazano ovdje. Način vraća objekt **tablica podataka** .

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. U na `Main()` način, dodajte naredbu koja se poziva na `ParseCSVFile()` metoda:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Stvaranje metode koja učitava na popisu poruka

1. Izvan `Main()`, definirajte na `GenerateMessages()` način koji vodi objekt **tablica podataka** koji je vratio `ParseCSVFile()` i učitava tablice u popis svakako upisali brokered poruka. Način vraća **popis** objekata kao u sljedećem primjeru. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. U `Main()`, odmah nakon poziv `ParseCSVFile()`, dodajte naredbu koja se poziva na `GenerateMessages()` način povratnu vrijednost iz `ParseCSVFile()` kao argument:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Nabavite korisničke vjerodajnice

1. Prvo, stvorite tri varijable globalni niza na čekanju te vrijednosti. Deklariranje tih varijabli odmah nakon prethodne varijable deklaracija; Ako, na primjer:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Nakon toga stvorite funkciju koja prihvaća i pohranjuje polje naziva servisa i SAS ključ. Dodajte ovu metodu izvan `Main()`. Ako, na primjer: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. U `Main()`, odmah nakon poziv `GenerateMessages()`, dodajte naredbu koja se poziva na `CollectUserInput()` metoda:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Stvaranje rješenja

Na izborniku **Sastavljanje** u Visual Studio kliknite **Stvaranje rješenja** ili pritisnite **Ctrl + Shift + B** da biste potvrdili točnost svoje poslovne dosad.

## <a name="create-management-credentials"></a>Stvaranje vjerodajnica za upravljanje

U ovom ćete koraku definirati operacija upravljanja koju ćete koristiti za stvaranje zajedničkog pristupa vjerodajnica za potpis (SAS) koji će biti ovlašteni aplikacije.

1. Za prikaz, pomoću ovog praktičnog vodiča postavlja sve operacije reda čekanja u zaseban način. Stvaranje programa asinkrone `Queue()` metoda u na `Program` klase, nakon što instalirate na `Main()` način. Ako, na primjer:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Sljedeći je korak da biste stvorili SAS vjerodajnica pomoću [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) objekta. Način stvaranja uzima naziv ključa SAS i vrijednost nabavili u na `CollectUserInput()` način. Dodati sljedeći kod da biste na `Queue()` metoda:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Stvorite novi objekt upravljanja prostora za naziv, s URI koja sadrži polja naziva i vjerodajnice za upravljanje nabavili u prethodnom koraku kao argumente. Dodajte kod odmah nakon koda dodali u prethodnom koraku. Provjerite je li da biste zamijenili `<yourNamespace>` pod nazivom prostora za naziv servisa:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Primjer

U ovom trenutku kod trebao bi izgledati otprilike ovako:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Slanje poruka u redu čekanja

U ovom ćete koraku stvaranje reda, zatim slati poruke koje se nalaze na popisu brokered poruke taj red.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Stvaranje reda čekanja i slanje poruka u redu čekanja

1. Prvo, stvorite red. Ako, na primjer, nazovite ga `myQueue`, i deklarirati odmah nakon operacija upravljanja koji ste dodali u na `Queue()` način u posljednjem koraku:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. U na `Queue()` način stvaranja razmjenu tvorničke objekta s novostvorenu servisa Bus URI kao argument. Dodajte sljedeći kod odmah nakon operacija upravljanja koji ste dodali u posljednjem koraku. Provjerite je li da biste zamijenili `<yourNamespace>` pod nazivom prostora za naziv servisa:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Nakon toga stvorite objekt reda čekanja pomoću klase [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Dodajte sljedeći kôd neposredno iza kod koji ste dodali u posljednjem koraku:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Nakon toga dodati kod koji petlje po popisu brokered poruke koju ste stvorili ranije, slanje svaki red čekanja. Dodati sljedeći kod izravno nakon na `CreateQueueClient()` izjava u prethodnom koraku:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Primanje poruka iz reda čekanja

U ovom ćete koraku dobiti na popisu poruka iz reda koju ste stvorili u prethodnom koraku.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Stvaranje s tekstnog okvira i primanje poruka iz reda čekanja

U na `Queue()` metoda iteracija putem reda čekanja i primanje poruka metodom [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) ispis svaku poruku na konzolu. Odmah nakon kod koji ste dodali u prethodnom koraku, dodajte sljedeći kod:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Imajte na umu da se `Thread.Sleep` koristi se samo u programu Publisher poruku i obrada vjerojatno ne ispis u real razmjenu aplikacije.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Završili metodu reda čekanja i čišćenja resursi

Odmah nakon prethodne kod, dodajte sljedeći kod da biste očistili poruke tvorničke i reda čekanja resurse:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Pozvati metodu reda čekanja

Posljednji korak je da biste dodali naredbu koja se poziva na `Queue()` način iz `Main()`. Dodajte sljedeći istaknuti redak koda na kraju Main():
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Primjer

Sljedeći kod sadrži dovršeno **QueueSample** aplikacije.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Stvaranje i pokretanje aplikacije QueueSample

Sad kad ste dovršili prethodnih koraka, možete izraditi i pokrenuti aplikaciju **QueueSample** .

### <a name="build-the-queuesample-application"></a>Sastavljanje QueueSample aplikacije

U Visual Studio, na izborniku **Sastavljanje** kliknite **Stvaranje rješenja**ili pritisnite **Ctrl + Shift + B**. Ako naiđete na pogreške, provjerite kod točni na temelju dovršeno primjeru prikazane na kraju u prethodnom koraku.

## <a name="next-steps"></a>Daljnji koraci

Pomoću ovog praktičnog vodiča prikazivao sastavljanje klijent Bus servisa aplikacija i servisa pomoću mogućnosti razmjene poruka servisa Bus brokered. Slične vodič koji koristi Bus usluge [preusmjeravanja](service-bus-messaging-overview.md#Relayed-messaging), potražite u članku [servis Bus povezivati razmjenu vodič](../service-bus-relay/service-bus-relay-tutorial.md).

Da biste saznali više o [Bus servisa](https://azure.microsoft.com/services/service-bus/), potražite u sljedećim temama.

- [Bus usluge SMS pregled](service-bus-messaging-overview.md)
- [Osnove Bus servisa](service-bus-fundamentals-hybrid-solutions.md)
- [Servis Bus arhitekture](service-bus-architecture.md)

