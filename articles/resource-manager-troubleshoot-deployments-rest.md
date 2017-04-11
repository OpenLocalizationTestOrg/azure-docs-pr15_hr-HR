<properties
   pageTitle="Prikaz postupci za implementaciju s REST API-JA | Microsoft Azure"
   description="U članku se opisuje kako koristiti Azure resursima REST API-JA za otkrivanje poteškoća s resursima implementacije."
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
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Prikaz postupci implementacije s Azure resursima REST API-JA

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure EŽA](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-JA](resource-manager-troubleshoot-deployments-rest.md)

Ako ste primili pogreške prilikom implementacije resursi za Azure, preporučujemo vam da biste vidjeli dodatne detalje o postupci implementacije koja su izvršava. REST API-JA nudi operacije koje omogućuju pronalaženje pogrešaka i određivanje potencijalnih rješenja.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Neke pogreške možete izbjeći tako da provjeri predloška i infrastrukture prije implementacije. Možete prijaviti i zahtjev za dodatnim i informacije o odzivu tijekom implementacije koji mogu biti korisne kasnije za otklanjanje poteškoća. Da biste saznali više o provjeri valjanosti i zapisivanje podataka zahtjeva i odgovora, u odjeljku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Rješavanje problema s REST API-JA

1. Implementacija resursa s operacija [stvaranja predloška implementacije](https://msdn.microsoft.com/library/azure/dn790564.aspx) . Želite li zadržati informacije koje se mogu biti korisne za ispravljanje pogrešaka, postavite svojstvo **debugSetting** u zahtjevu za JSON **requestContent** i/ili **responseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",      
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Prema zadanim postavkama, vrijednost **debugSetting** postavljen na **ništa**. Prilikom određivanja vrijednost **debugSetting** razmisliti o vrstu podataka u su prosljeđivanje tijekom implementacije. Zapisivanje podaci o zahtjeva i odgovora, nije moguće potencijalno izlažu osjetljive podatke dohvaćene putem postupci implementacije. 

2. Zatražite informacije o implementaciji operacijom [informacije o implementaciji za predložak](https://msdn.microsoft.com/library/azure/dn790565.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    U odgovoru, imajte na umu posebno elementi **provisioningState** , **correlationId** i **pogreške** . **CorrelationId** služi za praćenje povezane događaja, a može biti korisna prilikom rada s tehničku podršku da biste riješili problem.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Zatražite informacije o postupci implementacije operacijom [sve operacije uvođenje predloška popisa](https://msdn.microsoft.com/library/azure/dn790518.aspx) . 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Odgovor neće sadržavati stavku zahtjev i/ili odgovor informacije koje se temelje na što ste naveli u svojstvu **debugSetting** tijekom implementacije.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Pronađite događaje iz zapisnika nadzora za implementaciju operacijom [popisa upravljanje događaja u pretplatu](https://msdn.microsoft.com/library/azure/dn931934.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Daljnji koraci

- Pomoć za ispravljanje pogrešaka određeni implementaciju, potražite u članku [Riješite uobičajene pogreške prilikom implementacije resurse za Azure s Azure Voditelj resursa](resource-manager-common-deployment-errors.md).
- Dodatne informacije o korištenju zapisnika nadzora praćenje drugih vrsta akcije, potražite u članku [nadzora operacije s Voditelj resursa](resource-group-audit.md).
- Da biste provjerili valjanost uvođenja prije no što je izvršavanja, potražite u članku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md).
