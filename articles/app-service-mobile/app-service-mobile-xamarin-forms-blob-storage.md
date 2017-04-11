<properties
    pageTitle="Povezivanje sa spremištem Azure aplikaciji Xamarin.Forms"
    description="Dodavanje slika obveze mobilne aplikacije popis Xamarin.Forms povezivanje sa spremištem blobova platforme Azure"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Povezivanje sa spremištem Azure aplikaciji Xamarin.Forms

## <a name="overview"></a>Pregled

Azure mobilne aplikacije klijenta i poslužitelja SDK podrške Izvanmrežna sinkronizacija strukturiranih podataka CRUD operacije u odnosu na krajnjoj točki /tables. Obično ovo podaci se pohranjuju u bazi podataka ili slične pohrane i obično ne možete tih podataka trgovine učinkovito spremiti velike binarne podatke. Osim toga, neke aplikacije imate povezane podatke koji su pohranjena negdje drugdje (npr., blobova, zajedničko korištenje datoteka) te je korisno omogućiti stvaranje veza između zapisa u krajnju točku /tables i ostale podatke.

U ovoj se temi objašnjava dodali podrške za slike u popis brzi početak rada obveze mobilne aplikacije. Najprije morate dovršiti vodič [Stvaranje Xamarin.Forms aplikacije].

U ovom ćete praktičnom vodiču će stvoriti račun za pohranu i dodajte niza za povezivanje pozadinskog sustava mobilnu aplikaciju. Zatim će dodati u novi nasljeđuju novu vrstu aplikacije Mobile `StorageController<T>` u projekt poslužitelja.

>[AZURE.TIP] Pomoću ovog praktičnog vodiča sadrži [ogledne pomoćnom](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) dostupna, koji mogu biti implementirano u vlastiti račun za Azure. 

## <a name="prerequisites"></a>Preduvjeti

