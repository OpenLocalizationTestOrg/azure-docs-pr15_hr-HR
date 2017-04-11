<properties
    pageTitle="Značajka web-mjesto inženjerska i u okvir za odabir u Azure strojnog učenja | Microsoft Azure"
    description="U članku se objašnjava Svrha odabir značajki i značajka inženjering, a nalaze Primjeri njihove uloge u postupku poboljšanje podataka od strojnog učenja."
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
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Odabir u Azure strojnog učenja i značajka inženjering

U ovoj se temi objašnjava Svrha značajke inženjerska i odabir značajki u postupku poboljšanje podataka strojnog učenja. Prikazuje što se ti procesi obuhvaćaju pomoću Primjeri nudi Azure strojnog učenja Studio.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Obuka podaci koji se koriste u strojnog učenja često mogu biti nadograđeni odabira ili izdvajanje značajki iz sirovim podacima koji se prikupljaju. Primjer engineered značajke u kontekstu učenjem kako se klasificirati slike vlastoručnog znakova je kartu bitne gustoće konstruirana iz podataka neobrađenog bitne raspodjele. Ova mapa može pridonijeti učinkovitijem neobrađenog raspodjele pronađite rubova znakove.

Značajke izgrađen i odabrane povećanje učinkovitosti obuka procesa koji se pokušava izdvojiti ključne informacije koje se nalaze u podacima. Poboljšavaju power te modela točno klasifikaciju ulaznih podataka i više robustly predviđanje rezultata koji vas zanimaju. Odabir i značajka inženjering možete kombinirati da biste za učenje što više tractable. To radi poboljšavanje i smanjiti broj značajke koje su potrebne za kalibrirali ili uvježbati modela. Matematički govoriti, značajke odabrali uvježbati model su minimalnim nezavisnu varijablu koji objašnjavaju uzorci podataka i uspješno predviđanje ishoda.

Odabir značajki i inženjering je jedan dio većeg procesa koji obično se sastoji od četiri koraka:

* Prikupljanje podataka
* Poboljšanje podataka
* Model izgradnju
* Nakon obrade

Odabir i inženjering čine koraku poboljšanja podataka strojnog učenja. Tri vrste postupak mogu se razlikuju naš svrhe:

* **Prije obradu podataka**: pokuša ovaj postupak da biste bili sigurni da je prikupljene podatke clean i dosljedni. To obuhvaća zadatke kao što su integriranje više skupova podataka, rukovanje podatke koji nedostaju, rukovanje nedosljedno podataka i vrste podataka.
* **Značajka inženjering**: pokušava ovaj postupak da biste stvorili dodatne značajke odgovarajući iz postojećih neobrađenog značajki u podacima i da biste povećali predvidljivu power algoritam učenje.
* **Odabir značajki**: ovaj postupak odabire ključa podskup izvorne značajke podataka da biste smanjili dimensionality obuka problem.

