<properties
    pageTitle="Značajka izbor u postupku timu podataka znanstvenog | Microsoft Azure" 
    description="U članku se objašnjava Svrha odabir značajki i nalaze Primjeri njihove uloge u postupku poboljšanja podataka od strojnog učenja."
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
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Odabir značajki u u timu podataka znanstvenog procesa (TDSP)

U ovom se članku objašnjava svrhe odabir značajki i nalaze Primjeri njegova uloga u podataka poboljšanje postupka strojnog učenja. U ovim se primjerima se crtaju od Azure strojnog učenja Studio. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


U ovoj se temi objašnjava Svrha odabir značajki i nalaze Primjeri njegova uloga u podataka poboljšanje postupka strojnog učenja. U ovim se primjerima se crtaju od Azure strojnog učenja Studio. 

Odabir značajki i inženjering je jedan dio TDSP navedene u na [postupak znanstvenog timu podataka?](data-science-process-overview.md). Odabir i značajka inženjering su dijelovi **razvoju značajke** koraku u TDSP.

* **značajka inženjering**: pokušava ovaj postupak da biste stvorili dodatne značajke odgovarajući iz postojećih neobrađenog značajki u podacima, a da biste povećali predvidljivu power algoritam učenje.

* **Odabir značajki**: ovaj postupak odabire ključa podskup izvorne značajkama u pokušaj da biste smanjili dimensionality obuka problem.

Obično **inženjering značajka** primjenjuje se prvi put da biste generirali dodatne značajke i tada se izvodi **Odabir značajki** korak da biste uklonili nevažnih, redundantnih i vrlo povezanog značajke.


## <a name="filtering-features-from-your-data---feature-selection"></a>Filtriranje značajke iz podataka – odabir značajki 

Odabir značajki je postupak koji se obično primjenjuje za izgradnju skupova podataka za obuku za predvidljivu Modeliranje zadatke kao što su klasifikacija ili regresije zadataka. Cilj je da biste odabrali podskup značajke s izvornog skup podataka koje smanjuju njegove dimenzije pomoću minimalan skup značajki za predstavljanje maksimalno povećali varijanca u podatke. U ovom podskup značajke su, pa samo značajke želite obuhvatiti uvježbati modela. Odabir značajki služi u dvije glavne svrhe.

* Najprije odabir značajki često povećava točnost klasifikacija uklanjanjem nevažnih, suvišne ili iznimno povezanog značajke.
* Drugo, smanjuje broj značajke čega postupak obuka modela učinkovitiji. To je osobito važno za learners koji su skupi uvježbati kao što je podrška vektorski strojeva.

Iako odabir značajki rješenja da biste smanjili broj značajke u skupu podataka za uvježbati model, ona ne obično naziva se prema pojmu "dimensionality smanjenja". Značajka odabiru metode izdvojiti podskup izvorne značajke podataka bez promjene ih.  Metode smanjenja dimensionality uključivanja izgrađen značajke koje se mogu transformirati izvorne značajke i stoga ih mijenjati. Metode smanjenja dimensionality Primjeri glavnicu komponente Analysis, Kanonski korelacije analizu i jedinstven razlaganja vrijednost.

Između ostalog, jedna kategorija široko primijenjena značajka odabir metode supervised kontekst se zove "Odabir značajki filtra temeljenog". Analizom korelacije između svake značajke i ciljni atribut ovih metoda primijeniti statističke mjere da biste dodijelili rezultat svake značajke. Značajke zatim rangiranja prema rezultatu koji se mogu koristiti za pomoć za postavljanje praga za ažuriranje ili uklanjanje određene značajke. Statistika korištene u ovih metoda mjere Primjeri korelacije, međusobna informacije i testiranje kvadrata k osobe.

U Azure strojnog učenja Studio postoje module za odabir značajki. Kao što je prikazano na sljedećoj slici, te moduli sadrže [filtar temelji odabir značajki] [ filter-based-feature-selection] i [Fisher linearni Discriminant analiza][fisher-linear-discriminant-analysis].

![Primjer odabir značajki](./media/machine-learning-data-science-select-features/feature-Selection.png)


Razmislite o, na primjer, koristite [filtar temelji odabir značajki] [ filter-based-feature-selection] modul. Radi praktičnosti ne možemo nastaviti koristiti tekstni primjer dubinsko pretraživanje strukturirani iznad. Pretpostavimo želimo da biste sastavili model regresije nakon stvaranja skup značajki 256 putem [Značajke raspršivanje] [ feature-hashing] modul i varijabla odgovor je stupcu "1", a predstavlja knjiga pregled ocjene u rasponu od 1 do 5. Postavljanjem "Značajke bilježenja rezultata metode" biti "Pearson korelacije", "ciljnom stupcu" stupcu "1" i "Broj željene značajke" do 50. Zatim modul [filtar temelji odabir značajki] [ filter-based-feature-selection] će stvoriti skup podataka koji sadrži 50 značajke zajedno s atribut ciljnom "stupcu 1". Sljedeća slika prikazuje tijek ovaj eksperiment i ulazne parametre smo samo opisane.

![Primjer odabir značajki](./media/machine-learning-data-science-select-features/feature-Selection1.png)

Sljedeća slika prikazuje rezultat skupova podataka. Svaku značajku je na testu dobije na temelju Pearson korelacije između sa samim sobom i atribut ciljnom "stupcu 1". Značajke s gornji rezultati čuvaju.

![Primjer odabir značajki](./media/machine-learning-data-science-select-features/feature-Selection2.png)

Odgovarajući rezultati odabranih značajki prikazane su na sljedećoj slici.

![Primjer odabir značajki](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Primjenom [filtar temelji odabir značajki] [ filter-based-feature-selection] modula, 50 od 256 značajke odabrane jer su značajke najčešće povezanog s varijablom ciljnom stupcu "1", na temelju metodu bilježenja rezultata "Pearson korelacije".

## <a name="conclusion"></a>Zaključak
Značajka inženjerska i odabir značajki su dvije najčešće izgrađen odabrane značajke povećanje učinkovitosti obuka procesa koji se prilikom pokušaja da biste izdvojili ključne informacije koje se nalaze u podatke. Poboljšavaju power te modela točno klasifikaciju ulaznih podataka i više robustly predviđanje rezultata koji vas zanimaju. Odabir i značajka inženjering možete kombinirati da biste za učenje što više tractable. To radi poboljšavanje i smanjiti broj značajke koje su potrebne za kalibrirali ili uvježbati modela. Matematički govoriti, značajke odabrali uvježbati model su minimalnim nezavisnu varijablu koji objašnjavaju uzorci podataka i uspješno predviđanje ishoda.

Imajte na umu da nije uvijek nužno da biste izvršili odabir značajki inženjering ili značajku. Hoće li je potreban ili ne ovisi o podataka ne možemo imate ili prikupljanje, algoritam smo odaberite i cilj pokusa.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 
