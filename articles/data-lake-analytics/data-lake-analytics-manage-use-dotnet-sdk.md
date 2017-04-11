<properties 
   pageTitle="Upravljanje Azure podataka Lake analize pomoću Azure .NET SDK | Azure" 
   description="Saznajte kako upravljati zadacima analize podataka Lake, izvora podataka, a zatim korisnika. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/23/2016"
   ms.author="jgao"/>

# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Upravljanje Azure podataka Lake analize pomoću Azure .NET SDK

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informirajte se o upravljanju Lake analize podataka za Azure račune, izvora podataka, korisnici i zadatke pomoću Azure .NET SDK. Da biste vidjeli upravljanje teme pomoću drugih alata, kliknite odaberite karticu iznad.

**Preduvjeti**

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


## <a name="connect-to-azure-data-lake-analytics"></a>Povezivanje s analize Lake Azure podataka

Potrebno je sljedeće Nuget paketa:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Common 
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre


Sljedećim primjerom koda pokazuje kako se povezati s Azure i popis postojećih podataka Lake analize računa u odjeljku pretplate Azure.

    using System;
    using System.Collections.Generic;
    using System.Threading;

    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Analytics;
    using Microsoft.Azure.Management.DataLake.Analytics.Models;

    namespace ConsoleAcplication1
    {
        class Program
        {

            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.

            private static DataLakeAnalyticsAccountManagementClient _adlaClient;

            private static void Main(string[] args)
            {

                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);

                _adlaClient = new DataLakeAnalyticsAccountManagementClient(creds);
                _adlaClient.SubscriptionId = SUBSCRIPTIONID;

                var adlaAccounts = ListADLAAccounts();

                Console.WriteLine("You have %i Data Lake Analytics account(s).", adlaAccounts.Count);
                for (int i = 0; i < adlaAccounts.Count; i ++)
                {
                    Console.WriteLine(adlaAccounts[i].Name);
                }

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            public static ServiceClientCredentials AuthenticateAzure(
            string domainName,
            string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                return accounts;
            }
        }
    }


## <a name="manage-accounts"></a>Upravljanje računima

Prije pokretanja svih poslova analize podataka Lake, morate imati račun analize podataka Lake. Za razliku od servisom Azure HDInsight ne platiti račun analize kada je pokrenut posao.  Samo plaćate put kada je pokrenut posao.  Dodatne informacije potražite u članku [Pregled Analytics za Azure podataka Lake](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Stvaranje računa

Morate imati grupe Azure Upravljanje resursima sustava i račun spremišta podataka Lake mogli pokrenuti sljedeći primjer.

Sljedeći kod prikazuje kako stvoriti grupu resursa:

    public static async Task<ResourceGroup> CreateResourceGroupAsync(
        ServiceClientCredentials credential,
        string groupName,
        string subscriptionId,
        string location)
    {

        Console.WriteLine("Creating the resource group...");
        var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };
        var resourceGroup = new ResourceGroup { Location = location };
        return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
    }

Sljedeći kod prikazuje kako stvoriti račun spremišta Lake podataka:

    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

Sljedeći kod prikazuje kako stvoriti račun analize Lake podataka:

    var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
    var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
    var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: location);
    var adlaAccount = _adlaClient.Account.Create(resourceGroupName, adlaAccountName, adlaParameters);

###<a name="list-accounts"></a>Računi popisu

Potražite u članku [Povezivanje s analize Lake Azure podataka](#connect_to_azure_data_lake_analytics).

###<a name="find-an-account"></a>Pronalaženje računa

Kada primite objekta popis analize podataka Lake računa, možete koristiti sljedeće da biste pronašli račun:

    Predicate<DataLakeAnalyticsAccount> accountFinder = (DataLakeAnalyticsAccount a) => { return a.Name == adlaAccountName; };
    var myAdlaAccount = adlaAccounts.Find(accountFinder);

###<a name="delete-data-lake-analytics-accounts"></a>Brisanje računa analize podataka Lake

Sljedeći isječak koda briše analize podataka Lake računa:

    _adlaClient.Account.Delete(resourceGroupName, adlaAccountName);

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Upravljanje izvorima podataka na račun

Analitički Lake podataka trenutno podržava sljedećih izvora podataka:

- [Pohrana Lake Azure podataka](../data-lake-store/data-lake-store-overview.md)
- [Azure prostora za pohranu](../storage/storage-introduction.md)

Kada stvorite račun analize, mora se odrediti račun Lake pohrana podataka za Azure bude zadani račun za pohranu. Zadani račun za spremište Lake podataka koristi se za pohranu posao metapodataka i posao zapisnika nadzora. Kada stvorite račun analize, možete dodati dodatne račune za pohranu podataka Lake i/ili račun za Azure prostora za pohranu. 

### <a name="find-the-default-data-lake-store-account"></a>Pronalaženje zadanog računa za spremište Lake podataka

Pročitajte članak pronalaženje račun u ovom članku da biste pronašli analize podataka Lake računa. Koristiti sljedeće:

    string adlaDefaultDataLakeStoreAccountName = myAccount.Properties.DefaultDataLakeStoreAccount;


## <a name="use-azure-resource-manager-groups"></a>Korištenje upravitelja resursa Azure grupa

Aplikacija obično se sastoje od više komponente, primjerice web-aplikacijama, baze podataka, poslužitelj baze podataka, za pohranu i 3 davatelja usluga. Azure Voditelj resursa omogućuje rad s resursima u aplikaciji grupe se nazivaju i grupu resursa Azure. Možete uvesti, ažuriranje, praćenje i brisanje svi resursi aplikacije u jednom, usklađenih postupak. Pomoću predloška za implementaciju i taj predložak možete iskoristiti različitim okruženjima kao što su testiranje pripremna i proizvodnje. Pregledom kumulativne troškove za cijelu grupu možete pojašnjavaju naplate za tvrtku ili ustanovu. Dodatne informacije potražite u članku [Pregled upravljanja resursima Azure](../azure-resource-manager/resource-group-overview.md). 

Servis za analize podataka Lake mogu sadržavati sljedeće komponente:

- Račun za Azure podataka Lake analize
- Potrebne zadani račun za Azure podataka Lake prostora za pohranu
- Dodatni Azure podataka Lake prostora za pohranu računa
- Dodatne račune za pohranu za Azure

Možete stvoriti te komponente u odjeljku jedne grupe Upravljanje resursima radi lakšeg upravljanja.

![Račun za Azure podataka Lake analize i prostora za pohranu](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Analize podataka Lake računa i račune zavisne prostora za pohranu mora biti smješten u isti Azure podatkovnog centra.
Grupa Upravljanje resursima no može se nalaziti u centru za različite podatke.  

##<a name="see-also"></a>Vidi također 

- [Pregled analize podataka Lake za Microsoft Azure](data-lake-analytics-overview.md)
- [Početak rada s podacima Lake analize pomoću portala za Azure](data-lake-analytics-get-started-portal.md)
- [Upravljanje analizom Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-manage-use-portal.md)
- [Praćenje i rješavanje problema s poslove Lake analize podataka za Azure pomoću portala za Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