* Dovršite vodič za [Stvaranje Xamarin.Forms aplikacije] koji navodi druge preduvjeti. U ovom se članku koristi dovršene aplikacije iz tog vodiča.

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](https://tryappservice.azure.com/?appServiceName=mobile). Postoji, možete odmah stvoriti short-lived starter aplikacije za mobilne uređaje u aplikacije servisa za – bez kreditne kartice potrebna, a ne preuzete obveze.

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

1. Stvaranje računa za pohranu slijedeći vodič [stvorite račun za Azure prostora za pohranu]. 

2. Na portalu Azure idite na račun servisa novostvorenu prostora za pohranu, a zatim kliknite ikonu **tipke** . Kopirajte **niz za povezivanje primarni**.

3. Dođite do svoje pozadinskog mobilne aplikacije. U odjeljku **Postavke za sve** -> **Postavke aplikacije** -> **Nizove veze**, stvorite novi ključ pod nazivom `MS_AzureStorageAccountConnectionString` i koristite vrijednost kopirali s računa za pohranu. Korištenje **prilagođene** kao vrsta ključa.

## <a name="add-a-storage-controller-to-the-server"></a>Dodavanje prostora za pohranu kontroler na poslužitelj

Morate dodati novi kontroler u projekt poslužitelja koji će odgovora na zahtjeve za SAS token za pohranu Azure, kao i vratiti popis datoteka koje odgovaraju zapis:

- [Dodavanje prostora za pohranu kontroler sustava project server](#add-controller-code)
- [Usmjerava registrirane kontrolerom prostora za pohranu](#routes-registered)
- [Postupak klijentske i poslužiteljske komunikacije](#client-communication)

###<a name="add-controller-code"></a>Dodavanje prostora za pohranu kontroler sustava project server

1. U Visual Studio, otvorite projekta poslužitelja za .NET. Dodavanje paketa Nuget [Microsoft.Azure.Mobile.Server.Files]. Obavezno odaberite **Uključi predizdanja**.

2. U Visual Studio, otvorite projekta poslužitelja za .NET. Desnom tipkom miša kliknite mapu **kontrolera** , a zatim odaberite **Dodaj** -> **kontroler** -> **2 kontroler API Web - Isprazni**. Naziv kontrolerom `TodoItemStorageController`.

3. Dodajte sljedeće pomoću naredbe:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Promjena klasu osnovni `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Dodati klasa na sljedeće načine:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Ažurirajte konfiguracije Web API da biste postavili atribut usmjeravanja. U **Startup.MobileApp.cs**, dodajte sljedeći redak da biste na `ConfigureMobileApp()` metoda nakon definicije na `config` varijabla:

        config.MapHttpAttributeRoutes();

7. Objavite projekt poslužitelja za vaše pozadinskog mobilne aplikacije.

###<a name="routes-registered"></a>Usmjerava registrirane kontrolerom prostora za pohranu

Novi `TodoItemStorageController` izlaže dva pod resurse u odjeljku zapis je:

- StorageToken

    + HTTP POST: Stvara token za pohranu
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Vraća popis datoteka povezana sa zapisom
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + Izbriši HTTP: Briše otvorenu datoteku navedenu u identifikator resursa datoteka
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Postupak klijentske i poslužiteljske komunikacije

Imajte na umu da `TodoItemStorageController` ne *neće* imati usmjeravanje za prijenos ili preuzimanje blob. To je zato mobilnu verziju klijentskog stupi u interakciju s blob prostora za pohranu *izravno* da bi se izvoditi ove postupke nakon početak SAS token (zajednički pristup potpisa) da biste sigurno pristupali određeni blob ili spremnik. To je važno arhitektonski dizajn, kao što je drukčije pristup pohrani će biti ograničeno količinom skalabilnost i dostupnost mobilne pozadinskog. Umjesto toga povezivanjem izravno sa spremištem Azure mobilnog klijenta možete iskoristiti njegovim značajkama kao što je automatsko particija i zemlj.-distribuciju.

Potpis zajednički pristup omogućuje delegirana pristup resursa u račun za pohranu. To znači da možete dodijeliti klijent ograničene dozvole za objekte na vašem računu za pohranu za određeno vrijeme i određeni skup dozvola, bez potrebe za zajedničko korištenje ključeva za pristup računu. Dodatne informacije potražite u članku [Objašnjenje zajednički pristup potpisa].

Dijagramu u nastavku prikazuje postupak klijentske i poslužiteljske interakcije. Prije prijenosa datoteke klijent zahtijeva SAS tokena iz servisa. Servis koristi niz za povezivanje za pohranu da biste generirali nove SAS koja zatim vraća klijentu. Na SAS je ograničeno vrijeme i ograničava dozvole samo određene datoteke ili spremnik. Mobilnu verziju klijentskog zatim koristi ovaj SAS i Azure prostora za pohranu klijenta SDK prenijeti datoteku sa spremištem blobova.

![Traženje SAS tokena](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Ažuriranje aplikacije klijenta da biste dodali sliku podrška

Otvaranje projekta za brzi početak rada Xamarin.Forms u Visual Studio ili Xamarin Studio. Će se instalirati Nuget paketi i ažuriranje projekta prijenosni biblioteke i iOS, Android i Windows klijent projekata:

- [Dodavanje Nuget paketa](#add-nuget)
- [Dodavanje IPlatform sučelja](#add-iplatform)
- [Dodavanje FileHelper klase](#add-filehelper)
- [Dodavanje rukovatelj za sinkronizaciju datoteka](#file-sync-handler)
- [Ažuriranje TodoItemManager](#update-todoitemmanager)
- [Dodavanje prikaza detalja](#add-details-view)
- [Ažuriranje glavni prikaz](#update-main-view)
- [Ažuriranje Android projekt](#update-android), [iOS projekta](#update-ios), [projekt programa Windows](#update-windows)

>[AZURE.NOTE] Pomoću ovog praktičnog vodiča samo sadrži upute za Android, iOS i platforme iz Windows trgovine, ne Windows Phone.

###<a name="add-nuget"></a>Dodavanje Nuget paketa

Desnom tipkom miša kliknite rješenje, a zatim odaberite **Upravljanje Nuget paketa rješenja**. Dodajte sljedeće Nuget paketa u **svim** projektima rješenja. Obavezno provjerite **sadrže predizdanja**.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Pogodnost, ovaj primjer koristi [PCLStorage] biblioteke, ali je potreban klijent Azure mobilne aplikacije SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>Dodavanje IPlatform sučelja

Stvorite novo sučelje `IPlatform` u programu project glavni prijenosni biblioteke. To slijedi uzorak [Xamarin.Forms DependencyService] da biste učitali predmete desno ovisne prilikom izvođenja. Kasnije ćete dodati ovisne implementacije u svim projektima klijenta.

1. Dodajte sljedeće pomoću naredbe:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. Zamijenite implementaciju učinili sljedeće:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>Dodavanje FileHelper klase

1. Stvaranje nove klase `FileHelper` u programu project glavni prijenosni biblioteke. Dodajte sljedeće pomoću naredbe:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Dodajte definiciju klasa:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Dodavanje rukovatelj za sinkronizaciju datoteka

Stvaranje nove klase `TodoItemFileSyncHandler` u programu project glavni prijenosni biblioteke. Klase sadrži pozive iz SDK Azure će obavijestiti kod prilikom dodavanja ili ukloniti datoteku.

Klijent SDK za Azure Mobile ne sadrže podatke iz datoteke: klijent SDK poziva implementaciju sustava `IFileSyncHandler` koji shodno određuje želite li i kako se datoteke spremaju na lokalnim uređajem.

1. Dodajte sljedeće pomoću naredbe:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Zamijenite definiciju klase sljedeće: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>Ažuriranje TodoItemManager

1. U **TodoItemManager.cs**, uklonite redak `#define OFFLINE_SYNC_ENABLED`.

2. U **TodoItemManager.cs**, dodajte sljedeće pomoću naredbe:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. U Graditelj od `TodoItemManager`, dodajte sljedeće nakon poziv na `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. U Graditelj, zamijenite poziv `InitializeAsync` sa sljedećim. To će provjeriti postoje li pozive prilikom izmjene zapisa u lokalnom spremištu. Značajke sinkronizacije datoteka koristi te pozive za pokretanje sustava rukovatelj sinkronizaciju datoteka.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. U `SyncAsync()`, dodajte sljedeće nakon poziv na `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Dodavanje načina za `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Dodavanje prikaza detalja

U ovom odjeljku ćete dodati novi prikaz detalja o za stavku obveze. Prikaz stvara se kada korisnik odabere stavku obveze i omogućuje nove slike koja će se dodati na stavku.

1. Dodavanje novog klase **TodoItemImage** projekt prijenosni biblioteke sa sljedećim implementacije:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. Uređivanje **App.cs**. Zamjena inicijalizaciju `MainPage` sa sljedećim:
    
        MainPage = new NavigationPage(new TodoList());

3. U **App.cs**, dodajte sljedeća svojstva:

        public static object UIContext { get; set; }

4. Desnom tipkom miša kliknite projekt prijenosni biblioteke, a zatim odaberite **Dodaj** -> **Nova stavka** -> **različite platforme** -> **Obrazaca Xaml stranice**. Naziv prikaza `TodoItemDetailsView`.

5. Otvorite **TodoItemDetailsView.xaml** i zamijenite tijelo na ContentPage sljedeće:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. Uređivanje **TodoItemDetailsView.xaml.cs** i dodajte sljedeće pomoću naredbe:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Zamjena implementacije `TodoItemDetailsView` sa sljedećim:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>Ažuriranje glavni prikaz 

Ažurirajte glavni prikaz da biste otvorili prikaz detalja o odabrano stavke na popis obveza.

U **TodoList.xaml.cs**zamijenite implementacije `OnSelected` sa sljedećim:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Ažuriranje Android projekta

Dodavanje koda za ovisne Android projekta, uključujući kod za preuzimanje datoteke i koristi kameru da biste snimili novu sliku. 

Kod koristi Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) da biste učitali predmete desno ovisne prilikom izvođenja.

1. Dodajte komponentu **Xamarin.Mobile** Android projekt.

2. Dodavanje novog klase `DroidPlatform` sa sljedećim implementacije. "YourNamespace" zamijenite glavni naziva projekta.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Uređivanje **MainActivity.cs**. U `OnCreate`, dodajte sljedeće prije poziv na `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>Ažuriranje projekta za iOS

Dodavanje koda za ovisne iOS projekta.

1. Dodajte komponentu **Xamarin.Mobile** iOS projekt.

2. Dodavanje novog klase `TouchPlatform` sa sljedećim implementacije. "YourNamespace" zamijenite glavni naziva projekta.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Uređivanje **AppDelegate.cs** i uklonite poziv na `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Projekt programa Windows Update

1. Instalirajte proširenje za Visual Studio [SQLite za Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Dodatne informacije potražite u članku vodič [Omogućivanje izvanmrežne sinkronizacije za aplikacija za Windows](app-service-mobile-windows-store-dotnet-get-started-offline-data.md). 

2. Uredite **Package.appxmanifest** i provjera u **web-kamere** .

3. Dodavanje novog klase `WindowsStorePlatform` sa sljedećim implementacije. "YourNamespace" zamijenite glavni naziva projekta.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Sažetak

U ovom se članku opisuje kako koristiti novu podršku za datoteke u Azure mobilnog klijenta i poslužitelja SDK za rad s Azure prostora za pohranu. 

- Stvaranje računa za pohranu i dodajte niz za povezivanje u pozadinskom mobilne aplikacije. Samo pozadinski ima ključ za pohranu Azure: mobilnu verziju klijentskog zahtjeve SAS token (zajednički pristup potpisa) kad god je potrebno da biste pristupili Azure prostora za pohranu. Da biste saznali više o SAS tokena iz spremišta Azure, potražite u članku [Objašnjenje zajednički pristup potpisa].

- Stvaranje kontroler te podklase `StorageController` da bi upravljati zahtjevima tokena SAS te za dohvaćanje datoteka koje su pridružene zapisu. Po zadanom su datoteke pridružena zapisu pomoću ID zapisa kao dio naziva spremnik; moguće je prilagoditi ponašanje navođenjem implementacija `IContainerNameResolver`. Pravila tokena SAS moguće je prilagoditi i.

- SDK klijenta za mobilne Azure pohranu sadrže podatke iz datoteke. Umjesto toga klijent SDK poziva na `IFileSyncHandler`, koji se zatim odlučuje kako (i ako) datoteke spremaju na lokalnim uređajem. Rukovatelj sinkronizaciju registriran na sljedeći način:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`bit će pozvan kada SDK Azure mobilnog klijenta mora podatkovne datoteke (npr., u sklopu postupka prijenos). Tako ćete dobiti mogućnost upravljanje kako (i ako) datoteke spremaju na lokalnim uređajem i vratili se te podatke po potrebi.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`se poziva kao dio tijeka sinkronizacije datoteka. Referenca za datoteke i numeriranje vrijednost FileSynchronizationAction postoje tako da možete odlučiti koliko aplikacije tretirati taj događaj (npr. automatsko preuzimanje datoteke prilikom stvaranja ili ažuriranje, brisanje datoteke s lokalnim uređajem nakon brisanja tu datoteku na poslužitelju).

- A `MobileServiceFile` može koristiti u mrežnog i izvanmrežnog načina pomoću na `IMobileServiceTable` ili `IMobileServiceSyncTable`, odnosno. U izvanmrežni scenarij prijenos pojavit će se prilikom poziva aplikaciju `PushFileChangesAsync`. Zbog toga red izvanmrežne postupak da biste se obrađuju za svaku datoteku operaciju klijent za Azure Mobile SDK će pozivanje na `GetDataSource` način na na `IFileSyncHandler` instancu da biste dohvatili sadržaj datoteke za prijenos.

- Dohvaćanje datoteka na stavku poziva na "GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` instance. Ta metoda vraća navedeni popis datoteka povezana sa stavkom podataka. (Napomena: Ovo je *lokalne* operacije koja će vratiti datoteke na temelju stanje objekta kada je zadnji put sinkronizirani. Da biste dobili ažurirani popis datoteka s poslužitelja, trebali biste pokrenete operaciju sinkronizacije najprije.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- Značajke sinkronizacije datoteka koristi obavijesti o promjeni zapisa na lokalnom spremištu dohvaćanje zapisa koji klijent primili kao dio automatske ili istaknuti postupak. Pritom se uključite obavijesti o lokalnom i poslužitelj za korištenje sinkronizacije kontekst u `StoreTrackingOptions` parametar. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Druge mogućnosti praćenja spremište dostupnih, kao što su obavijesti samo za lokalne ili samo na poslužitelju. Možete dodati ili vlasnik pomoću prilagođenih povratnog na `EventManager` svojstvo `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Nije moguće dodati ili ukloniti datoteke sa zapisom izmjenom blobova izravno Nakon pridruživanja se postiže putem programa konvencija imenovanja. Međutim, u ovom slučaju, trebali biste uvijek **Ažuriranje vremenske oznake zapis kada mijenjaju se povezana blob-ova**. Klijent za Azure Mobile SDK uvijek ažurira zapis prilikom dodavanja ili uklanjanja datoteke. 

    Razlog za taj zahtjev je da neki mobilni klijenti će već zapis u lokalno spremište. Kada te klijenti rastuće istaknuti, taj zapis neće vratiti i klijent će upit za nove pridružene datoteke. Da biste izbjegli taj problem, preporučuje se ažuriranje zapisa vremenske oznake prilikom izvršavanja promjene spremište blobova platforme koji ne koristi klijent za Azure Mobile SDK.

- Klijent projekt koristi uzorak [Xamarin.Forms DependencyService] da biste učitali predmete desno ovisne vrijeme izvođenja. U ovom primjeru definirali sučelja `IPlatform` s implementacije u svakoj ovisne projekata.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Stvaranje Xamarin.Forms aplikacije]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Razumijevanje zajednički pristup potpisa]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Stvorite račun za Azure prostora za pohranu]:  ../storage/storage-create-storage-account.md#create-a-storage-account
