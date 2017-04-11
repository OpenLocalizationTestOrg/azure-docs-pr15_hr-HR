<properties 
    pageTitle="Implementacija i upravljati pomoću komponente PowerShell koncentratora obavijesti" 
    description="Upute za stvaranje i upravljanje njima pomoću komponente PowerShell za automatizaciju koncentratora za obavijesti" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Implementacija i upravljati pomoću komponente PowerShell koncentratora obavijesti

##<a name="overview"></a>Pregled

U ovom se članku objašnjava korištenje stvaranje i upravljanje Azure obavijesti koncentratora pomoću komponente PowerShell. U ovoj temi prikazane su sljedeće uobičajenih automatizaciju zadataka.

+ Stvaranje obavijesti koncentratora
+ Postavljanje vjerodajnica

Ako morate stvoriti novi servis bus prostor naziva za vaše koncentratora obavijesti, potražite u članku [Upravljanje Bus servisa sa servisom PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Upravljanje koncentratora obavijesti ne podržava izravno cmdleta u sklopu Azure PowerShell. Najbolje iz PowerShell je koristiti referencu u sklopu Microsoft.Azure.NotificationHubs.dll. U sklopu distribuira s [paketa Microsoft Azure obavijesti koncentratora NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).


## <a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- Azure pretplate. Azure je pretplatu. Dodatne informacije o Nabavljanje pretplate, pročitajte članak [Mogućnosti za kupnju], [Nudi člana]ili [Besplatnu probnu verziju].

- Računalo s Azure PowerShell. Upute potražite u članku [instalirati i konfigurirati Azure PowerShell].

- Općenito razumijevanja skripte komponente PowerShell, NuGet paketi i .NET Framework.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Uključujući referenca .NET skupa za servis Bus

Upravljanje Azure obavijesti koncentratora još nije uvršten cmdleta ljuske PowerShell u Azure PowerShell. Dodjela koncentratora obavijesti, možete koristiti klijent .NET navedenih u [paket Microsoft Azure obavijesti koncentratora NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Najprije provjerite je li skriptu možete pronaći u sklopu **Microsoft.Azure.NotificationHubs.dll** koji se instalira kao paket NuGet u programu project za Visual Studio. Da bi se fleksibilne, skripta izvršava sljedeće korake:

1. Određuje put na kojoj je pozvan.
2. Putuje, a put dok se ne pronađe mapu pod nazivom `packages`. Ova mapa se stvara pri instalaciji paketa NuGet za Visual Studio projekte.
3. Rekurzivno pretraživanja na `packages` mapu u sklopu pod nazivom **Microsoft.Azure.NotificationHubs.dll**.
4. Odnosi u sklopu tako da se vrste su dostupni za kasnije korištenje.

Evo kako se u skriptu PowerShell implementirana ove korake:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>Stvaranje NamespaceManager klase

Dodjela koncentratora obavijesti, stvaranje instanca klase [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) iz SDK-a. 

Cmdlet [Get-AzureSBAuthorizationRule] u sklopu Azure PowerShell možete koristiti za dohvaćanje pravilo za provjeru autentičnosti koja se koristi za pružanje niza za povezivanje. Ne možemo ćete spremiti referenca na `NamespaceManager` instancu u na `$NamespaceManager` varijablu. Koristit ćemo `$NamespaceManager` Dodjela koncentratora za obavijesti.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Dodjeljivanje novi koncentrator obavijesti 

Dodjela nove koncentrator obavijesti, koristite [.NET API -JA za obavijesti koncentratora].

U ovom dijelu skriptu postaviti četiri lokalne varijable. 

1. `$Namespace`: Postavite taj naziv prostora za naziv mjesto na koje želite stvoriti koncentratora za obavijesti.
2. `$Path`: Postavite ovaj put naziv novi koncentrator obavijesti.  Na primjer, "MyHub".    
3. `$WnsPackageSid`: Postavlja to pakirati SID aplikacija u sustavu Windows putem [Razvojni centar za Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Postavljanje to tajnu ključ za web-aplikaciju Windows iz [Razvojni centar za Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Ove varijable koriste se za povezivanje s vašeg prostor naziva i stvorite novu obavijest koncentrator konfigurirano za rukovanje obavijesti sustava Windows obavijesti Services (WNS) s vjerodajnicama WNS za aplikacija u sustavu Windows. Da biste saznali stjecanje paket SID i tajnu ključ potražite u odjeljku vodič za [Početak rada s koncentratorima obavijesti](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) . 

+ Koristi isječak skripte na `NamespaceManager` objekt da biste provjerili da biste vidjeli ako središtu obavijesti otkrije `$Path` postoji.

+ Ako ne postoji, će stvoriti skriptu programa `NotificationHubDescription` s WNS vjerodajnice i prenesite s na `NamespaceManager` klase `CreateNotificationHub` način.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Dodatni resursi

- [Upravljanje Bus servisa sa servisom PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Kako stvoriti redova Bus servisa, teme i pretplata pomoću skriptu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Kako stvoriti polje naziva za Bus servisa i pomoću skriptu PowerShell koncentratora za događaj](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Neke od gotovih skripte su dostupni za preuzimanje:
- [Servis Bus skripte komponente PowerShell](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Mogućnosti za kupnju]: http://azure.microsoft.com/pricing/purchase-options/
[Ponuda za člana]: http://azure.microsoft.com/pricing/member-offers/
[Besplatna probna verzija]: http://azure.microsoft.com/pricing/free-trial/
[Instaliranje i konfiguriranje Azure PowerShell]: ../powershell-install-configure.md
[.NET API-JA za obavijesti koncentratora]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
