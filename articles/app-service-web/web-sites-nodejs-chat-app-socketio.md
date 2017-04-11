<properties
    pageTitle="Stvaranje aplikacije Node.js razgovor s Socket.IO u aplikacije servisa za Azure"
    description="Praktični vodič koji pokazuje pomoću socket.io u web-aplikacijama node.js hostirane na Azure."
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

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Stvaranje aplikacije Node.js razgovor s Socket.IO u aplikacije servisa za Azure

Socket.IO omogućuje u stvarnom vremenu komunikaciju između node.js poslužitelj i klijente pomoću WebSockets. Podržava i pričuvne da biste druge prijenosa (kao što su dugi ankete) koje funkcioniraju sa starijim preglednicima. Pomoću ovog praktičnog vodiča će vas voditi kroz hosting aplikacija za razgovor Socket.IO temelji kao Azure web-aplikaciju programa, a vam pokazati kako skaliranje aplikacije pomoću [Predmemorije Redis Azure]. Dodatne informacije o Socket.IO potražite u članku <http://socket.io/>.

> [AZURE.NOTE] Postupci u ovom ćete zadatku primjenjuju se na [Web-aplikacije servisa za aplikaciju]; Servisi u Oblaku, potražite u članku [Stvaranje aplikacija za razgovor za Node.js s Socket.IO na servis u Oblaku programa Azure].

## <a name="download-the-chat-example"></a>Preuzimanje primjer razgovor

Za taj projekt koristit ćemo primjeru razgovor [Socket.IO GitHub spremište]. Izvršite sljedeće korake da biste preuzeli primjeru i dodavanje projekt koji ste prethodno stvorili.

1.  Preuzimanje programa [ZIP ili GZ arhiviraju izdanje] Socket.IO projekta (verzija 1.3.5 koristila za taj dokument)

1.  Izdvajanje arhive i Kopiraj u **primjeri\\razgovor** imeničkog na novo mjesto. Na primjer, ** \\čvor\\razgovor**.

## <a name="modify-appjs-and-install-modules"></a>Izmjena app.js i instalirali module

1.  Preimenujte datoteku **index.js** **app.js**. Time se omogućuje Azure da biste otkrili je li to Node.js aplikacije.

1.  Otvorite **app.js** datoteku u uređivaču teksta. Promjena crta koja sadrži `var io = require('../..')(server);` kao što je prikazano u nastavku:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Otvorite datoteku **package.json** i dodavanje reference u socket.io u odjeljku `dependencies`, kao što je prikazano u nastavku:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Iz naredbenog retka, promijenite u ** \\čvor\\razgovor** direktorija i korištenje npm da biste instalirali module potrebnih ovu aplikaciju:

        npm install

    To će instalirati module u podmapu s nazivom **node_modules**.

## <a name="create-an-azure-web-app"></a>Stvaranje aplikacije za Azure Web

Slijedite ove korake da biste stvorili Azure web-aplikaciju programa, omogućite brojka objavljivanje, a zatim omogućiti WebSocket podrška za web-aplikaciji.

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure besplatnu probnu verziju</a>.

1. Instalirajte Azure sučelja naredbenog retka (Azure EŽA) i povezati Azure pretplatu. Pročitajte članak [Instaliranje i konfiguriranje Azure EŽA](../xplat-cli-install.md).

1. Ako je ovo prvi put postavljate spremište u Azure, morate stvoriti vjerodajnice za prijavu. Iz EŽA Azure unesite sljedeću naredbu:

        azure site deployment user set [username] [password]

1. Promijenite u ** \\node\chat** direktorija i koristite sljedeću naredbu da biste stvorili novi Azure web-aplikacije i lokalnom spremištu brojka. Ta naredba stvara i na brojka udaljene imenovani 'azure".

        azure site create mysitename --git

    "Mysitename" morate zamijeniti jedinstveni naziv za web-aplikacije.

1. Primjenu postojeće datoteke u lokalnom spremištu pomoću sljedeće naredbe:

        git add .
        git commit -m "Initial commit"

1. Slanje datoteke u spremište Azure web-aplikacije pomoću sljedeće naredbe:

        git push azure master

    Kada se to od vas zatraži, unesite vjerodajnice iz korak 2. Primit ćete poruke o statusu kao moduli uvoze se na poslužitelju. Kada se taj postupak dovrši, aplikacija će se nalaziti na Azure web-aplikaciju programa.

    > [AZURE.NOTE] Tijekom instalacije modul, mogli biste primijetiti pogrešaka koje ' uvezene projekta... nije pronađen ". To možete slobodno zanemariti.

