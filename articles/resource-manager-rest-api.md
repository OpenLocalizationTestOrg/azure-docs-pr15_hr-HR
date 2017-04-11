<properties
   pageTitle="Voditelj resursa REST API-ji | Microsoft Azure"
   description="Pregled resursima REST API-ji za provjeru autentičnosti i korištenje Primjeri"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Voditelj resursa REST API-ji

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure EŽA](xplat-cli-azure-resource-manager.md)
- [Portal](./azure-portal/resource-group-portal.md) 
- [REST API-JA](resource-manager-rest-api.md)

Iza svakog poziva za Upravitelj Azure resursa, iza svaki distribuiranih predložak iza svaki račun konfiguriran za pohranu postoji jedan ili više pozive RESTful API Voditelj resursa za Azure. U ovoj se temi devoted je tih API-ji i kako ih možete nazvati bez korištenja sve SDK uopće. To može biti vrlo koristan ako želite potpunu kontrolu nad sve zahtjeve za Azure ili SDK za željeni jezik nije dostupan ili ne podržava operacije koju želite izvesti.

U ovom se članku će proći kroz svaki API predstavljeni u Azure, ali radije koristite neki kao primjer kako se nazivaju i povezati ih. Ako znate osnove možete nastaviti i pročitajte [Azure resursima REST API Reference](https://msdn.microsoft.com/library/azure/dn790568.aspx) da biste pronašli detaljne informacije o korištenju rest API-ji.

## <a name="authentication"></a>Provjera autentičnosti
Provjera autentičnosti za OBLAK obrađuje po Azure Active Directory (AD). Da bi se povezati s API-JA najprije morate uspješnoj Azure AD prima token za provjeru autentičnosti koje možete proslijediti svaki zahtjev. Kao što smo su s opisom čisto poziva izravno REST API-ji, ne možemo će i da ne želite autentičnost lozinkom normalni korisničko ime gdje je pop-up zaslona možda tražiti korisničko ime i lozinku i možda čak i druge Mehanizmi za provjeru autentičnosti u dva scenarija faktor provjere autentičnosti. Stoga ćemo će stvoriti pod nazivom Azure AD aplikacije i glavni servisa koji će se koristiti za prijavu s. No imajte na umu da Azure AD podržava nekoliko postupaka za provjeru autentičnosti i sve ih nije moguće koristiti za dohvaćanje te provjeru autentičnosti token koji potrebna za daljnji zahtjevi API-JA.
Slijedite [Stvaranje Azure AD aplikacija i servisa načelo](./resource-group-create-service-principal-portal.md) upute korak po korak.

### <a name="generating-an-access-token"></a>Generiranje Token za pristup 
Provjera autentičnosti na temelju Azure AD obavlja tvrtka pozivanje na Azure AD nalazi se na login.microsoftonline.com. Da bi se provjere autentičnosti koje moraju imati sljedeće informacije:

* Azure AD klijentu ID (naziv tog Azure AD koristite prijava, često isti kao tvrtke, ali nije potrebno)
* ID aplikacije (cjelokupnu korak stvaranja aplikacije za Azure AD)
* Lozinku (koju ste odabrali prilikom stvaranja aplikacije za Azure AD)

U u nastavku HTTP zahtjev provjerite "Azure AD klijentu ID", "ID aplikacije" i "Lozinku" zamijenite odgovarajuće vrijednosti.

**Generički HTTP zahtjev:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... će (Ako se potvrdi provjere autentičnosti) rezultiraju slično kao odgovor na ovo:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Access_token iznad odgovor su skraćena da biste povećali čitljivosti)

**Generiranje token za pristup pomoću tulumu:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generiranje token za pristup pomoću komponente PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Odgovor sadrži programa Access tokena, informacije o koliko te token vrijedi i informacije o koje resursa možete koristiti taj tokena za.
Pristupni token koji ste primili prijašnji HTTP mora se proslijediti u svih zahtjeva za ARM API kao zaglavlje pod nazivom "Autorizacije" s vrijednošću "Nošenja YOUR_ACCESS_TOKEN". Obratite pozornost na to razmaka između "Nošenja" i tokena vaše programa Access.

Kao što možete vidjeti iz iznad rezultata HTTP, token vrijedi za određeno razdoblje tijekom kojeg treba predmemorije i iskoristiti tu istu token. Čak i ako je moguće za provjeru autentičnosti Azure AD za svaki API poziv, bio bi iznimno NEUČINKOVIT.

