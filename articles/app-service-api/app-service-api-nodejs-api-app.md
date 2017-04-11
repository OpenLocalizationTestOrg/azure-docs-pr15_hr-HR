<properties
    pageTitle="Node.js API aplikaciju u aplikacije servisa za Azure | Microsoft Azure"
    description="Saznajte kako stvoriti Node.js RESTful API i implementacija aplikacije API-JA u servisu Azure aplikacije."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Stvaranje Node.js RESTful API i njegova implementacija u API aplikaciji Azure

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti jednostavan [Node.js](http://nodejs.org) API-JA i implementirati [API aplikaciju](app-service-api-apps-why-best-platform.md) u [Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md) pomoću [brojka](http://git-scm.com). Koristite operacijski sustav koji se može izvoditi Node.js pa ćete učiniti sve što ste napravili putem alata naredbenog retka kao što su cmd.exe ili tulum.

## <a name="prerequisites"></a>Preduvjeti

1. Račun za Microsoft Azure ([otvorite besplatan račun](https://azure.microsoft.com/pricing/free-trial/))
1. [Node.js](http://nodejs.org) instaliran (ovaj uzorak podrazumijeva da imate Node.js verziju 4.2.2)
2. [Brojka](https://git-scm.com/) instaliran
1. [GitHub](https://github.com/) računa

Dok aplikacije servisa za podržava mnogo načina za implementaciju kod za aplikaciju API-JA, pomoću ovog praktičnog vodiča prikazuje način brojka i podrazumijeva da imate Osnovno znanje o tome kako raditi s brojka. Informacije o drugim načinima potražite u članku [uvođenja aplikacije za aplikacije servisa za Azure](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>Početak uzorak koda

1. Otvorite naredbenim sučeljem za redak koji se može izvoditi naredbe Node.js i brojka.

1. Dođite do mape koje možete koristiti za lokalnom spremištu brojka i Kloniraj [GitHub spremište koji sadrže kod uzorka](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    API uzorka nudi dva krajnje točke: Get zahtjev za `/contacts` vraća popis imena i adrese e-pošte u obliku JSON, dok `/contacts/{id}` vraća samo odabrani kontakt.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Scaffold (automatsko stvaranje) kod Node.js na temelju Swagger metapodataka

Oblik datoteke za metapodatke koji opisuje RESTful API [swagger](http://swagger.io/) je. Aplikacije servisa za Azure ima [ugrađenu podršku za Swagger metapodataka](app-service-api-metadata.md). U ovom odjeljku vodič modelima API razvoj tijek rada u kojem prvo stvorite Swagger metapodataka i koje koristite za scaffold (automatsko stvaranje) kod poslužitelja za na API-JA. 

>[AZURE.NOTE] Ako ne želite da biste saznali kako scaffold Node.js kod iz datoteke Swagger metapodataka, preskočite ovaj odjeljak. Ako želite samo uzorak koda implementirati novu aplikaciju API prijeđite izravno na odjeljak [Stvaranje aplikacije programa API-JA u Azure](#createapiapp) .

### <a name="install-and-execute-swaggerize"></a>Instalacija i izvršiti Swaggerize

1. Izvođenje sljedeće naredbe za instaliranje modula NPM **yo** i **generator swaggerize** globalno.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize je alat koje generira poslužitelj kod API opisane pomoću datoteke Swagger metapodataka. Datoteka Swagger koji ćete koristiti pod nazivom *api.json* i nalazi u mapi *pokretanje* u spremištu koje klonirana.

2. Dođite do mape u *start* , a zatim izvršite na `yo swaggerize` naredbe. Swaggerize će zatražiti niz pitanja.  Za **što da biste nazvali taj projekt**, "ContactList", unesite **put do swagger dokumenta**, unesite "api.json" te **zadovoljni, Express ili Restify**, "express".

        yo swaggerize

    ![Swaggerize naredbenog retka](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Napomena**: Ako naiđete na pogrešku u ovom ćete koraku, na sljedeći korak objašnjava alata za popravak.

    Swaggerize stvara mapom aplikacije, rukovatelja scaffolds i konfiguracijske datoteke i generira **package.json** datoteku. Modul eksplicitnih prikaz služi za generiranje stranice Swagger pomoći.  

3. Ako u `swaggerize` naredba ne uspijeva s "neočekivani token" ili "koji nisu valjani slijed" Pogreška, uzrok pogreške ispraviti uređivanjem generirani *package.json* datoteku. U na `regenerate` retka u odjeljku `scripts`, promijenite natrag kosa crta koja prethodi *api.json* kose crte, tako da u retku izgleda kao u sljedećem primjeru:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Dođite do mape koja sadrži scaffolded kod (u ovom slučaju */start/ContactList* podmape).

1. Pokrenite `npm install`.
    
        npm install
        
2. Instalirajte modul NPM **jsonpath** . 

        npm install --save jsonpath
        
    ![Instalacija Jsonpath](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. Instalirajte modul NPM **swaggerize korisničkog sučelja** . 

        npm install --save swaggerize-ui
        
    ![Swaggerize Instaliraj korisničkog sučelja](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>Prilagodba scaffolded kod

1. Kopirajte mapu **Biblioteka** iz mape **Početak** u mapu **ContactList** stvorenu u scaffolder. 

1. Zamijenite kod u datoteci **handlers/contacts.js** sljedeći kod. 

    Kod koristi JSON podatke pohranjene u datoteci **lib/contacts.json** koja je služila po **lib/contactRepository.js**. Nova šifra contacts.js odgovori HTTP zahtjeva da se svi kontakti i vratili ih kao JSON tereta. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. Zamijenite kod u datoteci **handlers/contacts/{id}.js** fofllowing kod. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. Zamijenite kod u **server.js** sljedeći kod. 

    Promjene unesene u datoteku server.js su oblačićima pomoću komentara da biste vidjeli promjene u tijeku. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>Testiranje s API-jem izvodi lokalno

1. Aktiviranje poslužitelja pomoću izvršne datoteke naredbenog retka za Node.js. 

        node server.js

1. Kada pregledavate Internet ****http://localhost:8000/kontaktima, pročitajte članak JSON Izlaz na popisu kontakata (ili se od vas zatraži da biste je preuzeli, ovisno o pregledniku). 

    ![Poziv Api svih kontakata](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Kada pregledavate ****kontakata/http://localhost:8000/2, prikazat će se kontakt predstavlja vrijednost ID-a.

    ![Određeni kontakt Api poziva](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Podaci Swagger JSON je poslužena putem krajnju **točku/swagger** :

    ![Kontakti Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Korisničko Sučelje Swagger je poslužena putem krajnju točku **/docs** . U Swagger UI obogaćenog HTML značajke klijenta možete koristiti da biste testirali izvan vaše API-JA.

    ![Swagger korisničkog sučelja](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Stvorite novu aplikaciju API-JA

U ovom odjeljku da biste stvorili novu aplikaciju API-JA u Azure pomoću portala za Azure. Ta aplikacija API predstavlja računalnim resursa koje Azure vam ponuditi da biste pokrenuli kod. U odjeljcima kasnije ćete implementacije kod novu aplikaciju API-JA.

1. Otvorite [Portal za Azure](https://portal.azure.com/). 

1. Kliknite **Novo > Web + Mobile > aplikacije za API**. 

    ![Nove aplikacije API-JA na portalu](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Unesite **naziv aplikacije** koje su jedinstvene *azurewebsites.net* domenu, kao što su NodejsAPIApp plus broj da bude jedinstven. 

    Na primjer, ako je naziv `NodejsAPIApp`, URL će biti `nodejsapiapp.azurewebsites.net`.

    Ako unesete naziv koji je netko drugi već koristio, vidjet ćete crveni uskličnik udesno.

6. U **Grupi resursa** padajući popis, kliknite **Novo**, a zatim u **novi naziv grupe resursa** unesite "NodejsAPIAppGroup" ili neki drugi naziv po želji. 

    [Grupa resursa](../azure-resource-manager/resource-group-overview.md) je zbirka Azure resurse kao što je aplikacija, baza podataka i VMs API-JA. Za ovaj vodič, preporučuje se da biste stvorili novu grupu resursa jer je koji olakšava da biste izbrisali u jednom koraku svih Azure resursa koje ste stvorili za vodič.

4. Kliknite **Plan mjesto aplikacije servisa za**, a zatim kliknite **Stvori novu**.

    ![Stvaranje aplikacije servisa za planiranje](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    U sljedećim koracima, stvaranje aplikacije servisa za Osmišljavanje novu grupu resursa. Tarifu aplikacije servisa za određuje računalnim resursi koji se izvodi aplikacije API-JA na. Na primjer, ako odaberete besplatne sloju, aplikacije API pokreće zajedničke VMs tijekom za neke plaćenu razine ga na namjenski VMs. Informacije o tarifama za aplikacije servisa za potražite u članku [Pregled aplikacije servisa za tarife](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. U plohu **Planiranje usluga aplikacije** unesite "NodejsAPIAppPlan" ili neki drugi naziv po želji.

5. Na padajućem popisu **mjesto** odaberite mjesto na koji je najsličniji vama.

    Tom se postavkom određuje koje Azure podatkovnog centra aplikacije funkcionirat će u. U ovom ćete praktičnom vodiču možete odabrati sve regije i ga neće učiniti uočljivijima razlika. Ali za aplikaciju proizvodnje, želite vaš poslužitelj najbliže moguće klijentima koji su mu da biste minimizirali [Latencija](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)pristupa.

5. Kliknite **sloju određivanje cijena > Prikaži sve > F1 besplatne**.

    Za ovaj vodič besplatne cijene sloju dat će dovoljno dobre performanse.

    ![Odaberite oslobađanje cijene sloju](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. U plohu **Plan za aplikaciju servisa** kliknite **u redu**.

7. U plohu **API aplikacije** kliknite **Stvori**.

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Postavljanje nove aplikacije API brojka implementacije

Kod ćete implementirati aplikaciju API pritiskom na pamti brojka spremište na servisu Azure aplikacije. U ovom ćete odjeljku vodiča za stvaranje vjerodajnice i brojka spremište u Azure koji će se koristiti za implementaciju.  

1. Nakon stvaranja aplikacije API-JA kliknite **aplikacije servisa > {API aplikacije}** na početnoj stranici portala. 

    Na portalu prikazuje blades **API aplikacije** i **Postavke** .

    ![Portal za aplikacije API-JA i plohu postavke](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. U plohu **Postavke** pomaknite se prema dolje do odjeljka za **Objavljivanje** , a zatim **Implementacija vjerodajnice**.
 
3. U plohu **Postavljanje vjerodajnica za implementaciju** unesite korisničko ime i lozinku, a zatim kliknite **Spremi**.

    Koristit ćete te vjerodajnice za objavljivanje Node.js kod u aplikaciju za API-JA. 

    ![Vjerodajnice za implementaciju](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. U plohu **Postavke** kliknite **implementaciju izvora > Odabir izvora > lokalnom spremištu brojka**, zatim kliknite **u redu**.

    ![Stvaranje Repo brojka](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Nakon što vaše spremište brojka promjene plohu da bi se prikazala na aktivni implementacije. Budući da je spremište novi, imate nije aktivna implementacijama na popisu. 

    ![Nema aktivnog implementacije](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Kopirajte URL brojka spremište. Da biste to učinili, prijeđite na plohu za novu Aplikciju API-JA i pogledajte u odjeljku **Osnove** na plohu. Obratite pozornost na to **brojka Kloniraj URL-a** u odjeljku **Osnove** . Kada pokazivač ovaj URL vam se prikazati ikona s desne strane će kopirajte URL u međuspremnik. Kliknite ikonu da biste kopirali URL-a.

    ![Dohvaćanje URL-a brojka s portala](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Napomena**: morat ćete Kloniraj brojka URL-a u sljedećem odjeljku pa provjerite da biste spremili negdje na trenutak.

Sad kad imate aplikaciju API-JA s brojka spremište sigurnosno kopiranje, možete automatske kod u spremištu za implementaciju kod aplikaciju API-JA. 

## <a name="deploy-your-api-code-to-azure"></a>Implementacija kod API Azure

U ovom odjeljku stvorite lokalni spremište brojka koja sadrži kod poslužitelja za na API-JA, a zatim ste automatske kod tog spremištu spremište u Azure koju ste ranije stvorili.

1. Kopiraj u `ContactList` mapu na mjesto na koje možete koristiti za nove lokalne brojka spremište. Ako niste prvi dio vodič, kopirajte `ContactList` iz na `start` mapu; u suprotnom, kopirajte `ContactList` iz na `end` mapu.

1. U alatu za naredbeni redak, idite u novu mapu, a zatim pokrenite sljedeću naredbu da biste stvorili novu lokalnu brojka spremište. 

        git init

     ![Nove lokalne brojka Repo](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Pokrenite sljedeću naredbu da biste dodali brojka koji je udaljene za spremište API aplikacije. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Napomena**: niz "YOUR_GIT_CLONE_URL_HERE" zamijenite vlastitim brojka Kloniraj URL koji ste prethodno kopirali. 

1. Izvođenje sljedeće naredbe za stvaranje potvrdi koja sadrži sve koda. 

        git add .
        git commit -m "initial revision"

    ![Izvršavanje brojka Izlaz](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Izvršavanje naredbe da biste kod na Azure. Kada se od vas zatraži za lozinku, unesite onaj koji ste prethodno stvorili Azure portal.

        git push azure master

    Time se pokreće implementacije u aplikaciju za API-JA.  

1. U pregledniku, vratite se na plohu **implementacijama** aplikacije API-JA, a vidite implementacijskih se pojavljuje. 

    ![Uvođenje događa](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Istodobno, sučelje naredbenog retka spremno odražava stanje implementaciju sustava dok ga se događa. 

    ![Čvor Js implementacije događa](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Kada implementacijskih završi, plohu **implementacijama** odražava uspješno uvođenje kod promjene u aplikaciju za API-JA. 

## <a name="test-with-the-api-running-in-azure"></a>Testiranje s API-jem radi u Azure
 
3. Kopirajte **URL** u odjeljku **Osnove** plohu vaše aplikacije API-JA. 

    ![Uvođenje dovršeno](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. Klijenti REST API-JA kao što su Postman ili Fiddler (ili web-preglednik), navedite URL kontakata API poziva, što je na `/contacts` krajnja točka aplikacije API-JA. Bit će URL-a`https://{your API app name}.azurewebsites.net/contacts`

    Kada zahtjevom GET problema na ovom krajnje točke, dobit ćete izlaz JSON aplikacije API-JA.

    ![Postman odlazak API-ja](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. U pregledniku idite na `/docs` krajnja točka za isprobavanje Swagger korisničkog Sučelja kao što je pokreće se u Azure.

Sad kad ste neprekinuti isporuke wired prema gore, možete mijenjati kod i uvesti ih Azure jednostavno pritiskom pamti vaše Azure brojka spremište.

## <a name="next-steps"></a>Daljnji koraci

Sada ste uspješno stvorili aplikaciju API-JA i uvesti kod Node.js API-JA. Sljedeći Praktični vodič prikazuje kako [zauzeti API aplikacije klijenata JavaScript pomoću CORS](app-service-api-cors-consume-javascript.md).
