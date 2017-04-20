## <a name="obtain-an-azure-resource-manager-token"></a>Nabavite token Azure upravitelj resursa

Azure Active Directory morate provjeriti autentičnost sve zadatke koje možete izvršiti na resurse pomoću upravitelja resursa Azure. Primjer ovdje prikazane koristi provjeru autentičnosti lozinke za druge postupke pogledajte [zahtjeve greški upravitelj resursa Azure][lnk-authenticate-arm].

1. Dodajte sljedeći kôd **glavni** način u Program.cs dohvatiti token iz Azure AD pomoću aplikacije id i lozinku.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. Stvaranje objekta **ResourceManagementClient** koji koristi token dodavanjem sljedeći kod kraj **glavni** način:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Stvaranje ili dobiti referencu za grupe resursa koristite:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx