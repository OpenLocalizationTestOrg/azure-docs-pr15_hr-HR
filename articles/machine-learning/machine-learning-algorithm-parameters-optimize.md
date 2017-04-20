<properties
    pageTitle="Odaberite parametre da biste optimizirali algoritmi u učenje stroj Azure | Microsoft Azure"
    description="U članku se objašnjava odabir optimalnim parametar postavljen za algoritam učenje Azure strojne grupe."
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


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Odaberite parametre da biste optimizirali algoritmi u Azure stroj učenje

Ova tema opisuje kako odabrati desno hyperparameter postavite za algoritam u učenje Azure strojne grupe. Većina algoritmi učenje stroj imati parametre za postavljanje. Kada uvježbati model morate odrediti vrijednosti za te parametre. Efficacy obučeni model ovisi o model parametre koji odaberete. Proces traženja optimalno skupa parametara poznate kao *model odabira*.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Odabir modela različitih načina. U učenje stroj-valjanosti je jedan najrašireniji metode za odabir modela i je mehanizam odabira zadanog modela u učenje Azure strojne grupe. Jer Azure stroj učenje podržava R i Python, uvijek možete implementirati vlastite mehanizme odabira model korištenjem R ili Python.

Postoje četiri koraka u procesu pronalaženje najbolje skup parametara:

1.  **Definiranje prostora parametar**: za algoritam, prvo odlučiti točno parametar vrijednosti koje želite razmotriti.
2.  **Postavke provjere valjanosti izdvojeno Definiraj**: odlučiti kako odabrati savijanja valjanosti za skup podataka.
3.  **Definirajte metriku**: odlučiti što metriku za određivanje najbolje skup parametara kao što su točnost, mean korijenski kvadratnu pogreška, preciznost, opoziv ili f rezultat.
4.  **Podučavanje procijeniti, i usporedite**: za svaku jedinstvenu kombinaciju vrijednosti parametra-valjanosti je provodi po i na temelju metrike pogreška definirate. Nakon usporedbe i procjenu, možete odabrati najmanjim predstavljanja modela.

Sljedeća slika ilustrira prikazuje kako to možete postići u učenje Azure strojne grupe.

![Pronaći najbolje skup parametara](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Definiranje prostora parametar
Možete definirati parametar postavljen na korak inicijalizacije modela. Parametar okno sve strojne grupe učenje algoritmi ima dva trainer načina: *Jedan parametar* i *Raspon parametar*. Odaberite raspon parametar načinu. U načinu parametar raspon možete unijeti više vrijednosti za svaki parametar. U tekstni okvir možete unijeti vrijednosti odvojenih zarezom.

![Dva klase boosted odluka stabla, jedan parametar](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Također, možete definirati maksimalne i minimalne točaka u rešetku i ukupni broj točaka generiraju pomoću **Korištenje Sastavljača raspon**. Po zadanom, vrijednosti parametra generiranih linearni mjerilo. Ali ako je označeno **Log skala** vrijednosti su generirane u Logaritamsko mjerilo (, omjer susjednih točke je konstanta umjesto njihovih razlika). Za cijeli broj parametara možete definirati raspon korištenjem spojnice. Na primjer, "1-10" znači sve cijelih brojeva između 1 i 10 (i uključivo) obrasca je skup parametara. Miješani način rada također podržava. Na primjer, postavite parametar "1-10, 20, 50" želite uključiti cijelim brojevima 1-10, 20, i 50.

![Dva klase boosted odluka stabla, parametar raspon](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Definiranje savijanja valjanosti
Na [particiju i uzorak] [ partition-and-sample] modul možete koristiti za nasumično dodjelu savijanja podatke. U sljedeći uzorak konfiguracija za modul, možemo definirati pet savijanja i nasumično dodijeliti preklop broj instanci uzorka.

![Particija i uzorak](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>Definirajte metriku
[Ugodi Hyperparameters Model] [ tune-model-hyperparameters] modul pruža podršku za empirically odabir najbolje skupa parametara za algoritam dani i dataset. Uz druge informacije vezano uz osposobljavanje model oknu **Svojstva** ovom modulu uključuje metriku za određivanje najbolje skup parametara. Odnosno ima dva različite padajućeg popisa okvira za klasifikaciju i regresije algoritama. Ako je algoritam pod obzir klasifikacija algoritam, metrika regresije zanemaruju i obrnuto. U ovom primjeru određenu metriku je **Točnost**.   

![Parametri potez](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Uvježbavanje, procijeniti i Usporedba  
Isti [Ugodi Hyperparameters Model] [ tune-model-hyperparameters] modul trains modela koji odgovaraju skup parametara, procjenjuje različite metriku i stvara najmanjim obučeni modela na temelju metrike koju odaberete. Ovom modulu ima dva obavezna ulaza:

* Untrained učenik
* Skup podataka

Modul je neobavezna dataset unos. Skup podataka povezati s informacijama preklop ulaz obavezna dataset. Ako skup podataka je dodijeljeno bilo kakve informacije preklop, zatim 10-fold valjanosti izdvojeno automatski izvršava se po zadanom. Ako preklop Dodjela se vrši i provjere valjanosti dataset navedeni na priključak neobavezne dataset, odabrali Podučavanje Probni način rada i prvi dataset koristi za uvježbavanje model za svaku kombinaciju parametar.

![Klasifikatora stabla odluka boosted](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

Model zatim procijeni na dataset provjere valjanosti. Lijevi izlazni priključak modul prikazuje različite metriku kao funkcije vrijednosti parametra. Desni izlazni priključak daje obučeni model koji odgovara najmanjim predstavljanja modela prema odabranu metriku (**Točnost** u ovom slučaju).  

![Provjera valjanosti dataset](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Možete vidjeti točan parametri odabrao pojednostaviti desno izlazni priključak. Ovaj model može se koristiti u bodovanja test skup ili operationalized web-servis nakon spremanja kao obučeni modela.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
