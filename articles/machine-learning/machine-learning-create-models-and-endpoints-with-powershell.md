<properties
pageTitle="Stvaranje više modela iz jednog eksperiment | Microsoft Azure"
description="Pomoću komponente PowerShell da biste stvorili više modela učenje računala i web-servisa krajnje točke s istom algoritam, ali obuka različite skupove podataka."
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
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Stvaranje mnogo modelima učenje računala i web-krajnje točke servisa iz jednog eksperiment pomoću komponente PowerShell

Evo uobičajenih problema učenje računalu: želite li stvoriti mnogo različitih modela koji imaju isti tijek rada za obuku i koristiti isti algoritam, ali ste obuka različite skupove podataka kao ulaznih. U ovom se članku objašnjava da biste to učinili na razini Azure strojnog učenja Studio pomoću samo jedan eksperiment.

Na primjer, recimo da posjedujete globalni bicikle najam franšizne poslovanja. Želite da biste sastavili model regresije za predviđanje najam zahtjev koji se temelji na povijesnim podacima. Imate 1000 najam mjesta širom svijeta i ste prikupili skup podataka za svaku mjesto koje sadrži važne značajke kao što su datum, vrijeme, vremenske prognoze i promet koji se odnose na svakom mjestu.

Nije moguće uvježbati modela jednom pomoću verzije programa sve skupove podataka za spojene preko sva mjesta. No jer svaki od lokacija ima jedinstvene okruženja, bolje pristup bio uvježbati modela regresije zasebno za svaku lokaciju pomoću skupu podataka. Na taj način svaki obučeni model može potrajati u obzir različite spremište veličine, glasnoću, Zemljopis, populacije, prikladna za bicikle promet okruženja, *itd.*.

Koje možda najbolji način, ali ne želite da biste stvorili 1000 eksperimenata obuka u Azure strojnog učenja svaki od njih koji predstavlja jedinstveni mjesto. Osim što izvršilo zadatka, preporučuje se i čini vrlo Neučinkovit jer se svaki eksperiment imati iste komponente osim obuka skupu podataka.

Srećom, ne možemo možete postići pomoću [Azure strojnog učenja retraining API-JA](machine-learning-retrain-models-programmatically.md) i Automatiziranje zadataka s [Azure strojnog učenja PowerShell](machine-learning-powershell-module.md).

> [AZURE.NOTE] Da biste oglednim ubrzati, možemo ćete smanjiti broj mjesta od 1000 do 10. No načela i postupke odnose se na mjesta 1000. Jedina razlika je da ako želite uvježbati od 1000 skupove podataka vjerojatno želite zamislite da je paralelno pokretanje sljedeće skripte komponente PowerShell. Kako to učiniti izlazi iz ovog članka, ali ne možete pronaći Primjeri PowerShell usporedno izvršavanje zadataka na Internetu.  

## <a name="set-up-the-training-experiment"></a>Postavljanje pokusa obuka

