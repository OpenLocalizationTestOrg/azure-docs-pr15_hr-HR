<properties
    pageTitle="Stvaranje izvješća povijest promjena programa access | Microsoft Azure"
    description="Generiranje izvješća koja sadrži popis svih promjena u programu access da biste pretplate Azure s kontrola pristupa na temelju uloga zadnjih 90 dana."
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
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Stvaranje izvješća povijest promjena programa access

Bilo kada netko daje ili opoziva pristup unutar pretplate, promjene se prijaviti Azure događaja. Možete stvarati izvješća za povijest promjena programa access da biste vidjeli sve promjene za zadnjih 90 dana.

## <a name="create-a-report-with-azure-powershell"></a>Stvaranje izvješća pomoću komponente PowerShell Azure
Da biste stvorili izvješće povijest promjena programa access u ljusci PowerShell, koristite na `Get-AzureRMAuthorizationChangeLog` naredbe. Dodatne pojedinosti o ovaj cmdlet dostupne su u [Galeriji PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Kada poziv tu naredbu, možete navesti svojstva kojem dodjele koji želite unijeti, uključujući sljedeće:

| Svojstvo | Opis |
| -------- | ----------- |
| **Akcija** | Hoće li je pristup odobren ili povučen |
| **Pozivatelja** | Vlasnik odgovoran za promjenu programa access |
| **Datum** | Datum i vrijeme koje je promijenjeno programa access |
| **Direktorija** | Azure Active Directory direktorija |
| **PrincipalName** | Naziv korisnika, grupe ili aplikacije |
| **PrincipalType** | Hoće li je dodjelu za korisnika, grupa ili aplikacije |
| **RoleId** | GUID ulozi koja je dodijeljena ili povučen |
| **Naziv uloge** | Uloga koja je dodijeljena ili povučen |
| **ScopeName** | Naziv pretplate, grupa resursa ili resursa |
| **ScopeType** | Hoće li je dodjelu na pretplatu, grupa resursa ili opseg resursa |
| **SubscriptionId** | GUID Azure pretplate |
| **SubscriptionName** | Naziv Azure pretplate |

U ovom primjeru command dohvaća sve promjene programa access u pretplatu za zadnjih sedam dana:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog - snimka](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Stvaranje izvješća pomoću EŽA Azure
Da biste stvorili izvješće povijest promjena programa access u Azure sučelja naredbenog retka (EŽA), koristite na `azure role assignment changelog list` naredbe.

## <a name="export-to-a-spreadsheet"></a>Izvoz u proračunsku tablicu
Da biste spremili izvješće, ili rukovanje podacima, izvoz access mijenja u .csv datoteku. Izvješće možete vidjeti u proračunskoj tablici na pregled.

![Changelog prikazati kao proračunsku tablicu - snimka](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Vidi također
- Početak rada s [Azure Role-Based kontrola pristupa](role-based-access-control-configure.md)
- Rad sa [Prilagođeno ulogama u Azure RBAC](role-based-access-control-custom-roles.md)
