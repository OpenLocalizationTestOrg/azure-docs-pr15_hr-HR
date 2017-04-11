<properties
    pageTitle="Aplikacija na – lokalno/oblaka hibridnog (.NET) | Microsoft Azure"
    description="Saznajte kako stvoriti aplikaciju na – lokalno/oblaka hibridnog .NET koja koristi preusmjeravanja Bus servisa Azure."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.NET na – lokalno/oblaka hibridnog aplikacije pomoću Bus preusmjeravanja servisa za Azure

## <a name="introduction"></a>Uvod

U ovom se članku opisuje način stvaranja aplikacija za oblak hibridnog s web-mjesto Microsoft Azure i u okvir za Visual Studio. Vodič pretpostavlja da ste bez rad Azure prethodnog sa. Za manje od 30 minuta, imat ćete aplikacije koja koristi više Azure resursa prema gore i pokretanje u oblaku.

Saznat ćete:

-   Kako stvoriti ili prilagoditi postojeće web-servisa za potrošnju web rješenje.
-   Upute za zajedničko korištenje podataka između programa Azure i web-servisa pomoću servisa Azure servisa Bus preusmjeravanja hostira negdje drugdje.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Kako će se Bus usluge preusmjeravanja pomaže vam hibridnog rješenja

Poslovna rješenja obično se sastoje od kombinacije prilagođeni kod napisan prijeći na nove i jedinstveni poslovanja i postojeće funkcijama sustava koji se već nalaze na mjestu i rješenja.

Rješenje projektantima su početak korištenja oblaka za lakše rukovanje skaliranje preduvjeti i manji trošak radu. Tako, mogu pronaći da postojeći servis imovine odražava kao i sastavne blokove njihova rješenja unutar vatrozid tvrtke i Odjava iz njega jednostavno ih želite doći do za pristup rješenje oblaka. Mnoge interne servise su ugrađeni ili hostira tako da ih možete jednostavno izložiti na rub korporacijskom mrežom.

Bus usluge preusmjeravanja namijenjen je korištenje slučaj ponijeti postojeće web-servisi Windows Communication Foundation (WCF), a povećanje tih servisa sigurnog pristupačnosti u rješenja koja nalaze izvan tvrtke opseg bez većih promjena unutar poslovne mrežne infrastrukture. Takve usluge preusmjeravanja servisa knjižna se uvijek nalazi unutar svoje okruženje, no oni delegatu slušanje za dolazne sesije i zahtjevi za servis Bus oblaka hostira. Servis Bus štiti tih servisa i od neovlaštenog pristupa pomoću provjere autentičnosti [Zajednički pristup potpis](../service-bus-messaging/service-bus-sas-overview.md) (SAS).

## <a name="solution-scenario"></a>Scenarij rješenja

U ovom ćete praktičnom vodiču ćete stvoriti ASP.NET web-mjesto koje omogućuje vam da biste vidjeli popis proizvoda na stranici zaliha proizvoda.

![][0]

Vodič pretpostavlja da su podaci o proizvodu u postojeće lokalnim sustavom i koristi Bus usluge preusmjeravanja dođu u tom sustavu. To je Simulirani po web-servisa koji pokreće se u program jednostavan konzole i sigurnosno se tako da u memoriji skupa proizvoda. Moći za izvršavanje te aplikacije konzole na vašem računalu i implementaciju web uloga u Azure. Na taj način, vidjet ćete kako će uloga web koji se izvodi u Azure podatkovnog centra uistinu poziva na računalo, čak i ako je vaše računalo se gotovo certainly nalaze iza najmanje jedan vatrozid i prevođenje (NAT) sloj adresu mreže.

Snimka zaslona početne stranice dovršene web-aplikacije je.

![][1]

## <a name="set-up-the-development-environment"></a>Postavljanje okruženja za razvoj

Prije no što započnete razvoj aplikacija za Azure, dobiti alate i postaviti razvojno okruženje.

1.  Na stranici [Početak Alati i SDK][] instalirati Azure SDK za .NET.

