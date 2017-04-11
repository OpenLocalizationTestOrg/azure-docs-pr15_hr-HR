<properties
    pageTitle="Upravljanje Bus servisa sa servisom PowerShell | Microsoft Azure"
    description="Upravljanje Bus servis pomoću skripte komponente PowerShell"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Upravljanje Bus servisa sa servisom PowerShell

## <a name="overview"></a>Pregled

Microsoft Azure PowerShell je skriptiranje okruženje koje možete koristiti za upravljanje i automatizirati implementacije i upravljanja vaše radnih opterećenja servisu Azure. U ovom se članku opisuje kako pomoću ljuske PowerShell za dodjelu resursa i upravljanje entiteti Bus servisa kao što su polja naziva, redove te pomoću lokalne konzole za Azure PowerShell koncentratora za događaj.

## <a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće preduvjete:

- Azure pretplate. Azure je pretplatu. Dodatne informacije o Nabavljanje pretplate, pročitajte članak [Mogućnosti za kupnju][], [Nudi člana][]ili [Besplatnu probnu verziju][].

- Računalo s Azure PowerShell. Upute potražite u članku [instalirati i konfigurirati Azure PowerShell][].

- Općenito razumijevanja skripte komponente PowerShell, NuGet paketi i .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Uključujući referenca .NET skupa za servis Bus

Dostupno ograničen broj PowerShell cmdleti za upravljanje Bus servisa. Dodjela entiteti koji nije vidljiva kroz postojeće cmdleti za klijent za .NET možete koristiti za servis Bus [paket NuGet Bus servisa][].

Najprije provjerite je li skriptu možete pronaći u sklopu **Microsoft.ServiceBus.dll** koji se instalira uz NuGet paketa. Da bi se fleksibilne, skripta izvršava sljedeće korake:

1. Određuje put na kojoj je pozvan.
2. Putuje, a put dok se ne pronađe mapu pod nazivom `packages`. Ova mapa se stvara pri instalaciji paketa NuGet.
3. Rekurzivno pretraživanja na `packages` mapu u sklopu pod nazivom **Microsoft.ServiceBus.dll**.
4. Odnosi u sklopu tako da se vrste su dostupni za kasnije korištenje.

Evo kako se u skriptu PowerShell implementirana ove korake:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Dodjela resursa za servis Bus prostor naziva

Dva PowerShell cmdleti podržava operacije prostor naziva Bus servisa. Umjesto .NET SDK API-ji, možete koristiti [Get-AzureSBNamespace][] i [Novo AzureSBNamespace][].

U ovom se primjeru stvara nekoliko lokalne varijable u skripti; `$Namespace` and `$Location`.

- `$Namespace`je naziv servisa Bus prostora za naziv koji ne možemo želite raditi.
- `$Location`služi za identifikaciju podatkovnog centra u kojem skriptu dodjeljuje naziva.
- `$CurrentNamespace`sprema referencu u prostor za naziv koji skriptu dohvaća (ili stvara).

U skripte stvarni `$Namespace` i `$Location` možete proslijediti kao parametri.

Taj dio skripta izvršava sljedeće zadatke:

1. Pokušava dohvatiti servisa Bus prostor naziva s navedeno ime.
2. Ako se pronađe naziva, izvješća što nije pronađen.
3. Ako se ne pronađe naziva, stvara naziva i dohvaća novostvorenu naziva.

    ```
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Dodjela druge usluge Bus entiteti stvaranje instanca klase [NamespaceManager][] iz SDK-a.
Cmdlet [Get-AzureSBAuthorizationRule][] možete koristiti za dohvaćanje pravilo za provjeru autentičnosti koja se koristi za pružanje niza za povezivanje. Ne možemo ćete spremiti referenca na `NamespaceManager` instancu u na `$NamespaceManager` varijablu. Koristit ćemo `$NamespaceManager` kasnije u skripti Dodjela ostali entiteti.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Ostali entiteti Bus servis za dodjelu resursa

Da bi se Dodjela druge entiteti, kao što su redova, teme i koncentratora događaja pomoću [.NET API -JA za Bus servisa][]. U ovom se članku usredotočuje se samo na događaj koncentratora, ali slični su koraci za drugi entiteti. Osim toga, detaljnije Primjeri, uključujući druge entiteti se pozivaju na kraju ovog članka.

Taj dio skripta stvara četiri dodatne lokalne varijable. Ove varijable koriste se za stvaranje instance programa `EventHubDescription` objekt. Skripta izvršava sljedeće zadatke:

1. Korištenje na `NamespaceManager` objekta, provjerite ako središtu događaj otkrije `$Path` postoji.
2. Ako ne postoji, stvorite je `EventHubDescription` i prenesite s na `NamespaceManager` klase `CreateEventHubIfNotExists` način.
3. Nakon određivanje središtu događaj dostupna, stvorite pomoću na grupu potrošača `ConsumerGroupDescription` i `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Migriranje prostor naziva na drugu pretplatu na Azure

Sljedeće slijeda naredbi te pomicanje prostor naziva s jednog Azure pretplate na drugi. Za izvođenje ovog postupka, naziva mora već biti aktivna, a korisnik koji se pokreće naredbe ljuske PowerShell morate biti administrator na izvorišno i odredišno pretplate.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku dobili osnovne strukture za dodjelu resursa entiteti Bus servis pomoću komponente PowerShell. Bilo što možete učiniti pomoću biblioteka klijentski .NET, možete učiniti u skriptu PowerShell.

Nema više detaljne primjere dostupnih na ovih članaka za blog:

- [Kako stvoriti redova Bus servisa, teme i pretplata pomoću skriptu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Kako stvoriti polje naziva za Bus servisa i pomoću skriptu PowerShell koncentratora za događaj](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Neke od gotovih skripte su dostupni za preuzimanje:
- [Servis Bus skripte komponente PowerShell](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Mogućnosti za kupnju]: http://azure.microsoft.com/pricing/purchase-options/
[Ponuda za člana]: http://azure.microsoft.com/pricing/member-offers/
[Besplatna probna verzija]: http://azure.microsoft.com/pricing/free-trial/
[Instaliranje i konfiguriranje Azure PowerShell]: ../powershell-install-configure.md
[Servis Bus NuGet paketa]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Novi AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET API-JA za servis Bus]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
