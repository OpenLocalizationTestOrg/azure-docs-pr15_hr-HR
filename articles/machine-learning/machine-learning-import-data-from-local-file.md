<properties
    pageTitle="Uvoz podataka u strojnog učenja Studio iz lokalne datoteke | Microsoft Azure"
    description="Upute za uvoz podataka obuka Azure strojnog učenja Studio iz lokalne datoteke."
    keywords="Uvoz podataka, oblik podataka, vrste podataka, izvora podataka, obuka podataka"
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
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Uvoz podataka obuka u Azure strojnog učenja Studio iz lokalne datoteke

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Da biste svoje podatke iz strojnog učenja Studio, možete prenijeti datoteku podataka na vrijeme s tvrdog diska da biste stvorili modula skup podataka u radnom prostoru. 


## <a name="import-data-from-a-local-file"></a>Uvoz podataka iz lokalne datoteke

Uvoz podataka iz lokalnog tvrdog diska tako da učinite sljedeće:

1. Kliknite **+ NOVO** pri dnu prozora Studio strojnog učenja.
2. Odaberite **skup podataka** i **iz LOKALNE datoteke**.
3. U dijaloškom okviru **Prijenos novi skup podataka** dođite do datoteke koju želite prenijeti
4. Unesite naziv, vrstu podataka za prepoznavanje i po želji unesite opis. Preporučuje se opis – omogućuje snimanje sve značajke o podacima koji se želite Imajte na umu prilikom korištenja podataka u budućnosti.
5. Potvrdni okvir **Ovo je nova verzija postojeći skup podataka** omogućuje vam da biste ažurirali postojeći skup podataka novim podacima. Samo kliknite ovaj okvir, a zatim unesite naziv postojećeg skupa podataka.

Tijekom prijenosa, vidjet ćete poruku da se prenosi datoteku. Prijenos vrijeme ovisi o veličini vaših podataka i brzine veze sa servisom.
Ako znate da datoteku će dugo trajati, to možete učiniti ostalog unutar strojnog učenja Studio tijekom čekanja. Međutim, zatvorite preglednik uzrokovat će prijenos podataka nije uspjela.

Nakon prijenosa podataka pohranjena u modulu skup podataka i je dostupan za sve eksperiment u radnom prostoru.
Ako uređujete pokusa, možete pronaći skupove podataka koji ste stvorili u popisu **Moje skupova podataka** na popisu **Spremili skupove podataka** u paleti modulu. Možete povucite i ispustite skupa podataka na područje crtanja eksperiment kada želite koristiti zadani skup podataka za daljnje analize i strojnog učenja.



