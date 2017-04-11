Pomoću postupka koji odgovara vašem pozadinskog vrsta projekta&mdash; [pozadinskog .NET](#dotnet) ili [Node.js pozadinskog](#nodejs).

### <a name="dotnet"></a>.NET pozadinskog projekta

1. U Visual Studio, desnom tipkom miša kliknite server project i kliknite **Upravljanje NuGet paketa**, potražite `Microsoft.Azure.NotificationHubs`, zatim kliknite **Instaliraj**. Time se instalira klijentska biblioteka koncentratora obavijesti.

2. U mapi kontrolera otvorite TodoItemController.cs i dodajte sljedeće `using` izvješća:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;

3. Zamjena na `PostTodoItem` način sljedeći kod:  

      
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write the success result to the logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write the failure result to the logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Ponovno objaviti server project.

### <a name="nodejs"></a>Node.js pozadinskog projekta

1. Ako ste još niste učinili, [Preuzmite brzi početak rada projekta](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) jer inače koristite [online editor na portalu za Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
 
1. Zamijenite postojeće kod u datoteci todoitem.js sljedeće:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    To će poslati primatelju obavijest GCM koja sadrži na item.text umetanju novu stavku obveze. 

2. Prilikom uređivanja datoteka na lokalnom računalu, ponovno objaviti server project. 
