<properties
    pageTitle="Modul ljuske PowerShell za strojnog učenja | Microsoft Azure"
    description="Modul ljuske PowerShell za Azure strojnog učenja dostupan je u javnom pretpregleda. Pomoću ljuske PowerShell za stvaranje i upravljanje radnim prostorima, eksperimenata, web services i drugo."
    keywords="Isprobajte linearne regresije, strojnog učenja algoritmi, strojnog učenja vodič, predvidljivu Modeliranje tehnike, eksperiment znanstvenog podataka"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Modul ljuske PowerShell za Microsoft Azure strojnog učenja

Modul ljuske PowerShell za Azure strojnog učenja Napredna alat je koji omogućuje vam korištenje komponente Windows PowerShell za upravljanje radnim prostorima, eksperimenata, skupova podataka, web serivces i više.

Možete pogledati u dokumentaciji i preuzimanje modula, zajedno s puno izvornog koda, pri [https://aka.ms/amlps](https://aka.ms/amlps). 

## <a name="what-is-the-machine-learning-powershell-module"></a>Što je modul strojnog učenja PowerShell?

Modul ljuske PowerShell strojnog učenja je na. Neto sustavom DLL modul koji vam omogućuje potpuno upravljanje radnim prostorima, eksperimenata, skupova podataka, web-servisi i krajnje točke servisa web Azure strojnog učenja iz komponente Windows PowerShell. Uz modul, možete preuzeti cijeli izvorni kod koji uključuje cleanly odvojene [C# API sloja](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). To znači da možete pozvati ovaj DLL iz .NET projekta i upravljanje Azure strojnog učenja putem .NET koda. Osim toga, DLL ovisi o pozadini REST API-ji koji omogućuje korištenje izravno iz Omiljene klijent.

## <a name="what-can-i-do-with-the-powershell-module"></a>Što učiniti s modul ljuske PowerShell?

Slijede zadaci koje možete izvršiti pomoću ovog modul ljuske PowerShell. Pogledajte [cijeli dokumentaciju](https://aka.ms/amlps) za te i mnogo više funkcija.

- Dodjela resursa za novog radnog prostora putem upravljanja certifikata ([Novo AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
- Izvoz i uvoz datoteke JSON koji predstavlja grafikonu eksperiment ([AmlExperimentGraph za izvoz](https://github.com/hning86/azuremlps#export-amlexperimentgraph) i [Uvoz AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Pokretanje pokusa ([Start AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Stvaranje web-servisa iz predvidljivu eksperiment ([Novo AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Stvorili krajnju točku na objavljenu web-servisa ([Dodaj AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Pozivanje RRS i/ili BES web servisa krajnje ([Pozovite AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) i [Pozovite AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Evo brzog primjera pomoću komponente PowerShell da biste pokrenuli pokusa postojeće:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Detaljnije korištenje slova, potražite u članku o korištenju modul ljuske PowerShell za automatizaciju zadataka vrlo često zatražio: [Stvaranje mnogo modelima učenje računala i web-krajnje točke servisa s jednog eksperiment pomoću komponente PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Kako započeti rad?

Za početak rada sa servisom PowerShell strojnog učenja preuzmite [pustite paketa](https://github.com/hning86/azuremlps/releases) iz GitHub, a zatim slijedite [upute za instalaciju](https://github.com/hning86/azuremlps/blob/master/README.md). Morat ćete deblokiranje DLL preuzeti/raspakirana, a zatim je uvezite u okruženju sustava PowerShell. Većina s cmdletima potrebno navesti Identifikator radnog prostora, ovlaštenja radnog prostora i Azure regiju koju je radni prostor u. Najjednostavniji način za to je do datoteke config.json zadani je opisana su u odjeljku pojedinosti u upute za instalaciju. Naravno, možete Kloniraj stabla brojka i izmijeniti i Kompiliranje kod lokalno korištenjem Visual Studio.

## <a name="next-steps"></a>Daljnji koraci

Modul ljuske PowerShell će nastaviti Poboljšana i proširiti tijekom tog razdoblja pretpregled. Nadzirali [Obavještavanje Cortana i strojnog učenja Blog](https://blogs.technet.microsoft.com/machinelearning/) dodatne interesne grupe i informacije.
