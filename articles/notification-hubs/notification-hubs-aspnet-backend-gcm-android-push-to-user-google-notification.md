<properties
    pageTitle="Azure obavijesti koncentratora obavijestite korisnike za Android s pozadinskom .NET"
    description="Saznajte kako automatske obavijesti poslati korisnicima u Azure. Primjere koda pisane Java za Android"
    documentationCenter="android"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a>Azure obavijesti koncentratora obavijestite korisnike za Android s pozadinskom .NET


[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

##<a name="overview"></a>Pregled

Automatske obavijesti podrška u Azure omogućuje vam pristup je jednostavno korištenja, multiplatform i automatske neproporcionalno izlaz infrastrukture koji za komercijalni ispis olakšava implementacije slanje obavijesti za korisnika i enterprise aplikacije za mobilne platforme. Pomoću ovog praktičnog vodiča pokazuje kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti na određenu aplikaciju korisnika na određeni uređaj. U ASP.NET WebAPI pozadinskog služi za provjeru autentičnosti klijente i za generiranje obavijesti, kao što je prikazano u temi smjernice [Registracija iz vaše aplikacije pozadine](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Pomoću ovog praktičnog vodiča sastavlja na središtu obavijesti koji ste stvorili u ovom praktičnom vodiču za [Početak rada s obavijesti koncentratora (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) .

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča pretpostavlja da ste stvorili i konfigurirali koncentratora za obavijesti, kao što je opisano u [Početak rada s obavijesti koncentratora (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a>Stvaranje projekta za Android

Sljedeći je korak da biste stvorili aplikacija sa sustavom Android.

1. Slijedite vodič [Početak rada s obavijesti koncentratora (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) za stvaranje i konfiguriranje aplikacije da biste primajte automatske obavijesti iz GCM.

2. Otvorite datoteku **res/layout/activity_main.xml** , zamijenite na sa sljedećim definicije sadržaja.

    Time se dodaje nove EditText kontrole za prijava u ulozi korisnika. Polja i dodaje se za oznaku korisničko ime koje će biti dio obavijesti šaljete:

        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
            android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

        <EditText
            android:id="@+id/usernameText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/usernameHint"
            android:layout_above="@+id/passwordText"
            android:layout_alignParentEnd="true" />
        <EditText
            android:id="@+id/passwordText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/passwordHint"
            android:inputType="textPassword"
            android:layout_above="@+id/buttonLogin"
            android:layout_alignParentEnd="true" />
        <Button
            android:id="@+id/buttonLogin"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/loginButton"
            android:onClick="login"
            android:layout_above="@+id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:layout_marginBottom="24dp" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="WNS on"
            android:textOff="WNS off"
            android:id="@+id/toggleButtonWNS"
            android:layout_toLeftOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="GCM on"
            android:textOff="GCM off"
            android:id="@+id/toggleButtonGCM"
            android:checked="true"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="APNS on"
            android:textOff="APNS off"
            android:id="@+id/toggleButtonAPNS"
            android:layout_toRightOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessageTag"
            android:layout_below="@id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_tag_hint" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessage"
            android:layout_below="@+id/editTextNotificationMessageTag"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_hint" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/send_button"
            android:id="@+id/sendbutton"
            android:onClick="sendNotificationButtonOnClick"
            android:layout_below="@+id/editTextNotificationMessage"
            android:layout_centerHorizontal="true" />
        </RelativeLayout>



3. Otvorite datoteku **res/values/strings.xml** i zamijeni u `send_button` definiciju s sadržan redefinirati njihove niz za na `send_button` i dodavanje nizova za ostale kontrole:

        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>

    Grafički izgled main_activity.xml trebala izgledati ovako:

    ![][A1]

4. Stvaranje nove predmete pod nazivom **RegisterClient** u paketu isti kao vaše `MainActivity` predmete. Pomoću koda ispod nove datoteke predmete.

        import java.io.IOException;
        import java.io.UnsupportedEncodingException;
        import java.util.Set;

        import org.apache.http.HttpResponse;
        import org.apache.http.HttpStatus;
        import org.apache.http.client.ClientProtocolException;
        import org.apache.http.client.HttpClient;
        import org.apache.http.client.methods.HttpPost;
        import org.apache.http.client.methods.HttpPut;
        import org.apache.http.client.methods.HttpUriRequest;
        import org.apache.http.entity.StringEntity;
        import org.apache.http.impl.client.DefaultHttpClient;
        import org.apache.http.util.EntityUtils;
        import org.json.JSONArray;
        import org.json.JSONException;
        import org.json.JSONObject;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.util.Log;

        public class RegisterClient {
            private static final String PREFS_NAME = "ANHSettings";
            private static final String REGID_SETTING_NAME = "ANHRegistrationId";
            private String Backend_Endpoint;
            SharedPreferences settings;
            protected HttpClient httpClient;
            private String authorizationHeader;

            public RegisterClient(Context context, String backendEnpoint) {
                super();
                this.settings = context.getSharedPreferences(PREFS_NAME, 0);
                httpClient =  new DefaultHttpClient();
                Backend_Endpoint = backendEnpoint + "/api/register";
            }

            public String getAuthorizationHeader() {
                return authorizationHeader;
            }

            public void setAuthorizationHeader(String authorizationHeader) {
                this.authorizationHeader = authorizationHeader;
            }

            public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
                String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);

                JSONObject deviceInfo = new JSONObject();
                deviceInfo.put("Platform", "gcm");
                deviceInfo.put("Handle", handle);
                deviceInfo.put("Tags", new JSONArray(tags));

                int statusCode = upsertRegistration(registrationId, deviceInfo);

                if (statusCode == HttpStatus.SC_OK) {
                    return;
                } else if (statusCode == HttpStatus.SC_GONE){
                    settings.edit().remove(REGID_SETTING_NAME).commit();
                    registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                    statusCode = upsertRegistration(registrationId, deviceInfo);
                    if (statusCode != HttpStatus.SC_OK) {
                        Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                        throw new RuntimeException("Error upserting registration");
                    }
                } else {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            }

            private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                    throws UnsupportedEncodingException, IOException,
                    ClientProtocolException {
                HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
                request.setEntity(new StringEntity(deviceInfo.toString()));
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                request.addHeader("Content-Type", "application/json");
                HttpResponse response = httpClient.execute(request);
                int statusCode = response.getStatusLine().getStatusCode();
                return statusCode;
            }

            private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
                if (settings.contains(REGID_SETTING_NAME))
                    return settings.getString(REGID_SETTING_NAME, null);

                HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                HttpResponse response = httpClient.execute(request);
                if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                    throw new RuntimeException("Error creating Notification Hubs registrationId");
                }
                String registrationId = EntityUtils.toString(response.getEntity());
                registrationId = registrationId.substring(1, registrationId.length()-1);

                settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();

                return registrationId;
            }
        }

    Komponente implementira OSTALE pozive morati obratiti pozadinskog aplikacije da bi se registrirati za slanje obavijesti. Lokalno i sprema *registrationIds* stvorenu u središtu obavijesti kao detaljne u [Registracija iz vaše aplikacije pozadine](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Imajte na umu da koristi oznaku ovlaštenja pohranjene u lokalno spremište kada kliknete gumb **Prijava** .

5. U svoje `MainActivity` predmete uklonite ili komentar izvan vaše polje private za `NotificationHub`, i dodavanje polja za na `RegisterClient` predmete i niza za krajnju točku pozadinskog ASP.NET. Ne zaboravite da biste zamijenili `<Enter Your Backend Endpoint>` s na na krajnjoj točki stvarni pozadinskog nabavili prethodno. Na primjer, `http://mybackend.azurewebsites.net`.


        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


6. U svoje `MainActivity` klase, u na `onCreate` način, uklonite ili komentar izvan inicijalizaciju na `hub` polja i poziv na na `registerWithNotificationHubs` način. Dodavanje koda za inicijalizaciju instance komponente na `RegisterClient` predmete. Način mora sadržavati sljedeće retke:

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            MyHandler.mainActivity = this;
            NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
            gcm = GoogleCloudMessaging.getInstance(this);

            //hub = new NotificationHub(HubName, HubListenConnectionString, this);
            //registerWithNotificationHubs();

            registerClient = new RegisterClient(this, BACKEND_ENDPOINT);

            setContentView(R.layout.activity_main);
        }

7. U vašem `MainActivity` klase, brisanje ili komentar izvan čitavog `registerWithNotificationHubs` način. Ne će se koristiti u ovom ćete praktičnom vodiču.

8. Dodajte sljedeće `import` izjave **MainActivity.java** datoteku.

        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;


9. Zatim dodajte načina rukovanja gumb **prijavite se u** kliknite događaj i slanje automatskih obavijesti.

        @Override
        protected void onStart() {
            super.onStart();
            Button sendPush = (Button) findViewById(R.id.sendbutton);
            sendPush.setEnabled(false);
        }

        public void login(View view) throws UnsupportedEncodingException {
            this.registerClient.setAuthorizationHeader(getAuthorizationHeader());

            final Context context = this;
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(SENDER_ID);
                        registerClient.register(regid, new HashSet<String>());
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to register", e.getMessage());
                        return e;
                    }
                    return null;
                }

                protected void onPostExecute(Object result) {
                    Button sendPush = (Button) findViewById(R.id.sendbutton);
                    sendPush.setEnabled(true);
                    Toast.makeText(context, "Logged in and registered.",
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
            return basicAuthHeader;
        }

        /**
         * This method calls the ASP.NET WebAPI backend to send the notification message
         * to the platform notification service based on the pns parameter.
         *
         * @param pns     The platform notification service to send the notification message to. Must
         *                be one of the following ("wns", "gcm", "apns").
         * @param userTag The tag for the user who will receive the notification message. This string
         *                must not contain spaces or special characters.
         * @param message The notification message string. This string must include the double quotes
         *                to be used as JSON content.
         */
        public void sendPush(final String pns, final String userTag, final String message)
                throws ClientProtocolException, IOException {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {

                        String uri = BACKEND_ENDPOINT + "/api/notifications";
                        uri += "?pns=" + pns;
                        uri += "&to_tag=" + userTag;

                        HttpPost request = new HttpPost(uri);
                        request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                        request.setEntity(new StringEntity(message));
                        request.addHeader("Content-Type", "application/json");

                        HttpResponse response = new DefaultHttpClient().execute(request);

                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            DialogNotify("MainActivity - Error sending " + pns + " notification",
                                response.getStatusLine().toString());
                            throw new RuntimeException("Error sending notification");
                        }
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                        return e;
                    }

                    return null;
                }
            }.execute(null, null, null);
        }


    Na `login` rukovatelja gumb **prijaviti** generira Osnovna provjera autentičnosti token pomoću na unos korisničko ime i lozinka (obratite pozornost na to predstavlja sve token koristi shemu za provjeru autentičnosti), a zatim koristi `RegisterClient` da biste nazvali pozadinski za registraciju.

    Na `sendPush` način poziva pozadinskog tako da pokreću sigurne obavijest da biste korisnika na temelju oznaka korisnika. Obavijesti platformu servisa koji `sendPush` ovisi o ciljnih web-mjesta u `pns` proslijeđen niz.

