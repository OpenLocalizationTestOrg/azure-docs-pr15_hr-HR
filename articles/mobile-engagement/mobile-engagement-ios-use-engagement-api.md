<properties
    pageTitle="Upute za korištenje radnje API na iOS"
    description="Najnovije iOS SDK – upute za korištenje radnje API na iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>Upute za korištenje radnje API na iOS

Dokument je dodatak dokumentu kako integrirati radnje na iOS: pruža dubine detalje o načinu korištenja API radnje da biste prijavili statistici aplikacije.

Imajte na umu da ako želite samo radnje da biste prijavili vaše aplikacije sesije, aktivnosti, ruši i tehničke informacije, zatim najjednostavnije je da biste sve prilagođene `UIViewController` objekti nasljeđuju iz odgovarajućeg `EngagementViewController` predmete.

Ako želite dodatnu, na primjer ako morate aplikacije određene događaje, pogrešaka i zadacima, izvješća ili ako imate da biste prijavili aktivnosti vaše aplikacije na drugačiji način od onog implementirana u na `EngagementViewController` klasa iz registra, zatim morate koristiti API radnje.

Pruža API radnje u `EngagementAgent` predmete. Instance komponente klase mogu biti dohvaćeni tako da nazovete na `[EngagementAgent shared]` statične način (Imajte na umu da u `EngagementAgent` objekta, vraća se na jednočlana).

Prije nego što sve API poziva na `EngagementAgent` objekt mora biti pokrenut tako da nazovete metodu`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Pojmovi radnje

Sljedeći dijelovi suzite uobičajenih [Koncepata Mobile radnje](mobile-engagement-concepts.md) za iOS platformu.

### <a name="session-and-activity"></a>`Session`i`Activity`

*Aktivnosti* obično je povezana s jedan zaslon aplikacije, što znači *aktivnosti* počinje na zaslonu se prikazuje i zaustavlja kad je zatvoren zaslona: to je to slučaj kada SDK radnje integriran pomoću na `EngagementViewController` klase.

No *aktivnosti* i upravlja ručno pomoću API radnje. Time da biste podijelili na navedeni zaslon u nekoliko dijelova sub da biste vidjeli dodatne detalje o korištenju zaslon (na primjer poznati učestalosti i koliko koriste dijaloški okviri unutar ovaj zaslon).

##<a name="reporting-activities"></a>Izvješćivanje o aktivnosti

### <a name="user-starts-a-new-activity"></a>Korisnik pokrene novu aktivnost

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Morate nazvati `startActivity()` svaki put kada se promijeni aktivnosti korisnika. Prvi poziv Ova funkcija pokreće novu sesiju korisnika.

### <a name="user-ends-his-current-activity"></a>Korisnik završava svoje trenutne aktivnosti

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Koje treba **NIKAD** poziv Ova funkcija sami, osim ako želite podijeliti iskoristili vaše aplikacije u nekoliko sesija: poziv za tu funkciju bi završili trenutna sesija odmah, dakle, sljedeći poziv `startActivity()` bi pokrenite novu sesiju. Ova funkcija je automatski pozove SDK kad je zatvoren aplikacije.

##<a name="reporting-events"></a>Izvješćivanje o događajima

### <a name="session-events"></a>Sesije događaja

Sesije događaje obično koriste se za izvješća radnji koja korisnik svoj sesiji.

**Primjer bez dodatnih podataka:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Primjer s dodatni podaci:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Samostalne događaja

Contrary to događaje sesiju samostalne događaja može se koristiti izvan konteksta sesije.

**Primjer:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Izvješćivanje o pogreškama

### <a name="session-errors"></a>Sesije pogreške

Sesije pogreške obično koriste se za pogreške koje utječu na korisnik svoj sesiji.

**Primjer:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Samostalne pogreške

Contrary to sesiju pogreške, samostalne pogrešaka može se koristiti izvan konteksta sesije.

**Primjer:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Izvješćivanje o zadacima

**Primjer:**

Pretpostavimo da želite prijaviti trajanja procesa prijave:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Izvješće o pogreškama pri tijekom posla

Pogreške se može povezati s izvodi posla umjesto koji se odnose na trenutnoj sesiji korisnika.

**Primjer:**

Pretpostavimo da želite prijaviti pogreške tijekom prijave:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Događaji tijekom posla

Događaji se može povezati s izvodi posla umjesto koji se odnose na trenutnoj sesiji korisnika.

**Primjer:**

Pretpostavimo da imamo društvenih mreža i koristimo posao izvješće Ukupno vrijeme tijekom kojeg je korisnik povezan s poslužiteljem. Korisnik poruke možete primati od postane prijateljima, to je posao događaj.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Dodatni parametri

Proizvoljne podataka mogu priložiti događaje, pogreške, aktivnosti i zadacima.

Ove podatke možete strukturirane, koristi iOS, NSDictionary predmete.

Imajte na umu da mogu sadržavati dodataka `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` ili nekog drugog `NSDictionary` instance.

> [AZURE.NOTE] Dodatni parametar Serijalizirano je u JSON. Ako želite proslijediti druge objekte od onih navedenih u svojoj učionici morate provesti na sljedeći način:
>
             -(NSString*)JSONRepresentation;
>
> Način mora vratiti JSON predstavljanje objekta.

### <a name="example"></a>Primjer

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Ograničenja

#### <a name="keys"></a>Tipke

Svaki ključ u na `NSDictionary` moraju se podudarati sljedeće uobičajenog izraza:

`^[a-zA-Z][a-zA-Z_0-9]*`

To znači da tipke mora počinjati barem jedno slovo, nakon čega slijedi slova, brojki ili podvlaka (\_).

#### <a name="size"></a>Veličina

Dodaci ograničeni su na **1024** znakova po poziva (jednom kodirana JSON po agent radnje).

U prethodnom primjeru, JSON poslane na poslužitelj je 58 znakova:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Informacije o aplikaciji za izvješćivanje o pogreškama

Možete ručno prijaviti za praćenje podataka (ili bilo koje druge aplikacije određene informacije) pomoću na `sendAppInfo:` (opis funkcije).

Imajte na umu da se ove informacije možete slati postupno: samo najkasnija vrijednost za dani ključ će biti zadržane za zadani uređaj.

Dodaci za događaj, kao što su u `NSDictionary` klase se koristi za apstraktnog informacije o aplikaciji, imajte na umu da se polja ili podmjesta rječnici tretira kao paušalni nizovi (pomoću JSON serijalizacije).

**Primjer:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Ograničenja

#### <a name="keys"></a>Tipke

Svaki ključ u na `NSDictionary` moraju se podudarati sljedeće uobičajenog izraza:

`^[a-zA-Z][a-zA-Z_0-9]*`

To znači da tipke mora počinjati barem jedno slovo, nakon čega slijedi slova, brojki ili podvlaka (\_).

#### <a name="size"></a>Veličina

Informacije o aplikaciji ograničeni su na **1024** znakova po poziva (jednom kodirana JSON po agent radnje).

U prethodnom primjeru, JSON poslane na poslužitelj je 44 znakova:

    {"birthdate":"1983-12-07","gender":"female"}