## <a name="calling-arm-rest-apis"></a>Pozivanje ARM REST API-ji

[Azure resursima REST API -ji su ovdje navedenih](https://msdn.microsoft.com/library/azure/dn790568.aspx) i on je izvan opseg za ovog praktičnog vodiča za korištenje svakog i svaki. Ove dokumentacije će koristiti samo nekoliko API-ji objašnjavaju osnovni korištenja API-ji i nakon toga ne možemo pogledajte koje službeni dokumentaciju.

### <a name="list-all-subscriptions"></a>Popis svih pretplata

Jedna od najjednostavniji operacije možete učiniti jest da biste dobili popis dostupnih pretplate na kojima možete pristupiti. U na ispod zahtjev vidjet ćete kako Access tokena se prenosi u kao zaglavlja.

(Zamijenite YOUR_ACCESS_TOKEN na stvarni pristup tokena.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... i zbog toga prikazat će se popis pretplata između kojih je ovaj servis glavnicu dopušten pristup

(ID-ove pretplate ispod su skraćena zbog čitljivosti)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Popis svih grupa resursa u određenu pretplatu

Dostupno u sklopu ARM API-ji svi resursi su ugnježđivati unutar grupu resursa. Ne možemo ćete upit ARM za postojeće grupe resursa u našem pretplate pomoću u nastavku zahtjev HTTP GET. Obratite pozornost na to kako ID pretplate se prenosi u sklopu URL ovaj put.

(Zamjena YOUR_ACCESS_TOKEN i SUBSCRIPTION_ID s stvarni tokena za pristup i ID pretplate)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Odgovor se ovise imaju li sve grupe resursa definirani i ako je tako, koliko.

(ID-ove pretplate ispod su skraćena zbog čitljivosti)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Stvaranje grupa resursa

Dosad smo ste samo je ispitivanje API-ji ARM informacije, to je vrijeme ćemo stvoriti nekoliko resursa umjesto i Započnimo najjednostavniji ih svi, grupu resursa. Sljedeće HTTP zahtjev stvara novu grupu resursa u regiji/mjesto po izboru i dodaje jednu ili više oznaka (dodaje uzorak ispod zapravo samo jednu oznaku).

(Zamjena YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME s stvarni pristup tokena, ID pretplate i naziv koji želite stvoriti grupu resursa)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Ako ne uspije, prikazat će se slično kao odgovor na ovo

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Uspješno ste stvorili grupu resursa u Azure. Čestitamo!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Implementacija resursa u grupu resursa pomoću predloška ARM

Pomoću OKVIRA, možete implementirati resursa pomoću OKVIRA predložaka. Predložak programa ARM definira nekoliko resursa i njihove ovisnosti. Za ovaj odjeljak smo će samo da ste upoznati s predlošcima OKVIRA i samo Pokazat ćemo vam kako upućivanje poziva API da biste pokrenuli implementacije jednog. Detaljne dokumentaciju predložaka ARM nalazi se ovdje.

Uvođenje predloška ARM ne razlikuju se puno da biste kako pozvati druge API-ji. Jedan važne aspekte je uvođenje predloška može potrajati vrlo dugo, ovisno o tome što je unutar predložak, i poziv API samo će vratiti te ga je vaša kao upit za status implementacije za razvojne inženjere da biste saznali nakon dovršetka implementacijskih.

U ovom primjeru ćemo koristiti javno prikazanog OKVIRA predloška dostupna na [GitHub](https://github.com/Azure/azure-quickstart-templates). Ne možemo namjeravate koristiti predložak će implementirati Linux VM s područjem Zapad SAD-a. Iako ovaj predložak će imati predlošku koji su dostupni u javnim spremište kao što su GitHub, možete odabrati i prenesite cijeli predložak kao dio zahtjev. Imajte na umu kao dio zahtjev koji će se koristiti u predlošku korištenih dajemo vrijednosti parametara.

(Zamjena SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD i DNS_NAME_FOR_PUBLIC_IP vrijednosti odgovarajuće da vaš zahtjev)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Vrlo dugo JSON odgovor za taj zahtjev ste je izostavljena za poboljšanje čitljivosti ove dokumentacije. Odgovor će sadržavati informacije o implementaciji s predloškom koji ste upravo stvorili.

