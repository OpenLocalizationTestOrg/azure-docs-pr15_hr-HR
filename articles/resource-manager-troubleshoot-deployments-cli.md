<properties
   pageTitle="Prikaz postupci implementacije s Azure EŽA | Microsoft Azure"
   description="U članku se opisuje kako koristiti EŽA Azure da biste otkrili probleme s resursima implementacije."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Prikaz postupci implementacije s EŽA Azure

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure EŽA](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-JA](resource-manager-troubleshoot-deployments-rest.md)

Ako ste primili pogreške prilikom implementacije resursi za Azure, preporučujemo vam da biste vidjeli dodatne detalje o postupci implementacije koja su izvršava. Azure EŽA sadrži naredbe koje omogućuju pronalaženje pogrešaka i određivanje potencijalnih rješenja.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Neke pogreške možete izbjeći tako da provjeri predloška i infrastrukture prije implementacije. Možete prijaviti i zahtjev za dodatnim i informacije o odzivu tijekom implementacije koji mogu biti korisne kasnije za otklanjanje poteškoća. Da biste saznali više o provjeri valjanosti i zapisivanje podataka zahtjeva i odgovora, u odjeljku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Korištenje zapisnika nadzora za otklanjanje poteškoća

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Da biste vidjeli pogreške za implementaciju, poduzmite sljedeće korake:

1. Da biste pogledali zapisnike nadzora, pokrenite naredbu **Prikaži zapisnik azure grupe** . Možete uključiti u **– posljednje implementaciju** mogućnost za dohvaćanje zapisnika najnovije implementacije.

        azure group log show ExampleGroup --last-deployment

2. Naredba **Prikaži zapisnik azure grupe** vraća velike količine podataka. Za otklanjanje poteškoća, obično želite usredotočite se na operacije koje nije uspjelo. Sljedeću skriptu koristi mogućnost **– json** i JSON uslužni [jq](https://stedolan.github.io/jq/) za pretraživanje zapisnika za implementaciju pogreške.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Vidjet ćete **Svojstva** sadrži informacije u json o postupka nije uspjelo.

    Možete koristiti u **– opširno** i **- vv** mogućnosti da biste vidjeli dodatne informacije iz zapisnike.  Korištenje na **– opširno** mogućnost da biste prikazali korake operacije proći kroz na `stdout`. Za dovršavanje zahtjeva povijest, upotrijebite mogućnost **- vv** . Poruke često nude ključni promatrajući uzrok sve pogreške.

3. Da biste fokus na poruka o stanju za stavke nije uspio, koristite sljedeću naredbu:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Rješavanje problema pomoću postupci implementacije

1. Dohvatite cjelokupan status implementacije pomoću naredbe **Prikaži implementacije azure grupe** . U primjeru u nastavku implementacijskih nije uspjelo.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Da biste vidjeli poruku za nije uspjelo postupci za implementaciju, koristite:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Daljnji koraci

- Pomoć za ispravljanje pogrešaka određeni implementaciju, potražite u članku [Riješite uobičajene pogreške prilikom implementacije resurse za Azure s Azure Voditelj resursa](resource-manager-common-deployment-errors.md).
- Dodatne informacije o korištenju zapisnika nadzora praćenje drugih vrsta akcije, potražite u članku [nadzora operacije s Voditelj resursa](resource-group-audit.md).
- Da biste provjerili valjanost uvođenja prije izvršavanja ga, potražite u članku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md).
