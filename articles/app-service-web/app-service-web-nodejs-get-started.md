<properties
    pageTitle="Početak rada s Node.js web-aplikacije u aplikacije servisa za Azure"
    description="Saznajte kako implementirati Node.js aplikacije web-aplikaciju u aplikacije servisa za Azure."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Početak rada s Node.js web-aplikacije u aplikacije servisa za Azure

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti jednostavan [Node.js] aplikaciju i implementacija [Aplikacije servisa za Azure] iz naredbenog retka okruženja, kao što su cmd.exe ili tulum. U operacijskom sustavu koji se može izvoditi Node.js mogu slijediti upute u ovom ćete praktičnom vodiču.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Preduvjeti

- [Node.js]
- [Bower]
- [Yeoman]
- [Brojka]
- [Azure EŽA]
- Račun za Microsoft Azure. Ako nemate račun, možete ga [Registrirajte se za besplatnu probnu verziju] ili [aktivirali svoje prednosti pretplatnika Visual Studio].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Stvorite i implementirajte jednostavne Node.js web-aplikacijama

1. Otvorite naredbeni redak terminal po izboru, a zatim instalirati [Express generator za Yeoman].

        npm install -g generator-express

2. `CD`Da biste radni direktorij i generiranje eksplicitnih aplikacije pomoću sljedeće sintakse:

        yo express
        
    Odaberite željene mogućnosti sljedeći upit:  

    `? Would you like to create a new directory for your project?`**Da**  
    `? Enter directory name`**{appname}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Ništa**  
    `? Select a database to use:`**Ništa**  
    `? Select a build tool to use:`**Grunt**

3. `CD`u korijenskom direktoriju nove aplikacije i počnite ga da biste bili sigurni da će se pokrenuti u razvojno okruženje:

        npm start

    U pregledniku dođite do <http://localhost:3000> da biste provjerili možete li vidjeti na početnoj stranici Express. Kada potvrdite aplikacija se pokreće ispravno, koristite `Ctrl-C` da biste ga zaustavili.
    
