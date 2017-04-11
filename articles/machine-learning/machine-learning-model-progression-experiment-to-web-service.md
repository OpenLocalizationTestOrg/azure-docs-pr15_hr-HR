<properties
    pageTitle="Kako se model strojnog učenja nastavlja iz pokusa operationalized web-uslugu | Microsoft Azure"
    description="Pregled mehanika od Poigrajte kako se vaše Azure strojnog učenja modela napreduje iz u razvoju operationalized web-uslugu."
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


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Kako se model strojnog učenja nastavlja iz pokusa operationalized web-uslugu

Azure strojnog učenja Studio nudi interaktivne područje crtanja koji vam omogućuje da razviti pokretanje, testirajte i iteracija programa ***Poigrajte*** koji predstavlja predvidljivu analizu modela. Postoje razna module dostupna koji možete:

* Unos podataka u vašem eksperiment
* Rukovanje podacima
* Obuka modela pomoću strojnog učenja algoritama
* Rezultat u model
* Analiza rezultata
* Izlaz konačne vrijednosti

Kada budete zadovoljni s vašeg eksperiment, možete je uvesti kao ***web-za učenje računala za klasični Azure servisa*** ili ***Novo Azure strojnog učenja web-mjesto servisa*** tako da korisnici mogu poslati nove podatke i dobivate natrag rezultate.

U ovom se članku ne možemo dati pregled mehanika od Poigrajte kako se vaše strojnog učenja modela napreduje iz u razvoju operationalized web-uslugu.

>[AZURE.NOTE] Postoje drugi načini razviti i implementacija strojnog učenja modela, no ovaj članak namijenjen je korištenju strojnog učenja Studio. Rasprave kako stvoriti klasični predvidljivu web-servisa s R, potražite u članku na blogu [Sastavi i implementacija predvidljivu Web Apps pomoću RStudio i Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).

Dok Azure strojnog učenja Studio namijenjen da vam pomogne razviti i implementacija *model predvidljivu analizu*, moguće je da biste koristili Studio za razvoj pokusa koji ne uključuje predvidljivu analizu modela. Pokusa možda, na primjer, samo za unos podataka, njima rukovati i izlaz rezultate. Baš kao i na eksperiment predvidljivu analizu možete implementirati ovaj koje nisu predvidljivu eksperiment kao web-servisa, ali to znači da jednostavnije procesa pokusa nije obuka ili bilježenje rezultata model strojnog učenja. Dok se ne nalazi u standardne da biste koristili Studio na taj način, ne možemo ćete ga obuhvatiti rasprave tako da se ne možemo dati potpuno objašnjenje funkcioniranja Studio.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Razvoj i implementacija predvidljivu web-servisa

Ovdje su faze koji se nalazi iza uobičajena rješenja kao razviti i implementirajte ga pomoću strojnog učenja Studio:

