<properties
    pageTitle="WordPress Enterprise predmete na servisu Azure aplikacije | Microsoft Azure"
    description="Upute za hostiranje programa enterprise predmete WordPress na web-mjestu aplikacije servisa za Azure"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Enterprise predmete WordPress na aplikacije servisa za Azure

Aplikacije servisa za Azure okruženje skalabilni, siguran i jednostavno je za korištenje zaštita njihove privatnosti ovise ključnih, veliki skaliranja [WordPress] [ wordpress] web-mjesta. Microsoft sama pokreće enterprise klase web-mjesta kao što je [Office] [ officeblog] i [Bing] [ bingblog] blogovi. Ovaj dokument prikazuje kako možete koristiti Azure aplikacije servisa web-aplikacije da biste ostvarili i održavanje enterprise predmete, oblaku WordPress web-mjesto koje možete učiniti velik broj posjetitelja.

## <a name="architecture-and-planning"></a>Arhitektura i planiranje

Osnovna instalacija WordPress sadrži samo dva preduvjeti.

* **Baze podataka MySQL** - dostupne putem [ClearDB u trgovine Windows Azure][cdbnstore], možete upravljati vlastitim MySQL instalacije na virtualnim računalima sustava Azure pomoću [sustava Windows] ili[ mysqlwindows] ili [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB nudi nekoliko konfiguracijama MySQL s performanse različitih karakteristikama za svaki konfiguraciju. Potražite u članku [Iz trgovine Azure] [ cdbnstore] informacije o ponude dobili Azure trgovini ili izravno kao što se vidi na [web-mjestu ClearDB](http://www.cleardb.com/pricing.view).

* **i 5.2.4 ili noviji** – aplikacije servisa za Azure trenutno pružaju [i verzije 5.4, 5,5, i 5.6][phpwebsite].

    > [AZURE.NOTE] Preporučujemo da uvijek koristite na najnoviju verziju i da biste bili sigurni da imate najnovija sigurnosne popravke.

### <a name="basic-deployment"></a>Osnovni implementacije

Koristite samo basic preduvjetima, mogli biste stvoriti Osnovno rješenje unutar Azure područja.

![Azure web-aplikacije i baze podataka MySQL smješten u jedno područje Azure][basic-diagram]

Dok je to će vam omogućiti skaliranje iz aplikacije stvaranjem više instanci web-aplikacije web-mjesta, sve se hostira unutar centre za podatke u određenim regiji. Posjetitelji iz izvan tom području možete vidjeti odgovora biti dugo vrijeme prilikom korištenja web-mjesta i idite na centre za podatke u tom području prema dolje, pa ne aplikacije.


### <a name="multi-region-deployment"></a>Uvođenje regije

Pomoću Azure [Promet Upravitelj][trafficmanager], moguće je skalirali web-mjestu WordPress preko više zemljopisna područja tijekom koja omogućuje samo jedan URL za posjetitelji. Sve posjetitelji dolaze u putem upravitelja promet, a zatim usmjeriti na područje ovisno o konfiguraciji ujednačavanje opterećenja.

![Azure web app, smješten u više područja pomoću CDBR visoke dostupnosti usmjerivač da biste usmjerili na MySQL preko područja][multi-region-diagram]

Unutar svakog područja web-mjesta WordPress će i dalje skalirana preko više instanci web-aplikacije, ali ovaj skaliranje je područje određene; moguće je razlikuju od niskog promet one skalirana velikog prometa područja.

Replikaciju i usmjeravanje više baza podataka MySQL možete učiniti pomoću ClearDB na [CDBR visoke dostupnosti usmjerivač] [ cleardbscale] (prikazanom lijevom) ili [MySQL klaster CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Implementacija regije s medijskih sadržaja za pohranu i predmemoriranja
Ako web-mjesto prihvaća prijenosi i glavno računalo medijske datoteke, poslužite se spremište blobova platforme Azure. Ako vam je potrebna predmemoriranja, razmislite o [Redis predmemorije][rediscache], [Memcache oblaka](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)ili neki drugi predmemoriranja ponude iz [Trgovine Azure](http://azure.microsoft.com/gallery/store/).

![Azure web app, smješten u više područja pomoću CDBR visoke dostupnosti usmjerivač za MySQL s upravlja predmemorije, blobova i CDN][performance-diagram]

Spremište blobova platforme je raspodijeljenih zemlj preko područja prema zadanim postavkama, pa ne morate brinuti o replikaciju datoteke na svim web-mjesta. Možete omogućiti i Azure [Sadržaja raspodjele mreže (CDN)] [ cdn] za spremište blobova platforme koja distribuira datoteke da biste završili čvorove bliže da biste posjetiteljima.

### <a name="planning"></a>Planiranje

#### <a name="additional-requirements"></a>Dodatni preduvjeti

Da biste to učinili... | Tom se mogućnošću poslužite...
------------------------|-----------
**Prijenos i pohranu velikih datoteka** | [Dodatak za WordPress za korištenje spremište blobova platforme][storageplugin]
**Slanje e-pošte** | [SendGrid] [ storesendgrid] [WordPress dodatak za korištenje SendGrid] i[sendgridplugin]
**Prilagođenih naziva domena** | [Konfiguriranje prilagođenog naziva domene u aplikacije servisa za Azure][customdomain]
**HTTPS** | [Omogućivanje HTTPS za web-aplikaciju u aplikacije servisa za Azure][httpscustomdomain]
**Prije proizvodnje provjere valjanosti** | [Postavljanje pripremna okruženja za web-aplikacije u aplikacije servisa za Azure][staging] <p>Izmjena web-aplikaciji iz i pripremna radnog pomiče se za konfiguraciju WordPress. Potrebno je provjeriti da su sve postavke na preduvjeti za aplikaciju radnog ažuriraju prije prebacivanja postupnu aplikacije u radnog.</p>
**Nadzor i otklanjanje poteškoća** | [Omogućite zapisivanje Dijagnostika za web-aplikacije u aplikacije servisa za Azure] [ log] i [Monitor Web Apps u aplikacije servisa za Azure][monitor]
**Implementacija web-mjesta** | [Implementacija web app u aplikacije servisa za Azure][deploy]

#### <a name="availability-and-disaster-recovery"></a>Dostupnost i Izrada oporavak

Da biste to učinili... | Tom se mogućnošću poslužite...
------------------------|-----------
**Učitavanje saldo web-mjesta** ili **zemlj.-distribucija web-mjesta** | [Usmjeravanje prometa s Azure promet Manager][trafficmanager]
**Sigurnosno kopiranje i vraćanje** | [Sigurnosno kopiranje web app u aplikacije servisa za Azure] [ backup] i [Vraćanje web-aplikacijama u aplikacije servisa za Azure][restore]

#### <a name="performance"></a>Performanse

Da biste u oblak performanse se postiže prvenstveno putem predmemorije i skaliranje iz; No memorije, propusnost i druge atribute web-aplikacije koje se nalaze u razmatranje.

Da biste to učinili... | Tom se mogućnošću poslužite...
------------------------|-----------
**Razumijevanje mogućnosti instanci aplikacije servisa** | [Informacije o, uključujući mogućnosti aplikacije servisa za razine][websitepricing]
**Predmemorija resursi** | [Redis predmemorije][rediscache], [Memcache oblaka](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)ili neki drugi predmemoriranja ponude iz [Trgovine Azure](/gallery/store/)
**Promjena veličine aplikacije** | [Promijenite veličinu web-aplikacijama u aplikacije servisa za Azure] [ websitescale] i [ClearDB visoke dostupnosti usmjeravanje][cleardbscale]. Ako se odlučite za glavno računalo i upravljanje MySQL instalacije, trebali biste [MySQL klaster CGE] [ cge] za skaliranje Izlaz

#### <a name="migration"></a>Migracije

Postoje dva načina Migracija postojeće web-mjesto WordPress aplikacije servisa za Azure.

* ** [WordPress izvoz] [ export] ** – to izvozi sadržaj članka na blogu, koje možete uvesti novo mjesto WordPress na Azure aplikacije servisa za korištenje [dodatka za uvoz WordPress][import].

    > [AZURE.NOTE] Dok je ovaj postupak omogućuje vam da biste migrirali sadržaj, ga nije moguće migrirati sve dodaci, teme ili druge prilagodbe. To mora biti instaliran ponovno ručno.

* **Ručnoj migraciji** - [sigurnosnu kopiju vašeg web-mjesta] [ wordpressbackup] i [baza podataka][wordpressdbbackup], zatim ručno vraćanje web-aplikaciju u aplikacije servisa za Azure i povezane baze podataka MySQL koju želite migrirati visoko prilagođenih web-mjesta i izbjegli zamoran ručno instaliranje dodaci, tema i druge prilagodbe.

## <a name="step-by-step-instructions"></a>Detaljne upute

### <a name="create-a-wordpress-site"></a>Stvaranje web-mjesta WordPress

1. Pomoću [Servisa Azure Marketplace] [ cdbnstore] da biste stvorili bazom podataka MySQL veličine ste naveli u odjeljku [arhitektura i planiranje](#planning) u region(s) da će hostirati web-mjesta.

2. Slijedite korake u odjeljku [Stvaranje web-aplikacijama WordPress u aplikacije servisa za Azure] [ createwordpress] da biste stvorili web-aplikacijama WordPress. Prilikom stvaranja web-aplikacije, odaberite **Koristi postojeću bazu podataka MySQL** , a zatim baza podataka stvorena u koraku 1.

Ako premještate postojećem web-mjestu WordPress, potražite u članku [Migracija postojeće WordPress mjesto za Azure](#Migrate-an-existing-WordPress-site-to-Azure) nakon stvaranja novog web-aplikacijama.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Migracija postojeće web-mjesto WordPress za Azure

Kao što je rečeno u odjeljku [arhitektura i planiranje](#planning) , dva su načina za migriranje web-mjesta WordPress.

* **Izvoz i uvoz** – za web-mjesta bez koliko prilagodbe ili samo željeno da biste premjestili sadržaj.

* **sigurnosno kopiranje i vraćanje** – za web-mjesta s mnogo prilagodbe na mjesto na koje želite premjestiti sve.

Koristite neku od sljedećih odjeljaka za migriranje web-mjesta.

#### <a name="the-export-and-import-method"></a>Izvoz i uvoz način

1. Korištenje [WordPress izvoz] [ export] da biste izvezli postojećeg web-mjesta.

2. Stvaranje web-aplikacijama slijedeći korake u odjeljku [Stvaranje web-mjesta WordPress](#Create-a-new-WordPress-site) .

3. Prijavite se na web-mjesto WordPress na web-aplikacije, a zatim kliknite **Dodaci** -> **Dodaj novi**. Potražite i instalirajte dodatak za **Uvoz WordPress** .

4. Nakon što instalirate dodatak za uvoz kliknite **Alati** -> **Uvoz**, a zatim odaberite **WordPress** da biste koristili dodatak za uvoz WordPress.

5. Na stranici **Uvoz WordPress** kliknite **Odabir datoteke**. Dođite do datoteke WXR izvezene iz postojećeg WordPress web-mjesta, a zatim odaberite **Prijenos datoteke i uvoz**.

6. Kliknite **Pošalji**. Vas Uvoz uspješan.

8. Nakon što ste dovršili sve korake, ponovno pokrenite web-mjesta iz njegove web app plohu [Azure portal][mgmtportal].

Nakon uvoza web-mjesto, morate poduzeti sljedeće korake da biste omogućili postavke koje se ne nalazi u datoteku za uvoz.

Ako ste koristili to... | Postupak...
------------------ | ----------
**Trajne veze** | Na WordPress nadzornoj ploči novog web-mjesta kliknite **Postavke** -> **trajne veze** , a zatim Ažuriraj strukturu trajne veze
**Slika/medijske veze** | Da biste ažurirali veze na novo mjesto, koristite [dodatak za Velvet Blues ažuriranje URL-ovi][velvet], alatu pretraživanja i zamjene ili ručno u bazi podataka
**Tema** | Idite na **izgled** -> **tema** i ažuriranje tema web-mjesta po potrebi
**Izbornici** | Ako temu podržava izbornike, veza na početnu stranicu i dalje možda stare podređenu imenika ugrađen u njima. Idite na **izgled** -> **izbornike** i ih ažurirati

#### <a name="the-backup-and-restore-method"></a>Način sigurnosnog kopiranja i vraćanja

1. Sigurnosno kopiranje vaše postojeće WordPress web-mjesta pomoću informacija na [sigurnosne kopije WordPress][wordpressbackup].

2. Sigurnosno kopiranje s postojećom bazom podataka pomoću informacija u [sigurnosno kopiranje baze podataka][wordpressdbbackup].

3. Stvaranje baze podataka i vraćanja sigurnosne kopije.

    1. Kupite novu bazu podataka iz [Trgovine Azure][cdbnstore], ili postavljanje bazom podataka MySQL u sustavu [Windows] [ mysqlwindows] ili [Linux] [ mysqllinux] VM.

    2. Klijenti za MySQL kao što su [MySQL Workbench][workbench], povezivanje s novom bazom podataka i uvoz WordPress baze podataka.

    3. Ažuriranje baze podataka da biste promijenili stavke domene na nove aplikacije servisa za Azure domenu. Na primjer, mywordpress.azurewebsites.net. [Traženje i zamjena za skripte WordPress baza podataka] [ searchandreplace] da biste sigurno promijenili sve instance.

4. Stvaranje web-aplikacijama na portalu za Azure i objavljivanje WordPress sigurnosnu kopiju.

    1. Stvaranje web-aplikacijama [Azure portal] [ mgmtportal] s bazom podataka pomoću **Novo** -> **Web + Mobile** -> **Trgovine Windows Azure** -> **Web-aplikacije** -> **Web app + SQL** (ili **web-aplikacije + MySQL**) -> **Stvori**. Konfiguriranje sve potrebne postavke da biste stvorili praznu web-aplikaciju programa.

    2. U WordPress sigurnosnu kopiju, pronađite datoteku **wp config.php** i otvorite je u uređivaču. Informacije za nove baze podataka MySQL zamijenite sljedeće stavke.

        * **DB_NAME** - korisničko ime baze podataka

        * **DB_USER** - korisničko ime za pristup bazi podataka

        * **DB_PASSWORD** - korisničku lozinku

        Nakon promjene ove stavke, spremite i zatvorite datoteku **wp config.php** .

    3. Korištenje [uvođenja web app u aplikacije servisa za Azure] [ deploy] informacije da biste omogućili način implementacije koje želite koristiti, a zatim implementacija WordPress sigurnosnu kopiju na web-aplikaciju u aplikacije servisa za Azure.

5. Kada je postavila WordPress web-mjesta mora biti možete pristupiti pomoću novog web-mjesta (kao što je aplikacije servisa za web-aplikaciju programa) na *. azurewebsite.net URL web-mjesta.

### <a name="configure-your-site"></a>Konfiguriranje web-mjesta

Nakon WordPress web-mjesta stvoren je ili migrirati, poslužite se sljedećim informacijama poboljšati performanse i omogućivanje dodatne funkcije.

Da biste to učinili... | Tom se mogućnošću poslužite...
------------- | -----------
**Postavljanje aplikacije servisa za planiranje način, veličina i Omogući skaliranja** | [Promijenite veličinu web-aplikacijama u aplikacije servisa za Azure][websitescale]
**Omogućivanje veze stalnih baze podataka** | Prema zadanim postavkama WordPress pomoću veze stalnih baze podataka, što može izazvati vezu s bazom podataka za postaju ograničio vrijeme nakon više veza. Da biste omogućili trajne veze, instalirajte (trajne veze prilagodnik) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Poboljšanja performansi** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Onemogućivanje kolačića ARR</a> – možete poboljšati performanse prilikom izvođenja WordPress na više instanci web-aplikacije</p></li><li><p>Omogućivanje predmemoriranja. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis predmemorije</a> (pretpregled) mogu se s <a href="https://wordpress.org/plugins/redis-object-cache/">Redis dodatak za WordPress predmemorije objekta</a>ili nekim drugim predmemoriranja ponude iz <a href="/gallery/store/">Trgovine Azure</a></p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Kako ubrzati WordPress s Wincache</a> - Wincache po zadanom je omogućena za web-aplikacije</p></li><li><p><a href="../web-sites-scale/">Skaliranje web app u aplikacije servisa za Azure</a> i <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB visoke dostupnosti usmjeravanje</a> ili <a href="http://www.mysql.com/products/cluster/">CGE MySQL klaster</a></p></li></ul>
**Korištenje blob polja za pohranu** | <ol><li><p><a href="../storage-create-storage-account/">Stvorite račun za pohranu za Azure</a></p></li><li><p>Saznajte kako se <a href="../cdn-how-to-use/">koristi u mreži raspodjele sadržaja (CDN)</a> da biste zemlj.-distribucija podataka pohranjenih u BLOB-ova.</p></li><li><p>Instaliranje i konfiguriranje <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure prostor za pohranu WordPress dodatak</a>.</p><p>Detaljne postavljanje i informacije o konfiguraciji za dodatak, potražite u <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">vodiču</a>.</p> </li></ol>
**Omogućivanje e-pošte** | Omogućivanje <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> pomoću iz trgovine Azure. Instalirajte <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">dodatak za SendGrid</a> za WordPress.
**Konfiguriranje prilagođenog naziva domene** | [Konfiguriranje prilagođenog naziva domene u aplikacije servisa za Azure][customdomain]
**Omogućivanje HTTPS za prilagođeni naziv domene** | [Omogućivanje HTTPS za web-aplikaciju u aplikacije servisa za Azure][httpscustomdomain]
**Učitavanje saldo ili zemlj.-distribucija web-mjesta** | [Usmjeravanje prometa s Azure promet Upravitelj][trafficmanager]. Ako koristite prilagođenu domenu, potražite u članku [Konfiguriranje prilagođenog naziva domene u aplikacije servisa za Azure] [ customdomain] informacije o korištenju upravitelja promet s nazivima prilagođene domene
**Omogućivanje automatskog sigurnosnog kopiranja** | [Stvaranje sigurnosne kopije web app u aplikacije servisa za Azure][backup]
**Uključite Zapisivanje dijagnostičkih podataka** | [Omogućite zapisivanje Dijagnostika za web-aplikacije u aplikacije servisa za Azure][log]

## <a name="next-steps"></a>Daljnji koraci

* [Optimizacija WordPress](http://codex.wordpress.org/WordPress_Optimization)

* [Pretvaranje WordPress Multisite u aplikacije servisa za Azure](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB nadogradnje Čarobnjak za Azure](http://www.cleardb.com/store/azure/upgrade)

* [Hostiranje WordPress u podmapu web-aplikacije u aplikacije servisa za Azure](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Korak po korak: Stvaranje web-mjesta WordPress pomoću Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Glavno računalo postojeći WordPress blog na Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Omogućivanje pretty trajne veze u WordPress](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Kako se migrirati i pokrenuti WordPress blog aplikacije servisa za Azure](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Besplatno pokretanje WordPress na aplikacije servisa za Azure](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress na Azure u dvije minute ili manje](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Premještanje na blogu WordPress Azure - 1 dijela: stvaranje bloga WordPress na Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Premještanje bloga WordPress Azure – dio 2: prijenos sadržaja](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Premještanje bloga WordPress Azure – dio 3: postavljanje prilagođene domene](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Premještanje na blogu WordPress Azure - 4 dijela: Pretty trajne veze i URL dopune pravila](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Premještanje bloga WordPress Azure - 5 dijela: premještanje iz podmapa u korijenskom](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Kako postaviti WordPress web app u račun za Azure](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping gore WordPress na Azure](http://www.johnpapa.net/wordpress-on-azure/)

* [Savjeti za WordPress na Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
