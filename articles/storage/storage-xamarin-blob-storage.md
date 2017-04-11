<properties
    pageTitle="Upute za korištenje spremišta blobova iz Xamarin | Microsoft Azure"
    description="U biblioteci klijent za Azure prostora za pohranu za Xamarin omogućuje inženjerima omogućuje stvaranje iOS, Android i Windows aplikacije iz trgovine s nativni korisničkih sučelja. Pomoću ovog praktičnog vodiča pokazuje kako koristiti Xamarin za stvaranje aplikacija koja koristi spremište blobova platforme Azure."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Upute za korištenje spremišta blobova iz Xamarin

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Pregled

Da biste stvorili iOS, Android i Windows trgovine aplikacija njihov izvorni korisnička sučelja kodna baza Xamarin razvojnim inženjerima omogućuje korištenje zajedničke C#. Pomoću ovog praktičnog vodiča pokazuje kako koristiti spremište blobova platforme Azure s aplikacijom Xamarin. Ako želite da biste saznali više o Azure prostor za pohranu, prije čitanje kod, potražite u članku [Uvod u spremište na platformi Microsoft Azure](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Stvaranje nove aplikacije Xamarin

Za ovaj početak rada, ne možemo ćete stvoriti aplikaciju namijenjen za Android, iOS i Windows. Aplikacija će jednostavno stvaranje spremnika i prenijeti blob u ovom spremniku. Ne možemo hoćete li koristiti Visual Studio u sustavu Windows za početak rada, ali isti learnings se mogu primijeniti prilikom stvaranja aplikacije pomoću Xamarin Studio sustavu Mac OS.

Slijedite ove korake da biste stvorili aplikacija:

1. Ako to već niste učinili, preuzmite i instalirajte [Xamarin za Visual Studio](https://www.xamarin.com/download).
2. Otvorite Visual Studio i stvorite prilagođenom web-aplikacijom (nativni zajednički): **Datoteka > novo > projekt > više platformi > prazna App(Native Shared)**.
3. Desnom tipkom miša kliknite rješenje u oknu Eksplorer za rješenja, a zatim odaberite **Upravljanje NuGet paketa rješenja**. Traženje **WindowsAzure.Storage** i instalirajte najnoviju verziju stabilan u svim projektima rješenje.
4. Stvaranje i pokretanje projekta.

Trebala bi aplikacija koja vam omogućuje da kliknete gumb koji se povećava brojač.

> [AZURE.NOTE] U biblioteci klijenta za Azure prostora za pohranu za Xamarin trenutno podržava sljedećih vrsta projekta: nativni zajednički koristi, zajednički koristiti Xamarin.Forms, Xamarin.Android i Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Stvaranje spremnika i prijenos blobova platforme

Nakon toga će dodavanje koda za neke zajedničke klase `MyClass.cs` koji stvara spremnik i prenese blob u ovom spremniku. `MyClass.cs`trebao bi izgledati ovako:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Provjerite je li zamijeniti "your_account_name_here" i "your_account_key_here" naziv stvarni računa i ključ za račun. Možete koristiti zajedničku klase iOS, Android i Windows Phone aplikacije. Možete jednostavno dodati `MyClass.createContainerAndUpload()` za svaki projekt. Ako, na primjer:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Pokrenite aplikaciju

Sada možete pokrenuti ovu aplikaciju u emulator sustava Android ili Windows Phone. Možete i pokrenuti ovu aplikaciju u emulator sustava iOS, ali to je potrebno maca. Detaljne upute o tome kako to učiniti, pročitajte dokumentaciju o [povezivanju sa servisom Visual Studio za Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Kada pokrenete aplikaciju, stvorit će spremnik `mycontainer` na vašem računu za pohranu. Ona mora sadržavati blob-om, `myblob`, koji sadrži tekst, `Hello, world!`. To možete provjeriti pomoću [Programa Explorer prostora za pohranu za Microsoft Azure](http://storageexplorer.com/).

## <a name="next-steps"></a>Daljnji koraci

U ovom početak rada, naučili kako stvoriti različite platforme aplikacije u Xamarin koji koristi pohranu Azure. Ovaj uvodni posebno filtriran na jedan scenarij u spremište blobova platforme. Međutim, to možete učiniti mnogo više s ne samo blobova, ali i tablice, datoteke i reda čekanja za pohranu. Pogledajte sljedeće članke dodatne informacije:
- [Početak rada s blobova platforme Azure pomoću .NET](storage-dotnet-how-to-use-blobs.md)
- [Početak rada sa spremištem tablica Azure pomoću .NET](storage-dotnet-how-to-use-tables.md)
- [Početak rada s spremištem reda čekanja Azure pomoću .NET](storage-dotnet-how-to-use-queues.md)
- [Početak rada s Azure pohranu datoteka u sustavu Windows](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
