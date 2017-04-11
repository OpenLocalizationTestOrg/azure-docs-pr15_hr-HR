<properties
    pageTitle="Azure mobilne radnje Web SDK API-ji | Microsoft Azure"
    description="Najnovija ažuriranja i postupci Web SDK za Azure Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Korištenje mobilne API radnje za Azure u web-aplikaciji

Dokument je dodatak za dokument koji pokazuje kako [integrirati Mobile radnje u web-aplikaciji](mobile-engagement-web-integrate-engagement.md). Pruža detaljne upute kako koristiti radnje API-JA Mobile Azure da biste prijavili statistici aplikacije.

Radnje API Mobile nudi na `engagement.agent` objekt. Zadani Azure Mobile radnje Web SDK pseudonim je `engagement`. Ponovno možete definirati pseudonima iz SDK konfiguracije.

## <a name="mobile-engagement-concepts"></a>Korištenje mobilne koncepti

Sljedeći dijelovi suzite uobičajenih [koncepata Mobile radnje](mobile-engagement-concepts.md) za platformu web.

### <a name="session-and-activity"></a>`Session`i`Activity`

Ako korisnik ostaje neaktivnosti više od nekoliko sekundi između dvije aktivnosti, redoslijed korisničke aktivnosti dijeli na dva različita sesije. Ove nekoliko sekundi prije nego se nazivaju vremensko ograničenje sesije.

Ako web-aplikacija ne deklarirati kraj aktivnosti korisnika samostalno (tako da nazovete na `engagement.agent.endActivity` funkcija), poslužitelj Mobile radnje automatski ističe korisničke sesije u tri minute nakon zatvaranja aplikacije stranice. To se naziva vremensko poslužitelja.

### `Crash`

Po zadanom se stvaraju automatski izvješća je neuhvaćenu JavaScript iznimke. Međutim, možete je prijaviti ruši ručno pomoću na `sendCrash` funkcija (u odjeljku reporting ruši).

## <a name="reporting-activities"></a>Izvješćivanje o aktivnosti

Izvješćivanje o pogreškama na aktivnosti korisnika sadrži kada korisnik pokrene novu aktivnost, a kada korisnik istekne trenutne aktivnosti.

### <a name="user-starts-a-new-activity"></a>Korisnik pokrene novu aktivnost

    engagement.agent.startActivity("MyUserActivity");

Morate nazvati `startActivity()` mijenja svaki put aktivnosti korisnika. Prvi poziv Ova funkcija pokreće novu sesiju korisnika.

### <a name="user-ends-the-current-activity"></a>Korisnik završava trenutne aktivnosti

    engagement.agent.endActivity();

Morate nazvati `endActivity()` barem jednom kada korisnik Završi njihove zadnja aktivnost. To je li korisnik trenutno neaktivne, a da korisničke sesije nije potrebno je zatvoriti nakon isteka vremensko ograničenje sesije obavještava SDK Mobile radnje Web. Ako pozovete `startActivity()` prije no što istekne vremensko ograničenje sesije, jednostavno nastaviti sesiju.

Jer nema pouzdanog poziv zatvaranja prozora navigator, često je teško ili nemoguće privući pažnju na kraj aktivnosti korisnika unutar web-okruženju. Zašto je poslužitelj Mobile radnje automatski ističe korisničke sesije u tri minute nakon zatvaranja aplikacije stranice.

## <a name="reporting-events"></a>Izvješćivanje o događajima

Izvješćivanje o događajima pokriva sesiju događaji i događaji samostalne.

### <a name="session-events"></a>Sesije događaja

Sesije događaje obično koristi da biste prijavili radnji koja korisnik sesiji korisnika.

**Primjer bez dodatnih podataka:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Primjer s dodatni podaci:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Samostalne događaja

Za razliku od sesija događaje, samostalne događaja može se pojaviti izvan konteksta sesije.

Da biste postigli, koristite ``engagement.agent.sendEvent`` umjesto ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Izvješćivanje o pogreškama

Izvješćivanje o greškama pokriva sesije pogreške i pogreške samostalne.

