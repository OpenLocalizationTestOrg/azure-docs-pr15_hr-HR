<properties
    pageTitle="Obavijest o koncentratora najnovije vijesti Praktični vodič – Android"
    description="Saznajte kako koristiti Azure servisa Bus obavijesti koncentratora za slanje obavijesti o novostima prijelom za uređaje sa sustavom Android."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Korištenje koncentratora obavijesti da biste poslali važnih vijesti

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Pregled

U ovoj se temi objašnjava da biste koristili Azure obavijesti koncentratora za emitiranje prijelom novosti obavijesti aplikacija za Android. Po dovršetku će moći Registrirajte se za najnovije vijesti kategorije koje vas zanimaju te primati samo slanje obavijesti za te kategorije. Taj se scenarij uobičajeni obrazac za mnoge aplikacije gdje se obavijesti imaju slati grupe korisnika koje ste prethodno deklarirane kamate na njima, npr RSS čitač, aplikacije za sporta glazbu, itd.

Scenariji za emitiranje omogućeni uključivanjem jednu ili više _oznaka_ prilikom stvaranja je registracija u središtu obavijesti. Kada se šalju obavijesti o oznaci, sve uređaje koje ste registrirali za oznaku će primiti obavijest. Budući da oznake jednostavno nizovi, ne moraju unaprijed dodjeli. Dodatne informacije o oznake, pogledajte [usmjeravanje koncentratora obavijesti i oznaka izraza](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Preduvjeti

U ovoj se temi sastavlja u aplikaciju koju ste stvorili u [Početak rada s obavijesti koncentratora][get-started]. Prije pokretanja ovog praktičnog vodiča, morate već dovršite [Početak rada s obavijesti koncentratora][get-started].

##<a name="add-category-selection-to-the-app"></a>Dodavanje kategorija odabira u aplikaciju

U prvi je korak da biste dodali elemente korisničkog Sučelja u postojeće glavni aktivnosti koje korisnik može odabrati kategorije da biste registrirali. Kategorije koji je odabrao korisnik pohranjuju se na uređaj. Prilikom pokretanja aplikacije Registracija uređaja se stvara u koncentratora za obavijesti s odabrane kategorije kao oznake.

1. Otvorite datoteku res/layout/activity_main.xml i zamijeniti sadržaja pomoću sljedeće:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Otvorite datoteku res/values/strings.xml, dodajte sljedeće retke:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Grafički izgled main_activity.xml trebala izgledati ovako:

    ![][A1]

3. Sada stvorite predmete **obavijesti** u paketu isti kao svojoj učionici **MainActivity** .

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Klase se koristi lokalno spremište za pohranu kategorija novosti koja sadrži ovaj uređaj da biste primali. Sadrži i metode da biste registrirali za te kategorije.


4. U svojoj učionici **MainActivity** uklanjanje vaša privatne polja za **NotificationHub** i **GoogleCloudMessaging**i dodavanje polja za **obavijesti**:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Nakon toga u način **onCreate** ukloniti inicijalizaciju polje **koncentratora** i metodu **registerWithNotificationHubs** . Zatim dodajte sljedeće retke koji Inicijalizacija instanca klase **obavijesti** . 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`i `HubListenConnectionString` već biti postavljen s na `<hub name>` i `<connection string with listen access>` rezerviranih mjesta s naziva koncentrator obavijesti i niz za povezivanje za *DefaultListenSharedAccessSignature* koji ste nabavili neke starije verzije.

    > [AZURE.NOTE] Budući da su vjerodajnice koje su distributed s klijentskom aplikacijom obično sigurne, samo distribucija ključ za pristup preslušavanja s aplikacijom klijenta. Poslušajte omogućuje pristup ne može se mijenjati aplikaciju za registraciju na obavijesti, ali postojeće registracije i ne može se obavijesti poslati. Puni pristup ključ se koristi u zaštićenim pozadinskog servisa za slanje obavijesti i promjene postojećih registracije.


6. Zatim dodajte sljedeće uvozi i `subscribe` način rukovanja gumb pretplati kliknite događaj:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Ovaj postupak stvara popis kategorija i koristi predmete **obavijesti** za pohranu na popisu u lokalno spremište i registrirati odgovarajuće oznake putem koncentratora za obavijesti. Kada se mijenjaju se kategorije, Registracija je ponovno stvoriti pomoću novih kategorija.

Aplikacija je moći pohranu skup kategorije u lokalno spremište na uređaju te morate registrirati središtu obavijesti svaki put kada korisnik promijeni odabira kategorije.

##<a name="register-for-notifications"></a>Registrirajte se za obavijesti

Ove korake registrirati putem središta obavijesti na pokretanja pomoću konfiguracije kategorije koje su pohranjeni u lokalno spremište.

> [AZURE.NOTE] Budući da atributima registrationId dodijeljeno po Google Cloud razmjenu poruka (GCM) možete promijeniti u bilo kojem trenutku, trebala bi se registrirati za obavijesti često da biste izbjegli pogreške obavijesti. U ovom se primjeru registrira za obavijesti prilikom svakog pokretanja aplikacije. Za aplikacije koje se često više puta dan, vjerojatno preskočite Registracija da biste očuvali propusnost Ako manji od jednog dana prošlo od prethodnog registracije.


1. Na kraju metoda **onCreate** u predmete **MainActivity** dodati sljedeći kod:

        notifications.subscribeToCategories(notifications.retrieveCategories());

    To jamči da prilikom svakog pokretanja aplikacije ga dohvaća kategorija iz lokalno spremište i zahtjeve registeration za te kategorije. 

2. Zatim ažurirajte na `onStart()` način u `MainActivity` klase na sljedeći način:

    @Overridezaštićeni Poništi onStart() {super.onStart();      isVisible = true;

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Time će se obnoviti glavni aktivnosti ovisno o statusu prethodno spremljene kategorije.

Aplikacija je dovršen i možete spremiti skup kategorije u lokalno spremište uređaj koristi za registriranje središtu obavijesti svaki put kada korisnik promijeni odabira kategorije. Zatim ćemo definirati pozadinskog koji mogu slati obavijesti kategorija za aplikaciju.

##<a name="sending-tagged-notifications"></a>Slanje obavijesti označenog

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Pokrenite aplikaciju i generiranje obavijesti

1. U Android Studio stvaranje aplikacije i pokretanje na uređaj ili emulator.

    Napomena da aplikaciju korisničkog Sučelja nudi skup preklapa koji vam omogućuje odaberite kategorije da biste se pretplatili.

2. Omogućivanje jednu ili više kategorija preklapa, a zatim kliknite **Pretplata**.

    Aplikaciju pretvara odabrane kategorije u oznake i zahtjeve novi Registracija uređaja za odabrane oznake u središtu obavijesti. Registrirani kategorije su vraća i prikazana u skočnoj obavijesti.

4. Pošaljite obavijest o novoj tako da pokrenete aplikaciju konzole za .NET.  Osim toga, možete poslati obavijesti označeni predloška [Azure klasični Portal]pomoću kartice ispravljanje pogrešaka koncentratora za obavijesti.

    Obavijesti za odabrane kategorije prikazuju se kao skočnoj obavijesti.

##<a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču smo naučili kako emitiranje važnih vijesti po kategoriji. Imajte na umu dovršavanje neku od sljedeće vodiči za kojima se ističu druge napredne scenarije koncentratora obavijesti:

+ [Korištenje obavijesti koncentratora za emitiranje lokalizirane važnih vijesti]

    Saznajte kako da biste proširili aplikaciju prijelom novosti da biste omogućili slanje lokalizirane obavijesti.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Korištenje obavijesti koncentratora za emitiranje lokalizirane važnih vijesti]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure klasični Portal]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
