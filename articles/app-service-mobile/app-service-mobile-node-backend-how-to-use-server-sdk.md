<properties
    pageTitle="Upute za rad s poslužiteljem pozadinske Node.js SDK za mobilne aplikacije | Aplikacije servisa za Azure"
    description="Saznajte kako raditi s poslužiteljem pozadinske Node.js SDK za Azure servisa mobilna aplikacija."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Kako koristiti Azure mobilne aplikacije Node.js SDK-a

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Ovaj članak sadrži detaljne informacije i primjeri pokazuje kako raditi s pozadinskom Node.js u Azure servisa mobilna aplikacija.

## <a name="Introduction"></a>Uvod

Usluga mobilne Azure aplikacija pruža mogućnost da biste dodali optimizirane za mobilne podataka access Web API u web-aplikaciju.  Azure aplikacije usluga mobilne aplikacije SDK navedeni su za ASP.NET i Node.js web-aplikacije.  SDK-a nudi sljedeće postupke:

- Postupci tablice (za čitanje, umetanje, ažuriranje, brisanje) za pristup podacima
- Prilagođeno API operacije

Operacije omogućavaju provjere autentičnosti preko svih davatelji identiteta dopušteno mjerodavnim Azure aplikacije servisa, uključujući davatelji identiteta društvenih kao što su Facebook, Twitter, Google i Microsoft kao i Azure Active Directory za identitet tvrtke.

O svakom slučaju koristi u [direktoriju uzorke na GitHub], potražite uzorka.

## <a name="supported-platforms"></a>Podržane platforme

Azure SDK za mobilne aplikacije čvor podržava trenutno izdanje LTS čvora i novijim verzijama.  Na tekst koji unosite, najnoviju verziju LTS je v4.5.0 čvor.  Druge verzije programa čvor bi mogla funkcionirati, ali nisu podržane.

Azure SDK za mobilne aplikacije čvor podržava dva upravljačkih programa baze podataka – čvor mssql upravljački program podržava SQL Azure i lokalne instance sustava SQL Server.  Upravljački program za sqlite3 podržava SQLite baze podataka na samo jednu instancu.

### <a name="howto-cmdline-basicapp"></a>Kako: Stvaranje osnovne Node.js pozadinskog pomoću naredbenog retka

Pokreće svaki pozadinskog Azure aplikacije usluga mobilne aplikacije Node.js kao ExpressJS aplikaciju.  ExpressJS je najpopularnije web servisa framework dostupne za Node.js.  Možete stvoriti programskog jezika basic [Express] aplikacije na sljedeći način:

1. Naredba ili prozor PowerShell stvorite direktorij projekta.

        mkdir basicapp

2. Pokrenite npm init Inicijalizacija strukturu paketa.

        cd basicapp
        npm init

    Naredba init npm pita skup pitanja Inicijalizacija projekta.  Pogledajte primjer izlaza:

    ![Izlaz init npm][0]

3. Instalirajte biblioteke eksplicitnih i azure mobilne aplikacije iz spremišta npm.

        npm install --save express azure-mobile-apps

4. Stvaranje datoteke app.js implementaciju osnovni mobilne poslužitelja.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Ta aplikacija stvara WebAPI koji je optimiziran za mobile s jednom krajnje točke (`/tables/TodoItem`) koji omogućuje Neprovjereni pristup u podlozi SQL spremišta podataka pomoću dinamičke sheme.  Dobro je prikladan za praćenje brzog pokretanja biblioteke klijent:

- [Brzi početak rada sa sustavom android klijenta]
- [Brzi početak rada za klijent Cordova Apache]
- [iOS klijent brzi početak rada]
- [Brzi početak rada za klijent iz Windows trgovine]
- [Brzi početak rada Xamarin.iOS klijenta]
- [Brzi početak rada Xamarin.Android klijenta]
- [Brzi početak rada Xamarin.Forms klijenta]

Za ovaj osnovni aplikacije u [uzorka basicapp na GitHub]možete pronaći kod.

### <a name="howto-vs2015-basicapp"></a>Kako: Stvaranje čvor pozadinskog s Visual Studio 2015.

Visual Studio 2015 potreban je datotečni nastavak za razvoj aplikacija Node.js unutar na IDE.  Da biste započeli, instalirajte [Node.js 1.1 Alati za Visual Studio].  Kada su instalirani alati za Node.js za Visual Studio, stvorite aplikaciju Express 4.x:

1. Otvorite dijaloški okvir **Novi projekt** (iz **datoteke** > **Novo** > **projekta...**).

2. Proširite **predložaka** > **JavaScript** > **Node.js**.

3. Odaberite **aplikaciju osnovni Azure Node.js Express 4**.

4. Unesite naziv projekta.  Kliknite *u redu*.

    ![Novi projekt Visual Studio 2015.][1]

5. Desnom tipkom miša kliknite čvor **npm** , a zatim odaberite **... instaliranje novih paketa npm**.

6. Možda ćete morati osvježiti kataloga npm o stvaranju prvi Node.js aplikacije.  Ako je potrebno, kliknite **Osvježi** .

7. U okvir za pretraživanje unesite _azure mobilne aplikacije_ .  Kliknite **azure-– aplikacije mobile 2.0.0** paket, a zatim kliknite **Instalirajte paket**.

    ![Instaliranje nove npm paketa][2]

8. Kliknite **Zatvori**.

