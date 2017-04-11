<properties
    pageTitle="Implementacija web-aplikacijama Sails.js aplikacije servisa za Azure"
    description="Saznajte kako implementirati aplikaciju Node.js aplikacije servisa za Azure. Pomoću ovog praktičnog vodiča prikazuje kako implementirati Sails.js web-aplikacijama."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Implementacija web-aplikacijama Sails.js aplikacije servisa za Azure

Pomoću ovog praktičnog vodiča objašnjava implementacija aplikacije Sails.js za aplikacije servisa za Azure. U tijeku, možete je toga neke opće znanja o tome kako konfigurirati Node.js aplikacije da biste pokrenuli u aplikacije servisa. 

Imat ćete raditi poznavanje Sails.js. Pomoću ovog praktičnog vodiča nije namijenjen će vam pomoći u problema vezanih uz pokrenut Sail.js Općenito.


## <a name="prerequisites"></a>Preduvjeti

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Brojka](http://www.git-scm.com/downloads)
- [Azure EŽA](../xplat-cli-install.md)
- Račun za Microsoft Azure. Ako nemate račun, možete ga [Registrirajte se za besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F) ili [aktivirali svoje prednosti pretplatnika Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Da biste vidjeli aplikacije servisa za Azure u akciji prije registracije za račun za Azure, otvorite [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751). Postoji, možete odmah stvoriti short-lived starter aplikaciju u aplikacije servisa za – bez kreditne kartice potrebna, bez preuzete obveze.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Korak 1: Stvaranje aplikacije Sails.js lokalno

Najprije brzo stvoriti zadane aplikacije Sails.js u okruženje za razvoj slijedeći ove korake:

1. Otvorite naredbeni redak terminal po izboru i `CD` da biste radni imenik.

2. Stvaranje Sails.js aplikacije, a zatim ga pokrenuti:

        sails new <appname>
        cd <appname>
        sails lift

    Provjerite je li možete doći do zadanom početnom stranicom pri http://localhost:1377.

## <a name="step-2-create-the-azure-app-resource"></a>Korak 2: Stvaranje resursa Azure aplikacije

Nakon toga stvaranje aplikacije servisa za resurse u Azure. Namjeravate implementacija aplikacije Sails.js ga kasnije.

1. Prijavite se u sustav Azure poput pa:
1. U istoj terminal pretvoriti u načinu ASM i prijavite se Azure:

        azure config mode asm
        azure login

    Slijedite upit da biste nastavili prijavu u pregledniku pomoću Microsoftova računa koji ima Azure pretplatu.

2. Provjerite jeste li i dalje u korijenskom direktoriju Sails.js projekta. Stvaranje resursa aplikacije servisa za aplikacije u Azure s nazivom jedinstveni aplikacije pomoću sljedeće naredbe. URL adresa web app je http://&lt;appname >. azurewebsites.net.

        azure site create --git <appname>

    Slijedite upit za odabir Azure područja za uvođenje. Ako nikada ne postavite brojka/FTP implementacije vjerodajnice za pretplatu Azure, ćete i biti upit za njihovo stvaranje.

    Nakon stvaranja aplikacije resursa aplikacije servisa:

    - Sails.js aplikacija je brojka pokrenut,
    - Lokalnom spremištu brojka pokrenut je povezano s nove aplikacije servisa za aplikacije kao brojka koji je remote, aptly naziva "azure", a
    - I iisnode.yml datoteka je stvorena u korijenskom direktoriju. Da biste konfigurirali [iisnode](https://github.com/tjanczuk/iisnode), koji koristi aplikacije servisa za pokretanje aplikacije Node.js možete koristiti tu datoteku.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Korak 3: Konfiguriranje i implementacija aplikacije Sails.js

 Rad s Sails.js aplikaciju u aplikacije servisa za sastoji se od tri glavna koraka:

 - Konfiguriranje aplikacija za pokretanje u aplikacije servisa
 - Implementacija aplikacije servisa
 - Čitanje stderr i stdout zapisnika za otklanjanje poteškoća implementacije

Slijedite ove korake:

1. Otvorite novu datoteku iisnode.yml u korijenskom direktoriju i dodajte sljedeća dva retka:

        loggingEnabled: true
        logDirectory: iisnode

    Zapisivanje sada je omogućena za iisnode. Dodatne informacije o kako to funkcionira, potražite u članku  [prikupiti zapisnike stdout i stderr iz iisnode](app-service-web-nodejs-get-started.md#iisnodelog).

2. Otvorite config/env/production.js konfigurirati vaše okruženje proizvodnje i postavljanje `port` i `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Dokumentacija za ove postavke konfiguracije možete pronaći u  [Dokumentaciji Sails.js](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Ćete morati provjerite je li [Grunt](https://www.npmjs.com/package/grunt) kompatibilan s Azure, mrežne pogone. Verzija grunt manji od 1.0.0 koristi zastarjelih [glob](https://www.npmjs.com/package/glob) paket (manje od 5.0.14), koji ne podržava mrežnih jedinica. 

3. Otvorite package.json i promijenite u `grunt` verziju `1.0.0` i ukloniti sve `grunt-*` paketa. Vaše `dependencies` svojstvo trebao bi izgledati ovako:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. U package.json, dodajte sljedeće `engines` svojstva da biste postavili verziju Node.js da želimo.

        "engines": {
            "node": "6.6.0"
        },

6. Spremite promjene i testirali promjene da biste bili sigurni da aplikacije i dalje izvodi lokalno. Da biste to učinili, izbrišite na `node_modules` mapu, a zatim Pokreni:

        npm install
        sails lift

4. Sada pomoću brojka implementacija aplikacije Azure:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Na kraju, samo pokretanje uživo Azure aplikacije u pregledniku:

        azure site browse

    Prikazat će se sada iste Sails.js početnu stranicu.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Otklanjanje poteškoća s implementaciju sustava

Sails.js aplikacija ne uspije zbog nekog razloga u aplikaciji servisa, pronaći zapisnike stderr pomoći u rješavanju problema ga.
Dodatne informacije potražite u članku [prikupiti zapisnike stdout i stderr iz iisnode](app-service-web-nodejs-sails.md#iisnodelog).
Ako je uspješno pokrenut, zapisnika stdout treba Prikaži poznatih poruka:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Možete kontrolirati preciznosti zapisnika stdout u datoteci [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) . 

## <a name="connect-to-a-database-in-azure"></a>Povezivanje s bazom podataka u Azure

Da biste se povezali s bazom podataka u Azure, stvoriti bazu podataka po izboru u Azure, kao što su baze podataka SQL Azure MySQL, MongoDB, Azure (Redis) predmemorije, itd., i korištenje odgovarajućih [datastore prilagodnika](https://github.com/balderdashy/sails#compatibility) s njim povezati. Koraci u ovom odjeljku objašnjavaju način da biste se povezali s bazom podataka MySQL u Azure.

1. Slijedite na vodiča [ovdje](../store-php-create-mysql-database.md) da biste stvorili bazom podataka MySQL u Azure.

2. Iz vaše naredbenog retka terminal instalirati prilagodnik MySQL:

        npm install sails-mysql --save

3. Otvorite config/connections.js i dodati sljedeći objekt veze na popis: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Za svaki varijablu okruženja (`process.env.*`), prvo morate postaviti na servisu aplikacije. Da biste to učinili, pokrenite sljedeće naredbe iz vaše terminal. Sve informacije o vezi koje su vam je potrebna je na portalu za Azure (potražite u članku [Povezivanje u bazu podataka MySQL](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Stavljanje postavki u postavki Azure aplikacije zadržava osjetljive podatke iz izvora kontrole (brojka). Nakon toga će konfigurirati razvojno okruženje za korištenje iste podatke o vezi.

4. Otvorite config/local.js i dodati sljedeći objekt veze:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Tu konfiguraciju nadjačava postavke u datoteci config/connections.js lokalnom okruženju. Datoteka je isključio .gitignore zadani u projektu, tako da ga neće biti pohranjen u brojka. Sada se možete povezati s baze podataka MySQL iz Azure web-aplikaciju programa i na lokalni platforme.

4. Otvorite config/env/production.js konfigurirati vaše okruženje proizvodnje, a zatim dodajte sljedeće `models` objekta:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Otvorite config/env/development.js konfigurirati okruženje za razvoj, a zatim dodajte sljedeće `models` objekta:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`omogućuje korištenje značajki Migracija baze podataka za stvaranje i ažuriranje tablica baze podataka u vašem MySQL jednostavno. Međutim, `migrate: 'safe'` se koristi u okruženju sustava Azure (Proizvodnja) Sails.js ne dopušta korištenje `migrate: 'alter'` u okruženju proizvodnje (pogledajte  [Dokumentaciju Sails.js](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. Iz terminal, [Generiranje](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [API nacrt](http://sailsjs.org/documentation/concepts/blueprints) kao što ste obično činite, zatim pokreće `sails lift` da biste stvorili bazu podataka s Sails.js Migracija baze podataka. Ako, na primjer:

         sails generate api mywidget
         sails lift

    Na `mywidget` modela generira ta naredba je prazan, ali koristimo da bi se prikazala imamo povezivanje baza podataka.
    Kada pokrenete `sails lift`, stvara nedostaju tablice za u modelima aplikacije koristi.

6. Pristup nacrt API koji ste upravo stvorili u pregledniku. Ako, na primjer:

        http://localhost:1337/mywidget/create
    
    U API trebali biste vratili stvorene stavke u prozoru preglednika, što znači da je uspješno stvorena bazu podataka.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Automatske promjene za Azure i pronađite aplikaciju da biste bili sigurni da je i dalje funkcionira.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Pristup nacrt API Azure web-aplikacije. Ako, na primjer:

        http://<appname>.azurewebsites.net/mywidget/create

    Ako je na API vraća drugi novi unos, Azure web-aplikaciju programa govori u bazu podataka MySQL.

## <a name="more-resources"></a>Dodatni resursi

- [Početak rada s Node.js web-aplikacije u aplikacije servisa za Azure](app-service-web-nodejs-get-started.md)
- [Pomoću Node.js modula Azure aplikacije](../nodejs-use-node-modules-azure-apps.md)
