<properties 
    pageTitle="Azure mobilne radnje - pozadinskog Integracija" 
    description="Povezivanje radnje Mobile Azure s pozadinskom sustava SharePoint da biste stvorili kampanje iz sustava SharePoint" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure mobilne radnje - Integracija API-JA

U automatiziranog marketinške sustavu, stvaranje i aktiviranje marketinške kampanje pojaviti automatski. U tu svrhu - radnje Azure Mobile omogućuje stvaranje takve automatiziranog marketinške kampanje kao i korištenja API-ji. 

Obično klijentima pomoću sučelja sučelje radnje Mobile možete stvoriti najave/ankete itd kao dio njihove marketinških kampanja. No kao marketinške kampanje postaju starijih, ne postoji treba koristiti podatke zaključan u sustavima pozadinskog (kao što je sustav CRM ni a CMS sustava kao što je SharePoint) tako da se potpuno automatsko kanal moguće stvoriti koji stvara kampanje u Mobile radnje dinamički na temelju podataka slijedi u iz pozadinskog sustava. 

![][5]

Pomoću ovog praktičnog vodiča prolazi kroz takve scenarij u kojem Poslovni korisnik sustava SharePoint popunjava popisa sustava SharePoint s marketinškom podacima i automatiziranog procesa uzima stavke s popisa i povezuje sa sustavom radnje Mobile pomoću dostupna REST API-ji za stvaranje marketinške kampanje iz podataka sustava SharePoint. 
 
> [AZURE.IMPORTANT] Općenito govoreći, možete koristiti ovaj uzorak kao početnu točku za objašnjenje kako poziva bilo kojeg mobilnog radnje REST API-JA kao detalje o dva ključa aspekte svake API - parametri provjeru autentičnosti i prosljeđivanje poziva. 

## <a name="sharepoint-integration"></a>Integracija sustava SharePoint
1. Evo izgleda ogledni popis sustava SharePoint. **Naslov**, **kategorija**, **NotificationTitle**, **poruka** i **URL** se koriste za stvaranje objava. Postoji stupac pod nazivom **IsProcessed** koji se koristi proces Automatizacija uzorak u obrascu konzole programa. Možete pokretanje konzole programa kao programa Azure WebJob tako da ga možete zakazati ili izravno možete koristiti tijek rada sustava SharePoint u program stvaranja i aktiviranje priopćenja umetanju stavke na popis sustava SharePoint. U ovom primjeru koristimo konzole za program koji prolazi kroz stavki na popisu sustava SharePoint i stvaranje objava u Azure Mobile radnje za svaku od njih, a zatim na kraju Označi **IsProcessed** zastavice true na stvaranje uspješno objava.

    ![][1]

2. Ne možemo su pomoću koda iz oglednog *udaljene provjere autentičnosti u sustavu SharePoint Online pomoću klijentski objektni Model* [ovdje](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) za provjeru s popisa sustava SharePoint.
 
3. Nakon provjere autentičnosti smo prolazi kroz stavke popisa da biste saznali sve novostvorenu stavke (koji će imati **IsProcessed** = false). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Integracija Mobile radnje
1.  Nakon što smo pronašli stavke koji zahtijeva obradu – ne možemo izdvojiti podatke potrebne za stvaranje objava iz stavke popisa i poziv `CreateAzMECampaign` ga stvoriti i naknadno `ActivateAzMECampaign` da biste ga aktivirali. To su zapravo REST API-JA pozive pozivanje pozadinskog Mobile radnje. 

2.  REST API-ji radnje Mobile zahtijevaju **osnovne provjere autentičnosti shemu autorizacije HTTP zaglavlja** koji se sastoji od na `ApplicationId` i `ApiKey` koje dobivate s portala za Azure. Provjerite koristite li tipku iz odjeljka **api tipke** , a *ne* u odjeljku **sdk tipke** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Za stvaranje objava upišite kampanje – [dokumentacije](https://msdn.microsoft.com/library/azure/mt683750.aspx). Morate da biste bili sigurni da su navodeći kampanje `kind` kao *Objava* i [tereta](https://msdn.microsoft.com/library/azure/mt683751.aspx) i prosljeđivanje kao FormUrlEncodedContent. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Nakon što dodate objava stvorili, vidjet ćete otprilike ovako portala za mobilne radnje (Imajte na umu da stanje = skica i aktivirano = n/d)

    ![][3]

5. `CreateAzMECampaign`stvara se objava kampanjom i vraća njegov Id pozivatelja. `ActivateAzMECampaign`potreban je taj Id kao parametar da biste aktivirali kampanja. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Nakon što dodate objava aktivirati, vidjet ćete otprilike ovako portala za mobilne radnje:

    ![][4]

7. Čim kampanje dobiva aktiviran, počet će bilo kojeg uređaja koji zadovoljavaju kriterij na kampanje prikazuju obavijesti. 

8. Primijetit ćete da stavke popisa koji je označen IsProcessed = false postavljen na True nakon stvaranja kampanje objava.  

Ovaj primjer stvorili jednostavan objava kampanjom navodeći uglavnom odgovarajuća svojstva. Ne možete prilagoditi koliko god možete s portala sustava pomoću podataka [ovdje](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