2.  Kliknite **Instaliraj SDK** za verziju programa Visual Studio koristite. Koraci u ovom ćete praktičnom vodiču pomoću Visual Studio 2015.

4.  Kada se to od vas zatraži da pokrenete ili spremite instalacijski program, kliknite **Pokreni**.

5.  U **Web platformu Installer**, kliknite **Instaliraj** i nastaviti s instalacijom.

6.  Nakon dovršetka instalacije, imat ćete sve potrebne za pokretanje za razvoj aplikacija. SDK sadrži alata koji omogućuju vam da jednostavno razvoj aplikacija za Azure u Visual Studio. Ako nemate ugrađen Visual Studio, SDK instalira i na besplatnu Visual Studio Express.

## <a name="create-a-namespace"></a>Stvaranje prostor naziva

Da biste počeli koristiti značajke servisa Bus u Azure, prvo morate stvoriti polje naziva servisa. Prostor naziva nudi scoping spremnik adresiranja resursima servisa Bus unutar vaše aplikacije.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Stvaranje lokalnog poslužitelja

Najprije će izgraditi (mock) lokalni sustav kataloga proizvoda. Bit će prilično jednostavno; vidjet ćete ovo kao koji predstavlja sustava kataloga proizvoda stvarni lokalnog s dovršeno servisa površina Pokušavamo da biste integrirali.

Taj projekt je aplikacije konzole za Visual Studio, a koristi [Azure servisa Bus NuGet paketa](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) da biste dodali na servis Bus biblioteke i konfiguracijske postavke.

### <a name="create-the-project"></a>Stvaranje projekta

1.  Koristite administratorske ovlasti, pokrenite Microsoft Visual Studio. Da biste pokrenuli Visual Studio s administratorskim ovlastima, desnom tipkom miša kliknite ikonu programa **Visual Studio** , a zatim kliknite **Pokreni kao administrator**.

2.  U Visual Studio, na izborniku **datoteka** kliknite **Novo**, a zatim kliknite **projekt**.

3.  **Instalirani predlošci**, u odjeljku **Visual C#**, kliknite **Konzolu**. U okvir **naziv** upišite naziv **ProductsServer**:

    ![][11]

4.  Kliknite **u redu** da biste stvorili **ProductsServer** projekta.

