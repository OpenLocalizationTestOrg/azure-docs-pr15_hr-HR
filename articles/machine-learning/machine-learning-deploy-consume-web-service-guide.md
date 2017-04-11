<properties
    pageTitle="Azure strojnog učenja web-servisi: Uvođenje i potrošnje | Microsoft Azure"
    description="Resursi za implementaciju i drugim web-servisa."
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
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure strojnog učenja web-servisi: Uvođenje i potrošnje

Azure strojnog učenja možete koristiti za implementaciju tijekovi rada i modela kao web-servise strojnog učenja. Ove web-servisi možete se koristiti za poziv modela strojnog učenja iz aplikacije putem Interneta da biste učinili predviđanja u stvarnom vremenu ili u načinu rada za obradu. Budući da web-servisi RESTful, možete ih nazvati iz različitih programskog jezika i platforme, kao što su .NET i Java, i drugih aplikacija, kao što je Excel.

Sljedeći odjeljci sadrže veze na vodiči, kod i u dokumentaciji za jednostavniji početak rada.

## <a name="deploy-a-web-service"></a>Implementacija web-servisa

### <a name="with-azure-machine-learning-studio"></a>S Azure strojnog učenja Studio

Strojnog učenja Studio portal sustava Microsoft Azure strojnog učenja web-servisi omogućuju uvođenje i upravljanje web-servisa bez pisanja koda.

Na sljedećim vezama navedene opće informacije o korištenju za implementaciju nove web-usluge:

* Da biste saznali kako uvesti novog web-servisa koji se temelji na Azure upravljanja resursima potražite u članku [uvođenja novog web-servisa](machine-learning-webservice-deploy-a-web-service.md).
* Objašnjenje kako implementirati web-servisa, potražite u članku [uvođenja u web-servisa Azure strojnog učenja](machine-learning-publish-a-machine-learning-web-service.md).
* Potpuna objašnjenje kako stvoriti i implementirati web-servisa potražite u članku [vodič korak 1: stvaranje radnog prostora strojnog učenja](machine-learning-walkthrough-1-create-ml-workspace.md).
* Za određene Primjeri implementacija web-servisa, pogledajte:

    * [Vodič korak 5: Implementacija web-servisa Azure strojnog učenja](machine-learning-walkthrough-5-publish-web-service.md)
    * [Kako implementirati web-servisa na više područja](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Kod davatelja usluga resursa web services API-ji (Azure resursima API-ji)

Azure strojnog učenja davatelja resursa za web-usluge omogućuje implementacije i upravljanja web-servisa pomoću pozive REST API-JA. Dodatne informacije potražite u članku referenca [Strojnog učenja Web usluge (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) na MSDN-u.

### <a name="with-powershell-cmdlets"></a>Pomoću cmdleta ljuske PowerShell

Azure davatelja strojnog učenja resursa za web-servisi omogućuju implementacije i upravljanja web-servisa pomoću cmdleta ljuske PowerShell.

Da biste koristili s cmdletima, morate najprije prijave u vaš račun za Azure iz okruženja ljuske PowerShell pomoću cmdleta [Dodaj AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) . Ako niste upoznati s postupkom da biste nazvali naredbe ljuske PowerShell koji se temelje na Voditelj resursa, potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Da biste izvezli vaše predvidljivu eksperiment, koristite [kod uzorka](https://github.com/ritwik20/AzureML-WebServices). Kada stvorite .exe datoteke iz kod, možete upisati:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Pokretanje aplikacije stvara web-servisa JSON predložak. Da biste pomoću predloška za implementaciju web-servisa, morate dodati sljedeće informacije:

* Naziv računa za pohranu i tipki

    Naziv računa za pohranu i tipki možete doći s [portala za Azure](https://portal.azure.com/) ili [Azure klasični portal](http://manage.windowsazure.com/).
* ID izvršenja plan

    Plan ID možete pronaći na portalu [Servisa Azure strojnog učenja Web Services](https://services.azureml.net) prijave i kliknete naziv plana.

Dodajte ih u predlošku JSON podređenih čvor *Svojstva* na istoj razini kao čvor *MachineLearningWorkspace* .

Evo jednog primjera:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Potražite u sljedećim člancima i uzorak koda dodatne informacije:

* [Azure strojnog učenja cmdleta]( https://msdn.microsoft.com/library/azure/mt767952.aspx) referenca na MSDN-u
* Ogledni [Prikaz](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) GitHub

## <a name="consume-the-web-services"></a>Korištenje web-usluge

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Iz Azure strojnog učenja web-servisa korisničkog Sučelja (testiranje)

Možete testirati i web-servisa na portalu servisa Azure strojnog učenja Web Services. To uključuje testiranje servis za odgovor na zahtjev (RRS) i servis za obradu izvođenja (BES) sučelja.

* [Implementacija nove web-usluge](machine-learning-webservice-deploy-a-web-service.md)
* [Implementacija programa Azure strojnog učenja web-servisa](machine-learning-publish-a-machine-learning-web-service.md)
* [Vodič korak 5: Implementacija web-servisa Azure strojnog učenja](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Iz programa Excel

Možete preuzeti predložak programa Excel koja koristi web-servisa:

* [Troše Azure strojnog učenja web-servisa iz programa Excel](machine-learning-consuming-from-excel.md)
* [Dodatka programa Excel za Azure strojnog učenja web-usluge](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>S OSTATKOM klijentska

Azure web-servise strojnog učenja su RESTful API-ji. Može raditi te API-ji iz različite platforme, kao što su .NET Python, R, Java, itd. Stranici **utrošak** za web-servisa na [portalu Microsoft Azure strojnog učenja web-servisi](https://services.azureml.net) sadrži ogledne kod koji vam mogu pomoći da započnete. Dodatne informacije potražite u članku [upute za korištenje u web-servisa Azure strojnog učenja koji je uveden iz strojnog učenja eksperiment](machine-learning-consume-web-services.md).

