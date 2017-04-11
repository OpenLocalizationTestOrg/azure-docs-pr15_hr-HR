<properties
    pageTitle="Vodič za početak Node.js rada | Microsoft Azure"
    description="Saznajte kako stvoriti jednostavan Node.js web-aplikacije i implementaciju sa servisom Azure oblaka."
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
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Pomoću ovog praktičnog vodiča prikazuje kako stvoriti jednostavan Node.js aplikacija radi u Oblaku programa Azure. Servisi u oblaku su sastavni blokovi skalabilni oblaka aplikacije u Azure. Oni omogućuju odvojenosti i neovisno upravljanje i skaliranje odgovaranjem sučelja i stražnje komponenti aplikacije.  Servisi u oblaku omogućavaju pouzdano hosting ulogama robusne namjenski virtualnog računala.

Dodatne informacije o servise u Oblaku i po čemu se razlikuje od Azure web-mjesta i virtualnim strojevima, potražite u članku [Azure web-mjesta, servisa u Oblaku i virtualnim strojevima usporedbe].

>[AZURE.TIP] Želite li stvaranje jednostavne web-mjesto? Ako vaš scenarij obuhvaća samo jednostavne web-mjesto sučelja, razmislite o [korištenju laganih web-aplikacijama]. Možete jednostavno nadograditi na servis u Oblaku kao što je web-aplikaciju programa rastom i promijenite svojim potrebama.

Slijedeći ovog praktičnog vodiča će izrađivati jednostavne web-aplikacije nalazi unutar ulogu web. Koje će pomoću emulator računalnim da biste testirali lokalno aplikacije, a zatim uvesti pomoću alata za PowerShell naredbenog retka.

Aplikacija je jednostavno "Pozdrav svijeta" aplikacija:

