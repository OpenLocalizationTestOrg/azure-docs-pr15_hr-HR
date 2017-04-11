<properties
    pageTitle="Integracija sa sustavom Android SDK Azure mobilne radnje"
    description="Najnovija ažuriranja i postupke za Android SDK za Azure Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>Kako integrirati GCM s mobilnim radnje

> [AZURE.IMPORTANT] Morate slijediti Integracija postupak opisan u kako integrirati radnje sa sustavom Android dokument prije nego nastavite slijediti ovaj vodič.
>
> Dokument je korisno samo ako je već integriran modul za razgovor i namjeravate automatske uređaji Google Play. Da biste integrirali razgovor kampanje u aplikaciji, pročitajte prvo kako integrirati radnje doći do android.

##<a name="introduction"></a>Uvod

Integriranje GCM omogućuje aplikacije da biste se pomiču.

GCM payloads pomiču SDK uvijek sadrže na `azme` ključa u podatkovnom objektu. Stoga ako koristite GCM druge svrhe u aplikaciji, možete filtrirati ih gura na temelju te tipke.

> [AZURE.IMPORTANT] Samo uređajima sa sustavom Android 2.2 ili noviji, imate Google Play instaliran, ali imate Google omogućeno pozadine povezivanje možete se pomiču pomoću GCM; Međutim, možete integrirati kod sigurno na nepodržane uređaje (samo koristi reprodukcije).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>Stvaranje projekta Google Cloud poruka s ključ za API-JA

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>Integracija SDK

### <a name="managing-device-registrations"></a>Upravljanje registracije uređaja

Svakom uređaju morate poslati naredbu za registraciju na poslužitelje Google, u suprotnom ne može otvoriti.

Uređaj također možete odjaviti iz obavijesti o GCM (uređaj je automatski registriran Ako deinstalirate aplikacije).

Ako ne koristite [Google reproducirati SDK] ili ne već šaljete svrhu Registracija sami, možete učiniti radnje automatski registrirali uređaj za vas.

Da biste to omogućili, dodajte na sljedeći način vaše `AndroidManifest.xml` datoteke, unutar na `<application/>` oznaka:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Komunikacija Registracija id sa servisom automatske radnje i primati obavijesti

Da biste mogli komunicirati id Registracija uređaja sa servisom automatske radnje i primati njegov obavijesti, dodajte na sljedeći način vaše `AndroidManifest.xml` datoteke, unutar na `<application/>` oznake (čak i ako ste registracije uređaja sami upravljati):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Provjerite je li imati sljedeće dozvole svoje `AndroidManifest.xml` (nakon na `</application>` oznaka).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Dodijelite pristup mobilne radnje ključ za API GCM

Slijedite [ovaj vodič](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) za dodjelu radnje mobilnog pristupa ključ za API GCM.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
