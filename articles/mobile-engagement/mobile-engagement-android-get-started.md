<properties
    pageTitle="Početak rada s Android aplikacije Azure Mobile radnje"
    description="Saznajte kako koristiti radnje Mobile Azure s analize i automatske obavijesti za Android aplikacije."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Početak rada s radnje Azure Mobile za Android aplikacije

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ova tema prikazuje kako ga koristiti Azure Mobile radnje da biste shvatili Upotreba aplikacije i automatske obavijesti poslati segmentiranog korisnicima aplikacije za Android.
Pomoću ovog praktičnog vodiča pokazuje jednostavni scenarij emitiranje pomoću mobilnog radnje. U njoj stvorite prazan aplikacija za Android koji prikuplja osnovne podatke i prima automatske obavijesti pomoću poruka Google Cloud (GCM).

## <a name="prerequisites"></a>Preduvjeti

Dovršavanje ovog praktičnog vodiča potreban je [Android alate za razvojne inženjere](https://developer.android.com/sdk/index.html), koji obuhvaća integrirano razvojno okruženje Android Studio i najnovije platformi Android.

Zahtijeva [Mobile radnje sa sustavom Android SDK](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Da biste dovršili ovaj Praktični vodič, potreban vam je račun Azure active. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Postavljanje mobilnih radnje za aplikacija za Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

Pomoću ovog praktičnog vodiča predstavlja na "Osnovni Integracija", što je minimalnim potrebne za prikupljanje podataka i slanje automatske obavijesti. Stvorite osnovni aplikaciju s Android Studio da bismo pokazali integraciju sa servisom.

Potpuna Integracija dokumentaciju pronaći ćete u [Integracija sa sustavom Android SDK za mobilne radnje](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Stvaranje Android projekta

1. Pokrenite **Android Studio**i u skočnom izborniku odaberite **Počni novi projekt Studio sa sustavom Android**.

    ![][1]

2. Navedite aplikacije imenu i tvrtki domenom sustava. Zabilježite što koji ispunjavate, jer je potrebno je kasnije. Kliknite **Dalje**.

    ![][2]

3. Odaberite ciljni obrazac faktor i API razinu, a zatim kliknite **Dalje**.

    >[AZURE.NOTE] Korištenje mobilne zahtijeva API razinu 10 minimalne (Android 2.3.3).

    ![][3]

4. Odaberite **Praznu aktivnosti** ovdje, što je zaslonu samo za ovu aplikaciju, a zatim kliknite **Dalje**.

    ![][4]

5. Na kraju, ostavite zadane postavke kao što je, a zatim kliknite **Završi**.

    ![][5]

Android Studio stvara sada aplikaciju pokazni videozapis u koje ćemo integrirati Mobile radnje.

### <a name="include-the-sdk-library-in-your-project"></a>Uključivanje SDK biblioteke u projektu

1. Preuzmite [Android SDK mobilne radnje](https://aka.ms/vq9mfn).
2. Izdvajanje arhivsku datoteku u mapu na vašem računalu.
3. Prepoznajte .jar biblioteka za trenutnu verziju taj SDK i kopiranje u međuspremnik.

      ![][6]

4. Dođite na sekciju **projekta** (1) i zalijepite u .jar u mapi libs (2).

      ![][7]

5. Da biste učitali u biblioteku, sinkronizirati projekta.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Povezivanje aplikacije s pozadinskom radnje Mobile s nizom za povezivanje

1. Kopirajte sljedeće retke koda u stvaranje aktivnosti (mora poduzeti samo na jednom mjestu aplikacije, obično glavni aktivnosti). Za ovu aplikaciju uzorka Otvaranje kopije MainActivity u odjeljku src -> glavne -> java mapu, a zatim dodajte sljedeće:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Pritiskom na Alt + Enter ili dodavanje sljedeće naredbe uvoz Razriješi reference:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Vratite Azure klasični portal na stranici vaše aplikacije **Veze informacije** i kopirajte **Niz za povezivanje**.

      ![][9]

4. Ga zalijepiti u na `setConnectionString` parametar, zamjene cijeli niz koji se prikazuju u sljedeći kod:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Dodavanje dozvole i servis za deklariranje

1. Dodavanje tih dozvola Manifest.xml projekta neposredno prije ili nakon na `<application>` oznaka:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Da biste deklarirati servis agenta, dodajte kod između na `<application>` i `</application>` oznaka:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. U kodu zalijepljeni zamjena `"<Your application name>"` na naljepnici koja se prikazuje na izborniku **Postavke** na kojem ćete moći vidjeti services pokrenut na uređaju. Možete dodati riječ "Servisa" u taj natpis, primjerice.

### <a name="send-a-screen-to-mobile-engagement"></a>Slanje na zaslon Mobile radnje

Za početak slanja podataka i provjerite jesu li korisnici aktivna, pozadinski Mobile radnje mora poslati barem jedan zaslon (aktivnosti).

Idite na **MainActivity.java** i dodajte sljedeće da biste zamijenili osnovni klasa **MainActivity** **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Ako Osnovna klasa nije *aktivnosti*, pogledajte kako nasljeđuju iz različitih klase [Napredne Android izvješćivanje](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) .


Stvaranje komentara out sljedeći redak za taj scenarij jednostavne uzorak:

    // setSupportActionBar(toolbar);

Ako želite da ostanu u `ActionBar` u svojoj aplikaciji potražite u članku [Napredne Android izvješćivanje](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

Tijekom kampanje radnje Mobile omogućuje interakciju s i DOSEGNE korisnici automatske obavijesti i poruka u aplikaciji. U ovom modul naziva RAZGOVOR na portalu Mobile radnje.
U sljedećoj sekciji postavlja aplikacije primati.

### <a name="copy-sdk-resources-in-your-project"></a>Kopiranje SDK resursa u projektu

1. Vratite se na preuzimanje sadržaja SDK i kopirajte mapu **res** .

    ![][10]

2. Prijeđite natrag u Android Studio odaberite **glavni** direktorij projektnih datoteka, a zatim zalijepite da biste dodali resurse u projekt.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Daljnji koraci

Idite na [Android SDK](mobile-engagement-android-sdk-overview.md) da biste dobili detaljne znanja o integraciju sa servisom SDK.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
