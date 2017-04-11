<properties
    pageTitle="Stvaranje grupe aplikacija programa elastic bazu podataka s C# | Microsoft Azure"
    description="Koristite C# baze podataka razvojnih tehnika stvaranja grupe aplikacija skalabilni elastic baze podataka u bazi podataka SQL Azure tako da možete omogućiti zajedničko korištenje resursa preko mnoge baze podataka."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="create-an-elastic-database-pool-with-cx23"></a>Stvaranje grupe aplikacija programa elastic baze podataka pomoću C i #x 23;

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


U ovom se članku opisuje način korištenja C# stvoriti na skup elastic baze podataka Azure SQL s [Biblioteke za baze podataka SQL Azure za .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Da biste stvorili samostalni SQL baze podataka, potražite u članku [Korištenje C# za stvaranje baze podataka za SQL s bibliotekom baze podataka SQL za .NET](sql-database-get-started-csharp.md).

Biblioteka za baze podataka SQL Azure za .NET nudi [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md)-temelji API koji se prelama [Resursima sustavom SQL baza podataka REST API -JA](https://msdn.microsoft.com/library/azure/mt163571.aspx).

>[AZURE.NOTE] Mnoge nove značajke baze podataka SQL podržani su samo ako koristite [model implementacije Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md), tako da uvijek koristite najnoviji **biblioteke za upravljanje Azure SQL baze podataka za .NET ([Dokumenti](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet paketa](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Stariji [model klasični implementacije temelji biblioteke](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) podržani su za kompatibilnost s prijašnjim verzijama samo, stoga preporučujemo da koristite noviju biblioteke Voditelj resursa koji se temelje.

Da biste dovršili korake u ovom članku, potrebno je sljedeće:

- Azure pretplate. Ako vam je potrebna pretplata na Azure jednostavno kliknite **BESPLATAN RAČUN** pri vrhu ove stranice, a zatim vratite do kraja ovog članka.
- Visual Studio. Besplatnog primjerka Visual Studio potražite u članku stranici [Visual Studio preuzimanja](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="create-a-console-app-and-install-the-required-libraries"></a>Stvaranje aplikacije konzole i instalirajte potrebne biblioteke

1. Pokrenuti Visual Studio.
2. Kliknite **datoteka** > **nove** > **projekta**.
3. Stvaranje C# **Aplikacije konzole** i nazovite ih: *SqlElasticPoolConsoleApp*


Da biste stvorili bazom podataka SQL C#, učitavanje biblioteke potrebno upravljanja (pomoću [Konzola upravitelja paketa](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Kliknite **Alati** > **Upravitelj paketa NuGet** > **Konzole za Upravitelj paketa**.
2. Vrsta `Install-Package Microsoft.Azure.Management.Sql –Pre` da biste instalirali [Biblioteke za upravljanje Microsoft Azure SQL](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Vrsta `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` da biste instalirali [Microsoft Azure resursima biblioteke](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Vrsta `Install-Package Microsoft.Azure.Common.Authentication –Pre` da biste instalirali [Microsoft Azure uobičajenih provjera autentičnosti biblioteke imenika](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] Primjerima u ovom članku pomoću sinkrono obrazac za svaki zahtjev za API-JA i blokirati dok obavljanjem OSTALE poziva na pozadini servis. Dostupne su asinkrone metode.


## <a name="create-a-sql-elastic-database-pool---c-example"></a>Stvaranje grupe za elastic baze podataka SQL - C# primjer

Sljedeći primjer stvara grupu resursa, poslužitelj, pravila vatrozida, elastic skup, a zatim stvoriti bazu podataka sustava SQL u. Potražite u odjeljku [Stvori glavni pristup resursima servisa](#create-a-service-principal-to-access-resources) da biste na `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` varijabli.

Zamijenite sadržaj **Program.cs** sljedeće i ažuriranje na `{variables}` vrijednostima aplikacije (nemojte uvrstiti u `{}`).


```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
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

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



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

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
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



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
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
                    ElasticPoolName = poolName
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
```





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

- [Upravljanje vaše grupe aplikacija](sql-database-elastic-pool-manage-csharp.md)
- [Stvaranje elastic poslovi](sql-database-elastic-jobs-overview.md): Elastic poslove omogućuju vam da pokrećete T SQL skripte za bilo koji broj baza podataka u zajedničko područje.
- [Skaliranje odgovor s bazom podataka SQL Azure](sql-database-elastic-scale-introduction.md): pomoću alata elastic baze podataka za prikaz izgleda.

## <a name="additional-resources"></a>Dodatni resursi

- [SQL baze podataka](https://azure.microsoft.com/documentation/services/sql-database/)
- [API-ji za upravljanje Azure resursa](https://msdn.microsoft.com/library/azure/dn948464.aspx)
