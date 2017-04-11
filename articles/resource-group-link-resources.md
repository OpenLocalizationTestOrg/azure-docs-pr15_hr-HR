<properties 
    pageTitle="Povezivanje resursa u Azure Voditelj resursa | Microsoft Azure" 
    description="Stvaranje veze između povezanih resursa u grupama različite resursa u Azure Voditelj resursa." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Povezivanje resursa u Azure Voditelj resursa

Tijekom implementacije, možete označiti resursa kao ovisi o drugi resurs, no taj životni ciklus završava na implementaciju. Nakon implementacije, nema identificirani odnosa između zavisne resursi. Voditelj resursa pruža značajku resursa povezivanje da biste uspostavili trajnog odnose između resursi.

S povezivanjem resursa, možete dokumenta odnosa koji obuhvaćaju grupa resursa. Ako, na primjer, uobičajeno je da bi baze podataka pomoću vlastitu životni ciklus nalaze u jednu grupu resursa i aplikacije s različitim životni ciklus nalaze u grupi drugi resurs. Aplikaciju povezuje s bazom podataka tako da želite označiti vezu između aplikacija i baze podataka. 

Sve povezane resurse mora pripadati iste pretplate. Svaki resurs može povezati s 50 ostali resursi. Na upit povezani resursi se samo putem REST API-JA. Ako bilo koji od povezani Resursi su izbrisali ili premjestili, vlasnik vezu morate očistiti preostale vezu. Vi ste **ne** upozoreni kada brisanje resursa koja je povezana s drugih resursa.

## <a name="linking-in-templates"></a>Povezivanje u predlošcima

Da biste definirali vezu na predložak, uvrstite vrsta resursa koji kombinira prostora za naziv davatelja resursa i vrsta izvora resurs s **/providers/links**. Naziv mora sadržavati naziv resursa izvora. Navedite identifikator resursa resursa cilj. U sljedećem primjeru uspostavlja vezu između web-mjesta i račun za pohranu.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Opis cijelog u obliku predloška potražite u članku [resursa veze - shemu predloška](resource-manager-template-links.md).

## <a name="linking-with-rest-api"></a>Povezivanje s REST API-JA

Da biste definirali vezu između distribuiranih resursa, pokrenite:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

{Id pretplate} zamijenite identifikacijskog broja za pretplatu. Zamjena {resursa grupni}, {prostora za naziv davatelja usluge, {resursa – vrsta} i {-naziv resursa} s vrijednostima koje označavaju prvi resursa u vezu. {Naziv veze} zamijenite nazivom vezu da biste stvorili. Korištenje 2015-01-01 api-verzije.

U zahtjevu za obuhvaćaju objekt koji definira drugi resursa u vezi:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Element svojstva sadrži identifikator za drugi resurs.

Upit je možete veze u pretplatu:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Još primjera, uključujući Dohvaćanje informacija o vezama, potražite u članku [Povezani resursi](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## <a name="next-steps"></a>Daljnji koraci

- Možete organizirati i resursi s oznakama. Da biste saznali više o označavanju resursa, potražite u članku [Korištenje oznake da biste organizirali resurse](resource-group-using-tags.md).
- Opis stvaranju predložaka i definiranje resursa koji će biti implementirano, potražite u članku [Predlošci na dokumentima](resource-group-authoring-templates.md).
