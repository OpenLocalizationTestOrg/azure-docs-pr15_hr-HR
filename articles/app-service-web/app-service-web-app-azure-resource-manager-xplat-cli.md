<properties
    pageTitle="Azure utemeljen na resursima različite platforme alate naredbenog retka za Azure web-aplikacije | Microsoft Azure"
    description="Saznajte kako pomoću novih alata naredbenog retka utemeljen na Voditelj resursa Azure različite platforme za upravljanje aplikacijama Web Azure."
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

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Pomoću EŽA XPlat utemeljen na Upravitelj Azure resursa za Azure Web App#

> [AZURE.SELECTOR]
- [Azure EŽA](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Uz izdanje alata naredbenog retka za različite platforme Microsoft Azure verzija 0.10.5, novi naredbe dodane. Te naredbe korisniku omogućuje korištenje naredbe utemeljen na Voditelj resursa Azure ljuske PowerShell za upravljanje web-aplikacije.

Da biste saznali više o upravljanju grupe resursa, potražite u članku [Korištenje EŽA Azure upravljanja Azure resursa i grupa resursa](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Upravljanje servisom aplikacije tarife ##

### <a name="create-an-app-service-plan"></a>Stvaranje tarifu aplikacije servisa ###
Da biste stvorili plan usluge za aplikaciju, pomoću naredbe **Stvori azure appserviceplan** .

Slijede opisi različitih parametara:

-   **– grupa resursa**: grupa resursa koji sadrži plan usluge za novostvorenu aplikacije.
-   **– naziv**: naziv plan usluge za aplikacije.
-   **– mjesto**: mjesto plan usluge za aplikacije.
-   **– sloju**: željeni cijene SKU-om (su sljedeće mogućnosti: F1 (besplatno). D1 (zajedničko korištenje). B1 (osnovni male), B2 (osnovni Medium) i B3 (Basic velike). S1 (standardni male), S2 (standardni Medium) i S3 (standardni velike). P1 (Premium male), P2 (Premium Medium) i P3 (Premium velike).)
-   **– instance**: broj zaposlenici zaduženi za u aplikaciji servisa plan (Zadana vrijednost je 1).

Primjer korištenja ovaj cmdlet:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Aplikacije servisa za postojeći popis tarife ###

Da biste popis postojeće aplikacije paketi pretplate na usluge, koristite naredbe **azure appserviceplan popis** .

Da biste popis sve tarife servisa aplikacija za određene grupe resursa, koristite:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Da biste dobili plan usluge za određenu aplikaciju, poslužite se naredbom **azure appserviceplan prikaz** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfiguriranje programa postojeće aplikacije servisa Plan ###

Da biste promijenili postavke za postojeće aplikacije tarifa za servis, koristite naredbu **azure appserviceplan konfiguracije** . Možete promijeniti na sku i broj zaposlenici zaduženi za 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Promjena veličine tarifu aplikacije servisa ####

Da biste skalirali na postojeće aplikacije servisa planiranje, koristite:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Promjena SKU tarifu aplikacije servisa ####

Da biste promijenili sku od programa postojeće aplikacije servisa planiranje, koristite:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Brisanje na postojeće aplikacije servisa Plan ###

Da biste izbrisali postojeću aplikacije tarifa za servis, sve dodijeljene web-aplikacije moraju biti premješten ili izbrisan prvi put. Zatim pomoću naredbe za **Brisanje azure webapp** možete izbrisati plan usluge za aplikacije.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Upravljanje aplikacije servisa za web-aplikacije ##

### <a name="create-a-web-app"></a>Stvaranje web-aplikacijama ###

Da biste stvorili web-aplikacijama, pomoću naredbe **Stvori azure webapp** .

Slijede opisi različitih parametara:

- **– naziv**: naziv web-aplikaciji.
- **– plan**: naziv tarifa za servis za glavno računalo web-aplikaciji.
- **– grupa resursa**: grupa resursa u kojem je smještena plan usluge za aplikacije.
- **– mjesto**: web-mjesto aplikacije.

Primjer korištenja ovaj cmdlet:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Brisanje postojeće Web App ###

Da biste izbrisali web-aplikaciju programa postojeće pomoću naredbe **Izbriši azure webapp** , morate navesti naziv web-aplikacije i grupu resursa.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Popis postojeće web-aplikacije ###

Da biste dobili popis postojeće web-aplikacije pomoću naredbe za **azure webapp popis** .

Da biste popis svih web-aplikacije u odjeljku određene grupe resursa, koristite:

    azure webapp list --resource-group ContosoAzureResourceGroup

Da biste dobili određene web-aplikacijama, pomoću naredbe **Prikaži azure webapp** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Konfiguriranje postojeće Web App ###

Da biste promijenili postavke i konfiguracije za postojeće web-aplikacije, koristite naredbu **azure webapp konfiguracije postavite** .

Primjer (1): promjena php verziju web-aplikacijama 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Primjer (2): Dodavanje i mijenjanje postavki aplikacije

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Saznajte što može biti druge konfiguracije mijenjaju, upotrijebite naredbu **azure webapp konfiguracije postavite -h** .

### <a name="change-the-state-of-an-existing-web-app"></a>Promjena stanja postojeće Web App ###

#### <a name="restart-a-web-app"></a>Ponovno pokrenite aplikaciju za web ####

Da biste ponovno pokrenuli web-aplikacijama, morate navesti grupi ime i resursa web-aplikacije.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Zaustavljanje web-aplikacijama ####

Da biste prestali web-aplikacijama, morate navesti grupi ime i resursa web-aplikacije.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Pokretanje web-aplikacijama ####

Da biste pokrenuli web-aplikacijama, morate navesti grupi ime i resursa web-aplikacije.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Upravljanje web-aplikacije objavu profilima ###

Svaki web-aplikacije sadrži objavljivanje profila koji se mogu koristiti za objavljivanje aplikacija.

#### <a name="get-publishing-profile"></a>Početak objavljivanje profila ####

Da biste objavljivanje profila za web-aplikacije, koristite:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Ta se naredba echoes objavljivanje profila korisničko ime i lozinku za naredbeni redak.

### <a name="manage-web-app-hostnames"></a>Upravljanje hostnames Web App ###

Da biste upravljali povezivanja naziv glavnog računala za web-aplikacije, koristite naredbu **hostnames config azure webapp**  

#### <a name="list-hostname-bindings"></a>Povezivanje popisa naziv glavnog računala ####

Da biste trenutnu povezivanja naziv glavnog računala za web-aplikacije, koristite:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Dodavanje povezivanja naziv glavnog računala ####

Da biste dodali povezivanja naziv glavnog računala web-aplikacijama, koristite:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Brisanje povezivanja naziv glavnog računala ####

Da biste izbrisali povezivanja naziv glavnog računala, koristite:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Daljnji koraci ###
- Informacije o podršci za Azure resursima EŽA potražite u članku [omogućuju upravljanje Azure resursa i grupa resursa Azure EŽA.](../xplat-cli-azure-resource-manager.md)
- Informacije o upravljanju aplikacije servisa za pomoću komponente PowerShell potražite u članku [Using Azure Resource Manager-Based PowerShell na web-aplikacije upravljanje Azure.](app-service-web-app-azure-resource-manager-powershell.md)
