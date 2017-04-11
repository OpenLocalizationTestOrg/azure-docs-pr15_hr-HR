<properties
    pageTitle="Stvaranje krajnje točke servisa za Web u strojnog učenja | Microsoft Azure"
    description="Stvaranje krajnje točke servisa za Web u Azure strojnog učenja"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Stvaranje krajnje točke

>[AZURE.NOTE] U ovoj se temi opisuju tehnike odnosi se na klasične web-servisa.

Kada stvorite web-usluge koje prodajete Proslijedi klijentima, morate pružanje obučeni modela svakog klijenta koji i dalje povezan s pokusa iz kojeg je stvorena web-servisa. Osim toga, sva ažuriranja pokusa zatvara selektivno krajnje bez prebrisivanja prilagodbe.

Da biste to postigli, Azure strojnog učenja omogućuje stvaranje više krajnje točke za web-usluga uvodi. Svaki krajnje točke u web-servisa neovisno je adresirana, ograničio vrijeme i upravlja. Svaki krajnje je jedinstveni URL i autorizacije ključ koji možete raspodijeliti klijentima.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Dodavanje krajnje točke na web-servisa

Tri su načina za dodavanje krajnje web-servisa.

* Programski
* Putem portala za Azure strojnog učenja web-servisi
* Iako Azure klasični portal

Nakon stvaranja krajnju točku možete zauzeti kroz sinkrono API-ji, obradu API-ji, i radni listovi programa excel. Osim dodavanja krajnje točke kroz ovaj korisničkog Sučelja, možete koristiti i API-ji za upravljanje krajnjoj točki programski dodavanje krajnje točke.

 >[AZURE.NOTE] Ako ste dodali dodatni krajnje točke u web-servisa, nećete moći izbrisati zadane krajnje točke.

## <a name="adding-an-endpoint-programmatically"></a>Dodavanje krajnje programski

Krajnje možete dodati na web-servisa programski pomoću koda za [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) uzorka.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Dodavanje krajnje pomoću portala za Azure strojnog učenja web-servisi

1. U strojnog učenja Studio stupac lijevom navigacijskom oknu kliknite web-servisa.
2. Pri dnu zaslona nadzornu ploču servisa za Web, kliknite **Upravljanje krajnje točke**. Otvorit će se na portalu servisa Azure strojnog učenja Web Services na stranici krajnje točke za web-servisa.
3. Kliknite **Novo**.
4. Upišite naziv i opis nove krajnjoj točki. Krajnja točka imena mora biti 24 znakova ili manje duljina i mora se sastoji od malih pismima ili brojeve. Odaberite željenu razinu zapisivanja i je li omogućeno ogledne podatke. Dodatne informacije o zapisivanje potražite u članku [omogućite zapisivanje za strojnog učenja Web services](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Dodavanje krajnje pomoću portala za Azure klasični


1. Prijava na [portal za Azure klasični](http://manage.windowsazure.com), kliknite **Strojnog učenja** u lijevom stupcu. Kliknite radni prostor koji sadrži web-servisa na koji vas zanima.

    ![Dođite do radnog prostora](./media/machine-learning-create-endpoint/figure-1.png)

2. Kliknite **web-servisa**.

    ![Dođite do web-usluge](./media/machine-learning-create-endpoint/figure-2.png)

3. Kliknite web-servisa koji vas zanima da biste vidjeli popis dostupnih krajnje točke.

    ![Dođite do krajnje točke](./media/machine-learning-create-endpoint/figure-3.png)

4. Pri dnu stranice kliknite **Dodavanje krajnjoj točki**. Upišite naziv i opis, provjerite postoje drugi krajnje točke s istim nazivom u ovom web-servisa. Osim ako imate posebne zahtjeve, ostavite razinu ograničenja s njezina je zadana vrijednost. Da biste saznali više o ograničavanje, u odjeljku [Skaliranje krajnje točke API -JA](machine-learning-scaling-webservice.md).

    ![Stvaranje krajnje točke](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Daljnji koraci

[Kako se koristi objavljeni servisa Azure strojnog učenja Web](machine-learning-consume-web-services.md).
