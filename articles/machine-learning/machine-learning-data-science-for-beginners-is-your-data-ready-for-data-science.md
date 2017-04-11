<properties
   pageTitle="Podataka spremna je za znanstvenog podataka? Izračun podataka | Microsoft Azure"
   description="Saznajte 4 kriterije za podataka spremna za znanstvenog podataka. Podaci znanosti za početnike videozapis 2 sadrži konkretni primjeri koji će pomoći s osnovnim podatkovnim procjenu."
   keywords="relevantnih podataka vrednovati podatke, pripremite podatke, kriterij podataka, Priprema podataka"
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


# <a name="is-your-data-ready-for-data-science"></a>Podataka spremna je za znanstvenog podataka?

## <a name="video-2-data-science-for-beginners-series"></a>Videozapis 2: Podataka znanosti za početnike niz

Saznajte kako analiza podataka da biste bili sigurni da je zadovolji osnovni bude spremna za znanstvenog podataka.

Da biste na najbolji mogući način niza, pogledajte ih sve. [Idite na popis videozapisa](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-is-your-data-ready-for-data-science]

## <a name="other-videos-in-this-series"></a>Ostali videozapisi u ovoj seriji

*Podaci znanosti za početnike* je kratkog uvoda znanstvenog podataka u pet kratke videozapise.

  * Videozapis 1: [Odgovore znanstvenog 5 pitanja podataka](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
  * Videozapis 2: Podataka spremna je za znanstvenog podataka?
  * Videozapis 3: [Postavite pitanje možete odgovoriti s podacima](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
  * [Predviđanje odgovor s jednostavne modelom](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) videozapis 4: *(7 min 42 sec)*
  * Videozapis 5: [Kopiranje drugim korisnicima rad da biste učinili znanstvenog podataka](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Transkript: Podataka spremna je za znanstvenog podataka?

Dobro došli u "Podataka spremna je za znanstvenog podataka?" drugi videozapis u nizu *Podataka znanosti za početnike*.  

Prije znanstvenog podataka mogu vam dati odgovore želite, morate joj neke visoke kvalitete sirovina da biste radili s. Baš kao i upućivanje kolači bolje ingredients započnete s ćete bolju krajnje.

## <a name="criteria-for-data"></a>Kriteriji za podatke

Dakle, slučaju znanstvenog podatke, postoje neki ingredients koje ćemo morate skupite.

Potrebna podatke koji su:

  * Odgovarajući
  * Povezani
  * Točan
  * Dovoljno za rad s

## <a name="is-your-data-relevant"></a>Vrijedi podataka?

Da bi se prvi ingredient - moramo relevantne podatke.

![Relevantnih podataka nasuprot nevažnih podataka – analiza podataka](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-relevant-and-irrelevant-data.png)

Pogledajte tablicu na lijevoj strani. Ne možemo ispunjeni sedam osoba izvan Meso bostonske trake, mjeri njihovoj razini ALKOHOL krvnog, prosjek batting crveno Sox njihove posljednje igre i cijena mlijeko u najbliži praktičnost spremištu.

To je sve savršeno valjane podatke. Njegova samo kvara je nije odgovarajući. Nema očite odnosa između tih brojeva. Ako se dao trenutne cijene mlijeka i batting prosjek Sox crveno, nema načina nije pogoditi Moj sadržaj ALKOHOL krvnog.

Sada pogledajte tablicu na desnoj strani. Trenutno ne možemo mjeri svaku osobu tijelo mase i Broji koliko pića oni ste vodili.  Brojevi u svakom retku su odgovarajuće međusobno. Ako se dao Moje tijelo mase i broj Margaritas koje ste vodili nije unesete Procjena na moj krvnog ALKOHOL sadržaja.

## <a name="do-you-have-connected-data"></a>Želite imati povezanih podataka?

Sljedeći ingredient je povezanih podataka.

![Povezanih podataka nasuprot prekinuta veza podataka – podaci kriterija, Priprema podataka](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-connected-vs-disconnected-data.png)

Evo nekih relevantne podatke o kvaliteti hamburgers: Roštilj temperature, patty težina i ocjena u lokalnom Hrana časopis. No obratite pozornost na to praznine u tablici s lijeve strane.

Većina skupova podataka nedostaje neke vrijednosti. Zajednička ste rupe ovako je i načina da biste ih riješili. No ako postoji previše nedostaje, podataka počinje izgledaju kao Švicarska Sir.

Pogledajte tablicu na lijevoj strani, postoji li pa količinom podataka nedostaje je teško pojavljuju s bilo koje vrste odnosa između grill temperature i patty Debljina. Ovo je primjer prekinuta veza podataka.

Tablica s desne strane kroz, pun i dovršavanje - primjera povezanih podataka.

## <a name="is-your-data-accurate"></a>Je li točne podatke?

Sljedeći ingredient potrebna je točnost. Ovo su četiri ciljnih web-mjesta koje želimo pogoditi iz prvog sa strelicama.

![Unošenju podataka nasuprot netočnih podataka – podaci kriterija](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-inaccurate-vs-accurate-data.png)

Pogledajte cilj u gornjem desnom kutu. Imamo čvrsto grupiranja udesno oko na bullseye. To, Naravno, je točne. Oddly, na jeziku podataka znanstvenog naš performanse s desne strane ciljne ispod njega i smatra točne.

Ako ste da biste mapirali out središte te strelice, prikazat će je vrlo blizu na bullseye. Strelice su proširenje svih oko cilja, da bi se smatra Neprecizna, no oni se centrirano oko bullseye, tako da se smatra točne.

Sada pogledajte cilj gornjem lijevom kutu. Ovdje naš strelice pritisnite vrlo bliže, čvrsto grupiranja. To su precizno, ali postanu netočan jer centru je način isključiti na bullseye. I, Naravno, u donjem lijevom cilj strelice su netočne i Neprecizna. U ovom archer mora dodatne vježbe.

## <a name="do-you-have-enough-data-to-work-with"></a>Imate li dovoljno podataka da biste radili s?

Na kraju, ingredient #4 – ne možemo moraju imati dovoljno podataka.

![Imate li dovoljno podataka za analizu? Analiza podataka](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-barely-enough-data.png)

Zamislite da je svaka točka podataka u tablici kao otiska programa Bojanje. Ako imate samo nekoliko njih, na Bojanje može biti vrlo mutno – je teško prepoznati što je to.

Ako dodate neke više poteza Kist, zatim svoje Bojanje pokreće da biste dobili nešto oštrija.

Ako imate dovoljno takoreći poteza, vidjet ćete toliko da neke općenite odluke. Je li neko mjesto želim posjetite? Izgleda čistog, koji izgleda kao čistu vodu – da, koji se nalazi mjesto ću na godišnji odmor.

Prilikom dodavanja više podataka, postaje jasniju sliku, a biste mogli donositi odluke detaljnije. Sada možete pogledati tri Hoteli na lijevom banke. Znate, li zaista kao što su arhitektonski značajke onog u prvom planu. Koje će ostati postoji, na treću floor.

Pomoću odgovarajuće, veze, točne podatke i dovoljno, ne možemo ste sve ingredients moramo učinite neke znanstvenog visoke kvalitete podataka.

Svakako pogledajte druge četiri videozapisa u *Podataka znanosti za početnike* iz Microsoft Azure strojnog učenja.




## <a name="next-steps"></a>Daljnji koraci

  * [Pokušajte prvi eksperimentiranje znanstvenog podataka s strojnog učenja Studio](machine-learning-create-experiment.md)
  * [Uvod u strojnog učenja na nabaviti Microsoft Azure](machine-learning-what-is-machine-learning.md)
