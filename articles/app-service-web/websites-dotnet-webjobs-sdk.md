<properties 
    pageTitle="Što je Azure WebJobs SDK" 
    description="Uvod u Azure WebJobs SDK. U članku se objašnjava SDK novosti, uobičajeni scenariji korisne su za, a kod uzorka." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Što je Azure WebJobs SDK

## <a id="overview"></a>Pregled

U ovom se članku objašnjava što je WebJobs SDK, pregledava neke uobičajeni scenariji korisne su za i daje pregled kako ga koristiti u kodu.

[WebJobs](websites-webjobs-resources.md) je značajka servisa Azure aplikacije koja omogućuje da biste pokrenuli program ili skripte u kontekstu isti kao web app, aplikacije API-JA ili mobilnoj aplikaciji. Svrha [WebJobs SDK](websites-webjobs-resources.md) je da biste pojednostavnili kod u pišete za uobičajene zadatke u WebJob možete izvršiti, kao što su obrada slike, red čekanja obrade, RSS zbrajanja, datoteka održavanja i slanje poruke e-pošte. WebJobs SDK ima ugrađene značajke za rad s Azure i prostor za pohranu servisa Bus, za zakazivanje zadataka i rukovanja pogreškama i mnoge druge uobičajeni scenariji. Osim toga, ga namijenjenu biti extensible. Na [WebJobs SDK je Otvori izvor](https://github.com/Azure/azure-webjobs-sdk/), i nema programa [otvorite spremištu izvora za nastavak](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

WebJobs SDK obuhvaća sljedeće komponente:

* **NuGet paketa**. Paketi NuGet koje dodate u projekt aplikacije konzole za Visual Studio pružaju framework koji koristi kod po Dekorativni načinima WebJobs SDK atributa.
  
* **Nadzorne ploče**. Dio WebJobs SDK je uključen u aplikacije servisa za Azure i njihovi obogaćenog nadzor i Dijagnostika za programe koji koriste NuGet paketa. Ne morate pisati kod da biste koristili te značajke za nadzor i dijagnostici.

## <a id="scenarios"></a>Scenariji

Evo nekih uobičajeni scenarija jednostavnije možete učiniti s Azure WebJobs SDK:

* Slika obradi ili nekog drugog procesora ćete morati usko rad. Zajednička značajka web-mjesta koja je mogućnost prijenos slike ili videozapise. Često želite rukovati sadržaja nakon prijenosa, ali ne želite da bi korisnik Pričekajte da se to učiniti.

* Obrada reda. Uobičajeni način za sučelju web možete komunicirati s pozadinskom servis je koristiti redova. Kada web-mjesta morate obaviti zadatke, na ga ih gura poruke na u redu. Usluge pozadinskog povlači poruke iz reda čekanja, a ne rad. Možete koristiti redovi za obradu slika:, na primjer, kada korisnik prenese broj datoteka, stavite nazive datoteka u poruci reda čekanja za izdvajanje prema gore po pozadinski za obradu. Ili redove nije moguće koristiti da biste poboljšali odziv web-mjesta. Ako, na primjer, umjesto pisanje izravno s bazom podataka SQL i pisanje na red čekanja obavijest korisniku završite, omogućuju pozadinska servisa ručicu visoke Latencija relacijskih baza podataka rad. Primjer reda čekanja obrade s procesom slike potražite u članku [Vodič za početak rada WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md).

* Zbrajanje RSS. Ako imate web-mjesto koje se održava popis RSS sažetaka sadržaja, nije moguće uvesti u svim člancima iz sažetaka sadržaja u postupka u pozadini.

* Datoteka održavanja, kao što su zbrajanje ili čišćenje datoteke zapisnika.  Možda ćete imati datoteke zapisnika stvoren nekoliko web-mjesta ili proteže zasebnom vrijeme koje želite kombinirati da biste pokrenuli analizu zadatke na njima. Ili, možda ćete morati zakazivanje zadataka da biste pokrenuli tjedno da biste očistili stare datoteke zapisnika.

* Ingress u Azure tablice. Možda imate datoteka pohranjenih i blob-ova i želite ih analizirati i spremanje podataka u tablicama. Funkcija ingress nije pisanje puno redaka (milijune u nekim slučajevima), a WebJobs SDK omogućuje jednostavno implementirati tu funkciju. SDK se nalaze u stvarnom vremenu nadzor pokazatelje napretka kao što su broj redaka koji su zapisan u tablici.

* Ostali zadaci dugoročnih koji želite pokrenuti u pozadini niti, kao što su [Slanje poruke e-pošte](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 

* Sve zadatke za koje želite pokrenuti na rasporedu, kao što su izvođenje operacije na kopija svake noći.

U mnogim scenarija trebali biste skalirali web-aplikacijama za rad na više VMs koji istodobno raditi više WebJobs. U nekim slučajevima to može rezultirati s istim podacima obrađuju više puta, no to nije problem kada koristite ugrađeni reda čekanja, blob i usluge Bus okidača SDK WebJobs. SDK osigurava da vaše funkcije obradit će se samo jednom za svaku poruku ili blob.

WebJobs SDK pojednostavljuje rukovati uobičajenih pogreškom scenarijima. Možete postaviti upozorenja za slanje obavijesti kad funkcija ne uspijeva te vremensko ograničenje možete postaviti tako da se funkcije automatski prekinuti ako ga ne dovrši unutar određenog vremena ograničenja.

## <a id="code"></a>Primjere koda

Kod za rukovanje uobičajene zadatke koje funkcioniraju sa spremištem Azure je jednostavno. U aplikaciji konzole `Main` način stvaranja na `JobHost` objekt koji koordinate pozive načinima u pišete. Framework WebJobs SDK zna kada da biste nazvali načinima i što vrijednosti parametara da biste koristili na temelju atribute WebJobs SDK koristite u njima. SDK-a nudi *okidača* koji određuju što uvjeta prouzročiti funkcija koje se zove i *fascikli* odredite koliko će se da biste dobili informacije u i Odjava iz njega način parametara.

Na primjer, atribut [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) uzrokuje funkcija pozivanje kada je poruka primljena na redu, a ako je oblik poruke JSON bajtni polja ili prilagođena vrsta, poruku je automatski deserialized. Atribut [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) pokreće postupak prilikom svakog stvaranja novog blob u račun za Azure prostora za pohranu.

Evo jednostavnog programa koji polls red i stvara blob za svaki primljene poruke reda čekanja:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

Na `JobHost` objekt je spremnik za skup funkcije pozadine. Na `JobHost` objekta nadzire funkcije, nadzora za događaja koji pokreću ih i izvršava funkcije prilikom okidača događaja. Pozovite na `JobHost` način odredite želite li da se postupak spremnik za izvođenje na trenutnoj niti ili pozadine niti. U primjeru u `RunAndBlock` način neprestano pokreće postupak na trenutnoj niti.

Budući da u `ProcessQueueMessage` metoda u ovom primjeru ima na `QueueTrigger` atribut, okidača za tu funkciju je prijam novu poruku red. Na `JobHost` objekt nadzirane novih reda čekanja poruka na navedeni reda čekanja ("webjobsqueue" u ovom primjeru), a kada pronađe, poziva `ProcessQueueMessage`. 

Na `QueueTrigger` atributa povezuje s `inputText` parametar vrijednost reda čekanja poruke. I `Blob` atributa povezuje s `TextWriter` objekt blob pod nazivom "blobname" u spremniku pod nazivom "naziv spremnika".  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Funkcija koristi parametara pisati vrijednost poruke reda čekanja blob-om:

        writer.WriteLine(inputText);

Značajke okidača i binder WebJobs SDK znatno Pojednostavnite kod morate pisati. Najniže za obradu reda čekanja, blob-ova ili datoteka ili da biste započeli Zakazani zadaci koje je potrebno je obavljen po framework WebJobs SDK. Ako, na primjer, framework stvara redova koje ne postoje još otvara reda, čitanja red čekanja poruke, briše red poruke kada obrada ne završi, stvara blob spremnika koje ne postoje još, zapisuje blob-ova i tako dalje.

Sljedeći primjer koda prikazuje različite uvlake u jedan WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, i `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Planiranje

Na `TimerTrigger` atribut vam omogućuje pokretanje funkcije da biste pokrenuli prema rasporedu. Možete unijeti na WebJob cijela kroz Azure ili raspored pojedinačne funkcije WebJob pomoću WebJobs SDK `TimerTrigger`. Slijedi primjer kod.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Dodatne uzorak koda potražite u članku [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) u spremištu azure-webjobs-sdk-extensions na GitHub.com.

## <a name="extensibility"></a>Mogućnost proširenja

Niste ograničeni na ugrađene funkcije – WebJobs SDK omogućuje napišite prilagođenu okidača i fascikli.  Ako, na primjer, možete napisati okidača za događaje predmemorije i periodičku rasporeda. [Detaljni vodič na WebJobs SDK proširenja](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) i uzorak koda za jednostavniji početak rada za pisanje vlastite okidača i fascikli programa [otvorite spremištu izvora](https://github.com/Azure/azure-webjobs-sdk-extensions) sadrži.

## <a id="workerrole"></a>Pomoću SDK WebJobs izvan WebJobs

Program koji koristi u WebJobs SDK je standardni aplikacije konzole i bilo kojeg mjesta možete pokrenuti--ne sadrži da biste pokrenuli kao u WebJob. Program možete testirati lokalno na vašem računalu razvoj, a u radnog možete ga pokrenuti u ulogu suradnika u Oblaku ili servis Windows ako želite neku od tih okruženja. 

Međutim, nadzorne ploče dostupna je samo kao datotečni nastavak za web-aplikaciju programa aplikacije servisa za Azure. Ako želite da biste pokrenuli izvan programa WebJob i i dalje koristite upravljačku ploču, možete konfigurirati web-aplikacijama koristiti isti prostor za pohranu račun koji se odnosi niz za povezivanje nadzorne ploče SDK WebJobs i nadzorne ploče WebJobs web app prikazat će se podaci o izvođenja funkcije iz programa koji se izvode neko drugo mjesto. Pomoću na URL https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions možete pristupiti na nadzornoj ploči. Dodatne informacije potražite u članku [Početak nadzorne ploče za razvoj lokalne s WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ali imajte na umu da objava na blogu prikazuje stari naziv niz veze. 

## <a id="nostorage"></a>Značajke nadzorne ploče

WebJobs SDK nudi nekoliko prednosti čak i ako ne koristite WebJobs SDK okidača ili fascikli:

* Možete pozvati funkcije na nadzornoj ploči.
* Možete ponavljanje funkcije na nadzornoj ploči.
* Zapisnici možete pogledati na nadzornoj ploči povezane s određenom WebJob (aplikaciju zapisnike, zapisan pomoću Console.Out Console.Error, praćenje, itd.) ili povezane s poziva određena funkcija koji je generirao ih (zapisnika zapisan pomoću programa `TextWriter` objekt koji SDK prosljeđuje funkciji kao parametar). 

Dodatne informacije potražite u članku [kako ručno Pozovite funkcije](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) i [Kako napisati zapisnika](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Daljnji koraci

Dodatne informacije o WebJobs SDK potražite u članku [Azure WebJobs preporučuje resursi](http://go.microsoft.com/fwlink/?linkid=390226).

Informacije o najnovijim poboljšanja WebJobs SDK potražite u članku [Napomene](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).
 
