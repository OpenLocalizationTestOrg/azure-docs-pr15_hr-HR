<properties 
    pageTitle="Analiza performansi modela u strojnog učenja | Microsoft Azure" 
    description="U članku se objašnjava procijeniti performansama model u Azure strojnog učenja." 
    services="machine-learning"
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="bradsev;garye" />


# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Upute za procjenu performansama model u Azure strojnog učenja

U ovoj se temi objašnjava kako vrednovati performansama model u Azure strojnog učenja Studio i nudi kratko objašnjenje metriku dostupnih za ovaj zadatak. Tri uobičajeni scenariji supervised učenje predstavljanja: 

* regresije
* Binarni klasifikacija 
* multiclass klasifikacija

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Procjena performansama model jedan je od core faza u postupku znanstvenog podataka. Pokazuje kako uspješno na bilježenja rezultata (predviđanja) dataset je obučeni model. 

Azure strojnog učenja podržava modela procjenu do dva njegova glavni strojnog učenja modula: [Analiza Model] [ evaluate-model] i [Unakrsno Provjera valjanosti modela][cross-validate-model]. Ove moduli omogućuju da biste vidjeli kako će se model izvodi pomoću nekoliko mjernih podataka koji se često koriste u strojnog učenja i statističkih podataka.

##<a name="evaluation-vs-cross-validation"></a>Procjena nasuprot unakrsno provjere valjanosti##
Procjena i unakrsni provjere valjanosti načina standard za mjerenje performansama model. Oba stvaraju metriku procjenu koje možete pregledati ili usporedbu onih drugih modela.

[Analiza Model] [ evaluate-model] očekuje scored skupu podataka kao unos (ili 2 u slučaju da želite usporediti performanse 2 različitim modelima). To znači da morate uvježbati modela pomoću [Uvježbati Model] [ train-model] modul i izvršite predviđanja na neki skup podataka pomoću [Modela rezultat] [ score-model] modula, prije nego što se može vrednovati rezultate. Analizu se ovisno o scored oznake/vjerojatnosti uz true natpisa koji su izlazni [Rezultat Model] [ score-model] modul.

Umjesto toga koristite unakrsni provjere valjanosti za izvođenje broj vlaku-rezultat-evaluate operacija (10 savijanje) automatski na drugi podskupova ulaznih podataka. Unos podataka je podijelite 10 dijelove, gdje je nešto rezervirana za testiranje, a 9 za obuku. Taj proces se ponavlja 10 puta i procjenu metriku se određuje prosječna vrijednost. Omogućuje određivanje koliko će se dobro bi model generalize novi skupovima podataka. [Provjera valjanosti unakrsno Model] [ cross-validate-model] modul uzima u untrained modela i neke označene skup podataka i šalje rezultate procjenu svake od 10 savijanje, osim averaged rezultate.

U sljedećim se odjeljcima smo će Stvaranje jednostavne regresije i klasifikacija modeli i izračunate njihove performanse pomoću oba na [Vrednovanje Model] [ evaluate-model] i [Unakrsno Provjera valjanosti modela] [ cross-validate-model] module.

##<a name="evaluating-a-regression-model"></a>Procjena regresijskog modela##
Pretpostavimo želimo predviđanje u automobilu cijena korištenje nekih značajki kao što su dimenzije, Konjska snaga, engine specifikacija i tako dalje. To je problem tipičnog regresije, pri čemu je varijabla cilj (*cijena*) neprekinuti numeričku vrijednost. Ne možemo stane jednostavna linearna regresija modelu koji, dobili vrijednosti za značajke određene automobila, možete predviđanje cijena te automobila. Ovaj model regresije može se koristiti u rezultatu isti skup podataka ne možemo obučavanju članova na. Kada imamo predviđene cijene za sve u automobilima, ne možemo tako da pogledate koliko se predviđanja proizlaze iz stvarnog cijena u rezultirati performansama model. Da biste to prikazuju, u odjeljku **Spremili skupove podataka** u Azure strojnog učenja Studio koristimo na *automobili cijena podataka (Raw) dataset* dostupna.
 
###<a name="creating-the-experiment"></a>Stvaranje pokusa###
Dodajte sljedeće module radnog prostora u Azure strojnog učenja Studio:

