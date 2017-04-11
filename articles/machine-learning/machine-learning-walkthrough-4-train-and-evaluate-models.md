<properties
    pageTitle="Korak 4: Obuka i procjenu predvidljivu analitički modelima | Microsoft Azure"
    description="Koraku 4 od razvoju vodič predvidljivu rješenja: vlaku, rezultat i procjenu više modelima u Azure strojnog učenja Studio."
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
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Prođite kroz korake 4: Obuka i procjenu predvidljivu analitički modela

Ova tema sadrži četvrtom koraku u prikazu [razvoju rješenja predvidljivu analize u Azure strojnog učenja](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Stvaranje radnog prostora za učenje za računala](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Prijenos postojećih podataka.](machine-learning-walkthrough-2-upload-data.md)
3.  [Stvaranje nove eksperiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Obuka i analiza u modelima**
5.  [Implementacija web-usluge](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Pristup web-usluge](machine-learning-walkthrough-6-access-web-service.md)

----------

Jedna od prednosti korištenja Azure strojnog učenja Studio za stvaranje strojnog učenja modelima je mogućnost pokušajte više od jedne vrste modela po jedan pokusa i uspoređuju rezultate. Ovu vrstu početak olakšava pronalaženje najbolje rješenje za problem.

U pokusa smo razvoja u prikazu, ne možemo ćete stvoriti dvije vrste modela i usporedite rezultata njihovih bilježenja rezultata odlučiti koju algoritam želimo da biste koristili u našem konačni eksperiment.  

Postoje različite modelima smo mogli odabrati. Da biste vidjeli dostupne modela, proširite čvor **Strojnog učenja** u paleti modulu, a zatim **Pokretanje modela** i čvorove ispod nje. Svrhe ovaj eksperiment smo odabrat ćete podršku vektorski računala (SVM) i dva klase Boosted odlukama module.    

> [AZURE.TIP] Da biste lakše odlučili algoritam strojnog učenja koji najbolje odgovara određeni problem koji pokušavate riješiti, potražite u članku [Da biste odabrali algoritama za Microsoft Azure strojnog učenja](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>Obuka u modelima
Najprije postavimo modela stabla odlučivanja boosted:  

1.  Traži [Stabla odlučivanja Boosted dva klase] [ two-class-boosted-decision-tree] modul u paleti modulu i povucite ga na područje crtanja.
2.  Pronalaženje [Vlaku modela] [ train-model] modula, povucite ga na područje crtanja i povežite se izlaz modula stabla odlučivanja boosted s lijevom ulazni priključak ("Untrained modela") [Vlaku modela] [ train-model] modul.
    
    [Dva klase Boosted stabla odlučivanja] [ two-class-boosted-decision-tree] modul inicijalizira generički model i [Obuka Model] [ train-model] koristi podatke Tečajevi obuke za rad u model. 
     
3.  Povezivanje lijevom izlazna ("Dataset rezultat") lijevom [Izvršavanje skripte R] [ execute-r-script] modul udesno za unos priključak ("Dataset") [Vlaku modela] [ train-model] modul.

    > [AZURE.TIP] Ne potrebna dva ulaza i jedan izlaza [Izvršavanje skripte R] [ execute-r-script] modul za ovaj eksperiment, pa ćemo ih ostavite nepovezane. 

4.  Odaberite [Model vlaku] [ train-model] modul. U oknu **Svojstva** kliknite **pokretanje birač stupca**, odaberite **Sve vrste** na padajućem popisu u odjeljku **Dostupni stupci** , a zatim unesite "Kreditna rizika" u tekstno polje. Odaberite **Sve vrste** na padajućem popisu u odjeljku **Odabrane stupce**. Odaberite "Kreditna rizika" i kliknite gumb sa strelicom istaknuti da biste premjestili **Odabrane stupce**. 
5.  Kliknite **Spremi**.


Ovaj dio pokusa sada izgleda otprilike ovako:  

![Obuka modela][1]

Nakon toga ne možemo postaviti SVM modela.  

Prvi, malo objašnjenje SVM. Boosted odlukama rad s značajke bilo koje vrste. Međutim, jer modul SVM generira linearni klasifikatora, modelu koji stvara ima najbolje test pogrešku kada sve značajke numerički imati isti skale. Da biste pretvorili sve značajke numerički isti mjerilo, koristimo "Tanh" transformaciju ( [Normalizacija podataka] [ normalize-data] modul.) To pretvorbe naš brojeva u rasponu od [0,1]. Modul SVM categorical značajke, a zatim binarni značajke 0 i 1, Pretvara niz značajke pa ćemo ne morate ručno pretvaranje značajke niz. Osim toga, ne želimo transformirati stupac odobrenja rizika (stupac 21) – nije numerička, ali je vrijednost smo ste obuka model za predviđanje, pa ćemo morate ga ostavite samostalno.  

Da biste postavili SVM model, učinite sljedeće:

1.  Traži [Dva predmete podršku vektorski strojno] [ two-class-support-vector-machine] modul u paleti modulu i povucite ga na područje crtanja.
2.  Desnom tipkom miša kliknite [Vlaku modela] [ train-model] modul, odaberite **Kopiraj**, pa desnom tipkom miša kliknite područje crtanja i odaberite **Zalijepi**. Kopiraj u [Vlaku modela] [ train-model] modul ima isti stupac Odabir kao izvornik.
3.  Izlaz iz modula SVM povezati lijevom ulazni priključak ("Untrained modela") drugi [Vlaku modela] [ train-model] modul.
4.  Pronalaženje [Normalizacija podataka] [ normalize-data] modul i povucite ga na područje crtanja.
5.  Unos ovom modulu povezati lijevom izlaz lijevom [Izvršavanje skripte R] [ execute-r-script] modul (Imajte na umu izlazni priključak modula može biti povezani s više od jedne modul).
6.  Povezivanje lijevom izlazni priključak ("transformacije Dataset") [Normalizacija podataka] [ normalize-data] modul udesno za unos priključak ("Dataset") drugi [Vlaku modela] [ train-model] modul.
7.  U oknu **Svojstva** za [Normalizacija podataka] [ normalize-data] modula, a zatim odaberite **Tanh** za parametar **transformacije način** .
8.  Kliknite **Pokreni birač stupca**odaberite "Bez stupaca" za **Počinje sa**, odaberite **Obuhvaćanje** na prvom padajućem popisu, odaberite **Vrsta stupca** u drugi padajući popis, a treći padajućem popisu odaberite **numeričke** . Određuje se kakve sve stupce s brojčanim vrijednostima (i samo numeričke).
9.  Kliknite znak plus (+) s desne strane redak – Time se stvara redak dropdowns. Odaberite **Isključi** u prvi padajući popis, odabir **naziva stupaca** u padajućem izborniku drugi i kliknite tekstno polje, a zatim "Kreditna rizika" na popisu stupaca. Određuje da je zanemariti stupcu odobrenja rizika (potrebna da biste to učinili jer je ovaj stupac numeričkih i da bi inače transformacije).
10. Kliknite **u redu**.  


[Normalizacija podataka] [ normalize-data] modul sada je definiran za izvođenje Tanh transformaciju na brojčanih stupaca osim stupcu rizika kreditne kartice.  

Ovaj dio naš eksperimenta trebala izgledati otprilike ovako:  

![Obuka drugi modela][2]  

##<a name="score-and-evaluate-the-models"></a>Rezultat i procjenu u modelima

Koristimo testiranja podatke koji je odvojene [Razdijeljeni podaci] [ split] modul rezultatu naš obučeni modela. Ne možemo možete usporediti rezultate dva modela da biste vidjeli koje generira boljih rezultata.  

1.  Traženje u [Rezultatu Model] [ score-model] modul i povucite ga na područje crtanja.
2.  Povezivanje [Vlaku modela] [ train-model] modul koji je povezan s [Dva klase Boosted stabla odlučivanja] [ two-class-boosted-decision-tree] modul ulijevo za unos priključak [Rezultat Model] [ score-model] modul.
3.  Povezivanje desnom ulazni priključak [Rezultat Model] [ score-model] modul lijevom izlaz desnom [Izvršavanje skripte R] [ execute-r-script] modul.

    [Rezultat Model] [ score-model] modul možete sada odobrenja informacije iz testiranja podataka, pokrenite kroz model i Usporedba predviđanja model stvara sa stupcem rizika stvarni odobrenja testiranja podataka.

4.  Kopiranje i lijepljenje u [Rezultatu Model] [ score-model] modul da biste stvorili drugu kopiju ili povucite novi modul u područje crtanja.
5.  Povezivanje lijevom ulazni priključak ovaj modula model SVM (to jest, povezivanje s izlazni priključak [Vlaku modela] [ train-model] modul koji je povezan s [Dva predmete podršku vektorski strojno] [ two-class-support-vector-machine] modul).
6.  Za SVM model potrebno isti transformacije podataka test kao smo obuka podataka. Stoga kopiranje i lijepljenje [Normalizacija podataka] [ normalize-data] modul da biste stvorili drugu kopiju i povežite ga s lijevom izlaz desnom [Izvršavanje skripte R] [ execute-r-script] modul.
7.  Povezivanje desnom ulazni priključak [Rezultat Model] [ score-model] modul lijevom izlaz [Normalizacija podataka] [ normalize-data] modul.  

Da biste procijenili dva bilježenja rezultata rezultata, koristimo [Procijeniti Model] [ evaluate-model] modul.  

1.  Pronalaženje [Procijeniti Model] [ evaluate-model] modul i povucite ga na područje crtanja.
2.  Lijevi ulazni priključak povezati izlazni priključak [Rezultat Model] [ score-model] modul povezan s modelom stabla odlučivanja boosted.
3.  Povezivanje desnom ulazni priključak druge [Rezultat Model] [ score-model] modul.  

Da biste pokrenuli pokusa, kliknite gumb **POKRENI** ispod područje crtanja. Može potrajati nekoliko minuta. Tortni i 3D grafikoni pokazatelj na svakom modulu na pokazuje koja se izvodi, a potom Zelena kvačica po završetku modul. Kada sve module ima kvačicu, pokusa Završi.

Stalna trebala izgledati ovako:  

![Procjena i modela][3]

Da biste provjerili rezultate, kliknite priključak izlazne [Procijeniti Model] [ evaluate-model] modul i odaberite **Vizualiziraj**.  

[Analiza Model] [ evaluate-model] modul daje par krivulje i metrike koji omogućuju uspoređuju rezultate dva scored modela. Možete pogledati rezultate kao krivulje tekstnog okvira Operator karakteristikama (ROC), preciznost/opoziv krivulje ili Dizalica krivulje. Dodatni podaci prikazani sadrži matricu zbrku i kumulativna vrijednosti za područje u odjeljku krivulje (AUC) i druge metrike. Možete promijeniti vrijednosti praga tako da premjestite klizač ulijevo ili udesno i vidjeti kako utječe skup metriku.  

S desne strane na grafikonu kliknite **Scored dataset** ili **Scored skup podataka da biste usporedili** da biste istaknuli pridružene krivulje i da se prikazuje ispod povezane metriku. Na legendi krivulje "Testu dobije dataset" odgovara lijevom unos priključak za [Procjenu Model] [ evaluate-model] modul - u našem slučaju to je model stabla boosted odluka. "Testu dobije skup podataka da biste usporedili" odgovara desnom ulazni priključak - model SVM naš velikim slovom. Kad kliknete neku od ovih naljepnice, krivulje za model istaknuta je i prikazati odgovarajuće metriku kao što je prikazano u sljedeća grafika.  

![ROC krivulje modela][4]

Tako da Provjera te vrijednosti možete odlučiti model koji je najsličniji daje rezultate koju tražite. Možete vratiti i iteracija na vašem eksperiment tako da promijenite vrijednosti u različitim modelima. 

> [AZURE.TIP] Svaki put kada pokrenete pokusa zapis o tom iteracije zadržavaju se u povijesti pokrenuti. Možete pregledavati te iteracija i vratite na neku od njih, klikom na **PRIKAZ POVIJESTI pokrenuti** ispod područje crtanja. Možete kliknuti i **Prethodnog pokrenuti** u oknu **Svojstva** da biste se vratili odmah prethodni iteracije u jedan imate otvorene.
> 
Napravite kopiju bilo koji broj ponavljanja vaše eksperimenta tako da kliknete **SPREMI kao** ispod područje crtanja. Pomoću svojstva na pokusa **Sažetak** i **Opis** zabilježite što ste pokušali u vašem eksperiment iteracija.

>  Dodatne informacije potražite u članku [Upravljanje Poigrajte iteracija u Azure strojnog učenja Studio](machine-learning-manage-experiment-iterations.md).  


----------

**Sljedeće: [uvođenje web-usluge](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
