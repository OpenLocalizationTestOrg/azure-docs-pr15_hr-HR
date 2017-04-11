<properties 
    pageTitle="Node.js aplikacije pomoću Socket.io | Microsoft Azure" 
    description="Saznajte kako koristiti socket.io u aplikaciji node.js hostirane na Azure." 
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

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Sastavljanje Node.js razgovor aplikacijom Socket.IO na Azure Oblaku

Socket.IO omogućuje komunikaciju Realno vrijeme između između node.js poslužitelj i klijenti. Pomoću ovog praktičnog vodiča će vas voditi kroz hosting krajnju točku veze. IO temelji aplikacije za razgovor na Azure. Dodatne informacije o Socket.IO potražite u članku <http://socket.io/>.

Snimka zaslona dovršene aplikacija je ispod:

![Prikaz servisa hostirane na Azure prozoru preglednika][completed-app]  

## <a name="prerequisites"></a>Preduvjeti

Provjerite da su sljedeće proizvode i verzije instaliran za uspješan dovršetak primjer u ovom članku:

* Instalirajte [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* Instalacija [Node.js](https://nodejs.org/download/)
* Instalacija [Python verzija 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Stvaranje projekta servisa oblaka

Sljedeći koraci stvaranje projekta servisa oblaka koji će biti smješteno Socket.IO aplikacije.

1. Na **Izborniku Start** ili **Početnom zaslonu**pretraživanje za **Windows PowerShell**. Na kraju, desnom tipkom miša kliknite **Windows PowerShell** i odaberite **Pokreni kao Administrator**.

    ![Ikona Azure PowerShell][powershell-menu]

2. Stvaranje direktorij pod nazivom **c:\\čvor**. 
 
        PS C:\> md node

3. Promjena imenika na **c:\\čvor** direktorija
 
        PS C:\> cd node

4. Unesite sljedeće naredbe za stvaranje novog rješenja pod nazivom **chatapp** i ulogu suradnika pod nazivom **WorkerRole1**:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    Prikazat će se sljedeće odgovor:

    ![Izlaz iz novi azureservice i dodavanje azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Preuzimanje primjer razgovor

Za taj projekt koristit ćemo primjeru razgovor [Socket.IO GitHub spremište]. Izvršite sljedeće korake da biste preuzeli primjeru i dodavanje projekt koji ste prethodno stvorili.

1.  Stvaranje lokalne kopije spremište pomoću gumba **Kloniraj** . Da biste preuzeli projekt možda koristi i gumb **poštanski broj** .

    ![Prikaz https://github.com/LearnBoost/socket.io/tree/master/examples/chat, s istaknutom ikonom preuzimanje ZIP prozoru preglednika][chat-example-view]

3.  Kretanje po strukturi directory u lokalnom spremištu dok ne dođete na na **primjeri\\razgovor** direktorija. Kopiranje sadržaja taj imenik da biste na **C:\\čvor\\chatapp\\WorkerRole1** direktorija ste ranije stvorili.

    ![Explorer prikazuje sadržaj Primjeri\\razgovor direktorija dobivenih iz arhive][chat-contents]

    Istaknute stavke u snimku zaslona koja se nalazi iznad su datoteke kopirana na **primjeri\\razgovor** direktorija

4.  U na **C:\\čvor\\chatapp\\WorkerRole1** direktorija, izbrišite **server.js** datoteku, a zatim preimenujte datoteku **app.js** **server.js**. Time se uklanjaju na zadani **server.js** datoteku stvorenu u prethodno cmdlet za **Dodavanje AzureNodeWorkerRole** , a zamjenjuje datoteke aplikacije iz primjera razgovor.

### <a name="modify-serverjs-and-install-modules"></a>Izmjena Server.js i instalirali module

Prije testirati aplikacije Azure emulator dajemo neke manji izmjene. Izvršite sljedeće korake da biste server.js datoteku:

1.  Otvorite datoteku **server.js** u Visual Studio ili bilo kojem uređivaču teksta.

2.  Pronađite odjeljak **Modul ovisnosti** na početku server.js i promijenite u retku koji sadrži **sio = require('.. //.. lib//Socket.IO')** za **sio = require('socket.io')** kao što je prikazano u nastavku:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Da biste osigurali aplikacije očekuje podatke odgovarajući priključak, otvorite server.js u blok za pisanje ili uređivač omiljene, a zatim promijenite sljedeći redak jer zamjenjuju **3000** **process.env.port** kao što je prikazano u nastavku:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Kada spremite promjene da biste **server.js**, poduzmite sljedeće korake da biste instalirali module potrebne, a zatim testirajte aplikacije u Azure emulator:

1.  Pomoću **Komponente PowerShell Azure**promijeniti imenika na **C:\\čvor\\chatapp\\WorkerRole1** direktorija i koristite sljedeću naredbu da biste instalirali module potrebnih ovu aplikaciju:

        PS C:\node\chatapp\WorkerRole1> npm install

    To se može instalirati moduli navedenih u datoteci package.json. Po završetku naredbu trebali biste vidjeti izlaz sličnu ovoj:

    ![Izlaz iz sustava npm instalacija naredbe][The-output-of-the-npm-install-command]

4.  Budući da u ovom se primjeru je izvorno dio spremište Socket.IO GitHub i Socket.IO biblioteke izravno pozivaju relativni put, Socket.IO je nije navedena u datoteci package.json pa ćemo morate instalirati slanjem sljedeću naredbu:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testirajte i implementacija

1.  Pokretanje sustava emulator slanjem sljedeću naredbu:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Otvorite preglednik i idite na **http://127.0.0.1**.

3.  Kad se otvori prozor preglednika, unesite je Nadimak, a zatim unesite uspješnosti.
    Time ćete sve želite slati poruke kao određene nadimaka. Da biste testirali funkcionalnost više korisnika, otvorite dodatne prozore preglednika pomoću iste URL i unesite drugu nadimke.

    ![Prikaz razgovora poruke Korisnik1 i korisnika2 dva prozora preglednika](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Nakon testiranje aplikacije Zaustavi na emulator slanjem sljedeću naredbu:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Za implementaciju aplikacije Azure pomoću cmdleta **Objavi AzureServiceProject** . Ako, na primjer:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Obavezno upišite jedinstveni naziv, u suprotnom neće uspjeti postupak objavljivanja. Po dovršetku implementaciju web-pregledniku će otvoriti i idite na servis distribuiranih.
    > 
    > Ako se pojavi pogreška koja govori da naziv navedeni pretplate ne postoji u profilu uvezene Objavi, morate preuzeti i uvoz objavljivanje profila za vašu pretplatu prije no što implementirate Azure. U odjeljku **Implementacija aplikacije Azure** [Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

    ![Prikaz servisa hostirane na Azure prozoru preglednika][completed-app]

    > [AZURE.NOTE] Ako se pojavi pogreška koja govori da naziv navedeni pretplate ne postoji u profilu uvezene Objavi, morate preuzeti i uvoz objavljivanje profila za vašu pretplatu prije no što implementirate Azure. U odjeljku **Implementacija aplikacije Azure** [Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

Aplikacija je sada radi Azure i možete prenijeli poruku razgovora između različitih klijente pomoću Socket.IO.

> [AZURE.NOTE] Zbog jednostavnosti, ovaj uzorak ograničeno je na razgovarati između korisnika koji su povezani s istoj instanci. To znači da ako servis u oblaku stvara dvije instance ulogu suradnika, korisnici će samo moći razgovarati s drugima povezani u istoj instanci ulogu suradnika. Da biste skalirali aplikacije za rad s više instanci uloga nije tehnologije kao što su Bus servisa zajedničkog korištenja trgovine stanje Socket.IO kroz sve instance. Primjeri, potražite u članku korištenje uzoraka servisa Bus redovima i teme u [Azure SDK Node.js GitHub spremište](https://github.com/WindowsAzure/azure-sdk-for-node).

##<a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako stvoriti aplikaciju osnovni razgovor smješten u Oblaku programa Azure. Da biste saznali kako glavnog računala te aplikacije na web-mjestu servisa Azure, potražite u članku [Stvaranje aplikacija za razgovor za Node.js s Socket.IO na programa Azure Web-mjestu][chatwebsite].

Dodatne informacije potražite u odjeljku [Razvojni centar za Node.js](/develop/nodejs/).

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Spremište Socket.IO GitHub]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
