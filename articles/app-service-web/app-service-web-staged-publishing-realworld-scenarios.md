<properties
  pageTitle="Učinkovito korištenje DevOps okruženja za web-aplikacije"
  description="Saznajte kako koristiti slobodnih implementaciju da biste postavili i upravljanje više razvojnih okruženja za aplikaciju"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Učinkovito korištenje DevOps okruženja web-aplikacijama

Ovaj članak objašnjava postavljanje i upravljanje implementacijama aplikacije web za više verzija programa kao što su razvoj pripremna značajke pitanja i odgovora i radnih. U svakoj verziji aplikacije mogu smatrati razvojno okruženje za specifičnim potrebama od proces implementacije. Na primjer okruženje značajke pitanja i odgovora mogu poslužiti vaš tim razvojnim inženjerima da biste testirali kvalitete aplikacije prije automatske promjene u.
Postavljanje više okruženju za razvoj može biti zahtjevne zadatak kad su vam potrebne za praćenje, Upravljanje resursima (računalnim, web-aplikacije, baze podataka, predmemorije itd.) i implementacija kod u okruženjima.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Postavljanje nije radnog okruženja (fazu, razvojni, značajke pitanja i odgovora)
Nakon što dodate web-aplikacijama radnog prema gore i pokrenut, sljedeći je korak radi stvaranja koje nisu radnog okruženja. Da biste mogli koristiti slobodnih implementaciju provjerite je li pokrećete u načinu za planiranje **Standardno** ili aplikacije servisa za **Premium** . Uvođenje slobodnih su zapravo uživo web-aplikacije s vlastite hostnames. Elementi sadržaja i konfiguracije web app možete se zamjenjuju između dva slobodnih implementaciju, uključujući vremensko razdoblje radnog. Implementacija aplikacije da biste vremensko razdoblje za implementaciju ima sljedeće prednosti:

1. Prije zamjena s vremensko razdoblje radnog može provjeriti valjanost web app promjene u pripremna vremensko razdoblje implementacije.
2. Najprije implementacija web-aplikacijama na vremensko razdoblje i zamjena u radnog osigurava da su sve instance na vremensko razdoblje warmed prije nego što se zamjenjuju u radni. To uklanja nedostupnost kada implementacija web-aplikaciju programa. Preusmjeravanje prometa je objedinjenog i bez zahtjeva za ispuštaju se zbog operacije Zamijeni. U ovom cijeli tijek rada moguće je automatizirati konfiguriranjem [Zamijenili automatski](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) prilikom provjere valjanosti stara Zamijeni nije potrebna.
3. Nakon Zamijeni, vremensko razdoblje i prethodno postupnu web app sada ima prethodne radnog web-aplikaciji. Ako su promjene zamjenjuju u jedno područje radnog ne onako kako ste očekivali, možete izvršiti iste zamjena odmah da biste dobili "zadnji poznati dobar web-aplikaciju programa" natrag.

Da biste postavili pripremna vremensko razdoblje implementaciju, potražite u članku [Postavljanje pripremna okruženja za web-aplikacije u servisu Azure aplikacije](web-sites-staged-publishing.md). Okruženje za svaku trebali biste uključiti vlastiti skup resursa za primjer ako web-aplikacija koristi bazu podataka, a zatim Proizvodnja i pripremna web-aplikacije trebali biste koristiti različite baze podataka. Dodajte pripremna resurse razvojno okruženje kao što su baze podataka, za pohranu ili predmemorije za postavljanje pripremna razvojno okruženje.

## <a name="examples-of-using-multiple-development-environments"></a>Primjeri korištenja više okruženju za razvoj

Neki projekt morate slijediti kod upravljanja za izvor s najmanje dva okruženja, razvoj i radnog okruženja, ali kada koriste sustave za upravljanje sadržajem, okviri aplikacije etc. ćemo naići problemi gdje aplikacija ne podržavaju taj scenarij izvan okvira za. To vrijedi za neke popularne okviri opisan u nastavku. Veći broj pitanja Dođite na umu prilikom rada s na a CMS/okviri kao što su

