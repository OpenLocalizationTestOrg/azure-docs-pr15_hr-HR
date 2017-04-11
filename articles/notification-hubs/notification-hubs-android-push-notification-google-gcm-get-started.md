<properties
    pageTitle="Automatske obavijesti da pošaljete Android s koncentratorima obavijesti Azure | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču, Saznajte kako koristiti Azure obavijesti koncentratora za slanje obavijesti za uređaje sa sustavom Android."
    services="notification-hubs"
    documentationCenter="android"
    keywords="automatske obavijesti, automatske obavijesti, android automatske obavijesti"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Automatske obavijesti da pošaljete Android s koncentratorima obavijesti Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Pregled

> [AZURE.IMPORTANT] U ovoj se temi objašnjava automatske obavijesti s Google Cloud razmjenu poruka (GCM). Ako koristite Google Firebase oblaka razmjenu poruka (FCM), potražite u članku [Slanje slanje obavijesti za Android s koncentratorima Azure obavijesti i FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).

Pomoću ovog praktičnog vodiča pokazuje kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti programa sa sustavom Android.
Stvorit ćete prazne aplikacija za Android koji prima slanje obavijesti putem razmjene poruka Google Cloud (GCM).

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dovršeni kod za ovaj vodič možete preuzeti s GitHub [ovdje](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).


##<a name="prerequisites"></a>Preduvjeti

> [AZURE.IMPORTANT] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

