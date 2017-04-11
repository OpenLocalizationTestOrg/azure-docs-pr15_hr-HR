<properties
    pageTitle="Upute za ispravljanje pogrešaka u web-aplikacijama Node.js u aplikacije servisa za Azure"
    description="Saznajte kako web-aplikacijama Node.js u Azure aplikacije servisa za ispravljanje pogrešaka."
    tags="azure-portal"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Upute za ispravljanje pogrešaka u web-aplikacijama Node.js u aplikacije servisa za Azure

Azure nudi ugrađene Dijagnostika radi jednostavnijeg Node.js aplikacije hostirane u web-aplikacijama [Azure aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=529714) za ispravljanje pogrešaka. U ovom se članku ćete saznati kako omogućite zapisivanje stdout i stderr, prikaz podataka o pogreškama u pregledniku te kako preuzeti i pregledavati datoteke zapisnika.

Dijagnostika za aplikacije Node.js hostirane na Azure osigurava [IISNode]. Dok je u ovom se članku govori o najčešće postavke za prikupljanje informacija Dijagnostika, ga ne nudi cjelovit referenca za rad s IISNode. Dodatne informacije o radu s IISNode potražite u članku [IISNode Readme] na GitHub.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Omogućite zapisivanje

Prema zadanim postavkama web-aplikaciju programa aplikacije servisa za samo snima dijagnostičke informacije o implementaciji, kao što su kada implementacija web-aplikacijama pomoću brojka. Ove informacije je korisno ako imate poteškoća s tijekom uvođenja, kao što su pogreške prilikom instalacije modula poziva **package.json**ili ako koristite prilagođeni implementacijsku skriptu.

Da biste omogućili zapisivanje stdout i stderr strujanja, morate stvoriti datoteku **IISNode.yml** u korijenu Node.js aplikacije i dodajte sljedeće:

    loggingEnabled: true

Time se omogućuje bilježenje stderr i stdout iz aplikacije Node.js.

Datoteka **IISNode.yml** može se koristiti i za kontrolu li neslužbeni pogreške ili pogrešaka za razvojne inženjere se vraćaju u preglednik kada dođe do pogreške. Da biste omogućili pogrešaka za razvojne inženjere, dodajte sljedeći redak **IISNode.yml** datoteku:

    devErrorsEnabled: true

Kada je omogućen tu mogućnost, IISNode će vratiti posljednje 64K podaci koji se šalju stderr umjesto neslužbeni pogreške kao što su "Pojavila se interna pogreška poslužitelja".

> [AZURE.NOTE] Dok devErrorsEnabled koristan je kada dijagnosticiranje problema prilikom razvoja, omogućivanje u okruženju proizvodnje može uzrokovati razvoj pogreške koja se šalje krajnjim korisnicima.

Ako datoteku **IISNode.yml** nije još ne postoji u aplikaciji, morate ponovno pokrenuti web-aplikaciju programa nakon objavljivanja ažurirane aplikacije. Ako samo želite promijeniti postavke u postojeću datoteku **IISNode.yml** prethodno objavljeni, potreban je bez ponovnog pokretanja.

> [AZURE.NOTE] Ako web-aplikaciju programa stvorena pomoću alata za Azure naredbenog retka ili cmdleta ljuske PowerShell Azure, automatski se stvara zadana **IISNode.yml** datoteka.

