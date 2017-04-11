<properties
   pageTitle="Postavite pitanje možete odgovoriti s podacima – formulate pitanja | Microsoft Azure"
   description="Saznajte kako formulate pitanje znanstvenog podataka podataka znanosti za početnike videozapis 3. Obuhvaća usporedbu klasifikaciju i regresije pitanja."
   keywords="pitanja znanstvenog podataka, formulate pitanja, regresije pitanja, a zatim klasifikacija pitanja, s povisilicom pitanje"
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

# <a name="ask-a-question-you-can-answer-with-data"></a>Postavite pitanje možete odgovoriti s podacima

## <a name="video-3-data-science-for-beginners-series"></a>Videozapis 3: Podataka znanosti za početnike niz

Saznajte kako formulate pitanje znanstvenog podataka podataka znanosti za početnike videozapis 3. U ovom se videozapisu obuhvaća usporedbu pitanja za klasifikaciju i regresije algoritama.

Da biste na najbolji mogući način niza, pogledajte ih sve. [Idite na popis videozapisa](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>Ostali videozapisi u ovoj seriji

*Podaci znanosti za početnike* je kratkog uvoda znanstvenog podataka u pet kratke videozapise.

  * Videozapis 1: [Odgovore znanstvenog 5 pitanja podataka](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
  * Videozapis 2: [podataka spremna je za znanstvenog podataka?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
  * Videozapis 3: Postavite pitanje možete odgovoriti s podacima
  * [Predviđanje odgovor s jednostavne modelom](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) videozapis 4: *(7 min 42 sec)*
  * Videozapis 5: [Kopiranje drugim korisnicima rad da biste učinili znanstvenog podataka](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Transkript: Postavite pitanje možete odgovoriti s podacima

Dobro došli u trećem videozapis u nizu "Podataka znanosti za početnike".  

U ovome, dobit ćete nekoliko savjeta za formulating pitanje možete odgovoriti s podacima.

Više iz ovog videozapisa možete dobiti ako pogledajte videozapisi dva ranije u ovoj seriji: "znanstvenog podataka 5 pitanja može odgovarati na" i "Je podataka spremna je za znanstvenog podataka?"

## <a name="ask-a-sharp-question"></a>Postavite pitanje s povisilicom

Ne možemo ste je bila riječ o načinu podataka znanstvenog postupak korištenja imena (naziva se i kategorije ili natpisi) i brojevi za predviđanje odgovor na pitanje. Ali ne može biti samo pitanje ona mora biti u *s povisilicom pitanje.*

Nejasno pitanje nema odgovoriti u ime ili broj. Morate se s povisilicom pitanje.

Zamislite pronaći uz žarulje s genie tko truthfully odgovor bilo koja pitanja koja postavljate. Ali je mischievous genie i on će pokušate unijeti svoj odgovor kao nejasno i pregledniji kao što je on odmah možete postići pomoću. Želite prikvačiti ga prema dolje s pa airtight da mu ne može pomoći, ali reći što želite saznati pitanje.

Ako ste za zajednicu neodređenim, kao što su "Što će se dogoditi sa Moje burzovni?", može odgovoriti na genie, "cijenu će se promijeniti". To je truthful odgovor, ali nije vrlo koristan.

Ali su za zajednicu s povisilicom, kao što su "Što Moje burzovni prodajnu cijenu bit će sljedeći tjedan?", u genie ne može pomoći, ali vam određenu odgovorite, a zatim predviđanje prodajna cijena.

## <a name="examples-of-your-answer-target-data"></a>Primjeri odgovor: podaci ciljne

Kada formulate svoje pitanje, provjerite je li Primjeri odgovor u vašim podacima.

Ako je naš pitanje "Što Moje burzovni prodajnu cijenu bit će sljedeći tjedan?" zatim imamo provjerite je li podatke uključuje povijest cijena dionica.

Ako je naš pitanje "koji automobila u moj tim će uspjeti najprije?" zatim imamo provjerite je li podatke sadrži informacije o prethodno pogreške.

![Ciljne podataka – Primjeri odgovor. Formulate pitanje znanstvenog podataka.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

U ovim se primjerima odgovore nazivaju cilj. Cilj je što smo pokušavate predviđanje o točke podataka, hoće li se kategorije ili broj.

Ako nemate cilj podatke, morat ćete dobiti neke. Nećete moći odgovor na pitanje bez nje.

## <a name="reformulate-your-question"></a>Reformulate pitanje

Ponekad možete reword pitanje da biste dobili korisnijim odgovor.

Pitanje "Je taj odgovora točke podataka ili B?" predviđa kategorija (ili naziv ili natpis) nečega. Da biste odgovorili, koristimo *algoritam klasifikacija*.

Pitanje "Koliko?" ili "Koliko?" predviđa iznosa. Da biste odgovorili ga koristimo *algoritam regresije*.

Da biste vidjeli kako ćemo mogu transformirati te, pogledajmo na pitanje "koji novinsku priču je zanimljive za ovaj čitač?" Ga pita za predviđanje od jednog izbora s više mogućnosti – drugim riječima "Su ovaj odgovora ili B ili C i D?" - i koristila klasifikacija algoritam.

No možda je ovo pitanje lakše odgovor ako ga kao reword "kako zanimljive je svaki priča na ovom popisu da biste ovaj čitač?" Sada možete dati svakog članka brojčanih rezultata, a pa je lakše u članku najveće bilježenje rezultata. Ovo je rephrasing klasifikacija pitanja u pitanje regresije ili koliko?

![Reformulate svoje pitanje. Klasifikacija pitanje odnosu regresijska pitanje.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

Kako se postavljate pitanje se podsjetnik koji algoritam mogu vam dati odgovor.

Možete pronaći određene linije algoritama - slične ovima u našem primjeru priče novosti - usko su povezane. Možete reformulate pitanje da biste koristili algoritam koji omogućuje najkorisnije odgovor.

No, najvažnije, zamolite s povisilicom pitanje - pitanje na koje odgovorite s podacima. I provjerite je li imate odgovarajuće podatke na koja treba odgovoriti ga.

Ne možemo ste je bila riječ o neke osnovna načela s pitanjem pitanje možete odgovoriti s podacima.

Svakako pogledajte videozapise u "Podataka znanosti za početnike" iz Microsoft Azure strojnog učenja.


## <a name="next-steps"></a>Daljnji koraci

  * [Pokušajte prvi eksperimentiranje znanstvenog podataka s strojnog učenja Studio](machine-learning-create-experiment.md)
  * [Uvod u strojnog učenja na nabaviti Microsoft Azure](machine-learning-what-is-machine-learning.md)
