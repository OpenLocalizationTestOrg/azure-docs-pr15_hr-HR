<properties 
    pageTitle="Što je Azure strojnog učenja Studio? | Microsoft Azure"
    description="Pregled Azure ML Studio, povucite i ispustite alat za brzo stvaranje modela iz biblioteke spremni za korištenje algoritmi i moduli."
    keywords="Azure strojnog učenja, azure ml ml studio"
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
    ms.topic="get-started-article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Što je Azure strojnog učenja Studio?

Microsoft Azure strojnog učenja Studio je alat za suradnju, povucite i ispustite možete koristiti za sastavljanje, testirajte i implementacija rješenja predvidljivu analize nad podacima. Strojnog učenja Studio objavljuje modela kao web-usluge koje možete jednostavno potrošena prilagođenih aplikacija ili alatima za Poslovno obavještavanje kao što je Excel.

Strojnog učenja Studio je gdje znanstvenog podataka, predvidljivu analize, oblaka resurse i podataka ispunjavaju.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Radni prostor interaktivne strojnog učenja Studio

Za razvoj predvidljivu analizu model, koje obično pomoću podataka iz jednog ili više izvora, pretvaranje i analizirati podatke putem različitih rukovanje podataka i statističke funkcije i generiranje skup rezultata. Razvoj modela ovako je izračun s iteracijama postupak. Spoji dok mijenjate različite funkcije i njihovim parametrima, rezultate dok ne budete zadovoljni imate obučeni, učinkovitih modela.

**Azure strojnog učenja Studio** daje interaktivne, vizualne prostora da biste jednostavno stvaranje, testiranje i iteracija na model predvidljivu analizu. Koje povlačenje i ispuštanje ***skupova podataka*** i analize ***modula*** na interaktivni crtanja i povezati ih zajedno obrasca programa ***Poigrajte***, koji se izvodi u Studio strojnog učenja. Iteracija na dizajnu modela uređivanje pokusa, spremiti kopiju po želji, a zatim ga ponovno pokrenite. Kada budete spremni, pretvorite vaše ***obuka Poigrajte*** ***predvidljivu eksperiment***, a zatim je objaviti kao ***web-servisa*** tako da se modela mogu pristupati drugim korisnicima.

>[AZURE.TIP] Preuzeti i ispisati dijagram koji daje pregled mogućnosti strojnog učenja Studio, potražite u članku [Pregled dijagrama Azure strojnog učenja Studio mogućnosti](machine-learning-studio-overview-diagram.md).

Postoji bez potrebe za programiranjem, samo vizualno povezivanje skupova podataka i module za sastavljanje predvidljivu analizu modela.

