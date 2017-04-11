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


#<a name="upgrade-procedures"></a>Nadogradnja postupaka

Ako već imate integrirali stariju verziju naš SDK u aplikaciji, morate Imajte na umu sljedeće prilikom nadogradnje SDK-a.

Možda ćete morati slijedite nekoliko postupaka ako Propušteni nekoliko verzija SDK-a. Ako, primjerice migriranja iz 1.4.0 u 1.6.0 morate najprije slijedite postupak "iz 1.4.0 za 1.5.0" pa postupak "iz 1.5.0 za 1.6.0".

Sve verzije ste nadogradili morate zamijeniti u `mobile-engagement-VERSION.jar` novim.

##<a name="from-420-to-421"></a>Iz 4.2.0 za 4.2.1

Taj korak mogu zapravo izvršiti na bilo koju verziju SDK, to je sigurnost unaprjeđivanja kada integrirati aktivnosti razgovor.

Trebali biste sada dodati `exported="false"` svim aktivnostima razgovor.

Razgovor aktivnosti trebala izgledati ovako vašem `AndroidManifest.xml`:

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

##<a name="from-400-to-410"></a>Iz 4.0.0 za 4.1.0

SDK sada ručicu novog dozvola modela iz Android M.

Ako koristite značajke mjesto ili širu sliku obavijesti pročitajte [Ovaj odjeljak](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Osim novog modela dozvola sada podržavamo konfiguriranje značajke mjesto prilikom izvođenja.
Ne možemo i dalje kompatibilni su s manifesta parametara za mjesto, ali sada je zastario. Da biste koristili konfiguriranje izvođenja, uklonite u sljedećim se odjeljcima iz vaše ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

i pročitajte [postupak ažurirane](mobile-engagement-android-integrate-engagement.md#location-reporting) namijenjenu runtime konfiguracije.

##<a name="from-300-to-400"></a>Iz 3.0.0 za 4.0.0

### <a name="native-push"></a>Izvorni pribadače

Izvorni automatske (GCM na Admin) sada koristi se za u obavijestima aplikacije da morate konfigurirati nativni automatske vjerodajnice za bilo koju vrstu automatske kampanje.

Ako već nije slijedite [Ovaj postupak](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Integracija razgovor izmijenjena ``AndroidManifest.xml``.

Zamijenite ovo:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Po

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
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

Postoji vjerojatno učitavanja zaslona sada prilikom klika na obavijest (s tekst/web-sadržaj) ili ankete.
Morate dodati za te kampanje za rad u 4.0.0:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Resursi

Ugrađivanje novi `res/layout/engagement_loading.xml` datoteke u projekt.

##<a name="from-240-to-300"></a>Iz 2.4.0 za 3.0.0

Sljedeće opisuje kako migrirati programa SDK Integracija sa servisa Capptain u aplikaciju pokreće Azure Mobile radnje koje nudi Capptain SAS. Ako premještate iz starije verzije, obratite se web-mjesta Capptain će se migrirati 2.4.0 prvi put, a zatim primijenite postupak u nastavku.

>[AZURE.IMPORTANT] Capptain i Mobile radnje nisu iste usluge i postupak dolje samo ističe migriranju aplikaciju klijenta. Migracija SDK u aplikaciji će nije moguće migrirati podatke s poslužitelja Capptain poslužiteljima Mobile radnje.

### <a name="jar-file"></a>POSUDU datoteka

Zamjena `capptain.jar` po `mobile-engagement-VERSION.jar` u vašem `libs` mapu.

### <a name="resource-files"></a>Datoteka resursa

Sve datoteke resursa koje ćemo dali (mjestu po `capptain_`) mora biti zamijenjen novima (mjestu s `engagement_`).

Ako ste prilagodili te datoteke, morate ponovno primijenite prilagodbe na nove datoteke, **sve identifikatore datoteke resursa i preimenovana**.

### <a name="application-id"></a>ID aplikacije

Sada radnje koristi niz za povezivanje da biste konfigurirali identifikatora SDK kao što je identifikator aplikacije.

Morate koristiti `EngagementAgent.init` metoda u aktivnosti pokretač ovako:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Niz za povezivanje za svoju aplikaciju prikazat će se na Portal za Azure.

Uklonite sve poziv `CapptainAgent.configure` kao `EngagementAgent.init` zamjenjuje taj način.

Na `appId` više ne može konfigurirati pomoću `AndroidManifest.xml`.

Uklonite ovaj odjeljak iz vaše `AndroidManifest.xml` ako ste ga:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API

Svaki poziva na bilo koji klase jezika Java naš SDK ne može preimenovati. na primjer, `CapptainAgent.getInstance(this)` moguće preimenovati `EngagementAgent.getInstance(this)`, `extends CapptainActivity` moguće preimenovati `extends EngagementActivity` itd...

Ako su integriran s zadani agent preferenca datoteke, zadani naziv datoteke je sada `engagement.agent` i ključno je `engagement:agent`.

Prilikom stvaranja web najave Javascript binder je sada `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Velik broj promjena se dogodilo s dva, servis nije više dijeljena i mnogo primatelja više nisu izvesti.

Izvješća servisa sada je jednostavnije; Uklanjanje svrhe filtriranje i sve meta-podataka unutar njega i dodavanje `exportable=false`.

Uz sve što je preimenovati da biste koristili radnje.

Sada izgleda:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Kada želite omogućiti test zapisnika meta-podataka sada premještena oznaku aplikacije i preimenovana je:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Sve druge meta-podataka samo preimenovana, Evo čitavog popisa (Preimenuj tečaja samo one koji koristite):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play i SmartAd praćenja uklonjeni su iz SDK samo morate ukloniti to bez zamjena:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Aktivnosti razgovor sada su deklarirane ovako:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Ako imate prilagođene aktivnosti razgovor, morate samo promijeniti svrhe akcije tako da odgovara ili `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` ili `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Emitiranje primatelje preimenovana plus sada dodat ćemo `exported=false`. Evo cijeli popis primatelja s nove specifikacije (od Preimenuj tečaja samo one koji koristite):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Praćenje tekstnog okvira uklonjena, da biste imali da biste uklonili ovaj odjeljak:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Imajte na umu da deklariranje svoju implementaciju emitiranje **EngagementMessageReceiver** tekstnog okvira promijenio u na `AndroidManifest.xml`. To je jer je uklonjena API da biste uklonili proizvoljne XMPP poruke iz proizvoljne XMPP entiteti i API-JA za slanje i primanje poruka između uređaja. Dakle, imate i da biste izbrisali sljedeće pozive iz implementaciju **EngagementMessageReceiver** :

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

i

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

zatim izbrišite sve poziva na **EngagementAgent** za:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

i

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard konfiguracije možete mogu negativno utjecati na značajku rebranding, pravila sada traženi kao što su:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
