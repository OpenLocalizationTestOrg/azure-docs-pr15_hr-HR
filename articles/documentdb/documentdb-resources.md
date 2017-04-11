<properties 
    pageTitle="DocumentDB modela hijerarhijski resursa i koncepata | Microsoft Azure" 
    description="Saznajte više o DocumentDB na hijerarhijski model baze podataka, zbirke, korisnički definirane funkcije (UDF), dokumente, dozvola za upravljanje resursima i drugo."
    keywords="Hijerarhijski model, a zatim documentdb, azure, Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>Model DocumentDB hijerarhijski resursa i koncepti

Entiteti baze podataka koja upravlja DocumentDB se nazivaju i **resursa**. Logička URI jedinstveno identificirati svaki resurs. Možete raditi s resursima pomoću standardni glagoli HTTP, zaglavlja zahtjeva i odgovora i šifre statusa. 

Tako da pročitate članak ćete je moći odgovaraju na sljedeća pitanja:

- Što je model resursa DocumentDB korisnika?
- Koji su sistemski definirani resursi umjesto korisnički definirane resursa?
- Kako se adresa resursa?
- Kako raditi zbirke?
- Rad s pohranjenim procedurama, okidača i korisnički definirane funkcije (UDF-ove)?

## <a name="hierarchical-resource-model"></a>Hijerarhijski resursa modela
Kako ilustrira na sljedećem su dijagramu, hijerarhijski DocumentDB **resursa modela** sastoji se od skupa resursa u odjeljku račun baze podataka, svakog moguće adresirati putem logičke i stabilan URI. Skup resursa će se nazivaju i na **sažetak sadržaja** u ovom članku. 

>[AZURE.NOTE] DocumentDB nudi iznimno učinkovitog TCP protokol koji nije RESTful na njegov komunikacije model, koja je dostupan putem [klijenta za .NET SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx).

![Model DocumentDB hijerarhijski resursa][1]  
**Hijerarhijski resursa modela**   

Da biste započeli rad s resursima, morate [stvoriti račun baze podataka DocumentDB](documentdb-create-account.md) korištenja pretplate za Azure. Račun za baze podataka mogu se sastojati od skupa **baze podataka**, svaka sadrži više **zbirki**, od kojih svaka sadrže **pohranjene procedure, pokreće UDF-ove, dokumenti** i povezane **privitaka** (značajke pretpregleda). Baze podataka sadrži povezane **korisnika**, svaka s skup **dozvola** za pristup zbirkama, pohranjene procedure, okidača, UDF-ove, dokumenti ili privici. Dok baze podataka, korisnici, dozvole i zbirki su definirani u sustavu resursi poznati sheme, dokumenata i privici sadrže proizvoljne, korisnički definirane JSON sadržaj.  

