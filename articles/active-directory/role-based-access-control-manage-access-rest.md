<properties
    pageTitle="Upravljanje pristupom utemeljeno na ulogama kontrole s REST API-JA"
    description="Upravljanje pristupom utemeljeno na ulogama kontrole s REST API-JA"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Upravljanje pristupom utemeljeno na ulogama kontrole s REST API-JA

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure EŽA](role-based-access-control-manage-access-azure-cli.md)
- [REST API-JA](role-based-access-control-manage-access-rest.md)

Na temelju uloga pristup kontrola (RBAC) u Azure Portal i Azure resursima API pomaže vam upravljanje pristupom za svoju pretplatu i resursima preciznije razine. Pomoću te značajke možete dodijeliti pristupa za korisnike, grupe ili upravitelji servisa Active Directory tako da im dodijelite neke uloge pri dani doseg.

## <a name="list-all-role-assignments"></a>Popis svih dodjele uloga

Navodi sve dodjele uloga na navedeni opseg i subscopes.

Dodjele uloga popis, morate imati pristup `Microsoft.Authorization/roleAssignments/read` postupak opseg. Ugrađeni uloge su vam je pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Metodom **DOHVATI** koristite sa sljedećim URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijenite opseg za koji želite da popis dodjele uloga. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine:

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1  
  - Resursa: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Api-verzija}* zamijenite 2015-07-01.

3. *{Filtar}* zamijenite uvjet koji želite primijeniti filtriranje popisa dodjele uloga:

  - Popis uloge dodijeljene samo navedeni opseg, ne uključujući dodjele uloga na subscopes:`atScope()`    
  - Dodjela uloga popisa za određenog korisnika, grupa ili aplikacije:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Popis dodjele uloga za određenog korisnika, uključujući one naslijeđeno i iz grupa |`assignedTo('{objectId of user}')`

### <a name="response"></a>Odgovor

Šifra stanja: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Informacije o Dodjela uloga

Dohvaća podatke o jednom uloga dodjele određen identifikator dodjele uloga.

Da biste dobili informacije o Dodjela uloge, morate imati pristup `Microsoft.Authorization/roleAssignments/read` operacija. Ugrađeni uloge su vam je pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Metodom **DOHVATI** koristite sa sljedećim URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijenite opseg za koji želite da popis dodjele uloga. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine:

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1  
  - Resursa: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Uloga dodjele-id}* zamijenite GUID identifikator Dodjela uloge.

3. *{Api-verzija}* zamijenite 2015-07-01.

### <a name="response"></a>Odgovor

Šifra stanja: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Stvaranje i Dodjela uloga

Stvaranje Dodjela uloga na navedeni opseg za navedeni Upravitelj dodjelu navedenu ulogu.

Da biste stvorili Dodjela uloge, morate imati pristup `Microsoft.Authorization/roleAssignments/write` operacija. Ugrađeni uloga samo *vlasnik* i *Korisnički pristup administratora* su omogućen pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Pomoću metode **STAVITI** sljedeće URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijenite opseg na kojoj želite stvoriti dodjele uloga. Kada stvorite Dodjela uloga na nadređeni opseg, svih podređenih opsega nasljeđuju iste dodjele uloga. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine:

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1   
  - Resursa: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Uloga dodjele-id}* zamijenite nove GUID koja postaje GUID identifikator Dodjela nove uloge.

3. *{Api-verzija}* zamijenite 2015-07-01.

Za tijelu zahtjeva, unesite vrijednosti u sljedećem obliku:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Naziv elementa     | Obavezno | Vrsta   | Opis |
|------------------|----------|--------|-------------|
| roleDefinitionId | Da      | Niz | Identifikator uloge. Oblik identifikator je:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Da      | Niz | ID objekta glavnice Azure AD (korisniku, grupi ili usluge) koji je dodijeljen ulogu. |

### <a name="response"></a>Odgovor

Šifra stanja: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Brisanje Dodjela uloga

Brisanje Dodjela uloga na navedeni opseg.

Da biste izbrisali Dodjela uloge, morate imati pristup na `Microsoft.Authorization/roleAssignments/delete` operacija. Ugrađeni uloga samo *vlasnik* i *Korisnički pristup administratora* su omogućen pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Način za **Brisanje** pomoću sljedećih URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijenite opseg na kojoj želite stvoriti dodjele uloga. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine:

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1  
  - Resursa: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Uloga dodjele-id}* zamijenite id dodjele uloga GUID.

3. *{Api-verzija}* zamijenite 2015-07-01.

### <a name="response"></a>Odgovor

Šifra stanja: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Popis svih uloga

Popis svih uloga koji su dostupni za dodjelu na navedeni opseg.

Na popisu uloge, morate imati pristup `Microsoft.Authorization/roleDefinitions/read` postupak opsega. Ugrađeni uloge su vam je pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Metodom **DOHVATI** koristite sa sljedećim URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijenite opseg za koji želite da popis uloge. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine:

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1  
  - /Subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 resursa  

2. *{Api-verzija}* zamijenite 2015-07-01.

