<properties
    pageTitle="Upravljanje kontrola pristupa na temelju uloga (RBAC) s Azure PowerShell | Microsoft Azure"
    description="Kako upravljati RBAC sa servisom PowerShell Azure, uključujući popis uloge, Dodjela uloga i brisanje dodjele uloga."
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

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Upravljanje kontrola pristupa na temelju uloga s Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure EŽA](role-based-access-control-manage-access-azure-cli.md)
- [REST API-JA](role-based-access-control-manage-access-rest.md)


Na temelju uloga kontrole pristupa (RBAC) na portalu za Azure i Azure resursa upravljanje API-JA možete koristiti za upravljanje pristupom u pretplatu za preciznije razine. Pomoću te značajke možete dodijeliti pristupa za korisnike, grupe ili upravitelji servisa Active Directory tako da im dodijelite neke uloge pri dani doseg.

Prije korištenja ljuske PowerShell za upravljanje RBAC, morate imati sljedeće:

- Azure PowerShell verzije 0.8.8 ili noviji. Da biste instalirali najnoviju verziju i pridružiti Azure pretplatu, pročitajte članak [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

- Azure cmdleta Voditelj resursa. Instalirajte [cmdleta Azure upravljanja resursima](https://msdn.microsoft.com/library/mt125356.aspx) u ljusci PowerShell.

## <a name="list-roles"></a>Popis uloge

### <a name="list-all-available-roles"></a>Popis svih dostupnih uloga
Popis RBAC ulogama koji su dostupni za dodjelu te provjerite operacije na koje mogu odobriti pristup, koristite `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Snimka zaslona PowerShell – Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Popis akcija uloge
Da biste popis akcija za određenu ulogu, koristite `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Snimka zaslona PowerShell – Get-AzureRmRoleDefinition za određenu ulogu - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Informacije o tome tko ima pristup
Da biste popis RBAC pristup dodjele, koristite `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Popis dodjele uloga u određenim dosegu
Vidjet ćete sve dodjele pristup za određenu pretplatu, grupa resursa ili resurs. Na primjer, da biste vidjeli na sve u aktivni dodjele za grupu resursa, koristiti `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![Snimka zaslona PowerShell – Get-AzureRmRoleAssignment za grupu resursa – RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Uloge popisa Dodijeljeno korisniku
Da biste popis svih uloga koje su dodijeljene navedeni korisnik i uloge koje su dodijeljene grupe kojoj pripada korisnika, poslužite se `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![Snimka zaslona PowerShell – Get-AzureRmRoleAssignment za korisnika – RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Popis klasični servisa administrator i coadmin dodjele uloga
Popis dodjele pristup za administratora klasični pretplate i coadministrators, koristite:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Dopuštanje pristupa
### <a name="search-for-object-ids"></a>Traženje objekata ID-a
Da biste dodijelite uloge, morate je prepoznavanje objekta (korisniku, grupi ili aplikacija) i opseg.

Ako ne znate ID pretplate, možete je pronaći u plohu **pretplate** na portalu Azure. Da biste saznali kako postaviti upit za ID pretplate, potražite u članku [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) na MSDN-u.

Da biste objekt ID-a za Azure AD grupi, koristite:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Da biste dobili ID objekta za Upravitelj servisa Azure AD ili aplikacije, koristite:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Dodijelite uloge aplikacije u dosegu pretplate
Da biste dodijelili pristup aplikacije u dosegu pretplatu, koristite:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Snimka zaslona PowerShell – novi AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Dodijelite uloge korisnika u grupu dosegu resursa
Da biste dodijelili pristup za korisnika u grupu dosegu resursa, koristite:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Snimka zaslona PowerShell – novi AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Dodijelite uloge grupa u dosegu resursa
Da biste dodijelili pristup grupi u dosegu resursa, koristite:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Snimka zaslona PowerShell – novi AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Uklanjanje pristupa
Da biste uklonili pristupa za korisnike, grupe i aplikacije, koristite:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Snimka zaslona PowerShell – uklanjanje AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Stvaranje prilagođene uloga
Da biste stvorili prilagođenu ulogu, koristite na `New-AzureRmRoleDefinition` naredbe.

Kada stvorite prilagođeni uloga pomoću komponente PowerShell, morate početi s jednom od [ugrađenih uloge](role-based-access-built-in-roles.md). Uređivanje atributa dodajte *Akcije*, *notActions*i *opsega* koju želite, a pa spremite promjene kao novo radno mjesto.

U sljedećem primjeru započinje ulogu *Suradnika virtualnog računala* i koji se koristi za stvaranje prilagođene uloge naziva *Operator virtualnog računala*. Novo radno mjesto dopušta pristup sve operacije čitanja *Microsoft.Compute*, *Microsoft.Storage*i *Microsoft.Network* davatelja resursa daje pristup da biste započeli, ponovno pokrenite i praćenje virtualnim računalima. Prilagođeni uloga može se koristiti u dva pretplate.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![Snimka zaslona PowerShell – Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Izmjena prilagođenih uloga
Da biste izmijenili prilagođene uloge, prvo, poslužite se na `Get-AzureRmRoleDefinition` naredbu za dohvaćanje definicije uloga. Da biste definicije uloge, unesite željene promjene. Naposljetku, koristiti u `Set-AzureRmRoleDefinition` naredbu Spremi postavljeni izmijenjene uloge.

Sljedeći primjer dodaje u `Microsoft.Insights/diagnosticSettings/*` operacija ulogu prilagođene *Operator virtualnog računala* .

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Snimka zaslona PowerShell – postavljanje AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Sljedeći primjer dodaje Azure pretplatu koju valja dodijeliti opsega prilagođene uloge *Operator virtualnog računala* .

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![Snimka zaslona PowerShell – postavljanje AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Brisanje prilagođene uloge

Da biste izbrisali prilagođenu uloge, koristite na `Remove-AzureRmRoleDefinition` naredbe.

U sljedećem primjeru uklanja ulogu prilagođene *Operator virtualnog računala* .

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Snimka zaslona PowerShell – uklanjanje AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Popis prilagođene uloge
Da biste popis uloge koje su dostupne za dodjelu na opseg, koristite na `Get-AzureRmRoleDefinition` naredbe.

Sljedeći primjer prikazuje sve uloge koje su dostupne za dodjelu u odabrane pretplate.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Snimka zaslona PowerShell – Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

U sljedećem primjeru ulogu prilagođene *Virtualnog računala Operator* nije dostupna u pretplatu *Production4* jer tu pretplatu nije u **AssignableScopes** uloge.

![Snimka zaslona PowerShell – Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Vidi također
- [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
