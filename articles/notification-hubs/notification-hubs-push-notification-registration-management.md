<properties
    pageTitle="Upravljanje Registracija"
    description="U ovoj se temi objašnjava kako registrirati uređaja s koncentratorima obavijest da bi se primajte automatske obavijesti."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Upravljanje Registracija

##<a name="overview"></a>Pregled

U ovoj se temi objašnjava kako registrirati uređaja s koncentratorima obavijest da bi se primajte automatske obavijesti. U temi opisuju registracije visoke razine, a zatim predstavlja dva glavna uzoraka za registriranje uređaje: prijava s uređaja izravno u središtu obavijesti i registracija putem programa pozadinska aplikacija. 


##<a name="what-is-device-registration"></a>Što je registracija uređaja

Registracija uređaja s obavijesti koncentrator se postiže pomoću **Registracija** ili **instalacije**.

#### <a name="registrations"></a>Registracija
Na Registracija pridružuje ručicu za servis obavijesti na platformi (PNS) za uređaj oznake i vjerojatno predloška. Držač za PNS može biti ChannelURI, token uređaja ili id Registracija GCM. Oznake koriste se za usmjeravanje obavijesti u odgovarajući skup od ručica za uređaj. Dodatne informacije potražite u članku [usmjeravanje i oznaka izraza](notification-hubs-tags-segment-push-message.md). Predlošci se koriste za implementaciju po Registracija transformaciju. Dodatne informacije potražite u članku [Predlošci](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Instalacija
Instalacija je je Poboljšana svojstva povezanih Registracija koja obuhvaća Torba od automatske. To je pristup najnovijim i najbolje Registracija uređaja. Međutim, nije podržan tako da na strani klijenta .NET SDK ([SDK koncentrator obavijesti pozadinskog operacije](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) od još.  To znači da ako su Registracija iz klijentskog uređaja sam, promijenile da biste koristili pristup [Obavijesti koncentratora REST API -JA](https://msdn.microsoft.com/library/mt621153.aspx) za podršku instalacije. Ako koristite uslugu pozadinskog, moći koristiti [Obavijesti SDK koncentrator za pozadinskog operacije](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Ovo su neke ključne prednosti korištenja instalacije:

* Stvaranje ili ažuriranje instalacije su u potpunosti idempotent. Možete ga pa ponovno bez bilo kojeg razloga duplicirane registracije.
* Model instalacije olakšava učinite pojedinačnih ih gura - ciljanja određeni uređaj. Oznake sustava **"$InstallationId: [installationId]"** automatski se dodaju s svaki Registracija instalacije temelji. Tako da biste uputili poziv Pošalji u ovoj oznaci ciljne određeni uređaj bez potrebe za dodatne kodiranje.
* Korištenje instalacija omogućuje vam da biste učinili Registracija djelomičnog ažuriranja. Djelomično ažurirati Instalacija je zatraženo metodom ZAKRPU pomoću [standardne JSON zakrpu](https://tools.ietf.org/html/rfc6902). Ovo je osobito je korisna kad želite ažurirati oznake na registracije. Ne morate padajući cijelu Registracija i zatim ponovno prethodne oznake.

Instalacija može sadržavati na sljedeća svojstva. Cjelovit popis instalacije svojstva potražite u članku, [Stvaranje ili Prebriši instalacija s REST API -JA](https://msdn.microsoft.com/library/azure/mt621153.aspx) ili [Svojstva za instalaciju](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) sustava.

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

Važno Imajte na umu da Registracija i instalacije po zadanom više istječe je.

Registracija i instalacije mora sadržavati valjani pokazivač PNS za svaki uređaj/kanal. Jer PNS ručice je moguće dohvatiti samo u klijentskom aplikaciji na uređaju, jedan uzorak je da biste registrirali izravno na tom uređaju pomoću aplikacije za klijenta. S druge strane, sigurnosna pitanja vezana uz i poslovne logike vezane uz oznake ćete možda morati upravljanje Registracija uređaja u aplikaciji pozadinske. 

#### <a name="templates"></a>Predlošci

Ako želite koristiti [predloške](notification-hubs-templates-cross-platform-push-messages.md)instalacija uređaja i držite sve predloške povezan s uređaja u na JSON oblikovanje (pogledajte primjer iznad). Nazivi predložaka pomoći cilj različite predloške za istom uređaju.

Imajte na umu da svaki naziv predloška karte tijelo predloška i neobavezna skup oznaka. Nadalje, svaki platformu može imati predložak dodatna svojstva. Iz Windows trgovine (pomoću WNS) i Windows Phone 8 (pomoću MPNS), dodatni skup zaglavlja može biti dio predloška. U slučaju APN-ove, možete postaviti svojstva u programu isteka ili konstanta ili izraz za predložak. Cjelovit popis instalacije svojstva sintaksi, [Stvaranje ili Prebriši instalacija s OSTATKOM](https://msdn.microsoft.com/library/azure/mt621153.aspx) .

#### <a name="secondary-tiles-for-windows-store-apps"></a>Sekundarni pločice za aplikacije iz Windows trgovine

Za klijentske aplikacije iz Windows trgovine, slanje obavijesti sekundarne pločice je isti kao slanjem primarni jedan. Ovo je podržano i u instalacije. Imajte na umu da sekundarne pločice imaju različite ChannelUri koji SDK na aplikaciju klijent rukuje proziran.

Rječnik SecondaryTiles koristi iste TileId koji se koristi za stvaranje SecondaryTiles objekta u aplikaciji iz Windows trgovine.
Kao i kod primarni ChannelUri ChannelUris sekundarne pločica možete promijeniti u bilo kojem trenutku. Da biste zadržali instalacije u središtu obavijest ažuriranja, na uređaju morate osvježiti ih s trenutnom ChannelUris sekundarne pločica.


##<a name="registration-management-from-the-device"></a>Upravljanje Registracija s uređaja

Kada upravljanje Registracija uređaja s klijentskim aplikacijama, pozadinski se samo odgovoran za slanje obavijesti. Aplikacije klijenta PNS ručice Ostanite u tijeku i registrirati oznake. Sljedeća slika prikazuje ovaj uzorak.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Uređaj najprije dohvaća ručicu PNS iz u PNS, a zatim registrira izravno s središtu obavijesti. Nakon registracije ne uspije, pozadinska aplikacija možete poslati obavijest ciljanja te registraciju. Dodatne informacije o načinu slanja obavijesti potražite u članku [usmjeravanje i oznaka izraza](notification-hubs-tags-segment-push-message.md).
Imajte na umu da u tom slučaju će koristiti samo poslušali prava pristupa na koncentratora obavijesti s uređaja. Dodatne informacije potražite u članku [Sigurnost](notification-hubs-push-notification-security.md).

Registracija s uređaja je najjednostavniji način, ali ima nekoliko Nedostaci.
Prvi drawback je da neku aplikaciju za klijenta možete samo ažurirati njegove oznake kada je aplikacija aktivna. Na primjer, ako korisnik ima dvije uređaje koji se registrirati oznake koje se odnose na timovima sport pri prvom uređaju registrira za dodatne oznaku (na primjer, Seahawks), drugi uređaj neće primiti obavijesti o na Seahawks dok aplikaciju na drugom uređaju se izvršava drugi put. Više je Općenito govoreći, kada oznake utječe više uređaja, upravljanje oznake iz pozadinski je poželjno mogućnost.
Drugi drawback upravljanja Registracija iz aplikacije klijenta je da, budući da aplikacije mogu biti hacked, zaštita Registracija oznake specifične za zahtijeva dodatni pažnju, kao što je opisano u odjeljku "sigurnosti na razini oznake".



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Primjer koda za registriranje koncentrator obavijesti na uređaju koristiti u instalaciji 

Trenutku, to je podržana samo pomoću [Obavijesti koncentratora REST API -JA](https://msdn.microsoft.com/library/mt621153.aspx).

Možete koristiti i metodu ZAKRPU pomoću [standardne JSON zakrpu](https://tools.ietf.org/html/rfc6902) ažuriranja instalaciju.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Primjer kod da biste registrirali s obavijesti koncentrator s uređaja koji se koristi u Registracija


Načini stvaranje ili ažuriranje Registracija uređaja na kojem se nazivaju. To znači da da biste ažurirali ručicu ili oznake, morate prebrisati cijelu registracije. Imajte na umu da registracije tranzitne, da biste trebali biste uvijek pouzdanog store pomoću trenutne oznake koju je potrebno za određeni uređaj.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Upravljanje Registracija iz programa pozadine

Upravljanje registracije iz pozadinski zahtijeva pisanje dodatni kod. Aplikaciju s uređaja morate omogućiti ažurirane PNS pokazivač da bi se pozadinski prilikom svakog pokretanja aplikacije (zajedno s oznakama i predloške) i pozadinski morate ažurirati tom pokazivaču na središte obavijesti. Sljedeća slika prikazuje ovaj dizajn.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Prednosti upravljanje registracije iz pozadinski obuhvaćaju mogućnost da biste izmijenili oznake da biste registracije čak i kad odgovarajuću aplikaciju na uređaju nije aktivna, a za provjeru autentičnosti aplikaciju klijenta prije dodavanja oznake njegov Registracija.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Primjer koda za registriranje koncentrator obavijesti iz pozadine koristiti u instalaciji

Klijentskog uređaja i dalje dobiva njegov PNS ručicu i odgovarajuće instalacije svojstva kao prije i poziva prilagođene API na pozadinskog koje možete provesti Registracija i autorizirali oznake itd. Pozadinski omogućuje korištenje [SDK koncentrator obavijesti pozadinskog operacija](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Možete koristiti i metodu ZAKRPU pomoću [standardne JSON zakrpu](https://tools.ietf.org/html/rfc6902) ažuriranja instalacije.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Primjer koda za registriranje koncentratora za obavijesti putem uređaja pomoću ID-a registracija

Iz pozadine vaše aplikacije možete izvršavati osnovne operacije CRUDS registracije. Ako, na primjer:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


Pozadinski morate učiniti istodobnosti između Registracija ažuriranja. Servis Bus nudi optimistična Procjena istodobnosti kontrole za upravljanje registracije. Na razini HTTP-a to je implementirana uz pomoć e-oznake za registraciju upravljanje operacije. Microsoft SDKs koji vratiti iznimku ako ažuriranje Odbaci razloga istodobnosti proziran koriste tu značajku. Pozadinska aplikacija je zadužen za obradu tih iznimke i ponovni ažuriranje po potrebi.