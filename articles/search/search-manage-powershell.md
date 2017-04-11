<properties 
    pageTitle="Upravljanje pretraživanjem Azure s skripte komponente Powershell | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka" 
    description="Upravljati na servisu Azure pretraživanja s skripte komponente PowerShell. Stvaranje ili ažuriranje servis za pretraživanje Azure i upravljati ključevima administrator Azure pretraživanja" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>Upravljanje servis za pretraživanje Azure pomoću komponente PowerShell
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST API-JA](search-get-started-management-api.md)

U ovoj se temi opisuju naredbe ljuske PowerShell za mnoge zadatke upravljanja servisa Azure pretraživanja. Ne možemo će voditi kroz stvaranje servisa za pretraživanje, promjene veličine i upravljanje svoje ključeve API-JA.
Te naredbe Napredne postavke preglednika mogućnosti upravljanja dostupne [Azure pretraživanje upravljanje REST API -JA](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Preduvjeti
 
- Morate imati Azure PowerShell 1.0 ili noviji. Upute potražite u članku [instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).
- Morate biti prijavljeni u pretplatu za Azure u ljusci PowerShell prema uputama u nastavku.

Najprije morate prijaviti za Azure s ta naredba:

    Login-AzureRmAccount

Navedite adresu e-pošte te račun za Azure lozinku u dijaloški okvir za prijavu Microsoft Azure.

Umjesto toga možete [Prijava nije-interaktivno sa servisom glavni](../resource-group-authenticate-service-principal.md).

Ako imate više pretplata Azure, morate postaviti Azure pretplatu. Da biste vidjeli popis svoje trenutne pretplate, pokrenite sljedeću naredbu.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Da biste odredili pretplatu, pokrenite sljedeću naredbu. U sljedećem primjeru je naziv pretplate `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Naredbe za početak rada

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Daljnji koraci
    
Sad kad je stvorena na servisu, možete poduzeti sljedeće korake: stvaranje [indeksa](search-what-is-an-index.md), [upitu indeksa](search-query-overview.md)i na kraju stvaranje i upravljanje vlastitim aplikacije za pretraživanje koji koristi Azure pretraživanja.

- [Stvaranje indeksa pretraživanja Azure na portalu za Azure](search-create-index-portal.md)

- [Indeks pretraživanja Azure pomoću programa Explorer za pretraživanje na portalu za Azure za upite](search-explorer.md)

- [Postavljanje indeksiranje da biste učitali podatke s drugih servisa](search-indexer-overview.md)

- [Korištenje pretraživanja za Azure u .NET](search-howto-dotnet-sdk.md)

- [Analiza prometa Azure pretraživanja](search-traffic-analytics.md)
