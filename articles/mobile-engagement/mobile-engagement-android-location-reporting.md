<properties
    pageTitle="Mjesto navedeno za Azure mobilne radnje SDK za Android"
    description="U članku se opisuje kako konfigurirati mjesto navedeno za Azure Mobile radnje sa sustavom Android SDK"
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Mjesto navedeno za Azure mobilne radnje SDK za Android

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

U ovoj se temi opisuje kako mjesto navedeno za svoju aplikaciju za Android.

## <a name="prerequisites"></a>Preduvjeti

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Izvješćivanje o pogreškama mjesto

Ako želite mjesta da biste se prijavili, morate dodati nekoliko redaka konfiguracije (između na `<application>` i `</application>` oznake).

### <a name="lazy-area-location-reporting"></a>Izvješćivanje o pogreškama drži područje mjesto

Izvješćivanje o pogreškama drži područje mjesto omogućuje izvješćivanja država, regija i kraj povezan s uređaja. Ovu vrstu izvješćivanja o mjestu koristi samo mrežna mjesta (na temelju ID-a u ćeliju ili Wi-Fi veze). Područje uređaj prijavljuje najviše jednom po sesiji. U GPS nikada niste koristili, a ta vrsta izvješća mjesto ima malo utjecaj na baterija.

Prijavljenim područja se koriste za izračun geografske statističkih podataka o korisnicima, sesije, događaji i pogreške. Možete također se koriste kao kriterij u razgovor kampanja.

Omogućivanje lokacija drži područja Izvješćivanje pomoću konfiguracije prethodno spomenute u ovaj postupak:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Morate navesti mjesto dozvola. Kod koristi ``COARSE`` dozvola:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Ako aplikaciju zahtijeva, možete koristiti ``ACCESS_FINE_LOCATION`` umjesto toga.

### <a name="real-time-location-reporting"></a>Izvješćivanje o pogreškama u stvarnom vremenu mjesto

Izvješćivanje o pogreškama u stvarnom vremenu lokacije omogućuje izvješćivanje o zemljopisnoj širini i dužini povezan s uređaja. Ovu vrstu izvješćivanja mjesto po zadanom koristi samo mrežna mjesta, na temelju ID-a u ćeliju ili Wi-Fi veze. Prijavljivanje aktivan samo kad je program pokrenut u planu (primjerice, tijekom sesije).

U stvarnom vremenu mjesta se *ne* koriste za izračun statistike. Svoj razlog samo je omogućiti korištenja u stvarnom vremenu fencing zemlj \<razgovor publike geofencing\> kriterij u razgovor kampanja.

Da biste omogućili u stvarnom vremenu mjesto izvješćivanja, dodajte redak koda gdje postaviti niz za povezivanje radnje u pokretač aktivnosti. Rezultat izgleda ovako:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS temelji izvješćivanje o pogreškama

Prema zadanim postavkama, izvješćivanje o pogreškama u stvarnom vremenu mjesto koristi samo mjesta utemeljen na mreži. Da biste omogućili korištenje utemeljen na GPS mjesta, koje su znatno preciznije, koristite konfiguracije objekta:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Morate ako nema, dodajte sljedeće dozvole:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Izvješćivanje o pogreškama pozadine

Prema zadanim postavkama, izvješćivanje o pogreškama u stvarnom vremenu mjesto aktivan samo kad je program pokrenut u planu (primjerice, tijekom sesije). Da biste omogućili izvješćivanje i u pozadini, koristite ovaj objekt konfiguracije:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Kada je program pokrenut u pozadini, samo utemeljen na mreži mjesta prijavljene, čak i ako je omogućeno u GPS.

Ako korisnik izvršen svoj uređaj, mjesto izvješća pozadine se zaustaviti. Da biste automatski ponovno pokrenite prilikom pokretanja, dodati kod.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Morate ako nema, dodajte sljedeće dozvole:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M dozvole

Počevši od Android M, neke dozvole upravlja se prilikom izvođenja i potrebno odobrenje korisnika.

Ako ciljani Android API razinu 23, dozvole za vrijeme izvođenja isključeni su kao zadano za nove instalacije aplikacije. U suprotnom oni su po zadanom uključen.

Koje mogu omogućiti ili onemogućiti te dozvole na izborniku postavke uređaja. Isključivanje dozvole na izborniku sustava kills pozadinski procesi aplikacije koje se ponašanje sustav i ne utječe na mogućnost primanja automatske u pozadini.

U kontekstu Mobile radnje mjesto izvješćivanja, dozvole koje je potrebno odobrenje prilikom izvođenja su sljedeće:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Dozvole zatražite od korisnika pomoću dijaloškog okvira standardne sustava. Ako korisnik odobrenje, recite ``EngagementAgent`` da bi ta promjena u obzir u stvarnom vremenu. U suprotnom promjena odvija se sljedeći put korisnik pokreće aplikaciju.

Slijedi primjer kod da biste koristili u aktivnosti vaše aplikacije zatražite dozvole i prosljeđivanje rezultat ako je pozitivan da biste ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