1. Kako prekinuti smanjivati u različitim okruženjima
2. Koje datoteke možete promijeniti i riješi problem utjecati framework ažuriranja verzija
3. Kako upravljati konfiguracije po okruženju
4. Kako upravljati ažuriranja verzija modula/dodaci, core framework verzije ažuriranja

Postoji mnogo načina da biste postavili okruženje za više projekta i primjere u nastavku su samo jednu takav način odgovarajući aplikacije.

### <a name="wordpress"></a>WordPress
U ovom odjeljku će Saznajte kako postaviti pomoću slobodnih WordPress implementacije tijeka rada. WordPress kao što je većina a CMS rješenja ne podržava rad s više okruženju za razvoj iz okvir. Aplikacije servisa za Web Apps sadrži nekoliko značajki koje olakšavaju spremanje postavki konfiguriranje izvan kod.

Prije stvaranja pripremna vremensko razdoblje, postaviti kod aplikacija podržava više okruženja. Podržava više okruženja WordPress ćete morati urediti `wp-config.php` na web-aplikaciju programa lokalne razvoj dodati sljedeći kod na početku datoteku. To će omogućuje aplikacije da biste odabrali ispravnu konfiguraciju ovise o okruženju za odabrani.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Stvaranje mape u odjeljku web app korijenski pod nazivom `config` i dodajte datoteku dvije datoteke: `wp-config.azure.php` i `wp-config.local.php` koji predstavlja vaše okruženje azure i lokalnih odnosno.