Ne možemo ćete koristiti na primjer [isprobajte tečaj](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) koji smo već stvorene u [Galeriju obavještavanje Cortana](http://gallery.cortanaintelligence.com). Otvorite ovaj eksperiment u radnom prostoru [Azure strojnog učenja Studio](https://studio.azureml.net) .

>[AZURE.NOTE] Da bi se popratne u ovom primjeru, trebali biste koristiti standardne radnog prostora umjesto slobodnog prostora. Ne možemo ćete stvoriti jednu krajnja točka za svakog korisnika – za Ukupno 10 krajnje točke - i koje je potrebno standardne radnog prostora Budući da je ograničeno na 3 krajnje točke slobodnog prostora. Ako imate samo slobodnog prostora, jednostavno izmijeniti skripte u nastavku da biste dopustili samo 3 mjesta.

Stalna koristi modul za **Uvoz podataka** da biste uvezli dataset obuka *customer001.csv* s računa za Azure prostora za pohranu. Recimo da bismo obuka skupove podataka prikupljenih sva mjesta bicikle najam i ih pohranjena na isto mjesto spremišta blobova platforme s nazivima datoteka u rasponu od *rentalloc001.csv* do *rentalloc10.csv*.

![Slika](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Imajte na umu da je dodana modula **Izlaz servisa Web** modul **Vlaku modela** .
Kada je ova eksperiment implementiran kao web-servisa, krajnju točku pridružene izlaz će vratiti obučeni modela u obliku datoteke .ilearner.

Imajte na umu da ne možemo postaviti parametar web-usluge za URL koji koristi modul za **Uvoz podataka** . Time se omogućuje nam kako koristiti parametar da biste odredili pojedinačne obuka skupove podataka da biste uvježbati model za svaku lokaciju.
Postoje drugi načini smo nije to učinite, kao što su za dohvaćanje podataka iz baze podataka SQL Azure SQL upita pomoću parametar web-usluge ili jednostavno pomoću modula **Web servisa unos** za prosljeđivanje u skup podataka web-servisa.

![Slika](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Sada ćemo pokrenite ovaj tečaj eksperiment pomoću vrijednost zadane *rental001.csv* kao skup podataka za obuku. Ako pogledate Izlaz iz modula **procijeni** (kliknite Izlaz i odaberite **Vizualiziraj**), vidjet ćete da se koristiti decent performanse *AUC* = 0.91. Sada ćemo spremni ste za implementaciju web-servisa iz ovaj tečaj eksperiment.

## <a name="deploy-the-training-and-scoring-web-services"></a>Implementacija u obuka i bilježenje rezultata web-servisi

Za implementaciju web-servisa za osposobljavanje, kliknite gumb **Postavite web-servisa** ispod područja crtanja eksperiment i odaberite **Implementacija web-servisa**. Nazovite ovo web-servisa "" bicikle najam obuka".

Sada ćemo morati implementacija bilježenja rezultata web-servisa.
Da biste to učinili, ne možemo možete **Postaviti web-servisa** ispod kliknite područje crtanja i odaberite **Predvidljivu web-servisa**. Time se stvara bilježenja rezultata eksperiment.
Potrebna prilagodbe nekoliko pomoćna za rad kao web-servisa, kao što su uklanjanje natpisa stupca "cnt" iz ulazne podatke i ograničavanje izlazne podatke da bi samo id instance i odgovarajući predviđene vrijednosti.

Da biste spremili sami koji funkcioniraju, jednostavno možete otvoriti [predvidljivu eksperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) u galeriji koji je već pripremljeni.

Za implementaciju web-servisa, pokrenite predvidljivu pokusa, a zatim kliknite gumb **Implementacija web-servisa** ispod područje crtanja. Naziv bilježenja rezultata web-servisa "Bilježenje rezultata bicikle najam" ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Stvaranje 10 krajnje točke servisa identične web pomoću komponente PowerShell

U ovom web-servisa u sklopu zadane krajnje točke. No ne možemo zanima ne kao zadana krajnja točka jer nije moguće ažurirati. Što mi treba je da biste stvorili 10 dodatne krajnje točke, jedan za svaku lokaciju. To ćemo učiniti sa servisom PowerShell.

Najprije ćemo postavljanje naš okruženja ljuske PowerShell:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Zatim, pokrenite sljedeću naredbu komponente PowerShell:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Sada ćemo ste stvorili 10 krajnje točke, a svi oni sadrže isti model obučeni obučavanju članova na *customer001.csv*. Možete pogledati na portalu za upravljanje Azure.

![Slika](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Ažuriranje krajnje točke za korištenje skupova podataka zasebnom obuka pomoću komponente PowerShell

Sljedeći je korak da biste ažurirali krajnjih točaka s modelima jedinstveno obučavanju članova na pojedinačne podaci za svakoga od njih. No najprije moramo čime se dobiva te modela iz web-servisa za **Bicikle najam obuku** . Podsjetimo se natrag u web-servisa za **Bicikle najam obuka** . Ne možemo morati nazvati njezin krajnju točku BES 10 puta s 10 obuka različite skupove podataka da bi se dobiva 10 različitim modelima. Cmdlet ljuske PowerShell **InovkeAmlWebServiceBESEndpoint** ćemo koristiti da biste to učinili.

Također morat ćete unijeti vjerodajnice za vaš račun spremište blobova platforme u `$configContent`, odnosno, u polja `AccountName`, `AccountKey` i `RelativeLocation`. Na `AccountName` može biti jedna od naziva računa u **Klasični Portal za upravljanje Azure** (*prostora za pohranu* kartica). Kada kliknete na račun za pohranu, njegov `AccountKey` možete pronaći tako da pritisnete gumb **Upravljanje pristupnih tipki** pri dnu i kopiranje *Primarni ključ za Access*. U `RelativeLocation` je put odnosu prostora za pohranu pohranjuju novog modela. Na primjer, put `hai/retrain/bike_rental/` u skripti ispod točaka spremniku pod nazivom `hai`, i `/retrain/bike_rental/` su podmape. Trenutno nije moguće stvoriti podmape putem portala za korisničko Sučelje, ali postoji [Nekoliko Azure pohranu programu Software Explorer](../storage/storage-explorers.md) omogućuju vam da biste to učinili. Preporučuje se stvaranje novog spremnika u prostor za pohranu za spremanje nove obučeni modele (.ilearner datoteke) na sljedeći način: sa stranice za pohranu, kliknite gumb **Dodaj** pri dnu i nazovite ih `retrain`. Ukratko, potrebne promjene u nastavku skriptu odnose na `AccountName`, `AccountKey` i `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] Krajnja točka BES je način samo podržani za ovaj postupak. RRS nije moguće koristiti za proizvodnju obučeni modela.

Kao što vidite, umjesto izgradnje 10 različitih BES posao konfiguracije json datoteka, ne možemo dinamički umjesto stvorite niz config i sadržaja parametar *jobConfigString* **InvokeAmlWebServceBESEndpoint** cmdlet jer zaista nema potrebe da biste zadržali kopiju na disku.

Ako je sve dobro prošlo, nakon nekog vremena trebali biste vidjeti 10 .ilearner datoteke iz *model001.ilearner* *model010.ilearner*na vašem računu Azure prostora za pohranu. Sada ćemo spremni ste za ažuriranje naš 10 bilježenja rezultata web krajnje točke servisa te modela pomoću cmdleta ljuske PowerShell **Zakrpu AmlWebServiceEndpoint** . Nemojte zaboraviti ponovno da ne možemo možete samo zakrpu krajnje točke koji nisu zadani smo programski ste ranije stvorili.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Brzo prilično trebale bi funkcionirati to. Kada se dovrši izvođenje, ćete uspješno stvorili smo 10 predvidljivu web krajnje točke servisa, svaka sadrži obučeni modela jedinstveno obučavanju članova na određeni skup podataka na mjesto najam sve iz jedne obuka eksperiment. Da biste to provjerili, možete pokušati pozivanje te krajnje točke pomoću cmdleta **InvokeAmlWebServiceRRSEndpoint** koja omogućuje s istim podacima unos, a možete očekivati da biste vidjeli rezultate različite predviđanje jer su u modelima put uvježbavan s različitim obuka skupovima.

## <a name="full-powershell-script"></a>Potpuna skriptu komponente PowerShell

Slijedi popis šifru cijelog izvora:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
