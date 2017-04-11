<properties
    pageTitle="Kopirajte podatke iz spremišta blobova s bazom podataka SQL | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča pokazuje kako pomoću aktivnosti Kopiraj u kanalu na tvorničke Azure podataka da biste kopirali podatke iz spremišta blobova s bazom podataka SQL."
    Keywords="blob sql, spremište blobova platforme kopiju podataka"
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
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Kopirajte podatke iz spremišta blobova u SQL baze podataka pomoću tvorničke podataka 
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopiranje čarobnjaka](data-factory-copy-data-wizard-tutorial.md)
- [Portal za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Predloška Azure Voditelj resursa](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-JA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API-JA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


U ovom ćete praktičnom vodiču stvorite podataka tvorničke s kanala da biste kopirali podatke iz spremišta blobova SQL baze podataka.

Kopiraj aktivnosti izvodi premještanje podataka na tvorničke Azure podataka. Pokreće se globalno dostupna usluga koje možete kopirati podataka između različitih podataka trgovine sigurne pouzdan te skalabilni način. Članak [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) potražite u članku dodatne informacije o aktivnosti Kopiraj.  

> [AZURE.NOTE] Detaljne pregled usluge tvorničke podataka potražite u članku [Uvod u tvorničke Azure podataka](data-factory-introduction.md) .

##<a name="prerequisites-for-the-tutorial"></a>Preduvjeti za vodič
Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Azure pretplate**.  Ako nemate pretplatu, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Besplatnu probnu verziju](http://azure.microsoft.com/pricing/free-trial/) .
- **Račun za azure prostora za pohranu**. Spremište blobova platforme koristiti kao izvor podataka **izvor** ovog praktičnog vodiča. Ako nemate račun za Azure prostora za pohranu, u članku [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) za korake da biste je stvorili.
- **Baze podataka azure SQL**. Korištenje baze podataka Azure SQL kao **odredišno** spremište podataka pomoću ovog praktičnog vodiča. Ako nemate baze podataka Azure SQL koje možete koristiti u ovom praktičnom vodiču, potražite u članku [Stvaranje i konfiguriranje baze podataka SQL Azure](../sql-database/sql-database-get-started.md) da biste je stvorili.
- **2014. / SQL Server 2012 ili Visual Studio 2013**. Pomoću SQL Server Management Studio ili Visual Studio stvaranje baze podataka za uzorak i prikaz rezultata podataka u bazi podataka.  

## <a name="collect-blob-storage-account-name-and-key"></a>Prikupljanje naziv računa za spremište blobova platforme i tipki 
Potreban je naziv računa i ključ za račun vašeg računa Azure prostora za pohranu za ovog praktičnog vodiča. Imajte na umu **naziv računa** i **ključ računa** za vaš račun za Azure prostora za pohranu.

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. **Druge usluge** na lijevom izborniku kliknite, a zatim odaberite **Računi za pohranu**.

    ![Pregled - račune za pohranu](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. U plohu **Prostora za pohranu računa** odaberite **račun za Azure prostora za pohranu** koji želite koristiti u ovom ćete praktičnom vodiču.
4. Odaberite vezu **tipke za pristup** u odjeljku **Postavke**.
5.  Kliknite gumb **Kopiraj** (slike) uz **naziv računa za** tekstni okvir i Spremi ga i zalijepite vezu negdje gdje (na primjer: u tekstnoj datoteci).
6. Ponavljajte prethodni korak Kopiraj ili bilješke dolje **ključ1**.
    
    ![Tipkovni prečac za pohranu](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Zatvorite sve blades tako da kliknete **X**.

## <a name="collect-sql-server-database-user-names"></a>Prikupljanje SQL server, baze podataka, korisnička imena
Morate imena Azure SQL poslužitelja, bazu podataka i korisnik učiniti pomoću ovog praktičnog vodiča. Imajte na umu dolje nazive **poslužitelja**, **bazu podataka**i **korisnika** za baze podataka Azure SQL.

1. **Portal za Azure**kliknite **dodatnih servisa** na lijevoj strani, a zatim odaberite **baze podataka SQL**.
2. U **SQL baza podataka plohu**odaberite **bazu podataka** koju želite koristiti u ovom ćete praktičnom vodiču. Imajte na umu dolje **Naziv baze podataka**.  
3. U plohu **SQL baze podataka** u odjeljku **Postavke**kliknite **Svojstva** .
4. Imajte na umu dolje vrijednosti za **Naziv POSLUŽITELJA** i **Prijava ADMINISTRATOR POSLUŽITELJA**.
5. Zatvorite sve blades tako da kliknete **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Dopusti Azure services da biste pristupili SQL server 
Provjerite je li **da **Dopusti pristup servisima Azure** postavku uključeno za poslužitelj za Azure SQL da servis tvorničke podataka možete pristupiti poslužitelj za Azure SQL** . Da biste provjerili i uključite tu postavku, učinite sljedeće:

1. Kliknite **Dodatne servise** središte na lijevoj strani, a zatim kliknite **SQL poslužitelja**.
2. Odaberite poslužitelj pa kliknite **vatrozid** u odjeljku **Postavke**. 
4. U plohu **Postavke vatrozida** kliknite **Dalje** za **Dopusti pristup Azure servisima**.
5. Zatvorite sve blades tako da kliknete **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Priprema blobova i SQL baze podataka 
Sada Priprema blobova platforme Azure i baze podataka Azure SQL za vodič pomoću sljedećih koraka:  

1. Pokretanje Blok za pisanje, zalijepite sljedeći tekst pa je spremite kao **emp.txt** **C:\ADFGetStarted** mapu na tvrdom disku.

        John, Doe
        Jane, Doe

2. Pomoću alata kao što su [Azure prostora za pohranu Explorer](https://azurestorageexplorer.codeplex.com/) da biste stvorili spremnik **adftutorial** i prijenos datoteke **emp.txt** spremniku.

    ![Explorer Azure prostora za pohranu. Kopirajte podatke iz spremišta blobova u SQL baze podataka](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Da biste stvorili tablicu **emp** u bazi podataka SQL Azure, koristite sljedeću skriptu SQL.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Ako imate SQL Server 2012/2014 instaliran na vašem računalu:** slijedite upute iz [Korak 2: povezivanje s bazom podataka SQL upravljanje Azure SQL baze podataka pomoću SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md#Step2) članak povezivanje s poslužiteljem Azure SQL i pokrenuti skriptu SQL. U ovom se članku koristi [klasične Azure portalu](http://manage.windowsazure.com), ne na [novom portalu Azure](https://portal.azure.com), Konfiguriranje Vatrozida za poslužitelj za Azure SQL.

    Ako vaš klijent nije dopuštena za pristup Azure SQL server, morate konfigurirati Vatrozid za poslužitelj za Azure SQL dopustili pristup s računala (IP adresa). Pogledajte [Ovaj članak](../sql-database/sql-database-configure-firewall-settings.md) upute za konfiguriranje vatrozida za poslužitelj za Azure SQL.

Dovršite preduvjete. Možete stvoriti na tvorničke podataka pomoću jednog od sljedećih načina. Kliknite jednu od kartice na vrhu ili na sljedećim vezama za izvođenje vodič.     

- [Portal za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API-JA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API-JA](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Kopiranje čarobnjaka](data-factory-copy-data-wizard-tutorial.md)
