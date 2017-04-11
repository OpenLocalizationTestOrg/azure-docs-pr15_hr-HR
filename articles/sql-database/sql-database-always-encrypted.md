<properties
    pageTitle="Uvijek šifrirane: Zaštitite povjerljive podatke u bazi podataka SQL Azure s šifriranje baze podataka | Microsoft Azure"
    description="Zaštita povjerljive podatke u bazi podataka za SQL u minutama."
    keywords="šifriranje podataka, sql šifriranja, šifriranje baze podataka, a zatim osjetljive podatke uvijek šifrirane"
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

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Uvijek šifriranih: Zaštita povjerljive podatke u bazi podataka SQL i pohraniti ključeva za šifriranje u Windows spremište certifikata

> [AZURE.SELECTOR]
- [Azure sigurnog ključa](sql-database-always-encrypted-azure-key-vault.md)
- [Windows spremište certifikata](sql-database-always-encrypted.md)


U ovom se članku objašnjava sigurne osjetljivih podataka u bazi podataka sustava SQL s šifriranje baze podataka korištenjem [Uvijek šifrirane čarobnjaka](https://msdn.microsoft.com/library/mt459280.aspx) u [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Također pokazuje kako spremiti ključeva za šifriranje u Windows spremište certifikata.

Uvijek je šifrirana novu tehnologiju šifriranja podataka u bazi podataka SQL Azure SQL Server pomaže u zaštiti povjerljive podatke na ostale na poslužitelju, tijekom pomicanja između klijenta i poslužitelja i dok se podaci nalaze u koristi, nikad ne jamči osjetljivih podataka će se prikazati kao običan tekst unutar baze podataka sustava. Nakon šifriranja podataka samo klijentske aplikacije ili poslužitelje aplikacije koje imate pristup tipki može pristupiti podacima običan tekst. Detaljne informacije potražite u članku [Uvijek šifrirane (modul baze podataka)](https://msdn.microsoft.com/library/mt163865.aspx).

Nakon konfiguriranja baze podataka za korištenje uvijek šifrirane ćete stvoriti klijentske aplikacije u C# s Visual Studio za rad sa šifriranim podacima.

Slijedite korake u ovom članku da biste saznali kako postaviti uvijek šifrirane za baze podataka Azure SQL. U ovom se članku će naučiti kako izvršiti sljedeće zadatke:

- Pomoću čarobnjaka za uvijek šifrirane u SSMS da biste stvorili [Uvijek šifrirane ključeve](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
    - Stvaranje [Stupca glavni ključ (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Stvaranje [ključa za šifriranje stupca (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Stvaranje tablica baze podataka i šifriranje stupaca.
- Stvaranje aplikacije koja umeće odabire i prikazuje podatke iz šifrirane stupaca.

## <a name="prerequisites"></a>Preduvjeti

Za ovaj vodič, morat ćete:

- Račun za Azure i pretplate. Ako ga nemate, prijavite se za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verziju 13.0.700.242 ili noviji.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) ili noviji (na klijentskom računalu).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Stvorite praznu bazu podataka za SQL
1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **Novi** > **podataka + prostor za pohranu** > **SQL baze podataka**.
3. Stvorite **praznu** bazu podataka s nazivom **Clinic** na poslužitelju novi ili postojeći. Detaljne upute o stvaranju baze podataka na portalu za Azure potražite u članku [Stvaranje baze podataka SQL u minutama](sql-database-get-started.md).

    ![Stvorite praznu bazu podataka](./media/sql-database-always-encrypted/create-database.png)

Niz za povezivanje ćete kasnije u ovom praktičnom vodiču. Nakon stvaranja baze podataka, idite na novu Clinic bazu podataka i kopirajte niz za povezivanje. U bilo kojem trenutku možete dobiti niz za povezivanje, ali je lako da biste ga kopirali kada ste na portalu za Azure.

1. Kliknite **baze podataka SQL** > **Clinic** > **Prikaz niza veze baze podataka**.
2. Kopirajte niz za povezivanje za **ADO.NET**.

    ![Kopirajte niz za povezivanje](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Povezivanje s bazom podataka s SSMS

Otvorite SSMS i povezati s poslužiteljem s bazom podataka Clinic.


1. Otvorite SSMS. (Kliknite **Poveži** > **Modul baze podataka** da biste otvorili prozor **za povezivanje s poslužiteljem** ako nije otvorena).
2. Unesite naziv poslužitelja i vjerodajnice. Naziv poslužitelja možete pronaći na plohu baze podataka SQL i u nizu za povezivanje koju ste ranije kopirali. Upišite naziv poslužitelja za dovršavanje uključujući *database.windows.net*.

    ![Kopirajte niz za povezivanje](./media/sql-database-always-encrypted/ssms-connect.png)

Ako se otvori prozor **Novog pravila vatrozida** , prijavite se u Azure i pričekajte SSMS stvaranje novog pravila vatrozida za vas.


## <a name="create-a-table"></a>Stvaranje tablice

U ovom ćete odjeljku će stvoriti tablicu veličini pacijenta podataka. To će biti normalni tablice prethodno – će konfigurirati šifriranje u sljedećem odjeljku.

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

SSMS nudi Čarobnjak za jednostavno konfigurirati uvijek šifrirane postavljanjem CMK, CEK ili šifrirane stupaca umjesto vas.

1. Proširivanje **baze podataka** > **Clinic** > **tablice**.
2. Desnom tipkom miša kliknite tablicu **bolesnike koji dolaze** , a zatim odaberite **Šifriraj stupaca** da biste otvorili čarobnjak uvijek šifrirane:

    ![Šifriranje stupaca](./media/sql-database-always-encrypted/encrypt-columns.png)

Čarobnjak za uvijek šifrirane obuhvaća sljedeće odjeljke: **Odabir stupaca**, **Konfiguracije glavni ključ** (CMK), **Provjera valjanosti**i **Sažetak**.

### <a name="column-selection"></a>Odabir stupca ###

Kliknite **Dalje** na stranici **Uvod** da biste otvorili stranicu **Odabir stupaca** . Na ovoj stranici koju ćete odabrati koje stupce želite šifrirati, [vrstu šifriranja, i koje stupac ključa za šifriranje (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) da biste koristili.

Šifriranje informacija **zdravstveno osiguranje** i **DatumRođenja** pacijent. **Zdravstveno osiguranje** stupac koristit će deterministic šifriranja koji podržava jednakosti pretraživanja, spajanja i Grupiraj po. Stupac **DatumRođenja** koristit će kombinaciji šifriranja koja ne podržava operacije.

Postavljanje **Vrsta šifriranja** za stupac **zdravstveno osiguranje** **Deterministic** i stupac **DatumRođenja** **Randomized**. Kliknite **Dalje**.

![Šifriranje stupaca](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Konfiguriranje glavnog ključa###

**Konfiguriranje glavnog ključa** je stranica kojem postaviti svoje CMK i odaberite ključ trgovine davatelja pohranjuju u CMK. Trenutno, možete spremiti na CMK u Windows spremište certifikata, Azure ključ sigurnog ili sigurnost modula hardver (HSM). Pomoću ovog praktičnog vodiča pokazuje kako spremiti ključeva u Windows spremište certifikata.

Provjerite je li odabran **Windows spremište certifikata** , a zatim kliknite **Dalje**.

![Konfiguriranje glavnog ključa](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Provjera valjanosti###

Možete šifriranje stupce sada ili kasnije pokrenuti skriptu PowerShell. Za ovaj vodič odaberite **nastavili da biste dovršili sada** , a zatim kliknite **Dalje**.

### <a name="summary"></a>Sažetak###

Provjerite jesu li postavke sve ispraviti, a zatim kliknite **Završi** da biste dovršili postavljanje za uvijek šifrirane.

![Sažetak](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Provjerite je li u čarobnjaku akcije

Nakon što završite čarobnjak, bazu podataka postavljena za uvijek šifrirane. Čarobnjak izvesti sljedeće radnje:

- Stvorena je CMK.
- Stvorena je CEK.
- Konfigurirati odabranim stupcima za šifriranje. Tablica **pacijenata koji** je trenutno nema podataka, ali sada šifriran neki postojeći podaci u odabranim stupcima.

Stvaranje ključeva u SSMS možete provjeriti tako da **Clinic** > **Sigurnost** > **Uvijek šifrirane tipke**. Sada možete vidjeti novi prečaci koji se generira čarobnjak za vas.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Stvaranje klijentska aplikacija koja koristi šifrirane podatke

Postavite uvijek šifrirane možete sastaviti aplikacije koja izvršava *umeće* i *odabire* šifrirane stupaca. Da biste uspješno pokrenuti aplikaciju uzorka, pokrenite ga na istom računalu gdje ste pokrenuli čarobnjak za uvijek šifrirane. Da biste pokrenuli aplikaciju na neko drugo računalo, morate implementirati certifikatima uvijek šifrirane na računalo sa sustavom aplikaciju klijenta.  

> [AZURE.IMPORTANT] Aplikacija morate koristiti [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekte kada slanjem podataka običan tekst na poslužitelj s uvijek šifrirane stupaca. Prosljeđivanje doslovni vrijednosti bez korištenja SqlParameter objekata rezultirat će iznimku.


1. Otvorite Visual Studio i stvorite novu C# konzole za aplikaciju. Provjerite je li projektu postavljena na **.NET Framework 4.6** ili noviju verziju.
2. Dodijelite naziv projekta **AlwaysEncryptedConsoleApp** i kliknite **u redu**.

![Nova aplikacija konzole](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Izmjena niz za povezivanje da biste omogućili uvijek šifrirane

U ovom se odjeljku objašnjava kako omogućiti uvijek šifrirane u nizu za povezivanje baze podataka. Mijenjate aplikaciju konzole ste upravo stvorili u sljedećem odjeljku "Uvijek šifrirane aplikacije konzole za uzorak."


Da biste omogućili uvijek šifrirane, morate da biste dodali ključnu riječ **Postavku šifriranja stupca** niz za povezivanje i postavite ga na **omogućeno**.

To možete učiniti izravno u nizu za povezivanje ili možete postaviti da se pomoću [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Primjer aplikacije u sljedećem odjeljku pokazuje kako koristiti **SqlConnectionStringBuilder**.

> [AZURE.NOTE] To je potrebna za klijentske aplikacije određene da biste uvijek šifrirane samo promjena. Ako imate postojeću aplikaciju koja pohranjuje njegov niz za povezivanje vanjsko (to jest, u konfiguracijskoj datoteci), možda nećete moći omogućiti uvijek šifrirane bez promjene kod.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Omogućivanje uvijek šifrirane u nizu za povezivanje

Dodajte sljedeće ključnu riječ u nizu za povezivanje:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Omogući uvijek šifrirane i zaštićene na SqlConnectionStringBuilder

Sljedeći kod prikazuje kako omogućiti uvijek šifrirane postavljanjem [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [omogućeno](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Uvijek šifrirana aplikacije konzole uzorka

Ovaj primjer pokazuje kako:

- Izmjena niz za povezivanje da biste omogućili uvijek šifrirane.
- Umetnite podatke u šifrirane stupaca.
- Odaberite zapis filtriranjem određenu vrijednost u stupcu programa šifrirane.

Zamijenite sadržaj **Program.cs** sljedeći kod. Niz za povezivanje za globalnu connectionString varijablu u retku iznad metodu glavne zamijenite vaše valjani niz za povezivanje s portala za Azure. To je jedini promjena koje morate donijeti kod.

Pokrenite aplikaciju da biste vidjeli uvijek šifrirane u akciji.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
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

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
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

Brzo možete provjeriti postavljanjem upita **bolesnike koji dolaze** podaci SSMS šifriran stvarnih podataka na poslužitelju. (Koristi trenutnu vezu koju postavku šifriranja stupca nije još omogućena.)

Pokrenite sljedeći upit Clinic bazu podataka.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Vidjet ćete šifrirane stupaca sadrže podatke običan tekst.

   ![Nova aplikacija konzole](./media/sql-database-always-encrypted/ssms-encrypted.png)


Da biste koristili SSMS za pristup podacima običan tekst, možete dodati na **postavku šifriranja stupac = omogućeno** parametar vezu.

1. U SSMS, desnom tipkom miša kliknite na poslužitelju u **Programu Explorer objekt**i zatim kliknite **Prekini vezu**.
2. Kliknite **Poveži** > **Modul baze podataka** da biste otvorili prozor **za povezivanje s poslužitelja** , a zatim kliknite **Mogućnosti**.
3. Kliknite **Dodatne parametre veze** pa upišite **postavku šifriranja stupac = omogućeno**.

    ![Nova aplikacija konzole](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Pokrenite sljedeći upit **Clinic** bazu podataka.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Sada možete vidjeti podatke običan tekst u šifrirane stupcima.


    ![Nova aplikacija konzole](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Ako se povezujete s SSMS (ili bilo kojeg klijenta) s nekog drugog računala, neće imati pristup ključeva za šifriranje i nećete moći dešifrirati podatke.



## <a name="next-steps"></a>Daljnji koraci
Kada stvorite bazu podataka koja se koristi uvijek šifrirane, preporučujemo vam da učinite sljedeće:

- Pokrenite ovaj uzorak s nekog drugog računala. Da bi ga neće imati pristup podacima običan tekst i ne pokreće uspješno je nećete moći pristupiti ključeve za šifriranje.
- [Rotiraj i čišćenja ključeva](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migriranje podataka koji je već šifrirane i zaštićene uvijek šifrirane](https://msdn.microsoft.com/library/mt621539.aspx).
- [Implementacija uvijek šifrirane certifikata na drugim klijentskim računalima](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (pogledajte odjeljak "Upućivanje potvrde dostupne za aplikacije i korisnici").

## <a name="related-information"></a>Povezane informacije

- [Uvijek šifrirane (razvoj klijenta)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Šifriranje prozirne podataka](https://msdn.microsoft.com/library/bb934049.aspx)
- [Šifriranje SQL Server](https://msdn.microsoft.com/library/bb510663.aspx)
- [Uvijek šifrirane čarobnjaka](https://msdn.microsoft.com/library/mt459280.aspx)
- [Uvijek šifrirane bloga](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
