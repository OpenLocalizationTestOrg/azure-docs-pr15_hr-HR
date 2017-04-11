<properties
    pageTitle="Prijavite se u analize zapisnika pretraživanja | Microsoft Azure"
    description="Zapisnika pretraživanja jednostavan način možete kombinirati i povezivanje strojno podatke iz više izvora u okruženju sustava."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>Zapisnika pretraživanja u zapisnik Analytics

Temeljni prijava analitiku biti zapisnika značajke pretraživanja koje omogućuje za kombiniranje i povezivanje strojno podatke iz više izvora u okruženju sustava. Rješenja također se pokreće zapisnika pretraživanja da biste otvorili metriku pivoted oko područje određenog problema.

Na stranici pretraživanja možete stvoriti upit, a zatim prilikom pretraživanja, možete filtrirati rezultate pomoću kontrola fasete. Možete stvoriti i naprednih upita pretvorba, filtar i izvješća na rezultate.

Uobičajeni upita za pretraživanje zapisnika prikazuju se na većini stranica rješenja. Cijeloj konzole OMS kliknite pločice ili prikažete u na druge stavke da biste pogledali detalje o stavci pomoću zapisnika pretraživanja.

U ovom ćete praktičnom vodiču ćemo vas provesti primjera pokriva sve osnove kada koristite zapisnika pretraživanja.

Ne možemo ćete započeti s primjerima jednostavne, praktično i stvorite na njima da bi mogli pristupiti poznavati praktično korištenje slučajeva kako pomoću ove sintakse uvida želite izdvojiti iz podataka.

