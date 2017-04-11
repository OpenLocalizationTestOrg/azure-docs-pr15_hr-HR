<properties 
    pageTitle="Vodič za neto # Neural mreža specifikacije jezika | Microsoft Azure" 
    description="Sintaksa za u neto # neural mrežama specifikacije jezika, zajedno s primjere stvaranje prilagođenog neural mrežnog modela u programu Microsoft Azure ML pomoću neto#" 
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
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Vodič kroz neto # neural mreže specifikacije jezika za Azure strojnog učenja

## <a name="overview"></a>Pregled
Neto # je jezik koji je razvio Microsoft koji se koristi da biste definirali arhitekturi neural mreže za neural mreže module u Microsoft Azure strojnog učenja. U ovom se članku opisano:  

-   Osnovni koncepti vezane uz neural mreža
-   Preduvjeti za neural mreže i upute za definiranje glavna odjeljka
-   Sintaksa i ključne riječi specifikacije jezika neto #
-   Primjeri prilagođenih neural mreža stvoren pomoću neto# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Osnove neural mreže
Struktura neural mreže sastoji se od ***čvorove*** su organizirane u ***slojevima***i ponderiranog ***veze*** (ili ***rubova***) između čvorove. Veze su usmjereni, a svaku vezu s ima čvor ***izvora*** i čvor ***odredište*** .  

Svaki ***trainable sloja*** (u skrivene ili je sloj Izlaz) sadrži jednu ili više ***objedinjuje veze***. Veza paket sastoji se od sloj izvora i specifikacija veze iz taj sloj izvora. Sve veze u određenom paket zajedničko korištenje iste ***sloja izvora*** i isti ***odredište sloja***. U neto # veze paket smatra se kao pripadaju u paket odredište sloja.  
 
Neto # podržava različitih vrsta veze mapirati skrivenim slojevima i mapirati na izlaze objedinjuje, možete prilagoditi način unosa.   

Zadane ili standardni paket je **cijeli paket**, povezan svaki čvor u sloju izvor svaki čvor u sloju odredište.  

Uz to, neto # podržava sljedeće četiri vrste objedinjuje napredne veze:  

-   **Filtrirana objedinjuje**. Korisnik možete definirati predikata pomoću mjesta čvor sloja izvora i čvor sloja odredište. Čvorovi povezani svaki put kada se predikata je True.
-   **Convolutional objedinjuje**. Korisnik možete definirati small susjedstva čvorove u sloju izvora. Svaki čvor u sloju odredište povezan je s jedan susjedstvo čvorove u sloju izvora.
-   **Red čekanja objedinjuje** i **objedinjuje normalizacija odgovor**. Te su slične convolutional objedinjuje u tom se korisniku definira small susjedstva čvorove u sloju izvora. Razlika je debljine rubova u te objedinjuje nisu trainable. Umjesto toga, unaprijed definiranih funkcija primjenjuje se na izvorne vrijednosti čvor interpolacijom određuje vrijednost čvor odredište.  

Neto # definiranje pomoću struktura neural mreže omogućuje definiranje složene strukture kao što su duboke neural mreže ili convolutions proizvoljne dimenzije, koje se gube se da biste poboljšali učenje podataka kao što su slike, audiozapis ili videozapis.  

## <a name="supported-customizations"></a>Podržani prilagodbe
Pomoću tipki neto # možete jako specifične arhitektura neural mreže modela koji ste stvorili u Azure strojnog učenja. možeš:  

-   Stvaranje skrivenim slojevima i kontrolirati broj čvorove u svakom sloja.
-   Odredite kako slojeve ćete biti međusobno povezani.
-   Definirajte posebne povezivanje strukture, kao što su convolutions i Debljina objedinjuje za zajedničko korištenje.
-   Navedite aktivaciju različite funkcije.  

