<properties
    pageTitle="Upute za korištenje radnje API na Android"
    description="Najnovije SDK sa sustavom Android – upute za korištenje radnje API na Android"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Upute za korištenje radnje API na Android

Dokument je dodatak dokument [Napredne izvješćivanja mogućnosti za Android SDK Mobile radnje](mobile-engagement-android-advanced-reporting.md). Pruža dubine detalje o načinu korištenja API radnje da biste prijavili statistici aplikacije.

Imajte na umu da ako želite samo radnje da biste prijavili vaše aplikacije sesije, aktivnosti, ruši i tehničke informacije, zatim najjednostavnije je da biste sve svoje `Activity` podređenu klase nasljeđuju od odgovarajući `EngagementActivity` predmete.

Ako želite dodatnu, na primjer ako morate aplikacije određene događaje, pogrešaka i zadacima, izvješća ili ako imate da biste prijavili aktivnosti vaše aplikacije na drugačiji način od onog implementirana u na `EngagementActivity` klasa iz registra, zatim morate koristiti API radnje.

Nudi API radnje u `EngagementAgent` predmete. Instance komponente klase mogu biti dohvaćeni tako da nazovete na `EngagementAgent.getInstance(Context)` statične način (Imajte na umu da u `EngagementAgent` objekta, vraća se na jednočlana).

##<a name="engagement-concepts"></a>Pojmovi radnje

Sljedeći dijelovi suzite uobičajenih [Koncepata radnje Mobile](mobile-engagement-concepts.md), za platforme Android.

### <a name="session-and-activity"></a>`Session`i`Activity`

Ako korisnik ostaje neaktivnosti između dvije *aktivnosti*više od nekoliko sekundi, njegov nizu *aktivnosti* je podijeljen u dva različita *sesije*. Ove nekoliko sekundi prije nego se nazivaju "sesiju vremensko ograničenje".

*Aktivnosti* obično je povezana s jedan zaslon aplikacije, što znači *aktivnosti* počinje na zaslonu se prikazuje i zaustavlja kad je zatvoren zaslona: to je to slučaj kada SDK radnje integriran pomoću na `EngagementActivity` klase.

No *aktivnosti* i upravlja ručno pomoću API radnje. Time da biste podijelili na navedeni zaslon u nekoliko dijelova sub da biste vidjeli dodatne detalje o korištenju zaslon (na primjer poznati učestalosti i koliko koriste dijaloški okviri unutar ovaj zaslon).

##<a name="reporting-activities"></a>Izvješćivanje o aktivnosti

> [AZURE.IMPORTANT] Ne morate izvješća aktivnosti kao što je opisano u ovom odjeljku ako koristite na `EngagementActivity` predmete i njegov varijante kao što je opisano u kako integrirati radnje na Android dokumentu.

### <a name="user-starts-a-new-activity"></a>Korisnik pokrene novu aktivnost

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Morate nazvati `startActivity()` svaki put kada se promijeni aktivnosti korisnika. Prvi poziv Ova funkcija pokreće novu sesiju korisnika.

Najbolje da biste nazvali Ova funkcija je na svakoj aktivnosti `onResume` povratnog.

### <a name="user-ends-his-current-activity"></a>Korisnik završava svoje trenutne aktivnosti

            EngagementAgent.getInstance(this).endActivity();

Morate nazvati `endActivity()` barem jednom kada korisnik Završi svoje zadnje aktivnosti. To SDK radnje obavještava da je korisnik trenutno neaktivne, a koje korisnik sesije potrebno zatvoriti jednom vremensko ograničenje sesije će isteći (Ako pozovete `startActivity()` prije no što istekne vremensko ograničenje sesije sesiju jednostavno nastaviti).

Najbolje da biste nazvali Ova funkcija je na svakoj aktivnosti `onPause` povratnog.

##<a name="reporting-events"></a>Izvješćivanje o događajima

### <a name="session-events"></a>Sesije događaja

Sesije događaje obično koriste se za izvješća radnji koja korisnik svoj sesiji.

**Primjer bez dodatnih podataka:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Primjer s dodatni podaci:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Samostalne događaja

Contrary to događaje sesiju samostalne događaja može se pojaviti izvan konteksta sesije.

**Primjer:**

Pretpostavimo da želite događaja izvješće kada se pokrene emitiranje tekstnog okvira:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Izvješćivanje o pogreškama