|Resurs   |Opis
|-----------|-----------
|Račun baze podataka   |Račun za baze podataka povezan je s skup baze podataka i fiksni iznos spremište blobova platforme za privitke (značajke pretpregleda). Stvorite jedan ili više računa baze podataka pomoću Azure pretplatu. Dodatne informacije potražite u našem [cijene stranice](https://azure.microsoft.com/pricing/details/documentdb/).
|Baze podataka   |Baza podataka nije logičke spremnik za pohranu dokumenata particije u zbirkama. Preporučuje se i spremniku korisnika.
|Korisnik   |Logička prostor naziva za određivanje opsega dozvola. 
|Dozvole |Oznaku ovlaštenja pridružene korisnika za pristup resursu određene.
|Zbirka |Zbirka je spremnik JSON dokumenata i pridruženi logike aplikacije JavaScript. Zbirka je naplatu entitet, gdje [trošak](documentdb-performance-levels.md) određuju razinu performanse pridružene zbirke. Zbirke mogu obuhvaćati jedan ili više particija/poslužitelja, a mogu mijenjati veličinu učiniti gotovo neograničeno količine prostora za pohranu ili propusnost.
|Pohranjena procedura   |Aplikacija logika u JavaScript koji je registriran u zbirci web i putem transakcije izvršava unutar modul baze podataka.
|Pokretanje    |Aplikacija logika u JavaScript izvršava ispred ili iza ili umetnut, zamjena ili operacija brisanja.
|UDF    |Aplikacija logika u JavaScript. Korisnički definiranih funkcija omogućuju modela operator prilagođene upita i time proširivanje core DocumentDB jezik upita.
|Dokument   |Korisnički definirane (proizvoljne) JSON sadržaj. Prema zadanim postavkama nema sheme potrebno definirati niti sekundarne indeksi potrebno koristiti za sve dokumente koje su dodane u zbirku.
|(Pretpregled) Privitak   |Privitak je posebno dokument koji sadrži reference i pridruženi metapodataka za vanjske blob/medijske. Programer možete odabrati da biste mogli blob-om upravlja DocumentDB ili spremiti kod davatelja usluga vanjskih blob kao što je OneDrive, Dropbox, itd. 


## <a name="system-vs-user-defined-resources"></a>Sustav nasuprot korisnički definirane resursi
Resurse kao što su računi baze podataka, baza podataka, zbirke, korisnici, dozvole, pohranjene procedure, okidača i UDF - imaju fixed sheme i nazivaju sistemske resurse. Nasuprot tome, resurse kao što su dokumenti i privitaka bez ograničenja na imate shemi i nekoliko primjera korisnički definirane resursi. U DocumentDB, sustav i korisnički definirane resurse su predstavljene i upravljati kao standardnu usklađen JSON. Svi resursi sustava ili korisnički definirane, imate sljedeće zajednička svojstva.

> [AZURE.NOTE] Napomena sve sustava generira svojstva u resursu su mjestu podvlakom (_) u njihov prikaz JSON.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Svojstvo</strong></p></td>
            <td valign="top"><p><strong>Korisnik je moguće postaviti ili generira sustav?</strong></p></td>
            <td valign="top"><p><strong>Svrha</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Generira sustav</p></td>
            <td valign="top"><p>Ovi koje generira sustav, jedinstven i hijerarhijski identifikator resursa</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Generira sustav</p></td>
            <td valign="top"><p>e-oznake potrebne za kontrolu optimistična Procjena istodobnosti resursa</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Generira sustav</p></td>
            <td valign="top"><p>Zadnji ažurirane vremenske oznake resursa</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_isto</p></td>
            <td valign="top"><p>Generira sustav</p></td>
            <td valign="top"><p>Jedinstveni moguće adresirati URI resursa</p></td>
        </tr>
        <tr>
            <td valign="top"><p>ID-a</p></td>
            <td valign="top"><p>Generira sustav</p></td>
            <td valign="top"><p>Korisnički definirane jedinstveni naziv resursa (s istom particija vrijednost ključa). Ako korisnik odredite ID-ove, id se generira sustav</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Predstavljanje žičani resursa
DocumentDB mandate vlasničkih proširenja na JSON standardno ili posebne kodiranja; funkcionira s standardnim dokumentima JSON usklađen.  
 
### <a name="addressing-a-resource"></a>Adresiranja resursa
Resursi za sve su URI moguće adresirati. Vrijednost svojstva **_isto** resursa predstavlja relativni URI resursa. Oblikovanje URI sastoji se od na /\<sažetka sadržaja\>/ {_rid} odsječaka puta:  

|Vrijednost na _isto |Opis
|-------------------|-----------
|/DBS   |Sažetak sadržaja baze podataka u odjeljku račun baze podataka
|/DBS/ {dbName}  |Baze podataka pomoću ID-a koji se podudaraju vrijednost {dbName}
|/colls/ /DBS/ {dbName}   |Sažetak sadržaja zbirki u odjeljku baze podataka
|/colls/ /DBS/ {dbName} {collName} |Zbirka s ID-a koji se podudaraju vrijednost {collName}
|/colls/ /DBS/ {dbName} {collName} / dokumenti    |Sažetak sadržaja dokumenata u odjeljku zbirke
|/docs/ /colls/ {collName} /DBS/ {dbName} {docId}    |Dokument s ID-a koji se podudaraju vrijednost {doc}
|/users/ /DBS/ {dbName}   |Sažetak sadržaja korisnika u odjeljku baze podataka
|/users/ /DBS/ {dbName} {korisnički ID}   |Korisnik s ID-a koji se podudaraju vrijednost {korisnika}
|/users/ /DBS/ {dbName} {korisnički ID} / dozvole   |Sažetak sadržaja dozvole u odjeljku korisnika
|/permissions/ /DBS/ {dbName} /users/ {korisnički ID} {permissionId}    |Dozvola s ID-om koji se podudaraju vrijednost {dozvola}
  
Svaki resurs ima jedinstveni korisnički definirani naziv izložen putem svojstvo ID-a. Napomena: za dokumente, ako korisnik odredite id, naš podržani SDK-ovi će automatski stvoriti jedinstveni id dokumenta. Id je niz korisnički definirane od 256 znakova koji je jedinstveni u kontekstu određene nadređenog resursa. 

Svaki resurs ima na generira sustav hijerarhijski identifikator resursa (naziva se RID), koji je dostupan putem svojstvo _rid. Na RID kodira cijelu hijerarhiju navedene resurse te je praktičan interni prikaz za nametanje referencijalnog integriteta raspodijeljeno način. Na RID je jedinstven unutar račun baze podataka i ga interno koristi DocumentDB za učinkovito usmjeravanje bez unakrsni particija pretraživanja. Vrijednosti u _isto i svojstva _rid su zamjenska i Kanonski reprezentacije resurs. 

DocumentDB REST API-ji podržava adresiranja resursa i usmjeravanje zahtjeva za id i _rid svojstva.

## <a name="database-accounts"></a>Računi za baze podataka
Možete Dodjela jedan ili više DocumentDB baze podataka računa korištenja pretplate za Azure.

Možete [stvoriti i upravljanje računima DocumentDB baze podataka](documentdb-create-account.md) putem portala Azure na [http://portal.azure.com/](https://portal.azure.com/). Stvaranje i upravljanje računom bazu podataka morate imati Administrativni pristup te se izvoditi samo u odjeljku pretplate Azure. 

### <a name="database-account-properties"></a>Svojstva računa baze podataka
Kao dio dodjele resursa i upravljanje računom baze podataka možete konfigurirati i čitati sljedeća svojstva:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Naziv svojstva</strong></p></td>
            <td valign="top"><p><strong>Opis</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Dosljednost pravila</p></td>
            <td valign="top"><p>Postavljanje tog svojstva da biste konfigurirali Zadana razina dosljednost za sve zbirke na vašem računu baze podataka. Možete nadjačati razinu dosljednost na temelju zahtjeva za po pomoću zaglavlja zahtjev za [x ms-dosljednost-razinom]. <p><p>Imajte na umu da to svojstvo primjenjuje samo na <i>korisnički definirane resursi</i>. Sve sustava definirani resursi konfigurirani tako da podržava čitanja/upiti s istaknuti dosljednost.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Tipke za provjeru autentičnosti</p></td>
            <td valign="top"><p>To su primarnih i sekundarnih matrice i samo za čitanje tipke koje omogućuju Administrativni pristup svim resursa u odjeljku račun baze podataka.</p></td>
        </tr>
    </tbody>
</table>

Imajte na umu da osim dodjele resursa, konfiguriranje i upravljanje računom baze podataka na portalu Azure, programski također možete stvoriti i upravljanje računima DocumentDB baze podataka pomoću [Azure DocumentDB REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx) , kao i [klijent SDK-ovi](https://msdn.microsoft.com/library/azure/dn781482.aspx).  

## <a name="databases"></a>Baza podataka
DocumentDB baze podataka je logički spremnik jednu ili više zbirki i korisnika, kao što je prikazano na sljedećem su dijagramu. Možete stvoriti bilo koji broj baza podataka na računu za bazu podataka DocumentDB odnose ograničenja ponudu.  

![Model baze podataka računa i zbirki hijerarhijski][2]  
**Baza podataka nije logičke spremnik korisnika i zbirki**

Baze podataka može sadržavati gotovo neograničeno spremišta particije po zbirke, obrasca transakcije domenama dokumenata koji se nalaze u njima. 

### <a name="elastic-scale-of-a-documentdb-database"></a>Elastic skale DocumentDB baze podataka
Baza podataka za DocumentDB je elastic po zadanom – od nekoliko GB do petabytes spremišta SSD sigurnosno i dodijeljenu propusnost. 

Za razliku od baza podataka u tradicionalni RDBMS, baze podataka u DocumentDB ne implementaciju ograničen je na jednom računalu. S DocumentDB, kao što je mora skaliranje vaše aplikacije radi povećanja, možete stvoriti više zbirki, baze podataka ili oboje. Uistinu, razne prvi proizvođača aplikacije unutar Microsoft ste koristili DocumentDB pri skalu potrošača stvaranjem iznimno velikim DocumentDB bazama podataka svaki koji sadrže tisuće zbirke s terabajta prostora za pohranu dokumenata. Možete Povećaj ili Smanji baze podataka dodavanjem ili uklanjanjem zbirki prema zadovoljava preduvjete skaliranje vaše aplikacije. 

Možete stvoriti bilo koji broj zbirki baze podataka primjenjuju ponudu. Svakoj zbirci ima SSD sigurnosno prostora za pohranu i propusnost dodijeljena ovisno o sloju odabrane performansi.

Bazu podataka DocumentDB je spremnik korisnika. Korisnik u – uključivanje, je logički prostor naziva za skup dozvola koji omogućuje preciznije autorizacije i pristupa zbirke, dokumente i privici.  
 
Kao i kod drugih resursa u modelu resursa DocumentDB baze podataka mogu stvoriti, zamijeniti, izbrisane, pročitajte ili videouređaja pomoću [Azure DocumentDB REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx) ili bilo koji od [klijenta SDK-ovi](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB jamčiti istaknuti dosljednost za čitanje i slanje upita metapodataka resursa za bazu podataka. Automatsko brisanje baze podataka osigurava da neki od zbirke ili korisnike koji se nalaze u njoj ne mogu pristupiti.   

## <a name="collections"></a>Zbirke
Zbirka DocumentDB je spremnik za dokumente JSON. Zbirka je jedinica mjerilo za transakcije i upitima. 

### <a name="elastic-ssd-backed-document-storage"></a>Elastic SSD sigurnosno spremišta
Zbirka je intrinsically elastic – automatski rastom i skupit će se kao što je dodavanje ili uklanjanje dokumenata. Zbirke logičke resursa i mogu obuhvaćati fizičke particije ili poslužiteljima. Broj particija unutar zbirke ovisi o DocumentDB na temelju veličine prostora za pohranu i dodijeljenu propusnost zbirke. Svaki particije u DocumentDB sadrži fiksni iznos SSD sigurnosno prostora za pohranu pridružen pa je replicirati za visoke dostupnosti. Upravljanje particija potpuno upravlja Azure DocumentDB, a morate složene kod ili upravljali vaše particije. DocumentDB zbirke su **gotovo neograničeno** prostora za pohranu i propusnost. 

### <a name="automatic-indexing-of-collections"></a>Automatsko indeksiranje zbirki
DocumentDB je true baze podataka sheme slobodno sustav. Ga ne pretpostavlja ili obavezan sve sheme za dokumente JSON. Dodavanje dokumenata u zbirci, DocumentDB automatski indeksi ih i su dostupni za upit. Automatsko indeksiranje dokumenata bez shemu ili sekundarne je ključa mogućnost DocumentDB web-mjesta i omogućena je prema tehnike održavanja Optimizirano za pisanje, bez zaključavanja i zapisnika strukturiranim indeksa. DocumentDB podržava osigurale trajne količinu iznimno brzo zapisivanje dok još uvijek posluživanje dosljedan upita. Prostor za pohranu dokumenata i indeks se koriste za izračun prostora za pohranu koji svake zbirke. Možete kontrolirati na prostora za pohranu i performanse gubitke pridružene indeksiranje konfiguriranjem indeksiranja pravila za zbirku. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Konfiguracija indeksiranja pravila zbirke
Indeksiranje Pravilnik za svaku zbirku omogućuje da biste performanse i pohranu gubitke pridružene indeksiranja. Dostupne kao dio Konfiguracija indeksiranja su sljedeće mogućnosti:  

-   Odaberite hoće li zbirke automatski indeksi sve dokumente ili ne. Prema zadanim postavkama, svi dokumenti se automatski indeksirati. Možete odabrati da biste isključili automatsko indeksiranje i selektivno dodati samo određene dokumente u indeks. Nasuprot tome, možete selektivno odabrati da biste izuzeli određene dokumente. To možete postići postavite svojstvo automatski biti true ili false na indexingPolicy zbirke i korištenjem zaglavlje zahtjev za [x-ms-indexingdirective] prilikom umetanja, zamjene ili brisanje dokumenta.  
-   Odaberite želite li uključiti ili isključiti određenih putova ili uzoraka u dokumentima iz indeksa. To možete postići tako da postavku includedPaths i excludedPaths na indexingPolicy zbirke odnosno. Možete konfigurirati i u prostor za pohranu i performanse gubitke raspon i raspršivanje upita za određeni put uzoraka. 
-   Odabir između sinkrono (dosljedan) i ažuriranja asinkronog indeksa (drži). Po zadanom je ažuriranje indeksa sinkronizirano na svakom Umetanje, zamjena ili Izbriši dokument u zbirku. Time se omogućuje upite za proslavu iste razine dosljednost kao čitanja dokumenta. Dok DocumentDB pisanja Optimizirano te podržava osigurale trajne količine zapisivanja dokumenta uz održavanje sinkrono indeksa i posluživanje dosljedan upite, možete konfigurirati određene zbirke lazily ažuriranje njihove indeksa. Drži indeksiranje pojačava performanse pisanje dodatno i idealna je za skupno ingestion namjene primarno zbirki čitanje podebljano.

Pravilnik za indeksiranje mogu se promijeniti izvršavanjem na STAVI u zbirci. To se može postići ili putem [klijenta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx), [Azure Portal](https://portal.azure.com) ili [Azure DocumentDB REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### <a name="querying-a-collection"></a>Slanje upita zbirka
Dokumente unutar zbirke možete imati proizvoljne sheme i upit možete poslati dokumenata u zbirci bez osiguravanja sheme ni upfront sekundarne indeksi. Upit možete poslati zbirke pomoću [DocumentDB SQL sintaksa](https://msdn.microsoft.com/library/azure/dn782250.aspx), koji pruža obogaćeni hijerarhijski, relacijskih i prostorno operatori i proširivanje putem utemeljen na JavaScript UDF-ove. Omogućuje JSON gramatike za Modeliranje JSON dokumente kao stabala s natpisima kao čvorove stabla. To je exploited i DocumentDB na automatsko indeksiranje tehnike, kao i DocumentDB na SQL dijalektu. Jezik upita DocumentDB sastoji se od tri glavna aspekata:   

1.  Mali skup operacija upita koji mapiranje prirodan strukturu stabla uključujući hijerarhijski upita i projekcija. 
2.  Podskup relacijski operacije uključujući slaganja, filtar, projekcija, zbrajanja i prošao na samostalnom spojeva. 
3.  Čisto JavaScript na temelju UDF-ovi koji rade s (1) i (2).  

Model upita DocumentDB pokušava pak postizanje ravnoteže između funkcija, učinkovitost i jednostavnosti. Modul za baze podataka DocumentDB nativno kompilira i izvršava SQL naredbe upita. Upit možete poslati zbirke pomoću [Azure DocumentDB REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx) ili bilo koji od [klijenta SDK-ovi](https://msdn.microsoft.com/library/azure/dn781482.aspx). U sklopu .NET SDK davatelja LINQ.

> [AZURE.TIP] Možete isprobati DocumentDB i izvoditi SQL upite na našem skup podataka u [Playground upita](https://www.documentdb.com/sql/demo).

### <a name="multi-document-transactions"></a>Transakcije više dokumenata
Transakcije baze podataka navedite sigurnih i predvidljivi model programiranja za istovremene promjene podataka. U RDBMS, tradicionalni načina pisanja poslovne logike je pisanje **pohranjene procedure** i/ili **okidača** i isporuka s poslužiteljem baze podataka za izvršavanje transakcijskih. U RDBMS, programerskom aplikacije potreban je radnju s dva raznovrsne programskog jezika: 

- (Nije-transakcijskih) aplikacije programskom jeziku (npr. JavaScript, Python, C#, Java itd.)
- T-SQL, transakcijskih programski jezik koji ima nativnu izvršio baze podataka

By virtue of njegov precizno izvršenja JavaScript i JSON izravno u modul baze podataka, DocumentDB nudi intuitivno model programiranja za izvođenje logike aplikacije JavaScript temelji izravno na zbirkama pohranjene procedure i okidača. Time za oboje od sljedećeg:

- Učinkovito implementaciju istodobnosti kontrolu, oporavak, automatsko indeksiranje objekt grafikona JSON izravno u modul baze podataka
- Prirodno kojem se iznosi ono tijek kontrola, varijable određivanje opsega, dodjelu i integraciju obrade primitives s transakcije baze podataka izravno pomoću programskom jeziku JavaScript iznimki

Logika JavaScript registrirana na razini zbirke možete zatim izdati postupaka baze podataka na dokumentima određene zbirke. DocumentDB implicitno prelama JavaScript temelji pohranjene procedure i okidača unutar razine okolne ACID transakcija s odvajanja snimke preko dokumenata u zbirci. Tijekom njegova izvođenja, ako JavaScript throws iznimku, zatim cijelu transakcija je prekinuta. Rezultat model programiranja je vrlo jednostavne još Napredna. Razvojni inženjeri JavaScript se "durable" model programiranja dok još uvijek koriste njihove konstrukta poznatih jezika i primitives biblioteke.   

Mogućnost dopušta izvršavanje JavaScripta izravno u modul baze podataka u isti prostor za adresu kao skupinu međuspremnika omogućuje performant i transakcijskih izvršavanje operacija baze podataka na temelju dokumenata zbirke. Osim toga, modul baze podataka programa DocumentDB čini precizno izvršenja da biste na JSON i JavaScript uklanja sve impedance Neslaganje sustavi za vrstu aplikacije i baze podataka.   

Nakon stvaranja zbirke registrirati pohranjene procedure, okidača i UDF-ove sa zbirkom pomoću [Azure DocumentDB REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx) ili bilo koji od [klijenta SDK-ovi](https://msdn.microsoft.com/library/azure/dn781482.aspx). Nakon registracije, možete referencirati i izvršiti ih. Razmislite o sljedećim pohranjena procedura napisali u potpunosti JavaScript, ispod kod traži dva argumenta (adresara ime i ime autora) i stvara novi dokument, upiti za dokument i ažurira je – sve unutar implicitno ACID transakcije. U bilo kojem trenutku tijekom izvršavanja ako je JavaScript nije izbačena, cijeli transakcije prekida.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Klijent možete "Isporuka" iznad logika JavaScript bazu podataka za transakcijskih izvršavanje putem HTTP POST. Dodatne informacije o korištenju HTTP metode potražite u članku [RESTful interakcije s resursima DocumentDB](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Obratite pozornost na to jer bazu podataka možete koristiti kao takav JSON i JavaScript, ima li bez Nepodudarnost sustava, bez "Ili mapiranje" ili kod generacije poseban obavezno.   

Pohranjene procedure i okidača raditi s zbirka i dokumenata u zbirci kroz dobro definiranom objektni model, koji se izlaže trenutni kontekst zbirke.  

Zbirke u DocumentDB moguće stvoriti, izbrisane, pročitajte ili videouređaja jednostavno pomoću [Azure DocumentDB REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx) ili bilo koji od [klijenta SDK-ovi](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB uvijek sadrži istaknuti dosljednost za čitanje i slanje upita metapodataka zbirke. Automatsko brisanje zbirke osigurava da ne može pristupati svim dokumentima, privitke, pohranjene procedure, okidača i korisnički definiranih funkcija koje se nalaze u njoj.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Pohranjene procedure, okidača i korisnički definirane funkcije (UDF)
Kao što je opisano u prethodnom odjeljku možete napisati logika aplikacije da biste pokrenuli izravno unutar transakcije unutar modul baze podataka. Logika aplikaciju možete staviti u potpunosti JavaScript, a možete katalog modeliran kao pohranjena procedura, pokretanje ili na UDF. JavaScript kod unutar pohranjena procedura ili okidač možete umetnuti, zamijenite, brisanje, čitanje ili upit dokumenata u zbirci. JavaScript unutar na UDF nije moguće, s druge strane, umetanje, zamjena ili brisanje dokumenata. Korisnički definiranih funkcija Nabrajanje dokumenata skup rezultata upita i dobivanje drugi skup rezultata. Za više tenancy DocumentDB nameće upravljanja za resursa izričite rezervacija temelje. Svaki pohranjena procedura, pokretanje ili na UDF dobiva fixed quantum operacijski sustav resursa za rad. Osim toga, pohranjene procedure, okidača ili korisnički definiranih funkcija ne može se povezati s vanjskim JavaScript biblioteke i blacklisted ako premaše proračuna resursa dodijeliti ih. Možete zabilježiti, pomoću REST API-ji s zbirka unregister pohranjene procedure, okidača ili UDF-ove.  Nakon registracije pohranjena procedura, pokretanje ili na UDF se unaprijed prikupljaju i pohranjenih kao bajt kod koji se izvršiti poslije. U sljedećem odjeljku prikazuju kako koristiti DocumentDB JavaScript SDK da biste registrirali, izvršavanje i unregister pohranjena procedura, pokretanje i na UDF. JavaScript SDK je jednostavno omot putem [DocumentDB REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

### <a name="registering-a-stored-procedure"></a>Registracija pohranjena procedura
Registracija pohranjena procedura stvara novi resurs pohranjena procedura zbirke putem HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Izvršavanja pohranjene procedure
Izvršavanje pohranjena procedura se postiže izdavanja HTTP POST na temelju postojeće resursa za pohranjena procedura prosljeđivanjem parametara postupak u tijelu zahtjeva.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Poništavanje registriranja pohranjena procedura
Poništavanje registriranja pohranjena procedura jednostavno obavlja tvrtka izdavanja HTTP brisanje na temelju postojeće resursa za pohranjena procedura.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registracija stara okidača
Registracija okidač obavlja tako da stvorite novi resurs okidača zbirci putem HTTP POST. Možete navesti ako okidača je na pre ili okidač objavu i vrstu operacije mogu se pridružiti (npr. Stvori, Zamijeni, brisanje ili sve).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Izvođenje stara okidača
Izvršavanje okidača obavlja navođenjem naziv postojeće okidača u trenutku izdavanja zahtjev za objavu/STAVI/brisanje resursa dokumenta putem zaglavlje zahtjev.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Poništavanje registriranja stara okidača
Poništavanje registriranja okidač jednostavno obavlja putem izdavanja HTTP brisanje na temelju postojeće resursa za pokretanje.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Prijava na UDF
Registracija na UDF obavlja tako da stvorite novi resurs UDF zbirci putem HTTP POST.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Izvođenje UDF kao dio upit
Na UDF možete navesti kao dio SQL upita i koristi se kao način da biste proširili core [SQL upita jezik DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Poništavanje registriranja na UDF 
Poništavanje registriranja na UDF jednostavno obavlja tvrtka izdavanja HTTP brisanje na temelju postojeće resursa za UDF.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Iako isječci iznad prikazivao Registracija (objavu), registracije (STAVI), Čitaj/popis (DOHVATI) i izvršavanja (objavu) putem [DocumentDB JavaScript SDK](https://github.com/Azure/azure-documentdb-js), možete koristiti [REST API -ji](https://msdn.microsoft.com/library/azure/dn781481.aspx) ili drugi [klijent SDK-ovi](https://msdn.microsoft.com/library/azure/dn781482.aspx). 

## <a name="documents"></a>Dokumenti
Možete umetnuti, zamijeniti, brisanje, čitanje, numeriranje i upit proizvoljne JSON dokumenata u zbirci. DocumentDB mandate sve sheme i potrebni sekundarne da bi se podržava upita putem dokumenata u zbirci.   

Se uistinu otvorene baze podataka usluge DocumentDB ne iznaći vrste specijalizirane podataka (primjerice datum vrijeme) ili određene kodiranja JSON dokumenata. Imajte na umu da DocumentDB zahtijeva sve posebno JSON konvencije codify odnose između različitih dokumenata SQL sintaksa DocumentDB sadrži vrlo Napredna hijerarhijski i relacijskih upita operatori za upit i project dokumenata bez posebne napomene ili treba codify odnose između dokumenata pomoću razlikuju svojstva.  
 
Kao uz sve ostale resurse dokumente možete stvoriti, zamijenjeni, izbrisane, čitanje, numerirana i traženih pomoću REST API-ji ili bilo koji od [SDK-ovi klijenta](https://msdn.microsoft.com/library/azure/dn781482.aspx). Brisanje dokumenta trenutačno oslobađa kvote odgovara svih ugniježđenih privitaka. Razine čitanje dosljednost dokumenata slijedi pravila dosljednost na računu za bazu podataka. Moguće poništiti ovo pravilo na temelju-zahtjev ovisno o potrebama dosljednost podataka aplikacije. Kada ispitivanje dokumenata, čitanje dosljednost slijedi indeksiranja način postavljen u zbirci. "Dosljedan", to slijedi pravila dosljednost na račun. 

## <a name="attachments-and-media"></a>Privici i medijskih sadržaja
>[AZURE.NOTE] Privitak i medijskih resursa su značajke pretpregleda.
 
DocumentDB vam omogućuje pohranu binarni blob-ova i medijske sadržaje s DocumentDB ili na udaljenim medijskim spremište. Omogućuje vam predstavljaju metapodataka medijskih sadržaja pomoću posebnih dokument koji se zove privitak. Privitak u DocumentDB je posebno dokument (JSON) koja se odnosi media/blob pohranjena negdje drugdje. Privitak je jednostavno posebno dokument koji opisuje metapodataka (npr. mjesto, autor itd.) medija pohranjuju u udaljenim medijskim prostor za pohranu. 

Razmislite o društvenim čitanje aplikacije koja koristi DocumentDB za pohranu rukopisne primjedbe i metapodataka Komentari, uključujući ističe, knjižne oznake, ocjene, oznaka sviđanja/tome što itd pridruženi za e-dnevnik navedeni korisnika.   

-   Sadržaj adresara sam pohranjena u prostor za pohranu medijskih sadržaja ili dostupni kao dio račun DocumentDB baze podataka ili u trgovini udaljene medijske sadržaje. 
-   Aplikacije mogu pohranjivati metapodatke za svakog korisnika kao različita dokumenta--npr. Joe poštanskog metapodaci za radnoj knjizi Knjiga1 spremaju se u dokumentu na koje referencira /colls/joe/docs/book1. 
-   Privici koja pokazuje na stranice sadržaja navedene knjige korisnika spremaju se u odjeljku odgovarajući dokument npr /colls/joe/docs/book1/chapter1 /colls/joe/docs/book1/chapter2 itd. 

Imajte na umu da Primjeri naveden koriste neslužbeni ID-a da biste prenijeli hijerarhiji resursa. Resursi se pristupa putem REST API-ji kroz resursa jedinstveni ID-a. 

Za medijske sadržaje koje je upravlja DocumentDB, svojstvo _media privitka pozivati medijskih sadržaja tako da njegov URI. DocumentDB će biti sigurni da biste smeća prikupljanje medijskih sadržaja kada sva preostala reference ispuštaju. DocumentDB automatski generira privitak prilikom učitavanja novi medij i popunjava _media tako da pokazuje na novododani medijske sadržaje. Ako se odlučite za pohranu medijskih sadržaja u spremištu udaljene blob upravlja vam (npr. OneDrive, prostor za pohranu Azure, DropBox itd), i dalje možete koristiti privitke referentni medijske sadržaje. U ovom slučaju će stvoriti privitak i popunjavanje njegovo svojstvo _media.   

Kao i kod svih drugih resursa, privitke moguće stvoriti, zamijeniti, izbrisane, pročitajte ili označuju pomoću REST API-ji ili bilo koji od klijenta SDK-ovi. Kao i kod dokumente, razini čitanja dosljednost privitaka slijedi pravila dosljednost na računu za bazu podataka. Moguće poništiti ovo pravilo na temelju-zahtjev ovisno o potrebama dosljednost podataka aplikacije. Prilikom postavljanja upita za privitke, pročitajte dosljednost slijedi indeksiranja način postavljen u zbirci. "Dosljedan", to slijedi pravila dosljednost na račun. 
 
## <a name="users"></a>Korisnici
Korisnik DocumentDB predstavlja logičke prostor naziva za grupiranje dozvole. Korisnik DocumentDB odgovarati korisniku web-mjesto u sustavu za upravljanje identiteta ili u okvir za uloge unaprijed definirane aplikacije. Za DocumentDB, korisnik jednostavno predstavlja programa apstrakcije da biste grupirali skup dozvola u odjeljku baze podataka.   

Za implementaciju više tenancy u aplikaciji, možete stvoriti korisnika u DocumentDB koji odgovara stvarnim korisnicima ili klijenata za aplikacije. Zatim možete stvoriti dozvole za dani korisnika koji odgovaraju kontrola pristupa preko različitih zbirke dokumenata, privitke, itd.   

Aplikacija je potrebno da biste skalirali s vaš korisnički rasta, možete je prihvaćaju razni načini shard podataka. Možete svakog korisnika modela na sljedeći način:   

-   Svaki korisnik karata u bazu podataka.
-   Svaki korisnik karata u zbirku. 
-   Dokumenti koje odgovaraju većem broju korisnika otvorite namjenski zbirke. 
-   Dokumente koje odgovaraju većem broju korisnika idite na postavljanje zbirki.   

Bez obzira na određene sharding strategije odaberete, model stvarnim korisnicima kao korisnike u bazi podataka DocumentDB i pridruživanje precizno grained dozvole za svakog korisnika.  

![Zbirke korisnika][3]  
**Strategije sharding i Modeliranje korisnika**

Kao što su svi ostali resursi korisnika u DocumentDB moguće stvoriti, zamijeniti, izbrisane, pročitajte ili videouređaja pomoću REST API-ji ili bilo koji od klijenta SDK-ovi. DocumentDB uvijek sadrži istaknuti dosljednost za čitanje i slanje upita metapodataka korisniku resursa. To vrijedi ističu da automatski brisanjem korisnika jamči da neki od dozvole koje se nalaze u njoj ne mogu pristupiti. Iako u DocumentDB reclaims kvote dozvole kao dio izbrisanim korisničkim u pozadini, izbrisane dozvole dostupna trenutačno ponovno za korištenje.  

## <a name="permissions"></a>Dozvole
Iz programa access kontrola perspektive, resurse kao što su baze podataka računa, baze podataka, korisnici i dozvole smatraju *administratora* resurse jer se one zahtijevaju administratorske dozvole. S druge strane, resurse uključujući zbirke, dokumente, privitke, pohranjene procedure, okidača i UDF-ovi su namijenjeni u odjeljku danu bazu podataka i smatra *resursima aplikacija*. Odgovara dvije vrste resursa i uloge koje pristup (odnosno administrator i korisnika), model autorizacije definira dvije vrste *pristupnih tipki*: *glavni ključ* i *ključ resursa*. Glavni ključ je dio račun baze podataka, a za razvojne inženjere (ili administrator) koji je dodjeljivanje račun baze podataka. Ovaj glavni ključ ima semantiku administrator po tome što se može koristiti da biste autorizirali pristup resursima administratora i aplikacije. Nasuprot tome, ključ resursa je zrnastog pristupna tipka koji omogućuje pristup resursu *određene* aplikacije. Dakle, pohranjuju odnos između korisnika baze podataka i dozvole korisnik ima određenog resursa (npr. zbirke, dokumenta, privitak, pohranjena procedura, okidača ili UDF).   

Jedini način da biste dobili ključ resursa je stvaranjem dozvola resursa u odjeljku zadani korisnika. Imajte na umu da biste stvorili ili dohvatiti dozvole, glavni ključ moraju biti navedeni u zaglavlju autorizacije. Resurs dozvola ties resursa, pristup i korisnika. Nakon stvaranja dozvola resursa, korisnik samo treba izlaganje tipku pridružene resurse da biste pristupili resurs. Dakle, ključ resursa mogu se prikazati kao logička i sažeti prikaz resursa dozvola.  

Kao i kod svih drugih resursa, dozvole u DocumentDB moguće stvoriti, zamijeniti, izbrisane, pročitajte ili videouređaja pomoću REST API-ji ili bilo koji od klijenta SDK-ovi. DocumentDB uvijek sadrži istaknuti dosljednost za čitanje i slanje upita metapodataka dozvole. 

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o radu s resursima pomoću naredbe HTTP u [RESTful interakcije s resursima DocumentDB](https://msdn.microsoft.com/library/azure/mt622086.aspx).


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

