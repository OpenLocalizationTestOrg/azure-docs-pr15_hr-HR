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


#<a name="how-to-integrate-adm-with-engagement"></a>Kako integrirati Admin radnje

> [AZURE.IMPORTANT] Morate slijediti Integracija postupak opisan u kako integrirati radnje sa sustavom Android dokument prije nego nastavite slijediti ovaj vodič.
>
> Dokument je korisno samo ako je već integriran modul za razgovor i namjeravate automatske Amazon uređaja. Da biste integrirali razgovor kampanje u aplikaciji, pročitajte prvo kako integrirati radnje doći do android.

##<a name="introduction"></a>Uvod

Integriranje Admin omogućuje aplikacije da biste pomiču kada ciljanja Amazon Android uređaja.

Payloads Admin pomiču SDK uvijek sadrže na `azme` ključa u podatkovnom objektu. Stoga ako koristite Admin druge svrhe u aplikaciji, možete filtrirati ih gura na temelju te tipke.

> [AZURE.IMPORTANT] Samo Amazon Kindle uređajima sa sustavom Android 4.0.3 ili noviji podržava Amazon uređaj poruka; Međutim, možete integrirati kod sigurno na drugim uređajima.

##<a name="sign-up-to-adm"></a>Prijavite se na Admin

Ako već niste učinili, morate omogućiti Admin na računu za Amazon.

Postupak je detaljan pri: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Nakon dovršetka postupka, primit ćete:

-   OAuth vjerodajnice (ID klijenta i klijent tajna) za radnje omogućiti slanje uređajima.
-   Ključ API morate integriran u aplikaciji.

##<a name="sdk-integration"></a>Integracija SDK

### <a name="managing-device-registrations"></a>Upravljanje registracije uređaja

Svakom uređaju morate poslati naredbu za registraciju na poslužitelje Admin, u suprotnom ne može otvoriti.

Ako već koristite [Admin klijentska biblioteka], a već imate [integrirani Admin] možete izravno na android-sdk-Admin-primanje.

Ako Admin ste nije još integrirana, radnje sadrži jednostavniji način da biste omogućili u aplikaciji:

Uređivanje vaše `AndroidManifest.xml` datoteke:

-   Dodavanje naziva Amazon datoteku započeti ovako:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   Unutar na `<application/>` oznaku, dodajte Ova sekcija:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Kad dodate oznaku amazon, možda ćete Sastavi pogreške ako projekt sastavljanje ciljne ispod Android 2.1. Morate koristiti na ciljnom Sastavi za **Android 2.1 +** (ne brinite, možete i dalje imate u `minSdkVersion` postavljena na 4).
-   Integrirati ključ za API Admin kao sredstvo po sljedeći [postupak].

Zatim slijedite upute u sljedećim odjeljcima.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Komunikacija Registracija id sa servisom automatske radnje i primati obavijesti

Da biste mogli komunicirati id Registracija uređaja sa servisom automatske radnje i primati njegov obavijesti, dodajte na sljedeći način vaše `AndroidManifest.xml` datoteke, unutar na `<application/>` oznake (čak i ako koristite Admin bez radnje):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Imate sljedeće dozvole vaše `AndroidManifest.xml` (prije se `</application>` oznaka).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Dodjela radnje OAuth vjerodajnice

Slanje OAuth vjerodajnice (ID klijenta i tajna klijenta) portalu radnje.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[Admin klijentska biblioteka]:https://developer.amazon.com/sdk/adm/setup.html
[integrirano Admin]:https://developer.amazon.com/sdk/adm/integrating-app.html
[postupak]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