### <a name="session-errors"></a>Sesije pogreške

Sesije pogreške obično koriste se za pogreške koje utječu na korisnik svoj sesiji.

**Primjer:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Samostalne pogreške

Contrary to sesiju pogreške, samostalne pogreške se mogu pojaviti izvan konteksta sesije.

**Primjer:**

Sljedeći primjer prikazuje način da biste prijavili pogrešku kad god memorije ponestane na telefonu dok je pokrenut proces aplikacije.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Izvješćivanje o zadacima

### <a name="example"></a>Primjer

Pretpostavimo da želite prijaviti trajanja postupka prijave:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Izvješće o pogreškama pri tijekom posla

Pogreške se može povezati s izvodi posla umjesto koji se odnose na trenutnoj sesiji korisnika.

**Primjer:**

Pretpostavimo da želite prijaviti pogreške tijekom koje prijava postupak:

[...] Javni Poništi prijava (kontekst kontekst,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Izvješćivanje o pogreškama događaji tijekom posla

Događaji se može povezati s izvodi posla umjesto koji se odnose na trenutnoj sesiji korisnika.

**Primjer:**

Pretpostavimo da imamo društvenih mreža i koristimo posao izvješće Ukupno vrijeme tijekom kojeg je korisnik povezan s poslužiteljem. Korisnik može Ostanite povezani u pozadini, čak i kada on koristi neku drugu aplikaciju ili telefona je u stanju mirovanja, pa nema sesiju.

Korisnik poruke možete primati od postane prijateljima, to je posao događaj.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Dodatni parametri

Proizvoljne podataka mogu priložiti događaje, pogreške, aktivnosti i zadacima.

Ove podatke možete strukturirane, koristi Android na paket klase (zapravo funkcionira kao što su dodatni parametri u Android reprodukcije). Imajte na umu da je paket mogu sadržavati polja ili neki drugi paket instance.

> [AZURE.IMPORTANT] Ako ste postavili parcelable ili serializable parametara, provjerite je li njihove `toString()` način je implementirana za vraćanje Ljudski čitljiv niza. Serializable klase koje sadrže koje nisu tranzitne polja koja nisu serializable će postati Android rušenje kada će poziv`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Kratke polja u dodatni parametri nisu podržane, tj., on neće serijalizirani kao polja. Trebali biste pretvorite ih u standardne polja prije korištenja u dodatni parametri.

### <a name="example"></a>Primjer

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Ograničenja

#### <a name="keys"></a>Tipke

Svaki ključ u na `Bundle` moraju se podudarati sljedeće uobičajenog izraza:

`^[a-zA-Z][a-zA-Z_0-9]*`

To znači da tipke mora počinjati barem jedno slovo, nakon čega slijedi slova, brojki ili podvlaka (\_).

#### <a name="size"></a>Veličina

Dodaci ograničeni su na **1024** znakova po poziva (jednom kodirana JSON servis radnje).

U prethodnom primjeru, JSON poslane na poslužitelj je 58 znakova:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Informacije o aplikaciji za izvješćivanje o pogreškama

Možete ručno prijaviti za praćenje podataka (ili bilo koje druge aplikacije određene informacije) pomoću na `sendAppInfo()` (opis funkcije).

Imajte na umu da se ove informacije možete slati postupno: samo najkasnija vrijednost za dani ključ će biti zadržane za zadani uređaj.

Kao što su dodaci događaj klase paket koristi se za apstraktnog informacije o aplikaciji, imajte na umu da se polja ili podmjesta objedinjuje tretira kao paušalni nizove (pomoću JSON serijalizacije).

### <a name="example"></a>Primjer

Slijedi primjer kod da biste poslali spol korisnika i DatumRođenja:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Ograničenja

#### <a name="keys"></a>Tipke

Svaki ključ u na `Bundle` moraju se podudarati pravilnim sljedeći izraz:

`^[a-zA-Z][a-zA-Z_0-9]*`

To znači da tipke mora počinjati barem jedno slovo, nakon čega slijedi slova, brojki ili podvlaka (\_).

#### <a name="size"></a>Veličina

Informacije o aplikaciji ograničeni su na **1024** znakova po poziva (jednom kodirana JSON servis radnje).

U prethodnom primjeru, JSON poslane na poslužitelj je 44 znakova:

            {"expiration":"2016-12-07","status":"premium"}
