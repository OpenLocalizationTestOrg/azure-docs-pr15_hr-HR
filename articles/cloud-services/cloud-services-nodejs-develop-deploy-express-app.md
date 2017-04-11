<properties 
    pageTitle="Web-aplikaciju pomoću Express (Node.js) | Microsoft Azure" 
    description="Praktični vodič koji stvara na servis vodič oblaka i pokazuje kako koristiti modul Express." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Stvaranje web-aplikacije Node.js pomoću Express na servis u Oblaku programa Azure

Node.js obuhvaća minimalnim funkcionalnost u core runtime.
Razvojni inženjeri često koristite 3 strana module za pružanje dodatne funkcije prilikom razvoja Node.js aplikacije. U ovom ćete praktičnom vodiču će stvoriti novu aplikaciju modul [Express][] , koji pruža programa MVC framework za stvaranje Node.js web-aplikacije.

Snimka zaslona dovršene aplikacija je ispod:

![Web-pregledniku prikažete Dobro došli do Express servisu Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Stvaranje projekta servisa oblaka

Poduzmite sljedeće korake da biste stvorili novi projekt servisa oblaka pod nazivom "expressapp":

1. Na **Izborniku Start** ili **Početnom zaslonu**pretraživanje za **Windows PowerShell**. Na kraju, desnom tipkom miša kliknite **Windows PowerShell** i odaberite **Pokreni kao Administrator**.

    ![Ikona Azure PowerShell](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Promjena imenika na **c:\\čvor** direktorija, a zatim unesite sljedeće naredbe za stvaranje novog rješenja pod nazivom **expressapp** i uloga web naziva **WebRole1**:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] Prema zadanim postavkama, **Dodavanje AzureNodeWebRole** koristi stariju verziju Node.js. Naredba **Skup AzureServiceProjectRole** iznad upućuje Azure da biste koristili v0.10.21 čvora.  Imajte na umu parametre velika i mala slova.  Možete provjeriti ispravnu verziju Node.js odabran je tako da svojstvo **module** u **WebRole1\package.json**.

##<a name="install-express"></a>Instalacija Express

1. Instalirajte Express generator slanjem sljedeću naredbu:

        PS C:\node\expressapp> npm install express-generator -g

    Izlaz iz naredbu npm bi trebala izgledati ovako da bi se rezultat u nastavku. 

    ![Windows PowerShell prikazuje rezultat u npm instalirajte eksplicitnih naredbe.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Promjena direktorija u direktoriju **WebRole1** i upotrijebite naredbu eksplicitnih da biste generirali nove aplikacije:

        PS C:\node\expressapp\WebRole1> express

    Zatražit će se da biste prebrisali starije aplikacije. Unesite **y** ili **da** biste nastavili. Express generirat će app.js datoteku i struktura mapa za aplikacije.

    ![Izlaz iz naredbu express](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Da biste instalirali dodatne zavisnosti definirana u datoteci package.json, unesite sljedeću naredbu:

        PS C:\node\expressapp\WebRole1> npm install

    ![Izlaz iz sustava npm instalacija naredbe](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Koristite sljedeću naredbu da biste kopirali datoteke **smeće / "www"** **server.js**. To je da bi servisa u oblaku mogli pronaći točku unosa za ovu aplikaciju.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Po završetku ta naredba imat ćete **server.js** datoteke u direktoriju WebRole1.

7.  Izmjena **server.js** da biste uklonili jednu od na '. "znakova iz sljedeći redak.

        var app = require('../app');

    Nakon toga Ova izmjena redak prikazivati na sljedeći način.

        var app = require('./app');

    Ta promjena potreban je jer smo premjestili datoteku (bivši **smeće / "www"**) isti direktorij kao datoteke aplikacije pojavljivanje upita. Nakon toga, spremite datoteku **server.js** .

8.  Da biste pokrenuli aplikaciju u Azure emulator, koristite sljedeću naredbu:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Na web-stranicu koja sadrži Dobro došli u express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Izmjena prikaza

Sada Izmijeni prikaz da biste prikazali poruku "Dobro došli u Express u Azure".

1.  Unesite sljedeću naredbu da biste otvorili datoteku index.jade:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![Sadržaj datoteke index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade je modul zadani prikaz koriste Express aplikacije. Dodatne informacije o modul Jade prikaza potražite u članku [http://jade-lang.com][].

2.  Izmjena zadnji redak teksta dodavanjem **Azure**.

    ![Datoteka index.jade posljednjeg retka čita: p Dobro došli u \#{naslov} u Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Datoteku spremite i zatvorite Blok za pisanje.

4.  Osvježite preglednik i vidjet ćete promjene.

    ![U prozoru preglednika stranica sadrži Dobro došli u Express servisu Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Nakon testiranje aplikacije pomoću cmdleta **Zaustavi AzureEmulator** Zaustavi na emulator.

##<a name="publishing-the-application-to-azure"></a>Objavljivanje aplikacija za Azure

U prozoru Azure PowerShell pomoću cmdleta **Objavi AzureServiceProject** za implementaciju aplikacije na servis u oblaku

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Nakon dovršetka postupka implementacije pregledniku će otvorili i prikazali web-stranicu.

![Web-pregledniku koji se prikazuje stranici Express. URL označava sada hostirane na Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere Node.js](/develop/nodejs/).

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://jade-lang.com]: http://jade-lang.com

 
