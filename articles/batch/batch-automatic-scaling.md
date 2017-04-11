<properties
    pageTitle="Automatski izračun skaliranje čvorove u programa Azure obradu | Microsoft Azure"
    description="Omogući automatsko skaliranje na cloud skup dinamički prilagodite broj računalnim čvorove u."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Automatski promijeniti omjer računalnim čvorove u programa grupe za Azure

Uz automatsko skaliranje servisa Azure grupe možete dinamički dodati ili ukloniti računalnim čvorove u grupu na temelju parametara koje sami definirate. Potencijalno uštedjet ćete vrijeme i novac automatski prilagodbom iznos računalnim napajanja koristi aplikacija – dodavanje čvorove kao povećanje zahtjeve za zadatak vaš posao, a zatim ih uklonite kada ih smanjiti.

Omogući automatsko skaliranje na skup računalnim čvorove Pridruživanjem ga programa *Automatsko skaliranje formula* koje sami definirate, kao što su s [PoolOperations.EnableAutoScale] [ net_enableautoscale] način u biblioteci [.NET grupe](batch-dotnet-get-started.md) . Servis za obradu zatim koristi sljedeću formulu za određivanje broja računalnim čvorove koje su potrebne za izvršavanje svoje radno opterećenje. Obradu odgovori servisa metriku uzorka podataka koji se povremeno prikupe i prilagođava broj računalnim čvorove u konfigurirati interval na temelju formule.

Možete omogućiti automatsko skaliranje stvaranja na grupu ili na postojeću grupu. Možete promijeniti i postojeće formule na skup koji je "Automatsko skaliranje" omogućena. Grupe omogućuje procjenu formulama prije dodijelite grupe, kao i praćenje statusa automatski se pokreće skaliranja.

## <a name="automatic-scaling-formulas"></a>Automatski formule skaliranja.

Automatsko skaliranja formula je vrijednost niza koje sami definirate koja sadrži jedan ili više izvješća i vam je dodijeljen na skup [autoScaleFormula] [ rest_autoscaleformula] element (obradu REST) ili [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] svojstvo (grupe za .NET). Kada dodijelite zajedničko područje, servis za obradu koristi formulu za određivanje ciljnog broj računalnim čvorove u sljedeći intervalu obrade (više o vremenskim razmacima kasnije). Formule niz ne može prelaziti 8 KB veličine, možete uključiti do 100 naredbe koje su razdvojenih točkom sa zarezom, a može obuhvaćati prijelomi redaka i komentare.

