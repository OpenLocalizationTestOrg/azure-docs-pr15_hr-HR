U ovom ćete odjeljku ažurirate kod u postojeći projekt pozadinskog mobilne aplikacije za slanje automatske obavijesti svaki put kada se doda nova stavka. Time se pokreće značajka [predložaka](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) koncentratora obavijesti, omogućivanje ih gura različite platforme. Klijenti za različite registrirane za slanje obavijesti s predlošcima i jedan univerzalni automatske možete pristupiti svim klijentskim platformama.

Odaberite postupak koji zadovoljava vaše vrsta projekta pozadinski&mdash; [Pozadinski .NET](#dotnet) ili [Node.js pozadinski](#nodejs).

### <a name="dotnet"></a>.NET pozadinskog projekta
1. U Visual Studio, desnom tipkom miša kliknite server project i kliknite **Upravljanje NuGet paketa**, potražite `Microsoft.Azure.NotificationHubs`, zatim kliknite **Instaliraj**. To se instalira biblioteku obavijesti koncentratora za slanje obavijesti iz svoje pozadine.

3. U programu project server otvorite **kontrolera** > **TodoItemController.cs**i dodajte sljedeće pomoću naredbe:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. U načinu **PostTodoItem** nakon poziv **InsertAsync**dodati sljedeći kod:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    To će poslati primatelju obavijest predložak koji sadrži stavku. Tekst umetanju nove stavke.

4. Ponovno objaviti server project. 

### <a name="nodejs"></a>Node.js pozadinskog projekta

1. Ako ste još niste učinili, [Preuzmite pozadinskog projekta brzi početak rada](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) jer inače koristite [online editor na portalu za Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Zamijenite postojeće kod u todoitem.js sljedeće:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    To će poslati primatelju obavijest predložak koji sadrži na item.text umetanju nove stavke.

2. Prilikom uređivanja datoteka na lokalnom računalu, ponovno objaviti server project.
