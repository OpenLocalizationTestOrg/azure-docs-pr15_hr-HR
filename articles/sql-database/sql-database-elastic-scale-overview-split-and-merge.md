<properties 
    pageTitle="Premještanje podataka između baza podataka za skalirana iz oblaka | Microsoft Azure" 
    description="U članku se objašnjava kako rukovati shards i premještanje podataka putem samostalnih servis bazi elastic API-ji." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Premještanje podataka iz oblaka skalirana iz baze podataka

Ako ste softver kao servis za razvojne inženjere i iznenada aplikacije prolazi kroz goleme zahtjev, morate kako bi odgovarao rast. Tako da dodate više baza podataka (shards). Kako redistribuirati podatke za novu bazu podataka ne prekida integriteta podataka? Premještanje podataka iz ograničenog baza podataka na nove baze podataka pomoću **alata za podjelu cirkularnog pisma** .  

Alat za podjelu cirkularnog pisma izvodi kao servis Azure web. Administrator ili za razvojne inženjere koristi alat za premještanje shardlets (podatke iz programa shard) između različitih baza podataka (shards). Alat koristi upravljanje karte shard Održavanje baze podataka usluge metapodataka i osigurate dosljedan mapiranja.

![Pregled][1]

## <a name="download"></a>Preuzimanje
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Dokumentacija
1. [Praktični vodič alat elastic baze podataka podjelu cirkularnog pisma](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Konfiguracija sigurnosti Podijeli cirkularnog pisma](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Podjela cirkularnog pisma sigurnosna pitanja vezana uz](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Upravljanje shard karte](sql-database-elastic-scale-shard-map-management.md)
* [Migracija postojeće baze podataka da biste skala Izlaz](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Alati elastic baze podataka](sql-database-elastic-scale-introduction.md)
* [Elastic Pojmovnik Alati baze podataka](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Zašto koristiti alat za podjelu cirkularnog pisma?

**Fleksibilnost**

Aplikacije moraju rastezanje flexibly izvan ograničenja od jedne baze podataka SQL Azure baze podataka. Pomoću alata za premještanje podataka prema potrebi na nove baze podataka, ali zadržava integritet.

**Podjela radi povećanja** 

Morati povećati cjelokupni kapacitet učiniti rasla. Da biste to učinili, stvorite dodatne kapaciteta po sharding podatke i distribucija preko postupno više baza podataka dok su ispunjena kapaciteta potrebama. Ovo je značajke prime primjera značajku "Podjela". 

**Smanjivanje cirkularnih pisama**

Kapacitet mora smanjiti zbog sezonski prirode poslovanja. Alat za omogućuje skaliranja manje mjerilo jedinicama kada usporava tvrtke. Značajka 'spajanja' u podijeljeni spajanje servisu Elastic mjerilo pokriva taj zahtjev. 

**Upravljanje pristupne točke pomicanjem shardlets**

S više klijenata po bazi podataka dodijeljeni shardlets za shards može dovesti do kapaciteta grla na nekim shards. To potreban je ponovno dodjeljivanje shardlets ili premještanje zauzet shardlets u novi ili manje koriste shards. 

## <a name="concepts--key-features"></a>Koncepti i ključne značajke

**Korisnička hostira services**

Podjela cirkularnog pisma je isporučen kao kupca hostira servis. Potrebno je uvesti i hostira servis pretplate na Microsoft Azure. Paket preuzimanja s NuGet sadrži predložak konfiguracije da biste dovršili s podacima za implementaciju sustava određene. Pogledajte [Vodič za podjelu cirkularnog pisma](sql-database-elastic-scale-configure-deploy-split-and-merge.md) detalje. Jer servis pokreće se u pretplatu za Azure, možete odrediti i konfigurirati većinu sigurnost aspekata servis. Zadani predložak sadrži mogućnosti za konfiguriranje SSL, provjeru autentičnosti utemeljenu na certifikat klijenta, a zatim šifriranje pohranjenih vjerodajnica, DoS guarding i IP ograničenja. Dodatne informacije o sigurnosti aspekte možete pronaći u sljedeću dokument [konfiguracija sigurnosti Podjela cirkularnog pisma](sql-database-elastic-scale-split-merge-security-configuration.md).

Zadani uvode se pokreće servis s jednog suradnika i jednu ulogu web. Svaki koristi veličina A1 VM u Azure servise u Oblaku. Dok ne možete mijenjati postavke pri implementaciji paket, možete ih promijeniti nakon uspješne implementacije u izvodi servisa u oblaku, (putem portala za Azure). Imajte na umu da ulogu suradnika morate nije konfiguriran za više od jedne instance tehničke razloga. 

**Integracija shard kartu**

Servis za podjelu cirkularnog pisma stupi u interakciju s kartom shard aplikacije. Prilikom korištenja servisa za podjelu spajanje podijeliti ili spajanje raspona ili da biste se kretali shardlets shards, servis čuva shard karte u tijeku. Da biste to učinili, servis povezuje shard karte Upravitelj baze podataka aplikacije i održava raspona i mapiranja kao tijeku zahtjeva za spajanje/Podjela/Premjesti. Time se osigurava da karti shard uvijek prikazuje ažuran prikaz nakon podjele cirkularnog pisma operacije prelaska. Podijelite, Spoji i shardlet operacije premještanja primjenjuju pomicanjem skupine shardlets iz izvora shard shard cilj. Tijekom na shardlet premještanja operacija shardlets primjenjuju grupe za trenutni označavaju kao izvanmrežne na karti shard i nisu dostupne za usmjeravanje veza za ovisne o podacima pomoću **OpenConnectionForKey** API-JA. 

**Dosljedne shardlet veze**

Kada se pokrene premještanje podataka za Nova grupa za shardlets, sve shard kartu dobiveni podataka ovisne usmjeravanje veze shard pohrani na shardlet su drugom procesu i sljedeće veze iz mape shard API-ji ta shardlets blokiraju tijekom pomicanja podataka da biste izbjegli nedosljednosti. Veze na druge shardlets na istom shard će također se drugom procesu, ali neće uspjeti ponovno odmah na pokušaj. Kada se premješta u grupe, na shardlets označavaju online radi shard ciljne i izvorišnih podataka je uklonjena iz shard izvora. Servis prolazi kroz korake za svaku grupu dok ne prijeđete sve shardlets. To će dovesti do nekoliko operacija uklanjanje veza tijekom postupka dovrši spajanje/Podjela/Premjesti.  

**Upravljanje shardlet dostupnosti**

Ograničavanje veze killing trenutni skupine shardlets kako je opisano iznad ograničava opsega nedostupnosti skupine shardlets odjednom. To je Preferirani putem programa pristup kojem dovršeno shard će ostati izvanmrežne za njegov shardlets tijekom operacije Podijeli i cirkularnih pisama. Veličina grupe, definirana kao broj različitih shardlets za pomicanje po jedan je parametar za konfiguraciju. Može se definirati za svaku Podjela i spajanje operaciju ovisno o dostupnosti i performanse potrebama aplikacije. Imajte na umu raspona koji je u tijeku zaključan na karti shard može biti veći od definirane veličine grupe. To je zato servis izdvajanja raspon veličinu tako da stvarni broj sharding ključa vrijednosti u podacima približno odgovara veličini grupe. Ovo važno je upamtiti posebno sparsely popunjena sharding tipke. 

**Spremište metapodataka**

Podjela spajanje servisu koristi baze podataka da biste zadržali njezin status i čuvati zapisnike tijekom obrade zahtjeva. Korisnik stvara bazu podataka u svoje pretplate i njihovi niz za povezivanje za njega u konfiguracijskoj datoteci radi implementacije servisa. Administratori u tvrtki ili ustanovi za korisnikovu možete povezati s bazom podataka da biste pregledali tijeku zahtjev, a da biste istražili detaljne podatke o potencijalne pogreške.

**Sharding Informiranost**

Servis za podjelu cirkularnog pisma razlikuje (1) sharded tablica, tablica reference (2) i (3) normalno tablice. Semantiku operacije spajanje/Podjela/premještanja ovise o vrsti tablicu koja se koristi i definira na sljedeći način: 

* **Sharded tablica**: Podjela, pisma i Premjesti operacije premještanje shardlets iz izvora shard cilj. Nakon uspješan dovršetak zahtjeva za cjelokupan te shardlets postoje više izvora. Primijetite da ciljne tablice moraju postojati na ciljnom shard i ne smije sadržavati podatke u odredišni raspon prije no što obradu operacije. 

* **Tablica referenca**: referenca tablice, podjelu, spajanje i premještanje operacije kopirajte podatke iz izvora shard cilj. Međutim, imajte na umu da se bez promjene pojaviti na shard cilj za određene tablice ako je bilo koji redak već postoji u ovoj tablici na odredištu. U tablici mora biti prazna za sve operacije Kopiraj za referencu tablice da biste se obrađuju.

* **Druga tablica**: drugih tablica može biti prisutno izvorišnog web-mjesta ili cilj Podjela i spajanje operacija. Servis za podjelu cirkularnog pisma ne uzima u obzir u ovim su tablicama za sve operacije kopiranje ili premještanje podataka. Međutim, imajte na umu da možete ometaju te operacije u slučaju ograničenja.

Informacije o referenci nasuprot sharded tablica nudi **SchemaInfo** API-ji na karti shard. Sljedeći primjer ilustrira korištenje ove API-ji na navedeni shard smm kartu Upravitelj objekta: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Tablica "regija" i 'značaja' definiraju se kao referenca tablice i kopirat će se u funkcioniranju spajanje/Podjela/Premjesti. 'kupca' i 'narudžbe' shodno definiraju se kao sharded tablice. C_CUSTKEY i O_CUSTKEY služe kao tipku sharding. 

**Referencijalni integritet**

Podijeli spajanje servisu analizira ovisnosti među tablicama i koristi ključ primarnog ključa odnosi temeljeni na vanjskom da faze postupci za premještanje reference tablice i shardlets. Općenito govoreći, referenca tablice kopiraju se najprije zavisnost redoslijedom, a zatim shardlets kopiraju u redoslijedu njihove ovisnosti unutar svake grupe. To je potrebno da bi se FK PK ograničenja cilj shard su na snazi prilikom dolaska nove podatke. 

**Dosljednost shard kartu i usmjerenog dovršetka**

X. pogrešaka, servisa za podjelu spajanje primijenila operacije nakon bilo koji prekida i trebao za dovršenje u tijeku zahtjevima. Međutim, mogu postojati nepopravljiva situacijama – primjerice, kada izgubite ili ugrožena može popraviti shard cilj. Tim okolnostima neke shardlets koji su bi trebao će se premjestiti nastaviti smjestiti na shard izvor. Servis osigurava da shardlet mapiranja samo obnavljaju nakon potrebne podatke je uspješno kopiran na odredište. Shardlets samo izbrisano izvorišnog web-mjesta kad ciljnoj su kopirani svoje podatke i odgovarajuće mapiranja ažurirane uspješno. Postupak brisanja se događa u pozadini dok je raspon već online na shard cilj. Servis za podjelu cirkularnog pisma uvijek osigurava ispravnosti mapiranja pohranjena na karti shard.


## <a name="the-split-merge-user-interface"></a>Podijeli cirkularnog pisma korisničkog sučelja

Podjela spajanje servisu paket sadrži tempiranja uloge i uloge web. Uloga web koristi se za slanje zahtjeva za podjelu spajanje interaktivni način. Glavne komponente korisničkog sučelja su na sljedeći način:

-    Vrsta operacija: Operacija je vrsta izborni gumb kojim se kontrolira vrste postupak obavlja služba za taj zahtjev. Na raspolaganju podjele, spajanje i premještanje scenarijima. Da biste odustali prethodno poslane operacija. Možete koristiti podjele, spajanje i premještanje zahtjeva za raspon shard karte. Popis shard karata samo operacije Premjesti za podršku.

-    Karta shard: U sljedećem odjeljku parametara zahtjev obuhvaća informacije o shard karte i hostiranje karti shard bazu podataka. Točnije, morate Navedite naziv poslužitelja baze podataka SQL Azure i baza podataka nalaze u shardmap vjerodajnice za povezivanje shard karte baze podataka i na kraju naziv mape shard. Trenutno operacija prihvaća samo jedan skup vjerodajnica. Te vjerodajnice mora biti potrebne dozvole za izvođenje promjena kartu shard kao i kao korisničke podatke na na shards.

-    Izvorišni raspon (Podjela i spajanje): Podjela i spajanje operacija obrađuje raspon gornju i donju ključ. Da biste odredili operaciju neograničeno visoke ključa vrijednošću, potvrdite okvir "visoke ključ je maksimalni" i visoke ključa polje ostavite prazno. Raspon vrijednosti ključa koji navedete ne moraju točno odgovara mapiranje i njegov granice na karti shard. Ako uopće ne navedete sve granice raspona servis će koristiti najbliže raspon umjesto vas automatski. Skriptu GetMappings.ps1 PowerShell možete koristiti za dohvaćanje trenutne mapiranja na karti navedeni shard.

-    Ponašanje Podijeli izvora (Podijeli): operacije Podijeli definiranje točke za podjelu raspona izvorišnih. To se unosom tipku sharding gdje želite da se podijeliti prozor da se pokreće. Korištenje izborni gumb navedite želite li donjem dijelu raspona (bez tipku Podjela) da biste premjestili, ili želite li gornji dio da biste premjestili (uključujući tipku Podjela).

-    Izvorni Shardlet (Pomicanje): premještanje operacije razlikuju se od podjelu i cirkularnih pisama operacije dok ne zahtijeva raspon opisuje izvorišnog web-mjesta. Izvor za premještanje jednostavno otkrije sharding vrijednost ključa koji će se premjestiti.

-    Ciljani Shard (Podijeli): kada na izvoru postupak podjele koju ste naveli podatke, potrebno definirati gdje želite da se podaci za kopiranje unosom poslužitelj i bazu podataka naziv baze podataka SQL Azure za cilj.

-    Odredišni raspon (spajanje): operacije spajanja premještanje shardlets postojeće shard. Odredite postojeće shard unosom granice raspona postojeći raspon koji želite spojiti s.

-    Veličina grupe: Veličinu serije određuje broj shardlets koji će izvanmrežni tijekom pomicanja podataka. To je cjelobrojnu vrijednost gdje možete koristiti manje vrijednosti kad ste osjetljivi dugo razdoblja nedostupnost za shardlets. Veće vrijednosti povećava termin koji je zadani shardlet izvanmrežne ali mogu poboljšati performanse.

-    Operacija Id (Odustani): Ako imate tijeku postupak koji više nije potrebna, možete otkazati postupak unosom njegov ID operacije u ovom polju. ID operacije možete dohvatiti iz tablica status zahtjev (pogledajte odjeljak 8.1) ili iz izlaza u web-pregledniku na mjesto na koje ste poslali zahtjev.


## <a name="requirements-and-limitations"></a>Preduvjeti i ograničenja 

Trenutni implementaciji servisa za podjelu spajanje podložni sljedeće preduvjete i ograničenja: 

* Na shards morati postoji i registrirana na karti shard prije nego što se može izvršiti operaciju Podijeli spajanja na te shards. 

* Servis nije moguće stvoriti tablice ili drugih objekata baze podataka automatski kao dio operacije. To znači da sheme za sve sharded tablice i referenci tablice moraju postojati na ciljnom shard prije no što sve operacije spajanje/Podjela/Premjesti. Sharded tablica posebno moraju biti prazna u rasponu gdje su nove shardlets potrebno dodati spajanje/Podjela/Premjesti postupak. U suprotnom, postupak neće uspjeti Provjera dosljednosti početne shard cilj. Imajte na umu i referenci tog podataka kopira se samo ako je referenca prazna tablica i ima li bez jamstva dosljednost s obzirom na druge Istodobni pisanje operacije u referenci tablice. Preporučujemo da to: kada radi podjele/spajanja operacije, bez operacije pisanja promjene u referenci tablice.

* Servis ovisi o retka identiteta postavio jedinstveni indeks ili ključ koji obuhvaća ključ sharding da biste poboljšali performanse i pouzdanost za velike shardlets. Time se omogućuje servisa da biste premjestili sadržaj na čak i precizniji preciznosti od samo sharding vrijednost ključa. Pomaže da biste smanjili maksimalno povećali prostor zapisnika i zaključavanja koji su potrebni tijekom postupka. Razmislite o stvaranju jedinstveni indeks ili primarni ključ uključujući tipku sharding danoj tablici ako želite koristiti tu tablicu s zahtjevima za spajanje/Podjela/Premjesti. Radi boljih performansi tipku sharding mora biti početne stupca ključ ili indeks.

* Tijekom obrade zahtjeva, neki podaci shardlet možda izlaganje i na izvorišnog web-mjesta i shard cilj. To je potrebno da biste zaštitili pogreške tijekom pomicanja shardlet. Integracija Podijeli cirkularnog pisma s kartom shard osigurava veza do podataka o njima ovisne usmjeravanje API-ji metodom **OpenConnectionForKey** na karti shard vidjeti sve nedosljedno Srednja stanja. Međutim, prilikom povezivanja s izvorišnog web-mjesta ili shards cilj bez korištenja metode **OpenConnectionForKey** , koje nisu usklađene Srednja stanja možda vidljive kad zahtjeva za spajanje/Podjela/Premjesti prelaska. Ove veze može prikazivati djelomično ili dvostruke rezultate ovisno o tome tempiranja ili shard podlozi vezu. To ograničenje trenutno sadrži veze načinio Elastic mjerilo višestruka-Shard-upiti.

* Baza podataka metapodataka servisa za podjelu spajanja moraju biti zajedničke između različite uloge. Ako, na primjer, uloge servisa za podjelu spajanje izvodi u pripremna treba pokažite na različitim metapodataka baze podataka od uloga radnog.
 

## <a name="billing"></a>Naplata 

Servis za podjelu cirkularnog pisma izvodi kao neki servis u oblaku u pretplatu na Microsoft Azure. Stoga za servise u oblaku naplaćuje vašoj instanci servisa. Ako često izvođenje operacije spajanja/Podjela/Premjesti, preporučujemo da izbrišete na servis u oblaku za podjelu cirkularnog pisma. Koji se sprema troškove za pokretanje ili implementiran instanci servisa u oblaku. Možete ponovno uvesti i počnite konfiguraciju lako runnable koje po potrebi izvođenje operacija Podjela i cirkularnih pisama. 
 
## <a name="monitoring"></a>Nadzor 
### <a name="status-tables"></a>Status tablice 

Servis za podjelu cirkularnog pisma nudi **RequestStatus** tablice u metapodacima spremanje baze podataka za praćenje dovršenih i trajnih zahtjeva. U tablici nalaze retku za svaki zahtjev za podjelu cirkularnog pisma koja je poslana u ovoj instanci servisa podjelu cirkularnog pisma. Pruža sljedeće informacije za svaki zahtjev:

* **Vremenska oznaka**: vremena i datuma kada je pokrenuta zahtjev.

* **OperationId**: GUID koji služi kao jedinstvena identifikacija zahtjev. Ovaj zahtjev može se koristiti i za odustati od operacije dok je i dalje tijeku.

* **Status**: trenutnom stanju zahtjev. Tijeku zahtjeva za navodi i Trenutna faza u kojem je zahtjev.

* **CancelRequest**: zastavice koja označava je li zahtjev je otkazana.

* **Tijek**: članak Procjena uvjeta postotak dovršenog za operaciju. Vrijednost od 50 označava da je postupak približno 50% dovršeno.

* **Detalji**: XML vrijednost koja sadrži detaljnije tijeku izvješća. Izvješća tijeka povremeno ažurira se kao skupa redaka se kopiraju iz izvora cilj. U slučaju pogreške ni iznimke ovaj stupac sadrži detaljne informacije o pogrešci.


### <a name="azure-diagnostics"></a>Azure dijagnostiku

Podjela spajanje servisu koristi Azure Dijagnostika za nadzor i dijagnostici na temelju Azure SDK 2,5. Konfiguracija Dijagnostika kontrolu kao što je opisano u nastavku: [Omogućivanje Dijagnostika Azure servise u Oblaku i virtualnim strojevima](../cloud-services/cloud-services-dotnet-diagnostics.md). Preuzmite paket sadrži dvije Dijagnostika konfiguracija – jedan za ulogu web i jedan za ulogu suradnika. Ove konfiguracije Dijagnostika za servis slijedite navedene upute s [Osnovama o Oblaku servisa u Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Obuhvaća definicije da biste se prijavili mjerača performansi, IIS zapisnike, zapisnika događaja sustava Windows i zapisnike događaja aplikacije Podijeli cirkularnog pisma. 

## <a name="deploy-diagnostics"></a>Implementacija dijagnostiku 

Da biste omogućili nadzor i dijagnostici pomoću dijagnostičkih konfiguracije za web- a tempiranja uloge koje ste dobili od NuGet paketa, pokrenite sljedeće naredbe pomoću komponente PowerShell Azure: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Dodatne informacije o tome kako konfigurirati i implementacija postavki dijagnostike Ovdje možete pronaći: [Omogućivanje Dijagnostika Azure servise u Oblaku i virtualnim računalima](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Dohvaćanje dijagnostiku 

Možete jednostavno pristupiti vašem Dijagnostika iz Visual Studio poslužitelj programa Explorer Azure dio stabla Explorer poslužitelja. Otvaranje instance Visual Studio, a na traci izbornika kliknite prikaz, a Explorer poslužitelja. Kliknite ikonu Azure povezati Azure pretplatu. Zatim pronađite Azure -> Spremanje -> <your storage account> -> WADLogsTable -> tablica. Dodatne informacije potražite u članku [Pregledavanje resursa za pohranu pomoću programa Explorer poslužitelja](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

WADLogsTable istaknuta na slici iznad sadrži detaljne događaje iz zapisnika aplikacije servisa za podjelu spajanje. Imajte na umu da zadanu konfiguraciju preuzete paketa je usmjerena prema radnog implementacije. Stoga interval na kojoj se zapisnike i mjerača koji se povlače iz instanci servisa je velike (5 minuta). Testiranje i razvoj, smanjite interval prilagodbom postavki dijagnostike weba ili ulogu suradnika vašim potrebama. Desnom tipkom miša na ulozi u Visual Studio poslužitelja Exploreru (vidi iznad) i prilagodite razdoblje prijenos u dijaloški okvir za konfiguraciju postavki dijagnostike: 

![Konfiguracija][3]


## <a name="performance"></a>Performanse

Općenito govoreći, bolje performanse je očekivati od na višu više performant servisa razine u bazi podataka SQL Azure. Veća IO, procesora i memorije alokacija za veće razine servisa prednosti Kopiraj skupno i izbrišite operacije koje koristi servis za podjelu cirkularnog pisma. Zbog toga, povećajte sloju servisa samo za te baze podataka za definirani, ograničeni vremensko razdoblje.

Servis izvodi i provjere valjanosti upita kao dio normalni operacije. Tih provjere valjanosti upita provjerite neočekivane prisutnosti podataka u rasponu cilj i bili sigurni da se sve operacije spajanje/Podjela/Premjesti počinje s ujednačeno stanje. Ove sve upite radi nad sharding ključa raspona koji definira opsega postupak i veličinu serije navedene kao dio definiciju zahtjev. Te upite najbolje izvođenje kada je indeksa prezentacije koja sadrži ključ sharding kao stupac početne. 

Svojstvo jedinstvenosti s ključem sharding kao stupac početne će omogućiti i servis optimizirana pristup koji ograničava potrošnje resursa prostor zapisnika i memorije. Ovo svojstvo jedinstvenosti potreban je da biste premjestili veličine velike podataka (obično veće od 1GB). 

## <a name="how-to-upgrade"></a>Upute za nadogradnju

1. Slijedite korake u [uvođenje servisa Podjela cirkularnog pisma](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Promijenite oblaka servisa konfiguracijska datoteka za implementaciju sustava Podijeli cirkularnog pisma u skladu s vizualnim nove konfiguracijske parametre. Novi obavezan parametar je informacije o potvrdi koja se koristi za šifriranje. Jednostavan je način za to je da biste usporedili novi konfiguracije datoteku predloška iz preuzimanje protiv postojeću konfiguraciju. Provjerite je li dodati postavke za "DataEncryptionPrimaryCertificateThumbprint" i "DataEncryptionPrimary" na webu i ulogu suradnika.
3. Prije implementacije ažuriranja za Azure, provjerite je li dovršite sve operacije trenutno izvode Podijeli cirkularnog pisma. Možete jednostavno to činite RequestStatus i PendingWorkflows tablica u bazi podataka Podijeli spajanja metapodataka tijeku zahtjeva za upite.
4. Ažurirajte postojeće uvođenje servisa oblaka za podjelu cirkularnog pisma u pretplatu za Azure s novog paketa i ažurirane servisa konfiguracijskoj datoteci.

Dodjela resursa za novu bazu podataka metapodataka za podjelu cirkularnog pisma da biste nadogradili nije potrebno. Novu verziju automatski nadograditi postojeće metapodataka baze podataka u novu verziju. 

## <a name="best-practices--troubleshooting"></a>Najbolje prakse i otklanjanje poteškoća
 
-    Definiranje test klijenta i Nekorištenje najvažnije spajanje/Podjela/Premjesti operacije s klijentom za testiranje preko nekoliko shards. Provjerite je li sve metapodatke ispravno definirani u karti shard i da operacije kršiti ograničenja ili vanjski ključevi.
-    Zadrži klijentu test probleme vezane uz veličina podataka iznad Maksimalna veličina datoteke od najvećeg klijentu da bi vam se pojavili veličina podataka. Pomaže pri procjeni gornja granica na vrijeme potrebno da biste premještali jednog klijenta. 
-    Provjerite dopušta li sheme brisanja. Servis za podjelu cirkularnog pisma potrebna mogućnost da biste uklonili podatke iz izvora shard kada je uspješno kopirani na odredište. Na primjer, **Brisanje okidača** možete spriječiti da servis za brisanje podataka na izvoru i može uzrokovati operacije uvoza.
-    Ključ sharding mora biti početne stupca u primarni ključ ili definicija jedinstveni indeks. Osigurati najbolje performanse za provjeru valjanosti upite Podijeli i cirkularnih pisama i stvarnih podataka operacija premještanje i brisanje koje uvijek rade na sharding ključa raspona.
-    Collocate usluge Podjela cirkularnog pisma u centru regija i podataka gdje se nalaze baze podataka. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 
