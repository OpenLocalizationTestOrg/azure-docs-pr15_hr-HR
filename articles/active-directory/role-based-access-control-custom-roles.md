<properties
    pageTitle="Prilagođene uloge u Azure RBAC | Microsoft Azure"
    description="Saznajte kako odrediti prilagođene uloge s Azure Role-Based kontrola pristupa za preciznije upravljanje identitetom u pretplatu za Azure."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Prilagođene uloge u Azure RBAC


Stvorite prilagođene uloga u kontrolu Azure Role-Based pristup (RBAC) ako nijedan od ugrađenih uloga odgovara vašim potrebama određenog programa access. Prilagođene uloge moguće stvoriti pomoću [Komponente PowerShell Azure](role-based-access-control-manage-access-powershell.md), [sučelje naredbenog retka za Azure](role-based-access-control-manage-access-azure-cli.md) (EŽA) i [REST API -JA](role-based-access-control-manage-access-rest.md). Baš kao i ugrađene uloge prilagođene uloge se mogu dodijeliti korisnike, grupe i aplikacije, na pretplatu, grupa resursa i opsega resursa. Prilagođene uloge spremaju se u klijent za Azure AD i mogu zajednički koristiti putem pretplate na koji koriste taj klijent kao imenik Azure AD za na subsciption.

Slijedi primjer prilagođene uloge za nadzor i ponovno pokrenuti virtualnim računalima sustava:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Akcija
Svojstvo **Akcija** prilagođene uloge određuje Azure operacije na koji se uloga dopušta pristup. Zbirka nizova operacije koje označavaju sigurnosni operacije davatelja Azure resurs je. Operacija nizovi koji sadrže zamjenskih znakova (\*) dopustiti pristup sve operacije koje odgovaraju niz operacija. Na primjer:

-   `*/read`daje pristup operacija za sve vrste resursa sve Azure resursa davatelja čitanja.
-   `Microsoft.Network/*/read`daje pristup operacija za sve vrste resursa u davatelja resursa Microsoft.Network od Azure čitanja.
-   `Microsoft.Compute/virtualMachines/*`daje pristup za sve operacije virtualnim strojevima i njegove podređene vrste resursa.
-   `Microsoft.Web/sites/restart/Action`daje pristup da biste ponovno pokrenuli web-mjesta.

Korištenje `Get-AzureRmProviderOperation` (u PowerShell) ili `azure provider operations show` (u EŽA Azure) na popisu operacija davatelja Azure resursa. Te naredbe mogu koristiti i da biste provjerili je li valjani niz za operaciju i da biste proširili zamjenskih operacija nizova.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell snimka zaslona s primjerom - Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | Operacija FT, OperationName](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure EŽA snimka - azure davatelja operacije Prikaži "Microsoft.Compute/virtualMachines/\*/action" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Svojstvo **NotActions** koristite skup operacije koje želite dopustiti jednostavnije definira bez ograničenih operacija. Pristup odobren prilagođene ulogom je izračunati oduzimanjem operacije **NotActions** operacija **Akcije** .

> [AZURE.NOTE] Ako se korisniku Dodijeli ulogu isključuje postupak u **NotActions**, a drugi uloga koja omogućuje pristup isti postupak vam je dodijeljen, korisnik će moći izvršiti postupak. **NotActions** nije pravilo Uskrati – je jednostavno prikladan je način za stvaranje skupa dopuštenih operacije kada određene operacije potrebno izuzeti.

## <a name="assignablescopes"></a>AssignableScopes
Svojstvo **AssignableScopes** prilagođene uloge određuje opsega (pretplate, grupa resursa ili resursa) unutar koji je dostupan za dodjele prilagođene uloge. Omogućavanje prilagođene ulogama za dodjelu u samo pretplate ili grupa resursa koje je potrebno je i nije Zagušenje korisničkog sučelja za ostale pretplate ili grupe resursa.

Primjeri valjani koju valja dodijeliti opsega obuhvaćaju sljedeće:

-   "/ pretplate/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ pretplate/e91d47c4-76f3-4271-a796-21b4ecfe3624" - stavlja ulogu za dodjelu u dva pretplate.
-   "/ pretplate/c276fc76-9cd4-44c9-99a7-4fd71546436e" - stavlja ulogu za dodjelu u jednom pretplate.
-  "/ pretplate/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/mreže" - postaje dostupan za dodjelu uloga samo u grupi mrežni resurs.

> [AZURE.NOTE] Koristite najmanje jedan pretplatu, grupa resursa ili ID resursa.

## <a name="custom-roles-access-control"></a>Prilagođene uloge pristup kontrola
Svojstvo **AssignableScopes** prilagođene uloge i određuje tko može vidjeti, izmjena i brisanje ulogu.

- Tko može stvarati prilagođene uloga?
    Vlasnici (i korisničkog pristupa administratorima) pretplata, grupa resursa i resursa možete stvoriti prilagođene uloge za korištenje u tim dosezima.
    Korisnik stvaranje ulogu mora moći izvršiti `Microsoft.Authorization/roleDefinition/write` operacije na **AssignableScopes** uloge.

- Tko možete izmijeniti prilagođene uloga?
    Vlasnici (i korisničkog pristupa administratorima) pretplata, grupa resursa i resursa možete izmijeniti prilagođene uloge u tim dosezima. Korisnici moraju moći izvršiti na `Microsoft.Authorization/roleDefinition/write` operacije na **AssignableScopes** prilagođene uloge.

- Tko može pregledavati prilagođene uloge?
    Sve ugrađene uloge u Azure RBAC omogućuju prikaz uloge koje su dostupne za dodjelu. Korisnici koji mogu izvršavati na `Microsoft.Authorization/roleDefinition/read` postupak opseg možete pogledati RBAC uloge koje su dostupne za dodjelu na taj doseg.

## <a name="see-also"></a>Vidi također
- [Kontrola pristupa temelji uloga](role-based-access-control-configure.md): početak rada s RBAC na portalu za Azure.
- Informirajte se o upravljanju pristupa:
    - [PowerShell](role-based-access-control-manage-access-powershell.md)
    - [Azure EŽA](role-based-access-control-manage-access-azure-cli.md)
    - [REST API-JA](role-based-access-control-manage-access-rest.md)
- [Ugrađeni uloge](role-based-access-built-in-roles.md): dohvaćanje detalja o uloge od kojih svaka standard u RBAC.
