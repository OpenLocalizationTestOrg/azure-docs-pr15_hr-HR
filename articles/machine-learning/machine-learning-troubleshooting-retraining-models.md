<properties
    pageTitle="Otklanjanje poteškoća s Retraining web-sustava Azure strojnog učenja klasični servisa | Microsoft Azure"
    description="Prepoznajte i ispravljanje uobičajenih problema encounted kada su retraining model za web-servisa Azure strojnog učenja."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Otklanjanje poteškoća s retraining web-sustava Azure strojnog učenja klasični servisa

## <a name="retraining-overview"></a>Pregled retraining

Ako pokrenete predvidljivu eksperiment kao bilježenja rezultata web-servisa je statički modela. Kao nove podatke postane dostupan, ili kada korisnik u API ima vlastite podatke, model mora biti retrained. 

Vodič retraining postupka klasične web-servisa potražite u članku [Retrain strojnog učenja modelima programski](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Retraining postupak

Kada vam je potrebna retrain web-servisa, morate dodati neke dodatne dijelova:

* Web-servisa implementiran putem pokusa obuku. Stalna mora imati modula **Web servisa izlaz** priložiti Izlaz iz modula **Vlaku modela** .  

    ![Priloži izlaz servisa web vlaku model.][image1]

* Krajnju točku dodali bilježenja rezultata web-servisa.  Možete dodati krajnju točku programski pomoću koda za uzorak poziva u modelima Retrain strojnog učenja programski temu ili putem portala za Azure klasični.

Ogledna C# kod sa stranice za pomoć servisa Web za osposobljavanje API-JA možete koristiti da biste retrain modela. Nakon što ste procjenjuje rezultate i zadovoljni s njima, ažurirajte obučeni modela bilježenje rezultata web-servisa pomoću krajnju koju ste dodali.

Svih dijelova na mjestu, važnih koraka koje morate poduzeti da biste retrain model su na sljedeći način:

1.  Poziva web-servisa obuka: poziv je da biste na obradu izvođenja servisa (BES), ne na zahtjev za odgovor servisa (RRS). Ogledni C# kod na stranici pomoći API-JA možete koristiti da biste poziv. 
2.  Traženje vrijednosti za *BaseLocation*, *RelativeLocation*i *SasBlobToken*: ove vrijednosti vraćaju se u Izlaz iz poziva obuka web-servisa. 
      ![prikazuje rezultat retraining uzorak i BaseLocation, RelativeLocation i SasBlobToken vrijednosti.][image6]
3.  Ažuriranje dodane krajnjoj točki bilježenja rezultata web-servisa s novim obučeni modelom: pomoću koje se nalaze u modelima Retrain strojnog učenja programski kod uzorka ažuriranje krajnju koji ste dodali bilježenja rezultata model s upravo obučeni modelom obuka web-servisa.

## <a name="common-obstacles"></a>Uobičajeni prepreke

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Provjerite koristite li točan URL ZAKRPU

Koristite URL ZAKRPU mora biti ona pridruženo novi bilježenja rezultata krajnjoj točki dodali bilježenja rezultata web-servisa. Postoji nekoliko načina dobivanja ZAKRPU URL:

**Mogućnost 1: Programatically**

Da biste dobili točan URL ZAKRPU:

1.  Pokrenite [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) uzorak koda.
2.  Iz izlaza AddEndpoint, pronaći vrijednost *HelpLocation* i kopirajte URL-a.

    ![HelpLocation u izlaz addEndpoint uzorka.][image2]

3.  Zalijepite URL u pregledniku da biste došli do stranice na kojoj se navode veze za pomoć za web-servisa.
4.  Kliknite vezu **Ažuriraj resursa** da biste otvorili stranicu pomoć zakrpu.

**Mogućnost 2: Korištenje portala za Azure klasični**

1.  Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2.  Otvorite karticu strojnog učenja. 
     ![Kartica leaning računala.][image4]
3.  Kliknite naziv radnog prostora, zatim **Web-servisa**.
4.  Kliknite bilježenja rezultata radite s web-servisa. (Ako ne uspijete izmijenili zadani je naziv web-servisa, ona će se zaustaviti u [bilježenje rezultata Exp]..)
5.  Kliknite **Dodaj krajnjoj točki**.
6.  Nakon dodavanja krajnju točku, kliknite naziv krajnje točke. Kliknite **Ažuriraj resursa** da biste otvorili kojemu stranicu pomoć.

>[AZURE.NOTE] Ako krajnju točku ste dodali u web-servisa obuka umjesto predvidljivu web-servisa, primit ćete sljedeću pogrešku prilikom klika na vezu **Ažuriraj resursa** : Nažalost, ali ta značajka nije podržana ili nije dostupan u ovom kontekstu. U ovom web-servisa sadrži resursa koji se mogu ažurirati. Ne možemo apologize za neugodnosti i se radi o poboljšanju ovaj tijek rada.

![Nova nadzorna ploča krajnjoj točki.][image3]

Stranice pomoći ZAKRPU sadrži ZAKRPU URL-a morate koristiti i njihovi uzorak koda možete koristiti da biste ga pozovite.

![URL zakrpu.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Provjerite ažurirate točan bilježenja rezultata krajnje točke

* Zakrpu web-servisa obuka: operacija zakrpu moraju izvršiti na bilježenja rezultata web-servisa.
* Zakrpu zadane krajnje točke na web-servisa: operacija zakrpu moraju izvršiti na novu bilježenja rezultata Web krajnju točku usluge koje ste dodali.

Možete provjeriti koji web-servisa na krajnjoj točki nalazi na web-mjestu portala za Azure klasični. 

>[AZURE.NOTE] Provjerite je li dodajete krajnju točku predvidljivu web-uslugu, ne web-servisa obuku. Ako ste pravilno implementiran na obuka i predvidljivu web-servisa, trebali biste vidjeti dva zasebna web-servisi na popisu. "[Predvidljivu exp.]" mora završavati predvidljivu web-servisa.

1.  Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2.  Otvorite karticu strojnog učenja. 
     ![Strojnog učenja radnog prostora korisničkog Sučelja.][image4]
3.  Odaberite radni prostor.
4.  Kliknite **web-servisa**.
5.  Odaberite servis za predvidljivu Web.
6.  Provjerite je li vaša krajnju točku dodan web-servisa.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Provjera radnog prostora koji web-servisa se da biste bili sigurni da je u području ispravne

1.  Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2.  Na izborniku odaberite strojnog učenja.
      ![Učenje područje korisničko Sučelje na računalu.][image4]
3.  Provjerite mjesto radnog prostora.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png