Kopirajte sljedeće u `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Postavljanje sigurnosnih ključeva iznad mogu pomoći onemogućuje se hacked web-aplikaciju programa, pa jedinstvene vrijednosti. Ako vam je potrebna radi generiranja niza za sigurnosni ključevi spomenutih, možete ići na automatsko generator da biste stvorili novi tipke/vrijednosti pomoću [vezu] (https://api.wordpress.org/secret-key/1.1/salt)

Kopirajte sljedeći kod u `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Korištenje relativni putovi
Jedan preostaje je da biste konfigurirali aplikaciju WordPress da biste koristili relativni putovi. WordPress pohranjuje URL podaci u bazi podataka. Na taj način premještanja sadržaja iz jednog okruženja na drugu teže vam je potrebno ažurirati bazu podataka svaki put kada premještate iz lokalne faza ili faze na radnog okruženja. Da biste smanjili rizik od problemi koji se može uzrokovati uz implementaciju baze podataka prilikom svakog implementacija iz jednog okruženja u drugu pomoću [dodatak relativni korijenski veze](https://wordpress.org/plugins/root-relative-urls/) koje možete instalirati putem nadzorne ploče WordPress administratora ili ručno preuzimanje iz [ovdje](https://downloads.wordpress.org/plugin/root-relative-urls.zip).


Dodajte sljedeće unose u vašem `wp-config.php` datoteku prije na `That's all, stop editing!` komentar:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktiviranje dodatak kroz na `Plugins` izbornika WordPress Administrator nadzornoj ploči. Spremanje postavki Stalna veza za WordPress aplikacije.

#### <a name="the-final-wp-configphp-file"></a>Konačnih `wp-config.php` datoteka
Sva ažuriranja WordPress Core neće utjecati na `wp-config.php`, `wp-config.azure.php` i `wp-config.local.php` datoteke. Na kraju ovog kako `wp-config.php` datoteke izgledat će ovako

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Postavljanje okruženja pripremna
Pod pretpostavkom da ste već imate web-aplikacijama WordPress izvodi na webu Azure, prijavite se na [portal Azure upravljanja pretpregled](http://portal.azure.com) web-mjesta i idite na web-aplikaciju WordPress. Ako aplikacija ne možete stvoriti jednu iz trgovine. Da biste saznali više, [kliknite ovdje](web-sites-php-web-site-gallery.md).
Kliknite Postavke -> implementacije slobodnih -> Dodaj da biste stvorili vremensko razdoblje za implementaciju s radni prostor za naziv. Razdoblje za implementaciju je zajedničko korištenje iste resurse kao primarni web-aplikaciji stvorena iznad drugog web-aplikacije.

![Stvaranje vremensko razdoblje faza implementacije](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Dodavanje druge baze podataka MySQL izgovorite `wordpress-stage-db` u grupu resursa `wordpressapp-group`.

 ![Dodavanje baze podataka MySQL grupa resursa](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

Ažuriranje niza veze za vaše faza implementacije vremensko razdoblje da upućuju na novostvorenom bazu podataka, `wordpress-stage-db`. Napomena proizvodnje web-aplikacije, `wordpressprodapp` i pripremna web app `wordpressprodapp-stage` moraju pokazivati različite baze podataka.

#### <a name="configure-environment-specific-app-settings"></a>Konfiguriranje postavki specifične za okruženja aplikacije
Razvojni inženjeri možete spremiti parove ključa vrijednosti niza u Azure kao dio informacije o konfiguraciji povezan s web-aplikacijama naziva postavki aplikacije. Tijekom izvođenja aplikacije servisa web-aplikacije automatski dohvaća te vrijednosti za vas i postaje dostupan za kod koji se izvodi u web-aplikaciji. Iz vrijednosnicu perspektive koji je bolje strana prednosti od povjerljive podatke kao što su nizu za povezivanje baze podataka pomoću lozinke nikad ne prikazuju kao običnog teksta u datoteci kao što su `wp-config.php`.

Ovaj postupak definirani ispod koristan je prilikom izvršavanja kao sadrži promjene datoteke i promjene baze podataka za aplikaciju WordPress:
- Nadogradnja WordPress verzija
- Dodavanje novog ili uređivanje ili nadogradnja dodataka
- Dodavanje novog ili uređivanje ili nadogradnja teme

Konfiguriranje postavki aplikacije za:

- informacije o bazi podataka
- Uključivanje/isključivanje WordPress zapisivanje
- Postavke sigurnosti WordPress

![Postavki aplikacije za Wordpress web app](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Provjerite je li za vremensko razdoblje vašeg radnog web app i faza koje ste dodali sljedeće postavke aplikacije. Imajte na umu da radnog web-aplikacije i pripremna web app koristiti različite baze podataka.
Poništite potvrdni okvir **Vremensko razdoblje postavke** za sve parametre postavke osim WP_ENV. To će zamijenite konfiguracije web App, uz sadržaj datoteka i baza podataka. Ako je **Postavka vremensko razdoblje** **uključen**, postavki aplikacije web-aplikacije i konfiguracija niza povezivanja neće premjestiti u okruženjima pri izvođenju operacije ZAMIJENI i dakle ako postoje promjene baze podataka to nije prekinut će web-aplikaciju programa radnog.

Implementacija lokalne razvojno okruženje web-aplikaciji faza web-aplikacije i baze podataka pomoću WebMatrix ili tool(s) po izboru kao što su FTP, brojka ili PhpMyAdmin.

![Web-dijaloški okvir Objavi matrice za WordPress web app](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Pregled i testiranje web-aplikaciju programa pripremna. Considering scenarij gdje je tema web-aplikaciji ažurirati, Evo pripremna web-aplikaciji.

![Pregled prije zamjena slobodnih pripremna web-aplikacije](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Ako se sve čini dobar, kliknite gumb **Zamjena** na pripremna web-aplikaciju programa da biste premjestili sadržaj radnog okruženja. U ovom slučaju koje Zamijeni web-aplikacije i baze podataka u okruženjima tijekom operacije svaki **Zamijeni** .

![Zamjena pretpregled promjene za WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Ako imate scenarij u kojem trebate samo automatske datoteke (nema ažuriranja baze podataka), zatim **provjerite** **Vremensko razdoblje postavku** sve baze podataka povezan *postavki aplikacije* i *Postavke niza veze* u web app postavka plohu unutar portala Azure pretpregled prije prelaska na ZAMIJENI. U ovom slučaju DB_NAME DB_HOST, DB_PASSWORD, DB_USER, zadanu postavku niza povezivanja treba neće se prikazivati u pretpregledu promjene pri izvođenju na **Zamijeni**. Za ovaj put kada ste cjelovit operacija **Zamjena** web-aplikaciji WordPress će imati ažuriranja datoteka **samo**.

Prije prelaska na ZAMIJENI, Evo web-aplikaciji WordPress radnog ![radnog web-aplikacije prije zamjena slobodnih](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Nakon operacije ZAMJENA tema je ažuriran na web-aplikaciju programa proizvodnje.

![Proizvodne web-aplikacije nakon zamjena slobodnih](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

U situaciji kada se morate **vratiti**, možete idite na postavke za aplikaciju radnog web i kliknite gumb **Zamijeni** da biste zamijenili web app i bazu podataka iz radnog pripremna vremensko razdoblje. Važno što biste trebali upamtiti jest da ako su promjene baze podataka isporučuje se uz **Zamijeni** operacije u bilo kojem trenutku, nakon čega sljedeći put ponovno implementirati pripremna web-aplikaciju programa morate uvesti bazu podataka mijenja u trenutnu bazu podataka za vaše pripremna web-aplikaciji koja može biti prethodnu radnog bazu podataka ili faze baze podataka.

#### <a name="summary"></a>Sažetak
Da biste generalize postupka za bilo koju aplikaciju s bazom podataka

1. Instalirajte aplikaciju na vašem lokalnom okruženju
2. Sadrže određene konfiguracije okruženja (lokalni i Azure Web App)
3. Postavljanje okruženja u kojima u u aplikaciju servisa web-aplikacije – pripremna, radni
4. Ako imate aplikaciju radnog već pokrenut na Azure, sinkronizirati radnog sadržaja (datoteka/kod + baze podataka) lokalne i pripremna okruženje za.
5. Razvoj aplikacija na vašem lokalnom okruženju
6. Postavite web-aplikaciju programa proizvodnje u odjeljku održavanja ili zaključani način i baze podataka za sinkronizaciju sadržaj iz proizvodnje pripremna baza podataka i razvojni okruženja
7. Implementacija pripremna okruženje i Test
8. Implementacija radnog okruženja
9. Ponovite korake od 4 do 6

### <a name="umbraco"></a>Umbraco
U ovom odjeljku ćete saznati kako a CMS Umbraco koristi prilagođene modula za implementaciju s preko više DevOps okruženju. U ovom se primjeru nudi drukčiji pristup s upravljanjem više okruženju za razvoj.

[A CMS Umbraco](http://umbraco.com/) jedan je od popular.NET a CMS rješenja koristi mnogo razvojnim inženjerima omogućuje [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul implementacija razvoj pripremna na radnog okruženja. Jednostavno možete stvoriti lokalnu razvojno okruženje za Umbraco a CMS web-aplikaciju programa Visual Studio ili WebMatrix.

1. Stvaranje web-aplikacije programa Umbraco s Visual Studio, [kliknite ovdje](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Da biste stvorili web-aplikaciju programa Umbraco WebMatrix, [kliknite ovdje](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Uvijek imajte na umu da biste uklonili na `install` mapa u odjeljku aplikacije, a nikada prenijeti na faza ili proizvodnje web-aplikacije. Za ovaj vodič koje ćete koristiti WebMatrix

#### <a name="set-up-a-staging-environment"></a>Postavljanje okruženja za pripremna
- Stvorite vremensko razdoblje za implementaciju prethodno navedenim Umbraco a CMS web App, ako već imate web-aplikaciju programa a CMS Umbraco prema gore i pokrenut. Ako ne možete stvoriti iz trgovine.

- Ažurirajte niz za povezivanje za vaše faza implementacije vremensko razdoblje da upućuju na novostvorenom bazu podataka, **umbraco faza db**. Proizvodne web-aplikacije (umbraositecms-1) i pripremna web-aplikacije (umbracositecms-1-faze) **morate** pokažite na različite baze podataka.

![Ažuriranje niz za povezivanje za pripremna web app s nove pripremna baze podataka](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Kliknite **Postavke Dohvati objavljivanja** za implementaciju vremensko razdoblje **fazi**. To će preuzimanje datoteke postavke objavljivanja koji sadrže sve informacije potrebno Visual Studio ili Web matrice da biste objavili aplikacije iz lokalne razvoju web-aplikacije Azure web App.

 ![Dohvaćanje objavljivanje postavka pripremna web-aplikacije](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Otvorite web-aplikaciju programa lokalne razvoja u **WebMatrix** ili **Visual Studio**. Pomoću ovog praktičnog vodiča koristim matrice Web i najprije vam je potrebno da biste uvezli datoteku s postavkama Objavi za pripremna web-aplikaciji

![Uvoz postavki Objavi Umbraco pomoću matrice Web](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Pregled promjena u dijaloškom okviru i implementacija lokalne web-aplikaciju programa Azure web App, *umbracositecms, 1 i razine*. Ako pokrenete datoteka izravno na web-aplikaciju programa pripremna izostavite će sve datoteke u na `~/app_data/TEMP/` mape kao što je to će regenerated pri prvom web-aplikaciji fazu rada. Treba izostaviti na `~/app_data/umbraco.config` datoteku kao toga, će biti regenerated.

![Pregled promjena Objavi u matrici web](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Nakon uspješno objavljivanje lokalne web-aplikaciji Umbraco pripremna web-aplikacije, dođite do web-aplikaciju programa pripremna i pokrenuti nekoliko testove neki problemi s.

#### <a name="set-up-courier2-deployment-module"></a>Postavljanje modul Courier2 implementacije
S [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul možete automatske sadržaj, listovi stilova, a zatim module za razvoj i informacije s jednostavan desnu tipku miša iz pripremna web-aplikacije radnog web App za dodatne implementacijama besplatne smetnja i smanjenje rizika otpornosti web-aplikaciju programa radnog pri implementaciji ažuriranje.
Kupite licence za Courier2 za domenu `*.azurewebsites.net` i prilagođenu domenu (primjerice http://abc.com) kada ste kupili licencu, postavite preuzete licenca (. LIC datoteka) u na `bin` mapu.

![Ispustite datoteku licence u mapi za smeće](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Preuzmite paket Courier2 iz [ovdje](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Prijavite se na web-aplikaciju fazu, izgovorite http://umbracocms-site-stage.azurewebsites.net/umbraco pa kliknite izbornik **za razvojne inženjere** i odaberite **paketa**. Kliknite na **instalirati** lokalni paketa

![Instalacijski paket Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Prijenos paketa courier2 pomoću instalacijski program.

![Prijenos paketa za courier modul](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Da biste konfigurirali ste morati ažurirati courier.config datoteke u mapi **konfiguracije** web-aplikacije.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

U odjeljku `<repositories>`, unesite radnog web-mjesta URL-a i korisničke informacije. Ako koristite davatelja članstva Umbraco zadani, zatim dodajte ID za administriranje korisnika u <user> sekciju. Ako koristite prilagođeni davatelja članstva Umbraco, koristite `<login>`,`<password>` Courier2 modul znati kako se povezati s web-mjesta radnog. Dodatne informacije, pregledajte [dokumentaciju](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) za Courier modul.

Isto tako, instalirati modul Courier na web-mjestu radnog i konfigurirajte je točka faza web App u njegove odgovarajući courier.config datoteke kao što je prikazano ovdje

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Kliknite karticu Courier2 a Umbraco CMS web app nadzornoj ploči i odaberite mjesta. Trebali biste vidjeti naziv spremište kao što je rečeno u `courier.config`. To se na proizvodnje i pripremna web-aplikacije.

![Prikaz odredišno web app spremište](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Sada omogućuje implementacija neki sadržaj s pripremna web-mjesta i web-mjesta radnog. Idite na sadržaj i odaberite postojeće stranice ili stvorite novu stranicu. Će odaberite postojeće stranice iz moje web-aplikacije koje naslov stranice mijenja se **Početak rada – novi** i sada kliknite **Spremi i Objavi**.

![Promjena naslova stranice i objavljivanje](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Sada odaberite izmijenjene stranicu i *desnom tipkom miša kliknite* da biste vidjeli sve mogućnosti. Kliknite **Courier** da biste prikazali dijaloški okvir za implementaciju. Kliknite na **uvođenja** da biste započeli implementacije

![Dijaloški okvir za implementaciju modula za Courier](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Pregled promjene pa kliknite Nastavi.

![Courier modul implementacije dijaloški okvir Pregledaj promjene](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Uvođenje zapisnika prikazuje implementacijskih uspio.

 ![Prikaz zapisnika implementacije iz modula Courier](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Pronađite web-aplikaciju programa proizvodnje da biste vidjeli ako su promjene vidljive.

 ![Pregledavanje radnih web-aplikacije](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Da biste saznali više o tome kako koristiti Courier, pregledajte dokumentaciju.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Kako nadograditi a CMS Umbraco verzija

Courier će pomoći implementirati uz nadogradnju iz jedne verzije a CMS Umbraco na drugi. Prilikom nadogradnje a CMS Umbraco verziju, morate potražiti nekompatibilnosti s module prilagođene ili drugih proizvođača moduli i Umbraco Core biblioteke. Kao preporučenim načinom rada

1. UVIJEK sigurnosno kopirajte web-aplikacije i baza podataka prije prelaska na nadogradnju. Na Azure Web App možete postaviti automatsko sigurnosno kopiranje za svoje web-mjesta pomoću sigurnosnog kopiranja značajki i vraćanje web-mjesta ako je potrebno pomoću vratiti značajke. Dodatne informacije potražite u članku [Stvaranje sigurnosne kopije web-aplikaciju programa](web-sites-backup.md) i [Vraćanje web-aplikaciju programa](web-sites-restore.md).

2. Provjerite kompatibilan s verzijom prelazite na pakete drugih proizvođača koji koristite. Značajke pakiranja preuzmite stranice, pogledajte kompatibilnosti projekta s verzijom a CMS Umbraco.

Dodatne informacije o nadogradnji web-aplikaciju programa lokalno, slijedite smjernice kao spomenute [u nastavku](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general).

Nakon nadogradnje na web-mjesto lokalnog razvoj objavite promjene pripremna web-aplikacije. Testirajte aplikacije i ako se sve čini dobar, gumbom **Zamijeni** da biste **zamijenili** pripremna web-mjesta radnog web App. Prilikom izvršavanja operacije **Zamijeni** , možete vidjeti promjene koje će utjecati u konfiguraciji web app. S ovim postupkom **Zamjena** smo su zamjena web-aplikacije i baza podataka. To znači da, nakon ZAMJENA web-aplikaciji radnog sada će pokažite na umbraco faza db baze podataka, a zatim će pripremna web-aplikacije pokažite na umbraco RN db baze podataka.

![Zamjena pretpregled za implementaciju a CMS Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Prednost zamjena web app i baza podataka:
1. Omogućuje vam da biste vratili prethodnu verziju web-aplikacije s drugom **Zamjena** ako postoje neki problemi aplikacije.
2. Za nadogradnju morate uvesti datoteke i bazu podataka iz pripremna web app radnog web-aplikacije i baze podataka. Može poći po zlu pri implementaciji datoteke i baze podataka na mnogo stvari. Pomoću značajke **Zamjena** od slobodnih ćemo smanjiti nedostupnost tijekom nadogradnje i smanjenju rizika od pogreške koje se mogu pojaviti pri implementaciji promjene.
3. Vam omogućuje da biste učinili **A / B testiranje** pomoću značajke za [testiranje u radnog](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

U ovom primjeru prikazano fleksibilnost platforme gdje možete sastaviti prilagođeni moduli slično Umbraco Courier modula za upravljanje implementacije u okruženjima.

## <a name="references"></a>Reference
[Agilno softver razvoja s aplikacije servisa za Azure](app-service-agile-software-development.md)

[Postavljanje pripremna okruženja za web-aplikacije u aplikacije servisa za Azure](web-sites-staged-publishing.md)

[Blokiranje pristupa web slobodnih implementacije koje nisu radnog](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
