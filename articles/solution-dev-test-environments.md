<properties
   pageTitle="Razvoj i testiranje okruženja | Microsoft Azure"
   description="Saznajte kako koristiti predloške Voditelj resursa Azure dosljedno i brzo stvaranje i brisanje razvoj i testiranje okruženja."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Razvoj i testiranje okruženja u Microsoft Azure

Prilagođene aplikacije obično su implementiran na više razvoja i testiranja okruženja prije implementacije proizvodnje. Prilikom stvaranja okruženja lokalno, računalnih resurse se nabavljaju ili za svako okruženje za svaku aplikaciju. Ta okruženja često sadrže nekoliko fizičke ili virtualnim strojevima s određenim konfiguracije koji su raspoređeni ručno ili složene Automatizacija skripti. Implementacijama često potrajati sati, a rezultat u kojima nisu dosljedno konfiguracijama preko okruženja.

## <a name="scenario"></a>Scenarij ##

Prilikom dodjele resursa razvoj i testiranje okruženja u Microsoft Azure, samo platiti za resurse koji koristite.  U ovom se članku objašnjava kako brzo i dosljedno možete stvarati, zadržali, i brisanje razvoj i testiranje okruženja pomoću predložaka Voditelj resursa Azure i datoteke parametar, kao što je prikazano ispod.

![Scenarij](./media/solution-dev-test-environments/scenario.png)

Tri razvoja i testiranja okruženja u kojima se prikazuju se iznad.  Svaka ima web-aplikacija i resursa SQL baze podataka koji su navedeni u datoteku predloška.  Nazivi računala i baze podataka u svakom okruženju razlikuju, a su navedeni u datotekama jedinstveni parametar za svako okruženje.  

Ako niste upoznati s koncepata Voditelj resursa Azure, preporučuje se prije pročitajte ovaj članak pročitajte u članku [Pregled upravljanja resursima Azure](azure-resource-manager/resource-group-overview.md) .

Preporučujemo vam da najprije proći kroz korake u ovom članku kao što je navedeno bez čitanje svih članaka referencirani brzo dobiti iskustva s predlošcima Azure Voditelj resursa. Kada je što ste bili kroz korake puta, ćete moći Saznajte odgovore na najčešće pitanja na koja su prve vremena kroz eksperimentiranja daljnje korake i tako da pročitate referencirani članke.

## <a name="plan-azure-resource-use"></a>Planiranje korištenja resursa za Azure
Nakon što dodate više razine dizajna za aplikaciju, možete definirati:

- Odabir resursa koji Azure aplikacija neće sadržavati stavku. Možda stvorite aplikaciju i implementirajte ga kao Azure Web App s baze podataka SQL Azure.  Možda stvaranja aplikacija na virtualnim računalima sustava pomoću PHP i MySQL IIS i SQL Server ili drugih komponenti. U članku [aplikacije servisa za Azure, servise u Oblaku i Usporedba virtualnim strojevima]( app-service-web/choose-web-site-cloud-service-vm.md) olakšava odlučite Azure resursa koji želite koristiti za svoju aplikaciju.
- Što servisa razine preduvjeti kao što su dostupnost, sigurnost i skaliranje koji će zadovoljavaju vaše aplikacije.

## <a name="download-an-existing-template"></a>Preuzimanje postojećeg predloška
Predložak programa upravitelj resursa Azure definira sve Azure resurse koji koristi vaše aplikacije. Nekoliko predložaka već postoji da možete uvesti izravno na portalu Azure ili preuzimanje, izmjena i spremite u izvorišnom sustavu kontrola s kodu aplikacije.  Slijedite korake u nastavku da biste preuzeli postojećeg predloška.

