<properties
    pageTitle="Web App kloniranje pomoću komponente PowerShell"
    description="Saznajte kako Kloniraj web-aplikacije pomoću komponente PowerShell novog web-aplikacije."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Aplikacije servisa za Azure aplikacije kloniranje pomoću komponente PowerShell#

Uz izdanje sustava Microsoft Azure PowerShell verzije 1.1.0 nove mogućnosti je dodana nova AzureRMWebApp koji želite omogućiti korisniku Kloniraj postojeće web-aplikacije programa novostvorenu aplikaciju u nekoj drugoj regiji ili u istom području. To će omogućiti klijentima za implementaciju broj aplikacije preko različitih područja brzo i jednostavno.

Aplikacija kloniranje trenutno podržava samo za tarife aplikacije servisa za sloju premium. Nova značajka koristi iste ograničenja kao značajka sigurnosno kopiranje Web Apps potražite u odjeljku [sigurnosno kopiranje web app u aplikacije servisa za Azure](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Dodatne informacije o korištenju upravitelja resursa Azure temelji Azure PowerShell cmdleti za upravljanje aplikacijama Web provjerite [Voditelj resursa Azure temelji naredbe ljuske PowerShell za Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Kloniranje postojeće aplikacije ##

Scenarij: Postojeće web aplikacije u regiji Jug središnje hr, korisnik želi Kloniraj sadržaj web-aplikaciju u Sjevernoj središnje NAM regiji. To je moguće napraviti pomoću upravitelja resursa Azure verziju cmdletu komponente PowerShell da biste stvorili novu web-aplikaciju s mogućnošću - SourceWebApp.

Zna naziv grupe resursa koja sadrži web-aplikaciji izvora, ne možemo možete koristiti sljedeću naredbu komponente PowerShell pronaći podatke za izvor web app (u tom slučaju pod nazivom izvora webapp):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Da biste stvorili na novu aplikaciju servisa planiranje, možete koristiti naredbe nova AzureRmAppServicePlan kao u sljedećem primjeru

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Pomoću naredbe za novi AzureRmWebApp, ne možemo možete stvoriti novu web-aplikaciju u regiji Sjeverna središnje NAM i sve da biste postojeću premium sloju Plan za aplikaciju servisa, nadalje smo možete koristiti istoj grupi resursa web-aplikacije izvornog web App ili Definiraj novu grupu resursa, sljedeće pokazuje koja:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Da biste Kloniraj postojeće web-aplikacije programa uključujući sve povezane implementacije slobodnih, korisnik mora koristiti parametar IncludeSourceWebAppSlots, sljedeću naredbu komponente PowerShell pokazuje korištenje taj parametar pomoću naredbe nova AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Da bi Kloniraj postojeće web-aplikacije programa unutar iste područja, korisnik mora stvoriti novu grupu resursa i nove aplikacije usluge planiranje na istom području i pomoću sljedeće naredbe ljuske PowerShell Kloniraj web-aplikaciji

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kloniranje postojeće aplikacije u okruženju servisa aplikacija ##

Scenarij: Postojeće web aplikacije u regiji Jug središnje NAM, korisnik želi Kloniraj sadržaj da biste novu web-aplikaciju da biste u postojeći aplikacije servisa okruženje (elika i mala slova).

Zna naziv grupe resursa koja sadrži web-aplikaciji izvora, ne možemo možete koristiti sljedeću naredbu komponente PowerShell pronaći podatke za izvor web app (u tom slučaju pod nazivom izvora webapp):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Zna se elika i mala slova, naziv, a naziv grupe resursa koji pripada u elika i mala slova, korisnik možete koristiti naredbu novi AzureRmWebApp da biste stvorili novu web-aplikaciju na postojeće elika i mala slova, sljedeće pokazuje koja:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Mjesto parametar nije potrebna naslijeđene razloga, ali u slučaju stvaranja aplikacije u programa elika i mala slova ga će zanemariti. 

## <a name="cloning-an-existing-app-slot"></a>Kloniranje postojeće vremensko razdoblje aplikacije ##

Scenarij: Korisnik želi Kloniraj programa postojeće Web App vremensko razdoblje da biste ili novog web-aplikacijama ili novi blok Web App. Nova web-aplikaciji može biti u području isti kao izvornu vremensko razdoblje u web-aplikacije ili u nekoj drugoj regiji.

Zna naziv grupe resursa koja sadrži web-aplikaciji izvora, ne možemo možete koristiti sljedeće naredbe ljuske PowerShell da biste saznali jedno izvorišnog web app područje (u tom slučaju pod nazivom izvora webappslot) uz izvor webapp za web-aplikacije:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Sljedeće prikazuje stvaranje Kloniraj izvorišnog web-aplikacije za novu web-aplikaciju:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Konfiguriranje upravitelja promet prilikom kloniranje u aplikaciji ##

Stvaranje više područja web-aplikacije i konfiguriranju Azure promet Upravitelj da biste promet usmjerili na te web-aplikacije, je n važna značajka da biste osigurali da su aplikacije klijenata iznimno dostupne, kada kloniranje web-aplikaciju programa postojeće imate mogućnost kako se povezati i web-aplikacije novi profil Upravitelj promet ili postojećeg-Imajte na umu da samo Voditelj resursa Azure verzija upravitelja promet podržana.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Stvaranje novog profila Upravitelja promet tijekom kloniranje u aplikaciji ###

Scenarij: Korisnik želi Kloniraj web aplikacije na drugoj regiji, tijekom konfiguriranja Voditelj resursa Azure promet Upravitelj profil programa koji sadrže obje web-aplikacije. Sljedeće prikazuje stvaranje Kloniraj izvorišnog web-aplikacije za novu web-aplikaciju tijekom konfiguriranja novi profil Upravitelj promet:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Dodavanje novih klonirana web-aplikacije u postojeći profil Upravitelj promet ###

Scenarij: Korisnik već imate Voditelj resursa Azure promet Upravitelj profil programa koji mu želite dodati i web-aplikacije kao krajnje točke. Da biste učinili pa ćemo najprije morate prikupiti postojeće id profila Upravitelja promet, moramo će id pretplate, naziv grupe resursa i postojeći naziv profila Upravitelja promet.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Nakon napuštanja Upravitelj id promet sljedeće prikazuje stvaranje Kloniraj izvorišnog web-aplikacije za novu web-aplikaciju prilikom njihova dodavanja u postojeći profil Upravitelj promet:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Trenutna ograničenja ##

Ta je značajka trenutno u pretpregledu, radimo da biste dodali nove mogućnosti tijekom vremena, na popisu u nastavku su poznati ograničenja na trenutnu verziju aplikacije kloniranje:

- Postavke mjerila automatski se klonirana
- Raspored sigurnosnog kopiranja postavke su klonirana
- Postavke VNET su klonirana
- Uvid u aplikaciji ne automatski se postavljaju na web-aplikaciji odredište
- Jednostavno postavke provjere autentičnosti su klonirana
- Proširenje kudu su klonirana
- Savjet pravila su klonirana
- Sadržaj baze podataka su klonirana


### <a name="references"></a>Reference ###
- [Azure Voditelj resursa koji se temelje naredbe ljuske PowerShell za Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Web App kloniranje pomoću portala za Azure](app-service-web-app-cloning-portal.md)
- [Stvaranje sigurnosne kopije web app u aplikacije servisa za Azure](web-sites-backup.md)
- [Azure Voditelj resursa podrške za pregled upravitelja promet Azure](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Uvod u okruženje za aplikacije servisa](app-service-app-service-environment-intro.md)
- [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md)
