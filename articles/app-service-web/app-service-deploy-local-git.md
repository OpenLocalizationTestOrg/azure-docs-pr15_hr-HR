<properties
    pageTitle="Lokalni brojka implementacije aplikacije servisa za Azure"
    description="Saznajte kako omogućiti lokalne implementacije brojka aplikacije servisa za Azure."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Lokalni brojka implementacije aplikacije servisa za Azure

Pomoću ovog praktičnog vodiča pokazuje kako implementirati aplikaciju [Aplikacije servisa za Azure] iz spremišta brojka na lokalnom računalu. Aplikacije servisa za podržava takvog s mogućnošću **Brojka lokalnu** implementaciju [Azure Portal].  
Mnoge naredbe brojka opisane u ovom članku izvršavaju automatski prilikom stvaranja aplikacije servisa za aplikacije pomoću [sučelja Azure naredbenog retka] kao što je opisano [u nastavku](app-service-web-get-started.md).

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, potrebno je:

- Brojka. Možete preuzeti s instalacijom binarni [ovdje](http://www.git-scm.com/downloads).  
- Osnovno znanje brojka.
- Račun za Microsoft Azure. Ako nemate račun, možete ga [Registrirajte se za besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial) ili [aktivirali svoje prednosti pretplatnika Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvorite short-lived starter aplikaciju u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.  

## <a name="Step1"></a>Korak 1: Stvaranje lokalnom spremištu

Izvođenje sljedećih zadataka da biste stvorili novi brojka spremište.

1. Pokrenite alat naredbenog retka, kao što su **GitBash** (Windows) ili **tulum** (Unix ljuske). U sustavima OS X možete pristupiti naredbenog retka pomoću aplikacije **Terminal** .

2. Idite do gdje bi se smjestiti sadržaja za implementaciju.

3. Pokretanje nove brojka spremište, koristite sljedeću naredbu:

        git init

## <a name="Step2"></a>Korak 2: Izvršavanje sadržaja

Aplikacije servisa za podržava aplikacije koje su stvorene u mnogim programskim jezicima. 

1. Ako vaš spremište već sadrži sadržaja Preskoči to pokažite i premjestite pokazivač 2 ispod. Ako vaš spremište već sadrži sadržaj jednostavno popuniti statične .html datoteke na sljedeći način: 

    - Korištenje uređivaču teksta, stvorite novu datoteku pod nazivom **index.html** korijenu brojka spremište
    - Dodajte sljedeći tekst kao sadržaj na index.html datoteku i spremite ga: *Pozdrav brojka!*
        
2. Iz naredbenog retka, provjerite jesu li vam u korijenskom direktoriju vaše brojka spremište. Da biste dodali datoteke u vašem spremište koristiti sljedeću naredbu:

        git add -A 

4. Nakon toga Zapiši promjene u spremište pomoću sljedeće naredbe:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Korak 3: Omogućivanje aplikacije servisa za spremište aplikacije

Izvršite sljedeće korake da biste omogućili brojka spremište aplikacije servisa za aplikacije.

1. Prijavite se na [Portal za Azure].

2. U plohu aplikacije servisa za aplikaciju, kliknite **Postavke > izvor implementaciju**. Kliknite **Odabir izvora**, a zatim kliknite **Lokalnom spremištu brojka**, a zatim kliknite **u redu**.  

    ![Lokalni brojka spremište](./media/app-service-deploy-local-git/local_git_selection.png)

3. Ako je ovo prvi put postavljate spremište u Azure, morate stvoriti vjerodajnice za prijavu za njega. Da biste se prijavili u Azure spremište i automatske promjene na lokalnom spremištu brojka će ih koristiti. Pokrenite aplikaciju plohu, kliknite **Postavke > vjerodajnice za implementaciju**, konfigurirate implementacije korisničko ime i lozinku. Kada završite, kliknite **Spremi**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Korak 4: Implementacija projekta

Poduzmite sljedeće korake da biste objavili aplikacije u aplikaciju servisa putem lokalnog brojka.

1. Pokrenite aplikaciju plohu na portalu za Azure, kliknite **Postavke > Svojstva** za **Brojka URL-a**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **URL brojka** je udaljene referenca za implementaciju na vašem lokalnom spremištu. URL ćete koristiti u sljedećim koracima.

2. Korištenje naredbenog retka, provjerite je li se u korijenskoj mapi na lokalnom spremištu brojka.

3. Korištenje `git remote` da biste dodali udaljene reference na popisu **URL brojka** iz korak 1. Naredba će izgledati otprilike ovako:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] Naredbe **udaljene** dodaje imenovani referenca udaljene spremište. U ovom primjeru stvara reference na imenovane 'azure' za spremište web app.

4. Slanje sadržaja za aplikacije servisa za korištenje u novi **azure** udaljene koji ste upravo stvorili.

        git push azure master

    Će se tražiti lozinku koju ste ranije stvorili kada ponovno postavljanje vjerodajnica implementacije na portalu za Azure. Unesite lozinku za (Imajte na umu da Gitbash jeku zvjezdice na konzolu kao upišite svoju lozinku). 
       
5. Vratite se u aplikaciju na portalu za Azure. Kao vrijednost unosa zapisnika vaše najnovije automatske moraju biti prikazane u plohu **implementacije** . 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Kliknite gumb **Pregledaj** pri vrhu plohu aplikacije da biste provjerili je postavila sadržaj. 
    
## <a name="Step5"></a>Otklanjanje poteškoća

Slijede pogreške ili najčešće došlo je do problema prilikom korištenja brojka objavljivanje aplikacije servisa za aplikacije u Azure:

****

**Simptoma**: nije moguće pristupiti '[siteURL]': nije uspjelo povezivanje sa [scmAddress]

**Uzrok**: ta se pogreška može dogoditi ako aplikacija nije s radom.

**Razlučivost**: na portalu za Azure pokrenite aplikaciju. Uvođenje brojka neće funkcionirati ako aplikacija nije pokrenut. 


****

**Simptoma**: ne može riješiti glavnog računala 'naziv glavnog računala"

**Uzrok**: ta se pogreška može dogoditi ako podatke o adresi unijeli prilikom stvaranja 'azure' udaljenom točni.

**Razlučivost**: korištenje na `git remote -v` naredba popis svih remotes, zajedno s odgovarajući URL. Provjerite jesu li URL-u 'azure' udaljenom točni. Ako je potrebno, uklonite i ponovno stvoriti ovaj alat za analizu daljinske pomoću točan URL.

****

**Simptoma**: nema refs u zajednički i ništa navedeno; Ništa ne učinite. Možda trebate navesti granu kao što su "matrica".

**Uzrok**: ta se pogreška može dogoditi ako ne navedete grani kada izvođenje na brojka automatske operacija, a niste postavili vrijednost push.default koristi brojka.

**Razlučivost**: Ponovite automatske postupak, pri određivanju osnovne grani. Ako, na primjer:

    git push azure master

****

**Simptoma**: src refspec [branchname] ne odgovara.

**Uzrok**: ta se pogreška može pojaviti ako pokušate da biste primijenili na granu osim matrice na 'azure' udaljene.

**Razlučivost**: Ponovite automatske postupak, pri određivanju osnovne grani. Ako, na primjer:

    git push azure master

****

**Simptoma**: pogreška – promjene izvršene udaljene spremište, ali web-aplikaciju programa neće ažurirati.

**Uzrok**: ta se pogreška može dogoditi ako implementirate Node.js aplikacije package.json datotekom koja određuje dodatne potrebne module.

**Razlučivost**: dodatne poruke sadrže "npm ERR!" mora biti prijavljenih prethodne ove pogreške, a možete unijeti dali kontekst na pogreške. Evo prijavljenih uzroka tu pogrešku i na odgovarajuće 'npm ERR!" poruka:

* **Oblikovan package.json datoteke**: npm ERR! Ne može čitati ovisnosti.

* **Izvorni modul koji imaju binarni distribucije za Windows**:

    * npm ERR! \`cmd "/ c" "čvor gyp izvođenje"\` nije uspjela 1

        OR

    * npm ERR! [modulename@version]predinstalirati: \`provjerite || gmake\`


## <a name="additional-resources"></a>Dodatni resursi

* [Dokumentacija brojka](http://git-scm.com/documentation)
* [Dokumentacija Kudu projekta](https://github.com/projectkudu/kudu/wiki)
* [Continous implementacije aplikacije servisa za Azure](app-service-continuous-deployment.md)
* [Kako pomoću ljuske PowerShell za Azure](../powershell-install-configure.md)
* [Kako koristiti sučelje naredbenog retka za Azure](../xplat-cli-install.md)

[Aplikacije servisa za Azure]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Portal za Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure sučelje naredbenog retka]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
