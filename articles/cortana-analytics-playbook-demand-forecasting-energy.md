<properties
    pageTitle="Cortana obavještavanje rješenje predložak Playbook za predviđanje zahtjev od energiju | Microsoft Azure"
    description="Rješenje predložak s Microsoft Cortana obavještavanje koji olakšava predviđanja zahtjev za poduzeće utility energiju."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana obavještavanje rješenje predložak Playbook za zahtjev predviđanje od energiju  

## <a name="executive-summary"></a>Izvršni sažetak  

U zadnjih nekoliko godina da biste stvorili veliku prilike u domeni utility i energiju su spaja Internet stvari (IoT), druge energiju izvora i velikih skupova podataka. U isto vrijeme uslužni i sektora cijeli energiju ste vidjeti potrošnje stapanja s koje korisnici zahtjevne bolji načini kontrolirati njihova upotreba energiju. Dakle, utility i pametnih rešetke tvrtki su odličan potrebe pokretati inovacije i obnavljanje sami. Osim toga, mnogo potencija i utility rešetke se sve zastarjele i vrlo skup za održavanje i upravljanje njima. Tijekom prošle godine u tim sadrži radili broj obaveze unutar energiju domene. Tijekom te obaveze ne možemo naišao mnogim slučajevima u kojima uslužni programi ili ISV-ovi (proizvođačima softvera) ste je pogled u predviđanje za buduće energiju zahtjev. Ove predviđanja reproducirajte važnu ulogu u svoje trenutne i buduće business i prekorače foundation za različite slučajeve korištenja. To obuhvaća budu kratke i Dugoročne power učitavanja predviđanje, trgovina, opterećenja, rešetke optimizaciju itd. Velikih skupova podataka i načine napredne analize (AA) kao što su strojnog učenja (ML) su ključa enablers za proizvodnju točni i pouzdan predviđanja.  

U ovom playbook ćemo sastavili tvrtke i analytical smjernice koja su potrebna za uspješan razvoj i implementaciju energiju zahtjev predviđanja rješenja. Ovim smjernicama predložene može pridonijeti uslužni programi, fizičari podataka i podataka inženjeri u uspostavljanje potpuno operationalized u oblaku, zahtjev predviđanje rješenja. Za tvrtkama koje samo pokretanja njihove velikih skupova podataka i naprednom analitikom putovanja, kao što je rješenje može se odnositi na početnu vrijednost u svoje Dugoročne strategije pametno rešetke.

>[AZURE.TIP] Da biste preuzeli dijagram koji nudi pregled ovog predloška za arhitekture, potražite [predložak rješenja za obavještavanje Cortana arhitektura za predviđanje zahtjev od energiju](cortana-analytics-architecture-demand-forecasting-energy.md).  

## <a name="overview"></a>Pregled  

Ovaj dokument pokriva tvrtke, podataka i tehničke aspektima korištenja Cortane Inteligencija i u određeni Azure strojnog učenja (AML) za implementaciju i implementacija rješenja predviđanje energiju. Dokument sastoji se od tri glavna dijela:  

1. Razumijevanje tvrtke  
2. Razumijevanje podataka  
3. Tehničku implementaciju

**Razumijevanje tvrtke** dio navode razmjer tvrtke nešto treba razumijevanje i preporučujemo prije uspostavljanja programa investicija odlučivanja. Objašnjava ispunjavate uvjete poslovne probleme konkretnom da bi bili predvidljivu analize i strojnog učenja uistinu učinkovitih i primjenjivo. U dokumentu objašnjava osnove strojnog učenja i kako se koristi da biste riješili probleme energiju predviđanja. Se navode preduvjete i kriterij kvalifikacije korištenje slučaja. Primjerima pomoću slučajeve i slučaj poslovne scenarije također se navode.

Podaci su glavni ingredient za sve strojnog učenja rješenja. **Razumijevanje podataka** dio ovog dokumenta pokriva neke važne aspekte podatke. Se navode vrstu podataka koje je potrebno za predviđanje energiju, potrebama kvalitete podataka i obično postoji koje izvora podataka. Ne možemo objašnjava kako se koristi sirovim podacima Priprema podataka značajke koje se zapravo pogon Modeliranje dio.

Treći dio dokumenta pokriva razmjer **Tehničku implementaciju** rješenja. Značajka inženjerska i Modeliranje na Osnovni proces znanstvenog podataka i stoga su koji se spominju u nekoliko detalja. Pokriva pojam web-servisi koji su važne vozila oblaka implementacije rješenja predvidljivu analize. Ne možemo strukture i arhitekturu uobičajeni do kraja do kraja operationalized rješenja.

Dokument obuhvaća i referencu materijale koje možete koristiti da biste dobili dodatnu razumijevanja domene i tehnologije.

