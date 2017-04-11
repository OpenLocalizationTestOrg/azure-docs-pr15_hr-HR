<properties 
    pageTitle="Nadzor i upravljanje zadacima strujanje analize sa servisom PowerShell | Microsoft Azure" 
    description="Saznajte kako koristiti Azure i njezinim cmdletima za nadzor i upravljanje zadacima strujanje analize." 
    keywords="Azure powershell, azure powershell cmdleti, a zatim naredbu komponente powershell skripti komponente powershell" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Nadzor i upravljanje zadacima strujanje analize pomoću cmdleta ljuske PowerShell Azure

Saznajte kako nadzor i upravljanje resursima strujanje analize pomoću cmdleta ljuske PowerShell Azure i powershell skriptiranje koje se izvršiti osnovni zadaci za strujanje analize.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Preduvjeti za pokretanje Azure PowerShell cmdleti za strujanje Analytics

 - Stvaranje grupe za resurse sustava Azure u svoju pretplatu. Slijedi primjer skripte Azure PowerShell. Azure PowerShell informacije potražite u članku [instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md);  

Azure PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Strujanje analize poslove programski napravljen nemaju po zadanom omogućena za nadzor.  Možete ručno omogućavanje nadzor na portalu za Azure tako da odete na posao Monitor stranice i klikom na gumb Omogući ili to možete učiniti programski slijedeći korake nalazi se na [Azure strujanje analize - Monitor strujanje analize poslove Programatically](stream-analytics-monitor-jobs.md).

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell cmdleti za strujanje Analytics
Sljedeće Cmdlete Azure PowerShell može se koristiti za nadzor i upravljanje zadacima Azure strujanje analize. Imajte na umu da Azure PowerShell ima različite verzije. 
**U primjerima naveden prve naredbe je Azure PowerShell 0.9.8, drugi naredba je za Azure PowerShell 1.0.** Naredbe za Azure PowerShell 1.0 uvijek će imati "AzureRM" u naredbi.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Navodi sve analize strujanje zadatke koji su definirani u Azure pretplatu ili navedena grupa resursa ili dobiti informacije o zadatku o određeni posao u grupu resursa.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Sljedeću naredbu komponente PowerShell vraća informacije o sve zadatke za strujanje analize u Azure pretplate.

**Primjer 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Sljedeću naredbu komponente PowerShell vraća informacije o sve zadatke za strujanje analize u grupi resursa StreamAnalytics-zadane-središnje-hr.

**Primjer 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Sljedeću naredbu komponente PowerShell vraća informacije o posao analize strujanje StreamingJob u grupi resursa StreamAnalytics-zadane-središnje-hr.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Prikazuje sve unose koji su definirani u navedeni strujanje analize posao ili dohvaća podatke o određenim unos.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Sljedeću naredbu komponente PowerShell vraća informacije o sve unose definirano u posao StreamingJob.

**Primjer 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Sljedeću naredbu komponente PowerShell vraća informacije o unos naziva EntryStream koji su definirani u posao StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Popis svih izlaza koji su definirani u navedeni strujanje analize posao ili dohvaća podatke o određenim izlaz.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Sljedeću naredbu komponente PowerShell vraća informacije o izlaze definirano u posao StreamingJob.

**Primjer 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Sljedeću naredbu komponente PowerShell vraća informacije o rezultatu pod nazivom izlaz definirano u posao StreamingJob.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Dohvaća podatke o kvoti strujanja jedinice u navedenom regiji.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Sljedeću naredbu komponente PowerShell vraća informacije o kvota i korištenje strujanje jedinice u regiji središnje SAD-a.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Dohvaća podatke o određene transformaciju definirano u posao strujanje analize.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Sljedeću naredbu komponente PowerShell vraća informacije o transformaciju naziva StreamingJob u zadatku StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Novi AzureStreamAnalyticsInput | Novi AzureRMStreamAnalyticsInput
Stvara novi unos unutar strujanje analize posao ili ažurira postojeći navedeni unos.
  
Možete navesti naziv ulaza u datoteci .json ili u naredbenom retku. Ako oba naveden naziv u naredbenom retku mora biti jednak onome u datoteci.