Možete shvatiti automatski skaliranja formule kao što su korištenje automatsko obradu skaliranje "jezik". Formule izjave su besplatno oblikovan izraza koji obuhvaćaju servisa definirane varijable (varijable definira servis obradu) i korisnički definirane varijable (varijabli koje sami definirate). Pomoću ugrađenih vrste, operatore i funkcije ih možete izvršiti razne operacije na ove vrijednosti. Izjava o, na primjer, može potrajati sljedećem obliku:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formule obično sadrže višestruke izjave o izvođenje operacije na vrijednosti koje se dobivaju se u izjave prethodni. Ako, na primjer, najprije ćemo dobiti vrijednost za `variable1`, prenesite funkcije za popunjavanje `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Pomoću te naredbe u formuli je vaš cilj da stigne na broj čvorove računalnim koji na resurse moraju biti skalirana da biste – **ciljni** broj **namjenski čvorove**. Taj broj može biti veći, lower ili jednaki trenutni broj čvorove u. Obradu vrednuje na skup automatsko skaliranje formulu u određenim intervalima ([automatske intervale skaliranja](#automatic-scaling-interval) se spominju ispod). Zatim ga prilagodite broj ciljne čvorove u broj koji određuje automatsko skaliranje formulu u vrijeme izvođenja.

Kao brzi primjer ovu formulu automatsko skaliranje dva retka određuje se da se broj čvorove prilagoditi prema Broj aktivnih zadataka, maksimalno 10 računalnim čvorove do:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

Na sljedeće nekoliko odjeljke u ovom se članku navode razne entiteti koji će se sastoje automatsko skaliranje formule, uključujući varijable, operatore, operacije i funkcije. Pronaći ćete informacije o načinu da biste dobili razne računalnim resursa i zadataka metriku unutar grupe. Pomoću ove metriku energija prilagodite na skup čvor Brojanje na temelju resursa korištenje i statusima zadataka. Zatim ćete saznati kako sastavljanje formulu i Omogući automatsko skaliranje na zajedničko područje pomoću obrade ostalih i .NET API-ji. Ne možemo ćete dovršili postupak s nekoliko primjera formula.

> [AZURE.IMPORTANT] Svaki račun za Azure obradu ograničeno je na maksimalni broj jezgri (i zato računalnim čvorove) koje je moguće koristiti za obradu. Servis za obradu će stvoriti čvorove samo na to ograničenje core. Stoga je možda ne dođete do ciljne broj računalnim čvorove određen formule. Informacije o prikaz i povećanje kvote za račun potražite [kvotama i ograničenja servisa Azure grupe](batch-quota-limit.md) .

## <a name="variables"></a>Varijable

I **usluge definirani** i **korisnički definirane** varijable možete koristiti u formulama automatsko skaliranje. Servis definirane varijable su ugrađena na servis za obradu – neke su čitanja i pisanja, a neke su samo za čitanje. Korisnički definirane varijable su varijabli koje *ste* definirali. U formulu kao primjer dva retka iznad, `$TargetDedicated` je servis definirane varijable tijekom `$averageActiveTaskCount` varijabla korisnički definirane.

U tablicama u nastavku Pokaži čitanja i pisanja i samo za čitanje varijable koji su definirani grupe za servis.

Možete **se** i **pripremite** vrijednosti od tih servisa definirane varijabli da biste upravljali broj računalnim čvorove u zbirci web:

| Čitanje i pisanje servisa definirane varijable | Opis |
| --- | --- |
| $TargetDedicated | **Cilj** broj **namjenski računalnim čvorove** za na resurse. To je broj čvorove računalnim koji na resurse moraju biti skalirana da biste. Budući da je da skup da ne dođete do cilja broj čvorove je broj "cilja". To se može dogoditi ako cilj broj čvorove je izmijenio ponovno kasnije automatsko skaliranje procjenu prije na resurse je Pristigla početne cilj. Ga se može dogoditi i ako obradu račun čvor "ili" Jezgra ograničenja zbirka dostigne prije zbirka dostigne cilj broj čvorove. |
| $NodeDeallocationOption | Akcija koja se pojavljuje kada računalnim čvorove uklanjaju se iz skupa. Moguće vrijednosti su:<ul><li>**requeue**– odmah prekida zadaci i stavlja ih ponovno reda čekanja tako da ih ponovno isplanirati.<li>**Prekid**– odmah prekida zadataka te ih uklanja iz reda čekanja.<li>**taskcompletion**– čekanje trenutno izvode zadatke da biste dovršili, a zatim uklanja čvor iz na resurse.<li>**retaineddata**– čeka za sve lokalne podatke zadatka zadržavaju čvor da biste očistiti prije uklanjanja čvor iz na resurse.</ul> |

Možete **dobiti** vrijednost od tih servisa definirane varijabli prilagodbe koje se temelje na metriku iz serije servisa:

| Samo za čitanje servisa definirane varijable | Opis |
| --- | --- |
| $CPUPercent | Prosječna postotak središnjeg procesora koristi. |
| $WallClockSeconds | Broj sekundi potrošena. |
| $MemoryBytes | Prosječan broj megabajta koji se koristi. |
| $DiskBytes | Prosječan broj gigabajta koji se koriste na lokalni disk. |
| $DiskReadBytes | Saznajte više broja bajtova. |
| $DiskWriteBytes | Broja bajtova napisali. |
| $DiskReadOps | Broj operacija čitanja diska izvršiti. |
| $DiskWriteOps | Broj pisanje na disku operacije obavljene. |
| $NetworkInBytes | Broj ulaznih bajtova. |
| $NetworkOutBytes | Broj izlaznih bajtova. |
| $SampleNodeCount | Broj računalnim čvorove. |
| $ActiveTasks | Broj zadataka u aktivni stanju. |
| $RunningTasks | Broj zadataka u stanju izvršavanja. |
| $PendingTasks | Zbroj $ActiveTasks i $RunningTasks. |
| $SucceededTasks | Broj zadataka koji je uspješno dovršena. |
| $FailedTasks | Broj zadataka koji nije uspjelo. |
| $CurrentDedicated | Trenutni broj namjenski izračunati čvorove. |

> [AZURE.TIP] Samo za čitanje, servis definirane varijable koje se prikazuju iznad su *objekata* koji sadrže različite metode pristup podacima povezana sa svakom. Dodatne informacije potražite u članku [Nabavljanje oglednih podataka](#getsampledata) ispod.

## <a name="types"></a>Vrste

Ove **vrste** podržane u formuli.

- Dvostruki
- doubleVec
- doubleVecList
- niz
- Vremenska oznaka – sustava složenih strukturu koja sadrži sljedeće članove:

    - godine
    - mjesec (1 do 12)
    - Day (1-31)
    - WEEKDAY (u oblik broja, npr. 1 za ponedjeljak)
    - HOUR (u 24-satni oblik broja, npr 13 znači 1 PM)
    - minute (00 do 59)
    - Second (00 do 59)
- timeinterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Operacije

Te **operacije** dopušteno na vrste koje su navedene.

| Postupak                             | Podržani operatora   | Vrsta rezultata   |
| ------------------------------------- | --------------------- | ------------- |
| Dvostruki *operator* double              | +, -, *, /            | Dvostruki            |
| Dvostruki *operator* timeinterval        | *                     | timeinterval      |
| doubleVec *operator* double           | +, -, *, /            | doubleVec         |
| doubleVec doubleVec *operatora*        | +, -, *, /            | doubleVec         |
| timeinterval *operator* double        | *, /                  | timeinterval      |
| timeinterval timeinterval *operatora*  | +, -                  | timeinterval      |
| Vremenska oznaka timeinterval *operatora*     | +                     | Vremenska oznaka         |
| Vremenska oznaka *operator* timeinterval     | +                     | Vremenska oznaka         |
| Vremenska oznaka *operator* vremenska oznaka        | -                     | timeinterval      |
| *operator*double                      | -, !                  | Dvostruki            |
| *operator*timeinterval                | -                     | timeinterval      |
| Dvostruki *operator* double              | < < =, ==, > =, >,! =  | Dvostruki            |
| niz *operator* niza              | < < =, ==, > =, >,! =  | Dvostruki            |
| Vremenska oznaka *operator* vremenska oznaka        | < < =, ==, > =, >,! =  | Dvostruki            |
| timeinterval timeinterval *operatora*  | < < =, ==, > =, >,! =  | Dvostruki            |
| Dvostruki *operator* double              | & &, & #124; & #124;      | Dvostruki            |

Kada testiranje dvaput ternary operatorom (`double ? statement1 : statement2`), **osim nule vrijedi**i nula je **false**.

## <a name="functions"></a>Funkcija

Ove unaprijed definirane **funkcije** dostupne su za korištenje prilikom definiranja formule automatskog skaliranja.

| Funkcija                          | Vrsta povrata   | Opis
| --------------------------------- | ------------- | --------- |
| Avg(doubleVecList)                | Dvostruki        | Vraća prosječnu vrijednost svih vrijednosti u na doubleVecList.
| LEN(doubleVecList)                | Dvostruki        | Vraća duljinu vektorski stvorenog iz na doubleVecList.
| lg(double)                        | Dvostruki        | Vraća zapisnik baza 2 udvostručavanje.
| lg(doubleVecList)                 | doubleVec     | Vraća componentwise zapisnika baza 2 na doubleVecList. Na vec(double) izričito mora proslijediti parametra. U suprotnom, dvostruki lg(double) verziju pretpostavlja se da je.
| ln(double)                        | Dvostruki        | Vraća prirodnim zapisnik udvostručavanje.
| ln(doubleVecList)                 | doubleVec     | Vraća componentwise zapisnika baza 2 na doubleVecList. Na vec(double) izričito mora proslijediti parametra. U suprotnom, dvostruki lg(double) verziju pretpostavlja se da je.
| log(double)                       | Dvostruki        | Vraća zapisnik baza 10 udvostručavanje.
| log(doubleVecList)                | doubleVec     | Vraća componentwise zapisnika baza 10 na doubleVecList. Na vec(double) izričito mora proslijediti jedan dvostruki parametra. U suprotnom, dvostruki log(double) verziju pretpostavlja se da je.
| MAX(doubleVecList)                | Dvostruki        | Vraća najveću vrijednost u na doubleVecList.
| min(doubleVecList)                | Dvostruki        | Vraća najmanju vrijednost na doubleVecList.
| Norm(doubleVecList)               | Dvostruki        | Vraća dva pravila vektorski stvorenog iz na doubleVecList.
| PERCENTILE (v doubleVec, dvostruki p) | Dvostruki        | Vraća percentil element vektorski v.
| rand()                            | Dvostruki        | Vraća slučajni vrijednost između 0.0 i 1.0.
| Range(doubleVecList)              | Dvostruki        | Vraća razliku između min i Maks vrijednosti u doubleVecList.
| std(doubleVecList)                | Dvostruki        | Vraća uzorak standardne devijacije od vrijednosti u doubleVecList.
| Stop()                            |               | Procjena tabulatora autoscaling izraza.
| SUM(doubleVecList)                | Dvostruki        | Vraća zbroj svih komponenti na doubleVecList.
| vrijeme (dateTime niza = "")          | Vremenska oznaka     | Ako se vraća vremenskog pečata trenutno vrijeme ako prosljeđuju se bez parametara ili vremenskog pečata s datumom i vremenom niza. Oblici dateTime podržani su W3C DTF i RFC 1123.
| Val (v doubleVec, dvostruki i)        | Dvostruki        | Prikazuje vrijednost elementa koji se nalazi na lokaciji i u vektoru v, s indeks s početne nule.

Kao argument neke funkcije koji su opisani u prethodno navedenoj tablici možete prihvatiti popis. Popis odvojenih zarezom je bilo koja kombinacija *Dvostruki* i *doubleVec*. Ako, na primjer:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Vrijednost *doubleVecList* se pretvara u jednom *doubleVec* prije izvođenja. Na primjer, ako `v = [1,2,3]`, zatim pozivanje `avg(v)` jednako zovete `avg(1,2,3)`. Pozivanje `avg(v, 7)` jednako Pozivna `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Nabavite oglednih podataka

