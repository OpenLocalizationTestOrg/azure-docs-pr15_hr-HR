<properties
    pageTitle="Azure Active Directory aktivnosti za prijavu u izvješće API uzoraka | Microsoft Azure"
    description="Upute za početak rada s Azure Active Directory izvješćivanja API-jem"
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

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory uzoraka izvješća API aktivnosti za prijavu

U ovoj se temi je dio zbirka teme za izvješćivanje o pogreškama API Azure Active Directory.  
Azure AD izvješćivanje o pogreškama omogućuje API-JA koji omogućuje pristup podacima aktivnosti za prijavu pomoću koda ili Alati za povezane.  
Da vam pruži uzorak koda za **prijavu aktivnosti API**je djelokrug ove teme.

Pročitajte sljedeće:

- [Zapisnike nadzora](active-directory-reporting-azure-portal.md#audit-logs) više konceptualnih informacija
- [Početak rada s Azure Active Directory izvješćivanja API -jem](active-directory-reporting-api-getting-started.md) za dodatne informacije o izvješćivanja API-JA.

Pitanja, probleme i povratne informacije, zatražite [Pomoć za izvješćivanje o pogreškama AAD](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Preduvjeti
Primjere mogli koristiti u ovoj temi, morate dovršiti [preduvjeti za pristup Azure AD izvješćivanja API-JA](active-directory-reporting-api-prerequisites.md).  


##<a name="powershell-script"></a>Skriptu komponente PowerShell

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Izvođenje skripti
Kada završite s uređivanjem skriptu, pokrenite ga, pa provjerite je li očekivane podatke iz nadzora zapisnike je izvješće, vraća se.

Skripta vraća Izlaz iz izvješća o prijavi u JSON OSNOVNI oblik. Stvara sustava `SigninActivities.json` datoteku s istom izlaz. Možete isprobati izmjenom skriptu da biste se vratili podataka iz drugih izvješća i komentar izvan izlaznom obliku koje su vam potrebne.



## <a name="next-steps"></a>Daljnji koraci

- Želite li da biste prilagodili uzoraka u ovoj temi? Pogledajte [Azure Active Directory prijavu aktivnosti API reference](active-directory-reporting-api-sign-in-activity-reference.md). 

- Ako želite vidjeti cjelovit pregled korištenja API-JA za izvješćivanje o pogreškama Azure Active Directory potražite u članku [Uvod u Azure Active Directory izvješćivanja API-JA](active-directory-reporting-api-getting-started.md).

- Ako želite da biste saznali više o prijavljivanju Azure Active Directory potražite u članku [Azure Active Directory izvješćivanja vodič](active-directory-reporting-guide.md).  