Je važno je napomenuti da ne možemo ne namjeravate naslovnice u ovom dokumentu dublju znanstvenog postupka podataka, njegov matematičkih i tehničke aspekte. Te detalje pronaći ćete u [dokumentaciji Azure ML](http://azure.microsoft.com/services/machine-learning/) i [blogova](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Ciljne publike   
Ciljne publike za taj dokument je tvrtke i Tehnički osoblju koji biste željeli da bi se dobio znanja i razumijevanja strojnog učenja na temelju rješenja i kako se koriste se izričito unutar domene energiju predviđanja.

Fizičari podataka i može se koristiti u ovom dokumentu da bismo bolje razumjeli više razine procesa koji se pogoni implementaciju sustava energiju predviđanje rješenja za čitanje. U ovom kontekstu i korištenja da biste uspostavili dobar referentne vrijednosti i početnu točku za više detaljne i materijala za napredno.

### <a name="industry-trends"></a>Industrijske trendova  
U zadnjih nekoliko godina da biste stvorili veliku prilike u prostoru utility i energiju su spaja IoT, zamjenski energiju izvora i velikih skupova podataka. U isto vrijeme uslužni i sektori cijelu energiju ste vidjeti potrošnje stapanja s koje korisnici zahtjevne bolji načini kontrolirati njihova upotreba energiju.

Mnoge utility i pametnih energiju tvrtke ste je PC prikaz u sustavu [pametno rešetke](https://en.wikipedia.org/wiki/Smart_grid) uvođenjem broj koristi slučajeva koje se koriste podataka generira rešetke. Mnogim slučajevima koristi vezane uz osobina postojećih radnih električne energije: ne može biti akumulirani ni odvojite pohranjenih kao zaliha. Tako, koja se koristi. Uslužni programi koji želite pretvoriti učinkovitiji morati jednostavno predviđanja potrošnju energije jer koji će im dodijelili veći mogućnost **Saldo opskrbu i zahtjev**, što wastage energiju, **smanjite greenhouse goriva ispuštanje**i kontrola trošak.

Kada se radi o troškovima, postoji drugi važne aspekte, što je cijena. Novi kako biste trgovanje power između uslužni programi uveli ste sjajno potrebe za **predviđanje buduće zahtjev i buduće cijena električne energije**. To može pomoći tvrtki određuju njihov količine radnog.

Kada koristimo riječ "pametno", ne možemo zapravo odnose s rešetkom koji možete informacije, a zatim unesite predviđanja. Ga možete će sezonski promjene u potrošnje kao i **foresee privremene preopterećenja situacijama i automatski prilagodi za njega**. Po daljinski regulating potrošnje (uz pomoć ove pametne metar), može riješiti lokalizirane preopterećenja situacije. **Tako da najprije predviđanje, a zatim u**rešetki čini sam pametnije tijekom vremena.

Do kraja ovog dokumenta ne možemo će usredotočite se na određene obitelji slučajeva za korištenje pokrivaju predviđanje od budućnost, kratki termin i Dugoročne energiju zahtjev. Radimo ste radili u ta područja nekoliko mjeseca i stekli neke znanja i vještina koji omogućuju nam dobivanje industrijskih ocjene rezultata. Drugim slučajevima korištenje će biti obuhvaćeno kao i dokument u skorijoj budućnosti.

## <a name="business-understanding"></a>Razumijevanje tvrtke

### <a name="business-goals"></a>Ciljevi za tvrtke
Cilj **Pokazni videozapis energiju** jest da bismo pokazali uobičajeni predvidljivu analize i strojnog učenja rješenje koje možete uvesti u okviru vrlo kratko vrijeme. Konkretno, je naš trenutni fokus na omogućavanje energiju zahtjev predviđanja rješenja tako da vrijednost tvrtke mogu biti brzo Rač i leveraged nakon. Informacije u ovom playbook bit će vam je korisnik odabrao obavljanju sljedeće ciljeve:
-   Kratko vrijeme vrijednost strojnog učenja temelji rješenja
-   Mogućnost da biste proširili probnog koristite slučaj drugim slučajevima korištenje ili šira doseg koji se temelji na svoje poslovne potrebe
-   Brzo dobiti znanja paket obavještavanje Cortana proizvoda

Pomoću ove ciljeve na umu ovu playbook trebao pri izlaganja tvrtke i tehničke znanja koje će vam pomoći u postizanje te ciljeve.

### <a name="power-load-and-demand-forecasting"></a>Učitavanje dodatka Power i predviđanje zahtjev
Unutar sektor energiju može biti mnogo načina u koje zahtjev predviđanje olakšavaju rješavanje problema ključnih tvrtke. Zapravo, predviđanje zahtjev možete smatrati foundation za mnogim slučajevima koristi za temeljni se grana. Općenito govoreći, ne možemo razmislite o dvije vrste predviđanja energiju zahtjev: kratki termina i dugoročnu. Svaki od njih mogu poslužiti drugu namjenu i korištenje drukčiji pristup. Glavna razlika između dviju je predviđanja opseg, što znači vremenskog raspona u budućnosti za koje bi smo predviđanja.

#### <a name="short-term-load-forecasting"></a>Kratki termina učitavanja predviđanja
U kontekstu energiju zahtjev, kratki termina učitavanje predviđanje (STLF) se definira kao zbroja opterećenje koje je predviđenu ubuduće najbliži na razne dijelove rešetke (ili rešetku cijela). U ovom kontekstu kratki termina definiran opseg vremena u rasponu od jednog sata 24 sata. U nekim slučajevima opseg 48 sati moguće je. Stoga STLF vrlo uobičajeno je u slučaj radu pomoću rešetke. Evo nekoliko primjera STLF utemeljenima na korištenje slučajeva:
-   Opskrbu i zahtjev za ujednačavanje
-   Power kotiranja podrška
-   Tržište unošenju (postavka power cijena)
-   Rešetka radu optimizaciju
-   [Odgovor na zahtjev](https://en.wikipedia.org/wiki/Demand_response)
-   Vršna zahtjev predviđanja
-   Upravljanje strani zahtjev
-   Uravnoteženje opterećenja i preopterećenja sprječavanje
-   Dugo termina učitavanja predviđanja
-   Otkrivanje kvara i značajkom
-   Vršna curtailment/uravnoteživanju 

STLF model uglavnom temelje se na najbliži prošlosti (posljednji dan ili tjedan) potrošnje podataka i korištenje predviđenu temperatura kao Važno predictor. Nabavljanje točne temperatura predviđanje sljedećih sata i prema gore da biste 24 sata postaje manja je test sada dana. Ove modelima manje osjetljivi su na sezonski uzoraka ili trendova Dugoročne potrošnje.

SLTF rješenja su vjerojatno da biste generirali veliku količinu predviđanje poziva (zahtjevima za uslugu) jer oni su koji se poziva na svaki sat temelj, a u nekim slučajevima čak i uz viša frekvencija. I vrlo uobičajeno je da biste vidjeli implantation gdje svaki pojedinačne substation ili pretvarača predstavlja se kao samostalni modela i stoga su čak i veće glasnoće zahtjeva za predviđanje.

#### <a name="long-term-load-forecasting"></a>Dugo termina učitavanja predviđanja
Cilj od dugo termina učitavanja predviđanje (LTLF) je predviđanja power zahtjev s opseg vremena u rasponu od jednog tjedna u više mjeseci (ili u nekim slučajevima za određeni broj godina). Ovaj raspon opseg uglavnom je primjenjivo za planiranje i ulaganja koristite slučajeva.

Dugoročne scenarijima za je važno je imati visoke kvalitete podataka koji obuhvaća raspon više godina (minimalne 3 godine). Ove modelima obično izdvojiti sezonalnost uzoraka iz ranijih podataka i poslužite se vanjski predicators kao što su kao vremenske prognoze i klimatske uzoraka.

Važno pojašnjavaju da na dulje predviđanja opseg, manje točne predviđanja možda je. Stoga je važno čime se dobiva neke intervala pouzdanosti uz stvarni predviđanje koje omogućuje ljudi da biste faktor moguće varijacija njihove planiranja postupak.

Budući da scenarij potrošnje za LTLF uglavnom planiranje, bismo očekivati koliko donjem predviđanje količine (to STLF). Ne možemo obično vidjeti te predviđanja ugrađen u vizualizaciju alate kao što su Excel ili PowerBI i mogu ručno pozvani korisnik.

### <a name="short-term-vs-long-term-prediction"></a>Predviđanje kratki termina nasuprot dugu termina
U sljedećoj su tablici uspoređuje STLF i LTLF u odnosu na najvažnije atributima:

|Atribut|Kratki termina učitavanja predviđanja|Dugo termina predviđanje učitavanja|
|---|---|---|
|Opseg predviđanje|Na 1 h do 48 sati|Od 1 do 6 mjeseci ili više njih|
|Preciznosti podataka|Svaki sat|Svaki sat ili svaki dan|
|Tipična Upotreba slučajeva|<ul><li>Zahtjev/ponude za ujednačavanje</li><li>Odaberite sat predviđanje</li><li>Odgovor na zahtjev</li></ul>|<ul><li>Dugo termina planiranja</li><li>Rešetka imovine planiranje</li><li>Planiranje resursa</li></ul>|
|Uobičajeni predictors|<ul><li>Dan i tjedan</li><li>Sat danu</li><li>Svaki sat temperatura</li></ul>|<ul><li>Mjesec u godini</li><li>Dan u mjesecu</li><li>Temperatura dugoročnu i klimatske</li></ul>|
|Raspon povijesne podataka|Dvije do tri godine vrijedne podataka|Pet do 10 godina vrijedne podataka|
|Uobičajeni točnosti|MAPE * od 5% ili manju|MAPE * od 25% ili manju|
|Učestalost predviđanja|Proizvodi svaki sat ili svaka 24 sata|Proizvodi kada mjesečno, tromjesečno i godišnje|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – srednje Average postotka pogreške

Kao što je vidljivo iz ove tablice, prilično važno je da biste razlikovali na kratki i dugoročnu predviđanje scenariji kao te predstavljaju različite poslovne potrebe, a može imati različite implementacije i potrošnje uzoraka.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Primjer koristi slučaj 1: eSmart sustavi – Optimizacija preopterećenja
Važnu ulogu [pametno rešetke](https://en.wikipedia.org/wiki/Smart_grid) se dinamički neprestano optimiziranje i prilagođavanje za promjenom uzoraka potrošnje. Možete potrošnju energije utječe kratkotrajni promjene koje uglavnom uzrokovanih prilagođavati razlikama temperatura (*npr.*, više potencija se koristi za zraka uvjet ili zagrijavanje). U isto vrijeme potrošnju energije je i utjecati Dugoročne trendova. To obuhvaća sezonalnost efekata, nacionalni praznike, Dugoročne growth potrošnje i čak i Ekonomske čimbenike kao što su korisnička indeks, ULJE cijena i GDP.

U ovom slučaju korištenja [eSmart](http://www.esmartsystems.com/) željeli implementacija rješenja u oblaku koji omogućuje procjenjivanje propensity programa preopterećenja situacija na sve navedene substation rešetke. Posebno eSmart željeli prepoznavanje substations koji su vjerojatno preopterećenja unutar sljedećih sata tako trenutne akcije ne može se otvorila izbjeći ili riješiti takve situacije.

Programa točni i brzo izvođenje predviđanje zahtijeva implementaciju tri predvidljivu modela:
-   Dugo model termina koji omogućuje predviđanje od potrošnju energije na svakom substation tijekom sljedećeg nekoliko tjedana ili mjeseci
-   Model kratki termina koji omogućuje predviđanje situacije preopterećenja na navedeni substation tijekom sljedećeg sat
-   Temperatura modela koji omogućuje predviđanje od buduće temperatura preko više scenariji

Cilj Dugoročne modela rangiranje na substations prema svojim propensity da biste preopterećenja (navedeni prijenosa kapaciteta power) tijekom sljedeći tjedan ili mjesec. Time se omogućuje stvaranje kratki popis substations koji će vam poslužiti kao ulaz za kratkotrajni predviđanje. Kao što je temperatura je važno predictor Dugoročne modela, je potrebno neprestano proizvesti više scenarij temperatura predviđanja i sažetak sadržaja kao ulaz u Dugoročne modela. Predviđanje kratki termina zatim pozvati za predviđanje koje substation će vjerojatno preopterećenja putem sljedećih sata.

Kratkotrajni i Dugoročne modeli uvode se pojedinačno po svakom substation. Stoga praktično izvršavanje te modela zahtijeva proširenom djelovanje. Da bi se dobio veći točnosti predviđanja u kratki termina, precizniji modela predano za svaki sat dana. Ove modeli se provode svaki sat i završi izvođenje sljedećih nekoliko minuta da biste omogućili dovoljno vremena za odgovaranje i poduzeti preventivnog po potrebi. Zbirku modela je ažuriran pomoću časopisa retraining najnovije podatke.

Moguće je pronaći dodatne informacije o ovom slučaju korištenja [ovdje](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Korištenje kriterija slučaja kvalifikacije – preduvjeti
Glavni jačinu obavještavanje Cortana se sposobnost Napredna implementacije i skaliranje strojnog učenja usmjereni na rješenja. Osmišljena je za podršku za tisućice predviđanja koja se izvršava istovremeno. Automatski možete mjerilo da bi odgovarao promjenom potrošnje uzorka. Stoga je rješenja u fokus na točnost i računalne performansi. Ako, na primjer, utility tvrtke je zanimati proizvodnje točne energiju zahtjev predviđanja sljedeći h, a za svaki sat dana. S druge strane, ne možemo zanima manje odgovaranje na pitanje Zašto se na zahtjev predviđene se jer se (modela sam će voditi brigu o koji).

Stoga je važno shvatili da sve koristiti slučajeva, a poslovne probleme može se učinkovito riješiti pomoću strojnog učenja.

Obavještavanje Cortana i strojnog učenja može biti vrlo učinkovitih u rješavanju problema s određenom tvrtke kada se zadovolje sljedeće kriterije:
-   Poslovne probleme u skladištu je **predvidljivu** u karaktera. Primjer slučaja s predvidljivu koristi je utility tvrtke koje želite omogućiti predviđanje power opterećenje navedeni substation tijekom sljedećih sata. S druge strane, analiza i rangiranje upravljačke programe za povijesne zahtjev bi **opisni** prirode i stoga manje primjenjivo.
-   Postoji Očisti **put od akcija** izvedena kada dostupna je za predviđanje. Ako, na primjer, procjenjivanje preopterećenja na na substation tijekom sljedećeg sat možete pokrenuti određene proaktivne akcija smanjivanje opterećenja koji je pridružen tom substation i potencijalno što je preopterećenja.
-   Slučaj koristi predstavlja **uobičajene vrste problema** takve riješiti te kada ga možete teren za rješavanje druge slične koristite slučajeva.
-   Klijent možete postaviti **Kvantitativni i Kvalitativni ciljeve** da bismo pokazali uspješno rješenje implementacije. Na primjer, dobro Kvantitativni cilj za predviđanje zahtjev energiju bio potreban točnost prag (*npr.*, do 5% pogreške je dopušteno) ili kada se procjenjivanje substation zatim preopterećenja preciznost (rata true positives) i opoziv (opseg true positives) mora biti gore navedene praga. Ove ciljeve mora biti izvedena iz klijenta poslovnim ciljevima.
-   Postoji Očisti **integraciju** s blokova tvrtke. Na primjer, forecast učitavanja substation možete integrirati u Upravljačko središte rešetke da biste omogućili preopterećenja sprječavanje aktivnosti.
-   Kupac ima jeste li spremni za korištenje **podataka kvalitetom potrebne** za podršku kutije korištenje (Pročitajte više u sljedećem odjeljku, **Kvalitete podataka**o ovom playbook).
-   Klijent embraces arhitektura oblaka usmjereni na podatke ili **oblaku strojnog učenja**, uključujući Azure ML i druge komponente Cortana obavještavanje paket.
-   Klijent će naznačiti da biste uspostavili **tijeku do završetka na kraj** tog funkcije isporuku podataka u oblak pridonositi, te je willing da biste **operationalize** rješenja.
-   Klijent je **odvojiti resursa** koji će biti aktivno aktivan tijekom početne implementacije testnih tako da se znanja i vlasništvo nad rješenje može prenijeti klijentu nakon uspješan dovršetak.
-   Resurs za klijenta mora biti **profesionalni stručne podataka**, dajući prednost pozvana podataka.

Diploma koristi slučaj na temelju gore navedene kriterije znatno možete poboljšati postocima uspjeha slučaja koristi i uspostaviti dobro beachhead implementacije slučajeva za buduću upotrebu.

### <a name="cloud-based-solutions"></a>Oblaku rješenja
Cortana obavještavanje paket na Azure je okruženje koji se nalazi u oblaku. Implementacija rješenja za napredne analize u okruženje oblaka sadrži znatno pogodnosti za tvrtke i istovremeno može značiti veliki promjena tvrtki koje se i dalje koristiti na lokaciji IT rješenja. Unutar sektor energiju postoji trend Očisti postupne migracije operacije u oblak. Ovaj trend ide pripremiti u ruci uz razvojno pametno rešetke kako je opisano iznad, u **Industrijskih trendova**. Kao što je ovaj playbook namijenjen je rješenje oblaku u domeni energiju, važno je da biste objasnili prednosti i ostala razmatranja implementacije rješenja u oblaku.

Možda najvećih prednosti rješenje oblaku je trošak. Rješenje čini koristiti kao oblak implementiran komponenti je određeno troškove ni troškove komponente troška prodane robe (trošak od robe prodane) pridružen. To znači da nema potrebe da biste ulažete u hardver, softver i održavanje IT i stoga je znatno smanjenje rizika tvrtke.

Druga prednost važno je struktura pay-as-you-go trošak oblaku rješenja. Oblaku poslužitelji za računalstvo ili prostora za pohranu možete uvesti i skalirana na temelju samo--vodeće. Time se povećava prednost učinkovitosti trošak rješenja u oblaku.

Na kraju, nema potrebe za rad u web-mjesto održavanja IT ili u okvir za razvoj infrastruktura za buduće kao što je sve to je dio nuditi oblaku. U toj mjeri Cortana obavještavanje paket sadrži najbolje predmete servise, a njegov cesta map stalno razvoj. Nove značajke, komponente i mogućnosti su neprestano uvedene i poslovanja.

Pronađite tvrtke koje se pokreće samo njegov prijelaza u oblaku, ne možemo su visoko preporučiti da bi postupna pristup implementacijom cesta karte migracije oblaka. Ne možemo smatrate da uslužni programi i tvrtke energiju domeni, slučajeve korištenja koji se spominju u ovom playbook predstavljaju izvrstan prilika za piloting predvidljivu analize rješenja u oblaku.

#### <a name="business-case-justification-considerations"></a>Razmatranja poslovne slučaja poravnanje
U mnogim slučajevima kupca možda će vas zanimati upućivanje tvrtke poravnanje za dani korištenje slučaja u kojima oblaku rješenja i strojnog učenja su važne komponente. Za razliku od lokalnog rješenja, u slučaju rješenja u oblaku je komponenta za određeno trošak minimalne i Većina elemenata trošak je pridružen stvarnu upotrebu. Kada je riječ o implementaciji sustava energiju predviđanje rješenje na Cortana obavještavanje paket, većem broju servisa možete integrirati s jednom uobičajeni trošak strukturu. Na primjer, baza podataka (*primjerice*, SQL Azure) može se koristiti za pohranu sirovim podacima i zatim za stvarni predviđanja Azure ML služi za hostiranje predviđanja services. U ovom primjeru strukture trošak može obuhvaćati i transakcijskih komponente prostor za pohranu.

S druge strane, jedan moraju imati dobro upoznavanje tvrtke vrijednosti operacijskog sustava zahtjev energiju predviđanje (kratko ili dugo termina). Zapravo, važno je da biste shvatili poslovne vrijednosti svake predviđanja operacije. Na primjer, točno predviđanje power učitavanja sljedeći 24 sata možete spriječiti overproduction ili možete spriječiti overloads na rešetki i to možete quantified pomoću financijske računanje po danu.

Osnovni formule za izračunavanje financijske prednost zahtjev predviđanja rješenje bila: ![osnovni formule za izračunavanje financijske prednost zahtjev predviđanja rješenja](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Budući da Cortana obavještavanje paket sadrži pay-as-you-go model za cijene, nema potrebe za povećavanja komponente fiksni trošak ovu formulu. Ova formula izračunava se na temelju mjesečno ili godišnje svakodnevno.

Trenutni Cortana obavještavanje paketa i Azure ML cijene tarife nalazi se [ovdje](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Postupak rješavanja razvoj
Ciklus razvoj programa zahtjev energiju predviđanje rješenja obično uključuje 4 faze koje možemo pružiti pomoću tehnologije oblaku i servisa unutar obavještavanje paket Cortana.

To je što je prikazano na sljedećem su dijagramu:

![Slika rešetke ciklusa](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Sljedeći odlomak opisuje postupak 4 koraka:

1.  **Prikupljanje podataka** – rješenjem naprednom analitikom temelji ovisi o podacima (pogledajte **Objašnjenje podataka**). Konkretno, kada je riječ o predvidljivu analitikom i predviđanje, ne možemo oslanjate tijeku, dinamičnim protok podataka. Slučaju energiju zahtjev predviđanja, te podatke možete izvorni termini izravno iz pametno metar ili već zbrojiti na bazu podataka lokalno. Ne možemo i oslanjate drugim vanjskim izvorima podataka kao što su vremenske prognoze i temperatura. U ovom tijeku protok podataka mora biti orchestrated, zakazano, a pohranjene. [Tvorničke Azure podataka](http://azure.microsoft.com/services/data-factory/) (ADF) je naš glavnom workhorse za izvršavanje ovaj zadatak.
2.  **Modeliranje** – za točni i pouzdan energiju predviđanja, jedan morate razviti (vlaku) i održavanje sjajno modela čini korištenje povijesne podatke, a izdvaja smislen i predvidljivu uzoraka podataka. Područje od strojnog učenja (ML) rast hitro naprednije algoritmima ažurirate razvijaju. Azure ML Studio nudi odlične korisničkog sučelja koja omogućuje korištenje najčešće napredne algoritama ML unutar dovršeno radnog tijeka. Taj tijek rada je podržali intuitivno dijagrama toka, a obuhvaća Priprema podataka, izdvajanje značajki, Modeliranje i procjenu modela. Korisnik možete uvesti u stotine različitim modelima uvrštene u ovom okruženju. Do kraja ove faze podataka pozvana će imati radni model koji je potpuno provjeriti u odnosu i jeste li spremni za implementaciju.

    Na sljedećem su dijagramu je ilustracija tijeka rada za uobičajene:

    ![Modeliranje tijeka rada](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Uvođenje** – s modelom rad u skladištu, sljedeći je korak implementacije. Ovdje se model pretvara se u web-servisa koji će prikazati RESTful API koji mogu se istovremeno pozvati putem Interneta iz različitih potrošnje klijenata. Azure ML omogućuje jednostavan način implementacije modela izravno iz Studio ML Azure jednim klikom na gumb. Proces cijelu implementacije događa Pokaži napredne postavke. Rješenje može automatski promijeniti omjer da bi odgovarao potrebna potrošnju.

4.  **Potrošnje** – u ovoj fazi zapravo dajemo pomoću predviđanja modela, čime se dobiva predviđanja. Potrošnju možete utemeljenima iz aplikacije korisničkih (*npr.*, nadzorne ploče) ili izravno iz radu sa servisom sustava kao što su zahtjev/ponude ujednačavanje sustava ili rješenja za optimizaciju rešetke. Više slučajeva koristi se mogu utemeljenima iz jedne modela.

## <a name="data-understanding"></a>Razumijevanje podataka
Nakon prekrivajući (pogledajte **Objašnjenje tvrtke**) razmatranja tvrtke od programa zahtjev energiju predviđanje rješenja, ne možemo spremni o dio podataka. Rješenjem predvidljivu analize ovisi o pouzdanog podataka. Za štednju zahtjev predviđanje, ne možemo oslanjate povijesne potrošnje podatke s različitih razina preciznosti. Povijesne podataka koristi se kao na sirovine. To će undergo oprezni analizu u kojem pozvana podataka prepoznavanje predictors (naziva značajke) koja se može smjestiti u model koji naposljetku generiranje potrebna predviđanja.

U ostatku u ovom se odjeljku smo opisane različite korake i zahtjevi za razumijevanje podataka i kako ponijeti upotrebljivosti obrazac.

### <a name="the-model-development-cycle"></a>Ciklus razvoja modela
Proizvodnje dobar predviđanje modela zahtijeva neke oprezni Priprema i planiranje. Raščlaniti postupka Modeliranje u više korake i usredotočite se na jedan korak po nije znatno poboljšati rezultat cijeli postupak.

Sljedeći dijagram prikazuje kako Modeliranje postupak ne može se svesti više korake:

![Model razvoj ciklusa](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Kao što je vidljivo ciklusu sastoji se od šest koraka:
-   Problem formulation
-   Ingestion podataka i Istraživanje podataka
-   Priprema podataka i značajka inženjering
-   Modeliranje
-   Procjena modela
-   Razvoj

U ostatku u ovom se odjeljku smo opisane su pojedinačne korake i stavke koje treba uzeti u obzir prilikom svakog koraka.

### <a name="problem-formulation"></a>Problem Formulation
Ne možemo možete razmislite o formulation problem najvažnija koraku nešto treba poduzeti prije implementacije rješenjem predvidljivu analize. Ovdje želite transformirati poslovne probleme i decompose na određene elemente koji se može riješiti pomoću podataka i Modeliranje tehnike. Nije dobro formulate problem kao skup pitanja željeli bismo odgovor. Evo nekih moguće pitanja na koja možda je primjenjivo unutar opsega energiju zahtjev predviđanje:
-   Što je očekivani opterećenje pojedinačne substation u sljedeći sat ili dan?
-   Na koje se doba dana će Moje rešetke sučelje Vršna zahtjev?
-   Kako vjerojatno je moj rešetke nastavka rješenja za učitavanje očekivani Vršna?
-   Koliko power treba stanice power generiranje tijekom svaki sat dana?

Formulating ta pitanja nam omogućuje fokusiranje na početak s točnim podacima i implementaciju rješenje koje se potpuno poravnat s problemom tvrtke konkretnom. Osim toga, ne možemo možete postaviti neke ključne metrike koji omogućuju nam da biste procijenili performansama model. Na primjer, koliko predviđanja treba i što je raspon pogrešku da i dalje će biti prihvatljiva po tvrtke?

### <a name="data-sources"></a>Izvori podataka
Moderna pametno rešetke prikuplja podatke iz različitih dijelova i komponente rešetke. Ovi podaci predstavljaju razne aspekte postupak i upotreba power rešetke. Unutar opseg zahtjev energiju predviđanja, ne možemo su ograničavanje rasprave na izvori podataka koje odražavaju potrošnje stvarni zahtjev. Jedan izvor važne potrošnju energije su pametne metar. Uslužni programi oko globusa hitro implementirate pametno metar za osobe koje korisnici. Pametno metar snimiti potrošnju energije stvarni i neprestano preusmjeravanje ove podatke natrag za tvrtke. Podaci koji se prikupljaju i šalje natrag na fixed intervalu od svakih 5 minuta do 1 sat. Naprednije pametno metar se mogu programirati daljinski za upravljanje i saldo stvarne potrošnje unutar u kućanstvu. Pametno metar relativno pouzdanog i podataka sadrži vremensku oznaku. Koji olakšava važne ingredient za zahtjev predviđanja. Metar podataka možete zbrojiti (za zbrojenih) na različitim razinama unutar topologije rešetka: pretvarača substation, regija, *itd*. Ne možemo možete odabrati razina potrebna skupljanja da biste sastavili predviđanja model za njega. Na primjer, ako tvrtke utility htjeli predviđanjem budućeg opterećenja za svaku od njezinih substations rešetke pa sve metar podataka možete se pridružuje za svaki pojedinačne substation i koristiti kao ulaz predviđanja modela. Nazivamo pametno metar internom izvoru podataka.

Predviđanje zahtjev pouzdanog energiju će je i za druge vanjske izvore podataka. Jedan važan faktor koje utječu na potrošnju energije je na vremensku prognozu ili preciznije temperature. Proteklih podataka prikazuje istaknuti korelacije između izvan temperature i potrošnju energije. Tijekom dana tipkovni ljetnih provjerite koje korisnici koristiti svoje klima i tijekom zima uključeno grijanje sustave. Pouzdanog izvora povijesne temperature lokaciji rešetke stoga je ključ. Osim toga, ne možemo i oslanjate točne prognozu temperature kao predictor od potrošnju energije.

Druge vanjske izvore podataka mogu pomoći za izgradnju energiju zahtjev predviđanja modele. Uključuju dugoročnu klimatske promjene, najekonomičniji indeksa (*npr.*, GDP) i drugim korisnicima. U ovom dokumentu ne možemo neće sadržavati te drugih izvora podataka.

### <a name="data-structure"></a>Struktura podataka
Nakon identificiranje izvora potrebnih podataka, željeli bismo da biste bili sigurni da sirovim podacima prikupljene sadrži značajke točne podatke. Da biste sastavili model predviđanja pouzdanog zahtjev, ne možemo potrebni da biste bili sigurni da podatke prikupljene sadrži podatkovnih elemenata koje omogućuju predviđanje buduće zahtjev. Ovo su neki osnovni sistemski preduvjeti tiču struktura podataka (sheme) sirovim podacima.

Sirovim podacima sastoji se od redaka i stupaca. Sve mjere predstavlja se kao jedan redak podataka. Svaki redak podataka sadrži više stupaca (naziva značajke ili polja).

1.  **Vremenska oznaka** – polje s vremenskom oznakom predstavlja stvarni vrijeme kada je naveden mjerne jedinice. Je moraju biti usklađene s jednim od uobičajenih oblika datuma i vremena. Datum i vrijeme dijelove potrebno obuhvatiti. U većini slučajeva, nema potrebe za trajanje će zabilježen prije pomaka druge razine preciznosti. Važno je da odredite željenu vremensku zonu u kojoj je naveden podatke.
2.  **Metar ID** – to polje označava mjerača ili uređaj mjera. Varijabla categorical i mogu biti kombinacija znamenke i znakova.
3.  **Vrijednost potrošnje** – to je stvarne potrošnje navedeni datum/vrijeme. Možete potrošnju mjeriti u kWh (kilowatt-hour) ili nekoj drugoj Preferirani jedinice. Nije važno Imajte na umu da mjernu jedinicu mora ostati dosljedan preko svih mjere u podacima. U nekim slučajevima potrošnje možete navesti veće od 3 power faze. U tom slučaju bi moramo prikupiti sve faze neovisno potrošnje.
4.  **Temperatura** – temperatura prikupljaju se obično neovisno izvora. Međutim, mora biti kompatibilan s podacima potrošnje. On mora uključivati vremenskog pečata prethodno opisan omogućit će da bi se sinkronizirati s podacima stvarne potrošnje. Vrijednost temperatura bude navedena u stupnjevima c ili Fahrenheitovu, ali trebaju biti dosljedan preko svih mjere.
5.  **Mjesto –** Polje mjesto pridruženo obično na mjestu gdje je prikupljeni podaci o temperaturi. Možete biti predstavljeni kao broj poštanski broj ili u obliku zemljopisnu širinu i dužinu (lat na dugi).

Sljedeća tablica sadrži primjere dobar potrošnje i temperatura podataka oblikovanje:

|**Datum**|**Vrijeme**|**ID metar**|**Faza 1**|**Faza 2**|**Faza 3**|
|--------|--------|------------|-----------|-----------|-----------|
|7/1/2015.|10:00:00|ABC1234     |7.0        |2.1        |5,3        |
|7/1/2015.|10:00:01|ABC1234     |7.1        |2.2        |4,3        |
|7/1/2015.|10:00:02|ABC1234     |6.0        |2.1        |4.0        |

|**Datum**|**Vrijeme**|**Mjesto**|**Temperatura**|
|--------|--------|-------------|---------------|
|7/1/2015.|10:00:00|11242        |24.4           |
|7/1/2015.|10:00:01|11242        |24.4           |
|7/1/2015.|10:00:02|11242        |24.5           |

Kao što je vidljivo iznad, u ovom se primjeru sadrži 3 različite vrijednosti potrošnje povezan s 3 power faze. Osim toga, imajte na umu da polja datuma i vremena razdjelnik, no možete i kombinirati u jedan stupac. U ovom slučaju stupcu mjesto predstavlja u obliku poštanski broj 5 znamenke i temperatura u obliku Celzijev stupanj.

### <a name="data-format"></a>Oblikovanje podataka
Cortana obavještavanje paket podržava najčešće oblike podataka kao što je CSV TSV, JSON, *itd*. Nije važno da oblik podataka ostaje dosljedan za cijelu životnog ciklusa projekta.

### <a name="data-ingestion"></a>Ingestion podataka
Budući da energiju zahtjev predviđanje je stalno i često predviđene, ne možemo osigurati da su sirovim podacima slijedi pomoću procesa ingestion puna i pouzdane podataka. Postupak ingestion morate jamči da neobrađenog podaci dostupni su za proces predviđanja u trenutku potrebna. Koji podrazumijeva ingestion učestalost podataka mora biti veći od predviđanja učestalost.

Na primjer: Ako naš zahtjev predviđanje rješenja želite generirati novi predviđanja pri 8:00 Prijepodne po danu, zatim moramo da biste bili sigurni da sve podatke prikupljene tijekom zadnja 24 sata sadrži nisu potpuno ingested prije pomaka tog trenutka i ima čak i uključiti zadnjem satu podataka.

Da biste to postigli, Cortana obavještavanje paket nudi različite načine za podršku procesima ingestion pouzdanog podataka. To će biti dodatno spominju u odjeljku **implementacije** ovog dokumenta.

### <a name="data-quality"></a>Kvalitete podataka
Izvor neobrađenog podataka koji je potreban za izvođenje pouzdanog i točne zahtjev predviđanje mora zadovoljiti neke osnovne podatke kvalitete kriterija. Iako napredne statističkim postupcima može se koristiti za vaše za neki problem kvalitete mogućih podataka, i dalje moramo da biste bili sigurni da su smo presjeka neke prag kvalitete baza podataka kada ingesting nove podatke. Evo nekoliko napomene vezane uz kvalitete sirovim podacima:
-   **Nedostaje vrijednost** – odnosi se na situaciji kada određenu veličinu je prikupljeni. Osnovni potrebna ovdje je da nedostaje vrijednost rate ne smije biti veći od 10% za sve određenog vremenskog razdoblja. U slučaju da jednu vrijednost nema ga mora biti naznačen pomoću unaprijed definiranih vrijednosti (na primjer: "9999") i ne "0", koja može biti valjana mjera.
-   **Mjeru točnosti** – stvarna vrijednost potrošnje ili temperatura trebali biste točno naveden. Netočan mjere će stvoriti netočan predviđanja. Najčešće pogreške mjera mora biti manja od 1% odnosu vrijednost true.
-   **Vrijeme mjerne** – potrebna je prikupljeni stvarni vremenske oznake podataka će proizlaze za više od 10 sekundi true vremenu stvarni mjera.
-   **Sinkronizacija** – kada se koriste više izvora podataka (*npr.*, potrošnje i temperatura) ne možemo osigurati da postoji su problemi bez sinkronizacija vremena između njih. To znači da vrijeme razlika između prikupljenih vremensku oznaku iz bilo kojeg izvora dviju nezavisnih podataka smije više od 10 sekundi.
-   **Latencija** – kako je opisano iznad, u **Ingestion podataka**, ne možemo ovise o pouzdanog podataka tijek i ingestion postupak. Da biste odredili koji ne možemo osigurati smo kontrolirati Latencija podataka. To je navedena kao vrijeme razlikuju stvarni mjera snimljena vremena i vrijeme na kojoj je učitana u paket obavještavanje Cortana prostora za pohranu i spreman je za korištenje. Za učitavanje kratki termina predviđanje Ukupna Latencija ne smije biti veća od 30 minuta. Za učitavanje dugoročnu predviđanje Ukupna Latencija ne smije biti veća od 1 dana.

### <a name="data-preparation-and-feature-engineering"></a>Priprema podataka i značajka inženjering
Kada sirovim podacima je ingested (pogledajte **Ingestion podataka**), a sigurno spremljena, spremna je za obradu. Faza Priprema podataka s poduzimanja sirovim podacima i pretvaranje (pretvorba, reshaping) u obrazac za Modeliranje faza. Koji mogu sadržavati jednostavno operacija kao što su korištenje stupcu sirovim podacima, kao što je s njihovu stvarnu vrijednost mjerni, standardizirani vrijednosti, složenije operacija kao što su [vrijeme kašnjenja](https://en.wikipedia.org/wiki/Lag_operator)i drugim korisnicima. Stupce novostvorenu podataka se nazivaju i značajkama i procesa stvaranja te se naziva inženjering značajke. Do kraja ovog postupka smo novi skup podataka koji sadrži sirovim podacima dobivenima i može se koristiti za Modeliranje. Osim toga, faza Priprema podataka treba voditi brigu o vrijednosti koje nedostaju (pogledajte **Kvalitete podataka**), a vaše za njih. U nekim slučajevima bi i moramo normalizacija podataka da biste bili sigurni prikazane su sve vrijednosti u isti skalu.

U ovom odjeljku smo neke od uobičajenih značajki podatke koji su uključeni u na energiju popis zahtjev predviđanja modela.

**Vrijeme utemeljenima na značajke:** Te značajke potječu iz podataka datuma i vremenske oznake. Te su dobivenih i pretvoriti u categorical značajke kao što su:
-   Vrijeme dana – to je sat dan koji vodi vrijednosti od 0 do 23
-   Dan u tjednu – to predstavlja dan u tjednu i uzima vrijednosti od 1 (nedjelja) do 7 (subota)
-   Dan u mjesecu – to predstavlja Stvarni datum, a može potrajati vrijednosti od 1 do 31
-   Mjesec godine – to predstavlja mjesec i uzima vrijednosti od 1 (siječanj) do 12 (prosinac)
-   Vikenda – to je značajka binarnu vrijednost koja vas vodi vrijednosti 0 za dane u tjednu ili 1 za vikenda
-   Blagdanska – to je značajka binarnu vrijednost koja vas vodi vrijednosti 0 za obične dan ili 1 za praznika
-   Fourierova – uvjete Fourierova su izrazi debljine koji potječu iz vremenska oznaka i koriste se za snimanje sezonalnost (ciklusa) u podacima. Budući da ne možemo može imati više godišnja doba u oglednim podacima ipak Trebamo više Fourierova termina. Na primjer, zahtjev vrijednosti možda godišnje, tjednih i dnevnih godišnja doba/ciklusa koji će rezultirati 3 Fourierova uvjeta.

**Neovisno mjera značajke:** Neovisno značajke obuhvaćaju sve elemente podataka želimo da biste upotrijebili predictors u našem modelu. U nastavku ćemo izuzimanje zavisne značajku što smo potrebni za predviđanje.
-   Značajka kašnjenja – to su vrijeme pomaknut vrijednosti stvarni zahtjev. Ako, na primjer, značajke kašnjenja 1 će držite vrijednost zahtjev prethodne hour (Ako svaki sat podataka) odnosu trenutnu vremensku oznaku. Isto tako, ne možemo možete dodati kašnjenja 2, kašnjenja 3 *itd*. Stvarni kombinaciju kašnjenja značajke koje se koriste ovise o tijekom faze Modeliranje vrednovanju modela rezultate.
-   Dugo termina trendova – Ova značajka predstavlja linearni growth u zahtjev između godina.

**Zavisne značajka:** Zavisne značajka je stupac podataka koje želimo našem modelu za predviđanje. S [supervised strojnog učenja](https://en.wikipedia.org/wiki/Supervised_learning), moramo najprije uvježbati korištenjem značajki za zavisne model (koji se naziva se i kao natpise). Time se omogućuje model da biste saznali uzorke u podataka vezanih uz značajku zavisne. U zahtjev za štednju predviđanje obično želimo predviđanje stvarni zahtjev i stoga koristit ćemo ga kao značajku zavisne.

**Rukovanje vrijednosti koje nedostaju:** Tijekom faze Priprema podataka, ne možemo morate odrediti najbolji Strategije za rukovanje vrijednosti koje nedostaju. To se uglavnom radi razne statističke [podatke imputation metode](https://en.wikipedia.org/wiki/Imputation_(statistics)). U slučaju energiju zahtjev predviđanja, ne možemo obično impute vrijednosti koje nedostaju pomoću pomični prosjek iz prethodne točke podataka dostupna.

**Normalizacija podataka:** Normalizacija podataka je neku drugu vrstu transformacije koji se koristi za uvoz numeričke podatke kao što su zahtjev predviđanja u skalu sličnim. To obično se poboljšava točnost modela i preciznosti. Ne možemo bi inače to dijeljenjem stvarna vrijednost u rasponu podataka.
To će promijeniti veličinu izvornu vrijednost C1, obično između -1 i 1.

## <a name="modeling"></a>Modeliranje
Faza Modeliranje je gdje pretvorbe podataka u model odvija. Temeljni taj proces su napredne algoritmima pomoću kojih skeniranje na povijesne podatke (tečaj), izdvojiti uzoraka i sastavljanje modela. Model možete kasnije za predviđanje na nove podatke koji ne koriste za izradu modela.

Kada imamo radni pouzdanog model smo pa pomoću njega možete rezultatu nove podatke koje strukturirano je da biste dodali potrebne značajke (X). Postupak bilježenja rezultata će postati koristite postojanog modela (objekt iz faza obuka) i predviđanje cilj varijabla koja je označena Ŷ.

### <a name="demand-forecasting-modeling-techniques"></a>Zahtjev predviđanje Modeliranje tehnike
U slučaju zahtjev predviđanje dajemo koristite povijesne podataka koji je poredane po vremenu. Ne možemo obično pogledajte podatke koji sadrže vremensku dimenziju kao [vremenski niz](https://en.wikipedia.org/wiki/Time_series). Cilj Modeliranje niz vrijeme je da biste pronašli vrijeme povezane trendova, sezonalnost; automatsko korelacije (korelacije s vremenom,) i one u model formulate.

Nedavne godine napredne algoritama razvijena su kako bi odgovarao predviđanja niza vremena i poboljšanje točnosti predviđanja. Ukratko ćemo razmotriti nekoliko ih ovdje.

> [AZURE.NOTE] U ovom se odjeljku nije namijenjen koja će se koristiti kao strojnog učenja i predviđanja pregled, ali umjesto kao kratki anketu Modeliranje tehnike koje se najčešće koriste za predviđanje zahtjev. Dodatne informacije i obrazovne materijala o predviđanja niza vremena preporučujemo online adresara [predviđanja: načela pa uvježbati](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (pomični prosjek)**](https://www.otexts.org/fpp/6/2)
Pomični prosjek jedan je od prvog analytical tehnika kojima se koristila za predviđanje niza vremena te je i dalje jedna od najčešće korištenih tehnike na današnji dan. Preporučuje se i temelj za naprednije predviđanje tehnike. S pomični prosjek smo su predviđanje sljedeće točke podataka po računanjem prosjeka putem K najnovije točke, gdje je K označava redoslijeda pomični prosjek.

Pomični prosjek tehnika je učinak izglađivanje predviđanje i stoga možda obradu i velike volatility u podacima.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**Grafičke oznake (eksponencijalno izglađivanje)**](https://www.otexts.org/fpp/7/5)
Eksponencijalno izglađivanje (ETS) je obitelj različite metode koje koriste ponderirani prosjek nedavne točke podataka da bi se predviđanje sljedeće točke podataka. Ideja je veći debljine dodijelite novija vrijednosti i postupno Smanji ovaj težina starije mjerni vrijednosti. Postoji nekoliko različitih načina s ovom obitelji, neke od njih obuhvatiti rukovanje sezonalnost podataka kao što su [Holt Winters Sezonski način](https://www.otexts.org/fpp/7/5).

Neke od ovih metoda i faktor u sezonalnost podatke.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatski regresije integrirane mogućnosti premještanja prosjek)**](https://www.otexts.org/fpp/8)
Automatsko regresije integrirani premještanje Average (ARIMA) je drugi obitelj od načina koji se obično koristi za vremenski niz predviđanja. Gotovo kombinira metode automatski regresije s pomični prosjek. Automatsko regresije metode koriste regresije modelima prihvaćanjem prethodne vrijednosti niza vremena da bi se izračunati sljedeći točku datuma. Metode ARIMA primijeniti i razlikovanje načina koji sadrže izračun razlike između podatkovnih točaka i pomoću onih umjesto izvornu mjerni vrijednost. Na kraju, ARIMA koristiti čini pokretnog prosjeka tehnika koji se spominju iznad. Kombinacija sve od ovih metoda na različite načine je što konstrukata obitelji ARIMA metode.

Grafičke oznake i ARIMA široko koriste danas za predviđanje zahtjev energiju i mnogih drugih problema predviđanja. U mnogim slučajevima se kombinirati zajedno za isporuku vrlo točne rezultate.

**Višestruki Općenito regresije** Modeli regresije može biti najvažnije pristup Modeliranje unutar domene strojnog učenja i statističkih podataka. U kontekstu vremenski niz regresije koristimo za predviđanje budućih vrijednosti (*npr.*, od zahtjev). U regresije ne možemo poduzeti linearni kombinacije u predictors i Saznajte debljine (naziva koeficijenti) od tih predictors tijekom postupka obuka. Cilj je čime se dobiva regresijskog pravca koji će predviđanja naš predviđene vrijednosti. Metode regresije su prikladne nakon varijabla ciljne numeričke i stoga i prilagođava predviđanja niza vremena. Postoji brojne načine regresije uključujući vrlo jednostavne regresije modela kao što su [Linearne regresije](https://en.wikipedia.org/wiki/Linear_regression) i naprednije one kao što su odlukama, [Slučajno šuma](https://en.wikipedia.org/wiki/Random_forest), [Neural mreža](https://en.wikipedia.org/wiki/Artificial_neural_network)i Boosted odlukama.

Izgradnje energiju zahtjev predviđanje kao problem regresije daje nam mnoštvo u našem podataka značajke koje se mogu kombinirati odabirom stvarni zahtjev vrijeme niz podataka i vanjske čimbenicima kao što su temperature. Dodatne informacije o značajkama za odabrani se spominju u značajku inženjerska dio ovog playbook (potražite u članku **Priprema podataka i inženjerskih značajku**).

Iz naših doživljaj implementaciji i implementaciju energiju zahtjev predviđanja pilot ste naišli napredne regresije modela koje su dostupne u Azure ML obično čime se dobiva najbolje rezultate, a dajemo koristite od njih.

## <a name="model-evaluation"></a>Procjena modela
Procjena model sadrži ključnih uloga unutar **Ciklusa razvoja modela**. U ovom koraku smo pogleda Provjera valjanosti modela i njegovih performansi realni vijek podacima. Tijekom korak Modeliranje koristimo dio podataka dostupnih za osposobljavanje modela. Tijekom faze procjenu smo potrebno ostatak podataka da biste testirali modela. Gotovo znači Umetanje modela nove podatke koje je omogućeno restrukturiran i sadrži iste značajke kao obuka skupu podataka. Međutim, tijekom provjere valjanosti koristimo model predviđanje varijabla cilj umjesto pružaju varijabla dostupna cilj. Ne možemo često potražite taj proces kao model bilježenje rezultata. Ne možemo bi pomoću true ciljnim i usporedili ih s onima predviđene. Cilj ovdje je mjerenje i minimiziranja pogreška predviđanje, što znači da se razlikuju u predviđanja i vrijednost true. Quantifying mjerne jedinice pogreške je ključ jer želimo da biste prilagodili modela i provjerili koristi li se pogreške zapravo silazno. Prilagodbom modela mogu se izvršiti izmjenom modela parametre koje upravljati postupkom učenje ili dodavanjem ili uklanjanjem značajke podataka (naziva se [Parametri Počisti](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Gotovo znači da ne možemo možda morate ponavljati između inženjerska značajke modeliranja, i model procjenu faze više puta dok ne možemo da biste saželi pogrešku potrebnu razinu.

Važno je da isticanje da pogreške predviđanje nikad neće biti nula kao Nikad ne postoji modelu koji možete savršeno predviđanje svaki rezultat. No postoji određenih veličina pogreške koje je moguće je postaviti tako da tvrtke. Tijekom provjere valjanosti željeli bismo provjerite je li pogrešku predviđanje modela na razini i bolje nego razinu odstupanje tvrtke. Stoga je važno da biste postavili razinu tolerable pogreške na početku ciklusa tijekom faze **Problem Formulation** .

### <a name="typical-evaluation-techniques"></a>Procjena standardne tehnike
Postoje razni načini na koje predviđanje pogreške mogu mjeri i quantified. U ovom odjeljku ne možemo će fokus rasprave na procjenu tehnike odgovarajući vremenski niz i u specifičnih za štednju zahtjev predviđanja.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE označava znače apsolutne pogreške postotak. Pomoću MAPE ne možemo su računalstvo razlika između svakog predviđenu točke i stvarna vrijednost tog trenutka. Ne možemo zatim brojčano pogreške po točki izračunom proporcije razlika odnosu stvarna vrijednost. U posljednjem koraku smo Prosječna te vrijednosti. Matematičke formula koja se koristi za MAPE je sljedeće:

![Formula za MAPE](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*gdje<sub>t</sub> je stvarna vrijednost, F<sub>t</sub> je predviđene vrijednosti, a n je predviđanja opseg.*

## <a name="deployment"></a>Uvođenje
Nakon što smo obećajemo faza Modeliranje i provjeriti performansama model smo spremna za slanje u faza implementacije. U ovom kontekstu implementacije znači Omogućivanje kupca trošiti model tako da pokrenete stvarni predviđanja na njemu at velike mjerilo. Koncept implementacije je ključ u Azure ML jer je naš glavni cilj neprestano pozvati predviđanja umjesto samo stjecanje na uvid iz podataka. Faza implementacije je dio gdje ćemo omogućiti modela se utrošiti pri velike mjerilo.

U kontekstu energiju zahtjev predviđanje naših ciljem je pozvati neprekinuti i periodičku predviđanja štede Osvježi podaci dostupni za model i predviđene podatke slanja ponovno dosta klijenta.

### <a name="web-services-deployment"></a>Implementacija web Services
Glavni deployable sastavnih blokova u Azure ML je web-servisa. To je najučinkovitiji način da biste omogućili potrošnje predvidljivu modela u oblaku. Web-servisa encapsulates modela i prelama prema gore s na [RESTful](http://www.restapitutorial.com/) API-JA (Application Programming Interface). U API može se koristiti kao dio klijent kod kao što je prikazano u dijagramu u nastavku.

![Ne možemo servisa implementacije i potrošnje](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Kao što je vidljivo, web-usluge je implementiran u oblaku Cortana obavještavanje paket, a možete pozvati putem njezin krajnju točku prikazanog REST API-JA. Različite vrste klijenti preko različitih domena možete istodobno pozivanje usluge putem Web API. Web-servisa također mogu mijenjati veličinu za podršku tisuće Istodobni pozive.

### <a name="a-typical-solution-architecture"></a>Arhitekturu uobičajena rješenja
Pri implementaciji sustava zahtjev energiju predviđanje rješenja, ne možemo zanima implementacija end da biste end rješenje koja vodi izvan web-servisa za predviđanje i olakšava tijek čitavog sadržaja. Trenutku smo aktiviranja nove predviđanja bi moramo da biste bili sigurni da se model ulaže sa značajkama ažurirane podatke. Koji je podrazumijeva da upravo prikupljenih sirovim podacima je stalno ingested, obrađeni ili pretvoreni u potrebnu značajku postavljena na koji je ugrađen u model. U isto vrijeme željeli bismo omogućiti predviđene podataka za kraj troše klijente. Na primjer podataka ciklusa tijek (ili podataka kanal) prikazanom u dijagramu u nastavku:

![Zahtjev za štednju predviđanja toka završetka na kraj podataka](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Ovo su koraci koji se odvijaju kao dio energiju ciklusa predviđanja zahtjev:
1.  Milijune distribuiranih podataka metar neprestano generiraju podataka potrošnju energije u stvarnom vremenu.
2.  Se ti podaci koji se prikupljaju i prenijeti u spremištu oblaka (*npr.*, blobova platforme Azure).
3.  Prije nego što se obrađuju sirovim podacima se pridružuje substation ili regionalne razine, kako je definirano tvrtke.
4.  Obrada značajka (potražite u članku **Priprema podataka i značajke za obradu**) pa odvija i daje podatke koji su potrebni za model obuka ili bilježenje rezultata – skupu podataka značajke pohranjuju u bazi podataka (*primjerice*, SQL Azure).
5.  Servis za ponovno obuke se poziva da biste ponovno uvježbati predviđanja model – taj ažuriranu verziju modela je ista i tako da ga mogu poslužiti bilježenja rezultata web-servisa.
6.  Raspored koji odgovara potrebna predviđanja učestalost se poziva bilježenja rezultata web-servisa.
7.  Predviđene podaci se pohranjuju u bazi podataka koji mogu pristupiti klijent potrošnje završetka.
8.  Klijent potrošnje dohvaća prognoze, primjenjuje natrag u rešetki i troši skladu kutije potrebna koristi.

Nije važno Imajte na umu da ovu cijelu ciklusa potpuno automatski i pokreće se u rasporedu. Cijeli djelovanje ovaj ciklusa podataka mogu se izvršiti pomoću alata kao što su [Tvorničke Azure podataka](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Arhitektura END da biste završetka implementacije
Da bi se gotovo implementirate programa energiju zahtjev predviđanja na Cortana obavještavanje, moramo da biste bili sigurni da su potrebne komponente uspostaviti i ispravno konfigurirano.

Na sljedećem su dijagramu ilustrira uobičajene Cortana obavještavanje temelji arhitekturu koji implementira i orchestrates ciklusa protok podataka koji je opisan iznad:

![Arhitektura END da biste završetka implementacije](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Dodatne informacije o pojedinim komponente i cijeli arhitektura potražite predložak energiju rješenja.