Ako odredite ulazni koji već postoji, a ne navedete parametar u – prisilno cmdlet će vas pitati želite li da biste zamijenili postojeće unos ili ne.

Ako navedete u – prisilno parametar i navedite naziv postojećeg za unos, unos zamijenit će se bez potvrde.

Detaljne informacije o JSON datoteke strukture i sadržaja u priručniku [Stvaranje unos (Azure strujanje analize)] [ msdn-rest-api-create-stream-analytics-input] odjeljak [strujanje analize REST API Reference biblioteke za upravljanje][stream.analytics.rest.api.reference].

**Primjer 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Sljedeću naredbu komponente PowerShell stvara novi unos iz datoteke Input.json. Ako je već definiran postojeće unos naziva naveden u datoteci unos definicije, cmdlet će vas pitati hoćete li ga zamijeniti.

**Primjer 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Sljedeću naredbu komponente PowerShell stvara novi unos u zadatak pod nazivom EntryStream. Ako je postojeći unos s ovim nazivom već definiran, cmdlet će vas pitati hoćete li ga zamijeniti.

**Primjer 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Sljedeću naredbu komponente PowerShell zamjenjuje definiciju postojećeg izvora unos naziva EntryStream s definicijom iz datoteke.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Novi AzureStreamAnalyticsJob | Novi AzureRMStreamAnalyticsJob
Stvara novi zadatak strujanje analize u Microsoft Azure ili ažurira definiciju postojećeg navedeni posla.

Možete navesti naziv zadatka u datoteci .json ili u naredbenom retku. Ako oba naveden naziv u naredbenom retku mora biti jednak onome u datoteci.

Ako Navedite naziv zadatka koji već postoji, a ne navedete parametar u – prisilno cmdlet će vas pitati želite li zamijeniti postojeći posao ili ne.

Ako navedete u – prisilno parametar i navedite postojeći zadatak naziv definiciju posla zamijenit će se bez potvrde.

Detaljne informacije o JSON datoteke strukture i sadržaja referira na [Stvaranje zadatka analize strujanje] [ msdn-rest-api-create-stream-analytics-job] sekciji [strujanje analize REST API Reference biblioteke za upravljanje][stream.analytics.rest.api.reference].

**Primjer 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Sljedeću naredbu komponente PowerShell stvara novi zadatak iz definicije u JobDefinition.json. Ako je postojeći posao pod nazivom naveden u datoteci definicije posla već definiran, cmdlet će vas pitati hoćete li ga zamijeniti.

**Primjer 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Sljedeću naredbu komponente PowerShell zamjenjuje definiciju posla za StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Novi AzureStreamAnalyticsOutput | Novi AzureRMStreamAnalyticsOutput
Stvara novi izlaz unutar strujanje analize posao ili ažurira postojeći izlaz.  

Možete navesti naziv izlaza u datoteci .json ili u naredbenom retku. Ako oba naveden naziv u naredbenom retku mora biti jednak onome u datoteci.

Ako odredite programa izlaza koji već postoji i ne navedete parametar u – prisilno cmdlet će vas pitati želite li da biste zamijenili postojeće Izlaz ili ne.

Ako navedete u – prisilno parametar i navedite postojeći izlaz naziv, izlaz zamijenit će se bez potvrde.

Detaljne informacije o JSON datoteke strukture i sadržaja odnose na [Stvaranje izlazna (Azure strujanje analize)] [ msdn-rest-api-create-stream-analytics-output] sekciji [strujanje analize REST API Reference biblioteke za upravljanje][stream.analytics.rest.api.reference].

**Primjer 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Sljedeću naredbu komponente PowerShell stvara novi izlaz pod nazivom "Izlaz" u zadatku StreamingJob. Ako je već definiran postojeće izlaz s ovim nazivom, cmdlet će vas pitati hoćete li ga zamijeniti.

**Primjer 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Sljedeću naredbu komponente PowerShell zamjenjuje definiciju za "Izlaz" u zadatku StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Novi AzureStreamAnalyticsTransformation | Novi AzureRMStreamAnalyticsTransformation
Stvara nova transformacija unutar strujanje analize posao ili ažurira postojeći transformaciju.
  
