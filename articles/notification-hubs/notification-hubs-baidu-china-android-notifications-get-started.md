<properties
    pageTitle="Početak rada s koncentratorima obavijesti za Azure pomoću Baidu | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču, Saznajte kako koristiti Azure obavijesti koncentratora za slanje obavijesti za uređaje sa sustavom Android pomoću Baidu."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Početak rada s obavijesti koncentratora pomoću Baidu

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Pregled

Automatske oblaka Baidu je servis za kineski oblaka koje možete koristiti da biste poslali slanje obavijesti za mobilne uređaje. Taj servis je osobito je korisna u Kini, gdje izlaganja automatske obavijesti da biste Android je složene zbog prisutnosti trgovine različitih aplikacija i automatske servisima, osim dostupnost uređaje sa sustavom Android obično niste povezani s GCM (Google Cloud porukama).

##<a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ Android SDK (smo pretpostavlja da će koristiti Eclipse), koji možete preuzeti s <a href="http://go.microsoft.com/fwlink/?LinkId=389797">web-mjesta sa sustavom Android</a>
+ [Mobilni Services SDK za Android]
+ [Automatske Baidu SDK za Android]

>[AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Stvaranje računa Baidu

Da biste koristili Baidu, morate imati račun Baidu. Ako već postoji, prijavite se na [Baidu portal] i prijeđite na sljedeći korak. U suprotnom, potražite u članku dolje navedene upute za stvaranje Baidu računa.  

1. Idite na [Baidu portal] i kliknite vezu**登录**(**Prijava**). Kliknite**立即注册**da biste pokrenuli postupak registracije za račun.

    ![][1]

2. Unesite potrebne podatke – telefona/e-pošte adresu, lozinku i potvrdu kod – a zatim kliknite **Prijava**.

    ![][2]

3. Će se poslati poruku e-pošte na adresu e-pošte koji ste unijeli s vezom za aktivaciju računa Baidu.

    ![][3]

4. Prijavite se na račun e-pošte, otvorite Baidu aktivaciju e-pošte i kliknite vezu za aktivaciju za aktivaciju računa Baidu.

    ![][4]

Nakon što dodate račun aktivirani Baidu, prijavite se na [Baidu portal].

##<a name="register-as-a-baidu-developer"></a>Registrirajte se kao Baidu za razvojne inženjere

1. Kada ste prijavljeni na [Baidu portal], kliknite**更多 >>** (**više**).

    ![][5]

2. Pomaknite se prema dolje u odjeljku**站长与开发者服务 (administratora web-mjesta i servisi za razvojne inženjere)** , a zatim kliknite**百度开放云平台**(**Baidu otvorite oblaka platformu**).

    ![][6]

3. Na sljedećoj stranici kliknite**开发者服务**(**Servisi za razvojne inženjere**) u gornjem desnom kutu.

    ![][7]

4. Na sljedećoj stranici kliknite**注册开发者**(**Registrirana razvojni inženjeri**) na izborniku u gornjem desnom kutu.

    ![][8]

5. Unesite naziv, opis i broj mobilnog telefona za tekstna poruka provjere valjanosti, a zatim kliknite**送验证码**(**Slanje kod za potvrdu**). Imajte na umu da međunarodnih telefonskih brojeva, najprije morate pozivni broj zemlje napišite u zagradama. Na primjer, za broj Sjedinjenih Američkih Država, bit će **(1) 1234567890**.

    ![][9]

6. Zatim trebale primiti tekstnu poruku s brojem potvrdu, kao što je prikazano u sljedećem primjeru:

    ![][10]

7. Unesite broj potvrdu iz poruke u**验证码**(**kod za potvrdu**).

8. Na kraju, dovršite registraciju za razvojne inženjere tako da prihvaća Baidu ugovor, a zatim kliknete**提交**(**Pošalji**). Vidjet ćete sljedeće stranice na uspješan dovršetak Registracija:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Stvaranje projekta automatske Baidu oblaka

Kada stvorite Baidu oblaka automatske projekta, primite identifikacijskog Broja za aplikaciju ključ za API-JA i tajnu ključ.

1. Kada ste prijavljeni na [Baidu portal], kliknite**更多 >>** (**više**).

    ![][5]

2. Pomaknite se prema dolje u odjeljku**站长与开发者服务**(**administratora web-mjesta i servisi za razvojne inženjere**), a zatim kliknite**百度开放云平台**(**Baidu otvorite oblaka platformu**).

    ![][6]

3. Na sljedećoj stranici kliknite**开发者服务**(**Servisi za razvojne inženjere**) u gornjem desnom kutu.

    ![][7]

4. Na sljedećoj stranici kliknite**云推送**(**Oblaka automatske**) u odjeljku**云服务**(**Servise u Oblaku**).

    ![][12]

5. Kada otvorite registrirani programiranje, vidjet ćete**管理控制台**(**Konzola**) pri vrhu izbornika. Kliknite**开发者服务管理**(**razvojni inženjeri servisa upravljanje**).

    ![][13]

6. Na sljedećoj stranici kliknite**创建工程**(**Stvori projekt**).

    ![][14]

7. Unesite naziv aplikacije, a zatim kliknite**创建**(**Stvori**).

    ![][15]

8. Nakon uspjelo stvaranje Baidu oblaka automatske projekta, vidjet ćete stranicu s **ID programa**, **Ključ za API -JA**i **Tajna ključ**. Zabilježite ključ za API-JA i tajnu ključ koji koristit ćemo kasnije.

    ![][16]

9. Konfiguriranje projekta za slanje obavijesti tako da kliknete**云推送**(**Oblaka automatske**) u lijevom oknu.

    ![][31]

10. Na sljedećoj stranici, kliknite gumb**推送设置**(**automatske postavke**).

    ![][32]  

11. Na stranici Konfiguracija dodajte naziv paketa da koje će biti pomoću u Android projektu u polju**应用包名**(**Aplikacijski paket**), a zatim**保存设置**(**Spremi**).  

    ![][33]

Prikazat će se na**保存成功!** (**uspješno spremljenu!**) poruka.

##<a name="configure-your-notification-hub"></a>Konfiguriranje koncentratora za obavijesti

1. Prijava na [Portal za klasični Azure], a zatim kliknite **+ NOVO** pri dnu zaslona.

2. Kliknite **Aplikaciju servisa**, kliknite **Knjižna servisa**, kliknite **Koncentrator obavijesti**, a zatim **Brzo stvaranje**.

3. Navedite naziv **Koncentrator obavijesti**, odaberite **područje** i **polja naziva** kojima koncentrator Ova obavijest o stvorit će se, a zatim kliknite **Stvori novi koncentrator obavijesti**.  

    ![][17]

4. Kliknite polje naziva u kojem ste stvorili koncentratora za obavijesti, a zatim **Koncentratora obavijesti** na vrhu.

    ![][18]

5. Odaberite središtu obavijesti koji ste stvorili, a zatim **Konfiguriranje** gornje izborniku.

    ![][19]

6. Pomaknite se prema dolje do odjeljka **baidu postavki obavijesti** , a zatim unesite ključ za API-JA i tajnu ključ koji ste nabavili konzole za Baidu prethodno Baidu oblaka automatske projekta. Kliknite **Spremi**.

    ![][20]

7. Kliknite karticu **nadzorne ploče** na vrhu za središtu obavijesti, a zatim **Prikaz niza za povezivanje**.

    ![][21]

8. Zabilježite **DefaultListenSharedAccessSignature** i **DefaultFullSharedAccessSignature** iz prozora **informacije o povezivanju programa Access** .

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Povezivanje aplikacije koncentrator obavijesti

1. U Eclipse ADT, stvorite novi projekt sa sustavom Android (**datoteka** > **Novo** > **Android projekt aplikacije**).

    ![][23]

2. Unesite **Naziv aplikacije** , a zatim provjerite je li verzija **Najmanji potreban SDK** postavljen **API 16: sustav Android 4.1**.

    ![][24]

3. Kliknite **Dalje** , a nastavili pratiti čarobnjaka dok se ne prikazuje se prozor za **Stvaranje aktivnosti** . Provjerite je li **Prazne aktivnosti** i na kraju odaberite **Završi** da biste stvorili novu aplikaciju za Android.

    ![][25]

4. Provjerite je li **Cilj sastavljanje projekta** ispravno postavljen.

    ![][26]

5. Preuzmite datoteku obavijesti koncentratora 0.4.jar [Obavijesti-koncentratora-Android-SDK Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4)na kartici **datoteka** . Dodavanje datoteka u mapu **libs** Eclipse projekta i osvježavanje mapu *libs* .

6. Preuzmite i raspakirajte [Baidu automatske Android SDK], otvorite mapu **libs** , a zatim kopirajte datoteku posudu **x.y.z, pushservice-a** i **armeabi** & **mips** mape u mapi **libs** aplikacije za Android.

7. Otvorite datoteku **AndroidManifest.xml** Android projekta i dodajte dozvole koje su potrebne Baidu SDK.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. Dodajte svojstvo **android: naziv** elementa vaše **aplikacije** u **AndroidManifest.xml**, zamjene *yourprojectname* (npr., **com.example.BaiduTest**). Provjerite taj projekt naziv odgovara li onaj koji ste konfigurirali u Baidu konzoli sustava.

        <application android:name="yourprojectname.DemoApplication"

9. Dodajte sljedeće konfiguracija unutar elementa aplikacije nakon na **. MainActivity** element aktivnosti zamjene *yourprojectname* (npr., **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Dodajte novi klase pod nazivom **ConfigurationSettings.java** u projekt.

    ![][28]

    ![][29]

10. Da biste ga dodati sljedeći kod:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Postavite vrijednost od **API_KEY** uz dohvaća iz Baidu oblaka projekta ranije, **NotificationHubName** vašim imenom koncentrator obavijesti na portalu klasični Azure i **NotificationHubConnectionString** s DefaultListenSharedAccessSignature na portalu klasični Azure.

11. Dodavanje novog klase pod nazivom **DemoApplication.java**i dodati sljedeći kod:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Dodajte drugi novi klase pod nazivom **MyPushMessageReceiver.java**i dodati kod ispod. Ovo je klasa koja upravlja automatske obavijesti koji su primili s poslužitelja automatske Baidu.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Otvorite **MainActivity.java**i dodajte sljedeće **onCreate** metoda:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Otvorite sljedeće naredbe Uvezi na vrhu:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Slanje obavijesti u aplikaciju


Brzo možete testirati primanje obavijesti u svojoj aplikaciji slanjem obavijesti na [Portal za Azure](https://portal.azure.com/) pomoću gumba za **Testiranje slanje** na središte obavijesti kao što je prikazano na zaslonu u nastavku.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Automatske obavijesti obično se šalju u pozadinskoj servis kao što je mobilnih usluga ili ASP.NET pomoću kompatibilne biblioteke. REST API-JA možete koristiti i izravno za slanje obavijesti o ako biblioteci nije dostupna za vaš pozadinske.

U ovom ćete praktičnom vodiču ćemo će jednostavnost i samo demonstrirati testiranje aplikacije klijenta slanjem obavijesti pomoću .NET SDK za obavijesti koncentratora u aplikaciji konzole umjesto pozadinskog servisa. Praktični vodič za [Korištenje obavijesti koncentratora za slanje obavijesti korisnicima](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) preporučujemo kao na sljedeći korak za slanje obavijesti iz programa ASP.NET pozadine. Međutim, sljedeće postupke mogu koristiti za slanje obavijesti:

* **OSTALE sučelja**: podržavaju obavijesti na bilo kojoj platformi pozadinskog pomoću [sučelja OSTALE](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure obavijesti koncentratora .NET SDK**: U alatu Nuget Upravitelj paketa za Visual Studio, pokrenite [Instalaciju paketa Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [kako koristiti koncentratora obavijesti iz Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Mobilne aplikacije**: primjerice slanje obavijesti iz mobilne aplikacije za Azure aplikacija servisa pozadinskog sustava koji je integriran s koncentratorima obavijesti potražite u članku [Dodavanje slanje obavijesti za mobilne aplikacije](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: primjerice slanja obavijesti pomoću REST API-ji potražite u članku "da biste koristili koncentratora obavijesti iz Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Neobavezno) Slanje obavijesti iz aplikacije konzole za .NET.

U ovom ćete odjeljku Pokazat ćemo slanja obavijesti aplikacija konzole za .NET.

1. Stvaranje nove Visual C# konzole aplikacije:

    ![][30]

2. U prozoru upravitelja paketima konzole postavite **zadane projekta** na novi projekt konzole za aplikaciju, a zatim u prozoru konzole, pokrenite sljedeću naredbu:

        Install-Package Microsoft.Azure.NotificationHubs

    Time se dodaje referenca SDK koncentratora obavijesti Azure pomoću značajke <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification koncentratora NuGet paketa</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Otvorite datoteku **Program.cs** i dodajte sljedeće pomoću naredbe:

        using Microsoft.Azure.NotificationHubs;

4. U vašem `Program` klase, dodajte sa sljedećim korakom i zamijenite *DefaultFullSharedAccessSignatureSASConnectionString* i *NotificationHubName* vrijednosti koje imate.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Dodajte sljedeće retke u način **glavne** :

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Testiranje aplikacije

Da biste isprobali ovu aplikaciju s stvarni telefonom, samo priključite telefon na računalo pomoću USB kabela. To se učitava aplikacije na priložene telefona.

Da biste testirali aplikacije s emulator na Eclipse gornjoj alatnoj traci kliknite **Pokreni**, a zatim pokrenite aplikaciju. To će se pokreće emulator, a zatim učitava i pokreće aplikaciju.

Aplikacija dohvaća 'ID korisnika' i 'channelId' iz servisa Baidu automatske obavijesti i registrira s središtu obavijesti.

Da biste poslali obavijest test možete koristiti karticu Provjera pogrešaka portala klasični Azure. Ako ugrađena aplikacija za .NET konzole za Visual Studio, samo pritisnite tipku F5 u Visual Studio li pokrenuti aplikaciju. Aplikacija će poslati obavijest koji će se pojaviti u području obavijesti za gornje uređaju ili emulator.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobilni Services SDK za Android]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Automatske Baidu SDK za Android]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure klasični Portal]: https://manage.windowsazure.com/
[Portal za Baidu]: http://www.baidu.com/