Uz račun sustava active Azure spomenutih, pomoću ovog praktičnog vodiča samo zahtijeva najnoviju verziju sustava [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve obavijesti koncentratora vodiče za Android aplikacije.

##<a name="creating-a-project-that-supports-google-cloud-messaging"></a>Stvaranje projekta koji podržava Google Cloud poruka

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Konfiguriranje novi koncentrator obavijesti


[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. u plohu **Postavke** odaberite **Servise obavijesti** , a zatim **Google (GCM)**. Unesite ključ za API-JA, a zatim kliknite **Spremi**.

&emsp;&emsp;![Azure obavijesti koncentratora - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Koncentratora za obavijesti sada je konfiguriran za rad s GCM, a imate nizove veze i registrirati aplikacije da biste primanja i slanja automatske obavijesti.

##<a id="connecting-app"></a>Povezivanje aplikacije koncentrator obavijesti

###<a name="create-a-new-android-project"></a>Stvaranje novog projekta sa sustavom Android

1. U Android Studio započeti novi projekt Studio sa sustavom Android.

    ![Android Studio – novi projekt][13]

2. Odaberite obrazac faktor **mobitela i tableta** i **Minimum SDK** koju želite za podršku. Zatim kliknite **Dalje**.

    ![Android Studio - tijek rada za stvaranje projekta][14]

3. Odaberite **Prazan aktivnosti** glavni aktivnosti, kliknite **Dalje**, a zatim **Završi**.

###<a name="add-google-play-services-to-the-project"></a>Dodavanje servisa Google Play projekta

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Dodavanje biblioteke Azure obavijesti koncentratora


1. U na `Build.Gradle` datoteka za **aplikaciju**, dodajte sljedeće retke u odjeljku **ovisnosti** .

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Dodajte sljedeće spremište nakon odjeljka **ovisnosti** .

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>Ažuriranje na AndroidManifest.xml.


1. Za podršku GCM, ne možemo morate provesti ga slušatelj servis za Instance ID u našem kod koji se koristi za [nabaviti Registracija tokeni](https://developers.google.com/cloud-messaging/android/client#sample-register) pomoću [Google instancu ID API -JA](https://developers.google.com/instance-id/). U ovom ćete praktičnom vodiču smo će naziv klase `MyInstanceIDService`. 
 
    Dodajte sljedeću definiciju servisa datoteku AndroidManifest.xml unutar na `<application>` oznaka. Zamjena na `<your package>` rezervirano mjesto s na naziv stvarni paketa prikazani pri vrhu na `AndroidManifest.xml` datoteke.

        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>


2. Nakon što smo ste primili naše GCM Registracija tokena iz Instance ID API, koristit ćemo ga da biste [registrirali s obavijesti o središtu Azure](notification-hubs-push-notification-registration-management.md). Će podržavamo ovaj registracija u pozadini pomoću programa `IntentService` pod nazivom `RegistrationIntentService`. Bit će odgovoran za [Osvježavanje naš token Registracija GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens)taj servis.
 
    Dodajte sljedeću definiciju servisa datoteku AndroidManifest.xml unutar na `<application>` oznaka. Zamjena na `<your package>` rezervirano mjesto s na naziv stvarni paketa prikazani pri vrhu na `AndroidManifest.xml` datoteke. 

        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>



3. Ne možemo će definirati tekstnog okvira da biste primali obavijesti. Dodavanje tekstnog okvira definiciju sljedeće datoteku AndroidManifest.xml unutar na `<application>` oznaka. Zamjena na `<your package>` rezervirano mjesto s na naziv stvarni paketa prikazani pri vrhu na `AndroidManifest.xml` datoteke.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Dodajte sljedeće potrebne GCM povezane dozvola u nastavku na `</application>` oznaka. Provjerite je li da biste zamijenili `<your package>` s paketa ime koje se prikazuje pri vrhu na `AndroidManifest.xml` datoteku.

    Dodatne informacije o tih dozvola potražite u članku [Postavljanje GCM klijentskih aplikacija za Android](https://developers.google.com/cloud-messaging/android/client#manifest).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>


### <a name="adding-code"></a>Dodavanje koda


1. U prikaz projekta proširite **aplikacije** > **src** > **glavni** > **java**. Desnom tipkom miša kliknite mapu paketa u odjeljku **java**, kliknite **Novo**, a zatim kliknite **Klase jezika Java**. Dodavanje novog klase pod nazivom `NotificationSettings`. 

    ![Android Studio – novi klase jezika Java][6]

    Provjerite jeste li ažurirati ta tri rezerviranih mjesta u sljedeći kod za na `NotificationSettings` klasa:
    * **SenderId**: broj projekta koji ste nabavili ranije u [Google Cloud konzole](http://cloud.google.com/console).
    * **HubListenConnectionString**: **DefaultListenAccessSignature** niz za povezivanje za vaše središte. Klikom na **Pravilnike za pristup** plohu **Postavke** od vašeg koncentrator za [Portal za Azure]možete kopirati taj niz za povezivanje.
    * **HubName**: koriste naziv vaše središte obavijesti koji se prikazuje u plohu koncentrator za [Azure Portal].

    `NotificationSettings`kod:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Your default listen connection string>";
        }

2. Pomoću gore navedene korake dodavanje drugi novi klase pod nazivom `MyInstanceIDService`. To će biti naš implementaciji servisa za ga slušatelj Instance ID.

    Kod za ovu klasu podići naše `IntentService` [GCM token osvježavanje](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) u pozadini.

        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;
        
        
        public class MyInstanceIDService extends InstanceIDListenerService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.i(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Dodajte drugi novi predmete projekta pod nazivom, `RegistrationIntentService`. To će biti implementacije za naše `IntentService` koji će se obrađuju [Osvježavanje GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) i [registrirate za središtu obavijesti](notification-hubs-push-notification-registration-management.md).

    Koristite sljedeći kod za ovu klasu.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
        
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {      
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
        
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {      
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting to register with NH using token : " + token);

                        regID = hub.register(token).getRegistrationId();

                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();

                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);       
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete token refresh", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. U vašem `MainActivity` klase, dodajte sljedeće `import` izjave iznad deklariranje predmete.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. Dodajte sljedeće članove privatno pri vrhu klasu. Koristit ćemo te [Provjera dostupnosti Google Play s servisi kao preporučuje Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. U vašem `MainActivity` klase, dodajte na sljedeći način dostupnost Google Play s servisi. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. U vašem `MainActivity` klase, dodajte sljedeći kod koji će Google Play s servisi prije provjerite zovete vaše `IntentService` da biste svoju registraciju GCM tokena pa morate registrirati koncentratora za obavijesti.

        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
    
            if (checkPlayServices()) {
                // Start IntentService to register this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. U na `OnCreate` način u `MainActivity` klase, dodajte sljedeći kod da biste pokrenuli postupak registracije prilikom stvaranja aktivnosti.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Dodavanje dodatnih metode u `MainActivity` da biste provjerili status stanje i izvješća aplikaciju u aplikacije.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. Na `ToastNotify` metoda koristi u *"Pozdrav svijeta"* `TextView` status izvješća i obavijesti o persistently u aplikaciji kontrole. U izgled activity_main.xml, dodajte sljedeći id za tu kontrolu.

        android:id="@+id/text_hello"

11. Dodat ćemo podklasa za naše tekstnog okvira definirali na AndroidManifest.xml dalje. Dodajte drugi novi predmete projekta pod nazivom `MyHandler`.

12. Dodajte sljedeće naredbe uvoz pri vrhu `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Dodati sljedeći kod za na `MyHandler` predmete upućivanje podklase od `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Nadjačava kod u `OnReceive` način, pa rukovatelj prijavljuje obavijesti koji su primili. Rukovatelj šalje automatske obavijesti upravitelju obavijesti sa sustavom Android pomoću sustava `sendNotification()` način. Na `sendNotification()` način se izvršava kada aplikacija nije pokrenut, a zatim je primili obavijest.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. U Android Studio na traci izbornika kliknite **Sastavljanje** > **Ponovno stvaranje projekta** da biste bili sigurni da ne postoje pogreške u kodu.

##<a name="sending-push-notifications"></a>Slanje automatskih obavijesti

Možete testirati primanje automatske obavijesti u svojoj aplikaciji tako da im pošaljete putem [Portala za Azure] - potražite u odjeljku **Otklanjanje poteškoća** plohu koncentrator kao što je prikazano u nastavku.

![Slanje obavijesti Azure koncentratora - Test](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Neobavezno) Automatske obavijesti poslati izravno iz aplikacije

Obično bi slanje obavijesti putem pozadinskog poslužitelja. Za nekim slučajevima možda ćete moći poslati automatske obavijesti izravno iz klijentske aplikacije. U ovom se odjeljku objašnjava kako slanje obavijesti putem klijentskog programa pomoću [Azure obavijesti koncentrator REST API -JA](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. U Android prikaz projekta Studio proširite **aplikacije** > **src** > **glavni** > **res** > **izgled**. Otvaranje u `activity_main.xml` datoteke izgleda i kliknite karticu **tekst** da biste ažurirali tekstni sadržaj datoteke. Ažurirati s kodom u nastavku, čime se dodaju novi `Button` i `EditText` kontrole za slanje automatske obavijesti o središtu obavijesti. Dodavanje kod pri dnu neposredno prije `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. U Android prikaz projekta Studio proširite **aplikacije** > **src** > **glavni** > **res** > **vrijednosti**. Otvaranje u `strings.xml` datoteku i dodavanje vrijednosti niza koje se pozivaju na novu `Button` i `EditText` kontrole. Dodavanje te pri dnu datoteku, neposredno prije `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. U vašem `NotificationSetting.java` datoteke, dodajte sljedeće postavke da biste na `NotificationSettings` predmete.

    Ažuriranje `HubFullAccess` s nizom za povezivanje **DefaultFullSharedAccessSignature** za vaše središte. Niz za povezivanje mogu kopirati s [Portala za Azure] tako da kliknete **Pravilnike za pristup** na plohu **Postavke** za koncentratora za obavijesti.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";

4. U vašem `MainActivity.java` datoteka, dodajte sljedeće `import` izjave iznad u `MainActivity` predmete.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. U vašem `MainActivity.java` datoteke, dodajte sljedeće članove pri vrhu od na `MainActivity` predmete.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. Morate stvoriti potpis programa Access (SaS) token za provjeru autentičnosti zahtjeva za objavu slanje poruka koncentratora za obavijesti. To se postiže Raščlanjivanje ključnih podataka iz niza za povezivanje i stvaranje tokena SaS, kao što je rečeno u referenci REST API-JA [Uobičajenih koncepata](http://msdn.microsoft.com/library/azure/dn495627.aspx) . Sljedeći kod je primjeru implementacija.

    U `MainActivity.java`, dodajte na sljedeći način u `MainActivity` predmete raščlaniti niz za povezivanje.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. U `MainActivity.java`, dodajte na sljedeći način u `MainActivity` klase za stvaranje tokena za provjeru autentičnosti SaS.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. U `MainActivity.java`, dodajte na sljedeći način u `MainActivity` predmete rukovati kliknite gumb za **Slanje obavijesti** i poslati obavijest automatske središtu pomoću ugrađenih REST API-JA.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Testiranje aplikacije

####<a name="push-notifications-in-the-emulator"></a>Automatske obavijesti u na emulator

Ako želite testirati automatske obavijesti unutar programa emulator provjerite podržava li sliku emulator razinu Google API koju ste odabrali za aplikacije. Ako sliku ne podržava nativni Google API-ji, koji će božićnim u **SERVISA\_ne\_dostupno** iznimke.

Osim navedenog, provjerite je li koji ste dodali račun za Google izvodi Emulator-a u odjeljku **Postavke** > **računa**. U suprotnom može uzrokovati pokušava morate registrirati GCM na **provjere autentičnosti\_nije uspjelo** iznimke.

####<a name="running-the-application"></a>Pokretanje aplikacije

1. Pokrenite aplikaciju i obratite pozornost ID Registracija prijavljuje za uspješan registraciju.

    ![Testiranje na Android - Registracija kanala][18]

2. Unesite poruku na svim uređajima sa sustavom Android koji ste registriran u središtu.

    ![Testiranje na Android - slanja poruke][19]

3. Pritisnite **Slanje obavijesti**. Prikazat će se sve uređaje koji se izvodi aplikacija programa `AlertDialog` instanca s automatske obavijesti. Uređaje koji nemaju aplikaciju pokrenut, ali su prethodno registrirani za slanje obavijesti će primiti obavijest u upravitelju obavijesti Android. One moguće je prikazati tako da prstom prijeđete prema dolje u gornjem lijevom kutu.

    ![Testiranje na sa sustavom Android – obavijesti][21]

##<a name="next-steps"></a>Daljnji koraci

Praktični vodič za [Korištenje obavijesti koncentratora za slanje obavijesti korisnicima] preporučujemo kao u sljedećem koraku. Ga će vam pokazati kako slanje obavijesti iz ASP.NET pozadinskog sustava pomoću oznaka ciljnih određene korisnike.

Ako želite fazi korisnika po grupama kamatu, pogledajte vodič za [Korištenje koncentratora obavijesti da biste poslali važnih vijesti] .

Dodatne općenite informacije o koncentratora obavijesti potražite u članku naše [Obavijesti koncentratora upute].

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Upute za koncentratora obavijesti]: http://msdn.microsoft.com/library/jj927170.aspx
[Automatske obavijesti korisnicima pomoću obavijesti koncentratora]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Portal za Azure]: https://portal.azure.com
