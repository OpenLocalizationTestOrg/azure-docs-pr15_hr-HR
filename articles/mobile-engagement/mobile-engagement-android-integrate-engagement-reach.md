<properties
    pageTitle="Integracija sa sustavom Android SDK Azure mobilne radnje"
    description="Najnovija ažuriranja i postupke za Android SDK za Azure Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Kako integrirati radnje razgovor na sustavu Android

> [AZURE.IMPORTANT] Morate slijediti Integracija postupak opisan u kako integrirati radnje sa sustavom Android dokument prije nego nastavite slijediti ovaj vodič.

##<a name="standard-integration"></a>Standardni Integracija

SDK dosegne potreban je **Android podršku biblioteke (v4)**.

Najbrži način za dodavanje biblioteke u projekt u **Eclipse** je `Right click on your project -> Android Tools -> Add Support Library...`.

Ako ne koristite Eclipse, možete čitati upute [u nastavku].

Kopiranje datoteka resursa razgovor s SDK u projektu:

-   Kopiranje datoteke iz na `res/layout` mape koja se isporučuje s SDK u na `res/layout` mapu aplikacije.
-   Kopiranje datoteke iz na `res/drawable` mape koja se isporučuje s SDK u na `res/drawable` mapu aplikacije.

Uređivanje vaše `AndroidManifest.xml` datoteke:

-   Dodavanje u sljedećoj sekciji (između na `<application>` i `</application>` oznake):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   Morate tu dozvolu na ponavljanje obavijesti sustava koje nisu kliknute prilikom pokretanja sustava (u suprotnom oni će biti zadržane na disku, ali više neće se prikazati, zaista morate uključiti to).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Odredite ikone koja se koristi za obavijesti (i u aplikaciji i onih sustava) tako da kopirate i uređujete u sljedećoj sekciji (između na `<application>` i `</application>` oznake):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] U ovom se odjeljku je **obavezna** Ako planirate o korištenju sustava obavijesti prilikom stvaranja kampanje razgovor. Android onemogućuje prikazuje li se obavijesti sustava bez ikone. Pa izostavite u ovom se odjeljku krajnji korisnici neće moći primati.

