<properties
   pageTitle="Stvaranje rješenja za upravljanje u paketu operacije upravljanja (OMS) | Microsoft Azure"
   description="Rješenja za upravljanje proširiti mogućnosti funkcije od operacije upravljanja paket (OMS) unosom scenarija zapakirani upravljanja koje korisnici mogu dodavati njihove OMS radnog prostora.  U ovom se članku navodi pojedinosti o kako stvarati rješenja za upravljanje koja će se koristiti u vlastitim okruženju ili učinili dostupnima korisnicima."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Stvaranje rješenja za upravljanje u servis operacije upravljanja paket (OMS) (pretpregled)

>[AZURE.NOTE]Ovo je prva verzija dokumentacije za stvaranje rješenja za upravljanje OMS koje su trenutno u pretpregledu. Bilo koji sheme opisanim mogu se promijeniti.  

Rješenja za upravljanje proširiti mogućnosti funkcije od operacije upravljanja paket (OMS) unosom scenarija zapakirani upravljanja koje korisnici mogu dodavati njihove OMS radnog prostora.  Ovaj članak sadrži detalje o stvaranju vlastite rješenja za upravljanje korištenje u vlastitu okruženju ili oslobađanje klijentima putem zajednice.

## <a name="planning-your-management-solution"></a>Planiranje rješenje za upravljanje
Rješenja za upravljanje OMS uključuju više resursi podrške scenarij određeni upravljanja.  Prilikom planiranja rješenje, trebali biste fokus na scenarij u kojem se pokušavate postigli i sve potrebne resurse za podršku ga.  Svakog rješenja mora biti sebe nalaze i definiranje svakog resursa koje je potrebno, čak i ako druga rješenja i definira jedan ili više resursa.  Nakon instalacije rješenje za upravljanje svakog resursa nastaje, osim ako ga već postoji, a možete definirati što se događa s resursima je rješenje ukloniti.  

Na primjer, rješenje za upravljanje mogu sadržavati programa [automatizacije Azure runbook](../automation/automation-intro.md) koja prikuplja podatke u spremište prijava analitiku pomoću [raspored](../automation/automation-schedules.md) i [Prikaz](../log-analytics/log-analytics-view-designer.md) koji nudi razne vizualizacije prikupljene podatke.  Isti raspored možda koristi drugo rješenje.  Kao autor rješenje upravljanja želite definirati sve tri resurse, ali odredite da runbook i prikaz mora biti uklanjaju se automatski kada je rješenje ukloniti.    Želite definirati raspored, ali odrediti da je ostaju nedirnute Ako rješenje uklonjeni u slučaju da je i dalje se koristi u rješenje.

## <a name="management-solution-files"></a>Upravljanje datoteka rješenja
Rješenja za upravljanje primjenjuju se kao [Upravljanje resursima predlošci](../resource-manager-template-walkthrough.md).  Glavni zadatak u učenjem kako se autor rješenja za upravljanje je učenje kako [autor predloška](../resource-group-authoring-templates.md).  Ovaj članak sadrži jedinstvene pojedinosti predložaka koji se koriste za rješenja i upute za definiranje resursa uobičajena rješenja.

