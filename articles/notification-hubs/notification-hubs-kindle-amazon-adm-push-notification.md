<properties
    pageTitle="Početak rada s Azure obavijesti koncentratora za aplikacije na uređaju Kindle | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču saznati kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti aplikacija na uređaju Kindle."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Početak rada s obavijesti koncentratora za aplikacije na uređaju Kindle

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča pokazuje kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti aplikacija na uređaju Kindle.
Stvorit ćete prilagođenom uređaju Kindle web-aplikacijom koja prima slanje obavijesti putem razmjene poruka Amazon uređaj (Admin).

##<a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ Pronađite Android SDK (smo pretpostavlja da će koristiti Eclipse) s <a href="http://go.microsoft.com/fwlink/?LinkId=389797">web-mjesta sa sustavom Android</a>.
+ Slijedite korake u <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Postavku kopije vaše okruženje za razvoj</a> da biste postavili razvojno okruženje za uređaju Kindle.

##<a name="add-a-new-app-to-the-developer-portal"></a>Dodavanje nove aplikacije portala za razvojne inženjere

1. Prvo, stvorite aplikaciju [Amazon portala za razvojne inženjere].

    ![][0]

2. Kopirajte **Aplikacijsku tipku**.

    ![][1]

3. Na portalu za kliknite naziv aplikacije, a zatim na kartici **Poruka uređaja** .

    ![][2]

4. Kliknite **Stvori novi profil za sigurnost**, a zatim stvorite novi profil sigurnosti (na primjer, **TestAdm sigurnosni profil**). Zatim kliknite **Spremi**.

    ![][3]

5. Kliknite **Sigurnosnih profila** da biste pogledali sigurnosni profil koji ste upravo stvorili. Kopirajte vrijednosti **ID klijenta** i **Tajna klijenta** za kasnije korištenje.

    ![][4]

## <a name="create-an-api-key"></a>Stvoriti ključ za API-JA

1. Otvorite naredbeni redak s administratorskim ovlastima.
2. Dođite do mape u Android SDK.
3. Unesite sljedeću naredbu:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  Lozinka **keystore** upišite **sa sustavom android**.

5.  Kopirajte **MD5** prstiju.
6.  Vratite se u portala za razvojne inženjere, na kartici **razmjena poruka** kliknite **Android/uređaju Kindle** unesite naziv paketa aplikacije (na primjer, **com.sample.notificationhubtest**) i **MD5** vrijednost i kliknite **Stvori ključ za API -JA**.

## <a name="add-credentials-to-the-hub"></a>Dodavanje vjerodajnice u središtu

Na portalu dodajte tajna klijenta i ID klijenta na karticu **Konfiguracija** koncentratora za obavijesti.

## <a name="set-up-your-application"></a>Postavljanje aplikacije

> [AZURE.NOTE] Kada stvarate aplikaciju, koristite najmanje 17 razinu API-JA.

Dodavanje biblioteka Admin Eclipse projekta:

1. Da biste dobili Admin biblioteke, [Preuzmite SDK]. Izdvajanje SDK zip datoteku.
2. U Eclipse, desnom tipkom miša kliknite projekt, a zatim kliknite **Svojstva**. Odaberite **Put sastavljanje Java** na lijevoj strani, a zatim na kartici **Biblioteka **na vrhu. Kliknite **Dodavanje vanjskog posudu**i odaberite datoteku `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` iz imenika dobivenih Amazon SDK.
3. Preuzmite Android SDK NotificationHubs (veza).
4. Raspakiraj paket, a zatim povucite datoteku `notification-hubs-sdk.jar` u na `libs` mape u Eclipse.

Uređivanje vaše manifest aplikacije za podršku Admin:

1. Dodavanje prostora za naziv Amazon u korijenskom elementu manifesta:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Dodajte dozvole kao prvi element u odjeljku manifesta element. **[Naziv vaše PAKETA]** zamijeniti paket koji ste koristili za stvaranje aplikacije.

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Umetnite sljedeći element kao prvi podređeni element aplikacije. Ne zaboravite zamijeniti **[Naziv vaše usluge]** s nazivom svoje rukovatelj Admin poruke koje ste stvorili u sljedećem odjeljku (uključujući paketa), a **[Naziv vaše PAKETA]** zamijenite naziv paketa koji ste stvorili aplikacije.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Stvaranje vaše rukovatelj Admin poruke

1. Stvaranje nove predmete koji nasljeđuju od `com.amazon.device.messaging.ADMMessageHandlerBase` i nazovite ih `MyADMMessageHandler`, kao što je prikazano na sljedećoj slici:

    ![][6]

2. Dodajte sljedeće `import` izvješća:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Dodajte sljedeći kod u razredu koji ste stvorili. Imajte na umu zamjenu koncentrator naziv i veze niza (preslušavanja):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Dodati sljedeći kod da biste na `OnMessage()` metoda:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Dodati sljedeći kod da biste na `OnRegistered` metoda:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Dodati sljedeći kod da biste na `OnUnregistered` metoda:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. U na `MainActivity` metoda dodajte sljedeća naredba uvoz:

        import com.amazon.device.messaging.ADM;

8. Dodati sljedeći kod na kraju na `OnCreate` metoda:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Dodajte ključ za API na aplikaciju

1. U Eclipse, stvorite novu datoteku pod nazivom **api_key.txt** u imenik imovine projekta.
2. Otvorite datoteku i kopirajte ključ za API koji je generirao na portalu za razvojne inženjere Amazon.

## <a name="run-the-app"></a>Pokrenite aplikaciju

1. Započnite s emulator.
2. U emulator, prijeđite prstom od vrha i kliknite **Postavke**, zatim kliknite **moj račun** i morate registrirati valjan račun za Amazon.
3. U Eclipse, pokrenite aplikaciju.

> [AZURE.NOTE] Ako se pojavi problem, provjerite vrijeme emulator (ili uređaj). Vremensku vrijednost mora biti točan. Da biste promijenili vrijeme na uređaju Kindle emulator, iz imenika Alati za platforme Android SDK možete pokrenite sljedeću naredbu:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Slanje poruke

Da biste poslali poruku pomoću .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon portala za razvojne inženjere]: https://developer.amazon.com/home.html
[Preuzmite SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
