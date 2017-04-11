<properties 
    pageTitle="Stvaranje i implementacija web-aplikacijama Node.js Azure pomoću WebMatrix" 
    description="Praktični vodič ručica kako koristiti WebMatrix za razvoj aplikacija za Node.js i njegova implementacija Azure aplikacije servisa web-aplikacije." 
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


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Stvaranje i implementacija web-aplikacijama Node.js Azure pomoću WebMatrix

Pomoću ovog praktičnog vodiča pokazuje kako koristiti WebMatrix za razvoj aplikacija za Node.js i njegova implementacija [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714) web-aplikacije. WebMatrix je alat za razvoj besplatne web od Microsofta koji uključuje sve što vam je potrebno za razvoj aplikacija za web-mjesta ili web. WebMatrix sadrži nekoliko značajki koje se jednostavno je za korištenje Node.js manje uključujući dovršetka kod, gotove predloške i uređivač podrška za Jade, i CoffeeScript. Dodatne informacije o [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Nakon dovršetka ovaj vodič, imat ćete web-aplikacijama Node.js izvodi u aplikacije servisa za Azure.
 
Snimka zaslona dovršene aplikacija je ispod:

![Azure čvor Web-mjesta][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="sign-into-azure"></a>Prijavite se u Azure

Slijedite ove korake da biste stvorili web-aplikacijama u aplikacije servisa za Azure.

1. Pokretanje WebMatrix
2. Ako je ovo prvi put koristite WebMatrix, od vas će se zatražiti da biste se prijavili u Azure.  U suprotnom, kliknite gumb **Prijava** i odaberite **Dodaj račun**.  Odaberite **prijavite se** pomoću Microsoftova Account.

    ![Dodavanje računa][addaccount]

3. Ako ste se registrirali za račun za Azure, morate prijaviti pomoću Microsoftova Account:

    ![Prijavite se u Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Stvaranje web-mjesta pomoću predloška za ugrađeni za Azure

1. Na početnom zaslonu pritisnite gumb **Novo** pa odaberite **Galeriju predložaka** za stvaranje novog web-mjesta u galeriji predložaka:

    ![Nova web-mjesta iz galerije predložaka][sitefromtemplate]

2. U dijaloškom okviru **web-mjesta iz predloška** odaberite **čvor** , a zatim odaberite **Express web-mjesta**. Na kraju, kliknite **Dalje**. Ako nedostaje neki preduvjeti za predložak **Express web-mjesta** , zatražit će se da biste ih instalirali.

    ![Odaberite predložak express][webmatrix-templates]

3. Ako ste prijavljeni na Azure, sada imate mogućnost stvaranja web-aplikacije programa aplikacije servisa za lokalnog web-mjesta.  Odaberite jedinstveni naziv, a zatim odaberite podatkovnog centra u kojoj želite web-aplikaciju programa aplikacije servisa za stvaranje: 

    ![Stvaranje web-mjesta na Azure][nodesitefromtemplateazure]
    
4. Kada WebMatrix Završi Izrada lokalnog web-mjesta i stvaranju aplikacije servisa za web-lokacije, prikazuje se WebMatrix IDE.

    ![webmatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Objavljivanje aplikacija za Azure

1. U WebMatrix, kliknite **Objavi** na vrpci **Home** da biste prikazali dijaloški okvir **Pretpregled za objavljivanje** web-mjesta.

    ![Objavljivanje pretpregled][webmatrix-node-publishpreview]

2. Kliknite **Nastavi**. Po dovršetku objavljivanje URL za web-aplikaciji aplikacije servisa za se prikazuje pri dnu WebMatrix IDE

    ![Objavljivanje dovršeno][webmatrix-publish-complete]

3. Kliknite vezu da biste otvorili aplikacije servisa za web-aplikaciju u pregledniku.

    ![Express web-aplikacije][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Izmijenite i ponovno objavite aplikaciju

Možete jednostavno izmijeniti i ponovno objavite aplikaciju. Ovdje će postati jednostavno promijeniti naslov u u datoteci **index.jade** i ponovno objaviti u aplikaciji.

1. U WebMatrix, odaberite **datoteka**, a zatim proširite mapu **prikaza** . Otvorite datoteku **index.jade** dvoklikom.

    ![index.jade webmatrix prikaz][webmatrix-modify-index]

2. Promjena retka odlomka na sljedeće:

        p Welcome to #{title} with WebMatrix on Azure!

3. Spremili promjene, a zatim kliknite ikonu Objavi. Na kraju, kliknite **Nastavi** u dijaloškom okviru **Pretpregled za objavljivanje** i pričekajte da ažuriranja za objavljivanje.

    ![Objavljivanje pretpregled][webmatrix-republish]

4. Kada objavljivanje dovrši, koristite vezu vraća nakon dovršetka postupka Objavi da biste vidjeli ažurirane aplikacije servisa za web-aplikaciju.

    ![Azure čvor web-aplikacije][webmatrix-node-completed]

##<a name="next-steps"></a>Daljnji koraci

Da biste saznali više o verzijama Node.js koji se isporučuju sa Azure i određivanje verzija koja će se koristiti s aplikacijom, potražite u članku [Određivanje verzije Node.js u aplikaciji za Azure](../nodejs-specify-node-version-azure-apps.md).

Ako naiđete na probleme s aplikacijom nakon uveden Azure, potražite u članku [upute za ispravljanje pogrešaka u web-aplikacijama Node.js u aplikacije servisa za Azure](web-sites-nodejs-debug.md) informacije o dijagnosticiranje problema.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 