Nakon što ste upoznati s tehnike, možete pregledati [analize zapisnika prijave referenca za pretraživanje](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Korištenje osnovni filtri

Najprije znati je li prvi dio pretraživanje upita, prije bilo "|" okomito okomitu crtu, uvijek je *Filtar*. Možete shvatiti ga kao WHERE u TSQL – određuje *koje* podskup podataka da biste izvukli iz spremišta podataka OMS. Pretraživanje u spremištu podataka je uglavnom o karakteristikama podatke koje želite izdvojiti, tako da bude prirodnim upita želite počinju uvjet WHERE.

Osnovni filtri možete koristiti su *ključne riječi*, kao što su "Pogreška" ili "isteklo" ili naziv računala. Ove vrste jednostavne upite obično vratite raznih oblika podataka u isti skup rezultata. To je zato prijava analitiku sadrži različite *vrste* podataka u sustavu.


### <a name="to-conduct-a-simple-search"></a>Izvršite jednostavno pretraživanje
1. Na portalu OMS kliknite **Zapisnika pretraživanja**.  
    ![Pločica za pretraživanje](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. U polje upita upišite `error` , a zatim kliknite **pretraživanje**.  
    ![pretraživanje pogreške](./media/log-analytics-log-searches/oms-search-error.png)  
    Na primjer, upit za `error` na sljedećoj slici vraća 100 000 zapisi **događaja** (prikupljene putem upravljanja zapisnika), 18 **ConfigurationAlert** (generira konfiguraciji Procjena) i 12 **ConfigurationChange** zapisa (zabilježene praćenjem promjena).   
    ![Rezultati pretraživanja](./media/log-analytics-log-searches/oms-search-results01.png)  

Ti se filtri nisu zaista vrste klase objekta. *Vrsta* je samo oznake, ili svojstvo ili niz/naziv/kategoriju, koja je priložena dio podataka. Neke dokumente u sustavu su označene kao **Vrstu: ConfigurationAlert** i neke su označene kao **Vrstu: mjerača performansi**ili **Vrste: događaja**i tako dalje. Svaki rezultat pretraživanja, dokumenta, zapis ili stavka prikazuje neobrađenog svojstva i njihovih vrijednosti za svaki od tih podataka, a možete koristiti nazive polja da biste odredili u filtru kada želite dohvatiti samo zapise gdje polje ima koja je zadana vrijednost.

*Vrsta* je zaista samo polja koja ste sve zapise, neće se razlikuje od bilo kojeg polja. To je uspostavljena na temelju vrijednosti polja Vrsta. Taj zapis će imati nekog drugog obrasca ili oblika. Incidentally, **Vrsta = mjerača performansi**, ili **Vrsta = događaj** je i vidjet ćete da sintaksa koje su vam potrebne informacije za dohvaćanje podataka o performansama ili događaja.

Kad naziv polja, a prije vrijednosti možete koristiti dvotočka (:) ili znak jednakosti (=). **Vrsta: događaja** i **Vrsta = događaj** su jednaki značenje, možete odabrati željeni stil koji želite.

Pa, ako vrsta = mjerača performansi zapisa imaju polje pod nazivom "CounterName", a zatim možete napisati upita nalik `Type=Perf CounterName="% Processor Time"`.

To će vam samo na podataka o performansama gdje je naziv brojač performansi "% procesor vrijeme".

### <a name="to-search-for-processor-time-performance-data"></a>Da biste potražili podataka o performansama procesor vremena
- U polje upita za pretraživanje upišite`Type=Perf CounterName="% Processor Time"`

Možete određeno i koristiti **InstanceName = _ "Ukupno"** u upitu, koji se prikazuje se brojač performanse sustava Windows. Možete odabrati i na fasete a kao drugu **vrijednost: polja**. Filtar automatski se dodaju na filtar na traci upita. To možete vidjeti na sljedećoj slici. Pokazuje gdje kliknite da biste dodali **InstanceName: '_Total'** upit bez upisivanja ništa.

![pretraživanje fasete](./media/log-analytics-log-searches/oms-search-facet.png)

Upit sada postaje`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

U ovom primjeru, ne morate navesti **Vrsta = mjerača performansi** da biste pristupili taj rezultat. Budući da polja CounterName i InstanceName postoje samo za zapise vrste = mjerača performansi, dovoljno specifični da biste se vratili iste rezultate kao jedan dulje, prethodni se upit:
```
CounterName="% Processor Time" InstanceName="_Total"
```

To je zato svih filtara u upitu vrednuju se kao *i* međusobno. Učinkovito, više polja dodate kriterije, nailazite na manji, određenih i dobili profinjeniji rezultate.

Na primjer, upit `Type=Event EventLog="Windows PowerShell"` jednak `Type=Event AND EventLog="Windows PowerShell"`. Vraća sve događaje koji su prijavljeni, a koji se prikupljaju iz zapisnika događaja sustava Windows PowerShell. Ako dodate filtra više puta uzastopno odabirom iste fasete, a zatim se problem ne isključivo kozmetičkih – je možda Zagušenje traka za pretraživanje, ali i dalje vraća isti rezultat jer operator IMPLICITNIH i postoji li uvijek.

Možete jednostavno poništiti operator IMPLICITNIH i rada ne izričito. Ako, na primjer:

`Type:Event NOT(EventLog:"Windows PowerShell")`ili njegov ekvivalentne `Type=Event EventLog!="Windows PowerShell"` vratiti sve događaje iz sve zapisnike koji nisu u zapisniku komponente Windows PowerShell.

Možete koristiti druge logičkih operatora kao što su "Ili". Sljedeći upit vraća zapise za koje je u zapisniku događaja ili aplikacije ili sustav.

```
EventLog=Application OR EventLog=System
```

Pomoću gore navedeni upit, prikazat će se stavke za oba zapisnika u isti skup rezultata.

Međutim, ako u ili uklonili ostavite na IMPLICITNIH i na mjestu, pa sljedeći upit neće proizvesti rezultate jer nema unos zapisnik događaja koji pripada oba zapisnika. Svaka stavka zapisnik događaja je pisati samo jedan od dva zapisnike.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Koristite dodatne filtre

Sljedeći upit vraća stavke za 2 zapisnika događaja na sva računala koje ste poslali podatke.

```
EventLog=Application OR EventLog=System
```

![Rezultati pretraživanja](./media/log-analytics-log-searches/oms-search-results03.png)

Odabirom jedne od polja ili filtri će suzili upit za određeno računalo, isključujući sve njih. Rezultat upita će otprilike ovako.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Koji je jednaka sljedeće zbog implicitno AND.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Svaki upit se vrednuje sljedećim redoslijedom eksplicitnih. Imajte na umu zagrada.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Baš kao i polje zapisnik događaja možete dohvatiti podatke samo za skup određenim računalima dodavanjem ili. Ako, na primjer:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Isto tako, to sljedeći upit vraća **% vrijeme procesora** za odabrani dva računala samo.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Logički operatori
S datumom i vremenom i brojčana polja, možete potražiti vrijednosti pomoću *veće*, *manje od*, a *manje od ili jednaka*. Možete koristiti jednostavne operatore kao što su >, <>; =, < =,! = u upit traka za pretraživanje.


Određeni zapisnik događaja možete upit za određeno vremensko razdoblje. Zadnja 24 sata, na primjer, izražen je sa sljedećim mnemonic izraz.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Da biste pronašli korištenje logičkih operatora
- U polje upita za pretraživanje upišite`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![pretraživanje s Booleove vrijednosti](./media/log-analytics-log-searches/oms-search-boolean.png)

Iako možete odrediti razdoblje grafički i većina puta preporučujemo da to učinite, postoje prednosti uključujući filtrom vrijeme izravno u upit. Na primjer, to odgovara pomoću nadzornih ploča kojima možete nadjačati vremena za svaku pločicu, bez obzira na to *globalni* izbornik vremena na stranicu nadzorne ploče. Dodatne informacije potražite u članku [Stvarima vremena na nadzornu ploču](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Prilikom filtriranja po vremenu, ne zaboravite dohvaćanja rezultata za *Presjek* dva vremenskih razdoblja: one koja je navedena na portalu OMS (S1) i one koja je navedena u upitu (S2).

![Sjecište](./media/log-analytics-log-searches/oms-search-intersection.png)

To znači da, ako je vremenska razdoblja ne sijeku, primjerice na portalu OMS gdje odaberite **Ovaj tjedan** i u upitu mjesto na koje ste definirali **prošli tjedan**, zatim postoji nema sjecišta te nećete primati rezultate.

Operatori usporedbe koji se koriste za polje TimeGenerated i korisne su u drugim situacijama. Na primjer, s brojčana polja.

Na primjer, dali da upozorenja o konfiguraciji Procjena imati sljedeće težinu vrijednosti:

- 0 = informacije
- 1 = upozorenje
- 2 = od ključne važnosti

Možete upita za upozorenje i ključnih upozorenja, a i izuzimanje Informativna one s sljedeći upit:

```
Type=ConfigurationAlert  Severity>=1
```


Možete koristiti i raspon upita. To znači da možete unijeti početka i kraja raspon vrijednosti u nizu. Ako, na primjer, ako želite događaje iz zapisnika događaja sustava Operations Manager gdje se nalazi na Iddogađaja veće od ili jednako 2100, ali nije veće od 2199, zatim sljedeći upit vratiti ih.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] Sintaksa raspona morate koristiti je vrijednost polja: razdjelnik dvotočka (:) i *ne* znak jednakosti (=). Stavite donje i gornje kraj raspona u uglate zagrade, a zatim ih razdvojite dvije točke (.).

## <a name="manipulate-search-results"></a>Rukovanje rezultata pretraživanja

Kada tražite podatke, ćete htjeti suzili upit za pretraživanje, a imate dobar razinu kontrole nad rezultatima. Kada se dohvaćaju rezultata, možete primijeniti naredbi za pretvorbu ih.

Naredbe u analize zapisnika pretraživanja *morate* slijedite nakon znaka okomiti kanala (|). Filtar moraju uvijek biti prvi dio niza upita. Definira skup podataka radite, a zatim "cijevi" te rezultate u naredbu. Kanal možete koristiti da biste dodali dodatne naredbe. To je slobodnije slično kanal komponente Windows PowerShell.

Općenito govoreći, jezik za pretraživanje zapisnika analize pokušava slijedite smjernice da biste slično Stručnjaci za IT i da biste olakšali krivulje učenje i stil PowerShell.

Naredbe imaju imena glagoli tako da možete jednostavno prepoznati njihovih funkcija.  

### <a name="sort"></a>Sortiranje

Naredba Sortiraj omogućuje da odredite redoslijed sortiranja tako da jedno ili više polja. Čak i ako ne koristite, po zadanom, provest će se vrijeme silaznim redoslijedom. Najnovije rezultati su uvijek na vrhu rezultata pretraživanja. To znači da pri pokretanju pretraživanja, s `Type=Event EventID=1234` što zapravo se izvršava umjesto vas je:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

To je zato što je vrsta sučelje koje poznajete u zapisnicima. Na primjer, u preglednik događaja sustava Windows.

Sortiranje možete koristiti da biste promijenili način na koji se rezultati se vraćaju. Sljedeći primjeri pokazuju kako to funkcionira.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Jednostavan iznad primjeri pokazuju kako funkcioniraju naredbe – mijenjaju oblika rezultati koje je vratio filtar.

### <a name="limit-and-top"></a>Ograničenje i vrha
Drugi manje poznate naredba je ograničenje. Ograničenje je PowerShell nalik glagolski. Ograničenje jednak functionally GORNJU naredbu. Upiti za sljedeće vraćaju iste rezultate.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Da biste pokrenuli vrha
- U polje upita za pretraživanje upišite`Type=Event EventID=600 | Top 1`   
    ![Vrh pretraživanja](./media/log-analytics-log-searches/oms-search-top.png)

U gornjoj slici nema zapisa tisuće 358 s Iddogađaja = 600. Polja, pozornici i filtre na lijevoj strani uvijek prikazuje informacije o rezultatima koje vraćaju *po dijelu filtar* upita koja je dio prije bilo koji znak kanala. Okno s **rezultatima** samo vraća zadnje 1 rezultat jer naredbu primjer oblika i transformacije rezultate.

### <a name="select"></a>Odaberite

Odaberite naredbu ponaša se kao odaberite objekt u ljusci PowerShell. Vraća filtriranim rezultatima koje nemaju instalirane sve njihove izvorne svojstva. Umjesto toga odabire svojstva koji navedete.

#### <a name="to-run-a-search-using-the-select-command"></a>Da biste pokrenuli pretraživanje pomoću naredbe za odabir

1. U odjeljku pretraživanje, upišite `Type=Event` , a zatim kliknite **pretraživanje**.
2. Kliknite **+ Pokaži više** na jedan od rezultata da biste pogledali svojstva koje sadrže rezultate.
3. Odaberite neke od tih izričito i upit mijenja `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Odaberite pretraživanje](./media/log-analytics-log-searches/oms-search-select.png)

Ovo je naredba osobito je korisna kad želite kontrolirati izlaz za pretraživanje, a zatim odaberite samo dijelove podatke koje zaista bitan za istraživanje, koji često nije cijeli zapis. To je korisno kada zapisa različitih vrsta imaju *nekoliko* uobičajenih svojstava, ali ne *sve* svoje svojstava su uobičajeni. U odjeljku možete generirati izlaz više prirodan izgleda kao tablicu ili funkcionira dobro kada se izvoze u CSV datoteku, a zatim massaged u programu Excel.



## <a name="use-the-measure-command"></a>Koristite naredbu mjera

MJERE jedan je od najsvestraniji naredbi u analize zapisnika pretraživanja. Omogućuje da biste primijenili statističke *funkcije* s podacima i zbrajanja rezultate grupirane neko polje. Nema više statističke funkcije koji podržava mjere.

### <a name="measure-count"></a>Izmjerite count()

Statistička funkcija za rad sa i od najjednostavniji da biste shvatili je funkcija *count()* .

Rezultati iz bilo kojeg upit za pretraživanje kao što su `Type=Event`, prikaz Filtri naziva se i pozornici na lijevoj strani rezultata pretraživanja. Filtri Pokaži raspodjela vrijednosti neko polje za rezultate pretraživanja izvršiti.

![count mjere za pretraživanje](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Na primjer, slike iznad vidjet ćete polje **računala** i ga prikazuje koje unutar gotovo 739 tisuće događaji u rezultatima postoje 68 jedinstven i distinct vrijednosti za polje **računala** u tim zapisima. Pločica samo prikazat će gornjoj 5, koji su najčešće 5 vrijednosti koje se pišu u poljima s **računala** ), sortirane prema broju dokumenata koji sadrže određene vrijednosti tog polja. Na slici vidjet ćete da – među događaje gotovo 369 tisuće – 90 tisuće potjecati iz OpsInsights04.contoso.com računala, 83 tisuće s računala DB03.contoso.com i tako dalje.


Ako želite vidjeti sve vrijednosti, budući da pločicu samo prikazuje samo prvih 5?

To je naredba mjere pruža funkciju count(). Ova funkcija ne koristi parametre. Samo odredite polje po kojem želite grupirati po – na **računalo** u tom slučaju:

`Type=Event | Measure count() by Computer`

![count mjere za pretraživanje](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

No **računalo** je samo u polje koristi *u* svaki dio podataka – postoje vezanih bez relacijske baze podataka, a ne postoji zasebnom **računalo** objekt bilo kojeg mjesta. Samo na vrijednosti *u* podacima možete opisati koje entitet generira ih, a broj druge karakteristike i aspekte podataka – dakle termina *fasete*. Međutim, baš kao i grupirate prema ostala polja. Budući da izvorni rezultati gotovo 739 tisuće događaja koji su piped u naredbu mjere postoje i poljem pod nazivom **Iddogađaja**, možete primijeniti istu tehniku da biste grupirali prema to polje i dobili ukupan broj događaja po Iddogađaja:

```
Type=Event | Measure count() by EventID
```

Ako vas zanima nije broj stvarni zapisa koji sadrže određene vrijednosti, ali umjesto ako samo želite da se popis vrijednosti sami možete dodati naredbu *Odaberite* na kraju se i samo odaberite prvi stupac:

```
Type=Event | Measure count() by EventID | Select EventID
```

Zatim možete dobiti više složene i unaprijed sortirali rezultate u upitu ili možete jednostavno kliknuti stupaca u rešetki previše.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Da biste pokrenuli count mjera

- U polje upita za pretraživanje upišite`Type=Event | Measure count() by EventID`
- Dodavanje `| Select EventID` na kraj upita.
- Na kraju, dodavanje `| Sort EventID asc` na kraj upita.


Postoji nekoliko važnih točaka obratite pozornost na to i naglašavanje:

Najprije rezultat koji vidite nisu izvorni neobrađenog rezultati više. Umjesto toga su Zbrojeno rezultate – zapravo grupe rezultata. To nije problem, ali upoznati nalazite li se interakcija s vrlo različito oblika podataka koji se razlikuje od izvorne neobrađenog oblik koji dohvaća stvorili hodu zbrajanja statističke funkcije.

Drugo, **mjera count** trenutno vraća samo 100 najvećih distinct rezultate. To ograničenje ne primjenjuju se na druge statističke funkcije. Tako, obično morat ćete najprije korištenje preciznije filtra da biste potražili određene stavke prije primjene count() mjere.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Korištenje funkcije max i min naredbom mjera

Postoje različite scenariji pri čemu su korisne **Max() mjere** i **Min() mjere** . No Budući da je svaka funkcija suprotan od drugoga, ne možemo ćete ilustrirali Max() i Eksperimentirajte s Min() vlastite.

Ako je upit za sigurnost događaje, imaju **razinu** svojstvo koje se može razlikovati. Ako, na primjer:

```
Type=SecurityEvent
```

![pretraživanje mjere count početka](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Ako želite pregledati najvišu vrijednost svih sigurnost događaje dali uobičajenih računalo Grupiraj po polju, možete koristiti

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![pretraživanje mjere Maks računala](./media/log-analytics-log-searches/oms-search-measure-max02.png)

To će prikaz da za računala koja je imala **razini** zapisa Većina ih imati barem razinu 8, mnoge imali razinu od 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![pretraživanje mjere Maks vrijeme koje generira računalo](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Ova funkcija dobro funkcionira s brojevima, ali funkcionira i sa polja datuma i vremena. Ovo je korisno da biste provjerili zadnji ili najnovije vremenska oznaka za neki podatak indeksirana za svako računalo. Na primjer: kada je najnovija sigurnosnog događaja potrebnom za svaki računala?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Korištenje funkcija avg naredbom mjera

Statističke funkcije Avg() koristi s mjere omogućuje vam da biste izračunali prosječnu vrijednost za neko polje i grupiranje rezultata prema polju iste ili drugih. To je korisno u mnogim slučajevima, kao što su podataka o performansama.

Ne možemo ćete počinju podataka o performansama. Imajte na umu da OMS trenutno prikuplja mjerača performansi za računala za Windows i Linux.

Da biste potražili *svih* podataka o performansama, osnovni upit je:

```
Type=Perf
```

![Pretraživanje početka avg](./media/log-analytics-log-searches/oms-search-avg01.png)

Najprije Primijetit ćete da je li prijava analitiku prikazuje tri perspektive: popis koji pokazuje koja prikazuje stvarni zapise iza grafikonima; Tablicu koja prikazuje tablični prikaz podataka o performansama brojač; i metrike koji se prikazuje grafikone za mjerača performansi.

U gornjoj slici postoje dva skupa polja označena koje označavaju sljedeće:

- Prvi skup označava naziv brojač performanse sustava Windows, naziv objekta i naziv Instance u filtru upita. To su polja vjerojatno ćete najčešće koristiti kao pozornici/filtri
- **CounterValue** je stvarna vrijednost brojač. U ovom primjeru vrijednost je *75*.
- **TimeGenerated** je 12:51 u 24-satni oblik vremena.

Ovdje je prikaz metriku u grafikon.

![Pretraživanje početka avg](./media/log-analytics-log-searches/oms-search-avg02.png)

Nakon čitanja o mjerača performansi oblik zapisa, a pritom Saznajte više o drugih tehnika pretraživanja, možete koristiti mjere Avg() za zbrajanje ovu vrstu numeričke podatke.

Evo jednostavnog primjera:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![pretraživanje avg samplevalue](./media/log-analytics-log-searches/oms-search-avg03.png)

U ovom primjeru odaberite procesora Ukupno vrijeme performanse brojača i average na računalu. Ako želite da biste suzili rezultate samo zadnjih 6 sati, možete koristiti kontrolu za filtar vremena ili odredite u upitu na sljedeći način:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Da biste pronašli funkcija avg pomoću naredbe mjera
- U okvir za upit za pretraživanje upišite `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


Možete zbrajanja i povezivanje podataka *putem* računala. Ako, na primjer, zamislite da imate skup domaćini u nekim sortiranje farme gdje svaki čvor jednak je bilo koji drugi znak samo iste upišu rada i učitavanje trebali biste otprilike raspoređen. Nije moguće dobiti njihove mjerača sve u jednom izabrati sljedeće upita, a zatim Dohvati prosjeci za cijelu farmu. Možete pokrenuti tako da odaberete računalima s u sljedećem primjeru:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Sad kad ste računala, također samo želite odabrati dva Ključni pokazatelji uspješnosti (KPI): % procesora i % slobodnog prostora na disku. Tako, postaje dio upit:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Sada možete dodati računala i mjerača s u sljedećem primjeru:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Budući da imate određene odabira, naredbu **mjere Avg()** možete se vratiti prosjek ne na računalu, ali farme, jednostavno tako Grupiranje po CounterName. Ako, na primjer:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Tako ćete dobiti korisne sažeti prikaz nekoliko vaše okruženje KPI-ja.

![pretraživanje avg grupiranja](./media/log-analytics-log-searches/oms-search-avg04.png)


Jednostavno koristite upit za pretraživanje na nadzornoj ploči. Na primjer, nije moguće spremiti upit za pretraživanje i stvaranje nadzorne ploče iz nje pod nazivom *Web farme KPI-ja*. Da biste saznali više o korištenju nadzorne ploče, potražite u članku [Stvaranje prilagođenih nadzorne ploče u zapisnik analize](log-analytics-dashboards.md).

![Nadzorna ploča za pretraživanje avg](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Korištenje funkcija sum pomoću naredbe mjera

Funkcija sum je slična druge funkcije naredbe mjere. Vidjet ćete primjer kako pomoću funkcije sum pri [W3C IIS zapisnika pretraživanja u radu sa servisom uvida u programu Microsoft Azure](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Možete koristiti Max() i Min() s brojevima, vremena i tekstnih nizova. Tekstni nizovi sortirani su po abecedi i dobiti prvi i zadnji.

Međutim, ne možete koristiti Sum() s nečim osim brojčanih polja. To se odnosi na Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Koristite funkciju percentil naredbom mjera

Percentile (funkcija) je slična Avg() i Sum() po tome možete koristiti samo je za brojčanih polja. Možete koristiti bilo koji percentila između 1 do 99 na numeričko polje. Možete koristiti i naredbe **percentil** i **postotak** . Evo nekoliko primjera:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Korištenje gdje naredbe

Na gdje je naredba funkcionira kao što je filtar, ali se može primijeniti u kanalu da biste dodatno filtrirali Zbrojeno rezultate koji su osnovu naredbu mjere – umjesto neobrađenog rezultata koji su filtrirani na početku upita.

Ako, na primjer:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Možete dodati druge kanala "|" znakova i gdje naredbu za samo početak računala čiji se prosjek procesora veća od 80% pomoću u sljedećem primjeru:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Ako ste upoznati s Microsoft System Center - Operations Manager možete shvatiti gdje naredbu u termine upravljanja paket. Ako se primjeru pravilo, prvi dio upit bio izvora podataka i gdje naredba biti otkrivanje uvjet.

Upit možete koristiti kao pločicu u **Moje nadzorne ploče**kao monitor sortiranja, da biste vidjeli kad su Prekomjerno procesora u računalu. Da biste saznali više o nadzorne ploče, potražite u članku [Stvaranje prilagođenih nadzorne ploče u zapisnik analize](log-analytics-dashboards.md). Stvaranje i korištenje nadzornih ploča pomoću mobilne aplikacije. Dodatne informacije potražite u članku [OMS mobilnu aplikaciju ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). U dnu dvije pločice na sljedećoj slici možete vidjeti monitor prikazuje popis i kao broj. Zapravo, uvijek želite broj nula i popis da biste će prazan. U suprotnom označava upozorenja uvjet. Ako je potrebno, možete je koristite da biste potražite na kojim računalima su u odjeljku tlaka.

![Mobilni nadzorne ploče](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Upotrijebite in operator

*U* operator, zajedno s *NOT IN* omogućuje vam korištenje subsearches pretraživanja koja uključuju pretražite kao argument. Se nalaze se u vitičaste zagrade {} unutar drugog *primarnog* i *vanjskog* pretraživanja. Rezultat subsearch često popis različitih rezultata koristi se kao argument primarni pretraživanju.

Možete koristiti subsearches tako da odgovara podskupova podataka ne može li se opisuju izravno u izraz za pretraživanje, ali ne i koje možete generirati iz pretraživanja. Ako, na primjer, ako vas zanima pomoću jedne pretraživanja da biste pronašli sve događaje s *računala koja nedostaje sigurnosna ažuriranja*, pa ćete morati dizajniranje subsearch koja najprije služi za identifikaciju *računala nedostaje sigurnosna ažuriranja* prije pronađe događaje koje pripadaju tim domaćini.

Tako, nije moguće express *računala trenutno nema potrebne sigurnosna ažuriranja* s sljedeći upit:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![U primjeru pretraživanja](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Nakon što dodate na popisu, pretraživanje možete koristiti kao unutarnji pretraživanja na sažetak sadržaja na popisu računala u vanjski (primarni) pretraživanja koja će izgledati za događaje za tih računala. To možete učiniti tako umetnute unutarnji pretraživanja u vitičaste zagrade, a Umetanje rezultate kao mogućih vrijednosti za filtriranje/polje vanjskog pretraživanja za početak rada. Nalikovat će tablici upita:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![U primjeru pretraživanja](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Također obavijest filtar vremena koristi unutarnji pretraživanja jer je ažuriranje procjenu sustava stvara snimku sva računala svaka 24 sata. Unutarnji upita možete učiniti laganih i precizno pretraživanjem samo za dan. Vanjsko pretraživanje već koristi vrijeme odabir u korisničkom sučelju dohvaćanja događaje iz zadnjih sedam dana. Dodatne informacije o operatori vrijeme potražite u članku [logički operatori](#boolean-operators) .

Budući da ste doista koristiti samo rezultate pretrage unutarnji kao filtar vrijednosti za vanjski, uvijek možete primijeniti naredbe u vanjskog pretraživanja. Na primjer, i dalje možete grupirati iznad događaji s druge mjere naredbe:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![U primjeru pretraživanja](./media/log-analytics-log-searches/oms-search-in03-revised.png)


Općenito govoreći, želite unutarnji upit da biste brzo izvršiti jer prijava analitiku je davatelj usluge vremenska ograničenja za njega i da biste se vratili mali rezultata. Ako unutarnji upit vraća više rezultata, decimale se popisa rezultata, koji bi mogli uzrokovati potencijalno vanjskog pretraživanja da biste se vratili netočne rezultate.

Drugo pravilo je da unutarnji pretraživanje trenutno treba radi dobivanja rezultata *zbroja* . Drugim riječima, ona mora sadržavati *mjere* naredbu; nije moguće trenutno sažetka sadržaja neobrađenog rezultate u vanjskog pretraživanja.

Osim toga, može biti samo jedan operator u i mora biti posljednjeg filtra u upitu. Ne može biti više operatora u ili želite – to zapravo onemogućuje izvodi više subsearches: točke važno je samo jedan sub/inner pretraživanje za svaki vanjskog pretraživanja.

Čak i uz ta ograničenja u omogućuje razne vrste povezanog pretraživanja, a omogućuje da odredite sličnim grupama kao što su računala, korisnika ili datoteke – bilo kakve polja u vašim podacima je sadrži. Evo još primjera:

**Sva ažuriranja nedostaje s računala na kojem je onemogućena postavka automatskog ažuriranja**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Svi događaji pogreške s računala sa sustavom SQL Server (= gdje pokrenuta procjenu SQL)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Svi događaji sigurnost s računala na kojima su kontrolera domena (= gdje pokrenuta AD Procjena)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Koje drugih računa ste prijavljeni na isti računala gdje račun BACONLAND\jochan prijave?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Koristite naredbu distinct

Kao što je naziv predlaže, ove naredbe nalaze se popis različitih vrijednosti za polje. Je surprisingly jednostavni, ali vrlo koristan. Isti može se postići pomoću naredbe count() mjera kao i, kao što je prikazano u nastavku.

```
Type=Event | Measure count() by Computer
```

![Primjer naredbe RAZLIČITA pretraživanja](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Međutim, ako se svi koji vas zanima je samo popis različitih vrijednosti i ne broj dokumenata koje možete unijeti vrijednosti, a zatim DISTINCT čišćenje i lakše čitati izlazne i kraću sintaksu, kao što je prikazano ispod.

```
Type=Event | Distinct Computer
```
![Primjer naredbe RAZLIČITA pretraživanja](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Koristite funkciju countdistinct pomoću naredbe mjera
Funkcija countdistinct broji različitih vrijednosti unutar svake grupe. Ako, na primjer, ne može se koristiti za Brojanje jedinstvenih računala izvješćivanja za svaku vrstu:

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Koristite naredbu interval mjera
S blizu prikupljanje podataka u stvarnom vremenu performanse, možete prikupiti i vizualizacija sve brojač performanse u zapisnik analize. Ovisno o broju mjerača i poslužitelji u okruženju sustava prijava analitiku jednostavnim unosom upita **Vrstu: mjerača performansi** će vratiti tisuće metričkim grafikona. S osvježavati metričkim zbrajanja, možete pogledati cjelokupan metriku u svom okruženju na visoku razinu i niže dive precizniji podatke kao što je potrebno.

Recimo da ako želite saznati što je prosjek CPU-a na svim računalima. Pogled na prosjek procesora na svakom računalu možda neće biti korisno jer se rezultati mogu dobiti izglađenim. Da biste vidjeli u više detalja, možete zbrajati rezultat u manje vremena blokova prozora, a potom pogledajte u vremenski niz preko različitih dimenzije. Na primjer, možete izvesti svaki sat prosjek procesora na svim računalima na sljedeći način:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Prosječan interval mjera](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Prema zadanim postavkama tih rezultata prikazat će se na više nizova interaktivne linijskom grafikonu.  Na grafikonu podržava niz prebacivanja (s osi y rescaling), zumiranje i zadržavanja pokazivača.  Mogućnost prikaza tablica i dalje dostupna je za prikaz sirovim podacima po potrebi.

Možete grupirati i ostala polja. U ovom primjeru koju tražim na sve mjerača % za jedno računalo određene i Saznajte što je hourly 70 percentili od svakog brojač:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Jedan napomenuti kako je tih upita nisu ograničeni na mjerača performansi. Možete ih primijeniti na sve metriku. U ovom primjeru koju tražim na W3C IIS zapisnika. Želim znati što je maksimalno vrijeme potrebno intervalu 5 minuta za obradu svaki zahtjev:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Korištenje više zbrajanja u upita
Možete navesti više uvjeta zbrajanja mjere naredba.  Svaki od njih samostalno može biti sukobima.  Ako je zadan pseudonima dobivene naziv polja bit će funkciju zbrajanja koju ste koristili (odnosno "avg(CounterValue)" za avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Evo još jednog primjera:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o zapisniku pretraživanja potražite u članku:

- Da biste proširili zapisnika pretraživanja pomoću [prilagođenih polja u zapisnik analize](log-analytics-custom-fields.md) .
- Pregledajte na [analize zapisnika prijave referenca za pretraživanje](log-analytics-search-reference.md) da biste vidjeli sva polja pretraživanja i pozornici dostupne u zapisnik analize.
