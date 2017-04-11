<properties
    pageTitle="Upravljanje kontrola pristupa na temelju uloga (RBAC) s Azure EŽA | Microsoft Azure"
    description="Saznajte kako upravljati na temelju uloga kontrole pristupa (RBAC) s Azure sučelja naredbenog retka popisom uloge i uloge akcije i Dodjela uloga opsega pretplate i aplikacije."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Upravljanje kontrola pristupa na temelju uloga s Azure sučelja naredbenog retka

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure EŽA](role-based-access-control-manage-access-azure-cli.md)
- [REST API-JA](role-based-access-control-manage-access-rest.md)

Možete koristiti na temelju uloga kontrole pristupa (RBAC) na portalu za Azure i API Azure resursima za upravljanje pristupom za svoju pretplatu i resursima preciznije razine. Pomoću te značajke možete dodijeliti pristupa za korisnike, grupe ili upravitelji servisa Active Directory tako da im dodijelite neke uloge pri dani doseg.

Da biste upravljali RBAC biste mogli koristiti Azure sučelja naredbenog retka (EŽA), morate imati sljedeće:

- Azure EŽA verziju 0.8.8 ili noviji. Da biste instalirali najnoviju verziju i povezati s pretplatom Azure, potražite u članku [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md).
- Voditelj resursa u Azure EŽA Azure. Idite na [pomoću EŽA Azure s Voditelj resursa](../xplat-cli-azure-resource-manager.md) za više detalja.

## <a name="list-roles"></a>Popis uloge

### <a name="list-all-available-roles"></a>Popis svih dostupnih uloga
Da biste popis svih dostupnih uloga, koristite:

        azure role list

Sljedeći primjer prikazuje popis *svih dostupnih uloga*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Snimka zaslona naredbenog retka - azure uloga popis - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Popis akcija uloge
Da biste popis akcije uloge, koristite:

    azure role show "<role name>"

Sljedeći primjer prikazuje akcije uloge *suradnika* i *Suradnik virtualnog računala* .

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Snimka zaslona naredbenog retka - azure uloga Prikaži - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Popis programa access
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Učinkovita na grupu resursa dodjele uloga popisa
Da biste popis dodjele uloga koji postoje u grupu resursa, koristite:

    azure role assignment list --resource-group <resource group name>

Sljedeći primjer prikazuje dodjele uloga u grupi *pharma Prodaja projecforcast* .

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure naredbenog retka - azure uloga dodjele popisa prema grupi snimka](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Popis dodjele uloga za korisnika
Da biste popis dodjele uloga za određenog korisnika i dodjela koje su dodijeljene grupama korisnika, koristite:

    azure role assignment list --signInName <user email>

Možete vidjeti i dodjele uloga koje nasljeđuju iz grupa izmjenom naredbu:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Sljedeći primjer prikazuje dodjele uloga koje su dodijeljene u *sameert@aaddemo.com* korisnika. To obuhvaća uloge koje su dodijeljene izravno korisnika i uloge koje nasljeđuju iz grupe.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Snimka zaslona naredbenog retka - azure uloga popis obaveza korisnik - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Dopuštanje pristupa
Da biste dodijelili pristup nakon što ste identificirali ulogu koju želite dodijeliti, koristite:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Dodijelite uloge grupa u dosegu pretplate
Da biste dodijelili uloge grupi u dosegu pretplatu, koristite:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

U sljedećem primjeru dodjeljuje ulozi *čitač* *Christine Koch tima* u dosegu *pretplate* .


![RBAC Azure naredbenog retka - Dodjela uloge azure stvara grupu snimka](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Dodijelite uloge aplikacije u dosegu pretplate
Dodijelite uloge aplikacije u dosegu pretplatu, koristite:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

U sljedećem primjeru daje ulogu *suradnika* u aplikaciju *Azure AD* na odabrane pretplate.

 ![RBAC Azure naredbenog retka - Dodjela uloge azure stvaranje aplikacija](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Dodijelite uloge korisnika u grupu dosegu resursa
Dodijelite uloge korisnika u grupu dosegu resursa, koristite:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

U sljedećem primjeru daje ulogu *Suradnika virtualnog računala* da biste *samert@aaddemo.com* korisnika *Pharma Prodaja ProjectForcast* resursa grupi opseg.

![RBAC Azure naredbenog retka - Dodjela uloge azure stvara korisnika snimka](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Dodijelite uloge grupa u dosegu resursa
Dodjeljivanje uloge grupi u dosegu resursa koristite:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

U sljedećem primjeru daje ulogu *Suradnika virtualnog računala* u grupu *Azure AD* na *podmreži*.

![RBAC Azure naredbenog retka - Dodjela uloge azure stvara grupu snimka](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Uklanjanje pristupa
Da biste uklonili Dodjela uloge, koristite:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

U sljedećem primjeru uklanja Dodjela uloge *Suradnika virtualnog računala* s na *sammert@aaddemo.com* korisnika u grupu resursa *Pharma Prodaja ProjectForcast* .
U primjeru zatim uklanja Dodjela uloge iz grupa na pretplatu.

![Snimka zaslona naredbenog retka - azure uloga dodjele Izbriši - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Stvaranje prilagođene uloga
Da biste stvorili prilagođeni ulogu, koristite:

    azure role create --inputfile <file path>

Sljedeći primjer stvara ulogu prilagođenog naziva *Operator virtualnog računala*. Prilagođeni uloga dopušta pristup sve operacije čitanja *Microsoft.Compute*, *Microsoft.Storage*i *Microsoft.Network* resursa davatelja dopušta pristup da biste započeli, ponovno pokrenite i praćenje virtualnim računalima. Prilagođeni uloga može se koristiti u dva pretplate. U ovom se primjeru koristi JSON datoteku kao ulaz.

![Snimka zaslona JSON - definicije prilagođene uloge-](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure naredbenog retka - azure uloga stvaranje - snimka](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Izmjena prilagođenih uloga

Da biste izmijenili prilagođene uloge, prvi put koristite na `azure role show` naredbu za dohvaćanje definicije uloge. Drugo, unesite željene promjene u datoteku definicije uloga. Naposljetku, koristiti `azure role set` da biste spremili definicije izmijenjene uloge.

    azure role set --inputfile <file path>

Sljedeći primjer dodaje postupka *Microsoft.Insights/diagnosticSettings/* **Akcije**i Azure pretplatu na **AssignableScopes** prilagođene uloge Operator virtualnog računala.

![JSON - izmjena definicije prilagođene uloge - snimka](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Snimka zaslona naredbenog retka - azure uloga set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Brisanje prilagođene uloge

Da biste izbrisali prilagođenu uloge, prvi put koristite na `azure role show` naredbu da biste odredili **ID** uloge. Zatim, poduzmite na `azure role delete` naredbu da biste izbrisali ulogu navođenjem **ID-a**.

U sljedećem primjeru uklanja ulogu prilagođene *Operator virtualnog računala* .

![Snimka zaslona naredbenog retka - azure uloga Izbriši - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Popis prilagođene uloge

Da biste popis uloge koje su dostupne za dodjelu na opseg, koristite na `azure role list` naredbe.

Sljedeći primjer prikazuje sve uloge koje su dostupne za dodjelu u odabrane pretplate.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Snimka zaslona naredbenog retka - azure uloga popis - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

U sljedećem primjeru ulogu prilagođene *Virtualnog računala Operator* nije dostupna u pretplatu *Production4* jer tu pretplatu nije u **AssignableScopes** uloge.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Snimka zaslona naredbenog retka - azure uloga popis za prilagođene uloge - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC teme
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
