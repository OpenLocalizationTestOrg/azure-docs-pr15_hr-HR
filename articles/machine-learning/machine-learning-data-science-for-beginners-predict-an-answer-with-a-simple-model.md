<properties
   pageTitle="Predviđanje odgovor s jednostavne modelom - model regresije | Microsoft Azure"
   description="Upute za stvaranje jednostavne regresije modela za predviđanje cijena u podataka znanosti za početnike videozapis 4. Obuhvaća linearna regresija podacima cilj."                                  
   keywords="Stvaranje modela, jednostavno model, cijena predviđanje, model jednostavne regresije"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="predict-an-answer-with-a-simple-model"></a>Predviđanje odgovor s jednostavne modelom

## <a name="video-4-data-science-for-beginners-series"></a>Videozapis 4: Podataka znanosti za početnike niz

Saznajte kako stvoriti jednostavan regresije model za predviđanje cijena romb u podataka znanosti za početnike videozapis 4. Ne možemo ćete nacrtajte model regresije ciljnih podataka.

Da biste na najbolji mogući način niza, pogledajte ih sve. [Idite na popis videozapisa](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-predict-an-answer-with-a-simple-model]

## <a name="other-videos-in-this-series"></a>Ostali videozapisi u ovoj seriji

*Podaci znanosti za početnike* je kratkog uvoda znanstvenog podataka u pet kratke videozapise.

  * Videozapis 1: [Odgovore znanstvenog 5 pitanja podataka](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
  * Videozapis 2: [podataka spremna je za znanstvenog podataka?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
  * Videozapis 3: [Postavite pitanje možete odgovoriti s podacima](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
  * Videozapis 4: Predviđanje odgovor s jednostavne modelom
  * Videozapis 5: [Kopiranje drugim korisnicima rad da biste učinili znanstvenog podataka](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Transkript: Predviđanje odgovor s jednostavne modelom

Dobro došli u četvrtom videozapisa u na "podataka znanosti za početnike" niz. U ovome, ne možemo ćete Stvaranje jednostavne modela i provjerite na predviđanje.

*Model* je pojednostavljeni priče o oglednim podacima. Li vam pokazati li značenje.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Prikupi odgovarajući točne, veze, dovoljno podataka

Ne želim pogon za romba. Imam Nazovi koji pokazali Moje NO@LOC postavku za 1.35 carat romb i želim da biste dobili ideju o tome koliko stoji. Poduzeti Blok za pisanje i olovka u spremište jewelry pa se zapišite cijenu svih Rombovi u slučaju i koliko ih obzir uzeli i u carats. Počevši od prvog romb - njegova 1.01 carats i $7,366.

Sada mogu proći i učiniti za sve druge Rombovi u spremištu.

![Stupci romb podataka](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Obratite pozornost na to da našem popisu ima dva stupca. Svaki stupac ima različite atribut - težinu u carats, cijena - a svaki redak je pojedinačnu točku podataka koji predstavlja jednu romb.

Ne možemo zapravo ste stvorili mali podataka postavljena ovdje – tablice. Obratite pozornost na to ispunjava li naš kriterije za kvalitete:

* Podaci se **odgovarajući** - Debljina sigurno povezan popusta
* Nije **točan** – ne možemo double-checked cijene koje ćemo zapišite
* Je **povezano** - postoje bez razmaka u bilo koju od tih stupaca
* I kao ćemo vidjeti, ono je **dovoljno** podataka da biste odgovorili naš pitanje

## <a name="ask-a-sharp-question"></a>Postavite pitanje s povisilicom

Sad ćemo će predstavljati naš pitanje s povisilicom način: "koliko to košta za kupnju 1.35 carat romb?"

Našem popisu nema 1.35 carat romb, pa imamo pomoću ostale podatke možete dobiti odgovor na pitanje.

## <a name="plot-the-existing-data"></a>Iscrtavanje postojećih podataka

Najprije ćemo učiniti je povucite vodoravnu crtu broj, pod nazivom osi, na grafikonu u debljine. Raspon u debljine je 0 do 2, pa ne možemo ćete Crtanje crte koji prekriva koja raspon i postavili crtice na osi za svaki pola carat.

Sljedeće smo nacrtajte okomite osi da biste snimili cijenu i povežite ga s Debljina vodoravne osi. Ta će se u jedinicama dolarima. Sad imamo skup koordinata osi.

![Težina i cijena osi](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Ne možemo ćete sada poduzeti te podatke i pretvoriti u *raspršenom crtanja*. To je odličan način vizualizacija numeričke skupa podataka.

Za prvu točku podataka, ne možemo eyeball okomitu crtu pri 1.01 carats. Zatim ćemo eyeball vodoravne crte na $7,366. Gdje se ne odgovara, ne možemo nacrtajte točka. Time se povećava naš prvi romb.

Sada ćemo proći kroz svaki romb na ovom popisu i na isti način. Kada se Ispričavamo se kroz, to je što smo dobili: skup točke, jedan za svaki romb.

![Raspršeni crtanja](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Crtanje modela kroz točke podataka

Sada ako pogledate točkice i squint zbirke izgleda fat mutno crte. Ne možemo možete koristiti naše oznake a nacrtali ravnu crtu kroz nju.

Crtanjem crte koju smo stvorili u *modelu*. Smatrajte ovo neobično stvarnog svijeta i unese simplistic strip verziju ga. Sada na strip Kriva – u retku ne prolaze kroz sve točke podataka. Ali je korisno simplification.

![Linearne regresije.](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Fact koje sve točke ne prolaze točno redak je u redu. Fizičari podataka objašnjavaju to tako da postoji model – koji se nalazi u retku -, a zatim svake točke sadrži neke *Šum* ili *varijancu* pridružen. Temeljni savršen odnos, a zatim postoji gritty, realni svijeta koji dodaje Šum i nesigurnosti.

Budući da Pokušavamo odgovarati na pitanja *koliko?* ta mogućnost naziva *regresije*. Te jer smo koristite ravnu crtu, *linearne regresije*.

## <a name="use-the-model-to-find-the-answer"></a>Korištenje modela da biste pronašli odgovor

Sada imamo model, a zatim ga tražimo naš pitanje: koliko 1.35 carat romb košta?

Da biste odgovorili naš pitanje, ne možemo eyeball 1.35 carats i nacrtajte okomitu crtu. Gdje siječe redak modela smo eyeball vodoravne crte dolar osi. Ga dodirne nadohvat 10 000. Boom! To je odgovor: 1.35 carat romb troškove oko 10 000 USD.

![Pronalaženje odgovora na model](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Stvaranje interval pouzdanosti

Prirodno pitate kako precizno je ovo predviđanje je. Je korisno je znati je li romb 1.35 carat će biti vrlo blizu 10 000 USD, i mnogo više ili niže. Da biste to izgleda, stavljanje na omotnicu oko regresije koja obuhvaća veći dio točke. Ovaj omotnica zove naš *interval pouzdanosti*: Ispričavamo se vrlo sigurni da se cijene nalaziti unutar ograničenja omotnica, jer u zadnjih najviše od njih. Ne možemo možete crtati dva više vodoravne crte s gdje u retku 1.35 carat siječe na vrhu i dnu omotnice.

![Interval pouzdanosti](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Sad ćemo izgovorite nešto o naš interval pouzdanosti: ne možemo izgovorite Povjerljivo cijenu romba 1.35 carat je otprilike $ 10 000, no možda je manje, kao $8,000 te možda najviša $12,000.

## <a name="were-done-with-no-math-or-computers"></a>Ne možemo završetku bez matematičkih i računala

Ne možemo jeste li koje fizičari podataka plaćenu zatražite da biste učinili, a ćemo je niste baš crtanjem:

* Ne možemo od vas zatraži pitanje smo može odgovoriti s podacima
* Ne možemo ugrađen u *modelu* pomoću *linearna regresija*
* Ćemo proizvela *predviđanje*, ispunjen *interval pouzdanosti*

A niste koristimo matematičkih i računalima to učiniti.

Sada ćemo imali imali više informacija, li...

* Izrezivanje romba
* varijacije boja (kako Zatvori romba je u tijeku bijela)
* broj uvrštene u romba

.. zatim ćemo bi imali više stupaca. U tom slučaju matematičke postaje korisno. Ako imate više od dva stupca, je teško da biste nacrtali točke na papiru. Matematičke operacije možete vrlo radi boljeg smještaj retka ili taj ravnini s podacima.

Osim toga, ako je umjesto samo handful Rombovi, ne možemo imali dvije tisuće ili dva milijuna, a zatim koji funkcioniraju možete učiniti mnogo brže s računalom.

Danas, ne možemo ste je bila riječ o tome linearne regresije, a zatim ćemo proizvela predviđanje korištenja podataka.

Svakako pogledajte videozapise u "Podataka znanosti za početnike" iz Microsoft Azure strojnog učenja.



## <a name="next-steps"></a>Daljnji koraci

  * [Pokušajte prvi eksperimentiranje znanstvenog podataka s strojnog učenja Studio](machine-learning-create-experiment.md)
  * [Uvod u strojnog učenja na nabaviti Microsoft Azure](machine-learning-what-is-machine-learning.md)
