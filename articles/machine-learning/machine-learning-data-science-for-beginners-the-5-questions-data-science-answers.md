<properties
   pageTitle="Pitanja znanstvenog 5 podataka – podaci znanosti za početnike | Microsoft Azure"
   description="Pronađite kratkog uvoda znanstvenog podataka iz podataka znanosti za početnike pet kratke videozapise koji započinju na 5 pitanja podataka znanstvenog odgovore."
   keywords="način znanstvenog podataka, početnike znanstvenog podataka, a zatim znanstvenog podataka za početnike, vrste pitanja, podataka znanstvenog pitanja, a zatim videozapis znanstvenog podataka"
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

# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Podaci znanosti za početnike videozapis 1: U 5 pitanja odgovore znanstvenog podataka

Pogledajte Uvod znanstvenog podataka iz *Podataka znanosti za početnike* u pet kratke videozapise zatražite od gornji podataka pozvana. Ove videozapise u kojima su osnovni, ali koristan, bilo koja vas zanima način znanstvenog podataka ili radite s fizičari podataka.

Ovaj prvi je videozapis vrste pitanja na koja se može odgovarati na znanstvenog podataka. Da biste na najbolji mogući način niza, pogledajte ih sve. [Idite na popis videozapisa](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-the-5-questions-data-science-answers]

## <a name="other-videos-in-this-series"></a>Ostali videozapisi u ovoj seriji

