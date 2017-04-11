<properties
    pageTitle="Određivanje verzije Node.js"
    description="Saznajte kako odrediti verziju Node.js koriste Azure web-mjesta i servise u Oblaku"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Određivanje verzije Node.js u aplikaciji za Azure

Kada u kojem se nalazi Node.js aplikacije, preporučujemo vam da biste bili sigurni da vaša aplikacija koristi određenu verziju programa Node.js. Da biste to postigli za aplikacije koje se nalaze na Azure na nekoliko načina.

##<a name="default-versions"></a>Zadani verzije

Verzija Node.js nudi Azure neprestano ažuriraju. Osim ako je naveden u suprotnom, na zadanu verziju koja je navedena u na `WEBSITE_NODE_DEFAULT_VERSION` koristit će se varijabla okruženja. Da biste nadjačali zadanu vrijednost, slijedite korake u sljedećim odjeljcima ovog članka

> [AZURE.NOTE] Ako su domaćini aplikacije u servisu oblaka Azure (weba ili tempiranja uloga), a je prvi put uveli aplikacije, Azure će pokušati koristiti istu verziju Node.js kao što je instaliran na razvojno okruženje ako se podudara s jednim na zadani verzije na Azure.

##<a name="versioning-with-packagejson"></a>Rad s verzijama s package.json

Možete odrediti verziju Node.js koja će se koristiti tako da dodate sljedeće **package.json** datoteku:

    "engines":{"node":version}

Pri čemu je *verzija* broj verzije za korištenje. Možete možete odrediti složenije uvjeta za verziju, kao što su:

    "engines":{"node": "0.6.22 || 0.8.x"}

Budući da 0.6.22 nije jedna verzija koje su dostupne u okruženja za hosting, najveći verziju 0,8 bit će niz koji je dostupan koristit će - 0.8.4.

##<a name="versioning-websites-with-app-settings"></a>Određivanje verzije web-mjesta s postavki aplikacije
Ako su domaćini aplikacije na web-mjestu, možete postaviti varijabla okruženja **WEBSITE_NODE_DEFAULT_VERSION** u željenoj verziji. 

##<a name="versioning-cloud-services-with-powershell"></a>Rad s verzijama servise u Oblaku sa servisom PowerShell

Ako su domaćini računala u Oblaku, a implementirate aplikacije pomoću komponente PowerShell Azure pomoću cmdleta komponente PowerShell **Skup AzureServiceProjectRole** možete nadjačati na zadanu verziju Node.js. Ako, na primjer:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Imajte na umu parametara u naredbu iznad velika i mala slova.  Možete provjeriti ispravnu verziju Node.js odabran je tako da svojstvo **module** u vaša uloga **package.json**.

**Get-AzureServiceProjectRoleRuntime** možete koristiti i da biste dohvatili popis Node.js verzija dostupne za aplikacije hostira kao neki servis u Oblaku.  Uvijek provjerite je li verzija Node.js ovisi o projektu na ovom popisu.

##<a name="using-a-custom-version-with-azure-websites"></a>Korištenje prilagođene verzije s web-mjesta Azure

Dok Azure nudi nekoliko verzija zadane Node.js, preporučujemo vam da biste koristili verziju koja se nudi prema zadanim postavkama. Ako se aplikacija nalazi se kao programa Azure web-mjesto, koje možete izvršiti pomoću datoteke **iisnode.yml** . Sljedeći koraci voditi kroz postupak korištenja prilagođenu verziju Node.Js programa Azure web-mjesta:

1. Stvorite novi direktorij, a zatim stvoriti datoteku **server.js** u direktoriju. Datoteka **server.js** mora sadržavati sljedeće:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Time će se prikazati verziju Node.js koristi prilikom pregledavanja na web-mjestu.

2. Stvaranje novog web-mjesta i zabilježite naziv web-mjesta. Na primjer, sljedeće koristi [Azure alati naredbenog retka] da biste stvorili novi Azure web-mjesto s nazivom **mywebsite**i omogućivanje brojka spremište web-mjesta.

        azure site create mywebsite --git

3. Stvorite novi direktorij pod nazivom **smeće** kao podređeni direktorija **server.js** datotekom.

4. Preuzmite određenu verziju **node.exe** (verzija sustava Windows) koji želite koristiti s aplikacijom. Na primjer, sljedeće koristi **curl** da biste preuzeli verziju 0.8.1:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Spremite datoteku **node.exe** u mapu **smeće** stvoreno.

5. Stvorite datoteku **iisnode.yml** u direktoriju isti kao datoteku **server.js** , a zatim dodajte sljedeći sadržaj datoteke **iisnode.yml** :

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Taj je put gdje datoteku **node.exe** unutar projekta nalazit će se nakon što ste objavili aplikacija na web-mjesto Azure.

6. Objavite svoju aplikaciju. Ako, na primjer, budući da se novo web-mjesto s parametrom – brojka ste ranije stvorili, sljedeće naredbe će dodavanje datoteka aplikacije Moje lokalnom spremištu brojka, i automatske ih u spremište web-mjesta:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Nakon objavljivanja sadrži aplikaciju, otvorite web-mjesta u pregledniku. Trebali biste vidjeti poruku koja govori "Zdravo Azure izvodi čvor verzije: v0.8.1".

##<a name="next-steps"></a>Daljnji koraci

Sad kad znate kako odrediti verziju Node.js koristi aplikacije, Saznajte kako [raditi s module], [stvorite i implementirajte Node.js Web-mjesta]i [upute za korištenje alata za Azure naredbenog retka za Mac i Linux].

Dodatne informacije potražite u [Centru za razvojne inženjere Node.js](/develop/nodejs/).

[Upute za korištenje alata za Azure naredbenog retka za Mac i Linux]: xplat-cli-install.md
[Alati za Azure naredbenog retka]: xplat-cli-install.md
[Rad s moduli]: nodejs-use-node-modules-azure-apps.md
[Stvorite i implementirajte Node.js Web-mjesta]: web-sites-nodejs-develop-deploy-mac.md
