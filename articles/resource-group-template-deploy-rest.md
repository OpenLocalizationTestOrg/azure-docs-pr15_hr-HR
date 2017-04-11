<properties
   pageTitle="Implementacija resursi REST API-JA i predložak | Microsoft Azure"
   description="Pomoću upravitelja resursa Azure i resursima REST API-JA za implementaciju resurs za Azure. Resursi su definirani u predlošku Voditelj resursa."
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
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Implementacija resursa s resursima predloške i resursima REST API-JA

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure EŽA](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST API-JA](resource-group-template-deploy-rest.md)

U ovom se članku objašnjava kako pomoću upravitelja resursa REST API-JA s predlošcima resursima za implementaciju resurse za Azure.  

> [AZURE.TIP] Pomoć za ispravljanje pogrešaka pogreška tijekom uvođenja, pogledajte:
>
> - Otklanjanje poteškoća s [postupci implementacije prikaz s REST API -JA](resource-manager-troubleshoot-deployments-rest.md) dodatne informacije o tome kako izvući podatke koji olakšava pogrešku
> - [Otklanjanje uobičajenih pogrešaka prilikom implementacije resurse za Azure s Azure Voditelj resursa](resource-manager-common-deployment-errors.md) da biste saznali kako riješiti uobičajenih pogrešaka implementacije

Predložak može biti lokalne datoteke ili vanjske datoteke koji je dostupan putem URI. Kada se predložak nalazi se u račun za pohranu, možete ograničiti pristup predloška i dati token za potpis (SAS) zajednički pristup tijekom implementacije.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Implementacija s REST API-JA
1. Postavljanje [uobičajenih parametara i zaglavlja](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), uključujući tokeni za provjeru autentičnosti.
2. Ako nemate postojeću grupu resursa, stvorite grupu resursa. Unesite svoj id za pretplatu, naziv nove grupe resursa i mjesto na koje su vam potrebne za rješenje. Dodatne informacije potražite u članku [Stvaranje grupu resursa](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Provjerite valjanost uvođenja prije izvršavanja tako da pokrenete postupak [Provjera uvođenja predloška](https://msdn.microsoft.com/library/azure/dn790547.aspx) . Kada testiranje implementaciju, navesti parametra točno onako kako želite da se prilikom izvršavanja implementacije (prikazano u sljedećem koraku).

3. Stvaranje implementacije. Navedite id pretplate, naziv grupe resursa za implementaciju, naziv uvođenje i veze na vašem predlošku. Informacije o datoteku predloška potražite u članku [parametar datoteke](#parameter-file). Dodatne informacije o REST API-JA da biste stvorili grupu resursa potražite u članku [Stvaranje predloška implementacije](https://msdn.microsoft.com/library/azure/dn790564.aspx). Obavijest o **načinu rada** postavljen na **rastuća**. Da biste pokrenuli dovršeno implementaciju, postavite **način rada** u **Dovršeno**. Budite oprezni prilikom korištenja dovršeno načina kako ste slučajno izbrisali resursa koji se ne nalaze u vašem predlošku.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
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
              }
            }
          }
   
      Ako želite da biste se prijavili sadržaj odgovor, zahtjev i sadržaja, obuhvatiti **debugSetting** zahtjev.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Da biste koristili token za potpis (SAS) zajednički pristup možete postaviti vaš račun za pohranu. Dodatne informacije potražite u članku [Prenošenjem pristup potpisom zajedničko korištenje programa Access](https://msdn.microsoft.com/library/ee395415.aspx).

4. Pronađite status uvođenje predloška. Dodatne informacije potražite u članku [informacije o implementaciji za predložak](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Daljnji koraci
- Primjer implementacija resursi putem klijenta biblioteke .NET, potražite u članku [uvođenja resursima pomoću .NET biblioteke i predložak](virtual-machines/virtual-machines-windows-csharp-template.md).
- Definiranje parametara u predložak, potražite u odjeljku [Predlošci na dokumentima](resource-group-authoring-templates.md#parameters).
- Upute za implementaciju rješenje u različitim okruženjima, potražite u članku [razvoj i testiranje okruženja u Microsoft Azure](solution-dev-test-environments.md).
- Pojedinosti o korištenju KeyVault referenca za prosljeđivanje sigurne vrijednosti, potražite u članku [proći sigurne vrijednosti tijekom implementacije](resource-manager-keyvault-parameter.md).
