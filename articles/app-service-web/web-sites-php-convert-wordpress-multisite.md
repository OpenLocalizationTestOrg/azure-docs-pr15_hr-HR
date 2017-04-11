<properties 
    pageTitle="Pretvaranje WordPress Multisite u aplikacije servisa za Azure" 
    description="Saznajte kako iskoristite postojeći WordPress web-aplikaciju programa stvorene pomoću galerije u Azure te ga pretvorili u WordPress Multisite" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Pretvaranje WordPress Multisite u aplikacije servisa za Azure

## <a name="overview"></a>Pregled

*Po [Ben Lobaugh][ben-lobaugh], [Microsoft Otvori tehnologije Inc.][ms-open-tech]*

U ovom ćete praktičnom vodiču će Saznajte kako postojeće WordPress web-aplikaciju programa stvorene pomoću galerije u Azure i pretvorite ga u WordPress Multisite Instaliraj. Uz to, ćete saznati kako dodijeliti prilagođenu domenu za svaki od podmjesta unutar instalacijom.

Pretpostavlja se da imate postojeće instalacije WordPress. Ako to ne učinite, slijedite upute navedene u [Stvaranje WordPress web-mjesta iz galerije u Azure][website-from-gallery].

Pretvaranje postojeće WordPress Instaliraj jednog web-mjesta da biste Multisite obično je prilično jednostavan i mnoge od tih početnog koraka potjecati izravno iz [Mreža stvaranje] [ wordpress-codex-create-a-network] stranice na [WordPress Codex](http://codex.wordpress.org).

Započnimo.

## <a name="allow-multisite"></a>Dopusti Multisite

Najprije morate omogućiti Multisite kroz na `wp-config.php` datoteka u **WP\_DOPUSTI\_MULTISITE** konstante. Postoje dva načina za uređivanje datoteka web app: prvi je putem FTP i drugi putem brojka. Ako niste upoznati s postupkom postavljanje neku od ovih metoda, pogledajte sljedeće upute:

* [Web-mjesto i s MySQL i FTP][website-w-mysql-and-ftp-ftp-setup]

* [Web-mjesto i s MySQL i brojka][website-w-mysql-and-git-git-setup]

Otvaranje u `wp-config.php` datoteku pomoću uređivača vašem odabiru i dodajte sljedeće iznad u `/* That's all, stop editing! Happy blogging. */` redak.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Svakako spremite datoteku i prenesite ga na poslužitelj!

## <a name="network-setup"></a>Postavljanje mreže

Prijavite se u područje *wp administrator* web-aplikacija, a trebali biste vidjeti novu stavku na izborniku **Alati** pod nazivom **Postavljanje mreže**. Kliknite **Postavljanje mreže** i ispunite detalje o vašoj mreži.

![Zaslon za postavljanje mreže][wordpress-network-setup]

Pomoću ovog praktičnog vodiča koristi shemi *podređenu direktorija* web-mjesta jer je uvijek surađivati i smo će biti postavljanje prilagođene domene za svako podmjesto kasnije u ovom praktičnom vodiču. Međutim, mora biti omogućuje postavljanje poddomenu instalirati ako pravilno mapiranje domene putem zamjenskih [Azure Portal](https://portal.azure.com) i postavljanje DNS-a.

Dodatne informacije o Dodavanje veze za poddomenu s vanjskih sub-directory postavu potražite u članku [vrste dodjeli resursa mrežni] [ wordpress-codex-types-of-networks] članak WordPress Codex.

## <a name="enable-the-network"></a>Omogućivanje mreže

Mreža konfigurirana sada u bazi podataka, ali postoji jedan preostale korake da biste omogućili funkcije mreže. Dovršavanje na `wp-config.php` postavke i provjerite jesu li `web.config` ispravno usmjerava svakog web-mjesta.


Nakon klika na gumb **instalirati** na stranici *Postavljanje mreže* , WordPress će pokušati ažuriranje na `wp-config.php` i `web.config` datoteke. Međutim, trebali biste uvijek provjeri datoteke da biste bili sigurni da su ažuriranja uspješno. Ako nije, ovaj zaslon prikazati vam potrebne ažuriranjima. Uređivanje, a zatim spremite datoteke.


Nakon ažuriranja ćete morati odjaviti se i prijavite se natrag u wp administratorske nadzorne ploče.

Sada treba dodatne izbornik na stranici Administracija traka s natpisima **Moja web-mjesta**. Izbornik omogućuje vam da biste odredili novi mreže putem **Mreže administratorske** nadzorne ploče.

## <a name="adding-custom-domains"></a>Dodavanje prilagođene domene

[WordPress MU domene mapiranja] [ wordpress-plugin-wordpress-mu-domain-mapping] dodatak olakšava Povjetarac za dodavanje prilagođene domene bilo kojeg mjesta u vašoj mreži. Dodatak za ispravno funkcioniranje, morate učiniti nekoliko dodatnih postavljanje na portalu, kao i kod registrara domena.

## <a name="enable-domain-mapping-to-the-web-app"></a>Omogućivanje mapiranje domene na web-aplikaciji

Način rada **slobodno** [Aplikacije servisa za](http://go.microsoft.com/fwlink/?LinkId=529714) planiranje ne podržava dodavanje prilagođene domene na web-aplikacije. Morat ćete prijeći u način rada za **zajedničko korištenje** ili **Standard** . Da biste to učinili:

* Prijavite se na Azure Portal i pronađite web-aplikaciju programa. 
* Na kartici **proširenja** u **postavkama**, kliknite.
* U odjeljku **Općenito**odaberite *zajednički se koristi* ili *STANDARD*
* Kliknite **Spremi**

Možete primiti poruku da biste potvrdili promjene te svjesni web-aplikaciju programa sada mogu uzrokovati trošak, ovisno o korištenju i druge konfiguracije postavite.

Potrebno nekoliko sekundi da obradi nove postavke pa sada je dobro vrijeme početka postavljanja domene.

## <a name="verify-your-domain"></a>Potvrda domene

Prije nego što Azure web-aplikacije će vam omogućiti da biste mapirali domene web-mjesto, najprije morate Provjerite imate li autorizacije da biste mapirali domene. Da biste to učinili, morate dodati novi CNAME zapis za unos DNS-a.

* Prijavite se u upravitelju DNS za svoju domenu
* Stvorite novi CNAME *awverify*
* Pokažite *awverify* *awverify. YOUR_DOMAIN.azurewebsites.NET*

To može potrajati neko vrijeme da bi promjene DNS odlaze učinke, pa ako sljedeći koraci ne funkcioniraju odmah, otvorite provjerite cup od primjer, a zatim vratite i pokušajte ponovno.

## <a name="add-the-domain-to-the-web-app"></a>Dodavanje domene u web-aplikaciji

Povratak na web-aplikaciju putem portala za Azure, kliknite **Postavke**, a zatim kliknite **prilagođenih domena i SSL**.

Kada se prikažu *SSL postavke* , vidjet ćete polja gdje će unos sve domene koje želite dodijeliti na web-aplikaciju. Ako domena nije ovdje navedeno, ona neće biti dostupna za mapiranje unutar WordPress, bez obzira na to kako domene DNS je postavljena.

![Dijaloški okvir prilagođene domene za upravljanje][wordpress-manage-domains]

Kad unesete domene u tekstni okvir, Azure ćete provjeriti CNAME zapis koji ste prethodno stvorili. Ako se DNS-om potpuno ne prenose, prikazat će se crveni indikator. Ako je uspjelo, prikazat će se Zelena kvačica. 

Uzeti u obzir IP adresu na popisu pri dnu dijaloškog okvira. Trebat će vam to postavljanje A zapis za svoju domenu.

## <a name="setup-the-domain-a-record"></a>Postavljanje domene zapisa

Ako su druge korake uspješan, možda sada dodijeliti domene Azure web aplikacije do odgovora DNS zapis. 

Važno Imajte na umu ovdje Azure web-aplikacije mora prihvatiti CNAME i zapisa, međutim, *morate* koristiti A zapis da biste omogućili mapiranje proper domena je. CNAME nije moguće proslijediti drugi CNAME koji je što Azure stvara s YOUR_DOMAIN.azurewebsites.net.

Koristi se s IP adresom u prethodnom koraku, vratite upravitelju DNS-a i postavljanje A zapis tako da pokazuje na tom IP.


## <a name="install-and-setup-the-plugin"></a>Instalacija i postavljanje dodatak

WordPress Multisite trenutno nema ugrađenu način da biste mapirali prilagođene domene. No postoji dodatak naziva [WordPress MU domene mapiranja] [ wordpress-plugin-wordpress-mu-domain-mapping] funkcionalnost koje doda umjesto vas. Prijavite se u mrežni administrator dio vašeg web-mjesta i instalirajte dodatak za **Mapiranje WordPress MU domene** .

Nakon što instalirate i aktivirate dodatak, posjetite **Postavke** > **Mapiranja domene** da biste konfigurirali dodatak. U prvi tekstni okvir *IP adresu poslužitelja*za unos IP adresa koji ste koristili za postavljanje A zapisa za domenu. Postavite sve *Mogućnosti domene* po volji (zadanih često su precizno) i kliknite **Spremi**.

## <a name="map-the-domain"></a>Mapiranje domene

Posjetite **nadzorne ploče** za web-mjesto koje želite mapirati domene koje želite. Kliknite **Alati** > **Mapiranja domene** i upišite novu domenu u tekstni okvir i kliknite **Dodaj**.

Prema zadanim postavkama, novu domenu će biti potrebno ponovno napisati na domenu autogenerated web-mjesta. Ako želite da su sve promet poslane na izborniku nova domena, potvrdite okvir *primarne domene za ovaj blog* prije spremanja. Neograničen broj domene možete dodati na web-mjesto, ali samo jedan može biti primarni.

## <a name="do-it-again"></a>Učini ponovno

Azure Web Apps omogućuju dodavanje neograničen broj domene web App. Da biste dodali drugu domenu morate izvršiti sekcije **stranici Potvrda domene** i **Postavljanje domene zapis** za svaku domenu.  

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
