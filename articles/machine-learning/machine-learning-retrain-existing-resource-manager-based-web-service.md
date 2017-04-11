<properties
    pageTitle="Retrain postojeći servis za predvidljivu web | Microsoft Azure"
    description="Saznajte kako retrain modela i ažuriranje web-usluge za korištenje upravo obučeni modela u Azure strojnog učenja."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Postojeći servisi za predvidljivu web retrain

Ovaj dokument opisuje postupak retraining sljedeće scenarija:

* Imate obuka eksperiment i predvidljivu eksperiment koja ste postavila kao operationalized web servis.
* Imate nove podatke koje želite da vaše predvidljivu web-servisa koristite da biste izvršili njegov bilježenje rezultata.

Počevši s postojećeg web-servisa i eksperimenata, morate slijedite ove korake:

1. Ažurirajte modela.
    1. Izmijenite svoje eksperiment tečaj da biste dopustili web servisa unosa i izlaza.
    2. Implementacija pokusa obuka kao retraining web-servisa.
    3. Pomoću pokusa obuka grupe za izvršavanje usluge (BES) retrain modela.
2. Pomoću cmdleta Azure strojnog učenja PowerShell da biste ažurirali predvidljivu pokusa.
    1.  Prijavite se na račun za Azure Voditelj resursa.
    2.  Pronađite definiciju web servisa.
    3.  Izvezite definiciju web servisa kao JSON.
    4.  Ažurirajte referencu blob ilearner u na JSON.
    5.  Uvoz u JSON u definiciju web servisa.
    6.  Ažurirajte web-servisa novu definiciju web servisa.

## <a name="deploy-the-training-experiment"></a>Implementacija pokusa obuka

Da biste implementirali pokusa obuka kao retraining web-servisa, morate dodati unosa servisa web i izlaze modelu. Povezivanjem modula *Izlaz servisa Web* -pokusa * [Modela vlaku] [ train-model] * modula, omogućite pokusa obuka za stvaranje novog obučeni modela koje možete koristiti u vašem predvidljivu eksperiment. Ako imate modul za *Procjenu Model* , možete priložiti web servisa izlaz radi dohvaćanja rezultata za procjenu kao rezultat.

Da biste ažurirali svoje eksperiment obuka:

1. Povezivanje *Web servisa unos* modul unosa podataka (na primjer, *Clean nema podataka* modula). Obično želite biti sigurni ulaznih podataka obrađuje na isti način kao izvorne podatke obuku.
2. Povezivanje modula *Izlaz servisa Web* izlaz modul za *Vlaku modela* .
3. Ako imate modul za *Procjenu modela* , a želite poslati procjenu rezultate, modulu *Izlaz servisa Web* povezati izlaz modul za *Procjenu modela* .

Pokretanje sustava eksperiment.

Nakon toga morate uvesti pokusa obuka kao web-servisa koja daje obučeni modela i model procjenu rezultate.  

Pri dnu područja crtanja eksperiment, kliknite **Postavljanje web-servisa**, a zatim **Implementacija web-servisa [novi]**. Portal za Azure strojnog učenja web-servisi otvara se stranica **Implementacija web-servisa** . Upišite naziv web-servisa, odaberite plan plaćanja, a zatim **Implementiraj**. Način obrade izvođenja možete koristiti samo za stvaranje obučeni modela.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Retrain modela novim podacima pomoću BES

U ovom primjeru priznanje C# za stvaranje retraining aplikacije. Da biste izvršili ovaj zadatak možete koristiti i Python ili R uzorak koda.

Da biste uputili poziv retraining API-ji:

1. Stvaranje konzole aplikacije C# u Visual Studio (**Novo** > **projekta** > **Radnu površinu sustava Windows** > **Aplikacije konzole**).
2.  Prijavite se na portal strojnog učenja Web Services.
3.  Kliknite web-servisa koji radite.
2.  Kliknite **Korištenje**.
3.  Pri dnu stranice **utrošak** , u odjeljku **Uzorak koda** kliknite **grupe**.
5.  Kopirajte ogledne C# kod za obradu izvođenja i zalijepite ih u datoteku Program.cs. Pripazite da prostora za naziv ostaje netaknuta.

