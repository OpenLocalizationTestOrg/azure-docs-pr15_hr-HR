<properties
    pageTitle="Pokušajte SQL baza podataka: Korištenje C# za stvaranje baze podataka SQL | Microsoft Azure"
    description="Pokušajte SQL baze podataka za razvoj aplikacija za SQL i C# i stvaranje baze podataka SQL Azure s C# pomoću SQL baza podataka biblioteke za .NET."
    keywords="Pokušajte sql, sql c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="sstein"/>

# <a name="try-sql-database-use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Pokušajte SQL baza podataka: Korištenje C# za stvaranje baze podataka SQL biblioteku baze podataka SQL za .NET


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Saznajte kako koristiti C# za stvaranje baze podataka Azure SQL s [Microsoft Azure SQL upravljanje biblioteka za .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). U ovom se članku opisuje kako stvoriti jednu bazu podataka s SQL i C#. Stvaranje grupe elastic baze podataka potražite u članku [Stvaranje grupe aplikacija programa Elastic baze podataka](sql-database-elastic-pool-create-csharp.md).

Upravljanje biblioteka Azure baze podataka SQL za .NET omogućuje [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md)-temelji API koji se prelama [Resursima sustavom SQL baza podataka REST API -JA](https://msdn.microsoft.com/library/azure/mt163571.aspx).

>[AZURE.NOTE] Mnoge nove značajke baze podataka SQL podržani su samo ako koristite [model implementacije Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md), tako da uvijek koristite najnoviji **biblioteke za upravljanje Azure SQL baze podataka za .NET ([Dokumenti](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet paketa](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Stariji [model klasični implementacije temelji biblioteke](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) podržani su za kompatibilnost s prijašnjim verzijama samo, stoga preporučujemo da koristite noviju biblioteke Voditelj resursa koji se temelje.

Da biste dovršili korake u ovom članku, potrebno je sljedeće:

- Azure pretplate. Ako vam je potrebna pretplata na Azure jednostavno kliknite **BESPLATAN RAČUN** pri vrhu ove stranice, a zatim vratite do kraja ovog članka.
- Visual Studio. Besplatnog primjerka Visual Studio potražite u članku stranici [Visual Studio preuzimanja](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

>[AZURE.NOTE] U ovom se članku stvara novu, praznu SQL baze podataka. Promijenite način *CreateOrUpdateDatabase(...)* u sljedeći primjer kopiranje baze podataka, promjena veličine baze podataka, stvoriti bazu podataka u grupu, itd. Dodatne informacije potražite u članku [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) i [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) klase.



## <a name="create-a-console-app-and-install-the-required-libraries"></a>Stvaranje aplikacije konzole i instalirajte potrebne biblioteke

1. Pokrenuti Visual Studio.
2. Kliknite **datoteka** > **nove** > **projekta**.
3. Stvaranje C# **Aplikacije konzole** i nazovite ih: *SqlDbConsoleApp*


Da biste stvorili bazom podataka SQL C#, učitavanje biblioteke potrebno upravljanja (pomoću [Konzola upravitelja paketa](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Kliknite **Alati** > **Upravitelj paketa NuGet** > **Konzole za Upravitelj paketa**.
2. Vrsta `Install-Package Microsoft.Azure.Management.Sql –Pre` da biste instalirali najnoviju [Biblioteke za upravljanje Microsoft Azure SQL](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Vrsta `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` da biste instalirali [Microsoft Azure resursima biblioteke](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Vrsta `Install-Package Microsoft.Azure.Common.Authentication –Pre` da biste instalirali [Microsoft Azure uobičajenih provjera autentičnosti biblioteke imenika](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] Primjerima u ovom članku pomoću sinkrono obrazac za svaki zahtjev za API-JA i blokirati dok obavljanjem OSTALE poziva na pozadini servis. Dostupne su asinkrone metode.


## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Stvaranje baze podataka SQL server, pravila vatrozida i baze podataka SQL - C# primjer

Sljedeća Ogledna formula stvara grupu resursa, poslužitelj, pravila vatrozida i SQL baze podataka. Potražite u odjeljku [Stvori glavni pristup resursima servisa](#create-a-service-principal-to-access-resources) da biste na `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` varijabli.

Zamijenite sadržaj **Program.cs** sljedeće i ažuriranje na `{variables}` vrijednostima aplikacije (nemojte uvrstiti u `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Stvorite glavni pristupa resursima uslugu

Sljedeću skriptu komponente PowerShell stvara aplikacije Active Directory (AD) i glavnicu usluge koje je potrebna za provjeru autentičnosti naša C# aplikacija. Skripta proizvodi vrijednosti koje su potrebna za prethodni C# uzorka. Detaljne informacije potražite u članku [Korištenje ljuske PowerShell Azure da biste stvorili glavni pristupa resursima servisa](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Daljnji koraci
Sad kad ste pokušali SQL baze podataka i postavljanje baze podataka pomoću C#, spremni ste za u sljedećim člancima:

- [Povezivanje s bazom podataka SQL s SQL Server Management Studio i obaviti primjer T SQL upita](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Dodatni resursi

- [SQL baze podataka](https://azure.microsoft.com/documentation/services/sql-database/)
- [Predmet baze podataka](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png
