<properties
   pageTitle="Povezivanje s bazom podataka SQL ili SQL Data Warehouse pomoću provjere autentičnosti Azure Active Directory | Microsoft Azure"
   description="Saznajte kako se povezati s bazom podataka SQL pomoću provjere autentičnosti Azure Active Directory."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Povezivanje s bazom podataka SQL ili SQL Data Warehouse pomoću provjere autentičnosti Azure Active Directory

Provjera autentičnosti za Azure Active Directory je mehanizam od povezivanje s bazom podataka Microsoft SQL Azure i [Skladištu podataka SQL](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) pomoću identiteta servisa Azure Active Directory (Azure AD). S provjerom autentičnosti Azure Active Directory, možete središnje Upravljanje identitetima korisnika baze podataka i druge Microsoftove servise na jednom središnjem mjestu. Središnje upravljanje ID-a nudi jedno mjesto za upravljanje korisnicima baze podataka i pojednostavnjuje upravljanje dozvolama. Prednosti obuhvaćaju sljedeće:

- Pruža alternative provjeru autentičnosti sustava SQL Server.
- Pridonosi zaustavite proliferation identitetima korisnika na poslužiteljima baze podataka.
- Omogućuje zakretanje lozinku na jednom mjestu
- Korisnici mogu upravljati dozvole za bazu podataka pomoću vanjske grupe (AAD).
- Omogućivanjem integriranu provjeru autentičnosti sustava Windows i druge oblike provjeru autentičnosti podržava Azure Active Directory ga možete isključiti pohranu lozinki.
- Koristi provjeru autentičnosti za Azure Active Directory sadrži korisnike baze podataka za provjeru identiteta na razini baze podataka.
- Azure Active Directory podržava provjeru autentičnosti utemeljenu na token za aplikacije za povezivanje s bazom podataka SQL.
- Azure Active Directory provjere autentičnosti podržava ADFS (vanjski pristup domeni) ili provjere autentičnosti nativni korisnika i lozinke za na lokalni Azure Active Directory bez sinkronizacije domene.  
- Azure Active Directory podržava veze iz sustava SQL Server Management Studio koje koriste Active Directory univerzalni provjeru autentičnosti, što obuhvaća višestruke provjere autentičnosti (MFA).  MFA obuhvaća Jaka provjera autentičnosti s raspon mogućnosti jednostavno provjere – telefonskog poziva, tekstne poruke, a zatim pametne kartice s PIN-a ili obavijest o mobilnoj aplikaciji. Dodatne informacije potražite u članku [SSMS podrške za Azure AD MFA s bazom podataka SQL i SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).

Koraci za konfiguraciju uključuju sljedeće postupke za konfiguriranje i korištenje provjere autentičnosti Azure Active Directory.

