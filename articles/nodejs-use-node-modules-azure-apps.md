<properties
    pageTitle="Rad s Node.js moduli"
    description="Saznajte kako raditi s Node.js moduli prilikom korištenja aplikacije servisa za Azure i servise u Oblaku."
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


# <a name="using-nodejs-modules-with-azure-applications"></a>Pomoću Node.js modula Azure aplikacije

Ovaj dokument sadrži smjernice o korištenju module Node.js s aplikacijama na Azure. Sadrži smjernice jamči da aplikacija koristi određenu verziju modula kao i pomoću nativnog modula Azure.

Ako ste već upoznati s pomoću modula Node.js **package.json** i **npm shrinkwrap.json** datoteke, sljedeće je brzi sažetak što se spominju u ovom članku:

* Aplikacije servisa za Azure razumije **package.json** i **npm shrinkwrap.json** datoteke, a možete instalirati moduli ovisno o stavkama u te datoteke.
* Azure servise u Oblaku očekivati sve module za instalaciju na razvojno okruženje i **čvor\_modula** direktorija biti dio paketa za implementaciju. Moguće je da biste omogućili podršku za instaliranje modula pomoću **package.json** ili **npm shrinkwrap.json** datoteka na servise u Oblaku, no potreban prilagodbe skripti zadanom koristi projekata u Oblaku. Primjer kako biste to postigli, potražite u članku [pokretanje npm Instaliraj da biste izbjegli implementacija čvor modula Azure pokretanje zadatka](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azure virtualnim strojevima se spominju u ovom se članku kao okruženje za implementaciju u na VM će se ovisno o operacijskom sustavu koji hostira tvrtka virtualnog računala.

##<a name="nodejs-modules"></a>Moduli Node.js

Moduli se može učitati JavaScript paketa koji sadrže određene funkcije za svoju aplikaciju. Modul obično se instalira alatom **npm** naredbenog retka, ali neke (kao što su modul http) su navedene kao dio paketa Node.js core.

Nakon instalacije moduli se pohranjuju u na **čvor\_modula** imeničkog u korijenski direktorij strukturu aplikacije. Svakom modulu unutar na **čvor\_modula** direktorija održava vlastitu **čvor\_modula** direktorij koji sadrži module ovisi o, a to ponovno ponavlja za svaku modul usidrili dolje lanac ovisnosti. Time se omogućuje svakom modulu instalirati vlastitu preduvjeta verzije za module ovisi o, no može se dogoditi vrlo velike direktorija strukture.

Kada implementacija u **čvor\_modula** directory kao dio aplikacije je povećava veličinu implementacije u usporedbi s pomoću datoteke **package.json** ili **npm shrinkwrap.json** ; ga pak jamči da su verziju modula koji se koriste u radni jednaki onima u razvoju.

###<a name="native-modules"></a>Izvorni moduli

Dok većina moduli su samo običan tekst JavaScript datoteke, neke moduli su ovisne binarne slike. Ove moduli su prevedene Instaliraj trenutku, obično pomoću Python i čvor gyp. Budući da se oslanjate Azure servise u Oblaku na **čvor\_modula** mapu uvodi se kao dio aplikacije, sve nativni modul uključiti kao dio instalirani moduli raditi u oblaku kao instaliran i prevesti u sustavu Windows razvoj.

Aplikacije servisa za Azure ne podržava sve nativni module i možda neće uspjeti pri Kompiliranje s vrlo konkretnih preduvjeti. Dok neke popularne moduli kao što su MongoDB imati neobavezno nativni zavisnosti i rad samo precizno bez njih, dva zaobilazna rješenja proved uspješno s gotovo sve nativni module danas dostupne:

* Pokrenite **npm instalirati** na Windows računalu koje ima instaliran preduvjeti za sve nativni modul na. Nakon toga implementacija stvara **čvor\_modula** mapu kao dio aplikacije da biste aplikacije servisa za Azure.
* Aplikacije servisa za Azure moguće je konfigurirati za izvršavanje prilagođene tulumu ili skripti ljuske tijekom uvođenja, daje pokretanja prilike možete izvršiti prilagođene naredbe i točno konfigurirati način **npm instalirati** . Videozapis koji se prikazuje kako to učiniti, potražite u članku [Prilagođene skripte za implementaciju web-mjesta s Kudu].

###<a name="using-a-packagejson-file"></a>Pomoću datoteke package.json

Datoteka **package.json** je način za određivanje ovisnosti najviše razine aplikacije potreban je tako da se hostinga platformu možete instalirati ovisnosti umjesto potrebno da biste uvrstili u **čvor\_paketa** mapu kao dio uvođenje. Kada je postavila aplikacije naredba **npm instalacija** koristi se analizirati datoteke **package.json** i njihovu instalaciju svih ovisnosti na popisu.

Tijekom razvoju, možete koristiti u **– Spremanje**, **– Spremanje razvojni**, ili **– Spremi neobavezno** parametre prilikom instalacije module da biste dodali stavku za modul **package.json** datoteku automatski. Dodatne informacije potražite u članku [npm instalacija](https://docs.npmjs.com/cli/install).

Mogući problem s datotekom **package.json** je da ga samo određuje verziju ovisnost najviše razine. Svakom modulu instaliran možda ili ne navedete verziju modula ovisi o i tako da je moguće da ste možda božićnim lancem različite ovisnosti od jednog koristi u razvoju.

> [AZURE.NOTE]
> Kada implementacija aplikacije servisa Azure, ako se odnosi na datoteci <b>package.json</b> modula nativni vidjet ćete slično sljedećem pogreške pri objavljivanju aplikacije pomoću brojka:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Pomoću datoteke npm shrinkwrap.json

Datoteka **npm shrinkwrap.json** je pokušaj adresa ograničenja verzija modula **package.json** datoteke. Dok **package.json** datoteka sadrži samo verzije za module najviše razine, **npm shrinkwrap.json** datoteka sadrži preduvjeta verzije za ovisnost lanac cijelog modul.

Kada aplikacija bude spremna za proizvodnju, možete zaključati-dolje preduvjeta verzije i stvoriti datoteku **npm shrinkwrap.json** pomoću naredbe **npm shrinkwrap** . Korištenje verzija instalirana u na **čvor\_modula** mape i snimite te **npm shrinkwrap.json** datoteku. Kada aplikacija implementiran okruženja za hosting, naredba **npm instalacija** koristi se analizirati datoteke **npm shrinkwrap.json** i njihovu instalaciju svih ovisnosti na popisu. Dodatne informacije potražite u članku [npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [AZURE.NOTE]
>Kada implementacija aplikacije servisa Azure, ako se odnosi na datoteci <b>npm shrinkwrap.json</b> modula nativni vidjet ćete slično sljedećem pogreške pri objavljivanju aplikacije pomoću brojka:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Daljnji koraci

Sad kad znate kako pomoću Node.js modula Azure, Saznajte kako [odrediti Node.js verziju], [Stvaranje i implementaciju web-aplikacijama Node.js]i [kako koristiti Azure sučelja naredbenog retka za Mac i Linux].

Dodatne informacije potražite u [Centru za razvojne inženjere Node.js](/develop/nodejs/).

[Određivanje verzije Node.js]: nodejs-specify-node-version-azure-apps.md
[Kako koristiti Azure sučelja naredbenog retka za Mac i Linux]: xplat-cli-install.md
[Stvaranje i implementaciju web-aplikacijama Node.js]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Skripte za implementaciju prilagođene web-mjesta s Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