1. Socket.IO koristi WebSockets koji nisu omogućeni po zadanom na Azure. Da biste omogućili web sockets, koristite sljedeću naredbu:

        azure site set -w

    Ako se to od vas zatraži, unesite naziv web-aplikacije.

    >[AZURE.NOTE]
    >"Azure web-mjesta set -w" naredba će samo sa verziju 0.7.4 ili veća od sučelje naredbenog retka Azure. Možete omogućiti i podršku WebSocket pomoću [Portala za Azure](https://portal.azure.com).
    >
    >Da biste omogućili WebSockets pomoću portala za Azure, kliknite web-aplikaciji iz web-aplikacije plohu pa **sve postavke** > **Postavke aplikacije**. **U odjeljku **Web Sockets**, kliknite.** Zatim kliknite **Spremi**.

1. Da biste vidjeli web-aplikaciji Azure, koristite sljedeću naredbu za pokretanje web-preglednik i idite na glavnom računalu web-aplikaciji:

        azure site browse

Pokrenite aplikaciju sada radi Azure i možete prenijeli poruku razgovora između različitih klijente pomoću Socket.IO.

## <a name="scale-out"></a>Vremensko mjerilo

Socket.IO aplikacije možete skalirana odgovor pomoću __prilagodnik__ za raspodjelu poruka i događaje između više instanci aplikacije. Iako postoje nekoliko prilagodnika, prilagodnik [socket.io redis] mogu se lako koristiti sa značajkom Azure Redis predmemoriju.

> [AZURE.NOTE] Dodatni obavezu skaliranje out rješenje Socket.IO je podrška za ljepljive sesije. Ljepljive sesije omogućene su po zadanom Azure web-aplikacijama kroz Azure zahtjev za usmjeravanje. Dodatne informacije potražite u članku [Instancu afinitet u Azure web-mjesta].

### <a name="create-a-redis-cache"></a>Stvaranje Redis predmemorije

Dovršite korake iz [predmemorije u predmemoriji Redis Azure Stvori] da biste stvorili novu predmemoriju.

> [AZURE.NOTE] Spremanje __naziv glavnog računala__ i __primarni ključ__ za predmemorije, kao što je to će biti potrebno u sljedećim koracima.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Dodavanje redis i socket.io redis moduli

1. Iz programa naredbenog retka, promijenite u __ \\čvor\\razgovor__ direktorija i koristite sljedeću naredbu.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] Verzije naveden u toj naredbi su verzije koristi kada testiranje ovog članka.

1. Izmjena __app.js__ datoteku dodajte sljedeće retke odmah nakon`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Zamijenite __redishostname__ i __rediskey__ naziv glavnog računala i ključ za Redis predmemoriju.

    To će se stvoriti u Objavi i pretplata klijenta u predmemoriju Redis stvoreno. Klijente zatim koriste prilagodnika da biste konfigurirali Socket.IO da biste koristili Redis predmemoriju za prosljeđivanje poruke i događaje između instanci aplikacije

    > [AZURE.NOTE] Dok __socket.io redis__ prilagodnika možete komunicirati izravno u Redis trenutnu verziju ne podržava provjeru autentičnosti potrebnih Azure Redis predmemorije. Stoga prvotne veze je stvoren pomoću modula __redis__ , a zatim klijent se prenosi __socket.io redis__ prilagodnika.
    >
    > Vrijeme predmemorije Redis Azure podržava sigurnu vezu koristi priključak 6380, module u ovom primjeru koriste ne podržava sigurnu vezu od 7/14/2014.. Iznad kod koristi zadanu, nesigurnom priključak 6379.

1. Spremanje izmijenjene __app.js__

### <a name="commit-changes-and-redeploy"></a>Zapiši promjene i ponovno implementirate

Iz naredbenog retka u u __ \\čvor\\razgovor__ direktorija, koristite sljedeće naredbe da biste potvrdili promjene i ponovno implementirate aplikaciju.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Nakon promjene ste je pritisak na poslužitelj, web-mjesto možete skaliranja preko više instanci pomoću sljedeće naredbe.

    azure site scale instances --instances #

Gdje __#__ je broj instanci da biste stvorili.

Možete se povezati na web-aplikaciju iz više preglednika ili računala da biste provjerili je li poruke ispravno šalju sve klijente.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a name="connection-limits"></a>Ograničenja povezivanja

Azure web-aplikacije dostupan je u više SKU-ove, koji određuju resursa koji su dostupni na web-mjesto. To obuhvaća broj dopuštenih WebSocket veza. Dodatne informacije potražite u članku [cijene za Web Apps stranice].

### <a name="messages-arent-being-sent-using-websockets"></a>Poruke nisu koja se šalje pomoću WebSockets

Ako klijent preglednici zadržati kvartalne natrag da biste dugo provjere umjesto korištenja WebSockets, možda je zbog nešto od sljedećeg.

* **Pokušajte ograničavanje da samo WebSockets prijenos**

    Kako bi Socket.IO da biste upotrijebili WebSockets razmjenu prijenosa, poslužitelja i klijenta mora podržavati WebSockets. Ako je jedan ili drugi, Socket.IO će sklopite drugi prijenosa, kao što su dugi ankete. Zadani popis prijenosa koristi Socket.IO je ` websocket, htmlfile, xhr-polling, jsonp-polling`. Možete nametnuti ga koristite dodavanjem sljedeći kod datoteku **app.js** nakon retka koji sadrži samo WebSockets `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Imajte na umu da starije preglednika koji podržavaju WebSockets neće moći povezati s web-mjesta u iznad kod je aktivna, kao što je ograničava komunikaciju na WebSockets samo.