Možete navesti naziv transformacije u datoteci .json ili u naredbenom retku. Ako oba naveden naziv u naredbenom retku mora biti jednak onome u datoteci.

Ako odredite transformaciju koji već postoji, a ne navedete parametar u – prisilno cmdlet će vas pitati želite li da biste zamijenili postojeće transformacije ili ne.

Ako navedete dio – prisilno parametar i navedite postojeći transformaciju naziv transformaciju zamijenit će se bez potvrde.

Detaljne informacije o JSON datoteke strukture i sadržaja odnose na [Stvaranje transformaciju (Azure strujanje analize)] [ msdn-rest-api-create-stream-analytics-transformation] odjeljak [strujanje analize REST API Reference biblioteke za upravljanje][stream.analytics.rest.api.reference].

**Primjer 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Sljedeću naredbu komponente PowerShell stvara nova transformacija naziva StreamingJobTransform u zadatku StreamingJob. Ako postojeće transformaciju već definiran s ovim nazivom, cmdlet će vas pitati hoćete li ga zamijeniti.

**Primjer 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Sljedeću naredbu komponente PowerShell zamjenjuje definiciju StreamingJobTransform u zadatku StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Uklanjanje AzureStreamAnalyticsInput | Uklanjanje AzureRMStreamAnalyticsInput
Asinkrono briše određene unos s posla strujanje analize u Microsoft Azure.  
Ako navedete parametar u – prisilno unos će se izbrisati bez potvrde.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Sljedeću naredbu komponente PowerShell uklanja unos EventStream u posao StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Uklanjanje AzureStreamAnalyticsJob | Uklanjanje AzureRMStreamAnalyticsJob
Asinkrono briše određeni posao strujanje analize u Microsoft Azure.  
Ako navedete parametar u – prisilno posao će se izbrisati bez potvrde.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Sljedeću naredbu komponente PowerShell uklanja posao StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Uklanjanje AzureStreamAnalyticsOutput | Uklanjanje AzureRMStreamAnalyticsOutput
Asinkrono briše određene Izlaz iz posao strujanje analize u Microsoft Azure.  
Ako navedete parametar u – prisilno izlaz izbrisat će se bez potvrde.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

U ovom izlaz Izlaz u zadatku StreamingJob naredba uklanja "PowerShell".  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Početak AzureStreamAnalyticsJob | Početak AzureRMStreamAnalyticsJob
Asinkrono uvodi i pokrenuti posao strujanje analize u Microsoft Azure.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Sljedeću naredbu komponente PowerShell pokreće posao StreamingJob s vremenom početka prilagođene izlaz postavite prosinac 12, 2012, 12:12:12 UTC-a.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Zaustavi AzureStreamAnalyticsJob | Zaustavi AzureRMStreamAnalyticsJob
Asinkrono zaustavlja posao strujanje analize pokretanje u programu Microsoft Azure i poništite dodjeljuje resurse koji su koji su koristi. Definicija zadatka i metapodacima ostat će dostupna u svoju pretplatu putem portala za Azure i upravljanje API-ji, tako da se zadatak možete uređivati i ponovno pokrenuti. Će ne naplaćuje za posao u prestao stanju.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Sljedeću naredbu komponente PowerShell zaustavlja posao StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test AzureStreamAnalyticsInput | Test AzureRMStreamAnalyticsInput
Testira mogućnost analize strujanje povezivanja s navedeni unos.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Sljedeću naredbu komponente PowerShell testira stanja veze unos EntryStream u StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test AzureStreamAnalyticsOutput | Test AzureRMStreamAnalyticsOutput
Testira mogućnost analize strujanje povezivanja s navedeni izlazni.

**Primjer 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

U ovom stanje veze izlaza Izlaz u StreamingJob naredba testira "PowerShell".  

## <a name="get-support"></a>Podrška
Za dodatnu pomoć, pokušajte našem [forumu Azure strujanje analize](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 


## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
