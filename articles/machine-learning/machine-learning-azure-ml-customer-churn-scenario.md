<properties
    pageTitle="Analiza kupca Churn pomoću strojnog učenja | Microsoft Azure"
    description="Studije slučaja od razvoj integrirani model za analizu i bilježenje rezultata churn klijenta"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Pomoću Azure strojnog učenja analiza Churn klijenta

##<a name="overview"></a>Pregled
U ovoj se temi predstavlja referenca implementacije projekta za analizu churn kupca koja je ugrađena pomoću Azure strojnog učenja Studio. Ga govori o pridružene generički modela za holistically rješavanje problema churn industrijske klijenta. Ne možemo mjeriti i točnosti modela koji su ugrađeni pomoću strojnog učenja i smo procijenite upute za daljnje razvoj.  

### <a name="acknowledgements"></a>Acknowledgements

U ovom eksperiment je razvio i testirati Serge Berger, pozvana podataka glavnicu Microsoft i Roger Barga, stari mu je naziv proizvoda Manager for Microsoft Azure strojnog učenja. Tim za Azure dokumentaciju gratefully priznaje njihove stručna znanja i Hvala ih za zajedničko korištenje tu studiju.

>[AZURE.NOTE] Podaci koji se koriste za ovaj eksperiment nije dostupnom javnosti. Primjer za izradu modela za učenje računala za analizu churn, potražite u članku: [Telco churn modela predloška](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) u [Galeriju obavještavanje Cortana](http://gallery.cortanaintelligence.com/)


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Problem churn klijenta
Tvrtke u tržištu potrošača i sve enterprise sektori morati baviti churn. Ponekad churn je viškom i utječe odluke pravila. Tradicionalni rješenje je predviđanje visoke propensity churners i adresa njihovim potrebama putem servisa concierge marketinških kampanja, ili primjenom posebno dispensations. Ove postupke mogu se razlikovati iz industrijskih za industrijske i čak i određeni potrošača klaster na drugu unutar jedne industrijskih (na primjer, Telekomunikacije).

Uobičajeni faktor je da tvrtke moraju da biste minimizirali te naporima zadržavanja posebno klijenta. Dakle, prirodnim methodology bi da biste rezultatu svaki klijenta s vjerojatnost churn i adresa gornjoj N one. Najbolji klijenti možda najisplativiji one; Ako, na primjer, u više sofisticirane scenarijima dobiti funkcija stvorenoj tijekom odabira ikone za posebne dispensation. Međutim, te pitanja vezana uz su samo dio jednu zaokruženu Strategije za churn. Tvrtke i morati poduzeti u račun rizika (i pridruženi rizika odstupanje), razinu i trošak intervencije i segmente plausible klijenta.  

##<a name="industry-outlook-and-approaches"></a>Outlook industrijskih i pristupa
Sofisticirane rukovanje churn je znak od starijih grana. Klasični primjer je industrijskih Telekomunikacije gdje se gube pretplatnike često prebacivanje iz jednog davatelja u drugu. U ovom dobrovoljna churn je problem značajke prime. Nadalje, davatelji ste akumulirani značajan znanja o *churn upravljačke programe*, koje su u čimbenika kupci pogon da biste se prebacili.

Ako, primjerice, slušalice ili uređaj izbor vam je dobro poznatog upravljački program churn tvrtke mobilnog telefona. Zbog toga popularne pravila je subsidize cijena slušalicu za pretplatnike na nove i puni cijelog cijenu za postojeće korisnike za nadogradnju. Prethodno, to pravilo ima koji vode kupci skakutanje davatelja jedan drugome da biste dobili novi popust koji pak sadrži upit davatelji da biste prilagodili svoje strategije.

Visoke volatility u ponude slušalica je faktor vrlo brzo poništava modelima churn koji se temelje na trenutni slušalica modela. Uz to, mobilnim telefonima nisu samo uređaji telekomunikaciju; su način izvješća (preporučuje se Iphoneu), a te društvenih predictors su izlaze običnog Telekomunikacije skupa podataka.

Neto rezultat Modeliranje je nije moguće razviju zvuk pravila jednostavno uklanjanjem poznati razloge churn. Zapravo je neprekinuti Modeliranje strategije, uključujući klasični modela koji brojčano categorical varijable (kao što su odlukama), **obavezno**.

Korištenje veliki skupa podataka na svojim klijentima, tvrtkama ili ustanovama izvršavate velikih skupova podataka analize (posebice u churn otkrivanje velikih skupova podataka na temelju) kao učinkovitih pristup problema. Možete pronaći dodatne informacije o pristup velikih skupova podataka problema churn u preporuke o ETL sekciju.  

##<a name="methodology-to-model-customer-churn"></a>Methodology da biste churn kupca modela
Uobičajeni rješavanje problema procesa da biste riješili kupca churn je prikazan u slika 1-3:  

1.  Rizik model omogućuje vam treba uzeti u obzir utjecaj akcije vjerojatnost i rizika.
2.  O modelu intervencije omogućuje vam treba uzeti u obzir kako razinu intervencije može utjecati na prikaz vjerojatnost churn i iznos kupca vrijednost trajanja (CLV).
3.  Ovu analizu lends sam da biste analizirali qualitative koji je escalated određene proaktivne marketinškom kampanjom namijenjen klijentima izlaganje optimalnih ponudu.  

![][1]

Takvog naprijed izgleda je najbolji način tretira churn, ali ga u sklopu složenosti: imamo za razvoj više modela archetype i praćenje međuzavisnosti u modelima. Interakcija među modela mogu biti encapsulated kao što je prikazano na sljedećem su dijagramu:  

![][2]

*Slika 4: Sjedinjeno komuniciranje više modela archetype*  

Interakcija raznih u modelima je ključ ako smo isporuka jednu zaokruženu pristup zadržavanja klijenta. Svaki model nužno smanjuje tijekom vremena; Stoga arhitektura je programa implicitno petlji (slično archetype odredio JASNO DM podataka dubinsko pretraživanje standardni, [***3***]).  

Cjelokupan ciklus segmente razlaganja rizika, odluka i marketing ostaje generalizirano strukturu, primijeniti na brojne poslovne probleme. Analiza churn je jednostavno istaknuti predstavnika ovu grupu problema jer je prikazuje publikom složene poslovne probleme koji omogućuju pojednostavljeni predvidljivu rješenja. Društvene aspekte Moderna pristup churn nije osobito u istaknute su pristup, ali su društvene aspekte encapsulated u archetype Modeliranje kao što bi bili u bilo kojem modelu.  

Dodatak zanimljive ovdje je analize velikih skupova podataka. Današnji telekomunikaciju i maloprodaja tvrtke prikupljanje iscrpan podataka o svoje kupce, a ne možemo možete jednostavno foresee da potrebe za povezivanje s većim brojem modela će postati uobičajenih trend dali novih trendova kao što su Internet stvari i ubiquitous uređaja koje omogućuju tvrtke uključivanja pametna rješenja na više slojeva.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Implementacijom archetype Modeliranje u strojnog učenja Studio
Dani problem koji se samo opisuje što je najbolji način za implementaciju integrirani Modeliranje i bilježenje rezultata pristup? U ovom ćete odjeljku smo će demonstrirati kako ćemo napraviti to pomoću Azure strojnog učenja Studio.  

Više modela pristup je na mora prilikom dizajniranja globalnog archetype za churn. Čak i bilježenja rezultata (predvidljivu) dio pristup mora biti više modela.  

Na sljedećem su dijagramu prikazani u predložak koju smo stvorili koji uključuje četiri algoritama bilježenja rezultata u strojnog učenja Studio za predviđanje churn. Razloga za korištenje više modela pristup nije samo za stvaranje klasifikatora ensemble kako bi se povećala točnost, ali da biste zaštitili prekoračenja staviti i za poboljšanje odabir prescriptive značajki.  

![][3]

*Slici 5: Brisanje knjižne od churn Modeliranje pristup*  

U sljedećim se odjeljcima sadrže dodatne pojedinosti o predložak bilježenje rezultata modela koji ne možemo implementiran putem Studio strojnog učenja.  

###<a name="data-selection-and-preparation"></a>Odabir podataka i pripreme
Podaci koji se koriste za izgradnju u modelima i klijentima rezultat je dobivenog CRM okomiti rješenja, podatke nečitljivima za zaštitu privatnosti kupca. Podaci sadrže informacije o 8.000 pretplate u SAD- a kombinira tri izvora: dodjeljivanje podataka (pretplatu metapodaci), aktivnosti podataka (korištenje sustava) i podatke o klijentu podrška. Podaci ne obuhvaća sve poslovne povezanih informacija o klijentima; na primjer, ne uključuje vjernost metapodataka ili odobrenja rezultati.  

Zbog jednostavnosti, ETL i čišćenja procesa podataka su izvan opsega jer smo pretpostavlja da se Priprema podataka već ima obavljeno drugdje.   

Odabir značajki za Modeliranje temelji se na preliminarni Značajnost bilježenje rezultata skupa predictors, obuhvaćeno postupak koji koristi slučajni skupa stabala modul. Implementacije u strojnog učenja Studio ćemo izračunato srednje vrijednosti, median i raspone za predstavniku značajke. Na primjer, dodali smo zbrajanja Kvalitativni podataka, kao što su minimalne i maksimalne vrijednosti za aktivnosti korisnika.    

Ne možemo zabilježene i vremenski informacije za zadnjih šest mjeseci. Ne možemo analizirati podatke traje jednu godinu, a zatim ćemo uspostaviti da čak i ako postoje statistically značajan trendova, učinak na churn je znatno smanjiti Nakon šest mjeseci.  

Najvažnije je cijeli postupak, uključujući ETL, odabir značajki i Modeliranje implementiran u Studio učenje računala, pomoću izvora podataka u programu Microsoft Azure.   

Sljedeći dijagrami ilustriraju podataka koja se koristila.  

![][4]

*Slika 6: Isječak izvora podataka (nečitljivima)*  

![][5]


*Slika 7: Značajke dobivenih iz izvora podataka*
 
> Imajte na umu da je privatna ove podatke i stoga model i podataka nije moguće zajednički koristiti.
> Međutim, model slične pomoću javno objavljivanje podataka, potražite u članku ovaj uzorak Poigrajte se u [Galeriju obavještavanje Cortana](http://gallery.cortanaintelligence.com/): [Telco kupca Churn](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Da biste saznali više o kako biste mogli implementirati u model churn analize pomoću Cortana obavještavanje paket, preporučujemo i [Ovaj videozapis](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) po Tok Hyong Wee za Upravitelj više programa. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Algoritmima u predložak

Da biste sastavili predložak (nema prilagodbe) koristi sljedeće četiri strojnog učenja algoritmima pomoću:  

1.  Logistika regresije (LR)
2.  Stabla odlučivanja boosted (BT)
3.  Određuje prosječna vrijednost perceptron (AP)
4.  Podrška za vektorski računala (SVM)  


Sljedeći dijagram prikazuje dio površini za dizajniranje eksperiment koji pokazuje redoslijed kojim su stvorene u modelima:  

![][6]  


*Slika 8: Stvaranje modela u strojnog učenja Studio*  

###<a name="scoring-methods"></a>Bilježenje rezultata metode
Ne možemo testu dobije četiri modela pomoću označene obuka skup podataka.  

Ne možemo prijaviti i bilježenja rezultata dataset usporediti model ugrađeni korištenjem radne površine izdanje SAS Enterprise maloljetnik 12. Ne možemo mjeri točnosti SAS modela i sve četiri strojnog učenja Studio modela.  

##<a name="results"></a>Rezultati
U ovom se odjeljku smo nude naše findings o točnosti modele, ovisno o bilježenja rezultata skupu podataka.  

###<a name="accuracy-and-precision-of-scoring"></a>Točnost i preciznosti bilježenje rezultata
Općenito govoreći, implementaciji u Azure strojnog učenja nalazi iza SAS točnost za otprilike 10 do 15% (područje u odjeljku krivulje ili AUC).  

Najvažnije metriku u churn je stopa misclassification: to jest, od vrha N churners kao predviđene tako da na klasifikatora, koji od njih činila **ne** churn i još primio poseban tretman? Na sljedećem su dijagramu uspoređuje ovaj misclassification stopu za sve modela:  

![][7]


*Slika 9: Passau predložak područje u odjeljku krivulje*

###<a name="using-auc-to-compare-results"></a>Korištenje AUC za usporedbu rezultata
Područje u odjeljku krivulje (AUC) je metriku koja predstavlja globalni mjera *separability* između distribucija brojčane pokazatelje za pozitivne i negativne populacije. Je slično tradicionalni grafikonu tekstnog okvira Operator karakteristikama (ROC), ali jedan važna razlika je da metriku AUC ne zahtijeva odabir vrijednosti praga. Umjesto toga putem **sve** moguće odabire zbroji rezultate. Nasuprot tome, tradicionalni ROC graph prikazuje pozitivne stopa na okomitoj osi, a false pozitivan stopa na vodoravnoj osi, a mijenja se praga klasifikacije.   

AUC obično koristi se kao mjera vrijedi za različite algoritama (ili druge sustave) jer dopušta modeli se uspoređuje pomoću njihove AUC vrijednosti. Ovo je popularne pristup u djelatnosti kao što su meteorology i biosciences. Dakle, AUC predstavlja popularne alat za assessing klasifikatora performansi.  

###<a name="comparing-misclassification-rates"></a>Usporedba misclassification stope
Ne možemo usporedbi stope misclassification na skupu podataka u pitanju pomoću podataka CRM približno 8.000 pretplata.  

-   Stopa misclassification SAS je 10 do 15%.
-   Stopa misclassification strojnog učenja Studio je 15-20% za churners vrha 200 300.  

U industrijskih Telekomunikacije je važno da biste riješili samo korisnici koji imaju najviših rizik za churn pomoću ih concierge servisa ili drugi poseban tretman. U tom odnosu implementaciju strojnog učenja Studio postiže rezultate on par with SAS modela.  

Uz iste token, točnost je važnija od preciznosti jer smo zanima uglavnom pravilno klasifikaciju potencijalne churners.  

Na sljedećem su dijagramu iz Wikipedije prikazuje odnos u životopisnim, jednostavno-na-razumijevanje grafiku:  

![][8]

*Slika 10: Tradeoff između točnost i preciznosti*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Točnost i preciznosti rezultata za model stabla odlučivanja boosted  

Sljedeći grafikon prikazuje neobrađenog rezultate iz bilježenje rezultata pomoću strojnog učenja predložak za stabla modela boosted odluke koje se najčešće točne jednu od četiri modela:  

![][9]

*Slika 11: Boosted odluka stabla modela značajke*

##<a name="performance-comparison"></a>Usporedba performansi
Ne možemo usporedbi Brzina kojom podataka je testu dobije pomoću modela strojnog učenja Studio i usporediti model stvoren pomoću radne površine izdanje SAS Enterprise maloljetnik 12.1.  

U sljedećoj su tablici prikazane su performanse algoritmima pomoću:  

*Tablica 1. Općenite performanse (točnost) algoritmima pomoću*

| LR|BT|AP|SVM|
|---|---|---|---|
|Prosječna modela|Najbolji modela|Ne funkcioniraju dovoljno uspješno|Prosječna modela|

Modeli smješten u strojnog učenja Studio outperformed SAS 15 25% za brzinu izvođenja, ali točnost je uglavnom nominalnom.  

##<a name="discussion-and-recommendations"></a>Rasprave i preporuke
U industrijskih Telekomunikacije emerged imaju nekoliko prakse da biste analizirali churn, uključujući:  

-   Proizlaziti metriku četiri osnovne kategorije:
    -   **Entitet (na primjer, pretplatu)**. Dodjela resursa za osnovne informacije o pretplati i/ili klijenta koji je predmet churn.
    -   **Aktivnosti**. Nabavite sve informacije o korištenju moguće koji se odnose na entitet, na primjer, broj prijave.
    -   **Korisničkoj podršci**. Festival sredine podaci iz zapisnika podršku klijenta koji određuju hoće li pretplata imali problema ili interakcije s korisničkoj službi.
    -   **Konkurencije i poslovnih podataka**. Dobiti moguće informacije o klijentu (na primjer, može biti dostupne ili teško pratiti).
-   Koristite važnost za odabir značajki pogon. To podrazumijeva da modela stabla odlučivanja boosted uvijek je obećanjima pristup.  

Korištenje ove četiri kategorije stvara dao privid koji jednostavne *deterministic* pristup koja se temelji na indeksima oblikovan na pametnije čimbenika po kategoriji, trebali biste suffice za prepoznavanje korisnika na odgovornost za churn. Nažalost, iako ova notion čini plausible, je false Pretvorba. Razlog je churn je vremenski efekt, a čimbenika pojačava churn obično su u tranzitne stanja. Što potencijalnih klijenata kupca treba uzeti u obzir ostavite danas mogu se razlikovati sutra pa ga certainly razlikovat će se šest mjeseci od sada. Stoga je *probabilistic* na šalju se model.  

Ovo važno opažanje često uvidjeti u tvrtke koji se obično business intelligence usmjerena pristup želi analize, uglavnom jer je to je jednostavnije prodaje i pristupiti jednostavne automatizaciju.  

Međutim, obećanje samoposlužnog analize pomoću strojnog učenja Studio je četiri kategorije podataka, gradiran odjel ili odjelu postaju je koristan izvora za strojnog učenja o churn.  

Drugi zanimljiv mogućnost uskoro u Azure strojnog učenja je mogućnost dodavanja prilagođene modul u spremište unaprijed definirane moduli koji su već dostupni. Tu mogućnost zapravo, stvara priliku da biste odabrali biblioteke i stvaranje predložaka za okomite tržištima. Nije važno differentiator od Azure strojnog učenja na tržištu mjestu.  

Nadamo da biste nastavili u ovoj se temi u budućnosti, osobito vezane uz analize velikih skupova podataka.
  
##<a name="conclusion"></a>Zaključak
Ova dokumentacija opisuje prvo pristup tackling uobičajenih problema kupca churn pomoću generički okvira. Ne možemo smatra predložak za bilježenje rezultata modela i implementirati pomoću Azure strojnog učenja. Na kraju, ne možemo kažnjeni točnost i performanse rješenja predložak pogledu usporediti algoritama u SAS.  

**Dodatne informacije:**  

Jeste li ovog članka pomoći? Pošaljite nam povratne informacije. Recite nam u rasponu od 1 (slabo) 5 (izvrstan), kako bi ocijenili ovog članka i zašto su dobit je ovaj ocjena? Ako, na primjer:  

-   Su vam ocjena je visoke zbog imate dobar Primjeri, snimki izvrstan zaslona, poništite okvir pisanje ili drugog razloga?
-   Su vam ocjena je malo nisku Primjeri, mutno zaslona ili nejasno pisanje?  

Ovaj povratne informacije će Pridonesite poboljšanju kvalitete studije smo izdanja.   

[Slanje povratnih informacija](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Reference
[1] predvidljivu analize: izvan predviđanja, Srednja McKnight upravljanje informacije srpanj/kolovoza 2011, p.18 20.  

[2] članka Wikipediji: [točnost i preciznosti](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [JASNO DM 1.0: vodič za dubinsko pretraživanje detaljne podatke](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [Marketing velikih skupova podataka: učinkovitije sudjelovati klijentima i pogona vrijednost](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco churn modela predloška](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) u [Galeriju obavještavanje Cortana](http://gallery.cortanaintelligence.com/) 
 
##<a name="appendix"></a>Dodatak

![][10]

*Slika 12: Snimku prezentacije na churn predložak*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
