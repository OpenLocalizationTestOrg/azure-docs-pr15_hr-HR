<properties 
    pageTitle="Baza podataka DocumentDB pitanja – najčešća pitanja o | Microsoft Azure" 
    description="Saznajte odgovore na najčešća pitanja o Azure DocumentDB NoSQL dokumenta servisa baze podataka za JSON. Odgovaranje na pitanja baze podataka o kapacitetu, razine performanse i skaliranja." 
    keywords="Pitanja iz baze podataka, najčešća pitanja, documentdb, azure, Microsoft azure"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Najčešća pitanja o DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Baza podataka pitanja o osnovama sustava Microsoft Azure DocumentDB

### <a name="what-is-microsoft-azure-documentdb"></a>Što je Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB je na blazing brzo, planeta skaliranje NoSQL dokument baze podataka-kao-u – servis koji nudi obogaćenog ispitivanje iznad sheme slobodno podataka, pomaže izlaganje konfigurirati i pouzdane performanse i omogućuje brz razvoj all through upravljanih platformu sigurnosno tako da na potenciju i dosegne sustava Microsoft Azure. DocumentDB je pravo rješenje za mobilne, igraće web-mjesto i IoT aplikacije kad su predvidljivi propusnost, visoke dostupnosti, niske latencije i sheme slobodno podatkovnog modela tipki preduvjetima. DocumentDB nudi fleksibilnost sheme i obogaćeni indeksiranje putem nativni JSON podatkovnog modela, a obuhvaća više dokumenata transakcijskih podršku s Integriranom JavaScript.  
  
