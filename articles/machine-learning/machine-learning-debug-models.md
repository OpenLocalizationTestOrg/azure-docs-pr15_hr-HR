<properties 
    pageTitle="Ispravljanje pogrešaka modela u Azure strojnog učenja | Microsoft Azure" 
    description="U članku se objašnjava kako upute za ispravljanje pogrešaka u modelu u Azure strojnog učenja." 
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
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>Model u Azure strojnog učenja za ispravljanje pogrešaka

U ovom se članku objašnjava kako se vaše modelima u Microsoft Azure strojnog učenja za ispravljanje pogrešaka. Konkretno, pokriva potencijalne razloga zašto jedan od sljedeća dva scenarija pogrešku može doći prilikom izvođenja modela:

* [Model vlaku] [ train-model] modul throws pogreške 
* [Rezultat Model] [ score-model] modul daje netočne rezultate 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Obuka modela modul throws pogreške

![slika1](./media/machine-learning-debug-models/train_model-1.png)

[Model vlaku] [ train-model] modul očekuje sljedeće unose 2:

1. Vrsta klasifikacija/regresijskog modela iz zbirke modela nudi Azure strojnog učenja
2. Obuka podaci navedeni stupac natpis. Stupac oznakom određuje varijabla za predviđanje. Ostale stupaca uključenih pretpostavlja da su značajke.

U ovom modul throws pogreške u sljedećim slučajevima:

1. Natpis stupca navedena je neispravno jer više od jednog stupca označen kao oznaku ili je odabran pogrešan stupac indeksa. Na primjer, drugom slučaju primjenjivati ako stupac indeksa 30 je koristiti za unos skup podataka koji su samo 25 stupaca.

2. Skup podataka ne sadrži sve značajke stupce. Na primjer, ako unos dataset sadrži samo 1 stupac koji je označen kao stupac natpis, bi postojati nema značajke s kojom za izradu modela. U ovom slučaju [Vlaku modela] [ train-model] modul će vratiti pogrešku.

3. Unos skup podataka (značajke ili natpis) sadrže beskonačno kao vrijednost.


## <a name="score-model-module-does-not-produce-correct-results"></a>Modul rezultat Model neće proizvesti točne rezultate

![slika2](./media/machine-learning-debug-models/train_test-2.png)

Uobičajeni obuka/testiranje grafikon za supervised učenje, [Razdijeljeni podaci] [ split] modul dijeli izvorni skup podataka u dva dijela: dijela koji se koristi za uvježbati modela i dijela koja je rezervirana u rezultatu koliko će se dobro obučeni modela provodi podataka ga niste uvježbati na. Obučeni model koristi se za rezultatu podataka test rezultate nakon kojeg se procjenjuju da biste odredili točnost modela.

[Rezultat Model] [ score-model] modul zahtijeva dva unosa:

1. Obučeni modela Izlaz iz [Modela vlaku] [ train-model] modul
2. Bilježenja rezultata skup podataka ne koje model je put uvježbavan na

Ga se može dogoditi koji iako pokusa uspješnog [Rezultat Model] [ score-model] modul daje netočne rezultate. Da se to dogodi može uzrokovati nekoliko scenarija:

1. Ako navedeni etiketa je categorical i model regresije je put uvježbavan na podacima, pogrešnim izlazom bi će proizvesti [Rezultat Model] [ score-model] modul. To je zato regresije zahtijeva neprekinuti odgovor tjednog prikaza kalendara. U ovom slučaju mora biti prikladniji tako da koristi klasifikacija model. 
2. Isto tako, ako model klasifikacija put je uvježbavan na skup podataka pojavljuju pluta brojeve u stupcu oznake, on može dati rezultate neželjenog. To je zato klasifikacija zahtijeva varijablu samostalni odgovor samo dopušta vrijednosti rasponu konačne i obično Pomalo male skup klase.
3. Ako se bilježenja rezultata skup podataka ne sadrži sve značajke za uvježbati model, a [Rezultat Model] [ score-model] će stvoriti pogrešku.
4. [Rezultat Model] [ score-model] želite proizvesti sve izlazne odgovara retka u skupu podataka bilježenja rezultata koji sadrži nedostaje vrijednost ili beskonačno vrijednost za sve njegove značajke.
5. [Rezultat Model] [ score-model] može dati jednake izlaze za sve retke u bilježenja rezultata skupu podataka. To može postojati, na primjer, u odjeljku pri pokušaju klasifikacija putem odluka šuma ako najmanji broj uzoraka po listovima čvor odabran biti više broj obuka Primjeri dostupna.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
