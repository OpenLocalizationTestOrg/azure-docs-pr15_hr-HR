<properties
    pageTitle="Prijava analitiku pretraživanje referenca | Microsoft Azure"
    description="Analitički zapisnika pretraživanja referenca opisuje jezik za pretraživanje i nudi sintaksu upita Općenite mogućnosti možete koristiti pri pretraživanju podataka i filtriranje izraze kako biste suzili pretraživanje."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="log-analytics-search-reference"></a>Prijava analitiku pretraživanje reference

U sljedećoj sekciji referenca o jezik za pretraživanje opisuju mogućnosti sintaksa Općenito upita možete koristiti pri pretraživanju podataka i filtriranje izraze kako biste suzili pretraživanje. Opisuje i naredbe koje možete koristiti akcijama dohvatiti podatke.

Čitajte polja u pretraživanja i pozornici koje olakšavaju dill-u slične kategorije podataka u [polje za pretraživanje i fasete referencu sekcije](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Sintaksa Općenito upita

Sintaksa:

```
filterExpression | command1 | command2 …
```

Izraz filter (`filterExpression`) definira uvjet "gdje" za upit. Naredbe primjenjuju se na rezultata koji je vratio upit. Više naredbi moraju biti razdvojeni znakom okomite crte (|).

### <a name="general-syntax-examples"></a>Primjeri sintakse Općenito

Primjeri:

```
system
```

Ovaj upit vraća rezultate koje sadrže riječ "sustav" u bilo kojem polju koje indeksirano za cijeli tekst ili uvjeta pretraživanja.

>[AZURE.NOTE] Sva polja ne indeksiraju na taj način, ali najčešće tekstnih polja (kao što su opisi i nazivi) obično bio bi.

```
system error
```

Ovaj upit vraća rezultate koji sadrže riječi *sustava* i *pogreške*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Ovaj upit vraća rezultate koji sadrže riječi *sustava* i *pogreške*. Zatim se sortira rezultata prema polju *ManagementGroupName* (uzlaznim redoslijedom), a zatim *TimeGenerated* (silaznim redoslijedom). Uzima samo prvih 10 rezultate.

>[AZURE.IMPORTANT] Svi nazivi polja i vrijednosti za polja niza i tekst se velika i mala slova.

## <a name="filter-expression"></a>Izraz filtra

Pododlomci koji slijede objašnjavaju izraze za filtar.

### <a name="string-literals"></a>Niz literala

Doslovni niz je bilo koji niz koji se analizator ne prepoznaje kao ključnu riječ ili vrstu unaprijed definiranih podataka (na primjer, broj ili datum).

Primjeri:

```
These all are string literals
```

Ovaj upit traži rezultate koji sadrže pojavljivanja sve pet riječi. Za izvođenje složenih niz pretraživanja, primjerice stavite doslovni navodnim znakovima, niz:

```
" Windows Server"
```

To je samo vraća rezultate s točna podudaranja za "Windows Server".

### <a name="numbers"></a>Brojevi

Na analizator podržava decimalni cijeli broj i s pomičnim zarezom broj sintaksa za brojčanih polja.

Primjeri:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="datetime"></a>Datuma/vremena

Svaki dio podataka u sustavu ima svojstvo *TimeGenerated* , koji predstavlja izvornog datuma i vremena zapisa. Neke vrste podataka uz to može imati više polja datuma/vremena (na primjer, *LastModified*).

Birač grafikona i vremena vremenske crte u prijava analitiku prikazuje distribucija rezultata s vremenom (prema trenutni upit pokrenut), na temelju *TimeGenerated* polja. Datumu/vremenu polja s oblikovanjem određeni niz koji se mogu koristiti u upitima da biste ograničili upit određenog vremenskog razdoblja. Sintaksa možete koristiti i da biste se pozvali na relativnu vremenske intervale (na primjer, "između 3 dana i prije 2 sati").

Sintaksa:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Primjer:

```
TimeGenerated:2013-10-01T12:20
```

Naredba prethodne vraća samo zapise s vrijednošću *TimeGenerated* točno 12:20 na listopadu 1 2013. Je vjerojatno da dat će bilo koji rezultat, ali shvatiti ideju.

Na analizator podržava i mnemonic vrijednost datuma i vremena ODMAH.


Ponovno je vjerojatno da će to yield bilo koji rezultat jer podataka ne bi u sustavu to brzo.

U ovim se primjerima su sastavni blokovi namijenjen relativne i apsolutne datuma. U sljedeća tri Pododlomci koji smo ćete objašnjavaju kako ih koristiti u dodatne filtre na primjerima koji koriste relativni datumskih raspona.

### <a name="datetime-math"></a>Matematika datuma/vremena

Matematičke operatore datuma/vremena pomoću pomak ili zaokružiti vrijednost datuma/vremena pomoću jednostavnih izračuna datuma/vremena.

Sintaksa:

```
datetime/unit
```

```
datetime[+|-]count unit
```

|Operator|Opis|
|---|---|
|/|Zaokružuje datuma i vremena za navedenu jedinicu. Primjer: SADA / dan zaokružuje trenutnog datuma/vremena u ponoći trenutni dan.|
|+ ili -|Pomiče datuma/vremena određeni broj jedinica. Primjeri: SADA + 1 sat pomiče trenutnog datuma/vremena po jedan sat ahead.2013-10-01T12:00-10 dana pomiče vrijednost datuma natrag za 10 dana.|



Možete povezati u lanac matematičke operatore datuma/vremena zajedno, na primjer:

```
NOW+1HOUR-10MONTHS/MINUTE
```

U sljedećoj su tablici navedeni podržani jedinica datuma/vremena.

Jedinica datuma/vremena|Opis
---|---
YEAR, GODINE|Zaokružuje trenutne godine ili pomiče određeni broj godina.
MJESEC, MJESECI|Zaokružuje trenutni mjesec ili pomake po navedenog broja mjeseci.
DNEVNO, DATUM|Zaokružuje trenutni dan u mjesecu ili pomiče određeni broj dana.
SAT, SATI|Zaokružuje trenutnog sata ili pomiče određeni broj sati.
MIN, MINUTA|Zaokružuje trenutni minutu ili pomiče određeni broj minuta.
DRUGO, SEKUNDI|Zaokružuje na drugo ili pomiče određeni broj sekundi.
OD MILISEKUNDI MILISEKUNDAMA, MILLI, MILLIS|Zaokružuje od trenutnog milisekundi ili pomiče određeni broj milisekundi.


### <a name="field-facets"></a>Polje pozornici

Pomoću polja pozornici možete odrediti uvjeta pretraživanja za određena polja i njihove točno vrijednosti, umjesto pisanje "besplatne tekst" upita za različite uvjete cijeloj indeksa. Ne možemo ste već koristili ove sintakse u nekoliko primjera u prethodno odlomaka. Ovdje, dajemo složenije primjere.

**Sintaksa**

```
field:value
```

```
field=value
```

**Opis**

Pretražuje polje za određenu vrijednost. Vrijednost može biti slovni niz, broja ili datuma/vremena.

Primjer:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Sintaksa**

*polje > vrijednost*

*polje < vrijednost*

*polje > = vrijednost*

*polje < = vrijednost*

*polje! = vrijednost*

**Opis**

Nudi usporedbu.

Primjer:

```
TimeGenerated>NOW+2HOURS
```

**Sintaksa**

```
field:[from..to]
```

**Opis**

Nudi faceting raspona.

Primjer:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="logical-operators"></a>Logički operatori

Upit jezike podržava logički operatori (*i*, *ili*i *ne*) i njihovih C stil pseudonima (*&&*, *||*, a *!*) odnosno. Zagrade možete koristiti da biste grupirali sljedeće operatore.

Primjeri:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Možete izostaviti logičkih operatora za argumente najviše razine filtar. U ovom slučaju operator i pretpostavlja se da je.


Izraz filtra|Ekvivalent u
---|---
Pogreška sustava|sustav i pogreške
sustav "Windows Server" ili težinu: 1|sustav i ("Windows Server" ili težinu: 1)

### <a name="wildcarding"></a>Wildcarding

Podržava jezik upita u (*\*) znak za predstavljanje jedan ili više znakova za vrijednosti u upitu.

Primjeri:

 Pronađite svim računalima s "SQL" u nazivu, primjerice "Redmond SQL".

```
Type=Event Computer=*SQL*
```

>[AZURE.NOTE] Zamjenski znakovi nije moguće koristiti unutar citatima danas. Poruka =`"*This text*"` ćemo dodavanje razmotriti u (\*) koristi se kao na literal (\*) znakova.
## <a name="commands"></a>Naredbe

Naredbe primjenjuju se na rezultati koje je vratio upit. Koristite znak okomite crte (|) da biste primijenili naredbu dohvaćene rezultate. Više naredbi moraju biti razdvojeni znakom kanala.

>[AZURE.NOTE] Nazive naredbi možete staviti u velika slova ili mala slova, za razliku od naziva polja i podatke.

### <a name="sort"></a>Sortiranje

Sintaksa:

    sort field1 asc|desc, field2 asc|desc, …

Sortiranje rezultata prema određenom polja. Asc/desc prefiks nije obavezno. Ako se izostavi, pretpostavlja se da je redoslijed sortiranja *asc* . Ako je upit pomoću naredbe za *Sortiranje* izričito, sortiranje **TimeGenerated** desc zadano ponašanje i će uvijek prvo vratiti najnovije rezultate.

### <a name="toplimit"></a>Vrh/ograničenje

Sintaksa:

    top number


    limit number
Ograničava odgovor na vrhu N rezultate.

Primjer:

    Type:Alert errors detected | top 10

Vraća 10 odgovarajuće rezultate.

### <a name="skip"></a>Preskoči

Sintaksa:

    skip number

Preskače broj rezultata na popisu.

Primjer:

    Type:Alert errors detected | top 10 | skip 200

Vraća gornji 10 odgovarajućih rezultata počevši od 200 rezultata.

### <a name="select"></a>Odaberite

Sintaksa:

    select field1, field2, ...

Ograničenja rezultate polja koje odaberete.

Primjer:

    Type:Alert errors detected | select Name, Severity

Ograničava opseg vraćenih rezultata polja za *ime* i *težinu*.

### <a name="measure"></a>Mjera

Naredba *mjere* koristi se za primjenu statističke funkcije neobrađenog rezultati. Ovo je vrlo koristan da biste dobili prikaza *Grupiraj po* nad podacima. Kada koristite naredbu *mjere* , analize zapisnika pretraživanja prikazuje tablice s rezultatima zbroja.

Sintaksa:

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Zbrajaju rezultati prema *groupField* i izračunava vrijednosti zbroja mjere pomoću *aggregatedField*.


|Mjerenje statističke funkcije|Opis|
|---|---|
|*aggregateFunction*|Naziv funkcije zbrajanja (slova). Podržane su sljedeće funkcije zbrajanja: zbroj MAX MIN zbroj AVG STDDEV COUNTDISTINCT PERCENTIL ## ili Postotak ## (## je bilo koji broj između 1 i 99)|
|*aggregatedField*|Polje koje se pridružuje. To polje nije obavezno za funkciju zbrajanja COUNT, ali mora biti postojeće numeričko polje za ZBRAJANJE, MAX, MIN, AVG STDDEV ili PERCENTIL ## ili Postotak ## (## je bilo koji broj između 1 i 99). Na aggregatedField može biti bilo koji od funkcije Proširi podržane.|
|*fieldAlias*|(Neobavezno) pseudonim za izračunate agregatnu vrijednost. Ako nije naveden, naziv polja bit će AggregatedValue.|
|*groupField*|Naziv polja skup rezultata grupirane.|
|*Interval*|Razdoblje u obliku:**nnnNAME** gdje: nnn je broj pozitivan cijeli broj. **Naziv** je naziv interval. Podržani interval nazivi sadrže (velika i mala slova): od MILISEKUNDI [S] drugi [S] MINUTE [S] dan sat [S] [S] MJESEC [S] godine [S]|


Mogućnost interval može se koristiti samo u grupirati polja datuma/vremena (kao što su *TimeGenerated* i *TimeCreated*). Trenutno ne nameće servis, ali polje bez datuma/vremena koja se prosljeđuje pozadinski će uzrokovati pogreške prilikom izvođenja. Kada je provjera valjanosti sheme implementirana, API servisa odbacuje upite koji koriste polja bez datuma i vremena za interval zbrajanja. Trenutni implementaciju *mjere* podržava interval grupiranja za bilo koju funkciju zbrajanja.

Ako je ispušten BY uvjet, ali interval je navedeno (kao što je drugi sintaksa), polje *TimeGenerated* pretpostavlja se da je prema zadanim postavkama.

Primjeri:

**Primjer 1**

    Type:Alert | measure count() as Count by ObjectId

*Objašnjenje*

Grupira upozorenja putem *ID objekta* i izračunava broj upozorenja za svaku grupu. Vraća agregatnu vrijednost kao polje *Count* (pseudonim).

**Primjer 2**

    Type:Alert | measure count() interval 1HOUR

*Objašnjenje*

Grupa upozorenja prema 1 sat intervalima pomoću polja *TimeGenerated* i vraća broj upozorenja u svakom razdoblju.

**Primjer 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

*Objašnjenje*

Isto kao u prethodnom primjeru, no pomoću pseudonima Zbrojeno polje (*AlertsPerHour*).

**Primjer 4**

    * | Izmjerite count() po TimeCreated interval 5DAYS

*Objašnjenje*

Grupa rezultate prema intervalima 5 dana pomoću polja *TimeCreated* i vraća broj rezultata u svakom razdoblju.

**Primjer 5**

    Type:Alert | measure max(Severity) by WorkflowName

*Objašnjenje*

Grupira upozorenja prema nazivu radno opterećenje i vraća vrijednost Maksimalna težinu upozorenja za svaki tijek rada.

**Primjer 6**

    Type:Alert | measure min(Severity) by WorkflowName

*Objašnjenje*

Isto kao u prethodnom primjeru, no s *Min* pridružuje (opis funkcije).

**Primjer 7**

    Type:Perf | measure avg(CounterValue) by Computer

*Objašnjenje*

Grupira mjerača performansi na računalu i izračunava prosječnu vrijednost (avg).

**Primjer 8**

    Type:Perf | measure sum(CounterValue) by Computer

*Objašnjenje*

Isto kao u prethodnom primjeru, no koristi *zbroj*.

**Primjer 9**

    Type:Perf | measure stddev(CounterValue) by Computer

*Objašnjenje*

Isto kao u prethodnom primjeru, no koristi *STDDEV*.

**Primjer 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

*Objašnjenje*

Isto kao u prethodnom primjeru, ali koristi *PERCENTILE70*.

**Primjer 11**

    Type:Perf | measure pct70(CounterValue) by Computer

*Objašnjenje*

Isto kao u prethodnom primjeru, ali koristi *PCT70*. Imajte na umu da *Postotak ##* samo na pseudonima *PERCENTIL ##* funkcije.

**Primjer 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

*Objašnjenje*

Najprije grupira mjerača performansi na računalu i zatim CounterName i izračunava prosječnu vrijednost (avg).

**Primjer 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

*Objašnjenje*

Dohvaća gornji pet tijekovi rada s maksimalan broj upozorenja.

**Primjer 14**

    * | Izmjerite countdistinct(Computer) po vrsti

*Objašnjenje*

Broji jedinstvene računala izvješćivanja za svaku vrstu.

**Primjer 15**

    * | Izmjerite countdistinct(Computer) Interval 1 sat

*Objašnjenje*

Broji jedinstvene računala navedeno za svaki sat.

**Primjer 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

*Objašnjenje*

Grupa % procesor vrijeme na računalu, a vraća prosječnu vrijednost za svaku 1 sat.

**Primjer 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

*Objašnjenje*

Grupa W3CIISLog metodom i vraća najveći svakih 5 minuta.

**Primjer 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

*Objašnjenje*

Grupe % procesor vrijeme na računalu, a vraća minimalne, prosjek, 75 percentil i maksimalne za svaku 1 sat.

**Primjer 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

*Objašnjenje*

Grupe % procesor vrijeme najprije na računalu, a zatim po naziv Instance i vraća minimalne, prosjek, 75 percentil i maksimalne za svaki sat

**Primjer 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

*Objašnjenje*

Izračunava maksimalno diska zapisuju u minuti za svaku disk na računalu

### <a name="where"></a>Gdje

Sintaksa:

```
**where** AggregatedValue>20
```

Može se koristiti samo nakon *mjere* naredbe da biste dodatno filtrirali Zbrojeno rezultate koji je sastavio funkciju zbrajanja *mjere* .

Primjeri:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)

