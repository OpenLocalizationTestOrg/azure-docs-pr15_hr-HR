<properties 
    pageTitle="Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću .NET API | Microsoft Azure" 
    description="U ovom ćete praktičnom vodiču stvarate kanal na tvorničke Azure podataka s Kopiraj aktivnosti pomoću .NET API-JA." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću .NET API-JA
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopiranje čarobnjaka](data-factory-copy-data-wizard-tutorial.md)
- [Portal za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Predloška Azure Voditelj resursa](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-JA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API-JA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Pomoću ovog praktičnog vodiča prikazuje kako stvoriti i praćenje na tvorničke Azure podataka pomoću .NET API-JA. Kanal na tvorničke podataka koristi aktivnost Kopiraj da biste kopirali podatke iz spremišta blobova platforme Azure s bazom podataka SQL Azure.

Kopiraj aktivnosti izvodi premještanje podataka na tvorničke Azure podataka. Aktivnost se pokreće globalno raspoloživ servis koji možete kopirati podataka između različitih podataka trgovine sigurne pouzdan te skalabilni način. Članak [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) potražite u članku dodatne informacije o aktivnosti Kopiraj.   

> [AZURE.NOTE] 
> Ovaj članak obuhvaća sve na podataka tvorničke .NET API-JA. Detalje o podacima tvorničke .NET SDK potražite u članku [Podataka tvorničke .NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) . 

## <a name="prerequisites"></a>Preduvjeti
- Idite do [vodič pregled i stara requisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) da biste dobili pregled vodiča i dovršite korake **preduvjeta** . 
- Visual Studio 2012 "ili" 2013 "ili" 2015.
- Preuzmite i instalirajte [Azure.NET SDK](http://azure.microsoft.com/downloads/)
- Azure PowerShell. Slijedite upute u članku [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md) da biste instalirali Azure PowerShell na vašem računalu. Stvaranje aplikacije komponente Azure Active Directory pomoću Azure PowerShell.

### <a name="create-an-application-in-azure-active-directory"></a>Stvaranje aplikacije servisa Azure Active Directory
Stvaranje aplikacije komponente Azure Active Directory, stvorite glavni servis za aplikaciju i dodijeliti ulogu **Suradnika tvorničke podataka** .  

1. Pokrenite **PowerShell**. 
1. Pokrenite sljedeću naredbu, a zatim unesite korisničko ime i lozinku koje koristite za prijavu na portal za Azure.
    
        Login-AzureRmAccount   
2. Pokrenite sljedeću naredbu da biste pogledali sve pretplate za taj račun.

        Get-AzureRmSubscription 
3. Pokrenite sljedeću naredbu da biste odabrali pretplatu u koju želite raditi. Zamjena ** &lt;NameOfAzureSubscription** &gt; pod nazivom Azure pretplatu. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Imajte na umu **SubscriptionId** i **TenantId** iz izlaza tu naredbu. 
4. Stvorite grupu za Azure resursa koji se naziva **ADFTutorialResourceGroup** pokretanjem sljedeće naredbe u u ljusci PowerShell.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Ako već postoji u grupu resursa, navedite želite li ažurirati (Y) ili zadržati kao (N). 

    Ako koristite grupu različite resursa, morate je nazovite grupu resursa umjesto ADFTutorialResourceGroup ovog praktičnog vodiča.
5. Stvorite aplikaciju Azure Active Directory. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Ako se pojavi pogreška, navedite različite URL i ponovno pokrenite naredbu. 

        Another object with the same value for property identifierUris already exists.

6. Stvoriti u AD usluge. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Dodajte usluge ulogu **Suradnika tvorničke podataka** . 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Dohvaćanje aplikacije ID.

        $azureAdApplication

    Imajte na umu dolje ID aplikacije (**applicationID** iz izlaza).

Imat ćete pratiti četiri vrijednosti iz sljedeće korake: 

- ID klijenta
- ID pretplate
- ID aplikacije 
- Lozinka (navedena u prve naredbe)   

## <a name="walkthrough"></a>Vodič
1. Koristite Visual Studio 2012/2013/2015, stvaranje C# .NET konzole za aplikacije.
    1. Pokrenite **Visual Studio** 2012/2013/2015.
    2. Kliknite **datoteka**, pokažite na **Novo**, a kliknite **projekt**.
    3. Proširite **predložaka**pa odaberite **Visual C#**. U ovom vodiču koristite C#, ali možete koristiti bilo koji jezik .NET.
    4. Odaberite **Konzolu aplikaciju** s popisa vrsta projekata na desnoj strani.
    5. Unesite **DataFactoryAPITestApp** naziv.
    6. Odaberite **C:\ADFGetStarted** lokacije.
    7. Kliknite **u redu** da biste stvorili projekta.
2. Kliknite **Alati**, pokažite na **Upravitelj paketa Nuget**pa kliknite **Upravitelj paketa konzolu**.
3.  Na **Konzoli upravitelja paketa**, učinite sljedeće: 
    1.  Pokrenite sljedeću naredbu da biste instalirali paket tvorničke podataka:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Pokrenite sljedeću naredbu da biste instalirali paket Azure Active Directory (koristite Active Directory API kod):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. Dodajte sljedeće sekcije **appSetttings** **App.config** datoteku. Ove postavke koriste metodu preglednika: **GetAuthorizationHeader**. 

    Zamjena vrijednosti za ** &lt;ID aplikacije&gt;**, ** &lt;lozinku&gt;**, ** &lt;ID pretplate&gt;**, a ** &lt;smjernice za ID&gt; ** pomoću vlastitih vrijednosti. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. Dodajte sljedeće naredbe **pomoću** izvorišna datoteka (Program.cs) u projektu.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Dodajte sljedeći kod koji stvara instancu klase **DataPipelineManagementClient** **glavne** korakom. Pomoću tog objekta da biste stvorili podataka tvorničke, povezane servisa, ulazni i izlazni skupova podataka i na kanal. Pomoću tog objekta praćenje isječke skup podataka prilikom izvođenja.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Zamijenite vrijednost **resourceGroupName** naziv grupe za Azure resursa. 
    > 
    > Ažurirajte naziv tvorničke podataka (**dataFactoryName**) moraju biti jedinstvene. Naziv tvorničke podataka mora biti globalno jedinstveni. Pogledajte temu [Tvorničke podataka – pravila imenovanja](data-factory-naming-rules.md) za imenovanja pravila za artefakte tvorničke podataka. 

7. Dodajte sljedeći kod koji stvara **podataka tvorničke** **glavne** korakom.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Dodajte sljedeći kod koji stvara programa **Azure prostora za pohranu povezana servisa** **glavne** način. 

    > [AZURE.IMPORTANT] Zamijenite **storageaccountname** i **accountkey** ime i ključ računa za Azure prostora za pohranu. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Dodajte sljedeći kod koji stvara sustava **Azure SQL povezana servisa** metodu **glavne** .
 
    > [AZURE.IMPORTANT] **Naziv poslužitelja**, **ImeBazePodataka**, **korisničko ime**i **lozinku** zamijenite imena Azure SQL poslužitelja, bazu podataka, korisnika i lozinku.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Dodajte sljedeći kod koji stvara **Ulazni i izlazni skupova podataka** na **glavni** način. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Dodajte sljedeći kod te **stvara i aktiviranje na kanal** **glavne** korakom. U ovom kanal ima **CopyActivity** koja vas vodi **BlobSource** kao izvor i **BlobSink** kao na primatelj.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Dodajte sljedeći kod **glavne** način da biste dobili status isječka podataka od izlazni skup podataka. Postoji samo isječak očekuje u ovom primjeru.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Dodajte sljedeće kod koji će se pokrenuti detalje o podacima isječak **glavne** korakom.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Dodajte sljedeću metodu helper koristi **glavne** metode predmete **Program** .  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
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


15. U pregledniku rješenja proširenje projekta (**DataFactoryAPITestApp**), desnom tipkom miša kliknite **reference**i kliknite **Dodaj referenca**. Potvrdite okvir za sastavljanje "**System.Configuration**", a zatim kliknite **u redu**. 
16. Stvaranje aplikacije konzole. Na izborniku kliknite **Sastavi** , a zatim kliknite **Stvaranje rješenja**. 
16. Provjerite ima li datoteka barem jedan u spremniku **adftutorial** u Azure blobova. Ako nije, stvorite **Emp.txt** datoteke u Bloku za pisanje pomoću sljedećeg sadržaja i prijenos spremniku adftutorial.

        John, Doe
        Jane, Doe
     
17. Klikom na uzorka **za ispravljanje pogrešaka** -> na izborniku**Start ispravljanje pogrešaka** . Kada se prikaže **početak pokrenuti detalja podataka isječka**, pričekajte nekoliko minuta pa pritisnite **ENTER**. 
18. Da biste potvrdili da tvorničke podataka **APITutorialFactory** stvara se pomoću sljedeće artefakte pomoću portala za Azure: 
    - Povezana servisa: **LinkedService_AzureStorage** 
    - Skup podataka: **DatasetBlobSource** i **DatasetBlobDestination**.
    - Kanal: **PipelineBlobSample** 
18. Provjerite je li u tablici "**emp**" u navedenoj bazi podataka Azure SQL stvaraju dva zaposlenika zapisa.

## <a name="next-steps"></a>Daljnji koraci

- Pročitajte članak [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) , koji pruža podrobne informacije o aktivnosti Kopiraj koji se koriste u praktičnom vodiču.
- Detalje o podacima tvorničke .NET SDK potražite u članku [Podataka tvorničke .NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) . Ovaj članak obuhvaća sve na podataka tvorničke .NET API-JA. 

 
