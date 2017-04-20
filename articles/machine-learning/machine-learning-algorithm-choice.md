<properties
    pageTitle="Kako odabrati stroj učenje algoritmi | Microsoft Azure"
    description="Kako odabrati Azure stroj učenje algoritme za učenje supervised i unsupervised u Klasteriranje, klasifikacije ili regresije eksperimenata."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Kako odabrati Algoritmi za učenje Microsoft Azure strojne grupe

Odgovor na pitanje "Što stroj učenje algoritam koristiti?" uvijek je "Ovisi." Ovisi veličine, kvalitete i prirodi podataka. To ovisi o tome što želite učiniti s odgovor. Ovisi o kako je matematičke algoritam prevedene u upute računalu koje koristite. I ga ovisi o tome na koliko vremena imate. Čak i većinu iskusnim podataka fizičari ne može odrediti algoritam koji će izvršiti najbolje prije nego ih.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Algoritam stroj učenje Cheat lista

**Microsoft Azure stroj učenje algoritam Cheat list** pomaže vam odabrati desno stroj učenje algoritam za predvidljivih analytics rješenja iz biblioteke Microsoft Azure stroj učenje algoritama.
Ovaj članak vas vodi kroz kako ga koristiti.

> [AZURE.NOTE] Preuzimanje list oglasi i slijedite uz ovaj članak, idite na [stroj učenje list oglasi algoritam za Microsoft Azure stroj učenje Studio](machine-learning-algorithm-cheat-sheet.md).

Ovaj list oglasi ima vrlo određene publike na umu: na početku podataka pozvana s učenje Dodiplomski student razinu stroj pokušava započeti s odaberite algoritam u Azure učenje Studio strojne grupe. To znači da čini neke generalizations i oversimplifications, ali možete ga će pokažite u sigurnom smjeru. Također znači da nema mnogo algoritmi nije navedena ovdje. Kako se rješava problem više potpuni skup dostupne metode raste Azure stroj učenje, ćemo dodati ih.

Te preporuke su kompilirani povratne informacije i savjete iz mnogo fizičari podataka i stručnjaci za učenje strojne grupe. Možemo niste suglasni na sve, ali ste pokušao sam harmonize naših mišljenja u grubi suglasnost. Većina izvatke neslaganjima počinju s "Ovisi..."

### <a name="how-to-use-the-cheat-sheet"></a>Kako koristiti list oglasi

Čitanje put i algoritam natpise na grafikonu kao "za * &lt;put natpis&gt; * koristiti * &lt;algoritam&gt;*." Na primjer, "za *brzinu* koristite *dva klase logistika regresije*." Ponekad više granu će se primijeniti.
Ponekad ništa od njih će biti savršeno pristajanje. Oni si namijenjen se pravilo palac preporuke tako nemojte brinuti o tome koji se točan.
Nekoliko fizičari podataka I bila riječ s said koji je pokušajte ih sve samo sigurni način za pronalaženje vrlo najbolje algoritam.