![Tijek implementacije](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Slika 1 - faze modela tipičnog predvidljivu analizu*

### <a name="the-training-experiment"></a>Stalna obuka

***Isprobajte tečaj*** je početna faza razvoj web-servisa u Studio strojnog učenja. Svrha pokusa obuka je da bi se dobilo mjesto za razvoj, testirajte, iteracija i naposljetku uvježbati strojnog učenja modela. Možete čak i uvježbati više modela istodobno potražite najbolje rješenje, no kada završite eksperimentiranja koje ćete odaberite pojedinačni put uvježbavan modela i uklonite ostale iz pokusa. Primjer razvoj predvidljivu analizu Poigrajte potražite u [razvoju rješenje predvidljivu analize odobrenja rizika u Azure strojnog učenja](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Predvidljivu pokusa

Nakon što dodate obučeni modela u vašem eksperiment obuka, kliknite **Postavljanje web-servisa** i odaberite **Predvidljivu web-servisa** u strojnog učenja Studio da biste započeli postupak pretvaranja vaše eksperiment obuka ***predvidljivu eksperiment***. Svrha predvidljivu pokusa je rezultatu nove podatke, s ciljem naposljetku postanete operationalized kao servis Azure Web pomoću obučeni modela.

Pretvorba je obavljen kroz sljedeće korake:

-   Pretvaranje skup module za obuku u modulu jedan i spremanje u obliku obučeni modela

-   Uklonite sve druge module nisu povezani s bilježenje rezultata

-   Dodavanje ulazni i izlazni priključci koji će koristiti usmjerenog web-servisa

Možda postoje dodatne promjene koje želite da vaše predvidljivu eksperiment Priprema korisnika za implementaciju kao web-servisa. Ako, na primjer, ako želite da se web-servisa za slanje samo podskupa rezultata, možete dodati modula filtriranja prije izlazni priključak.

U ovom postupku pretvorbe pokusa obuka ne odbacuju. Nakon dovršetka postupka u Studio imate dvije kartice: jedan za obuku pokusa i jedan za predvidljivu pokusa. Na taj način možete unijeti promjene u pokusa obuka prije implementacije web-servisa i ponovno stvaranje predvidljivu pokusa. Ili možete spremiti kopiju pokusa tečaj da biste pokrenuli drugi redak početak.

>[AZURE.NOTE] Kada kliknete **Predvidljivu web-servisa** pokretanje automatskog postupak da biste pretvorili vaše eksperiment obuka predvidljivu eksperiment, a to funkcionira i u većini slučajeva. Ako je vaš eksperiment obuka složene (na primjer, imate više puta za obuku koji ste spajanje), možda želite učiniti pretvorbe ručno. Dodatne informacije potražite u članku [Pretvaranje eksperiment za obuku strojnog učenja za predvidljivu eksperiment](machine-learning-convert-training-experiment-to-scoring-experiment.md).

### <a name="the-web-service"></a>Web-usluge

Kada ste zadovoljni da vaše predvidljivu eksperiment spreman, možete implementirati na servisu kao servis klasične Web ili novog web-servisa koji se temelji na Azure Voditelj resursa. Da biste operationalize modela uvođenjem kao *Klasični strojnog učenja web-servisa*, kliknite **Implementacija web-servisa** , a zatim odaberite **Implementacija web-servisa [klasični]**. Da biste implementirali kao *Novi strojnog učenja web-servisa*, kliknite **Implementacija web-servisa** i odaberite **Implementacija web-servisa [novi]**. Korisnici sada možete slati podatke modela pomoću web-servisa REST API-JA i primati natrag rezultate. Dodatne informacije potražite u članku [upute za korištenje programa Azure strojnog učenja web-servisa koji je uveden iz strojnog učenja eksperiment](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Uobičajeni koji nisu slova: Stvaranje koje nisu predvidljivu web-servisa

Vaš eksperiment uvježbati predvidljivu analizu model, a ne morate stvoriti obuka eksperiment i bilježenja rezultata eksperiment - postoji samo jednu eksperiment, a Implementirajte ga u obliku web-servisa. Strojnog učenja Studio otkriva sadrži li vaš eksperiment predvidljivu modela analizom moduli koji ste koristili.

Nakon što ste iterated na vašem eksperiment i zadovoljni je:

1.  Kliknite **Postavljanje web-servisa** i odaberite **Retraining web-servisa** – ulazni i izlazni čvorove automatski se dodaju

2.  Kliknite **Pokreni**

3. Kliknite **Implementacija web-servisa** i odaberite **Implementacija web-servisa [klasični]** ili **Implementacija web-servisa [novi]** ovisno o okruženju na koju želite uvesti.

Web-servisa sada uvesti, a možete pristupiti i upravljanje baš kao i predvidljivu web-servisa.

## <a name="updating-your-web-service"></a>Ažuriranje web-servisa

Sad kad ste implementiran na eksperiment kao web-servisa, što se događa ako trebate ažurirati?

To ovisi o što vam je potrebno da biste ažurirali:

**Želite li promijeniti ulazni i izlazni ili želite promijeniti način na koji web-servisa upravlja podataka**

Ako mijenjate model, ali želite samo promijeniti način postupanja s web-servisa podataka, možete uređivati predvidljivu pokusa i zatim kliknite **Implementacija web-servisa** i ponovno odaberite **Implementacija web-servisa [klasični]** ili **Implementacija web-servisa [novi]** . Web-servis je zaustavljena, ažurirane predvidljivu pokusa je implementiran i ponovnog pokretanja web-servisa.

Evo jednog primjera: pretpostavimo da vaše predvidljivu eksperiment vraća cijeli redak ulaznih podataka predviđene rezultatom. Možda odlučite želite li web-servisa samo vraćaju rezultat. Tako da možete dodati predvidljivu pokusa desno prije izlazni priključak da biste izuzeli stupce koji nisu rezultat modula **Projekta stupaca** . Kada kliknete **Implementacija web-servisa** i ponovno odaberite **Implementacija web-servisa [klasični]** ili **Implementacija web-servisa [novi]** , ažurira se web-servisa.

**Želite li retrain modela novim podacima**

Ako želite zadržati svoje strojnog učenja model, ali želite retrain novim podacima, imate dvije mogućnosti:

1.  **Retrain modela dok se izvodi web-servisa** – želite li retrain modelu dok se predvidljivu web-servisa nije pokrenut, to možete učiniti tako da nekoliko izmjene pokusa tečaj da biste ga ***retraining eksperiment***, a zatim Implementirajte ga u obliku ** *retraining web-* servisa**. Upute o tome kako to učiniti potražite u članku [Retrain strojnog učenja modelima programski](machine-learning-retrain-models-programmatically.md).

2.  **Vratite se na izvornu eksperiment obuka i koristite drugi tečaj podatke za razvoj modela** - vaše predvidljivu eksperiment je povezana s web-servisa, ali pokusa obuka nisu izravno povezani na taj način. Ako izmijeniti izvornu pokusa obuka i kliknite **Postavljanje web-servisa**, stvorit će *nove* predvidljivu koji isprobati kada implementiran, će stvoriti *novu* web-servisa. Samo ne ažurira izvornog web-servisa.

    Ako vam je potrebna za izmjenu pokusa obuka, otvorite je i kliknite **Spremi kao** da biste stvorili kopiju. To će ostavite ostaje netaknuta izvorne obuka pokusa, predvidljivu eksperiment i web-servisa. Sad možete stvarati nova web-servisa s vašim promjenama. Nakon što ste implementiran zatim možete odlučiti želite li prekinuti prethodne web-servisa ili je pokrenut uz novi ostavite novog web-servisa.

**Želite li uvježbati različitih modela**

Ako želite da biste unijeli promjene na izvornu eksperiment predvidljivu, kao što su odabir algoritam za učenje na drugo računalo pokušate drugi tečaj način, itd., a zatim ćete morati slijedite prethodno opisan za retraining modela drugi postupak: otvorite pokusa obuka pa kliknite **Spremi kao** da biste stvorili kopiju, a zatim pokrenite dolje novi put razvoj modela , stvaranje predvidljivu pokusa i implementacija web-servisa. To će stvoriti novo web-mjesto servisa nepovezanih izvorne onu – možete odlučiti koju ili oboje, da biste zadržali pokrenut.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o postupka razvoja i eksperiment potražite u sljedećim člancima:

-   Pretvorba pokusa – [Pretvaranje eksperiment za obuku strojnog učenja za predvidljivu eksperiment](machine-learning-convert-training-experiment-to-scoring-experiment.md)

-   Implementacija web-servisa - [uvođenja u web-servisa Azure strojnog učenja](machine-learning-publish-a-machine-learning-web-service.md)

-   retraining modela - [Retrain strojnog učenja programski modelima](machine-learning-retrain-models-programmatically.md)

Primjeri cijelog procesa, potražite u članku:

-   [Strojnog učenja vodič: Stvaranje vaš prvi eksperiment u Azure strojnog učenja Studio](machine-learning-create-experiment.md)

-   [Vodič: Razviti rješenje predvidljivu analize odobrenja rizika u Azure strojnog učenja](machine-learning-walkthrough-develop-predictive-solution.md)