<properties
    pageTitle="Predvidljivu rješenje odobrenja rizika s strojnog učenja | Microsoft Azure"
    description="Detaljni vodič pokazuje kako stvoriti rješenje predvidljivu analize odobrenja rizika u Azure strojnog učenja Studio."
    keywords="rizik kreditne kartice, predvidljivu analize rješenja, a zatim rizika"
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
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Vodič: Razviti rješenje predvidljivu analize odobrenja rizika u Azure strojnog učenja

Ako trebate predvidjeli s određenom osobom odobrenja rizika na temelju informacija daju na aplikaciji kreditne kartice.  

Odobrenja rizika je složene problem, Naravno, ali ćemo malo Pojednostavnite parametre pitanje. Zatim ćemo ga moći koristiti kao primjera kako možete koristiti Microsoft Azure strojnog učenja s Studio učenje računala i web-servisa za strojno učenje da biste stvorili takvo rješenje predvidljivu analize.  

U ovom detaljni vodič smo ćete pratiti postupka razvoja model predvidljivu analize u strojnog učenja Studio i zatim je implementacija kao na web-servisa Azure strojnog učenja. Ne možemo ćete počinju javno dostupna odobrenja rizika podataka, razvoj uvježbati predvidljivu modela na temelju tih podataka i zatim implementirati model kao web-servisa koje je moguće koristiti drugi za odobrenja rizika.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Preuzeti i ispisati dijagram koji daje pregled mogućnosti strojnog učenja Studio, potražite u članku [Pregled dijagrama Azure strojnog učenja Studio mogućnosti](machine-learning-studio-overview-diagram.md).

Da biste stvorili rješenje procjenu rizika kreditne kartice, ne možemo ćete slijedite ove korake:  

1.  [Stvaranje radnog prostora za učenje za računala](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Prijenos postojećih podataka.](machine-learning-walkthrough-2-upload-data.md)
3.  [Stvaranje nove eksperiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Obuka i analiza u modelima](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Implementacija web-usluge](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Pristup web-usluge](machine-learning-walkthrough-6-access-web-service.md)

Ovaj vodič temelji se na pojednostavnjeni verziju na [Binarni Classfication: Kreditna rizika predviđanje](http://go.microsoft.com/fwlink/?LinkID=525270) eksperimentiraj uzorak u [Galeriju obavještavanje Cortana](http://gallery.cortanaintelligence.com/).
