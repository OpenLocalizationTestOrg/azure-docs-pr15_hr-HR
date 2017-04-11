<properties
    pageTitle="Implementacija predložaka uz naredbeni redak u stogu Azure | Microsoft Azure"
    description="Saznajte kako koristiti različite platforme naredbeni redak sučelja (EŽA) za implementaciju predložaka iz unutar na ClientVM ili nakon korištenja VPN-om za povezivanje s Azure stogu."
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
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Implementacija predložaka u stogu Azure pomoću naredbenog retka

Korištenje naredbenog retka za implementaciju Voditelj resursa Azure predložaka u stogu PNA Azure. Azure resursima predlošci implementaciju, i dodjela resursa za aplikaciju u operaciji jedinstvenog i usklađenih.

## <a name="download-template"></a>Preuzimanje predloška        
Da biste testirali implementacija s na EŽA, preuzmite azuredeploy.json datoteke i azuredeploy.parameters.json iz [stvorite predložak za primjer računa za pohranu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Uvođenje predloška
Dođite do mape u kojima su te datoteke preuzeti i pokrenite sljedeću naredbu za implementaciju predložak:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Ta se naredba uvodi predložak za grupu resursa **cliRG** Azure PNA snop zadano mjesto.

## <a name="validate-template-deployment"></a>Provjerite valjanost uvođenje predloška
Da biste vidjeli ovaj resurs grupe i pohranu račun, koristite sljedeće naredbe:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Daljnji koraci

[Upravljanje korisničkim dozvolama](azure-stack-manage-permissions.md)
