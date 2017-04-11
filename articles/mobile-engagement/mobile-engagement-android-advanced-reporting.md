<properties
    pageTitle="Dodatne mogućnosti izvješćivanja za Azure Mobile radnje sa sustavom Android SDK"
    description="U članku se opisuje kako naprednih izvješća da biste snimili analytics za Azure Mobile radnje sa sustavom Android SDK"
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
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Dodatna izvješća s mogućnostima na sustavu Android

> [AZURE.SELECTOR]
- [Univerzalni sustava Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

U ovoj se temi opisuju dodatne scenarije izvješćivanja u aplikaciji za računala sa sustavom Android. Te mogućnosti možete primijeniti na aplikaciju stvorili praktičnog vodiča za [Početak rada](mobile-engagement-android-get-started.md) .

## <a name="prerequisites"></a>Preduvjeti

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Praktični vodič što dovršili je namjerno Izravni i jednostavno, ali su Napredne mogućnosti možete odabrati.

## <a name="modifying-your-activity-classes"></a>Izmjena na `Activity` klase

U [Vodič za početak rada](mobile-engagement-android-get-started.md), svi morali učinite je da bi vaše `*Activity` podklase nasljeđuju od odgovarajući `Engagement*Activity` klase. Na primjer, ako je prošireno naslijeđene aktivnosti `ListActivity`, bi ga proširili `EngagementListActivity`.

> [AZURE.IMPORTANT] Prilikom korištenja `EngagementListActivity` ili `EngagementExpandableListActivity`, provjerite je li nešto poziva da biste `requestWindowFeature(...);` izvršite prije poziv `super.onCreate(...);`, u suprotnom se pojavljuje neočekivanog.

Možete pronaći ove klase u na `src` mapu, a možete ih kopirati u projektu. Klasa su i **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternativni metoda: poziva `startActivity()` i `endActivity()` ručno

Ako ne možete ili ne želite preopterećenja vaše `Activity` klase, umjesto toga možete početka i završetka aktivnosti tako da nazovete na `EngagementAgent`, izravno metode.

> [AZURE.IMPORTANT] Android SDK nikad ne poziva na `endActivity()` način, čak i kad zatvaranja (android, programi nikad zatvoreni). Stoga je *vrlo* preporučuje se da biste uputili poziv na `startActivity()` metoda u na `onResume` povratnog od *svih* aktivnosti, i `endActivity()` metoda u na `onPause()` povratnog od *svih* aktivnosti. To je jedini način da biste bili sigurni da sesije su leaked. Ako je leaked sesije, servis radnje nikad ne prekida vezu s pozadinskom radnje (jer je servis ostaje povezana dok god je sesije na čekanju).

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

Ovaj primjer sličan je na `EngagementActivity` predmete i njegov varijante čije izvorni kod ugrađen u na `src` mapu.

## <a name="using-applicationoncreate"></a>Korištenje Application.onCreate()

Kod stavite u `Application.onCreate()` i pozive u drugoj aplikaciji se izvodi za sve svoje aplikacije postupke, uključujući servis radnje. Može imati neželjene strani efekata, kao što su alokacija nepotrebne memorije i niti u postupak u radnje ili duplicirane emitiranje primatelja ili usluge.

Ako ste nadjačati `Application.onCreate()`, preporučujemo da dodate sljedeće isječak koda na početku vaše `Application.onCreate()` funkcija:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Na isti način možete učiniti za `Application.onTerminate()`, `Application.onLowMemory()`, i `Application.onConfigurationChanged(...)`.

Možete proširiti `EngagementApplication` umjesto proširivanje `Application`: povratnog `Application.onCreate()` ne potvrdite postupak i pozive `Application.onApplicationProcessCreate()` samo ako trenutni proces nije jednak onome hostiranje na radnje, primijenite ista pravila za ostale pozive.

## <a name="tags-in-the-androidmanifestxml-file"></a>Oznake u datoteci AndroidManifest.xml

U servis oznake u datoteci AndroidManifest.xml u `android:label` atribut možete odabrati naziv servisa radnje kao što je prikazano krajnjim korisnicima na zaslonu "Pokretanje servisa" telefon. Preporučujemo da postavku taj atribut `"<Your application name>Service"` (na primjer, `"AcmeFunGameService"`).

Određivanje u `android:process` atribut osigurava servis radnje pokreće se u vlastitom procesa (izvodi radnje u na isti način kao što je aplikacija čini vaš glavne/korisničkog Sučelja niti potencijalno reagiraju).

## <a name="building-with-proguard"></a>Stvaranje s ProGuard

Ako stvarate Aplikacijski paket s ProGuard, morate zadržati neke klase. Možete koristiti sljedeće konfiguracije isječak:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