Formula za automatsko skaliranje djeluje na metriku podataka (Uzorci) koja se nudi grupe za servis. Formule poveća ili smanjuje veličina skupa na temelju vrijednosti koje dohvaća iz servisa. Servis definirane varijable koje su prethodno opisan su objekata koji sadrže različite metode pristup podacima koji je pridružen tom objektu. Na primjer, sljedeći izraz prikazuje zahtjev da biste dobili posljednjih pet minuta procesora koristi:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Način | Opis |
| --- | --- |
| GetSample() | Na `GetSample()` način vraća vektor uzorka podataka.<br/><br/>Uzorak je 30 sekundi vrijedi metriku podataka. Drugim riječima, uzoraka dobivaju se svakih 30 sekundi. No postoji kao što je naveden u nastavku, razmak između prikupljanja uzorka i kad je dostupan na formulu. Kao takve, svi uzorci za određenog vremenskog razdoblja možda biti dostupno za procjenu formule.<ul><li>`doubleVec GetSample(double count)`<br/>Određuje broj uzoraka da biste dobili iz najnovije uzorke koje ste prikupili.<br/><br/>`GetSample(1)`Vraća posljednji dostupna uzorka. Za metriku sviđa `$CPUPercent`, no to se ne smiju koristiti jer nije moguće znaju *Kada* je prikupljeni uzorka. Možda je nedavno ili zbog problema s sustav, možda je mnogo starije. Je bolje u tim slučajevima da biste koristili vremenski interval kao što je prikazano u nastavku.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Određuje vremensko razdoblje za prikupljanje ogledne podatke. Po želji i određuje postotak uzorke koje se moraju biti dostupne u traženi vremenski okvir.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`vratiti 20 uzoraka ako su prisutne u povijesti CPUPercent sve uzorke za posljednjih deset minuta. Ako u zadnjoj minuti povijesti nije dostupna, no samo 18 primjere želite vratiti. U ovom slučaju:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`želite uspjeti jer dostupne su samo 90 postotak primjere.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`želite uspjeti.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Određuje vremenski okvir prikupljanje podataka, vrijeme početka i vrijeme završetka.<br/><br/>Kao što je rečeno iznad postoji razmak između prikupljanja uzorka i kad je dostupan na formulu. To morate smatrati prilikom korištenja u `GetSample` način. U odjeljku `GetSamplePercent` ispod.|
| GetSamplePeriod() | Vraća razdoblje uzoraka poduzete u skupu podataka povijesne uzorka. |
| Count() | Vraća ukupni broj uzoraka u metričkim povijest. |
| HistoryBeginTime() | Vraća vremenskog pečata najstarije uzorka podataka dostupnih za metriku. |
| GetSamplePercent() |Vraća postotak uzorke koje su dostupne u određenom vremenskom intervalu. Ako, na primjer:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Jer u `GetSample` postupak neće uspjeti ako postotak uzorke da funkcija Vrati manji od na `samplePercent` naveden, možete koristiti u `GetSamplePercent` način najprije je potrebno provjeriti. Zatim možete izvršiti akciju ako nema dovoljno uzoraka postoje, bez halting automatskog izračuna skaliranja.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Uzorci, postotak uzorak i metodu *GetSample()*

Osnovni postupak formule automatsko skaliranje je da biste dobili zadataka i resursa metričkim podataka i prilagoditi veličina skupa na temelju tih podataka. Kao takve, važno je imati Očisti razumijevanja način na koji se automatsko skaliranje formule rade s podacima metriku ili "uzorci."

**Uzorci**

Servis za obradu povremeno uzima *primjere* metriku zadataka i resursa i postaje dostupan za automatsko skaliranje formula. Ta uzorka bilježe svakih 30 sekundi grupe za servis. No postoji obično neke Latencija koji uzrokuje razmak između kad nisu snimljene te primjere i kada one postaju dostupne (i mogu čitati) automatsko skaliranje formula. Uz to, zbog različitim čimbenicima kao što su mreže ili drugi problemi infrastrukture primjere možda nisu snimljeni određenom razdoblju. Rezultira "nema" primjere.

**Ogledna postotka**

Kada `samplePercent` se prenosi u `GetSample()` način ili `GetSamplePercent()` način zove, "postotak" se odnosi na usporedbu između ukupni *mogući* broj uzoraka koji bilježe servis grupe i broj uzoraka koje su *dostupne* u svoju formulu automatsko skaliranje.

Pogledajmo vremenski raspon 10 minuta kao primjer. Jer uzoraka bilježe svakih 30 sekundi unutar 10 minuta mogući vremenski raspon Maksimalna ukupan broj uzoraka bilježe obradu bi 20 uzorka (2 u minuti). Međutim, zbog postojećih Latencija mehanizam za izvješćivanje ili neki drugi problem unutar Azure infrastrukture mogu postojati samo 15 uzorke koje su raspoložive za automatsko skaliranje formulu za čitanje. To znači da, za to razdoblje 10 minuta samo **75 posto** ukupan broj uzoraka snimljena dostupan u svoju formulu.

**Raspon GetSample() i uzorka**

Automatsko skaliranje formula ćete se i grupe – čvorove dodavanjem ili uklanjanjem čvorove. Jer čvorove troškova, želite biti sigurni da formula koristi Inteligentna način analizu koji se temelji na potrebne podatke. Stoga preporučujemo da koristite popularne vrsta analize u formulama. Ta vrsta bit će Povećaj i Smanji vaše grupe utemeljenog na *rasponu* prikupljene uzoraka.

Da biste to učinili, koristite `GetSample(interval look-back start, interval look-back end)` da biste se vratili **vektorski** uzorke:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Kada u gornjem retku spada grupe, će se vratiti raspon uzoraka kao vektor vrijednosti. Ako, na primjer:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Nakon što ste prikupili vektorski uzorka, možete koristiti funkcije poput `min()`, `max()`, a `avg()` za dobivanje glavne smislene vrijednosti iz raspona prikupljene.

Radi dodatne sigurnosti možete pokrenuti formule pri prijelazu na *neće uspjeti* ako je manji od određenog postotka uzorka dostupan za određeno vremensko razdoblje. Kada prisilno formule pri prijelazu uvoza, uputite obradu prestat dodatno vrednovanju formulu Ako navedeni postotak uzoraka nije dostupna –, a ne mijenja se veličina skup bit će. Da biste odredili potreban postotak uzoraka procjene da odredite kao parametar treći `GetSample()`. Ovdje je naveden preduvjet 75 posto uzorke:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Također je važno Odgoda prethodno spomenute u dostupnost uzorka, navesti vremenski raspon s vremenom početka izgleda natrag starija od jedne minute. To je zato što je potrebno više od jedne minute za uzorke proširiti u sustavu pa uzoraka u rasponu `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` često neće biti dostupne. Ponovno možete koristiti parametar postotka `GetSample()` da biste nametnuli preduvjet postotka određenog uzorka.

> [AZURE.IMPORTANT] Ne možemo **preporučujemo** taj *samo *izbjegli potrebe za oslanjanjem ** na `GetSample(1)` u vašem formule automatsko skaliranje **. Razlog je tome što `GetSample(1)` zapravo piše da servis obrade "Daj mi posljednje uzorka imate, bez obzira na to kako davno koji ste nabavili." Budući da je samo jedan uzorak i možda je starije uzorka, možda neće biti predstavnika veće slike nedavne zadatak ili stanje resursa. Ako koristite `GetSample(1)`, bili sigurni da je dio veći izjava i ne samo podatak koji se zasniva formulu na.

## <a name="metrics"></a>Mjerenja

Metriku **resursa** i **zadatak** možete koristiti kad definirate formulu. Prilagodite broj ciljne namjenski čvorove u grupe aplikacija utemeljenih na podacima metriku dobiti i procjenu. U odjeljku [varijable](#variables) iznad dodatne informacije na svakom metriku.

<table>
  <tr>
    <th>Mjerenja</th>
    <th>Opis</th>
  </tr>
  <tr>
    <td><b>Resurs</b></td>
    <td><p><b>Resurs metriku</b> temelje se na korištenje procesora, propusnost i memorije računalnim čvorove, kao i broj čvorove.</p>
        <p> Ove usluge definirane varijable su korisne za prilagođavanje čvor Brojanje na temelju:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Ove usluge definirane varijable su korisne za prilagođavanje koji se temelji na čvor resursa:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Zadatak</b></td>
    <td><p><b>Zadacima mjerenja</b> temelje se na status zadataka, kao što je aktivan, na čekanju, a dovršena. Sljedeće usluge definirane varijable su korisne za prilagođavanje skup veličina temelju metrike zadatka:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Pisanje formule automatsko skaliranje

Stvaranje formule automatsko skaliranje stvaranjem naredbe koje koriste iznad komponente, zatim kombiniranje te izjave u Dovršeno formulu. U ovom ćete odjeljku smo stvorit ćete primjer formule automatsko skaliranje koje možete izvršiti neke odluke skaliranja stvarnog života.

Najprije ćemo definirati zahtjeve za naše novu formulu automatsko skaliranje. Formula treba:

1. **Povećavanje** cilj broj računalnim čvorove u skup ako je procesora visoka.
2. **Smanjite** broj ciljne računalnim čvorove u skup kada nedostaje procesora.
3. Uvijek ograničiti **maksimalni** broj čvorove 400.

Da biste *povećali* broj čvorove tijekom visoke procesora, ne možemo definirati rečenicu koja popunjava korisnički definirane varijable (`$totalNodes`) vrijednost koja je postotak 110 broj trenutnog cilj čvorove, ali samo ako minimalni prosjek procesora koristi tijekom posljednjih 10 minuta je iznad 70 posto. U suprotnom se koristi trenutnu vrijednost namjenski.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

Da biste *smanjili* broj čvorove tijekom niskog procesora, sljedeće naredbe u našem formuli postavlja isti `$totalNodes` varijable za 90 postotak trenutni broj ciljne čvorove ako average procesora u proteklih 60 minuta je u odjeljku 20 posto. U suprotnom koristite trenutnu vrijednost `$totalNodes` koje ćemo popunjavaju u izjavi iznad.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Sada ograničiti broj ciljne namjenski računalnim čvorove da biste na **Maksimalna** vrijednost 400:

```
$TargetDedicated = min(400, $totalNodes)
```

Ovdje je dovršena formula:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Stvaranje grupe za automatsko skaliranje s omogućenim

Da biste stvorili novi skup autoscaling omogućena, možete koristiti jedan od sljedećih načina:

**Grupe za .NET**

1. Stvorite na resurse s [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx).
1. Postavite svojstvo [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) na `true`.
1. Postavite svojstvo [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) s formulom automatsko skaliranje.
1. (Neobavezno) Postavite svojstvo [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) (Zadana vrijednost je 15 minuta).
1. Izvršavanje skup [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) ili [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx).

**Obradu REST API-JA**

* [Dodavanje grupe aplikacija s poslovnim subjektom](https://msdn.microsoft.com/library/azure/dn820174.aspx): odredite na `enableAutoScale` i `autoScaleFormula` elemenata u zahtjevu za REST API-JA da biste konfigurirali automatsko skaliranje za zajedničko područje kad ga stvorite.

Sljedeći isječak koda stvara automatsko skaliranje s omogućenim grupe aplikacija pomoću [Grupe za .NET] [ net_api] biblioteke. Formula za automatsko skaliranje na resurse postavlja cilj broj čvorove 5 na Ponedjeljci i 1 na svaki dan u tjednu. [Interval automatskog skaliranja](#automatic-scaling-interval) postavljen je na 30 minuta. U ovom i u drugim C# isječci u ovom članku, "myBatchClient" nije pravilno inicijalizirana pojavljivanja [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Osim obradu REST API-JA i .NET SDK, možete koristiti bilo koji od na druge [Grupe SDK-ovi](batch-technical-overview.md#batch-development-apis), [cmdleta ljuske PowerShell za obradu](batch-powershell-cmdlets-get-started.md)i [EŽA grupe](batch-cli-get-started.md) da biste radili s autoscaling.

> [AZURE.IMPORTANT] Kada stvorite grupu za automatsko skaliranje s omogućenim, morate **ne** odredite na `targetDedicated` parametar. Osim toga, ako želite ručno promijeniti veličinu skup za automatsko skaliranje s omogućenim (Ako, na primjer, s [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), zatim morate prvi **Onemogućivanje** automatske promjene veličine na na resurse, a zatim promijenite veličinu.

### <a name="automatic-scaling-interval"></a>Interval automatskog skaliranja.

Prema zadanim postavkama, servis za obradu prilagođava veličinu u skup prema njegova formula automatsko skaliranje svakih **15 minuta**. Interval je konfigurirati, no pomoću sljedećih svojstava za skup:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (skupna .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API-JA)

Najmanji interval pet minuta, a maksimum 168 sati. Ako je naveden interval izvan tog raspona, servis za obradu će vratiti pogrešku u neispravan zahtjev (400).

> [AZURE.NOTE] Autoscaling trenutno nije namijenjen da odgovaraju promjenama manje od nekoliko minuta, ali umjesto namijenjen da biste prilagodili veličinu svoje skup postupno kao pokrenete radno opterećenje.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Omogućivanje autoscaling na postojeću grupu

Ako ste već stvorili zajedničko područje s brojem skup računalnim čvorove pomoću parametar *targetDedicated* , i dalje možete omogućiti autoscaling na na resurse. Svaki obrade SDK sadrži operaciju "Omogući automatsko skaliranje", na primjer:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (skupna .NET)
*  [Omogući automatsko skaliranje na zajedničko područje] [ rest_enableautoscale] (REST API-JA)

Kada omogućite autoscaling na postojeću grupu, primjenjuje se sljedeće:

* Ako automatsko skaliranje trenutno **onemogućen** na resurse kada je poslao zahtjev za "Omogući automatsko skaliranje", *morate* navesti formula valjana automatsko skaliranje kada je poslao zahtjev. *Po želji* možete navesti interval procjenu automatsko skaliranje. Ako ne navedete interval, koristi se zadanu vrijednost od 15 minuta.

* Ako automatsko skaliranje trenutno nije **omogućena** na resurse, možete odrediti formulu automatsko skaliranje, procjenu interval ili oboje. Ne možete izostaviti oba svojstva.

  * Ako navedete interval za procjenu novi automatsko skaliranje postojeći raspored procjenu je zaustavljena i pokreće novi raspored. Vrijeme početka novi raspored je vrijeme izdavanja zahtjev za "Omogući automatsko skaliranje".

  * Ako izostavite automatsko skaliranje formulu ili intervala za procjenu, servis za obradu nastaviti koristiti trenutnu vrijednost tu postavku.

> [AZURE.NOTE] Ako vrijednost nije naveden za parametar *targetDedicated* kada je stvorena na resurse, se zanemaruje prilikom procjene automatskog skaliranja formulu.

Ovaj C# koda koristi [Obradu .NET] [ net_api] biblioteke da biste omogućili autoscaling na postojeće grupe aplikacija:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Ažuriranje formule automatsko skaliranje

Koristite isti "Omogući automatsko skaliranje" zahtjev za *Ažuriranje* formulu na postojeći skup automatsko skaliranje s omogućenim (na primjer, s [EnableAutoScale] [ net_enableautoscale] u .NET obradu). Postoji nijedna posebno "ažuriranje automatsko skaliranje" operacija. Na primjer, ako autoscaling je već omogućeno na "myexistingpool" kada se izvršava sljedeći kod, njegova automatsko skaliranje formula je zamijenjena funkcijom sadržaj `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Automatsko skaliranje interval ažuriranja

