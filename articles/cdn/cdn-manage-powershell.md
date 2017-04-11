<properties
    pageTitle="Upravljanje Azure CDN sa servisom PowerShell | Microsoft Azure"
    description="Saznajte kako upravljanje Azure CDN pomoću cmdleta ljuske PowerShell Azure."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>Upravljanje Azure CDN sa servisom PowerShell

PowerShell sadrži jedan od najviše fleksibilne načina za Upravljanje profilima Azure CDN i krajnje točke.  PowerShell interaktivno ili pisanjem skripte možete koristiti da biste automatizirali zadatke upravljanja.  U ovom ćete praktičnom vodiču upoznati nekoliko najčešće zadatke koje možete izvršiti pomoću ljuske PowerShell za Upravljanje profilima Azure CDN i krajnje točke.

## <a name="prerequisites"></a>Preduvjeti

Da biste koristili ljuske PowerShell za Upravljanje profilima Azure CDN i krajnje točke, morate imati instaliran modul Azure PowerShell.  Da biste saznali kako instalirati Azure PowerShell i povezati Azure pomoću na `Login-AzureRmAccount` cmdlet, Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Morate se prijaviti u `Login-AzureRmAccount` prije nego što možete izvršiti cmdleta ljuske PowerShell Azure.

## <a name="listing-the-azure-cdn-cmdlets"></a>Popis cmdleta Azure CDN

Svi mogu navode cmdleti Azure CDN pomoću na `Get-Command` cmdlet.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Pristup pomoći

Možete zatražiti pomoć s bilo kojom od tih cmdleta pomoću na `Get-Help` cmdlet.  `Get-Help`omogućuje korištenje i sintaksa i prikazuje primjere.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Unos postojeće Azure CDN profile

Na `Get-AzureRmCdnProfile` cmdlet bez parametre dohvaća sve svoje postojeće CDN profile.

```powershell
Get-AzureRmCdnProfile
```

Ovaj izlaz se može piped da biste cmdleti za numeriranje.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Jedan profil možete vratiti i navođenjem profila grupa naziv i resursa.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] Nije moguće imati više profila CDN s istim nazivom dok god se nalazili u grupama različitih resursa.  Ispuštanje u `ResourceGroupName` parametar vraća svih profila s odgovarajućom nazivom.

## <a name="listing-existing-cdn-endpoints"></a>Postojeće CDN krajnje točke unosa

`Get-AzureRmCdnEndpoint`možete dohvatiti krajnje pojedinačne ili sve krajnje točke na profil.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Stvaranje profila CDN i krajnje točke

`New-AzureRmCdnProfile`i `New-AzureRmCdnEndpoint` se koriste za stvaranje profila CDN i krajnje točke.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Provjera dostupnosti naziv krajnje točke

`Get-AzureRmCdnEndpointNameAvailability`Vraća objekta u kojoj stoji da je naziv krajnje točke dostupna.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Dodavanje prilagođene domene

`New-AzureRmCdnCustomDomain`Dodaje prilagođenog naziva domene za postojeće krajnjoj točki.

>[AZURE.IMPORTANT] Morate postaviti na CNAME kod davatelja usluge DNS-a kao što je opisano [kako mapiranje prilagođene domene u krajnju točku sadržaja isporuke mreže (CDN)](./cdn-map-content-to-custom-domain.md).  Možete testirati i mapiranje prije no što izmijenite krajnjoj točki pomoću `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Izmjena krajnje točke

`Set-AzureRmCdnEndpoint`mijenja postojeće krajnjoj točki.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Pročišćavanje/Pre-loading CDN resursi

`Unpublish-AzureRmCdnEndpointContent`prolazi predmemorirani resursi, dok `Publish-AzureRmCdnEndpointContent` unaprijed učitava imovine na podržani krajnje točke.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Pokretanje i zaustavljanje CDN krajnje točke
`Start-AzureRmCdnEndpoint`i `Stop-AzureRmCdnEndpoint` može se koristiti za pokretanje i zaustavljanje pojedinačne krajnje točke ili grupe krajnje točke.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Brisanje CDN resursi

`Remove-AzureRmCdnProfile`i `Remove-AzureRmCdnEndpoint` možete koristiti da biste uklonili profili i krajnje točke.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako automatizirati CDN Azure s [.NET](./cdn-app-dev-net.md) ili [Node.js](./cdn-app-dev-node.md).

Da biste saznali više o značajkama CDN, potražite u članku [Pregled CDN](./cdn-overview.md).


