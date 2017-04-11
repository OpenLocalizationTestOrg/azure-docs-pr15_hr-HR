<properties
   pageTitle="Vodič za stvaranje podatkovnog servisa za Marketplace | Microsoft Azure"
   description="Detaljne upute kako stvoriti, potvrđivanje i implementacija podatkovnog servisa za kupnju na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Objašnjenje shemi čvorove za mapiranje postojeće web-usluge OData kroz CSDL

>[AZURE.IMPORTANT] **Trenutno ne možemo više nisu za uhodavanje sve nove izdavači podatkovnog servisa. Novi dataservices će dobiti odobrene za unos.** Ako imate aplikaciju SaaS tvrtke koje želite objaviti na AppSource možete pronaći dodatne informacije [u nastavku](https://appsource.microsoft.com/partners). Ako imate programa IaaS aplikacije ili servisa za razvojne inženjere koje želite objaviti na Azure Marketplace možete pronaći dodatne informacije [u nastavku](https://azure.microsoft.com/marketplace/programs/certified/).

Ovaj dokument će pojasnili strukturu čvor za mapiranje OData protokol s CSDL. Važno je da Imajte na umu da je struktura čvor dobro oblikovan XML. Pa korijen, nadređenog i podređenog shema je primjenjivo prilikom dizajniranja OData mapiranje.

## <a name="ignored-elements"></a>Zanemareni elemenata
U nastavku su s više razine CSDL elemente (XML čvorove) koje su namjeravate koristiti tako da pozadinski trgovine Windows Azure tijekom uvoza metapodataka web-servisa. Mogu postojati ali će je zanemariti.

| Element | Opseg |
|----|----|
| Korištenje Element | Čvor, čvorove sub i svih atributa |
| Dokumentaciju elementu | Čvor, čvorove sub i svih atributa |
| ComplexType | Čvor, čvorove sub i svih atributa |
| Pridruživanje Element | Čvor, čvorove sub i svih atributa |
| Proširena svojstva | Čvor, čvorove sub i svih atributa |
| EntityContainer | Zanemaruju se samo u sljedećim atributima: *proširuje* i *AssociationSet* |
| Shema | Zanemaruju se samo u sljedećim atributima: *prostor naziva* |
| FunctionImport | Zanemaruju se samo u sljedećim atributima: *način* (Zadana vrijednost od ln pretpostavlja se da je) |
| Vrsta entiteta | Zanemaruju se samo sljedeće sub čvorove: *ključ* i *PropertyRef* |

Sljedeće opisuje promjene (dodali, a zanemaruje elementi) da biste različite CSDL XML čvorove detaljno.

## <a name="functionimport-node"></a>FunctionImport čvor
FunctionImport čvor predstavlja jednu URL (točka unosa) koji se izlaže usluge krajnji korisnik. Čvor dopušta koji opisuje kako je adresirana URL-a, koje parametara dostupnih za krajnji korisnik i kako parametara služe.

Detalje o čvor nalaze se u [ovdje] [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Slijede dodatne atribute (ili dodatke atribute) koje su vidljiva pomoću čvor FunctionImport:

**d:BaseUri** -
u URI predložak za OSTALE resurse predstavljeni Marketplace. Tržište koristi predložak za sastavljanje upite odabiranja REST web-servisa. Predložak URI sadrži rezervirana mjesta za parametre u obliku {parameterName}, pri čemu je parameterName parametar. Ex. apiVersion = {apiVersion}.
Parametri dopušteno prikazuju se kao URI parametara ili kao dio puta URI. U slučaju izgled na putu budu uvijek obavezno (nije moguće označiti kao koji). *Primjer:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Ime** - naziv uvezene funkcije.  Ne mogu biti jednaki druge definirani nazivi u na CSDL.  Ex. Naziv = "GetModelUsageFile"

**EntitySet** *(neobavezno)* – ako zbirka vrste entitet, funkcija vraća vrijednost **EntitySet** mora biti entitet postavljena na kojem pripada zbirke. U suprotnom **EntitySet** atributa ne smije koristiti. *Primjer:*`EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(Neobavezno)* – navodi vrstu elemenata vratio URI.  Koristite ovaj atribut ako se funkcija ne vraća vrijednost. Podržane vrste su:

 - **Zbirke (<Entity type name>)**: određuje skup definirani entitet vrste. Naziv je koje su prisutne u atribut naziv čvor Vrsta entiteta. Primjer je zbirka (WXC. HourlyResult).
 - **Raw (<mime type>)**: određuje neobrađenog dokument/blob koje se vraćaju korisnika. Primjer je Raw(image/jpeg) još primjera:

  - ReturnType="Raw(text/plain)"
  - ReturnType = "zbirke (hiperveze. DeleteAllUsageFilesEntity) "*

**d:Paging** - određuje kako REST resursa upravlja broja stranica. Vrijednosti parametara koriste unutar zakrivljene braches, primjerice stranice = {$page} & itemsperpage = {$size} dostupne su sljedeće mogućnosti:

- **Ništa:** dostupna nijedna broja stranica
- **Preskoči:** broja stranica izražen je putem logičke "Preskoči" i "Preuzimanje" (gore). Preskoči prelazi preko M elemente i preuzimanje vraća sljedeće elemente N. Vrijednost parametra: $skip
- **Poduzeti:** Preuzimanje vraća sljedeće elemente N. Vrijednost parametra: $take
- **PageSize:** broja stranica izražen je putem logičke stranice i veličine (stavki po stranici). Stranica predstavlja trenutnu stranicu koja je vraćena. Vrijednost parametra: $page
- **Veličina:** veličina predstavlja broj stavki u rezultatima za svaku stranicu. Vrijednost parametra: $size

**d:AllowedHttpMethods** *(Neobavezno)* – određuje koje glagolski rukuje OSTATKOM resursa. Osim toga, ograničava prihvaćenom glagolski određenu vrijednost.  Zadani = objavu.  *Primjer:* `d:AllowedHttpMethods="GET"` Dostupne su sljedeće mogućnosti:

- **Početak:** obično koristi za vraćanje podataka
- **Objave:** obično koristi za umetanje novih podataka
- **STAVITI:** obično se koristi da biste ažurirali podatke
- **Brisanje:** koristi se za brisanje podataka

Dodatni podređene čvorove (ne pokrivaju dokumentaciju CSDL) unutar čvor FunctionImport su:

**d:RequestBody** *(Neobavezno)* – tijelu zahtjeva se koristi da biste naznačili da zahtjev očekuje tijelo slati. Parametri može biti zadano u tijelu zahtjeva. Oni npr izražena u vitičaste zagrade {parameterName}. Parametara mapiraju se iz korisničkom unosu u tijelo prenose se na servis davatelja sadržaja. RequestBody element ima atribut s httpMethod naziv. Atribut omogućuje dvije vrijednosti:

- **Objave:** Koristi se ako je zahtjev HTTP POST
- **Početak:** Koristi se ako je zahtjev HTTP GET

    Primjer:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** i **d:Namespace** - čvor opisuju prostori koji su definirani u XML koji je vratio uvoz funkcija (URI krajnje točke). XML koji je vratio servis pozadinskog može sadržavati bilo koji broj prostora naziva razlikovati sadržaj koji je vratio. **Sve te se prostori naziva, ako se koristi u d:Map ili d:Match XPath upita moraju biti navedena.** Čvor d:Namespaces sadrži skup/popis d:Namespace čvorove. Svaku od njih prikazuje jednu prostor naziva koriste u odgovoru pozadinskog servisa. Slijede atribut čvor d:Namespace:

-   **d:Prefix:** Prefiks prostora za naziv, kao što se vidi u XML rezultatima vratio servisa, primjerice f:FirstName, f:LastName, gdje je f prefiks.
- **d:Uri:** Potpuna URI prostora za naziv korišteni u dokumentu rezultat. Predstavlja vrijednost prefiksa riješen tijekom rada.

**d:ErrorHandling** *(Neobavezno)* – čvor sadrži uvjete za obradu pogreške. Svaki od uvjeta se provjerava prema najvišoj rezultat koji je vratio sadržaja davatelja usluga. Ako se uvjet podudara predložene HTTP kod pogreške poruka o pogrešci se vraćaju u krajnji korisnik.

**d:ErrorHandling** *(Neobavezno)* i **d:Condition** *(neobavezno)* – čvor uvjet sadrži jedan uvjet prijavljene rezultat koji je vratio sadržaja davatelja usluga. Ovo su **potrebne** atribute:

- **d:Match:** Izraz XPath provjerava je li se za izlaz davatelj sadržaja XML navedene čvor/vrijednosti. XPath je pokrenuti izlazne i mora vratiti true ako je uvjet podudaranje ili false u suprotnom.
- **d:HttpStatusCode:** Šifra stanja HTTP koja bi trebala biti vraćena po trgovine u slučaju uvjet odgovara. Trgovina signalizes pogreške za korisnika putem HTTP kodovima stanja. Popis kodova stanja HTTP dostupne su na http://en.wikipedia.org/wiki/HTTP_status_code
- **d:ErrorMessage:** Poruka o pogrešci koje se vraćaju – uz kod stanja HTTP – krajnji korisnik. To mora biti u neslužbeni poruka o pogrešci koja ne sadrži sve tajne.

**d:Title** *(Neobavezno)* – omogućuje s opisom naslov funkciju. Vrijednost za naslov koji dolaze iz

- Neobavezni karte atribut (xpath) koji određuje gdje možete pronaći naslov u odgovoru vratio zahtjev za uslugu.
- – Ili – naslov određen kao vrijednost čvor.

**d:Rights** *(Neobavezno)* – prava (primjerice autorskih prava) povezan s funkcijom. Vrijednost za prava dolazi iz:

- Neobavezni karte atribut (xpath) koji određuje gdje možete pronaći prava u odgovoru vratio zahtjev za uslugu.
-   – Ili – prava određen kao vrijednost čvor.

**d:Description** *(Neobavezno)* – kratak opis funkcije. Vrijednost za opis dolazi iz

- Neobavezni karte atribut (xpath) koji određuje gdje možete pronaći opis u odgovoru vratio zahtjev za uslugu.
- – Ili – opis određen kao vrijednost čvor.

**d:EmitSelfLink** - *potražite u članku iznad primjer "FunctionImport za Javljena kroz vraćenih podataka"*

**d:EncodeParameterValue** - neobavezno proširenje OData

**d:QueryResourceCost** - neobavezno proširenje OData

**d:Map** - neobavezno proširenje OData

**d:Headers** - neobavezno proširenje OData

**d:Headers** - neobavezno proširenje OData

**d:Value** - neobavezno proširenje OData

**d:HttpStatusCode** - neobavezno proširenje OData

**d:ErrorMessage** - neobavezno proširenje OData

## <a name="parameter-node"></a>Parametarski čvor

Ovaj čvor predstavlja jedan parametar predstavljeni kao dio predloška URI / zahtjev tijelo koji je naveden u čvor FunctionImport.

Stranicu dokumenta vrlo koristan pojedinosti o čvor "Parametar Element" nalazi se na [ovdje](http://msdn.microsoft.com/library/ee473431.aspx) (pomoću **Drugih verzija** padajući popis da biste odabrali neku drugu verziju ako je potrebno da biste vidjeli dokumentaciju). *Primjer:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Atribut parametra | Potreban je | Vrijednost |
|----|----|----|
| Ime | Da | Naziv parametra. Velika i mala slova!  Velika/mala slova BaseUri. **Primjer:**`<Property Name="IsDormant" Type="Byte" />` |
| Vrsta | Da | Vrsta parametra. Ta vrijednost mora biti **EDMSimpleType** ili složene vrste unutar opsega modela. Dodatne informacije potražite u članku "6 parametar svojstvo podržane vrste".  (Velika i mala slova! Prvi znak je napisano velikim slovima, ostale malim slovima.)  Vidjeti, [konceptualni modela vrste (CSDL)] [MSDNParameterLink]. **Primjer:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Način rada | ne | **U**, ili InOut, ovisno o tome je li parametar za unos, izlazna ili ulazni i izlazni parametar. ("U" je dostupan samo u Azure Marketplace.) **Primjer:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | ne | Najveći dopušteni duljinu parametar. **Primjer:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Preciznost | ne | Preciznost parametar. **Primjer:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Promjena veličine | ne | Promjena veličine parametra. **Primjer:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Slijede atributa koji su dodani u specifikacija CSDL:

| Atribut parametra | Opis |
|----|----|
| **d:RegEx** *(Neobavezno)* | Regex izjava koji se koriste za provjeru valjanosti unos vrijednosti parametra. Ako se ulazne vrijednosti ne podudaraju u izjavi vrijednost je odbijena. Time da biste odredili i postavljanje mogućih vrijednosti, npr ^ [0 do 9] +? $ dopustiti pristup samo brojeve. **Primjer:** ' < naziv parametra = "naziv" način = "U" Vrsta = "Niz" d: koji = "false" d:Regex = "^ [a-zA-Z] * $" d:Description = d:SampleValues "na naziv koji se ne smije sadržavati razmake ni znakove koji nisu engleski koji nisu alfa" = "Goran|Marko|Pošta|James "/ >" |
| **d:ENUM** *(Neobavezno)* | Na kanal odvojene popis vrijednosti koje su valjane za parametar. Vrsta vrijednosti mora odgovarati definirani vrsti parametra. Primjer: "engleski|mjerenja|neobrađenog`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< naziv parametra = "Trajanje" Vrsta = "Niz" način = "U" Nullable = "true" d:Enum = "godinu dana.|5years|10years "/ >" |
| **d: koji** *(Neobavezno)* | Omogućuje definiranje li parametar može biti null. Zadana vrijednost je: true. Međutim, parametre koje su predstavljeni kao dio puta u predlošku URI ne smije biti null. Kada atribut postavljen na false za ove parametre – nadjačati je korisničkom unosu. **Primjer:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Neobavezno)* | Ogledne vrijednosti da biste prikazali kao bilješke klijenta u korisničkom Sučelju.  Nije moguće dodati nekoliko vrijednosti na temelju popisa kanala odvojene, odnosno "na|b|c` **Example:** `< naziv parametra = "BikeOwner" Vrsta = "Niz" način = "U" d:SampleValues = "Goran|Marko|Pošta|James "/ >" |

## <a name="entitytype-node"></a>Vrsta entiteta čvor

Ovaj čvor predstavlja neke vrste koje se vraćaju iz trgovine krajnjeg korisnika. Sadrži i mapiranje iz izlaza koje se vraćaju sadržaja davatelja usluga vrijednosti koje vraćaju krajnji korisnik.

Detalje o čvor nalaze se na [ovdje](http://msdn.microsoft.com/library/bb399206.aspx) (pomoću **Drugih verzija** padajući popis da biste odabrali neku drugu verziju ako je potrebno da biste vidjeli dokumentaciju.)

| Naziv atributa | Potreban je | Vrijednost |
|----|----|----|
| Ime | Da | Naziv vrste entitet. **Primjer:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| Osnovna vrsta | ne | Naziv neku drugu vrstu entitet osnovne vrste vrstu entitet koji je definiran. **Primjer:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Slijede atributa koji su dodani u specifikacija CSDL:

**d:Map** - programa XPath izraz koji se izvršava na temelju izlaz servisa. Ovdje pretpostavlja se da izlaz servisa sadrži skup elemenata koje se ponavljaju, kao što je ATOM sažetaka sadržaja gdje postoji skup čvorove stavke koje se ponavljaju. Svaki od ovih ponavljajuće čvorove sadrži jedan zapis. XPath zatim navesti tako da pokazuje na pojedinačne ponavljajuće čvor u rezultatu servisa davatelja sadržaja koja sadrži vrijednosti za pojedinačni zapis. Primjer Izlaz iz servisa:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

XPath izraz će biti /foo/trake jer svaki trake čvor je ponavljajućoj čvor u Izlaz i sadrži njegov sadržaj koji se vraćaju u krajnji korisnik.

**Ključ** - taj atribut bit će zanemarene po trgovine. POSTAVITE na temelju web-servisi, Općenito ne izlažu primarni ključ.


## <a name="property-node"></a>Svojstvo čvor

Ovaj čvor sadrži jedno svojstvo zapisa.

Detalje o čvor nalaze se na [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (pomoću **Drugih verzija** padajući popis da biste odabrali neku drugu verziju ako je potrebno da biste vidjeli dokumentaciju.) *Primjer:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Obavezno | Vrijednost |
|----|----|----|
| Ime | Da | Naziv svojstva. |
| Vrsta | Da | Vrsta vrijednosti svojstva. Svojstvo vrsta vrijednosti mora biti **EDMSimpleType** ili složene vrste (označenu puni naziv) koji se nalazi unutar djelokruga modela. Dodatne informacije potražite u članku vrste konceptualni Model (CSDL). |
| Bez vrijednosti | ne | **True** (Zadana vrijednost) ili **False** ovisno o tome je li svojstvo ima vrijednost null. Napomena: U verziji CSDL ikonom definiranog naziva [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) u složenoj svojstvo mora imati Nullable = "False". |
| DefaultValue | ne | Zadane vrijednosti svojstva. |
|MaxLength | ne | Maksimalna duljina za vrijednosti svojstva. |
| FixedLength | ne | **True** ili **False** ovisno o tome je li vrijednost svojstva spremit će se kao fiexed duljina niza. |
| Preciznost | ne | Odnosi se na najveći broj znamenki da biste zadržali u numeričku vrijednost. |
| Promjena veličine | ne | Maksimalan broj decimalnih mjesta da biste zadržali u numeričku vrijednost. |
| Unicode | ne | **True** ili **False** ovisno o tome hoće li biti svojstva vrijednost pohranjena kao niz Unicode. |
| Razvrstavanje | ne | Niz koji određuje slaganja koja će se koristiti u izvoru podataka. |
| ConcurrencyMode | ne | **Ništa** (Zadana vrijednost) ili **Fiksno**. Ako je vrijednost postavljena na **Fiksno**, svojstva vrijednost će se koristiti u optimistična Procjena istodobnosti provjere. |

Slijede dodatne atributa koji su dodani u specifikacija CSDL:

**d:Map** - XPath izraz koji se izvršava na temelju servis izlazne i izdvaja jedno svojstvo izlaz. XPath naveden je odnosu ponavljajućoj čvor koji je odabran u čvor Vrsta entiteta XPath. Također je moguće navesti apsolutne XPath da biste omogućili uključujući statične resursa u svakoj od čvorove Izlaz, kao što je, primjerice autorskih prava izjava pronađenu samo jednom izvornom servisu Izlaz, ali moraju postojati u svakom od redaka u rezultatu OData. Primjer iz servisa:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

Ovdje izraza XPath bi ./bar/baz0 da biste dobili čvor baz0 iz sadržaja davatelja usluge.

**d:CharMaxLength** - za vrste niz, možete odrediti maksimalni duljine. Pogledajte primjer DataService CSDL

**d:IsPrimaryKey** - označava je li stupac primarni ključ u tablici/prikaz. Pogledajte primjer DataService CSDL.

Određuje **d:isExposed** – ako shemu tablica predstavljeni (obično true). Pogledajte primjer DataService CSDL

**d:IsView** *(Neobavezno)* – vrijednost true ako je to se temelji na prikaz umjesto tablice.  Pogledajte primjer DataService CSDL

**d:Tableschema** – potražite u članku DataService CSDL primjer

**d:ColumnName** - je naziv stupca u tablici/prikaz.  Pogledajte primjer DataService CSDL

**d:IsReturned** - je Booleove vrijednosti koji određuje izlaže li servis tu vrijednost klijentu.  Pogledajte primjer DataService CSDL

**d:IsQueryable** - je Booleove vrijednosti koji određuje ako stupac može se koristiti u upitu baze podataka.   Pogledajte primjer DataService CSDL

**d:OrdinalPosition** - je stupca numeričke položaj izgled, x, u tablici ili u prikazu, gdje je x između 1 i broj stupaca u tablici.  Pogledajte primjer DataService CSDL

**d:DatabaseDataType** - je vrsta podataka stupca u bazi podataka, odnosno vrstu podataka za SQL. Pogledajte primjer DataService CSDL

## <a name="supported-parametersproperty-types"></a>Podržani parametara/svojstvo vrste
U nastavku su podržane vrste za parametre i svojstva. (Velika i mala slova)

| Jednostavne vrste | Opis |
|----|----|
| Null | Predstavlja izostanak vrijednosti |
| Booleove vrijednosti | Predstavlja matematičke pojam logike binarni vrijednosti|
| Bajt | Nepotpisane 8-bitnu cjelobrojnu vrijednost|
|Datum i vrijeme| Pomoću vrijednosti u rasponu od ponoći 12:00:00, siječanj 1, 1753 N.E. predstavlja datum i vrijeme do 11:59:59 P.M, N.E. prosinac 9999|
|Broja decimalnih mjesta | Predstavlja numeričkih vrijednosti s fiksnom preciznošću i promjena veličine. Ovu vrstu možete opisati numeričku vrijednost u rasponu od negativan 10 ^ 255 + 1 za pozitivne 10 ^ 255 -1|
| Dvaput | Predstavlja s pomičnim broj s 15 znamenaka preciznosti koja predstavlja vrijednosti pomoću djelomičnog raspon ± 2.23e-308 do ± 1.79e + 308. **Koristite decimalni zbog problema Exel izvoza**|
| Jedan | Predstavlja s pomičnim broj uz točnost 7 znamenki koja predstavlja vrijednosti pomoću djelomičnog raspon ± do ± 1.18e-38 3.40e +38|
|GUID |Predstavlja vrijednost Jedinstveni identifikator 16-bajtni (128-bitni) |
|Int16|Predstavlja traje 16-bitnu cjelobrojnu vrijednost |
|Int32|Predstavlja traje 32-bitnu cjelobrojnu vrijednost |
|Int64|Predstavlja traje 64-bitnu cjelobrojnu vrijednost |
|Niz | Prikazuje podatke znak ili varijabla – utvrđene širine |


## <a name="see-also"></a>Vidi također
- Ako vas zanima objašnjenje cjelokupan OData mapiranje postupak i svrhu, pročitajte ovaj članak [Mapiranje OData servisa podataka](marketplace-publishing-data-service-creation-odata-mapping.md) da biste pregledali definicije, strukture i upute.
- Ako vas zanima pregled primjera, pročitajte ovaj članak [Podataka usluge OData mapiranja primjere](marketplace-publishing-data-service-creation-odata-mapping-examples.md) potražite u članku uzorak koda i razumijevanje Sintaksa koda i kontekst.
- Da biste vratili propisanim put za objavljivanje podatkovnog servisa Azure Marketplace, pročitajte ovaj članak [Vodič za objavljivanje servisa podataka](marketplace-publishing-data-service-creation.md).
