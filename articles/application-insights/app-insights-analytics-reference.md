<properties 
    pageTitle="Referenca u analize u aplikaciji uvida | Microsoft Azure" 
    description="Vodič za naredbe u analize, alat za napredno pretraživanje uvida aplikacije. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Referenca za analizu

[Analitički](app-insights-analytics.md) je napredna pretraživanja značajka [Uvida aplikacije](app-insights-overview.md). Ove stranice opišite analize jezika za upite.

> [AZURE.NOTE] [Probnu vožnju analize na našem Simulirani podataka](https://analytics.applicationinsights.io/demo) ako aplikacija nije slanje podataka do uvida aplikacije još.

## <a name="index"></a>Indeks


**Omogućivanje** [Omogućivanje](#let-clause)


**Upiti i operatora** [Count](#count-operator)  |  [procijeniti](#evaluate-operator) | [Proširivanje](#extend-operator) | [spoj](#join-operator) | [ograničenje](#limit-operator) | [mvexpand](#mvexpand-operator) | [raščlaniti](#parse-operator) | [projekta](#project-operator) | [projekta Odsutan](#project-away-operator) | [raspon](#range-operator) | [smanjiti](#reduce-operator) | [prikazivanje uputa](#render-directive) | [ograničiti uvjet](#restrict-clause) | [Sortiranje](#sort-operator) | [Sažetak](#summarize-operator) | [poduzeti](#take-operator) | [vrha](#top-operator) | [vrha ugniježđene](#top-nested-operator) | [unije](#union-operator) | [gdje](#where-operator) | [u where](#where-in-operator)

**Zbirne funkcije** [sve](#any)  |  [argmax](#argmax) | [argmin](#argmin) | [avg](#avg) | [buildschema](#buildschema) | [count](#count) | [countif](#countif) | [dcount](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [Maks](#max) | [min](#min) | [percentil](#percentile) | [percentili](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [stdev](#stdev) | [zbroj](#sum) | [varijance](#variance)

**Scalars** [Booleova literala](#boolean-literals)  |  [Logički operatori](#boolean-operators) | [Casts](#casts) | [skalarnu usporedbe](#scalar-comparisons) | [gettype](#gettype) | [raspršivanje](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**Brojevi** [Aritmetički operatori](#arithmetic-operators)  |  [Numerički literala](#numeric-literals) | [abs](#abs) | [smeće](#bin) | [exp](#exp) | [floor](#floor) | [gama](#gamma) | [zapisnika](#log) | [rand](#rand) | [sqrt](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

**Datum i vrijeme** [Datum i vrijeme izrazi](#date-and-time-expressions)  |  [Datuma i vremena](#date-and-time-literals) | [prije](#ago) | [datepart](#datepart) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) | [dayofyear](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [sada](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**Niz** [GUID](#guids)  |  [Nečitljivima literala niz](#obfuscated-string-literals) | [Niz literala](#string-literals) | [usporedbama niza](#string-comparisons) | [countof](#countof) | [izdvojiti](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [Zamjena](#replace) | [Podjela](#split) | [strcat](#strcat) | [strlen](#strlen) | [podniz](#substring) | [tolower](#tolower) | [toupper](#toupper)

**Polja, objekata i dinamičkih** [Polja i objekt literala](#array-and-object-literals)  |  [Dinamički objekt funkcije](#dynamic-object-functions) | [dinamički objekata u omogućite rečenice](#dynamic-objects-in-let-clauses) | [put JSON izraza](#json-path-expressions) | [imena](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [raspon](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>Omogućivanje

### <a name="let-clause"></a>Omogućivanje uvjet

**Tablično omogućuju - imenovanja tablice**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Skalar omogućuju - imenovanja vrijednost**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda omogućuju - imenovanja funkcija**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Omogućite uvjet povezuje [naziv](#names) tablice rezultata, skalarnu vrijednost ili funkcije. Uvjet prefiks u upit, a opsega povezivanje taj upit. (Omogućuju ne nudi način sadržaju ime koje koristite u nastavku sesiju.)

**Sintaksa**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *Vrsta:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:* Upit koji se ne mjestu tako da Omogući uvjet.

**Primjeri**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Samostalno uključivanje:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Upiti i operatora

Upit putem vaše telemetrijskih sastoji se od referenca izvorni tok, nakon čega slijedi kanal filtara. Ako, na primjer:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Svaki filtar mjestu znakom kanala `|` je instanca *operatora*s nekim parametra. Unos operatoru je tablice koja je rezultat prethodnog kanal. U većini slučajeva parametre [izraza](#scalars) koji se skalarnu na stupcima unos. U nekoliko slučajeva parametre su nazivi stupaca za unos, a u nekoliko slučajeva je parametar druge tablice. Rezultat upita uvijek je tablica, čak i ako ima se samo jedan stupac i jedan redak.

Upiti mogu sadržavati prijelome jedan redak, ali prekinuta prazan redak. Možda sadrže komentare između `//` i na kraju retka.

Upit može mjestu po jednu ili više [omogućuju rečenice](#let-clause), mogu se koji definiranje scalars, tablice ili funkcije koje se mogu koristiti u upitu.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`koristi se u upitu primjere u nastavku da biste označili prethodnoj tablici kanalu ili izvora.
> 

### <a name="count-operator"></a>operator za brojanje

Na `count` operator vraća broj zapise (retke) u unos skup zapisa.

**Sintaksa**

    T | count

**Argumenti**

* *T*: tablične podatke čiji zapisi se za brojanje.

**Vraća**

Ova funkcija vraća tablicu s jednim zapisom i stupac vrste `long`. Vrijednost ćelije samo je broja zapisa u *T*. 

**Primjer**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>Analiza operator

`evaluate`je mehanizam za nastavak koji omogućuje specijalizirane algoritama za biti dodan u upite.

`evaluate`mora biti posljednje operator u kanalu upita (osim u moguće `render`). Mora biti u tijelo (opis funkcije).

[Analiza autocluster](#evaluate-autocluster) | [procijeniti košarica](#evaluate-basket) | [procijeniti diffpatterns](#evaluate-diffpatterns) | [procijeniti extractcolumns](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>Analiza autocluster

     T | evaluate autocluster()

AutoCluster pronalazi uobičajene uzorke samostalni atributa (dimenzije) u podacima i će mali broj uzoraka smanjite rezultate izvorne upita (hoće li se 100 ili 100 k redaka). AutoCluster je razvio radi analize pogreške (primjerice iznimke, ruši), ali je potencijalno raditi na bilo kojem filtrirani skup podataka. 

**Sintaksa**

    T | evaluate autocluster( arguments )

**Vraća**

AutoCluster vraća (obično mali) skup znakova koje snimite dijelove podataka pomoću zajedničke uobičajenih vrijednosti preko više samostalni atributa. Svaki uzorak predstavlja retka u rezultate. 

Prva dva stupca su count i postotak retke iz izvorni upit koji snimaju s uzorkom. Ostali stupci s izvornog upita, a njihov vrijednost je ili određenu vrijednost iz stupca ili "*" znači vrijednostima varijable. 

Imajte na umu da nisu zasebnim uzorke: možda koji se preklapaju i obično pokriva sve izvorne retke. Neki reci se mogu nalaziti u odjeljku sve uzorka.

**Savjeti**

* Korištenje `where` i `project` u unos kanala da biste saželi podatke samo što vas zanima.
* Kada pronađete zanimljive retka, preporučujemo da ga dodatno dodavanjem određene vrijednosti da biste dubinski vaše `where` filtar.

**Argumente (sve neobavezno)**

* `output=all | values | minimal` 

    Oblikovanje rezultata. Stupci Count i postotak uvijek se pojavljuju u rezultatima. 

 * `all`-sve stupce iz unos su Izlaz
 * `values`– filtrira stupaca samo "*" u rezultatima
 * `minimal`-i filtrira stupaca koji su jednaki za sve retke u izvorni upit. 


* `min_percent=`*Dvostruki* (Zadana: 1)

    Opseg minimalnog postotka generirani redaka.

    Primjer:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*Int* (Zadana: 25) 

    Broj sjemenke određuje broj početnog lokalno pretraživanje točke algoritam. U nekim slučajevima, ovisno o strukturu podataka, povećava broj sjemenke povećava broj (ili kvalitetu) rezultata putem povećana pretraživanje prostor na sporije tradeoff upita. Num_seeds argument ima diminishing rezultate u oba smjera tako da se smanjuje ispod 5 postigli poboljšanja performansi negligible i povećanje iznad 50 rijetko generiranje dodatne uzorke.

    Primjer:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < dvostruki < 1*+ (zadani: 0,5)

    Imate neki kontrolu nad saldo između generic (visoke opseg) i Informativna (mnoge zajedničke vrijednosti). Povećavati size_weight obično smanjuje broj uzoraka, a svaki uzorak ima da prekrije veći postotak. Smanjivanje size_weight obično daje konkretne uzoraka s zajedničke vrijednosti i manji opseg postotak. Na Pokaži napredne postavke formula je ponderiranog geometrijsku sredinu između normaliziranu generički rezultat i Informativna rezultat s size_weight i 1 size_weight kao na debljine. 

    Primjer:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*column_name*

    Smatra svaki redak u unos prema navedenom Debljina (po zadanom svaki redak sadrži debljine "1"), uobičajeni korištenje stupca Debljina je da bi se u račun za stvaranje uzoraka ili bucketing/prikupljanja podataka koji je već ugrađen u svaki redak.

    Primjer:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>Analiza košarica

     T | evaluate basket()

Košarica pronalazi sve Česti uzoraka samostalni atributa (dimenzije) u podacima, a sve Česti uzorke proslijeđen praga učestalost izvorni upit koji će vratiti. Košarica uvijek je da biste pronašli sve Česti uzorci podataka, ali se zajamčeno Polinomna za vrijeme izvođenja. Izvođenju upita je linearni broj redaka, no u nekim slučajevima može biti eksponencijalne u željeni broj stupaca (dimenzije). Košarica temelji se na algoritam Apriori izvorno razvijena za dubinsko pretraživanje programa košarica analize podataka. 

**Vraća**

Svi uzorci koji se pojavljuje u više od navedenog razlomka (zadano 0,05) događaje.

**Argumente (sve neobavezno)**


* `threshold=`*0.015 < dvostruki < 1* (Zadana: 0,05) 

    Postavlja minimalan omjer redaka smatraju Česti (uzoraka s manjim omjer neće vratiti).

    Primjer:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*column_name*

    Smatra svaki redak u unos prema navedenom Debljina (po zadanom svaki redak sadrži debljine "1"), uobičajeni korištenje stupca Debljina je da bi se u račun za stvaranje uzoraka ili bucketing/prikupljanja podataka koji je već ugrađen u svaki redak.

    Primjer: T | Analiza basket("weight_column=sample_Count")


* `max_dims=`*1 < int* (Zadana: 5)

    Postavlja maximal broj uncorrelated dimenzija po košarica, ograničen po zadanom da biste smanjili izvođenje upita.


* `output=minimize` | `all` 

    Oblikovanje rezultata. Stupci Count i postotak uvijek se pojavljuju u rezultatima.

 * `minimize`– filtrira stupaca samo "*" u rezultatima.
 * `all`-sve stupce iz unos su izlaz.




#### <a name="evaluate-diffpatterns"></a>Analiza diffpatterns

     requests | evaluate diffpatterns("split=success")

Diffpatterns uspoređuje dva skupa podataka iste strukture i pronalazi uzoraka samostalni atributa (dimenzije) koji karakteriziraju razlike između dva skupa podataka. Diffpatterns je razvio radi analize pogreške (primjerice po Usporedba pogrešaka u ne-problemi s određenom vremensko razdoblje), ali možete pronaći razlike u bilo kojem dva skupa podataka istu strukturu. 

**Sintaksa**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Vraća**

Diffpatterns vraća (obično mali) skup znakova koje snimite različite dijelove podataka u dva skupa (odnosno uzorak hvatanje velike postotak reci u prvi skup podataka i niska postotak reci u drugi skup). Svaki uzorak predstavlja retka u rezultate.

Prva četiri su count i postotak retke iz izvorni upit koji snimaju uzorak u svakom skupu, peti stupac je razlika (u točkama apsolutne) između dva skupa. Ostali stupci s izvornog upita, a njihov vrijednost je ili određenu vrijednost iz stupca ili * znači vrijednostima varijable. 

Imajte na umu da uzorke ne razlikuju: možda koji se preklapaju i obično pokriva sve izvorne retke. Neki reci se mogu nalaziti u odjeljku sve uzorka.

**Savjeti**

* Korištenje gdje i project u unos kanala da biste saželi podatke samo što vas zanima.

* Kada pronađete zanimljive retka, možda ćete ga dodatno dodavanjem određene vrijednosti da biste filtrirali vaše gdje dubinski.

**Argumenti**

* `split=`*naziv stupca* (obavezno)

    Stupac mora imati točno dvije vrijednosti. Ako je potrebno, stvorite takav stupac:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*niz*

    Nalaže algoritam da biste potražili samo uzorke koje imaju veći postotak u ciljni skup podataka, cilj mora biti jedna od dvije vrijednosti Podijeli stupac.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < dvostruki < 1* (Zadana: 0,05) 

    Postavlja minimalni uzorak (proporcije) razliku između dva skupa.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    Oblikovanje rezultata. Stupci Count i postotak uvijek se pojavljuju u rezultatima. 

 * `minimize`– filtrira stupaca samo "*" u rezultatima
 * `all`-sve stupce iz unos su Izlaz

* `weight_column=`*column_name*

    Smatra svaki redak u unos prema navedenom Debljina (po zadanom svaki redak sadrži debljine "1"). Često se koristi stupca Debljina je da bi se u račun za stvaranje uzoraka ili bucketing/prikupljanja podataka koji je već ugrađen u svaki redak.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>Analiza extractcolumns

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns koristi se za obogaćivanje tablicu s više jednostavne stupaca koji su dinamički izdvojenih iz (očekivan) strukturirane stupce, ovisno o vrsti. Trenutno podržava json stupaca samo oba dinamičkih i niza serijalizacije jsons.


* `max_columns=`*Int* (Zadana: 10) 

    Broj novih stupaca dodane su dinamički i može biti vrlo velike (zapravo je broj različitih tipki u svim zapisima json) pa ćemo potrebno ograničiti. Nove stupce su Sortirano silaznim redoslijedom koji se temelji na njihove učestalost, a najviše max_columns se dodaju u tablicu.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*Dvostruki* (Zadana: 10.0) 

    Drugi način da biste ograničili nove stupce po zanemarujući stupaca čije učestalost je manja od min_percent.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*booleovom* (Zadana: true) 

    Ako je istinito naziv stupca s složene dodaje se kao prefiks imena izdvojene stupaca.


* `prefix_delimiter=`*niz* (Zadana: "_") 

    Ako add_prefix = true ovaj parametar određuje graničnik koji će se koristiti za spajanje imena nove stupce.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*booleovom* (Zadana: false) 

    Ako je istinito izvornim stupcima (json) će biti zadržane u izlaznoj tablici.


* `output=query | table` 

    Oblikovanje rezultata. 

 * `table`-Izlaz je istoj tablici kao primljen minus navedenim stupcima unos plus novih stupaca koji su dobivenih iz ulaznih stupaca.
 * `query`-Izlaz je niz koji predstavlja upita bi da biste dobili rezultat kao tablicu. 




### <a name="extend-operator"></a>Proširivanje operator

     T | extend duration = stopTime - startTime

Dodavanje izračunatih stupaca u tablici. 


**Sintaksa**

    T | extend ColumnName = Expression [, ...]

**Argumenti**

* *T:* Ulazna tablica.
* *Nazivstupca:* Naziv stupce koje želite dodati. [Nazivi](#names) se velika i mala slova i mogu sadržavati slova, numeričkih ili '_' znakova. Korištenje `['...']` ili `["..."]` da biste ponuda za ključne riječi ili imena s drugih znakova.
* *Izraz:* Izračun iznad postojećih stupaca.

**Vraća**

Kopija ulazna tablica s navedenom dodatnih stupaca.

**Savjeti**

* Korištenje [`project`](#project-operator) umjesto toga, ako želite ispustite ili preimenovanje neke stupce.
* Nemojte koristiti `extend` da biste pristupili kraće ime koje želite koristiti u izrazu dugo, jednostavno. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    Izvorni stupci tablice imaju nisu indeksirani; novi naziv definira dodatni stupac koji nije indeksirana, tako da se vjerojatno će funkcionirati sporije upit.

**Primjer**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>operator spoj

    Table1 | join (Table2) on CommonColumn

Spaja redaka dvije tablice po vrijednosti koje se podudaraju navedenog stupca.


**Sintaksa**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumenti**

* *Tablica1* - 'Lijeva strana' strane spoja.
* *Tablica2* - "desnoj strani' spoja. Možda ćete se izraz ugniježđeni upit koji proizvodi tablicu.
* *CommonColumn* - stupac koji ima isti naziv u dvjema tablicama.
* *Vrsta* - određuje kako se reci iz dviju tablica se uspoređuje.

**Vraća**

Tablica s:

* Stupac za svaki stupac u svakoj od dviju tablica, uključujući tipke koji se podudaraju. Stupci s desne strane automatski će se preimenovati ako postoje clashes naziv.
* Redak za svaki podudaranje između ulaznih tablica. Rezultat je redak odabran iz jedne tablice koja sadrži iste vrijednosti za sve u `on` polja kao redak u drugoj tablici. 

* `Kind`neodređeni

    Samo jedan redak s lijeve se uparuje za svaku vrijednost parametra u `on` ključa. Izlaz sadrži redak za svaki podudaranje redak s redaka s desne strane.

* `Kind=inner`
 
     Je izlaz za svaku kombinaciju reci koji se podudaraju s lijeve i desne redak.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Osim na unutarnji odgovara postoji redak za svaki redak na lijevom (ili desno), čak i ako ga ne sadrži nijedan rezultat. U tom slučaju neuparenih izlaz ćelije sadrže vrijednosti null.

* `kind=leftanti`

     Vraća sve zapise iz s lijeve strane koji nemaju podudaranja s desne strane. Tablica s rezultatima samo ima stupca s lijeve strane. 
 
Ako postoji nekoliko redaka sadrži jednake vrijednosti za ta polja, prikazat će se redaka za sve kombinacije.

**Savjeti**

Za najbolje performanse:

* Korištenje `where` i `project` da biste smanjili broj redaka i stupaca u unos tablice prije na `join`. 
* Ako jedna tablica je uvijek manja od drugog, pomoću njega kao (piped) lijevoj strani spoja.
* Stupci za podudaranje spoj mora imati isti naziv. Ako je potrebno da biste promijenili naziv stupca u jednu od tablica, upotrijebite operator projekta.

**Primjer**

Početak proširena aktivnosti iz zapisnika u koje neke stavke označavanje početka i završetka aktivnosti. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>operator ograničenje

     T | limit 5

Vraća navedeni broj redaka iz tablice za unos. Nema jamstva zapisa koje se vraćaju. (Da biste vratili određene zapise, koristite [`top`](#top-operator).)

**Pseudonim**`take`

**Sintaksa**

    T | limit NumberOfRows


**Savjeti**

`Take`je jednostavno i učinkovito način da biste vidjeli uzorak rezultate kada radite interaktivno. Imajte na umu da ne jamči proizvesti određene retke ili ponuditi ih u nekom određenom redoslijedu.

Postoji implicitno ograničenja broja redaka za klijenta, čak i ako ne koristite `take`. Da biste Podignite to ograničenje, koristite na `notruncation` mogućnost traženja klijenta.



### <a name="mvexpand-operator"></a>mvexpand operator

    T | mvexpand listColumn 

S popisa iz ćelije (JSON) za dinamičku upisali proširuje da bi se svaka stavka ima zasebnom retku. Postoje i sve druge ćelije u retku prošireno. 

(Vidi također [`summarize makelist`](#summarize-operator) koji se izvodi funkciju suprotnu.)

**Primjer**

Pretpostavimo da je ulazna tablica:

|A:INT|B:String|D:Dynamic|
|---|---|---|
|1|"Pozdrav"|{"tipke": "vrijednost"}|
|2|"svijeta"|[0,1, "k", "v"]|

    mvexpand D

Rezultat je:

|A:INT|B:String|D:Dynamic|
|---|---|---|
|1|"Pozdrav"|{"tipke": "vrijednost"}|
|2|"svijeta"|0|
|2|"svijeta"|1|
|2|"svijeta"|"k"|
|2|"svijeta"|"v"|


**Sintaksa**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumenti**

* *Nazivstupca:* U rezultatu polja u stupcu imenovane su proširene više redaka. 
* *ArrayExpression:* Izraz prinos polja. Ako se koristi obrazac dodaje se novi stupac, a zadržava se postojeći.
* *Naziv:* Naziv za novi stupac.
* *Typename:* Oblikuje prošireno izraz u određenu vrstu
* *RowLimit:* Maksimalan broj redaka koji su stvorene iz svaki redak izvorne. Zadano je 128.

**Vraća**

Više redaka za svaku od vrijednosti u bilo kojem polju u stupcu imenovani ili u izrazu polja.

Prošireno stupac uvijek ima dinamički vrsta. Korištenje programa glumcima kao što su `todatetime()` ili `toint()` ako želite izračunati ili vrijednost zbroja.

Podržani su dva načina širenja spremnika svojstava:

* `bagexpansion=bag`: Svojstvo vrećica proširuju u vrećica svojstvo jednu stavku. Ovo je zadana proširenja.
* `bagexpansion=array`: Svojstvo vrećica proširuju u dva element `[` *ključ*`,`*vrijednost* `]` polja strukture, dopuštanja pristupa uniform ključeva i vrijednosti (kao i, ako, na primjer, radi točnog broja zbrajanja preko naziva svojstava). 

**Primjeri**


    exceptions | take 1 
  	| mvexpand details[0]

Zapis poslovnog iznimku dijeli redaka za svaku stavku u polju pojedinosti.



### <a name="parse-operator"></a>Raščlanjivanje operator

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Izdvaja vrijednosti iz niza. Možete koristiti jednostavne ili običnog izraz koji se podudaraju.

**Sintaksa**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumenti**

* `T`: Ulazna tablica.
* `kind`: 
 * `simple`(zadano): u `Match` su nizovi običnog nizova.
 * `relaxed`: Ako tekst ne raščlaniti kao vrste stupca, stupac postavljena na null i u rastavljanju nastavi 
 * `regex`: u `Match` su nizovi regularne izraze.
* `Text`: Stupac ili druge izraz koji se vrednuje kao ili može pretvoriti u niz.
* *Rezultat:* Odgovaraju sljedeće dio niza i odbacite.
* *Stupac:* Dodijelite sljedeće dio niz stupac. Ako ne postoji, stvara se stupac.
* *Vrsta:* Sljedeće dio niza raščlaniti kao navedene vrste, kao što je cijeli broj, datum, dvaput. 


**Vraća**

Ulazna tablica, prošireni prema popis stupaca.

Elementi u na `with` uvjet se uparuje s izvornog teksta u nizu. Svaki element chews isključivanje bloka izvornog teksta: 

* Slovni niz ili Uobičajeni izraz pokazivač miša odgovarajuće duljinom podudaranje.
* U rastavljanju u regex Uobičajeni izraz možete koristiti minimization operator '? "da biste premjestili čim sljedeći rezultat.
* Naziv stupca s vrstom raščlanjuje tekst kao navedene vrste. Osim ako vrsta = Opuštena, neuspješnih rastavljanju poništava koji se podudaraju cijeli uzorak.
* Naziv stupca bez vrstu ili s vrstom 'niza', kopira najmanji broj znakova da biste pristupili sljedeći rezultat.
* " *" Preskače najmanji broj znakova da biste pristupili sljedeći rezultat. Možete koristiti '*' na početku i kraju s uzorkom ili nakon vrstu osim niz ili između niz rezultata.

Svi elementi u uzorku rastavljanju moraju se podudarati pravilno; u suprotnom, rezultati će proizvesti. To pravilo jedina je iznimka koja vrsta = Opuštena Raščlanjivanje upisani varijabla ne uspije, i dalje ostatak u rastavljanju.

**Primjeri**

*Jednostavan:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | Brojač | prezentacija | Godine
---|---|---|---
1 | 2 | SOCKS | 63

*Opuštena:*

Kada unos sadrži odgovarajuće podudaranje za svaki stupac upisani, Opuštena rastavljanju daje jednake rezultate kao jednostavni rastavljanju. No ako jedan od upisani stupaca ne može raščlaniti pravilno, Opuštena rastavljanju i dalje za obradu ostalih uzorka, dok je jednostavno rastavljanju Zaustavi i ne uspije da biste generirali bilo koji rezultat.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | prezentacija | Godine
---|---|---
1 |  SOCKS | 63


*RegEx:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

resurs | odsječak | Zaključavanje | izdanje | Prethodni
---|---|---|---|---
Raspored | 16 | 17/02/2016 08:41:00 | 17/02/2016 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>operator projekta

    T | project cost=price*quantity, price

Odaberite stupce koje želite uvrstiti, preimenovanje i ispustite pa umetnite novi izračunati stupci. Redoslijed stupaca u rezultatu navedena je by the order of argumente. Samo na navedene stupce u argumentima uključeni u rezultatu: sva ostala u unos ispuštaju.  (Vidi također `extend`.)


**Sintaksa**

    T | project ColumnName [= Expression] [, ...]

**Argumenti**

* *T:* Ulazna tablica.
* *Nazivstupca:* Naziv stupca u izlaz. Ako vam se prikazuje bez *izraz*stupca taj naziv mora biti naveden u unos. [Nazivi](#names) se velika i mala slova i mogu sadržavati slova, numeričkih ili '_' znakova. Korištenje `['...']` ili `["..."]` da biste ponuda za ključne riječi ili imena s drugih znakova.
* *Izraz:* Neobavezni skalarni izraz pozivanju ulaznih stupaca. 

    To je pravne da biste vratili novi izračunati stupac s istim nazivom kao postojeći stupac u unos.

**Vraća**

Tablicu koja sadrži stupaca imenovan kao argumenti i što je više redaka kao ulazna tablica.

**Primjer**

Sljedeći primjer prikazuje nekoliko vrsta operacije koje je moguće izvršiti pomoću na `project` operator. Ulazna tablica `T` ima tri stupca vrste `int`: `A`, `B`, i `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>operator projekta Odsutan

    T | project-away column1, column2, ...

Izuzimanje navedene stupce. Rezultat sadrži sve unos stupce osim onih koje dodijelite naziv.

### <a name="range-operator"></a>operator raspona

    range LastWeek from ago(7d) to now() step 1d

Stvara tablica jedan stupac s vrijednostima. Obratite pozornost na to da ga nema kanal za unos. 

|LastWeek|
|---|
|2015 12 05 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|09:10:04.627 2015 12 12|



**Sintaksa**

    range ColumnName from Start to Stop step Step

**Argumenti**

* *Nazivstupca:* Naziv jednog stupca u tablici izlaz.
* *Start:* Najmanje vrijednosti u izlaz.
* *Zaustavi:* Najveća vrijednost generira izlaz (ili granica najveću vrijednost, ako je *korak* korake iznad tu vrijednost).
* *Korak:* Razlika između dva uzastopna vrijednosti. 

Argumenti moraju biti numeričkih, datum ili vremenski raspon vrijednosti. Oni ne možete se referencirati stupaca sve tablice. (Ako želite izračunati raspona na temelju ulazna tablica pomoću [rasponu *(funkcija)*](#range), možda pomoću [operatora mvexpand](#mvexpand-operator).) 

**Vraća**

Tablica s jednim stupcem naziva *ColumnName*, čije su vrijednosti *pokretanje*, *pokrenite* + *korak*... do i uključujući *Zaustavi*.

**Primjer**  

```AIQL
range Steps from 1 to 8 step 3
```

Tablica s jednim stupcem naziva `Steps` vrste `long` i čije su vrijednosti `1`, `4`, i `7`.

**Primjer**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Tablica ponoći u zadnjih sedam dana. Funkcija smeće (floor) smanjuje svaki put na početak dana.

**Primjer**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Prikazuje kako `range` operator može se koristiti za stvaranje male, ad-hoc, tablicu dimenzija zatim koristi se za predstavljanje nule gdje izvorišnih podataka ima nema vrijednosti.

### <a name="reduce-operator"></a>Smanjivanje operator

    exceptions | reduce by outerMessage

Pokušava Grupiranje sličnih zapisa. Za svaku grupu operator Proizvodi na `Pattern` smatra najbolje opisuje te grupe i `Count` zapisa u toj grupi.


![](./media/app-insights-analytics-reference/reduce.png)

**Sintaksa**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumenti**

* *Nazivstupca:* Stupac da biste pregledali. Mora biti niz.
* *Prag:* Vrijednosti u rasponu {0..1}. Zadana vrijednost je 0,001. Za velike unosa prag mora biti mala. 

**Vraća**

Dva stupca, `Pattern` i `Count`. U mnogim slučajevima, uzorak će biti potpunu vrijednost iz stupca. U nekim slučajevima može prepoznavanje uobičajenih uvjeta i zamjena varijable dijelove s "*".

Na primjer, rezultat `reduce by city` mogu sadržavati: 

|Uzorak | Count |
|---|---|
| San * | 5182 |
| Sveti * | 2846 |
| Moskva | 3726 |
| \*-na-\* | 2730 |
| Pariz | 27163 |


### <a name="render-directive"></a>Prikaz uputa

    T | render [ table | timechart  | barchart | piechart ]

Prikazivanje usmjerava sloj prezentacije kako prikazati u tablici. Mora biti posljednjeg elementa kanala. To je praktičan alternativa pomoću kontrola na zaslonu omogućujući vam da biste spremili upit s metode određenu prezentaciju.

### <a name="restrict-clause"></a>Ograničavanje uvjet 

Određuje skup nazive tablica dostupni operatore koje pratite. Ako, na primjer:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>operator sortiranje 

    T | sort by country asc, price desc

Sortirajte retke unos tablice u redoslijedu po jedan ili više stupaca.

**Pseudonim**`order`

**Sintaksa**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumenti**

* *T:* Tablica za unos za sortiranje.
* *Stupac:* Stupac *T* po kojem želite sortirati. Vrsta vrijednosti mora biti numerička, datum, vrijeme ili niz.
* `asc`Sortiranje po u uzlazni redoslijed, najniža na Visoka. Zadana vrijednost je `desc`, silazno visoko na nisko.

**Primjer**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Svi reci u kašnjenja tablice koje sadrže određenu `ActivityId`, sortirani prema svojim vremenske oznake.

### <a name="summarize-operator"></a>Sažimanje operator

Stvara tablicu koja objedinjuje sadržaj ulazna tablica.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Tablica koja se prikazuje broj, prosjek u drugoj i skup gradova u svakoj državi. Postoji redak u rezultatu uz svaku državu distinct. Izlazne stupce Prikaži broj, Prosječno trajanje, gradove i države. Sve ostale stupce unos se zanemaruju.


    T | summarize count() by price_range=bin(price, 10.0)

Tablice koji pokazuje koliko stavki imaju cijene u svakom razdoblju [0,10.0], [10.0,20.0] i tako dalje. U ovom se primjeru sadrži stupac za broj i jedan za raspon cijena. Sve ostale stupce unos se zanemaruju.


**Sintaksa**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumenti**

* *Stupac:* Neobavezan naziv stupca rezultat. Po zadanom je naziv izvedene iz izraza. [Nazivi](#names) se velika i mala slova i mogu sadržavati slova, numeričkih ili '_' znakova. Korištenje `['...']` ili `["..."]` da biste ponuda za ključne riječi ili imena s drugih znakova.
* *Zbrajanja:* Poziv u funkciju zbrajanja kao što su `count()` ili `avg()`, s nazivima stupaca kao argumente. U odjeljku [zbrajanja](#aggregations).
* *GroupExpression:* Izraz iznad stupce, koji pruža skup različitih vrijednosti. Obično je ili naziv stupca koji već sadrži ograničenog skupa vrijednosti, ili `bin()` sa stupcem numerička ili vremena kao argument. 

Ako unesete numerički ili vrijeme izraz bez korištenja `bin()`, analize ga automatski primjenjuje s intervala od `1h` za Izostanak, ili `1.0` za brojeve.

Ako ne unesete *GroupExpression,* cijelu tablicu je navedene u izlaz jedan redak.



**Vraća**

Unos redaka raspoređena su u grupe koje se pojavljuju iste vrijednosti u `by` izraza. Zatim funkcije zbrajanja navedeni su izračunati preko svake grupe proizvodnje redak za svaku grupu. Prikazuje rezultat u `by` stupaca i i barem jedan stupac za svaki unos izračunati zbroj. (Neke funkcije zbrajanja vratiti više stupaca).

Rezultat je proizvoljan broj redaka, kao što su distinct kombinacije `by` vrijednosti. Ako želite sažeti preko raspona numeričke vrijednosti, koristite `bin()` da biste saželi raspona samostalni vrijednosti.

**Napomena**

Iako možete unijeti proizvoljne izraza za zbrajanje i grupiranje izraza, je učinkovitije korištenje naziva jednostavan stupčasti ili primijenite `bin()` numerički stupac.



### <a name="take-operator"></a>iskoristite operator

Pseudonim [ograničenje](#limit-operator)


### <a name="top-operator"></a>Gornji operator

    T | top 5 by Name desc nulls first

Vraća prvi zapise *N* sortirani prema navedenim stupcima.


**Sintaksa**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumenti**

* *NumberOfRows:* Broj redaka *T* da biste se vratili.
* *Sort_expression:* Izraz po kojem želite sortirati retke. To je obično samo naziv stupca. Možete navesti više sort_expression.
* `asc`ili `desc` (zadano) može prikazati kontrolu je li odabrano zapravo od "dna" ili "vrh" raspon.
* `nulls first`ili `nulls last` kontrole mjesto pojavljivanja null vrijednosti. `First`Zadana postavka za `asc`, `last` je zadana postavka za `desc`.


**Savjeti**

`top 5 by name`jednako superficially `sort by name | take 5`. Međutim, izvodi brže i uvijek vraća sortiraju rezultati, dok `take` ne daje takve nikakva jamstva.

### <a name="top-nested-operator"></a>Vrh ugniježđene operator

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Daje hijerarhijski rezultata, pri čemu je svaku razinu u naniže iz prethodne razine. Je korisno za odgovaranje na pitanja koja se zvuk kao "koji su najvažniji 5 zahtjevi za svaki od njih, koje su polja gornje 3 performanse i za svaki od njih, koje su države vrha 3 zahtjeve potjecati iz?"

**Sintaksa**

   T | ugniježđene funkcije najvećih N od stupca prema ZBRAJANJA [,...]

**Argumenti**

* N:INT – broj redaka koji želite vratiti ili proslijediti sljedeću razinu. U upit s tri razine pri čemu je N 5, 3 i 3, ukupnog broja redaka bit će 45.
* STUPAC - stupac za grupiranje prema za zbrajanje. 
* ZBRAJANJE - [funkcije zbrajanja](#aggregations) da biste primijenili svake grupe redaka. Rezultati te zbrajanja će odrediti gornji grupe da se prikazuje.


### <a name="union-operator"></a>operator unije

     Table1 | union Table2, Table3

Koristi dvije ili više tablica i vraća ih sve retke. 

**Sintaksa**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumenti**

* *Tablica1*, *tablica2* ...
 *  Naziv tablice, kao što su `requests`, ili tablicu definirano u na [omogućuju uvjet](#let-clause); ili
 *  A upit izraz, kao što su`(requests | where success=="True")`
 *  Skup tablica navedenim s zamjenski znak. Na primjer, `e*` bi obrazac unije sve tablice koje su definirani u prethodno omogućite rečenice čiji naziv započeo s "e", zajedno s 'iznimke' tablice.
* `kind`: 
 * `inner`-Rezultat je podskup stupaca koji su zajednički sve unos tablice.
 * `outer`-Rezultat je sve stupce koji se pojavljuju u bilo kojem od ulaza. Ćelije koje su definira unos retka postavljene na `null`.
* `withsource=`*Nazivstupca:* Ako navedeni, izlaz neće sadržavati stavku pod nazivom *ColumnName* čija se vrijednost upućuje na to koji izvorišnu tablicu je pridonio svaki redak stupca.

**Vraća**

Tablica s koliko ima proizvoljan broj redaka u unos tablice te koliko ima proizvoljan broj stupaca naziva jedinstvenih stupaca u ulaza.

**Primjer**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
UNION sve tablice čiji nazivi početka "tt".


**Primjer**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Broj različitih korisnika koji su proizvodi ili na `exceptions` događaj ili `traces` događaja tijekom proteklih dan. U rezultatu stupcu "Izvorna" označava "Upita" ili "Naredbe".

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Ova verzija učinkovitiji ima isti učinak. Prije stvaranja je zajednica filtriraju svaku tablicu.

### <a name="where-operator"></a>gdje operator

     requests | where resultCode==200

Filtrira tablicu na podskup retke koji zadovoljavaju predikata.

**Pseudonim**`filter`

**Sintaksa**

    T | where Predicate

**Argumenti**

* *T:* Tablični unos čiji zapisi se filtriraju.
* *Predicate:* A `boolean` [izraz](#boolean) iznad stupca *T*. Se vrednuje za svaki redak u *T*.

**Vraća**

Redaka u *T* koje *Predicate* se `true`.

**Savjeti**

Da biste dobili najbrže performanse:

* **Korištenje jednostavne usporedbu** dvaju nazive stupaca i konstante. ('Konstante znači konstanta preko u tablicu – da bi `now()` i `ago()` su u redu, a tako skalarnu vrijednosti dodjeljuju pomoću na [ `let` uvjet](#let-clause).)

    Na primjer, radije `where Timestamp >= ago(1d)` da biste `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest odredbe prvog**: Ako imate više uvjeta conjoined s `and`, najprije postavite rečenice koje uključuju samo jedan stupac. Stoga `Timestamp > ago(1d) and OpId == EventId` bolja nego, Suprotno tome.


**Primjer**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Zapisi koje nisu starije od 1 sat dolazi iz izvora pod nazivom "Kuskus" i imaju dva stupca s istom vrijednošću. 

Imajte na umu da ne možemo staviti usporedbe između dva stupca zadnje, ne možete koristiti indeks i navodi pregleda.


### <a name="where-in-operator"></a>operator za mjesto

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Sintaksa**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumenti**

* `col`: Stupac u tablici.
* `expr1`...: Popis skalarnu izraza.

Korištenje `in` se uključiti samo retke u kojima `col` jednak je jedan od izraza parametra `expr1...`.

Korištenje `!in` da biste uvrstili samo retke u kojima `col` nije jednako na bilo koji od izraza `expr1...`.  


## <a name="aggregations"></a>Zbirne funkcije

Zbirne funkcije su funkcija koje se koriste za spajanje vrijednosti u grupama koje su stvorene u [Sažetak operacija](#summarize-operator). Ako, na primjer, u ovom upitu dcount() je funkciji zbrajanja:

    requests | summarize dcount(name) by success

### <a name="any"></a>sve 

    any(Expression)

Nasumično odabire jedan redak grupe i vraća vrijednost navedenog izraza.

To je korisno, na primjer, kada neki stupac sadrži velik broj slične vrijednosti (npr., "tekst pogreške" stupca), a želite poslušajte taj stupac po jednom jedinstvena vrijednost ključa složenih grupe. 

**Primjer**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin, argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Pronalazi retka u grupi koja se minimizira/maximises *ExprToMaximize*i vraća vrijednost *ExprToReturn* (ili `*` da biste se vratili na cijeli redak).

**Savjet**: prošli kroz stupce preimenuju se automatski. Da biste bili sigurni da koristite nazive desno, provjera rezultate pomoću `take 5` prije pipe rezultate u drugi operator.

**Primjeri**

Za svaki zahtjev za naziv Prikaži kada došlo je do najdulje zahtjev:

    requests | summarize argmax(duration, timestamp) by name

Prikaži detalje najdulje zahtjev, ne samo vremenska oznaka:

    requests | summarize argmax(duration, *) by name


Traži najmanju vrijednost svakog mjerenja zajedno s njegova vremenske oznake i ostalih podataka:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>Avg

    avg(Expression)

Izračunava prosjek *izraza* u grupi.

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

Vraća minimalnu sheme pristupiti sve vrijednosti *DynamicExpression*. 

Vrsta stupca parametar mora biti `dynamic` -Torba u polje ili svojstvo. 

**Primjer**

    exceptions | summarize buildschema(details)

Rezultat:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Primijetit ćete da `indexer` se koristi da biste označili gdje treba koristiti numerički indeks. Za ovu shemu neke valjani putova će biti (Ako su tih primjer indeksa u rasponu):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Primjer**

Pretpostavimo unos stupac ima tri dinamičke vrijednosti:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Rezultat sheme bio sljedeći:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Shema nam govore koji:

* Korijenski objekt je kontejner s četiri svojstva pod nazivom x, y, z i t.
* Svojstvo pod nazivom "x", koji može biti vrste "int" ili vrste "niza".
* Svojstvo pod nazivom "d", koji bi se mogao nijedne vrste "dvostruki" ili drugi spremnik uz svojstvo pod nazivom "w" Vrsta "niz".
* Na ``indexer`` označava ključnu riječ "z" i "t" jesu li polja.
* Svaku stavku u polju "z" je na cijeli broj ili niz.
* "t" je polje nizova.
* Svaki svojstvo je implicitno neobavezno i bilo kojeg polja mogu biti prazna.

##### <a name="schema-model"></a>Model sheme

Vidjet ćete da sintaksa vraćeni sheme je:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

To su jednake podskup vrsta primjedbe TypeScript kodira kao dinamičke vrijednosti. U Typescript, bio bi primjer sheme:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>Count

    count([ Predicate ])

Vraća broj redaka za koje *Predicate* vrednuje kao `true`. Ako je naveden u bez *Predicate* , vraća ukupan broj zapisa u grupi. 

**Savjet mjerača performansi**: korištenje `summarize count(filter)` umjesto`where filter | summarize count()`

> [AZURE.NOTE] Izbjegavajte count() da biste pronašli broj zahtjeve, iznimke ili druge događaje koje su se pojavile. Kada je [uzorkovanje](app-insights-sampling.md) u operaciji, broj točaka podataka ostaju u aplikaciji uvida bit će manji broj izvorne događaja. Umjesto toga koristite `summarize sum(itemCount)...`. Svojstvo BrojArtikala odražava broj izvorne događaje koje predstavljaju svaku točku zadržava se podataka.

### <a name="countif"></a>COUNTIF

    countif(Predicate)

Vraća broj redaka za koje *Predicate* vrednuje kao `true`.

**Savjet mjerača performansi**: korištenje `summarize countif(filter)` umjesto`where filter | summarize count()`

> [AZURE.NOTE] Izbjegavajte countif() da biste pronašli broj zahtjeve, iznimke ili druge događaje koje su se pojavile. Kada je [uzorkovanje](app-insights-sampling.md) u operaciji, broj točaka podataka bit će manji broj stvarni događaja. Umjesto toga koristite `summarize sum(itemCount)...`. Svojstvo BrojArtikala odražava broj izvorne događaje koje predstavljaju svaku točku zadržava se podataka.

### <a name="dcount"></a>DCount

    dcount( Expression [ ,  Accuracy ])

Vraća Procjena broj različitih vrijednosti *izraz* u grupi. (Da biste popis različitih vrijednosti, poslužite se [`makeset`](#makeset).)

*Točnost*, ako je naveden, kontrole saldo između brzinu i točnost.

 * `0`= najmanje točni i najbrže izračuna.
 * `1`Zadani salda vrijeme točnost i izračuna; o % 0,8 pogreške.
 * `2`= najčešće točni i najmanju izračuna; o 0,4% pogreške.

**Primjer**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Vraća Procjena broj različitih vrijednosti *izraz* redaka u grupi za koje *Predicate* je true. (Da biste popis različitih vrijednosti, poslužite se [`makeset`](#makeset).)

*Točnost*, ako je naveden, kontrole saldo između brzinu i točnost.

 * `0`= najmanje točni i najbrže izračuna.
 * `1`Zadani salda vrijeme točnost i izračuna; o % 0,8 pogreške.
 * `2`= najčešće točni i najmanju izračuna; o 0,4% pogreške.

**Primjer**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

Vraća na `dynamic` (JSON) polja s vrijednostima *izraz* u grupi. 

* *MaxListSize* je neobavezno cijeli broj ograničenja najveći broj elemenata koji se vraćaju (Zadana vrijednost je *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Vraća na `dynamic` polja (JSON) skupa različitih vrijednosti *izraz* koji se otvara u grupi. (Savjet: da biste samo broj različitih vrijednosti, poslužite se [`dcount`](#dcount).)
  
*  *MaxSetSize* je neobavezno cijeli broj ograničenja najveći broj elemenata koji se vraćaju (Zadana vrijednost je *128*).

**Primjer**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Vidi također na [ `mvexpand` operator](#mvexpand-operator) za funkciju suprotnu.


### <a name="max-min"></a>Maksimalni, min

    max(Expr)

Izračunava maksimalno *izraz*.
    
    min(Expr)

Izračunava minimum *izraz*.

**Savjet**: to vam min ili Maks samostalno – na primjer, na najviša ili najniže cijene. No ako želite da se drugim stupcima u retku – na primjer, naziv dobavljača i najniže cijene - pomoću [argmin ili argmax](#argmin-argmax).


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>percentil, percentili, percentilew, percentilesw

    percentile(Expression, Percentile)

Vraća Procjena za *izraz* navedeni percentil u grupi. Točnost ovisi o gustoće populacije u regiji percentil.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Kao što su `percentile()`, ali izračunava broj vrijednosti percentila (što je brže nego pojedinačno izračun svaki percentil).

    percentilew(Expression, WeightExpression, Percentile)

Ponderirani percentil. Ta postavka za unaprijed skupne podatke.  `WeightExpression`nije cijeli broj koji pokazuje koliko izvorne retke prikazani su svaki redak zbroja.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Kao što su `percentilew()`, ali se izračunava broj vrijednosti percentila.

**Primjeri**


Vrijednost `duration` veći od 95% uzorka postavljeno, a manja od 5% skupa uzorka izračunava za svaki zahtjev za naziv:

    request 
  	| summarize percentile(duration, 95)
      by name

Izostavite "tako da..." da biste izračunali za cijelu tablicu.

Istodobno izračun nekoliko percentili za zahtjev za različite nazive:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

Rezultati prikazali koji za /Events/Index zahtjev, 5% zahtjeva za su odgovorili u manje od 2.44s, polovica ih 3.52s, i 5% su manja od 6.85s.

Izračun višestrukih statističkih podataka:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Ponderirani percentili

Koristite funkcije ponderiranog percentil u slučajevima gdje je unaprijed Zbrojeno podatke. 

Ako, na primjer, pretpostavimo da aplikacije izvodi mnoge tisuće operacije sekundi i želite saznati njihove Latencija. Da biste generirali zahtjev za uvid aplikaciju ili prilagođeni događaj za svaku operaciju bio bi jednostavno rješenje. Time bi se stvorila mnogo promet, iako prilagodljivo uzorkovanje osvojio efekt da biste ga smanjili. No ipak možete implementirati još bolje rješenje: će pisanje neke koda u svojoj aplikaciji radi grupiranja podataka prije no što pošaljete uvida aplikacije. Zbrojeno sažetak poslat će se u pravilnim vremenskim razmacima možda smanjiti brzinu podataka na nekoliko točke u minuti.

Kod vodi strujanje mjere kašnjenje u milisekundama. Ako, na primjer:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Broji mjere u intervale sljedeće:`{ 10, 20, 30, 40, 50, 100 }`

Povremeno provjerava niz TrackEvent poziva, jedan za svaki grupe s prilagođene mjere u svaki poziv: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

U analize, vidjeti jedne grupe takvih događaja ovako:

`opCount` | `latency`| značenje
---|---|---
8 | 10 | = 8 postupke u smeće 10ms
6 | 20 | = 6 postupke u smeće 20ms
3 | 30 | = 3 postupke u smeće 30ms
1 | 40 | = 1 postupke u smeće 40ms

Da biste dobili točne slike izvorne distribucije latencies događaj, koristimo `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

Rezultati su isti kao da ste koristi običan `percentiles` na izvorni skup mjere.

> [AZURE.NOTE] Ponderirani percentili nije primjenjivo [ogledne podatke](app-insights-sampling.md), pri čemu svaki redak sampled predstavlja slučajni uzorak izvorne retke, a ne u smeće. Funkcije običnog percentil su odgovarajuće za ogledne podatke.

#### <a name="estimation-error-in-percentiles"></a>Pogreška procjeni u percentili

Aggregate percentili nudi približna vrijednost pomoću [T sažetka](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf). 

Nekoliko važne stavke: 

* Granica pogreške procjeni razlikuju se s vrijednošću tražene percentil. Najbolji točnost je na kraju [0..100] skaliranje percentili od 0 i 100 su točno minimalne i maksimalne vrijednosti distribucije. Točnost postupno se smanjuje prema sredini skalu. Se najgorim pri Medijan i capped se na 1%. 
* Pogreška granica vrše na položaj, ne i na vrijednost. Pretpostavimo da percentil (X 50) vratila vrijednost Xm. U procjeni jamčiti da barem 49%, a najviše 51% od vrijednosti X su manji od Xm. Nema ograničenja theoretical o razlikama između Xm i stvarne median vrijednost od X.

### <a name="stdev"></a>STDEV

     stdev(Expr)

Vraća standardnu devijaciju *izraz* iznad grupe.

### <a name="variance"></a>varijance

    variance(Expr)

Vraća varijancu *izraz* iznad grupe.

### <a name="sum"></a>zbroj

    sum(Expr)

Vraća zbroj *izraz* iznad grupe.                      


## <a name="scalars"></a>Scalars

[casts](#casts) | [usporedbe](#scalar-comparisons)
<br/>
[gettype](#gettype) | [raspršivanje](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

Podržane vrste su:

| Vrsta      | Dodatni imena   | Ekvivalentan vrsta .NET |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Casts

Možete se oblikuje iz jedne vrste u drugu. Općenito govoreći, ako pretvorbe smisla funkcionirat će:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Provjerite je li se niz moguće pretvoriti u određenu vrstu:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Skalarna usporedbe

||
---|---
`<` |Manje
`<=`|Manje ili jednako
`>` |Veća
`>=`|Veće ili jednako
`<>`|Nije jednako
`!=`|Nije jednako 
`in`| Desni operand je (dinamički) polja i Lijevi operand jednak je jedan od njegovih elemenata.
`!in`| Desni operand je (dinamički) polja i Lijevi operand nije jednako na neki od njegovih elemenata.




### <a name="gettype"></a>gettype

**Vraća**

Niz koji predstavlja podlozi vrsta pohrane jednog argumenta. To je posebno korisno kada su vrijednosti vrste `dynamic`: u ovom slučaju `gettype()` prikazuje kako kodira vrijednost.

**Primjeri**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>Raspršivanje

**Sintaksa**

    hash(source [, mod])

**Argumenti**

* *izvor*: izvorišnog web-mjesta skalarna raspršivanje se izračunava.
* *Mod*: U modulo vrijednost koja će se primijeniti na raspršivanje rezultat.

**Vraća**

Vrijednost xxhash (dugi) navedeni skalarnu, modulo navedene vrijednosti mod (Ako je naveden).

**Primjeri**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

Na `iff()` funkcija vrednuje kao prvi argument (predikata) i vraća argumenata druge i treće ovisno o tome je li u predikata ili vrijednost `true` ili `false`. Drugi i treći Argumenti moraju biti iste vrste.

**Sintaksa**

    iff(predicate, ifTrue, ifFalse)


**Argumenti**

* *predikata:* Izraz koji se procjenjuje kao u `boolean` vrijednost.
* *ifTrue:* Izraz koji se procjenjuje i njezina vrijednost vraća funkciju ako *predikata* daje `true`.
* *ifFalse:* Izraz koji se procjenjuje i njezina vrijednost vraća funkciju ako *predikata* daje `false`.

**Vraća**

Ova funkcija vraća vrijednost *ifTrue* ako se procijeni kao *predikata* `true`, ili vrijednost *ifFalse* drukčije.

**Primjer**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull isnotnull, notnull

    isnull(parsejson("")) == true

Traje jedan argument, a pokazuje je li vrijednost null.

**Sintaksa**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Vraća**

TRUE ili false ovisno o tome u tome je vrijednost null ili nije null.


|x|IsNull(x)
|---|---
| "" | FALSE
|"x" | FALSE
|parsejson("")|TRUE
|parsejson("[]")|FALSE
|parsejson("{}")|FALSE

**Primjer**

    T | where isnotnull(PossiblyNull) | count

Obratite pozornost na to da postoje drugi načini postizanje taj učinak:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Procjenjuje upita ili izraz i vraća rezultat kao jedna vrijednost. Ova funkcija je korisno za postupnu izračunima; na primjer, izračunavanje ukupan broj događaja i koja koristi kao referentne vrijednosti.

**Sintaksa**

    toscalar(query)
    toscalar(scalar)

**Vraća**

Argument provjeriti u odnosu. Ako je argument tablice, vraća se prvi stupac prvog retka. (Preporučuje se da biste rasporedili argument sadrži samo jedan stupac i redak.)

**Primjer**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Booleova literala

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Logički operatori

    and 
    or 

    

## <a name="numbers"></a>Brojevi

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Brojčani literala

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Aritmetički operatori

|| |
|---|-------------|
| + | Dodavanje         |
| - | Oduzimanje    |
| * | Množenje    |
| / | Dijeljenje      |
| % | Modulo      |
||
|`<` |Manje
|`<=`|Manje ili jednako
|`>` |Veća
|`>=`|Veće ili jednako
|`<>`|Nije jednako
|`!=`|Nije jednako 


### <a name="abs"></a>Abs

**Sintaksa**

    abs(x)

**Argumenti**

* x - cijeli broj, realni ili vremenski raspon

**Vraća**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>smeće, floor

Zaokružuje vrijednosti prema dolje do cijeli višekratnik veličine zadanog smeće. Koristi mnogo u na [`summarize by`](#summarize-operator) upita. Ako imate scattered skup vrijednosti, oni će grupirane u manji skup određene vrijednosti.

Pseudonim `floor`.

**Sintaksa**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumenti**

* *vrijednost:* Broj, datum ili vremenski raspon. 
* *roundTo:* "Smeće veličina". Broj, datum ili vremenski raspon koji dijeli *vrijednosti*. 

**Vraća**

Na najbliži višekratnik broja *roundTo* ispod *vrijednosti*.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Primjeri**

Izraz | Rezultat
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


Sljedeći izraz izračunava histograma trajanje s veličinom grupe od 1 sekunde:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>Exp

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>FLOOR

Pseudonim za [`bin()`](#bin).

### <a name="gamma"></a>gamma

[Gama-funkcije](https://en.wikipedia.org/wiki/Gamma_function)

**Sintaksa**

    gamma(x)

**Argumenti**

* *x:* Realni broj

Za pozitivni cijeli brojevi `gamma(x) == (x-1)!` , na primjer, `gamma(5) == 4 * 3 * 2 * 1`.

Vidi također [loggamma](#loggamma).


### <a name="log"></a>zapisnik

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`mora biti realni broj > 0. U suprotnom vraća vrijednost null.

### <a name="loggamma"></a>loggamma


Prirodni logaritam apsolutna vrijednost [gama-funkcije](#gamma).

**Sintaksa**

    loggamma(x)

**Argumenti**

* *x:* Realni broj


### <a name="rand"></a>rand

Slučajni broj generator.

* `rand()`-realnog broja između 0.0 i 1.0
* `rand(n)`-cijeli broj između 0 i n-1




### <a name="sqrt"></a>SQRT

Funkcija kvadratni korijen.  

**Sintaksa**

    sqrt(x)

**Argumenti**

* *x:* Realni broj > = 0.

**Vraća**

* Pozitivan broj tako da`sqrt(x) * sqrt(x) == x`
* `null`Ako je argument je negativan ili nije moguće pretvoriti u `real` vrijednost. 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>todouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Datum i vrijeme


[Prije](#ago) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) |  [dayofyear](#dayofyear) |[datepart](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [sada](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Datum i vrijeme literala

|||
---|---
**Datum i vrijeme**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Vrijeme uvijek su u UTC-u. Ispuštanje datum daje jedan danas.
`now()`|Trenutno vrijeme.
`now(`-*Vremenski raspon*`)`|`now()-`*Vremenski raspon*
`ago(`*Vremenski raspon*`)`|`now()-`*Vremenski raspon*
**Vremenski raspon**|
`2d`|2 dana
`1.5h`|1,5 sat 
`30m`|30 minuta
`10s`|10 sekundi
`0.1s`|0,1 sekundi
`100ms`| od 100 milisekundi
`10microsecond`|
`1tick`|100ns
`time("15 seconds")`|
`time("2")`| 2 dana
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Datum i vrijeme izraza

Izraz |Rezultat
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Manje
`<=`|Manje ili jednako
`>` |Veća
`>=`|Veće ili jednako
`<>`|Nije jednako
`!=`|Nije jednako 




### <a name="ago"></a>Prije

Oduzima navedeni vremenski raspon iz trenutnog vremena sat UTC-a. Kao što su `now()`, ova funkcija može koristiti više puta u naredbi i UTC sat vremena koja se referencira će biti ista za sve instantiations.

**Sintaksa**

    ago(a_timespan)

**Argumenti**

* *a_timespan*: Interval oduzimati od trenutnog vremena sat UTC-a (`now()`).

**Vraća**

    now() - a_timespan

**Primjer**

Svi reci s vremenske oznake u zadnjem satu:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Izdvaja određenog dijela datuma kao cijeli broj.

**Sintaksa**

    datepart(part, datetime)

**Argumenti**

* `part:String`-{"Godina", "Mjesec", "Dan", "H", "Minute", "Drugi", "Od milisekundi", "Microsecond", "Nanosecond"}
* `datetime`

**Vraća**

Dugi koji predstavlja određenog dijela.


### <a name="dayofmonth"></a>dayofmonth

    dayofmonth(datetime("2016-05-15")) == 15 

Redni broj dana u mjesecu.

**Sintaksa**

    dayofmonth(a_date)

**Argumenti**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>dayofweek

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Cijeli broj dana od prethodni nedjelja, kao na `timespan`.

**Sintaksa**

    dayofweek(a_date)

**Argumenti**

* `a_date`: A `datetime`.

**Vraća**

Na `timespan` od ponoći na početku prethodni nedjelja, zaokružuje cijeli broj dana.

**Primjeri**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>dayofyear

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Redni broj dana u godini.

**Sintaksa**

    dayofyear(a_date)

**Argumenti**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday, endofweek, endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getmonth

Broj mjeseca (1 do 12) zatražite od s datumom i vremenom.

**Primjer**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getyear

Dohvaćanje godinu iz s datumom i vremenom.

**Primjer**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>Sada

    now()
    now(-2d)

Trenutno vrijeme UTC sata, po želji pomaka navedeni vremenski raspon. Ova funkcija može koristiti više puta u naredbi i sat vremena koja se referencira bit će na isti način za sve instance.

**Sintaksa**

    now([offset])

**Argumenti**

* *Pomak:* A `timespan`, dodani u trenutno vrijeme sat UTC-a. Zadani: 0.

**Vraća**

Trenutno vrijeme sat UTC-a kao na `datetime`.

    now() + offset

**Primjer**

Određuje razmak nakon događaja otkrije na predikata:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday, startofweek, startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>todatetime

Pseudonim `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Provjerite je li niz ispravan datum:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>totimespan

Pseudonim `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Rezultat cijeli broj predstavlja broj tjedna za ISO 8601 standard. Prvi dan u tjednu nedjelja, a prvi tjedan u godini u tjednu koji sadrži prvi četvrtak te godine. (Posljednjih dana u godini stoga može sadržavati neke od dana u tjednu 1 sljedeće godine, ili prvog dana može sadržavati neke od tjedan 52 ili 53 prethodne godine).


## <a name="string"></a>Niz

[countof](#countof) | [izdvojiti](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [Zamjena](#replace) | [Podjela](#split) | [strcat](#strcat) | [strlen](#strlen) | [podniz](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>Niz literala

Pravila su jednaki onima JavaScript.

Nizovi može biti zatvoren u jednoj ili dvostruko ponudu znakova. 

Kosa crta (`\`) koristi escape znakove kao što su `\t` (kartica) `\n` (čuva) i pojavljivanja na koji umetnute ponudu znakova.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Zatamnjenog niz literala

Zatamnjenog niz literala su nizovi koji će se analize prikriti kada bilježiti niza (na primjer, kada praćenje). Postupak obfuscation zamjenjuje sve zatamnjenog znakova po početka (`*`) znakova.

Da biste obrazac programa zatamnjenog doslovni niz prepend `h` ili "H". Ako, na primjer:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Usporedbama niza

Operator|Opis|Velika i mala slova|Primjer TRUE
---|---|---|---
`==`|Jednako |Da| `"aBc" == "aBc"`
`<>` `!=`|Nije jednako|Da| `"abc" <> "ABC"`
`=~`|Jednako |ne| `"abc" =~ "ABC"`
`!~`|Nije jednako |ne| `"aBc" !~ "xyz"`
`has`|RIGHT-hand strani (RHS) je cijeli izraz u left-hand strani (LHS)|ne| `"North America" has "america"`
`!has`|RHS nije cijeli izraz u LHS|ne|`"North America" !has "amer"` 
`hasprefix`|RHS je prefiks terminu u LHS|ne|`"North America" hasprefix "ame"`
`!hasprefix`|RHS nije prefiks bilo koji izraz u LHS|ne|`"North America" !hasprefix "mer"`
`hassuffix`|RHS je sufiks termin na LHS|ne|`"North America" hassuffix "rth"`
`!hassuffix`|RHS nije sufiks bilo koji izraz u LHS|ne|`"North America" !hassuffix "mer"`
`contains` | RHS odvija se u obliku podniz LHS|ne| `"FabriKam" contains "BRik"`
`!contains`| RHS ne pojavljuju se u LHS|ne| `"Fabrikam" !contains "xyz"`
`containscs` | RHS odvija se u obliku podniz LHS|Da| `"FabriKam" contains "Kam"`
`!containscs`| RHS ne pojavljuju se u LHS|Da| `"Fabrikam" !contains "Kam"`
`startswith`|RHS je početna podniz LHS.|ne|`"Fabrikam" startswith "fab"`
`!startswith`|RHS nije početne podniz LHS.|ne|`"Fabrikam" !startswith "abr"`
`endswith`|RHS je terminal podniz LHS.|ne|`"Fabrikam" endswith "kam"`
`!endswith`|RHS nije terminal podniz LHS.|ne|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS sadrži podudaranje RHS|Da| `"Fabrikam" matches regex "b.*k"`
`in`|Jednako svih elemenata|Da|`"abc" in ("123", "345", "abc")`
`!in`|Nije jednako svih elemenata|Da|`"bc" !in ("123", "345", "abc")`

Korištenje `has` ili `in` ako ste testirati prisutnosti posve Leksički termina – to jest, simbol ili alfanumerički word bounded sadrži znakove koji nisu alfanumerički ili početak ili kraj polja. `has`izvodi brže nego `contains`, `startswith` ili `endswith`. Prvi od tih upita izvodi brže:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Broji pojavljivanja podniz u nizu. Običan niz podudaranja može se s njime preklapati; RegEx podudaranja ne.

**Sintaksa**

    countof(text, search [, kind])

**Argumenti**

* *tekst:* Niz.
* *pretraživanje:* Običan niz ili Uobičajeni izraz tako da odgovara unutar *teksta*.
* *vrstu:* `"normal"|"regex"` Zadani `normal`. 

**Vraća**

Broj niz za pretraživanje možete se podudaraju u spremniku. Običan niz podudaranja može se s njime preklapati; RegEx podudaranja ne.

**Primjeri**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (ne 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>izdvajanje

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Da biste se podudaranje za [Uobičajeni izraz](#regular-expressions) iz tekstnog niza. Po želji ga zatim pretvara izdvojene podniz navedenih vrsta.

**Sintaksa**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumenti**

* *regex:* [Uobičajeni izraz](#regular-expressions).
* *captureGroup:* Na pozitivne `int` konstanti koja označava grupi snimanja za izdvajanje. 0 označava cijeli rezultat, 1 za vrijednost uparuje tako da prvo '("zagrada')' u pravilnim izraz, 2 ili više za narednih zagrada.
* *tekst:* A `string` za pretraživanje.
* *typeLiteral:* Programa literal neobavezno upišite (npr., `typeof(long)`). Ako je navedena izdvojene podniz se pretvara u ovu vrstu. 

**Vraća**

Ako *regex* traži podudarnost u *tekstu*: podniz podudaraju u odnosu na navedeni snimanje grupe *captureGroup*, možete pretvoriti u *typeLiteral*.

Ako ne postoji podudarnost ili ne uspije pretvorbe vrsta: `null`. 

**Primjeri**

Primjer niz `Trace` se pretražuje definicija za `Duration`. Podudaranje se pretvara u `real`, zatim množi konstantu vrijeme (`1s`) tako da `Duration` vrste `timespan`. U ovom primjeru je jednako 123.45 sekundi:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

U ovom se primjeru jednako `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>IsEmpty isnotempty, notempty

    isempty("") == true

TRUE ako je argument je bio prazan niz ili null.
Vidi također [isnull](#isnull).


**Sintaksa**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Vraća**

Označava je li argument prazan niz ili isnull.

|x|IsEmpty(x)
|---|---
| "" | TRUE
|"x" | FALSE
|parsejson("")|TRUE
|parsejson("[]")|FALSE
|parsejson("{}")|FALSE


**Primjer**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>parseurl

Podijelite njezin dio URL-a.

**Sintaksa**

    parseurl(urlstring)

**Argumenti**

* *urlstring:* URL-A.

**Vraća**

Objekt koji sadrži dijelove kao niz.

**Primjer**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>Zamjena

Zamijenite sve parove regex drugog niza.

**Sintaksa**

    replace(regex, rewrite, text)

**Argumenti**

* *regex:* [Uobičajeni izraz](https://github.com/google/re2/wiki/Syntax) za pretraživanje *teksta*. Može sadržavati snimanje grupe u "("zagrade')". 
* *dopune:* Zamjena regex za podudaranje načinio *matchingRegex*. Korištenje `\0` da biste se pozvali cijeli podudaranje `\1` za prvu grupu snimanje `\2` i tako dalje za kasnije snimanje grupe.
* *tekst:* Niz.

**Vraća**

*tekst* iza zamjenjuje sve parove *regex* procjene *dopune*. Rezultati se preklapaju.

**Primjer**

Izjavu o zaštiti:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

Sadrži sljedeće rezultate:

| x    | str | zamjenjuje|
|---|---|---|
| 1    | Broj je 1.000000  | Broj je: 1.000000|
| 2    | Broj je 2.000000  | Broj je: 2.000000|
| 3    | Broj je 3.000000  | Broj je: 3.000000|
| 4    | Broj je 4.000000  | Broj je: 4.000000|
| 5    | Broj je 5.000000  | Broj je: 5.000000|
 



### <a name="split"></a>Podjela

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Dijeli navedeni niz prema danu razdjelnika i vraća niz polja s podnizova conatined. Po želji mogu biti određene podniz vraćenu ako postoji.

**Sintaksa**

    split(source, delimiter [, requestedIndex])

**Argumenti**

* *izvor*: izvorišnog niza koji će biti razdijeljenog prema danu graničnik.
* *graničnik*: graničnik koji će se koristiti da bi se podijeliti izvorišnog niza.
* *requestedIndex*: neobavezno polje index `int`. Ako je navedena polja vraćeni niz sadrži traženu podniz ako postoji. 

**Vraća**

Niz polja koja sadrži podnizovi dane izvorišne niza koji su zarezima prema danu graničniku.

**Primjeri**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Spaja između 1 i 16 argumente koji se moraju biti nizova.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

Duljina niza.

### <a name="substring"></a>podniz

    substring("abcdefg", 1, 2) == "bc"

Izdvoji podniz iz određene izvorišne niza počevši od Dani indeks. Po želji duljinu tražene podniz možete navesti.

**Sintaksa**

    substring(source, startingIndex [, length])

**Argumenti**

* *izvora:* Izvor niz koji će uzeti podniz.
* *startingIndex:* Polje početni položaj znaka traženi podniz.
* *Duljina:* Neobavezan parametar koji se mogu koristiti za određivanje traženi broj znakova u podniz. 

**Vraća**

Podniz iz navedene niza. Podniz započinje položaj znaka startingIndex (polje) i nastavljaju se do kraja niza ili duljina znakove ako je naveden.

**Primjeri**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>tolower

    tolower("HELLO") == "hello"

Pretvara u mala slova.

### <a name="toupper"></a>toupper

    toupper("hello") == "HELLO"

Pretvara u velika slova.



### <a name="guids"></a>GUID

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Polja, objekata i dinamičkih

[literala](#dynamic-literals) | [casting](#casting-dynamic-objects) | [operatora](#operators) | [omogućuju rečenice](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [raspon](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [poštanski broj](#zip)


Evo i rezultata upita iznimku uvida aplikacije. Vrijednost u `details` je polje.

![](./media/app-insights-analytics-reference/310.png)

**Indeksiranje:** Indeks polja i objekata kao i JavaScript:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* No, upotrijebite `arraylength` i druge funkcije analize (nije ".length"!)

**Koji bacaju** U nekim slučajevima je potrebno anketu element koji izdvojiti iz objekta, jer vrsta njegova može razlikovati. Na primjer, `summarize...to` mora određenu vrstu:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Literala** Da biste stvorili eksplicitnih polja ili spremnika svojstava objekta, napišite ga kao niz JSON i glumcima:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:** Da biste se uvesti razdvojite svojstva objekta u zaseban redak, koristite mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:** Da biste pronašli sve putove složene objekta:

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:** Da biste pronašli minimalne sheme pristupiti sve vrijednosti parametra expression u tablici:

    exceptions | summarize buildschema(details)

Rezultat:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Primijetit ćete da `indexer` se koristi da biste označili gdje treba koristiti numerički indeks. Za ovu shemu neke valjani putova će biti (Ako su tih primjer indeksa u rasponu):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Polja i objekt literala

Da biste stvorili dinamičnu literal, koristite `parsejson` (pseudonim `todynamic`) s na JSON niza argumenta:

* `parsejson('[43, 21, 65]')`-polja brojeva
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-jednu vrijednost dinamički vrste koji sadrži broj
* `parsejson('"21"')`-jednu vrijednost dinamički vrste koji sadrži niz

Napomena da, za razliku od JavaScript, JSON mandates pomoću dvostruke navodnike (`"`) oko nizova. Stoga je lakše ponuda niz kodiran JSON-om literala pomoću jednostruko navodnika (`'`).

U ovom se primjeru stvara dinamičke vrijednosti, a potom koristi njegova polja:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Funkcija dinamički objekta

|||
|---|---|
| *vrijednost* `in` *polja*| TRUE ako postoji element *polja* koja == *vrijednost*<br/>`where City in ('London', 'Paris', 'Rome')`
| *vrijednost* `!in` *polja*| TRUE ako postoji bez element *polja* koja == *vrijednost*
|[`arraylength(`polja`)`](#arraylength)| Null Ako još nije polja.
|[`extractjson(`put, objekta`)`](#extractjson)|Koristi put da biste prešli u objekt.
|[`parsejson(`Izvor`)`](#parsejson)| Pretvara niz JSON u dinamičku objekta.
|[`range(`šalje, prima korak`)`](#range)| Polje s vrijednostima
|[`mvexpand`listColumn](#mvexpand-operator) | Replicira redak za svaku vrijednost na popisu u određene ćelije.
|[`summarize buildschema(`stupac`)`](#buildschema) |Izvodi shema vrste sadržaja stupca
|[`summarize makelist(`stupac`)`](#makelist)| Poravnava grupe redaka i stavlja vrijednosti stupca u polju.
|[`summarize makeset(`stupac`)`](#makeset) | Poravnava grupe redaka i stavlja vrijednosti u stupcu u polju, bez dupliciranje.

### <a name="dynamic-objects-in-let-clauses"></a>Dinamični objekata u omogućite rečenice


[Omogućuju rečenice](#let-clause) spremište dinamičke vrijednosti kao nizovi, stoga ove dvije rečenice se ne razlikuju i oba potrebna na `parsejson` (ili `todynamic`) prije korištenja:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

Broj elemenata u dinamičku polja.

**Sintaksa**

    arraylength(array)

**Argumenti**

* *polja:* A `dynamic` vrijednost.

**Vraća**

Broj elemenata u *polje*ili `null` ako je *argument polje* ne polja.

**Primjeri**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Pronađite navedeni element izvan JSON tekst koji se koristi izraz puta. Po želji Pretvorba izdvojene niza na određenu vrstu.


**Sintaksa**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Vraća**

Ova funkcija izvodi JsonPath upita u izvor podataka koji sadrži valjani niz JSON, po želji pretvaranje tu vrijednost u neku drugu vrstu ovisno o trećeg argumenta.



**Primjer**

Označavanje [uglatu zagradu]] i notacija se ne razlikuju:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Savjeti za performanse**

* Primjena gdje rečenice prije korištenja`extractjson()`
* Razmislite o korištenju Uobičajeni izraz podudaranja s [Izdvoji](#extract) . To će se izvoditi brže vrlo slično i aktivan ako je na JSON proizvodi iz predloška.
* Korištenje `parsejson()` ako vam je potrebna da biste izdvojili više od jedne vrijednosti iz na JSON.
* Preporučujemo da imate JSON raščlaniti pri ingestion po deklariranje vrsta stupca se dinamički.

### <a name="json-path-expressions"></a>Put JSON izraza

|||
|---|---|
|`$`|Korijenski objekt|
|`@`|Trenutni objekt|
|`[0]`|Indeks polja|
|`.`ili`[0]` | Podređeni|

*(Ne možemo ne trenutno implementirati zamjenske znakove, rekurzije, unije ili isječke.)*




### <a name="parsejson"></a>parsejson

Tumači na `string` kao [JSON vrijednost](http://json.org/)) i vraća vrijednost kao `dynamic`. To je pomoću `extractjson()` kada morate omogućiti izdvojiti više od jednog elementa JSON složenih objekta.

**Sintaksa**

    parsejson(json)

**Argumenti**

* *json:* JSON dokument.

**Vraća**

Objekt vrste `dynamic` određen *json*.

**Primjer**

U sljedećem primjeru kada `context_custom_metrics` je na `string` koji izgleda ovako: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

a zatim sljedeće dio dohvaća vrijednost u `duration` vremensko razdoblje u objektu, kao i iz da dohvaća dva slobodnih `duration.value` i  `duration.min` (`118.0` i `110.0`, odnosno).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>raspon

Na `range()` funkcija (ne treba zbuniti s na `range` operator) generira dinamički polja držite niza ravnomjerno raspoređene vrijednosti.

**Sintaksa**

    range(start, stop, step)

**Argumenti**

* *start:* Vrijednost prvi element u rezultata polja. 
* *Zaustavi:* Vrijednost od posljednjeg elementa u rezultat polja ili najmanjih vrijednosti veće od posljednjeg elementa u rezultat polja i unutar cijeli više *koraka* iz *start*.
* *korak:* Razlika između dva uzastopna elementima polja.

**Primjeri**

Sljedeći primjer vraća `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

Sljedeći primjer vraća polja držite cijelog naziva dana u godini 2015:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Pretvara u dinamičku vrijednost.

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

Zbraja izraza put koje označavaju ostavlja dinamički objekta. 

**Vraća**

Polje put izraza.

**Primjeri**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Napomena taj "[0]" označava prisutnosti polja, ali odrediti indeks koji se koristi po određenom putu.

### <a name="zip"></a>Poštanski broj

    zip(list1, list2, ...)

Spaja skup popisa u jedan popis redni.

* `list1...`: Popis vrijednosti

**Primjeri**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Nazivi

Nazivi mogu biti najviše 1024 znakova. Velika i mala slova i im može sadržavati slova, brojki i podvlake (`_`). 

Ponuda naziv pomoću ["... '] or [" ... "] da biste uvrstili drugih znakova ili koristite ključnu riječ kao njihov naziv. Ako, na primjer:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|["put\\file\n\'x\'"] | Korištenje \ escape u znakova|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Nema escapes - \ je doslovni|
|[@"\now& zatim\"] | |
|[mjesta] | Pomoću jezika keyword naziv|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