1. Pregled postojećih predložaka u spremištu GitHub [Azure predložaka za brzi početak rada](https://github.com/Azure/azure-quickstart-templates/) . Na popisu vidjet ćete u mapu "[201 – web-aplikacije-sql-baze podataka](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)". Budući da više prilagođenih aplikacija obuhvatili web-aplikacija i SQL baze podataka, ovaj predložak služi kao primjer ostatak u ovom se članku pomoću kojih se objašnjava kako koristiti predloške. Ga izlazi iz ovog članka u potpunosti objašnjenja sve ovaj predložak stvara i konfigurira, ali ako planirate koristiti za stvaranje stvarni okruženja u kojima se u vašoj tvrtki ili ustanovi, ćete htjeti u potpunosti je razumjeti tako da pročitate članak [dodjele resursa web app s bazom podataka SQL](app-service-web/app-service-web-arm-with-sql-database-provision.md) .
Napomena: Ovaj članak verzija 2015 prosinac 201 – web-aplikacije-sql - predloška [baze podataka](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) . Veza u nastavku koji pokažite na predložak, a parametar datoteke su za tu verziju predloška.
2. Kliknite datoteku [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) u mapu 201 – web-aplikacije-sql-baze podataka da biste vidjeli njezin sadržaj. Ovo je datoteku predloška Azure Voditelj resursa. 
3. U načinu prikaza, kliknite gumb "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)". 
4. Pomoću miša, odaberite sadržaj ove datoteke, a možete ih spremiti na računalo kao datoteku pod nazivom "TestApp1-Template.json." 
5. Pregledajte sadržaj predloška i Primijetit ćete sljedeće:
 - Odjeljak **resursa** : u ovom se odjeljku definira vrste Azure resursi stvorio ovaj predložak. Među druge vrste resursa ovaj predložak stvara [Azure Web App](app-service-web/app-service-web-overview.md) i [Baze podataka SQL Azure](sql-database/sql-database-technical-overview.md) resurse. Ako želite pokrenuti i upravljanje web i SQL Server na virtualnim računalima sustava, možete koristiti predloške "[iis-2vm-sql-1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" ili "[žarulje aplikacije](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)", ali upute u ovom članku temelje se na predlošku [201 – web-aplikacije-sql-baze podataka](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) .
 - Odjeljak **parametara** : u ovom se odjeljku definira parametre koje se može konfigurirati svaki resurs. Neke od predložak obrasca u predlošku imaju svojstva "defaultValue", dok drugi ne. Kada implementirate Azure resursa s predloškom, navedite vrijednosti za sve parametre koje ste defaultValue svojstva naveden u predlošku.  Ako ne unesete vrijednosti za parametre sa svojstvima defaultValue, koristi se vrijednost navedena za parametar defaultValue u predlošku.

Predložak određuje resursa koji Azure stvaraju i parametre svaki resurs može konfigurirati. Dodatne informacije o predlošcima i upute za dizajniranje vlastite tako da za čitanje u članku [najbolje prakse za dizajniranje predložaka Voditelj resursa Azure](best-practices-resource-manager-design-templates.md) .

## <a name="download-and-customize-an-existing-parameter-file"></a>Preuzeti i prilagoditi postojeće datoteke parametra

Iako trebali na *istom* Azure resursa stvorene u svakom okruženju, vjerojatno željet ćete konfiguracije resursa koji će se *različite* u svakom okruženju.  Ovo je odakle parametar datoteke. Stvaranje parametra datoteke koje sadrže jedinstvene vrijednosti u svakom okruženju s povećanim korake u nastavku.   

1. Prikaz sadržaja [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) datoteke u mapi 201 – web-aplikacije-sql-baze podataka. Ovo je parametar datoteka za datoteku predloška koji ste spremili u prethodnom odjeljku. 
2. U načinu prikaza, kliknite gumb "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)". 
3. Mišem, odaberite sadržaj ove datoteke, a možete ih spremiti na tri zasebna datoteka na računalu s nazivima sljedeće:
 - TestApp1 parametara Development.json
 - TestApp1 parametara Test.json
 - TestApp1-parametara-Pre-Production.json

3. Koristite neki tekst ili uređivač JSON, uredite datoteku parametar okruženje za razvoj stvorenom u prvom koraku 3 zamjenu vrijednosti na popisu s desne strane vrijednosti parametara u datoteci *vrijednosti* na popisu s desne strane **parametara** ispod: 
 - **NazivWebMjesta**: *TestApp1DevApp*
 - **hostingPlanName**: *TestApp1DevPlan*
 - **siteLocation**: *Središnje SAD -a*
 - **naziv poslužitelja**: *testapp1devsrv*
 - **serverLocation**: *Središnje SAD -a*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *zamijenite lozinku*
 - **ImeBazePodataka**: *testapp1devdb*

4. Pomoću neki tekst ili uređivač JSON, uredite datoteku Test okruženje parametar koji ste stvorili u koraku 3, zamjene u vrijednosti na popisu s desne strane vrijednosti parametara u datoteci *vrijednosti* na popisu s desne strane **parametara** ispod:
 - **NazivWebMjesta**: *TestApp1TestApp*
 - **hostingPlanName**: *TestApp1TestPlan*
 - **siteLocation**: *Središnje SAD -a*
 - **naziv poslužitelja**: *testapp1testsrv*
 - **serverLocation**: *Središnje SAD -a*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *zamijenite lozinku*
 - **ImeBazePodataka**: *testapp1testdb*

5. Koristite neki tekst ili uređivač JSON, uredite prije proizvodnje parametar datoteku koju ste stvorili u koraku 3.  Što je ispod zamijenite cijeli sadržaj datoteke:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

U datoteci stara radnog parametara iznad parametara **sku** i **requestedServiceObjectiveName** su *dodali*, dok su nije dodao u datotekama parametara za razvoj i testiranje. Razlog je tome što su zadane vrijednosti za parametara u predlošku i u okruženju razvoj i testiranje koriste zadane vrijednosti, ali u stara radnog okruženja koje nisu zadane vrijednosti parametara koriste.

Razlog koje nisu zadane vrijednosti koje se koriste za parametara u okruženju stara radni je da biste testirali vrijednosti parametara možda radije okruženju sustava radnog tako da ih možete i testirati.  Parametara sve odnose se na Azure [hostinga tarife Web App](https://azure.microsoft.com/pricing/details/app-service/)ili **sku** i Azure [SQL baze podataka](https://azure.microsoft.com/pricing/details/sql-database/)ili **requestedServiceObjectiveName** koji se koriste u aplikaciji.  Različite SKU-ove i ciljna nazivi servisa imaju različite troškove i značajke te podržava servis za različite razine metriku.

U tablici u nastavku navedeni zadanih vrijednosti parametara navedene u predlošku i vrijednosti koje se koriste se umjesto zadane vrijednosti u datoteci s parametrima prije proizvodnje.

| Parametar | Zadana vrijednost | Vrijednost parametra datoteka |
|---|---|---|
| **SKU** | Besplatni | Standardna |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Stvaranje okruženja
Svi Azure resursi moraju se stvoriti unutar [Azure grupu resursa](azure-resource-manager/resource-group-overview.md). Grupa resursa omogućuju vam da biste grupirali Azure resurse tako skupno upravlja.  [Dozvole](./active-directory/role-based-access-built-in-roles.md) se mogu dodijeliti grupama resursa tako da određene osobe u vašoj tvrtki ili ustanovi mogu stvaranje, izmjena, brisanje i prikaz ih i resursima u njima.  [Portal za Azure](https://portal.azure.com)moguće je prikazati podatke o naplati za resurse u grupu resursa i upozorenja. Grupa resursa stvaraju se u Azure [regija](https://azure.microsoft.com/regions/).  U ovom se članku sve resurse stvaraju se u području središnje SAD-a. Kada pokrenete stvaranje stvarni okruženja, odabrat ću regija koji najbolje odgovara potrebama. 

Stvaranje grupe resursa za svako okruženje na bilo koji od metoda navedenih u nastavku.  Sve načine će postigli isti rezultat.

###<a name="azure-command-line-interface-cli"></a>Azure naredbeni redak sučelja (EŽA)

Provjerite je li da imate EŽA [instalirana](xplat-cli-install.md) na Windows OS X ili Linux računala i kojima ste [povezani](xplat-cli-connect.md) [račun za Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (naziva se i račun tvrtke ili obrazovne ustanove) u pretplatu za Azure. U naredbeni redak EŽA upišite naredbu dolje da biste stvorili grupu resursa za razvojno okruženje.

    azure group create "TestApp1-Development" "Central US"

Ako se potvrdi naredbu će vratiti sljedeće:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Da biste stvorili grupu resursa za testiranje okruženje, upišite naredbe u nastavku:

    azure group create "TestApp1-Test" "Central US"

Da biste stvorili grupu resursa stara radnom okruženju, upišite naredbe u nastavku:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>PowerShell

Provjerite imate li Azure PowerShell 1.01 ili noviji instaliran na računalu sa sustavom Windows i ste povezali svoj [račun za Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (naziva se i račun tvrtke ili obrazovne ustanove) za svoju pretplatu kao detaljne u članku [upute za instalaciju i konfiguriranje Azure PowerShell](powershell-install-configure.md) . Iz naredbenog retka za PowerShell upišite naredbu dolje da biste stvorili grupu resursa za razvojno okruženje.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

Ako se potvrdi naredbu će vratiti sljedeće:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Da biste stvorili grupu resursa za testiranje okruženje, upišite naredbe u nastavku:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Da biste stvorili grupu resursa stara radnom okruženju, upišite naredbe u nastavku:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Portal za Azure

1. Prijavite se na [portal za Azure](https://portal.azure.com) s računom [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (naziva se i tvrtke ili obrazovne ustanove). Kliknite novi--> Upravljanje--> grupa resursa i unesite "TestApp1 razvoj" u okvir naziv za grupu resursa, odaberite svoju pretplatu i odaberite "Središnje mi" u okvir mjesto grupu resursa kao što je prikazano na slici u nastavku.
   ![Portal](./media/solution-dev-test-environments/rgcreate.png)
2. Kliknite gumb Stvori da biste stvorili grupu resursa.
3. Kliknite Pregledaj, pomaknite se prema dolje na popisu grupe resursa, a zatim na grupe resursa kao što je prikazano u nastavku.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png) 
4. Nakon klika na grupe resursa vidjet ćete plohu grupe resursa s novu grupu resursa.
   ![Portal](./media/solution-dev-test-environments/rgview.png)
5. Stvaranje TestApp1-Test i TestApp1 Pre proizvodnog resursa grupe na isti način na koji ste stvorili iznad TestApp1 razvoj grupu resursa.

##<a name="deploy-resources-to-environments"></a>Implementacija resursi okruženja

Implementacija Azure resursi grupa resursa za svako okruženje pomoću datoteke predloška za rješenje i parametar datoteke za svako okruženje pomoću bilo koju od metoda navedenih u nastavku.  Obje metode će postigli isti rezultat.

###<a name="azure-command-line-interface-cli"></a>Azure naredbeni redak sučelja (EŽA)

Iz naredbenog retka EŽA upišite naredbu ispod za implementaciju resursa u grupu resursa koju ste stvorili za razvojno okruženje zamjena [put] put do datoteke koju ste spremili u prethodnim koracima.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

Nakon što se prikazuje poruka "Čeka se implementaciju da biste dovršili" za nekoliko minuta, naredba će vratiti sljedeće ako ga potvrdi:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Ako se naredba nije uspjela, riješiti sve poruke o pogreškama i pokušajte ponovno.  Uobičajeni problemi koriste vrijednosti parametara koja drži Azure resursa imenovanja ograničenja. Drugi Savjeti za otklanjanje poteškoća pronaći ćete u članku [Otklanjanje poteškoća implementacijama za grupu resursa u Azure](./resource-manager-troubleshoot-deployments-cli.md) .

Iz naredbenog retka EŽA upišite naredbu ispod za implementaciju resursa u grupu resursa koju ste stvorili za testnom okruženju zamjena [put] put do datoteke koju ste spremili u prethodnim koracima.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

Iz naredbenog retka EŽA upišite naredbu ispod za implementaciju resursa u grupu resursa koji ste stvorili za okruženje stara radnog zamjena [put] put do datoteke koju ste spremili u prethodnim koracima.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>PowerShell

Iz programa Azure PowerShell (verzija 1.01 ili noviji) naredbeni redak upišite naredbu ispod za implementaciju resursa u grupu resursa koju ste stvorili za razvojno okruženje zamjena [put] put do datoteke koju ste spremili u prethodnim koracima.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Nakon što se prikazuju u trepće za nekoliko minuta, naredba će vratiti sljedeće ako ga potvrdi:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Ako se naredba nije uspjela, riješiti sve poruke o pogreškama i pokušajte ponovno.  Uobičajeni problemi koriste vrijednosti parametara koja drži Azure resursa imenovanja ograničenja. Drugi Savjeti za otklanjanje poteškoća pronaći ćete u članku [Otklanjanje poteškoća implementacijama za grupu resursa u Azure](./resource-manager-troubleshoot-deployments-powershell.md) .

  Iz naredbenog retka za PowerShell upišite naredbu ispod za implementaciju resursa u grupu resursa koju ste stvorili za testnom okruženju zamjena [put] put do datoteke koju ste spremili u prethodnim koracima.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  Iz naredbenog retka za PowerShell upišite naredbu ispod za implementaciju resursa u grupu resursa koji ste stvorili za okruženje stara radnog zamjena [put] put do datoteke koju ste spremili u prethodnim koracima.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

Datoteka predloška i parametar može biti određuju i održavaju kod aplikacija u sustavu izvor kontrole.  Nije moguće spremiti i naredbe iznad da biste skripte datoteke i spremite ih s svoj kod.

> [AZURE.NOTE] Uvođenje predloška za Azure izravno tako da kliknete gumb "Implementacija za Azure" na članak [dodjele resursa Web App s bazom podataka sustava SQL](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) .  Korisne sljedeće informacije o predlošcima, ali time ne omogućuju uređivanje verzija i spremite predložak, a parametar vrijednosti kod aplikacije da dodatne pojedinosti o ovu metodu nije obuhvaćeno ovog članka.

## <a name="maintain-environments"></a>Održavanje okruženja
Cijeloj razvoj konfiguracije Azure resursa u različitim okruženjima možda nedosljedno moguća namjerno ili slučajno.  To može uzrokovati nepotrebne otklanjanje poteškoća i rješavanje problema tijekom ciklusa razvoja aplikacije.

1. Promjena u okruženju tako da otvorite [portal za Azure](https://portal.azure.com). 
2. Prijavite se u njega s istom račun koji se koristi da biste dovršili gore navedene korake. 
3. Kao što je prikazano na slici u nastavku, kliknite Pregledaj--> grupa resursa (možda morati pomaknuti prema dolje da biste vidjeli grupe resursa).
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png)
4. Nakon klika na grupe resursa na gornjoj slici, vidjet ćete plohu grupe resursa i tri resursa grupe koje ste stvorili u prethodnom koraku kao što je prikazano na slici u nastavku. Kliknite grupu resursa TestApp1 razvoj i prikazat će se plohu koja sadrži popis resursa stvorene u predlošku implementacije grupu resursa TestApp1 razvoj dovršiti u prethodnom koraku.  Izbrišite resursa TestApp1DevApp Web App tako da kliknete TestApp1DevApp u grupi plohu TestApp1 razvoj resursa, a zatim Izbriši u aplikaciji plohu TestApp1DevApp Web.
   ![Portal](./media/solution-dev-test-environments/portal2.png)
5. Kliknite 'Da' kada portala za vas kao li ste sigurni da želite izbrisati resurs. Zatvorite plohu grupu resursa TestApp1 razvoj i ponovno je otvorite sada prikazat će se bez web-aplikaciju koju ste upravo izbrisali.  Sada su drugačije nego trebali bi sadržaj u grupu resursa. Dodatno možete isprobati brisanjem višestrukih resursa iz više grupa resursa ili čak i promjena postavki konfiguriranje za neki od resursa. Umjesto pomoću portala za Azure da biste izbrisali resursa iz grupe resursa, možete koristiti naredbe ljuske PowerShell [Ukloni AzureResource](https://msdn.microsoft.com/library/azure/dn757676.aspx) ili ili naredbe "azure resursa Izbriši" iz u EŽA da biste izvršili isti zadatak.
6. Da biste sve resurse i konfiguracije koji bi se u grupama resursa u stanje trebali bi trebali, ponovno implementirati okruženja grupe resursa pomoću iste naredbe koje ste koristili u odjeljku [uvođenja resurse za okruženja](#deploy-resources-to-environments) , ali zamijeniti "Deployment1" s "Deployment2."
7.  Kao što je prikazano u odjeljku sažetak plohu TestApp1 razvoj slici prikazano u koraku 4, vidjet ćete da web-aplikaciju koju ste izbrisali portal u prethodnom koraku postoji ponovno, kao ostali resursi možda ste odabrali da biste izbrisali. Ako ste promijenili konfiguraciju neki od resursa, možete provjeriti da ste je postavljaju natrag na vrijednosti u datotekama parametar previše. Jedna od prednosti implementacija vašeg okruženja s predlošcima Voditelj resursa Azure je da možete lako ponovno implementirati okruženja u poznate stanje u bilo kojem trenutku.
8. Ako kliknete na tekst u odjeljku "Posljednje implementacije" slici u nastavku, vidjet ćete plohu koji prikazuje povijesti uvođenja za grupu resursa.  Budući da koristi naziv "Deployment1" za prve implementacije i "Deployment2" drugi implementacije, imat ćete dvije stavke.  Klikom na implementacije prikazat će se plohu koji prikazuje rezultate za svaki implementacije.
  ![Portal](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Brisanje okruženja
Kada završite s korištenjem okruženju, ćete htjeti izbrisati tako da ne plaćati naknade za korištenje za Azure resurse koji se više ne koristite.  Brisanje okruženja u kojima je pojednostaviti od njihovo stvaranje.  U prethodnim koracima, pojedinačne Azure resursa grupe stvorene za svako okruženje, a zatim su resursi implementiran u grupe resursa. 

Brisanje okruženja na bilo koji od metoda navedenih u nastavku.  Sve načine će postigli isti rezultat.

### <a name="azure-cli"></a>Azure EŽA

Iz EŽA zatraži, upišite sljedeće:

    azure group delete "TestApp1-Development"

Kada se to od vas zatraži, unesite y, a zatim pritisnite enter da biste uklonili razvojno okruženje i sve njegove resurse. Nakon nekoliko minuta, naredba će vratiti sljedeće:

    info:    group delete command OK

Iz EŽA zatraži, upišite sljedeće da biste izbrisali otvoreni okruženja:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>PowerShell

Iz programa Azure PowerShell (verzija 1.01 ili noviji) naredbeni redak upišite naredbu dolje da biste izbrisali grupu resursa i njegov sadržaj.    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Kada se to od vas zatraži ako ste sigurni da želite ukloniti grupe resursa, unesite y, a zatim tipku enter.

Upišite sljedeće da biste izbrisali otvoreni okruženja:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Portal za Azure

1. Na portalu Azure pronađite grupa resursa kao i u prethodnim koracima. 
2. Odaberite grupu resursa TestApp1 razvoj, a zatim kliknite Izbriši u grupi plohu TestApp1 razvoj resursa. Pojavit će se novi plohu. Unesite naziv grupe resursa i kliknite gumb Izbriši.
![Portal](./media/solution-dev-test-environments/rgdelete.png)
3. Izbrišite TestApp1-Test i TestApp1 Pre proizvodnog resursa grupe na isti način kao i izbrisati grupu resursa TestApp1 razvoj.

Bez obzira na način na koji koristite, grupa resursa, a svi oni nalaze resursi će više ne postoji, a više će uzrokovati naplatu troškova za resurse.  

Da biste minimizirali na Azure resursa Upotreba troškove plaćati tijekom razvoj aplikacija [Azure Automatizacija](automation/automation-intro.md) možete koristiti da biste zakazali zadaci koji:

- Zaustavite virtualnim strojevima na kraju svakog dana i ponovno pokrenite na početku svakog dana.
- Brisanje cijelih okruženja na kraju svakog dana i ponovno stvoriti na početku svakog dana.
 
Sad kad ste naišli kako je jednostavno možete stvarati, zadržali, i brisanje razvoj i testiranje okruženja, možete saznati više o mogućnostima samo jeste li dodatno eksperimentiranja s gore navedene korake i referencama koje se nalaze u ovom članku za čitanje.

## <a name="next-steps"></a>Daljnji koraci

- [Delegat administratorske kontrole](./active-directory/role-based-access-control-configure.md) na drugu resurse u svakom okruženju tako da Microsoft Azure AD grupama ili pojedinačnim korisnicima dodijelite određene uloge koje za bicikle podskup operacije Azure resursa.
- [Dodjela oznake](resource-group-using-tags.md) grupa resursa za svako okruženje i/ili pojedinačnih resursa. Možete dodati oznaku "Okruženje" grupama resursa i postavite njegove vrijednost tako da odgovaraju nazivima vaše okruženje. Oznaka može biti osobito korisni kada je potrebno da biste organizirali resursi za naplatu ili upravljanje.
- Praćenje upozorenja i naplata za resurse za grupu resursa [Azure portal](https://portal.azure.com).
