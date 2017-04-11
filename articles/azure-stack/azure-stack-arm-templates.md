<properties
    pageTitle="Korištenje predložaka Azure upravljanja resursima u stogu Azure (razvojni inženjeri klijenta) | Microsoft Azure"
    description="Saznajte kako koristiti predloške Azure upravljanja resursima u stogu Azure implementacije i dodjela resursa za sve resurse za aplikacije u operaciji jedinstvenog i usklađenih."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Korištenje predložaka Azure upravljanja resursima u stogu Azure

Azure resursima predlošci implementaciju, i dodjela resursa za sve resurse za aplikacije u operaciji jedinstvenog i usklađenih. Definiranje resursa za aplikaciju i kako će biti implementirano.  Možete i implementirati predloške da biste promijenili resursa u grupu resursa.

S portala sustava Microsoft Azure snop, PowerShell, naredbeni redak i Visual Studio može uvesti te predloške.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

Dostupni na [GitHub](http://aka.ms/azurestackgithub)su sljedeći predlošci:

## <a name="deploy-sharepoint-non-high-availability"></a>Implementacija sustava SharePoint (do visoka dostupnost)

Koristite PowerShell DSC proširenje za stvaranje farmi sustava SharePoint 2013 koji obuhvaća sljedeće:

-   Virtualne mreže

-   Tri računa za pohranu

-   Dva balancers vanjskih učitavanja

-   Jedan VM konfigurirati kao kontroler domene za skup stabala za novo s jedne domene

-   Jedan VM konfigurirati kao samostalni poslužitelj 2014 SQL Server.

-   Jedan VM konfigurirana kao jedan strojno farma sustava SharePoint 2013

## <a name="deploy-ad-non-high-availability"></a>Implementacija servisa Active Directory (do visoka dostupnost)

Proširenje PowerShell DSC možete koristiti za stvaranje AD domene kontroler poslužitelj za koji obuhvaća sljedeće:

-   Virtualne mreže

-   Jednog računa za pohranu

-   Jedan vanjski opterećenja

-   Jedan VM konfigurirati kao kontroler domene za skup stabala za novo s jedne domene

## <a name="deploy-adsql-non-high-availability"></a>Implementacija servisa Active Directory/SQL (do visoka dostupnost)

Proširenje PowerShell DSC možete koristiti za stvaranje 2014 SQL Server samostalne poslužitelje koji obuhvaća sljedeće:

-   Virtualne mreže

-   Dva računa za pohranu

-   Jedan vanjski opterećenja

-   Jedan VM konfigurirati kao kontroler domene za skup stabala za novo s jedne domene

-   Jedan VM konfigurirati kao samostalni poslužitelj 2014 SQL Server.

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Konfigurirajte postojeće virtualnog računala lokalni Upravitelj konfiguracije (LCM) i registrirati Azure Automatizacija račun DSC istaknuti poslužitelju pomoću proširenje PowerShell DSC.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Stvaranje virtualnog računala iz slike korisnika

Stvaranje virtualnog računala s korisnička slike. Ovaj predložak i uvodi virtualne mreže (uz postavke DNS-a), javnu IP adresa i sučelje mreže.

## <a name="simple-vm"></a>Jednostavan VM

Implementacija jednostavne VM Windows koja sadrži virtualne mreže (uz postavke DNS-a), javnu IP adresa i sučelje mreže.

## <a name="cancel-a-running-template-deployment"></a>Otkazivanje izvodi uvođenje predloška

Da biste poništili izvodi uvođenje predloška, koristite na `Stop-AzureRmResourceGroupDeployment` cmdlet ljuske PowerShell.


## <a name="next-steps"></a>Daljnji koraci

[Uvođenje predložaka s portala](azure-stack-deploy-template-portal.md)

[Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md)

