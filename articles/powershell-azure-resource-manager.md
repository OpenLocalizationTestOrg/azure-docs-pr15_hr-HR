<properties 
    pageTitle="Azure PowerShell s Voditelj resursa | Microsoft Azure" 
    description="Uvod u korištenje Azure PowerShell za implementaciju više resurse kao grupu resursa za Azure." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Korištenje Azure PowerShell s Azure Voditelj resursa

> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure EŽA](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST API-JA](resource-manager-rest-api.md)


Azure Voditelj resursa implementira Moderna pristup Azure resursi životni ciklus kontrolu. Umjesto stvaranja i upravljanje pojedinačnih resursa, počnite tako imagining cijeli rješenja, kao što su bloga, Galerija fotografija, SharePoint portal ili wiki. Pomoću predloška – deklarativno predstavljanje rješenja – da biste definirali grupu resursa koji sadrži sve resursa morate podržava rješenja. Nakon toga uvođenje i upravljanje toj grupi resursa kao logička jedinicu. 

U ovom ćete praktičnom vodiču Saznajte kako koristiti Azure PowerShell s Azure Voditelj resursa. Ga vodit će vas kroz postupak implementacije rješenja i rad s tog rješenja. Azure PowerShell i resursima predložak će se koristiti za implementaciju:

- SQL server – za hostiranje baze podataka
- Baze podataka SQL - radi pohrane podataka
- Pravila vatrozida – da biste omogućili web-aplikaciji za povezivanje s bazom podataka
- Planiranje aplikacije servisa za – definiranje mogućnosti i trošak web-aplikacije
- Na web-mjestu – pokretanje web-aplikaciji
- Konfiguracija web - za pohranu niz za povezivanje s bazom podataka 
- Upozorenja pravila - za praćenje performansi i pogreške
- Aplikacija uvida – automatsko skaliranje postavke

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, potrebno je:

