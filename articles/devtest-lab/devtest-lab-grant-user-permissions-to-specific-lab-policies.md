<properties
    pageTitle="Dodjeljivanje korisničkih dozvola na određene Laboratorija pravila | Microsoft Azure"
    description="Upute za dodjeljivanje korisničkih dozvola na određene Laboratorija pravila u DevTest Labs na temelju potreba za svakog korisnika"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Dodjeljivanje korisničkih dozvola na određene Laboratorija pravila

## <a name="overview"></a>Pregled

U ovom se članku prikazuje kako pomoću ljuske PowerShell da biste dodijelili dozvole za određenu Laboratorija pravila. Na taj način dozvole se mogu primijeniti ovisno o potrebama za svakog korisnika. Na primjer, možda ćete morati dodijeliti određeni korisnički mogućnost da biste promijenili postavke pravilnika VM, ali ne trošak pravila.

## <a name="policies-as-resources"></a>Pravila kao resurse

Kako je opisano u članku [Utemeljeno na ulogama Azure kontrola pristupa](../active-directory/role-based-access-control-configure.md) , RBAC omogućuje upravljanje preciznije pristup resursa za Azure. Korištenje RBAC, možete segregate obavezama koje imate u tim DevOps i dopustiti samo razinu pristupa korisnicima koji su potrebni za izvođenje svoje zadatke.

U DevTest Labs vrsta resursa koji omogućuje akciju RBAC **Microsoft.DevTestLab/labs/policySets/policies/**je pravilo. Svaki Laboratorija pravila je resurs u vrsti resursa pravila, a mogu dodijeliti kao opseg za ulogu RBAC.

Na primjer, da biste dodijelili korisnici čitanje/pisanje dozvolu za pravila **Dopuštene veličine VM** biste stvorili prilagođeni ulozi koja funkcionira s **Microsoft.DevTestLab/labs/policySets/policies/** * akciju, a zatim dodijelite odgovarajućim korisnicima ta prilagođene uloga u opsegu * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Da biste saznali više o prilagođene uloge u RBAC, potražite u članku [Kontrola pristupa uloge Prilagođeno](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>Stvaranje prilagođene uloge Laboratorija pomoću komponente PowerShell
Da bi se koraci, morat ćete pročitajte sljedeći članak koji će se objašnjava kako instalirati i konfigurirati Azure PowerShell cmdleti: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Kada postavite Azure PowerShell cmdleti, možete izvršiti sljedeće zadatke:

- Popis sve operacije/akcije za davatelja resursa
- Popis akcija u odgovarajućom ulogom:
- Stvaranje prilagođene uloga

Sljedeću skriptu komponente PowerShell ilustrira Primjeri kako izvršiti sljedeće zadatke:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Dodjela dozvola korisniku za određene pravila pomoću prilagođene uloge
Nakon što ste definirali prilagođene uloge, možete ih dodijeliti korisnicima. Da biste korisniku dodijelite prilagođene uloge, najprije morate dobiti **ID objekta** koji predstavlja taj korisnik. Da biste to učinili, koristite cmdlet **Get-AzureRmADUser** .

U sljedećem primjeru **ID objekta** korisnika *SomeUser* je 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Nakon što dodate **ID objekta** za korisnika i naziv prilagođene uloge, možete dodijeliti uloge korisnika pomoću cmdleta **New-AzureRmRoleAssignment** :

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

U prethodnom primjeru koristi pravila **AllowedVmSizesInLab** . Možete koristiti bilo koju od sljedećih pravila:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Daljnji koraci

Jednom dao ovlasti Korisnički određene Laboratorija pravila, Evo nekih daljnje mogućnosti:

- [Siguran pristup sustava na Laboratorija](devtest-lab-add-devtest-user.md).

- [Postavljanje pravila Laboratorija](devtest-lab-set-lab-policy.md).

- [Stvaranje predloška Laboratorija](devtest-lab-create-template.md).

- [Stvaranje prilagođene artefakte za vaše VMs](devtest-lab-artifact-author.md).

- [Dodavanje VM s artefakte da biste na Laboratorija](devtest-lab-add-vm-with-artifacts.md).
