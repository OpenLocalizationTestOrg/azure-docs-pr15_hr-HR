<properties
   pageTitle="Korištenje podataka Lake analize Java SDK za razvoj aplikacija | Azure"
   description="Korištenje Azure podataka Lake analize Java SDK za razvoj aplikacija"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-java-sdk"></a>Praktični vodič: Početak rada s Azure podataka Lake analize pomoću Java SDK

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Saznajte kako koristiti Java SDK Azure Lake analize podataka da biste stvorili Azure podataka Lake račun i izvođenje osnovnih operacija kao što su prilikom stvaranja mape, prijenos i preuzimanje datoteke s podacima, brisanje računa i rad sa zadacima. Dodatne informacije o Lake podataka potražite u članku [Azure podataka Lake analize](data-lake-analytics-overview.md).

U ovom ćete praktičnom vodiču će razvijajte aplikacije konzole Java koja sadrži primjere uobičajene administrativne zadatke kao i stvaranje test podataka i predavanje posla.  Da biste došli do isti praktičnom vodiču pomoću drugih alata za podržane, klikom na kartice pri vrhu stranice u ovom se odjeljku.

## <a name="prerequisites"></a>Preduvjeti

* Java Development Kit (JDK) 8 (pomoću Java verzije 1.8).
* IntelliJ ili drugu odgovarajuću Java razvojno okruženje. Ovo nije obavezno, ali preporučuje. Upute u nastavku pomoću IntelliJ.
* **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
* **Omogućivanje pretplate Azure** analize podataka Lake javno pretpregled. Pročitajte [upute](data-lake-analytics-get-started-portal.md#signup).
* Stvaranje aplikacije komponente Azure Active Directory (AAD) i dohvaćanje njegov **ID klijenta**, **ID klijenta**i **ključa**. Dodatne informacije o aplikacijama AAD i upute o tome kako se ID klijenta potražite u članku [Stvaranje servisa Active Directory aplikacija i servisa glavni pomoću portala](../resource-group-create-service-principal-portal.md). Odgovori URI i ključ bit će dostupne na portalu kada je stvorena aplikacija i ključ generira.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Kako autentičnost pomoću servisa Azure Active Directory?

Ispod isječak koda sadrži kod za **koji nije interaktivan** provjere autentičnosti koje aplikacije sadrži vlastitu vjerodajnice.

Morat ćete dati dozvolu aplikaciju za stvaranje resursa u Azure za ovog praktičnog vodiča za rad. Nije **se preporučuje** da samo date suradničke dozvole ovu aplikaciju u grupu resursa nove, koje se ne koriste i prazne Azure pretplatu za potrebe ovog praktičnog vodiča.

## <a name="create-a-java-application"></a>Stvaranje aplikacije Java

1. Otvorite IntelliJ i stvorite novi projekt Java pomoću predloška za **Aplikaciju naredbenog retka** .

2. Desnom tipkom miša kliknite na projektu s lijeve strane zaslona, a zatim kliknite **Dodaj Framework podrška**. Odaberite **Maven** , a zatim kliknite **u redu**.

3. Otvorite datoteku novostvorenu **"pom.xml"** i dodajte sljedeće isječak teksta između na ** \</version >** oznaka i ** \</project >** oznaka:

    Napomena: Taj korak je privremeno dok ne bude dostupno u Maven Azure podataka Lake analize SDK-a. U ovom se članku ažurirat će se kada SDK dostupan je u Maven. Sve buduće ažuriranja taj SDK bit će dostupnih putem Maven.

        <repositories>
            <repository>
                <id>adx-snapshots</id>
                <name>Azure ADX Snapshots</name>
                <url>http://adxsnapshots.azurewebsites.net/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>oss-snapshots</id>
                <name>Open Source Snapshots</name>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-authentication</artifactId>
                <version>1.0.0-20160513.000802-24</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-runtime</artifactId>
                <version>1.0.0-20160513.000812-28</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.rest</groupId>
                <artifactId>client-runtime</artifactId>
                <version>1.0.0-20160513.000825-29</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-store</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-analytics</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
        </dependencies>


4. Otvorite **datoteku**, a zatim **Postavke**, a zatim **stvaranja**, **izvođenja** **implementacije**. Odaberite **Stvaranje Alati**, **Maven**, **Uvoz**. Provjerite **Maven uvoz projekata automatski**.

5. Otvorite **Main.java** i zamijenite postojeće blokova Šifra sljedeći kod. Osim toga, navedite vrijednosti za parametre oblačićima u isječak koda, kao što su **localFolderPath**, **_adlaAccountName** **_adlsAccountName** **_resourceGroupName** i zamjena rezerviranih mjesta za **ID KLIJENTA**, **KLIJENT TAJNA**, **KLIJENT ID**i **ID PRETPLATE**.

    Kod prolazi kroz postupak stvaranje računa za spremište Lake podataka i analize podataka Lake, stvaranje datoteka u trgovini, pokrenut posao, početak stanja zadatka, preuzimanje posla izlazne i na kraju brisanja računa.

    >[AZURE.NOTE] Trenutno je poznat problem sa servisom Azure podataka Lake.  Ako aplikaciju uzorka prekine ili naiđe na pogrešku, možda morati ručno izbrisati spremišta Lake podataka i analize podataka Lake račune koji stvara skriptu.  Ako niste upoznati s portala, vodič za [Upravljanje Azure podataka Lake analize pomoću portala za Azure](data-lake-analytics-manage-use-portal.md) će vam pri prvim koracima.


        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.analytics.*;
        import com.microsoft.azure.management.datalake.analytics.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.nio.file.Files;
        import java.nio.file.Paths;
        import java.util.ArrayList;
        import java.util.UUID;
        import java.util.List;
        
        public class Main {
            private static String _adlsAccountName;
            private static String _adlaAccountName;
            private static String _resourceGroupName;
            private static String _location;
        
            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;
        
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
            private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;
        
            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";
        
                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";
        
                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.
        
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.
        
                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);
        
                // Create Data Lake Store and Analytics accounts
                WaitForNewline("Authenticated.", "Creating NEW accounts.");
                CreateAccounts();
                WaitForNewline("Accounts created.", "Displaying accounts.");
        
                // List Data Lake Store and Analytics accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                System.out.println(String.format("All ADL Analytics accounts that this app can access in subscription %s:", _subId));
                List<DataLakeAnalyticsAccount> adlaListResult = _adlaClient.getAccountOperations().list().getBody();
                for (DataLakeAnalyticsAccount acct : adlaListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Accounts displayed.", "Creating files.");
        
                // Create a file in Data Lake Store: input1.csv
                // TODO: these change order in the next patch
                byte[] bytesContents = "123,abc".getBytes();
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, "/input1.csv", bytesContents, true);
        
                WaitForNewline("File created.", "Submitting a job.");
        
                // Submit a job to Data Lake Analytics
                UUID jobId = SubmitJobByScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();", "testJob");
                WaitForNewline("Job submitted.", "Getting job status.");
        
                // Wait for job completion and output job status
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                System.out.println("Waiting for job completion.");
                WaitForJob(jobId);
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                WaitForNewline("Job completed.", "Downloading job output.");
        
                // Download job output from Data Lake Store
                DownloadFile("/output1.csv", localFolderPath + "output1.csv");
                WaitForNewline("Job output downloaded.", "Deleting file.");
        
                // Delete file from Data Lake Store
                DeleteFile("/output1.csv");
                WaitForNewline("File deleted.", "Deleting account.");
        
                // Delete account
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
                _adlaClient.getAccountOperations().delete(_resourceGroupName, _adlaAccountName);
                WaitForNewline("Account deleted.", "DONE.");
            }
        
            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
                _adlaClient = new DataLakeAnalyticsAccountManagementClientImpl(creds);
                _adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);
                _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClientImpl(creds);
                _adlsClient.setSubscriptionId(_subId);
                _adlaClient.setSubscriptionId(_subId);
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
        
                System.out.println(reason + "\r\nPress ENTER to continue...");
                try{System.in.read();}
                catch(Exception e){}
        
                if (!nextAction.isEmpty())
                {
                    System.out.println(nextAction);
                }
            }
        
            // Create Data Lake Store and Analytics accounts
            public static void CreateAccounts() throws InterruptedException, CloudException, IOException {
                // Create ADLS account
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.setLocation(_location);
        
                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);
        
                // Create ADLA account
                DataLakeStoreAccountInfo adlsInfo = new DataLakeStoreAccountInfo();
                adlsInfo.setName(_adlsAccountName);
        
                DataLakeStoreAccountInfoProperties adlsInfoProperties = new DataLakeStoreAccountInfoProperties();
                adlsInfo.setProperties(adlsInfoProperties);
        
                List<DataLakeStoreAccountInfo> adlsInfoList = new ArrayList<DataLakeStoreAccountInfo>();
                adlsInfoList.add(adlsInfo);
        
                DataLakeAnalyticsAccountProperties adlaProperties = new DataLakeAnalyticsAccountProperties();
                adlaProperties.setDataLakeStoreAccounts(adlsInfoList);
                adlaProperties.setDefaultDataLakeStoreAccount(_adlsAccountName);
        
                DataLakeAnalyticsAccount adlaParameters = new DataLakeAnalyticsAccount();
                adlaParameters.setLocation(_location);
                adlaParameters.setName(_adlaAccountName);
                adlaParameters.setProperties(adlaProperties);
        
                    /* If this line generates an error message like "The deep update for property 'DataLakeStoreAccounts' is not supported", please delete the ADLS and ADLA accounts via the portal and re-run your script. */
        
                _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }
        
            //todo: this changes in the next version of the API
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }
        
            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(filePath, _adlsAccountName);
            }
        
            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(srcPath, _adlsAccountName).getBody();
        
                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());
        
                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();
        
                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }
        
                pWriter.println(fileContents);
                pWriter.close();
            }
        
            // Submit a U-SQL job by providing script contents.
            // Returns the job ID
            public static UUID SubmitJobByScript(String script, String jobName) throws IOException, CloudException {
                UUID jobId = java.util.UUID.randomUUID();
                USqlJobProperties properties = new USqlJobProperties();
                properties.setScript(script);
                JobInformation parameters = new JobInformation();
                parameters.setName(jobName);
                parameters.setJobId(jobId);
                parameters.setType(JobType.USQL);
                parameters.setProperties(properties);
        
                JobInformation jobInfo = _adlaJobClient.getJobOperations().create(_adlaAccountName, jobId, parameters).getBody();
        
                return jobId;
            }
        
            // Wait for job completion
            public static JobResult WaitForJob(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                while (jobInfo.getState() != JobState.ENDED)
                {
                    jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName,jobId).getBody();
                }
                return jobInfo.getResult();
            }
        
            // Get job status
            public static String GetJobStatus(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                return jobInfo.getState().toValue();
            }
        }

6. Slijedite upute da biste pokrenuli i dovršili aplikacije.


## <a name="see-also"></a>Vidi također

- Da biste vidjeli iste praktičnom vodiču pomoću drugih alata, kliknite karticu Birači pri vrhu stranice.
- Da biste vidjeli složeniji upit, potražite u članku [zapisnika analiza web-mjesta pomoću Azure podataka Lake analize](data-lake-analytics-analyze-weblogs.md).
- Prvi koraci u razvoju aplikacija U SQL, potražite u članku [razviti U – SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Da biste saznali U SQL, potražite u članku [Početak rada s jezikom Azure podataka Lake analize U – SQL](data-lake-analytics-u-sql-get-started.md)i [U SQL jezične preporuke](http://go.microsoft.com/fwlink/?LinkId=691348).
- Upravljanje zadacima, potražite u članku [Upravljanje Azure podataka Lake analize pomoću portala za Azure](data-lake-analytics-manage-use-portal.md).
- Da biste dobili pregled analize podataka Lake, potražite u članku [Pregled Azure podataka Lake analize](data-lake-analytics-overview.md).