![Azure ML Studio dijagram: Stvaranje eksperimenata, podaci za razne izvore za čitanje, zapisivanje podataka testu dobije, pisanje modela.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Početak rada s strojnog učenja Studio

Pri unosu [Strojnog učenja Studio](https://studio.azureml.net) posjetite stranicu **Početna stranica** . Na tom mjestu možete pogledati dokumentacija, videozapise, webinari i ostale resurse koristan.

Postoje tri kartice na vrhu: **Početna stranica** (kojem započnete), **Studio**i **galerije**.

### <a name="studio"></a>Studio

Kliknite karticu **Studio** , a koje će se zatražiti da se prijavite pomoću Microsoftova računa ili pak računa tvrtke ili obrazovne ustanove. Kada se prijavite, vidjet ćete sljedeće kartice s lijeve strane:

- **PROJEKTI** – zbirke eksperimenata, skupova podataka, bilježnice i ostale resurse koji predstavlja jednu projekt
- **EKSPERIMENATA** - eksperimenata koje ste je stvorili, pokretanje i sprema kao skica
- **Web-SERVISI** – web-usluge koje ste implementiran putem vaše eksperimenata
- **BILJEŽNICA** – Jupyter bilježnice koje ste stvorili
- **Skupove podataka** – skupove podataka koji ste prenijeli u Studio
- **OBUČENI MODELIMA** - modela koji ste obučavanju članova u eksperimenata i spremili u Studio
- **Postavke** – skup postavki koje možete koristiti za konfiguriranje računa i resursa.

### <a name="gallery"></a>Galerija

Kliknite karticu **Galerija** , a koje će snimili u galeriju obavještavanje Cortana. Galerije je mjesto gdje zajednice fizičari podataka i razvojnim inženjerima možete omogućiti zajedničko korištenje rješenja stvoren pomoću komponente paketa obavještavanje Cortana.

Dodatne informacije o galeriji potražite u članku [zajedničko korištenje i pronađite rješenja u galeriji obavještavanje Cortana](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Komponente pokusa

Pokusa sastoji se od skupove podataka koje daju podatke analytical moduli koji zajedno povezivanje za sastavljanje predvidljivu analizu modela. Konkretno, valjani eksperiment sadrži sljedeće značajke:

- Stalna sadrži najmanje jedan skup podataka i jednog modula
- Skupove podataka mogu se povezati samo moduli
- Moduli može biti povezani s skupove podataka ili druge module
- Sve unos priključke za module morate imati neke vezu u tijeku
- Sve potrebne parametara za svakom modulu mora biti postavljeno

Možete stvoriti pokusa od nule ili uzorka pokusa postojeće možete koristiti kao predložak. Dodatne informacije potražite u članku [korištenje uzorka eksperimenata da biste stvorili novi eksperimenata](machine-learning-sample-experiments.md).

Primjer Stvaranje jednostavne eksperiment, potražite u članku [Stvaranje jednostavne eksperiment u Azure strojnog učenja Studio](machine-learning-create-experiment.md).

Potpuniji vodič stvaranja predvidljivu analize rješenje potražite u članku [razvoju predvidljivu rješenje s Azure strojnog učenja](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Skupove podataka

Skup podataka je podataka koji sadrži preneseni na računalu učenje Studio tako da se mogu koristiti u postupku Modeliranje. Broj skupova podataka uzorka uključene strojnog učenja Studio koje možete isprobati, a ne možete prenositi više skupova podataka kao što su vam potrebne. Evo nekoliko primjera uključene skupove podataka:

- **MPG podaci za razne automobiles** – milja po galon (MPG) vrijednosti za automobiles označenih brojevima cilindre, Konjska snaga itd.
- **Breast cancer podataka** – Breast cancer Dijagnostika podataka.
- **Skup stabala fires podataka** – veličine fire skupa stabala u sjeveroistočnom ogranku Portugal.

Tijekom stvaranja pokusa možete odabrati s popisa dostupnih skupova podataka s lijeve strane područje crtanja.

Popis skupova podataka uzorka obuhvaćeno strojnog učenja Studio, potražite u članku [Korištenje uzoraka skupa podataka u Azure strojnog učenja Studio](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Moduli

Modul je algoritam koje možete izvršiti nad podacima. Strojnog učenja Studio ima nekoliko module u rasponu od funkcije ingress podataka za obuku, bilježenje rezultata i procesa provjere valjanosti. Evo nekoliko primjera uključene modula:

- [Pretvaranje ARFF] [ convert-to-arff] -pretvara .NET serijalizirani skup podataka na oblik atribut odnosa datoteke (ARFF).
- [Izračunavanje osnovne Statistika] [ elementary-statistics] -izračunava osnovne Statistika kao što je srednja vrijednost, standardna devijacija itd.
- [Linearna regresija] [ linear-regression] -stvara modelu online prijelaza utemeljen na descent linearne regresije.
- [Rezultat Model] [ score-model] -Scores obučeni klasifikacija ili regresije model.

Tijekom stvaranja pokusa možete odabrati s popisa dostupni moduli s lijeve strane područje crtanja.  

Modul možda skupa parametara koje možete koristiti da biste konfigurirali u modulu Interna algoritama. Kad odaberete modul u području crtanja, u modulu parametri prikazuju se u oknu **Svojstva** s desne strane područje crtanja. Možete izmijeniti parametara u oknu za ugađanje modela.

Neke pomoć kretanje kroz velike biblioteke strojnog učenja algoritama dostupna, potražite u članku [kako odabrati algoritama za Microsoft Azure strojnog učenja](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Implementacija web-servisa predvidljivu analytics

Kad je spremna predvidljivu analize modelu, možete je implementirati kao web-servisa izravno iz Studio strojnog učenja. Dodatne informacije o tom se postupku potražite u članku [uvođenja u web-servisa Azure strojnog učenja](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
