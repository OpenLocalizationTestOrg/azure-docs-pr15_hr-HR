<properties
    pageTitle="Azure Active Directory prijavu izvješće o aktivnosti API reference | Microsoft Azure"
    description="Vodič za izvješće API-JA aktivnosti za prijavu Azure Active Directory"
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
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory prijavu izvješće o aktivnosti API reference


U ovoj se temi je dio skup tema o Azure Active Directory izvješćivanje API-JA.  
Azure AD izvješćivanje o pogreškama omogućuje API-JA koji omogućuje vam pristup podataka izvješća aktivnosti za prijavu pomoću koda ili Alati za povezane.
Opseg u ovoj se temi je da vam pruži referentne informacije o **aktivnosti izvješća API za prijavu**.

Pročitajte sljedeće:

- [Aktivnosti za prijavu](active-directory-reporting-azure-portal.md#sign-in-activities) za dodatne konceptualnih informacija
- [Početak rada s Azure Active Directory izvješćivanja API -jem](active-directory-reporting-api-getting-started.md) za dodatne informacije o izvješćivanja API-JA.

Pitanja, probleme i povratne informacije, zatražite [Pomoć za izvješćivanje o pogreškama AAD](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Tko može pristupiti podacima API-JA?

- Korisnici u ulozi administrator sigurnosti ili čitač sigurnosti

- Globalni administratori

- Bilo koju aplikaciju koja ima autorizacije da biste pristupili na API-JA (autorizacija aplikacije može biti postavljanje samo na temelju dozvola globalni administrator)



## <a name="prerequisites"></a>Preduvjeti

Da biste pristupili izvješće kroz izvješćivanja API, morate imati:

- Sa sustavom [Azure Active Directory Premium P1 ili P2 edition](active-directory-editions.md)

- [Preduvjeti za pristup Azure AD izvješćivanja API](active-directory-reporting-api-prerequisites.md)kao dovršene. 


##<a name="accessing-the-api"></a>Pristup s API-JA

Pristupite taj API pomoću [Programa Explorer grafikonu](https://graphexplorer2.cloudapp.net) ili programski pomoću, na primjer, komponente PowerShell. Kako bi PowerShell pravilno protumačiti sintaksu filtra za OData koja se koristi u AAD grafikonu OSTALE pozive, morate koristiti u backtick (ili: Gravis) znaka "escape" znak $. Znak backtick služi kao [PowerShell-prespojni znak](https://technet.microsoft.com/library/hh847755.aspx)dopuštanja PowerShell učiniti slovni tumačenja znak $ i izbjegli zbunjujućim kao naziv varijable PowerShell (ie: $filter).

Na grafikonu Explorer je žarište ove teme. Primjer s PowerShell potražite u članku [skriptu PowerShell](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>Krajnja točka API-JA

Možete pristupiti taj API pomoću sljedećih osnovni URI:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Zbog količinu podataka taj API ima ograničenje od milijun vraća zapisa. 

U ovom poziva vraća podatke u grupama. Svaku seriju ima najviše 1000 zapisa.  
Da biste sljedeći skup zapisa, koristite sljedeću vezu. Dobiti [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) informacije iz prvog skupa vraćenih zapisa. Preskoči token postavit će se na kraju rezultat.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Podržani filtara

Možete suzili broj zapisa koje je vratio API poziva u obliku filtra.  
API za prijavu u povezanim podacima, podržane su sljedeće filtre:

- **$top =\<broj zapisa koju želite vratiti\> ** – da biste ograničili broj vraćenih zapisa. Ovo je skupi operacija. Pomoću ovog filtra ne ako želite da biste se vratili tisuće objekte.  
- **$filter =\<izvješćem filtar\> ** – da biste odredili na temelju polja podržana filtra, vrste zapisa koje vas zanimaju



## <a name="supported-filter-fields-and-operators"></a>Podržani filtara i operatora

Da biste odredili vrstu zapisa koje vas zanimaju, možete izraditi filtar naredbu koja se može sadržavati jedno ili kombinaciju sljedeća polja filtra:

- definira [signinDateTime](#signindatetime) - datum ili raspon datuma

- [korisnički ID](#userid) - definira određenog korisnika koji se temelji na korisničkog ID-a

- [userPrincipalName](#userprincipalname) - definira određenog korisnika koji se temelje korisnika korisnikovo Glavno ime (UPN)

- [ID programa](#appid) - definira određenim aplikacija temelji ID aplikacije

- [appDisplayName](#appdisplayname) - definira određenim aplikacije koji se temelji na aplikaciju zaslonsko ime

- [loginStatus](#loginStatus) - definira statusa prijave (Uspjeh / pogreške)


> [AZURE.NOTE] Kada pomoću programa Explorer Graph, koje treba točan slučaj upotrebe za svako slovo je polja filtra.


Da biste suzili opseg vraćenih podataka, možete sastaviti kombinacijama podržanih filtri i polja filtra. Na primjer, sljedeća naredba vraća gornji 10 zapisa između srpanj 1st 2016 i srpanj 6. 2016.

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Podržani operatora**: eq, a zatim Spoji, s, gt, lt

**Primjer**:

Korištenje određenog datuma

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Korištenje raspona datuma    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Bilješke**:

Parametar datumvrijeme mora biti u obliku UTC-a 


----------

### <a name="userid"></a>ID korisnika

**Podržani operatora**: eq

**Primjer**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Bilješke**:

Vrijednost korisnički ID je vrijednost niza



----------

### <a name="userprincipalname"></a>userPrincipalName

**Podržani operatora**: eq

**Primjer**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Bilješke**:

Vrijednosti userPrincipalName je vrijednost niza

----------

### <a name="appid"></a>ID programa

**Podržani operatora**: eq

**Primjer**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Bilješke**:

Vrijednost ID programa je vrijednost niza

----------


### <a name="appdisplayname"></a>appDisplayName

**Podržani operatora**: eq

**Primjer**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Bilješke**:

Vrijednost appDisplayName je vrijednost niza

----------

### <a name="loginstatus"></a>loginStatus

**Podržani operatora**: eq

**Primjer**:

    $filter=loginStatus+eq+'1'  


**Bilješke**:

Postoje dvije mogućnosti za na loginStatus: 0 - uspjeha, 1 - Pogreška

----------



## <a name="next-steps"></a>Daljnji koraci

- Želite da biste vidjeli primjere za filtrirani aktivnosti za prijavu? Pogledajte [Azure Active Directory aktivnosti za prijavu u izvješću API primjere](active-directory-reporting-api-sign-in-activity-samples.md).

- Želite li saznati više o Azure AD izvješćivanja API-JA? Potražite u članku [Uvod u rad s Azure Active Directory izvješćivanja API -jem](active-directory-reporting-api-getting-started.md).