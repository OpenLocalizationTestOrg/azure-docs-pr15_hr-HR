<properties
    pageTitle="Agilno softver razvoja s aplikacije servisa za Azure"
    description="Saznajte kako stvoriti vrlo skaliranje složene aplikacije sa servisom Azure aplikacije na način koji podržava agilno software development."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Agilno softver razvoja s aplikacije servisa za Azure #

U ovom ćete praktičnom vodiču će Saznajte kako stvoriti vrlo skaliranje složene aplikacije sa [Servisom Azure aplikacije](/services/app-service/) na način koji podržava [agilno software development](https://en.wikipedia.org/wiki/Agile_software_development). Pretpostavlja se da ste već znate kako [implementirati složene aplikacije predvidljivije Azure](app-service-deploy-complex-application-predictably.md).

Ograničenja u tehničke procesa često može stajati in the way of uspješno implementaciju agilno methodologies. Aplikacije servisa za Azure pomoću značajke kao što su [Neprekinuti objavljivanja](app-service-continuous-deployment.md), [pripremna okruženja](web-sites-staged-publishing.md) (slobodnih) i [nadzor](web-sites-monitor.md), kada pametan povezano s djelovanje i upravljanje implementacije u [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md)može biti dio sjajno rješenja za razvojne inženjere koji obuhvaćaju agilno software development.

U sljedećoj su tablici je kratki popis preduvjeti pridružene agilno razvoj i kako servisa Azure omogućuje svaki od njih.

| Preduvjet | Kako se omogućuje Azure |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Sastavljanje sa svake bloka<br>– Stvaranje automatski i brzo | Kada je konfiguriran za korištenje neprekinuti implementaciju, aplikacije servisa za Azure možete funkcionirati kao izgradi live pokrenut na temelju razvojni grani. Svaki put kada je kod Završi granu, ona se automatski ugrađeni i radi uživo u Azure.|
| – Sastavlja provjerite samostalnih testiranje | Učitavanje testira testira web, itd., možete uvesti pomoću predloška Azure Voditelj resursa.|
| -Testove u Kloniraj radnog okruženja | Azure resursima predlošci mogu se koristiti za stvaranje clones Azure radnog okruženja (uključujući postavki aplikacije, predlošci niz veze, Skaliranje, itd.) za testiranje brzo i predvidljivije.|
| – Prikaz rezultata ažuran jednostavno | Neprekinuti implementacije Azure iz spremišta, to znači da možete testirati novu šifru u aplikaciji za uživo odmah nakon provođenja promjene. |
| -Primjenu glavni granu svaki dan<br>-Automatizirati implementacije | Neprekinuti Integracija radnog aplikacijom u spremištu glavni granu automatski uvodi svaki potvrdi/spajanja glavni granu da biste radni. |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Što ćete učiniti ##

Će voditi kroz Tipični razvojni-test-faze-radnog tijeka rada kako biste mogli objaviti novih Promjena uzorka aplikaciju [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) koji se sastoji od dva [web-aplikacije](/services/app-service/web/), jedan se sučelju (FE), a drugi se pozadinskog Web API (BE), a bazom [podataka SQL](/services/sql-database/). Će raditi arhitektura implementacije prikazano u nastavku:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Da biste umetnuli sliku u riječi:

-   Arhitektura implementacije je razdvojen tri različita okruženja (ili [grupe resursa](../azure-resource-manager/resource-group-overview.md) u Azure), svaka s vlastitom [aplikacije servisa za planiranje](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [Skaliranje](web-sites-scale.md) postavke i SQL baze podataka. 
-   Svako okruženje upravlja zasebno. Čak i može postojati u različite pretplate.
-   Pripremna i radnih primjenjuju se kao dva slobodnih iste aplikacije servisa za aplikacije. Glavni granu je postavljena za neprekinuti Integracija s pripremna vremensko razdoblje.
-   Kad se potvrdi za osnovne granu provjerava se na pripremna vremensko razdoblje (s podacima radnog), aplikaciju provjereno pripremna se zamjenjuju u prikazu radnog vremensko razdoblje [s bez nedostupnost](web-sites-staged-publishing.md).

Okruženje za proizvodnju i pripremna definira predloška na [ * &lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Razvojni i testiranje okruženja definira predloška na [ * &lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Uobičajeni grananja strategije će koristiti i s kodom premještanje s granu razvojni najviše granu probno zatim osnovne podružnice (Pomicanje prema gore u kvalitete, tako da speak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Što ćete ##

-   Račun za Azure
-   [GitHub](https://github.com/) računa
-   Brojka ljuske (instalirati s [GitHub za Windows](https://windows.github.com/)) – to vam omogućuje da izvođenje naredbe brojka i PowerShell u istu sesiju 
-   Najnovije [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi) bitova.
-   Osnovni razumijevanje od sljedećeg:
    -   [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md) predložak implementacije (i pogledajte [uvođenja složene aplikacije predvidljivije u Azure](app-service-deploy-complex-application-predictably.md))
    -   [Brojka](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Potreban vam je račun za Azure da biste dovršili ovaj Praktični vodič:
> + Možete je [besplatno otvorite račun za Azure](/pricing/free-trial/) - dobiti kredita možete koristiti da biste isprobali plaćenu servisa Azure, a čak i nakon što ste Iskorištena možete zadržati račun i korištenje slobodno Azure servisa, kao što su web-aplikacije.
> + Možete [aktivirati Visual Studio pretplatnika pogodnosti](/pricing/member-offers/msdn-benefits-details/) – vaše Visual Studio pretplata vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.
>
> Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="set-up-your-production-environment"></a>Postavljanje okruženja za proizvodnju ##

>[AZURE.NOTE] Skripta koja se koristi u ovom ćete praktičnom vodiču automatski će konfigurirati neprekinuti objavljivanje vaše GitHub spremištu. To su potrebne da vjerodajnice GitHub već pohranjena Azure, u suprotnom neće uspjeti skriptiranih implementacije prilikom pokušaja konfigurirati postavke izvora kontrole za web-aplikacije. 
>
>Da biste pohranili vjerodajnice GitHub u Azure, stvorite web-aplikacijama u [Azure Portal](https://portal.azure.com/) i [konfigurirati GitHub implementaciju](app-service-continuous-deployment.md). Što je potrebno da biste to učinili jedanput. 

U uobičajeni scenarij DevOps, imate aplikaciju koja se izvodi uživo u Azure, a želite mijenjati kroz neprekinuti objavljivanje. U ovom scenariju imate predložak koji razvijaju, testirati i koristi za implementaciju radnog okruženja. Koje će ga postaviti u ovom odjeljku.

1.  Stvaranje vlastite o ogranku [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) spremištu. Informacije o stvaranju vaše o ogranku potražite u članku [o ogranku na Repo](https://help.github.com/articles/fork-a-repo/). Nakon stvaranja vaše o ogranku, možete ga vidjeti u pregledniku.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Otvorite sesiju ljuske brojka. Ako još nemate ljuske brojka, instalirajte [GitHub za Windows](https://windows.github.com/) .

3.  Stvaranje lokalne Kloniraj od vašeg o ogranku izvršavanjem sljedeće naredbe:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Nakon što dodate vaše lokalne Kloniraj, dođite do * &lt;repository_root >*\ARMTemplates, a zatim pokrenite na deploy.ps1 skripte na sljedeći način:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Kada se to od vas zatraži, upišite željeni korisničko ime i lozinku za pristup bazi podataka.

    Trebali biste vidjeti dodjele resursa tijek raznih Azure resursa. Kada se dovrši implementaciju, skripta će pokrenuti aplikaciju u pregledniku i omogućuju neslužbeni zvučni signal.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Pogledajte * &lt;repository_root >*\ARMTemplates\Deploy.ps1 da biste vidjeli kako generira resursi jedinstvenih ID-ova. Na isti način možete koristiti da biste stvorili clones isti implementacije bez brige o sukobu nazive resursa.
 
6.  Natrag u sesiju ljuske brojka pokrenite:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Kada se dovrši skriptu, vratite se u idite na adresu u sučelju (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) da biste vidjeli aplikacije koje se izvode u proizvodnje.
 
5.  Prijavite se na [Portal za Azure](https://portal.azure.com/) i pogledajte koji je stvorio.

    Trebali biste moći vidjeti dva aplikacija za web apps u istoj grupi resursa, jedan s na `Api` sufiks u nazivu. Ako pogledate prikaz grupa resursa, vidjet ćete i SQL baze podataka i poslužitelj, aplikacije servisa za planiranje i pripremna slobodnih za web-aplikacije. Pregledavanje različitih resursa i usporedili ih s * &lt;repository_root >*\ARMTemplates\ProdAndStage.json da biste vidjeli o njihovoj konfiguraciji u predlošku.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Sada ste postavili radnog okruženja. Nakon toga će izbaciti novo ažuriranje aplikaciji.

## <a name="create-dev-and-test-branches"></a>Stvaranje razvojni i testiranje grana ##

Sad kad ste u složene aplikacije koje se izvode u proizvodnje u Azure, napravite ažuriranje u skladu s agilno methodology aplikaciju. U ovom ćete odjeljku će stvoriti na razvojni i testiranje grane koji morat ćete učiniti ažuriranja koja su potrebna.

1.  Najprije stvorite okruženje za testiranje. U sesiju ljuske brojka, pokrenite sljedeće naredbe radi stvaranja okruženja za nove granu pod nazivom **NewUpdate**. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Kada se to od vas zatraži, upišite željeni korisničko ime i lozinku za pristup bazi podataka. 

    Kada se dovrši implementaciju, skripta će pokrenuti aplikaciju u pregledniku i omogućuju neslužbeni zvučni signal. I samo tako da imate novi granu vlastitu okruženje za testiranje. Potrajati koji trenutak da biste pregledali nekoliko stvari o okruženje za testiranje:

    -   Možete je stvoriti u bilo kojem Azure pretplate. To znači da radnog okruženja njome se upravlja zasebno iz okruženje za testiranje.
    -   Okruženje za testiranje radi uživo u Azure.
    -   Okruženje za testiranje jednak okruženju proizvodnje, osim pripremna slobodnih i skaliranja postavke. To možete znati jer su samo razlike između ProdandStage.json i Dev.json.
    -   Okruženje za testiranje možete upravljati u zasebnom aplikacije servisa za plan s različitih cijena sloju (kao što su **besplatno**).
    -   Brisanje okruženje za testiranje bit će jednostavna koliko i brisanje grupa resursa. Vidjet ćete kako učinite ovo [kasnije](#delete).

2.  Idite stvaranje razvojni granu ponovnim pokretanjem sljedeće naredbe:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Kada se to od vas zatraži, upišite željeni korisničko ime i lozinku za pristup bazi podataka. 

    Potrajati koji trenutak da biste pregledali nekoliko stvari o okruženje za razvojni: 

    -   Okruženje za razvojni sadrži jednake probno okruženje za konfiguraciju jer je u upotrebi koristeći isti predložak.
    -   Svako okruženje razvojni mogu stvoriti u pretplatu Azure programere vlastite ostavite okruženje za testiranje zasebno upravljanje.
    -   Okruženje za razvojni radi uživo u Azure.
    -   Brisanje razvojni okruženje je jednostavna koliko i brisanje grupa resursa. Vidjet ćete kako učinite ovo [kasnije](#delete).

>[AZURE.NOTE] Ako imate više razvojnim inženjerima rad na novo ažuriranje, svaku od njih jednostavno možete stvarati podružnice i namjenski razvojni okruženje tako da učinite sljedeće:
>
>1. Stvaranje vlastite o ogranku u spremištu u GitHub (pogledajte [o ogranku na Repo](https://help.github.com/articles/fork-a-repo/)).
>2. Kloniraj o ogranku na svoje lokalno računalo
>3. Pokrenite iste naredbe za stvaranje vlastitih razvojni podružnice i okruženja.

Kada završite, potrebno je o ogranku vaše GitHub tri grana:

![](./media/app-service-agile-software-development/test-1-github-view.png)

I imat ćete šest web-aplikacije (tri skupa dva) u tri zasebna resursa grupe:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Imajte na umu da ProdandStage.json određuje radnog okruženja koristiti **standardne** cijene sloju koji odgovara skalabilnost aplikacije radnog.

## <a name="build-and-test-every-commit"></a>Stvaranje i testiranje svaki potvrdi ##

Datoteke predložaka ProdAndStage.json i Dev.json već navedite kontrolnih parametara izvora koji po zadanom postavlja neprekinuti objavljivanje za web-aplikacije. Zbog toga svaki potvrdi da biste granu GitHub pokreće automatski implementacije za Azure iz tog grani. Pogledajmo kako funkcionira postavu sada.

1.  Provjerite nalazite granu razvojni u lokalnom spremištu. Da biste to učinili, pokrenite sljedeću naredbu u ljusci brojka:

        git checkout Dev

2.  Unos promjena u jednostavne sloju korisničkog Sučelja aplikacije promjenom koda za korištenje popisa [samopokretanja programa](http://getbootstrap.com/components/) . Otvaranje * &lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml i provjerite istaknuti promjena ispod:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Ako se ne može čitati gornjoj slici: 
    >
    >- U retku 18 promjena `check-list` da biste `list-group`.
    >- U retku 19, promijenite `class="check-list-item"` da biste `class="list-group-item"`.

3.  Spremiti promjene. Natrag u ljusci brojka, pokrenite sljedeće naredbe:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Te naredbe brojka su slične "Prijava u kodu" drugih sustava za kontrolu izvora kao što su TFS. Kada pokrenete `git push`, novi izvršavanje pokreće se automatski kod automatske za Azure, ponovno aplikacija u skladu s vizualnim promjena u okruženju razvojni.

4.  Da biste provjerili ovaj kod automatske svoje radno okruženje razvojni pojavila, idite na razvojni okruženja web app plohu i pogledajte dio **implementacije** . Trebali biste moći vidjeti najnovije potvrdi poruku postoji.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  Iz njega, kliknite **Pregledaj** da biste vidjeli nove promjena u aplikaciji uživo u Azure.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    Ovo je vrlo manji promjena u aplikaciji. No mnoga vremena novih promjena za složene web-aplikacije sadrži neželjeni i neželjenog popratne pojave. Mogućnost jednostavno testirajte svaki potvrdi u uživo izdanja omogućuje vam da biste vidjeli promjene te probleme prije nego što ih korisnici vidjeti.

Tako da sada, morate biti upoznati s realization da kao programiranje na projektu **NewUpdate** , moći jednostavno stvaranje okruženja razvojni za sebe, a zatim sastavljanje svaki potvrdi i testiranje svaki Sastavi.

## <a name="merge-code-into-test-environment"></a>Spajanje kod u okruženje za testiranje ##

Kada budete spremni automatske koda iz podružnice razvojni najviše NewUpdate granu, je postupku standardni brojka:

1.  Spojite sve nove se obvezuje brinuti NewUpdate u grani razvojni u GitHub, kao što su pamti stvorile druge razvojnim inženjerima. Sve nove potvrdi na GitHub će pokrenuti kod automatske i stvorite u okruženju razvojni. Zatim možete biti sigurni kod u grani razvojni i dalje biti funkcionalna najnovije bitova iz NewUpdate grani.

2.  Spojite sve svoje nove pamti iz razvojni podružnice u NewUpdate grana na GitHub. Time se pokreće kod automatske i Sastavi u okruženje za testiranje. 

Imajte na umu ponovno da jer neprekinuti implementacije već postavljanje s ove brojka grana, ne morate ništa poduzimati druge kao da se izvodi Integracija sastavlja. Jednostavno morate izvršiti standardne izvor kontrole prakse pomoću brojka i Azure će izvođenje svih procesa Sastavi umjesto vas.

Sada ćemo automatske kod da biste **NewUpdate** grani. U ljusci brojka, pokrenite sljedeće naredbe:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

To je to! 

Idite na aplikaciju plohu web okruženju sustava test da biste vidjeli nove potvrdi (spojiti NewUpdate grani) sada pomiču okruženje za testiranje. Kliknite **Pregledaj** da biste vidjeli da Promjena stila sada se izvodi uživo u Azure.

## <a name="deploy-update-to-production"></a>Implementacija ažuriranja radnog ##

Odaberite kod okruženje pripremna i radnih treba čini vam se ne razlikuje se od što ste već dovršili pritisak kod okruženje za testiranje. Zaista to jednostavno je. 

U ljusci brojka, pokrenite sljedeće naredbe:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Imajte na umu da se temelji na način na koji se pripremna i radnog okruženja je postavljena na ProdandStage.json, novu šifru pomiču se za vremensko razdoblje **pripremna** i je li pokrenut. Da ako do URL-a pripremna razdoblje, prikazat će se novi kod koji se izvodi postoji. Da biste to učinili, pokrenite na `Show-AzureWebsite` cmdlet u ljusci brojka.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
I nakon potvrde ažuriranja u pripremna vremensko razdoblje samo stvar preostaje je sada da biste zamijenili u radnog. U ljusci brojka, jednostavno pokrenite sljedeće naredbe:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Čestitamo! Novo ažuriranje ste uspješno objaviti na web-aplikacije radnog. Što 's više je samo napravili je jednostavno stvaranjem razvojni i testiranje okruženja, stvaranje i testiranje svaki potvrdi. To su presudne sastavne blokove za razvoj agilno softvera.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Brisanje razvojni i testiranje enviroments ##

Budući da ste nastojanju architected razvojni i testiranje okruženja biti grupe samostalnih resursa, vrlo je jednostavno ih izbrisati. Da biste izbrisali one stvorene u ovom ćete praktičnom vodiču grana GitHub i Azure artefakte samo u ljusci brojka izvršite sljedeće naredbe:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Sažetak ##

Agilno software development je na dijaloški mora za mnoge tvrtke koji žele prihvaćaju Azure kao njihove aplikacijske platforme. U ovom ćete praktičnom vodiču ste naučili kako stvoriti i tear dolje točno replike ili blizu replike radnog okruženja jednostavno, čak i za složene aplikacije. Ste naučili i upute za korištenje Ova mogućnost da biste stvorili postupak razvoja koje možete izraditi i testirati svaki jedan potvrdi u Azure. Pomoću ovog praktičnog vodiča sadrži Međutim prikazuje vam kako najbolje pomoću aplikacije servisa za Azure i upravljanja resursima Azure zajedno da biste stvorili rješenje DevOps caters agilno methodologies. Nakon toga možete sastaviti na ovom scenariju izvođenjem napredne postupke DevOps kao što su [testiranju u radnog](app-service-web-test-in-production-get-start.md). Uobičajeni scenarij za testiranje-u – radnog potražite u članku [Flighting implementacije (beta testiranju) u aplikacije servisa za Azure](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Dodatni resursi ##

-   [Implementacija složene aplikacije predvidljivije servisu Azure](app-service-deploy-complex-application-predictably.md)
-   [Agilno razvoja u praksi: savjeti i upute za razvoj Modernized ciklusa](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Strategije napredne implementacije za Azure web-aplikacije pomoću predložaka Voditelj resursa](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Omogućeno na Azure Voditelj resursa](../resource-group-authoring-templates.md)
-   [JSONLint – u alat za provjeru valjanosti JSON](http://jsonlint.com/)
-   [ARMClient – postavljanje GitHub objavljivanje na web-mjesta](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Brojka grananja – osnovni grananja i spajanje](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Blog Nevena Ebbo](http://blog.davidebbo.com/)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Azure alati naredbenog retka za različite platforme](../xplat-cli-install.md)
-   [Stvaranje ili uređivanje korisnika u Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Wiki Kudu projekta](https://github.com/projectkudu/kudu/wiki)
