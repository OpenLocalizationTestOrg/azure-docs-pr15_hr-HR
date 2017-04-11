1. U projektu **aplikaciju** , otvorite datoteku `AndroidManifest.xml`. U kod u sljedeća dva koraka, zamijenite _`**my_app_package**`_ naziv paketa aplikacije za projekt, koji je vrijednost u `package` atribut u `manifest` oznaka.

2. Dodajte sljedeće nove dozvole nakon postojeći `uses-permission` elementa:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Dodajte sljedeći kôd nakon na `application` otvaranje oznaka:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Otvorite datoteku *ToDoActivity.java*i dodajte sljedeća naredba uvoz:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. Dodajte sljedeće privatne varijabla klasa: zamjena _`<PROJECT_NUMBER>`_ s broj projekta koji je dodijelio Google na aplikaciju iz prethodnog postupka:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Promijenite definiciju *MobileServiceClient* **privatne** **javno statični**tako da se sada izgleda ovako:

        public static MobileServiceClient mClient;

7. Sljedeće moramo da biste dodali novi predmete učiniti obavijesti. Project Explorer otvorite **src** => **glavni** => **java** čvorove i desnom tipkom miša kliknite čvor naziv paketa: kliknite **Novo**, a zatim kliknite **Predmet Java**.

8. **Naziv** vrste `MyHandler`, zatim kliknite **u redu**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. U datoteci MyHandler zamjena deklariranje predmete s

        public class MyHandler extends NotificationsHandler {


10. Dodajte sljedeće naredbe uvoz za na `MyHandler` klasa:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Uz dodavanje ovog člana u `MyHandler` klasa:

        public static final int NOTIFICATION_ID = 1;


12. U na `MyHandler` klase, dodajte sljedeći kod da biste nadjačali metodu **onRegistered** koji dnevnike uređaj sa servisom mobilne obavijesti koncentratora.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. U na `MyHandler` klase, dodajte sljedeći kod da biste nadjačali metodu **onReceive** , čime će se obavijest da bi se prikazao se po primitku.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. Vratite se u datoteku TodoActivity.java ažurirajte metodu **onCreate** klase *ToDoActivity* da biste registrirali rukovatelj predmet obavijesti. Provjerite je li da biste dodali kod kada je instancirati *MobileServiceClient* .


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Pokrenite aplikaciju sada ažurirati podržava automatske obavijesti.
