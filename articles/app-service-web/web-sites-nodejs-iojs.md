<properties 
    pageTitle="Kako koristiti io.js u Azure aplikacije servisa Web Apps" 
    description="Saznajte kako pomoću web-aplikacijama u aplikacije servisa za Azure io.js." 
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
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Kako koristiti io.js u Azure aplikacije servisa Web Apps

Popularne o ogranku čvor [io.js] značajke različite razlike projekt Node.js Joyent korisnika, uključujući više otvorenih modela upravljanja, brže ciklusa izdanje i brže Usvajanje nove i eksperimentalne JavaScript značajke.

Dok [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714) web-aplikacije ima mnogo Node.js verzija predinstaliran, omogućuje i pod uvjetom korisnika binarni Node.js. U ovom se članku govori o Omogućivanje korištenja io.js servisa Web aplikacija na dva načina: korištenje prošireni implementacijsku skriptu, koji se automatski konfigurira Azure koristiti najnoviju verziju dostupne io.js, kao i ručno prijenos u binarni io.js. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Korištenje Implementacijsku skriptu

Nakon implementacije Node.js aplikacije, web-aplikacije servisa za aplikaciju pokreće broj small naredbe da biste bili sigurni da okruženje ispravno konfigurirano. Pomoću implementacijsku skriptu, taj postupak može se prilagoditi radi preuzimanja i konfiguraciji io.js.

[Io.js Implementacijsku skriptu](https://github.com/felixrieseberg/iojs-azure) dostupna je na GitHub. Da biste omogućili io.js na web-aplikaciju programa, jednostavno kopiranje **.deployment**, **deploy.cmd** i **IISNode.yml** u korijensku mapu aplikacije i implementacija na web-aplikacije.  

Prvu datoteku **.deployment**upućuje web-aplikacije i pokretanje **deploy.cmd** nakon implementacije. Ova skripta pokreće uobičajeni korake za aplikaciju Node.js, ali se preuzimaju i najnoviju verziju io.js. Na kraju, **IISNode.yml** konfigurira web-aplikacije koristite samo na preuzete io.js binarni umjesto instalirana Node.js binarni.

> [AZURE.NOTE] Da biste ažurirali binarni korištenih io.js samo implementirati aplikacija – skripta će preuzeti novu verziju io.js svakog jedan je implementiran aplikacije.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Ručna instalacija pomoću

Ručna instalacija verzije prilagođenih io.js sadrži samo dva koraka. Najprije preuzmite na **win x64** binarni izravno iz [io.js distribuciju]. Potrebne su dvije datoteke - **iojs.exe** i **iojs.lib**. Spremite obje datoteke u mapu unutar web-aplikaciju programa, primjerice u **smeće/iojs**.

Da biste konfigurirali web-aplikacije koristite **iojs.exe** umjesto unaprijed instalirana verzija čvor, stvoriti datoteku **IISNode.yml** u korijenu aplikacije i dodajte sljedeći redak.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Daljnji koraci

U ovom članku naučili kako koristiti io.js u aplikaciju servisa Web Apps, pomoću oba navedeni implementacije skripte i kao Ručna instalacija. 

> [AZURE.NOTE] IO.js u podebljanom razvoj i ažurirati češće od Node.js. Broj Node.js moduli možda neće funkcionirati s io.js - ponovno Savjetujte [io.js na GitHub] za otklanjanje poteškoća.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

[IO.js]: https://iojs.org
[IO.js distribuciju.]: https://iojs.org/dist/
[IO.js na GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 