- Područja cijena podataka (Raw)
- [Linearna regresija][linear-regression]
- [Model vlaku][train-model]
- [Rezultat modela][score-model]
- [Analiza modela][evaluate-model]


Povezivanje priključke kao što je prikazano ispod slika 1 i postavite fotokopije [Modela vlaku] [ train-model] modul *cijena*.
 
![Procjena regresijskog modela](media/machine-learning-evaluate-model-performance/1.png)

Slika 1. Procjena Model regresije.

###<a name="inspecting-the-evaluation-results"></a>Pregled rezultata za procjenu###
Nakon izvođenja pokusa, možete kliknuti priključak izlazne [Procijeniti Model] [ evaluate-model] modul i odaberite *Vizualiziraj* da biste vidjeli rezultate procjenu. Su dostupni za modele regresije metriku procjenu: *Znače apsolutne pogreške*, *Korijenski znače apsolutne pogreške*, *Relativnih apsolutne pogreške*, *Relativni kvadratne pogreške*i *Koeficijent određivanja*.

Izraz "Pogreška" Ovdje predstavlja razlika između predviđene vrijednosti i vrijednost true. Apsolutna vrijednost ili kvadrat ove razlike su obično izračunati da biste snimili ukupnu veličinu pogreške preko sve instance kao razlika između predviđene, odnosno true ako vrijednost nije negativan u nekim slučajevima. Pogreška metriku Izmjerite predvidljivu učinka regresije modela pomoću devijacija Srednja njegov predviđanja iz vrijednosti true. Niže vrijednosti pogreške znači da je model točnije u mijenjanju predviđanja. Na cjelokupan metriku pogreške 0 znači da se model savršeno pristaje podacima.

Koeficijent određivanja, koji se nazivaju i R kvadratna, ujedno standardni način mjerenje koliko će se dobro model pristaje podacima. Mogu se prikazati kao proporcije varijacije objašnjenje model. Na višu proporcije bolje je u tom slučaju, pri čemu je 1 označava savršen Prilagodi.
 
![Linearna regresija procjenu mjerenja](media/machine-learning-evaluate-model-performance/2.png)

Slika 2. Metriku procjenu linearne regresije.

###<a name="using-cross-validation"></a>Korištenje unakrsni provjere valjanosti###
Kao što je rečeno neke starije verzije, na raspolaganju vam tečaj koji se ponavljaju, bilježenje rezultata i procjene automatski pomoću [Unakrsno Provjera valjanosti modela] [ cross-validate-model] modul. U ovom slučaju morate je skup podataka, untrained modela i [Unakrsno Provjera valjanosti modela] [ cross-validate-model] modul (pogledajte slici u nastavku). Napomena prvo morate postaviti natpis stupca *cijena* u [Više Provjera valjanosti modela] [ cross-validate-model] modul, svojstva.

![Unakrsno – Provjera valjanosti modela regresije](media/machine-learning-evaluate-model-performance/3.png)

Slika 3. Unakrsno – Provjera valjanosti regresije modela.

Nakon izvođenja pokusa, možete provjeriti rezultate procjenu klikom na desnu izlazni priključak [Unakrsno Provjera valjanosti modela] [ cross-validate-model] modul. To će omogućavaju detaljni prikaz metriku iteracija (preklop) te averaged rezultati svakog metriku (slika 4).
 
![Rezultati unakrsno valjanosti modela regresije](media/machine-learning-evaluate-model-performance/4.png)

Slika 4. Rezultati provjere valjanosti unakrsno Model regresije.

