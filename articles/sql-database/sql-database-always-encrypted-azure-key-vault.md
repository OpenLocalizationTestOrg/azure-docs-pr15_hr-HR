<properties
    pageTitle="Uvijek šifrirane: Zaštitite povjerljive podatke u bazi podataka SQL Azure s šifriranje baze podataka | Microsoft Azure"
    description="Zaštita povjerljive podatke u bazi podataka za SQL u minutama."
    keywords="šifriranje podataka, ključa za šifriranje, a zatim šifriranje oblaka"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Uvijek šifrirane: Zaštita povjerljive podatke u bazi podataka SQL i pohranjivati ključeva za šifriranje u sigurnog ključ Azure

> [AZURE.SELECTOR]
- [Azure sigurnog ključa](sql-database-always-encrypted-azure-key-vault.md)
- [Windows spremište certifikata](sql-database-always-encrypted.md)


U ovom se članku objašnjava sigurne osjetljivih podataka u bazi podataka sustava SQL šifriranjem podataka korištenjem [Uvijek šifrirane čarobnjaka](https://msdn.microsoft.com/library/mt459280.aspx) u [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Sadrži upute koje će vam pokazati kako spremiti svaki ključa za šifriranje u sigurnog ključ Azure.

Uvijek šifrirana je novu tehnologiju šifriranja podataka u bazi podataka SQL Azure i SQL Server štiti osjetljive podatke na ostale na poslužitelju, tijekom kretanje između postupak klijentske i poslužiteljske i dok se podaci nalaze u koristi. Uvijek šifrirana osigurava da povjerljive podatke nikad se ne pojavljuje kao običan tekst unutar baze podataka sustava. Kada konfigurirate šifriranje podataka, samo klijentske aplikacije ili poslužitelje aplikacije koje imate pristup tipki može pristupiti podacima običan tekst. Detaljne informacije potražite u članku [Uvijek šifrirane (modul baze podataka)](https://msdn.microsoft.com/library/mt163865.aspx).


Nakon konfiguriranja baze podataka za korištenje uvijek šifrirane stvarate klijentska aplikacija u C# s Visual Studio za rad sa šifriranim podacima.

Slijedite korake u ovom se članku i Saznajte kako postaviti uvijek šifrirane za baze podataka Azure SQL. U ovom članku ćete saznati kako obaviti sljedeće zadatke:

- Pomoću čarobnjaka za uvijek šifrirane u SSMS da biste stvorili [uvijek šifrirane ključeve](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
    - Stvaranje [stupca glavni ključ (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Stvaranje [ključa za šifriranje stupca (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Stvaranje tablica baze podataka i šifriranje stupaca.
- Stvaranje aplikacije koja umeće odabire i prikazuje podatke iz šifrirane stupaca.


## <a name="prerequisites"></a>Preduvjeti

Za ovaj vodič, morat ćete:

- Račun za Azure i pretplate. Ako ga nemate, prijavite se za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verziju 13.0.700.242 ili noviji.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) ili noviji (na klijentskom računalu).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
- [Azure PowerShell](../powershell-install-configure.md), 1.0 ili novijem. Vrsta **(Dohvati modul azure - ListAvailable). Verzija** da biste vidjeli koju verziju programa PowerShell koristite.



## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>Omogućivanje klijentska aplikacija za pristup servisu SQL baze podataka

Morate omogućiti klijentska aplikacija za pristup servisu SQL baze podataka postavljanjem potrebna provjera autentičnosti i dobivanje *ClientId* i *tajna* morat ćete autentičnost aplikacije u sljedeći kod.

1. Otvorite [portal za Azure klasični](http://manage.windowsazure.com).
2. Odaberite **Servisa Active Directory** i kliknite instanca servisa Active Directory koji će koristiti aplikaciju.
3. Kliknite **aplikacije**, a zatim kliknite **DODAJ**.
4. Upišite naziv aplikacije (na primjer: *myClientApp*) odaberite **Web-APLIKACIJU**, a zatim kliknite strelicu da biste nastavili.
5. Za **Prijavu na URL** i **Aplikacije ID URI** unesite valjani URL (na primjer, *http://myClientApp*) i nastaviti.
6. Kliknite **KONFIGURIRAJ**.
7. Kopirajte **ID KLIJENTA**. (Morat ćete tu vrijednost u kodu kasnije.)
8. U odjeljku **tipke** odaberite **1 godine** s padajućeg popisa **Odaberite trajanje** . (Će kopirate tipku nakon što spremite u koraku 14.)
11. Pomaknite se prema dolje, a zatim kliknite **Dodaj aplikaciju**.
12. Ostavite **PRIKAZ** postavljanje aplikacije **Microsoft** , a zatim odaberite **Upravljanje servisom za Microsoft Azure**. Kliknite kvačicu da biste nastavili.
13. Odaberite **Upravljanje servisom Azure programa Access** s padajućeg popisa **Dodijeliti dozvole** .
14. Kliknite **SPREMI**.
15. Po završetku Spremi u odjeljku **tipke** kopirajte vrijednost ključa. (Morat ćete tu vrijednost u kodu kasnije.)



## <a name="create-a-key-vault-to-store-your-keys"></a>Stvaranje ključa zbirke ključeva za pohranu ključeva

Sad kad je konfiguriran klijent aplikacije, a imate identifikacijskog Broja za klijenta, vrijeme je za stvaranje ključa sigurnog i konfiguriranje politiku access da i aplikacije mogli pristupati s sigurnog tajne (uvijek šifrirane tipki). Dozvole za * *Stvaranje*, *popisa*, *znak*, *Provjerite je li*, *wrapKey*te *unwrapKey* *su potrebni za stvaranje novog ključa matrica stupca i za postavljanje šifriranja s SQL Server Management Studio.

Ključni sigurnog možete brzo stvoriti tako da pokrenete sljedeću skriptu. Detaljno objašnjenje tih cmdleta i dodatne informacije o stvaranju i konfiguriranje ključa sigurnog potražite u članku [Početak rada s sigurnog ključ Azure](../Key-Vault/key-vault-get-started.md).



    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).SubscriptionId
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup –Name $resourceGroupName –Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Stvorite praznu bazu podataka za SQL
1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Otvorite **Novi** > **podataka + prostor za pohranu** > **SQL baze podataka**.
3. Stvorite **praznu** bazu podataka s nazivom **Clinic** na poslužitelju novi ili postojeći. Detaljne upute o tome kako stvoriti bazu podataka na portalu za Azure, potražite u članku [Stvaranje baze podataka SQL u minutama](sql-database-get-started.md).

    ![Stvorite praznu bazu podataka](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Potrebno je veza niza kasnije u ovom praktičnom vodiču, pa nakon što stvorite bazu podataka, odaberite novi Clinic bazu podataka i kopirajte niz za povezivanje. U bilo kojem trenutku možete dobiti niz za povezivanje, ali je lako da biste ga kopirali na portalu za Azure.

1. Idite na **baze podataka SQL** > **Clinic** > **Prikaz niza veze baze podataka**.
2. Kopirajte niz za povezivanje za **ADO.NET**.

    ![Kopirajte niz za povezivanje](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Povezivanje s bazom podataka s SSMS

Otvorite SSMS i povezati s poslužiteljem s bazom podataka Clinic.


1. Otvorite SSMS. (Idite na **Povezivanje** > **Modul baze podataka** da biste otvorili prozor **za povezivanje s poslužiteljem** ako već nije otvoren.)
2. Unesite naziv poslužitelja i vjerodajnice. Naziv poslužitelja možete pronaći na plohu baze podataka SQL i u nizu za povezivanje koju ste ranije kopirali. Upišite naziv poslužitelja za dovršavanje, uključujući *database.windows.net*.

    ![Kopirajte niz za povezivanje](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Ako se otvori prozor **Novog pravila vatrozida** , prijavite se u Azure i pričekajte SSMS stvaranje novog pravila vatrozida za vas.


## <a name="create-a-table"></a>Stvaranje tablice

U ovom ćete odjeljku će stvoriti tablicu veličini pacijenta podataka. Nije prethodno šifrirane – šifriranje će konfigurirati u sljedećem odjeljku.

1. Proširite **baze podataka**.
1. Desnom tipkom miša kliknite **Clinic** bazu podataka, a zatim kliknite **Novi upit**.
2. Zalijepite sljedeće Transact-SQL (T-SQL) u novom prozoru upita i **izvršavanje** ga.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Šifriranje stupaca (Konfiguriranje uvijek šifrirane)

SSMS nudi čarobnjak koji omogućuje jednostavno konfigurirati uvijek šifrirane postavljanjem stupca glavni ključ, stupac ključa za šifriranje i šifrirane stupaca umjesto vas.

1. Proširivanje **baze podataka** > **Clinic** > **tablice**.
2. Desnom tipkom miša kliknite tablicu **bolesnike koji dolaze** , a zatim odaberite **Šifriraj stupaca** da biste otvorili čarobnjak uvijek šifrirane:

    ![Šifriranje stupaca](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

Čarobnjak za uvijek šifrirane obuhvaća sljedeće odjeljke: **Odabir stupaca**, **Konfiguriranje glavnog ključa**, **provjere valjanosti**i **Sažetak**.

### <a name="column-selection"></a>Odabir stupca##

Kliknite **Dalje** na stranici **Uvod** da biste otvorili stranicu **Odabir stupaca** . Na ovoj stranici koju ćete odabrati koje stupce želite šifrirati, [vrstu šifriranja, a koje stupac ključa za šifriranje (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) da biste koristili.

Šifriranje **zdravstveno osiguranje** i **DatumRođenja** informacije za pacijent. Zdravstveno osiguranje stupac koristit će deterministic šifriranja koji podržava jednakosti pretraživanja, spajanja i Grupiraj po. Stupac DatumRođenja koristit će kombinaciji šifriranja koja ne podržava operacije.

Postavljanje **Vrsta šifriranja** za stupac zdravstveno osiguranje **Deterministic** i stupac DatumRođenja **Randomized**. Kliknite **Dalje**.

![Šifriranje stupaca](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Konfiguriranje glavnog ključa###

**Konfiguriranje glavnog ključa** je stranica kojem postaviti svoje CMK i odaberite ključ trgovine davatelja pohranjuju u CMK. Trenutno, možete spremiti na CMK u Windows spremište certifikata, Azure ključ sigurnog ili sigurnost modula hardver (HSM).

Pomoću ovog praktičnog vodiča pokazuje kako spremiti ključeva u sigurnog ključ Azure.

1.     Odaberite **Azure sigurnog ključa**.
1.     S padajućeg popisa odaberite željene ključne zbirke ključeva.
1.     Kliknite **Dalje**.

![Konfiguriranje glavnog ključa](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)


### <a name="validation"></a>Provjera valjanosti###

Možete šifriranje stupce sada ili kasnije pokrenuti skriptu PowerShell. Za ovaj vodič odaberite **nastavili da biste dovršili sada** , a zatim kliknite **Dalje**.

### <a name="summary"></a>Sažetak ###

Provjerite jesu li postavke sve ispraviti, a zatim kliknite **Završi** da biste dovršili postavljanje za uvijek šifrirane.


![Sažetak](./media/sql-database-always-encrypted-azure-key-vault/summary.png)


### <a name="verify-the-wizards-actions"></a>Provjerite je li u čarobnjaku akcije

Nakon što završite čarobnjak, bazu podataka postavljena za uvijek šifrirane. Čarobnjak izvesti sljedeće radnje:

- Stvorili stupac glavni ključ i pohranili ih u Azure ključ sigurnog.
- Stvara stupac ključa za šifriranje i spremljene u sigurnog ključ Azure.
- Konfigurirati odabranim stupcima za šifriranje. Tablica pacijenata koji je trenutno nema podataka, ali sada šifriran neki postojeći podaci u odabranim stupcima.

Stvaranje ključeva u SSMS možete provjeriti proširenjem **Clinic** > **Sigurnost** > **Uvijek šifrirane tipke**.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Stvaranje klijentska aplikacija koja koristi šifrirane podatke

Postavite uvijek šifrirane možete sastaviti aplikacije koja izvršava *umeće* i *odabire* šifrirane stupaca.  

> [AZURE.IMPORTANT] Aplikacija morate koristiti [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekte kada slanjem podataka običan tekst na poslužitelj s uvijek šifrirane stupaca. Prosljeđivanje slovni vrijednosti bez korištenja SqlParameter objekata rezultirat će iznimku.

1. Otvorite Visual Studio i stvorite novu C# konzole za aplikaciju. Provjerite je li projekta postavljeno na **.NET Framework 4.6** ili noviju verziju.
2. Dodijelite naziv projekta **AlwaysEncryptedConsoleAKVApp** i kliknite **u redu**.
![Nova aplikacija konzole](./media/sql-database-always-encrypted-azure-key-vault/console-app.png)
3. Instalirajte sljedeće paketa NuGet tako otvorite **alate**za > **Upravitelj paketa NuGet** > **Konzole za Upravitelj paketa**.

Pokrenite te dva retka koda na konzoli za Upravitelj paketa.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Izmjena niz za povezivanje da biste omogućili uvijek šifrirane

U ovom se odjeljku objašnjava kako omogućiti uvijek šifrirane u nizu za povezivanje baze podataka.


Da biste omogućili uvijek šifrirane, morate da biste dodali ključnu riječ **Postavku šifriranja stupca** niz za povezivanje i postavite ga na **omogućeno**.

To možete učiniti izravno u nizu za povezivanje ili možete postaviti da se pomoću [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Primjer aplikacije u sljedećem odjeljku pokazuje kako koristiti **SqlConnectionStringBuilder**.



### <a name="enable-always-encrypted-in-the-connection-string"></a>Omogućivanje uvijek šifrirane u nizu za povezivanje

Dodajte sljedeće ključnu riječ u nizu za povezivanje.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Omogući uvijek šifrirane i zaštićene SqlConnectionStringBuilder

Sljedeći kod prikazuje kako omogućiti uvijek šifrirane postavljanjem [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [omogućeno](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>Registrirajte se davatelja sigurnog ključ Azure

Sljedeći kod prikazuje kako registrirati davatelja sigurnog ključ Azure s upravljačkim ADO.NET.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Uvijek šifrirana aplikacije konzole uzorka

Ovaj primjer pokazuje kako:

- Izmjena niz za povezivanje da biste omogućili uvijek šifrirane.
- Registrirajte se Azure ključ sigurnog kao davatelja ključa pohrane aplikacije.  
- Umetnite podatke u šifrirane stupaca.
- Odaberite zapis filtriranjem određenu vrijednost u stupcu programa šifrirane.

Zamijenite sadržaj **Program.cs** sljedeći kod. Zamijenite niz za povezivanje za globalnu connectionString varijablu u retku koji izravno prethodi metodu glavne s vašeg valjani niz za povezivanje s portala za Azure. To je jedini promjena koje morate donijeti kod.

Pokrenite aplikaciju da biste vidjeli uvijek šifrirane u akciji.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }



## <a name="verify-that-the-data-is-encrypted"></a>Provjerite je li podatke šifriran

Brzo možete provjeriti stvarnih podataka na poslužitelju šifriran postavljanjem upita bolesnike koji dolaze podaci s SSMS (pomoću trenutnu vezu koju **Postavku šifriranja stupca** nije još omogućeno).

Pokrenite sljedeći upit Clinic bazu podataka.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Vidjet ćete šifrirane stupaca sadrže podatke običan tekst.

   ![Nova aplikacija konzole](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)


Da biste koristili SSMS za pristup podacima običan tekst, možete dodati na *postavku šifriranja stupac = omogućeno* parametar vezu.

1. U SSMS, desnom tipkom miša kliknite na poslužitelju u **Programu Explorer objekt** , a zatim odaberite **Prekini vezu**.
2. Kliknite **Poveži** > **: Modul baze podataka** da biste otvorili prozor **za povezivanje s poslužitelja** , a zatim kliknite **Mogućnosti**.
3. Kliknite **Dodatne parametre veze** pa upišite **postavku šifriranja stupac = omogućeno**.

    ![Nova aplikacija konzole](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)

4. Pokrenite sljedeći upit Clinic bazu podataka.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Sada možete vidjeti podatke običan tekst u šifrirane stupcima.


    ![Nova aplikacija konzole](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Daljnji koraci
Kada stvorite bazu podataka koja se koristi uvijek šifrirane, preporučujemo vam da učinite sljedeće:

- [Rotiraj i čišćenja ključeva](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migriranje podataka koji je već šifrirane i zaštićene uvijek šifrirane](https://msdn.microsoft.com/library/mt621539.aspx).


## <a name="related-information"></a>Povezane informacije

- [Uvijek šifrirane (razvoj klijenta)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Šifriranje prozirne podataka](https://msdn.microsoft.com/library/bb934049.aspx)
- [Šifriranje sustava SQL Server](https://msdn.microsoft.com/library/bb510663.aspx)
- [Uvijek šifrirana čarobnjaka](https://msdn.microsoft.com/library/mt459280.aspx)
- [Uvijek šifrirane bloga](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