Ovdje je primjer iz [Galerije Cortana inteligencije](http://gallery.cortanaintelligence.com/) eksperimentiraj koji pokušava nekoliko algoritmi protiv iste podatke i uspoređuje rezultata: [usporediti s višestrukim klase Classifiers: slovo prepoznavanja](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Za preuzimanje i ispis dijagram koji daje pregled mogućnosti stroj učenje Studio, pogledajte [Pregled dijagrama Studio učenje Azure strojne grupe mogućnosti](machine-learning-studio-overview-diagram.md).

## <a name="flavors-of-machine-learning"></a>Flavors stroj učenje

### <a name="supervised"></a>Supervised

Algoritmi supervised učenje provjerite predictions na temelju skupa primjeri. Na primjer, rizik pogađanja na buduće cijene mogu se povijesnog cijene dionica. Svaki primjer koristi za osposobljavanje označena je vrijednost kamate — u ovom slučaju cijene dionice. Algoritam supervised učenje traži uzorke u te natpise vrijednost. Možete koristiti informacije koje možda odgovarajuću — dan u tjednu, praznici, poduzeća financijskih podataka, Vrsta industrije, prisutnost događaji disruptive geopolicitical — i svaki algoritam izgleda za različite vrste uzorci. Nakon algoritam je pronašao najbolji uzorak ga možete, koristi taj uzorak da bi predictions neoznačena testiranja podataka — sutra 's cijene.

To je popularna i korisno vrsta učenje strojne grupe. Uz jednu iznimku sve module u Azure stroj učenje su supervised učenje algoritama. Postoji nekoliko određenih vrsta supervised učenje koji su predstavljeni unutar Azure stroj učenje: klasifikacija, regresije i značajkom otkrivanja.

* **Klasifikacija**. Kada se podaci koriste za predviđanje kategoriju, supervised učenje zove klasifikaciju. To je slučaj kada dodjeljujete sliku kao sliku 'mačka' ili 'pas'. Kada postoje samo dvije mogućnosti, to se naziva **dva klasa** ili **binomna klasifikaciju**. Kad postoji više kategorija, kao kada predviđanje Pobjednik Turnir Madness NCAA ožujak, problem je poznato kao **klasifikacija više klase**.

* **Regresije**. Kada vrijednost je u tijeku predviđene s cijene dionica, supervised učenje zove regresije.

* **Značajkom otkrivanja**. Ponekad cilj je identificirati točke podataka koje su jednostavno neuobičajene. U prijevara otkrivanja, na primjer, kupujete uzorci bilo visoko neuobičajene kreditne kartice su sumnjive. Tako brojne su mogući varijacije i osposobljavanje Primjeri tako nekoliko, nije izvedivo Saznajte što lažno aktivnost izgleda. Pristup koji uzima značajkom otkrivanja je jednostavno Saznajte što normalnu aktivnost izgleda (pomoću povijesti koji nisu Lažne transakcije) i identifikaciju ništa znatno se razlikuje.

### <a name="unsupervised"></a>Unsupervised

U unsupervised učenje točaka podataka imati nema natpisa povezana s njima. Umjesto toga, cilj algoritam učenje unsupervised je za organiziranje podataka na neki način ili opisuju njegova struktura. To znači grupiranja u klastere ili pronalaženje pogledate složene podatke tako da se pojavljuje jednostavniji ili više organiziranih različitih načina.

### <a name="reinforcement-learning"></a>Učenje reinforcement

U reinforcement učenje algoritam dobiva odaberite akciju kao odgovor na svakoj točki podataka. Algoritam učenje prima nagrada od signal kratko vrijeme kasnije, označavajući odluku kako dobra je.
Temelji na ovaj algoritam mijenja njezin strategije da biste postigli najvišu nagrada od. Trenutno postoje nema reinforcement učenje algoritam module u učenje Azure strojne grupe. Učenje reinforcement je uobičajena u robotics, gdje je skup čitanja senzora na jednu točku u vremenu točku podataka i algoritam morate odabrati na robot sljedeću akciju. Također je stane na prirodni za Internet stvari aplikacije.

## <a name="considerations-when-choosing-an-algorithm"></a>Razmatranja prilikom odabira algoritam

### <a name="accuracy"></a>Točnost

Dobivanje najtočnije odgovor moguće nije uvijek potrebno.
Ponekad je manjoj mjeri odgovarajuće, ovisno o tome što želite koristiti. Ako je to slučaj, možda nećete moći izrazito Izreži vrijeme obrade po sticking s više Približna metode. Druga prednost više Približna metoda je da oni prirodno obično izbjeći [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Vrijeme za osposobljavanje

Broj minuta ili sati potrebne za uvježbavanje model varira great dogovor između algoritama. Osposobljavanje vrijeme često usko povezana točnost — jedan drugi obično pridružuje. Uz to, neke algoritmi više osjetljivi su na broj točaka podataka od drugih.
Kada je ograničen vrijeme pogona možete izbora algoritam, naročito kada je velik skup podataka.

### <a name="linearity"></a>O linearnosti

Provjerite mnogo algoritmi učenje stroj koristiti linearnosti. Algoritmi linearni klasifikacija pretpostavljaju da klase može biti razdvojena ravnu crtu (ili njegov viši dimenzijama analogni). Te uključiti logistika regresije i podržava strojeva vektor (kao implementirana učenje Azure strojne grupe).
Algoritmi linearne regresije pretpostavljaju da slijedite trendova podataka ravnu crtu. Ove pretpostavke nisu loše neke probleme, ali na drugima oni točnost Premjesti.

![Linearni non klase bounday][1]

***Linearni koji nisu granice klasa*** *-potrebe za oslanjanjem na linearnom klasifikacija algoritam za posljedicu niska točnosti*

![Podatke s nelinearne trend][2]

***Podatke s nelinearne trend*** *-metodom linearne regresije želite generirati mnogo veće pogreške potrebne od*

Usprkos njihove opasnosti linearni algoritmi su vrlo popularne kao prvi redak napada. Oni obično se algorithmically jednostavno i brzo uvježbati.

### <a name="number-of-parameters"></a>Broj parametara

Parametri su knobs pozvana podataka dobiva uključiti prilikom postavljanja algoritam. Oni su brojevi koje utječu na algoritam ponašanje što odstupanje pogreške ili broj iteracija ili mogućnosti između varijanti kako se ponaša algoritam. Osposobljavanje vrijeme i točnost algoritam ponekad može prilično osjetljive dohvat samo desna postavke. Obično, algoritmi s parametrima velike brojeve zahtijevaju Većina pogreške i probne verzije kombinaciju dobra pronalaženje.

Alternativno, postoji [sweeping parametar](machine-learning-algorithm-parameters-optimize.md) modul blok učenje stroj Azure koja automatski pokušava sve kombinacije parametar na bilo kakve preciznosti odaberete. Dok je odličan način da biste bili sigurni da ste rastezati parametar prostora, vrijeme potrebno za uvježbavanje model povećava eksponencijalno s broj parametara.

U okrenut je da mnogi parametri obično znači da algoritam ima veću fleksibilnost. Često možete postići vrlo dobra točnost. Navedeni možete pronaći desno kombinacije postavki parametar.

### <a name="number-of-features"></a>Broj značajki

Za određene vrste podataka broj značajke mogu biti vrlo velike usporedbi broj točaka podataka. To je često slučaj s genetics ili tekstualnih podataka. Velik broj značajke možete bog dolje neke algoritmi učenje, čime osposobljavanje unfeasibly dugo vrijeme. Podrška vektor strojeva videopostavke osobito dobro za ovaj slučaj (vidi dolje).

### <a name="special-cases"></a>Posebni slučajevi

Neke algoritmi učenje provjerite određeni pretpostavke o strukturi podataka ili željene rezultate. Ako nađete onaj koji odgovara vašim potrebama, on može dati više korisne rezultate, točniji predictions ili puta brže obuku.

|**Algoritam**|**Točnost**|**Vrijeme za osposobljavanje**|**O linearnosti**|**Parametri**|**Bilješke**|
|---|:---:|:---:|:---:|:---:|---|
|**Dva klase klasifikacija**| | | | | |
|[logistika regresije](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[šume odluka](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[jungle odluka](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Premalo memorije footprint|
|[Stablo boosted odluka](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Footprint veliku memoriju|
|[mrežni neural](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[Moguće je dodatno prilagođavanje](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[Određuje prosječna vrijednost perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[podržava vektor stroj](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Dobra za velike skupove|
|[lokalno dublje podršku vektor stroj](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Dobra za velike skupove|
|[Stroj točka 'Bayes](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Klasifikacija više klase**| | | | | |
|[logistika regresije](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[šume odluka](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[jungle odluka](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Premalo memorije footprint|
|[mrežni neural](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[Moguće je dodatno prilagođavanje](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[jedan v sve](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|Pogledajte svojstva klase dvije metode odabrano|
|**Regresije**| | | | | |
|[Linearni](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Linearni Bayesian](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[šume odluka](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[Stablo boosted odluka](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Footprint veliku memoriju|
|[šume servisa FAST quantile](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Distribucija umjesto predictions točka|
|[mrežni neural](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[Moguće je dodatno prilagođavanje](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Tehnički zapisnika nelinearno. Za predviđanje broji|
|[Redni broj](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Za predviđanje rang redoslijed|
|**Značajkom otkrivanja**| | | | | |
|[podržava vektor stroj](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Osobito dobro za velike skupove|
|[PCA temelji značajkom otkrivanja](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[K znači](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|Algoritam klasteriranja|


**Algoritam svojstva:**

**●** - prikazuje izvrsna točnost, brza osposobljavanje vremena i korištenje linearnosti

**○** - prikazuje dobar točnost i puta Umjereni osposobljavanje

## <a name="algorithm-notes"></a>Algoritam bilješke

### <a name="linear-regression"></a>Linearna regresija

Kako je već spomenuto, [linearne regresije](https://msdn.microsoft.com/library/azure/dn905978.aspx) prilagođava crte (ili ravnina ili hyperplane) skupa podataka. Je workhorse, jednostavno i brzo, ali može biti pretjeranog simplistic za neke probleme.
Provjerite ovdje za [vodič linearne regresije](machine-learning-linear-regression-in-azure.md).

![Podatke s linearni trend][3]

***Podatke s linearni trend***

### <a name="logistic-regression"></a>Logistika regresije

Iako confusingly uključuje 'regresije' u naziv, logistika regresije je zapravo moćan alat za klasifikaciju [dva klasa](https://msdn.microsoft.com/library/azure/dn905994.aspx) i [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) . Je brz i jednostavan. Činjenica da koristi se 'S'-oblika krivulje umjesto ravnu crtu olakšava natural pristajanje za podjelom grupe podataka. Granice linearni klase daje logistika regresije, pa kada koristite, provjerite linearni manjoj mjeri je nešto možete live s.

![Logistika regresije dva klase podataka s samo jedna značajka][4]

***Logistika regresije dva klase podataka sa značajkom samo jednom*** *-klasa granicu je točka na kojoj logistika krivulje je upravo kao blizu oba klase*

### <a name="trees-forests-and-jungles"></a>Stabala, šuma i jungles

Odluka šuma ([regresije](https://msdn.microsoft.com/library/azure/dn905862.aspx), [dva klasa](https://msdn.microsoft.com/library/azure/dn906008.aspx)i [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), jungles odluka ([dva klasa](https://msdn.microsoft.com/library/azure/dn905976.aspx) i [multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)) i boosted odlukama ([regresije](https://msdn.microsoft.com/library/azure/dn905801.aspx) i [dva klase](https://msdn.microsoft.com/library/azure/dn906025.aspx)) temelje se na stabla odluka foundational stroj učenje koncept. Postoji mnogo varijanti stabla odluka, ali svi služe isto — Podjela značajka prostora u područjima s uglavnom isti natpis. To mogu biti područja dosljedan kategoriju ili vrijednost konstante, ovisno o tome su način klasifikacije ili regresije.

![Značajka prostor subdivides stabla odluka][5]

***Stabla odluka subdivides značajka prostora u područjima otprilike ujednačeno vrijednosti***

Jer značajka prostora možete subdivided u arbitrarily malog područja, jednostavno je zamislite dovoljno finely ga prepoloviti imati jednu točku podataka po regiji — ekstremnim primjer overfitting. Da biste to izbjegli, velikog skupa stabala standardnima s posebnu pažnju matematičke uzima se ne povezuju s stabala. Prosjek ovo "šume odluka" je stablo kojim se izbjegava overfitting. Odluka šuma možete koristiti mnogo memorije. Jungles odluka su varijante koja troši manje memorije štetu malo više vremena za osposobljavanje.

Boosted odlukama izbjeći overfitting ograničavanjem koliko puta možete Podjela i kako nekoliko točaka podataka su dopuštene u svakom području. Algoritam konstrukcije slijed stabala, od kojih svaka uči za kompenzaciju pogreška lijeva stablo prije. Rezultat je vrlo točan učenik koji ima koristi mnogo memorije. Za potpuno tehničke opis odjaviti [Friedman's izvorne papira](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Brza šume quantile regresije](https://msdn.microsoft.com/library/azure/dn913093.aspx) je varijacije stabla odluka za poseban slučaj gdje želite znati ne samo tipične (Medijan) vrijednost podataka unutar područja ali njegov raspodjele obrasca quantiles.

### <a name="neural-networks-and-perceptrons"></a>Neural mrežama i perceptrons

Neural mrežama su mozak-inspired učenje algoritmima koji pokrivaju probleme [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), [dva klasa](https://msdn.microsoft.com/library/azure/dn905947.aspx)i [regresije](https://msdn.microsoft.com/library/azure/dn905924.aspx) . Oni dolaze u beskonačnoj različite, ali sve obrasca usmjereno acyclic grafikoni su neural mreže unutar učenje Azure strojne grupe. To znači da unos značajke prosljeđuju se naprijed (Nikad prema nazad) kroz slijed Slojevi prije se uključen u izlaza. U svaki sloj ulaza su težinski faktor u različite kombinacije, praćene zbrojene i prenose na idući sloj. Ova kombinacija jednostavne izračune rezultira mogućnost prividno Saznajte sofisticirane klase trendova koje granice i podataka po Čarobni. Mnogo preklope mrežama ovaj sortiranja izvršiti u "dublje učenje" koji fuels toliko Tehnički izvješćivanje i znanosti fiction.

Visoke performanse ne dolaze besplatno, iako. Neural mreža može potrajati dugo vremena za uvježbavanje, osobito za velike skupove podataka s mnogo značajki. Također imaju više parametara od većine algoritmi, što znači da sweeping parametar proširuje vrijeme osposobljavanje great dogovor.
I za overachievers koji želite [navesti vlastite strukture mrežu](http://go.microsoft.com/fwlink/?LinkId=402867), mogućnosti su inexhaustible.

<a name="boundaries-learned-by-neural-networks6"></a>![Granice naučili neural mrežama][6]
---------------------------

***Granice naučili neural mreža može biti složen i nepravilni***

[Dva klase perceptron određuje prosječna vrijednost](https://msdn.microsoft.com/library/azure/dn906036.aspx) je neural mrežama odgovor skyrocketing osposobljavanje puta. Koristi mrežni strukturu koja daje granice linearni klase. Je gotovo jednostavne prema standardima današnji, ali ima dugu povijest robustly rad i dovoljno male da biste brzo saznali je.

### <a name="svms"></a>SVMs

Podrška strojeva vektor (SVMs) pronaći granicu koji razdvaja klase po kao širok margina kao moguće. Kada dva klase nije jasno odvojenih, na algoritmi pronaći najbolje granicu oni mogu. Kao pisane Azure stroj učenje [dva klase SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) to čini sa samo ravnu crtu. (U SVM govorite, on koristi linearni jezgre). Budući da izvrši ovaj linearni manjoj mjeri, je prilično brzo pokretati. Gdje ga doista shines je značajka Naglašeni podacima, poput teksta ili genomic. U tim slučajevima SVMs su moći odvojite klase brže i s manje overfitting od većine drugih algoritmi, uz zahtijevanje samo modest količinu memorije.

![Podrška vektor stroj klase granicu][7]

***Klasa granicu stroj vektor tipične podršku Maksimizira margina odvajanja dva klase***

Drugog proizvoda Microsoft Research, [dva klase lokalno Duboko SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) je nelinearnih varijanta zadržava većinu brzinu i memorije učinkovitost linearni verzija SVM. Je idealno za slučajeve gdje linearni pristup ne daju točne dovoljno odgovore. Programeri se čuvati po brzo raščlaniti problem u mnogo malih linearni SVM problema. Čitanje [Puni opis](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) za pojedinosti o kako oni povlače isključivanje ovog štih.

Pomoću dobra proširenje nelinearnih SVMs [jedan klase SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) crta granicu usko navodi cijeli skup podataka. Ona je korisna za otkrivanje značajkom. Nove točke podataka koje padaju daleko izvan tu granicu su dovoljno neuobičajene biti najboljim.

### <a name="bayesian-methods"></a>Metoda Bayesian

Metoda Bayesian imaju izrazito poželjno kvalitete: izbjeći overfitting. Oni to predviđajući neke prije toga vjerojatno raspodjele odgovor. Drugi byproduct ovakvog je imaju vrlo nekoliko parametara. Učenje Azure stroj ima obje Bayesian Algoritmi za klasifikaciju ([dva klase Bayes točku stroj](https://msdn.microsoft.com/library/azure/dn905930.aspx)) i regresije ([linearne regresije Bayesian](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Imajte na umu ove pretpostavljaju da podaci mogu podijeliti ili stane ravnu crtu.

Povijesni bilješku Bayes' točku strojeva razvijene su na Microsoft Research. Imaju neke iznimno prekrasnu theoretical rad iza njih. Zainteresirani učenika usmjereni na [originalnom članku u JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) i [insightful blog po Chris lovca](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Specijalizirane algoritmi

Ako imaju vrlo određeni cilj možda sreće. Unutar zbirke Azure stroj učenje postoje algoritmi specijalizirani za predviđanje rang ([redni broj regresije](https://msdn.microsoft.com/library/azure/dn906029.aspx)), predviđanje count ([Poisson regresije](https://msdn.microsoft.com/library/azure/dn905988.aspx)) i značajkom otkrivanja (jedan temelji na [glavni komponente analize](https://msdn.microsoft.com/library/azure/dn913102.aspx) i temelji na [podržava vektor strojne grupe](https://msdn.microsoft.com/library/azure/dn913103.aspx)s).
I nema u lone klasteriranja algoritam kao i ([znači K](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![PCA temelji značajkom otkrivanja][8]

***PCA temelji značajkom otkrivanja*** *-Golema većina podataka pada u stereotypical raspodjele; točke deviating izrazito iz raspodjele su je sumnjive*

![Skup podataka grupirane pomoću K znači][9]

***Skup podataka grupirane u 5 klastere pomoću K znači***

Također je programa ensemble [jedan v sve multiclass klasifikatora](https://msdn.microsoft.com/library/azure/dn905887.aspx), čime se prekida klasifikacija problem N klase u N-1 dvije klase klasifikacija probleme. Točnost, osposobljavanje vrijeme i svojstva linearnosti određuje dva klase classifiers koristi.

![Dva klase classifiers kombinirati obrasca klasifikatora tri klasa][10]

***Par classifiers dvije klase kombinirati obrasca klasifikatora tri klasa***

Učenje Azure strojne grupe uključuje i pristup framework učenje snažne stroj pod naslov [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
VW defies kategorizacije ovdje, budući da Naučite klasifikacija i regresije probleme i čak možete saznati iz djelomično neoznačena podataka. Možete ga koristiti bilo koju od broj učenje algoritmi, gubitak funkcije i algoritmi optimizacije konfigurirati. Dizajnirana je iz tlo biti učinkovitiji, paralelno i izuzetno brzo. Rukuje ridiculously velike skupove s malo truda vidljivu.
Koraci i su dovela po Microsoft Research's vlastite John Langford VW je Formula jednu stavku u polju algoritmi burzovni automobila. Nije svaki problem odgovara VW, ali ako vašima ne može biti vrijedi dok za climb krivulje učenje na njegov sučelja. Također je dostupan kao [samostalan Otvori izvor koda](https://github.com/JohnLangford/vowpal_wabbit) u nekoliko jezika.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