##<a name="evaluating-a-binary-classification-model"></a>Procjena binarni klasifikacija modela##
U scenariju s binarni klasifikacija cilj varijabla je samo dva moguća ishoda, na primjer: {0; 1} ili {false, true}, {negativan, pozitivan}. Pretpostavimo ponudit će vam dataset odrasle zaposlenika s nekim Demografski i zapošljavanju varijable, a da vas se za predviđanje razinu dobit binarni varijabla s vrijednostima {"< = 50K", "> 50K"}. Drugim riječima, negativne predmete predstavlja zaposlenike koji bi manja od ili jednaka 50K po godini, a pozitivni predmete predstavlja svih zaposlenika. Kao scenarij regresije ćemo način uvježbati model, rezultat neke podatke i analiza rezultata. Razlika ovdje je odabir metriku formula izračunava Azure strojnog učenja i izlaza. Da biste ilustrirali razine predviđanje scenarij dobit koristit ćemo [odrasle](http://archive.ics.uci.edu/ml/datasets/Adult) skup podataka za stvaranje pokusa Azure strojnog učenja i procjenu performansama model dva predmete logistika regresije, često korištenih binarni klasifikatora.

###<a name="creating-the-experiment"></a>Stvaranje pokusa###
Dodajte sljedeće module radnog prostora u Azure strojnog učenja Studio:

- Odrasle dataset statistiku dobit binarni klasifikacija
- [Dva predmete logistika regresije][two-class-logistic-regression]
- [Model vlaku][train-model]
- [Rezultat modela][score-model]
- [Analiza modela][evaluate-model]

Povezivanje priključke kao što je prikazano ispod slika 5 i postavite fotokopije [Modela vlaku] [ train-model] modul *dobit*.

![Procjena binarni klasifikacija modela](media/machine-learning-evaluate-model-performance/5.png)

Slika 5. Procjena binarni klasifikacija modela.

###<a name="inspecting-the-evaluation-results"></a>Pregled rezultata za procjenu###
Nakon izvođenja pokusa, možete kliknuti priključak izlazne [Procijeniti Model] [ evaluate-model] modul i odaberite *Vizualiziraj* da biste vidjeli rezultate procjenu (7 slici). Su dostupni za modele binarni klasifikacija metriku procjenu: *Točnost* *preciznosti*, *opoziv*, *Rezultat F1*i *AUC*. Osim toga, modul proizvodi zbrku matrice prikazuje broj true positives, false Negacija, false positives i true Negacija kao i *ROC*, *Točnost/opoziv*i *Podignite* krivulje.

Točnost je jednostavno proporcije pravilno klasificirati instance. To je obično metriku prvi pogledate prilikom procjene na klasifikatora. Međutim, kad test podaci neizravnane (kojima najčešće instance pripadaju jednoj od klasa) ili vas više zanima performanse na neki od klasa, točnost ne zaista snimiti učinkovitosti na klasifikatora. U slučaju razine klasifikacija dobit pretpostavlja testirate na neke podatke kojima 99% instance predstavljaju osobe stjecanje manja od ili jednaka 50K po godini. Moguće je postignete 0.99 točnost procjenjivanje klasa "< = 50K" za sve instance. Na klasifikatora u tom slučaju pojavit će se za taj dobar posao cjelokupan, ali u stvari, ne uspijeva za klasifikaciju bilo high-income pojedinaca (1%) pravilno.

Zbog toga je korisno za izračunavanje dodatne metriku koja snimiti točno određene vidove izračuna. Prije prelaska u pojedinosti takve metriku, je razumjeti matrice zbrku izvođenja binarni klasifikacija. Klase oznake u skupu obuka može potrajati na samo 2 mogućih vrijednosti koji se obično Pozivamo pozitivne i negativne. Pozitivne i negativne instance programa klasifikatora predviđa pravilno nazivaju true positives (TP) i true Negacija (TN), odnosno. Isto tako, pogrešno classified instance nazivaju false positives (FP), a false Negacija (FN). Matrica zbrku je jednostavno tablica koja prikazuje broj instanci koji ulaze u svakoj od tih kategorija 4. Azure strojnog učenja automatski odlučuje koji od dva klase u skupu podataka je pozitivan predmete. Ako su klase oznake Booleove vrijednosti ili cijele brojeve, instance označen kao "true" ili "1" se dodjeljuju pozitivan predmete. Ako natpisi su nizovi, kao u slučaju dataset dobit, natpisi su sortirani su po abecedi, a prva razina odabran biti negativan predmete dok je druge razine pozitivan predmete.

![Binarni klasifikacija zbrku matrice](media/machine-learning-evaluate-model-performance/6a.png)

6 slici. Binarni klasifikacija zbrku matrice.

Vraćanje klasifikacija problem dobit će želimo zatražite od nekoliko pitanja procjenu Pridonesite razumijevanje performanse klasifikatora koristi. Vrlo prirodnim pitanje je: "iz osobe kojoj se model predviđene da biste se uz > 50 K (TP + FP) koliko su klasificirani pravilno (TP)?" Ovo pitanje možete odgovoriti tako da pogledate **preciznosti** model koji je proporcije positives koji su klasificirani pravilno: TP/(TP+FP). Drugi uobičajena pitanja je "iz svih visoke uz zaposlenika dobit > 50 k (TP + FN) koliko jeste li na klasifikatora klasifikaciju pravilno (TP)". Ovo je zapravo na **opozvati**ili true pozitivan stopa: TP/(TP+FN) na klasifikatora. Mogli biste primijetiti da postoji očite trade-off između točnost i opoziv. Na primjer, uz relativno Balansirani skup podataka, klasifikatora koji predviđa uglavnom pozitivan instance promijenile visoke opoziv, ali radije niskog preciznosti koliko negativan instanci se bi misclassified rezultira velikog broja false positives. Da biste vidjeli grafički prikaz kako ove dvije metriku razlikuju, možete kliknuti krivulje "Točnost na OPOZIV" na stranici za izlazni rezultat izvođenja (gornjem lijevom dijelu 7 slici).

![Binarni klasifikacija procjenu rezultate](media/machine-learning-evaluate-model-performance/7.png) slici 7. Binarni klasifikacija procjenu rezultata.

Drugi povezane metriku koja se često koristi je **F1 rezultat**, koji traje preciznost i opoziv u obzir. Harmonijska sredina te 2 metriku se i kao izračunati: F1 = 2 (opoziv preciznosti x) / (preciznosti + opoziv). Rezultat F1 izvrstan je način za procjenu u jedan broj, ali uvijek je dobro pogledajte točnost i opoziv zajedno da biste bolje razumjeli kako se ponaša u klasifikatora.

Osim toga, jedan provjeri true pozitivan stopa nasuprot false pozitivan stopa u krivulje **Karakteristikama operacijskom tekstnog okvira (ROC)** i odgovarajuću vrijednost **Područje u odjeljku u krivulje (AUC)** . Što bliže ovaj krivulje je u gornjem lijevom kutu, ćete bolju performanse u klasifikatora (koji je maksimiziranja true pozitivan stopa tijekom minimiziranje false pozitivan stopa). Krivulje koji su blizu dijagonalno crtanja, rezultat classifiers koji jasniji predviđanja koje su blizu slučajni guessing.

###<a name="using-cross-validation"></a>Korištenje unakrsni provjere valjanosti###
Kao primjer regresije ne možemo možete izvršiti unakrsni provjere valjanosti za više puta uvježbati, rezultat i automatski procjenu drugi podskupova podataka. Slično tome, možete koristiti [Više Provjera valjanosti modela] [ cross-validate-model] modul, modelu untrained logistika regresije i skup podataka. Natpis stupca mora biti postavljeno na *dobit* u [Više Provjera valjanosti modela] [ cross-validate-model] modul, svojstva. Nakon pokretanja pokusa i klikom na desnu izlazni priključak [Unakrsno Provjera valjanosti modela] [ cross-validate-model] modula Vidimo binarni klasifikacija metričkim vrijednosti za svaki preklop nadalje srednju vrijednost i standardnu devijaciju svake. 
 
![Unakrsno – Provjera valjanosti modela binarni klasifikacija](media/machine-learning-evaluate-model-performance/8.png)

Slika 8. Unakrsno – Provjera valjanosti binarni klasifikacija modela.

![Rezultati provjere valjanosti unakrsno binarni klasifikatora](media/machine-learning-evaluate-model-performance/9.png)

Slika 9. Rezultati provjere valjanosti unakrsno binarni klasifikatora.

##<a name="evaluating-a-multiclass-classification-model"></a>Procjena Multiclass klasifikacija modela##
U ovom eksperiment koristit ćemo popularne [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") skupu podataka koja sadrži pojavljivanja 3 različitih vrsta (klase) biljke iris. Postoje 4 značajka vrijednosti (duljina širinu sepal i duljine širina svijetlosivi) za svaku instancu. U prethodno eksperimenata nećemo obučavanju članova i testirati modelima koristeći istu skupova podataka. Ovdje, koristit ćemo [Razdijeljeni podaci] [ split] modula za stvaranje 2 podskupova podataka, uvježbati na prvi, i rezultatu i procjenu na drugi. Skup podataka Iris javno dostupna je na [UCI strojnog učenja spremište](http://archive.ics.uci.edu/ml/index.html), a mogu se preuzeti pomoću [Uvoz podataka] [ import-data] modul.

###<a name="creating-the-experiment"></a>Stvaranje pokusa###
Dodajte sljedeće module radnog prostora u Azure strojnog učenja Studio:

- [Uvoz podataka][import-data]
- [Skup stabala multiclass odluka][multiclass-decision-forest]
- [Podjela podataka][split]
- [Model vlaku][train-model]
- [Rezultat modela][score-model]
- [Analiza modela][evaluate-model]

Povezivanje priključke kao što je prikazano ispod slika 10.

Postavljanje indeks natpisa stupca [Vlaku modela] [ train-model] modul 5. Skup podataka sadrži bez retka zaglavlja, no ne možemo znate da su klase oznake u stupcu peti.

Kliknite [Uvoz podataka] o[ import-data] modul i postavite svojstvo *izvor podataka* za *Web-URL putem HTTP-a*i *URL-a* za http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Postavljanje razlomak instanci koje će se koristiti za obuku u [Razdijeljeni podaci] [ split] modul (0.7, primjerice).
 
![Procjena Multiclass klasifikatora](media/machine-learning-evaluate-model-performance/10.png)

Slika 10. Procjena Multiclass klasifikatora

###<a name="inspecting-the-evaluation-results"></a>Pregled rezultata za procjenu###
Stalna možete pokrenuti i klikom na priključak izlazne [Procijeniti]modela[evaluate-model]. Rezultati procjenu predstavljanja u obliku matrice zbrku, u tom slučaju. Matrica prikazuje stvarni i predviđene instanci za sve klase 3.
 
![Multiclass klasifikacije procjenu rezultata](media/machine-learning-evaluate-model-performance/11.png)

Slika 11. Rezultati procjenu multiclass klasifikacija.

###<a name="using-cross-validation"></a>Korištenje unakrsni provjere valjanosti###
Kao što je rečeno neke starije verzije, na raspolaganju vam tečaj koji se ponavljaju, bilježenje rezultata i procjene automatski pomoću [Unakrsno Provjera valjanosti modela] [ cross-validate-model] modul. Koji su vam potrebni skup podataka, untrained modela i [Unakrsno Provjera valjanosti modela] [ cross-validate-model] modul (pogledajte slici u nastavku). Morate ponovno postaviti fotokopije [Unakrsno Provjera valjanosti modela] [ cross-validate-model] modul (indeks stupca 5 u ovom slučaju). Kada je pokrenut pokusa, a zatim klikom na desnoj strani izlaz priključak [Unakrsno Provjera valjanosti modela][cross-validate-model], možete provjeriti metričkim vrijednosti za svaki preklop, kao i srednje vrijednosti i standardne devijacije. Metriku ovdje prikazani su slične onima binarni klasifikacija velikim slovom. Međutim, imajte na umu da u multiclass klasifikacija računalstvo positives/Negacija true i false positives/Negacija obavlja tvrtka Brojanje na temelju po predmete, kao što je bez predmete ukupni pozitivne i negativne. Na primjer, kada računalstvo preciznosti ili opoziv klase 'Iris setosa', pretpostavlja se da je pozitivan predmete i sve ostale kao negativan.
 
![Unakrsno – Provjera valjanosti modela Multiclass klasifikacija](media/machine-learning-evaluate-model-performance/12.png)

Slika 12. Unakrsno – Provjera valjanosti Multiclass klasifikacija modela.


![Rezultati unakrsno valjanosti modela Multiclass klasifikacija](media/machine-learning-evaluate-model-performance/13.png)

Slika 13. Rezultati unakrsno valjanosti modela Multiclass klasifikacija.


<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
 
