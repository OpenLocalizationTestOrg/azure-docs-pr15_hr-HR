<properties
   pageTitle="Početak tražene vrijednosti za provjeru autentičnosti aplikacije da biste pristupili SQL baze podataka iz koda | Microsoft Azure"
   description="Stvorite glavni servisa za pristup SQL baze podataka iz koda."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Početak tražene vrijednosti za provjeru autentičnosti aplikacije da biste pristupili SQL baze podataka iz koda

Stvaranje i upravljanje bazom podataka SQL kod morate registrirati aplikacije u domeni Azure Active Directory (AAD) u pretplate gdje su stvoreni Azure resurse.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Stvorite uslugu glavni pristupa resursima iz aplikacije

Morate imati na najnovije [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) instaliran i pokrenut. Detaljne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

Sljedeću skriptu komponente PowerShell stvara aplikacije Active Directory (AD) i glavnicu usluge koje je potrebna za provjeru autentičnosti naša C# aplikacija. Skripta proizvodi vrijednosti koje su potrebna za prethodni C# uzorka. Detaljne informacije potražite u članku [Korištenje ljuske PowerShell Azure da biste stvorili glavni pristupa resursima servisa](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Vidi također

- [Stvaranje baze podataka SQL uz C#](sql-database-get-started-csharp.md)
- [Povezivanje s bazom podataka SQL pomoću provjere autentičnosti Azure Active Directory](sql-database-aad-authentication.md)


