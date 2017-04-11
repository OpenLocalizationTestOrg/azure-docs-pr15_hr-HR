<properties
    pageTitle="Upravljanje .NET SDK za strujanje analize | Microsoft Azure"
    description="Početak rada s strujanje analize upravljanje .NET SDK. Upute za postavljanje i pokretanje analize poslovi: Stvaranje projekta, unosa, izlaze i transformacije."
    keywords=".NET SDK analize API-JA"
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


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Upravljanje .NET SDK: Postavljanje i pokretanje analize zadatke pomoću analize API-JA strujanje Azure za .NET

Upute za postavljanje programa poslova izvođenja analize pomoću strujanje analize API-JA za .NET pomoću upravljanja .NET SDK-a. Postavljanje projekta, stvarati ulazni i izlazni izvora, transformacije i pokretanje i zaustavljanje poslova. Za svoje zadatke analize strujanja podataka iz spremišta blobova ili iz koncentratora za događaj.

Potražite u članku [Upravljanje referentnu dokumentaciju za API analize strujanje za .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure strujanje analize je potpuno Upravljani servis nudi obrada najniža Latencija, Visoko dostupna, prilagodljivi, složene događaja putem strujanja podataka u oblaku. Strujanje Analytics omogućuje klijentima da biste postavili strujanje zadataka radi analize podataka strujanja i im omogućuje da pogon blizu analize u stvarnom vremenu.  


## <a name="prerequisites"></a>Preduvjeti
Prije nego počnete u ovom se članku, morate imati sljedeće:

- Instalirajte Visual Studio 2012 ili 2013.
- Preuzmite i instalirajte [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Stvaranje grupe za resurse sustava Azure u svoju pretplatu. Slijedi primjer skripte Azure PowerShell. Azure PowerShell informacije potražite u članku [instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Postavljanje programa unos izvorišno i odredišno izlaz da biste koristili. Futher upute potražite u članku [Dodavanje unosa](stream-analytics-add-inputs.md) za postavljanje oglednih unos i [Dodavanje izlaze](stream-analytics-add-outputs.md) postavljanje oglednih izlaz.


## <a name="set-up-a-project"></a>Postavljanje projekta

Da biste stvorili zadatak programa analize pomoću strujanje analize API-JA za .NET, najprije postavite projekta.

1. Stvaranje aplikacije konzole za Visual Studio C# .NET.
2. Na konzoli za Upravitelj paketa pokrenite sljedeće naredbe za instalaciju paketa NuGet. Prvi je SDK za .NET Azure strujanje analize upravljanje. Drugu je klijent Azure Active Directory koja će se koristiti za provjeru autentičnosti.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. Dodajte sljedeće sekcije **appSettings** App.config datoteku:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Zamjena vrijednosti za **SubscriptionId** i **ActiveDirectoryTenantId** s pretplatom Azure i smjernice za ID-a. Tako da pokrenete sljedeći cmdlet Azure PowerShell prikazat će vam ove vrijednosti:

        Get-AzureAccount

5. Dodajte sljedeće naredbe **pomoću** izvorišna datoteka (Program.cs) u projektu:

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Dodaj pomoćnik način provjere autentičnosti:

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


## <a name="create-a-stream-analytics-management-client"></a>Stvaranje klijent za upravljanje strujanje Analytics

Objekt programa **StreamAnalyticsManagementClient** omogućuje upravljanje posla i komponente posla, kao što su ulazne, izlazne i transformacije.

Dodajte sljedeći kôd početak **Glavna** načina:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Varijabla **resourceGroupName** vrijednost mora biti jednak nazivu grupe resursa koje ste stvorili ili sakrije pripremni koraka.

Da biste automatizirali aspekt prezentacije vjerodajnica stvaranja posla, pogledajte [provjere autentičnosti glavni servis pomoću upravitelja za Azure resursa](../resource-group-authenticate-service-principal.md).

Preostali odjeljci u ovom se članku pretpostavlja da kod je na početku metodu **glavne** .

## <a name="create-a-stream-analytics-job"></a>Stvaranje zadatka strujanje Analytics

Sljedeći kod stvara zadatak strujanje analize u odjeljku grupa resursa koje ste definirali. Ćete dodati programa ulazne, izlazne i transformacije posao kasnije.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Stvorite izvor za strujanje analize za unos

Sljedeći kod stvara unos izvor strujanje analize blob ulazni izvor vrstom i serijalizacije CSV. Da biste stvorili koncentrator ulazni izvor događaja, koristite **EventHubStreamInputDataSource** umjesto **BlobStreamInputDataSource**. Isto tako, možete prilagoditi serijalizacije vrstu unosa izvora.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Unos izvora, hoće li iz spremišta blobova ili koncentratora za događaj, je uz određeni posao. Da biste koristili isti unos izvor različite poslove, morate ponovno pozvati metodu i navedite drugo ime posla.


## <a name="test-a-stream-analytics-input-source"></a>Testiranje strujanje analize ulazni izvor

Način **TestConnection** provjerava je li posao strujanje analize možete povezati s ulazni izvor, kao i druge aspekte specifične za vrstu izvora unos. Ako, na primjer, u blob unos izvoru koju ste stvorili u prethodnom koraku, metodu će provjeriti da naziv računa za pohranu i ključ može se koristiti za povezivanje s računom za pohranu, kao i provjerite postoji li navedeni spremnik.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Stvaranje ciljni izlaz strujanje Analytics

Stvaranje programa ciljni izlaz vrlo je sličan stvaranje izvora unos strujanje analize. Kao što je unos izvora, izlazna ciljeve je uz određeni posao. Da biste koristili iste ciljni izlaz različite poslove, morate ponovno pozvati metodu i navedite drugo ime posla.

Sljedeći kod stvara ciljni izlaz (baze podataka Azure SQL). Možete prilagoditi ciljni izlaz vrsta podataka i/ili serijalizacije vrsta.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Testiranje ciljni izlaz strujanje Analytics

Ciljni izlaz strujanje analize ima metodu **TestConnection** za testiranje veze.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Stvaranje strujanje analize transformacije

Sljedeći kod stvara strujanje analize transformacije s upitom "odaberite * iz unosa na" i određuje želite dodijeliti strujanje jedinice za posao strujanje analize. Dodatne informacije o prilagodbi strujanje jedinice, potražite u članku [poslove skaliranje Azure strujanje analize](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Kao što su ulazni i izlazni, na transformacije i vezan uz određeni posao strujanje analize je stavka stvorena u odjeljku.

## <a name="start-a-stream-analytics-job"></a>Pokretanje analize strujanje zadatka
Nakon stvaranja strujanje Analytics za posao i njegov input(s), output(s) i transformaciju, možete pokrenuti posao tako da nazovete način **pokrenuti** .

Sljedeći ogledni kod pokreće strujanje analize zadatak s vrijeme početka prilagođene izlaz postavljena na prosinac 12, 2012, 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Zaustavljanje analize toka posla
Izvodi analize tok posla, možete isključiti tako da nazovete metodu **Zaustavi** .

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Brisanje posla strujanje Analytics
Način za **Brisanje** izbrisat će se posla, kao i temeljni pod resurse, uključujući input(s), output(s) i transformacije posla.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Podrška
Za dodatnu pomoć, pokušajte našem [forumu Azure strujanje analize](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Daljnji koraci

Ste učenje osnove korištenja .NET SDK za stvaranje i pokretanje analize zadatke. Dodatne informacije potražite u sljedećim člancima:

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upravljanje .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
