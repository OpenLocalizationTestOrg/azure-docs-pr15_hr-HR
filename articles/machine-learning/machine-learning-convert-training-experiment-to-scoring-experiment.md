<properties
    pageTitle="Pretvaranje eksperiment za obuku strojnog učenja predvidljivu eksperiment | Microsoft Azure"
    description="Kako pretvoriti eksperiment strojnog učenja obuka, koristi za osposobljavanje modela predvidljivu analytics za predvidljivu eksperiment koje možete uvesti kao web-servisa."
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
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Pretvaranje eksperiment za obuku strojnog učenja predvidljivu eksperiment

Azure strojnog učenja omogućuje vam omogućuje stvaranje, testiranje i implementacija rješenja predvidljivu analize.

Nakon što ste napravili i iterated na na *isprobajte tečaj* da biste uvježbati modela predvidljivu analize i spremni ste ga koristiti za rezultatu nove podatke, morate za pripremu i pojednostaviti vaše eksperiment za bilježenje rezultata. Zatim možete implementirati *predvidljivu eksperiment* kao servis Azure web tako da korisnici mogu poslati podatke modela i primati na model predviđanja.

Pretvaranjem predvidljivu eksperiment primate obučeni modela spremni uvesti kao web-servisa. Korisnici web-usluge će poslati ulazne podatke modela i modela poslat će vratiti rezultate za predviđanje. Pa kao pretvorili predvidljivu eksperiment ćete željeti Imajte na umu kako ste i očekivali modela se može koristiti u drugim korisnicima.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Postupak pretvaranja obuka eksperiment predvidljivu eksperiment obuhvaća tri koraka:

1.  Spremite model učenje računalu koje ste obučavanju članova, a zatim zamijenite strojnog učenja algoritam i [Vlaku modela] [ train-model] moduli s spremljeni obučeni model.
2.  Obrezivanje pokusa da biste samo module koje su vam potrebne za bilježenje rezultata. Obuka eksperiment sadrži broj moduli koji su potrebni za osposobljavanje, ali su potrebne kada je model obučeni i spremni za bilježenje rezultata.
3.  Definiranje gdje u vašem eksperiment ćete prihvatiti podatke iz korisnik usluge web i podataka koje će se vratiti.

## <a name="set-up-web-service-button"></a>Gumb za postavljanje web-servisa

Nakon pokretanja eksperiment (**POKRENI** gumb pri dnu područja crtanja eksperiment), gumb za **Postavljanje web-servisa** (odaberite mogućnost **Predvidljivu web-servisa** ) će izvesti za tri koraka pretvaranja vaše eksperiment obuka predvidljivu eksperiment:

1.  Sprema obučeni modela kao modul u odjeljku **Obučeni modelima** paleti modulu (da biste s lijeve strane područje crtanja eksperiment), zatim zamjenjuje strojnog učenja algoritam i [Vlaku modela] [ train-model] moduli s spremljeni obučeni model.
2.  Uklanja module koji jasno nisu potrebne. U našem primjeru to uključuje [Razdijeljeni podaci][split], 2 [Rezultat Model][score-model], i [Procjenu Model] [ evaluate-model] module.
3.  Ga stvara unos servisa Web izlaz moduli i ih dodaje u zadana mjesta u vašem eksperiment.

Na primjer, sljedeće pokusa trains model stabla dva klase boosted odluka korištenje oglednih podataka za statistiku:

![Obuka eksperiment][figure1]

Moduli u ovom eksperiment obavljaju zapravo četiri različite funkcije:

![Modul za funkcije][figure2]

Kada pretvorite ovaj tečaj eksperiment predvidljivu eksperiment, neke od tih moduli više nije potrebna ili imaju različite svrhu:

- **Podataka** – podatke u ovom uzorka skup podataka ne obavlja tijekom bilježenje rezultata – korisnika web-servisa će izvor podataka koji će biti testu dobije. Međutim, metapodataka iz ovaj skup podataka, kao što su vrste podataka, koristi obučeni modela. Da morate zadržati skupu podataka u predvidljivu pokusa tako da se može osigurati ovaj metapodataka.

- **Avansne** - ovisno o podatke koji će biti poslana za bilježenje rezultata, te moduli možda ili možda neće biti potrebne za obradu podataka.

    Ako, primjerice, u ovom primjeru dataset uzorka možda vrijednosti koje nedostaju i sadrži stupce koji nisu potrebni za uvježbati modela. Pa [Očisti podataka koji nedostaju] [ clean-missing-data] modul isporučuje dijeli s vrijednostima koje nedostaju i [Odabir stupaca u skupu podataka] [ select-columns] modul isporučuje za isključivanje te dodatne stupce iz toka podataka. Ako znate da podatke koji će biti poslana za bilježenje rezultata putem web-servisa neće imati vrijednosti koje nedostaju, a zatim možete ukloniti [Clean podataka koji nedostaju] [ clean-missing-data] modul. Međutim, od [Odabir stupaca u skupu podataka] [ select-columns] modul pomaže definirati skup značajki se testu dobije, taj modul treba ostaju.