Kao što je s ažuriranje formule automatsko skaliranje, koristite iste [EnableAutoScale] [ net_enableautoscale] način da biste promijenili intervala procjenu automatsko skaliranje postojeći skup automatsko skaliranje omogućena. Na primjer, da biste postavili interval za procjenu automatsko skaliranje 60 minuta za skup koji je već automatsko skaliranje omogućen:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Analiza formule automatsko skaliranje

Formula može vrednovati prije primjene na resurse. Na taj način, na raspolaganju vam je "test Pokreni" formule da biste vidjeli kako će se njegove naredbe procijeniti prije nego što postavite formulu u radni.

Da biste procijenili formule automatsko skaliranje, morate prvi **omogućiti autoscaling** na skup **valjanu formulu**. Ako želite da biste testirali formulu na skup koji još nema autoscaling omogućena, možete koristiti jedan redak formulu `$TargetDedicated = 0` kada prvi put omogućite autoscaling. Nakon toga pomoću nešto od sljedećeg analiza formule koje želite testirati:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) ili [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Ove metode grupe za .NET zahtijevaju ID postojeće grupe aplikacija i niz koji sadrži formulu automatsko skaliranje za procjenu. Rezultati procjenu nalaze se u vraćenom [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) instanci.

* [Procjena automatskog skaliranja formule](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    U ovaj zahtjev REST API-JA navedite ID-a skup URI i automatsko skaliranje formulu u tijelu zahtjeva elementu *autoScaleFormula* . Odgovor operacije sadrži sve pogreške informacije koje se mogu biti povezane s formulom.

U [Grupe za .NET] [ net_api] isječak koda, ne možemo analiza formule prije primjene [CloudPool][net_cloudpool]. Ako na resurse autoscaling omogućena, ne možemo je omogućiti prvi put.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Uspješan izračun formula u ovom isječak rezultirat će izlaz sličnu ovoj:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Informacije o pokreće automatsko skaliranje

Da biste osigurali formulu izvršava prema očekivanjima, preporučujemo da povremeno rezultate autoscaling "se pokreće" obradu izvodi na vašem grupe aplikacija. Stoga se (ili Osvježi) referenca na resurse i pregledajte svojstva njegova zadnjeg automatsko skaliranje pokrenuti.

Svojstvo [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) .NET obradu ima nekoliko svojstava dajući informacije o najnovije automatskog skaliranja Mada izvesti na resurse grupe za servis.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

U REST API-JA, zahtjeva za [pristup informacijama o zajedničko područje](https://msdn.microsoft.com/library/dn820165.aspx) vraća informacije o skup sadrži najnovije automatskog skaliranja pokretanje informacije u [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun).

Sljedeće C# koda koristi biblioteku .NET grupe da biste ispisali informacije o posljednje autoscaling pokrenuti na skup "myPool":

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Ogledna Izlaz iz prethodnog isječak:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Primjer formule za automatsko skaliranje

Pogledajmo na nekoliko formula koje objašnjeni su različiti načini da biste prilagodili računalnim resursa u zajedničko područje.

### <a name="example-1-time-based-adjustment"></a>Primjer 1: Prilagođavanje koji se temelji na vrijeme –

Možda želite prilagoditi veličinu skup koji se temelji na dan u tjednu i dana da biste povećali ili smanjili broj čvorove u skladu s tim.

Ova formula najprije dohvaća trenutnog vremena. Ako je weekday (1-5) i unutar radnih sati (8 AM za 18: 00) Odredišna veličina skup postavljen je na 20 čvorove. U suprotnom je postavljen na 10 čvorove.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Primjer 2: Utemeljenoj na zadacima prilagodbe

U ovom primjeru skup veličina se prilagođava ovisno o broju zadataka u redu čekanja. Imajte na umu da komentare i prijelome prihvatljiva u nizovima formule.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Primjer 3: Računovodstvo za paralelne zadatke

To je drugi primjer prilagođava veličinu skup ovisno o broju zadataka. Ovu formulu i uzima u obzir [MaxTasksPerComputeNode] [ net_maxtasks] vrijednost koja je postavljena za na resurse. To je osobito je korisna u slučajevima gdje je omogućena [izvođenja paralelno zadatka](batch-parallel-node-tasks.md) na vašem skup.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Primjer 4: Postavljanje veličine do početne skup

U ovom je primjeru prikazano C# kod uzastopna s formule automatsko skaliranje koja se postavlja veličinu skup u čvorove broj početnog razdoblja. A zatim ga prilagođava veličinu skup ovisno o broju pokretanje i aktivni zadaci nakon početnog vremensko razdoblje isteka.

Formula u sljedećim koda:

- Postavlja veličinu početni skup četiri čvorove.
- Prilagodba veličine skup unutar prvog 10 minuta životni ciklus na resurse.
- Nakon 10 minuta dohvaća vrijednost maksimalni broj tekućeg i aktivni zadaci unutar proteklih 60 minuta.
  - Ako su oba vrijednosti 0 (koji označava da nema zadataka izvodi ili su aktivan u zadnji 60 minuta), veličina skup je postavljeno na 0.
  - Ako je neki vrijednost veća od nule, bez promjene.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Daljnji koraci

* [Maksimiziranje obradu Azure izračunati resursa sa zadacima Istodobni čvor](batch-parallel-node-tasks.md) sadrži detalje o kako se može izvesti više zadataka istodobno na računalnim čvorove u svoje grupe aplikacija. Osim autoscaling, ta značajka može pomoći u donji trajanje zadatka za neke radnih opterećenja, spremanje novca.

* Za drugi booster učinkovitosti, provjerite je li aplikacija obrade upita servis za obradu u najčešće optimalnih način. [Učinkovito upita servisa Azure grupe](batch-efficient-list-queries.md), ćete saznat ćete kako ograničiti količinu podataka koji obuhvaća na žičani kada upit status potencijalno tisuće računalnim čvorove ili zadatke.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
