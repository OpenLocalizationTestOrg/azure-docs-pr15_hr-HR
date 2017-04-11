<properties
    pageTitle="Tumačenje modela rezultira strojnog učenja | Microsoft Azure"
    description="Kako odabrati optimalnih parametar postavljen za algoritam pomoću i vizualizacija izlaze modela rezultat."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Tumačenje modela rezultira Azure strojnog učenja

U ovoj se temi objašnjava kako vizualni prikaz i tumačenje predviđanje rezultira Azure strojnog učenja Studio. Nakon što put uvježbavan model i učiniti predviđanja nudi ("testu dobije model"), morate razumijevanje i tumačenje rezultat predviđanje.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Postoje četiri glavna vrste strojnog učenja modelima u Azure strojnog učenja:

* Klasifikacija
* Klasteriranje
* Regresije
* Sustavi recommender

Moduli koji se koriste za predviđanje pri vrhu ove modelima su:

* [Rezultat Model] [ score-model] modula za klasifikaciju i regresije
* [Dodjela klastere] [ assign-to-clusters] modul za Klasteriranje
* [Rezultat Matchbox Recommender] [ score-matchbox-recommender] za sustave preporuka

Ovaj dokument objašnjava kako protumačiti predviđanje rezultate za svako od tih module. Pregled tih module, potražite u članku [kako odabrati parametara da biste optimizirali vaše algoritama u Azure strojnog učenja](machine-learning-algorithm-parameters-optimize.md).

U ovoj se temi adrese tumačenja predviđanje, ali ne procjenu modela. Dodatne informacije o tome da biste procijenili modela potražite [u](machine-learning-evaluate-model-performance.md)članku analiza performansama model u Azure strojnog učenja.

Ako ste novi korisnik Azure strojnog učenja i potrebna pomoć pri stvaranju jednostavne eksperiment za početak rada, pročitajte članak [Stvaranje jednostavne eksperiment u Azure strojnog učenja Studio](machine-learning-create-experiment.md) u Azure strojnog učenja Studio.

## <a name="classification"></a>Klasifikacija ##
Postoje dvije potkategorijama klasifikacije problema:

* Problemi sa samo dva klase (dva predmete ili binarni klasifikacija)
* Problemi s više od dva klase (više predmete klasifikacija)

Azure strojnog učenja sadrži različite moduli radnju za svaku od ove vrste klasifikacija, no slični su postupcima za tumačenje njihove rezultate za predviđanje.

### <a name="two-class-classification"></a>Klasifikacija dva klase###
**Primjer eksperiment**