Baza podataka pitanja, odgovora i upute o implementaciji i pomoću ove usluge potražite u članku [DocumentDB dokumentaciju stranice](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Kakvu vrstu baze podataka je DocumentDB?
DocumentDB je NoSQL dokument usmjeren baze podataka koji se sprema podatke u JSON OSNOVNI oblik.  DocumentDB podržava ugniježđene, samostalno-contained podataka strukture koje možete mu kroz obogaćenog DocumentDB [gramatike SQL upita](documentdb-sql-query.md). DocumentDB nudi visoke performanse transakcijskih obrada strani poslužitelja JavaScript kroz [pohranjene procedure, okidača, i korisnički definirane funkcije](documentdb-programming.md). Baza podataka podržava i za razvojne inženjere tunable dosljednost razine pridružene [razine performansi](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>Moram li baza podataka DocumentDB tablice kao što su relacijske baze podataka (RDBMS)?
Ne, DocumentDB sprema podatke u zbirke dokumenata JSON.  Informacije o resursima DocumentDB potražite u članku [DocumentDB resursa modela i koncepata](documentdb-resources.md). Dodatne informacije o čemu NoSQL rješenja kao što su DocumentDB razlikuju relacijski rješenja potražite u članku [Dodavanje veze vanjskih NoSQL SQL](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>Baza podataka DocumentDB podržavaju sheme slobodno podataka?
Da, DocumentDB aplikacijama omogućuje pohranu proizvoljne JSON dokumenata bez definicija sheme ili savjete. Podaci su odmah dostupan za upit putem sučelja DocumentDB SQL upita.   

### <a name="does-documentdb-support-acid-transactions"></a>Podržava li DocumentDB ACID transakcije?
Da, DocumentDB podržava više dokumenata transakcije izražen kao JavaScript pohranjene procedure i okidača. Transakcije su iz djelokruga jedna particija unutar svake zbirke i koja se s ACID semantiku kao za sve ili ništa Izolirani iz drugih istovremeno izvršni kod i korisnik zahtjeva.  Ako su iznimke izbačena kroz izvršavanje strani poslužitelja JavaScript kod aplikacije, cijeli transakcije je vraćen. Dodatne informacije o transakcije potražite u članku [program transakcije baze podataka](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Što su slučajevi Tipična Upotreba za DocumentDB?  
DocumentDB je dobar izbor za nove igraće mobilnim, web-mjesto i važno je aplikacija IoT gdje na koji se Automatska promjena veličine predvidljivi performanse brzo redoslijedom od milisekundi odgovor puta i mogućnost upita iznad sheme slobodno podataka. DocumentDB lends sam brz razvoj i neprekinuti iteracije modela podataka aplikacije za podršku. Programi koji upravljanje korisnika koji su generirani sadržaj i podaci su [uobičajeni slučajevi koristi za DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Kako DocumentDB nudi predvidljivi performansi?
[Jedinica zahtjev](documentdb-request-units.md) je mjera širine propusnost u DocumentDB. 1 Pravi odgovara propusnost obavljanje 1KB dokumenta. Svaki operacije u DocumentDB uključujući čitanja, pisanja, SQL upite i izvršavanja pohranjena procedura sadrži deterministic Pravi vrijednosti na temelju propusnost potrebne da biste dovršili postupak. Umjesto misle o procesora, IO i memorije i na koji način svaki korisnik utječe na propusnost aplikacije koje možete razmišljati jednu mjeru Pravi.

Svakoj zbirci DocumentDB može biti rezervirana s dodijeljenu propusnost pomoću RUs propusnost sekundi. Za aplikacije sve mjerilo, možete benchmark pojedinačne zahtjeve za mjerenje njihove Pravi vrijednosti i zbirki dodjele resursa za rukovanje ukupni zbroj jedinice zahtjev za sve zahtjeve. Možete proširenja ili skaliranje dolje propusnost vaše zbirke kao potrebama vaše evolve aplikacije. Dodatne informacije o zahtjevu jedinice i pomoć za određivanje zbirci web mora, pročitajte [Upravljanje performanse i kapaciteta](documentdb-manage.md) i pokušajte [Kalkulator propusnost](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Je li DocumentDB HIPAA usklađena?
Da, DocumentDB je HIPAA usklađen. HIPAA uspostavlja preduvjeti za korištenje otkrivanja, i zaštiti podataka pojedinačno njenu stanja. Dodatne informacije potražite u članku [Microsoftova centra za pouzdanost](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Što su ograničenja prostora za pohranu od DocumentDB? 
Nema ograničenja theoretical na željeni ukupni broj podataka koje zbirka možete spremiti u DocumentDB. Ako želite spremiti više od 250 GB podataka u jednu zbirku, imajte se [obratiti službi za podršku](documentdb-increase-limits.md) da biste da bi se povećati kvota za račun. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Što su ograničenja propusnost DocumentDB? 
Nema ograničenja theoretical za ukupni iznos propusnost koja podržava zbirka u DocumentDB, ako svoje radno opterećenje mogu distribuirati otprilike ravnomjerno među potpuno velik broj particije tipke. Ako želite biti dulji od 250.000 zahtjev jedinice/sekundi po zbirke ili računa Imajte se [obratiti službi za podršku](documentdb-increase-limits.md) da biste da bi se povećati kvota za račun. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Koliko stoji Microsoft Azure DocumentDB?
Pogledajte na stranici [informacije o cijenama DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/) detalje. Naknade za korištenje DocumentDB ovise o broju zbirki koristi, broj sati zbirke bili povezani s Internetom, kao i potrošenih prostora za pohranu i dodijeljenu propusnost za svaku zbirku. 

### <a name="is-there-a-free-account-available"></a>Je li besplatnog računa dostupne?
Ako ste novi korisnik Azure, možete se prijaviti za neki drugi [račun za Azure besplatne](https://azure.microsoft.com/free/)koja vam daje 30 dana i $200 pokušati Azure services. Ili, ako imate pretplatu na Visual Studio, ispunjavate uvjete za [$150 u besplatne Azure kredita mjesečno](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) za korištenje na bilo koji servis za Azure.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Kako dobiti dodatnu pomoć s DocumentDB?
Ako vam je potrebna pomoć za sve stupili nam na [Hrpu prelijevanje](http://stackoverflow.com/questions/tagged/azure-documentdb), na [Forume za razvojne inženjere za Azure DocumentDB MSDN](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)ili zakazivanje [1:1 razgovor s tim DocumentDB](http://www.askdocdb.com/). Da biste ostali u tijeku s najnovijim DocumentDB novosti i značajke, slijedite nam na [Twitteru](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Postavljanje Microsoft Azure DocumentDB

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Kako registracije za Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB je dostupan [Portal za Azure][azure-portal].  Najprije morate se prijaviti za pretplatu na Microsoft Azure.  Kada se registrirate za pretplatu na Microsoft Azure, DocumentDB računa možete dodati u pretplatu za Azure. Upute o dodavanju DocumentDB računa potražite u članku [Stvaranje računa DocumentDB baze podataka](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Što je matrica ključ?
Glavni ključ je sigurnosni token pristup svim resursima za račun. Osobe s ključem ste čitanje i pisanje pristup svim resursima u račun za baze podataka. Budite oprezni prilikom raspodjele tipke matrice. Primarni ključ matrice i sekundarne glavni ključ dostupne su u plohu **tipke ** [Azure Portal][azure-portal]. Dodatne informacije o ključevima potražite u članku [Prikaz, Kopiraj i regenerate pristupnih tipki](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Kako stvoriti bazu podataka?
Možete stvoriti baze podataka pomoću [Portala za Azure]() , kao što je opisano u [Stvori DocumentDB bazu podataka](documentdb-create-database.md), jednu od [DocumentDB SDK-ovi](documentdb-sdk-dotnet.md)ili putem [REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx).  

### <a name="what-is-a-collection"></a>Što je zbirka?
Zbirka je spremnik JSON dokumenata i pridruženi logike aplikacije JavaScript. Zbirka je naplatu entitet, gdje je [trošak](documentdb-performance-levels.md) određen je propusnost i storaged koristi. Zbirke mogu obuhvaćati jedan ili više particija/poslužitelja, a mogu mijenjati veličinu učiniti gotovo neograničeno količine prostora za pohranu ili propusnost.

Zbirke i su entiteti naplate za DocumentDB. Svaku zbirku naplate zaračunava na temelju dodijeljenu propusnost i koristiti prostora za pohranu. Dodatne informacije potražite u članku [DocumentDB cijene](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Kako postaviti korisnici i dozvole?
Možete stvoriti korisnici i dozvole pomoću jednog od [DocumentDB SDK-ovi](documentdb-sdk-dotnet.md) ili putem [REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx).   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Baza podataka pitanja o razvoju protiv Microsoft Azure DocumentDB

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Kako učiniti pokretanja razvoj protiv DocumentDB?
[SDK-ovi](documentdb-sdk-dotnet.md) dostupni su za .NET, Python, Node.js, jezika JavaScript i Java.  Razvojnim inženjerima omogućuje korištenje i [RESTful HTTP API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx) za interakciju s resursima DocumentDB iz različitih platforme i jezika. 

Uzorci za DocumentDB [.NET](documentdb-dotnet-samples.md), [jezika Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)i SDK-ovi [Python](documentdb-python-samples.md) dostupne su na GitHub.

### <a name="does-documentdb-support-sql"></a>Podržava li DocumentDB SQL?
Jezik DocumentDB SQL upita je poboljšana podskup funkcije upita podržava SQL. Jezik DocumentDB SQL upita nudi obogaćeni hijerarhijski i relacijske operatore i proširivanje putem JavaScript temelji korisnika definirane funkcije (UDF-ove). Omogućuje JSON gramatike za Modeliranje JSON dokumente kao stabala s natpisima kao čvorove stabla koji se koristi i tehnike automatsko indeksiranje DocumentDB kao i na SQL upita dijalektu od DocumentDB.  Detalje o korištenju provjere gramatike SQL provjerite potražite u člancima [Upita DocumentDB] [ query] članka.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Što su podataka koje podržava DocumentDB?
Na jednostavne su vrste podataka podržane u DocumentDB jednaki JSON. JSON ima jednostavna vrsta sustav koji se sastoji od nizovi, brojevi (IEEE754 dvostruke preciznosti) i Booleove vrijednosti - true, false ili null.  Složenije vrste podataka kao što su datum i vrijeme, Guid, Int64 i geometriju moguće predstaviti u JSON i DocumentDB kroz stvaranje ugniježđene objekata pomoću operatora {} i polja pomoću operatora []. 

### <a name="how-does-documentdb-provide-concurrency"></a>Kako DocumentDB pruža istodobnosti?
DocumentDB podržava optimistična Procjena istodobnosti kontrola (OCC) putem HTTP entitet oznake ili etags. Svaki resurs DocumentDB ima programa e-oznake, a postavljena na poslužitelju u e-oznake svakog ažuriranja dokumenta. Zaglavlja e-oznake i trenutnu vrijednost nalaze se u svim porukama odgovor. Da biste omogućili poslužitelja odlučite li se ažurirati resursa mogu se Etags sa zaglavljem If-Match. Ako rezultat vrijednost je jedna vrijednost za usporedbu. Ako jedna vrijednost udovoljava jedna vrijednost poslužitelj, ažurirat će se resurs. Ako trenutno nije više na e-oznake, poslužitelj odbacuje postupak s programa "HTTP 412 Preduvjet pogreška" kod odgovor. Klijent pa ćete morati refetch resursa za trenutnu vrijednost e-oznake za resurs. Osim toga, etags mogu se s uklonjenim zaglavljem If-ništa – Match određivanje ako je ponovno dohvatite resursa nije potrebna. 

Da biste koristili optimistična Procjena istodobnosti .NET, koristite [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) predmete. Uzorak .NET potražite u članku [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) u uzorku DocumentManagement na github.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Kako izvesti transakcije u DocumentDB?
DocumentDB podržava jezik integriran transakcije putem JavaScript pohranjene procedure i okidača. Sve operacije baze podataka unutar skripte se izvode u odjeljku snimke odvajanja iz djelokruga zbirke ako je zbirka jednom ili dokumenata s istom vrijednošću ključa particija unutar zbirke, ako je particije zbirke. Snimak verzije dokumenata (ETags) je izvedena na početku transakcije i izvršenom samo ako se potvrdi skriptu. Ako JavaScript throws pogrešku, transakcija je vraćen. Potražite u članku [DocumentDB poslužiteljsko programiranje](documentdb-programming.md) više pojedinosti.

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Kako masovno Umetanje dokumenata u DocumentDB? 
Da biste skupno Umetanje dokumenata u DocumentDB na tri načina:

- Podataka alata za migraciju, kao što je opisano u [Uvoz podataka u DocumentDB](documentdb-import-data.md).
- Dokument Explorer na portalu Azure, kao što je opisano u [masovno dodavanje dokumenata pomoću programa Explorer dokumenta](documentdb-view-json-document-explorer.md#BulkAdd).
- Pohranjene procedure, kao što je opisano u [DocumentDB poslužiteljsko programiranje](documentdb-programming.md).

### <a name="does-documentdb-support-resource-link-caching"></a>DocumentDB podržava predmemoriranje vezu resursa?
Da, budući da je DocumentDB RESTful servisa, resursa veze su immutable i mogu predmemorirane. Klijenti DocumentDB možete odrediti zaglavlje "If-ništa – Match" za čitanje s bilo kojeg resursa kao što je dokument ili prikupljanje i ažuriranje svoje lokalno kopira samo kada je promjena verziju na poslužitelju. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Dostupna je na lokalnu instancu programa DocumentDB?
Trenutno na lokalnu instancu programa DocumentDB nije dostupna. Možete pratiti stanje lokalne emulator i glasovanja ga na [forum za povratne informacije](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance).


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
