<properties
    pageTitle="Početak rada s bibliotekom CDN Azure za .NET | Microsoft Azure"
    description="Saznajte kako napisati upravljanje CDN Azure pomoću Visual Studio .NET aplikacijama."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Početak rada s razvoj Azure CDN

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

[Azure CDN biblioteka za .NET](https://msdn.microsoft.com/library/mt657769.aspx) možete koristiti da biste automatizirali stvaranje i Upravljanje profilima CDN i krajnje točke.  Pomoću ovog praktičnog vodiča vodi kroz stvaranje jednostavne aplikacije konzole .NET koji pokazuje nekoliko dostupno operacije.  Pomoću ovog praktičnog vodiča je namijenjen opisuju svim aspektima biblioteku CDN Azure za .NET detaljno.

Potreban vam je Visual Studio 2015 za dovršetak ovog praktičnog vodiča.  [Visual Studio zajednice 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) je besplatno preuzeti.

> [AZURE.TIP] [Dovršena projekta iz ovog praktičnog vodiča](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) dostupna je za preuzimanje na MSDN-u.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Stvaranje projekta i dodavanje Nuget paketa

Nakon što smo ste stvorili grupu resursa za naše profile CDN i dao naš Azure AD aplikacije dozvolu za Upravljanje profilima CDN i krajnje točke u toj grupi, ne možemo mogu početi stvarati naš aplikacije.

U Visual Studio 2015, kliknite **datoteku**, **Novi** **projekta...** da biste otvorili dijaloški okvir novi projekt.  Proširite **Visual C#**, a zatim odaberite **Windows** u oknu s lijeve strane.  Kliknite **Program konzole** središnjeg okna.  Naziv projekta, a zatim kliknite **u redu**.  

![Novi projekt](./media/cdn-app-dev-net/cdn-new-project.png)

Naš projekt će koristiti neke Azure biblioteke koje se nalaze u Nuget paketa.  Dodajmo one u projekt.

1. Kliknite **Alati** , **Upravitelj Nuget paketa**, a zatim **Konzole za Upravitelj paketa**.

    ![Upravljanje Nuget paketa](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. Na konzoli za Upravitelj paketa, pokrenite sljedeću naredbu da biste instalirali **Active Directory provjera autentičnosti biblioteke (ADAL)**:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Izvođenje sljedeće da biste instalirali **Biblioteke za upravljanje CDN Azure**:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Upute poslužitelja, konstante, glavni način i metode preglednika

Recimo da se osnovna struktura naš program napisan.

1. Na kartici Program.cs zamjena na `using` upute poslužitelja pri vrhu sa sljedećim:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Ne možemo potrebno definirati neke konstante će koristiti naše metode.  U na `Program` klase, ali prije no što se `Main` način, dodajte sljedeće.  Ne zaboravite da biste zamijenili rezervirana mjesta, uključujući u ** &lt;uglate zagrade&gt;**, pomoću vlastitih vrijednosti po potrebi.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Na razini predmete definirati i te dvije varijable.  Ćemo koristiti ih kasnije da biste odredili našu stranicu profila i krajnje točke već postoji.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Zamjena na `Main` metoda na sljedeći način:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Neke od naše druge načine namjeravate Pitaj korisnika s pitanjima "Da/ne".  Dodajte sljedeće način da biste olakšali koje malo:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Osnovna struktura naš program napisan, ne možemo trebali biste stvoriti metode pozove na `Main` način.

## <a name="authentication"></a>Provjera autentičnosti

Prije nego što koristimo biblioteke za upravljanje Azure CDN moramo autentičnost naših usluga glavnicu i dobiti token za provjeru autentičnosti.  Ova metoda koristi ADAL za dohvaćanje token.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Ako koristite provjeru autentičnosti pojedinačnih korisnika, u `GetAccessToken` način će izgledati malo drugačije.

>[AZURE.IMPORTANT] Ovaj uzorak koda koristiti samo ako odabirete da bi provjere autentičnosti pojedinačnih korisnika umjesto glavni servisa.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Ne zaboravite da biste zamijenili `<redirect URI>` s preusmjeravanje URI uneseni kada aplikacija registrira u Azure AD.

## <a name="list-cdn-profiles-and-endpoints"></a>Popis CDN profila i krajnje točke

Sada ćemo spremni ste za izvođenje operacija CDN.  Prvo što ne naš način je popis profili i krajnje točke u našem grupa resursa, a ako se pronađe podudarajući rezultat za nazive profil i krajnje točke naveden u našem konstante daje bilješke koje za kasnije tako da ne možemo pokušaja stvaranje duplikata.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Stvaranje profila CDN i krajnje točke

Zatim ćemo stvoriti profil.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Nakon stvaranja profila, stvorit ćemo ćete krajnje točke.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] Gornji primjer dodjeljuje krajnju točku sustava polazište pod nazivom *Contoso* s naziv glavnog računala `www.contoso.com`.  Trebali biste promijeniti tako da upućuju na vlastitu polazište naziv glavnog računala.

## <a name="purge-an-endpoint"></a>Čišćenje krajnje točke

Pod pretpostavkom da je stvorena krajnju točku, jedan zadatak za uobičajene koje želimo izvesti u naš program je čišćenja sadržaja u našem krajnjoj točki.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] U primjeru iznad niz `/*` označava koje želim Očisti sve u korijenski put krajnjoj točki.  To je jednako Provjera **Očisti sve** u dijaloškom okviru "Pročistite" Azure portal. U na `CreateCdnProfile` metoda sam stvorio naš profil kao profil programa **Azure CDN iz Verizon** pomoću koda `Sku = new Sku(SkuName.StandardVerizon)`tako da se to će biti uspješno.  Međutim, profili **CDN Azure s Akamai** ne podržavaju **Čišćenje svih**, tako da je pomoću profil programa Akamai za ovog praktičnog vodiča, bi trebam li uključiti određene putove za čišćenje.

## <a name="delete-cdn-profiles-and-endpoints"></a>Brisanje CDN profila i krajnje točke

Zadnji metode izbrisat će se naš krajnjoj točki i profila.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Izvođenje programa

Ćemo sada Kompiliranje i pokrenite program klikom na gumb **Start** u Visual Studio.

![Program za pokretanje](./media/cdn-app-dev-net/cdn-program-running-1.png)

Kada program je dostigne iznad pitanje, moći ćete vratite se u grupu resursa na portalu za Azure i potražite u članku profil je stvorena.

![Uspjeh!](./media/cdn-app-dev-net/cdn-success.png)

Ne možemo možete potvrditi upute za izvođenje ostatak programa.

![Dovršavanje program](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Daljnji koraci

Da biste vidjeli dovršene projekta iz ovog vodiča [uzorka za preuzimanje](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Da biste pronašli dodatne dokumentaciji na biblioteke za upravljanje Azure CDN .NET, prikaz [reference na MSDN-u](https://msdn.microsoft.com/library/mt657769.aspx).

Upravljanje resurse CDN sa [servisom PowerShell](./cdn-manage-powershell.md).