1. Stvaranje i popunjavanje Azure Active Directory.
2. Provjerite je li bazu podataka u V12 za baze podataka SQL Azure. (Nije potrebno za SQL Data Warehouse.)
3. Neobavezno: Pridružiti ili promijeniti koji je trenutno povezan s pretplatom Azure active directory.
4. Stvaranje administrator Azure Active Directory za Azure SQL Server ili [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Konfiguriranje klijentskim računalima.
6. Stvaranje korisnika sadrži baze podataka u bazi podataka mapirati Azure AD identiteta.
7. Povezivanje baze podataka pomoću identiteta Azure AD.


## <a name="trust-architecture"></a>Pouzdanost arhitekture

Sljedeći dijagram više razine navedene pomoću provjere autentičnosti za Azure AD s bazom podataka SQL Azure arhitektura rješenja. Isti koncepti se primjenjuju na SQL Data Warehouse. Da biste podržali Azure Active Directory izvorne korisničke lozinke, smatra se samo dio oblaka i Azure AD-Azure SQL baze podataka. Za podršku vanjskim provjere autentičnosti (ili korisnika i lozinke za vjerodajnice sustava Windows), potreban je komunikacije bojom ADFS. Strelice pokazuju staze komunikacije.

![Dijagram aad provjere autentičnosti][1]

Na sljedećem su dijagramu označava vanjski pristup, pouzdanost i hostinga odnosa koji dopušta povezivanje s bazom podataka slanjem token klijentu. Token provjere autentičnosti po Azure AD i pouzdanim iz baze podataka. Korisnička 1 može se odnositi na Azure Active Directory s nativni korisnicima ili Azure Active Directory s vanjskim korisnicima. Kupcu 2 predstavlja dostupno rješenje uključujući uvezene korisnike; u ovom primjeru koji dolaze iz s pridruženim Azure Active Directory uz ADFS sinkronizira sa servisom Azure Active Directory. Važno je da razumijete pristup bazi podataka pomoću provjere autentičnosti Azure AD zahtijeva hostinga pretplate povezan Azure Active Directory. Da biste stvorili SQL Server hostira baze podataka SQL Azure ili SQL Data Warehouse koristi se iste pretplate.

![odnos pretplate][2]

## <a name="administrator-structure"></a>Administrator strukture

Kada pomoću provjere autentičnosti Azure AD, postoje dvije administratorske račune za poslužitelj baze podataka SQL; izvorni administrator sustava SQL Server i Azure AD administrator. Isti koncepti se primjenjuju na SQL Data Warehouse. Samo administrator temeljenu na poslovnom kontaktu Azure AD možete stvoriti prvi Azure AD nalazi korisnik baze podataka u bazi podataka za korisnika. Prijava za Azure AD administrator može biti Azure AD korisnik ili grupa programa Azure AD. Kada je administrator računom grupa, moguće je koristiti bilo koji član grupe, omogućivanje više administratorima Azure AD instancu sustava SQL Server. Pomoću grupa računa kao administrator poboljšava mogućnost upravljanja omogućujući vam da središnje Dodavanje i uklanjanje članova grupe Azure AD bez promjene korisnici i dozvole u SQL baze podataka. Samo jedan Azure AD administrator (korisnik ili grupa) moguće je konfigurirati u bilo kojem trenutku.

![administrator strukture][3]

## <a name="permissions"></a>Dozvole

Da biste stvorili novi korisnici, morate imati na `ALTER ANY USER` dozvola u bazi podataka. Na `ALTER ANY USER` dozvola koje se mogu dodijeliti svim korisnicima baze podataka. Na `ALTER ANY USER` dozvola i zaključale poslužitelj administratorske račune i korisnicima baze podataka u `CONTROL ON DATABASE` ili `ALTER ON DATABASE` dozvola za baze podataka, a zatim članovi u `db_owner` uloga baze podataka.

Da biste stvorili korisnik sadržavao baze podataka u bazi podataka SQL Azure ili SQL Data Warehouse, morate se povezati s bazom podataka pomoću identitet Azure AD. Da biste stvorili prvog korisnika sadrži baze podataka, morate se povezati s bazom podataka pomoću administrator Azure Active Directory (koji je vlasnik baze podataka). To je planirati korake 4 i 5 u nastavku. Sve provjere autentičnosti Azure Active Directory je samo ako je administrator Azure Active Directory stvoren za baze podataka SQL Azure ili SQL Data Warehouse server. Ako administrator servisa Azure Active Directory uklonjena s poslužitelja, postojeće Azure Active Directory korisnike stvorili prethodno unutar SQL Server možete više s bazom podataka pomoću vjerodajnica Azure Active Directory.

## <a name="azure-ad-features-and-limitations"></a>Azure AD značajke i ograničenja

Sljedeće članove Azure Active Directory mogu biti dodijeljena Azure SQL Server ili SQL Data Warehouse:

- Izvorni članova: člana stvorene u Azure AD upravljanih domene ili domene klijenta. Dodatne informacije potražite u članku [Dodavanje vlastiti naziv domene za Azure AD](../active-directory/active-directory-add-domain.md).

- Vanjskih domena članova: člana stvorene u Azure AD pomoću pridruženoj domeni. Dodatne informacije potražite u članku [Microsoft Azure sada podržava vanjski pristup u sustavu Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).

- Uvezene članove iz imenika Active druge Azure koji su članovi nativne ili pridruženim domene.

- Grupama imenika Active Directory stvoreni kao sigurnosne grupe.

Microsoftovi računi (na primjer, outlook.com, hotmail.com, live.com) ili drugim račune za goste (na primjer gmail.com, yahoo.com) nisu podržane. Ako možete se prijaviti na [https://login.live.com](https://login.live.com) pomoću račun i lozinku, pa koristite Microsoftov račun koji nije podržan za provjeru autentičnosti Azure AD za baze podataka SQL Azure ili skladištu podataka za SQL Azure.

### <a name="additional-considerations"></a>Dodatne napomene

- Da biste poboljšali mogućnost upravljanja, preporučujemo da Dodjela namjenski grupe Azure Active Directory kao administrator.
- Samo jedan Azure AD administrator (korisnik ili grupa) moguće je konfigurirati za Azure SQL Server ili Azure SQL Data Warehouse u bilo kojem trenutku.
- Samo Azure Active Directory administrator za SQL Server na početku možete povezati s Azure SQL Server ili Azure SQL Data Warehouse pomoću računa sustava Azure Active Directory. Administrator servisa Active Directory može konfigurirati naknadnim korisnicima Azure Active Directory baze podataka.
- Preporučujemo da postavke vremensko ograničenje veze u 30 sekundi.
- SQL Server 2016 Management Studio i SQL Server Data Tools za Visual Studio 2015 (verzija 14.0.60311.1April 2016 ili noviji) podržava provjeru autentičnosti Azure Active Directory. (Provjera autentičnosti azure Active Directory podržana od **.NET Framework davatelj podataka za SqlServer**; pri najmanje verzije .NET Framework 4.6). Stoga najnovije verzije sustava alata i aplikacija sloja podataka (DAC i .bacpac) možete koristiti provjeru autentičnosti Azure Active Directory.
- [ODBC verzija 13,1](https://www.microsoft.com/download/details.aspx?id=53339) podržava provjeru autentičnosti Azure Active Directory no `bcp.exe` ne možete povezati pomoću provjere autentičnosti za Azure Active Directory jer koristi starije usluga za ODBC.
- `sqlcmd`podržava Azure Active Directory provjere autentičnosti počevši od verzije 13,1 dostupne putem [Centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkID=825643).  
- SQL Server Data Tools za Visual Studio 2015 zahtijeva barem 2016 u travnju verziju Alati podataka (verzija 14.0.60311.1). Trenutno Azure Active Directory korisnika ne prikazuju u programu Explorer SSDT objekta. Kao zaobilazno rješenje, pogledati korisnike [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
- [Microsoft JDBC 6.0 upravljački program za SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) podržava provjeru autentičnosti Azure Active Directory. Osim toga, pogledajte odjeljak [Postavljanje svojstva veze](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase nije moguće provjeriti autentičnost pomoću provjere autentičnosti Azure Active Directory.
- Neke alate kao što su BI i programa Excel nisu podržane.
- Provjera autentičnosti za Azure Active Directory podržava za bazu podataka sustava SQL na Azure portala **Uvoz** i **Izvoz baza podataka** blades. Uvoz i izvoz pomoću provjere autentičnosti Azure Active Directory podržano je i od naredbe ljuske PowerShell.


## <a name="1-create-and-populate-an-azure-ad"></a>1. Stvaranje i popunjavanje Azure AD

Stvaranje Azure Active Directory i popunjavanje s korisnicima i grupama. Azure Active Directory može biti početnu domenu Azure AD upravljanih domene. Azure Active Directory i može se lokalni Active Directory Domain Services združenu s Azure Active Directory.

Dodatne informacije potražite u članku [integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](../active-directory/active-directory-aadconnect.md), [dodajte vlastiti naziv domene za Azure AD](../active-directory/active-directory-add-domain.md), [Microsoft Azure sada podržava vanjski pristup u sustavu Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [administriranje imenika Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)i [Upravljanje Azure AD pomoću komponente Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## <a name="2-ensure-your-sql-database-is-version-12"></a>2. Provjerite je li SQL baze podataka verzije 12

Provjera autentičnosti za Azure Active Directory podržava najnovije V12 baze podataka SQL. Informacije o V12 SQL baze podataka, a da biste saznali je li dostupna u vašoj regiji potražite u članku [što je novo u najnovije ažuriranje V12 za SQL baze podataka](sql-database-v12-whats-new.md). Ovaj korak nije potreban za Azure SQL Data Warehouse jer SQL Data Warehouse dostupna je samo u V12.

Ako imate postojeću bazu podataka, provjerite je li da se hostira u SQL baze podataka V12 povezivanje s bazom podataka (primjerice se pomoću SQL Server Management Studio) i izvođenju `SELECT @@VERSION;`. Očekivani rezultat za bazu podataka u V12 baze podataka SQL je najmanje **Microsoft SQL Azure (RTM) - 12,0**. Ako bazu podataka ne hostira u SQL baze podataka V12, pročitajte članak [Planiranje i pripremite za nadogradnju na V12 baze podataka SQL](sql-database-v12-plan-prepare-upgrade.md), a zatim posjetite Azure klasičnih Portal za migraciju baze podataka u V12 baze podataka SQL.

Osim toga, možete stvoriti novu bazu podataka u SQL baze podataka V12 slijedeći korake navedene u [Stvaranje prve baze podataka Azure SQL](sql-database-get-started.md). **Savjet**: pročitajte sljedeći korak prije no što odaberete pretplatu za novu bazu podataka.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. Neobavezno: Pridružiti ili promijeniti koji je trenutno povezan s pretplatom Azure active directory

Da biste povezali bazu podataka s imenikom servisa Azure AD za tvrtku ili ustanovu, provjerite imenika pouzdanih direktorija za Azure pretplatu hostira baze podataka. Dodatne informacije potražite u članku [kako Azure pretplate pridružuju Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Dodatne informacije:** Svaki Azure pretplate ima pouzdani odnos s Azure AD instance. To znači da je smatra pouzdanima taj imenik za provjeru autentičnosti korisnika, servise i uređaje. Višestruke pretplate pouzdani isti direktorija, ali samo jedan direktorija smatra pouzdanima pretplatu. Možete vidjeti koje direktorija pouzdanim vaša pretplata na kartici **Postavke** na [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Pouzdani odnos koji ima pretplate s direktorij je odnos koji ima pretplatu sa svih drugih resursa u Azure (web-mjesta, baza podataka i tako dalje) koje su više kao podređeni resursi pretplate. Ako pretplata istekne, zatim pristup tim resursima povezan s pretplatom i prekida. No imenika ostaje u Azure, a možete pridružiti drugu pretplatu na taj imenik i nastaviti za upravljanje korisnicima direktorija. Dodatne informacije o resursima potražite u članku [objašnjenje resursa pristup servisu Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Sljedećih postupaka sadrže korak po korak upute o tome kako promijeniti pridružene direktorija za dani pretplatu.

1. Povezivanje s [Azure klasični Portal](https://manage.windowsazure.com/) putem administrator Azure pretplate.
2. Na lijevoj natpis odaberite **Postavke**.
3. Pretplate se prikazivati na zaslonu postavke. Ako se željeni pretplate ne prikazuje, kliknite **pretplate** na vrhu, padajući okvir **Filtar tako da IMENIKA** i odaberite direktorij koji sadrži pretplate, a zatim **PRIMIJENI**.

    ![Odaberite pretplatu][4]
4. U području **Postavke** kliknite pretplatu, a zatim **Uređivanje DIRECTORY** pri dnu stranice.

    ![ad postavke portal][5]
5. U okvir za **Uređivanje DIREKTORIJA** odaberite Azure Active Directory koji je povezan s SQL Server ili SQL Data Warehouse, a zatim strelicu uz dalje.

    ![Uređivanje direktorija odabir][6]
6. U dijaloškom okviru **Potvrda** directory mapiranje provjerite "**uklonit će sve dodatnih administratora.**"

    ![Uređivanje-directory-potvrda][7]
7. Kliknite Provjeri da biste ponovno učitali portalu.

> [AZURE.NOTE] Kada promijenite direktorija, pristup svim dodatnih administratora, Azure AD korisnici i grupe i uklanjaju direktorija sigurnosno resursa korisnicima i više imaju pristup ove pretplate ili njezinu resursi. Samo, administrator servisa, možete konfigurirati pristup za Upravitelji na temelju novog direktorija. Ta promjena može potrajati znatno vremenskog razdoblja zbog primjene na sve resurse. Promjena direktorija, mijenja administrator Azure AD za SQL baze podataka i SQL Data Warehouse i ne dopušta pristup bazi podataka za sve postojeće korisnike Azure AD. Azure AD administrator mora biti ponovno postavljanje (prema uputama u nastavku) i novi Azure AD korisnici moraju se stvoriti.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. stvaranje administrator Azure AD za Azure SQL Server

Svaki Azure SQL Server (koji hostira baze podataka SQL ili SQL Data Warehouse) pokreće se s računom za administratora poslužitelja koji je administrator cijelu Azure SQL Server. Drugi administrator sustava SQL Server potrebno je stvoriti, koji je račun za Azure AD. U ovom glavnicu stvara se kao korisnik sadržavao baze podataka u glavnom bazom podataka. Kao administratori administratorske račune poslužitelja su članovi uloge **db_owner** u bazi podataka svakog korisnika pa unesite svaki korisnik bazu podataka kao korisnik **vlasnika baze podataka** . Dodatne informacije o računima administratora poslužitelja u odjeljku [Upravljanje bazama podataka i prijave u bazi podataka SQL Azure](sql-database-manage-logins.md) i odjeljka **prijave i korisnici** [upute o sigurnosti baze podataka SQL Azure i ograničenja](sql-database-security-guidelines.md).

Kada Azure Active Directory pomoću zemlj replikacije Azure Active Directory administrator mora biti konfigurirana za primarnih i sekundarnih poslužiteljima. Ako na poslužitelju ne postoji administrator Azure Active Directory, zatim prijave Azure Active Directory i korisnicima primati na "ne možete povezati" Pogreška poslužitelja.

> [AZURE.NOTE] Korisnici koji se temelje na račun za Azure AD (uključujući administratorski račun za Azure SQL Server), nije moguće stvoriti Azure AD-poštu korisnika, jer nemaju dozvole za provjeru valjanosti predložene baze podataka korisnicima Azure AD.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Dodjela Azure Active Directory administratora sustava SQL Server Azure pomoću portala za Azure

1. [Portal za Azure](https://portal.azure.com/)u gornjem desnom kutu kliknite vezu na padajućem popisu mogućih aktivnog direktorija. Odaberite ispravan Active Directory kao zadano Azure AD. Ovaj korak povezuje pridruživanje pretplate s Azure SQL Server Provjerite koristi li se iste pretplate za oba servisa Active Directory Azure AD i SQL Server. (Azure SQL Server može biti hostira baze podataka SQL Azure ili Azure SQL Data Warehouse.)

    ![Odaberite ad][8]
2. U lijevom natpis odaberite **SQL Server**, odaberite **SQL server**, a zatim u Elektronička ploča **Poslužitelja za SQL** pri vrhu kliknite **Postavke**.

    ![Postavke servisa Active Directory][9]
3. U plohu **Postavke** kliknite ** administrator servisa Active Directory.
4. U plohu **administrator servisa Active Directory** kliknite **administrator servisa Active Directory**, a zatim pri vrhu kliknite **Postavi administrator**.
5. U plohu **Dodaj administrator** , traženje korisnika, odaberite korisnika ili grupu biti administrator, a zatim **Odaberite**. (Plohu administrator servisa Active Directory prikazuje sve članove i grupa servisa Active Directory. Korisnike ili grupe kojima su zasivljene nije moguće odabrati jer nije podržana kao Azure AD administratori. (Pogledajte popis podržanih administratori sustava **Azure AD značajke i ograničenja** iznad). Kontrola pristupa na temelju uloga (RBAC) odnosi se samo na portalu, a ne prenose se na SQL Server.
6. Pri vrhu plohu **administrator servisa Active Directory** , kliknite **SPREMI**.
    ![Odaberite administrator][10]

    Postupak za promjenu administrator može potrajati nekoliko minuta. Zatim novog administratora pojavit će se u okviru **administrator servisa Active Directory** .

> [AZURE.NOTE] Kada postavljate Azure AD administrator, novi naziv administrator (korisnik ili grupa) nije moguće već postojati u virtualne glavnom bazom podataka kao korisnik provjeru autentičnosti sustava SQL Server. Ako postoje, instalacijski program za administratore Azure AD neće uspjeti; Vraćanje njegova stvaranja i koja označava taj takve administrator (ime) već postoji. Budući da se takvo SQL Server provjere autentičnosti korisnika nije dio Azure AD, sve trud povezati s poslužiteljem pomoću provjere autentičnosti Azure AD neće uspjeti.

Da biste kasnije uklonili administrator, pri vrhu plohu **administrator servisa Active Directory** kliknite **Ukloni administrator**, a zatim kliknite **Spremi**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Dodjela administrator Azure AD za Azure SQL Server pomoću komponente PowerShell

Da biste pokrenuli cmdleta ljuske PowerShell, morate koristiti Azure PowerShell instaliran i pokrenut. Detaljne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

Dodjela administrator Azure AD izvršiti sljedeće naredbe Azure PowerShell:

- Dodavanje AzureRmAccount
- Odaberite AzureRmSubscription


Cmdleti za koristi se za dodjelu resursa i upravljanje Azure AD administrator:

| Naziv cmdleta                                       | Opis                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Postavljanje AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Dodjeljuje administrator Azure Active Directory za Azure SQL Server ili Azure SQL Data Warehouse. (Mora biti iz trenutne pretplate.) |
| [Uklanjanje AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Uklanja administrator Azure Active Directory za Azure SQL Server ili skladištu podataka za SQL Azure. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Vraća informacije o administratoru Azure Active Directory trenutno postavljenima za Azure SQL Server ili Azure SQL Data Warehouse. |

Korištenje ljuske PowerShell naredbu-pomoć da biste vidjeli dodatne detalje za svaki od tih naredbi, na primjer ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Dodjeljuje resurse za sljedeće skripte grupe administratora sustava Azure AD pod nazivom **DBA_Group** (id objekta `40b79501-b343-44ed-9ce7-da4c8cc7353f`) za **demo_server** poslužitelja u grupu resursa pod nazivom **grupe 23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

Na ulazni parametar **riješiti problem** prihvaća Azure AD zaslonsko ime ili korisnikovo Glavno ime. Na primjer, ``DisplayName="John Smith"`` i ``DisplayName="johns@contoso.com"``. Za Azure AD grupe Azure AD podržano je zaslonsko ime.

> [AZURE.NOTE] Naredba Azure PowerShell ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` sprječava dodjeljivanje Azure AD administratori za korisnika nije podržan. Nepodržane korisnika možete dodjeli, ali ne možete povezati s bazom podataka. (Pogledajte popis podržanih administratori sustava **Azure AD značajke i ograničenja** iznad).

Sljedeći primjer koristi neobavezno **ID objekta**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Kada se **riješiti problem** nisu jedinstveni, potreban je Azure AD **ID objekta** . Da biste dohvatili vrijednosti **ID objekta** i **riješiti problem** , pomoću rubrike servisa Active Directory Azure klasični Portal i prikaz svojstava korisnika ili grupu.

Sljedeći primjer vraća informacije o trenutnoj administrator Azure AD za Azure SQL Server:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

U sljedećem primjeru uklanja administrator Azure AD:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Možete i dodjelu resursa Azure Active Directory Administrator pomoću REST API-ji. Dodatne informacije potražite u članku [servis za upravljanje REST API Reference i postupci za postupke za baze podataka SQL Azure za baze podataka SQL Azure](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. konfiguriranje klijentskim računalima

Na svim klijentskim računalima, iz kojeg aplikacije ili pojedinačnim korisnicima povezivanje baze podataka SQL Azure ili Azure SQL Data Warehouse korištenju Azure AD identiteta, potrebno je instalirati sljedeće programe:

- .NET framework 4.6 ili noviji s [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Provjera autentičnosti biblioteke imenika Azure Active Directory za SQL Server (**ADALSQL. DLL**) je dostupan na više jezika (x86 i amd64) iz centra za preuzimanje na [Microsoft provjera autentičnosti biblioteke imenika Active Directory za Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

### <a name="tools"></a>Alati

- Instalacija [Sustava SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ili [SQL Server Data Tools za Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) zadovoljava obavezne .NET Framework 4.6.
- Verzija instalira na x86 SSMS **ADALSQL. DLL**.
- SSDT instalira amd64 verziju **ADALSQL. DLL**.
- Najnovije Visual Studio iz [Visual Studio preuzimanja](https://www.visualstudio.com/downloads/download-visual-studio-vs) zadovoljava preduvjete za .NET Framework 4.6, ali instalirati verziju potrebna amd64 **ADALSQL. DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. Stvaranje korisnika sadrži baze podataka u bazi podataka mapirani identitetima Azure AD

### <a name="about-contained-database-users"></a>O korisnicima sadržavao baze podataka

Azure Active Directory provjere autentičnosti zahtijeva korisnika baza podataka će biti stvoren kao korisnici sadržavao baze podataka. Korisnik sadržavao baze podataka na temelju identitet Azure AD je korisnik baze podataka koje imaju prijave u glavnom bazom podataka i koja mapira identiteta u imeniku Azure AD koji je povezan s bazom podataka. Identitet Azure AD može biti pojedinačni korisnički račun ili grupu. Dodatne informacije o korisnicima sadržavao baze podataka potražite u članku [Nalazi korisnici baze podataka unošenju vaše prijenosni baze podataka](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Korisnici baze podataka (osim za administratore) nije moguće stvoriti pomoću portala. Uloge RBAC ne prenose se na SQL Server, SQL baze podataka ili SQL Data Warehouse. Azure RBAC uloge se koriste za upravljanje resursima Azure, a ne primjenjuju se na dozvole za bazu podataka. Na primjer, ulogu **Suradnika SQL Server** dopustiti pristup za povezivanje s bazom podataka SQL ili SQL Data Warehouse. Dozvole za pristup mora biti odobren izravno u bazu podataka pomoću Transact-SQL naredbe.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Povezivanje s korisnik skladištu baze podataka ili podataka pomoću SQL Server Management Studio ili SQL Server Data Tools

Da biste potvrdili Azure AD administrator je ispravno postavljena, povezivanje s **glavnom** bazom podataka pomoću administratorskog računa Azure AD.
Dodjela programa Azure AD-poštu nalazi korisnik baze podataka (osim administratora poslužitelja koja je vlasnik baze podataka), povezivanje s bazom podataka s identitet Azure AD koja ima pristup bazi podataka.

> [AZURE.IMPORTANT] Podrška za provjeru autentičnosti Azure Active Directory dostupan je sa [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) i [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) za Visual Studio 2015. Izdanje kolovoz 2016 SSMS obuhvaća i podršku za Active Directory Univerzalna provjeru autentičnosti, što administratorima omogućuje zahtijevaju višestruka provjera autentičnosti pomoću telefonskog poziva, tekstne poruke pametne kartice s PIN-a ili obavijest o mobilnoj aplikaciji.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Povezivanje putem servisa Active Directory integriranu provjeru autentičnosti

Ovu metodu koristite ako ste prijavljeni pomoću vjerodajnica Azure Active Directory s pridruženim domene u sustavu Windows.

1. Pokrenite Management Studio ili Alati za podatke i u dijaloškom okviru **za povezivanje s poslužiteljem** (ili **Povezivanje s modul baze podataka**) u okviru **Provjera autentičnosti** odaberite **Active Directory integrirane provjere**. Bez lozinke potreban je ili može unijeti jer postojeće vjerodajnice prikazat će se veze.
    ![Odaberite AD integriranu provjeru autentičnosti][11]

2. Kliknite gumb **Mogućnosti** , a zatim na stranici **Svojstva veze** , u okvir za **Povezivanje s bazom podataka** upišite naziv baze podataka korisnika koji se želite povezati.
    ![Odaberite naziv baze podataka][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Povezivanje putem provjeru autentičnosti lozinke servisa Active Directory

Ovaj postupak koristite kada se povezujete s nazivom glavni Azure AD pomoću Azure AD upravlja domene. Možete koristiti i ga za pridruženi račun bez pristupa domenu, na primjer kada radili s udaljenog mjesta.

Ovaj postupak koristite ako ste prijavljeni u sustav Windows pomoću vjerodajnica iz domene koji nije naveden kao pridruženi s Azure ili kada se korištenjem provjere autentičnosti Azure AD pomoću Azure AD na temelju na početni ili domene klijenta.

1. Pokrenite Management Studio ili Alati za podatke i u dijaloškom okviru **za povezivanje s poslužiteljem** (ili **Povezivanje s modul baze podataka**) u okviru **Provjera autentičnosti** odaberite **Provjeru autentičnosti lozinke Active Directory**.
2. U okvir **korisničko ime** upišite svoje korisničko ime Azure Active Directory u obliku **username@domain.com**. To mora biti računa iz Azure Active Directory ili računa iz domene združivanje s Azure Active Directory.
3. U okvir **Lozinka** unesite korisničko lozinku za račun servisa Azure Active Directory ili vanjskog račun domene.
    ![Odaberite provjeru autentičnosti lozinke AD][12]

4. Kliknite gumb **Mogućnosti** , a zatim na stranici **Svojstva veze** , u okvir za **Povezivanje s bazom podataka** upišite naziv baze podataka korisnika koji se želite povezati. (Pogledajte sliku u na prethodnu mogućnost).


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Stvaranje Azure AD nalazi korisnik baze podataka u bazi podataka za korisnika

Da biste stvorili programa Azure AD-poštu nalazi korisnik baze podataka (osim administratora poslužitelja koja je vlasnik baze podataka), s bazom podataka s identitetom Azure AD kao korisnik s najmanje **PROMIJENITI bilo koje KORISNIČKIH** dozvola. Zatim upotrijebite sljedeću sintaksu Transact-SQL:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* može biti korisničko ime glavni Azure AD korisnik ili zaslonsko ime za Azure AD grupi.

**Primjeri:** Da biste stvorili bazu podataka sadrži korisnika koji predstavlja Azure AD vanjskog ili upravlja korisnik domene:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Da biste stvorili korisnik sadržavao baze podataka koji predstavlja Azure AD ili vanjskog grupa domene, unesite zaslonsko ime sigurnosne grupe:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Da biste stvorili korisnik sadržavao baze podataka koji predstavlja aplikacije koja povezuje pomoću token za Azure AD:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Dodatne informacije o stvaranju korisnika sadrži baze podataka na temelju identiteta Azure Active Directory potražite u članku [Stvaranje korisnika (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Uklanjanje administratora Azure Active Directory za Azure SQL Server onemogućuje sve Azure AD provjere autentičnosti korisnika povezivanja s poslužiteljem. Ako je potrebno, neupotrebljiv Azure AD korisnici mogu biti odbačeni ručno administrator baze podataka SQL.

Kada stvorite korisnik baze podataka, taj korisnik dobiva dozvolu **za POVEZIVANJE** , a možete se povezati s tu bazu podataka kao član uloge za **JAVNO** . Na početku samo dozvole za korisnika su sve dozvole dodijeljene **JAVNO** uloga ili bilo koji odobren za grupe sustava Windows koje su član. Nakon dodjele resursa za Azure AD-poštu nalazi korisnik baze podataka, možete dodijeliti korisnika dodatne dozvole, na isti način kao što je Dodjela dozvola za neku drugu vrstu korisnika. Obično Dodjela dozvola za uloge baze podataka i dodavanje korisnika u uloge. Dodatne informacije potražite u članku [Osnove dozvola modul za baze podataka](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Dodatne informacije o ulogama posebno baze podataka SQL potražite u članku [Upravljanje bazama podataka i prijave u bazi podataka SQL Azure](sql-database-manage-logins.md).
Pridruženoj domeni korisnika koji su uvezeni u upravljanje domenu, morate koristiti upravljane domene identitet.

> [AZURE.NOTE] Azure AD korisnici označeni su u metapodacima baze podataka s vrstom E (EXTERNAL_USER) i za grupe s vrstom X (EXTERNAL_GROUPS). Dodatne informacije potražite u članku [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).


## <a name="7-connect-by-using-azure-ad-identities"></a>7. povezati pomoću identiteta Azure AD

Azure Active Directory provjeru autentičnosti podržava povezivanje s bazom podataka pomoću identiteta Azure AD sljedećih metoda:

- Korištenje integriranu provjeru autentičnosti sustava Windows
- Korištenje Azure AD Glavno ime i lozinku
- Pomoću aplikacije tokena provjere autentičnosti

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Povezivanje putem integriranu provjeru autentičnosti (Windows)

Da biste koristili integrirani provjeru autentičnosti sustava Windows, vaše domene servisa Active Directory morate vanjskog s Azure Active Directory. Klijentska aplikacija (ili uslugu) povezivanje s bazom podataka mora biti pokrenut na računalo pridruženo za domene u odjeljku korisničke vjerodajnice za domene.

Da biste se povezali s bazom podataka pomoću integriranu provjeru autentičnosti i identitet Azure AD, provjere autentičnosti ključne riječi u nizu za povezivanje baze podataka mora biti postavljeno na integrirani Active Directory. Sljedeći C# kod primjer koristi ADO .NET.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Bilješke veza niza je ključnu riječ ``Integrated Security=True`` nije podržana za povezivanje s bazom podataka SQL Azure.
Imajte na umu da pri stvaranju ODBC vezu morat ćete ukloniti razmake i postavili provjeru autentičnosti za "ActiveDirectoryIntegrated".

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7,2. Povezivanje s Azure AD Glavno ime i lozinku
Da biste se povezali s bazom podataka pomoću integriranu provjeru autentičnosti i identitet Azure AD, ključnih riječi za provjeru autentičnosti mora biti postavljeno na Active Directory lozinku. Niz za povezivanje mora sadržavati ID UID korisnika i lozinka/PWD ključne riječi i vrijednosti. Sljedeći C# kod primjer koristi ADO .NET.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Saznajte više o načinima provjere autentičnosti Azure AD pomoću dostupna primjere koda pokazni videozapis na [Pokazni videozapis za GitHub provjere autentičnosti za Azure AD](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).


### <a name="73-connecting-with-an-azure-ad-token"></a>7,3 veze s token za Azure AD
Ta metoda provjere autentičnosti omogućuje srednjem sloju servisa za povezivanje s bazom podataka SQL Azure ili Azure SQL Data Warehouse unošenjem token iz Azure Active Directory (AAD). Omogućuje sofisticirane scenariji uključujući provjere autentičnosti utemeljene na certifikata. Morate poduzeti četiri Osnovni koraci za korištenje Azure AD tokena provjere autentičnosti:

1. Registracija aplikacije pomoću servisa Azure Active Directory i dobili id klijenta za kod. 
2. Stvaranje baze podataka korisnika koji predstavlja aplikaciju. (Gotov u koraku 6.)
3. Stvaranje certifikata na računalu pokrenuti klijent aplikacije.
4. Dodajte certifikat kao ključ za svoju aplikaciju.

Uzorak niza za povezivanje:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Dodatne informacije potražite u članku [Blog sigurnosti za SQL Server](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="connecting-with-sqlcmd"></a>Povezivanje s sqlcmd  
Sljedeće naredbe Poveži koristite verziju 13,1 sqlcmd, koji je dostupan putem [Centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Vidi također

[Upravljanje bazama podataka i prijave u bazi podataka Azure SQL](sql-database-manage-logins.md)

[Korisnici sadržavao baze podataka](https://msdn.microsoft.com/library/ff929071.aspx)

[Stvaranje korisnika (-SQL transakcija)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

