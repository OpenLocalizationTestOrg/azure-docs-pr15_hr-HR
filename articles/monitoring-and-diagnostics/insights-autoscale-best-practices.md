<properties
    pageTitle="Najbolje prakse za autoscaling Azure Monitor. | Microsoft Azure"
    description="Saznajte načela učinkovito korištenje autoscaling u Azure Monitor."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Najbolje prakse za autoscaling Azure monitora

U sljedećim se odjeljcima u ovom dokumentu pomoću kojih se objašnjava najbolje prakse za Azure za automatsko skaliranje. Nakon pregleda taj podatak, koju ćete moći bolje učinkovito korištenje automatsko skaliranje u infrastruktura za Azure.

## <a name="autoscale-concepts"></a>Automatsko skaliranje koncepti

- Resurs može imati samo *jedan* automatsko skaliranje postavka
- Postavka za automatsko skaliranje možete biti jedan ili više profila i svaki profil mogu sadržavati jedno ili više pravila za automatsko skaliranje.
- Postavka za automatsko skaliranje mijenja veličinu instance vodoravno, koji se *slaže* povećavanjem instanci i *u* tako da smanjite broj instanci.
 Postavka za automatsko skaliranje ima Maksimalna vrijednost za minimum i Zadana vrijednost instanci.
- Za automatsko skaliranje posla uvijek čita pridružene metriku za promjenu veličine tako Provjera ako ona sadrži su prijeđene praga konfiguriran za skaliranje Izlaz ili promjena veličine u. Možete prikazati popis od metriku te automatsko skaliranje mogu mijenjati veličinu tako da na [Azure Monitor autoscaling uobičajenih metriku](insights-autoscale-common-metrics.md).
- Sve pragovi izračunavaju se na razini instance. Na primjer, "skaliranje smanjivati po 1 instancu kada Prosječna procesora > 80% kada je broj instanci 2", znači skaliranje out, kada je veće od 80% average procesora preko sve instance.
- Uvijek primanje obavijesti o putem e-pošte. Konkretno, vlasnika, suradnika i čitatelji resursa cilj primanje e-pošte. *Oporavak* e-pošte i uvijek primiti kada automatsko skaliranje oporavlja iz pogreške i pokreće normalno funkcionira.
- Koje možete slaganja prima uspješno skala akcija obavijest putem e-pošte i webhooks.

## <a name="autoscale-best-practices"></a>Najbolje prakse za automatsko skaliranje

Koristite sljedeće najbolje prakse prilikom korištenja automatsko skaliranje.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Provjerite je li najveće i najmanje vrijednosti razlikuju se i imaju odgovarajuće margina između njih
Ako imate postavku koja sadrži najmanje = 2, maksimalna = 2 i trenutne instance je broj 2, javlja se nijedna akcija mjerilo. Ostavite dovoljno margina između broji najveće i najmanje instanci koje su uključujući te dvije vrijednosti. Automatsko skaliranje uvijek mijenja veličinu između ta ograničenja.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Ručna promjena veličine je ponovno postaviti tako da je automatsko skaliranje min i max
Ako ručno ažurirati broj instanci iznad ili ispod maksimalne vrijednosti, engine automatsko skaliranje mijenja veličinu automatski minimum (Ako je ispod) ili maksimum (Ako je iznad). Na primjer, postavite raspon između 3 i 6. Ako imate jedan pokrenute instance, engine automatsko skaliranje mijenja veličinu u 3 instance na njegov sljedećeg izvođenja. Isto tako, ona će skaliranje u 8 instance natrag na 6 na njegov sljedećeg pokretanja.  Ručna promjena veličine je vrlo privremeno osim ako ponovno postaviti pravila automatsko skaliranje.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Uvijek koristi kombinaciju skaliranje Izlaz i promjena veličine u pravila koja izvršava povećanje i smanjenje
Ako koristite samo jedan dio kombinacije, automatsko skaliranje skaliranje dodatak koji jednostruki izgleda ili, dok se ne maksimum ili minimum zbirka dostigne.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Ne prebacivanje između portala za Azure i portala za Azure klasični prilikom upravljanja automatsko skaliranje
Za servise u Oblaku i aplikacije usluge (Web Apps), pomoću portala za Azure (portal.azure.com) možete stvarati i upravljati postavkama automatsko skaliranje. Za skupove skaliranje virtualnog računala pomoću PoSH, EŽA ili REST API-JA možete stvarati i upravljanje postavkama automatsko skaliranje. Prebacivanje između Azure klasični portal (manage.windowsazure.com) i Azure portal (portal.azure.com) kada upravljanju konfiguracijama automatsko skaliranje. Azure klasični portal i njegov podlozi pozadinskog ima ograničenja. Premještanje Azure portal za upravljanje automatsko skaliranje pomoću grafičkog sučelja. Da biste koristili automatsko skaliranje PowerShell, EŽA ili REST API-JA (putem Explorer resursa za Azure) su sljedeće mogućnosti.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Odaberite odgovarajuće statistike vaše metrike dijagnostiku
Za metriku Dijagnostika možete među *prosjek*, *Minimum*, *Maksimalna* i *ukupan* kao metrike za promjenu veličine tako da. Najčešći statističkog je *prosjek*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Odaberite pragovi pažljivo za sve vrste mjerenja
Preporučujemo da pažljivo odaberete različite pragovi skaliranje iz i skaliranje u koji se temelji na praktično situacije.