- Račun za Azure
  + Možete je [besplatno otvorite račun za Azure](/pricing/free-trial/?WT.mc_id=A261C142F): dohvaćanje kredita možete koristiti da biste isprobali plaćenu servisa Azure i čak i kada se koriste najviše možete zadržati račun i korištenje slobodno Azure servisa, kao što su web-mjesta. Vaša kreditna kartica nikad naplatiti, osim ako izričito Promjena postavki i od nje zatražite da se naplatiti.
  
  + Možete [aktivirati pogodnosti pretplatnika MSDN](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN vaša pretplata vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.
- Azure PowerShell 1.0. Informacije o ovom izdanju i kako ga instalirati potražite u članku [kako instalirati i konfigurirati Azure PowerShell](powershell-install-configure.md).

Pomoću ovog praktičnog vodiča namijenjen za početnike PowerShell, ali pretpostavlja se da razumijete osnovni koncepti, kao što su module, cmdleta i sesije.

## <a name="get-help-for-cmdlets"></a>Pomoć za cmdleta

Da biste dobili detaljnu pomoć za sve cmdlet koja se prikazuju u ovom ćete praktičnom vodiču, koristite cmdlet Get-pomoć. 

    Get-Help <cmdlet-name> -Detailed

Na primjer, da biste dobili pomoć za cmdlet Get-AzureRmResource, upišite:

    Get-Help Get-AzureRmResource -Detailed

Da biste dobili popis cmdleta u modulu resursi s sinopsisa pomoć, upišite: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

Izlaz izgledat će slično kao sljedeći isječak:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Da biste dobili potpunu pomoć za na cmdlet, upišite naredbu u obliku:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Prijavite se na račun za Azure

Prije početka rada rješenje, morate Prijava na račun.

Prijava na račun za Azure, koristite cmdlet za **Dodavanje AzureRmAccount** .

    Add-AzureRmAccount

Cmdlet traži vjerodajnice za prijavu za vaš račun za Azure. Nakon prijave u, ona preuzima postavki računa pa su dostupni za Azure PowerShell. 

Postavke računa istječe, pa ćete morati osvježiti ih povremeno. Da biste osvježili postavke računa, ponovno pokrenite **Dodaj AzureRmAccount** . 

>[AZURE.NOTE] Voditelj resursa module zahtijeva Dodaj AzureRmAccount. Postavke objavljivanja datoteke nije dovoljno.     

Ako imate više pretplata, unesite id pretplatu koju želite koristiti za implementaciju sa cmdlet **Skup AzureRmContext** .

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Stvaranje grupa resursa

Prije implementacije nikakve resurse za pretplatu, morate stvoriti grupu resursa koja će sadržavati resurse. 

Da biste stvorili grupu resursa, pomoću cmdleta **New-AzureRmResourceGroup** .

Naredba koristi **naziv** parametra navesti naziv za grupu resursa i parametar **mjesto** da biste odredili položaju. Ovisno o tome što smo pronađu u prethodnom odjeljku, koristit ćemo "Zapad sad" lokacije.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
Rezultat će biti sličan:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Grupu resursa uspješno je stvorena.

## <a name="deploy-your-solution"></a>Implementacija rješenja

U ovoj se temi ne pokazuju da biste stvorili predložak ili raspravi strukturu predložak. Te informacije potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md) i [Resursima predložak vodič](resource-manager-template-walkthrough.md). Unaprijed definirani [dodjele resursa Web App s bazom podataka SQL](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) predloška iz [Predložaka za brzi početak rada Azure](https://azure.microsoft.com/documentation/templates/)će implementirati.

Grupu resursa, i imate predložak, tako da se sada ste spremni za implementaciju infrastrukture definirano u predlošku u grupu resursa. Implementacija resursima pomoću cmdleta **New-AzureRmResourceGroupDeployment** . Predložak određuje broj zadanih vrijednosti koje koristit ćemo da ne morate unesite vrijednosti za te parametre. Sintaksa osnovni izgleda:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Navedite grupu resursa i mjesto predloška. Ako je vaš predložak lokalne datoteke, koristite parametar **- TemplateFile** i navedite put do predloška. Možete postaviti na **-način** parametar **rastuća** ili **Dovršeno**. Prema zadanim postavkama resursima izvodi rastuće ažuriranje tijekom implementacije; Zbog toga nije ključna da biste postavili **-način** kada želite da se **rastuća**. Da biste razumjeli razlike između tih načina uvođenja, potražite u članku [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Parametri dinamički predložaka

Ako ste upoznati sa servisom PowerShell, znate da možete rotirati dostupnih parametara za na cmdlet tako da upišete znak minus (-), a zatim pritisnite tipku TAB. Ta je funkcija isti funkcionira i sa parametre koje sami definirate u predlošku. Čim upišite naziv predloška, cmdlet dohvaćanja predložak, raščlanjuje ga i dinamički dodaje parametri predložaka naredbu. To olakšava vrlo da biste odredili vrijednosti parametara predložak.

Kada unesete naredbu, od vas će se traži nedostaje obavezna parametar, **administratorLoginPassword**. Možete i kada upišete lozinku, vrijednost sigurne niza prekriti. U ovom strategije uklanja rizika od lozinke u obliku običnog teksta.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Ako predložak sadrži parametar s nazivom koji odgovara jednoj od parametara u naredbu za uvođenje predloška (na primjer, uključujući parametar pod nazivom **ResourceGroupName** u predlošku koji je isti kao parametar **ResourceGroupName** u cmdleta [New AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) ), morat ćete unesite vrijednost za parametar s postfix **FromTemplate** (kao što je **ResourceGroupNameFromTemplate**). Općenito govoreći, izbjegavajte ovaj zbrku ne imenovanje parametara s istim nazivom kao parametar koji se koristi za implementaciju operacije.

Naredba izvodi i vraća poruke kako se stvaraju resurse. Na kraju, vidjet ćete rezultat implementaciju sustava.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

U samo nekoliko koraka ćemo stvoriti i implementirati resursa potrebnih za složene web-mjesta. 

### <a name="log-debug-information"></a>Zapisnički podaci za ispravljanje pogrešaka

Prilikom implementacije predloška, dodatne informacije o zahtjeva i odgovora može prijaviti tako da navedete parametar **- DeploymentDebugLogLevel** kada se pokrene **Nova AzureRmResourceGroupDeployment**. Ove informacije može vam pomoći Otklonite pogreške prilikom implementacije. Zadana je vrijednost za **ništa** što znači da nema zahtjev ili prijavljen je odgovor sadržaj. Možete navesti prijave sadržaj s zahtjev, odgovor ili i jedno i drugo.  Dodatne informacije o implementaciji za otklanjanje poteškoća i bilježenje ispravljanje informacija potražite u članku [implementacijama za grupu resursa otklanjanje poteškoća sa servisom PowerShell Azure](resource-manager-troubleshoot-deployments-powershell.md). U sljedećem primjeru zapisuje sadržaj zahtjeva i odgovora sadržaja za implementaciju.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Prilikom postavljanja parametar DeploymentDebugLogLevel razmisliti o vrstu podataka u su prosljeđivanje tijekom implementacije. Zapisivanje podaci o zahtjeva i odgovora, nije moguće potencijalno izlažu osjetljive podatke dohvaćene putem postupci implementacije. 


## <a name="get-information-about-your-resource-groups"></a>Informacije o grupama za resurse

Kada stvorite grupu resursa, možete koristiti cmdleta u modulu resursima za upravljanje grupama za resursa.

- Da biste dobili grupu resursa u vašoj pretplati, koristite cmdlet **Get-AzureRmResourceGroup** :

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Koja vraća sljedeće informacije:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Ako ne odredite naziv grupe resursa, cmdlet vraća cjelokupan grupe resursa u svoju pretplatu.

- Da biste resursa u grupu resursa, koristite cmdlet **Pronalaženje AzureRmResource** i njezin parametar **ResourceGroupNameContains** . Bez parametara, pronalaženje AzureRmResource dohvaća sve resurse za Azure pretplatu.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Koji vraća popis resursa formatiran kao što su:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- Da biste logično organizirali resursa u svoju pretplatu i dohvatiti resursi web-mjesto **Traži AzureRmResource** i u okvir za **Pretraživanje AzureRmResourceGroup** cmdleta sustava možete koristiti oznake.

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Dodavanje u grupu resursa

Da biste dodali resursa u grupu resursa, možete pomoću cmdleta **New AzureRmResource** . Međutim, dodavanju resursa na taj način mogu zbuniti buduće jer novi resurs ne postoji u predlošku. Ako ponovno implementiran stari predložak, želite implementirati nepotpuna rješenja. Ako implementirate često, pronaći ćete ga lakše i više pouzdane da biste dodali novi resurs predloška i ponovno je implementacija.

## <a name="move-a-resource"></a>Premještanje resursa

Postojećih resursa možete premjestiti u novu grupu resursa. Primjeri, potražite u članku [Premještanje resursa ili pretplatu za novu grupu resursa](resource-group-move-resources.md).

## <a name="export-template"></a>Izvoz predloška

Za postojeće resursa grupe (implementiran putem komponente PowerShell ili od ostalih metoda poput portala), možete pogledati predložak resursima za grupu resursa. Izvoz predložak nudi dva pogodnosti:

1. Jednostavno možete automatizirati buduće implementacije rješenja jer sve Infrastruktura definiran u predlošku.

2. Koje možete se upoznali sa sintaksa predložak tako da pri na JavaScript objekt notaciju (JSON) koji predstavlja rješenje.

Putem komponente PowerShell, možete stvoriti predložak koji predstavlja trenutno stanje grupu resursa, ili dohvatiti predložak koji je korišten za određeni implementaciju.

Izvoz predložak za grupu resursa je korisno kada ste unijeli promjene u grupu resursa i zatrebati predstavljanje JSON njegovu trenutnom stanju. No stvoreni predložak sadrži samo Minimalni broj parametara i bez varijabli. Većina vrijednosti u predložak je programiranih. Prije no što implementirate stvoreni predložak želite pretvoriti više vrijednosti parametara tako da možete prilagoditi implementacije za različite okruženja.

Izvoz predložak za određeni implementaciju je korisno kada je potrebno da biste prikazali stvarne predložak koji je korišten za implementaciju resursi. Predložak neće sadržavati sve parametre i definirano izvorne implementacije varijabli. Međutim, ako netko u tvrtki ili ustanovi ima unijeli promjene u grupi resursa izvan što je definiran u predlošku, ovaj predložak ne predstavlja trenutno stanje grupu resursa.

> [AZURE.NOTE] Značajka predložak izvoz je u pretpregledu, a resursa podržavaju sve vrste trenutno izvoz predloška. Pri pokušaju da biste izvezli predloška, vidjet ćete pogrešku kojoj se navodi da neki resursi nisu izvezeni. Ako je potrebno, ručno možete definirati tih resursa u predlošku nakon preuzimanja.

###<a name="export-template-from-resource-group"></a>Izvoz predloška iz grupa resursa

Da biste pogledali predložak za grupu resursa, pokrenite cmdlet **Izvoz AzureRmResourceGroup** .

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Preuzimanje predloška iz implementacije

Da biste preuzeli predložak koji se koristi za određeni implementaciju, pokrenite cmdlet **Spremi AzureRmResourceGroupDeploymentTemplate** .

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Resursi za brisanje ili grupa resursa

- Da biste izbrisali resursa iz grupe resursa, koristite cmdlet **Ukloni AzureRmResource** . Ovaj cmdlet briše resursa, no izbrisati grupu resursa.

    Ta se naredba uklanja TestSite web-mjesta iz grupe resursa TestRG1.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Da biste izbrisali grupu resursa, koristite cmdlet **Ukloni AzureRmResourceGroup** . Ovaj cmdlet briše grupu resursa i njegovih resursa.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    Se od vas zatraži da potvrdite brisanje.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Skripta za implementaciju

Starije implementacije primjeri u ovoj temi prikazivao samo pojedinačne cmdleta potrebne za implementaciju resursi za Azure. Sljedeći primjer prikazuje implementacijsku skriptu koja stvara grupu resursa i implementira resurse.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o stvaranju predložaka Voditelj resursa, potražite u članku [Za izradu predložaka Azure Voditelj resursa](./resource-group-authoring-templates.md).
- Da biste saznali više o implementaciji predložaka, potražite u članku [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](./resource-group-template-deploy.md).
- Detaljne primjer implementacije projekta, potražite u članku [uvođenja microservices predvidljivije u Azure](app-service-web/app-service-deploy-complex-application-predictably.md).
- Dodatne informacije o otklanjanju poteškoća s implementacije koja nije uspjela, potražite u članku [Otklanjanje poteškoća implementacijama za grupu resursa u Azure](./resource-manager-troubleshoot-deployments-powershell.md).

