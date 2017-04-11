<properties
    pageTitle="Napredna konfiguracija za Azure Mobile radnje sa sustavom Android SDK"
    description="U članku se opisuje Napredna konfiguracija mogućnosti uključujući Android manifesta s Azure Mobile radnje sa sustavom Android SDK"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Napredna konfiguracija za Azure Mobile radnje sa sustavom Android SDK

> [AZURE.SELECTOR]
- [Univerzalni sustava Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Ovaj postupak objašnjava kako konfigurirati razne mogućnosti konfiguracije za Azure Mobile radnje sa sustavom Android aplikacije.

## <a name="prerequisites"></a>Preduvjeti

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Preduvjeti za dozvole
Neke mogućnosti potreban je određeni dozvole koje su navedene ovdje za referencu, a zatim u retku u određene značajke. Dodavanje tih dozvola AndroidManifest.xml projektu neposredno prije ili nakon na `<application>` oznaka.

Kod dozvola treba izgledati ovako, gdje ispunite odgovarajuće dozvole iz tablice koja slijedi.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Dozvole | Kada se koristi |
| ---------- | --------- |
| INTERNET | Obavezan. Za osnovni izvješća |
| ACCESS_NETWORK_STATE | Obavezan. Za osnovni izvješća |
| RECEIVE_BOOT_COMPLETED | Obavezan. Da biste prikazali sredine obavijesti nakon ponovnog pokretanja uređaja |
| WAKE_LOCK | Preporučuje se. Omogućuje prikupljanje podataka prilikom korištenja Wi-Fi veze ili kada je zaslon isključen |
| VIBRACIJA | Neobavezno. Omogućuje vibracije primitku obavijesti |
| DOWNLOAD_WITHOUT_NOTIFICATION | Neobavezno. Omogućuje Android širu sliku obavijesti |
| WRITE_EXTERNAL_STORAGE | Neobavezno. Omogućuje Android širu sliku obavijesti |
| ACCESS_COARSE_LOCATION | Neobavezno. Omogućuje izvješćivanje o pogreškama u stvarnom vremenu mjesto |
| ACCESS_FINE_LOCATION | Neobavezno. Omogućuje izvješćivanje utemeljen na GPS mjesto |

Počevši od Android M [neke dozvole upravlja prilikom izvođenja](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Ako već koristite ``ACCESS_FINE_LOCATION``, a zatim ne morate koristiti ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Mogućnosti android manifesta konfiguracije

### <a name="crash-report"></a>Pasti izvješća

Da biste onemogućili rušenje izvješća, dodati kod između na `<application>` i `</application>` oznaka:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Brzi niz praga.

Prema zadanim postavkama, izvješća o usluzi radnje zapisnike u stvarnom vremenu. Ako je vaše aplikacije izvješća zapisnika razlikuju se često bolje je da biste međuspremnika zapisnike, a da biste prijavili ih odjednom u pravilnim vremenskim base (pod nazivom "koji se Brzi niz i načinu"). Da biste to učinili, dodajte kod između na `<application>` i `</application>` oznaka:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Način neprekidnog rada malo povećava trajanja baterija ali ima utjecaj na monitoru radnje: sve sesije i zadacima trajanje se zaokružuje praga neprekidnog rada (dakle, sesije i zadacima kraće nego praga neprekidnog rada možda neće biti vidljiva). Prag vaše neprekidnog rada mora biti dulji od 30000 (30s).

### <a name="session-timeout"></a>Vremensko ograničenje

 Pritiskom na tipku **Home** ili **ponovno** postavljanjem neaktivan telefonu ili u drugu aplikaciju možete završiti aktivnost. Prema zadanim postavkama sesije je završen deset sekundi nakon završetka zadnje aktivnosti. Taj se način izbjegavaju podjelu sesije svaki put korisnik zatvori pa brzo vraća aplikacije koje možete događa kada korisnik odabire sliku, provjerava obavijesti, itd. Preporučujemo vam da biste izmijenili taj parametar. Da biste to učinili, dodajte kod između na `<application>` i `</application>` oznaka:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Onemogućivanje izvještavanja o zapisnika

### <a name="using-a-method-call"></a>Pomoću metode poziva

Ako želite da se radnje prestanu slati zapisnike, da biste uputili poziv:

    EngagementAgent.getInstance(context).setEnabled(false);

U ovom poziv je stalni: koristi zajednički preference datoteke.

Ako radnje aktivan prilikom pozivanja tu funkciju, može proći jedne minute za uslugu da biste zaustavili. No ga neće pokrenuti servis uopće sljedeći put pokrenete aplikaciju.

Možete omogućiti zapisnika izvješćivanja ponovno tako da nazovete istu funkciju s `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integracija u vlastiti`PreferenceActivity`

Umjesto da nazovete podršku za tu funkciju, možete i integrirati tu postavku izravno u postojeće `PreferenceActivity`.

Možete konfigurirati radnje da biste koristili osobne postavke datoteku (željeni način) u `AndroidManifest.xml` datoteka `application meta-data`:

-   Na `engagement:agent:settings:name` ključ se koristi za definiranje naziva datoteke zajednički Preferences (Preference).
-   Na `engagement:agent:settings:mode` ključ služi da biste odredili način preference zajedničke datoteke. Koristiti isti način kao u vašem `PreferenceActivity`. Način rada mora proslijediti kao broj: Ako koristite kombinaciju konstantnoj zastavice u kodu, potvrdite okvir ukupne vrijednosti.

Radnje uvijek koristi u `engagement:key` Booleova ključ unutar datoteke preference za upravljanje tu postavku.

Sljedeći primjer `AndroidManifest.xml` prikazuje zadane vrijednosti:

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

Možete dodati na `CheckBoxPreference` u izgled preferenca poput sljedećih:

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
