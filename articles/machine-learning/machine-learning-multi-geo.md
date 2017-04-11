<properties
   pageTitle="Dokumentacija za pomoć višestruki-zemlj. | Microsoft Azure"
   description="Saznajte kako stvoriti radni prostor i objavljivanje web-servisa u različitim iz na Jug središnje Sjedinjenih Američkih Država (SCUS) programa Azure regiji Azure regija."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Dokumentacija za pomoć višestruki-zemlj.

U ovom se članku opisuje kako možete stvoriti radni prostor i objavljivanje web-servisa u drugim regijama Azure.

## <a name="create-a-workspace"></a>Stvaranje radnog prostora

1. Prijavite se na portalu za Azure klasični.

2.  Kliknite **+ NOVO** > **DATA SERVICES** > **STROJNOG UČENJA** > **brzo stvaranje**.  U odjeljku **mjesta** odaberite neke druge regije, kao što su **Jugoistočne Azije**.
![Pomoć za višestruki-zemlj slika 1][1]
3. Odaberite radni prostor, a zatim kliknite **Prijava ML Studio**.
![Pomoć za višestruki-zemlj slika 2][2]

4. Sada imate radni prostor u drugoj regiji koje možete koristiti baš kao i radni prostor. Da biste se prebacivali između radnih prostora, pogledajte u gornjem desnom kutu zaslona. Kliknite padajući izbornik, odaberite područje, a zatim radni prostor. Sve je lokalni s područjem radnog prostora Ako, na primjer, sve svoje web-servisi stvorene iz radnog prostora bit će u istom radni prostor nalazi se u području.
![Pomoć za višestruki-zemlj slika 3][3]

## <a name="open-an-experiment-from-gallery"></a>Otvorite pokusa iz galerije

Ako otvorite pokusa iz galerije, možete odabrati i koje područje koje želite kopirati pokusa da biste.

![Pomoć za višestruki-zemlj slika 4][4a]

## <a name="web-service-management"></a>Upravljanje servisom za web

Da biste programski Upravljanje web-usluge, kao što su za retraining, koristite adresu namijenjena određenoj regiji:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Napomena

1.  Možete kopirati samo eksperimenata između radne prostore koji se nalaze u istom području na taj način. Ako vam je potrebna da biste kopirali eksperiment preko radne prostore u različitim područjima cmdleta [ljuske PowerShell](http://aka.ms/amlps) [*Kopiraj AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) možete koristiti da biste izvršili koji. Drugi zaobilazno rješenje je za objavljivanje pokusa u galeriju u nenavedenim načinu rada, otvorite je u radnom prostoru iz drugih područja.
2.  Birač regija će prikazati samo radne prostore za određenu regiju odjednom. U budućnosti moći da biste vidjeli cijeli popis radni prostori u kojima imate pristup svim sve regije u isto vrijeme.  
3.  Slobodni prostor ili pristup gosta (anonimno) radnog prostora bit će stvoren i smješten u Jug središnje SAD-a U budućnosti moći za stvaranje radnih prostora za pristup slobodnom/gosta u području koje odaberete.  
4.  Web-servisi implementiran putem radni prostor u Jugoistočne Azije će se nalaziti u Jugoistočne Azije. U budućnosti moći ćete imati mogućnost stvaranja eksperimenata u jednom području i implementacija generira krajnje točke servisa za web u različite regije.  

## <a name="more-information"></a>Dodatne informacije

Postavite pitanje na [forumu Azure strojnog učenja](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