Osnovna struktura datoteka rješenja za upravljanje je isti kao [Predložak Voditelj resursa](resource-group-authoring-templates.md#template-format) koji je na sljedeći način.  Svaki od u sljedećim se odjeljcima opisuju elementi najviše razine i i njihov sadržaj u rješenje.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametri

[Parametri](../resource-group-authoring-templates.md#parameters) se vrijednosti koje su vam potrebne korisniku kada se instalacija rješenja za upravljanje.  Postoji standardni parametre koje će imati sva rješenja, a dodatne parametre po potrebi možete dodati za određenog rješenja.  Kako korisnici će poslati vrijednosti parametara prilikom instalacije rješenje ovisi o određeni parametar i kako se instalira rješenja.


Kada korisnik instalira rješenje za upravljanje putem [Trgovine Windows Azure](operations-management-suite-solutions.md#finding-and-installing-solutions) ili [predložaka za brzi početak rada Azure](operations-management-suite-solutions.md#finding-and-installing-solutions) njih se traži da biste odabrali na [web-mjesto radnog prostora OMS i u okvir za automatizaciju računa](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account).  Koriste se za popunjavanje vrijednosti svakog standardne parametre.  Korisnik neće tražiti izravno unesite vrijednosti za parametre standardne, no oni Zatraži li vrijednosti za svaki dodatni parametar.

Kada korisnik instalira rješenje [drugi način](operations-management-suite-solutions.md#finding-and-installing-solutions), morate navesti vrijednost za sve parametre standardne i sve dodatne parametre.

Ogledna parametar je prikazano u nastavku.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

U sljedećoj tablici opisane atribute parametra.

| Atribut | Opis |
|:--|:--|
| Vrsta        | Vrsta podataka za parametar. Kontrola unosa za korisnika prikazuje ovisi o vrsti podataka.<br><br>booleovom – padajući okvir<br>niz - tekstnog okvira<br>INT - tekstnog okvira<br>securestring - polje lozinke<br> |
| Kategorija    | Neobavezni kategorija za parametar.  Parametri u istoj kategoriji grupiraju. |
| kontrola     | Dodatne funkcije za parametre niz.<br><br>prikazat će se datetime - kontrola datuma i vremena.<br>automatski se generira GUID - Guid vrijednost, a parametar neće se prikazati. |
| Opis | Neobavezan opis parametra.  Prikazuje se u oblačiću za podatke uz parametar. |


### <a name="standard-parameters"></a>Standardni parametara
U sljedećoj su tablici navedeni standardne parametara za sve rješenja za upravljanje.  Ove vrijednosti se unose korisnika umjesto upita za njih kada je rješenje instalirali iz trgovine Azure ili predloške za brzi početak rada.  Korisnik Navedite vrijednosti za njih Ako rješenje se instalira uz druge metode.

>[AZURE.NOTE]U korisničkom sučelju servisa Azure Marketplace i predloške za brzi početak rada očekuje Nazivi parametara u tablici.  Ako koristite Nazivi parametara različite zatim korisnika će se tražiti ih i oni će nije moguće automatski popunjavaju.


| Parametar | Vrsta | Opis |
|:--|:--|:--|
| accountName       | niz |  Azure Automatizacija naziv računa. |
| pricingTier       | niz | Cijene sloju prijava analitiku radnog prostora i račun za Azure automatizaciju. |
| regionId          | niz | Regija račun za Azure automatizaciju. |
| Naziv rješenja      | niz | Naziv rješenja. |
| workspaceName     | niz | Prijava analitiku naziv radnog prostora. |
| workspaceRegionId | niz | Područje radnog prostora zapisnika analize. |





### <a name="sample"></a>Uzorak
Sljedeći je entitet parametar uzorak rješenja.  To obuhvaća sve parametre standardne i dvije dodatne parametre u istoj kategoriji.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Pogledajte vrijednosti parametara u drugim elementima rješenja s sintaksa **parametara ('naziv parametra")**.  Na primjer, da biste pristupili naziv radnog prostora, koristite **parameters('workspaceName')** 

## <a name="variables"></a>Varijable

Element **varijable** sadrži vrijednosti koje ćete koristiti u ostatku rješenje za upravljanje.  Ove vrijednosti ne pojavljuje se korisniku instalacije rješenja.  Oni namijenjena autora s na jednom mjestu gdje možete upravljati vrijednosti koje se može koristiti više puta cijeloj rješenja. Stavite sve vrijednosti određene rješenje u varijable kao opposed za hardcoding ih u elementu **Resursi** .  To čini kod čitljiviji i omogućuje vam da jednostavno promijeniti te vrijednosti u novijim verzijama.

Slijedi primjer element **varijable** s uobičajeni parametri koji se koriste u rješenja.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Pogledajte vrijednostima varijable kroz rješenje s sintaksa **varijable ('naziv varijable")**.  Na primjer, da biste pristupili varijabla naziv rješenja, koristite **variables('solutionName')** 


## <a name="resources"></a>Resursi

Element **Resursi** definira drugi resursi obuhvaćeno rješenje za upravljanje.  To će biti najveće i najčešće složene dio predloška.  Resursi definiraju se sa sljedećom strukturom.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Zavisnosti
Elementi **dependsOn** određuje [ovisnosti](../resource-group-define-dependencies.md) na neki drugi resurs.  Kada instalirate rješenje resurs nije stvoren dok ne stvorite sve njezine ovisnosti.  Ako, na primjer, rješenje možda [pokrenuti na runbook](operations-management-suite-solutions-resources-automation.md#runbooks) kada je instaliran pomoću [resursa za posao](operations-management-suite-solutions-resources-automation.md#automation-jobs).  Resurs za posao bi ovisi o runbook resursa da biste bili sigurni da se stvara u runbook prije stvaranja posao.

### <a name="oms-workspace-and-automation-account"></a>Radni prostor OMS i račun za automatizaciju
Rješenja za upravljanje potreban [prostor OMS](../log-analytics/log-analytics-manage-access.md) sadrži prikaze i [račun za automatizaciju](../automation/automation-security-overview.md#automation-account-overview) runbooks i povezani resursi.  To mora biti dostupno prije resursa u rješenje stvaraju i nije potrebno je definirati u rješenje sam.  Korisnik će [Odredite radni prostor i račun](operations-management-suite-solutions.md#oms-workspace-and-automation-account) kada ih implementaciju rješenja, ali kao autor razmotrite sljedeće točke.


## <a name="solution-resource"></a>Rješenje resursa
Svaki je rješenje potrebno stavke resursa u elementu **Resursi** koji definira rješenje sam.  To će imati vrstu **Microsoft.OperationsManagement/solutions**.  Slijedi primjer resursa rješenja.  U sljedećim odjeljcima opisane su njegov različitih elemenata.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Naziv rješenja
Naziv rješenja mora biti jedinstvena za Azure pretplatu. Sljedeće se preporučene vrijednosti koju želite koristiti.  Koristi se zove **naziv rješenja** za osnovni naziv i parametar **workspaceName** tjednog prikaza kalendara na da biste bili sigurni da je jedinstveni naziv.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Razrješavanje bi s nazivom ovako.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Zavisnosti
Resurs rješenje mora imati [ovisnosti](../resource-group-define-dependencies.md) na svaki drugi resurs u rješenje jer moraju postojati prije stvaranja rješenja.  To se dodavanjem unos za svaki resurs **dependsOn** element.


### <a name="properties"></a>Svojstva
Resurs rješenje ima svojstava u tablici u nastavku.  To obuhvaća resurse poziva i nalaze rješenja koja definira kako upravljati resursa nakon instalacije rješenja.  Svakog resursa u rješenje mora biti navedeno **referencedResources** ili svojstvo **containedResources** .

| Svojstvo | Opis |
|:--|:--|
| workspaceResourceId | ID OMS radnog prostora u obliku * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<naziv radnog prostora\>*. |
| referencedResources   | Popis resursa u rješenje koje se moraju biti uklonjene u je rješenje ukloniti. |
| containedResources   | Popis resursa u rješenje koje treba ukloniti kada je rješenje ukloniti. |

Gornji primjer je rješenje na runbook, raspored i prikaz.  Raspored i runbook su *referencirani* u elementu **Svojstva** tako da se ne uklanjaju kada je rješenje ukloniti.  Prikaz je *sadržani* tako da je uklonjena kada je rješenje ukloniti.


### <a name="plan"></a>Planiranje
Entitet **Planiranje** resursa rješenja ima svojstava u tablici u nastavku. 

| Svojstvo | Opis |
|:--|:--|
| ime          | Naziv rješenja. |
| verzija       | Verzija rješenja odlukom donesenom autor. |
| proizvoda       | Jedinstveni niz za identifikaciju rješenja. |
| Publisher     | Izdavač rješenja. |


## <a name="other-resources"></a>Ostali resursi
Možete dobiti pojedinosti i uzorke resursa koji su zajednički rješenja za upravljanje u sljedećim člancima.

- [Prikazi i nadzorne ploče](operations-management-suite-solutions-resources-views.md)
- [Automatizacija resursi](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Testiranje rješenje za upravljanje
Prije no što implementirate rješenje za upravljanje, preporučuje se testiranje pomoću [Testa AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell).  To će provjeriti vaše datoteke rješenja i pomoć za probleme identificirati prije njegove implementacije kod.



## <a name="next-steps"></a>Daljnji koraci

- Dodatne pojedinosti [Voditelj resursa za Azure za izradu predložaka](../resource-group-authoring-templates.md).
- Pretraživanje [Predložaka za brzi početak rada Azure](https://azure.microsoft.com/documentation/templates) uzorka različite predloške Voditelj resursa.
- Prikaz detalja o za [Dodavanje prikaza rješenje za upravljanje](operations-management-suite-solutions-resources-views.md).
- Prikaz detalja o [dodavanju Automatizacija resursa za rješenje za upravljanje](operations-management-suite-solutions-resources-automation.md).
 