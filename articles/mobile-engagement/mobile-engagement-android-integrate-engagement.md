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

#<a name="how-to-integrate-engagement-on-android"></a>Integraciji radnje u sustavu Android

> [AZURE.SELECTOR]
- [Univerzalni za Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Ovaj postupak opisuje najjednostavniji način da biste aktivirali analize radnje korisnika te praćenje funkcije u aplikaciji za računala sa sustavom Android.

> [AZURE.IMPORTANT] Android SDK API razinu zaštite minimalne mora biti 10 ili noviji (Android 2.3.3 ili noviji).

Sljedeći koraci nisu dovoljno za aktivira izvješća zapisnika potrebne za izračun svih statističkih podataka vezanih uz korisnika, sesije, aktivnosti, ruši i Technicals. Izvješće zapisnika potrebne za izračunavanje ostalih statističkih podataka kao što su događaji, pogrešaka i poslove se mora poduzeti ručno pomoću radnje API-JA (pogledajte [upute za korištenje dodatne radnje Mobile označavanje API na uređaju sa sustavom Android](mobile-engagement-android-use-engagement-api.md) jer te statistike aplikacije zavisne.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Ugrađivanje SDK radnje i servis za Android projekta

Preuzmite Android SDK iz [ovdje](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` i pohranili ih u na `libs` mape sa sustavom Android projekta (Stvori mapu libs ako ne postoji još).

> [AZURE.IMPORTANT]
> Ako stvarate Aplikacijski paket s ProGuard, morate zadržati neke klase. Možete koristiti sljedeće konfiguracije isječak:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Odredite niz za povezivanje radnje tako da nazovete sa sljedećim korakom u pokretač aktivnosti:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Niz za povezivanje za svoju aplikaciju prikazat će se na Portal za Azure.

-   Ako nema, dodajte sljedeće dozvole sa sustavom Android (prije se `<application>` oznaka):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Dodavanje u sljedećoj sekciji (između na `<application>` i `</application>` oznake):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Promjena `<Your application name>` po nazivu aplikacije.

> [AZURE.TIP] Na `android:label` atribut možete odabrati naziv servisa radnje onako kako će izgledati krajnjim korisnicima na zaslonu "Pokretanje servisa" telefon. Preporučuje se da biste postavili taj atribut na `"<Your application name>Service"` (npr. `"AcmeFunGameService"`).

Određivanje u `android:process` atribut osigurava da servis za radnje funkcionirat će u vlastitom procesa (izvodi radnje u na isti način kao što je aplikacija će postati vaše glavne/korisničkog Sučelja niti potencijalno reagiraju).

> [AZURE.NOTE] Kod stavite u `Application.onCreate()` i druge pozive aplikacija će pokrenuti za sve svoje aplikacije postupke, uključujući servis radnje. Može imati neželjene popratne pojave (kao što je alokacija nepotrebne memorije i niti u tijeku s mogućnostima, duplicirane emitiranje primatelja ili usluge).

Ako ste nadjačati `Application.onCreate()`, preporučuje se dodavanje sljedeće isječak koda na početku vaše `Application.onCreate()` funkcija:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Na isti način možete učiniti za `Application.onTerminate()`, `Application.onLowMemory()` i `Application.onConfigurationChanged(...)`.

Možete proširiti `EngagementApplication` umjesto proširivanje `Application`: povratnog `Application.onCreate()` ne potvrdite postupak i pozive `Application.onApplicationProcessCreate()` samo ako trenutni proces nije jednak onome hostiranje na radnje, primijenite ista pravila za ostale pozive.

##<a name="basic-reporting"></a>Osnovni izvješćivanje o pogreškama

### <a name="recommended-method-overload-your-activity-classes"></a>Preporučeni način: preopterećenja vaše `Activity` klase

Da biste aktivirali izvješća sve zapisnike potrebnih radnje za izračunavanje korisnika, sesije, aktivnosti, ruši i tehničke Statistika, imate da biste sve svoje `*Activity` podređenu klase nasljeđuju od odgovarajući `Engagement*Activity` klase (npr. Ako naslijeđene aktivnosti proširuje `ListActivity`, provjerite da se proteže `EngagementListActivity`).

**Bez radnje:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**S mogućnostima:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Prilikom korištenja `EngagementListActivity` ili `EngagementExpandableListActivity`, provjerite je li nešto poziva da biste `requestWindowFeature(...);` izvršite prije poziv `super.onCreate(...);`, u suprotnom će doći do neočekivanog.

Možete pronaći ove klase u na `src` mapu, a možete ih kopirati u projektu. Klasa su i **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternativni metoda: poziva `startActivity()` i `endActivity()` ručno

Ako ne možete ili ne želite preopterećenja vaše `Activity` klase, umjesto toga možete početka i završetka aktivnosti tako da nazovete `EngagementAgent`, izravno metode.

> [AZURE.IMPORTANT] Android SDK nikad ne poziva na `endActivity()` način, čak i kad zatvaranja (android, programi zapravo nikad zatvoreni). Stoga je *vrlo* preporučuje se da biste uputili poziv na `startActivity()` metoda u na `onResume` povratnog od *svih* aktivnosti, i `endActivity()` metoda u na `onPause()` povratnog od *svih* aktivnosti. To je jedini način da biste bili sigurni da sesije će biti leaked. Ako je leaked sesije, servis radnje nikad prekid pozadinskog radnje (jer je servis ostaje povezana dok god je sesije na čekanju).

Evo jednog primjera:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

U ovom se primjeru slično je na `EngagementActivity` predmete i njegov varijante čije izvorni kod ugrađen u na `src` mapu.

##<a name="test"></a>Test

Sada provjerite je li vaša Integracija pokretanje aplikacije za mobilne uređaje u emulator ili uređaja i provjera znači da registrira sesije na kartici Monitor.

U sljedećim odjeljcima nije obavezno.

##<a name="location-reporting"></a>Izvješćivanje o pogreškama mjesto

Ako želite mjesta da biste se prijavili, morate dodati nekoliko redaka konfiguracije (između na `<application>` i `</application>` oznake).

### <a name="lazy-area-location-reporting"></a>Izvješćivanje o pogreškama drži područje mjesto

Izvješćivanje o pogreškama drži područje mjesto omogućuje da biste prijavili država, regija i kraj povezan s uređajima. Ovu vrstu izvješćivanja o mjestu koristi samo mrežna mjesta (na temelju ID-a u ćeliju ili Wi-Fi veze). Područje uređaj prijavljuje najviše jednom po sesiji. U GPS nikada niste koristili i ovu vrstu izvješća mjesto ima vrlo mali (ne želite reći bez) utjecaj na baterija.

Prijavljenim područja se koriste za izračun geografske statistike o korisnika, sesije, događaji i pogreške. Možete također se koriste kao kriterij u razgovor kampanja.

Da biste omogućili izvješćivanja mjesto drži područja, to možete učiniti pomoću konfiguracije već navedeno u ovom postupku:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Morate ako nema, dodajte sljedeće dozvole:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Ili možete nastaviti koristiti ``ACCESS_FINE_LOCATION`` ako već koristite u aplikaciji.

### <a name="real-time-location-reporting"></a>Izvješćivanje o mjestu u stvarnom vremenu

Izvješćivanje o mjestu u stvarnom vremenu omogućuje da biste prijavili zemljopisnoj širini i dužini povezan s uređajima. Prema zadanim postavkama ovu vrstu izvješćivanja o mjestu koristi samo mrežna mjesta (na temelju ID-a u ćeliju ili Wi-Fi veze), a prijavljivanje aktivan samo kad je program pokrenut u planu (odnosno tijekom sesije).

Mjesta u stvarnom vremenu trenutno *ne* koristi za izračun statistike. Svoj razlog samo je omogućiti korištenje zemlj fencing stvarnom vremenu \<razgovor publike geofencing\> kriterij u razgovor kampanja.

Da biste omogućili stvarnom vremenu mjesto izvješćivanja, to možete učiniti pomoću konfiguracije prethodno spomenute u ovaj postupak:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Morate ako nema, dodajte sljedeće dozvole:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Ili možete nastaviti koristiti ``ACCESS_FINE_LOCATION`` ako već koristite u aplikaciji.

#### <a name="gps-based-reporting"></a>GPS temelji izvješćivanje o pogreškama

Prema zadanim postavkama, izvješćivanje mjesto u stvarnom vremenu koristi samo temelji mrežna. Da biste omogućili korištenje GPS temelji mjesta (koje su znatno preciznije), pomoću konfiguracije objekta:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Morate ako nema, dodajte sljedeće dozvole:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Izvješćivanje o pogreškama pozadine

Prema zadanim postavkama, izvješćivanje mjesto u stvarnom vremenu aktivan samo prilikom pokretanja aplikacije u planu (odnosno tijekom sesije). Da biste omogućili izvješćivanje i u pozadini, koristite konfiguracije objekta:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Kada aplikacija izvodi u pozadini, samo mreže temelji mjesta prijavljene, čak i ako je omogućeno u GPS.

Mjesto izvješća pozadine će se zaustaviti ako korisnik izvršen njegov uređaj, možete dodati tako da bi se automatski ponovno pokrenite prilikom pokretanja:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Morate ako nema, dodajte sljedeće dozvole:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M dozvole

Počevši od Android M, neke dozvole upravlja se prilikom izvođenja i potrebno je odobrenje korisnika.

Dozvole za vrijeme izvođenja će se po zadanome isključen za nove instalacije aplikacija ako ciljani Android API razinu 23. U suprotnom je uključit će se po zadanom.

Korisnik može omogućiti ili onemogućiti te dozvole na izborniku postavke uređaja. Isključivanje dozvole iz izbornika sustava kills pozadinski procesi aplikacije, to je sustav ponašanje i ne utječe na mogućnost primanja automatske u pozadini.

U kontekstu radnje Mobile, dozvole koje je potrebno odobrenje prilikom izvođenja su sljedeće:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(samo kada određivanje razine Android API 23 ova)

Vanjsko pohranjivanje se koriste isključivo radi značajka širu sliku razgovor. Ako pronađete zatražiti korisnici tu dozvolu da je disruptive, možete ga ukloniti ako ste koristili samo za radnje Mobile, ali teret onemoguće širu sliku.

Za značajke mjesto treba zatražite dozvole za korisnika pomoću dijaloškog okvira standardne sustava. Ako korisnik odobrenje, morate znati ``EngagementAgent`` da bi ta promjena u obzir u stvarnom vremenu (u suprotnom promjena obrađuju sljedeći put korisnik pokreće aplikacije).

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
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Dodatna izvješća

Po želji, ako želite da biste prijavili aplikacije određene događaje, pogrešaka i zadacima, morate koristiti API radnje pomoću metode od na `EngagementAgent` predmete. Objekta te klase može biti retreived tako da nazovete podršku za `EngagementAgent.getInstance()` statične način.

API radnje omogućuje korištenje svih naprednih mogućnosti radnje korisnika i detaljne se u članku korištenje radnje API na Android (dobro, kao i u tehničku dokumentaciju o na `EngagementAgent` klase).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Napredna konfiguracija (u AndroidManifest.xml)

### <a name="wake-locks"></a>Aktivira zaključavanja

Ako želite da biste provjerili Statistika šalju u stvarnom vremenu tijekom korištenja Wi-Fi veze ili kada na zaslonu je isključen, dodajte sljedeće neobavezno dozvola:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Pasti izvješća

Ako želite onemogućiti rušenje izvješća, Dodaj (između na `<application>` i `</application>` oznake):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Brzi niz praga.

Prema zadanim postavkama, izvješća o usluzi radnje zapisnike u stvarnom vremenu. Ako aplikacija vrlo često izvješća zapisnika, bolje je da biste međuspremnika zapisnike, a da biste prijavili njih odjedanput na u pravilnim vremenskim base (to se naziva "način neprekidnog rada"). Da biste to učinili, dodajte to (između na `<application>` i `</application>` oznake):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Način neprekidnog rada malo povećati trajanja baterija, ali je utjecaj na monitoru radnje: sve sesije i zadacima trajanje će biti zaokruženi praga neprekidnog rada (dakle, sesije i zadacima kraće nego praga neprekidnog rada možda neće biti vidljiva). Preporučuje se da biste koristili praga neprekidnog rada više od 30000 (30s).

### <a name="session-timeout"></a>Vremensko ograničenje

Prema zadanim postavkama sesija je završen 10s nakon završetka njegov zadnja aktivnost (koji obično događa pritiskom na tipku Home ili natrag, postavljanjem telefona neaktivan ili skoka u neku drugu aplikaciju). To je da biste izbjegli podjelu sesije svaki put korisnički Izlaz i vratili se na aplikaciju vrlo brzo (što se događa kada on obraditi slike, pročitajte obavijesti, itd.). Preporučujemo vam da biste izmijenili taj parametar. Da biste to učinili, dodajte to (između na `<application>` i `</application>` oznake):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Onemogućivanje izvještavanja o zapisnika

### <a name="using-a-method-call"></a>Pomoću metode poziva

Ako želite da se radnje prestanu slati zapisnike, da biste uputili poziv:

            EngagementAgent.getInstance(context).setEnabled(false);

U ovom poziv je stalni: koristi zajednički preference datoteke.

Ako radnje aktivan prilikom pozivanja tu funkciju, može proći 1 minute za uslugu da biste zaustavili. No ga neće pokrenuti servis uopće sljedeći put pokrenete aplikaciju.

Možete omogućiti zapisnika izvješćivanja ponovno tako da nazovete istu funkciju s `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integracija u vlastiti`PreferenceActivity`

Umjesto da nazovete podršku za tu funkciju, možete i integrirati tu postavku izravno u postojeće `PreferenceActivity`.

Možete konfigurirati radnje da biste koristili osobne postavke datoteku (željeni način) u `AndroidManifest.xml` datoteka `application meta-data`:

-   Na `engagement:agent:settings:name` ključ se koristi za definiranje naziva datoteke zajednički Preferences (Preference).
-   Na `engagement:agent:settings:mode` ključ služi da biste odredili način preference zajedničke datoteke, trebali biste koristiti isti način kao u vašem `PreferenceActivity`. Način rada mora proslijediti kao broj: Ako koristite kombinaciju konstantnoj zastavice u kodu, potvrdite okvir ukupne vrijednosti.

Korištenje uvijek radnje u `engagement:key` Booleova ključ unutar datoteke preference za upravljanje tu postavku.

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

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