* **Korištenje SSL**

    WebSockets ovisi o neke manje korištenih HTTP zaglavlja, kao što su zaglavlja **nadogradnju** . Neke Srednja mrežne uređaje, kao što je proxy poslužitelja za web, možete ukloniti te zaglavlja. Da biste izbjegli taj problem, možete uspostaviti vezu WebSocket putem SSL.

    Jednostavan je način za to je da biste konfigurirali Socket.IO za `match origin protocol`. To upućuje Socket.IO sigurnost komunikacije WebSockets jednak izvornom zahtjev HTTP/HTTPS web-stranice. Ako preglednik koristi HTTPS URL posjetite web-mjesta, koja slijedi WebSocket komunikaciju putem Socket.IO će se zaštićenim putem SSL.

    Da biste izmijenili u ovom se primjeru da biste omogućili tu konfiguraciju, dodajte sljedeći kod **app.js** datoteku nakon retka koji sadrži `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Provjerite postavke web.config**

    Azure web aplikacije koje hostira Node.js aplikacije pomoću **web.config** datoteke da biste usmjerili zahtjevi za aplikaciju Node.js. Za WebSockets funkciji pravilno s aplikacijama Node.js **web.config** mora sadržavati sljedeće stavke.

        <webSocket enabled="false"/>

    To onemogućuje IIS WebSockets modul koji sadrži vlastitu implementaciju WebSockets i u sukobu s Node.js određene WebSocket moduli kao što su Socket.IO. Ako nema redak ili je postavljena na `true`, to može biti razloga zbog kojeg prijenosa WebSocket ne funkcioniraju za svoju aplikaciju.

    Obično Node.js aplikacije nemojte uvrstiti datoteku **web.config** tako Azure web-mjesta će automatski stvoriti jednu za aplikacije Node.js prilikom implementacije. Budući da tu datoteku automatski se generiraju na poslužitelju, morate koristiti FTP ili FTPS URL web-mjesta za prikaz datoteke. FTP i FTPS URL-ovi web-mjesta na portalu klasični možete pronaći tako da odaberete web-aplikaciju programa, a zatim vezu **nadzorne ploče** . U odjeljku **brzi pogled** prikazuju se URL-ove.

    > [AZURE.NOTE] **Web.config** datoteke samo web-mjesta Azure ako generira aplikacije navedite ga. Ako unesete **web.config** datoteke u korijenskoj mapi aplikacije projekta, koristit će se tako da Azure web-aplikacije.

    Ako se stavka ne postoji ili je postavljeno na vrijednost `true`, trebali biste stvoriti **web.config** u korijenskom direktoriju Node.js aplikacije, a zatim odredite vrijednost `false`.  Za referencu, na ispod je zadani **web.config** za aplikaciju koja koristi **app.js** kao točku unosa.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Ako aplikacija koristi ulaza osim **app.js**, sva pojavljivanja **app.js** morate zamijeniti točke točan unos. Na primjer, zamjena **app.js** **server.js**.

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa], gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako stvoriti aplikaciju razgovor smješten u Azure web-aplikaciju programa. Možete hostirati i ovaj program kao servis oblaka Azure. Upute o tome kako biste to postigli potražite u članku [Sastavljanje aplikacija za razgovor za Node.js s Socket.IO na servis u Oblaku programa Azure].

Dodatne informacije potražite u odjeljku [Razvojni centar za Node.js].

## <a name="whats-changed"></a>Što se promijenilo

* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima].

<!-- URL List -->

[Azure Redis predmemorije]: /documentation/services/redis-cache/
[Aplikacije servisa za web-aplikacije]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web-aplikacije cijene stranice]: http://go.microsoft.com/fwlink/?LinkId=511643
[Sastavljanje Node.js razgovor aplikacijom Socket.IO na Azure Oblaku]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Aplikacije servisa za Azure i njegov utjecaj na postojećim Azure servisima]: http://go.microsoft.com/fwlink/?LinkId=529714
[Razvojni centar za Node.js]: /develop/nodejs/
[Pokušajte aplikacije servisa]: http://go.microsoft.com/fwlink/?LinkId=523751
[Instance afinitet u Azure web-mjesta]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Stvoriti predmemoriju u predmemoriji Redis Azure]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[Socket.IO redis]: https://github.com/socketio/socket.io-redis
[Spremište Socket.IO GitHub]: https://github.com/socketio/socket.io
[ZIP ili GZ arhiviraju izdanje]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
