<properties
    pageTitle="Prilagodba klastere HDInsight pomoću akcije skripte | Microsoft Azure"
    description="Saznajte kako prilagoditi klastere HDInsight pomoću skripte akcije."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Prilagodba klastere utemeljen na sustavu Windows HDInsight pomoću skripte akcije

**Skripta akcija** se može koristiti za pozivanje [prilagođene skripte](hdinsight-hadoop-script-actions.md) tijekom procesa stvaranja klaster za instalaciju dodatnog softvera na klaster.

Informacije u ovom članku je za klastere HDInsight utemeljen na sustavu Windows. Sustavom Linux klastere potražite u članku [Prilagodba Linux sustavom HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster-linux.md). 

HDInsight klastere moguće je prilagoditi na brojne kao i drugi načini kao što su uključujući dodatne račune za pohranu Azure, promjene u Hadoop konfiguracije datoteke (core site.xml grozd site.xml, itd.) ili dodavanje zajedničke biblioteke (npr., grozd, Oozie) u uobičajena mjesta u klasteru. Te prilagodbe mogu se izvršiti kroz Azure PowerShell, SDK za .NET Azure HDInsight ili portala za Azure. Dodatne informacije potražite u članku [Stvaranje Hadoop klastere u HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Skripta akcija u postupku stvaranja klaster

Akcija skripte koristi samo dok je u tijeku stvoren je u klastere. Na sljedećem su dijagramu ilustrira prilikom izvršenja akcije skripte tijekom procesa stvaranja:

![HDInsight klaster prilagodbe i faze tijekom stvaranja klaster][img-hdi-cluster-states]

Kada je pokrenut skriptu, klaster unosi faza **ClusterCustomization** . U ovoj fazi skripta se izvodi u odjeljku administratorskog računa sustava paralelno na navedeni čvorovi u klasteru, i omogućuje potpunu administratorske ovlasti na čvorove.

> [AZURE.NOTE] Jer nemate administratorske ovlasti na čvorove klaster tijekom faze **ClusterCustomization** , poslužite se skripta za izvođenje operacija kao što su zaustavljanje i pokretanje servisa, uključujući vezane uz Hadoop usluge. Tako, kao dio skriptu, koje morate Ambari servise i drugim servisima za vezane uz Hadoop provjerite jesu li s radom prije skriptu završi s radom. Da biste uspješno provjerili stanja i stanje klaster dok je stvoren moraju tih servisa. Ako promijenite sve konfiguracije na klasteru koje utječu na te servise, morate koristiti funkcije preglednika koje služe. Dodatne informacije o funkcijama preglednika potražite u članku [razviti akcija skripte skripte za HDInsight][hdinsight-write-script].

Izlaz i Evidencija pogrešaka za skripte spremaju se u zadani račun za pohranu koje ste naveli za klaster. U zapisnicima spremaju se u tablicu pod nazivom **u < \cluster-name-fragment >< \time-stamp > setuplog**. To su prikupljanje zapisnika iz skripte pokrenuti na sve čvorove (glavni čvor i tempiranja čvorove) u klasteru.

Svaki klaster možete prihvatiti više akcija skriptu koja se poziva redoslijedom u kojem su navedeni. Skripta se može pokrenuli na glavni čvor, čvorove radnih ili i jedno i drugo.

HDInsight nudi nekoliko skripte na HDInsight klastere instalirati sljedeće komponente:

Ime | Skripta
----- | -----
**Instalacija Spark** | https://hdiconfigactions.blob.Core.Windows.NET/sparkconfigactionv03/spark-Installer-v03.ps1. Potražite u članku [Instalacija i korištenje povećati klastere HDInsight][hdinsight-install-spark].
**Instalacija R** | https://hdiconfigactions.blob.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Potražite u članku [Instalacija i korištenje R na klastere HDInsight][hdinsight-install-r].
**Instalacija Solr** | https://hdiconfigactions.blob.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Potražite u članku [Instalacija i korištenje klaster Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- **Instalacija Giraph** | https://hdiconfigactions.blob.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Potražite u članku [Instalacija i korištenje klaster Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).
| **Unaprijed učitati grozd biblioteke** | https://hdiconfigactions.blob.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Potražite u članku [Dodavanje vrste Hive biblioteka na klastere HDInsight](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Skripte pomoću portala za Azure

**Na portalu za Azure**

1. Stvaranje klaster kao što je opisano na [Stvaranje Hadoop klastere u HDInsight](hdinsight-provision-clusters.md#portal).
2. U odjeljku neobavezno konfiguracija plohu **Akcije skripte** kliknite **dodajte akciju skripte** možete unijeti detalje o akciju skripte kao što je prikazano u nastavku:

    ![Korištenje akcija skriptu da biste prilagodili klaster] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Korištenje akcija skriptu da biste prilagodili klaster")

    <table border='1'>
        <tr><th>Svojstvo</th><th>Vrijednost</th></tr>
        <tr><td>Ime</td>
            <td>Unesite naziv za skripte akciju.</td></tr>
        <tr><td>Skripta URI-JA</td>
            <td>Navedite URI za skriptu koja se poziva da biste prilagodili klaster. s</td></tr>
        <tr><td>Zaglavlje/tempiranja</td>
            <td>Odredite čvorove (**Zaglavlje** ili **tempiranja**) na koji je pokrenuti skriptu prilagodbe. </b>.
        <tr><td>Parametri</td>
            <td>Navedite parametre, ako je potrebno skripta.</td></tr>
    </table>

    Pritisnite ENTER da biste dodali više akcija skriptu da biste instalirali više komponenti na klaster.

3. Kliknite da biste spremili akcija konfiguracije skripte i nastavak stvaranja klaster **Odaberite** .

## <a name="call-scripts-using-azure-powershell"></a>Skripte pomoću komponente PowerShell Azure

U ovom sljedeću skriptu komponente PowerShell pokazuje kako instalirati Spark na Windows na temelju HDInsight klaster.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Da biste instalirali drugi softver, morat ćete zamijeniti datoteka skripte u skripti:


Kada se to od vas zatraži, unesite vjerodajnice za klaster. To može potrajati nekoliko minuta prije stvaranja klaster.

## <a name="call-scripts-using-net-sdk"></a>Poziv skripti pomoću .NET SDK 

Sljedeći primjer pokazuje kako instalirati Spark na Windows na temelju HDInsight klaster. Da biste instalirali drugi softver, morate zamijeniti datoteka skripte u kodu.

**Da biste stvorili programa klaster HDInsight Spark** 

1. Stvaranje konzole aplikacije C# u Visual Studio.
2. S konzole Upravitelj paketa Nuget pokrenite sljedeću naredbu.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Koristite sljedeće pomoću naredbi u datoteci Program.cs:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Potvrdite kod predmete sa sljedećim:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Pritisnite **F5** da biste pokrenuli aplikaciju.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Podrška za Otvori izvor softver koji se koriste na klastere HDInsight
Servis Microsoft Azure HDInsight je fleksibilne platformu koja vam omogućuje sastavljanje aplikacije velikih skupova podataka u oblak pomoću programa zajednici Otvori izvor tehnologije oblikovan oko Hadoop. Microsoft Azure nudi Općenito razina podrške za tehnologije Otvori izvor, kako je opisano u odjeljku **Podržava doseg** <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure podržava najčešća pitanja vezana uz web-mjesta</a>. Servis za HDInsight nudi dodatnu razinu podrške za neke komponente, prema uputama u nastavku.

Postoje dvije vrste Otvori izvor komponente koje su dostupne u servisa HDInsight:

- **Ugrađene komponente** - ove komponente unaprijed instaliranih na klastere HDInsight i funkcionalnosti core od klaster. Ako, na primjer, YARN ResourceManager, jezika za upite grozd (HiveQL) i biblioteku Mahout pripada kategoriju. Potpuni popis komponenata klaster dostupna je u [što je novo u verzijama klaster Hadoop nudi HDInsight?](hdinsight-component-versioning.md) </a>.
- **Prilagođeno komponente** -, kao korisnik klaster, možete instalirati ili koristiti u svoje radno opterećenje bilo koju komponentu dostupni u zajednici ili koje ste stvorili.

Ugrađene komponente potpuno podržane, a Microsoft Support ćete lakše izdvajanja i rješavanje problema vezanih uz te komponente.

> [AZURE.WARNING] Komponente dao klaster HDInsight potpuno podržane, a Microsoft Support pomoći će vam da biste izdvojili i rješavanje problema vezanih uz te komponente.
>
> Prilagođene komponente dobili komercijalno pametnije podršku radi daljnje rješavanje problema. To može rezultirati rješavanju problema ili s pitanjem želite li sudjelovati dostupnih kanala tehnologija Otvori izvor gdje se nalazi niže stručna znanja za taj tehnologiju. Na primjer, postoje mnogo web-mjesta zajednice koje je moguće koristiti, npr.: [MSDN forum za HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekti Apache imaju web-mjesta projekta na [http://apache.org](http://apache.org), na primjer: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

Servis za HDInsight nudi nekoliko načina za korištenje prilagođene komponente. Bez obzira na to kako koristiti ili instalirano klaster komponentu, istu razinu podrške odnosi. Slijedi popis Najčešći su načini prilagođene komponente može se koristiti u HDInsight klastere:

1. Predavanje posla - Hadoop ili drugih vrsta zadataka koji se izvoditi ili koristiti prilagođene komponente mogu poslati klaster.
2. Prilagodba klaster – tijekom stvaranja klaster možete navesti dodatne postavke i prilagođene komponente koje će biti instalirana na čvorove klaster.
3. Uzorke - za popularne prilagođene komponente, Microsoft i drugi korisnici mogu uzoraka kako koristiti te komponente na klastere HDInsight. Ta uzorka postoje bez podrške.

## <a name="develop-script-action-scripts"></a>Razvoj skripti skripte akcija

[Razvoj akcija skripte skripte za HDInsight]potražite u članku[hdinsight-write-script].


## <a name="see-also"></a>Vidi također

- [Stvaranje klastere Hadoop u HDInsight] [ hdinsight-provision-cluster] sadrži upute o stvaranju programa klaster HDInsight pomoću drugih prilagođene mogućnosti.
- [Razvoj skripti skripte akcija za HDInsight][hdinsight-write-script]
- [Instaliranje i korištenje Spark na klastere HDInsight][hdinsight-install-spark]
- [Instaliranje i korištenje R na klastere HDInsight][hdinsight-install-r]
- [Instalacija i korištenje klaster Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- [Instalacija i korištenje klaster Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Faza tijekom stvaranja klaster"