### <a name="in"></a>U

Sintaksa:

```
(Outer Query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Opis: Ova sintaksa omogućuje stvaranje prikupljanja i sažetak sadržaja popisa vrijednosti s tog zbrajanja u drugi vanjski pretraživanja (primarni) koja će izgledati za događaje te vrijednošću. To možete učiniti tako umetnute unutarnji pretraživanja u vitičaste zagrade, a Umetanje rezultate kao mogućih vrijednosti za polje u vanjskog pretraživanja za početak rada.

Unutarnji upita primjer: *računala trenutno nema sigurnosna ažuriranja* s sljedeći upit zbrajanja:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Nalikovat će tablici konačni upit koji pronalazi *sve događaja sustava Windows za računala trenutno nema sigurnosna ažuriranja* :

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="dedup"></a>Dedup

**Sintaksa**

    Dedup FieldName

**Opis** Vraća prvi dokument za svaku jedinstvene vrijednosti navedene polja.

**Primjer**

    Type=Event | sort TimeGenerated DESC | Dedup EventID

Gornji primjer vraća jedan događaj (najnoviji jer koristimo DESC na TimeGenerated) po Iddogađaja


### <a name="extend"></a>Proširivanje

**Opis** Omogućuje stvaranje izvođenju polja u upitima. Naredba mjere možete koristiti i nakon Proširi ako želite izvesti zbrajanja.

**Primjer 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Prikaži rezultat ponderiranog preporuke za procjenu SQL preporuke

**Primjer 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Prikazuje vrijednost brojač KBs umjesto bajtova

**Primjer 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Promjena veličine vrijednost WireData TotalBytes tako da se svi rezultati biti između 0 i 100.

**Primjer 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Označavanje mjerača performansi brojač vrijednosti manje od 50% las NAJNIŽA, a drugi kao VISOKO

**Primjer 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Izračunava maksimalno diska zapisuju u minuti za svaku disk na računalu

**Podržanih funkcija**


| Funkcija | Opis | Primjeri sintakse |
|---------|---------|---------|
| Abs | Vraća apsolutnu vrijednost navedena vrijednost ili funkciju. | `abs(x)` <br> `abs(-5)` |
| ACOS | Vraća kosinus luk vrijednost ili funkcija | `acos(x)` |
| i | Ako i samo ako svi operandi vrednovani kao true, vraća se vrijednost true. | `and(not(exists(popularity)),exists(price))` |
| ASIN | Vraća sinus luk vrijednost ili funkcija | `asin(x)` |
| ATAN | Vraća tangens luk vrijednost ili funkciju | `atan(x)` |
| ATAN2 |  Vraća kut dobivenima pretvorbe rectangular koordinate x, y za polar koordinata. | `atan2(x,y)` |
| cbrt | Treći korijen |  `cbrt(x)` |
| ceil | Zaokružuje cijeli broj | `ceil(x)`  <br> `ceil(5.6)`-Vraća 6 |
| cos | Vraća kosinus zadanog kuta. | `cos(x)` |
| COSH | Vraća hiperbolni kosinus kuta. | `cosh(x)` |
| Zadani | Zadani je kratica za zadano. Vraća vrijednost "polje" ili, ako polje ne postoji, vraća navedena zadana vrijednost i rezultira prva vrijednost gdje: `exists()==true`. | `def(rating,5)`-Vraća ocjena Ova funkcija def() ili ocjenu navedene u dokumentu, vraća 5 <br> `def(myfield, 1.0)`-jednako`if(exists(myfield),myfield,1.0)` |
| deg | Pretvaranje radijana u stupnjeve |  `deg(x)` |
| DIV | `div(x,y)`dijeli x, y. | `div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| DIST | Vraća udaljenost između dva vektori, (točke) n dimenzionalni prostor. Vodi na potenciju plus dva ili više instanci ValueSource i izračunava udaljenost između dva vektori. Svaki ValueSource mora biti broj. Mora biti paran broj instanci ValueSource proslijeđen i metodu pretpostavlja da prvi pola predstavljaju prvom vektoru i drugoj polovici predstavljaju drugi vektorski.  | `dist(2, x, y, 0, 0)`-Izračunava between,(0,0) Euclidean udaljenost i (x, y) za svaki dokument. <br> `dist(1, x, y, 0, 0)`-Izračunava Manhattan (taxicab), udaljenost između (0,0) i (x, y) za svaki dokument. <br> `dist(2,,x,y,z,0,0,0)`-Euclidean udaljenost između (0,0,0) i (x, y, z) za svaki dokument.<br>`dist(1,x,y,z,e,f,g)`-Manhattan udaljenost između (x, y, z) i (e, f, g), gdje je svakog slova naziva polja. |
| postoji | Vraća TRUE ako je bilo koji član polje postoji. | `exists(author)`-Vraća TRUE za neki dokument ima vrijednost u polju "autor".<br>`exists(query(price:5.00))`-Vraća vrijednost TRUE ako je udovoljava "cijena", "5.00". |
| Exp | Vraća broj Eulerova korisnika na potenciju x | `exp(x)` |
| FLOOR | Zaokružuje prema dolje na cijeli broj | `floor(x)`  <br> `floor(5.6)`-Vraća 5 |
| Hypo | Vraća sqrt(sum(pow(x,2),pow(y,2))) bez Srednja prelijevanja ili manje vrijednosti | `hypo(x,y)`  <br> ` |
| IF | Omogućuje uvjetno funkcija upita. U `if(test,value1,value2)` -Test je ili se odnosi na logičku vrijednost ili izraz koji vraća logičku vrijednost (TRUE ili FALSE).  `value1`je vrijednost koja je vratila funkcija ako testiranje rezultira TRUE. `value2`je vrijednost koja je vratila funkcija ako testiranje rezultira FALSE. Izraz može biti bilo koju funkciju koja proizvodi Booleove vrijednosti ili čak i funkcije vraćanja brojčane vrijednosti, u kojem će se slučaja vrijednost 0 interpretiraju kao false ili niza u tom slučaju prazan niz tumači kao false. | `if(termfreq(cat,'electronics'),popularity,42)`-Ova funkcija provjerava svakom dokumentu za se sadrži li izraz "elektroničkom" u polju mačka. Ako je tako, zatim vraća se vrijednost polja popularnosti, u suprotnom vrijednost od 42 vraća se. |
| Linearni | Primjenjuje `m*x+c` gdje m i c konstante i proizvoljne funkcija je x. To je jednako `sum(product(m,x),c)`, ali malo učinkovitiji kao implementira se kao jedan funkcija. | `linear(x,m,c) linear(x,2,4)`Vraća`2*x+4` |
| Funkcija ln| Vraća prirodnim zapisnik navedena funkcija |  `ln(x)` |
| zapisnik | Vraća zapisnik baza 10 navedena funkcija. | `log(x)   log(sum(x,100))` |
| karte | Karte sve vrijednosti unos funkcija x koji ulaze unutar minimum i maksimum, uključujući te dvije vrijednosti u navedeni cilj. Argumenti min i max mora biti konstante. Ciljna argumenata i zadane može biti konstante ili funkcije. Ako je vrijednost x se nalaziti između minimum i maksimum, zatim vraća vrijednost x ili zadanu vrijednost koja se vraća ako je naveden kao argument 5. |  `map(x,min,max,target) map(x,0,0,1)`– Mijenja sve vrijednosti 0 i 1. To može biti korisno obrade 0 zadane vrijednosti.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`– Mijenja sve vrijednosti između 0 i 100 do 1, a sve druge vrijednosti -1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`– Mijenja sve vrijednosti između 0 i 100 da biste x + 599 i sve druge vrijednosti učestalost termina solr u polje tekst. |
| Maksimalni | Vraća maksimalnu numeričku vrijednost većeg broja ugniježđene funkcije ili konstante koji su navedeni kao argumenti: `max(x,y,...)`. Maksimalni funkcije mogu biti korisne za "bottoming out" drugu funkciju ili navedeno polje na neke konstantu.  Korištenje na `field(myfield,max)` sintaksa za odabir najveću vrijednost od jednog polja s više vrijednosti.  | `max(myfield,myotherfield,0)` |
| min | Vraća najmanju numeričku vrijednost više ugniježđenih funkcija konstante koji su navedeni kao argumenti: `min(x,y,...)`. Funkcija min može biti korisno za pružanje "gornja granica" na funkcija koristi konstantu. Korištenje na `field(myfield,min)` sintaksa za odabir minimalnu vrijednost od jednog polja s više vrijednosti. | `min(myfield,myotherfield,0)` |
| Mod | Formula izračunava modul funkciju x y (opis funkcije). |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))`   |
| MS | Vraća milisekundama razlika između svoje argumente. Datumi su odnosu Unix ili POSIX epoch vrijeme, ponoći, siječanj 1, 1970 UTC-a. Argumenti mogu biti naziv indeksiranog TrieDateField ili Matematika datum koji se temelji na konstantnim datum ili sad. `ms()`jednako `ms(NOW)`, broj u milisekundama od na epoch. `ms(a)`Vraća broj milisekundama od epoch koji predstavlja argument. `ms(a,b)`Vraća broj milisekundi te b ranije, koji je `a - b`.| `ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| ne | Logički negiranih vrijednost funkcije prelomljeni. | `not(exists(author))`-TRUE samo kada `exists(author)` je vrijednost false. |
| ili | Logička disjunction. | `or(value1,value2)`– Vrijednost TRUE ako je istinito vrijednost1 ili vrijednost2. |
| pow | Podiže navedenoj bazi navedeni potenciju. `pow(x,y)`Podiže x na potenciju y. |  `pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`-Jednaki sqrt. |
| proizvoda | Vraća umnožak od više vrijednosti ili funkcije koji su navedeni na popisu odvojenih zarezom. `mul(...)`mogu se koristiti kao pseudonim za ovu funkciju. | `product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip | Izvodi recipročna funkcija s `recip(x,m,a,b)` implementacijom `a/(m*x+b)` gdje m, a, b konstante, a x je bilo koji arbitrarily complex (funkcija). Kada je b nisu jednake i x > = 0, ova funkcija je maksimalna vrijednost 1 koji pada x povećava. Povećava vrijednosti u i b zajedno rezultira premještanje funkcije cijela flatter dio krivulje. Tih svojstava možete postaviti idealna funkcija za povećanje više nedavno korišteni dokumenti kada je x `rord(datefield)`. | `recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| Rad | Pretvaranje stupnjeva u radijanima. | `rad(x)` |
| Ispiši| Zaokružuje broj na najbliži cijeli broj | `rint(x)`  <br> `rint(5.6)`-Vraća 6 |
| sin | Vraća sinus zadanog kuta. | `sin(x)` |
| SINH | Vraća hiperbolni sinus kuta. | `sinh(x)` |
| Promjena veličine | Mijenja veličinu vrijednosti funkcije x tako da se mogu nalaziti između navedeni minTarget i maxTarget, uključujući te dvije vrijednosti. Trenutni implementaciju putuje, a sve vrijednosti funkcije da biste dobili minimum i maksimum, pa možete odabrati ispravan skale. Trenutni implementaciju ne može razlikovati kada izbrišete dokumente ili dokumente na kojima nema vrijednosti. Koristi 0.0 vrijednosti za tim slučajevima. To znači da ako obično sve veći od 0,0 vrijednosti, jedan možete i dalje božićnim 0.0 kao minimalne vrijednosti koje će se mapirati. U tim slučajevima odgovarajuću `map()` funkcije mogu koristiti kao zaobilazno rješenje da biste promijenili 0.0 vrijednosti u rasponu od realni, kao što je prikazano ovdje:`scale(map(x,0,0,5),1,2)` | `scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`– Mijenja veličinu vrijednosti x tako da će se sve vrijednosti između 1 i 2, uključujući te dvije vrijednosti. |
| SQRT | Vraća drugi korijen od određenu vrijednost ili funkcija. | `sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist | Izračun udaljenost između dva niza. Koristi sučelje StringDistance alat za provjeru pravopisa Lucene i podržava sve dostupne u tom paketa implementacije te omogućuje aplikacijama priključite svoje putem resursa za Solr na mogućnosti učitavanja. vodi strdist `(string1, string2, distance measure)`. Moguće vrijednosti mjere udaljenost su: <br>jw: Jaro Winkler<br>Uređivanje: Levenstein ili Uredi udaljenost<br>ngram: U NGramDistance Ako navedeni, po želji prenositi veličine ngram previše. Zadana vrijednost je 2.<br>FQN: U potpunosti kvalificirana predmete naziv implementacija StringDistance sučelja. Morate imati bez argument Graditelj.|`strdist("SOLR",id,edit)` |
| Sub | Vraća x-y iz `sub(x,y)`. | `sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| zbroj | Vraća zbroj više vrijednosti ili funkcije koji su navedeni na popisu odvojenih zarezom. `add(...)`može se koristiti kao pseudonim za ovu funkciju. | `sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)`|
|termfreq | Vraća broj pojavljivanja pojam u polje za taj dokument. | termfreq(TEXT,'memory')|
| tan | Vraća tangens zadanog kuta. | `tan(x)` |
| TANH | Vraća hiperbolni tangens zadanog kuta. | `tanh(x)` |



## <a name="search-field-and-facet-reference"></a>Referenca za pretraživanje polja i fasete

Kada koristite zapisnika pretraživanja da biste pronašli podatke, rezultati prikazuju se različite polja i pozornici. Međutim, neke informacije prikazat će se možda neće prikazati vrlo opisni. Možete koristiti sljedeće informacije pomoću kojih se objašnjava rezultate.



|Polje|Vrsta pretraživanja|Opis|
|---|---|---|
|TenantId|Sve|Služi za particija podataka|
|TimeGenerated|Sve|Služi za Poticanje vremensku traku, timeselectors (u pretraživanju i drugih zaslona). Predstavlja kada dio podataka stvorena je (obično agent). Vrijeme je izražena u obliku ISO i uvijek je UTC-a. U slučaju "Vrsta", koji se temelje na postojeće instrumentation (odnosno događaja u zapisnik) to je obično stvarnom vremenu koji zapisnika stavka/redak/zapis je bio prijavljen na; za neke od drugih vrsta koji nastaju putem upravljanja paketi ili u oblak – odnosno preporuke/upozorenja/updateagent/itd, to vrijeme kada je ovo novi dio podataka s snimku konfiguracije neke sortiranja prikupljeni ili je sastavio preporuke/upozorenje temelji se na ga.|
|Iddogađaja|Događaja|Iddogađaja u zapisniku događaja sustava Windows|
|Zapisniku događaja|Događaja|Zapisnik događaja na kojoj je bio prijavljen događaj sustav Windows|
|EventLevelName|Događaja|Ključna / upozorenje / informacije / uspjeh|
|EventLevel|Događaja|Numeričke vrijednosti za ključne / upozorenje / informacije / uspjeh (EventLevelName umjesto toga koristite za lakše/čitljiviji upite)|
|SourceSystem|Sve|Odakle dolaze podaci (pomoću 'priložiti načinu rada sa servisom – odnosno Operations Manager, OMS (= podaci se generira u oblaku), Azure prostora za pohranu (podatke koji dolaze iz WAD) i tako dalje|
|ObjectName|PerfHourly|Naziv objekta performanse sustava Windows|
|InstanceName|PerfHourly|Naziv instance brojač performansi za Windows|
|CounteName|PerfHourly|Naziv brojač performanse sustava Windows|
|ObjectDisplayName|PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty|Zaslonski naziv objekta ciljani pravilom zbirke performanse Operations Manager ili koji objekt otkrio radu uvide ili prema kojem je generirana upozorenje|
|RootObjectName|PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty|Zaslonsko ime nadređenog nadređenog (u odnosu dvostruki hostinga: odnosno SqlDatabase hostira tvrtka SqlInstance hostira tvrtka računalo sa sustavom Windows) objekta ciljani pravilom zbirke performanse Operations Manager ili koji objekt otkrio radu uvida ili prema kojem je generirana upozorenje|
|Računalo|Većinu|Naziv računala koji pripada podataka|
|DeviceName|ProtectionStatus|Naziv računala podatke pripada (isto kao "Računalo")|
|DetectionId|ProtectionStatus| |
|ThreatStatusRank|ProtectionStatus|Položaj prijetnju status je numeričke Prikaz statusa prijetnju i slične šifre HTTP odgovor, ne možemo ste izašli praznina između navedenih brojeva (koji je zašto nema prijetnji je 150, a ne 100 ili 0) tako da se imamo prostora da biste dodali novi stanja. Kada moramo skupne vrijednosti za status prijetnju i status zaštitu, želimo da bi se prikazala najgorim stanje u kojem je računalo u tijekom odabranom vremenskom razdoblju. Koristimo brojeve za rangiranje različiti statusi pa ćemo možete potražiti zapis s najvećim brojem.|
|ThreatStatus|ProtectionStatus|Opis ThreatStatus, mapira 1:1 s ThreatStatusRank|
|TypeofProtection|ProtectionStatus|Zlonamjernog proizvod koji otkrije na računalu: ništa, alat za uklanjanje zlonamjernog softvera za Microsoft, Forefront i tako dalje|
|ScanDate|ProtectionStatus| |
|SourceHealthServiceId|ProtectionStatus, RequiredUpdate|ID HealthService agent za ovo računalo|
|HealthServiceId|Većinu|ID HealthService agent za ovo računalo|
|ManagementGroupName|Većinu|Naziv grupe upravljanje agenata Operations Manager priložene u suprotnom će biti null/prazno|
|Vrstu objekta|ConfigurationObject|Vrsta (kao Operations Manager paket za upravljanje "Vrsta" / klase) za taj objekt otkriti prijava analitiku konfiguraciji Procjena|
|UpdateTitle|RequiredUpdate|Naziv pronađen ažuriranje nije instalirano|
|PublishDate|RequiredUpdate|Kada je ažuriranje objavljena na Microsoft update?|
|Poslužitelj|RequiredUpdate|Naziv računala podatke pripada (isto kao "Računalo")|
|Proizvoda|RequiredUpdate|Proizvod koji pripada ažuriranje|
|UpdateClassification|RequiredUpdate|Vrsta ažuriranja (zbirna, servisnim paketom Service pack itd.)|
|KBID|RequiredUpdate|ID članka koji opisuje taj najbolja praksa ili ažuriranje|
|WorkflowName|ConfigurationAlert|Naziv pravila ili monitor koji je sastavio upozorenje|
|Težinu|ConfigurationAlert|Težinu upozorenja|
|Prioritet|ConfigurationAlert|Prioritet upozorenja|
|IsMonitorAlert|ConfigurationAlert|Upozorenje stvara monitora (istinito) ili pravilo (false)?|
|AlertParameters|ConfigurationAlert|XML s parametrima prijava analitiku upozorenja|
|Kontekst|ConfigurationAlert|XML s "kontekst" Prijava analitiku upozorenja|
|Radno opterećenje|ConfigurationAlert|Tehnologiju ili 'radno opterećenje"koja se odnosi na upozorenja|
|AdvisorWorkload|Preporuka|Tehnologiju ili 'radno opterećenje"koja se odnosi na preporuka|
|Opis|ConfigurationAlert|Opis upozorenja (kratko)|
|DaysSinceLastUpdate|UpdateAgent|Koliko dana (odnosu 'TimeGenerated' zapisa) jeste li ovaj agent instalirajte sva ažuriranja WSUS/Microsoft ažurirati?|
|DaysSinceLastUpdateBucket|UpdateAgent|Na temelju DaysSinceLastUpdate kategorizaciju u 'vrijeme memorijski blokovi' od davno kako je računalo zadnji put instalirati sva ažuriranja s WSUS/Microsoft ažurirati|
|AutomaticUpdateEnabled|UpdateAgent|Automatsko ažuriranje Provjera omogućuje ili onemogućuje na ovaj agent?
|AutomaticUpdateValue|UpdateAgent|Automatsko ažuriranje provjerava postavljen na automatsko preuzimanje i instalacija, preuzimali samo ili samo provjerite?|
|WindowsUpdateAgentVersion|UpdateAgent|Broj verzije agent za Microsoft Update|
|WSUSServer|UpdateAgent|Poslužitelj koji WSUS ciljanja je agent za ažuriranje?|
|OSVersion|UpdateAgent|Verzija operacijskog sustava radi agent za ažuriranje|
|Ime|Preporuke, ConfigurationObjectProperty|Naziv/naslov preporuke ili naziv svojstva iz zapisnika analize konfiguraciji Procjena|
|Vrijednost|ConfigurationObjectProperty|Vrijednost svojstva iz zapisnika analize konfiguraciji Procjena|
|Veza|Preporuka|URL za članak iz baze znanja koji opisuje taj najbolja praksa ili ažuriranje|
|RecommendationStatus|Preporuka|Preporuke među su nekoliko vrstama čiji zapisi se 'ažurirati", ne samo dodati indeks pretraživanja. Ovaj status mijenja li se preporuke je aktivno/otvori ili prijava analitiku otkrije da je riješen.|
|RenderedDescription|Događaja|Prikazanih opis (ponovno korištenim tekst s popunjena parametra) događaja sustava Windows|
|ParameterXml|Događaja|XML s parametrima u odjeljku 'podaci' događaja sustava Windows (kao što se vidi u preglednik događaja)|
|EventData|Događaja|XML odjeljka cijeli 'podaci' događaja sustava Windows (kao što se vidi u preglednik događaja)|
|Izvor|Događaja|Zapisnik događaja izvora koji je generirao događaja|
|EventCategory|Događaja|Kategorija događaja, izravno iz zapisnika događaja sustava Windows|
|Korisničko ime|Događaja|Korisničko ime događaja sustava Windows (obično NT AUTHORITY\LOCALSYSTEM)|
|SampleValue|PerfHourly|Prosječne vrijednosti za svaki sat Zbrajanje mjerača performansi|
|Min|PerfHourly|Najmanja vrijednost u svaki sat intervala performanse svaki sat zbrajanja brojač|
|Max|PerfHourly|Maksimalna vrijednost u svaki sat intervalu performanse svaki sat zbrajanja brojač|
|Percentile95|PerfHourly|Vrijednost percentila 95. za svaki sat interval performanse svaki sat zbrajanja brojač|
|SampleCount|PerfHourly|Koliko uzoraka brojač 'neobrađenog performanse su se koristile čime se dobiva taj zapis za svaki sat zbrajanja|
|Prijetnju|ProtectionStatus|Naziv zlonamjernog softvera pronađen|
|StorageAccount|W3CIISLog|Račun za Azure prostora za pohranu u zapisniku je pročitana iz|
|AzureDeploymentID|W3CIISLog|Pripada Azure implementacije ID servisa u oblaku u zapisnik|
|Uloga|W3CIISLog|Uloga servisa Azure oblaka pripada zapisnika|
|RoleInstance|W3CIISLog|RoleInstance uloge Azure koji pripada u zapisnik|
|sSiteName|W3CIISLog|IIS web-mjesto u zapisniku pripada (metabaze notacija); polje s NazivWebMjesta u izvornom zapisnika|
|sComputerName|W3CIISLog|Polje s computername u izvornom zapisnika|
|sIP|W3CIISLog|Adresirane je poslužitelj IP adresa HTTP zahtjev. Polje s ip u izvornom zapisnika|
|csMethod|W3CIISLog|HTTP metoda (objavu/GET/itd) koristi klijent u HTTP zahtjev. Cs metoda u izvornom zapisnika|
|cIP|W3CIISLog|Klijentski IP adresa HTTP zahtjev dolazi od. Polje c ip u izvornom zapisnika|
|csUserAgent|W3CIISLog|Korisnički Agent HTTP deklarirani klijenta (preglednika ili neki drugi način). Cs-korisnički agent u izvornom zapisnika|
|scStatus|W3CIISLog|HTTP Šifra stanja (200/403/500/itd) koju vraća poslužitelj klijentu. Cs statusa u zapisniku izvorne|
|TimeTaken|W3CIISLog|Koliko dugo (u milisekundama) snimljene zahtjev da biste dovršili. Polje timetaken u izvornom zapisnika|
|csUriStem|W3CIISLog|Relativna Uri (bez adresa glavnog računala, odnosno "/ pretraživanje ') koji je zatražio. Polje cs uristem u izvornom zapisnika|
|csUriQuery|W3CIISLog|URI upit. Upiti URI su potrebne samo za dinamičku stranica, kao što su ASP stranica, tako da to polje obično sadrži spojnica za statične stranice.|
|sPort|W3CIISLog|Priključak poslužitelja koji HTTP zahtjev poslan (i IIS očekuje podatke, budući da je izdvojen)|
|csUserName|W3CIISLog|Autentičnost korisničko ime, ako je zahtjev za čija je autentičnost provjerena i ne anonimno|
|csVersion|W3CIISLog|Verziju HTTP protokola koja se koristi u zahtjevu za (odnosno ' HTTP / 1.1')|
|csCookie|W3CIISLog|Kolačić podaci|
|csReferer|W3CIISLog|Web-mjesto koje je korisnik zadnjeg posjeta. Ovo web-mjesto navedene veze na trenutnu stranicu.|
|csHost|W3CIISLog|Zaglavlje glavnog računala (odnosno ' www.mysite.com') koji ste tražili|
|scSubStatus|W3CIISLog|Kod pogreške substatus|
|scWin32Status|W3CIISLog|Šifra stanja sustava Windows|
|csBytes|W3CIISLog|Poslano u zahtjevu za iz klijenta s poslužiteljem bajtova|
|scBytes|W3CIISLog|Bajtova vratiti natrag u odgovor poslužitelja klijentu|
|ConfigChangeType|ConfigurationChange|Vrsta promjene (WindowsServices / softver / itd)|
|ChangeCategory|ConfigurationChange|Kategorija promjene (izmijenio / dodaje / uklanja)|
|SoftwareType|ConfigurationChange|Vrsta softvera (Ažuriraj / aplikacije)|
|SoftwareName|ConfigurationChange|Naziv softvera (samo primjenjivo na softver promjene)|
|Publisher|ConfigurationChange|Dobavljač koji se objavljuje softver (samo primjenjivo na softver promjene)|
|SvcChangeType|ConfigurationChange|Vrstu promjene koju je primijenjena na servis Windows (stanje / StartupType / put / ServiceAccount) – samo primjenjivo promjenama servisa Windows|
|SvcDisplayName|ConfigurationChange|Zaslonsko ime usluge koje je promijenjeno|
|SvcName|ConfigurationChange|Naziv usluge koje je promijenjeno|
|SvcState|ConfigurationChange|Novo (trenutačna) stanje servisa|
|SvcPreviousState|ConfigurationChange|Prethodno poznati stanje servisa (samo primjenjivo ako stanje servisa promijeniti)|
|SvcStartupType|ConfigurationChange|Vrsta pokretanja servisa|
|SvcPreviousStartupType|ConfigurationChange|Prethodni servisa vrsta pokretanja (samo primjenjivo Ako vrsta pokretanja servisa promijeni)|
|SvcAccount|ConfigurationChange|Račun servisa|
|SvcPreviousAccount|ConfigurationChange|Prethodni račun servisa (samo primjenjivo Ako račun servisa promijeni)|
|SvcPath|ConfigurationChange|Put do datoteke servis sustava Windows|
|SvcPreviousPath|ConfigurationChange|Prethodni put izvršna datoteka za servis sustava Windows (samo primjenjivo ako je promijenjena)|
|SvcDescription|ConfigurationChange|Opis servisa|
|Prethodni|ConfigurationChange|Prethodno stanje ovaj softver (instalirani / nije instaliran / prethodne verzije)|
|Trenutni|ConfigurationChange|Najnovije stanje ovaj softver (instalirani / nije instaliran / trenutnu verziju)|

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o zapisniku pretraživanja:

- Upoznavanje s [zapisnika pretraživanja](log-analytics-log-searches.md) da biste pogledali detaljne podatke prikupljene putem rješenja.
- Da biste proširili zapisnika pretraživanja pomoću [prilagođenih polja u zapisnik analize](log-analytics-custom-fields.md) .
