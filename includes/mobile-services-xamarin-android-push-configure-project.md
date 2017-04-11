
1. U rješenje prikazu (ili **Preglednik rješenja** u Visual Studio), desnom tipkom miša kliknite mapu **komponente** , kliknite **Dohvati dodatne komponente...**, potražite komponentu **Google Cloud poruka klijenta** i dodati u projekt.

2. Otvorite datoteku ToDoActivity.cs projekta i dodajte sljedeće pomoću naredbu klasa:

        using Gcm.Client;

3. U predmete **ToDoActivity** dodajte nove sljedeći kod: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Omogućuje pristup instancu mobilnu verziju klijentskog iz postupka automatske rukovatelj servisa.

4.  Dodajte sljedeći kod metodu **OnCreate** nakon stvaranja **MobileServiceClient** :

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

**ToDoActivity** sada spremni za dodavanje automatske obavijesti.