<properties
   pageTitle="Prikazi u rješenja za upravljanje operacije upravljanja paket (OMS) | Microsoft Azure"
   description="Rješenja za upravljanje u paketu operacije upravljanja (OMS) obično neće sadržavati stavku jedan ili više prikaza za vizualni prikaz podataka.  U ovom se članku opisuje kako izvesti prikaz koji je stvoren pomoću dizajnera prikaz, a zatim uključiti u rješenje za upravljanje. "
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
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Prikazi u rješenja za upravljanje operacije upravljanja paket (OMS) (pretpregled)

>[AZURE.NOTE]Ovo je prva verzija dokumentacije za stvaranje rješenja za upravljanje OMS koje su trenutno u pretpregledu. Bilo koji sheme opisanim mogu se promijeniti.    

[Rješenja za upravljanje u paketu operacije upravljanja (OMS)](operations-management-suite-solutions.md) obično neće sadržavati stavku jedan ili više prikaza za vizualni prikaz podataka.  U ovom se članku opisuje kako izvesti u prikazu stvorena pomoću [Dizajner prikaza](../log-analytics/log-analytics-view-designer.md) , a zatim uključiti u rješenje za upravljanje.  

>[AZURE.NOTE]Uzorci u ovom članku pomoću parametara i varijabli koje su potrebne ili zajednički rješenja za upravljanje i što je opisano u [Stvaranje rješenja za upravljanje u paketu operacije upravljanja (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da ste već upoznati s postupkom da biste [stvorili rješenje za upravljanje](operations-management-suite-solutions-creating.md) i strukturu datoteke rješenja.


## <a name="overview"></a>Pregled

Da biste uključili prikaz u rješenje za upravljanje, stvarate **resursa** za njega u [datoteku rješenja](operations-management-suite-solutions-creating.md).  JSON koji opisuje detaljne konfiguraciju u prikazu je obično složene kroz i ne nešto autor uobičajena rješenja bio bi mogao stvoriti ručno.  Najčešći način je stvaranje prikaza pomoću [Dizajner prikaza](../log-analytics/log-analytics-view-designer.md), izvezete ga, a zatim dodajte svoju detaljne konfiguraciju rješenje. 

Slijede Osnovni koraci za dodavanje prikaza rješenje.  Svaki korak je opisan u Dodatno u sljedećim odjeljcima.

1. Izvoz prikaza u datoteku.
2. Stvorite prikaz resursa u rješenje.
3. Dodajte detalje o prikazu.

## <a name="export-the-view-to-a-file"></a>Izvoz prikaza u datoteku
Slijedite upute u [Dizajner prikaza analitičkih podataka za zapisnik](../log-analytics/log-analytics-view-designer.md) Izvoz prikaza u datoteku.  Izvezene datoteke bit će u JSON OSNOVNI oblik s istom [elemente kao datoteku rješenja](operations-management-suite-solutions-creating.md#management-solution-files).  

Element **Resursi** prikaz datoteke će imati resursa s vrstom **Microsoft.OperationalInsights/workspaces** koji predstavlja OMS radnog prostora.  Taj element će imati podelement vrstom **prikaze** koji predstavlja prikaz, a sadrži svoju detaljne konfiguraciju.  Će kopirajte detalje o ovog elementa, a zatim je kopirajte na rješenje.


## <a name="create-the-view-resource-in-the-solution"></a>Stvaranje prikaza resursa u rješenje
U sljedećem članku prikaz dodati element **Resursi** datoteke rješenja.  Koristi se varijabli koje su opisane ispod da morate dodati.  Imajte na umu da svojstva **nadzorne ploče** i **OverviewTile** rezervirana mjesta koja će prebrisati odgovarajuća svojstva iz prikaza izvezene datoteke.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

Dodajte sljedeće varijable element [varijable](operations-management-suite-solutions-creating.md#variables) datoteke rješenja i zamjena vrijednosti onima za rješenje.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Imajte na umu da nije moguće kopirati cijeli prikaz resursa iz prikaza izvezene datoteke, ali morate izvršiti sljedeće izmjene je za rad u rješenje.  

- **Vrsta** prikaza resursa potrebno promijeniti iz **prikaza** za **Microsoft.OperationalInsights/workspaces**.
- Svojstvo **ime** za prikaz resursa potrebno promijeniti da biste dodali naziv radnog prostora.
- Zavisnost o radnom prostoru mora biti uklonjen jer resursa za radni prostor nije definirano u rješenje.
- Svojstvo **riješiti problem** mora biti dodan u prikaz.  **Id**, **naziv**i **riješiti problem** mora sve odgovarati.
- Nazivi parametara moraju promijeniti tako da odgovara potreban skup parametara.
- Varijable treba biti definirani u rješenje i koristiti u odgovarajuća svojstva.

## <a name="add-the-view-details"></a>Dodavanje prikaz detalja
Prikaz resursa u datoteci izvezene prikaz će sadržavati dva elementa u elementu **Svojstva** pod nazivom **nadzorne ploče** i **OverviewTile** koji sadrže detaljne konfiguraciju prikaza.  Kopirajte te dvije elemente i njihov sadržaj u element **Svojstva** prikaz resursa u datoteci rješenja. 

## <a name="example"></a>Primjer
Ako, na primjer, sljedeći primjer prikazuje jednostavno rješenje datoteku s prikazom.  Tri točke (...) se prikazuju za sadržaja **nadzorne ploče** i **OverviewTile** razloga prostora.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Daljnji koraci

- Saznajte sve detalje o stvaranju [rješenja za upravljanje](operations-management-suite-solutions-creating.md).
- Uvrstite [Automatizacija runbooks u rješenje za upravljanje](operations-management-suite-solutions-resources-automation.md).