![Prikaz web-stranicu pozdrav svijeta web-pregledniku][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Preduvjeti

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi Azure PowerShell koji zahtijeva Windows.

- Instaliranje i konfiguriranje [Azure Powershell].
- Preuzmite i instalirajte [Azure SDK za .NET 2.7]. Postavljanje instalacije odaberite:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Stvaranje projekta programa Azure u Oblaku

Izvođenje sljedećih zadataka da biste stvorili novi projekt Azure u Oblaku, zajedno s osnovni Node.js scaffolding:

1. Pokrenite **Windows PowerShell** kao Administrator; na **Izborniku Start** ili **Početnom zaslonu**pretraživanje za **Windows PowerShell**.

2. [Povezivanje ljuske PowerShell] za svoju pretplatu.

3. Unesite sljedeći cmdlet ljuske PowerShell za stvaranje da biste stvorili projekta:

        New-AzureServiceProject helloworld

    ![Rezultat naredbe nova AzureService helloworld][The result of the New-AzureService helloworld command]

    Cmdlet **Novo AzureServiceProject** generira osnovna struktura za objavljivanje Node.js aplikacije na servis u Oblaku. Sadrži konfiguracijske datoteke potrebne za objavljivanje Azure. Cmdlet mijenja se i radni direktorij direktorij servisa.

    Cmdlet stvara sljedeće datoteke:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** i **ServiceDefinition.csdef**: Azure specifične datoteke potrebne za svoju aplikaciju za objavljivanje. Dodatne informacije potražite u članku [Pregled stvaranja hostira služba za Azure].

    -   **deploymentSettings.json**: pohranjuje lokalne postavke koje se koriste Azure PowerShell cmdleti za implementaciju.

4.  Unesite sljedeću naredbu da biste dodali novo web radno mjesto:

        Add-AzureNodeWebRole

    ![Izlaz iz naredbu Dodaj AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

    Cmdlet za **Dodavanje AzureNodeWebRole** stvara osnovne Node.js aplikacije. Mijenja i **.csfg** i **.csdef** datoteke da biste dodali unose konfiguracije nove uloge.

    > [AZURE.NOTE] Ako ne odredite naziv uloge, koristi se zadani naziv. Naziv možete unijeti kao prvi parametar cmdlet:`Add-AzureNodeWebRole MyRole`

Aplikaciju Node.js definirana je u u datoteku **server.js**, koja se nalazi u direktoriju uloge web (**WebRole1** po zadanom). Evo kod:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Kod je zapravo isti kao uzorak "Pozdrav svijetu" na web-stranici [nodejs.org] osim koristi broj priključka koji je dodijelio okruženje oblaka.

## <a name="deploy-the-application-to-azure"></a>Implementacija aplikacije Azure

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Preuzimanje Azure postavke objavljivanja

Da biste implementirali aplikaciju Azure, prvo morate preuzeti postavke objavljivanja Azure pretplatu.

1.  Pokrenite sljedeći cmdlet Azure PowerShell:

        Get-AzurePublishSettingsFile

    To će pomoću preglednika dođite do stranice za preuzimanje postavke objavljivanja. Možda zatražiti da biste se prijavili pomoću Microsoftova Account. Ako je tako, koristite račun povezan s pretplatom Azure.

    Spremiti preuzete profil možete jednostavno pristupiti mjesto datoteke.

2.  Pokrenite sljedeći cmdlet da biste uvezli objavljivanje profila koji ste preuzeli:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Nakon uvoza postavke objavljivanja, razmislite o brisanju datoteke preuzete .publishSettings jer sadrži podatke koje nije moguće dozvolite za pristup računu.

### <a name="publish-the-application"></a>Objavljivanje aplikacija

Da biste objavili, pokrenite sljedeće naredbe:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-Naziv servisa** određuje naziv za implementaciju. To mora biti jedinstven naziv, u suprotnom neće uspjeti postupak objavljivanja. Naredba za **Dohvaćanje datuma** tacks na niz datuma/vremena koje treba Jedinstvenost naziva.

- **– Mjesto** određuje s podatkovnim centrom aplikacija će se nalaziti u. Da biste vidjeli popis dostupnih podatkovnim centrima, koristite cmdlet **Get-AzureLocation** .

- **– Pokretanje** otvara prozor preglednika i vodi na glavnom računalu usluge po dovršetku implementacije.

Nakon uspješnog objavljivanje prikazat će se odgovor sličnu ovoj:

![Izlaz iz naredba Objavi AzureService][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Može potrajati nekoliko minuta za aplikaciju i postaju dostupne kada prvi put objavljena.

Kada implementacijskih završi, prozoru preglednika će otvoriti i idite na servis u oblaku.

![Prikaz stranice svijeta pozdrav; prozoru preglednika URL označava stranici nalazi na Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Azure sada radi aplikacije.

Cmdlet **Objavi AzureServiceProject** izvršava sljedeće korake:

1.  Stvaranje paketa za implementaciju. Paket sadrži sve datoteke u mapi aplikacije.

2.  Stvara novi **račun za pohranu** ako nešto ne postoji. Račun za Azure prostora za pohranu koristi se za pohranu Aplikacijski paket tijekom implementacije. Sigurno možete izbrisati račun za pohranu po završetku implementacije.

3.  Stvara novi **servis u oblaku** ako nešto još ne postoji. **Servis u oblaku** je spremnik u kojem se aplikacija hostira kada je implementiran u Azure. Dodatne informacije potražite u članku [Pregled stvaranja hostira služba za Azure].

4.  Da biste Azure objavljuje paketa za implementaciju.


## <a name="stopping-and-deleting-your-application"></a>Zaustavljanje i brisanje aplikacije

Nakon implementacija aplikacije, preporučujemo vam da biste onemogućili da izbjegavanje dodatnih troškova. Azure računi web-uloga instance po satu potrošena vrijeme na poslužitelju. Vrijeme na poslužitelju potrošnje kada aplikacija je implementiran, čak i ako instance nije pokrenut i u prestao stanju.

1.  U prozoru komponente Windows PowerShell Zaustavi uvođenje servisa stvorili u prethodnom odjeljku s sljedeći cmdlet:

        Stop-AzureService

    Zaustavljanje servisa može potrajati nekoliko minuta. Kada servis je zaustavljen, dobit ćete poruku da je on prestao.

    ![Status naredba Zaustavi AzureService][The status of the Stop-AzureService command]

2.  Da biste izbrisali servisa, nazovite sljedeći cmdlet:

        Remove-AzureService

    Kada se to od vas zatraži, unesite **Y** da biste izbrisali servis.

    Brisanje servis može potrajati nekoliko minuta. Nakon brisanja servis primit ćete poruku izbrisan je servis.

    ![Status naredbe Ukloni AzureService][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Brisanje servis izbrisati račun za pohranu stvoren prilikom servis objavljen na početku pa će i dalje se naplatiti za pohranu koji se koristi. Ako ništa drugo koristi prostora za pohranu, preporučujemo vam da biste je izbrisali.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere Node.js].

<!-- URL List -->

[Usporedba web-mjesta, servisa u Oblaku i virtualnim strojevima Azure]: ../app-service-web/choose-web-site-cloud-service-vm.md
[Korištenje laganih web-aplikacijama]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[Azure SDK za .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Povezivanje PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Pregled stvaranja servis za Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Razvojni centar za Node.js]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
