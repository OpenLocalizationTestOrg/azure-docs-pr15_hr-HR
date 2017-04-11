<properties
    pageTitle="Azure Active Directory nadzora API reference | Microsoft Azure"
    description="Upute za početak rada s API nadzora Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory nadzora API reference

U ovoj se temi je dio skup tema o Azure Active Directory izvješćivanje API-JA.  
Azure AD izvješćivanje o pogreškama omogućuje API-JA koji omogućuje vam pristup pomoću koda ili Alati za povezane podatke o nadzoru.
Kako bi pružio referentne informacije o **nadzora API**je djelokrug ove teme.

Pročitajte sljedeće:

- [Zapisnike nadzora](active-directory-reporting-azure-portal.md#audit-logs) više konceptualnih informacija
- [Početak rada s Azure Active Directory izvješćivanja API -jem](active-directory-reporting-api-getting-started.md) za dodatne informacije o izvješćivanja API-JA.

Pitanja, probleme i povratne informacije, zatražite [Pomoć za izvješćivanje o pogreškama AAD](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Tko može pristupiti podacima?

- Korisnici u ulozi administrator sigurnosti ili čitač sigurnosti

- Globalni administratori

- Bilo koju aplikaciju koja ima autorizacije da biste pristupili na API-JA (autorizacija aplikacija može biti postavljanje samo na temelju dozvola globalni administrator)



## <a name="prerequisites"></a>Preduvjeti

Da biste pristupili izvješće kroz API izvješća, morate imati:

- Sa sustavom [Azure Active Directory besplatne ili bolje edition](active-directory-editions.md)

- [Preduvjeti za pristup Azure AD izvješćivanja API](active-directory-reporting-api-prerequisites.md)kao dovršene. 
 

##<a name="accessing-the-api"></a>Pristup s API-JA

Pristupite taj API pomoću [Programa Explorer grafikonu](https://graphexplorer2.cloudapp.net) ili programski pomoću, na primjer, komponente PowerShell. Kako bi PowerShell pravilno protumačiti sintaksa filtar OData koristi u AAD grafikonu OSTALE pozive, morate koristiti u backtick (ili: Gravis) znaka "escape" znak $. Znak backtick služi kao [PowerShell-prespojni znak](https://technet.microsoft.com/library/hh847755.aspx)dopuštanja PowerShell učiniti slovni tumačenja znak $ i izbjegli zbunjujućim kao naziv varijable PowerShell (ie: $filter).

Na grafikonu Explorer je žarište ove teme. Primjer s PowerShell potražite u članku [skriptu PowerShell](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>Krajnja točka API-JA


Možete pristupiti taj API pomoću sljedećih URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Nema ograničenja broja zapisa vratio API nadzora Azure AD (pomoću OData numeriranje stranica).
Ograničenja zadržavanja podataka izvješća, pogledajte [Izvješća o zadržavanju](active-directory-reporting-retention.md).

U ovom poziva vraća podatke u grupama. Svaku seriju ima najviše 1000 zapisa.  
Da biste sljedeći skup zapisa, koristite sljedeću vezu. Skiptoken informacije zatražite od prvog skupa vraćeni zapise. Preskoči token postavit će se na kraju rezultat.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Podržani filtara

Možete suzili broj zapisa koje je vratio API poziva u obliku filtra.  
API za prijavu u povezanim podacima, podržane su sljedeće filtre:

- **$top =\<broj zapisa koju želite vratiti\> ** – da biste ograničili broj vraćenih zapisa. Ovo je skupi operacija. Pomoću ovog filtra ne ako želite da biste se vratili tisuće objekte.     
- **$filter =\<izvješćem filtar\> ** – da biste odredili na temelju polja podržana filtra, vrste zapisa koje vas zanimaju



## <a name="supported-filter-fields-and-operators"></a>Podržani filtara i operatora

Da biste odredili vrstu zapisa koje vas zanimaju, možete izraditi filtar naredbu koja se može sadržavati jedno ili kombinaciju sljedeća polja filtra:

- definira [DatumAktivnosti](#activitydate) - datum ili raspon datuma
- [activityType](#activitytype) - definira vrstu aktivnosti
- [aktivnosti](#activity) – definira aktivnosti kao niz  
- [ime/glumca](#actorname) - definira na glumca u obliku ime na glumca
- [glumca/ID objekta](#actorobjectid) – definira na glumca u obrascu u glumca ID-a   
- [glumca/upn](#actorupn) - definira na glumca u obrascu u glumca načelo ime (UPN) 
- [ciljno ime](#targetname) - definira cilj u obliku ime na glumca
- [cilj/ID objekta](#targetobjectid) – definira cilj u obrascu s ciljem ID-a  
- [cilj/upn](#targetupn) - definira na glumca u obrascu u glumca načelo ime (UPN)   




----------

### <a name="activitydate"></a>DatumAktivnosti

**Podržani operatora**: eq, a zatim Spoji, s, gt, lt

**Primjer**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Bilješke**:

Datum i vrijeme mora biti u obliku UTC-a

----------

### <a name="activitytype"></a>activityType

**Podržani operatora**: eq

**Primjer**:

    $filter=activityType eq 'User'  

**Bilješke**:

velika i mala slova

----------

### <a name="activity"></a>aktivnosti

**Podržani operatora**: eq, sadrži, startsWith

**Primjer**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Bilješke**:

velika i mala slova

----------

### <a name="actorname"></a>ime/glumca

**Podržani operatora**: eq, sadrži, startsWith

**Primjer**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Bilješke**:

velika i mala slova

    

----------
### <a name="actorobjectid"></a>glumca/ID objekta

**Podržani operatora**: eq

**Primjer**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>Odredišni naziv

**Podržani operatora**: eq, sadrži, startsWith

**Primjer**:

    $filter=targets/any(t: t/name eq 'some name')   

**Bilješke**:

Velika i mala slova

----------

### <a name="targetupn"></a>cilj/UPN-a

**Podržani operatora**: eq, startsWith

**Primjer**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Bilješke**:

- Velika i mala slova
- Najprije morate dodati potpuni kada ispitivanje Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

----------

### <a name="targetobjectid"></a>cilj/ID objekta

**Podržani operatora**: eq

**Primjer**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>glumca/UPN-a

**Podržani operatora**: eq, startsWith

**Primjer**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Bilješke**:

- Velika i mala slova 
- Najprije morate dodati potpuni prilikom postavljanja upita Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

----------




## <a name="next-steps"></a>Daljnji koraci

- Želite li pogledajte primjere filtrirani sustava aktivnosti? Pogledajte [Azure Active Directory nadzora API uzorka](active-directory-reporting-api-audit-samples.md).

- Želite li saznati više o Azure AD izvješćivanja API-JA? Potražite u članku [Uvod u rad s Azure Active Directory izvješćivanja API -jem](active-directory-reporting-api-getting-started.md).