9. Otvorite datoteku _app.js_ da biste dodali podrška za Azure mobilne aplikacije SDK.  U retku 6 na dnu biblioteke zahtijevaju izjave, dodajte sljedeći kod:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Na približno redak 27 nakon druge app.use izjave, dodajte sljedeći kod:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Spremite datoteku.

10. Pokrenite aplikaciju lokalno (u API je tvrtki http://localhost:3000) ili objavljivanje Azure.

### <a name="create-node-backend-portal"></a>Kako: Stvaranje Node.js pozadinskog pomoću portala za Azure

Možete stvoriti pravo pozadinskog mobilnu aplikaciju za [Azure portal]. Možete, slijedite sljedeće korake ili stvaraju postupak klijentske i poslužiteljske zajedno sljedeći Praktični vodič za [Stvaranje aplikacije za mobilne uređaje](app-service-mobile-ios-get-started.md) . Vodič sadrži pojednostavljeni verziju ove upute i najbolje odgovara dokaz pojam projekata.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Vratite se u plohu _Početak rada_ u odjeljku **Stvaranje tablice API-JA**, odaberite **Node.js** kao **pozadinskom jezik**. Potvrdite okvir "je**li potvrda da to prebrisat ćete sve web-mjesta sadržaj.**", a zatim kliknite **Stvori TodoItem tablica**.

### <a name="download-quickstart"></a>Kako: preuzimanje Node.js pozadinskog brzi početak rada s kodom projekta brojka

Kada stvorite Node.js mobilnu aplikaciju pozadinskog pomoću portala za **brzi početak** plohu, Node.js projekta je za vas stvara i implementiran na web-mjesto. Možete dodati tablice i API- i uređivanje datoteka kod za pozadinski Node.js na portalu. Da biste preuzeli projekta pozadinskog tako da možete dodati ili izmijeniti tablice i API-, a zatim ponovno objavite projekt možete koristiti i razni Alati za implementaciju. Dodatne informacije potražite u članku [Vodič za implementaciju servisa Azure aplikacije]. u nastavku koristi spremište brojka da biste preuzeli Šifra za brzi početak rada projekta.

1. Instalirajte brojka, ako to još niste učinili. Korake potrebne za instalaciju brojka razlikovati operacijske sustave. Pročitajte članak [Instaliranje brojka](http://git-scm.com/book/en/Getting-Started-Installing-Git) za distribucija specifične za operacijski sustav i upute za instalaciju.

2. Slijedite korake u [Omogućivanje aplikacije servisa za spremište aplikacije](../app-service-web/app-service-deploy-local-git.md#Step3) da biste omogućili spremište brojka pozadinskog web-mjesta, čime bilješku o implementaciji korisničko ime i lozinku.

3. U plohu za vaše pozadinskog mobilnu aplikaciju, zabilježite postavku **brojka Kloniraj URL** .

4. Izvršavanje u `git clone` naredba pomoću na brojka Kloniraj URL-a, unesete lozinku po potrebi, kao u sljedećem primjeru:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Dođite do lokalni direktorij koja u prethodnom primjeru /todolist, i obratite pozornost na to da su preuzete datoteke projekta. Pronađite na `todoitem.json` datoteka u na `/tables` direktorija.  Tu datoteku definira dozvole u tablici.  Pronaći i u `todoitem.js` datoteke u direktoriju isti koji definira tu operaciju skripte za tablicu.

6. Nakon što ste unijeli promjene u datoteke projekta, izvršiti sljedeće naredbe da biste dodali, zapisivanje, a zatim prenesite promjene na web-mjesto:

        $ git commit -m "updated the table script"
        $ git push origin master

    Kada dodate nove datoteke u projekt, najprije morate izvršiti na `git add .` naredbe.

Web-mjesto je ponovno objaviti u svaki put kada se novi skup pamti pritisak na web-mjesto.

### <a name="howto-publish-to-azure"></a>Kako: objavite svoje Node.js pozadinskog Azure

Microsoft Azure nudi brojne mehanizme za objavljivanje u pozadinskom Azure aplikacije usluga mobilne aplikacije Node.js sa servisom Azure.  To obuhvaća korištenja Alati za implementaciju integriran u Visual Studio, alati naredbenog retka i mogućnosti neprekinuti implementacije na temelju izvor kontrole.  Dodatne informacije o ovoj temi potražite u članku [Vodič za implementaciju servisa Azure aplikacije].

Aplikacije servisa za Azure sadrži određene savjete za Node.js aplikaciju koju treba pregledati prije implementacije:

- Određivanje [Verzije čvor]
- Kako [koristiti čvor moduli]

### <a name="howto-enable-homepage"></a>Kako: Omogućivanje Početna stranica aplikacije

Kombinacija web i mobilne aplikacije su mnoge aplikacije i ExpressJS framework omogućuje vam da biste spojili dva pozornici.  Ponekad, no možda želite samo implementirati mobilnog sučelja.  Ovo je korisna je odredišna stranica da biste bili sigurni aplikacije servisa s radom.  Možete pružaju početne stranice ili omogućiti privremene početnu stranicu.  Da biste omogućili privremene Početna stranica, koristite sljedeće stvoriti instancu Azure mobilne aplikacije:

    var mobile = azureMobileApps({ homePage: true });

Ako želite samo Ova mogućnost dostupna prilikom razvoja lokalno, možete dodati ta postavka omogućuje vaše `azureMobile.js` datoteku.

## <a name="TableOperations"></a>Postupci tablice 

Azure mobilne aplikacije Node.js Server SDK nudi mehanizme da biste otkrili podatkovne tablice koje se pohranjuju u bazi podataka SQL Azure kao u WebAPI.  Pet operacije služe.

| Postupak | Opis |
| --------- | ----------- |
| Početak /tables/_tablename_ | Dohvaćanje svih zapisa u tablicu |
| Početak /tables/_tablename_/:id | Početak na određeni zapis u tablici |
| OBJAVA /tables/_tablename_ | Stvorite zapis u tablici |
| ZAKRPU /tables/_tablename_/:id | Ažuriranje zapisa u tablicu |
| Brisanje /tables/_tablename_/:id | Brisanje zapisa u tablicu |

U ovom WebAPI podržava [OData] i proširuje shemu tablica za podršku [sinkronizaciju izvanmrežne podatke].

### <a name="howto-dynamicschema"></a>Kako: definiranje tablica pomoću dinamičke sheme

Prije korištenja tablice potrebno definirati.  Tablica može se definirati sa shemom statične (gdje programer definira stupce unutar shema) ili dinamički (gdje SDK kontrole shemu na temelju zahtjevi). Programer, k tome, možete upravljati određene aspektima u WebAPI dodavanjem Javascript kod definiciju.

Kao preporučenim načinom rada koje treba definirati svaku tablicu u Javascript datoteku u imeniku tablica, a zatim upotrijebite metodu tables.import() za uvoz tablica.  Proširivanje aplikaciju basic app.js datoteku želite prilagoditi:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definiranje tablice u. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tablica koristiti dinamičke shemu prema zadanim postavkama.  Da biste isključili dinamički sheme globalno, postavite postavku aplikacije **MS_DynamicSchema** FALSE unutar portala za Azure.

Dovršavanje primjer možete pronaći u [ogledne obveze na GitHub].

### <a name="howto-staticschema"></a>Kako: definiranje tablica pomoću statičke sheme

Možete definirati izričito stupaca da biste otkrili putem na WebAPI.  SDK Node.js azure mobilne aplikacije automatski dodaje sve dodatne stupce potrebne za sinkronizaciju izvanmrežnih podataka na popis koji navedete.  Na primjer, klijentske aplikacije za brzi početak rada zahtijevaju tablicu s dva stupca: tekst (niz) i dovršite (Booleove vrijednosti).  
U tablici može se definirati u tablici JavaScript datoteku definicije (koja se nalazi u direktoriju tablica) na sljedeći način:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Ako definirate tablice statički, zatim morate i pozovite metodu tables.initialize() da biste stvorili shemu baze podataka prilikom pokretanja.  Način tables.initialize() vraća [obećanje] tako da se web-servisa služe zahtjeva prije baze podataka koja se pokrenuti.

### <a name="howto-sqlexpress-setup"></a>Kako: korištenje SQL Express kao izvor podataka razvoj na lokalnom računalu

Azure mobilne aplikacije u AzureMobile aplikacije čvor SDK-a nudi tri mogućnosti za posluživanje podataka iz okvir: SDK sadrži tri mogućnosti za posluživanje podataka iz okvir:

- Koristite **memorije** upravljački program koji nisu stalni primjer spremišta
- Koristiti **mssql** upravljački program za izvor podataka SQL Express za razvoj
- Koristite **mssql** upravljački program za pružanje spremišta podataka za baze podataka SQL Azure za proizvodnju

Azure SDK za mobilne aplikacije Node.js koristi [mssql Node.js paketa] da biste ostvarili i pomoću veze za SQL Express i SQL baze podataka.  Ovaj paket zahtijeva omogućite TCP vezama na instancu sustava SQL Express.

> [AZURE.TIP]Upravljački program za memorije ne nudi cjelovit skup funkcije za testiranje.  Ako želite da biste testirali pozadinskog lokalno, preporučujemo korištenje spremišta podataka SQL Express i upravljački program za mssql.

1. Preuzmite i instalirajte [Microsoft SQL Server 2014 Express].  Provjerite je li uz izdanje alata instalirate Express 2014 za SQL Server.  Ako je potrebna vam je izričito 64-bitnu podršku, 32-bitnu verziju troši manje memorije kada se pokrene.

2. Pokrenite Upravitelj konfiguracije za SQL Server 2014..

  1. Proširite čvor **SQL Server mrežna konfiguracija** na izborniku lijeve stabla.
  2. Kliknite **protokoli za SQLEXPRESS**.
  3. Desnom tipkom miša kliknite **TCP/IP** , a zatim odaberite **Omogući**.  Kliknite **u redu** u dijaloškom okviru skočni.
  4. Desnom tipkom miša kliknite **TCP/IP** , a zatim odaberite **Svojstva**.
  5. Kliknite karticu **IP adrese** .
  6. Potražite čvor **IPAll** .  U polju **TCP priključak** unesite **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Kliknite **u redu**.  Kliknite **u redu** u dijaloškom okviru skočni.
  8. Na izborniku lijeve stabla kliknite **Servisa SQL Server** .
  9. Desnom tipkom miša kliknite **SQL Server (SQLEXPRESS)** , a zatim odaberite **ponovno pokrenite**
  10. Zatvorite Upravitelj konfiguracije za SQL Server 2014..

3. Pokrenite SQL Server 2014 Management Studio i povezivanje na lokalnu instancu SQL Express

  1. Desnom tipkom miša kliknite vašoj instanci u programu Explorer objekt, a zatim odaberite **Svojstva**
  2. Odaberite stranicu **Sigurnost** .
  3. Provjerite je li odabran na **SQL Server i načina provjere autentičnosti sustava Windows**
  4. Kliknite **u redu**

        ![Konfiguriranje provjere autentičnosti SQL Express][4]

  5. Proširite **Sigurnost** > **prijave** u programu Explorer objekta
  6. Desnom tipkom miša kliknite **prijave** , a zatim odaberite **Novi prijava...**
  7. Unesite ime za prijavu.  Odaberite **provjeru autentičnosti sustava SQL Server**.  Unesite lozinku, a zatim unesite istu lozinku u **Potvrda lozinke**.  Lozinka mora zadovoljavati uvjete složenosti sustava Windows.
  8. Kliknite **u redu**

        ![Dodavanje novog korisnika u SQL Express][5]

  9. Desnom tipkom miša kliknite svoje nove prijava, a zatim odaberite **Svojstva**
  10. Odaberite stranicu **Uloge poslužitelja**
  11. Potvrdite okvir pokraj uloga poslužitelja **dbcreator**
  12. Kliknite **u redu**
  13. Zatvorite SQL Server 2015 Management Studio

Provjerite je li snimanje korisničko ime i lozinku koju ste odabrali.  Možda ćete morati dodijeliti uloge dodatne poslužitelja ili dozvole po vlastitom nahođenju određene baze podataka.

Aplikacija Node.js čita **SQLCONNSTR_MS_TableConnectionString** varijablu okruženja za niz za povezivanje za tu bazu podataka.  Ova varijabla možete postaviti u okruženju sustava.  Ako, na primjer, možete koristiti PowerShell da biste postavili ovaj varijablu okruženja:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Pristup bazi podataka kroz TCP/IP vezu i navedite korisničko ime i lozinku za povezivanje.

### <a name="howto-config-localdev"></a>Kako: Konfiguriranje projekta za lokalni razvoj

Azure mobilne aplikacije čita JavaScript datoteku pod nazivom _azureMobile.js_ iz lokalne datotečnom sustavu.  Konfiguriranje SDK Azure mobilne aplikacije u radnog-korištenje postavki aplikacije unutar [Azure portal] pomoću ove datoteke.  Datoteka _azureMobile.js_ izvozi konfiguracije objekta.  Najčešće postavke su:

- Postavke baze podataka
- Postavke Zapisivanje dijagnostičkih podataka
- Zamjenski CORS postavke

Slijedi primjera datoteke _azureMobile.js_ implementacijom prethodne postavke baze podataka:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Preporučujemo da dodavanje _azureMobile.js_ u datoteku _.gitignore_ (ili drugih kontrola izvornog koda Zanemari datoteka) da biste spriječili pohranjene u oblaku lozinke.  Uvijek Konfiguriranje postavki proizvodnje u postavki aplikacije unutar [Azure portal].

### <a name="howto-appsettings"></a>Upute: Konfiguriranje postavki aplikacije za mobilne aplikacije

Većina postavki u datoteci _azureMobile.js_ imati ekvivalentne postavka App [Azure portal].  Pomoću sljedećeg popisa možete koristiti za konfiguriranje aplikacije u postavki aplikacije:

| Postavljanje aplikacije                 | Postavka _azureMobile.js_  | Opis                               | Valjane vrijednosti                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | ime                      | Naziv aplikacije                       | niz                                      |
| **MS_MobileLoggingLevel**   | Logging.Level             | Minimalna zapisnika razinu poruke za prijavu      | Pogreška, upozorenje opširno informacije, ispravljanje pogrešaka, a zatim silly |
| **MS_DebugMode**            | ispravljanje pogrešaka                     | Omogućivanje ili onemogućivanje načina rada za ispravljanje pogrešaka              | TRUE, false                                 |
| **MS_TableSchema**          | Data.Schema               | Zadani naziv sheme za SQL tablice        | niz (zadani: vlasnika baze podataka)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Omogućivanje ili onemogućivanje načina rada za ispravljanje pogrešaka              | TRUE, false                                 |
| **MS_DisableVersionHeader** | verzija (postavljeno na nedefiniranu)| Onemogućuje zaglavlje X-ZUMO--verzija poslužitelja | TRUE, false                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Onemogućuje Provjera verzija klijenta API-JA     | TRUE, false                                 |

Da biste postavili je postavka App:

1. Prijavite se na [portal za Azure].
2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv mobilnu aplikaciju.
3. Otvorit će se postavke plohu prema zadanim postavkama. Ako ne, kliknite **Postavke**.
4. Kliknite **Postavke aplikacije** na izborniku OPĆENITO.
5. Pomaknite se do odjeljka postavke aplikacije.
6. Ako aplikaciju postavljanje već postoji, kliknite vrijednost postavke aplikacije da biste uredili vrijednost.
7. Ako postavke za aplikaciju ne postoji, unesite postavku aplikacije u okvir ključ i vrijednost u okvir vrijednost.
8. Kada se dovrši, kliknite **Spremi**.

Promjena postavki Većina aplikacije zahtijeva ponovno pokretanje servisa.

### <a name="howto-use-sqlazure"></a>Kako: SQL baza podataka kao radnog spremište podataka

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Korištenje baze podataka SQL Azure kao izvor podataka jednak kroz sve vrste aplikacije servisa za Azure aplikacije. Ako ste još niste učinili, slijedite ove korake da biste stvorili pozadinskog za mobilnu aplikaciju.

1. Prijavite se na [portal za Azure].

2. U gornjem lijevom kutu prozora, kliknite gumb **+ NOVO** > **Web + Mobile** > **Mobilnu aplikaciju**, navedite naziv vaše pozadinskog mobilnu aplikaciju.

3. U okvir za **Grupu resursa** unesite isti naziv kao aplikacije.

4. Planiranje aplikacije servisa za zadani je odabran.  Ako želite promijeniti tarifu za aplikacije servisa, to možete učiniti tako da kliknete Plan usluge za aplikacije > **+ Stvori novo**.  Navedite naziv nove aplikacije servisa za tarife, a zatim odaberite odgovarajuće mjesto.  Kliknite sloju određivanje cijena te odaberite odgovarajući cijene sloju za servis. Odaberite **Prikaži sve** da biste vidjeli dodatne mogućnosti cijene, kao što su **besplatno** i **zajedničko korištenje**.  Kada odaberete cijene sloju, kliknite gumb **Odaberi** .  Vratite se u plohu **aplikacije servisa za planiranje** kliknite **u redu**.

5. Kliknite **Stvori**. Dodjeljivanje mobilnu aplikaciju pozadinskog može potrajati nekoliko minuta.  Kada je dodjeli pozadinskog mobilnu aplikaciju, na portalu otvara plohu **Postavke** za mobilnu aplikaciju pozadinski.

Nakon stvaranja pozadinskog mobilnu aplikaciju, možete odabrati postojeću bazu podataka sustava SQL povezati svoje pozadinskog mobilnu aplikaciju ili stvoriti novu bazu podataka sustava SQL.  U ovom ćete odjeljku ćemo stvoriti SQL baze podataka.

> [AZURE.NOTE]Ako već imate baze podataka na istom mjestu kao pozadinskom mobilne aplikacije, možete umjesto toga **koristite postojeću bazu podataka** , a zatim odaberite tu bazu podataka. Zbog veće latencies ne preporučuje se korištenje baze podataka na neko drugo mjesto.

6. U novi pozadinski mobilnu aplikaciju, kliknite **Postavke** > **Mobilnu aplikaciju** > **podataka** > **+ Dodaj**.

7. U plohu **Dodavanje podatkovne veze baze podataka** kliknite **Baza podataka SQL - konfiguriranje potrebne postavke** > **Stvori novu bazu podataka**.  U polje **naziv** unesite naziv nove baze podataka.

8. Kliknite **poslužitelja**.  U Elektronička ploča **Novi poslužitelja** unesite naziv poslužitelja jedinstveni u polje **naziv poslužitelja** i odgovarajuću **Prijava administrator poslužitelja** i **lozinku**.  Provjerite je li potvrđen okvir **Dopusti azure servisa za pristup poslužitelju** .  Kliknite **u redu**.

    ![Stvaranje baze podataka Azure SQL][6]

9. Na plohu **novu bazu podataka** , kliknite **u redu**.

10. Ponovno uključite plohu **Dodavanje podatkovne veze** odaberite **niz za povezivanje**, unesite prijava i lozinka koje ste naveli prilikom stvaranja baze podataka.  Ako koristite postojeću bazu podataka, navedite vjerodajnice za prijavu za tu bazu podataka.  Nakon što ste unijeli, kliknite **u redu**.

11. Natrag na plohu **Dodaj podatkovnu vezu** , kliknite **u redu** da biste stvorili bazu podataka.

<!--- END OF ALTERNATE INCLUDE -->

Stvaranje baze podataka može potrajati nekoliko minuta.  Praćenje napretka implementacije pomoću u područje **obavijesti** .  Tijek dok bazu podataka implementiran uspješno.  Nakon uspješnog uvođenja niza za povezivanje će se stvoriti instancu sustava SQL baze podataka u mobilnom pozadinskog postavki aplikacije.  Vidjet ćete tu postavku aplikacije u odjeljku **Postavke** > **Postavke aplikacije** > **nizove veze**.

### <a name="howto-tables-auth"></a>Kako: Traži provjeru autentičnosti za pristup tablice

Ako želite koristiti provjera autentičnosti za aplikaciju servisa s krajnju točku tablica, morate konfigurirati provjera autentičnosti za aplikaciju servisa [Azure portal] prvi put.  Dodatne informacije o konfiguriranju provjere autentičnosti na servisu Azure aplikacije, pročitajte članak vodič za konfiguraciju za davatelja identiteta namjeravate koristiti:

- [Konfiguriranje provjere autentičnosti Azure Active Directory]
- [Konfiguriranje provjere autentičnosti za Facebook]
- [Konfiguriranje provjere autentičnosti Google]
- [Kako konfigurirati Microsoft Authentication]
- [Upute za konfiguriranje provjere autentičnosti na Twitteru]

Svaka tablica ima svojstva u programu access koje je moguće koristiti za kontrolu pristupa u tablicu.  Sljedeći primjer prikazuje statički definirani tablicu s potrebna je provjera autentičnosti.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Svojstvo access možete poduzeti jednu od tri vrijednosti

  - označava *anonimni* klijentska aplikacija dopuštena za čitanje podataka bez provjere autentičnosti
  - *autentičnost* označava klijentska aplikacija morate poslati valjane token sa zahtjevom za
  - *onemogućeno* označava u ovoj su tablici trenutno je onemogućen

Ako je svojstvo pristupom Nedefinirano Neprovjereni pristup je dopušteno.

### <a name="howto-tables-getidentity"></a>Kako: korištenje zahtjevima za provjeru autentičnosti s tablicama

Možete postaviti razne zahtjevima koje su zatražili kada je postavljen provjere autentičnosti.  Ove zahtjevima nisu obično dostupne putem na `context.user` objekt.  Međutim, oni mogu biti dohvaćeni pomoću na `context.user.getIdentity()` način.  Na `getIdentity()` način vraća obećanje koji se pretvara u objekt.  Objekt je odnose prema načina provjere autentičnosti (facebook, google, twitter, microsoftaccount ili aad).

Na primjer, ako postavili Microsoftov Account o provjeri autentičnosti i zahtjev zahtjeva za adrese e-pošte, možete dodati adresu e-pošte na zapis s kontrolerom sljedeće tablice:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Da biste vidjeli dostupne su koje zahtjevima, koristite web-preglednik da biste prikazali na `/.auth/me` krajnjoj točki web-mjesta.

### <a name="howto-tables-disabled"></a>Kako: onemogućavanje pristupa postupci određene tablice

Osim pojavljuju se u tablici, access svojstvo se može koristiti da biste odredili pojedinačne operacije.  Postoje četiri postupci:

  - *čitanje* je postupak RESTful početak u tablici
  - *Umetanje* je postupak RESTful objavu u tablici
  - *Ažuriranje* je postupak RESTful ZAKRPU u tablici
  - *Brisanje* je postupak RESTful brisanje u tablici

Ako, na primjer, možda želite unijeti samo za čitanje Neprovjereni tablice:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Kako: Prilagodba upit koji se koristi s Postupci tablice

Postupci tablice potrebna je pružanje ograničena prikaz podataka.  Na primjer, može vam dati tablice koji je označen čija je autentičnost provjerena korisničkog ID-a tako da možete samo čitati ili ažurirati sami zapise.  Definiciju tablice sljedeće omogućuje ta je funkcija:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Operacije koje obično izvršavanje upita imaju upita svojstvo koje možete prilagoditi pomoću where uvjet. Svojstvo upita je [QueryJS] objekt koji se koristi za pretvaranje upita s OData nešto što možete obradu podataka pozadinskog.  Jednostavan jednakosti slučajeva (kao što je prethodni jedan), može se koristiti na karti. Možete dodati i određene SQL uvjeta:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Kako: Konfiguriranje meki Izbriši na tablicu

Meki Izbriši doista izbrisati zapise.  Umjesto toga ga označava da ih izbrisati baze podataka postavljanjem izbrisane stupac na true.  Azure mobilne aplikacije SDK automatski uklanja mekih izbrisati zapise iz rezultata osim ako klijent SDK Mobile koristi IncludeDeleted().  Da biste konfigurirali tablice za brisanje meki, postavite na `softDelete` svojstvo u datoteka za definiciju tablice:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Trebali biste uspostavili mehanizam za brisanje zapisa – od klijentska aplikacija, putem WebJob Azure funkcija ili prilagođene API-JA.

### <a name="howto-tables-seeding"></a>Kako: Seed bazu podataka s podacima

Prilikom stvaranja nove aplikacije, možda želite seed tablicu s podacima.  To možete učiniti unutar JavaScript datoteka za definiciju tablice na sljedeći način:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Seeding podataka samo radi stvaranja tablice tako da u SDK za Azure mobilne aplikacije.  Ako već postoji u tablici baze podataka, podaci se umetnutog u tablicu.  Ako je uključena mogućnost dinamički shemu, shema je nenamjerna iz seeded podataka.

Preporučujemo da izričito pozovete na `tables.initialize()` način da biste stvorili tablicu prilikom pokretanja servisa pokrenut.

### <a name="Swagger"></a>Kako: Omogućivanje Swagger podrške

Usluga mobilne Azure aplikacija u sklopu ugrađene [Swagger] podršku.  Da biste omogućili podršku za Swagger, najprije instalirajte korisničkog sučelja swagger kao ovisnosti:

    npm install --save swagger-ui

Nakon instalacije, možete omogućiti podršku Swagger u Graditelj Azure mobilne aplikacije:

    var mobile = azureMobileApps({ swagger: true });

Ste vjerojatno samo želite omogućiti podršku Swagger u razvoju izdanja.  To možete učiniti pomoću na `NODE_ENV` postavka app:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Krajnja točka swagger nalazi se na http://_yoursite_.azurewebsites.net/swagger.  Možete pristupiti Swagger korisničko Sučelje preko na `/swagger/ui` krajnjoj točki.  Ako odaberete da zahtijeva provjeru autentičnosti preko cijelu aplikaciju, Swagger daje pogrešku.  Da biste postigli najbolje rezultate, odaberite da biste omogućili Neprovjereni zahtjeva putem u provjera autentičnosti servisa Azure aplikacije / autorizacije postavke, zatim kontrolu pomoću provjere autentičnosti na `table.access` svojstvo.

Možete dodati i mogućnost Swagger vaše `azureMobile.js` datoteke ako samo želite Swagger podrške prilikom razvoja lokalno.

## <a name="a-namepushpush-notifications"></a><a name="push">Automatske obavijesti

Mobilne aplikacije integrira Azure koncentratora obavijesti da biste omogućili slanje ciljano automatske obavijesti milijune uređaji preko sve glavne platforme. Pomoću obavijesti koncentratora možete poslati automatske obavijesti iOS, Android i Windows uređaja. Dodatne informacije o svemu što možete učiniti s koncentratorima obavijesti, potražite u članku [Pregled koncentratora obavijesti](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Kako: slanje automatskih obavijesti

Sljedeći kod prikazuje kako koristiti automatske objekt da biste poslali emitiranje automatske obavijesti za uređaje sa sustavom iOS registrirani:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Stvaranjem registraciju za slanje predloška iz klijentskog programa umjesto toga možete poslati poruku automatske predložak za uređaje na sve podržane platforme. Sljedeći kod prikazuje kako poslati obavijest predložak:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Kako: Pošalji automatske obavijesti čija je autentičnost provjerena korisniku pomoću oznaka

Kada korisnik sustava čija je autentičnost provjerena registrira za slanje obavijesti, korisnički ID oznake automatski se dodaju za registraciju. Pomoću ove oznake automatske obavijesti možete poslati svim uređajima registrirane određenog korisnika. Sljedeći kod dobiva SID korisnika upućivanje zahtjeva i šalje automatske obavijesti predložak svaki Registracija uređaja za tog korisnika:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Kada se registrirate za slanje obavijesti putem čija je autentičnost provjerena klijenta, provjerite je li autentičnosti dovršetka prije registracije.

## <a name="CustomAPI"></a>Prilagođeni API-ji

###  <a name="howto-customapi-basic"></a>Kako: definiranje prilagođenog API-JA


Osim podataka pristup API putem krajnju točku /tables Azure mobilne aplikacije možete unijeti prilagođeni opseg API-JA.  Prilagođeni API-ji definiraju na sličan način definicije tablice i možete pristupiti iste funkcije, uključujući provjeru autentičnosti.

Ako želite koristiti provjera autentičnosti za aplikaciju servisa s prilagođeno API-jem, morate konfigurirati provjera autentičnosti za aplikaciju servisa [Azure portal] prvi put.  Dodatne informacije o konfiguriranju provjere autentičnosti na servisu Azure aplikacije, pročitajte članak vodič za konfiguraciju za davatelja identiteta namjeravate koristiti:

- [Konfiguriranje provjere autentičnosti Azure Active Directory]
- [Konfiguriranje provjere autentičnosti za Facebook]
- [Konfiguriranje provjere autentičnosti Google]
- [Kako konfigurirati Microsoft Authentication]
- [Upute za konfiguriranje provjere autentičnosti na Twitteru]

Prilagođeni API-ji definiraju mnogo na isti način kao tablica API-JA.

1. Stvaranje u imeniku **API-ja**
2. Stvaranje datoteke JavaScript API definicija u imeniku **API-ja** .
3. Da biste uvezli **api** direktorija, upotrijebite metodu uvoz.

Evo definiciju api predložak na temelju uzorka basic aplikacije ranije koristi.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Pogledajmo primjer API koji vraća datum poslužitelj pomoću metode _Date.now()_ .  Evo api/date.js datoteke:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Svaki parametar jedan je od u standardni RESTful glagoli - GET, objavu, ZAKRPU ili Izbriši.  Način je standardni funkcija [ExpressJS proizvod] koji šalje traženi izlaz.

### <a name="howto-customapi-auth"></a>Kako: Traži provjeru autentičnosti za pristup prilagođene API-JA

Azure mobilne aplikacije SDK implementira provjere autentičnosti na isti način za krajnje točke tablice i prilagođene API-ji.  Provjera autentičnosti API razvijene u prethodnom odjeljku, dodati svojstva u programu **access** :

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Možete odrediti i provjere autentičnosti na određene operacije:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Isti token koji se koristi za krajnju točku tablica mora se koristiti za prilagođene API-ji zahtijeva provjeru autentičnosti.

### <a name="howto-customapi-auth"></a>Kako: rukovati prijenosima velikih datoteka

Azure mobilne aplikacije SDK koristi [tijelo analizator proizvod](https://github.com/expressjs/body-parser) da biste prihvatili i dekodiranje sadržaj tijela poslane.  Unaprijed možete konfigurirati tijelo analizator da biste prihvatili veće datoteka:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Datoteka je osnovni 64 kodirani prije prijenosa.  Time povećava veličinu stvarni prijenos (i zato veličine morate računu za).

### <a name="howto-customapi-sql"></a>Kako: izvršavanje prilagođene SQL naredbe

Azure mobilne aplikacije SDK omogućuje pristup cijelu kontekst do objekta zahtjev vam omogućuje jednostavno izvršiti s parametrima SQL naredbi za davatelja definirani podataka:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Ispravljanje pogrešaka, jednostavno tablice i jednostavno API-ji

### <a name="howto-diagnostic-logs"></a>Kako: ispravljanje pogrešaka, dijagnosticiranje i otklanjanje poteškoća s Azure mobilne aplikacije

Aplikacije servisa Azure nudi nekoliko ispravljanje pogrešaka i otklanjanjem za Node.js aplikacije.
Pogledajte sljedeće članke za početak rada u vaš mobilni Node.js pozadinskog za otklanjanje poteškoća:

- [Nadzor Azure aplikacije servisa]
- [Uključite Zapisivanje dijagnostičkih podataka u aplikacije servisa za Azure]
- [Otklanjanje poteškoća s Azure aplikacije servisa u Visual Studio]

Aplikacija Node.js imaju pristup širokog raspona alata dijagnostičkog zapisnika.  Interno, Azure SDK za mobilne aplikacije Node.js koristi [Winston] za zapisivanje dijagnostičkih podataka.  Zapisivanje je automatski omogućena omogućivanjem ispravljanje pogrešaka način ili tako da postavite postavku aplikacije **MS_DebugMode** na true [Azure portal]. Generirani zapisnici prikazuju se u zapisnicima dijagnostičkih podataka za [Azure portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Kako: rad s tablicama za jednostavno na portalu za Azure

Jednostavno tablice na portalu omogućuju stvaranje i rad s tablicama desno na portalu. Čak i možete urediti Postupci tablice pomoću uređivača servisa aplikacija.

Kada kliknete **jednostavno tablice** u postavkama pozadinskog web-mjesta, možete dodati, izmjena ili brisanje tablice. Možete pogledati i podataka u tablici.

![Rad s tablicama jednostavno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Dostupne na naredbenoj traci u slučaju tablice su sljedeće naredbe:

+ **Promjena dozvola** - Izmjena dozvola za čitanje, umetanje, ažuriranje i brisanje operacija u tablici. 
  Da biste omogućili anonimni pristup zahtijeva provjeru autentičnosti ili da biste onemogućili pristupa postupak su mogućnosti. 
+ **Uređivanje skripte** – datoteka skripte za tablicu, otvara se u aplikaciju servisa uređivaču.
+ **Upravljanje shemom** - Dodavanje i brisanje stupaca ili izmijeniti indeks tablice.
+ **Očisti tablicu** – skraćuje postojećoj tablici se brisanje svih redaka podataka, ali ostavite shemi ne mijenja.
+ **Brisanje redaka** – brisanje pojedinačne redaka podataka.
+ **Prikaz strujanje zapisnika** – povezuje strujanje Zapisnički servis za web-mjesto.

###<a name="work-easy-apis"></a>Kako: rad s jednostavno API-ji na portalu za Azure

Jednostavno API-ji na portalu omogućuju stvaranje mapa i rad koja sadrži prilagođeni API-ji izravno na portalu. Možete urediti skripte API servisa uređivaču aplikacije.

Kada kliknete **API-je lako** u postavkama pozadinskog web-mjesta, možete dodati, mijenjati ili brisati prilagođene krajnjoj točki API-JA.

![Rad s jednostavno API-ji](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Na portalu možete Promjena dozvola pristupa za dani HTTP akciju, uređivanje skriptna datoteka API-JA u aplikaciji servisa uređivaču ili zapisnicima strujanje.

###<a name="online-editor"></a>Kako: uređivanje koda u aplikaciju servisa uređivaču

Portal za Azure omogućuje vam uređivanje Node.js pozadinskog skripte datoteka u aplikaciji servisa uređivaču bez potrebe za preuzimanje projekta s vašim lokalnim računalom. Da biste uređivali datoteke skripte u uređivaču online:

1. U vašem plohu pozadinskog za mobilnu aplikaciju kliknite **sve postavke** > **jednostavan tablice** ili **Jednostavno API -ji**kliknite tablicu ili API-JA, a zatim kliknite **Uređivanje skripte**. Skriptna datoteka se otvara u aplikaciji servisa uređivaču.

    ![Uređivač servisa aplikacija](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Izvršite promjene na kod datoteku u uređivaču online. Promjene se spremaju automatski tijekom unosa.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Brzi početak rada sa sustavom android klijenta]: app-service-mobile-android-get-started.md
[Brzi početak rada za klijent Cordova Apache]: app-service-mobile-cordova-get-started.md
[iOS klijent brzi početak rada]: app-service-mobile-ios-get-started.md
[Brzi početak rada Xamarin.iOS klijenta]: app-service-mobile-xamarin-ios-get-started.md
[Brzi početak rada Xamarin.Android klijenta]: app-service-mobile-xamarin-android-get-started.md
[Brzi početak rada Xamarin.Forms klijenta]: app-service-mobile-xamarin-forms-get-started.md
[Brzi početak rada za klijent iz Windows trgovine]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[Sinkronizacija izvanmrežnih podataka]: app-service-mobile-offline-data-sync.md
[Konfiguriranje provjere autentičnosti Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Konfiguriranje provjere autentičnosti za Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Konfiguriranje provjere autentičnosti Google]: app-service-mobile-how-to-configure-google-authentication.md
[Kako konfigurirati Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Upute za konfiguriranje provjere autentičnosti na Twitteru]: app-service-mobile-how-to-configure-twitter-authentication.md
[Vodič za implementaciju aplikacije servisa za Azure]: ../app-service-web/web-sites-deploy.md
[Nadzor Azure aplikacije servisa]: ../app-service-web/web-sites-monitor.md
[Uključite Zapisivanje dijagnostičkih podataka u aplikacije servisa za Azure]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Otklanjanje poteškoća s Azure aplikacije servisa u Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Određivanje verzije čvor]: ../nodejs-specify-node-version-azure-apps.md
[Korištenje čvor moduli]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Portal za Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Obećanje]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[Ogledna basicapp na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[popis obveza uzorka na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[Uzorci direktorij na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Alati za Node.js 1.1 za Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js paketa]: https://www.npmjs.com/package/mssql
[Express Microsoft SQL Server 2014.]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[Proizvod ExpressJS]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