- **Vlaku** – kada model sadrži je uspješno obučavanju članova, možete spremiti kao jedan obučeni model modul. Zatim zamijenite te pojedinačne moduli spremljeni obučeni model.

- **Rezultat** – u ovom primjeru modul Podijeli služi za podjele toka podataka u skupu test podataka i podataka za obuku. U predvidljivu pokusa to nije potreban i moguće ukloniti. Isto tako, 2 [Rezultat Model] [ score-model] modul i [Procjenu Model] [ evaluate-model] modul koriste se za usporedbu rezultata iz podataka za testiranje tako te moduli i nisu potrebne u predvidljivu pokusa. Preostali [Rezultat Model] [ score-model] modula, međutim, je potreban za vraćanje rezultata rezultat putem web-servisa.

Evo kako je našeg primjera izgleda nakon klika na **Postavi web-servisa**:

![Pretvara predvidljivu eksperiment][figure3]

To može biti potrebne da biste pripremili vaše eksperiment uvesti kao web-servisa. Međutim, preporučujemo vam da ne odnose na vaše eksperiment učiniti još nešto.

### <a name="adjust-input-and-output-modules"></a>Prilagodba ulazni i izlazni moduli

U svoje eksperiment obuka koristi skupa podataka za obuku i niste neke obrada dohvaćanje podataka u obliku koji je potrebno algoritam strojnog učenja. Ako podataka očekujete da ćete primiti putem web-servisa nije potrebno ovaj obrada, možete premjestiti **Web servisa unos modul** različite čvor u vašem eksperiment.

Ako, na primjer, po zadanom **Postavljanje web-servisa** stavlja modul **Web servisa unos** pri vrhu toka podataka, kao na slici iznad. Međutim, ako ulaznih podataka nije potrebno ovaj obrada, ručno možete smjestiti **Web servisa unos** prošle module za obradu podataka:

![Premještanje servisa unos web][figure4]

Unos podataka koji se odvija pomoću web-servisa sada proći kroz izravno u modulu rezultat modela bez sve pretprocesnih.

Isto tako, po zadanom **Postavljanje web-servisa** stavlja modul izlaz Web servisa pri dnu protok podataka. U ovom primjeru web-servisa koji će vratiti korisniku izlazni [Rezultat Model] [ score-model] modul koji sadrži vektorski dovršeno ulaznih podataka plus na bilježenje rezultata s rezultatima.

Međutim, ako želite da biste se vratili na nešto različite – na primjer, bilježenja rezultata rezultate, a ne cijelu vektorski ulazne podatke –, zatim možete umetnuti [Odabir stupaca u skupu podataka] [ select-columns] modul da biste isključili sve stupce osim bilježenja rezultata rezultate. Premještanje modul **Izlaz servis za Web** u izlaz [Odabir stupaca u skupu podataka] [ select-columns] modula:

![Premještanje servisa izlaz web][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Dodavanje i uklanjanje moduli dodatnu obradu podataka

Ako su vaše eksperiment koje znate da će biti potrebno tijekom bilježenje rezultata dodatne module, oni mogu se ukloniti. Ako, na primjer, jer smo prijeđete modul **Web servisa unos** točku nakon module za obradu podataka, ne možemo možete ukloniti [Clean podataka koji nedostaju] [ clean-missing-data] modula iz predvidljivu pokusa.

Naš predvidljivu eksperiment sada izgleda ovako:

![Uklanjanje dodatnih modula][figure6]

### <a name="add-optional-web-service-parameters"></a>Dodavanje neobavezni parametri Web servisa

U nekim slučajevima, preporučujemo vam da biste korisniku web-servisa da biste promijenili zadano ponašanje moduli kada je servis pristupa. *Parametri servisa web* omogućuju da biste to učinili.

[Uvoz podataka] postavlja uobičajeni primjer[ import-data] modul tako da korisnik distribuiranih web-usluge može odrediti drugog izvora podataka prilikom pristupa web-servisa. Ili konfiguriranju [Izvoz podataka] [ export-data] modul tako da možete navesti različite odredište.

Možete definirati parametre servisa Web i pridružiti jedan ili više parametara modul, a možete odrediti jesu li obavezan ili nije. Korisnik web-usluge možete pa unesite vrijednosti za parametara prilikom pristupa usluge i akcije modul bit će izmijenjeno sukladno tome.

Dodatne informacije o parametrima servisa Web potražite u članku [Pomoću Azure strojnog učenja Web servisa parametara ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Implementacija predvidljivu pokusa kao web-servisa

Sad kad predvidljivu pokusa potpuno je pripremiti, možete je uvesti kao servis Azure web. Pomoću web-servisa, korisnici mogu slati podatke modela i model će vratiti njegov predviđanja.

Dodatne informacije o proces dovršeno implementacije potražite u članku [uvođenja u web-servisa Azure strojnog učenja][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
