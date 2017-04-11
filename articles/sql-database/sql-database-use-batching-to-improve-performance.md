 <properties
    pageTitle="Kako poboljšati performanse aplikacije baze podataka SQL Azure pomoću grupnog slanja promjena"
    description="U temi dokazuje te batching postupaka baze podataka znatno imroves brzine i skalabilnost aplikacija za baze podataka SQL Azure. Iako ove tehnike batching raditi za sve baze podataka SQL Server, fokus u članku nalazi se na Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Kako poboljšati performanse aplikacije SQL baze podataka pomoću grupnog slanja promjena

Značajno grupnog slanja promjena operacije s bazom podataka SQL Azure poboljšava performanse i skalabilnost aplikacija. Da biste shvatili prednosti, prvi dio u ovom se članku pokriva rezultate testiranja neke ogledne kojima se uspoređuje uzastopne i odbacivanja zahtjevi za SQL baze podataka. Ostatak u članku prikazuje tehnike, scenariji i pitanja vezana uz da biste lakše koristiti grupno uspješno slanje promjena u Azure aplikacija.

**Autori**: Jason Roth, Silvano Coriani, Trent Swanson (Potpuna skaliranje 180 Inc)

**Pregledavatelji**: Conor Cunningham, Goran Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Zašto je grupnog slanja promjena važne za SQL baze podataka?
Grupno slanje promjena pozive na servis za daljinski je dobro poznatog Strategije za povećanje performanse i skalabilnost. Postoji fiksni obrada troškove sve interakcije s udaljenog servisa, kao što su serijalizacije, prijenos mreže i deserijalizacija. Pakiranje koliko je transakcija zasebnom u jednom skupinu minimizira te troškove.

U ovom dokumentu želimo pregledajte različite baze podataka SQL grupnog slanja promjena strategije i scenarijima. Iako te strategije važna su i za lokalne aplikacije koje koriste SQL Server, postoji nekoliko razloga za isticanje korištenje grupnog slanja promjena za SQL baze podataka:

- Je potencijalno veći latenciju mreže pristup SQL baze podataka, osobito ako pristupate SQL bazu podataka s izvan isti podatkovnog centra za Microsoft Azure.
- Osobina složene baze podataka SQL znači da učinkovitosti na podataka programa access sloja odgovara cjelokupan skalabilnost baze podataka. Baze podataka SQL morate onemogućuju sve jednog klijenta/korisnika da monopoliziranjem baze podataka resursa za detriment drugim korisnicima. Kao odgovor na korištenje više od unaprijed definiranih kvote SQL baze podataka možete smanjiti propusnost ili odgovor na poruke uz ograničavanje iznimke. Učinkovitosti, kao što su grupnog slanja promjena, omogućuju dodatne rad na baze podataka SQL prije da ta ograničenja. 
- Grupno slanje promjena stupa i za arhitekturi koje koriste više baza podataka (sharding). Učinkovitosti vaše interakcije s svaku jedinicu baze podataka i dalje je faktor ključa u vašem cjelokupan skalabilnost. 

Jedna od prednosti korištenja baze podataka SQL je ne morate upravljati poslužitelja koji hostira baze podataka. Međutim, infrastruktura za upravljane i znači da morate na umu drugačije optimizacije baze podataka. Više ne može izgledati da biste poboljšali infrastrukture hardver ili mreže baze podataka. Microsoft Azure kontrole te okruženja. Područje glavnog koje možete kontrolirati je interakciju aplikacije s bazom podataka SQL. Grupno slanje promjena jedan je od ovih optimizacije. 

Prvi dio papira ispituje različite tehnike batching za .NET aplikacije koje koriste SQL baze podataka. Zadnje dvije sekcije obuhvaća upute i scenariji grupnog slanja promjena.

## <a name="batching-strategies"></a>Grupno slanje promjena strategije

### <a name="note-about-timing-results-in-this-topic"></a>Napomena o tempiranja rezultate u ovoj temi
>[AZURE.NOTE] Rezultati nisu jednonitnih, ali su pokazivati **relativni performansi**. Tempiranja temelje se na prosječnu vrijednost od najmanje 10 test se pokreće. Operacije se umeće u praznu tablicu. Ove testira su mjerni pre V12, a oni ne moraju nužno biti odgovaraju propusnost koje koje se mogu pojaviti u bazi podataka V12 pomoću nove [razine servisa](sql-database-service-tiers.md). Relativna prednost batching postupak mora biti sličan.

