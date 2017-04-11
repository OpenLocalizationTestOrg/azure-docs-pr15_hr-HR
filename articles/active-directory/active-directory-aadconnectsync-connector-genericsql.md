<properties
   pageTitle="Generički SQL poveznik | Microsoft Azure"
   description="U ovom se članku objašnjava kako konfigurirati SQL poveznika web-mjesta tvrtke Microsoft generički."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-technical-reference"></a>Generički SQL poveznik Tehničke reference

U ovom se članku opisuju generički poveznik za SQL. U članku odnosi se na sljedeće proizvode:

- Microsoftov Upravitelj identiteta 2016 (MIM2016)
- Upravitelj identiteta Forefront 2010 R2 (FIM2010R2)
    -   Morate koristiti hitni popravak 4.1.3671.0 ili noviji [KB3092178](https://support.microsoft.com/kb/3092178).

Poveznik za MIM2016 i FIM2010R2, nije dostupan kao preuzeti iz [Microsoftova centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=717495).

Ovaj poveznik u akciji potražite u članku [Generički SQL poveznik korak po korak](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Pregled generički SQL poveznika

Generički SQL Connector omogućuje integraciju servisa sinkronizacije sa sustavom baze podataka koja nudi ODBC povezivanje.  

Iz perspektive više razine, sljedeće značajke su podržane u trenutnom izdanju sustava poveznik:

Značajka | Podrška
--- | ---
Izvora povezanih podataka | Poveznik podržan je sve 64-bitni ODBC upravljačke programe. Ga testirano sa sljedećim: <li>Microsoft SQL Server i SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 i 11 g</li><li>MySQL 5.x</li>
Scenariji   | <li>Upravljanje životni ciklus objekta</li><li>Upravljanje lozinkama</li>
Operacije | <li>Potpuni uvoz i izvoz Delta uvoza</li><li>Za izvoz: Dodavanje, brisanje, ažuriranja i zamijeni</li><li>Postavljanje lozinke, promjena lozinke</li>
Shema | <li>Dinamični otkrivanje objekti i atributi koje</li>

### <a name="prerequisites"></a>Preduvjeti
Prije korištenja poveznik, pripazite da imate sljedeće na poslužitelju sinkronizacije:

- 4.5.2 za Microsoft .NET Framework ili novije verzije
- 64-bitni ODBC klijent upravljačke programe

### <a name="permissions-in-connected-data-source"></a>Dozvole u izvora povezanih podataka
Da biste stvorili ili izvoditi sve podržane zadatke u generički SQL poveznika, morate imati:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Priključci i protokoli
Za priključke potrebne za ODBC upravljački program za rad, dokumentaciju prodavaču baze podataka.

## <a name="create-a-new-connector"></a>Stvaranje nove poveznika
Da biste stvorili generički SQL poveznika, u **Servis sinkronizacije** odaberite **Management Agent** i **Stvaranje**. Odaberite poveznik **generički SQL (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Povezivanje
Poveznik za povezivanje koristi datotečni ODBC DSN. Stvaranje datoteke DSN pomoću **ODBC izvori podataka** pronađen na izborniku start u odjeljku **Administrativni alati**. U alatu za administratora stvorite **Datotečni DSN** da bi se može pružati u Connector.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Prvi je zaslon povezivanje prilikom stvaranja novog generički SQL poveznik. Najprije morate unijeti sljedeće podatke:

- Put datoteke DSN
- Provjera autentičnosti
    - Korisničko ime
    - Lozinke

Baza podataka treba podržavati jednu od ovih metoda provjere autentičnosti:

- **Provjera autentičnosti sustava Windows**: provjeru autentičnosti baze podataka koristi vjerodajnice sustava Windows radi potvrde korisnika. Korisničko ime i lozinka navedena se koriste za provjeru s bazom podataka. Ovaj račun potrebne dozvole za bazu podataka.
- **SQL provjere autentičnosti**: koristi baze podataka za provjeru autentičnosti korisničko ime i lozinka definirani jedan zaslon povezivanje da biste se povezali s bazom podataka. Ako je korisničko ime i lozinka koje ste pohranili na datoteku DSN, vjerodajnice na zaslonu povezivanje imati prednost.
- **Provjera autentičnosti baze podataka SQL Azure**: dodatne informacije potražite u članku [Povezivanje sa SQL tako da pomoću Azure Active Directory provjera autentičnosti baze podataka](..\sql-database\sql-database-aad-authentication.md).

**DN je sidro**: Ako odaberete tu mogućnost, na DN koristi se kao atribut sidra. Moguće je koristiti za jednostavne implementaciju, ali ima sljedeća ograničenja:

-   Poveznik podržava vrsta samo jedan objekt. Stoga atribute referenca samo možete referencirati je ista vrsta objekta.

**Izvoz vrstu: objekt Zamijeni**: tijekom izvoza, kada ste promijenili neke atribute, cijeli objekt svih atributa izvozi se i zamjenjuje postojeći objekt.

### <a name="schema-1-detect-object-types"></a>Shema 1 (Otkrij objekt vrste)
Na ovoj stranici koju ćete konfigurirati kako će poveznik da biste pronašli vrste različitih objekata u bazi podataka.

Svaka vrsta objekta je prikazane kao particije i konfigurirali dalje **Konfiguriranje particije i hijerarhije**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Način za otkrivanje vrsta objekta**: U poveznik podržava načini za otkrivanje vrsta objekta.

- **Fiksna vrijednost**: Navedite popis vrsta objekta s popisom odvojenih zarezom. Na primjer: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Prikaz/tablica/spremljene Procedure**: Navedite naziv tablice/prikaz/pohranjene procedure, a zatim naziv stupca koji sadrži popis vrsta objekta. Ako koristite pohranjena procedura, zatim pružaju parametara za njega u obliku **[Ime]: [smjer]: [vrijednost]**. Navedite parametra u zasebnom retku (koristite kombinaciju tipki Ctrl + Enter da biste dobili novi redak).  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **SQL upit**: ta mogućnost omogućuje vam slanje SQL upit koji vraća jedan stupac s vrste objekata, na primjer `SELECT [Column Name] FROM TABLENAME`. Vraćeni stupac mora biti vrste niz (varchar).

### <a name="schema-2-detect-attribute-types"></a>Shema 2 (Otkrij atribut vrste)
Na ovoj stranici koju ćete konfigurirati kako atribut naziva i vrste ćete se provjeravati. Konfiguracija mogućnosti navedene su za svaku vrstu objekta prepoznat na prethodnoj stranici.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Način za otkrivanje vrste atributa**: U poveznik podržava ovih metoda otkrivanje za atribut vrsta s određenom vrstom otkriven objekt na zaslonu sheme 1.

- **Prikaz/tablica/spremljene Procedure**: Navedite naziv tablice/prikaz/pohranjene procedure koji želite koristiti da biste pronašli nazive atributa. Ako koristite pohranjena procedura, zatim pružaju parametara za njega u obliku **[Ime]: [smjer]: [vrijednost]**. Navedite parametra u zasebnom retku (koristite kombinaciju tipki Ctrl + Enter da biste dobili novi redak). Da biste otkrili nazive atributa u atribut s više vrijednosti, navedite popis odvojenih zarezom tablicama ili prikazima. Scenariji za polja s više nisu podržane kada nadređenog i podređenog tablice imaju isti nazivi stupaca.
- **SQL upit**: ta mogućnost omogućuje vam slanje SQL upit koji vraća jedan stupac s imenima atribut, na primjer `SELECT [Column Name] FROM TABLENAME`. Vraćeni stupac mora biti vrste niz (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Shema 3 (Definiraj sidra i DN)
Ova stranica omogućuje konfiguriranje sidra i DN atribut za svaku vrstu otkriven objekta. Možete odabrati više atribut Jedinstvenost na Sidrište.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Atributi s većim brojem vrijednosti i Booleova ne navode se.
- Isti atribut ne možete koristiti za DN i sidrenja, osim ako **DN sidrenja** je odabran na stranici povezivanje.
- Ako **DN sidro** je odabran na stranici povezivanje, ova stranica zahtijeva samo atribut DN. Taj atribut će se koristiti kao atribut sidra.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Shema 4 (definiranje Vrsta atributa, referenca i smjer)
Ova stranica omogućuje vam da konfigurirate vrstu atribut, kao što je cijeli broj, binarnu ili Booleove vrijednosti i smjer za svaki atribut. Sve atribute iz stranice **sheme 2** navedene su uključujući atribute s više vrijednosti.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Vrsta podataka**: služi za mapiranje vrsta atributa te vrste poznat modula za sinkroniziranje. Zadano je da biste koristili iste vrste kao što je otkriven u shemi SQL, ali DateTime i referenci mogu se jednostavno nalaziti neprepoznatljive. Za one, morate navesti **DateTime** ili **referenci**.
- **Smjer**: atribut smjeru možete postaviti za uvoz, izvoz ili ImportExport. ImportExport je zadanih vrijednosti.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Bilješke:

- Ako je vrsta atributa nije nalaziti neprepoznatljive po poveznik, koristit će se vrsta podatka String.
- **Ugniježđene tablice** možete smatrati baze podataka jedan stupac tablice. Oracle pohranjuje redaka ugniježđene tablice pojedinačnom redoslijedu. Međutim, prilikom učitavanja ugniježđene tablice u varijablu PL/SQL, reci ponudit će uzastopnih indeksi počevši od 1. Koji omogućuje vam pristup polja nalik pojedinačnih redaka.
- **VARRYS** nisu podržane u poveznik.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Shema 5 (Definiraj particija za atribute referenca)
Na ovoj stranici konfigurirati za sve reference atribute koje particije (Vrsta objekta) upućivanje atribut.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Ako koristite **DN je sidro**, morate koristiti iste vrste kao one koje upućuju na. Ne možete se referencirati neku drugu vrstu objekta.

### <a name="global-parameters"></a>Globalni parametara
Stranica globalni parametara koristi se za konfiguriranje Delta uvoz, oblik datuma/vremena i metodu lozinke.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

Generički poveznik SQL podržava sljedećih načina za uvoz Delta:

- **Pokretanje**: potražite u članku [Delta prikaza pomoću okidača za generiranje](https://technet.microsoft.com/library/cc708665.aspx).
- **Vodeni žig**: generički pristup koje je moguće koristiti s bilo kojeg baze podataka. Upit za vodeni žig je unaprijed unesene ovisno o prodavaču baze podataka. Vodeni žig stupac mora postojati na svaku tablicu/prikaz koji se koristi. Ovaj stupac morate pratiti umeće i ažuriranja tablice kao i njegov zavisne (više vrijednosti ili podređene) tablice. Satovi između servisa sinkronizacije i poslužitelj baze podataka mora se sinkronizirati. Ako nije, možda će ispušteni neke stavke u okviru Uvoz delta.  
Ograničenja:
    - Vodeni žig strategije ne ne podršku izbrisanih objekata.
- **Snimka**: (funkcionira samo s Microsoft SQL Server) [Generiranje Delta načina putem snimke](https://technet.microsoft.com/library/cc720640.aspx)
- **Evidentiranje promjena**: (funkcionira samo s Microsoft SQL Server) [o evidentiranja promjena](https://msdn.microsoft.com/library/bb933875.aspx)  
Ograničenja:
    - Sidrenje i DN atributa mora biti dio primarnog ključa za odabrani objekt u tablici.
    - SQL upit nije podržana tijekom uvoz i izvoz s evidentiranje promjena.

**Dodatni parametri**: odredite u bazi podataka vremenska zona poslužitelja koja upućuje na kojem se nalazi vaš poslužitelj baze podataka. Ta se vrijednost koristi za podršku različite oblike atributa datum i vrijeme.

Poveznik uvijek pohranjuje datuma i datuma i vremena u obliku UTC-a. Potrebno je navesti da biste mogli pravilno pretvaranje datuma i vremena, a zatim željenu vremensku zonu poslužitelja baze podataka i oblik koji se koristi. Oblik moraju se izraziti u obliku .net.

Tijekom izvoza atribut vremena svakog datuma mora se navesti Connector u oblik vremena UTC-a.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Konfiguriranje lozinku**: poveznik pruža mogućnosti sinkronizaciju lozinke i podržava postavljanje i promjena lozinke.

Poveznik omogućuje podržava sinkronizaciju lozinke na dva načina:

- **Pohranjena procedura**: Ova metoda zahtijeva dva pohranjene procedure za podršku postavljanje i promjena lozinke. Unesite sve parametre za dodavanje i promjena lozinke operacije u **SP lozinke za postavljanje** i **Promjena lozinke SP** parametara odnosno kao po sljedećem primjeru.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Proširenje lozinku**: ovu metodu potrebna je lozinka proširenje DLL (morate za davanje naziva DLL kućni broj koji je implementacije sučelja [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) ). Lozinka proširenje skupa moraju biti smještene u proširenje mapu tako da se poveznik možete učitati DLL prilikom izvođenja.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Morate omogućiti upravljanje lozinkama na stranici **Konfiguracija nastavak** .
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguriranje particije i hijerarhije
Na stranici particije i hijerarhije odaberite sve vrste objekata. Svaka vrsta objekta je vlastitu particija.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Možete nadjačati i vrijednosti definiran na stranici **Povezivanje** ili **Globalne parametara** .

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Konfiguriranje sidra
Ova stranica je samo za čitanje, budući da već je definiran na Sidrište. Atribut sidrenja odabranog uvijek dodaju se vrsti objekta da biste bili sigurni ostaje jedinstveni različitim vrstama objekta.

![sidra](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Konfiguriranje izvođenja korak parametra
Ove korake konfigurirane na izvođenja profila na poveznik. Ove konfiguracije učinite stvarni posao uvoza i izvoza podataka.

### <a name="full-and-delta-import"></a>Potpuna i uvoz Delta
Generički podršku za SQL poveznik cijelog i uvoz Delta pomoću ovih metoda:

- Tablica
- Prikaz
- Pohranjena procedura
- SQL upit

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabličnom prikazu**  
Da biste uvezli s većim brojem vrijednosti atribute objekta, morate unijeti naziv odvojenih zarezom/prikaz tablice u **tablicu prikazima naziv Multi-Valued** i uvjete odgovarajući spoja u **uvjet spoja** s nadređenoj tablici.

Primjer: Koju želite uvesti i svoje s većim brojem vrijednosti atribute objekta zaposlenika. Postoje dvije tablice pod nazivom zaposlenika (glavni tablica) i odjel (više vrijednosti).
Učinite sljedeće:

- Vrsta **zaposlenika** **Tablica/prikaz/SP**.
- U **tablici prikaza naziv Multi-Valued**upišite odjela.
- Upišite uvjet spoja između zaposlenika i odjela u **Uvjet spoja**, na primjer `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Pohranjene procedure**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Ako imate mnogo podataka, preporučuje se za implementaciju numeriranje stranica s spremljene procedure.
- Za postupak pohranjene podržava numeriranje stranica, morate pokrenuti indeksa i kraj indeksa. Pročitajte sljedeće: [učinkovito po stranicama kroz velike količine podataka](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexi @EndIndex zamjenjuje vrijeme izvršavanja odgovarajuća stranica veličine vrijednost konfiguriran na stranici **Korak konfiguriranje** . Na primjer, kada se poveznik dohvaća prve stranice i veličine stranice postavljen 500, u takvim situaciji @StartIndex bio 1 i @EndIndex 500. Ove vrijednosti povećali pri poveznik dohvaća podatke u sljedeće stranice te promijenite navigaciju na @StartIndex & @EndIndex vrijednost.
- Izvršavanje s parametrima pohranjene Procedure, navedite parametara u `[Name]:[Direction]:[Value]` oblik. Unos parametra u zasebnom retku (koristi Ctrl + Enter da biste dobili novi redak).
- Generički poveznik SQL podržava i operacije uvoza s povezanog poslužitelja u Microsoft SQL Server. Ako informacije moraju biti dohvaćeni iz tablice u povezani poslužitelj, tablice potrebno navesti u obliku:`[ServerName].[Database].[Schema].[TableName]`
- Generički poveznik SQL podržava samo objekte koji imaju slične strukturu (i pseudonim naziv i vrstu podataka) između pokrenuti korake informacije i sheme otkrivanje. Ako je odabrani objekt iz sheme i navedenih informacija prilikom izvođenja koraka drukčiji, zatim poveznik za SQL nije uspio podržava ovu vrstu scenarijima.

**SQL upit**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Skup više rezultata upita nije podržan.
- SQL upit podržava na numeriranje stranica i omogućuje pokretanje indeksa i indeks završetka kao varijabla podržava numeriranje stranica.

### <a name="delta-import"></a>Uvoz Delta
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta uvoz konfiguracije zahtijeva neke dodatne konfiguracije usporedbi sa potpuni uvoz.

- Ako odaberete pristup okidača ili brze snimke radi praćenja promjena delta, navedite povijest tablice ili brze snimke baze podataka u okviru **Povijest tablice ili brze snimke naziv baze podataka** .
- Morate unijeti uvjet spoja između tablice povijest i nadređenoj tablici, na primjer`Employee.ID=History.EmployeeID`
- Da biste pratili transakcije na nadređenoj tablici iz tablice povijest, navedite naziv stupca koji sadrži podatke o operacija (Dodaj/ažuriranja i brisanja).
- Ako ste odabrali vodeni žig radi praćenja promjena delta, navedite naziv stupca koji sadrži podatke o operacije u **Naziv stupca za vodu oznaka**.
- **Promjena atributa vrste** stupaca za Promijeni vrstu potreban je. Ovaj stupac karata promjena koji se pojavljuje u primarnu tablicu ili tablice s više vrijednosti s vrstom promjena u prikazu delta. Ovaj stupac može sadržavati Modify_Attribute Promjena vrste promjena razina atributa ili dodavanje, izmjena, ili brisanje Promjena vrste za vrstu promjene na razini objekta. Ako je brojem koji nije zadana vrijednost dodavanje, izmjena, ili Izbriši, a možete definirati te se vrijednosti pomoću ove mogućnosti.

### <a name="export"></a>Izvoz
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Generički SQL poveznik podrška za izvoz kao što su pomoću četiri podržanih metoda:

- Tablica
- Prikaz
- Pohranjena procedura
- SQL upit

**Tabličnom prikazu**  
Ako odaberete mogućnost tabličnom prikazu, poveznik generira odgovarajući upite za izvoz.

**Pohranjene procedure**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Ako odaberete mogućnost pohranjene Procedure, izvoz zahtijeva tri različite spremljene postupke za izvođenje operacija Umetanje/ažuriranja i brisanja.

- **Dodavanje naziva SP**: ovaj SP pokreće Ako bilo koji objekt potječe poveznik za unosa u tablici odgovarajuća.
- **Ažurirajte naziv SP**: ovaj SP pokreće Ako bilo koji objekt potječe poveznik za ažuriranje u tablici odgovarajuća.
- **Brisanje naziva SP**: ovaj SP pokreće Ako bilo koji objekt potječe poveznik za brisanje u tablici odgovarajuća.
- Atribut odabrali shema koja se koristi kao vrijednost parametra pohranjene procedure. Na primjer, `EmployeeName: INPUT: @EmployeeName` (EmployeeName odabran u shemi poveznika i poveznik zamjenjuje odgovarajuća vrijednost pritom izvoz)
- Da biste pokrenuli s parametrima pohranjena procedura, navedite parametara u `[Name]:[Direction]:[Value]` oblik. Unos parametra u zasebnom retku (koristi Ctrl + Enter da biste dobili novi redak).

**SQL upit**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Ako odaberete mogućnost SQL upita, izvoz zahtijeva tri različite upite za izvođenje operacija Umetanje/ažuriranja i brisanja.

- **Umetanje upita**: ovaj upit se pokreće Ako bilo koji objekt potječe poveznik za unosa u tablici odgovarajuća.
- **Ažuriranje upita**: ovaj upit se pokreće Ako bilo koji objekt potječe poveznik za ažuriranje u tablici odgovarajuća.
- **Brisanje upita**: ovaj upit se pokreće Ako bilo koji objekt potječe poveznik za brisanje u tablici odgovarajuća.
- Atribut odabrali shema koja se koristi kao vrijednost parametra upit, na primjer`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Otklanjanje poteškoća

-   Informacije o omogućivanju zapisivanja poveznik za otklanjanje poteškoća s potražite u članku [kako omogućiti praćenje ETW za poveznike](http://go.microsoft.com/fwlink/?LinkId=335731).