*Podaci znanosti za početnike* je kratkog uvoda podataka znanstvenog poduzimanja ukupni oko 25 minuta. Pogledajte druge četiri videozapisa:

  * Videozapis 1: 5 pitanja odgovore znanstvenog podataka
  * Videozapis 2: [podataka spremna je za znanstvenog podataka?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
  * Videozapis 3: [Postavite pitanje možete odgovoriti s podacima](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
  * [Predviđanje odgovor s jednostavne modelom](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) videozapis 4: *(7 min 42 sec)*
  * Videozapis 5: [Kopiranje drugim korisnicima rad da biste učinili znanstvenog podataka](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-the-5-questions-data-science-answers"></a>Transkript: 5 pitanja odgovore znanstvenog podataka

Najviša! Dobro došli u videozapis niza *Podataka znanosti za početnike*.

Znanstvena podataka može biti intimidating, tako da mogu ćete predstavljanje u osnove bez jednadžbe i računalo programiranje žargona.

U ovom ćete prvi videozapisu ćemo objasniti što "5 pitanja odgovore znanstvenog podataka."

Znanstvena podataka koristi brojevima i nazivima (poznat i kao kategorije ili natpisi) za predviđanje odgovore na pitanja.

To može, surprise ali *postoje samo pet pitanja te odgovore znanstvenog podataka*:

  * Je li ovo A i B?
  * Je li to weird?
  * Koliko – ili – koliko?
  * Kako to organizirano?
  * Što dalje?

  Svaki od njih ta pitanja odgovor po zasebnom obitelj načine učenje računalu, pod nazivom algoritama.


Ovo je razmisliti o algoritam kao s receptima i podataka kao u ingredients. Algoritam objašnjava kako se kombinirati, a zatim izmiješajte podatke da biste dobili rezultat. Kao što je stapanje su računala. Većina napravljenoga algoritam služe za vas, a mogu stvoriti vrlo brzo.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Pitanje 1: Je ovaj A i B? koristi algoritama klasifikacija

Započnimo s pitanje: je ovaj A i B?

![Klasifikacija algoritama: je ovaj A i B?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-classification-algorithms.png)

Ovaj obitelji algoritama zove klasifikacija dva predmete.

Ovo je korisno za bilo koja pitanja koja sadrži samo dva moguća odgovora.

Ako, na primjer:

  * Neće uspjeti ovaj guma u sljedeći 1000 miljama: da ili ne?
  * Koji se unosi u više korisnika: $5 kupon ili popust 25%?

Ovo pitanje može biti rephrased i da biste dodali više od dvije mogućnosti: ovaj A i B ili C ili D, itd.?  To se naziva multiclass klasifikacije i korisno ako imate više – ili nekoliko thousand – moguća odgovora. Multiclass klasifikacija odabire najvjerojatnije jedan.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Pitanje 2: Je to weird? koristi značajkom prepoznavanja algoritama

Sljedeće pitanje znanstvenog podataka može odgovarati je: je ovaj čudnog? U ovom se odgovara na pitanje obitelji algoritama naziva značajkom prepoznavanja.

![Značajkom prepoznavanja algoritama: je ovaj čudnog?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-anomaly-detection-algorithms.png)


Ako imate kreditnom karticom, koje ste već benefitted sa značajkom prepoznavanja. Vaša tvrtka kreditnom karticom analizira uzorci na kupnju tako da ih možete upozorava vas na moguće prijevara. Troškovi "weird" mogu kupiti u trgovini gdje ste ne obično Kupovina ili kupnjom neuobičajeno pricey stavke.

Ovo pitanje može biti korisno na mnogo načina. Na primjer:

  * Ako imate automobil s tlaka gauges, možda ćete morati znati: je ovaj mjerača tlaka normalno za čitanje?
  * Ako ste nadzor s Internetom, trebali znati: karakteristično je ovu poruku putem Interneta?

Otkrivanje značajkom zastavicama događaje neočekivane ili nepoznatu ili ponašanje. Pruža clues gdje treba provjeriti postoje li problemi.



## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Pitanje 3: Koliko? ili kako mnogo? koristi regresije algoritama

Učenje računala možete i mnogo predviđanje odgovor na način? ili kako mnogo? Algoritam obitelji koja odgovara ovo pitanje zove regresije.

![Algoritmi regresije: koliko? ili kako mnogo?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-regression-algorithms.png)


Algoritmi regresije provjerite numeričke predviđanja, kao što su:

  * Što je temperatura bit će sljedeći utorak?  
  * Što će Moje četvrti kvartal Prodaja?

Oni omogućuju dobiti odgovor na pitanje sve sa zahtjevom za broj.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Pitanje 4: Kako to organizirano? koristi Klasteriranje algoritama

Sada zadnje dvije pitanja su bitne dodatne mogućnosti.

Katkad je potrebno razumjeti strukturu skupa podataka – to način organiziranja? Za ovo pitanje nemate primjeri koji koje već poznajete ishoda za.

Postoji mnogo načina tease out strukturu podataka. Klasteriranje je jedan pristup. Za lakše tumačenja ga dijeli podataka u prirodnim "clumps,". S Klasteriranje, postoji jedan odgovor desno.

![Klasteriranje algoritama: to način organiziranja?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-clustering-algorithms.png)

Nekoliko uobičajenih primjera klasteriranja pitanja:

  * Koje uređaje li iste vrste filmova?
  * Koji se modeli pisača neće funkcionirati na isti način kao?

Po objašnjenje kako su podaci organizirani, možete bolje razumjeli - i predviđanje - ponašanja i događaje.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Pitanje 5: Što sada? koristi reinforcement učenje algoritama

Zadnja pitanje – što sada? – koristi obitelji algoritama naziva učenje reinforcement.

Učenje reinforcement je inspired tako kako brains rats i ljudi odgovoriti punishment i nagrade od. Tim algoritmima Saznajte iz ishoda i odlučite na sljedeću akciju.

Obično učenje reinforcement je dobro rješenje za automatiziranog sustavima koji imaju da biste mnogo small odluke bez Ljudski upute.

![Reinforcement učenje algoritama: što dalje?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-reinforcement-learning-algorithms.png)

Pitanja je odgovore uvijek su što treba uzeti – obično stroj ili stroj. Primjeri su:

  * Jeste li sustav temperatura kontrole za kuće: Prilagodba temperature ili ga ostavite gdje se nalazi?  
  * Ako se sama vožnju automobila: na žutoj svjetlo kočnica ili accelerate?  
  * Za robot usisavanje: zadržavanje vacuuming ili se vratite na punjenja mjesto?

Algoritmi učenje reinforcement Prikupite podatke kao one se, učenje iz probne verzije i pogreške.

Da bi to je to – u 5 pitanja podataka znanstvenog možete odgovoriti.



## <a name="next-steps"></a>Daljnji koraci

  * [Pokušajte prvi eksperimentiranje znanstvenog podataka s strojnog učenja Studio](machine-learning-create-experiment.md)
  * [Uvod u strojnog učenja na nabaviti Microsoft Azure](machine-learning-what-is-machine-learning.md)