10. U vašem `MainActivity` klase, ažuriranje na `sendNotificationButtonOnClick` način da biste uputili poziv na `sendPush` kojoj se korisnik odabrana metoda servisa obavijesti platformu na sljedeći način.

        /**
         * Send Notification button click handler. This method sends the push notification
         * message to each platform selected.
         *
         * @param v The view
         */
        public void sendNotificationButtonOnClick(View v)
                throws ClientProtocolException, IOException {

            String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                    .getText().toString();
            String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                    .getText().toString();

            // JSON String
            nhMessage = "\"" + nhMessage + "\"";

            if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
            {
                sendPush("wns", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
            {
                sendPush("gcm", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
            {
                sendPush("apns", nhMessageTag, nhMessage);
            }
        }



## <a name="run-the-application"></a>Pokrenite aplikaciju


1. Pokrenite aplikaciju na uređaj ili Emulator-a pomoću Studio sa sustavom Android.

2. U aplikaciji sa sustavom Android, unesite korisničko ime i lozinku. Moraju oba biti iste vrijednosti niza i oni ne smije sadržavati razmake ni posebne znakove.

3. U aplikaciji Android kliknite **prijavite se**. Pričekajte skočnoj poruku da **Evidentirani u i registrirani**. To će omogućiti gumb za **Slanje obavijesti** .

    ![][A2]

4. Kliknite klizač stavke gumbe da biste omogućili sve platforme gdje ste pokrenuli aplikaciju i registrirana korisnika.
5. Unesite korisničko ime koje će primiti obavijest. Korisnik mora biti registriran za obavijesti na uređajima cilj.

6. Unesite poruku za korisnika prima kao automatske obavijesti.
7. Kliknite **Slanje obavijesti**.  Svaki uređaj koji je registracija s odgovarajućom oznakom korisničko ime primit će automatske obavijesti.


[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