Da biste ponovno pokrenuli web-aplikaciju, odaberite web-aplikaciju na [Portal za Azure](https://portal.azure.com), a zatim **ponovno POKRENITE** gumb:

![ponovno pokrenite gumb][restart-button]

Ako u okruženje za razvoj su instalirani alati za Azure naredbenog retka, koristite sljedeću naredbu da biste ponovno pokrenuli web-aplikacije:

    azure site restart [sitename]

> [AZURE.NOTE] Dok loggingEnabled i devErrorsEnabled najčešće korištene IISNode.yml konfiguracija mogućnosti za snimanje dijagnostičke informacije, IISNode.yml može se koristiti za konfiguriranje raznih mogućnosti za okruženja za hosting. Potpuni popis mogućnosti konfiguracije potražite u članku [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) datoteku.

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Pristup zapisnika

Dijagnostičke zapisnike moguće pristupiti na tri načina; Korištenje na File Transfer Protocol (FTP), preuzimanje Zip arhive ili kao na uživo ažurirati strujanje zapisnika (poznat i kao krakom). Preuzimanje Zip arhive datoteka zapisnika ili prikaz aktivno strujanje zahtijevaju Alati za Azure naredbenog retka. Mogu se instalirati pomoću sljedeće naredbe:

    npm install azure-cli -g

Kad instalirate, alate možete pristupiti pomoću naredbe "azure". Alati naredbenog retka najprije morate ga konfigurirati da biste koristili Azure pretplatu. Informacije o tome da biste izvršili taj zadatak, potražite u članku na **kako preuzeti i Uvoz postavki objavljivanja** odjeljak u članku [upute za korištenje u Azure naredbenog retka Alati](../xplat-cli-connect.md) .

###<a name="ftp"></a>FTP

Da biste pristupili dijagnostičke informacije putem FTP, posjetite [Azure Portal](https://portal.azure.com), odaberite web-aplikaciju programa, a zatim **nadzorne PLOČE**. U odjeljku **Brze veze** veze **DIJAGNOSTIČKE ZAPISNIKE FTP** i **FTPS DIJAGNOSTIČKIH ZAPISNIKA** omogućuje pristup zapisnike pomoću protokola FTP.

> [AZURE.NOTE] Ako niste prethodno konfigurirali korisničko ime i lozinku za FTP ili implementaciju, to možete učiniti na stranici Upravljanje **brzi početak rada** tako da odaberete **Postavljanje vjerodajnica za implementaciju**.

URL FTP vraća na nadzornoj ploči je direktorij **LogFiles** koje će sadržavati sljedeće podređenu direktorija:

* [Način implementacije](web-sites-deploy.md) – ako koristite način implementacije kao što je brojka direktorij s istim nazivom stvorit će se i će sadržavati informacije vezane uz implementacije.

* nodejs - Stdout i stderr informacije zabilježene iz svih instanci aplikacije (kada je loggingEnabled je true)

###<a name="zip-archive"></a>ZIP arhive

Da biste preuzeli Zip arhive dijagnostičkih zapisnika, koristite sljedeću naredbu iz alata za Azure naredbenog retka:

    azure site log download [sitename]

To će se preuzeti **diagnostics.zip** u trenutnom direktoriju. Tu arhivu sadrži sljedeću strukturu direktorija:

* Implementacija - zapisnik informacije o implementaciji aplikacije

* LogFiles

    * [Način implementacije](web-sites-deploy.md) – ako koristite način implementacije kao što je brojka direktorij s istim nazivom stvorit će se i će sadržavati informacije vezane uz implementacije.

    * nodejs - Stdout i stderr informacije zabilježene iz svih instanci aplikacije (kada je loggingEnabled je true)

###<a name="live-stream-tail"></a>Aktivno strujanje (krakom)

Da biste pogledali uživo strujanje dijagnostičkih zapisnika podataka, koristite sljedeću naredbu iz alata za Azure naredbenog retka:

    azure site log tail [sitename]

Vratit će strujanje zapisnika događaja koji se ažuriraju kako nastaju na poslužitelju. Ovaj tok će vratiti informacije o implementaciji, kao i informacije o stdout i stderr (kada se loggingEnabled.)

<a id="nextsteps"></a>
## <a name="next-steps"></a>Daljnji koraci

U ovom članku naučili kako omogućiti i pristup informacijama Dijagnostika za Azure. Dok je taj podatak korisne razumijevanje problemi koji se pojavljuju s aplikacijom, možda će pokažite na problem s modulom koristite ili koji se razlikuje od onog koji je korišten u okruženje za implementaciju verziju Node.js koriste aplikacije servisa web-aplikacije.

Informacije u radu s modula na Azure potražite u članku [Korištenje modula Node.js s aplikacijama Azure](../nodejs-use-node-modules-azure-apps.md).

Informacije o određivanju verzije Node.js aplikacije, potražite u članku [Određivanje verzije Node.js u aplikaciji za Azure].

Dodatne informacije potražite u odjeljku [Razvojni centar za Node.js](/develop/nodejs/).

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

[IISNode]: https://github.com/tjanczuk/iisnode
[Datoteka pročitajme za IISNode]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Određivanje verzije Node.js u aplikaciji za Azure]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