Primjer dva predmete klasifikacija problema je klasifikacija cvijeća iris. Zadatak je klasificirati iris cvijeće na temelju njihove značajke. Zadani skup podataka Iris navedeni su u Azure strojnog učenja je podskup popularne [Iris zadani skup podataka](http://en.wikipedia.org/wiki/Iris_flower_data_set) koji sadrži pojavljivanja samo dva cvijet rijeci (klase 0 i 1). Postoje četiri značajke za svaki cvijet (sepal duljina, Širina sepal, svijetlosivi duljina i širina svijetlosivi).

![Snimka zaslona iris eksperiment](./media/machine-learning-interpret-model-results/1.png)

Slika 1. IRIS dva predmete klasifikacija problem eksperiment

Pokusa izvršena da biste riješili taj problem, kao što je prikazano na slici 1. Stablo modela dva klase boosted odluka put uvježbavan je i testu dobije. Sada možete vizualizirati predviđanje rezultate iz [Modela rezultat] [ score-model] modul tako da kliknete izlazni priključak [Rezultat Model] [ score-model] modul, a zatim kliknete **Vizualiziraj**.

![Modul modela rezultat](./media/machine-learning-interpret-model-results/1_1.png)

Tako ćete prikazati bilježenja rezultata rezultate kao što je prikazano na slici 2.

![Rezultati eksperimenta iris dva predmete klasifikacija](./media/machine-learning-interpret-model-results/2.png)

Slika 2. Vizualizacija rezultat modela rezultat u dva predmete klasifikacija

**Rezultat tumačenja**

Postoji šest stupaca u tablici rezultata. Lijevi četiri stupca su četiri značajke. Desno dva stupca, testu dobije naljepnica i testu dobije vjerojatnosti su predviđanje rezultati. Stupac testu dobije vjerojatnosti prikazuje vjerojatnost koji pripada cvijeta pozitivan klase (klase 1). Na primjer, prvi broj u srednje stupca (0.028571) tamo je 0.028571 vjerojatnost prvi cvijeta pripada klase 1. U stupcu testu dobije oznake prikazuje predviđene klasa za svaki cvijet. Temelji se na testu dobije vjerojatnosti stupca. Ako je vjerojatnost scored cvijeta veći od 0,5, predviđati kao 1 za predmete. U suprotnom ga je predviđati kao predmete 0.

**Web-servisa publikacije**

Nakon rezultate predviđanje imate prepoznata i judged zvuk, pokusa moguće objavljivati kao web-servisa tako da možete uvesti u raznim programima i nazovite ga da biste dobili predviđanja predmete na bilo koji novi iris cvijet. Da biste saznali kako promijeniti obuka eksperiment u bilježenja rezultata eksperiment i objavljivanje kao web-servisa, potražite u članku [Objavljivanje web-servisa Azure strojnog učenja](machine-learning-walkthrough-5-publish-web-service.md). Ovaj postupak omogućuje bilježenja rezultata eksperiment kao što je prikazano na slici 3.

![Snimka zaslona bilježenje rezultata eksperiment](./media/machine-learning-interpret-model-results/3.png)

Slika 3. Bilježenje rezultata pokusa problem iris dva predmete klasifikacija

Sada ćete morati postaviti ulazni i izlazni za web-servisa. Unos je desno ulazni priključak modela [Rezultat][score-model], koji je cvijeta Iris značajke unos. Odabir izlaz ovisi o tome koje vas zanimaju predviđene klase (testu dobije oznaka), scored vjerojatnost ili oboje. U ovom primjeru pretpostavlja se da vas zanima i jedno i drugo. Da biste odabrali željeni izlazni rezultat stupce, [Odaberite stupce u skupu podataka] koristiti[ select-columns] modul. Kliknite [Odabir stupaca u skupu podataka][select-columns]kliknite **pokretanje birač stupca**, a zatim odaberite **Testu dobije naljepnica** i **Testu dobije vjerojatnosti**. Nakon postavljanja izlazni priključak [Odabir] stupaca u skupu podataka[ select-columns] i ponovno pokrenuti, morate biti spremni objaviti bilježenja rezultata pokusa kao web-servisa tako da kliknete **OBJAVLJIVANJE web-SERVISA**. Konačni pokusa izgleda kao slika 4.

![Stalna iris dva predmete klasifikacija](./media/machine-learning-interpret-model-results/4.png)

Slika 4. Konačni bilježenja rezultata eksperiment programa iris dva predmete klasifikacije problema

Nakon što se pokrenuti web-servisa i unesite vrijednosti pokusnu instancu neke značajke, rezultat vraća dvaju brojeva. Prvi broj scored naljepnice, a drugi scored vjerojatnosti. Ovaj cvijet predviđene je kao 1 klase 0.9655 vjerojatnost.

![Testiranje tumačenje rezultat modela](./media/machine-learning-interpret-model-results/4_1.png)

![Bilježenje rezultata rezultata testa](./media/machine-learning-interpret-model-results/5.png)

Slika 5. Rezultat servisa web iris dva predmete klasifikacija

### <a name="multi-class-classification"></a>Klasifikacija više klase
**Primjer eksperiment**

U ovom eksperiment izvođenje zadatka slovo prepoznavanja kao primjer multiclass klasifikacije. Na klasifikatora pokušava predviđanje određenim slovom (klasa) na temelju neke vrijednosti rukom napisanih atribut dobivenih iz slike rukom napisanih.

![Primjer prepoznavanja pismo](./media/machine-learning-interpret-model-results/5_1.png)

U podacima obuka postoje 16 značajke dobivenih iz rukom napisanih slovo slike. 26 slova obrasca naš 26 klase. Slika 6 prikazuje pokusa koji će uvježbati multiclass klasifikacija modela za prepoznavanje slovo a predviđanje isti skup na skupu podataka testiranje značajki.

![Slovo prepoznavanja multiclass klasifikacije eksperiment](./media/machine-learning-interpret-model-results/6.png)

6 slici. Slovo prepoznavanja multiclass klasifikacija problem eksperiment

Vizualizacija rezultate iz [Modela rezultat] [ score-model] modul tako da kliknete izlazni priključak modela [Rezultat] [ score-model] modul i potom **Vizualiziraj**, trebali biste vidjeti sadržaj, kao što je prikazano na slici 7.

![Rezultat modela rezultata](./media/machine-learning-interpret-model-results/7.png)

Slika 7. Vizualizacija rezultat modela rezultira klasifikacija više klase

**Rezultat tumačenja**

Lijevi 16 stupci predstavljaju vrijednosti značajka skupa test. Stupci s nazivima kao što su testu dobije vjerojatnosti za predmete "XX" su samo kao što su stupcu testu dobije vjerojatnosti dva predmete velikim slovom. Prikazuju vjerojatnost koja se nalazi odgovarajuću stavku u određenim predmete. Na primjer, za prvu stavku postoji 0.003571 vjerojatnost da je u "A" 0.000451 vjerojatnost da je "B" i tako dalje. Zadnji stupac (testu dobije natpisi) je isti kao testu dobije natpise u slučaju dva predmete. Odabire predmete najveću scored vjerojatnost kao predviđene klase odgovarajuće stavke. Ako, na primjer, za prvu stavku oznaku scored je "F" jer on ima najveći vjerojatnost da je u "F" (0.916995).

**Web-servisa publikacije**

Za svaki unos i vjerojatnost oznaku scored možete dobiti i scored natpis. Osnovni logike je da biste pronašli najveću vjerojatnost među scored vjerojatnosti. Da biste to učinili, morate [Izvršiti skripte R] [ execute-r-script] modul. Kod R prikazuju se u slika 8, a rezultat pokusa prikazuju se u slici 9.

![Primjer koda R](./media/machine-learning-interpret-model-results/8.png)

Slika 8. R kod za izdvajanje testu dobije naljepnica i povezane vjerojatnosti naljepnica

![Rezultat eksperiment](./media/machine-learning-interpret-model-results/9.png)

Slika 9. Konačni bilježenja rezultata eksperiment slovo prepoznavanja multiclass klasifikacija problema

Nakon objavljivanja i pokrenuti web-servisa, a zatim unesite neke značajke za unos vrijednosti vraćeni rezultat izgleda kao što je slika 10. Ovo rukom napisanih pismo, njegov izdvojene 16 značajke je predviđati biti "T" s 0.9715 vjerojatnosti.

![Testiranje tumačenje modul rezultat](./media/machine-learning-interpret-model-results/9_1.png)

![Testiranje rezultata](./media/machine-learning-interpret-model-results/10.png)

Slika 10. Rezultat servisa web multiclass klasifikacija

## <a name="regression"></a>Regresije

Problemi regresije razlikuju se od klasifikacije problema. U problema s klasifikacija pokušavate predviđanje samostalni klase, kao što su koji klase programa cvijet iris pripada. No, kao što možete vidjeti u sljedećem primjeru problema regresije, pokušavate predviđanje neprekinuti varijablu, kao što je cijena automobila.

**Primjer eksperiment**

Korištenje područja cijena predviđanje kao na primjer za regresije. Pokušavate predviđanje cijena automobila na temelju njegove značajkama, uključujući stvaranjem, vrsta goriva, vrsta tijela i kotač pogon. Stalna prikazan je slika 11.

![Eksperiment regresije područja cijena](./media/machine-learning-interpret-model-results/11.png)

Slika 11. Područja cijena regresije problem eksperiment

Vizualizacija [Rezultat Model] [ score-model] modula, rezultat izgleda kao slika 12.

![Bilježenje rezultata rezultati za problem predviđanje područja cijena](./media/machine-learning-interpret-model-results/12.png)

Slika 12. Bilježenje rezultata rezultat predviđanje problema područja cijena

**Rezultat tumačenja**

Natpisi testu dobije je rezultat stupac u taj rezultat bilježenja rezultata. Predviđeni cijenu za svaku automobil su brojevi.

**Web-servisa publikacije**

Možete objaviti pokusa regresije u web-servisa i poziva za kupnju cijena predviđanje na isti način kao koristi slučaj klasifikacija dva predmete.

![Bilježenje rezultata eksperiment za kupnju cijena regresije problema](./media/machine-learning-interpret-model-results/13.png)

Slika 13. Bilježenje rezultata eksperiment regresije problema s područja cijena

Izvodi web-servisa, vraćeni rezultat izgleda kao slika 14. Predviđena cijena za ovaj automobil je $15,085.52.

![Testiranje tumačenje bilježenja rezultata modul](./media/machine-learning-interpret-model-results/13_1.png)

![Bilježenje rezultata modul rezultata](./media/machine-learning-interpret-model-results/14.png)

Slika 14. Rezultat servisa web regresije problema s područja cijena

## <a name="clustering"></a>Klasteriranje

**Primjer eksperiment**

Pogledajmo skup podataka Iris ponovno koristiti da biste sastavili klasteriranja eksperiment. Ovdje možete filtrirati out klase oznake u skupu podataka da bi samo je značajke i može se koristiti za Klasteriranje. U ovom iris slučaj upotrebe, navedite broj klastere se dva tijekom postupka obuka, što znači da bi skupine cvijeće u dva klase. Stalna prikazan je 15 slici.

![Isprobajte ih IRIS klasteriranja problema](./media/machine-learning-interpret-model-results/15.png)

Slika 15. Isprobajte ih IRIS klasteriranja problema

Klasteriranje razlikuje se od klasifikacija iz tog obuka zadani skup podataka ne sadrži natpise dna istine samostalno. Obuka instance skupa podataka u distinct klastere klasteriranja grupe. Tijekom postupka obuka model prikazana stavke po učenje razlike između njihove značajke. Nakon toga obučeni model može se koristiti za daljnje klasifikaciju buduće stavke. Postoje dva dijela rezultat ćemo vas zanimaju unutar klasteriranja problem. Prvi dio je označavanje obuka zadani skup podataka, a drugi je klasifikaciju novi skup podataka s obučeni modelom.

Prvi dio rezultata koje se mogu vizualizirati klikom na lijevom izlazni priključak modela [Klasteriranje vlaku] [ train-clustering-model] , a zatim kliknete **Vizualiziraj**. 16 slici prikazan je vizualizaciju.

![Klasteriranje rezultata](./media/machine-learning-interpret-model-results/16.png)

Slika 16. Vizualizacija Klasteriranje rezultat za skup podataka za osposobljavanje

Rezultat drugi dio Klasteriranje nove stavke s obučeni klasteriranja modelom prikazan je 17 slici.

![Vizualizacija Klasteriranje rezultata](./media/machine-learning-interpret-model-results/17.png)

Slika 17. Vizualizacija Klasteriranje rezultat na novi skup podataka

**Rezultat tumačenja**

Iako rezultate od dva dijela skrati iz različitih eksperiment faze, i im je izgledaju interpretiraju na isti način. Prva četiri su značajke. Zadnji stupac, dodjelu je rezultat predviđanje. Stavke dodjeljuje isti broj predviđene su se u istu klasteru, odnosno dijele sličnosti na mobitel (ovaj eksperiment koristi metriku zadane Euclidean udaljenost). Budući da navedete broj klastere da je 2, 0 ili 1 označeni su stavke u dodjele.

**Web-servisa publikacije**

Možete objaviti klasteriranja pokusa u web-servisa i poziva za Klasteriranje predviđanja slučaj upotrebe na isti način kao klasifikacija dva predmete.

![Bilježenje rezultata eksperiment iris klasteriranja problema](./media/machine-learning-interpret-model-results/18.png)

18 slici. Bilježenje rezultata eksperiment klasteriranja problema s iris

Nakon pokretanja web-servisa vraćeni rezultat izgleda kao slika 19. Ovaj cvijet predviđati je u klaster 0.

![Testiranje tumačenje bilježenja rezultata modul](./media/machine-learning-interpret-model-results/18_1.png)

![Bilježenje rezultata modul rezultata](./media/machine-learning-interpret-model-results/19.png)

Slika 19. Rezultat servisa web iris dva predmete klasifikacija

## <a name="recommender-system"></a>Recommender sustava
**Primjer eksperiment**

Za sustave recommender problem preporuke restoran možete koristiti kao primjer: restorani dobiti preporuku za korisnike koji se temelji na povijest ocjena. Unos podataka sastoji se od tri dijela:

* Ocjene restoran klijenata
* Značajka podatke o klijentu
* Restoran značajka podataka

Nekoliko je stvari koje ćemo pruža [Vlaku Matchbox Recommender] [ train-matchbox-recommender] modul u Azure strojnog učenja:

- Predviđanje ocjene za dani korisnika i stavke
- Preporučujemo da stavke na određenom korisnika
- Traženje korisnika koji se odnose na određene korisnika
- Traženje stavki koje su povezane s određenom stavkom

Možete odabrati što želite učiniti tako da odaberete neku od četiri mogućnosti u izborniku **Recommender predviđanje vrstu** . Ovdje možete voditi kroz sve četiri slučaja.

![Matchbox recommender](./media/machine-learning-interpret-model-results/19_1.png)

Uobičajeni eksperiment Azure strojnog učenja za sustav recommender izgleda kao slika 20. Informacije o korištenju tih module recommender sustava potražite u članku [vlaku matchbox recommender] [ train-matchbox-recommender] i [rezultat matchbox recommender][score-matchbox-recommender].

![Eksperiment sustava recommender](./media/machine-learning-interpret-model-results/20.png)

20 slici. Eksperiment sustava recommender

**Rezultat tumačenja**

**Predviđanje ocjene za dani korisnika i stavke**

Odabirom **Ocjena predviđanje** u odjeljku **Vrsta predviđanje Recommender**postavljate recommender sustava za predviđanje ocjena navedeni korisnika i stavke. Vizualizacija [Rezultat Matchbox Recommender] [ score-matchbox-recommender] izlaz izgleda kao slika 21.

![Rezultat rezultat sustava recommender – ocjena predviđanje](./media/machine-learning-interpret-model-results/21.png)

Slika 21. Vizualizacija rezultat rezultat sustava recommender – ocjena predviđanje

Prva dva stupca su parove korisnika stavke koje ste dobili od ulaznih podataka. Treći stupac sadrži predviđene ocjenama korisnika za određene stavke. Ako, na primjer, u prvom retku kupca U1048 je predviđati stopa restoranu 135026 kao 2.

**Preporučujemo da stavke na određenom korisnika**

Odabirom **Preporuke stavke** u odjeljku **Vrsta predviđanje Recommender**tražite sustava recommender kojom se preporučuje stavke određenom korisniku. Zadnji parametar da biste odabrali u tom slučaju nije *preporučeno stavke odabira*. Mogućnost **Iz Ocjenjivanje stavke (za procjenu model)** je prvenstveno za procjenu modela tijekom postupka obuku. Za tu fazu predviđanje smo odaberite **Iz svih stavki**. Vizualizacija [Rezultat Matchbox Recommender] [ score-matchbox-recommender] izlaz izgleda kao slika 22.

![Rezultat rezultat recommender sustava – stavke preporuka](./media/machine-learning-interpret-model-results/22.png)

Slika 22. Vizualizacija rezultat rezultat recommender sustava – stavke preporuka

Prvi šest stupaca predstavlja navedeni korisnički ID-ovi kojom se preporučuje stavki, koje ste dobili od ulaznih podataka. Na pet stupaca predstavljaju stavke preporučuje se korisniku silaznim redoslijedom relevantnosti. Ako, na primjer, u prvom retku najčešće preporučene restoran za klijenta U1048 je 134986, nakon čega slijedi 135018, 134975, 135021 i 132862.

**Traženje korisnika koji se odnose na određene korisnika**

Odabirom **Povezane korisnika** u odjeljku **Vrsta predviđanje Recommender**tražite recommender sustava da biste pronašli povezane korisnici određenom korisniku. Srodni korisnici su korisnicima koji imaju slične Preferences (Preference). Zadnji parametar za odabir u ovom scenariju je *Odabir povezane korisnika*. Mogućnost **Iz korisnicima da ocjenjuju stavke (za procjenu model)** je prvenstveno za procjenu modela tijekom postupka obuku. Odaberite **Iz svih korisnika** za tu fazu predviđanje. Vizualizacija [Rezultat Matchbox Recommender] [ score-matchbox-recommender] izlaz izgleda kao slika 23.

![Rezultat rezultat recommender sustava – povezane korisnika](./media/machine-learning-interpret-model-results/23.png)

Slika 23. Vizualni prikaz rezultata rezultat recommender sustava – povezane korisnika

Prvi šest stupaca prikazuje dani korisnički ID-a potrebno da biste pronašli povezane korisnika, koje ste dobili od ulaznih podataka. Na pet stupaca pohranite Predviđeni korisnici povezane korisnika silaznim redoslijedom važnosti. Ako, na primjer, u prvom retku najrelevantnije klijent za klijenta U1048 je U1051, nakon čega slijedi U1066, U1044, U1017 i U1072.

**Traženje stavki koje su povezane s određenom stavkom**

Odabirom **Povezane stavke** u odjeljku **Vrsta predviđanje Recommender**postavljate recommender sustava da biste pronašli povezane stavke na određenom stavku. Povezane stavke su stavke koje su najvjerojatnije sviđa mi po korisniku. Zadnji parametar da biste odabrali u tom slučaju nije *povezana stavka odabira*. Mogućnost **Iz Ocjenjivanje stavke (za procjenu model)** je prvenstveno za procjenu modela tijekom postupka obuku. Odaberite **Iz sve stavke** za tu fazu predviđanje. Vizualizacija [Rezultat Matchbox Recommender] [ score-matchbox-recommender] izlaz izgleda kao slika 24.

![Rezultat rezultat recommender sustava – povezane stavke](./media/machine-learning-interpret-model-results/24.png)

Slika 24. Vizualni prikaz rezultata rezultat recommender sustava – povezane stavke

Prvi šest stupaca predstavlja danog artikla ID-ove potreban da biste pronašli povezane stavke koje ste dobili od ulaznih podataka. Na pet stupaca pohranite predviđene povezane stavke stavke silaznim redoslijedom prema važnosti. Ako, na primjer, u prvom retku najrelevantnije stavke za stavku 135026 je 135074, nakon čega slijedi 135035, 132875, 135055 i 134992.

**Web-servisa publikacije**

Postupak objavljivanja te eksperimenata kao web-servisi da biste dobili predviđanja je sličan za svaku od četiri slučaja. U nastavku ćemo proći drugi scenarij (preporučeno stavke na određenom korisniku) kao primjer. Slijedite isti postupak s drugim tri.

Spremanje u sustavu obučeni recommender kao obučeni modela i filtriranja ulaznih podataka u stupcu jednog korisničkog ID-a kao što ste tražili, možete priključiti pokusa kao slika 25 i objavljivanje kao web-servisa.

![Bilježenje rezultata eksperiment problema restoran preporuka](./media/machine-learning-interpret-model-results/25.png)

Slika 25. Bilježenje rezultata eksperiment problema restoran preporuka

Izvodi web-servisa, vraćeni rezultat izgleda kao slika 26. Pet preporučeni restorani za korisnika U1048 su 134986, 135018, 134975, 135021 i 132862.

![Primjer recommender sistemski servis](./media/machine-learning-interpret-model-results/25_1.png)

![Ogledni rezultati eksperiment](./media/machine-learning-interpret-model-results/26.png)

Slika 26. Rezultat servisa web restoran preporuke problema


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
