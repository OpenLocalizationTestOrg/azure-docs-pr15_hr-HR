<properties
    pageTitle="Retrain novog web-servisa pomoću cmdleta ljuske PowerShell za upravljanje učenje za strojno | Microsoft Azure"
    description="Saznajte kako programski retrain modela i ažuriranje web-usluge za korištenje upravo obučeni modela u Azure strojnog učenja pomoću cmdleta ljuske PowerShell za upravljanje učenje za računala."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Retrain novog web-servisa pomoću cmdleta ljuske PowerShell za upravljanje učenje za računala

Kada retrain nove web-usluge, ažurirajte definiciju servisa predvidljivu web referentni novog obučeni modela.  

## <a name="prerequisites"></a>Preduvjeti

Morate ste postavili obuka eksperiment i predvidljivu eksperiment kao što je prikazano u [Retrain strojnog učenja modelima programski](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Predvidljivu pokusa mora biti implementirano kao programa upravitelj resursa za Azure (Novo) koji se temelje strojnog učenja web-servisa. 
 
Dodatne informacije o web-servisi Deploying potražite u članku [uvođenja u web-servisa Azure strojnog učenja](machine-learning-publish-a-machine-learning-web-service.md).

Ovaj postupak zahtijeva Provjerite jeste li instalirali cmdleti za učenje za strojno Azure. Informacije o instalacija cmdleta strojnog učenja potražite u članku referenca za [Azure strojnog učenja cmdleta](https://msdn.microsoft.com/library/azure/mt767952.aspx) na MSDN-u.

Kopirana retraining izlaz sljedeće informacije:

* BaseLocation
* RelativeLocation

Korake koje poduzmete su:

1.  Prijavite se na račun za Azure Voditelj resursa.
2.  Početak definiciju web servisa
3.  Izvoz servisa definiciju Web kao JSON
4.  Ažurirajte referencu blob ilearner u na JSON.
5.  Uvoz u JSON u definiciju Web servisa
6.  Ažurirajte web-servisa novu definiciju Web servisa

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Prijavite se na račun za Azure Voditelj resursa

Mora se najprije prijaviti u Azure računu iz okruženja ljuske PowerShell pomoću cmdleta [Dodaj AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition"></a>Početak definiciju Web servisa

Nakon toga se web-servisa tako da nazovete cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) . Servis definiciju Web Interna predstavljanje obučeni modela web-usluge i nije izravno mijenjati. Provjerite je li se dohvaćanje servisa definiciju Web za vaše predvidljivu eksperiment i ne obuka eksperiment.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Da biste utvrdili naziv grupe resursa postojeće web-usluge, pokrenite cmdlet Get-AzureRmMlWebService bez parametre da biste prikazali web-servisi u svoju pretplatu. Pronađite web-servisa, a zatim pogledajte njegov ID servisa web Naziv grupe resursa je četvrti element u polju ID neposredno nakon *resourceGroups* element. U sljedećem primjeru naziv grupe resursa je zadani MachineLearning SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Osim toga, da biste utvrdili naziv grupe resursa postojeće web-usluge, prijavite se na portal sustava Microsoft Azure strojnog učenja Web Services. Odaberite web-servisa. Naziv grupe resursa je element pete URL web-servisa samo nakon *resourceGroups* element. U sljedećem primjeru naziv grupe resursa je zadani MachineLearning SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Izvoz servisa definiciju Web kao JSON

Da biste izmijenili definiciju obučeni model da biste koristili upravo obučeni Model, morate koristiti cmdlet [Izvoz AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) da biste izvezli JSON format datoteke.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Ažurirajte referencu blob ilearner u na JSON.

U imovine, pronađite [obučeni model], ažurirajte *uri* vrijednost u čvor *locationInfo* URI ilearner blob-om. URI se generira kombiniranjem *BaseLocation* i *RelativeLocation* iz izlaza BES retraining poziv.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition"></a>Uvoz u JSON u definiciju Web servisa

[Uvoz AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet morate koristiti ponovno pretvoriti izmijenjenu JSON datoteku definicije Web usluga koje možete koristiti da biste ažurirali pokusa Predicative.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Ažurirajte web-servisa novu definiciju Web servisa

Na kraju, koristite cmdlet [Ažuriranje AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) da biste ažurirali predvidljivu pokusa.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Sažetak

Korištenje strojnog učenja PowerShell cmdleti za upravljanje, možete ažurirati obučeni model predvidljivu web-servisa, omogućivanjem scenarija kao što su:

* Povremeno model retraining novim podacima.
* Distribucija modela kupcima s ciljem da ih retrain modela pomoću vlastitih podataka.