3. *{Filtar}* zamijeniti uvjet koji želite primijeniti filtriranje popisa uloge:

  - Uloge popis dostupnih za dodjelu u navedenom dosegu i sve njezinih podređenih dosega:`atScopeAndBelow()`
  - Traženje uloga pomoću točno zaslonsko ime: `roleName%20eq%20'{role-display-name}'`. Koristi oblik kodirati URL točan zaslonsko ime uloge. Ako, primjerice,`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Odgovor

Šifra stanja: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Informacije o ulozi

Dohvaća podatke o jednom ulozi određen identifikator definicije uloga. Da biste dobili informacije o jednom ulozi pomoću njegov zaslonsko ime, pogledajte [popis svih uloga](role-based-access-control-manage-access-rest.md#list-all-roles).

Da biste dobili informacije o uloge, morate imati pristup `Microsoft.Authorization/roleDefinitions/read` operacija. Ugrađeni uloge su vam je pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Metodom **DOHVATI** koristite sa sljedećim URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijenite opseg za koji želite da popis dodjele uloga. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine:

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1  
  - Resursa: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Uloga definicija-id}* zamijenite GUID identifikator definicije uloge.

3. *{Api-verzija}* zamijenite 2015-07-01.

### <a name="response"></a>Odgovor

Šifra stanja: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Stvaranje prilagođene uloga
Stvorite prilagođene uloge.

Da biste stvorili prilagođeni uloge, morate imati pristup `Microsoft.Authorization/roleDefinitions/write` operacija na svim na `AssignableScopes`. Ugrađeni uloga samo *vlasnik* i *Korisnički pristup administratora* su omogućen pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Pomoću metode **STAVITI** sljedeće URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijeniti prvom *AssignableScope* prilagođene uloge. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine.

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1  
  - Resursa: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Uloga definicija-id}* zamijenite nove GUID koja postaje GUID identifikator nove prilagođene uloge.

3. *{Api-verzija}* zamijenite 2015-07-01.

Za tijelu zahtjeva, unesite vrijednosti u sljedećem obliku:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Naziv elementa | Obavezno | Vrsta | Opis |
|--------------|----------|------|-------------|
| ime         | Da | Niz   | GUID identifikator prilagođene uloge.    |
| properties.roleName               | Da | Niz   | Zaslonski naziv prilagođene uloge. Maksimalna veličina 128 znakova.                        |
| Properties.Description            | ne  | Niz   | Opis prilagođene uloge. Maksimalna veličina 1024 znakove.                                               |
| Properties.Type                   | Da | Niz   | Postavljeno na "CustomRole."                                         |
| Properties.Permissions.Actions    | Da | Niz] | Polja nizova akcija navodeći operacije dodijelio prilagođene uloge.             |
| properties.permissions.notActions | ne  | Niz] | Polja nizova akcija navodeći operacije da biste izuzeli iz operacije dodijelio prilagođene uloge. |
| properties.assignableScopes       | Da | Niz] | Polje opsega u kojem se može koristiti prilagođene uloge.   |

### <a name="response"></a>Odgovor

Šifra stanja: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Ažurirajte prilagođene uloga

Izmjena prilagođenih uloge.

Da biste izmijenili prilagođene uloge, morate imati pristup `Microsoft.Authorization/roleDefinitions/write` operacija na svim na `AssignableScopes`. Ugrađeni uloga samo *vlasnik* i *Korisnički pristup administratora* su omogućen pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Pomoću metode **STAVITI** sljedeće URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijeniti prvom *AssignableScope* prilagođene uloge. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine:

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1  
  - Resursa: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Uloga definicija-id}* zamijenite GUID identifikator prilagođene uloge.

3. *{Api-verzija}* zamijenite 2015-07-01.

Za tijelu zahtjeva, unesite vrijednosti u sljedećem obliku:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Naziv elementa | Obavezno | Vrsta | Opis |
|--------------|----------|------|-------------|
| ime         | Da      | Niz | GUID identifikator prilagođene uloge. |
| properties.roleName | Da | Niz | Zaslonsko ime ažurirane prilagođene uloge. |
| Properties.Description | ne | Niz | Opis ažurirane prilagođene uloge. |
| Properties.Type | Da | Niz | Postavljeno na "CustomRole." |
| Properties.Permissions.Actions | Da | Niz] | Polja nizova akcija navodeći operacije kojoj ažurirane prilagođene uloga dopušta pristup. |
| properties.permissions.notActions | ne | Niz] | Polja nizova akcija navodeći operacije da biste izuzeli iz operacije koje ažurirane prilagođene uloga daje. |
| properties.assignableScopes | Da | Niz] | Polje opsega u kojem se može koristiti ažurirani prilagođene uloge. |

### <a name="response"></a>Odgovor

Šifra stanja: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Brisanje prilagođene uloge

Brisanje prilagođene uloge.

Da biste izbrisali prilagođenu uloge, morate imati pristup `Microsoft.Authorization/roleDefinitions/delete` operacija na svim na `AssignableScopes`. Ugrađeni uloga samo *vlasnik* i *Korisnički pristup administratora* su omogućen pristup ovaj postupak. Dodatne informacije o dodjele uloga i upravljanje pristupom za Azure resursima potražite u članku [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md).

### <a name="request"></a>Zahtjev

Način za **Brisanje** pomoću sljedećih URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Unutar URI, provjerite sljedeće zamjene da biste prilagodili vaš zahtjev:

1. *{Opseg}* zamijenite opseg na kojoj želite izbrisati definicije uloge. Sljedeći primjeri pokazuju kako odrediti opseg za različite razine:

  - Pretplate: /subscriptions/ {id pretplate}  
  - Grupa resursa: /subscriptions/ {id pretplate} / resourceGroups/myresourcegroup1  
  - Resursa: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Uloga definicija-id}* zamijenite id definicije uloga GUID prilagođene uloge.

3. *{Api-verzija}* zamijenite 2015-07-01.

### <a name="response"></a>Odgovor

Šifra stanja: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
