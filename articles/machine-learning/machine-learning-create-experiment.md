<properties
    pageTitle="Jednostavni eksperimentiraj u stroj učenje Studio | Microsoft Azure"
    description="Ovaj vodič učenje stroj vas vodi kroz pokusa znanosti lako podataka. Možemo ćete predviđanje cijena automobil pomoću algoritam regresije."
    keywords="Eksperimentirajte linearne regresije, algoritmi učenje strojne grupe, stroj učenje vodič, predvidljivih modeliranja tehnike, eksperimentiraj znanosti podataka"
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
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Stroj učenje vodič: Stvaranje prvog eksperimentiraj podataka znanosti Azure stroj učenje Studio

Ovaj vodič učenje stroj vas vodi kroz pokusa znanosti lako podataka. Možemo ćete stvoriti linearne regresije model koji predviđa cijena automobili na temelju različite varijable kao što bi i tehničke specifikacije. Da biste to učinili, koristimo ćete Azure stroj učenje Studio razviti i iterirati na jednostavan predvidljivih analytics eksperimentirati.

*Predvidljivih analytics* je vrsta podataka znanosti koja koristi trenutne podatke za predviđanje buduće ishodi. Vrlo jednostavnom primjer predvidljivih analytics gledati podatke znanosti za početnike video 4: [predviđanje odgovora s jednostavan model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (runtime: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Kako se stroj učenje Studio ne pomoći?

Stroj učenje Studio olakšava postavljanje pokusa korištenjem Vuci i ispusti module preprogrammed predvidljivih modeliranja tehnike. Kako biste pokrenuli vaš eksperimentiraj i predviđanje odgovor ćete koristiti stroj učenje Studio za *Stvaranje model*, *Podučavanje model*i *rezultat i testiranje modela*.

Unesite Studio učenje stroja: [https://studio.azureml.net](https://studio.azureml.net). Ako ste prijavljeni na računalu učenje Studio prije, kliknite **ovdje prijava**. U suprotnom, kliknite **Potpiši gore** i odaberite između mogućnosti slobodnog i plaćenu.

Za više općenitih informacija o računalu učenje Studio pogledajte [što je stroj učenje Studio?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Pet korake za stvaranje pokusa

U ovom računalu učenje vodič ćete pratiti pet osnovnih koraka za izgradnju pokusa stroj učenje Studio za stvaranje, uvježbavanje i rezultatu vašeg modela:

- Stvaranje model
    - [Korak 1: Dohvati podatke]
    - [Korak 2: Preprocess podataka]
    - [Korak 3: Definiranje značajke]
- Uvježbavanje modela
    - [Korak 4: Odaberite i primijenite algoritam za učenje]
- Rezultat i testiranje modela
    - [Korak 5: Predviđanje nove podignete cijene]

[Korak 1: Dohvati podatke]: #step-1-get-data
[Korak 2: Preprocess podataka]: #step-2-preprocess-data
[Korak 3: Definiranje značajke]: #step-3-define-features
[Korak 4: Odaberite i primijenite algoritam za učenje]: #step-4-choose-and-apply-a-learning-algorithm
[Korak 5: Predviđanje nove podignete cijene]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>Korak 1: Dohvati podatke

Nema broj skupovima podataka uzorak uključene Studio učenje strojne grupe koje možete odabrati i uvesti podatke iz mnogo različitih izvora. U ovom primjeru koristimo će uključeni uzorak dataset **automobili cijena podataka (Raw)**.
Ovaj dataset uključuje stavke za broj pojedinačnih automobiles, uključujući informacije učini, modela, tehničke specifikacije i cijena.

1. Pokrenite novu eksperimentiraj pritiskom **+ NOVO** na dnu prozora Studio učenje strojne grupe, odaberite **EKSPERIMENTIRATI**i odaberite **Prazno eksperimentirati**. Odaberite zadani eksperimentiraj naziv na vrhu područja crtanja i preimenujte nešto smislen, možete, na primjer, **predviđanje cijena automobili**.

2. Lijevo pokusa područje crtanja je palete skupovima podataka i modulima. U okvir za pretraživanje na vrhu palete za pronalaženje dataset s natpisom **automobili cijena podataka ((Raw))**upišite **automobili** .

    ![Pretraživanje palete][screen1a]

3. Povucite u dataset eksperimentiraj područje crtanja.

    ![DataSet][screen1]

Da biste vidjeli izgleda ove podatke, kliknite izlazni priključak na dnu podignete dataset i odaberite **Vizualiziraj**.

![Modul izlazni priključak][screen1c]

Varijable u skup podataka pojavljuju se kao stupci i svaka instanca programa automobili pojavljuje se kao redak. Sasvim desno stupac (stupac 26 i s naslovom "cijena") je varijabla ciljne smo namjeravate pokušajte predvidjeti.

![DataSet vizualizacije][screen1b]

Zatvorite prozor vizualizaciju klikom na "**x**" u gornjem desnom kutu.

## <a name="step-2-preprocess-data"></a>Korak 2: Preprocess podataka

Dataset obično zahtijeva neke pretprocesnih prije nego ga mogu analizirati. Možda ste primijetili nedostaje vrijednosti prisutan u stupcima različite retke. Te vrijednosti nedostaje potrebno očistiti tako model možete analizirati podatke ispravno. U našem slučaju smo ćete ukloniti retke koji imaju vrijednosti nedostaju. Također, stupac **normaliziranim gubitaka** ima velike razmjer nedostaje vrijednosti tako smo ćete isključiti taj stupac iz modela sasvim.

> [AZURE.TIP] Čišćenje nedostaje vrijednosti iz ulaznih podataka je preduvjeta za korištenje većinu module.

Prvo ćete ukloniti stupac **normaliziranim gubitke** , a zatim ćete ukloniti redak koji ima podatke koji nedostaju.

1. U okvir za pretraživanje na vrhu palete modul za pronalaženje [Odaberite stupce u Dataset] upišite **Odaberite stupce** [ select-columns] modul, a zatim povucite ga pokusa platno i povežite ga s izlazni priključak dataset **automobili cijena podataka (Raw)** . Taj modul omogućuje nam odaberite koje stupce podataka smo želite uključiti ili isključiti u modelu.

2. Odaberite [Odabir stupaca u Dataset] [ select-columns] modul i pritisnite **pokretanje birač stupca** u oknu **Svojstva** .

    - S lijeve strane kliknite **s pravilima**
    - Pod **Počinje s**, pritisnite **sve stupce**. To nalaže [Odaberite stupce u Dataset] [ select-columns] da prođe kroz sve stupce (osim onih nas za isključivanje).
    - Iz padajuće izbornike odaberite **Isključi** i **nazive stupaca**, a zatim kliknite unutar tekstualnog okvira. Prikazuje se popis stupaca. Odaberite **normaliziranim negativne**i će dodati tekstni okvir.
    - Kliknite gumb potvrđeno (u redu) da biste zatvorili birač stupca.

    ![Odaberite stupce][screen3]

    Svojstva okno za **Odabir stupaca u Dataset** označava da ga proći kroz sve stupce iz dataset osim **normaliziranim gubitke**.

    ![Odaberite stupce u svojstvima Dataset][screen4]

    > [AZURE.TIP] Možete dodati komentar modul dvoklikom modul i unos teksta. To može pomoći u pogledom vidjeti što modul radi u vašem eksperimentiraj. U tom slučaju, dvokliknite [Odaberite stupce u Dataset] [ select-columns] modul i vrsta komentara "Isključi normaliziranim gubitke."

3. Povucite [Čistu podataka nedostaje] [ clean-missing-data] modul pokusa platno i povežite ga s [Odaberite stupce u Dataset] [ select-columns] modula. U oknu **Svojstva** odaberite **Ukloni cijeli redak** pod **Čišćenje načinu** očistiti podatke uklanjanjem retke koji imaju vrijednosti nedostaju. Dvokliknite modul i upišite komentar "Uklanjanje redaka nedostaje vrijednost".

    ![CLEAN svojstva nedostaju podataka][screen4a]

4. Pokrenite pokusa pritiskom **pokrenuti** pod eksperimentiraj područje crtanja.

Po završetku pokusa sve module imaju zelenu kvačicu da biste naznačili da oni uspješno je završeno. Također primijetite status **Dovršeno izvodi** u gornjem desnom kutu.

![Prvi eksperimentiraj pokrenuti][screen5]

Svi smo ste učinili u eksperimentiraj ove točke je očistiti podatke. Ako želite pogledati očišćene dataset kliknite lijevi izlazni priključak [Čistu podataka nedostaje] [ clean-missing-data] modul ("Cleaned dataset") i odaberite **Vizualiziraj**. Primijetite stupac **normaliziranim gubitaka** više nije uključena i nema nedostaje vrijednosti.

Sada bi se podaci čistu, smo spremni da biste odredili koje značajke smo namjeravate koristiti u predvidljivih modela.

## <a name="step-3-define-features"></a>Korak 3: Definiranje značajke

U računalu učenje *značajke* su pojedinačne mjerljivim svojstva nešto vas zanima. U našem dataset svaki redak predstavlja jedan automobili i svaki stupac je značajka taj automobili.

Pronalaženje i dobre skup značajki za stvaranje predvidljivih modela zahtijeva experimentation i znanja o problemu želite riješiti. Neke značajke su bolji za predviđanje cilj od drugih. Također, neke značajke imaju neprobojne korelacije s drugim značajkama (primjerice, Grad-mpg usporedbi highway mpg), tako da oni ne dodati model informacija koliko nove i može se ukloniti.

Pogledajmo izgradite model koji koristi podskup značajki u našem dataset. Možete dolaze natrag i odaberite različite značajke, ponovno pokrenite pokusa i vidjeli možete li dobiti bolje rezultate. No da biste započeli, Pokušajmo sljedeće značajke:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Povucite drugi [Odabir stupaca u Dataset] [ select-columns] modul pokusa platno i povežite ga s lijevom izlazni priključak [Čistu podataka nedostaje] [ clean-missing-data] modula. Dvokliknite modul i upišite "Odabir značajki za predviđanje."

2. Pritisnite **birač stupca za pokretanje** u oknu **Svojstva** .

3. Kliknite **s pravilima**.

4. Pod **Počinje s** kliknite **nema stupaca**, a zatim odaberite **Uključi** i **nazivi stupaca** u retku filtar. Unesite naš popis naziva stupca. Upućuje modul da prođe kroz samo stupce koje smo odredili.

    > [AZURE.TIP] Pokretanjem pokusa smo ste ensured da definicije stupaca za naše podatke prenesite iz skup podataka do [Čisto podataka nedostaje] [ clean-missing-data] modula. To znači da ostalim modulima povežete će imati informacije iz skupa podataka.

5. Kliknite gumb potvrđeno (u redu).

![Odaberite stupce][screen6]

Daje dataset koji će se koristiti u algoritam učenje u sljedećim koracima. Kasnije, vratite se i pokušajte ponovno s različitim odabira značajke.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Korak 4: Odaberite i primijenite algoritam za učenje

Sada da podatke spreman, izgradnji predvidljivih modela sastoji od osposobljavanje i testiranja. Koristimo ćete naše podataka za uvježbavanje model i testiranje modela da biste vidjeli koliki je moći predviđanje cijene. Zasad nemojte brinuti o Zašto moramo za uvježbavanje i testiranje model.

*Klasifikacija* i *regresije* su dvije vrste supervised stroj učenje tehnike. Klasifikacija predviđa odgovora iz definirani skup kategorije, primjerice boja (crvena, plavi ili zelena). Regresije koristi se za predviđanje broja.

Jer smo želite predvidjeti cijena koji je broj, koristimo ćete model regresije. Na primjer, možemo ćete uvježbati jednostavna *linearna regresija* modela i u sljedećem koraku smo ćete ga testirati.

1. Naše podatke koristimo za uvježbavanje i testiranje razdvajanjem u zasebnom osposobljavanje i testiranja skupova. Odaberite i povucite [Podjele podataka] [ split] modul pokusa platno i povežite ga s izlaza posljednji [Odabir stupaca u Dataset] [ select-columns] modula. Postavite **razlomak redaka u prvom dataset izlaza** 0,75. Ovaj način smo ćete koristiti 75 posto podataka za uvježbavanje model i držite 25 posto za testiranje.

    > [AZURE.TIP] Promjenom parametar **slučajno Klijanje** proizvesti različite slučajni uzorke za uvježbavanje i testiranja. Ovaj parametar kontrole seeding generator pseudo-slučajni broj.

2. Pokrenite pokusa. To omogućuje [Odabir stupaca u Dataset] [ select-columns] i [Podjele podataka] [ split] moduli za prosljeđivanje definicije stupaca module smo ćete dodavanjem dalje.  

3. Odaberite algoritam učenje, proširite kategoriju **Učenje stroj** u paleti modul lijevo područje crtanja, a zatim **Inicijalizirati modela**. Prikazuje nekoliko kategorija module koji mogu koristiti inicijalizirati algoritmi učenje strojne grupe.

    Ovaj eksperimentiraj odaberite [Linearne regresije] [ linear-regression] modul pod **regresijska** kategorija (možete pronaći modul upisivanjem "linearne regresije" u okvir za pretraživanje palete) i povucite eksperimentiraj područje crtanja.

4. Pronalaženje i povucite [Podučavanje Model] [ train-model] modul eksperimentiraj područje crtanja. Povezivanje lijevom ulazni priključak izlaza [Linearne regresije] [ linear-regression] modula. Povezivanje desno ulazni priključak izlaza osposobljavanje podataka (lijevi priključak) [Podjele podataka] [ split] modula.

5. Odaberite [Model Podučavanje] [ train-model] modula, pritisnite **pokretanje birač stupca** u oknu **Svojstva** , a zatim odaberite stupca **cijena** . To je vrijednost koja naše model namjeravate predviđanje.

    ![Odaberite stupac "cijena"][screen7]

6. Pokrenite pokusa.

Rezultat je obučeni regresije model koji se može koristiti za nove uzorke da bi predictions rezultatu.

![Primjena učenje algoritam strojne grupe][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>Korak 5: Predviđanje nove podignete cijene

Sada kad smo ste put uvježbavan modela pomoću 75 posto naše podataka, možemo možete ga koristiti za rezultat drugih 25 posto podataka da biste vidjeli koliko dobro naše funkcije modela.

1. Pronalaženje i povučete [Rezultatu Model] [ score-model] modul pokusa platno i povezati lijevom ulazni priključak izlaza [Podučavanje Model] [ train-model] modula. Povezivanje desno ulazni priključak testiranje podataka izlaz (desno priključak) [Podjele podataka] [ split] modula.  

    ![Rezultat Model modul][screen8a]

2. Pokrenite pokusa i prikaz izlaza iz [Modela rezultat] [ score-model] modul, kliknite izlazni priključak, a zatim odaberite **Vizualiziraj**. Izlaz prikazuje predviđene vrijednosti za cijenu i poznate vrijednosti iz testiranje podataka.  

3. Konačno, da biste testirali kvalitete rezultate, odaberite i povucite [Procijeniti Model] [ evaluate-model] modul pokusa platno i povezati lijevom ulazni priključak izlazni [Rezultat Model] [ score-model] modula. (Postoje dva ulaznih priključaka jer [Procijeniti Model] [ evaluate-model] modula mogu se usporediti dva modela.)

4. Pokrenite pokusa.

Za prikaz izlaza iz [Procijeniti Model] [ evaluate-model] modul, kliknite izlazni priključak, a zatim odaberite **Vizualiziraj**. Sljedećih Statistika su prikazani za naše modela:

- **Znači apsolutni pogreška** (MAE): prosjek apsolutnih pogreške ( *pogreška* je razlika između predviđene vrijednosti i stvarno vrijednost).
- **Korijenski Mean kvadratu pogreška** (RMSE): kvadratni korijen prosjek kvadrata pogreške predictions napravili test dataset.
- **Relativni apsolutni pogreška**: prosjek apsolutnih pogrešaka odnosu apsolutni razlika između stvarnih vrijednosti i prosjeka svih stvarnih vrijednosti.
- **Relativni kvadratu pogreška**: prosjek kvadrata pogreške odnosu kvadrata razlika između stvarnih vrijednosti i prosjeka svih stvarnih vrijednosti.
- **Koeficijent determinacije**: poznate kao **R kvadratnu vrijednost**, to je statističke metriku koja ukazuje koliko dobro model pristaje podacima.

Za svaku pogrešku bolje je manje Statistika. Manju vrijednost naznačuje u predictions pobliže odgovaraju stvarnim vrijednostima. Za **Koeficijent determinacije**što bliže je njegova vrijednost jedan (1.0) je bolja predictions.

![Procjena rezultata][screen9]

Konačni eksperimentiraj trebao bi izgledati ovako:

![Vodič za učenje stroj: dovršetak eksperimentiraj linearne regresije koji koristi predvidljivih modeliranja tehnike.][screen10]

## <a name="next-steps"></a>Sljedeći koraci

Sada kad dovršetka prvog vodič učenje stroj i imaju svoje eksperimentiraj postavili mogu iterirati pokušajte da biste poboljšali modela. Na primjer, možete promijeniti značajke koje koristite u vašem predviđanje. Izmijenite svojstva [Linearne regresije] ili[ linear-regression] algoritam ili pak zabranjujete pokušajte drugu algoritam. Čak možete dodati više učenje algoritmi vaše eksperimentiraj istovremeno stroj i usporediti dvije pomoću [Procijeniti Model] [ evaluate-model] modula.

> [AZURE.TIP] Kopiranje svih iteracija vaše eksperimentiraj koristite gumb **SPREMI kao** pod eksperimentiraj područje crtanja. Iteracija vaše eksperimentiraj možete vidjeti pritiskom na **PRIKAZ POVIJESTI pokrenuti** pod područje crtanja. Pogledajte [Upravljanje eksperimentirati iteracija Studio učenje stroj Azure] [ runhistory] za više detalja.

[runhistory]: machine-learning-manage-experiment-iterations.md

Kada ste zadovoljni modelu, možete je uvesti kao web-usluga koristi za predviđanje podignete cijene pomoću nove podatke. Pogledajte [uvođenja Azure stroj učenje web-servis] [ publish] za više detalja.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Prođite predvidljivih modeliranja tehnike za stvaranje na sveobuhvatne i detaljne osposobljavanje, bodovanja i implementaciji model, potražite [razvoju predvidljivih rješenja pomoću Azure stroj učenje][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