-   Ako stvorite kampanje s obavijesti sustava pomoću širu sliku, morate dodati sljedeće dozvole (nakon što u `</application>` oznaka) ako nema:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Na Android M i pronalaze ako aplikacija sa sustavom Android API razinu 23 ili noviji, ``WRITE_EXTERNAL_STORAGE`` dozvolu mora odobriti korisnika. Pročitajte [Ovaj odjeljak](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Za obavijesti sustava možete i odrediti u razgovor kampanje ako uređaj treba preusmjeravaju i/ili vibracija. Je za rad imate da biste bili sigurni deklarirane sljedeće dozvola (nakon što u `</application>` oznaka):

            <uses-permission android:name="android.permission.VIBRATE" />

    Bez tih dozvola Android sprječava obavijesti sustava se prikazuje ako je potvrđen pozivanje ili mogućnost vibrate u upravitelju dosegne kampanje.

-   Ako sastavljanje na aplikacije pomoću **ProGuard** , a imate pogreške vezane uz Android podršku biblioteke ili posudu radnje, dodajte sljedeće retke za vaše `proguard.cfg` datoteke:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Izvorni pribadače

Sad kad ste konfigurirali modul razgovor, morate konfigurirati nativni automatske da biste mogli primati kampanje na uređaju.

Podržavamo dva servisa u sustavu Android:

  - Uređaji Google Play: korištenje [Razmjene poruka Google Cloud] slijedeći [kako integrirati GCM s mogućnostima vodič](mobile-engagement-android-gcm-integrate.md) vodič.
  - Uređaji Amazon: korištenje [Poruka na uređaju Amazon] slijedeći [kako integrirati Admin s mogućnostima vodič](mobile-engagement-android-adm-integrate.md) vodič.

Ako želite ciljani Amazon i Google Play uređaja, njegov moguće da imate sve unutar 1 AndroidManifest.xml/APK za razvoj. No prilikom slanja Amazon, možda odbijanja aplikacije ako ih pronaći GCM kod.

U tom slučaju trebate koristiti više APKs.

**Aplikacija je sada primati i prikaz razgovor kampanje!**

##<a name="how-to-handle-data-push"></a>Kako rukovati automatske podataka

### <a name="integration"></a>Integracija

Ako želite da se aplikacije da biste mogli primati ih gura razgovor podataka, morate stvoriti podređene klasa `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` i referencu u na `AndroidManifest.xml` datoteke (između na `<application>` i/ili `</application>` oznake):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Zatim možete nadjačati na `onDataPushStringReceived` i `onDataPushBase64Received` pozive. Evo jednog primjera:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategorija

Parametra kategorija nije obavezna, prilikom stvaranja kampanje automatske podatke i omogućuje vam da filtriranje podataka ih gura. To je korisno ako imate nekoliko emitiranje primatelje zadužen za različite vrste podataka ih gura, ili ako želite automatske različite od `Base64` podatke i želite da biste odredili njihove vrste prije nego što ih analize.

### <a name="callbacks-return-parameter"></a>Povratni parametar pozive korisnika

Evo nekoliko smjernica za pravilno rukovanje vraćeni parametar `onDataPushStringReceived` i `onDataPushBase64Received`:

-   Emitiranje primatelj mora vratiti `null` u povratnog ako znate kako rukovati podacima pribadače. Da biste odredili hoće li da vaše emitiranje tekstnog okvira tretirati automatske podataka ili ne trebate koristiti kategorije.
-   Nešto emitiranje primatelj mora vratiti `true` u povratnog Ako prihvati automatske podataka.
-   Nešto emitiranje primatelj mora vratiti `false` u povratnog ako prepoznaje automatske podataka, ali odbacuje bilo razloga. Na primjer, vratiti `false` kada Primljeni podaci nisu valjani.
-   Ako nešto emitiranje vraća tekstnog okvira `true` dok drugi nešto vraća `false` za istu automatske podataka ponašanje nije definirana, koja nikad ne trebao učiniti.

Vrsta povrata koristi se samo za statistiku razgovor:

-   `Replied`se povećava ako nešto emitiranje primatelje Vrati `true` ili `false`.
-   `Actioned`povećava se samo ako je jedan od emitiranje primatelje vraća `true`.

##<a name="how-to-customize-campaigns"></a>Kako prilagoditi kampanje

Da biste prilagodili kampanje, možete izmijeniti rasporede navedenih u SDK dosegne.

Trebali biste zadržali sve identifikatore koristi u izgledima i zadržavanje vrsta prikaza koji koristite identifikator, posebice za prikazi teksta i slika prikazi. Da biste sakrili ili prikazali područja tako da se može promijeniti njihove vrste samo koriste neki prikazi. Provjerite izvornog koda ako namjeravate promijeniti vrstu prikaza u navedeni izgledima.

### <a name="notifications"></a>Obavijesti

Postoje dvije vrste obavijesti: sustav i u aplikaciji obavijesti koje koriste datoteke u drugi raspored.

#### <a name="system-notifications"></a>Obavijesti sustava

Da biste dijaloški okvir Prilagodi obavijesti sustava morate koristiti **kategorije**. Možete se prebaciti na [kategorije](#categories).

#### <a name="in-app-notifications"></a>Obavijesti u aplikaciji

Prema zadanim postavkama, obavijest u aplikaciji je prikaz koji se dinamički dodaje trenutni aktivnosti korisničko sučelje zahvaljujući metodu Android `addContentView()`. To se naziva obavijesti preklapanje. Obavijest o prekrivanja odlični su za brzo Integracija jer ne trebaju izmjenu izgleda u aplikaciji.

Da biste promijenili izgled vaše prekrivanja obavijesti, možete jednostavno izmijeniti datoteku `engagement_notification_area.xml` vašim potrebama.

> [AZURE.NOTE] Datoteka `engagement_notification_overlay.xml` onaj koji se koristi za stvaranje preklapanja na obavijesti sadrži datoteku `engagement_notification_area.xml`. Možete i prilagoditi ga u skladu s potrebama (kao što je položaja u područje obavijesti unutar preklapanja).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Uključite obavijesti izgleda kao dio sustava izgleda aktivnosti

Slojevi su odlične za brzo integraciju, ali se može inconvenient ili imaju popratne pojave u određenim slučajevima. Preklapanje sustava moguće je prilagoditi na razini aktivnosti, što olakšava da biste spriječili popratne pojave za posebne aktivnosti.

Možete odabrati da biste dodali naš izgleda obavijesti u postojeći izgled uz pomoć izjava **obuhvaćaju** sa sustavom Android. Slijedi primjer na izmijenjene `ListActivity` raspored koji sadrži samo na `ListView`.

**Prije integracije radnje:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Nakon Integracija radnje:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

U ovom primjeru dodali smo spremniku nadređenog jer izvorni izgled koristi prikaza popisa kao element najviše razine. I dodali smo `android:layout_weight="1"` da biste mogli dodati prikaz ispod prikaza popisa s `android:layout_height="fill_parent"`.

Radnje dosegne SDK automatski otkriva izgleda obavijesti je uključen u ovu aktivnost, a dodat će se ne pojavljuje se prekrivno za ovu aktivnost.

> [AZURE.TIP] Ako koristite na ListActivity u aplikaciji, vidljiv prekriveno razgovor onemogućuje se reagira da biste kliknuli više stavki u prikazu popisa. Ovo je poznat problem. Da biste riješili taj problem predlažemo da ugrađivanje izgleda obavijesti u vlastiti popis izgled aktivnosti kao što su, u prethodni uzorak.

##### <a name="disabling-application-notification-per-activity"></a>Onemogućivanje aplikacije obavijesti po aktivnosti

Ako ne želite prekriveno dodati aktivnosti i ako ne uključite izgleda obavijesti u vlastiti izgled, možete onemogućiti prekriveno za ovu aktivnost u na `AndroidManifest.xml` dodavanjem u `meta-data` odjeljku kao u sljedećem primjeru:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategorije

Kada mijenjate navedeni izglede, izmijenite izgleda sve obavijesti. Kategorije omogućuju da odredite raznim ciljnim izgleda (vjerojatno ponašanja) za obavijesti. Kategorije možete navesti prilikom stvaranja kampanje razgovor. Imajte na umu da kategorije omogućuju vam prilagodbu najave i ankete, koji je opisan u nastavku ovog dokumenta.

Da biste registrirali rukovatelja kategorija za obavijesti, morate dodati pozivu kada je pokrenut aplikacije.

> [AZURE.IMPORTANT] Pročitajte upozorenje o atribut android: postupak \<android-sdk-radnje-postupak\> u kako integrirati radnje na Android temu prije nastavka.

U sljedećem primjeru pretpostavlja označeni prethodne upozorenje i korištenje podređenu klasa `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

Na `MyNotifier` objekt je implementacija kategorija rukovatelja obavijesti. Je ili implementacija u `EngagementNotifier` sučelja ili sub razredu od zadan: `EngagementDefaultNotifier`.

Imajte na umu da isti obavijesti možete rukovati više kategorija, možete ih registrirati ovako:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Da biste zamijenili zadan u kategoriji, možete registrirati implementaciju kao u sljedećem primjeru:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Trenutni kategorija koja se koristi u rukovatelja prosljeđuje se kao parametar u većini metode možete nadjačati u `EngagementDefaultNotifier`.

Ili kao ona se prenosi u `String` parametar ili neizravno na `EngagementReachContent` objekt koji sadrži na `getCategory()` način.

Većina postupak stvaranja obavijesti možete promijeniti tako da ponovnog definiranja metode na `EngagementDefaultNotifier`, za naprednije prilagodbu slobodno pogledajte Tehnički dokumentaciju i na izvorni kod.

##### <a name="in-app-notifications"></a>Obavijesti u aplikaciji

Ako samo želite koristiti zamjenski raspored za određenu kategoriju, biste mogli implementirati kao u sljedećem primjeru:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Primjer `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Kao što vidite, identifikator za prikaz preklapanja razlikuje se od standardne. Važno je da svaki izgled koristite Jedinstveni identifikator za slojeva.

**Primjer `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Kao što vidite, identifikator za prikaz područje obavijesti razlikuje se od standardne. Nije važno da svaki izgled koristi jedinstveni identifikator za područja obavijesti.

U ovom se primjeru jednostavne kategorije čini aplikacije (ili u aplikaciji) obavijesti pri vrhu zaslona. Ne možemo niste promijenili standardne identifikatora u području obavijesti sam.

Ako želite promijeniti, morate ponovno definirati na `EngagementDefaultNotifier.prepareInAppArea` način. Preporučuje se da vidite dokumentaciju za Tehnički, a na izvornom kodu `EngagementNotifier` i `EngagementDefaultNotifier` ako želite razine dodatne prilagodbe.

##### <a name="system-notifications"></a>Obavijesti sustava

Produženjem `EngagementDefaultNotifier`, možete nadjačati `onNotificationPrepared` mijenjanja obavijest koju je pripremite tako da zadan.

Ako, na primjer:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

U ovom se primjeru čini obavijesti sustava za sadržaj koji se prikazuju kao događaja tijeku kada se koristi kategoriji "tijeku".

Ako želite izraditi na `Notification` objekt od nule, možete se vratiti `false` način i poziv `notify` sami na na `NotificationManager`. U tom slučaju važno je da zadržite na `contentIntent`, `deleteIntent` i identifikator obavijesti koji se koriste `EngagementReachReceiver`.

Evo točan primjera takve implementacija:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Samo najave obavijesti

Upravljanje klikom na obavijest jedina objava moguće je prilagoditi tako da nadjačavanje `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` da biste izmijenili na pripremljeni `Intent`. Pomoću ove metode omogućuje jednostavno ugađanje zastavice.

Da biste dodali, primjerice na `SINGLE_TOP` zastavicom:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Za korisnike naslijeđene radnje, uzmite u obzir da obavijesti sustava bez akcije URL-a sada pokreće aplikacije ako je u pozadini, tako da se ovaj postupak možete pozvati s obavijest bez URL akcije. Razmislite o koji prilikom prilagodbe namjera.

Možete i implementirati `EngagementNotifier.executeNotifAnnouncementAction` od nule.

##### <a name="notification-life-cycle"></a>Obavijest o životnog ciklusa

Kada koristite zadane kategorije, neki postupci za životni ciklus nazivaju na na `EngagementReachInteractiveContent` objekt Statistika izvješća i ažurirati stanje kampanje:

-   Kada se obavijest prikazati u aplikaciji ili staviti na traci stanja u `displayNotification` način zove (koji izvješća Statistika) tako da `EngagementReachAgent` ako `handleNotification` vraća `true`.
-   Ako nehotice se obavijesti, na `exitNotification` pod nazivom način statistike prijavljuje i dalje kampanje sada se obrađuju.
-   Ako kliknete obavijesti `actionNotification` je pod nazivom statistike prijavljuje i pridruženi cilj je pokrenuo.

Ako svoju implementaciju sustava `EngagementNotifier` zaobilazi zadano ponašanje moram nazvati ovih metoda životnog ciklusa tako da sami. Sljedeći primjeri pokazuju nekim slučajevima gdje je zadano ponašanje zaobići:

-   Ne proširi `EngagementDefaultNotifier`, npr implementirana kategorija rukovanje ispočetka.
-   Za obavijesti sustava overrode u `onNotificationPrepared` i koju ste izmijenili `contentIntent` ili `deleteIntent` u na `Notification` objekt.
-   Obavijesti u aplikaciji overrode `prepareInAppArea`, ne zaboravite da biste mapirali barem `actionNotification` na neki od vašeg U.I kontrole.

> [AZURE.NOTE] Ako `handleNotification` throws iznimku, sadržaj se briše i `dropContent` naziva. To je prikazano u Statistika i sljedeći kampanje sada se obrađuju.

### <a name="announcements-and-polls"></a>Objava i ankete

#### <a name="layouts"></a>Izgledi

Možete izmijeniti u `engagement_text_announcement.xml`, `engagement_web_announcement.xml` i `engagement_poll.xml` datoteke da biste prilagodili tekst objave, najave web i ankete.

Te se datoteke zajednički koristite dva uobičajena izgleda za područja naslova i gumb. Kada je raspored za naslov `engagement_content_title.xml` i koristi eponymous drawable datoteku za pozadinu. Kada je raspored za gumbe akcije i završne `engagement_button_bar.xml` i koristi eponymous drawable datoteku za pozadinu.

U ankete, izgleda pitanje i njihovih odabire se dinamički inflated pomoću nekoliko puta na `engagement_question.xml` datoteke izgleda za pitanja i `engagement_choice.xml` datoteke za mogućnosti.

#### <a name="categories"></a>Kategorije

##### <a name="alternate-layouts"></a>Zamjenski izgledi

Kao što su obavijesti, kategorija s kampanjom može se koristiti da bi zamjenski rasporeda za najave i ankete.

Na primjer, da biste stvorili kategoriju za tekst objave, možete proširiti `EngagementTextAnnouncementActivity` i referencu na `AndroidManifest.xml` datoteke:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Imajte na umu da kategorija u svrhe filtar štiti razlika s objava aktivnosti zadani.

SDK dosegne koristi svrhe sustav da biste riješili desnom aktivnosti za određenu kategoriju i pada vratite na zadanu kategoriju neuspješne razlučivost.

Zatim morate provesti `MyCustomTextAnnouncementActivity`, ako samo želite promijeniti izgled (uz zadržavanje isti prikaz identifikatora), imate da biste definirali predmete kao u sljedećem primjeru:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Da biste zamijenili kategoriju zadani tekst objave, samo zamijeniti `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` po implementaciju.

Na sličan način moguće je prilagoditi web najave i ankete.

Objava web možete proširiti `EngagementWebAnnouncementActivity` i deklarirati aktivnosti u na `AndroidManifest.xml` kao u sljedećem primjeru:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Ankete možete proširiti `EngagementPollActivity` i postavite svoje u na `AndroidManifest.xml` kao u sljedećem primjeru:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementacija ispočetka

Implementacija kategorija za aktivnosti objava (i ankete) bez proširivanja nešto od na `Engagement*Activity` klase nudi SDK dosegne. To je korisno, primjerice ako želite definirati izgled koji koriste isti prikaze kao standardnih izgleda.

Kao što su za prilagodbu napredne obavijesti, preporučuje se možete pregledati na izvornom kodu standardne implementacije.

Evo nekih Napomena koje valja imati na umu: aktivnosti s određenim namjera (odgovara svrhe filtar) razgovor će se pokrenuti plus dodatni parametar koji je identifikator sadržaja.

Za dohvaćanje sadržaja objekt koje sadrže polja koje ste naveli prilikom stvaranja kampanje na web-mjesta učinite sljedeće:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Za statistiku, trebali biste izvješća sadržaj se prikazuje u na `onResume` događaja:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Nakon toga ne zaboravite da biste uputili poziv ili `actionContent(this)` ili `exitContent(this)` na željenom objektu sadržaja prije aktivnosti prelazi u pozadini.

Ako ne poziva ili `actionContent` ili `exitContent`, Statistika neće biti poslana (odnosno nema analize na kampanje) i više važno je napomenuti da, sljedeći kampanje neće biti obaviješteni dok ponovnog pokretanja postupka aplikacije.

Usmjerenje ili drugih promjena konfiguracije možete napraviti kod Evidencija da biste odredili hoće li aktivnost ulazi u pozadinu ili ne, standardne implementaciju provjerava je li sadržaj prijavljuje kao što je napuste ako korisnik napusti aktivnost (ili pritiskom na tipku `HOME` ili `BACK`), ali ne ako promjene usmjerenja.

Ovdje je zanimljiv dio implementacije:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Kao što vidite, ako ste naziva `actionContent(this)` , a zatim Završi aktivnosti, `exitContent(this)` može bez opasnosti pozvati bez učinka.

[Ovdje]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud razmjene poruka]:http://developer.android.com/guide/google/gcm/index.html
[Poruka na uređaju Amazon]:https://developer.amazon.com/sdk/adm.html