### <a name="session-errors"></a>Sesije pogreške

Sesije pogreške obično koristi da biste prijavili pogreške koje imati utjecaj na korisnike sesiji korisnika.

**Primjer bez dodatnih podataka:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Primjer s dodatni podaci:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Samostalne pogreške

Za razliku od pogrešaka sesiju samostalne pogreške se mogu pojaviti izvan konteksta sesije.

Da biste postigli, koristite `engagement.agent.sendError` umjesto `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Izvješćivanje o zadacima

Izvješćivanje o pogreškama na zadacima naslovnice pogrešaka i događaja koji se pojavljuju tijekom posla i izvješćivanje o ruši.

**Primjer:**

Ako želite nadzirati zahtjeva za razmjenu AJAX-a, koristit ćete sljedeće:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Izvješćivanje o pogreškama tijekom posla

Pogreške se može povezati s izvodi posao umjesto trenutnog korisnika sesiju.

**Primjer:**

Ako želite da biste prijavili pogrešku ako zahtjev za AJAX-a ne uspije:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Izvješćivanje o pogreškama događaji tijekom posla

Događaji koje je moguće povezati s izvodi posao umjesto u trenutnoj sesiji korisnika zahvale na `engagement.agent.sendJobEvent` (opis funkcije).

Ova funkcija radi točno poput `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Izvješćivanje o pogreškama ruši

Korištenje na `sendCrash` funkcija izvješće ruši ručno.

Na `crashid` argument je niz koji označava vrstu pad sustava.
Na `crash` argument obično je stanje stoga rušenje kao niz.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Dodatni parametri

Proizvoljne podataka možete priložiti događaj, pogreška, aktivnosti ili posao.

Podataka može biti bilo koji objekt JSON (ali ne zadanom polju ili jednostavne vrsta).

**Primjer:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Ograničenja

Ograničenja koji se odnose na dodatni parametri su područja regularne izraze za tipke, vrijednost vrste i veličine.

#### <a name="keys"></a>Tipke

Svaki ključ u objektu moraju se podudarati običnog sljedeći izraz:

    ^[a-zA-Z][a-zA-Z_0-9]*

To znači da se tipke morate pokrenuti s najmanje jedno slovo, nakon čega slijedi slova, brojki ili podvlaka (\_).

#### <a name="values"></a>Vrijednosti

Ograničena niz, broj i Booleove vrste su vrijednosti.

#### <a name="size"></a>Veličina

Dodaci ograničeni su na 1024 znakova po poziva (nakon Web SDK za mobilne radnje je šifrira u JSON).

## <a name="reporting-application-information"></a>Informacije o aplikaciji za izvješćivanje o pogreškama

Možete ručno prijaviti za praćenje podataka (ili druge informacije specifične za aplikaciju) pomoću na `sendAppInfo()` (opis funkcije).

Imajte na umu da se ti podaci mogu poslati postupno. Samo najkasnija vrijednost za određeni ključ će biti zadržane za određeni uređaj.

Kao što su dodaci događaj, možete koristiti bilo koji objekt JSON da biste apstraktnog informacije o aplikaciji. Imajte na umu da polja ili podmjesta objekata tretira se kao paušalni nizovi (pomoću JSON serijalizacije).

**Primjer:**

Slijedi primjer kod za slanje spol korisnika i datum rođenja:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Ograničenja

Ograničenja koji se odnose na informacije o aplikaciji su područja regularne izraze za tipke i veličina.

#### <a name="keys"></a>Tipke

Svaki ključ u objektu moraju se podudarati pravilnim sljedeći izraz:

    ^[a-zA-Z][a-zA-Z_0-9]*

To znači da se tipke morate pokrenuti s najmanje jedno slovo, nakon čega slijedi slova, brojki ili podvlaka (\_).

#### <a name="size"></a>Veličina

Informacije o aplikaciji ograničeno je na 1024 znakova po poziva (nakon Web SDK za mobilne radnje je šifrira u JSON).

U prethodnom primjeru, JSON poslane na poslužitelj je 44 znakova:

    {"birthdate":"1983-12-07","gender":"female"}
