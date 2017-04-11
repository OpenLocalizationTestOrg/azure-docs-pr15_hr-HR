<properties 
    pageTitle="Stroj učenje web services Primjeri načinjene pomoću R | Microsoft Azure" 
    description="Pronađite korisne skup web services Primjeri stvorene pomoću koda R i strojnog učenja i objaviti trgovine Windows Azure." 
    keywords="csharp kod r, web services Primjeri"
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="web-services-examples-using-r-code-on-azure-machine-learning-and-published-to-microsoft-azure-marketplace"></a>Web services primjerima pomoću koda za R na Azure strojnog učenja i objaviti na web-servisa Microsoft Azure Marketplace

U ovom članku su primjeru web services stvorena pomoću Azure strojnog učenja i objaviti trgovine Windows Azure. Svaki web servisa: primjerice ima potpun dokument pridružen, ugrađivanje skupova podataka za testiranje servise i objašnjava kako korisnika možete stvoriti slične servisa sami. 

U Azure strojnog učenja Studio, korisnici mogu kod, R i samo nekoliko klikova objavljivanje kao web-servisa za aplikacije i uređaje trošiti diljem svijeta. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Iz proizvodnje jednostavnih izračuna koji omogućuju statističke funkcije za stvaranje prilagođene predictor tekst dubinsko pretraživanje šalju analizu, nove i iskusnih korisnika R su prednosti olakšani koji korisnici Azure strojnog učenja možete operationalize R kod. Dok je ove web services nije troše korisnicima potencijalno putem mobilne aplikacije ili web-mjesta, svrha u ovim se primjerima web services da biste prikazali kako možete operationalize strojnog učenja R skripte za svrhe, a mogu koristiti za stvaranje web-servisi pri vrhu R kod.

Svaki primjer uključuje C# primjer s za utrošak servisa za web.


![Dijagram R kod u Azure strojnog učenja: R rješenja za zakonom zaštićeno koristiti ili objaviti trgovine Windows Azure.][1]

Razmislite o sljedećim scenarijima.

##<a name="scenario-1-generic-model"></a>Scenarij 1: Generički modela 
Korisnik funkcionira s generički modelu koji se mogu primijeniti na nove korisničke podatke, kao što je osnovni predviđanje podataka niza vremena ili custom-built metodu R s naprednom analitikom. Taj korisnik objavljuje model kao web-servisa da ga drugi mogu zauzeti sa svojim podacima.



* [Binarni klasifikatora](machine-learning-r-csharp-binary-classifier.md)
* [Model klaster](machine-learning-r-csharp-cluster-model.md)
* [Multivariate linearna regresija](machine-learning-r-csharp-multivariate-linear-regression.md)
* [Predviđanje - eksponencijalno izglađivanje](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [Grafičke oznake + STL predviđanja](machine-learning-r-csharp-retail-demand-forecasting.md)
* [Predviđanje - Autoregressive integriran premještanje Average (ARIMA)](machine-learning-r-csharp-arima.md)
* [O analizi](machine-learning-r-csharp-survival-analysis.md)


##<a name="scenario-2-trained-model--specific-data"></a>Scenarij 2: Obučeni model – određene podatke 
Korisnik s podacima koje nudi korisne predviđanja putem koda R, kao što su velike uzorke od osobnost listići o profilu grupirani kroz k znači algoritam za predviđanje osobnost Vrsta korisnika ili stanje upitnik podataka koje je moguće koristiti za predviđanje rizika određene osobe za lung cancer s o analizi R paket. Korisnik objavljuje podataka putem web-servisa koji predviđa rezultat novog korisnika.

##<a name="scenario-3-trained-model--generic-data"></a>Scenarij 3: Obučeni model – generički podataka 
Korisnik ima generički podataka (kao što je tekst corpus) koji omogućuje web-usluge komponenti i primijeniti generically preko različite vrste slučajeva koristi i scenarijima.

* [Leksikon temelji šalju analizu](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

##<a name="scenario-4-advanced-calculator"></a>Scenarij 4: Napredne kalkulator 
Korisnik pruža dodatne izračune ili simulations koja ne zahtijevaju obučeni modela ni staviti modela korisničke podatke.

* [Razlike u testu proporcije](machine-learning-r-csharp-difference-in-two-proportions.md)
* [Paket normalnu distribuciju.](machine-learning-r-csharp-normal-distribution.md)
* [Paket binomnu distribuciju.](machine-learning-r-csharp-binomial-distribution.md)

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje u trgovinu, potražite u članku [ovdje](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png


 
