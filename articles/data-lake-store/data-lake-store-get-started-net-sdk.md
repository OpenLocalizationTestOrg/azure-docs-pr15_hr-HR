<properties
   pageTitle="Korištenje podataka Lake spremište .NET SDK za razvoj aplikacija | Microsoft Azure"
   description="Korištenje Azure podataka Lake spremište .NET SDK za razvoj aplikacija"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Početak rada s spremišta Lake podataka za Azure pomoću .NET SDK-a

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-JA](data-lake-store-get-started-rest-api.md)
- [Azure EŽA](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saznajte kako koristiti [Azure podataka Lake spremište .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) za izvođenje osnovnih operacija kao što su prilikom stvaranja mape, prijenos i preuzimanje datoteke s podacima, itd. Dodatne informacije o Lake podataka potražite u članku [Lake spremišta podataka za Azure](data-lake-store-overview.md).

## <a name="prerequisites"></a>Preduvjeti

* **Visual Studio 2013 ili 2015**. Upute u nastavku koristite Visual Studio 2015.

* **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

* **Račun za azure podataka Lake trgovine**. Upute za stvaranje računa potražite u članku [Početak rada s Lake spremišta podataka za Azure](data-lake-store-get-started-portal.md)

* **Stvaranje aplikacije komponente Azure Active Directory**. Pomoću aplikacije za Azure AD spremišta podataka Lake aplikacijom Azure AD za provjeru autentičnosti. Postoje različite postupke za provjeru s Azure AD koje su **provjere autentičnosti za krajnjeg korisnika** ili **provjere autentičnosti servis za servis**. Upute i dodatne informacije o provjeri autentičnosti potražite u članku [provjere autentičnosti s trgovinom Lake podatke pomoću servisa Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Stvaranje aplikacije za .NET

1. Otvorite Visual Studio i stvorite aplikacije konzole.

2. Na izborniku **datoteka** kliknite **Novo**, a zatim **projekta**.

3. Iz **Novi projekt**, upišite ili odaberite sljedeće vrijednosti:

  	| Svojstvo | Vrijednost                       |
  	|----------|-----------------------------|
  	| Kategorija | Predlošci/Visual C# / sustava Windows |
  	| Predložak | Aplikacije konzole         |
  	| Ime     | CreateADLApplication        |

4. Kliknite **u redu** da biste stvorili projekta.

5. Dodajte Nuget paketa u projekt.

    1. Desnom tipkom miša kliknite naziv projekta u pregledniku rješenja, a zatim kliknite **Upravljanje NuGet paketa**.
    2. Na kartici **Upravitelj paketa Nuget** provjerite je li **paket izvora** postavljen na **nuget.org** i taj potvrdni okvir **Uključi predizdanja** odabran.
    3. Pretraživanje i instalacija sljedeće paketa NuGet:

        * `Microsoft.Azure.Management.DataLake.Store`– U ovom ćete praktičnom vodiču koristi v0.12.5 pretpregled.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`– U ovom ćete praktičnom vodiču koristi v0.10.6 pretpregled.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`– U ovom ćete praktičnom vodiču koristi v2.2.8 pretpregled.

        ![Dodavanje izvora Nuget] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Stvaranje novog računa Lake Azure podataka")

    4. Zatvorite **Upravitelj Nuget paketa**.

6. Otvorite **Program.cs**, izbrišite postojeće kod, a zatim uključite sljedeće naredbe za dodavanje reference na prostora naziva.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Deklariranje varijable, kao što je prikazano u nastavku i ponuditi vrijednosti za spremište podataka Lake ime i naziv grupe resursa koje već postoje. Osim toga, provjerite je li na lokalni put i naziv datoteke ovdje unesete mora se nalaziti na računalu. Dodajte sljedeće koda nakon deklaracija prostor naziva.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

U ostale sekcije u članku vidjet ćete kako koristiti dostupni načini .NET za izvođenje operacija kao što je provjera autentičnosti, prijenos datoteke i Dr.

## <a name="authentication"></a>Provjera autentičnosti

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Ako koristite provjeru autentičnosti krajnjeg korisnika (preporučuje se za ovog praktičnog vodiča)

Tom se mogućnošću poslužite s postojeće Azure AD "Nativni klijent" aplikacije; jedan je već unesen ispod. Da biste lakše brže dovršetak ovog praktičnog vodiča, preporučujemo da koristite ovaj pristup.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Nekoliko stvari koje treba znati o ovom iznad isječka.

* Da bi vam pomoći u obavljanju vodič brže, se koristi ovaj isječak pojavila Azure AD domene i klijent ID koji je dostupan kao zadano za sve pretplate za Azure. Tako, možete ga **koristiti ovaj isječak kao-je u aplikaciji**.
* Međutim, ako želite koristiti vlastitu Azure AD domene i aplikacije ID klijenta, morate stvoriti Azure AD matičnoj aplikaciji i zatim koriste domenu Azure AD, ID klijenta i preusmjerite URI za aplikaciju koju ste stvorili. Upute potražite u članku [Stvaranje aplikacije sustava Active Directory](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) .

>[AZURE.NOTE] Upute za gore navedene veze su za Azure AD web-aplikacije. No korake jednake čak i ako se odlučite umjesto toga stvoriti aplikaciju za nativni klijent. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Ako koristite servis za servis provjere autentičnosti s tajna klijenta 

Sljedeće isječaka može se koristiti za provjeru autentičnosti aplikacije koje nisu-interaktivno, pomoću klijentskog programa tajnu / ključ za programa Upravitelj aplikacija / servisa. Tom se mogućnošću poslužite s postojeće [Azure AD "Web App" aplikacije](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Ako koristite servis za servis provjera autentičnosti pomoću certifikata

Kao što je treću mogućnost sljedeće isječak mogu se provjeriti autentičnost aplikacije koje nisu-interaktivno, pomoću certifikata za programa Upravitelj aplikacija / servisa. Tom se mogućnošću poslužite s postojeće [Azure AD "Web App" aplikacije](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Stvaranje klijentskog objekata

Sljedeći isječak stvara spremišta podataka Lake račun i datotečnom sustavu klijent objekte koji se koriste za izdavanje zahtjeva za uslugu.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Popis svi računi spremišta Lake podataka u pretplatu

Sljedeći isječak navedeni svi računi spremišta Lake podataka u određenom Azure pretplate.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Stvorite direktorij

U sljedećem isječak s `CreateDirectory` metode koje možete koristiti da biste stvorili direktorij spremišta podataka Lake subjekta.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Prijenos datoteke

U sljedećem isječak u `UploadFile` metode koje možete koristiti da biste prenijeli datoteke na račun za spremište Lake podataka.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`podržava rekurzivne prijenos i preuzimanje između lokalne datoteke put i spremišta podataka Lake put datoteke.    

## <a name="get-file-or-directory-info"></a>Dohvaćanje datoteka ili direktorij informacije

U sljedećem isječak s `GetItemInfo` metode koje možete koristiti za dohvaćanje informacija o datoteci ili direktorija koje su dostupne u spremištu Lake podataka. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Popis datoteke ili mape

U sljedećem isječak s `ListItem` metode koje možete koristiti za prikaz popisa datoteka i direktorija u spremištu Lake podataka računa.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>CONCATENATE datoteke

U sljedećem isječak s `ConcatenateFiles` metoda koju ćete koristiti za spajanje datoteke. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Dodavanje datoteke

U sljedećem isječak s `AppendToFile` metoda koju ćete koristiti dodavanje podataka u datoteku već pohranjene u spremištu Lake podataka računa.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Preuzimanje datoteke

U sljedećem isječak s `DownloadFile` način koji koristite za preuzimanje datoteke s računa za spremište Lake podataka.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Daljnji koraci

- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Pohrana podataka Lake .NET SDK Reference](https://msdn.microsoft.com/library/mt581387.aspx)
- [Pohrana podataka Lake OSTALE Reference](https://msdn.microsoft.com/library/mt693424.aspx)