Detalje o sintaksa specifikacije jezika potražite u članku [Specifikacija strukturu](#Structure-specifications).  
 
Primjeri koji definira neural mreža za neke uobičajene strojnog učenja zadatke iz jednostrano za složene, potražite u članku [primjeri](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Općeniti preduvjeti
-   Mora postojati jedan izlazni sloja, barem jedan unos sloj i nula ili više skrivenih slojeva. 
-   Svaki sloj ima fiksnim brojem čvorove, pojmovno raspoređeni u pravokutni raspon proizvoljne dimenzije. 
-   Unos slojeve imate pridružene parametre obučeni i predstavljaju točku gdje podaci instance unosi s mrežom. 
-   Trainable slojevima (skrivene i izlazni slojeva) imate pridružene obučeni parametri koji se nazivaju težina i biases. 
-   Čvorovi izvorišne i odredišne mora biti u zasebnom slojevima. 
-   Veza mora biti acyclic; drugim riječima, ne može lanac veza koje se oni mogu dovesti do vratite čvor početnog izvora.
-   Sloj izlaz ne mogu biti izvor sloja od veza paket.  

## <a name="structure-specifications"></a>Specifikacije strukture
Specifikacije strukturu neural mreže sastoji se od tri odjeljka: **konstante izvješća**, **deklariranje sloja**, a zatim **vezu deklariranje**. Postoji i sekcije za **zajedničko korištenje izvješća** nije obavezno. U odjeljcima možete navesti bilo kojim redoslijedom.  

## <a name="constant-declaration"></a>Konstanta deklaracija 
Konstanta deklariranje nije obavezno. Pruža znači da biste odredili vrijednosti koje se koriste na neko drugo mjesto u definiciji neural mreže. U upravi sastoji se od identifikatora slijedi znak jednakosti i vrijednost izraza.   

Na primjer, sljedeća naredba definira konstantu **x**:  


    Const X = 28;  

Da biste definirali dva ili više konstante istodobno, stavite s nazivima identifikatora i vrijednosti u vitičaste zagrade i odvojite ih točkom sa zarezom. Ako, na primjer:  

    Const { X = 28; Y = 4; }  

Na desnoj strani svaki dodjele izraz može biti cijeli broj, realni broj, Booleova vrijednost (True ili False) ili matematički izraz. Ako, na primjer:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Deklariranje sloja
Potreban je deklariranje sloja. Definira izvor sloja, uključujući njene objedinjuje veze i atributi i veličinu. U upravi započinje nazivom sloja (unos, skrivene ili izlaz), nakon čega slijedi dimenzije sloj (n-torke pozitivan cijelih brojeva). Ako, na primjer:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Umnožak dimenzije je broj čvorove u sloju. U ovom primjeru postoje dvije dimenzije [5,20], što znači da postoje 100 čvorove u sloju.
-   Slojevi mogu biti deklarirani u nekom redoslijedu, uz jednu iznimku: Ako je definiran više unos sloja, redoslijed kojim su deklarirane moraju se podudarati redoslijeda značajke ulaznih podataka.  


Da biste odredili bude automatski određen broj čvorove u sloja, koristite ključnu riječ **automatski** . Ključna riječ **automatski** je različitih efekata, ovisno o sloj:  

-   U izvješća programa unos sloja broj čvorove je brojne značajke ulaznih podataka.
-   U deklaracija skriveni sloj, broj čvorove je broj koji je vrijednost parametra određen broj **skrivene čvorove**. 
-   U programa deklariranje sloja izlaz broj čvorove je 2 za klasifikaciju dva predmete, 1 za regresije i jednak broj čvorove izlaz za multiclass klasifikaciju.   

Ako, na primjer, sljedeću definiciju mreže omogućuje veličinu svih slojeva automatski određivanja:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Deklariranje sloja za sloj trainable (skrivene ili izlazni slojeva) po želji možete uključiti izlaz funkciju (naziva se i funkciji aktivacije), što će **sigmoid** za klasifikaciju modele i **linearne** regresije modela. (Čak i ako koristite zadani, koje možete izričito navedite funkciju aktivaciju po želji za preglednosti.)

Podržane su sljedeće funkcije izlaz:  

-   sigmoid
-   Linearni
-   softmax
-   rlinear
-   kvadrat
-   SQRT
-   srlinear
-   Abs
-   TANH 
-   brlinear  

Ako, na primjer, sljedeće deklariranje koristi funkciju **softmax** :  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Deklariranje veze
Odmah nakon definiranja trainable sloja, morate postaviti veze između slojeve koje ste definirali. Deklariranje paket veze počinje u ključnu riječ **od**, nakon čega slijedi naziv sloja na paket izvora i vrsta paket vezu da biste stvorili.   

Trenutno podržani su pet vrste objedinjuje veze:  

-   **Potpuna** objedinjuje označenu ključnu riječ **sve**
-   **Filtrirana** objedinjuje označenu u ključnu riječ **koju**slijedi predikata izraza
-   **Convolutional** objedinjuje označenu u ključnu riječ **convolve**, nakon čega slijedi atribute convolution
-   **Pooling** objedinjuje označenu ključne riječi **Maks skup** ili **znače skup**
-   **Normalizacija odgovor** objedinjuje označenu ključnu riječ **odgovor pravila**      

## <a name="full-bundles"></a>Potpuna objedinjuje  

Pune veze paket sadrži veze s svaki čvor u sloju izvora na svakom čvor u sloju odredište. To je Zadana vrsta veze mreže.  

## <a name="filtered-bundles"></a>Filtrirani objedinjuje
Specifikacije paket filtrirani veze sadrži na predikata, sintaktički, izražena puno kao što je C# lambda izrazu. U sljedećem primjeru definira dva filtrirane objedinjuje:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   U predikata _ByRow_, **s** je parametar koji predstavlja indeksa u pravokutni raspon čvorove unos sloja _piksela_i **d** je parametar koji predstavlja indeksa u polju čvorove skriveni sloj _ByRow_. Vrsta podatka **s** i **d** je n-torke s cijelim brojevima duljine dva. Pojmovno, **s** raspona preko svih parova cijelih brojeva s _0 < = s [0] < 10_ i _0 < = s[1] < 20_, a **d** rasponi preko svih parova decimale, s _0 < = [0] d < 10_ i _0 < = d[1] < 12_. 
-   Na desnoj strani izraz predikata postoji li uvjet. U ovom primjeru, za svaku vrijednost parametra **s** i **d** tako da je True, uvjet postoji rub sloja čvorovi izvor sloja čvor odredište. Dakle, ovaj izraz filtra označava da u paket sadrži veze s čvor definira **s** čvor definira **d** u svim slučajevima gdje je jednako [0] d s [0].  

Po želji možete navesti skup debljine za filtrirani paket. Vrijednost atributa **debljine** mora biti n-torke plutajućih zareza vrijednosti duljine koji odgovara broju veza koje se definira na paket. Prema zadanim postavkama debljine slučajno generiraju.  

Težina vrijednosti grupiraju se funkcija index čvor odredište. To jest, odredište čvor prvi je povezano s čvorove izvor K, prvi _K_ elemenata **debljine** n-torke su debljine za prvu čvor odredište redoslijedom indeks izvora. Isto vrijedi za preostale čvorove odredište.  

Nije moguće navesti debljine izravno kao konstante. Na primjer, ako ste naučili na debljine prethodno, možete ih odrediti kao konstante pomoću ove sintakse:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional objedinjuje
Kad podatke tečaj sadrži istovrstan strukturu, convolutional veze najčešće koriste da biste saznali više razine značajke podataka. Ako, na primjer, u odjeljku slika audio ili video podaci, prostorno ili vremenski dimensionality mogu biti prilično ujednačeno.  

Convolutional objedinjuje uključivanja pravokutni **jezgre** koji su slid kroz dimenzije. Zapravo, svaki otklanjanje definira skup debljine primijenili u lokalnom susjedstva se nazivaju **Otklanjanje aplikacije**. Svaku aplikaciju otklanjanje odgovara čvor u sloju izvora koji se nazivaju i **središnje čvor**. Težina na otklanjanje zajednički koristi mnogo veze. U convolutional paket, svaki otklanjanje je pravokutni, a sve aplikacije otklanjanje su iste veličine.  

Convolutional objedinjuje podržavaju sljedećim atributima:

**InputShape** definira dimensionality sloj izvora u svrhu ovaj convolutional paket. Ta vrijednost mora biti n-torke pozitivni cijeli brojevi. Proizvoda s cijelim brojevima mora biti jednak broju čvorove u sloju izvora, ali u suprotnom je moraju se podudarati dimensionality deklarirane za sloj izvora. Duljina ovaj n-torke postaje vrijednost **broja argumenata funkcije** za convolutional paket. (Obično broj argumenata funkcije se odnosi na broj argumenata ili operanda koji funkcija može potrajati.)  

Da biste definirali oblika i mjesta na jezgre, koristite atribute **KernelShape** **koraka**, **ispune**, **LowerPad**i **UpperPad**:   

-   **KernelShape**: (obavezno) definira dimensionality svaki otklanjanje za convolutional paket. Ta vrijednost mora biti n-torke od duljine jednako je broj argumenata funkcije na paket pozitivni cijeli brojevi. Svaku komponentu ovaj n-torke mora biti nije veća od odgovarajuće komponente **InputShape**. 
-   **Koraka**: (nije obavezno) definira klizača veličine convolution (jedan korak veličina za svaku dimenzije), koji je udaljenost između središnje čvorove korak. Ta vrijednost mora biti n-torke od duljine koji je broj argumenata funkcije na paket pozitivni cijeli brojevi. Svaku komponentu ovaj n-torke mora biti nije veća od odgovarajuće komponente **KernelShape**. Zadana vrijednost je n-torke jednako nešto sve komponente. 
-   **Zajedničko korištenje**: (nije obavezno) definira debljina zajedničkog korištenja za svaku dimenzije u convolution. Vrijednost može biti jedna Booleova vrijednost ili n-torke Booleove vrijednosti duljine koji je broj argumenata funkcije na paket. Booleova vrijednost je proširena biti n-torke točne duljine sa svih komponenti jednako određenu vrijednost. Zadana vrijednost je n-torke koji se sastoji od svih vrijednosti True. 
-   **MapCount**: (nije obavezno) definira broj značajka karte za convolutional paket. Vrijednost može biti jedan pozitivan cijeli broj ili n-torke od duljine koji je broj argumenata funkcije na paket pozitivni cijeli brojevi. Jedan cijeli proširen je biti n-torke točne duljine prvi komponentama jednako određenu vrijednost i sve preostale komponente jednak jedan. Zadana vrijednost je jedan. Ukupan broj karte značajka je proizvod komponenti torke. Factoring ovaj ukupan broj preko komponente određuje način grupiranja značajka karte vrijednosti u čvorove odredište. 
-   **Težina**: (nije obavezno) definira početne debljine za u paket. Ta vrijednost mora biti n-torke plutajućih zareza vrijednosti duljine koji predstavlja broj puta jezgre broj debljine po otklanjanje, kako je definirano u nastavku ovog članka. Nasumično generiraju debljine zadani.  

Postoje dva skupa svojstava koja određuju udaljenosti od ruba, svojstava koja se isključuju:

-   **Popunjavaju**: (nije obavezno) Determines hoće li se unos treba popunjena pomoću **zadanu udaljenosti od ruba shemu**. Vrijednost može biti jedna Booleova vrijednost ili može biti n-torke Booleove vrijednosti duljine koji je broj argumenata funkcije na paket. Booleova vrijednost je proširena biti n-torke točne duljine sa svih komponenti jednako određenu vrijednost. Ako je vrijednost dimenzije True, izvorišnog web-mjesta logično popunjavati u dimenzije s ćelije s vrijednošću nula za podršku aplikacije dodatne otklanjanje tako da se središnje čvorove jezgre imena i prezimena u dimenzije su čvorove imena i prezimena u dimenzije u sloju izvora. Dakle, broj "sustava" čvorove u svaku dimenzije određuje se automatski, tako da stane točno _(InputShape [d] - 1) / koraka [d] + 1_ jezgre u sloju obloženi izvora. Ako je vrijednost dimenzije False, na jezgre se definiraju tako da je broj čvorove na svaku stranicu koja su izostavljeni isti (najviše razlika 1). Zadana vrijednost toga atributa je n-torke sve komponente jednako False.
-   **UpperPad** i **LowerPad**: (nije obavezno) osiguraj veću kontrolu nad količinu udaljenosti od ruba da biste koristili. **Važne:** Ti atributi može se definirati ako i samo ako je **popunjavaju** svojstvo iznad ***nije*** definiran. Vrijednosti mora biti cijeli broj vrijednosti redni s duljine koji su broj argumenata funkcije na paket. Kada se određeni su klauzulom tih atributa, "sustava" čvorove dodaju se završava donje i gornje svaku dimenzije unos sloja. Broj čvorove dodali završava donje i gornje u svaku dimenzije određen **LowerPad**[i] i **UpperPad**[i] odnosno. Da biste bili sigurni da odgovaraju jezgre samo na "realni" čvorove, a ne "sustava" čvorove, moraju biti ispunjeni sljedeći uvjeti:
    -   Svaka komponenta **LowerPad** mora biti isključivo manje KernelShape [d] / 2. 
    -   Svaka komponenta **UpperPad** mora biti nije veća od KernelShape [d] / 2. 
    -   Zadana vrijednost od ovih atributa je n-torke sve komponente jednako 0. 

Postavka **popunjavaju** = true omogućuje suvišni udaljenosti od ruba kao što je potrebno da biste zadržali "središte" Jezgra sustava unutar "realni" za unos. Time se mijenja matematičke malo za računalstvo izlazna veličina. Općenito govoreći, se izlazna veličina _D_ izračunati kao _D = (kojeg - K) / S + 1_, _pri čemu je unos veličina_ _K_ je veličina otklanjanje, _S_ je korak, i _/_ je cijeli broj s (round prema nuli). Ako ste postavili UpperPad = [1, 1], unos veličina _li_ efektivno iznosi 29, a time i _D = (29-5) / 2 + 1 = 13_. Međutim, kad **popunjavaju** = true, zapravo _mogu_ dobiti bumped _K - 1_; Dakle _D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Navođenjem vrijednosti za **UpperPad** i **LowerPad** se znatno veću kontrolu nad udaljenosti od ruba od ako upravo postavite **popunjavaju** = true.

Dodatne informacije o convolutional mreža i njihovim aplikacijama potražite u sljedećim člancima:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://people.csail.mit.edu/jvb/papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Red čekanja objedinjuje
Na **red čekanja paket** primjenjuje geometriju slično convolutional povezivanje, ali unaprijed definiranih funkcija na izvorne vrijednosti čvor koristi za dobivanje glavne čvor vrijednost odredište. Dakle, okupljanja objedinjuje imati trainable slika (debljine ili biases). Podrška za okupljanja objedinjuje sve convolutional atributa osim za **zajedničko korištenje**, **MapCount**i **debljine**.  

Obično jezgre sažeta prema susjedne okupljanja jedinica ne preklapati. Ako je korak [d] jednak KernelShape [d] u svaku dimenzije, sloj dobiven je tradicionalni lokalne okupljanja sloj, koji se često uključenih u convolutional neural mrežama. Svaki čvor odredište formula izračunava maksimum ili Srednja aktivnosti njegov otklanjanje u sloju izvora.  

Sljedeći primjer prikazuje okupljanja paket: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   Broj argumenata funkcije na paket je 3 (duljinu redni **InputShape**, **KernelShape**i **koraka**). 
-   Broj čvorove u sloju izvora _5 *24* 24 = 2880_. 
-   To je tradicionalni lokalne okupljanja sloj jer **KernelShape** i **koraka** jednaki. 
-   Broj čvorove u sloju odredište _5 *12* 12 = 1440_.  
    
Dodatne informacije o okupljanja slojeve potražite u sljedećim člancima:  

-   [http://www.CS.toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Sekcije 3.4)
-   [http://CS.nyu.edu/~koray/Publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://CS.nyu.edu/~koray/Publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Objedinjuje normalizacija odgovor
**Normalizacija odgovor** je normalizacija lokalni shemu uvedena je najprije po Geoffrey Hinton Dr, papira [ImageNet Classiﬁcation s precizno Convolutional Neural mrežama](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Normalizacija odgovor koristi da biste olakšali generalizacije u neural nets. Kada je jedan neuron aktiviraju na razini aktivaciju vrlo visoka, sloj normalizacija lokalne odgovor ukida razinu aktivaciju okolne neurons. To možete učiniti pomoću tri parametara (***α***, ***β***i ***k***) i convolutional strukture (ili susjedstvo oblika). Svaki neuron u sloju odredište ***y*** odgovara neuron ***x*** u sloju izvora. Aktivacija razinu ***y*** dobiva po sljedeću formulu, pri čemu je ***f*** razinu aktivacije programa neuron, a ***Nx*** Jezgra sustava (ili skup koji sadrži neurons u četvrti ***x***), kako je definirano parametrom convolutional sljedeću strukturu:  

![][1]  

Podrška za odgovor normalizacija objedinjuje sve convolutional atributa osim za **zajedničko korištenje**, **MapCount**i **debljine**.  
 
-   Ako Jezgra sustava sadrži neurons u istu mapu kao ***x***, sheme normalizacija se naziva **normalizacija iste mape**. Da biste definirali isti normalizacija karte, prvi koordinatnog u **InputShape** mora imati vrijednost 1.
-   Ako Jezgra sustava sadrži neurons na istom mjestu prostorno obliku ***x***, ali su u neurons u druge mape, sheme normalizacija zove se **preko normalizacija karte**. Ovu vrstu odgovora normalizacija primjenjuje oblik lateral inhibition nadahnutom po vrsti pronađeni u realne neurons stvaranje konkurencije za veliki aktivaciju razine navedenih neuron izlaze izračunati na različite mape. Da biste definirali preko normalizacija karte, prvi koordinatnog mora biti cijeli broj veći od jednog i ne veći broj karte, a ostatak koordinate mora imati vrijednost 1.  

Budući da odgovor normalizacija objedinjuje primijenite unaprijed definiranih funkcija izvorne vrijednosti čvor interpolacijom određuje vrijednost čvor odredište, imaju trainable slika (debljine ili biases).   

**Upozorenje**: čvorove u sloju odredište odgovaraju neurons koje su središnje čvorove na jezgre. Na primjer, ako je KernelShape [d] neparan, zatim _KernelShape [d] / 2_ odgovara čvor središnje otklanjanje. Ako još nije _KernelShape [d]_ , čvor središnje je na _KernelShape [d] / 2-1_. Dakle, ako je **popunjavaju**[d] False, prvi i zadnji _KernelShape [d] / 2_ čvorove nemate odgovarajuće čvorove u sloju odredište. Da biste izbjegli ovu situaciju, definirati **popunjavaju** kao [true, true,..., true].  

Uz atribute četiri ranije, odgovor normalizacija objedinjuje podržavaju sljedećim atributima:  

-   **Alfa**: (obavezno) određuje vrijednost s pomičnim zarezom koja odgovara ***α*** u prethodnoj formuli. 
-   **Beta**: (obavezno) određuje vrijednost s pomičnim zarezom koja odgovara ***β*** u prethodnoj formuli. 
-   **Pomak**: (nije obavezno) određuje vrijednost s pomičnim zarezom koja odgovara ***k*** u prethodnoj formuli. Po zadanom je odabrana 1.  

U sljedećem primjeru definira paket normalizacija odgovor, korištenje tih atributa:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   Izvor sloj sadrži pet karte, svaka s aof dimenziju 12 x 12, Zbrajanje u čvorove 1440. 
-   Vrijednost **KernelShape** označava da je isti karte normalizacija sloja, pri čemu je četvrti pravokutnik 3 x 3. 
-   Zadana vrijednost **popunjavaju** je False, sloj odredište ima samo 10 čvorove svaku dimenzije. Da biste obuhvatili jedan čvor odredište sloj koji odgovara svaki čvor u sloju izvora, dodajte udaljenosti od ruba = [true; true; true]; i promijenite veličinu RN1 [5; 12; 12].  

## <a name="share-declaration"></a>Zajedničko korištenje izvješća 
Neto # po želji podržava definiranje više objedinjuje sa zajedničkom debljine. Težina sve dva objedinjuje možete zajednički koristiti ako njihove strukture iste. Sljedeću sintaksu definira objedinjuje sa zajedničkom debljine:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Ako, na primjer, sljedeće deklariranje zajedničko korištenje navodi nazive sloj koji označava da težina i biases moraju se zajednički koristi:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   Unos značajke imaju u dva jednaka slojeve veličine unosa. 
-   Skriveni Slojevi izračunajte višu razinu značajki na dva unosa slojeve. 
-   Zajedničko korištenje izvješća određuje da _H1_ i _H2_ morate izračunati na isti način iz svoje odgovarajući unosa.  
 
Osim toga, to nije moguće navesti pomoću dvije zasebne deklaracija zajedničko korištenje na sljedeći način:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Kratki oblik možete koristiti samo kad slojeva sadrže jedan paket. Općenito govoreći, zajedničko korištenje je moguće samo kad je odgovarajući strukturu identične, što znači da imaju iste veličine, iste convolutional geometriju itd.  

## <a name="examples-of-net-usage"></a>Primjeri korištenja neto #
U ovom se odjeljku nalaze Primjeri kako možete koristiti neto # da biste dodali skrivenih slojeva, odrediti način interakcije s drugim slojeve skrivenim slojevima i sastavljanje convolutional mrežama.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Definiranje jednostavne prilagođenog neural mrežnog: "Pozdrav svijeta" primjer
U ovom primjeru jednostavne pokazuje kako stvoriti neural mreže modelu koji sadrži skriveni sloj.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Primjer ilustrira neke osnovne naredbe na sljedeći način:  

-   U prvom retku određuje unos sloj (pod nazivom _Podaci_). Kada koristite ključnu riječ **automatski** , neural mreže automatski uključuje sve značajke stupce u primjerima za unos. 
-   U drugom retku stvara skriveni sloj. Naziv _H_ je dodijeljen skriveni sloj koji ima 200 čvorove. U ovom sloja potpuno povezani s unos sloja.
-   U trećem retku definira izlaz sloj (pod nazivom _O_), koji sadrži 10 čvorove izlaz. Ako neural mreže se koristi za klasifikaciju, postoji jedan čvor izlaz po predmete. Ključne riječi **sigmoid** označava zatvara funkciju izlaz sloj izlaz.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Definiranje više skrivenih slojeva: primjer vidom računala
Sljedeći primjer pokazuje kako definirati malo složenije neural mreža, uz više prilagođenih skrivenih slojeva.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

U ovom se primjeru ilustrira nekoliko značajki specifikacije jezika neural mreže:  

-   Struktura je dva unosa slojeva, _piksela_ i _metapodacima_.
-   Sloj _pikselima_ je izvor sloja za dva objedinjuje veze, sa slojevima odredište, _ByRow_ i _ByCol_.
-   Slojevi _Prikupljanje_ i _rezultat_ su odredište slojeva više objedinjuje veze.
-   Izlaz sloja, _rezultat_je odredište sloj u dva objedinjuje veze; jedna na druge razine skrivene (prikupljanje) kao sloj odredište, a drugo s unos sloj (metapodaci) kao sloj odredište.
-   Skriveni slojevi, _ByRow_ i _ByCol_pomoću predikata izraza navede filtrirani povezivanje. Preciznije, čvor u _ByRow_ pri [x, y] je povezano s čvorove u _pikselima_ koji imaju prvi indeks koordinatnog jednako na čvor koordinate za prvu x. Isto tako, čvor u _ByCol pri [x, y] je povezano s čvorove u _pikselima_ koji imaju drugog indeksa koordiniranje unutar nešto na čvor koordinate za drugi y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Definiranje convolutional mreža za multiclass klasifikacije: primjer prepoznavanja znamenka
Definiciju sljedeće mreže namijenjen je prepoznati brojeva i prikazuju napredne postupke za prilagodbu neural mreže.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   Struktura je jedan unos sloja, _Slika_.
-   Ključna riječ **convolve** označava da su slojevi pod nazivom _Conv1_ i _Conv2_ convolutional slojeva. Svaki od tih deklaracija sloja slijedi popis atributa convolution.
-   Neto je treća skrivene sloja, _Hid3_, koji je potpuno povezan s drugi skriveni sloj, _Conv2_.
-   Izlaz sloj _znamenku_s samo treći skriveni sloj, _Hid3_. Ključna riječ **sve** označava da sloj izlaz potpuno povezani s _Hid3_.
-   Tri je broj argumenata funkcije na convolution (duljinu redni **InputShape**, **KernelShape**, **koraka**i **zajedničko korištenje**). 
-   Broj debljine po otklanjanje je _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Ili 26 * 50 = 1300_.
-   Čvorovi u svakom skriveni sloj možete izračunati na sljedeći način:
    -   **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
    -   **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
    -   **NodeCount**\[2] = (13-5) / 2 + 1 = 5. 
-   Ukupan broj čvorove moguće izračunati pomoću declared dimensionality sloja, [50, 5, 5], na sljedeći način: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Budući da je **zajedničko korištenje**[d] False samo za _d == 0_, broj jezgre _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Acknowledgements

Jezik neto # za prilagodbu arhitektura neural mreža je razvio Microsoft Shon Katzenberger (Graditelj, strojnog učenja) i Alexey Kamenev (softver inženjering, Microsoft Research). Koristi se interno za strojnog učenja projekata i aplikacije u rasponu od otkrivanje sliku da biste tekst analize. Dodatne informacije potražite u članku [Neural Nets u Azure ML - Uvod u neto #](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
