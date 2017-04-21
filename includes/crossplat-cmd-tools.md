#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Upute za korištenje alata za Azure naredbenog retka za Mac i Linux

Ovaj vodič opisuje kako pomoću alata za Azure naredbenog retka za Mac i Linux možete stvarati i upravljanje uslugama u Azure. Scenariji u kojima je moguće uvrstiti **Instaliranje alata**, **Uvoz postavki za objavljivanje**, **Stvaranje i upravljanje Azure web-mjesta**i **stvaranju pravila i upravljanju virtualnim računalima sustava Azure**. Sveobuhvatan referentnu dokumentaciju potražite u članku [Azure alata naredbenog retka za Mac i Linux dokumentaciju][reference-docs]. 

##<a name="table-of-contents"></a>Tablice sadržaja
* [Što su Alati za Azure naredbenog retka za Mac i Linux](#Overview)
* [Instalacija alata za Azure naredbenog retka za Mac i Linux](#Download)
* [Kako stvoriti račun za Azure](#CreateAccount)
* [Kako preuzeti i Uvoz postavki objavljivanja](#Account)
* [Kako stvoriti i upravljanje Azure Web-mjestom](#WebSites)
* [Kako stvoriti i upravljanje programa Azure virtualnog računala](#VMs)


<h2><a id="Overview"></a>Što su Alati za Azure naredbenog retka za Mac i Linux</h2>

Alati za Azure naredbenog retka za Mac i Linux su skup naredbenog retka alata za implementaciju i upravljanje uslugama Azure.
 
Podržani poslovi obuhvaćaju sljedeće:

* Uvezite postavke objavljivanja.
* Stvaranje i upravljanje Azure web-mjesta.
* Stvaranje i upravljanje virtualnim računalima sustava Azure.

Popis podržanih naredbi, upišite `azure -help` u naredbenom retku nakon instaliranja alata, ili potražite u [dokumentaciji referenca][reference-docs].

<h2><a id="Download">Instalacija alata Azure naredbenog retka za Mac i Linux</a></h2>

Sljedeći popis sadrži podatke za instaliranje alata naredbenog retka, ovisno o operacijskom sustavu:

* **Mac**: preuzimanje [instalacijskog programa Azure SDK][mac-installer]. Otvorite .pkg preuzetu datoteku, a zatim slijedite korake za instalaciju se od vas zatraži.

* **Linux**: instalirajte najnoviju verziju [Node.js] [ nodejs-org] (potražite u članku [Instalacija Node.js putem upravitelja paketima][install-node-linux]), pokrenite sljedeću naredbu:

        npm install azure-cli -g

    **Napomena**: možda ćete morati s dodatnim ovlastima pokrenite sljedeću naredbu:

        sudo npm install azure-cli -g

* **Windows**: pokretanje Winows instalacijskog programa (.msi datoteku), koji je dostupan ovdje: [Alati naredbenog retka za Azure][windows-installer].


Da biste testirali instalaciju, upišite `azure` naredbeni redak. Ako je instalacija bila uspješna, vidjet ćete popis svih dostupni `azure` naredbe.

<h2><a id="CreateAccount"></a>Kako stvoriti račun za Azure</h2>

Da biste koristili alate za Azure naredbenog retka za Mac i Linux, trebat će račun za Azure.

Otvorite web-preglednika, a zatim prijeđite do [http://www.windowsazure.com] [ windowsazuredotcom] i kliknite **besplatnu probnu verziju** u gornjem desnom kutu.

![Azure Web-mjesta][Azure Web Site]

Slijedite upute za stvaranje poslovnog subjekta.

<h2><a id="Account"></a>Kako preuzeti i Uvoz postavki objavljivanja</h2>

Da bismo započeli, morate preuzeti i uvoz vaše postavke objavljivanja. To će vam omogućiti za korištenje alata za stvaranje i upravljanje uslugama za Azure. Da biste preuzeli vaše postavke objavljivanja, koristite na `account download` naredba:

    azure account download

To će se otvoriti zadani preglednik i od vas zatražiti da se prijavite se na Portal za upravljanje. Nakon prijave u vašem `.publishsettings` preuzimanje datoteke. Zabilježite kojem je spremljena datoteka.

Nakon toga uvoz u `.publishsettings` datoteke pokretanjem sljedeće naredbe zamjene `{path to .publishsettings file}` putom do vaše `.publishsettings` datoteke:

    azure account import {path to .publishsettings file}

Možete ukloniti sve informacije pohraniti na <code>import</code> pomoću naredbe na <code>account clear</code> naredba:

    azure account clear

Da biste vidjeli popis mogućnosti za `account` koristite naredbe u `-help` mogućnost:

    azure account -help

Nakon uvoza vaše postavke objavljivanja, trebali biste izbrišite na `.publishsettings` datoteke sigurnosnih vam razloga.

> [AZURE.NOTE] Kada uvezete postavke objavljivanja, vjerodajnice za pristup pretplate Azure spremaju se unutar vaše `user` mapu. Vaše `user` je zaštićena operacijski sustav. Međutim, preporučuje se poduzimanja dodatnih koraka da biste šifrirali vaše `user` mapu. To možete učiniti na sljedeći način:    
> 
> - U sustavu Windows, izmijenite svojstva mape ili koristite BitLocker.
> - Na Mac, uključite FileVault mape.
> - Na Ubuntu, koristite značajku direktorija šifrirana Polazno. Distribucija druge Linux nude ekvivalentnu značajke.

Spremni ste za stvaranje i upravljanje Azure web-mjesta i virtualnim računalima sustava Azure.  

<h2><a id="WebSites"></a>Kako stvoriti i upravljanje web-mjestom programa Azure</h2>

###<a name="create-a-website"></a>Stvaranje web-mjesta

Da biste stvorili Azure web-mjesta, najprije stvorite prazan direktorij koji se naziva `MySite` i pregledavati u tom direktoriju.

Zatim, pokrenite sljedeću naredbu:

    azure site create MySite --git

Izlaz iz ta naredba sadržavat će na zadani URL novostvorenom web-mjesta. Na `--git` mogućnost omogućuje vam korištenje brojka za objavljivanje na web-mjesto tako da stvorite spremištima brojka u oba direktorija lokalnog računala i podatkovnog centra u web-mjesto. Imajte na umu da ako lokalnu mapu već brojka spremište, naredba dodat će na novi alat za analizu daljinske postojeće spremište, koja pokazuje na spremište u web-mjesto podatkovnog centra.

Imajte na umu da se može izvesti na `azure site create` naredbe ni u jednoj od sljedećih mogućnosti:

* `--location [location name]`. Ta mogućnost omogućuje da biste odredili lokaciju podatkovnog centra u kojem web-mjesto je stvorena (npr. "Zapad mi"). Ako izostavite tu mogućnost, bit će vam se promted da biste odabrali mjesto.
* `--hostname [custom host name]`. Ta mogućnost omogućuje da biste odredili prilagođeni naziv glavnog računala web-mjesta.

Zatim možete dodati sadržaj u imeniku web-mjesta. Koristite tijek običnog brojka (`git add`, `git commit`) da biste potvrdili sadržaj. Da biste sadržaj web-mjesta na Azure, koristite sljedeću naredbu brojka: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Postavljanje objavljivanja iz GitHub

Da biste postavili neprekinuti objavljivanje iz spremišta GitHub, koristite na `--GitHub` mogućnosti prilikom stvaranja web-mjesta:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Ako imate lokalni Kloniraj GitHub spremišta ili ako imate spremište s jednom udaljene referencom GitHub spremište, ta se naredba automatski objaviti kod u spremištu GitHub na web-mjesto. Od tada nadalje promjene pomiču spremište GitHub automatski će se objaviti na web-mjesto.

Kada postavite objavljivanje iz GitHub, zadanu granu koristi je osnovne grani. Da biste naveli drugi granu, pokrenite sljedeću naredbu na lokalnom spremištu:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>Konfiguriranje postavki aplikacije

Postavke za aplikaciju su parovima ključnih vrijednosti koje su dostupne u aplikaciji prilikom izvođenja. Kada je postavljeno na web-mjesta Azure, vrijednosti postavki aplikacije nadjačat će postavke s istim ključem koji su definirani u datoteci Web.config web-mjesta. Za Node.js i i aplikacijama postavki aplikacije su dostupni kao varijable okruženja. Sljedeći primjer prikazuje način da biste postavili par ključa vrijednosti:

    azure site config add <key>=<value> 

Da biste vidjeli popis svih parova ključa/vrijednosti, koristite sljedeće:

    azure site config list 

Ili ako znate ključ, a želite li vidjeti vrijednost, možete koristiti:

    azure site config get <key> 

Ako želite promijeniti vrijednost postojeće ključa morate poništiti postojeći ključ i zatim je ponovno dodajte. Naredba Očisti je:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Popis i prikaz web-mjesta

Da biste popis na web-mjesta, koristite sljedeću naredbu:

    azure site list

Da biste saznali više pojedinosti o web-mjesta, koristite na `site show` naredbe. Sljedeći primjer prikazuje detalje o `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Zaustavljanje, numeriranje, ili ponovno web-mjesta

Možete prekinuti, pokretanje ili ponovno pokrenite web-mjesta s na `site stop`, `site start`, ili `site restart` naredbe:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Brisanje web-mjesta

Na kraju, možete izbrisati web-mjesta s na `site delete` naredba:

    azure site delete MySite

Imajte na umu da ako koristite neku od iznad naredbe iz u mapi koju ste pokrenuli `site create`, morate navesti naziv web-mjesta `MySite` kao parametar zadnje.

Da biste vidjeli popis svih `site` koristite naredbe u `-help` mogućnost:

    azure site -help 

<h2><a id="VMs"></a>Kako stvoriti i upravljanje programa Azure virtualnog računala</h2>

virtualnog računala programa Azure stvara se iz virtualnog računala slike (.vhd datoteka) koje ste omogućili ili je dostupan u galeriji slika. Da biste vidjeli slike koje su dostupne, koristite na `vm image list` naredba:

    azure vm image list

Možete Dodjela resursa i počnite virtualnog računala neku od dostupnih slika s na `vm create` naredbe. Sljedeći primjer pokazuje kako stvoriti Linux virtualnog računala (pod nazivom `myVM`) slike u galeriji slika (CentOS 6.2). Korijenska korisničko ime i lozinka za virtualnog računala `myusername` i `Mypassw0rd` odnosno. (Imajte na umu da se `--location` parametar određuje podatkovnog centra u kojem je stvorena virtualnog računala. Ako izostavite na `--location` parametar, od vas će se zatražiti da biste odabrali mjesto.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Razmotrite Prenos na `--ssh` zastavice (Linux) ili `--rdp` Označi zastavicom (Windows) `vm create` da biste omogućili udaljene veze s novostvoreni virtualnog računala.

Ako biste radije Dodjela virtualnog računala s prilagođenu sliku, možete stvoriti sliku iz datoteke .vhd s na `vm image create` naredbu, a zatim pomoću na `vm create` naredba Dodjela virtualnog računala. Sljedeći primjer pokazuje kako stvoriti Linux slika (pod nazivom `myImage`) iz lokalnog .vhd datoteke. (U `--location` parametar određuje podataka u kojoj je pohranjen na slici.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Umjesto stvaranja sliku iz lokalnog .vhd, možete stvoriti sliku iz .vhd pohranjene u spremište blobova platforme Azure. To možete učiniti s na `blob-url` parametar:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Nakon stvaranja sliku, možete Dodjela virtualnog računala iz slike pomoću `vm create`. Naredba ispod stvara virtualnog računala pod nazivom `myVM` iz slike stvorena iznad (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Nakon što ste dodijeljeni resursi virtualnog računala, preporučujemo vam da biste stvorili krajnje točke dopustili daljinski pristup virtualnog računala (na primjer). Sljedeći primjer koristi u `vm create endpoint` naredbu da biste otvorili vanjskog priključka 22 i Lokalni priključak 22 na `myVM`:

    azure vm endpoint create myVM 22 22

Detaljne informacije o virtualnog računala (uključujući IP adresa, DNS naziva i podaci o krajnjoj točki) možete postići pomoću na `vm show` naredba:

    azure vm show myVM

Da biste isključili, pokretanje, ili ponovno pokrenite virtualnog računala, primijenite jednu od sljedećih naredbi:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

I na kraju, da biste izbrisali na VM, koristite na `vm delete` naredba:

    azure vm delete myVM

Popis svih naredbi za stvaranje i upravljanje virtualnim strojevima koristite na `-h` mogućnost:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

