<properties 
    pageTitle="Kako implementirati slojevito navigacije u pretraživanju Azure | Microsoft Azure | Navigacija kategorija za pretraživanje" 
    description="Dodajte slojevito navigacije aplikacije integrirane Azure pretraživanja, servis za pretraživanje oblak koji se nalaze na Microsoft Azure." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Kako implementirati slojevito navigacije u pretraživanju Azure

Navigacija po slojevito je filtriranja mehanizam koji omogućuje navigaciju koja se sama usmjerenog analiza u aplikacijama za pretraživanje. Iako termina 'slojevito navigacije' može biti nepoznatim, je gotovo u given koje ste koristili prije. Kao što je prikazano u sljedećem primjeru, slojevito Navigacija je ništa više kategorija koristiti radi filtriranja rezultata.

## <a name="what-it-looks-like"></a>Kako izgleda

 ![][1]
  
Pozornici može vam pomoći da pronađete ono što tražite, uz jamstvo da neće dobiti nula rezultata. Kao programer, pozornici omogućuju izložiti najkorisnije kriterija pretraživanja za navigaciju na corpus pretraživanja. U aplikacijama internetska maloprodaja slojevito navigacije često ugrađen putem marke, odjela (dijete 's shoes), veličinu, cijena, popularnosti i ocjena. 

Implementacije slojevito navigacije razlikuje se preko tehnologije pretraživanja web-mjesta i mogu biti vrlo složeni. U pretraživanju Azure slojevito navigacije ugrađen u trenutku upita pomoću polja u atribut prethodno naveden u sheme. U upitima koji unapređuje aplikacije upita morate poslati *fasete parametara upita* da biste dobili vrijednosti dostupne fasete filtra za taj dokument skup rezultata. Da biste zapravo obrezivanje rezultat dokumenta postavite, morate zatvoriti aplikaciju na `$filter` izraz.

Pomoću razvoj aplikacija pisanja koda za koji konstrukata upita čine skupno rad. Mnoge ponašanja aplikacije koje biste htjeli iz slojevito navigacije osigurava servisa, uključujući ugrađenu podršku za postavljanje raspona i broji početak fasete rezultate. Servis sadrži i prvo zadane postavke koje olakšavaju izbjeći unwieldy navigacije strukture. 

Ovaj članak sadrži sljedeće odjeljke:

- [Sastavljanje je](#howtobuildit)
- [Sastavljanje prezentacije sloja](#presentationlayer)
- [Stvaranje indeksa](#buildindex)
- [Provjera kvalitete podataka](#checkdata)
- [Sastavljanje upita](#buildquery)
- [Savjete za kontrolu slojevito navigacije](#tips)
- [Slojevito navigacije na temelju vrijednosti u rasponu](#rangefacets)
- [Slojevito navigacije na temelju GeoPoints](#geofacets)
- [Isprobajte sami](#tryitout)

##<a name="why-use-it"></a>Zašto koristiti
Učinkovitijeg aplikacije za pretraživanje imati više modela interakcije osim okvir za pretraživanje. Navigacija po slojevito je zamjenski ulaza za pretraživanje, koja nudi praktičan zamjena za ručno upisivati izraze složene pretraživanja.

##<a name="know-the-fundamentals"></a>Saznajte osnove

Ako ste novi korisnik pretraživanje razvoj, najbolji način da razmislite slojevito navigacije je da prikazuju se mogućnosti za samostalno usmjerenog pretraživanje. To je vrsta sučelja za pretraživanje kroz razine naniže, koji se temelji na unaprijed definirane filtre koji se koristi za brzo suziti rezultate pretraživanja kroz točke i kliknite Akcije. 

**Interakcija modela**

Sučelje za navigaciju slojevito pretraživanje je izračun s iteracijama, možemo najprije objašnjenje kao niz upita koji unfold kao odgovor na akcije korisnika.

Polazište je stranica aplikacije koja omogućuje slojevito navigacije, obično postaviti u periphery. Slojevito navigacije često je struktura stabla, s potvrdnim okvirima za svaku vrijednost ili kliknuti tekst. 

1.  Upit šalje Azure pretraživanja određuje slojevito navigacijske strukture putem jedan ili više parametara upita fasete. Ako, primjerice, mogu sadržavati upit `facet=Rating`, možda s na `:values` ili `:sort` mogućnost da biste dodatno precizirali prezentaciju.
2.  Sloj prezentacije prikazuje stranicu za pretraživanje koji omogućuje slojevito navigaciju pomoću pozornici naveden na zahtjev.
3.  Uz slojevito navigacijske strukture koja obuhvaća ocjena, korisnik klikne "4" da biste naznačili da se prikazati samo proizvoda s ocjenom 4 ili noviji. 
4.  Odgovor, aplikacija šalje upit koji sadrži`$filter=Rating ge 4` 
5.  Sloj prezentacije ažurira stranici prikazuje skup smanjene rezultata, koja sadrži samo onih stavki koje zadovoljavaju kriterij novi (u ovom slučaju proizvodi ocjenjivanje 4 i gore).

U fasete je parametra upita, no zbuniti s unos upita. Nikad ne služi kao odabira kriterija u upitu. Umjesto toga smatrati parametara upita fasete unosi u navigacijske strukture koji se isporučuju se vratiti u odgovoru. Za svaki fasete upita parametar unesete Azure pretraživanja će rezultirati koliko nalaze u rezultatima djelomične za svaku vrijednost fasete.

Obavijest na `$filter` u koraku 4. To je važno aspekt slojevito navigacije. Iako se pozornici i filtri neovisno u na API-JA, trebat će vam i da biste doživljaj namjeravate. 

**Dizajniranje obrazaca**

U kodu aplikacije uzorak jest korištenje parametara fasete upita da biste se vratili slojevito navigacijske strukture uz fasete rezultate, uz $filter izraz koji se rukuje kliknite događaj. Zamislite na `$filter` izraz kao kod iza stvarni ograničavanje rezultata pretraživanja je vratio sloju prezentacije. Zadana boja fasete, kliknite boja crvena je implementirana putem programa `$filter` izraz koji odabire samo onih stavki koje su boje crvene boje. 

**Osnove upit u pretraživanju Azure**

U odjeljku pretraživanje Azure zahtjeva za navedeni su kroz jedan ili više parametara upita (pogledajte [Pretraživanje dokumenata](http://msdn.microsoft.com/library/azure/dn798927.aspx) za opis svake od njih). Nijedna parametre upita nije obavezno, ali morate imati barem jedno da bi upit za moraju biti valjane.

Preciznost, obično prepoznata kao mogućnost za filtriranje nevažnih pristupa se postiže kroz jedno ili oboje od ovih izraza:

- **pretraživanje =**<br/>
Vrijednost tog parametra čine izraz za pretraživanje. Možda je jedinstveni dio teksta ili na složenog izraza za pretraživanje koja sadrži više uvjeta i operatore. Izraz za pretraživanje na poslužitelju, koristi se za pretraživanje po cijelom tekstu, slanje upita koji se može pretraživati polja u indeks za podudaranje uvjete, daje rezultate u redoslijedu ranga. Ako ste postavili `search` na null upita izvođenja je premašila dopuštenu cijeli indeks (to jest, `search=*`). U ovom slučaju ostale elemente na upit, kao što je `$filter` bilježenje rezultata profila, bit će primarni čimbenika utječe dokumenata koji se vraćaju ili `($filter`) i kojim redoslijedom (`scoringProfile` ili `$orderb`y).

- **$filter =**<br/>
Filtar je Napredna mehanizam za ograničavanje veličinu rezultata pretraživanja na temelju vrijednosti atributa određeni dokument. A `$filter` prvo, nakon čega slijedi faceting logike koje generira dostupne vrijednosti i odgovarajuće brojanja za svaku vrijednost

Pretraživanje složene izraze će smanjiti performanse upita. Gdje je to moguće, koriste izraze za dobro izgrađene filtar povećati točnost i poboljšati performanse upita.

Da biste bolje razumjeli kako filtra dodaje veća preciznost, usporediti s složenog izraza za pretraživanje da biste neki koji sadrži izraz filtra:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Iako vrijede upite, drugi je ako tražite nije-motels s parking u Seattle. Prvog upita ovisi o tim određene riječi navedenim ili ne spominju u nizu polja kao što su naziv, opis i bilo kojeg polja koje sadrže podatke koji se može pretraživati. Drugi upit traži precizno rezultata na strukturiranih podataka i vjerojatno će biti puno točnije.

U programima koji sadrže slojevito navigacije želite biti sigurni da svaku akciju korisnika putem slojevito navigacijske strukture je praćeni suziti rezultata pretraživanja, postići pomoću izraza filtra.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Sastavljanje je

Slojevito navigacije u pretraživanju Azure je implementirana u kodu aplikacije koje sastavlja zahtjev, ali ovisi unaprijed definirani elementi sheme.

Unaprijed definirani u rezultatima pretraživanja indeks je na `Facetable [true|false]` indeksni atribut, postavite na odabrana polja radi omogućivanja i onemogućivanja njihova upotreba u slojevito navigacijske strukture. Bez `"Facetable" = true`, polja ne mogu se koristiti u navigacijskom oknu fasete.

Upit trenutku kodu aplikacije stvara zahtjev koji uključuje `facet=[string]`, na zahtjev za parametar koji sadrži polje fasete tako da. Upit može imati više pozornici, kao što su `&facet=color&facet=category&facet=rating`, svaki od njih odvojene programa ampersand (&) znakova.

Kod aplikacije i morate izgraditi na `$filter` izraz za rukovanje događajima kliknite slojevito navigacijom. A `$filter` smanjuje rezultata pretraživanja, korištenje fasete vrijednosti kao kriterija filtra.

Azure pretraživanje vraća rezultate pretraživanja, po termine unese korisnik uz ažuriranja slojevito navigacijske strukture. U pretraživanju Azure slojevito Navigacijsko je izgradnju jedne razine, fasete vrijednostima i Broji koliko rezultata nalaze se za svaki od njih.

Sloj prezentacije u kodu nudi korisničkog sučelja. Ga treba popisa sastavnoj dijelove slojevito navigacije, kao što su oznake, vrijednosti, potvrdite okvire i broj. Azure pretraživanje REST API-JA je agnostic platforme, pa bilo kakve jezika i platformu želite. Važno je da biste dodali elemente korisničkog Sučelja koji podržavaju rastuće osvježavanja, ažurirane stanjem korisničkog Sučelja kako se odabire svaki dodatni fasete. 

U sljedećim se odjeljcima smo ćete pogledajte bliže sastavljanje svaki dio počevši od sloj prezentacije.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>Sastavljanje prezentacije sloja

Rad od sloj prezentacije može vam pomoći otkriti preduvjete koje možda je u suprotnom Propušteni i razumijevanje koje mogućnosti su ključna okruženje za pretraživanje.

Pomoću slojevito navigacije stranici weba ili računala prikazuje slojevito navigacijske strukture, otkriva korisničkom unosu na stranici, a umeće elemenata mijenjaju. 

Za web-aplikacije AJAX-a obično se koristi u sloju prezentacije jer omogućuje da biste osvježili rastuća promjena. Nije moguće koristiti i ASP.NET MVC ili druge platforme vizualizacije koji je moguće povezati sa servisom Azure pretraživanja putem HTTP-a. Primjer aplikacije koristi u cijelom u ovom se članku – **Kataloga AdventureWorks** – se događa s biti ASP.NET MVC aplikacija.

Sljedeći primjer PDF datoteku **index.cshtml** aplikacije uzorka sastavlja dinamički HTML strukturi za prikaz slojevito navigacije na stranici s rezultatima pretraživanja. U uzorku, slojevito navigacije ugrađen u stranice s rezultatima pretraživanja, a pojavljuje se nakon što korisnik je poslao pojam za pretraživanje.

Obavijest o ima li svaki fasete natpis (boja, kategorija cijene), povezivanje s poljem slojevito (boja, naziv kategorije, listPrice), pa na `.count` parametar koji se koristi za vraćanje broj pronađenih za taj rezultat fasete stavki.

  ![][2]
 

> [AZURE.TIP] Prilikom dizajniranja stranice s rezultatima pretraživanja, imajte na umu da biste dodali mehanizam za čišćenje pozornici. Ako koristite potvrdne okvire, korisnici mogu jednostavno intuit kako očistiti filtre. Za ostale rasporede, bilo bi dobro povratna uzorak ili neki drugi kreativni pristup. Na primjer, u aplikaciji za uzorak AdventureWorks kataloga možete kliknuti naslov, AdventureWorks katalog, ponovno postavite stranice pretraživanja.

<a name="buildindex"></a>
##<a name="build-the-index"></a>Stvaranje indeksa

Faceting je omogućena na temelju polja u indeksu putem taj atribut indeks: `"Facetable": true`.  
Sve vrste polja koje se mogu koristiti vjerojatno slojevito navigacijom su `Facetable` prema zadanim postavkama. Kao što su vrste polja `Edm.String`, `Edm.DateTimeOffset`, i sve vrste numeričkih polja (zapravo, sve vrste polja su facetable osim `Edm.GeographyPoint`, koji ne mogu se koristiti u navigaciji slojevito). 

Kada se stvara indeks, preporučenim načinom rada za navigaciju slojevito je izričito isključiti faceting za polja koja nikad ne želite koristiti kao u fasete.  Posebno niza polja za jednočlana vrijednosti, kao što su naziv ID ili proizvoda, mora biti postavljena na `"Facetable": false` da biste spriječili njihova upotreba slučajne (i Neučinkovit) u slojevito navigacijskom oknu.

Slijedi sheme za aplikaciju uzorka AdventureWorks kataloga (obrezati neke atributa radi smanjivanja cjelokupne veličine):

 ![][3]
 
Imajte na umu da `Facetable` je isključen za niza polja koja bi trebalo koristiti kao pozornici, kao što je ID ili naziv. Uključivanje faceting isključivanje gdje je ne morate olakšava održavanje veličina indeksa small i obično poboljšavaju performanse.

> [AZURE.TIP] Kao preporučenim načinom rada obuhvaćaju potpunog skupa atribute indeks za svako polje. Iako `Facetable` po zadanom je uključeno za gotovo sve polja nastojanju postavljanje svaki atribut omogućuju razmislite kroz posljedice svaki odluka shemu. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Provjera kvalitete podataka 

Prilikom razvoja bilo koju aplikaciju usmjereni na podatke, Priprema podataka često jedan je od veće dijelove posla. Aplikacije za pretraživanje su bez iznimke. Kvalitete podataka sadrži izravno sa slikom na li slojevito navigacijske strukture materializes kao što ste očekivali da biste, kao i njegov učinkovitosti u vam sastavljanje filtri koje smanjuju rezultat postavili.

U odjeljku pretraživanje Azure corpus pretraživanja je oblikovan iz dokumenata koji popunjavanje indeksa. Dokumenti sadrže vrijednosti koje se koriste za izračun pozornici. Ako želite fasete marke ili cijena, svakom dokumentu mora sadržavati vrijednosti za *Nazivrobnemarke* i *ProductPrice* valjanih, dosljedan i produktivni kao mogućnost za filtar.

Nekoliko podsjetnike koje valja scrub za navedena su u nastavku:

- Za svako polje koje želite fasete po zapitajte se sadrži li vrijednosti koje su prikladne kao filtri u koja se sama usmjerenog pretraživanja. Vrijednosti mora biti kratki, opisni i potpuno isticanjem nudi poništite odabir između konkurentnog mogućnosti.
- Pravopisne pogreške ili gotovo iste vrijednosti. Ako fasete na boje i vrijednosti polja uključuju narančastu i Ornage (pogrešno napisanu), fasete koji se temelji na polju boju želite obraditi oboje.
- Kombinirani tekst slučaja također možete wreak havoc u slojevito navigacijskom oknu narančastu i narančastu pojavljuju kao dvije različite vrijednosti. 
- Jedan i množini verzije iste vrijednosti može uzrokovati zasebnom fasete za svaki unos.

Kao što je možete zamisliti diligence u Priprema podataka je ključni aspekt učinkovitih slojevito navigacije.

<a name="buildquery"></a>
##<a name="build-the-query"></a>Sastavljanje upita

Kod koji pišete za izradi upita navedite svi dijelovi valjani upit, uključujući izraza za traženje, pozornici, filtri, bilježenje rezultata profili – ništa za formulate zahtjev. U ovom ćete odjeljku smo ćete Istražite gdje će se pozornici uklapaju u upitu i kako koriste filtara s pozornici izlaganje skup smanjene rezultata.

Primjer često dobro je mjesto za početak. Sljedeći primjer PDF datoteku **CatalogSearch.cs** sastavlja zahtjev koji stvara fasete navigacije na temelju boja, kategorija i cijena. 

Obratite pozornost na to jesu li pozornici sastavni dio u ovom primjeru aplikacije. Sučelje za pretraživanje u katalogu AdventureWorks osmišljena je oko slojevito navigacije i filtri. Ovo je evident u izravne poruke položaj slojevito navigacije na stranici. Primjer aplikacije sadrži URI parametara za pozornici (boja, kategorija, cijene) kao svojstva na način pretraživanja (kao što je nastala primjer aplikacije).

  ![][4]
 
Parametra upita fasete je polje s vrijednostima i ovisno o vrstu podataka, možete se dodatno s parametrima po popisu razdvojen zarezom koja obuhvaća `count:<integer>`, `sort:<>`, `intervals:<integer>`, i `values:<list>`. Popis vrijednosti nije podržana za numeričke podatke pri postavljanju raspona. Informacije o korištenju potražite u članku [Pretraživanje dokumenata (Azure pretraživanje API -JA)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Uz pozornici, zahtjev formulated tako da vaše aplikacije treba Sastavljanje i filtre da biste suzili skup kandidata dokumenti na temelju odabira fasete vrijednost. U trgovini bicikle slojevito navigacije predstavlja clues na pitanja kao što su "koje boje, proizvođači i vrste bicikli dostupnih", tijekom filtriranja odgovore na pitanja kao što su "koje točno bicikli su crvene, brdski bicikli u ovoj cijena raspon".

Kad korisnik klikne "Crveno" da biste naznačili bi trebala biti prikazana crvena proizvoda, sljedeći upit šalje aplikacije sadrži `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Najbolje prakse za slojevito navigacije

Na sljedećem su popisu navedene nekoliko najbolje prakse.

- **Preciznost**<br/>
Pomoću filtara. Ako vam je samo izraza za traženje samostalno korijen riječi može uzrokovati dokument se vraćaju koji ne sadrži vrijednost precizno fasete u bilo kojem od njegova polja. 

- **Ciljna polja**<br/>
U slojevito dubinske analize prema dolje, obično želite obuhvatiti samo dokumente koji imaju vrijednost fasete u određenom polju (Facetirana), ne bilo kojeg mjesta putem sva polja pretraživanja. Dodavanje filtra pojačavaju polje cilj preusmjerivanjem servisa za pretraživanje samo u polju slojevito za odgovarajuće vrijednosti.

- **Indeks učinkovitosti**<br/>
Ako aplikacija koristi slojevito navigacije isključivo (to jest, bez okvir za pretraživanje), možete označiti polje kao `searchable=false`, `facetable=true` čime se dobiva više sažetom indeksa. Osim toga, indeksiranje pojavljuje se samo na cijelu fasete vrijednosti bez nema word prekida ili Indeksiranje sastavnih dijelova vrijednosti više riječi.

- **Performanse**<br/>
Filtri suzili skup dokumenata za prijedlog za pretraživanje i ih isključiti iz rangiranja. Ako imate veliki skup dokumenata, pomoću značajke dubinske analize vrlo selektivno fasete dolje često steći ćete znatno bolje performanse.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Savjete za kontrolu slojevito navigacije

U nastavku je savjet list s smjernice o specifični problemi.

**Dodavanje natpisa za svako polje u oknu za navigaciju fasete**

Natpisi obično su definirani u HTML ili obrazac (**index.cshtml** u aplikaciji za uzorak). Postoji bez API-JA u pretraživanju Azure fasete navigacije naljepnice za ili druge vrste metapodataka.

**Definiranje polja koja mogu se koristiti kao fasete**

Opoziv koji određuje shemi indeksa polja koja su dostupni kao je fasete. Pod pretpostavkom da polje je facetable, upitu određuje polja koja će se fasete po. Polje po kojem ste faceting sadrži vrijednosti koje se prikazuju ispod natpisa. 

Vrijednosti koje se prikazuju u odjeljku svaku pojedinu naljepnicu dohvaćaju iz indeksa. Ako, na primjer, ako je polje fasete *boja*, dostupnih za dodatne filtriranje vrijednosti bit će vrijednosti za to polje (crveno, crno itd.).

Za numeričke i DateTime samo vrijednosti, izričito možete postaviti vrijednosti u polju fasete (na primjer, `facet=Rating,values:1|2|3|4|5`). Popis vrijednosti dopuštena za sljedećih vrsta polja da biste pojednostavnili odvojenosti fasete rezultata u susjedne rasponi (ili raspona na temelju numeričke vrijednosti ili vremenskih razdoblja). 

**Obrezivanje fasete rezultata**

Rezultati fasete su dokumenti pronađen u rezultatima pretraživanja koje odgovaraju fasete termina. U sljedećem primjeru u rezultatima pretraživanja za *računalstvo u oblaku*, 254 stavke i imaju *Interna specifikacija* kao vrstu sadržaja. Stavke nisu nužno isključuju. Stavke koji zadovoljavaju kriterij i filtri, broje u svakom od njih. To je moguće kada faceting na `Collection(Edm.String)` polja, često se koristi za implementaciju označavanja dokumenta.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Općenito govoreći, Utvrdite li da fasete rezultati su persistently prevelika, preporučujemo da dodate dodatne filtre, kao što je opisano u starijim sekcije, a korisnicima vašeg računala dodatne mogućnosti za sužavanje pretraživanja.

**Ograničenje stavki u oknu za navigaciju fasete**

Za svako slojevito polje u stablu navigacijskog je zadano ograničenje od 10 vrijednosti. To je zadana vrijednost smisla za navigaciju strukture jer je moguće upravljati veličinu zadržava popis vrijednosti. Zadani možete nadjačati dodjelom vrijednosti za brojanje.

- `&facet=city,count:5`određuje samo prvih 5 gradova pronaći u gornjem rangiranih rezultata vraćaju kao rezultat fasete. Daje pojam za pretraživanje "zračna luka" i 32 rezultata upita određuje `&facet=city,count:5`, samo prvih pet jedinstveni gradove s najviše dokumentima u rezultatima pretraživanja nalaze se u rezultatima fasete.

Obavijest o razliku između fasete rezultate i rezultata pretraživanja. Rezultati su sve dokumente koje odgovaraju upitu. Rezultati fasete su adrese za svaku vrijednost fasete. U primjeru, rezultati pretraživanja neće sadržavati nazive gradova koji se ne nalaze na popisu klasifikacija fasete (5 u našem primjeru). Rezultati koje su filtrira kroz slojevito navigacije postaju vidljivi korisniku kada korisnik čisti pozornici ili odabire druge pozornici osim grada. 

> [AZURE.NOTE] Razgovarate o `count` kada postoji više od jedne vrste može biti zbunjujuće. U sljedećoj su tablici daje kratak sažetak korištenja termin u API pretraživanja Azure, ogledni kod i dokumentacije. 

- `@colorFacet.count`<br/>
U kodu prezentacije, trebali biste vidjeti parametar count na fasete, služi za prikaz broja rezultata fasete. U rezultatima fasete broj označava broj dokumenata koji se podudaraju fasete termina ili raspona.

- `&facet=City,count:12`<br/>
U upitu fasete možete postaviti broj vrijednosti.  Zadana vrijednost je 10, no možete je postaviti viših ili nižih. Postavljanje `count:12` dobije vrhu 12 podudaranja u rezultatima fasete po broj dokumenata.

- "`@odata.count`"<br/>
U odgovoru upita tu vrijednost upućuje na to koliko stavki koje se podudaraju u rezultatima pretraživanja. U je veće od zbroj fasete rezultate u kombinaciji, zbog prisutnosti stavke koje odgovaraju pojam za pretraživanje, ali ste nema fasete vrijednost rezultata.


**Razina u slojevito navigacije** 

Kao što je naznačeno, ne postoji Izravni podrška za ugnježđivanje pozornici u hijerarhiji. U okvir slojevito navigacije podržava samo jednu razinu filtara. No postoje zaobilazna rješenja. Možete kodiranje fasete hijerarhijske strukture u na `Collection(Edm.String)` jedna stavka pokažite po hijerarhije. Implementacije takvog zaobilaznog rješenja izlazi iz ovog članka, ali možete pročitati o zbirkama u [OData dijela](http://msdn.microsoft.com/library/ff478141.aspx). 

**Provjera valjanosti polja**

Ako izgradnje popisa pozornici dinamički na temelju nepouzdanih korisničkom unosu, trebali biste ili provjeravate valjane naziva slojevito polja ili escape imena kada se stvara pomoću URL-ovi `Uri.EscapeDataString()` .NET ili u jednaku vrijednost u svoju platformu po izboru.

**Broji u rezultatima fasete**

Prilikom dodavanja filtra slojevito upit, možda ćete morati zadržati izjava fasete (na primjer, `facet=Rating&$filter=Rating ge 4`). Tehnički, fasete = ocjena nije potreban, ali njegovo vraća broji vrijednosti fasete ocjena 4 i noviji. Ako, na primjer, ako korisnik klikne "4" i upit uključuje filtar za veće ili jednako "4", broji se vraćaju za svaki ocjena koji je 4 i prema gore.  

**Sharding utjecaju na broji fasete**

U određenim uvjetima ponekad fasete broji odgovaraju skupovima rezultata (pogledajte [slojevito navigacije u pretraživanju Azure (forum objavu)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Broji fasete može biti netočan zbog arhitektura sharding. Svaki indeks pretraživanja sadrži više shards, svaki od njih izvješćima i pozornici najvećih N, broj dokumenata koji se zatim posložene jedan rezultat. Ako neki shards mnogo podudarnih vrijednosti, a neki imaju manje, vidjet ćete neke vrijednosti fasete nedostaje, ili u odjeljku-broje se u rezultatima.

Iako se taj problem može se promijeniti u bilo kojem trenutku Ako naiđete na takvo ponašanje danas, možete riješiti ga tako da umjetno inflating Brojanje:<number> vrlo veliki broj da biste nametnuli izvješćivanja cijelog svaki shard. Ako je vrijednost broj: je veći od ili jednak broju jedinstvene vrijednosti u polju, koje su zajamčena točne rezultate. Međutim, kad dokument broji zaista visoka, nema učinka kazna, tako da koristiti tu mogućnost uz Oprez.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Navigacija fasete na temelju vrijednosti u rasponu

Faceting preko raspona je Uobičajeni zahtjev aplikacije za pretraživanje. Rasponi podržani su za numeričke podatke i vrijednosti datuma i vremena. Dodatne informacije o svakom pristup u [Pretraživanje dokumenata (Azure pretraživanje API -JA)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Azure pretraživanje pojednostavljuje raspon izgradnju unosom na dva načina prikaza za računalstvo raspona. Za oba pristupa pretraživanje Azure stvara odgovarajuće raspona dali unose koji ste naveli. Na primjer, ako odredite raspon vrijednosti od 10 | 20 | 30, on će automatski stvoriti raspona 0 -10, 10 do 20, 20 – 30. Primjer aplikacije uklanja sve vremenskim razmacima koji su prazni. 

**Pristup 1: Koristite parametar interval**<br/>
Da biste postavili pozornici cijena u koracima $10, želite odrediti:`&facet=price,interval:10`


**Pristup 2: Korištenje popisa vrijednosti**<br/>
Za numeričke podatke, možete koristiti popis vrijednosti.  Razmislite o fasete raspon za listPrice, iscrtati na sljedeći način:

  ![][5]

Raspon je naveden u datoteci **CatalogSearch.cs** pomoću popisa vrijednosti:

    facet=listPrice,values:10|25|100|500|1000|2500

Svaki raspon ugrađena je korištenje 0 kao početnu točku vrijednost s popisa kao krajnje, a zatim se obrezuje prethodne raspona da biste stvorili samostalni vremenskim razmacima. Azure pretraživanja će to kao dio slojevito navigacije. Ne morate pisanje koda za strukturiranje svakom razdoblju.

### <a name="build-a-filter-for-facet-ranges"></a>Stvaranje filtra za raspone fasete ###

Da biste filtrirali dokumente koji se temelje na raspon koji je korisnik odabrao, možete koristiti u `"ge"` i `"lt"` filtriranje operatora u dva dijela izraz koji definira krajnje točke raspona. Na primjer, ako korisnik odabere raspon 10 do 25, filtar bio `$filter=listPrice ge 10 and listPrice lt 25`.

Izraz filtra u aplikaciji uzorka koristi parametri **priceFrom** i **priceTo** da biste postavili krajnjih točaka. Ta metoda **BuildFilter** u **CatalogSearch.cs** sadrži izraz za filtriranje koji vam dokumenata u rasponu.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>Filtrirani navigacije na temelju GeoPoints

Njegova zajednički da biste vidjeli filtrira kojih odaberite trgovine, restoran ili odredište na temelju njegove Blizina trenutno mjesto. Dok je ta vrsta filtar će izgledati kao slojevito navigacije, je zapravo filtar. Ovdje je smo spomenuti za one koji posebno tražite implementaciju savjete za taj problem određeni dizajna.

Postoje dvije funkcije geoprostornim Azure pretraživanja, **geo.distance** i **geo.intersects**.

- Funkcija **geo.distance** vraća udaljenost u kilometrima između dvije točke, jednog polja i nešto se konstante predugački kao dio filtra. 

- Funkcija **geo.intersects** vraća true ako je navedeni točka unutar zadanog poligona, gdje je polje i u poligona je navedena kao konstante popis koordinate proslijeđena kao dio filtra.

Primjeri filtar možete pronaći u [sintaksi izraza OData (Azure pretraživanje)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Isprobajte sami

Azure pretraživanje Adventure Works pokazni videozapis na Codeplex sadrži primjere navedeni u ovom članku. Kada radite s rezultatima pretraživanja, pogledajte URL-a za promjene u izgradnju upita. Da biste dodali pozornici URI dok odabirete svaku od njih će se dogoditi ove aplikacije.

1.  Konfiguriranje primjer aplikacije da biste koristili URL servisa i ključ za API-ja. 

    Obratite pozornost na to sheme koja je definirana u datoteci Program.cs CatalogIndexer projekta. Navodi polja facetable za boju, listPrice, veličina, težina, naziv kategorije i modelName.  U oknu za navigaciju slojevito zapravo primjenjuju samo nekoliko te (boja, listPrice, naziv kategorije).

3.  Pokrenite aplikaciju. 

    Najprije, vidljiv je samo u okvir za pretraživanje. Kliknite gumb za pretraživanje da biste pristupili sve rezultate, odmah ili upišite pojam za pretraživanje.

    ![][7]
 
4.  Unesite traženi pojam, primjerice bicikl, a zatim kliknite **Traži**. Upit se izvršava brzo.
 
    Slojevito navigacijske strukture i vraćaju s rezultatima pretraživanja.  U URL-a, pozornici za boje, kategorija i cijena su null. U rezultatima pretraživanja slojevito navigacijske strukture sadrži za svaki fasete rezultat.

     ![][8]
 
5.  Kliknite boja, kategorija i cijena raspon. Pozornici su null na početni pretraživanja, ali kao što su preuzeli vrijednosti, rezultati pretraživanja obrezivanja stavki koji se više ne podudaraju. Obratite pozornost na to URI uzima promjene u upit.

    ![][9]
 
6.  Da biste očistili slojevito upit tako da možete isprobati različite upit ponašanjem, kliknite **AdventureWorks kataloga** pri vrhu stranice.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Sljedeći korak

Da biste Provjerite svoje znanje, možete dodati polje fasete za *modelName*. Indeks već postavljena za ovaj fasete tako da nema promjena u indeks su potrebni. No morat ćete Izmjena HTML uvrstiti novi fasete modela i dodajte polje fasete Graditelj upita.

Bolji uvid na dizajn načela slojevito navigacije, preporučujemo sljedeće veze:

- [Dizajniranje slojevito pretraživanja](http://www.uie.com/articles/faceted_search/)
- [Uzorci dizajna: Slojevito navigacije](http://alistapart.com/article/design-patterns-faceted-navigation)

Možete gledati i [Azure pretraživanje precizno Dive](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). Pri 45:25, postoji u pokazni videozapis o tome kako implementirati pozornici.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 