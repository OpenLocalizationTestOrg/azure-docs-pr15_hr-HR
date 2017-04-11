<properties
    pageTitle="Zemlj fenced automatske obavijesti s koncentratorima Azure obavijesti i Bing prostorno podataka | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču ćete naučiti izlaganje temeljena automatske obavijesti s koncentratorima Azure obavijesti i Bing prostorno podataka."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="automatske obavijesti, automatske obavijesti"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Zemlj fenced automatske obavijesti s koncentratorima Azure obavijesti i Bing prostorno podataka
 
 > [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

U ovom ćete praktičnom vodiču ćete naučiti izlaganje temeljena automatske obavijesti s koncentratorima Azure obavijesti i podacima prostorno Bing, leveraged iz aplikacije univerzalni platformu sustava Windows.

##<a name="prerequisites"></a>Preduvjeti
Najprije i u prvom redu, morate pripazite da imate sve na softver i usluge stara requisites:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) ili noviji ([Izdanje zajednice](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) neće imati kao i). 
* Najnovija verzija preglednika [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Razvojni centar za Bing karte računa](https://www.bingmapsportal.com/) (možete ga besplatno stvoriti i povezati pomoću Microsoftova računa). 

##<a name="getting-started"></a>Početak rada

Započnimo stvaranjem projekta. U Visual Studio, započnite novi projekt vrste **Prilagođenom web -aplikacijom (Universal Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Nakon dovršetka stvaranjem projekta, imat ćete analitička za samoj aplikaciji. Sada postavimo sve za infrastrukturu zemlj fencing. Jer smo početak korištenja servisa Bing za to, postoji javno REST API-JA krajnja točka koja omogućuje nam upita za određeno mjesto okvira:

    http://spatial.virtualearth.net/REST/v1/data/
    
Morat ćete navesti sljedećih parametara da biste ga osposobili:

* **ID oznake izvora podataka** i **Naziv izvora podataka** – u API Bing karte za izvore podataka sadrže različite bucketed metapodaci, kao što su mjesta i radno vrijeme operacije. Dodatne informacije o tim ovdje. 
* **Naziv entitet** – onaj koji želite koristiti kao referencu točku za obavijesti. 
* **Ključ za API Bing karte** – to je ključ koji ste nabavili ranije prilikom stvaranja računa Razvojni centar za Bing.
 
Pogledajmo učinite na dive obuhvaća više razina na postavljanje za svaki od iznad elemenata.

##<a name="setting-up-the-data-source"></a>Postavljanje izvora podataka

To možete učiniti u Razvojni centar za Bing karte. Jednostavno kliknite **izvori podataka** na gornjoj navigacijskoj traci i odaberite **Upravljanje izvorima podataka**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Ako ste radili Bing karte API prije, najvjerojatnije ima neće se svi izvori podataka koje postoji, da bi jednostavno mogli stvarati novi tako da kliknete na prijenos podataka s izvorom podataka. Provjerite je li ispunite obavezna polja:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Koje možda se pitate li se – što je podatkovne datoteke i što treba vam se prijenos? U svrhu ovaj test možete samo koristimo uzorka utemeljen na kanal koji sadrže okvire područje waterfront Pula:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Navedenog predstavlja entitet:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Jednostavno kopirajte i zalijepite gornjeg niza na novu datoteku i spremanje u obliku **NotificationHubsGeofence.pipe**i prenesite u Razvojni centar za Bing.

>[AZURE.NOTE]Možda zatražiti da biste odredili novi ključ za **glavni ključ** koji se razlikuje od **Ključ upita**. Jednostavno stvaranje novog ključa putem nadzorne ploče i osvježite stranicu prijenos izvora podataka.

Kada prenesete podatkovnu datoteku, morat ćete svakako objavite izvora podataka. 

Otvorite **Upravljanje izvorima podataka**, kao što smo niste iznad, pronađite s izvorom podataka na popisu i kliknite **Objavi** u stupcu **Akcija** . U nekoliko riječi, trebali biste vidjeti s izvorom podataka na kartici **Objavljene izvore podataka** :

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Ako kliknete **Uređivanje**, moći da biste vidjeli na prvi pogled koje mjesta smo uvedene u njemu:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Sada na portalu nema vas ograničenja za geofence da koju smo stvorili – potrebna je potvrdu da je mjesto navedeno je u desnom vicinity.

Sada imate sve zahtjeve za izvor podataka. Da biste detalje na URL-u zahtjev za poziv API-JA u Razvojni centar za Bing karte, kliknite **izvori podataka** i odaberite **Izvor podataka**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

**URL upita** nije Ispričavamo se nakon ovdje. Ovo je krajnju točku prema kojima ne možemo izvršavanje upita provjerite je li uređaj trenutno unutar granica mjesto ili ne. Da biste izvršili tu provjeru, jednostavno moramo izvršavanje poziva GET protiv URL upita pomoću sljedećih parametara dodan:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Pokažite na taj način se određuje cilj koji ćemo dobiti s uređaja, a Bing karte automatski Izvedi izračuna da biste saznali je unutar na geofence. Kada je izvršenje zahtjeva putem preglednika (ili zakretanja), dobit ćete standardni odgovor JSON:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Odgovor samo će se dogoditi kada je točka zapravo unutar granica određenu. Ako nije, dobit ćete je prazan **rezultata** grupe:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>Postavljanje aplikacije UWP

Imamo izvora podataka spremna, ne možemo možete početi s radom u aplikaciji UWP koje ćemo bootstrapped neke starije verzije.

Najprije i u prvom redu, ne možemo morate omogućiti mjesto servisa za naše aplikaciju. Da biste to učinili, dvokliknite na `Package.appxmanifest` datoteku u **Programu Explorer rješenja**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

U paket svojstva na kartici koji upravo otvorili kliknite **Mogućnosti** , a zatim provjerite jeste li odabrali **mjesto**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Kao što je mogućnost mjesto prijavljen, stvorite novu mapu pod nazivom rješenje `Core`, i dodajte nove datoteke u njoj, pod nazivom `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Na `LocationHelper` predmete sam relativno jednostavna sada – sve to čini je dopustite nam da biste dobili korisničke lokacije u sustavu API-JA:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Dodatne informacije o tome kako izvući korisnikovu lokaciju u aplikacijama UWP u službeni [MSDN dokumenta](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Da biste provjerili funkcionira li se mjesto acquisition, otvorite strani kod glavne stranice (`MainPage.xaml.cs`). Stvaranje nove rukovatelja događajima u `Loaded` događaj na `MainPage` Graditelj:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Implementacija rukovatelja događajima je na sljedeći način:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Obratite pozornost na to da ne možemo deklarirane rukovatelj kao asinkrone jer `GetCurrentLocation` je awaitable i stoga zahtijeva želite izvršiti u kontekstu asinkrone. Također, budući da u određenim uvjetima smo možda završe s null mjesta (npr. mjesto onemogućene su servise ili neku drugu aplikaciju odbijen dozvole za pristup mjestu), moramo da biste bili sigurni da je ispravno obrađuje s null potvrdite okvir.

Pokrenite aplikaciju. Provjerite je li dopušten pristup mjestu:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Jednom pokreće aplikacije trebali da biste vidjeli koordinate u **izlaznom** prozoru:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Sada ste upoznati mjesto radi acquisition – slobodno da biste uklonili rukovatelj događajima test za Loaded jer smo neće biti koristi više.

Sljedeći je korak da biste snimili promjena lokacije. Za to prođimo natrag na `LocationHelper` klase i dodavanje rukovatelja događajima `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Implementacije prikazat će se koordinate lokacije u **izlaznom** prozoru:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Postavljanje pozadinski

Preuzmite [.NET pozadinskog uzorka iz GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Kada preuzimanje završi, otvorite na `NotifyUsers` mapu, a naknadno – u `NotifyUsers.sln` datoteku.

Postavljanje na `AppBackend` projekta kao **Pokretanje projekta** , a zatim ga pokrenuti.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Projekt već je konfiguriran za slanje obavijesti poslati ciljnih, pa ne možemo morat ćete učiniti samo dvije stvari – zamijeniti odgovarajuću mogućnost povezivanja niza za središtu obavijesti i dodavanje identifikacijskog granicu za slanje obavijesti samo kada korisnik se nalazi unutar na geofence.

Da biste konfigurirali niz za povezivanje u na `Models` mapu Otvori `Notifications.cs`. Na `NotificationHubClient.CreateClientFromConnectionString` funkcija bi trebala sadržavati informacije o vaše središte obavijesti koje možete dobiti [Azure Portal](https://portal.azure.com) (pogledajte unutar plohu **Pravilnike za pristup** u odjeljku **Postavke**). Spremite datoteku ažurirane konfiguracije.

Sada ćemo potrebnih za stvaranje modela za Bing karte API rezultat. Da biste to učinili najjednostavnije desnom tipkom miša kliknite na `Models` mapu, a zatim **Dodaj** > **predmete**. Nazovite ga `GeofenceBoundary.cs`. Kada završi, kopirajte na JSON iz API odgovor koji smo spominju u odjeljku prvi i Visual Studio koristi **Uređivanje** > **Posebno lijepljenje** > **JSON Zalijepi kao klase**. 

Tako ćemo bili sigurni da se objekt će deserijalizirati točno onako kako ga je predviđeno. Dobivene skupu predmete oblik treba sličan ovo:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Zatim otvorite `Controllers`  >  `NotificationsController.cs`. Moramo dotjerati poziva poštanski račun za ciljnu dužinu i širinu. Za to jednostavno dodajte dva niza u potpis funkcija – `latitude` i `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Stvaranje nove klase u projektu pod nazivom `ApiHelper.cs` – ćemo ga koristiti za povezivanje s Bing da biste provjerili pokažite presjecima granicu. Implementacija u `IsPointWithinBounds` (funkcija), ovako:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Provjerite jeste li zamijeniti krajnju točku API-JA s URL upita koji ste nabavili ranije (isto vrijedi za ključ za API-JA) Razvojni centar za Bing. 

Ako postoje rezultate upita, to znači da navedeni točka je unutar granica geofence, pa ne možemo vratili `true`. Ako nema rezultata, Bing je koja upozorava nam da je izvan okvira za pretraživanje pa ćemo vratili `false`.

Vratite se u `NotificationsController.cs`, stvaranje provjere desno prije naredbu promjena:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Na taj način, obavijesti se samo šalje kada je točka unutar granica.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Testiranje automatske obavijesti u aplikaciji UWP

Vraćanje aplikacije UWP ćemo sada moći da biste testirali obavijesti. Unutar na `LocationHelper` klase, stvorite novi funkcija – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Zamjena na `POST_URL` mjesto distribuiranih web-aplikaciju koju smo stvorili u prethodnom odjeljku. Zasad je u redu da biste se izvodi lokalno, no dok radite na implementaciji javno verzije, morat ćete hostirati kod vanjskog davatelja usluga.

Pogledajmo sada provjerite je li smo registrirati UWP aplikacije za slanje obavijesti. U Visual Studio, kliknite na **projektu** > **Pohrana** > **pridružiti aplikacije s trgovinom**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Kada se prijavite na račun za razvojne inženjere, provjerite je li odaberite aplikaciju za postojeću ili stvoriti novu i pridružiti paketa. 

Otvorite centar za razvojni i otvorite aplikaciju koju ste upravo stvorili. Kliknite **Services** > **Automatske obavijesti** > **Live bilježaka i brošura**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Na web-mjestu uzeti u obzir **Aplikaciji tajna** i **SID paketa**. Potrebno je i na portalu za Azure – otvaranje koncentratora za obavijesti, kliknite **Postavke** > **Servisa obavijesti** > **Sustava Windows (WNS)** , a zatim unesite podatke u obavezna polja.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Kliknite **Spremi**.

Desnom tipkom miša kliknite **reference** u **Pregledniku rješenja** , a zatim odaberite **Upravljanje NuGet paketa**. Ne možemo ćete morati dodati referencu **Bus usluge za Microsoft Azure upravlja biblioteke** – jednostavno tražiti `WindowsAzure.Messaging.Managed` i dodajte ga u projekt.

![](./media/notification-hubs-geofence/vs-nuget.png)

Za testiranje, možemo stvoriti u `MainPage_Loaded` događajima opet i dodajte ova isječak koda:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Navedenog registrira aplikaciju s središtu obavijesti. Spremni ste za slanje! 

U `LocationHelper`, unutrašnji na `Geolocator_PositionChanged` rukovatelj, možete dodati dio test kod koji program prisilno stavite na mjesto unutar na geofence:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Jer smo su prosljeđivanje realni koordinate (koji se možda neće biti unutar granica koristilo) i koristite unaprijed definirane test vrijednosti, ne možemo prikazat će se obavijest prikazuju se na ažuriranja:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Što je sljedeće?

Postoji nekoliko koraka koje morate slijediti osim iznad da biste bili sigurni da je rješenje podrška za radni.

Najprije i u prvom redu, možda ćete morati provjerite jesu li geofences dinamičke. To je potrebno nekoliko dodatnih rad s Bing API da bi se moći prenositi nove ograničenja unutar postojećeg izvora podataka. Potražite u [dokumentaciji Bing prostorno podataka API Services](https://msdn.microsoft.com/library/ff701734.aspx) dodatne informacije o toj temi.

Drugo, kao što radite da biste bili sigurni isporuku obavlja desnom sudionicima, možda ćete morati usmjerili ih putem [za označavanje](notification-hubs-tags-segment-push-message.md).

Rješenje gore navedenoj sintaksi opisuje scenarij u kojem možda razna ciljne platforme pa ćemo ograničiti geofencing u mogućnosti specifične za sustav. Koje rečeno, univerzalni platforme Windows nudi mogućnosti da biste [otkrili geofences desnom Izlaz u-tvorničke](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

Dodatne informacije o mogućnostima koncentratora obavijesti potražite u članku našem [portal dokumentacije](https://azure.microsoft.com/documentation/services/notification-hubs/).