Ne možemo *ne preporučujemo* automatsko skaliranje postavke kao što su primjeri dolje s istim ili slične praga vrijednosti za odgovor i u uvjetima:

- Povećavanje instance 1 Brojanje kada broja niti < = 600
- Smanjivanje instance 1 kada prebrojavanje broja niti > = 600

Pogledajmo primjer što može dovesti do ponašanje koje se mogu prestati zbunjujuće. Cosider sljedeće nizu.

1. Pretpostavimo da postoje počinje sa 2 slučajevi, a zatim Prosječan broj niti po instancu poveća na 625.
2. Automatsko skaliranje mijenja veličinu out dodavanje 3 instance.
3. Nakon toga pretpostavlja da count average niti preko instancu pada na 575.
4. Prije promjene veličine prema dolje, bit će automatsko skaliranje pokušava procjenu koje u završno stanje ako skalirana u. Na primjer, 575 x 3 (broj trenutne instance) = 1,725 / 2 (konačni broj instanci kada skalirana prema dolje) = 862.5 niti. To znači da automatsko skaliranje bit će potrebno odmah skaliranje iz ponovno čak i nakon skalirana se, ako je broj average niti ostaje ili čak i pada samo mali. Međutim, ako skalirana gore ponovno cijeli postupak bi ponovite, oni mogu dovesti do beskonačno Ponavljaj.
5. Da biste izbjegli tom slučaju (termed "flapping"), automatsko skaliranje ne skaliranje uopće. Umjesto toga preskače te reevaluates uvjet sljedeći put servisa izvršava. To može zbuniti mnogi jer se automatsko skaliranje ne funkcionira prilikom count average niti je 575.

Da biste izbjegli "flappy" situacijama namijenjen je procjeni tijekom na skaliranje u. Takvo ponašanje Imajte na umu kad odaberete isti pragovi za skaliranje iz i u.

Preporučujemo da odaberete odgovarajuće margina između odgovaranjem skaliranje i pragovi. Kao primjer, razmotrite sljedeća kombinacija bolje pravilo.

- Povećavanje instance 1 Brojanje kada procesora % > = 80
- Smanjivanje instance 1 Brojanje kada procesora % < = 60

U ovom slučaju  

1. Pretpostavimo da nema započeti s 2 instanci.
2. Ako je prosječna % procesora kroz sve instance vodi do 80, automatsko skaliranje mijenja veličinu out dodavanje treću pojavu.
3. Sada pretpostavlja da tijekom vremena % procesora pada u 60.
4. Automatsko skaliranje na skaliranje u pravilo procjenjuje završno stanje ako su za skaliranje. Na primjer, 60 x 3 (broj trenutne instance) = 180 / 2 (konačni broj instanci kada skalirana prema dolje) = 90. Da bi automatsko skaliranje ne skaliranje u jer bi mora skaliranje iz ponovno odmah. Umjesto toga ga preskače skaliranje prema dolje.
5. Automatsko skaliranje za sljedeći put provjerava, Procesor će se i dalje nalaziti 50. Procjenjuje ga ponovno - 50 x 3 instancu = 150 / 2 instance = 75; koja se nalazi ispod praga odgovaranjem skaliranje od 80, tako da ga mijenja veličinu u uspješno na 2 instance.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Zahtjevi za skaliranje vrijednosti praga za posebne mjerenja
 Za posebne metriku kao što je prostor za pohranu ili red čekanja Bus servisu metriku duljina je prag Prosječan broj poruka dostupnim po trenutni broj instanci. Pažljivo odaberite odabir vrijednosti praga za ovu metriku.

Pogledajmo prikazuju ga s primjerom da biste bili sigurni da razumijete bolje ponašanje.

- Povećavanje instance broj 1 kada reda čekanja za pohranu poruka count > = 50
- Smanjivanje instance broj 1 kada reda čekanja za pohranu poruka count < = 10

Imajte na umu sljedeće nizu:

1. Postoje slučajevi za reda 2 za pohranu.
2. Poruka pristizati i nakon što ste pregledali reda čekanja za pohranu, ukupan broj čita 50. Možda pretpostavlja taj automatsko skaliranje bi trebao počinjati skaliranje iz akcija. Međutim, imajte na umu da je i dalje 50/2 = 25 poruka po instance. Tako, skala iz ne pojavljuje. U prvom skaliranje iz dogoditi, ukupan broj poruka u redu čekanja za pohranu mora biti 100.
3. Nakon toga pretpostavlja ukupan broj poruka dođe do 100.
4. 3 instancu reda čekanja za pohranu dodaje se zbog skaliranje iz akcija.  Nikakvu akciju skaliranje iz će se dogoditi ne dok ukupan broj poruka u redu čekanja jer je dostigne 150 150/3 = 50.
5. Sada će se broj poruka u redu čekanja manje. S 3 instancama prvi skaliranje u akciju se događa kada ukupna poruke u svim redovima dodati do 30 jer 30/3 = 10 poruka po instancu, što je prag skaliranje u.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Zahtjevi za skaliranje kada više profila nije konfiguriran u je postavka automatsko skaliranje

