<properties
    pageTitle="Azure utemeljen na resursima PowerShell naredbe za Azure web-aplikaciji | Microsoft Azure"
    description="Saznajte kako pomoću naredbe novi utemeljen na Voditelj resursa Azure PowerShell za upravljanje aplikacijama Web Azure."
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
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Upravljanje Azure web-aplikacije pomoću komponente utemeljen na Upravitelj Azure resursa#

> [AZURE.SELECTOR]
- [Azure EŽA](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Uz Microsoft Azure PowerShell verzije 1.0.0 novi naredbe dodane, koji korisniku omogućuje korištenje naredbe utemeljen na Voditelj resursa Azure ljuske PowerShell za upravljanje web-aplikacije.

Da biste saznali više o upravljanju grupe resursa, potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md). 

Da biste saznali više o cijeli popis parametara i mogućnosti za PowerShell cmdleti, potražite u članku [Punu referencu Cmdlet utemeljen na Voditelj resursa za Web App Azure cmdleta ljuske PowerShell](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Upravljanje servisom aplikacije tarife ##

### <a name="create-an-app-service-plan"></a>Stvaranje tarifu aplikacije servisa ###
Da biste stvorili plan usluge za aplikaciju, pomoću cmdleta **New-AzureRmAppServicePlan** .

Slijede opisi različitih parametara:

-   **Naziv**: naziv plan usluge za aplikacije.
-   **Lokacija**: mjesto tarifu za servis.
-   **ResourceGroupName**: grupa resursa koji sadrži plan usluge za novostvorenu aplikacije.
-   **Razina**: željeni cijene sloju (Zadana vrijednost je besplatno, druge mogućnosti su zajednički se koristi, Basic, standardna i Premium.)
-   **WorkerSize**: veličina zaposlenici zaduženi za (Zadana vrijednost je small ako parametar sloju navedeno je kao Basic, Standard ili Premium. Druge mogućnosti su srednje i veliko.)
-   **NumberofWorkers**: broj zaposlenici zaduženi za u aplikaciji servisa plan (Zadana vrijednost je 1). 

Primjer korištenja ovaj cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Stvaranje tarifu za servis za aplikacije u okruženju aplikacije servisa za ###
Da biste stvorili plan usluge za aplikacije u okruženju aplikacije servisa, koristite naredbu za **Novo AzureRmAppServicePlan** iste naredbe s dodatni parametri da biste odredili na elika i mala slova, naziv i elika i mala slova, resursa grupe.

Primjer korištenja ovaj cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Da biste saznali više o okruženju aplikacije servisa, provjerite [Uvod u okruženje za aplikaciju servisa](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Aplikacije servisa za postojeći popis tarife ###

Da biste popis postojeće aplikacije paketi pretplate na usluge, koristite cmdlet **Get-AzureRmAppServicePlan** .

Da biste popis sve tarife servisa aplikacija za pretplatu, koristite: 

    Get-AzureRmAppServicePlan

Da biste popis sve tarife servisa aplikacija za određene grupe resursa, koristite:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Da biste plan usluge za određenu aplikaciju, koristite:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfiguriranje programa postojeće aplikacije servisa Plan ###

Da biste promijenili postavke za postojeće aplikacije tarifa za servis, koristite cmdlet **Skup AzureRmAppServicePlan** . Možete promijeniti u sloju, tempiranja veličina i broj zaposlenici zaduženi za 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Promjena veličine tarifu aplikacije servisa ####

Da biste skalirali na postojeće aplikacije servisa planiranje, koristite:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Promjena veličine radnih Plan za aplikaciju servisa ####

Da biste promijenili veličinu zaposlenici zaduženi za u programa postojeće aplikacije servisa planiranje, koristite:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Promjena sloju tarifu aplikacije servisa ####

Da biste promijenili sloj na postojeće aplikacije servisa planiranje, koristite:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Brisanje na postojeće aplikacije servisa Plan ###

Da biste izbrisali postojeću aplikacije tarifa za servis, sve dodijeljene web-aplikacije moraju biti premješten ili izbrisan prvi put. Zatim pomoću cmdleta **Ukloni AzureRmAppServicePlan** možete izbrisati plan usluge za aplikacije.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Upravljanje aplikacije servisa za web-aplikacije ##

### <a name="create-a-web-app"></a>Stvaranje web-aplikacijama ###

Da biste stvorili web-aplikacijama, pomoću cmdleta **New-AzureRmWebApp** .

Slijede opisi različitih parametara:

- **Naziv**: naziv web-aplikaciji.
- **AppServicePlan**: naziv tarifa za servis za glavno računalo web-aplikaciji.
- **ResourceGroupName**: grupa resursa u kojem je smještena plan usluge za aplikacije.
- **Lokacija**: web-mjesto aplikacije.

Primjer korištenja ovaj cmdlet:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Stvaranje web-aplikacijama u okruženju aplikacije servisa ###

Da biste stvorili web-aplikacijama u programa aplikacije servisa okruženje (elika i mala slova). Pomoću ista naredba **Novo AzureRmWebApp** s dodatni parametri odredite naziv elika i mala slova i resursa naziv grupe kojoj pripada u elika i mala slova.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Da biste saznali više o okruženju aplikacije servisa, provjerite [Uvod u okruženje za aplikaciju servisa](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Brisanje postojeće Web App ###

Da biste izbrisali web-aplikaciju programa postojeće koristite cmdlet **Ukloni AzureRmWebApp** , morate navesti naziv web-aplikacije i grupu resursa.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Popis postojeće web-aplikacije ###

Da biste popis postojeće web-aplikacije, koristite cmdlet **Get-AzureRmWebApp** .

Da biste popis svih web-aplikacije u odjeljku pretplate, koristite:

    Get-AzureRmWebApp

Da biste popis svih web-aplikacije u odjeljku određene grupe resursa, koristite:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Da biste dobili određene web-aplikacijama, koristite:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfiguriranje postojeće Web App ###

Da biste promijenili postavke i konfiguracije za postojeće web-aplikacije, koristite cmdlet **Skup AzureRmWebApp** . Potpuni popis parametara, provjerite [Cmdlet referentne veze](https://msdn.microsoft.com/library/mt652487.aspx)

Primjer (1): pomoću tog cmdleta da biste promijenili nizove veze

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Primjer (2): Dodavanje i mijenjanje postavki aplikacije

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Primjer (3): Postavljanje web-aplikaciju u 64-bitni način rada

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Promjena stanja postojeće Web App ###

#### <a name="restart-a-web-app"></a>Ponovno pokrenite aplikaciju za web ####

Da biste ponovno pokrenuli web-aplikacijama, morate navesti grupi ime i resursa web-aplikacije.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Zaustavljanje web-aplikacijama ####

Da biste prestali web-aplikacijama, morate navesti grupi ime i resursa web-aplikacije.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Pokretanje web-aplikacijama ####

Da biste pokrenuli web-aplikacijama, morate navesti grupi ime i resursa web-aplikacije.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Upravljanje web-aplikacije objavu profilima ###

Svaki web-aplikacije ima objavljivanje profila koji se mogu koristiti za objavljivanje aplikacija, možete izvršiti nekoliko postupaka na profila za objavljivanje.

#### <a name="get-publishing-profile"></a>Početak objavljivanje profila ####

Da biste objavljivanje profila za web-aplikacije, koristite:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Ta se naredba echoes objavljivanje profila da bi naredbenog retka kao i izlazni objavljivanje profila s tekstnom datotekom.

#### <a name="reset-publishing-profile"></a>Poništavanje objavljivanja profila ####

Da biste ponovno postavili i objavljivanje lozinke za FTP i web-implementacija web App, koristite:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Upravljanje certifikatima Web App ###

Da biste saznali kako upravljati web app certifikata, potražite u članku [Povezivanje SSL certifikata pomoću komponente PowerShell](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Daljnji koraci ###
- Informacije o podršci za Azure resursima PowerShell potražite u članku [pomoću ljuske Azure s Azure Voditelj resursa.](../powershell-azure-resource-manager.md)
- Da biste saznali više o aplikacije servisa okruženja, potražite u članku [Uvod u okruženje za aplikaciju servisa.](app-service-app-service-environment-intro.md)
- Informacije o upravljanju aplikacije servisa SSL certifikata pomoću komponente PowerShell potražite u članku [Povezivanje SSL certifikata pomoću komponente PowerShell.](app-service-web-app-powershell-ssl-binding.md)
- Da biste saznali više o cijeli popis utemeljen na Voditelj resursa Azure PowerShell cmdleti za Azure web-aplikacije, potražite u članku [referencu Cmdlet Azure cmdleta ljuske PowerShell za Web Apps Azure resursima.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Informacije o upravljanju aplikacije servisa za korištenje EŽA potražite u članku [Using Azure Resource Manager-Based XPlat EŽA za Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)
