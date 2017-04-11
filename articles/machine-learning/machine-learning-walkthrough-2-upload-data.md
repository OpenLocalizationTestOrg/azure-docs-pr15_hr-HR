<properties
    pageTitle="Korak 2: Prijenos podataka u strojnog učenja eksperiment | Microsoft Azure"
    description="Korak 2 od razvoju vodič predvidljivu rješenja: prijenos javne podatke pohranjene u Azure strojnog učenja Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Vodič korak 2: Prijenos postojeće podatke u pokusa Azure strojnog učenja

To je drugi korak u prikazu [razvoju rješenja predvidljivu analize u Azure strojnog učenja](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Stvaranje radnog prostora za učenje za računala](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Prijenos postojećih podataka.**
3.  [Stvaranje nove eksperiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Obuka i analiza u modelima](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Implementacija web-usluge](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Pristup web-usluge](machine-learning-walkthrough-6-access-web-service.md)

----------

Za razvoj predvidljivu modela za rizika kreditne kartice, moramo podataka koji koristimo za obuka i testiranje modela. Za ovaj vodič ćemo koristiti "UCI Statlog (njemački odobrenja podataka) skup podataka" u spremištu UCI strojnog učenja. To možete pronaći ovdje:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ics.uci.edu/ML/DataSets/Statlog+(German+Credit+Data)</a>

Ćemo koristiti datoteku pod nazivom **german.data**. Preuzmite ovu datoteku na lokalni disk.  

Ovaj skup podataka sadrži retke 20 varijabli 1000 proteklih prijavama za kreditne kartice. Ove 20 varijable predstavljaju vektorski značajku u skupu podataka koji omogućuje identifikacijski značajke za svaki kandidata kreditne kartice. Dodatni stupac u svakom retku predstavlja na kandidata izračunati odobrenja rizika, s 700 prijavama smatraju rizika malo odobrenja i 300 kao visoko rizika.

Na web-mjestu UCI sadrži opis atributa vektorski značajke koje obuhvaćaju financijske podatke, kreditnu povijest, status zaposlenja i osobnih podataka. Za svaki kandidata binarni ocjena je zadani koji označava jesu li na nisko ili visoko Kreditna rizika.  

Ćemo pomoću ove podatke uvježbati model predvidljivu analize. Kad ne možemo, našem modelu trebali biste moći prihvaćanje informacije za nove pojedinaca i predviđanje jesu li rizika malo ili visoke kreditne kartice.  

Evo jednog zanimljive twist. Opis skupu podataka objašnjava koji misclassifying osoba, kao što je više 5 puta skup za financijske ustanove od misclassifying rizika malo odobrenja kao visoko rizika malo odobrenja kada su zapravo rizika visoke kreditne kartice. Jedan jednostavan način da će u obzir u našem eksperiment je dupliciranje (5 puta) one stavke koje predstavljaju osoba koja ima rizika visoke kreditne kartice. Zatim, ako model misclassifies rizika visoke odobrenja kao niske, će učiniti tu misclassification 5 puta jednom za svaki duplikat. To će se povećati trošak ta se pogreška u rezultatima obuku.  

##<a name="convert-the-dataset-format"></a>Pretvaranje oblika skup podataka
Izvorni skup podataka koristi oblik prazno zarezom. Strojnog učenja Studio radi bolje pomoću datoteke vrijednosti odvojenih zarezom (CSV), pa ne možemo ćete pretvoriti skupu podataka jer zamjenjuju razmake zarezima.  

Da biste pretvorili ove podatke na više načina. Jedan je od načina korištenja sljedeću naredbu komponente Windows PowerShell:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Drugi način je pomoću naredbe sed Unix:  

    sed 's/ /,/g' german.data > german.csv  

U svakom slučaju, stvorili smo odvojenih zarezom verziju podatke u datoteku pod nazivom **german.csv** ćemo koristiti u našem eksperiment.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>Prijenos skupu podataka na računalu učenje Studio

Kada podatke je pretvorena u oblik CSV, moramo prenijeti u Studio strojnog učenja. Dodatne informacije o početku rada s strojnog učenja Studio potražite u članku [Microsoft Azure strojnog učenja Studio Home](https://studio.azureml.net/).

1.  Otvorite strojnog učenja Studio ([https://studio.azureml.net](https://studio.azureml.net)). Kada se prijavite od vas zatraži, pomoću računa koje ste naveli kao vlasnik radnog prostora.
1.  Kliknite karticu **Studio** pri vrhu prozora.
1.  Kliknite **+ NOVO** pri dnu prozora.
1.  Odaberite **skup podataka**.
1.  Odaberite **iz LOKALNE datoteke**.
1.  U dijaloškom okviru **Prijenos novi skup podataka** kliknite **Pregledaj** , a zatim pronađite datoteku **german.csv** koju ste stvorili.
1.  Unesite naziv za skup podataka. Za ovaj vodič smo ćete ga pozovite "UCI njemački kreditnom karticom Data".
1.  Vrsta podataka odaberite **generički CSV datoteke koji nema zaglavlje (. nh.csv)**.
1.  Ako želite, dodajte opis.
1.  Kliknite **u redu**.  

![Prijenos skup podataka][1]  


To prenosi podatke u skupu podataka modul koji koristimo u pokusa.

> [AZURE.TIP] Da biste upravljali skupove podataka koji ste prenijeli na Studio, kliknite karticu **skupova podataka** na lijevoj strani prozora Studio.

Dodatne informacije o uvozu različite vrste podataka na pokusa potražite u članku [Uvoz podataka obuka u Azure strojnog učenja Studio](machine-learning-data-science-import-data.md).

**Sljedeće: [Stvaranje novog eksperiment](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