U postavkama sustava automatsko skaliranje zadanog profila koji se uvijek primjenjuje bez sve zavisnost o raspored ili vremena, možete odabrati ili možete odabrati ponavljajućeg profila i profila za fiksnog razdoblja unutar nekog raspona datuma i vremena.

Kada je servis za automatsko skaliranje obrađuje ih, je uvijek provjerava sljedećim redoslijedom:

1. Fiksni datum profila
2. Ponavljajuća profila
3. Zadani profil ("uvijek")

Ako se ispuni uvjet profila, automatsko skaliranje potvrdite sljedeći uvjet profila ispod njega. Automatsko skaliranje samo obrađuje jedan profil odjednom. To znači da ako želite obuhvaćaju obrada uvjeta iz profila donjem sloja, morate uključiti ta pravila kao i u trenutni profil.

Pogledajmo to pomoću jednog primjera:

Na donjoj slici prikazano je postavka automatsko skaliranje s zadani profil minimalne instanci = 2 i maksimalne instance = 10. U ovom primjeru pravila konfigurirani tako da skala iz kada je broj poruka u redu čekanja veća od 10 i promjena veličine u kada je broj poruka u redu čekanja manje od 3. Odmah resursa mogu mijenjati veličinu između 2 i 10 instance.

Uz to, postoji ponavljajućeg profil za ponedjeljak. Postavlja se minimalne instanci = 2 i maksimalne instance = 12. To znači da u ponedjeljak, automatsko skaliranje za prvi put provjerava ovaj uvjet ako je broj instanci 2, mijenja veličinu novi najmanje 3. Dok god automatsko skaliranje Nastavi da biste pronašli ovaj uvjet profila uparuje (ponedjeljak), samo obrađuje na procesora pravila temeljena na skaliranje Izlaz/u konfigurirana za ovaj profil. Trenutno se ne provjerava Duljina reda čekanja. Međutim, ako želite uvjet Duljina reda čekanja potrebno provjeriti, trebate uključiti ta pravila iz zadanog profila kao i u svom profilu ponedjeljak.

Isto tako, kada automatsko skaliranje prebacuje natrag na zadani profil, najprije provjeravaju se ako su minimalne i maksimalne uvjeti zadovoljeni. Ako je broj instanci u trenutku 12, ona mijenja veličinu u do 10, maksimalne dopuštene zadani profil.

![Automatsko skaliranje postavke](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Zahtjevi za skaliranje kada više pravila nije konfiguriran u profil
Postoje slučajevi u kojima možda ćete morati postaviti više pravila u profilu. Sljedeći skup pravila za automatsko skaliranje koriste se pomoću servisa kada više pravila.

*Skaliranje out*automatsko skaliranje pokreće ako se ispuni bilo koje pravilo.
Na *Skaliranje u*odjeljku Automatsko skaliranje zahtijevaju sva pravila da se zadovolje.

Da biste ilustrirali, pretpostavlja da imate sljedeće 4 automatsko skaliranje pravila:

- Ako procesora < 30% skaliranje u 1
- Ako memorije < 50%, skala-1
- Ako procesora > 75%, skala iz 1
- Ako memorije > 75%, skaliranje iz 1

Zatim prati dogodit će se:

- Ako procesora je 76%, a memorija je 50%, ne možemo skaliranje iz.
- Ako procesora je 50%, a memorije 76% smo skaliranje iz.

S druge strane, ako procesora je 25%, a memorije automatsko skaliranje 51% ne **ne** skaliranje-u. Kako bi skaliranje-u, procesora mora biti 29% i memorije 49%.

### <a name="always-select-a-safe-default-instance-count"></a>Uvijek odaberite ukupan sigurni zadane instance
Instance broj zadani je važno mijenja automatsko skaliranje veličinu na servisu za taj broj kada metriku nisu dostupne. Dakle, odaberite broj instanci zadane sigurne za vaše radnih opterećenja.

### <a name="configure-autoscale-notifications"></a>Konfiguriranje obavijesti na automatsko skaliranje
Automatsko skaliranje obavještava administratori i suradnici resursa putem e-pošte ako se pojavi bilo koji od sljedećih uvjeta:

- Servis za automatsko skaliranje neće uspjeti poduzeti akciju.
- Metriku trenutno nije dostupna za servis za automatsko skaliranje odluku mjerilo.
- Metriku su dostupni (oporavak) da biste donošenje odluka mjerilo.
Uz gore navedene uvjete, možete konfigurirati obavijesti e-pošte ili webhook za primanje obavijesti o za uspješan skala akcije.