7.  Ako ste već instalirali paket Upravitelj NuGet za Visual Studio, prijeđite na sljedeći korak. U suprotnom, posjetite [NuGet][] i kliknite [Instaliranje NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Slijedite upute da biste instalirali paket Upravitelj NuGet, a zatim ponovno pokrenuti Visual Studio.

7.  U pregledniku rješenja, desnom tipkom miša kliknite **ProductsServer** projekta, a zatim kliknite **Upravljanje NuGet paketa**.

8.  Kliknite karticu **Pregled** , a zatim potražite `Microsoft Azure Service Bus`. Kliknite **Instaliraj**i prihvatite uvjete korištenja.

    ![][13]

    Imajte na umu sklopova potreban klijent sada se poziva.

9.  Dodajte novi klasa za ugovoru proizvoda. U pregledniku rješenja, desnom tipkom miša kliknite **ProductsServer** projekta i kliknite **Dodaj**pa kliknite **Predmet**.

10. U okvir **naziv** upišite naziv **ProductsContract.cs**. Zatim kliknite **Dodaj**.

11. U **ProductsContract.cs**zamijenite definiciju prostor naziva sljedeći kod koji definira ugovor za servis.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. U Program.cs, zamijenite definiciju prostor naziva sljedeći kod koji dodaje servis profila i glavno računalo za njega.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. U pregledniku rješenja, dvokliknite **App.config** datoteku da biste ga otvorili u uređivaču za Visual Studio. Pri dnu zaslona u ** &lt;sustava. ServiceModel&gt; ** element (ali i dalje unutar &lt;sustava. ServiceModel&gt;), dodajte sljedeći kôd XML. Ne zaboravite da biste zamijenili *yourServiceNamespace* naziv polja naziva i *yourKey* s ključem SAS koju ste ranije vratili na portalu:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. I dalje u App.config, u u ** &lt;appSettings&gt; ** element, Zamijeni veza niz vrijednost niza za povezivanje prethodno dobivenog na portalu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Pritisnite **Ctrl + Shift + B** ili na izborniku **Sastavljanje** kliknite **Stvaranje rješenja** omogućuje stvaranje aplikacija i provjerite točnost svoje poslovne dosad.

## <a name="create-an-aspnet-application"></a>Stvaranje aplikacije komponente platforme ASP.NET

U ovom odjeljku će izrađivati jednostavne ASP.NET aplikacija koji prikazuje podatke dohvatiti iz servisa proizvoda.

### <a name="create-the-project"></a>Stvaranje projekta

1.  Provjerite je li Visual Studio pokrenut s administratorskim ovlastima.

2.  U Visual Studio, na izborniku **datoteka** kliknite **Novo**, a zatim kliknite **projekt**.

3.  **Instalirani predlošci**, u odjeljku **Visual C#**, kliknite **ASP.NET web-aplikacije**. Naziv projekta **ProductsPortal**. Zatim kliknite **u redu**.

    ![][15]

4.  S popisa **Odaberite predložak** , kliknite **MVC**. 

6.  Potvrdite okvir pokraj **glavnog računala u oblaku**.

    ![][16]

5. Kliknite gumb **Promijeni provjere autentičnosti** . U dijaloškom okviru **Promjena provjere autentičnosti** kliknite **Bez provjere autentičnosti**, a zatim kliknite **u redu**. Za ovaj vodič se implementacija aplikacije koji se ne mora korisničko ime za prijavu.

    ![][18]

6.  U odjeljku **Microsoft Azure** dijaloški okvir **Novi projekt ASP.NET** provjerite da **glavnog računala u oblaku** odabrana i **Aplikacije servisa za** odabran na padajućem popisu.

    ![][19]

7. Kliknite **u redu**. 

8. Sada morate konfigurirati Azure resursi za novu web-aplikaciju. Slijedite korake u odjeljku [Konfiguriranje Azure resursi za novu web-aplikaciju](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Nakon toga vratite ovog praktičnog vodiča i prijeđite na sljedeći korak.

5.  U pregledniku rješenja desnom tipkom miša kliknite **modela** , a zatim kliknite **Dodaj**, a zatim kliknite **Predmet**. U okvir **naziv** upišite naziv **Product.cs**. Zatim kliknite **Dodaj**.

    ![][17]

### <a name="modify-the-web-application"></a>Izmjena web-aplikacije

1.  U datoteci Product.cs u Visual Studio, zamijenite postojeću definiciju prostor naziva sljedeći kod.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  U pregledniku rješenja, proširite mapu **kontrolera** , a zatim dvokliknite datoteku **HomeController.cs** da biste ga otvorili u Visual Studio.

3. U **HomeController.cs**zamijenite postojeću definiciju prostor naziva sljedeći kod.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  U pregledniku rješenja, proširite mapu Views\Shared, a zatim dvokliknite **_Layout.cshtml** da biste ga otvorili u uređivaču za Visual Studio.

5.  Da biste promijenili sve pojave **Moje** aplikacije ASP.NET **LITWARE o**proizvodima.

6. Uklanjanje veze **Početna stranica**, **o**i **kontakt** . U sljedećem primjeru, izbrišite istaknuti kod.

    ![][41]

7.  U pregledniku rješenja, proširite mapu Views\Home, a zatim dvokliknite **Index.cshtml** da biste ga otvorili u uređivaču za Visual Studio.
    Sljedeći kod zamijenite cijeli sadržaj datoteke.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Da biste provjerili točnost svoje poslovne dosad, možete pritisnuti **Ctrl + Shift + B** da biste sastavili projekta.


### <a name="run-the-app-locally"></a>Pokrenite aplikaciju lokalno

Pokrenite aplikaciju da biste provjerili funkcionira li.

1.  Provjerite je li **ProductsPortal** aktivni projekt. Desnom tipkom miša kliknite naziv projekta u programu Explorer rješenja, a zatim odaberite **Postavi kao pokretanje projekta**.
2.  U Visual Studio, pritisnite F5.
3.  Aplikacija prikazivati, izvodi u pregledniku.

    ![][21]

## <a name="put-the-pieces-together"></a>Sastavili dijelovi

Sljedeći je korak da biste priključiti lokalnog poslužitelja Proizvodi s ASP.NET aplikacija.

1.  Ako već nije otvorena, u Visual Studio ponovno otvoriti **ProductsPortal** projekta koji ste stvorili u odjeljku [Stvaranje ASP.NET aplikacija](#create-an-aspnet-application) .

2.  Slično koraku u odjeljku "Stvaranje programa uključeno lokalnog poslužitelja" dodajte paket NuGet reference projekta. U pregledniku rješenja, desnom tipkom miša kliknite **ProductsPortal** projekta, a zatim kliknite **Upravljanje NuGet paketa**.

3.  Traženje "Servisa Bus", a zatim odaberite stavku **Bus usluge za Microsoft Azure** . Dovršite instalaciju pa zatvorite dijaloški okvir.

4.  U pregledniku rješenja, desnom tipkom miša kliknite **ProductsPortal** projekta, a zatim kliknite **Dodaj**, zatim **Postojeće stavke**.

5.  Dođite do datoteke **ProductsContract.cs** iz **ProductsServer** konzole za projekt. Kliknite da biste istaknuli ProductsContract.cs. Kliknite strelicu prema dolje pokraj **Dodaj**, a zatim kliknite **Dodaj kao vezu**.

    ![][24]

6.  Sada otvorite **HomeController.cs** datoteku u uređivaču za Visual Studio i zamijenite definiciju prostor naziva sljedeći kod. Pripazite da zamijenite nazivom polje naziva servisa i *yourKey* *yourServiceNamespace* pomoću svojeg ključa SAS. To će omogućiti klijent za pozivanje lokalnog servisa, vraća rezultat poziva.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  U pregledniku rješenja, desnom tipkom miša kliknite rješenje **ProductsPortal** (Provjerite je li desnom tipkom miša kliknite rješenje, a ne u projekt). Kliknite **Dodaj**, a zatim kliknite **Postojeći projekt**.

8.  Dođite do **ProductsServer** projekta, a zatim dvokliknite datoteku **ProductsServer.csproj** rješenja da biste je dodali.

9.  Prikazivanje podataka na **ProductsPortal**mora biti pokrenut **ProductsServer** . U pregledniku rješenja, desnom tipkom miša kliknite rješenje **ProductsPortal** , a zatim kliknite **Svojstva**. Prikazat će se dijaloški okvir **Svojstava stranice** .

10. Na lijevoj strani kliknite **Pokretanje projekta**. Na desnoj strani kliknite **više projekata prilikom pokretanja**. Provjerite je li **ProductsServer** i koji **ProductsPortal** prikazuju, tim redoslijedom s **pokretanje** Postavi kao akcija za oba.

      ![][25]

11. I dalje u dijaloškom okviru **Svojstva** kliknite **Projekt ovisnosti** na lijevoj strani.

12. Na popisu **projekata** kliknite **ProductsServer**. Provjerite je li **ProductsPortal** **nije** odabrana.

14. Na popisu **projekata** kliknite **ProductsPortal**. Provjerite je li odabrana **ProductsServer** . 

    ![][26]

15. U pritisnite **u redu** dijaloški okvir **Svojstava stranice** .

## <a name="run-the-project-locally"></a>Lokalno pokrenite projekt

Da biste testirali lokalno aplikaciju, u Visual Studio pritisnite **F5**. Lokalni poslužitelj (**ProductsServer**) bi trebao počinjati prvi, a zatim aplikaciju **ProductsPortal** bi trebao počinjati u prozoru preglednika. Ovaj put, vidjet ćete da zaliha proizvoda popisi podatke dohvaćene s lokalnim sustavom za servis proizvoda.

![][10]

Na stranici **ProductsPortal** pritisnite **Osvježi** . Svaki put kada osvježite stranicu, vidjet ćete aplikaciju poslužitelja prikazati poruku kada `GetProducts()` iz **ProductsServer** naziva.

Zatvorite obje aplikacije prije prelaska na sljedeći korak.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Implementacija ProductsPortal projekta programa Azure web App

Sljedeći je korak da biste pretvorili **ProductsPortal** sučelju programa Azure web App. Najprije implementirati **ProductsPortal** projekta, slijedite korake u odjeljku [uvođenja projekt web Azure web-aplikaciji](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Po dovršetku implementacije vratite ovog praktičnog vodiča i prijeđite na sljedeći korak.

> [AZURE.NOTE] Vidjet ćete poruku o pogrešci u prozoru preglednika kada **ProductsPortal** web project se automatski pokrene nakon implementacijskih. To se očekuje i pojavljuje jer se aplikacija **ProductsServer** još nije pokrenut.

Kopirajte URL distribuiranih web-aplikacije, kao što je potrebno je URL-a u sljedećem koraku. U prozoru Azure aplikacije servisa aktivnosti u Visual Studio možete dobiti i ovaj URL:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Postavljanje ProductsPortal kao web-aplikacije

Prije pokretanja aplikacije u oblaku, morate osigurati da **ProductsPortal** pokrene iz programa Visual Studio kao web-aplikacijama.

1. U Visual Studio, desnom tipkom miša kliknite **ProjectsPortal** projekta, a zatim kliknite **Svojstva**.

3. U stupcu na lijevoj strani kliknite **Web**.

5. U odjeljku **Pokretanje akcije** kliknite gumb **Start URL** pa u tekstni okvir unesite URL za prethodno distribuiranih web app; na primjer, `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Na izborniku **datoteka** u Visual Studio, kliknite **Spremi sve**.

7. Na izborniku Sastavi u Visual Studio, kliknite **Ponovno stvaranje rješenja**.

## <a name="run-the-application"></a>Pokrenite aplikaciju

2.  Pritisnite F5 omogućuje stvaranje i pokretanje aplikacija. Lokalni poslužitelj (konzole aplikacija **ProductsServer** ) bi trebao počinjati prvi, a zatim aplikaciju **ProductsPortal** bi trebao počinjati u prozoru preglednika, kao što je prikazano na sljedećem zaslonu snimka. Obratite pozornost na to ponovno popisi podatke dohvaćene s lokalnim sustavom za servis proizvoda zaliha proizvoda, a prikazuje tih podataka u web-aplikaciji. Potvrdite okvir URL-a da biste bili sigurni da su **ProductsPortal** ostalo u oblaku, kao Azure web-aplikaciju programa. 

    ![][1]

    > [AZURE.IMPORTANT] Aplikacija konzole **ProductsServer** mora biti pokrenut i mogao posluživati podatke s aplikacijom **ProductsPortal** . Ako preglednik prikazuje pogrešku, pričekajte nekoliko sekundi više **ProductsServer** za učitavanje i prikazuje se sljedeća poruka. Zatim pritisnite gumb **Osvježi** u pregledniku.

    ![][37]

3. Vratite se u pregledniku pritisnite **Osvježi** na stranici **ProductsPortal** . Svaki put kada osvježite stranicu, vidjet ćete aplikaciju poslužitelja prikazati poruku kada `GetProducts()` iz **ProductsServer** naziva.

    ![][38]

## <a name="next-steps"></a>Daljnji koraci  

Da biste saznali više o Bus servisa, potražite u sljedećim resursima:  

* [Azure Service Bus][sbwacom]  
* [Upute za korištenje servisa Bus redovi][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Alati i SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

