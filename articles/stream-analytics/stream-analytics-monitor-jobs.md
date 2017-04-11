<properties
    pageTitle="programski praćenje zadataka na strujanje analize | Microsoft Azure"
    description="Saznajte kako programski pratite poslove strujanje analize stvoren pomoću REST API-ji, Azure SDK ili Powershell."
    keywords=".NET monitor, monitor posla, nadzor aplikacija"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Stvorili programski monitor analize toka posla
 U ovom se članku objašnjava kako omogućiti nadzor za posao strujanje analize. Strujanje analize poslove stvoren pomoću REST API-ji, Azure SDK ili Powershell učinite nije nadzor omogućio prema zadanim postavkama.  Možete ručno omogućiti to na portalu za Azure tako da odete na posao Monitor stranice i klikom na gumb Omogući ili postupak možete automatizirati tako da slijedite korake u ovom članku. Nadzor podataka prikazat će se na kartici "Praćenje" na portalu Azure za svoj posao strujanje analize.

![zadatak monitor kartica poslove](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Preduvjeti
Prije nego počnete u ovom se članku, morate imati sljedeće:

- Visual Studio 2012 ili 2013.
- Preuzmite i instalirajte [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Postojeći strujanje analize posao koji je potrebno omogućiti za nadzor.

## <a name="setup-a-project"></a>Postavljanje projekta

1.  Stvaranje aplikacije konzole za Visual Studio C# .net.
2.  Na konzoli za Upravitelj paketa pokrenite sljedeće naredbe za instalaciju paketa NuGet. Prvi je SDK za .NET Azure strujanje analize upravljanje. Drugu je SDK Azure Monitor koji će se koristiti da biste omogućili nadzor. Posljednje je klijent Azure Active Directory koja će se koristiti za provjeru autentičnosti.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  Dodajte sljedeće sekcije appSettings App.config datoteku.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Zamjena vrijednosti za *SubscriptionId* i *ActiveDirectoryTenantId* s pretplatom Azure i smjernice za ID-a. Tako da pokrenete sljedeći cmdlet PowerShell prikazat će vam ove vrijednosti:

    ```
    Get-AzureAccount
    ```
4.  Dodajte sljedeće pomoću naredbe izvorišna datoteka (Program.cs) u projektu.

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Dodajte način provjere autentičnosti preglednika.

        public static string GetAuthorizationHeader()
            {
                AuthenticationResult result = null;
                var thread = new Thread(() =>
                {
                    try
                    {
                        var context = new AuthenticationContext(
                            ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                            ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                        result = context.AcquireToken(
                            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                            clientId: ConfigurationManager.AppSettings["AsaClientId"],
                            redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                            promptBehavior: PromptBehavior.Always);
                    }
                    catch (Exception threadEx)
                    {
                        Console.WriteLine(threadEx.Message);
                    }
                });

                thread.SetApartmentState(ApartmentState.STA);
                thread.Name = "AcquireTokenThread";
                thread.Start();
                thread.Join();

                if (result != null)
                {
                    return result.AccessToken;
                }

                throw new InvalidOperationException("Failed to acquire token");
        }

## <a name="create-management-clients"></a>Stvaranje upravljanje klijentima
Sljedeći kod postavit će potrebno varijabli i upravljanje klijentima.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Omogućivanje nadzor za postojeći posao strujanje Analytics

Sljedeći kod će se omogućiti nadzor za **postojeći** posao strujanje analize. Prvi dio kod izvodi zahtjevom GET protiv servis za strujanje Analytics za dohvaćanje informacija o određeni posao strujanje analize. Koristi svojstvo "Id" (dohvaća iz zahtjeva za PRISTUP) kao parametar za metodu stavi u druga polovica kod koji se šalje na STAVI zahtjev za servis uvida da biste omogućili nadzor za posao strujanje analize.

> [AZURE.WARNING]
> Ako ste prethodno omogućili nadzor za različite analize toka posla, putem portala za Azure ili programski putem na ispod kod **preporučuje se unesete isti naziv računa za pohranu koji ste izvršili kada ste prethodno omogućili nadzor.**
>
> Prostor za pohranu račun povezan regije koju ste stvorili posla strujanje analize u, a posebice ne posao sam.
>
> Sve analize toka posla (i drugih Azure resursa) u toj istoj regiji zajedničko korištenje ovog računa za pohranu za spremanje podataka za nadzor. Ako unesete račun za različite prostora za pohranu, može uzrokovati neželjeni popratne pojave za nadzor druge zadatke strujanje analize i/ili drugih Azure resursa.
>
> Naziv računa spremišta koristiti da biste zamijenili ```“<YOUR STORAGE ACCOUNT NAME>”``` ispod mora biti račun za pohranu koji se nalazi u okviru iste pretplate tijekom analize strujanje posao su Omogućivanje nadzor za.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Podrška
Za dodatnu pomoć, pokušajte našem [forumu Azure strujanje analize](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
