<properties
    pageTitle="Korak 6: Pristup web-servisa za strojno učenje | Microsoft Azure"
    description="Korak 6 od razvoju vodič predvidljivu rješenja: pristup aktivni Azure strojnog učenja web-servisa."
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


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Vodič koraku 6: Pristup web-servisa Azure strojnog učenja

Ovo je posljednji korak u prikazu [razvoju rješenja predvidljivu analize u Azure strojnog učenja](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Stvaranje radnog prostora za učenje za računala](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Prijenos postojećih podataka.](machine-learning-walkthrough-2-upload-data.md)
3.  [Stvaranje nove eksperiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Obuka i analiza u modelima](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Implementacija web-usluge](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Pristup web-usluge**

----------

U prethodnom koraku u prikazu ćemo uvesti web-servisa koji koristi model za predviđanje rizika naš kreditne kartice. Sada korisnici moraju moći poslati podatke i dobivate rezultate. 

Web-servis je programa Azure web-servisa koji možete primati i vratiti podatke pomoću REST API-ji na jedan od dva načina:  

-   **Zahtjeva i odgovora** - korisniku šalje jedan ili više redaka podataka kreditne kartice sa servisom putem HTTP protokola i servis Odgovori s jedan ili više skupova rezultata.
-   **Grupe za izvršavanje** - korisnik pohranjuje jedan ili više redaka podataka kreditne kartice u blobova platforme Azure i šalje mjesto blob servis. Servis scores sve retke podataka u unos blob, rezultati spremaju u drugom blob i vraća URL-a od njih.  

Predlošci aplikacija za Web u [Trgovina aplikacija za Azure Web](https://azure.microsoft.com/marketplace/web-applications/all/)je najbrži i Najlakši način za pristup web-servisa.
Te predloške web app možete stvoriti prilagođene web-lokacije koje zna ulaznih podataka za web-servisa i što će vratiti. Sve što trebate napraviti je omogućiti pristup web-servisa i podataka, a predložak ne ostale.

Dodatne informacije o korištenju web-aplikacije predložaka potražite u članku [utrošak Azure strojnog učenja web-servisa s predloškom web app](machine-learning-consume-web-service-with-web-app-template.md).

Možete razviti i prilagođena aplikacija za pristup web-servisa pomoću starter kod koji ste dobili u R, C# i Python programskog jezika.
Dovršavanje detalje potražite u [uputama za korištenje u web-servisa Azure strojnog učenja koji je objavljen iz strojnog učenja eksperiment](machine-learning-consume-web-services.md).
