<properties
    pageTitle="Analitički zapisnika prijave pretraživanje REST API-JA | Microsoft Azure"
    description="Ovaj vodič sadrži osnovni vodič koji opisuje kako REST API-JA za pretraživanje zapisnika analize možete koristiti u na operacije upravljanja paket (OMS) i u njoj nalaze primjeri koji vam pokazuju kako pomoću naredbe."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Zapisnik analize zapisnika pretraživanja REST API-JA

Ovaj vodič sadrži osnovni vodič koji opisuje kako prijava analitiku pretraživanje REST API-JA možete koristiti u na operacije upravljanja paket (OMS) i u njoj nalaze primjeri koji vam pokazuju kako pomoću naredbe. Neke se primjerima u ovom se članku reference radu uvide, koji se naziva prethodnu verziju zapisnika analize.

## <a name="overview-of-the-log-search-rest-api"></a>Pregled zapisnika pretraživanja REST API-JA

Prijava analitiku pretraživanje REST API-JA je RESTful te im možete pristupiti putem upravitelja resursa API Azure. U ovom dokumentu pronaći ćete primjere gdje će se na API pristupiti putem na [ARMClient](https://github.com/projectkudu/ARMClient), alat naredbenog retka za Otvori izvor koji pojednostavljuje pozivanje Azure resursima API-JA. Korištenje ARMClient i PowerShell jedan je od mnogo mogućnosti za pristup API za pretraživanje zapisnika analize. Druga je mogućnost da biste koristili modul Azure PowerShell OperationalInsights koja obuhvaća cmdleta za pristup pretraživanja. Pomoću alata za te mogu koristiti RESTful API Azure resursima za upućivanje poziva na radne prostore OMS i izvršavanje naredbi pretraživanja unutar njih. U API će izlaz rezultata pretraživanja na vas u obliku JSON, što omogućuje korištenje rezultata pretraživanja na razne načine programski.

Voditelj resursa Azure može se koristiti putem [biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) , kao i [REST API -JA](https://msdn.microsoft.com/library/azure/mt163658.aspx). Pregled povezanih web-stranica da biste saznali više.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Praktični vodič za osnovni prijava analitiku pretraživanje REST API-JA

### <a name="to-use-the-arm-client"></a>Da biste koristili klijent ARM

1. Instalirajte [Chocolatey](https://chocolatey.org/), koji je za otvaranje upravitelja paketa izvora za Windows. Otvorite prozor naredbenog retka kao administrator, a zatim pokrenite sljedeću naredbu:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Instalirajte na ARMClient ponovnim pokretanjem sljedeće naredbe:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Da biste izvršili jednostavno pretraživanje pomoću na ARMClient

1. Prijava na račun za Microsoft ili OrgID:

    ```
    armclient login
    ```

    Uspješna prijava navodi sve pretplate uz zadani račun:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Dohvati radne prostore paket za upravljanje operacije:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Uspješno obavljanje poziv želite poslati sve radne prostore uz pretplatu:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Stvorite varijablu pretraživanje:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Pretraživanje pomoću novog varijablu pretraživanje:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Prijavite se primjerima referenca analize pretraživanje REST API-JA
Sljedeći primjeri pokazuju kako pomoću API pretraživanja.

### <a name="search---actionread"></a>Pretraživanje – akcija/pročitano

**Ogledna Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Zahtjev:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
U sljedećoj su tablici se opisuju svojstva koje su dostupne.

|**Svojstvo**|**Opis**|
|---|---|
|vrh|Maksimalan broj vraćenih rezultata.|
|Isticanje|Sadrži pre i objave parametara obično koristi za isticanje polja koja se podudaraju|
|Pre|Prefixes navedeni niz za usklađenim poljima.|
|Objava|Dodaje navedeni niz usklađenim poljima.|
|upit|Pretraživanje upit koji se koristi za prikupljanje i rezultate.|
|pokretanje|Početak termin koji želite pronaći rezultate.|
|END|Kraj termin koji želite pronaći rezultate.|


**Odgovor:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Pretraživanje / {ID} - akcija/pročitano

**Zahtjev za sadržaj spremljeno pretraživanje:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Pretraživanje vraća čiji je status "Na čekanju", provjere ažurirane rezultate možete riješiti kroz ovaj API-JA. Nakon 6 min rezultat pretraživanja će nestati s predmemorije i HTTP nestaje će vratiti. Ako zahtjev za pretraživanjem početne "Uspjelo" status odmah, ne dodat će se u predmemoriju uzrokuju ovaj API da biste se vratili HTTP nestaje ako mu. Sadržaj i HTTP 200 rezultat bit će jednak formatu zahtjev početne pretraživanje samo s ažurirane vrijednosti.

### <a name="saved-searches---rest-only"></a>Spremljena pretraživanja – samo REST

**Zahtjev za popis spremljena pretraživanja:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Podržani metoda: GET STAVITI brisanje

Podržani zbirke metoda: početak

U sljedećoj su tablici se opisuju svojstva koje su dostupne.

|Svojstvo|Opis|
|---|---|
|ID-a|Jedinstveni identifikator.|
|E-oznake|**Potrebne za zakrpu**. Ažuriranje poslužitelja na svaki unos. Vrijednost mora biti jednak trenutne vrijednosti pohranjene ili "*" da biste ažurirali. 409 vraća vrijednosti stari/nije valjan.|
|Properties.Query|**Obavezno**. Upit za pretraživanje.|
|properties.displayName|**Obavezno**. Korisničko ime definirani prikaz upita. Ako katalog modeliran kao Azure resursa, to bi oznake.|
|Properties.category|**Obavezno**. Korisnički definirane kategorije upita. Ako katalog modeliran kao Azure resurs to bi oznake.|

>[AZURE.NOTE] API-JA za pretraživanje zapisnika analize trenutno vraća stvorili kao korisnik spremljena pretraživanja kada provjeravan spremljena pretraživanja u radnom prostoru. U API će vratiti nudi rješenja trenutno je spremljena pretraživanja. Ta je funkcija dodat će se u budućnosti.

### <a name="create-saved-searches"></a>Stvaranje spremljenih pretraživanja

**Zahtjev:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Brisanje spremljena pretraživanja

**Zahtjev:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Ažuriranje spremljena pretraživanja

 **Zahtjev:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metapodaci – samo JSON

Evo kako da biste vidjeli polja za sve vrste zapisnika za podatke prikupljene u radnom prostoru. Ako, na primjer, ako želite da znate da ako je vrsta događaja ima polje pod nazivom računala, zatim to je da biste potražili i potvrdite.

**Zahtjev za polja:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Odgovor:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

U sljedećoj su tablici se opisuju svojstva koje su dostupne.

|**Svojstvo**|**Opis**|
|---|---|
|ime|Naziv polja.|
|riješiti problem|Zaslonsko ime polja.|
|Vrsta|Vrsta vrijednosti polja.|
|facetable|Kombinacija trenutne 'indeksirana', ' pohranjene "i svojstva 'fasete'.|
|Prikaz|Trenutno svojstvo "prikaz". TRUE ako je polje vidljive u pretraživanju.|
|ownerType|Smanjena samo vrstama pripadaju onboarded IP.|


## <a name="optional-parameters"></a>Neobavezni parametri
Informacije u nastavku opisuju neobavezni parametri dostupni.

### <a name="highlighting"></a>Isticanje

Parametar "Isticanje" nije neobavezan parametar koji se mogu koristiti da bi se zatražila podsustav pretraživanja obuhvatiti skup oznake njegov odgovor.

Ove oznake označavanje početka i završetka istaknuti tekst koji zadovoljava uvjete navedene u upit za pretraživanje.
Možete navesti početka i završetka oznake koja će se koristiti pretraživanja da bi se istaknuto termina.

**Primjer upit za pretraživanje**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Ogledna rezultat:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Obratite pozornost na to da iznad rezultata sadrži poruku o pogrešci koja ima mjestu, a dodan.

## <a name="computer-groups"></a>Grupa računala

Računalo grupe su posebno spremljena pretraživanja koje vraćaju skup računala.  Računalo grupe u druge upite možete koristiti da biste ograničili rezultate na računala u grupi.  Grupe računalo je implementirana kao spremljeno pretraživanje s oznakom grupe s vrijednošću računala.

Slijedi odgovor uzorka za grupu za računala.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Dohvaćanje grupe računala

Upotrijebite metodu Get ID grupe za dohvaćanje grupu za računala.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Stvaranje ili ažuriranje računala grupe

Način stavi pomoću jedinstvenih spremljene pomoću ID-a za pretraživanje da biste stvorili novu grupu za računala. Ako koristite postojeću ID grupe na računalu, zatim da jedna će se izmijeniti. Kada stvorite grupu za računalo na konzoli OMS, ID je stvorene iz grupe i ime.

Upit koji se koristi za definiciju grupa mora vratiti skup računalima za grupu koju želite pravilno funkcionirati.  Preporučuje se da prekinete upit s *| Različita računala* da bi se rezultat dobila točne podatke.

Definicija spremljeno pretraživanje mora sadržavati oznaku pod nazivom grupe s vrijednošću računala za pretraživanje da biste se klasificirati kao grupu za računala.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Brisanje grupe za računala

Upotrijebite metodu Izbriši ID grupe da biste izbrisali grupu računala.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) za sastavljanje upita pomoću prilagođenih polja kao kriterij.
