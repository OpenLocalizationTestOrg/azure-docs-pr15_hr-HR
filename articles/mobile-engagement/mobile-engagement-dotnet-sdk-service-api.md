<properties 
    pageTitle="Pristup Azure Mobile radnje servisa API-ji putem .NET SDK" 
    description="Opisuje kako koristiti SDK za .NET radnje mobilni pristup API servisa za Azure Mobile radnje"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Pristup Azure Mobile radnje servisa API-ji putem .NET SDK

Azure Mobile radnje izlaže skup API-ji za upravljanje uređajima, razgovor/automatske kampanje itd. Da biste unosili naredbe te API-ji, također dobivate [Swagger datoteka](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) koje možete koristiti s alatima za generiranje SDK-ovi za željeni jezik. Preporučujemo da pomoću alata za [AutoRest](https://github.com/Azure/AutoRest) da biste generirali vaše SDK iz naših Swagger datoteke. 

Stvorili smo .NET SDK na sličan način koji omogućuje interakciju s ove API-ji koristeći C# omot i ne morate učiniti Pregovori tokena provjeru autentičnosti i osvježavanje sami.  

Ovaj primjer prolazi kroz skup koraka da biste počeli pratiti da biste koristili .NET SDK:

1. Prvo, morate postaviti provjere autentičnosti za vaše API-ji pomoću Azure Active Directory kao što je opisano [u nastavku](mobile-engagement-api-authentication.md#authentication). Na kraju ove korake, imat ćete je valjan **SubscriptionId**, **TenantId**, **ApplicationId** i **tajna**. 

2. Koristit ćemo jednostavne aplikacije konzole za Windows da bismo pokazali rad s SDK .NET s scenarij stvaranja kampanje programa objava. Tako da otvorite Visual Studio i stvaranje **Aplikacije konzole**.   

3. Sljedeće morate preuzeti .NET SDK koji je dostupan kao **Biblioteka upravljanja sustava Microsoft Azure radnje** u galeriji Nuget [ovdje](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Ako instalirate na Nuget s Visual Studio, morate bili sigurni da imate potvrdite označena mogućnost **Uključi predizdanja** tijekom pretraživanja paketa:

    ![][1]

4. U na `Program.cs` datoteke, dodajte sljedeće prostori:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Sljedeće potrebno definirati konstante koji koristimo za provjeru autentičnosti i interakcija s mobilnu aplikaciju radnje u kojem stvarate kampanje objave:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Definiranje na `EngagementManagementClient` varijabla koji koristimo za pozivanje metode SDK radnje Mobile:

        static EngagementManagementClient engagementClient; 

7. Dodajte sljedeće da biste svoje `Main` metoda:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Definiranje sljedeću metodu koja vodi brigu o Inicijalizacija na `EngagementManagementClient` tako da najprije provjere autentičnosti i sam Pridruživanjem mobilnu aplikaciju radnje u kojoj namjeravate stvoriti kampanje objave:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Imajte na umu da morate koristiti **Naziv resursa aplikacije** onako kako su definirana na portalu za upravljanje Azure za parametar AppName. 

9. Na kraju, Definirajte metode CreateCampaign koji će voditi brigu o korištenju prethodno inicijalizirana EngagementClient za stvaranje na jednostavne **bilo kojem trenutku** & **samo za obavijesti** kampanje naslova i poruka: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Pokrenite aplikaciju konzole i vidjet ćete sljedeće na uspjelo stvaranje kampanje:

    **Id kampanje '159' uspješno je stvorena, a zatim je u stanju "Skica"**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
