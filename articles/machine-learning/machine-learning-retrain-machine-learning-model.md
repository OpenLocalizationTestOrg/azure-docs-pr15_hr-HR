<properties
    pageTitle="Retrain strojnog učenja modela | Microsoft Azure"
    description="Saznajte kako retrain modela i ažuriranje web-usluge za korištenje upravo obučeni modela u Azure strojnog učenja."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Retrain strojnog učenja modela

Ako je tijekom procesa operationalization strojnog učenja modela u Azure strojnog učenja, model obučavanju članova i spremiti. Zatim je koristite da biste stvorili predicative web-servisa. Web-servisa se može trošiti u web-mjesta, nadzornih ploča i mobilne aplikacije. 

Modeli stvorenih pomoću strojnog učenja nisu obično statične. Postaje dostupan nove podatke ili kada korisnik u API ima vlastite podatke modela mora biti retrained. 

Retraining može doći do često. Pomoću značajke programski Retraining API-JA možete programski retrain modela pomoću API-ji Retraining i ažurirati web-servisa upravo obučeni modela. 

Ovaj dokument u članku se opisuje postupak retraining te se pokazuje kako koristiti API-ji Retraining.

## <a name="why-retrain-defining-the-problem"></a>Zašto retrain: definiranje problema  

Kao dio strojnog učenja obuka procesa, model je put uvježbavan pomoću skupa podataka. Modeli stvorenih pomoću strojnog učenja nisu obično statične. Postaje dostupan nove podatke ili kada korisnik u API ima vlastite podatke modela mora biti retrained.

U sljedećim scenarijima programski API pruža jednostavan način da biste omogućili vama ili korisnik sustava API-ji stvaranje klijenta koji je moguće, na temelju jednokratni ili običnih, retrain modela pomoću vlastitih podataka. Ih možete Analiza rezultata retraining i ažuriranje API servisa Web tako da koristi nove obučeni model.

>[AZURE.NOTE] Ako imate postojeće eksperiment obuka i novo web-mjesto servisa, trebali biste potražite u članku Retrain postojeći servis predvidljivu Web umjesto pratiti vodič koji se spominju u sljedećem odjeljku.

## <a name="end-to-end-workflow"></a>Kraj do kraja tijeka rada 

Postupak obuhvaća sljedeće komponente: eksperiment obuka odgovora i predvidljivu eksperiment objavili kao web-servisa. Da biste omogućili retraining obučeni modela, obuka pokusa mora objaviti kao web-servisa s obučeni modela. Time se omogućuje pristup API modelu za retraining. 

Sljedeći koraci primjenjuju se na novo i klasične Web services:

Stvaranje početne predvidljivu web-usluge:

* Stvaranje eksperiment za osposobljavanje
* Stvaranje predvidljivu web eksperiment
* Implementacija predvidljivu web-servisa

Retrain web-servisa:

* Ažuriranje eksperiment tečaj da biste dopustili retraining
* Implementacija retraining web-servisa
* Pomoću koda za obradu izvođenja servisa retrain modela

Objašnjenje prethodnih koraka, potražite u članku [Retrain strojnog učenja modelima programski](machine-learning-retrain-models-programmatically.md).

Ako implementiran klasične web-servisa:

* Stvorili krajnju točku na predvidljivu web-usluge
* ZAKRPU URL i kod
* Koristite URL ZAKRPU pokazati novi krajnje točke na retrained modela 

Objašnjenje prethodnih koraka, potražite u članku [Retrain klasične web-servisa](machine-learning-retrain-a-classic-web-service.md).

Ako naiđete na poteškoće retraining klasične web-servisa, potražite u članku [Otklanjanje poteškoća s retraining web-sustava Azure strojnog učenja klasični servisa](machine-learning-troubleshooting-retraining-models.md).

Ako implementiran novog web-servisa:

* Prijavite se na račun za Azure Voditelj resursa
* Početak definiciju Web servisa
* Izvoz servisa definiciju Web kao JSON
* Ažuriranje referencu na `ilearner` blob u na JSON
* Uvoz u JSON u definiciju Web servisa
* Ažurirajte web-servisa novu definiciju Web servisa

Objašnjenje prethodnih koraka, potražite u članku [Retrain novog web-servisa pomoću cmdleta ljuske PowerShell za upravljanje učenje za računala](machine-learning-retrain-new-web-service-using-powershell.md).

Postupak za postavljanje retraining klasične web-uslugu obuhvaća sljedeće korake:

![Retraining pregled postupka][1]

Postupak za postavljanje retraining novo web-mjesto servisa obuhvaća sljedeće korake:

![Retraining pregled postupka][7]

## <a name="other-resources"></a>Ostali resursi

- [Retraining i ažuriranje učenje Azure strojno modeli s tvorničke Azure podataka](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Stvaranje mnogo modelima učenje računala i web-krajnje točke servisa iz jednog eksperiment pomoću komponente PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
- [AML Retraining modela pomoću API -ji](https://www.youtube.com/watch?v=wwjglA8xllg) videozapis pokazuje kako retrain strojnog učenja modele stvorene u Azure strojnog učenja pomoću Retraining API-ji i PowerShell.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