Dodavanje paketa NuGet Microsoft.AspNet.WebApi.Client, kao što je navedeno u komentarima. Da biste dodali referenca Microsoft.WindowsAzure.Storage.dll, možda najprije morate instalirati [klijentska biblioteka za pohranu Azure servise](https://www.nuget.org/packages/WindowsAzure.Storage).

Sljedeća slika prikazuje stranicu **utrošak** na portalu servisa Azure strojnog učenja Web Services.

![Korištenje stranica][1]

### <a name="update-the-apikey-declaration"></a>Ažuriranje izvješća apikey

Pronađite **apikey** izvješća:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

U odjeljku **Osnovni potrošnje informacije** stranici **utrošak** pronađite primarni ključ i njezino kopiranje **apikey** deklariranje.

### <a name="update-the-azure-storage-information"></a>Ažuriranje podataka za pohranu za Azure

Ogledni kod BES prenosi datoteke na lokalni pogon (na primjer, "C:\temp\CensusIpnput.csv") Azure prostora za pohranu, obrađuje je i zapisuje rezultate Azure prostora za pohranu.  

Da biste ažurirali podatke Azure prostora za pohranu, morate dohvatiti naziv računa za pohranu, ključ i informacije spremnik za vaš račun za pohranu na portalu Azure klasični, a zatim Ažuriraj correspondi nakon pokretanja eksperiment, rezultirajući tijek rada mora biti sličnu ovoj:

![Rezultata tijeka rada nakon pokretanja][4]vrijednosti pokazuju kod.

1. Prijavite se na portal Azure klasični.
1. U stupcu lijevom navigacijskom oknu kliknite **prostora za pohranu**.
1. Na popisu računa za pohranu, odaberite jednu da biste pohranili retrained modela.
1. Pri dnu stranice kliknite **Upravljanje pristupnih tipki**.
1. Kopiranje i spremanje za **Primarni ključ za pristup** i zatvorite dijaloški okvir.
1. Pri vrhu stranice kliknite **spremnika**.
1. Odaberite postojeći spremnik ili stvorite novi i spremite naziv.

Pronađite *StorageAccountName*, *StorageAccountKey*i *StorageContainerName* deklaracije i ažuriranje vrijednosti koje ste spremili na portalu klasični.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Također morate osigurati Ulazna datoteka je dostupna na mjestu koje ste naveli u kodu.

### <a name="specify-the-output-location"></a>Navedite mjesto za izlazne podatke

Kada odredite mjesto za izlazne podatke u opseg zahtjev, nastavak datoteke koja je navedena u *RelativeLocation* mora biti određena kao `ilearner`. Potražite u sljedećem primjeru:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Slijedi primjer retraining izlaza: ![Retraining Izlaz][6]

## <a name="evaluate-the-retraining-results"></a>Analiza retraining rezultata

Kada pokrenete aplikaciju, izlaz sadrži URL-ove i zajednički pristup potpisa tokena koje su potrebne da biste pristupili procjenu rezultate.

Vidjet ćete rezultate performanse retrained modela kombiniranje *BaseLocation*, *RelativeLocation*i *SasBlobToken* među rezultatima izlaz za *output2* (kao što je prikazano na prethodni retraining izlaz slici) i zalijepite cijeli URL u adresnoj traci preglednika.  

Pregledajte rezultate da biste odredili izvodi hoće li se upravo obučeni model dovoljno dobro da biste zamijenili postojeće.

Kopirajte *BaseLocation*, *RelativeLocation*i *SasBlobToken* u rezultatima izlaz.

## <a name="retrain-the-web-service"></a>Retrain web-usluge

Kada retrain nove web-usluge, ažurirajte definiciju servisa predvidljivu web referentni novog obučeni modela. Servis definiciju web Interna predstavljanje obučeni modela web-usluge i nije izravno mijenjati. Provjerite je li se dohvaćanje servisa definiciju web za vaše predvidljivu eksperiment i ne obuka eksperiment.

## <a name="sign-in-to-azure-resource-manager"></a>Prijavite se u Azure Voditelj resursa

Morate najprije prijave za vaš račun za Azure iz okruženja ljuske PowerShell pomoću cmdleta [Dodaj AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition-object"></a>Dohvati objekt definicije Web servisa

Nakon toga se objekt definicije Web servisa tako da nazovete cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) .

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Da biste utvrdili naziv grupe resursa postojeće web-usluge, pokrenite cmdlet Get-AzureRmMlWebService bez parametre da biste prikazali web-servisi u svoju pretplatu. Pronađite web-servisa, a zatim pogledajte njegov ID servisa web Naziv grupe resursa je četvrti element u polju ID neposredno nakon *resourceGroups* element. U sljedećem primjeru naziv grupe resursa je zadani MachineLearning SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Osim toga, da biste utvrdili naziv grupe resursa postojeće web-usluge, prijavite se na portal za Azure strojnog učenja web-servisi. Odaberite web-servisa. Naziv grupe resursa je element petoj URL web-servisa samo nakon *resourceGroups* element. U sljedećem primjeru naziv grupe resursa je zadani MachineLearning SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Izvoz Web servisa definicija objekt kao JSON

Da biste izmijenili definiciju obučeni modela tako da koristi nove obučeni model, najprije morate pomoću cmdleta [Izvoz AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) Izvoz u datoteku JSON OSNOVNI oblik.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Ažuriranje referencu ilearner blob

U imovine, pronađite [obučeni model], ažurirajte *uri* vrijednost u čvor *locationInfo* URI ilearner blob-om. URI se generira kombiniranjem *BaseLocation* i *RelativeLocation* iz izlaza BES retraining poziv.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>Uvoz u JSON u definiciju Web servisa objekta

[Uvoz AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet morate koristiti ponovno pretvoriti izmijenjenu JSON datoteku definicije Web servisa objekt koje možete koristiti da biste ažurirali predicative pokusa.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Ažuriranje web-usluge

Na kraju, pomoću cmdleta za [Ažuriranje AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) da biste ažurirali predvidljivu pokusa.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