1. Promjena u način ASM i prijavite se Azure (morate [Azure EŽA](#prereq)):

        azure config mode asm
        azure login

    Slijedite upit da biste nastavili prijavu u pregledniku pomoću Microsoftova računa koji ima Azure pretplatu.

2. Provjerite je li još u korijenskom direktoriju aplikacije, a zatim stvoriti aplikaciju resursa aplikacije servisa u Azure s nazivom jedinstveni aplikacije pomoću sljedeće naredbe. Na primjer: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Slijedite upit za odabir Azure područja za uvođenje. Ako nikada ne postavite brojka/FTP implementacije vjerodajnice za pretplatu Azure, ćete i biti upit za njihovo stvaranje.

3. Otvorite datoteku./config/config.js u korijenskom direktoriju aplikacije i promijenite priključak radnog `process.env.port`; vaše `production` svojstvo u na `config` objekt trebala bi izgledati kao u sljedećem primjeru:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Omogućuje Node.js aplikacije odgovoriti na web-zahtjeva na zadani je priključak te iisnode očekuje podatke.
    
4. Otvorite./package.json i dodajte u `engines` svojstva da biste [odredili željenoj verziji Node.js](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Spremite promjene, a zatim pomoću brojka za implementaciju aplikacije za Azure:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Express generator već sadrži datoteku .gitignore, stoga na `git push` ne koristi propusnost pokušavate prenijeti na node_modules / imeničkog.

5. Na kraju, pokretanje uživo Azure aplikacije u pregledniku:

        azure site browse

    Prikazat će se sada Node.js web-aplikaciju programa izvodi uživo u servisu Azure aplikacije.
    
    ![Primjer pregledavanja distribuiranih aplikaciju.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Ažuriranje web-aplikaciju programa Node.js

Da biste ažuriranja na Node.js web-aplikaciju u aplikacije servisa, jednostavno pokrenite `git add`, `git commit`, a `git push` kao da niste kada implementiran web-aplikaciju programa.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Kako aplikacije servisa za uvodi Node.js aplikacije

Aplikacije servisa za Azure koristi [iisnode] za izvođenje Node.js aplikacije. Azure EŽA i Kudu modul (brojka implementacija) zajedno daju pojednostavnjeno sučelje kada razviti i implementacija aplikacije Node.js iz naredbenog retka. 

- `azure site create --git`prepoznaje uobičajene uzorak Node.js server.js ili app.js i stvara sustava iisnode.yml u korijenskom direktoriju. Da biste prilagodili iisnode možete koristiti tu datoteku.
- Pri `git push azure master`, Kudu automatizira sljedeće zadatke implementacije:

    - Ako je package.json u korijenskoj mapi spremište, pokrenite `npm install --production`.
    - Generiranje konfiguraciju za iisnode koja upućuje na početak skripte u package.json (npr. server.js ili app.js).
    - Prilagodba Web.config radi spreman aplikacije za ispravljanje pogrešaka s čvor kontrola.
    
## <a name="use-a-nodejs-framework"></a>Korištenje Node.js framework

Ako koristite Popularni Node.js framework, kao što su [Sails.js] [ SAILSJS] ili [MEAN.js] [ MEANJS] razvoju aplikacija možete implementirati s aplikacije servisa. Popularne Node.js okviri imati njihove određene quirks i njihove ovisnosti paketa zadržati početak ažurirati. Međutim, aplikacije servisa za stavlja zapisnike stdout i stderr, da biste mogli dnevne točno s aplikacijom i unesite promjene u skladu s tim. Dodatne informacije potražite u članku [prikupiti zapisnike stdout i stderr iz iisnode](#iisnodelog).

Sljedeći vodiči za vidjet ćete kako raditi s određenim framework u aplikacije servisa:

- [Implementacija web-aplikacijama Sails.js aplikacije servisa za Azure]
- [Stvaranje aplikacije Node.js razgovor s Socket.IO u aplikacije servisa za Azure]
- [Kako koristiti io.js u Azure aplikacije servisa Web Apps]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Korištenje određene modul Node.js

U standardne tijekova rada, recite aplikacije servisa za korištenje određenih modul Node.js kao što to obično činite u package.json.
Ako, na primjer:

    "engines": {
        "node": "6.6.0"
    }, 

Modul za implementaciju Kudu određuje koje modul Node.js da biste koristili sljedećim redoslijedom:

- Prvo, pogledajte iisnode.yml jesu li `nodeProcessCommandLine` nije naveden. Ako je da, zatim koji koristite.
- Nakon toga pogledajte package.json jesu li `"node": "..."` je naveden u na `engines` objekt. Ako je da, zatim koji koristite.
- Odaberite zadanu verziju Node.js prema zadanim postavkama.

>[AZURE.NOTE] Preporučuje se eksplicitno definirati Node.js modul koji želite. Možete promijeniti zadanu verziju Node.js, a mogla bi vam se pogreške u Azure web-aplikaciju programa jer na zadanu verziju Node.js nije prikladna za aplikacije.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Dohvaćanje zapisnika stdout i stderr iz iisnode

Da biste pročitali iisnode zapisnike, slijedite ove korake.

> [AZURE.NOTE] Nakon tih koraka, datoteke zapisnika možda ne postoji dok se ne pojavljuje se pogreška.

1. Otvorite datoteku iisnode.yml koja omogućuje EŽA Azure.

2. Postavite dva parametra sljedeće: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Zajednički upućuju iisnode u aplikacije servisa da biste postavili rezultat stdout i stderror u D:\home\site\wwwroot\**iisnode** direktorija.

3. Spremite promjene, a zatim automatske promjene za Azure pomoću sljedeće naredbe i brojka:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    Sada je konfiguriran iisnode. Daljnji koraci pokazati kako pristupiti te zapisnika.
     
4. U pregledniku pristup Kudu konzole za ispravljanje pogrešaka za aplikaciju koja se nalazi pri:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Ovaj URL razlikuje od web-aplikacije URL po dodavanja "*.scm.*" naziv DNS-a. Ako izostavite te dodavanjem URL-a, dobit ćete o pogrešci 404.

5. Dođite do D:\home\site\wwwroot\iisnode

    ![Navigacija do mjesta datoteke zapisnika iisnode.][iislog-kudu-console-find]

6. Kliknite ikonu **Uređivanje** za zapisnik koju želite pročitati. Možete kliknuti i **Preuzimanje** ili **izbrisati** ako želite.

    ![Otvaranje datoteke zapisnika sustava iisnode.][iislog-kudu-console-open]

    Sada možete vidjeti zapisnika za implementaciju sustava aplikacije servisa za ispravljanje pogrešaka.
    
    ![Provjera datoteke zapisnika programa iisnode.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Ispravljanje pogrešaka aplikacije s čvor kontrola

Ako koristite čvor kontrola za ispravljanje pogrešaka aplikacije Node.js, možete ga koristiti za uživo aplikacije servisa za aplikaciju. Čvor kontrola predinstaliran je u instalaciji iisnode servisa za aplikacije. A ako implementacije kroz brojka Web.config automatski generirani iz Kudu već sadrži sve konfiguracije morate omogućiti čvor kontrola.

Da biste omogućili čvor kontrola, slijedite ove korake:

1. Otvorite iisnode.yml pri korijensko spremište i navedite sljedeće parametre: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Spremite promjene, a zatim automatske promjene za Azure pomoću sljedeće naredbe i brojka:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Sada samo dođite do datoteke start vaše aplikacije kao što je navedeno pomoću skripte početak u vašem package.json s /debug dodaje u URL. Na primjer,

        http://{appname}.azurewebsites.net/server.js/debug
    
    Ili,
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Dodatni resursi

- [Određivanje verzije Node.js u aplikaciji za Azure](../nodejs-specify-node-version-azure-apps.md)
- [Praktični savjeti i otklanjanje poteškoća vodič za Node.js aplikacije na Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Upute za ispravljanje pogrešaka u web-aplikacijama Node.js u aplikacije servisa za Azure](web-sites-nodejs-debug.md)
- [Pomoću Node.js modula Azure aplikacije](../nodejs-use-node-modules-azure-apps.md)
- [Aplikacije servisa za Azure web-aplikacije: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Razvojni centar za Node.js](/develop/nodejs/)
- [Početak rada s web-aplikacije u aplikacije servisa za Azure](app-service-web-get-started.md)
- [Istraživanje Super tajnu Kudu konzole za ispravljanje pogrešaka]

<!-- URL List -->

[Azure EŽA]: ../xplat-cli-install.md
[Aplikacije servisa za Azure]: ../app-service/app-service-value-prop-what-is.md
[Aktivacija sustava pogodnosti pretplatnika Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Stvaranje aplikacije Node.js razgovor s Socket.IO u aplikacije servisa za Azure]: ./web-sites-nodejs-chat-app-socketio.md
[Implementacija web-aplikacijama Sails.js aplikacije servisa za Azure]: ./app-service-web-nodejs-sails.md
[Istraživanje Super tajnu Kudu konzole za ispravljanje pogrešaka]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Express generator za Yeoman]: https://github.com/petecoop/generator-express
[Brojka]: http://www.git-scm.com/downloads
[Kako koristiti io.js u Azure aplikacije servisa Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[Prijavite se za besplatnu probnu verziju]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
