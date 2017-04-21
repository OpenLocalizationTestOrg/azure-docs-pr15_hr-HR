#<a name="data-management-and-business-analytics"></a>Upravljanje podacima i Business Analytics

Upravljanje i analiza podataka u oblaku samo kao Važno je kao što je bilo gdje drugdje. Kako biste to učinili, Azure nudi raspon tehnologije za rad s relacijskim i koje nisu relacijskih podataka. U ovom se članku predstavlja svake mogućnosti. 

##<a name="table-of-contents"></a>Tablice sadržaja      

- [Spremište blobova platforme](#blob)
- [Pokretanje programa DBMS u virtualnog računala](#dbinvm)
- [SQL baze podataka](#sqldb)
    - [Sinkronizacija podataka za SQL](#datasync)
    - [SQL podataka izvješća pomoću virtualnim strojevima](#datarpt)
- [Spremište tablica](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>Spremište blobova platforme

Riječ "blob" je skraćeno "Binary Large OBject" te točno opisuje što blob nije: zbirka binarne podatke. Još čak i ako takvi stupci jednostavni, blob-ova prilično korisne su. [Slika 1](#Fig1) ilustrira osnove spremište blobova platforme Azure.

<a name="Fig1"></a>![Dijagram blob-ova][blobs]
 
**Slika 1: Blobova platforme Azure sprema binarne podatke blob - polja – spremnika.**

Da biste koristili blob-ova, najprije stvorite Azure *račun za pohranu*. Kao dio ovoga, navedite Azure podatkovnog centra koja će sadržavati objekte koji stvarate pomoću ovog računa. Gdje se nalazi, svaki blob stvorite pripada neke spremnik na vašem računu za pohranu. Da biste pristupili blob, aplikacije sadrži URL-a u programu Lync:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*spremnik*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; je jedinstveni identifikator dodjeljuje prilikom stvaranja novog računa za pohranu, dok &lt; *spremnik* &gt; i &lt; *BlobName* &gt; imena određene spremnik i blob unutar njih. 

Azure sadrži dvije različite vrste blob-ova. Na raspolaganju su:

- *Blokiranje* blob-ova, od kojih svaki može sadržavati do 200 gigabajta podataka. Kao što je predloži njegov naziv bloka blob subdivided će se u neki broj blokova. Ako dođe do pogreške pri prijenosu blob blok, ponovno slanje možete nastaviti s najnovije blok umjesto ponovno slanje cijelu blob-om. Blokiranje blob-ova prilično Općenito pristup za pohranu, te se najčešće korištenih blob vrsta danas.

- *Stranica* blob-ova, koja može biti u jednom terabajta. Blob-Ova stranica namijenjena izravnim pristupom i tako da svaki od njih je podijeljen u neki broj stranica. Aplikacija je besplatna za čitanje i pisanje pojedinačne stranice nasumično u blob-om. U Azure virtualnih računala, na primjer, VMs stvorite upotrijebili blob-Ova stranica stalnih prostor za pohranu za OS diskova i diskova podataka.

Hoćete li odabrati blok blob-ova ili blob-Ova stranica, aplikacija može pristupiti podacima blob na nekoliko različitih načina. Mogućnosti obuhvaćaju sljedeće:

- Izravno putem programa RESTful (odnosno utemeljenu na HTTP) protokol za pristup. Azure aplikacije i vanjske aplikacije, uključujući aplikacije koji se izvodi lokalno, možete koristiti tu mogućnost.
- Pomoću biblioteke Azure prostora za pohranu klijenta, koji sadrži više za razvojne inženjere prikladnu sučelje pri vrhu neobrađenog RESTful blob protokol za pristup. I opet Azure aplikacije i vanjskim aplikacijama možete pristupiti pomoću biblioteke blob-ova.
- Korištenje Azure pogona, mogućnost koja omogućuje programa Azure Smatraj blob stranice na lokalni disk s s datotečnim sustavom NTFS. U aplikaciji blob stranice izgleda u običnom datotečnog sustava Windows pristupiti pomoću standardnih datoteka/i. Zapravo, čita i zapisuje šalju se u podlozi blob stranice koji implementira pogon Azure. 

Radi zaštite od hardverske pogreške te poboljšanja dostupnost, svaki blob je replicirati na tri računala u Azure podatkovnog centra. Pisanje na blob ažurira sve tri kopije tako da se kasnije čitanja neće se prikazivati rezultati koje nisu usklađene. Možete odrediti koje podatke s blob trebaju biti kopirane drugi Azure podatkovnog centra u istoj zemlj, ali najmanje 500 milja odmah. U ovom kopiranja, pod nazivom *zemlj replikacije*dogodi u roku od nekoliko minuta ažuriranje da biste blob-om, a zatim je korisno za oporavak Izrada.

Podaci u BLOB polja možete također biti dostupne putem Azure *Sadržaja isporuke mreže (CDN)*. Po predmemoriranje kopije podataka blob na desetke poslužitelje diljem svijeta na CDN možete ubrzati pristup informacijama u kojoj se pristupa više puta. 

Jednostavno kao što su blob-ova su odabir u većini slučajeva. Spremanje i strujanje audiozapisa i videozapisa su očite Primjeri, kao što su sigurnosne kopije i druge vrste podataka arhiviranje. Razvojni inženjeri možete koristiti blob-ova za bilo koju vrstu nestrukturirane podatke žele. Imate jednostavne omogućuje pohranu i dohvaćanje binarne podatke može biti surprisingly korisno.


## <a name="dbinvm"></a>Pokretanje programa DBMS u virtualnog računala

Mnoge aplikacije danas je za neke vrste sustava za upravljanje bazom podataka (DBMS). Relacijski sustave, primjerice, SQL Server su najčešće korištene izbora, ali nije relacijski postupke, obično naziva tehnologije *NoSQL* se češće svaki dan. Da biste omogućili oblaka aplikacije pomoću ove mogućnosti za upravljanje podacima, virtualnim računalima sustava Azure omogućuje vam pokretanje programa DBMS (relacijski ili NoSQL) u na VM. [Slika 2](#Fig2) prikazuje kako to izgleda sa sustavom SQL Server.

<a name="Fig2"></a>![Dijagram sustava SQL Server u virtualnog računala][SQLSvr-vm]
 
**Slika 2: Azure virtualnim strojevima omogućuje raditi s DBMS u VM, s postojanost nudi blob-ova.**

Razvojni inženjeri i administratore baze podataka, scenarij izgleda slično radi isti softver u vlastite podatkovnog centra. U ovom primjeru, ako, primjerice, gotovo sve mogućnosti sustava SQL Server može se koristiti, a imate potpuni Administrativni pristup sustavu. Imate odgovornost za upravljanje poslužitelja baze podataka, Naravno, baš kao da su izvodi lokalno.

Dok se prikazuje se [Slika 2](#Fig2) , baze podataka pojavljuju se biti spremljen na lokalnom disku VM poslužitelj pokreće se u. U odjeljku naslovnice, no svaki od tih diskova zapisuje na blobova platforme Azure. (To je slično korištenju na SAN u vlastiti podatkovnog centra s blob djeluje slično kao što je LUN.) Kao što je s bilo kojeg blobova platforme Azure, podatke u ćeliji je replicirati triput unutar s podatkovnim centrom i, ako je replicirati zemlj drugi podatkovnim centrom na istom području zahtjev. Također je moguće koristiti mogućnosti kao što su baze podataka SQL Server zrcaljenje za veća pouzdanost. 

Drugi način za korištenje sustava SQL Server u na VM je radi stvaranja hibridnog aplikacije, gdje podataka nalazi se na Azure tijekom logike aplikacije lokalnog. Ako, na primjer, to Provjerite odgovara kada aplikacija instalirana na više mjesta ili na raznim mobilnim uređajima morate zajednički koristiti iste podatke. Da biste komunikaciju između oblaka baze podataka i lokalne logičku jednostavniji tvrtki ili ustanovi možete koristiti Azure virtualne mreže da biste stvorili vezu virtualne privatne mreže (VPN-a) između Azure podatkovnog centra i vlastitu lokalnog podatkovnog centra.


## <a name="sqldb"></a>SQL baze podataka

Za veliki broj korisnika, radi s DBMS u na VM je prva mogućnost s kojom se sjetite za upravljanje strukturiranih podataka u oblaku. To je ne samo odabir kroz, niti ga uvijek je najbolji odabir. U nekim slučajevima, upravljanje podacima pomoću platforma kao usluge (PaaS) pristup više smisla. Azure sadrži tehnologiju PaaS pod nazivom SQL baze podataka koje možete učiniti za relacijskih podataka. [Slika 3](#Fig3) ilustrira tu mogućnost. 

<a name="Fig3"></a>![Dijagram baze podataka SQL][SQL-db]
 
**Slika 3: SQL baze podataka omogućuje zajednički servis PaaS relacijski prostora za pohranu.**

SQL baze podataka ne daju svaki klijent fizičke vlastito sustava SQL Server. Umjesto toga, u njoj nalaze servisa više klijenta s poslužiteljem baze podataka SQL logičke za svakog korisnika. Svi korisnici zajednički koristiti kapaciteta računalnim i prostora za pohranu koji omogućuje servis. I s spremište blobova platforme sve podatke u bazi podataka SQL pohranjuju se na tri zasebna računala u Azure Standard održavanja baze podataka ugrađene visoke dostupnosti (HA).

U aplikaciju baze podataka SQL izgleda slično kao što je SQL Server. Aplikacije možete problema SQL upite odabiranja relacijski tablice, koristite T-SQL pohranjene procedure i izvršavanje transakcije preko više tablica. I jer aplikacijama pristup bazi podataka SQL pomoću protokola tablične podatke strujanje (TDS), isti protokol koji se koristi za pristup sustava SQL Server, možete raditi s podacima pomoću Framework entitet, ADO.NET, JDBC i druge sučelja poznatih podataka programa access. 

No Budući da baze podataka SQL neki servis u oblaku radi u centre za Azure podataka, ne morate upravljati bilo koji od fizičke aspekata sustava, kao što su korištenje diska. Ne morate brinuti o ažuriranju softver ili obrade ostalih najniže razine administrativnih zadataka. Svaki kupca tvrtke ili ustanove i dalje upravlja vlastitu baza podataka, Naravno, uključujući osobe sheme i prijave korisnika, ali mnoge mundane administrativne zadatke su obavljen. 

Dok baze podataka SQL izgleda slično kao što je SQL Server aplikacijama, ona ne ponašaju točno onako kao DBMS sustavom fizičke ili virtualnog računala. Jer se izvodi na zajedničkom hardveru, njegovih performansi će se razlikovati za učitavanje smješta na taj hardver svih svojih klijenata. To znači da performanse, recimo, pohranjena procedura u SQL baze podataka mogu se razlikovati od jednog dana u drugi. 

Danas, baze podataka SQL omogućuje stvaranje baze podataka držite do 150 gigabajta. Ako morate raditi s većim bazama podataka, servis nudi mogućnost *vanjski pristup*. Da biste to učinili, administratora baze podataka stvara dva ili više *članova vanjski pristup*, od kojih svaka je zasebnu bazu podataka s vlastitu shemu. Podaci se dodijeliti većem te članove nešto što se često se nazivaju *sharding*s svakog člana dodijeljeni jedinstveni *ključ za vanjski pristup*. Trebali biste ciljne upite SQL problemi aplikacije na temelju tih podataka navođenjem tipku vanjski pristup koja služi za identifikaciju člana vanjski pristup upit. Time se omogućuje pomoću klasične relacijski pristup velike količine podataka. Kao i uvijek, postoje gubitke; Upiti ni transakcije mogu obuhvaćati članovi vanjski pristup, na primjer. No kada relacijski PaaS servis je najbolji odabir, a te gubitke su prihvatljiva, pomoću SQL Federacija može biti dobro rješenje.

SQL baze podataka mogu poslužiti aplikacije koje rade na Azure ili negdje drugdje, kao što su u vaše lokalne podatkovnog centra. To čini korisno za oblak aplikacije koje su potrebne relacijskih podataka, kao i lokalne aplikacije koje su prednosti spremanje podataka u oblaku. Mobilna aplikacija možda je za SQL baze podataka radi upravljanja relacijskih podataka za zajedničko korištenje, na primjer, kao možda zaliha aplikaciju koja se pokreće pri više dealers diljem svijeta.

Što mislite o baze podataka SQL podiže očite (i važne) problem: kada treba pokrenete SQL Server na VM i kada baze podataka SQL bolji je odabir? Uobičajeni, postoje gubitke i tako da pristupa je bolje ovisi o svojim potrebama. 

Jedan jednostavan način da razmislite o tome je da biste pogledali SQL bazu podataka kao za nove aplikacije, dok je SQL Server na na VM bolji bi izbor kada premještate postojećeg lokalnog računala s oblakom. Također može biti korisno da biste vidjeli tu odluku još preciznije način, no. Ako, na primjer, je lakše koristiti jer postoji minimalnog baze podataka SQL postavljanje i administracija. Ali izvodi SQL Server u na VM može imati predvidljivi performanse – nije zajednički servis - i podržava i veće koje nisu pridružene baze podataka od SQL baze podataka. I dalje, baze podataka SQL nudi ugrađene replikacije podataka i obrada učinkovito održavanje visoku dostupnost DBMS pomoću vrlo malo tvrtke. SQL Server daje veću kontrolu i donekle šira skup mogućnosti, baze podataka SQL je jednostavnije ćete postaviti i znatno manje posla za upravljanje.

Na kraju, važno je da biste pokazuju nije li baza podataka SQL samo PaaS usluga podataka dostupni na Azure. Microsoftovi partneri sadrže druge mogućnosti. Na primjer, ClearDB nudi MySQL PaaS daje dok Cloudant ustanova NoSQL mogućnost. PaaS podatkovne usluge su pravo rješenje u većini slučajeva, a da je taj se način za upravljanje podacima važan dio Azure.


### <a name="datasync"></a>Sinkronizacija podataka za SQL

Dok baze podataka SQL zadržali tri kopija svake baze podataka u jednom Azure podatkovnog centra, ona ne automatski replicirati podataka između Azure podatkovnim centrima. Umjesto toga nudi SQL sinkronizacije podataka, servis koji možete koristiti da biste to učinili. [4 slici](#Fig4) prikazano je kako to izgleda.

<a name="Fig4"></a>![Dijagram sinkronizacije podataka za SQL][SQL-datasync]
 
**Slika 4: Sinkroniziranje podataka SQL sinkronizira podataka u bazi podataka SQL s podacima u drugim Azure i lokalnih podatkovnim centrima.**

Kao što je dijagram prikazuje, sinkroniziranje podataka SQL možete sinkronizirati podataka preko različitih mjesta. Ako koristite aplikacije u više Azure podatkovnim centrima, na primjer, s podacima koji se pohranjuju u bazi podataka za SQL. Sinkroniziranje podataka SQL možete koristiti da biste zadržali tih podataka koji se sinkroniziraju. Sinkroniziranje podataka SQL možete sinkronizirati i podataka između Azure podatkovnog centra i instance komponente SQL Server u podatkovnim centrom na lokalni. To može biti korisno za održavanje lokalne kopije podataka koji se koriste lokalnog aplikacije i kopiju oblaka koriste aplikacije koji se izvode na Azure. I Premda je prikazano na slici, sinkroniziranje podataka SQL može se koristiti za sinkroniziranje podataka između SQL baze podataka i SQL Server u VM Azure ili neko drugo mjesto.

Sinkronizacija može biti dvosmjerni, a zaključite točno koje će se podaci sinkronizirati i koliko često to učiniti. (Sinkronizacija između baza podataka nije atomske, no - postoji uvijek barem neke odgode.) I Međutim služi, postavljanje sinkronizacije za sinkronizaciju podataka SQL je potpuno konfiguracije utemeljenih na; postoji bez koda za pisanje.


### <a name="datarpt"></a>SQL podataka izvješća pomoću virtualnim strojevima

Kada bazi podataka sadrži podatke, nečijeg će vjerojatno htjeti stvoriti izvješća na temelju tih podataka. Azure možete pokrenuti SQL Server Reporting Services (SSRS) u virtualnim računalima sustava Azure, koja je jednaka functionally izvodi SQL Server Reporting Services na lokalni. Zatim SSRS možete koristiti da biste pokrenuli izvješća za podatke pohranjene u bazi podataka SQL Azure.  [Slika 5](#Fig5) prikazuje funkcioniranje postupka.

<a name="Fig5"></a>![Dijagram izvješćivanja SQL][SQL-report]
 
**Slika 5: SQL Server Reporting Services pokrenut u na virtualnim računalima sustava Azure nudi servisu reporting services za podatke u SQL baze podataka. .**

Prije nego što korisnik može vidjeti izvješća, netko definira kako to izvješće treba izgledati (korak 1). S SSRS na na VM, to možete učiniti pomoću alata za dva: SQL Server Data Tools, dio SQL Server 2012 ili njegov drugi datum zbog međuovisnosti, Development Studio poslovne inteligencije (BI). Kao i kod SSRS, te definicije izvješća su izražena u u izvješću definicija jezik (RDL). Nakon stvaranja datoteke RDL izvješća će se prenijeti na VM u oblaku (korak 2). Definiciju izvješće je spremna za korištenje.

Nakon toga pristupa korisnika aplikacije izvješća (korak 3). Aplikacija prosljeđuje zahtjev VM SSRS (koraku 4), koji se obrati SQL baze podataka ili drugih izvora podataka da biste dobili podatke potrebne (korak 5). SSRS na temelju tih podataka i odgovarajuće RDL datoteke za prikaz izvješća (korak 6), zatim vraća izvješće s aplikacijom (korak 7), čime će se prikazati korisniku (korak 8).

Ugrađivanje izvješća u aplikaciji, scenarij ovi, ne samo mogućnost. Također je moguće prikazati izvješća u Report Manager na VM, sustava SharePoint na na VM ili druge načine. Izvješća mogu se kombinirati, s jedno izvješće s vezom na drugi.

SSRS na programa Azure VM daje punu funkcionalnost telefona kao izvješćivanja rješenje u oblaku. Izvješća možete koristiti bilo koji izvora podataka podržava SSRS. Aplikacije i izvješća mogu sadržavati ugrađeni kod ili skupovi za podršku prilagođene ponašanja. Izvršavanje izvješća i vizualizacije su brzo jer se sadržaj poslužitelj za izvješća i modul pokreću zajedno na istom poslužitelju virtualne.



## <a name="tblstor"></a>Spremište tablica

Relacijskih podataka je korisno u većini slučajeva, ali nije uvijek odabir. Ako aplikacija treba brzo jednostavan pristup vrlo velike količine slobodnije strukturiranih podataka, na primjer, relacijske baze podataka možda neće dobro funkcionirati. Tehnologija NoSQL je vjerojatno će biti bolje.

Primjer takav pristup NoSQL je Azure spremište tablica. Bez obzira na njegov naziv spremište tablica ne podržava standardnu relacijskih tablica. Umjesto toga, u njoj nalaze i što je poznato kao *ključa/vrijednosti pohranu*skupa podataka za pridruživanje određenog ključ, a zatim da programa access aplikacije tih podataka unosom tipku. [Slika 6](#Fig6) prikazuje osnove.

<a name="Fig6"></a>![Dijagram spremište tablica][SQL-tblstor]
 
**Slika 6: Azure spremište tablica je spremište ključa vrijednosti koje omogućuje brzo, jednostavan pristup velike količine podataka.**

Kao što je blob-ova, svaku tablicu povezan je s računa Azure prostora za pohranu. Tablica i mnogo naziva kao što je blob polja s URL-om obrasca

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*TableName*&gt;

Kao što je prikazano na slici, svaku tablicu je podijeljen u neki broj particije, od kojih svaka mogu spremiti na zasebnom računalo. (Ovo je obrazac od sharding, s vanjski pristup SQL.) Azure aplikacije i aplikacija negdje drugdje možete pristupiti tablice pomoću protokola RESTful OData ili u biblioteku Azure prostora za pohranu klijenta.

Svaki particije u tablici nalaze neki broj *entiteti*, svaka sadrži koliko god 255 *Svojstva*. Svaki svojstvo ima naziv, vrstu (kao što je binarni, booleovom, datumom i vremenom, Int ili niz) i vrijednosti. Za razliku od relacijski prostor za pohranu, u ovim su tablicama imati nema fixed sheme i tako da drugi entiteti u istoj tablici može sadržavati svojstva s različitim vrstama. Jedan entitet može imati samo svojstvo niz koji sadrži naziv, na primjer, dok drugi entitet u istoj tablici ima dvije funkcije Int svojstva koja sadrži korisnički ID broj i odobrenja ocjena.

Da biste odredili određeni entitet unutar tablice, aplikacije sadrži taj entitet ključ. Ključ ima dva dijela: *ključ particija* koja služi za identifikaciju određene particija i *redak ključa* koja služi za identifikaciju entitet unutar te particije. Na [slici 6](#Fig6), na primjer, klijent zahtijeva entitet s particije ključ A i retka ključ 3 i spremište tablica vraća taj entitet, uključujući sva svojstva sadrži.

Ta struktura omogućuje tablica se veliki - jedne tablice može sadržavati do 100 terabajta podataka – i omogućuje brz pristup podacima koje sadrže. To također donosi ograničenja, no. Na primjer, postoji ne podržava transakcijskih ažuriranja koji obuhvaćaju tablica ili čak particije u jednu tablicu. Skup ažuriranja u tablicu samo može biti grupiran u atomske transakcije ako su svi entiteti koji je uključen u istom particije. Došlo je i nije moguće poslati upit tablice na temelju vrijednosti svojstvima ni postoji podrška za spojeva preko više tablica. I za razliku od relacijskim bazama podataka, tablica ne podržava pohranjene procedure.

Azure spremište tablica je dobar izbor za aplikacije koje su potrebne brzo jeftino pristup velike količine slobodnije strukturiranih podataka. Internetske aplikacije koja pohranjuje informacije o profilu za veći broj korisnika, na primjer, mogu koristiti tablice. Brz pristup je važno u tom slučaju, a aplikacija vjerojatno ne morate punu snagu programa SQL. Daje se ta je funkcija da bi se dobio brzina i veličina možete ponekad smisla, a da je spremište tablica samo pravo rješenje za neke probleme.


## <a name="hadoop"></a>Hadoop

Tvrtke i ustanove imaju je stvara skladištima podataka za desetljeća. Te zbirke podataka, najčešće pohranjenih u relacijskih tablica, obavijestite zaposlenike raditi i Saznajte iz podataka na razne načine. Sa sustavom SQL Server, na primjer, uobičajeno je da biste koristili alate kao što je SQL Server Analysis Services da biste to učinili.

Međutim, pretpostavimo da želite učiniti analizi koji nisu relacijskih podataka. Podataka može potrajati nekoliko oblika: informacije iz senzori ili RFID oznake, datoteke zapisnika u farmi poslužitelja, clickstream podataka koje je stvorio web-aplikacije slike iz mjehuričaste dijagnostičkih uređaji i drugo. Te podatke može biti zaista veliki, prevelika da bi se mogla koristiti učinkovito za tradicionalni podataka skladištu. Problemi velikih skupova podataka ovako, rijetko prije, samo nekoliko godina prekorače sada prilično uobičajenih.

Da biste analizirali ove vrste velikih skupova podataka, naš industrijskih uglavnom konvergira na jednom rješenje: tehnologije Otvori izvor Hadoop. Hadoop pokreće klaster fizičke ili virtualnim strojevima šire podataka radi na svim tim uređajima i obrada paralelno. Više strojeva Hadoop je za korištenje, brže ga mogu provoditi ono rad radi.

Ove vrste problema je prirodnim odgovara javno oblaka. Umjesto Održavanje programa VMs vojske lokalne poslužitelje koji sjesti neaktivnosti velik dio vrijeme pokretanje Hadoop u oblaku omogućuje stvaranje (i plaćanje za) samo kada su vam potrebne. Čak i bolje itd više velikih skupova podataka koje želite analizirati s Hadoop se stvara u oblaku, spremanje imate li problema prilikom premještanja. Da biste lakše izrabljuje te synergies, Microsoft pruža Hadoop servisa na Azure. [Slika 7](#Fig7) prikazuje najvažnije komponente taj servis.

<a name="Fig7"></a>![Dijagram hadoop][hadoop]

**Slika 7: Hadoop na Azure pokreće MapReduce poslove koje obradu podataka paralelno pomoću više virtualnih računala.**

Da biste koristili Hadoop na Azure, najprije postavljate ovoj platformi oblaku da biste stvorili Hadoop klaster, navodeći broj VMs vam je potrebna. Postavljanje Hadoop klaster sami je koji nisu trivial zadatka i tako da Azure to vam odgovara. Kada završite pomoću klaster, isključivanja ga. Nema potrebe za plaćanje računalnim resursa koje ne koristite.

Hadoop aplikacije, pod nazivom *posla*, koristi model programiranja naziva *MapReduce*. Kao što je prikazano na slici, logike za posao MapReduce pokreće istodobno preko mnogo VMs. Po obradu podataka paralelno Hadoop možete analizirati podatke puno više informacija od jednog računala rješenja.

Na Azure, podaci MapReduce posla radi na obično čuva se u spremište blobova platforme. U Hadoop, međutim, MapReduce poslove očekujete da će podaci koji se spremaju u *Hadoop Distributed datoteka sustava (HDFS)*. HDFS je slična blobova donekle; ga replicira podataka preko više fizičke poslužitelja koja, primjerice. Umjesto dupliciranje ta je funkcija Hadoop na Azure umjesto izlaže blobova kroz HDFS API kao što je prikazano na slici. Dok logike u posao MapReduce misli pristupa običnih datoteka HDFS, posao zapravo je rad s podacima koji se strujanjem na njega s blob-ova. I za podršku i u ovom slučaju gdje se izvode više poslovi podatke, Hadoop na Azure omogućuju kopirate podatke s blob-ova u cijeli HDFS izvodi u na VMs. 

MapReduce poslove su najčešće pisane Java danas, na način koji podržava Hadoop na Azure. Microsoft i dodao podršku za stvaranje MapReduce zadatke na drugim jezicima, uključujući C#, F # i JavaScript. Cilj je da bi se jednostavnije pristupačno veću grupu razvojnim inženjerima tehnologije velikih skupova podataka.

Uz HDFS i MapReduce, Hadoop obuhvaća i druge tehnologije koje omogućuju osobe analize podataka bez pisanja posao MapReduce sami. Na primjer, Svinja je više razine jezik namijenjen analiza velikih skupova podataka dok je grozd nudi jezik SQL poput naziva HiveQL. Svinja i grozd zapravo generiranje MapReduce poslove koje obrada podataka HDFS, ali ih sakriti toga sa svojim korisnicima. Obje se isporučuju sa Hadoop na Azure.

Microsoft pruža i HiveQL upravljački program za Excel. Pomoću dodatka programa Excel, poslovni analitičar analitičar podataka možete stvoriti HiveQL upita (a time i zadacima MapReduce) izravno iz programa Excel, a zatim obradi i vizualizacija rezultata pomoću dodatka PowerPivot i drugih alata programa Excel. Hadoop na Azure obuhvaća kao i druge tehnologije kao što su strojnog učenja biblioteke Mahout, sustav za dubinsko pretraživanje grafikonu Pegasus i drugo.

Važno je analiza velikih skupova podataka, a pa Hadoop važno je. Unosom Hadoop kao upravljanog servisa na Azure, te veze na poznate alate kao što je Excel, Microsoft trebao pri upućivanje tehnologije pristupačnim širem skup korisnika.

Više njih svim korisnicima Dopusti, podatke o različitim važno je. To je Zašto Azure obuhvaća raspon mogućnosti za upravljanje i poslovne analizu podataka. Bilo kakve aplikaciju koju pokušavate stvoriti, vjerojatno je da ćete pronaći nešto u ovoj platformi oblaka koja će funkcionirati umjesto vas.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