U ovoj se temi pokriva samo inženjering značajke i značajke odabir aspekata poboljšanje postupka podataka. Dodatne informacije o korak unaprijed obradu podataka potražite u članku [predobradu podataka u Azure strojnog učenja Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## <a name="creating-features-from-your-data--feature-engineering"></a>Stvaranje značajke iz podataka – značajka inženjering

Obuka podataka sastoji se od matrice sastoji od primjera (zapisa ili opažanja pohranjene u recima), od kojih svaka ima skup značajki (varijable ili polja spremljene u stupcima). Značajke naveden u eksperimentalne dizajn se očekuje da karakteriziraju uzorci podataka. Iako Mnoga polja sirovim podacima izravno u možete uvrstiti na odabrani skup značajki za uvježbati model, dodatne značajke izgrađen često moraju biti konstruirana iz značajke u sirovim podacima za stvaranje skupa podataka poboljšane obuka.

Kakvu vrstu značajke treba stvoriti da biste poboljšali skup podataka nakon obuci model? Izgrađen značajke za poboljšanje na obuka unesite informacije koje bolje uzorci podataka. Očekujete nove značajke za pružanje dodatnih informacija koje nije jasno zabilježene ili jednostavno vidljivu u skup izvornu ili postojeću značajke, ali ovaj postupak nešto nije u crteža. Zvuk i produktivni odluke često potrebno stručna znanja neke domene.

Prilikom pokretanja Azure strojnog učenja najlakše ćete ga concretely grasp postupak pomoću uzoraka u Studio strojnog učenja. Dva primjera su navedene ovdje:

* Primjer regresije ([predviđanje broja i usluge najma bicikle](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) u supervised eksperiment gdje ciljnim gube
* Primjer klasifikacija za dubinsko pretraživanje teksta pomoću [Značajke raspršivanje][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Primjer 1: Dodavanje vremenski značajke za model regresije ###

Da bismo pokazali kako inženjeringom značajke za zadatak regresije, recimo koristite na eksperiment "zahtjev predviđanje od bicikli" u Azure strojnog učenja Studio. Cilj ovaj eksperimenta je za predviđanje zahtjev za bicikle, odnosno broj bicikle i usluge najma unutar određenog mjesec dana ili sat. **Skup podataka za bicikle najam UCI** zadani skup podataka koristi se kao neobrađenog ulaznih podataka.

Tom skupu podataka temelji se na stvarnih podataka tvrtke Bikeshare veliko slovo održava najam mreže za bicikle u Washington Kontroler u Sjedinjenim Državama. Skup podataka predstavlja broj i usluge najma bicikle unutar određenog sata dnevno, iz 2011 za 2012, a sadrži 17379 redaka i stupaca 17. Skup neobrađenog značajki sadrži vremenske prognoze uvjeta (temperatura humidity, brzina vjetra) i vrstu dan (praznika ili dan u tjednu). Polje za predviđanje je **cnt**, broj koji predstavlja i usluge najma bicikle unutar određenog sata, a koji rasponu od 1 do 977.

Sastavljanje učinkovitih značajke u podacima obuka četiri regresije modela ugrađenih pomoću algoritam isti, ali s četiri skupa podataka različite obuka. Četiri skupa podataka predstavljaju isti neobrađenog ulaznih podataka, ali s rastućim brojne značajke postaviti. Te značajke grupiraju se u četiri kategorije:

1. A = vremenske prognoze + praznika + weekday + vikendi značajke za predviđene dan
2. B = broj bicikli koji su rented u svakoj od prethodnog 12 sati
3. C = broj bicikli koji su rented u svakoj od prethodnog 12 dana u istom sat
4. D = broj bicikli koji su rented u svakoj od prethodnog 12 tjedana na istom sat i na isti dan

Osim značajki skup odgovora koja već postoji u izvornom sirovim podacima, druge tri skupa značajki stvaraju se putem značajke inženjerska postupak. Značajka postavljena B snimki nedavne zahtjev za za bicikle. Značajka postavljena C snimki zahtjev za bicikle u određenom sat. Značajka postavljena D snimljeni sadržaji zahtjev za bicikle na određeni sat i određenog dana u tjednu. Svaki od četiri skupa podataka tečaj sadrži skupove odgovora, A + B, A + B + C i odgovora + B + C + D, odnosno.

U pokusa Azure strojnog učenja te četiri skupa podataka za obuku su oblikovan putem četiri grana iz skupa unaprijed obrađeni unosa podataka. Osim krajnje lijeve granu svaki od tih grana sadrži [Izvršavanje skripte R] [ execute-r-script] modul u kojem skup izvedena značajke (značajka postavlja B, C i D) odnosno konstruirana i dodaje skup uvezenih podataka. Sljedeća slika prikazuje R skripta koja se koristi da biste stvorili skup značajki B u drugom lijevom grani.

![Stvaranje skupa značajki](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

U sljedećoj su tablici navedene usporedbe rezultate performanse četiri modela. Najbolje rezultate prikazane su značajke A + B + C. Imajte na umu da stopu pogreške smanjuje klikom dodatne skupove obuhvaćeni obuka podataka. Time se provjerava naše presumption skupove B i C pružaju je dodatne bitne podatke za zadatak regresije. Dodavanje skup značajki D ne čini se da sadrže sve dodatne smanjenje stopu pogreške.

![Usporedite rezultate performansi](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Primjer 2: Stvaranje značajke u dubinsko pretraživanje teksta  

Značajka inženjering široko primijeniti u Zadaci vezani uz dubinsko pretraživanje teksta, kao što je dokument klasifikaciju i šalju analize. Ako, na primjer, kada želite klasifikaciju dokumenata u nekoliko kategorija, uobičajeni pretpostavlja se riječi ili izraze koji su uključeni u jednoj kategoriji dokumenta su manje vjerojatno pojavljivati u drugu kategoriju dokumenta. Drugim riječima, učestalost distribucije riječ ili izraz je moći karakteriziraju kategorije u drugom dokumentu. U aplikacijama za dubinsko pretraživanje teksta, značajka inženjerska postupak je potrebno da biste stvorili značajke koje se odnose na riječ ili izraz učestalosti jer pojedinačne dijelove teksta sadržaj obično služe kao ulaznih podataka.

Da biste postigli ovaj zadatak, tehnika naziva *raspršivanje značajka* primjenjuje se na učinkovito značajki proizvoljne teksta pretvoriti indeksi. Umjesto određenog indeks način funkcije primjenom raspršivanje funkcija značajkama i pomoću njihovih raspršivanje vrijednosti kao indeksi izravno zastavicom svake značajke tekst (riječi ili izraza).

U Azure strojnog učenja, postoji u [Značajku raspršivanje] [ feature-hashing] modul koji stvara te značajke riječ ili izraz. Na sljedećoj je slici prikazan primjer korištenja ovom modulu. Unos podataka skup sadrži dva stupca: ocjena adresara rasponu od 1 5 i stvarni pregled sadržaja. Ova [Značajka raspršivanje] cilj[ feature-hashing] modul je dohvaćanje nove značajke koji prikazuje učestalost ponavljanja odgovarajućih riječi ili fraze unutar određenog knjiga pregled. Da biste koristili ovu modul, morate poduzeti sljedeće korake:

1. Odaberite stupac koji sadrži unos teksta (**st.2** u ovom primjeru).
2. Postavite *Hashing bitsize* 8, što znači 2 ^ 8 = 256 stvaraju se značajke. Riječ ili izraz u okvir tekst se zatim hashed 256 indeksi. Parametar *Hashing bitsize* u rasponu od 1 do 31. Ako je parametar postavljen na veći broj, riječi ili izraza vjerojatno manje biti hashed u isti indeks.
3. Postavljanje parametara *N gramima* 2. Učestalost pojavljivanja unigrams (značajka za svaku jednu riječ) i bigrams (značajku za svaki par susjedne riječi) to dohvaća iz unos teksta. Parametar *N gramima* u rasponu od 0 do 10, koja pokazuje maksimalni broj uzastopnih riječi koje želite uvrstiti u značajku.  

![Značajka raspršivanje modul](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Sljedeća slika prikazuje izgled novih značajki.

![Značajka raspršivanje primjer](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Značajke iz podataka – odabir značajki filtriranja  ##

*Odabir značajki* je postupak koji se obično primjenjuje izgradnju skupa podataka za obuku za predvidljivu Modeliranje zadatke kao što su klasifikacija ili regresije zadataka. Cilj je da biste odabrali podskup značajke s izvornog skup podataka koji se smanjuje njegove dimenzije pomoću minimalan skup značajki za predstavljanje maksimalno povećali varijanca u podatke. U ovom podskup značajke sadrži samo značajke želite uvrstiti uvježbati modela. Odabir značajki služi u dvije glavne svrhe:

* Odabir značajki često povećava točnost klasifikacije uklanjanjem nevažnih, suvišne ili iznimno povezanog značajke.
* Odabir značajki se smanjuje broj značajki, što čini postupka osposobljavanja modela učinkovitiji. To je osobito važno za learners koji su skupi uvježbati kao što je podrška vektorski strojeva.

Iako odabir značajki takvu da biste smanjili broj značajki u skupu podataka koji se koristi za uvježbati model, ga se ne obično upućuje pojam *dimensionality smanjenja.* Značajka odabiru metode izdvojiti podskup izvorne značajke podataka bez promjene ih.  Metode smanjenja dimensionality uključivanja izgrađen značajke koje se mogu transformirati izvorne značajke i stoga ih mijenjati. Metode smanjenja dimensionality Primjeri glavni komponente analysis, Kanonski korelacije analizu i razlaganja jedinstven vrijednost.

Jedna kategorija široko primijenjena značajka odabir metode supervised kontekst je odabir filtar temelji značajki. Analizom korelacije između svake značajke i ciljni atribut ovih metoda primijeniti statističke mjere da biste dodijelili rezultat svake značajke. Značajke zatim rangiranja prema rezultatu koji možete koristiti da biste postavili praga za ažuriranje ili uklanjanje određene značajke. Statistika korištene u ovih metoda mjere Primjeri Pearson korelacije, međusobna informacija i testiranje hi-kvadrata.

Azure strojnog učenja Studio nudi module za odabir značajki. Kao što je prikazano na sljedećoj slici, te moduli sadrže [filtar temelji odabir značajki] [ filter-based-feature-selection] i [Fisher linearni Discriminant analiza][fisher-linear-discriminant-analysis].

![Primjer odabir značajki](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Na primjer, koristite [filtar temelji odabir značajki] [ filter-based-feature-selection] modula primjer dubinsko pretraživanje teksta strukturirani prethodno. Pretpostavimo da želite izraditi model regresije nakon što je skup značajki 256 stvoren pomoću [Značajke raspršivanje] [ feature-hashing] modul i da varijabla odgovor **stupcu 1** i predstavlja knjiga pregled ocjena rasponu od 1 do 5. Postavite **značajku bilježenje rezultata način** **Pearson korelacije**, **ciljnom stupcu** **stupcu 1**i **brojne značajke koje željenom** **50**. Modul za [filtar temelji odabir značajki] [ filter-based-feature-selection] daje skupa podataka koji sadrži 50 značajke zajedno s atribut ciljnom **stupcu 1**. Sljedeća slika prikazuje tijek ovaj eksperiment i ulazne parametre.

![Primjer odabir značajki](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Na sljedećoj je slici prikazan dobivene skupa podataka. Svaku značajku je na testu dobije na temelju Pearson korelacije između sa samim sobom i atribut ciljnom **stupcu 1**. Značajke s gornji rezultati čuvaju.

![Filtar temelji značajke odabir skupa podataka.](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Sljedeća slika prikazuje odgovarajuće pokazatelje odabranih značajki.

![Rezultati odabrane značajke](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Primjenom [filtar temelji odabir značajki] [ filter-based-feature-selection] modula, 50 od 256 značajke odabrane jer su najčešće značajke usklađeni s cilj varijable **stupcu 1** na temelju bilježenja rezultata način **Pearson korelacije**.

## <a name="conclusion"></a>Zaključak
Značajka inženjerska i odabir značajki su dva koraka obično izvršavaju Priprema podataka za obuku kada stvaranje modela strojnog učenja. Obično inženjering značajka primjenjuje se prvi put da biste generirali dodatne značajke, a zatim korak za odabir značajki provodi da biste uklonili nevažnih, redundantnih i vrlo povezanog značajke.

Nije uvijek nužno da biste izvršili odabir značajki inženjering ili značajku. Hoće li je potrebna ovisi o podatke koje ste ili prikupljanje, algoritam odaberete i cilj pokusa.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
