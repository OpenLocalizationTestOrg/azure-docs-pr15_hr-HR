<properties 
    pageTitle="Prijava za web-servise strojnog učenja | Microsoft Azure" 
    description="Saznajte kako omogućiti zapisivanje za web-servise strojnog učenja. Zapisivanje pruža dodatne informacije za pomoć pri otklanjanju API-ji." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Omogućite zapisivanje za strojnog učenja web-usluge  

Ovaj dokument pruža informacije na mogućnost zapisivanje klasične web-servisa. Omogućavanje zapisivanje u web-servisi pruža dodatne informacije, osim samo broja pogreške i poruke koje mogu pomoći pri otklanjanju poteškoća s pozive strojnog učenja API-ji.  

Da biste omogućili zapisivanje u web-servisa na portalu za Azure klasični:   

1.  Prijavite se na [portal za klasični Azure](https://manage.windowsazure.com/)
2.  U stupcu lijevom značajke kliknite **STROJNOG UČENJA**.
3.  Kliknite radni prostor, zatim **Web-SERVISA**.
4.  Na popisu Web services, kliknite naziv web-servisa.
5.  Na popisu krajnje točke kliknite naziv krajnje točke.
6.  Kliknite **KONFIGURIRAJ**.
7.  Postavljanje **Razine PRAĆENJE DIJAGNOSTIKA** *pogreške* ili *sve*, a zatim kliknite **SPREMI**.

Da biste omogućili zapisivanje na portalu servisa Azure strojnog učenja Web Services.

1. Prijavite se na portalu [Servisa Azure strojnog učenja Web Services](https://services.azureml.net) .
2. Kliknite klasični Web Services.
3.  Na popisu Web services, kliknite naziv web-servisa.
4.  Na popisu krajnje točke kliknite naziv krajnje točke.
5.  Kliknite **Konfiguriraj**.
6.  Postavite **bilježenje u zapisnik** *pogrešaka* ili *sve*, a zatim kliknite **SPREMI**.

## <a name="the-effects-of-enabling-logging"></a>Efekti omogućivanja zapisivanja

Kada je bilježenje omogućeno, dijagnostiku i pogreške od odabranih krajnje točke prijavljen na račun za pohranu Azure povezani s radnim prostorom za korisnika. Vidjet ćete taj račun za pohranu Azure klasični portalu prikaz nadzorne ploče (dno odjeljka brzi Glance) njihove radnog prostora.  

Zapisnike moguće je prikazati na bilo koji od nekoliko alata dostupnih "Istraživanje" račun za Azure prostora za pohranu. Najlakši mogla bi biti samo idite na račun za pohranu na portalu za Azure klasični, a zatim **SPREMNIKA**. Zatim vidjeti kontejner **ml Dijagnostika**. Ovaj spremnik sadrži sve podatke za dijagnostiku za sve web servisa krajnjih točaka za sve radne prostore povezan s ovim računom za pohranu. 
 
## <a name="log-blob-detail-information"></a>Detaljne informacije o zapisniku blobova platforme

Svaki blob u spremniku nalaze informacije Dijagnostika za točno nešto od sljedećeg:

-   Za izvršavanje način obrade izvođenja  
-   Za izvršavanje metodu odgovor na zahtjev  
-   Pokretanje spremnika odgovor na zahtjev
  
Naziv svake blob sadrži prefiks obrasca za sljedeće: 

{Radni prostor Id}-{web-servis Id}-{Id krajnje točke} / {Vrsta dnevnika}  

Gdje je vrsta zapisnika jednu od sljedećih vrijednosti:  

- grupe  
- rezultat/zahtjeva  
- rezultat/init  