### <a name="transactions"></a>Transakcije
Čini se da biste započeli pregled grupnog slanja promjena tako da razgovarate o transakcije neobično. Ali koristi klijentsko transakcije Neupadljiva poslužiteljsko batching efekt koji poboljšavaju performanse. I transakcije možete dodati sa samo nekoliko redaka koda, pa pružaju brz način da biste poboljšali performanse uzastopne operacija.

Razmislite o sljedećim C# kod koji sadrži niz umetanje i ažurirati postupci za jednostavnu tablicu.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Sljedeći kod ADO.NET sekvencijalno izvodi te operacije.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Najbolji način da biste optimizirali kod je implementirati neki oblik klijentsko grupnog slanja promjena ove poziva. No postoji jednostavan način poboljšati performanse kod pomoću jednostavno prelamanja slijed pozive u transakcije. Evo istu šifru koja koristi transakcije.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

U ovim se primjerima zapravo koriste transakcije. U prvom primjeru svaki pojedinačne poziv je implicitna transakcija. U drugi se primjer izričite transakcije prelama sve pozivi. Po potražite u dokumentaciji za [pisanje za dovršena prije zapisnik transakcija](https://msdn.microsoft.com/library/ms186259.aspx), zapisi zapisnika su isprazniti na disk, kada se obvezuje transakcije. Pa uključivanjem pozive u transakcije zapisivanja u zapisnik transakcija može usporiti dok nastoji transakcije. Na snazi su Omogućivanje grupnog slanja promjena za zapisivanja u zapisnik transakcija na poslužitelj.

Sljedeća tablica prikazuje rezultate testiranja neke ad-hoc. Testova izvršena isti uzastopnih umeće sa ili bez transakcije. Za više perspektive prvi skup testira pokrenuli daljinski iz prijenosno računalo s bazom podataka u Microsoft Azure. Drugi skup testira pokrenuli iz servis u oblaku i baze podataka i resided unutar iste Microsoft Azure Standard (Zapad SAD-a). Sljedeća tablica prikazuje trajanje u milisekundama uzastopnih umeće sa ili bez transakcije.

**Lokalni za Azure**:

| Operacije | Nema transakcije (ms) | Transakcije (ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1000 | 128852 | 102917 |


**Azure za Azure (isti podatkovnog centra)**:

| Operacije | Nema transakcije (ms) | Transakcije (ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1000 | 21479 | 2756 |

>[AZURE.NOTE] Rezultati nisu jednonitnih. Pogledajte [napomenu o tempiranja rezultate u ovoj temi](#note-about-timing-results-in-this-topic).

Na temelju prethodne rezultate testiranja zapravo prelamanje jedne operacije u transakcije smanjuje performanse. Ali kako povećavate broj operacija unutar jedna transakcija, poboljšanje performansi postaje više označena kao. Razlika performanse je vidljivijih kada sve operacije pojavljuju se u podatkovnim centrom za Microsoft Azure. Povećana Latencija korištenja baze podataka SQL iz izvan podatkovnog centra sustava Microsoft Azure overshadows rast performanse korištenja transakcije.

Iako pomoću transakcije možete poboljšati performanse, i dalje [pridržavajte najbolje prakse za transakcije i veze](https://msdn.microsoft.com/library/ms187484.aspx). Zadrži transakcije što je moguće kraći i zatvorite vezu s bazom podataka nakon dovršetka posla. Na pomoću izjave u prethodnom primjeru pokazuje veza zatvoren kada se dovrši blok sljedeći kod.

U prethodnom primjeru pokazuje koje možete dodati lokalna transakcija kod ADO.NET s dva retka. Transakcije nude brz način da biste poboljšali performanse kod koji vam se čini uzastopnih Umetanje, ažuriranje i brisanje operacije. Međutim, najbrže performanse, razmislite o promjeni kod se bi iskoristila prednosti grupnog slanja promjena klijentsko, kao što su parametri s tabličnim vrijednostima.

Dodatne informacije o transakcija u ADO.NET potražite u članku [Lokalne transakcija u ADO.NET](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Parametri s tabličnim vrijednostima
S tabličnim vrijednostima parametara podržava korisnički definirane tablice vrste kao parametar u Transact-SQL naredbe, pohranjene procedure i funkcije. Ovu tehniku batching klijentsko omogućuje vam slanje više redaka podataka unutar parametra s tabličnim vrijednostima. Da biste koristili parametara s tabličnim vrijednostima, definirajte tablice. Sljedeća naredba Transact-SQL stvara tablice pod nazivom **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

U kodu, stvorite **tablica podataka** s točno istog naziva i vrste vrstu tablice. Prenesite **tablica podataka** parametra u upit teksta ili pohranjene procedure poziv. Sljedeći primjer prikazuje ovaj postupak:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

U prethodnom primjeru objekt **SqlCommand** umeće retke iz parametra s tabličnim vrijednostima **@TestTvp**. Taj parametar dodijeljena prethodno stvorena objekt **tablica podataka** pomoću metode **SqlCommand.Parameters.Add** . Putem uzastopne umeće znatno grupnog slanja promjena Umeće jedan poziv u poboljšavaju performanse.

Da biste poboljšali Dodatno u prethodnom primjeru, koristite pohranjena procedura umjesto naredbe temeljenom na tekstu. Sljedeća naredba Transact-SQL stvara pohranjena procedura koja vas vodi parametar **SimpleTestTableType** s tabličnim vrijednostima.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Zatim promijenite deklariranje objekt **SqlCommand** u prethodnom primjeru kod ovako.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

U većini slučajeva, s tabličnim vrijednostima parametre imati jednak ili bolje performanse od drugih tehnika batching. S tabličnim vrijednostima Parametri se preporučuje konjunktiv, često jer su fleksibilnije od druge mogućnosti. Na primjer, drugih tehnika, kao što je SQL skupno kopiju, samo dopušta umetanja novih redaka. No s s tabličnim vrijednostima parametra možete koristiti logike u pohranjena procedura da bi se utvrdilo koji su reci ažuriranja i koji su umeće. Vrsta tabličnih moguće je izmijeniti tako da sadrže stupca "Operacija" koji označava hoće li navedeni redak treba biti umetnuti, ažurirati ili izbrisati.

Sljedeća tablica prikazuje rezultate testiranja ad-hoc za korištenje s tabličnim vrijednostima parametara u milisekundama.

| Operacije | Lokalni za Azure (ms)  | Azure isti podatkovnog centra (ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Rezultati nisu jednonitnih. Pogledajte [napomenu o tempiranja rezultate u ovoj temi](#note-about-timing-results-in-this-topic).

Rast performanse iz grupnog slanja promjena se odmah vidljivu. U prethodno uzastopnih testu 1000 operacije trajalo 129 sekundi izvan s podatkovnim centrom i 21 sekunde iz s podatkovnim centrom. No s s tabličnim vrijednostima parametra 1000 operacije poduzeti samo 2.6 sekundi izvan s podatkovnim centrom i 0,4 sekundi unutar s podatkovnim centrom.

Dodatne informacije o parametrima s tabličnim vrijednostima potražite u članku [Table-Valued parametara](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>Kopiraj skupno SQL
SQL skupno kopija je drugi način da biste umetnuli velikih količina podataka u bazu podataka za cilj. .NET aplikacije mogu koristiti klase **SqlBulkCopy** za izvođenje operacija masovno Umetanje. **SqlBulkCopy** slično je u funkciji alat naredbenog retka, **Bcp.exe**ili Transact-SQL naredbe, **UMETNITE SKUPNO**. Sljedeći primjer koda prikazuje kako skupno Kopiraj retke u izvoru **tablica podataka**, tablicu, odredišnu tablicu u sustavu SQL Server, MojaTablica.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Postoje nekim slučajevima gdje Kopiraj masovno je Preferirani putem parametara s tabličnim vrijednostima. Pogledajte tablicu s tabličnim vrijednostima parametara nasuprot MASOVNO Umetanje operacije u temi [Table-Valued parametre](https://msdn.microsoft.com/library/bb510489.aspx).

Sljedeće rezultate testiranja ad-hoc Prikaži performanse grupnog slanja promjena s **SqlBulkCopy** u milisekundama.

| Operacije | Lokalni za Azure (ms)  | Azure isti podatkovnog centra (ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Rezultati nisu jednonitnih. Pogledajte [napomenu o tempiranja rezultate u ovoj temi](#note-about-timing-results-in-this-topic).

U manje veličine grupe za korištenje s tabličnim vrijednostima parametara outperformed **SqlBulkCopy** predmete. Međutim, **SqlBulkCopy** izvodi brže nego s tabličnim vrijednostima parametara % 12-31 za testova 1000 i 10 000 redaka. Kao što je s tabličnim vrijednostima parametara **SqlBulkCopy** je dobar izbor za odbacivanja umeće osobito kada u usporedbi s performanse koje nisu odbacivanja operacija.

Dodatne informacije o skupno kopiju ADO.NET potražite u članku [Masovne operacije Kopiraj u sustavu SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Izvješća s parametrima Umetanje više redaka
Jedan druga za manjim grupama je za sastavljanje velike s parametrima naredbi INSERT koji umeće više redaka. Sljedeći primjer koda pokazuje ovu tehniku.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

U ovom se primjeru pokazivati osnovni pojam. Realističniji scenarij bi prolazi kroz potrebne entiteti za sastavljanje nizu upita i naredba parametre istodobno. Ograničeni ste ukupnom zbroju parametara 2100 upit tako da na taj način ukupan broj redaka koji mogu biti obrađeni na taj način.

Sljedeće rezultate testiranja ad-hoc prikazivanje performansi te vrste naredbe za umetanje u milisekundama.

| Operacije | Parametri s tabličnim vrijednostima (ms) | Umetanje jedne izjava (ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Rezultati nisu jednonitnih. Pogledajte [napomenu o tempiranja rezultate u ovoj temi](#note-about-timing-results-in-this-topic).

Taj se način mogu malo brže za skupina koje su manje od 100 redaka. Iako je u unaprjeđivanja small, ta je tehnika neku drugu mogućnost koje će možda funkcionirati i u određenoj aplikaciji scenariju.

### <a name="dataadapter"></a>DataAdapter
Klase **DataAdapter** omogućuje izmjena **DataSet** objekt, a zatim poslati promjene kao što je UMETNUTI, ažuriranje i brisanje operacije. Ako koristite **DataAdapter** na taj način, važno je obratiti pažnju na zasebnom pozive vrše za svaku distinct operaciju. Da biste poboljšali performanse, koristite svojstvo **UpdateBatchSize** broj operacije koje treba batched odjednom. Dodatne informacije potražite u članku [Izvođenje grupe operacija pomoću DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entitet framework
Entitet Framework trenutno ne podržava grupnog slanja promjena. Različite inženjerima u zajednici ste pokušali demonstrirati zaobilazna rješenja, kao što su nadjačavanje metodu **SaveChanges** . No rješenja obično su složene i prilagođene aplikacije i podatkovnog modela. Project codeplex entitet Framework trenutno se stranici za raspravu na zahtjev za značajku. Da biste pogledali ovaj rasprave, potražite u članku [Bilješke sa sastanka dizajn – 2. kolovoz 2012.](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Za dovršenosti, ne možemo čini vam je važan objasniti XML kao batching strategije. Međutim, korištenje XML ima bez prednosti u odnosu na druge načine i nekoliko nedostatke. Pristup je slična parametara s tabličnim vrijednostima, ali XML datoteku ili niz se prenosi pohranjena procedura umjesto korisnički definirane tablice. Pohranjena procedura raščlanjuje naredbe u pohranjena procedura.

Postoji nekoliko nedostatke tog pristupa:

- Rad s XML može biti nezgodno i pogreškama podložni.
- Raščlanjivanje XML baze podataka može biti procesora ćete morati usko.
- U većini slučajeva, ta je metoda manja od vrijednosti parametara.

Korištenje XML za obradu upita nije preporučeno zbog sljedećih razloga.

## <a name="batching-considerations"></a>Grupno slanje promjena pitanja vezana uz
Sljedeći odjeljci sadrže dodatne upute za korištenje grupnog slanja promjena u SQL baze podataka aplikacije.

### <a name="tradeoffs"></a>Tradeoffs
Ovisno o vašem arhitektura grupnog slanja promjena može obuhvaćati tradeoff između performanse i otpornost. Na primjer, razmotrite scenarij u kojem vaša uloga neočekivano funkcionira. Ako izgubite jedan redak podataka, utjecaja je manja od utjecaj gubitka velike grupe unsubmitted redaka. Postoji veći rizik kada međuspremnika retke prije no što pošaljete baze podataka u prozoru određenog vremena.

Zbog ovaj tradeoff rezultirati vrstu operacije te koje grupe. Obrada agresivnije (veće serije i dulje vrijeme windows) s podacima koji je manji od ključne važnosti.

### <a name="batch-size"></a>Veličina grupe
U našem testova došlo je obično ne prednost prekidanje velikim grupama u manjim blokova. Zapravo, često pododjeljaka rezultirala sporije performanse od jedne velike grupe za slanje. Ako, na primjer, razmislite o scenariju mjesto na koje želite umetnuti retke 1000. Sljedeća tablica prikazuje vremena potrebnog za korištenje s tabličnim vrijednostima parametara za umetanje redaka 1000 kada je podijeljen u manjim grupama.

| Veličina grupe | Iteracija | Parametri s tabličnim vrijednostima (ms) |
| -------- | --- | --- |
| 1000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Rezultati nisu jednonitnih. Pogledajte [napomenu o tempiranja rezultate u ovoj temi](#note-about-timing-results-in-this-topic).

Vidjet ćete da je najbolje za retke 1000 da ih pošalju odjednom. U drugi testira (koji nisu ovdje prikazani) pojavio rast small performanse da biste prekinuli seriji 10000 retka u dva serijama od 5000. Dok je shema tablice te testira relativno jednostavan tako provoditi testova na konkretne podatke i veličine grupe da biste provjerili te findings.

Drugi faktor treba uzeti u obzir je ako grupe za Ukupno postane prevelika, baze podataka SQL možda throttle a odbiti za primjenu skupine. Da biste postigli najbolje rezultate, testirajte određene scenariju da biste odredili postoji li se veličina idealne grupe. Provjerite veličinu serije konfigurirati prilikom izvođenja da biste omogućili brzo prilagodbe koje se temelji na performanse ili pogreške.

Na kraju, saldo veličinu serije s rizika koji se odnose s grupnog slanja promjena. Ako postoje pogreške tranzitne ili uloga ne uspije, razmislite o posljedice ponovni postupka ili gubitak podataka u grupi.

### <a name="parallel-processing"></a>Paralelni obrada
Što ako snimljene pristup postupka smanjivanja veličine obradu ali koristiti više niti za izvršavanje posla? Ponovno naš testira prikazivao da nekoliko manje s usporednim nitima serija obično se izvoditi lošiji od jedne grupe za veće. Sljedeće probno pokušava Umetanje 1000 redaka u jednu ili više paralelnih skupinama. Ovaj test prikazuje kako više istodobno grupama zapravo smanjiti performanse.

| Veličinu serije [iteracija] | Dva niti (ms) | Četiri niti (ms) | Šest niti (ms) |
| -------- | --- | --- | --- |
| 1000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Rezultati nisu jednonitnih. Pogledajte [napomenu o tempiranja rezultate u ovoj temi](#note-about-timing-results-in-this-topic).

Postoji nekoliko mogućih razloga potencijalne za smanjene performanse performanse zbog parallelism:

- Nema više pozive istodobno mreže umjesto jednog.
- Operacije više od jedne tablice može rezultirati Nadmetanje i blokiranje.
- Postoje folije pridružene multithreading.
- Trošak od otvaranja više veza outweighs prednost paralelno obrada.

Ako se usmjeriti različitih tablica ili baze podataka, moguće je da biste vidjeli neke performanse rast s ovom strategije. Sharding baze podataka ili federations bi scenarij za taj se način. Sharding koristi više baza podataka i usmjerava druge podatke za svaku bazu podataka. Ako svaku small seriju će se u drugu bazu podataka, zatim izvođenje operacije paralelno može biti učinkovitiji. Međutim, rast performansama nije dovoljno značajan koristiti kao osnovu za odluku da biste koristili sharding baze podataka u rješenje.

U nekim dizajna paralelno izvođenja manjim grupama može rezultirati poboljšane propusnost zahtjeva za u sustavu opterećenju. U ovom slučaju, čak i ako se brže da obradi jedne grupe za veće, obradu višestruke serije paralelno možda učinkovitiji.

Ako koristite paralelne izvođenja, razmislite o kontroliranje maksimalni broj radnih niti. Manji broj može rezultirati manje Nadmetanje i brže vrijeme izvođenja. Uzmite u obzir dodatne opterećenje koje to smješta na ciljnom baze podataka u web-mjesto veze i u okvir za transakcije.

### <a name="related-performance-factors"></a>Čimbenici povezane performansi
Uobičajeni smjernice o performansama bazu podataka utječe i grupnog slanja promjena. Na primjer, umetnuti performanse smanjena je za tablice koje imaju velike primarni ključ ili mnogo nonclustered indeksi.

Ako s tabličnim vrijednostima parametara koristite pohranjena procedura, koristite naredbu **SET NOCOUNT ON** na početku postupak. Izjavu o zaštiti ukida povratna broj problematične redaka u postupku. Međutim, u našem testira iskorištavanju **SET NOCOUNT ON** imao utjecaja ili smanjiti performanse. Testiranje pohranjene procedure je jednostavno s na jednom **Umetanje** naredbe iz parametra s tabličnim vrijednostima. Moguće je složenije pohranjene procedure bi im izjavu o zaštiti. No ne pretpostavlja da automatsko dodavanje **SET NOCOUNT ON** pohranjena procedura poboljšavaju performanse. Da biste shvatili efekt, testirajte pohranjena procedura sa ili bez izjava **SET NOCOUNT ON** .

## <a name="batching-scenarios"></a>Grupno slanje promjena scenariji
U sljedećim odjeljcima opisuju kako koristiti s tabličnim vrijednostima parametara u tri aplikacije scenarija. Prvi scenarij prikazuje kako međuspremnik i grupnog slanja promjena možete surađivati. Drugi scenarij poboljšavaju performanse izvođenjem operacije matrice detalja u jednom pohranjena procedura poziv. Konačni scenarij pokazuje kako treba koristiti s tabličnim vrijednostima parametara u operaciji "UPSERT".

### <a name="buffering"></a>Međuspremnik
Iako je neke scenarije koji su očite prijedlog za Grupno slanje promjena, postoji mnogo scenarija koji nije iskoristite prednost grupnog slanja promjena po odgođeno obrada. Međutim, odgođeno obrada prenosi i veće rizika podatke nestaje slučaju pojavila se neočekivana pogreška. Važno je da taj rizik i njihovo razmislite o rezultate.

Na primjer, razmotrite web-aplikacije koja prati posjećenim svakog korisnika. Na svaki zahtjev za stranicu aplikacija nije provjerite baze podataka poziva da biste snimili prikaz stranice korisnika. No veći performanse i skalabilnost se može postići međuspremnik aktivnosti korisnika navigacije i slanjem tih podataka u bazu podataka u grupama. Možete pokrenuti ažuriranje baze podataka po Proteklog vremena i/ili Veličina međuspremnika. Na primjer, pravilo ne može odrediti skupine treba biti obrađen nakon 20 sekundi, odnosno kad međuspremnik dođe do 1000 stavki.

Sljedeći primjer koda koristi [Ponovno aktiviranje proširenja - Rx](https://msdn.microsoft.com/data/gg577609) radi obrade u međuspremniku događaja koje upućuju nadzora predmete. Prilikom međuspremnik unosi ili predloška zbirka dostigne vremensko ograničenje, skupine korisničkih podataka šalju se u bazu podataka s parametrom s tabličnim vrijednostima.

Sljedeće klase NavHistoryData modeli navigacije pojedinosti korisnika. Sadrži osnovne informacije, kao što je identifikator korisnika, pristupiti URL-a i vrijeme pristupa.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

Klase NavHistoryDataMonitor je zadužen za međuspremnik korisničkih podataka navigacije u bazu podataka. Sadrži metodu RecordUserNavigationEntry koji odgovara tako da prilikom otvaranja okvira podešavanje događaja **OnAdded** . Sljedeći kod prikazuje logiku Graditelj koji koristi Rx da biste stvorili observable zbirke koji se temelji na događaj. Ga zatim pretplaćuje se na zbirku observable metodom međuspremnik. Na preopterećenja određuje da međuspremnik moraju se poslati svakih 20 sekundi ili 1000 stavke.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

Rukovatelj pretvara sve buffered stavke u vrstu s tabličnim vrijednostima, a zatim prosljeđuje te vrste pohranjena procedura koja obrađuje grupu. Sljedeći kod prikazuje potpuni definicije na NavHistoryDataEventArgs i klase NavHistoryDataMonitor.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Da biste koristili međuspremanjem klase, aplikacija stvara statički NavHistoryDataMonitor objekt. Svaki put kada korisnik može pristupiti na stranicu aplikacija poziva metodu NavHistoryDataMonitor.RecordUserNavigationEntry. Međuspremanjem logike nastavlja se do voditi brigu o slanju te stavke u bazu podataka u grupama.

### <a name="master-detail"></a>Osnovne pojedinosti
S tabličnim vrijednostima parametara korisne su za jednostavne scenariji za umetanje. Međutim, možda ćete dodatne izazov umeće grupe koje uključuju više od jedne tablice. Scenarij "matrica/detalji" dobar je primjer. Glavna tablica koja služi za identifikaciju primarni entitet. Jednu ili više tablica detalja pohranjujete dodatne podatke o entitet. U ovom scenariju odnosi temeljeni na vanjskom ključa nametnuti odnos pojedinosti jedinstveni osnovne entitet. Razmislite o pojednostavljeni verziju PurchaseOrder tablice i njegov povezanu tablicu OrderDetail. Sljedeće Transact-SQL stvara tablicu PurchaseOrder s četiri stupca: IDNarudžbe, DatumNarudžbe, IdKlijenta i Status.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Svakom nalogu sadrži kupnje proizvoda. Ti podaci se hvata u tablici PurchaseOrderDetail. Sljedeće Transact-SQL PurchaseOrderDetail tablice stvara se pet stupaca: OrderDetailID IDNarudžbe, IDProizvoda, JediničnaCijena i OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

IDNarudžbe stupca u tablici PurchaseOrderDetail moraju referencirati reda iz tablice PurchaseOrder. Sljedeću definiciju vanjski ključ nameće ovo ograničenje.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Da biste mogli koristiti s tabličnim vrijednostima parametara, morate imati jednu vrstu korisnički definirane tablice za svaku tablicu cilj.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Zatim definirajte pohranjena procedura koja prihvaća tablice ove vrste. Ovaj postupak omogućuje aplikaciju lokalno skupna skup narudžbe i Detalji narudžbe u jednom poziv. Sljedeće Transact-SQL pruža deklariranje dovršeno pohranjena procedura u ovom primjeru narudžbe za kupnju.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

U ovom primjeru lokalno definirani @IdentityLink pohranjuje stvarnih vrijednosti IDNarudžbe iz novoumetnuti redaka. Ove identifikatora redoslijed razlikuju se od privremene IDNarudžbe vrijednosti u na @orders i @details parametara s tabličnim vrijednostima. Zbog toga u @IdentityLink tablice zatim se povezuje IDNarudžbe vrijednosti iz na @orders parametar realni IDNarudžbe vrijednosti za nove retke u tablici PurchaseOrder. Nakon ovaj korak u @IdentityLink tablice možete olakšati Umetanje Detalji narudžbe s stvarni IDNarudžbe koji zadovoljava vanjskog ključa ograničenje.

U ovom pohranjena procedura može se koristiti s kod ili druge pozive Transact-SQL. U odjeljku s tabličnim vrijednostima parametara ovaj papira za primjer koda. Sljedeće Transact-SQL pokazuje kako se poziva na sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Ovo je rješenje omogućuje svake grupe da biste koristili skup IDNarudžbe vrijednosti koje započinju od 1. Ove privremene vrijednosti IDNarudžbe opisuju odnosi u grupu, ali stvarnih vrijednosti IDNarudžbe ovise o trenutku operacija insert. Možete pokrenuti istih naredbi u prethodnom primjeru više puta i generiranje jedinstvenih narudžbe u bazi podataka. Zbog toga, razmislite o dodavanju više logiku kod ili u bazi podataka koja se onemogućuje duplicirane narudžbe kada se koristi to grupnog slanja promjena postupak.

U ovom se primjeru pokazuje čak i složenije postupaka baze podataka, kao što je matrica detalji operacije može biti batched pomoću parametara s tabličnim vrijednostima.

### <a name="upsert"></a>UPSERT
Drugi scenarij batching uključuje istodobno ažuriranje postojećih redaka i Umetanje novih redaka. Ovaj postupak je ponekad se naziva operacije "UPSERT" (ažuriranje + insert). Umjesto upućivanje poziva zasebnom možete UMETATI i AŽURIRATI, naredba SPOJI je najprikladniji za taj zadatak. Iskaz SPAJANJA možete provesti odabrane mogućnosti Umetni i ažuriranje postupke u jednom poziv.

Da biste izvršili ažuriranja i umeće mogu se s tabličnim vrijednostima parametara s naredbu SPOJI. Na primjer, razmotrite pojednostavljeni zaposlenika tablicu koja sadrži sljedeće stupce: IDZaposlenika, ime, prezime, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
U ovom primjeru možete koristiti činjenica da je u SocialSecurityNumber jedinstveni za spajanje više zaposlenika. Prvo, stvorite korisnički definirane tablice:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Nakon toga stvorite pohranjena procedura ili napišite kod koji se koristi iskaz PISMA da biste izvršili obnovu i umetnuli. Sljedeći primjer koristi naredbu SPOJI na parametar s tabličnim vrijednostima @employees, vrste EmployeeTableType. Sadržaj u @employees tablice nisu ovdje prikazani.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Dodatne informacije potražite u dokumentaciji i primjeri za naredbu SPOJI. Iako isti radni nije moguće izvesti s više koraka pohranjene poziv procedure s odvojite Umetanje, a operacija ažuriranja, naredbu SPOJI je učinkovitiji. Kod Database također možete sastaviti Transact-SQL poziva koje koriste naredbu SPOJI izravno bez dva pozive baze podataka za umetanje i ažuriranja.

## <a name="recommendation-summary"></a>Preporuka sažetka

Sljedeći popis sadrži sažetak batching preporuke koji se spominju u ovoj temi:

- Da biste povećali performanse i skalabilnost programa SQL baze podataka pomoću međuspremnik i grupnog slanja promjena.
- Razumijevanje tradeoffs između grupnog slanja promjena/međuspremnik i otpornost. Tijekom neuspješne uloga rizik od gubitka neprovedenih skupine tvrtke ključnih podataka možda su performanse prednost grupnog slanja promjena.
- Pokušajte da biste zadržali sve pozive u bazu podataka iz jednog podatkovnog centra da biste smanjili Latencija.
- Ako odaberete jednu batching postupak, s tabličnim vrijednostima parametara nude najbolje performanse i fleksibilnost.
- Najbrži Umetanje performanse, slijedite ove opće smjernice, ali testiranje scenariju:
    - Da biste postigli < 100 redaka, koristite jedan s parametrima naredbe UMETNI.
    - Za retke < 1000 pomoću parametara s tabličnim vrijednostima.
    - Za > = 1000 retke, koristite SqlBulkCopy.
- Za ažuriranje i brisanje operacije, s tabličnim vrijednostima parametre koristiti s pohranjena procedura koji određuje točan postupak za svaki redak u parametru tablice.
- Smjernice za obradu veličina:
    - Koristite najveću veličine grupe za aplikaciju i preduvjeti za tvrtke.
    - Saldo rast performanse velikih skupina s rizicima privremena ili do teškog oštećenja pogrešaka. Što je consequence ponovne pokušaje ili gubitak podataka u seriji? 
    - Testirajte najveću veličinu grupe da biste potvrdili da je SQL baze podataka ne Odbaci.
    - Stvaranje konfiguracijske postavke te kontrole grupnog slanja promjena, kao što su veličina grupe ili međuspremanjem termin. Postavke omogućuju fleksibilnost. Možete promijeniti batching ponašanje u radnog bez redeploying servisa u oblaku.
- Izbjegavajte paralelno izvođenja skupina koje rade na jednu tablicu u jednoj bazi podataka. Ako odaberete da biste podijelili jedne grupe preko više radnih niti, pokrenite testira da biste odredili idealna broj niti. Nakon neodređenim prag veći broj niti će smanjiti performanse umjesto da bi ga povećala.
- Razmislite o međuspremnik na veličinu i vrijeme kao način implementacije grupnog slanja promjena dodatne scenarije.

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku filtriran na kako dizajna baze podataka i kodiranje tehnike vezane uz grupnog slanja promjena možete poboljšati performanse aplikacije i skalabilnost. No to je samo jedan faktor u strategije cjelokupan. Više načina da biste poboljšali performanse i skalabilnost potražite u članku [baze podataka SQL Azure performanse smjernice za jednu baze podataka](sql-database-performance-guidance.md) i [cijena i performanse zahtjevi za grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-guidance.md).
