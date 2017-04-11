<properties
    pageTitle="Flighting implementacije (beta testiranju) u aplikacije servisa za Azure"
    description="Saznajte kako leta nove značajke u svojoj aplikaciji ili beta test ažuriranja ovog praktičnog vodiča završetka do kraja. Ga objedinjuje aplikacije servisa za značajke kao što su neprekinuti objavljivanja, slobodnih, usmjeravanje prometa i integracija uvida aplikacije."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting implementacije (beta testiranju) u aplikacije servisa za Azure

Pomoću ovog praktičnog vodiča pokazuje kako postići integriranje razne mogućnosti [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714) i [Azure aplikacije uvida](/services/application-insights/) *flighting implementacije* . 

*Flighting* je postupak implementacije koji provjerava nove značajke i promjena s ograničenim brojem stvarnih klijenata i glavnog testira u scenariju proizvodnje. To je akin to testiranje beta, a ponekad se naziva "nadziranim test leta". Većih tvrtki sa statusom prisutnosti web pomoću takvog Prijevremeni provjere valjanosti na svoje aplikacije ažuriranja u svoje vježbe [agilno](https://en.wikipedia.org/wiki/Agile_software_development)razvoja. Aplikacije servisa za Azure omogućuje integrirati test u radnog continous objavljivanje i uvida aplikacije za primjenu iste DevOps scenarij. Prednosti takvog obuhvaćaju sljedeće:

- **Rast ažuriranja realni povratne informacije _prije_ objavljivanja u** – samo stvar bolje nego pokušavaju povratne informacije čim otpustite se pokušavaju povratne informacije prije nego što pustite. Ažuriranja s pravi korisnik promet i ponašanja možete testirati i kao ranije u životnog ciklusa proizvoda.
- **Povećaj [utemeljenih na probno neprekinuti razvoj (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** – po integriranje probno u radni s neprekinutim integracije i instrumentation s uvida aplikacije provjere valjanosti korisnika događa ranijeg i automatski u vašem životnog ciklusa proizvoda. To tako smanjili količinu vremena ulaganja u izvođenja ručno test.
- **Optimizacija testirali tijek rada** – automatiziranje probno u proizvodnje s neprekinutim nadzora instrumentation, možete potencijalno izvršiti ciljeva različite vrste testove u jednom procesa, primjerice [integracije](https://en.wikipedia.org/wiki/Integration_testing), [regresije](https://en.wikipedia.org/wiki/Regression_testing), [upotrebljivosti](https://en.wikipedia.org/wiki/Usability_testing), pristupačnosti, lokalizaciju, [performanse](https://en.wikipedia.org/wiki/Software_performance_testing), [Sigurnost](https://en.wikipedia.org/wiki/Security_testing)i [prihvaćanje](https://en.wikipedia.org/wiki/Acceptance_testing).

Flighting implementaciju nije gotovo usmjerava promet uživo. U takvim uvođenja želite brzo moguće dobiti uvid li ga neočekivana pogreška, smanjene performanse performanse, a zatim korisničko sučelje problema. Imajte na umu radite realni klijentima. Tako da je desno, morate provjeriti da implementaciju sustava flighting ste postavili da prikupite sve podatke koji su vam potrebne da biste se informirali odluku za sljedeći korak. Pomoću ovog praktičnog vodiča prikazuje kako prikupiti podatke s uvida aplikacije, ali možete koristiti novu Relic ili druge tehnologije koji odgovara vašem scenariju. 

## <a name="what-you-will-do"></a>Što ćete učiniti

U ovom ćete praktičnom vodiču ćete naučiti da bi se prikazala sljedećim scenarijima zajedno da biste testirali aplikacije servisa za aplikacije u proizvodnje:

- [Usmjeravanje radnog promet](app-service-web-test-in-production-get-start.md) na aplikaciju beta
- [Instrument aplikacije](../application-insights/app-insights-web-track-usage.md) da biste dobili korisne mjerenja
- Neprestano implementacija aplikacije beta i praćenje metriku uživo aplikacije
- Usporedba mjernih podataka između radnog aplikacije i beta da biste vidjeli kako će se promjene kod prijevod rezultate

## <a name="what-you-will-need"></a>Što ćete

-   Račun za Azure
-   [GitHub](https://github.com/) računa
- Visual Studio 2015 - možete preuzeti [edition zajednice](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
-   Brojka ljuske (instalirati s [GitHub za Windows](https://windows.github.com/)) – to vam omogućuje da izvođenje naredbe brojka i PowerShell u istu sesiju
-   Najnovije [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bitova.
-   Osnovni razumijevanje od sljedećeg:
    -   Uvođenje predloška [Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md) (pogledajte [uvođenja složene aplikacije predvidljivije u Azure](app-service-deploy-complex-application-predictably.md))
    -   [Brojka](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Potreban vam je račun za Azure da biste dovršili ovaj Praktični vodič:
> + Možete je [besplatno otvorite račun za Azure](/pricing/free-trial/) - dobiti kredita možete koristiti da biste isprobali plaćenu servisa Azure, a čak i nakon što ste Iskorištena možete zadržati račun i korištenje slobodno Azure servisa, kao što su web-aplikacije.
> + Možete [aktivirati Visual Studio pretplatnika pogodnosti](/pricing/member-offers/msdn-benefits-details/) – vaše Visual Studio pretplata vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.
>
> Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="set-up-your-production-web-app"></a>Postavljanje web-aplikaciju programa proizvodnje

>[AZURE.NOTE] Skripta koja se koristi u ovom ćete praktičnom vodiču automatski će konfigurirati neprekinuti objavljivanje vaše GitHub spremištu. To su potrebne da vjerodajnice GitHub već pohranjena Azure, u suprotnom neće uspjeti skriptiranih implementacije prilikom pokušaja konfigurirati postavke izvora kontrole za web-aplikacije.
>
>Da biste pohranili vjerodajnice GitHub u Azure, stvorite web-aplikacijama u [Azure Portal](https://portal.azure.com/) i [konfigurirati GitHub implementaciju](app-service-continuous-deployment.md#Step7). Što je potrebno da biste to učinili jedanput.

U uobičajeni scenarij DevOps, imate aplikaciju koja se izvodi uživo u Azure, a želite mijenjati kroz neprekinuti objavljivanje. U ovom scenariju će implementacije radnog predložak koji ste razviti i testirati.

1.  Stvaranje vlastite o ogranku [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) spremištu. Informacije o stvaranju vaše o ogranku potražite u članku [o ogranku na Repo](https://help.github.com/articles/fork-a-repo/). Nakon stvaranja vaše o ogranku, možete ga vidjeti u pregledniku.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Otvorite sesiju ljuske brojka. Ako još nemate ljuske brojka, instalirajte [GitHub za Windows](https://windows.github.com/) .
3.  Stvaranje lokalne Kloniraj od vašeg o ogranku izvršavanjem sljedeće naredbe:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Nakon što dodate vaše lokalne Kloniraj, dođite do * &lt;repository_root >*\ARMTemplates, a zatim pokrenite na deploy.ps1 skripte jedinstveni nastavkom, kao što je prikazano u nastavku:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Kada se to od vas zatraži, upišite željeni korisničko ime i lozinku za pristup bazi podataka. Ne zaboravite vjerodajnice za bazu podataka jer je potrebno da biste odredili ih ponovno kada se ažuriraju u grupu resursa.

    Trebali biste vidjeti dodjele resursa tijek raznih Azure resursa. Kada se dovrši implementaciju, skripta će pokrenuti aplikaciju u pregledniku i omogućuju neslužbeni zvučni signal.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Natrag u sesiju ljuske brojka pokrenite:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Kada se dovrši skriptu, vratite se u idite na adresu u sučelju (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) da biste vidjeli aplikacije koje se izvode u proizvodnje.
5.  Prijavite se na [Portal za Azure](https://portal.azure.com/) i pogledajte koji je stvorio.

    Trebali biste moći vidjeti dva aplikacija za web apps u istoj grupi resursa, jedan s na `Api` sufiks u nazivu. Ako pogledate prikaz grupa resursa, vidjet ćete i SQL baze podataka i poslužitelj, aplikacije servisa za planiranje i pripremna slobodnih za web-aplikacije. Pregledavanje različitih resursa i usporedili ih s * &lt;repository_root >*\ARMTemplates\ProdAndStage.json da biste vidjeli o njihovoj konfiguraciji u predlošku.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Postavljanje radnog aplikaciju.  Sada ćemo zamislite primite povratnih informacija da je upotrebljivosti nisku aplikacije. Da bi se odlučite da biste istražili. Namjeravate instrumenata aplikacije za slanje povratnih informacija.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Istražite: Instrumenata aplikacije klijenta za nadzor/metriku

5. Otvaranje * &lt;repository_root >*\src\MultiChannelToDo.sln u Visual Studio.
6. Vrati sve pakete Nuget tako da desnom tipkom miša kliknete rješenje > **Upravljanje NuGet paketa rješenja** > **vratiti**.
6. Desnom tipkom miša kliknite **MultiChannelToDo.Web** > **Dodavanje Telemetrijskih aplikacije uvida** > **Konfiguriranje postavki** > Promijeni grupu resursa ToDoApp*&lt;your_suffix >* > **Dodavanje aplikacije uvid u projekt**.
7. Na portalu za Azure otvorite plohu za aplikaciju uvid resurs **MultiChannelToDo.Web** . U dijelu **aplikacije stanja** , zatim **upute da biste prikupili podatke za učitavanje preglednika stranice** > Kopiraj programski kod.
7. Dodavanje kopirane JS instrumentation koda za * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml neposredno prije zatvaranje `<heading>` oznaka. Ona mora sadržavati tipku jedinstveni instrumentation vaše aplikacije uvid resursa.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Slanje prilagođene događaje do uvida aplikacije za miša klikova dodavanjem sljedeći kod na dno tijelo:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    U ovom isječak jezika JavaScript šalje prilagođeni događaj aplikacije uvid u svaki put kada korisnik klikne gumb bilo gdje u web-aplikaciji.

12. U ljusci brojka izvršavanje i automatske promjene na vašem o ogranku u GitHub. Zatim, pričekajte klijentskim računalima i osvježite preglednik.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  Zamjena distribuiranih aplikacije promjene radnog:

        .\swap –Name ToDoApp<your_suffix>

13. Otvorite aplikaciju uvida resursa koje ste konfigurirali. Kliknite Prilagođeno događaja.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Ako ne vidite metriku prilagođene događaje, pričekajte nekoliko minuta pa kliknite **Osvježi**.

Pretpostavimo da se prikaže grafikon kao što su ispod:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

I rešetka događaj ispod njega:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Prema kodu aplikacije ToDoApp događaj **GUMB** odgovara gumb Pošalji, a događaj **unos** odgovara tekstni okvir. Dosad stvari smisla. Međutim, čini postoji dobar iznos klikova i vrlo nekoliko klikova na obaveza (događaje **Ćelije** ).

Ovisno o tome koje obrasca vaše pretpostavke koji su neki korisnici isto što se može kliknuti koji dio korisničkog Sučelja, a to znači da pokazivač stiliziran za odabir teksta kada zadržite na stavke popisa i njihove ikone.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

To može biti contrived primjer. Ipak, namjeravate učiniti poboljšanje u aplikaciju i zatim izvedite flighting implementaciju da biste dobili povratne informacije upotrebljivosti uživo klijenata.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Instrumenata poslužitelj aplikacije za nadzor/metriku
Ovo je na tangens jer scenarij planirati pomoću ovog praktičnog vodiča samo bavi aplikaciju klijenta. Međutim, za dovršenosti će postavite aplikaciju poslužiteljsko.

6. Desnom tipkom miša kliknite **MultiChannelToDo** > **Dodavanje Telemetrijskih aplikacije uvida** > **Konfiguriranje postavki** > Promijeni grupu resursa ToDoApp*&lt;your_suffix >* > **Dodavanje aplikacije uvid u projekt**.
12. U ljusci brojka izvršavanje i automatske promjene na vašem o ogranku u GitHub. Zatim, pričekajte klijentskim računalima i osvježite preglednik.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  Zamjena distribuiranih aplikacije promjene radnog:

        .\swap –Name ToDoApp<your_suffix>

To je to!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Istražiti: Dodavanje oznaka vremensko razdoblje specifične za vaš klijent aplikacije metriku

U ovom ćete odjeljku će konfigurirati slobodnih različitoj implementaciji da biste poslali telemetrijskih specifične za vremensko razdoblje isti resurs uvida aplikacije. Na taj način možete usporediti telemetrijskih podataka između promet s različitim slobodnih (implementacije okruženja) da biste jednostavno vidjeli promjene aplikacije. U isto vrijeme, možete razdvojiti promet radnog od ostatka tako da možete nastaviti praćenje radnog aplikacije po potrebi.

Budući da ste prikupljanje podataka na ponašanje, će [dodati telemetrijskih initializer da biste JavaScript kod](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) u index.cshtml. Ako želite testirati poslužiteljsko performanse, na primjer, i učiniti na sličan način u kodu server (pogledajte [API uvida aplikacije za prilagođene događaje i metrike](../application-insights/app-insights-api-custom-events-metrics.md).

1. Najprije dodajte kod bewteen dva `//` komentara ispod JavaScript blokirati koje ste dodali u `<heading>` označavanje neke starije verzije.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Kod initializer uzrokuje na `appInsights` objekt da biste dodali u prilagođeno svojstvo naziva `Environment` za svaki dio telemetrijskih šalje.

2. Zatim dodajte ovaj prilagođeno svojstvo kao [vremensko razdoblje postavke](web-sites-staged-publishing.md#AboutConfiguration) za web-aplikacije u Azure. Da biste to učinili, pokrenite sljedeće naredbe ljuske brojka sesiju.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Web.config u projektu već definira na `environment` postavke aplikacije. Uz tu postavku, pri testiranju aplikaciju lokalno, vaše mjernih podataka će biti označen `VS Debugger`. Međutim, kad automatske promjene za Azure, Azure će pronaći i koristiti u `environment` aplikacije postavku u konfiguraciji web-aplikacije i vaše metrike će biti označen `Production`.

3. Izvršavanje automatske promjene kod za vaše o ogranku na GitHub i pričekajte da korisnici upotrebljavaju novu aplikaciju (treba osvježite preglednik). Traje otprilike 15 minuta novo svojstvo koja će se prikazivati u aplikaciji uvide `MultiChannelToDo.Web` resursa.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Sada ponovno otvorite plohu **prilagođene događaje** i filtrirao metriku `Environment=Production`. Sada trebali biste moći vidjeti sve nove prilagođene događaje u jedno područje radnog s ovim filtrom.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Kliknite gumb **Favoriti** za spremanje trenutne postavke metrike Explorer nešto poput **prilagođene događaje: radni**. Koje možete jednostavno prijeći u ovom prikazu i prikazu vremensko razdoblje implementacije kasnije.

    > [AZURE.TIP] Još napredne analize uzmite u obzir [integriranje sustava resursa uvida aplikacije Power bi](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Dodavanje oznaka specifičnih za vremensko razdoblje u vaš poslužitelj aplikacije mjerenja
Servisi za dovršenosti će postavite aplikaciju poslužiteljsko. Za razliku od klijentskih aplikacija koje je instrumented u JavaScript oznake specifične za Blok za aplikaciju poslužitelj je instrumented .NET koda.

1. Otvaranje * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Dodavanje bloka kod ispod, neposredno prije zatvaranje vitičasta zagrada prostor naziva.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. Ispravljanje pogrešaka razlučivost naziva tako da dodate u `using` izjave ispod na početak datoteke:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. Dodavanje koda za ispod početku na `Application_Start()` metoda:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Izvršavanje automatske promjene kod za vaše o ogranku na GitHub i pričekajte da korisnici upotrebljavaju novu aplikaciju (treba osvježite preglednik). Traje otprilike 15 minuta novo svojstvo koja će se prikazivati u aplikaciji uvide `MultiChannelToDo` resursa.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Ažuriranje: Postavljanje sustava granu beta

2. Otvaranje * &lt;repository_root >*web-mjesto \ARMTemplates\ProdAndStagetest.json i u okvir za pretraživanje u `appsettings` resursi (Traženje `"name": "appsettings"`). Postoje 4 od njih, jedan za svaki vremensko razdoblje. 

2. Za svaku `appsettings` resursa, dodajte je `"environment": "[parameters('slotName')]"` postavka app do kraja na `properties` polja. Ne zaboravite da biste završili na prethodni redak sa zarezom.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    Koju ste upravo dodali na `environment` aplikacije postavku slobodnih u predlošku.
    
2. U okviru iste datoteke, pronađite u `slotconfignames` resursi (Traženje `"name": "slotconfignames"`). Postoje 2 od njih, jedan za svaku aplikaciju.

2. Za svaku `slotconfignames` resursa, dodajte `"environment"` do kraja na `appSettingNames` polja. Ne zaboravite da biste završili na prethodni redak sa zarezom.

    Koje ste upravo unijeli u `environment` aplikacije postavka uređaj za njegov odgovarajući implementacije vremensko razdoblje za oba aplikacije.  

3. U sesiju ljuske brojka pokrenite sljedeće naredbe s istom sufiks grupu resursa koje ste koristili prije.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Kada se to od vas zatraži, navedite u istom SQL vjerodajnice za bazu podataka kao prije. Zatim, kada se od vas zatraži da biste ažurirali grupu resursa, upišite `Y`, zatim `ENTER`.

    Nakon skriptu završi, sve resursa u izvornom grupu resursa se zadržavaju, ali novi blok pod nazivom "beta" stvara u konfiguraciji isti kao vremensko razdoblje "Pripremna" koja je stvorena u početka.

    >[AZURE.NOTE] Ova metoda stvaranja okruženja različitoj implementaciji razlikuje se od metoda u [razvoju Agilno softver sa servisom Azure aplikacije](app-service-agile-software-development.md). Ovdje, stvorite okruženja za implementaciju s slobodnih implementaciju, gdje koliko ima stvaranja okruženja za implementaciju s grupama resursa. Upravljanje okruženja za implementaciju s grupama resursa omogućuje da radnog okruženja off-limits za razvojne inženjere, ali nije lako učiniti testiranju u proizvodnje, možete napraviti jednostavno slobodnih.

Ako želite, alfa aplikacije možete stvoriti tako da pokrenete

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Za ovaj vodič samo nastaviti koristiti beta aplikacije.

## <a name="update-push-your-updates-to-the-beta-app"></a>Ažuriranje: Slanje ažuriranja aplikaciju beta

Vratite se u aplikaciju koju želite popraviti.

1. Provjerite je li sada ste u vašem beta grani

        git checkout beta

2. U * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, pronaći u `<li>` oznaku i dodati na `style="cursor:pointer"` atribut, kao što je prikazano u nastavku.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. Potvrđivanje i automatske za Azure.

4. Provjerite je li da se promjene je sada odražavaju u blok beta tako da odete http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Ako ne vidite promjene još, osvježite preglednik da biste dobili novi javascript kod.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Sad kad ste promjene koje izvodi u blok beta, spremni ste za izvođenje flighting implementacije.

## <a name="validate-route-traffic-to-the-beta-app"></a>Provjerite valjanost: Usmjeravanje prometa aplikaciju beta

U ovom ćete odjeljku će promet usmjerili na aplikaciju beta. For sake of preglednosti pokazni, namjeravate usmjeravanje značajan dio promet korisnika. U stvari, količinu prometa koji želite preusmjeriti će ovise o vašoj određene. Na primjer, ako je web-mjesta na razini web-mjesta Microsoft.com, zatim morat ćete manje od jednog postotak ukupni prometa da bi se dobio korisne podatke.

1. U sesiju ljuske brojka, pokrenite sljedeće naredbe za usmjeravanje polovicu promet radnog vremensko razdoblje beta:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  Na `ReroutePercentage=50` svojstvo navodi da 50% promet radnog moguće usmjeriti na aplikaciju beta URL (određeno u `ActionHostName` svojstvo).

2. Sada dođite do http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50% promet sada mora biti preusmjereni na beta razdoblje.

3. U resursu vaše aplikacije uvida filtrirati metriku okruženje = "beta".

    > [AZURE.NOTE] Ako spremite ovaj filtrirani prikaz drugog favorite, pa možete jednostavno zrcalite prikazi metričkim explorer između radnog i beta prikaza.

Pretpostavimo da u aplikaciju uvida vidjet ćete nešto slično sljedećem:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Ne samo to prikazuje da nema više dodatnih klikova na na `<li>` oznake, ali postoji čini se da se na stabilizatorom klikova na `<li>` oznake. Zatim možete zaključite ljudi imati otkrio novi `<li>` oznaka je moguće kliknuti i sada poništite sve svoje prethodno dovršiti zadatke u aplikaciji.

Na temelju podataka flighting uvođenja odlučite novo korisničko sučelje bilo spreman za proizvodnju.

## <a name="go-live-move-your-new-code-into-production"></a>Otvorite uživo: premještanje novu šifru radnog

Sada ste spremni za premještanje ažuriranje proizvodnje. Što je odličan je da sada znate da ažuriranje već je provjerene _prije nego što_ ga je Završi proizvodnje. Odmah možete Povjerljivo njegove implementacije kod. Budući da ste napravili ažuriranje aplikacije za klijenta u AngularJS, samo provjeriti kodom na strani klijenta. Ako ste da biste unijeli promjene u web-aplikaciju pozadinskih Web API App, ne može provjeriti promjene na sličan način i jednostavno.

1. U ljusci brojka ukloniti promet pravila za usmjeravanje pokretanjem sljedeće naredbe:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. Pokretanje naredbe brojka:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Pričekajte nekoliko minuta za nove kod koji će biti implementirano u pripremna vremensko razdoblje, a zatim pokrenite http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net da biste potvrdili da novo ažuriranje warmed prema gore u pripremna vremensko razdoblje. Imajte na umu da u svoje o ogranku osnovne granu je povezana s pripremna vremensko razdoblje aplikacije.

3. Sada zamjena pripremna vremensko razdoblje u proizvodnje

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Sažetak ##

Aplikacije servisa za Azure olakšava male – da biste srednjim tvrtkama da biste testirali svojih aplikacija kupaca dostupnog u proizvodnje nešto koji Najčešći obavljeno veliki tvrtki. Međutim, ovaj Praktični vodič vam je dala znanja morate objediniti aplikacije servisa i uvida aplikacije da bi moguće flighting implementacije i scenariji i druge test u radni na svijetu DevOps. 

## <a name="more-resources"></a>Dodatni resursi ##

-   [Agilno softver razvoja s aplikacije servisa za Azure](app-service-agile-software-development.md)
-   [Postavljanje pripremna okruženja za web-aplikacije u aplikacije servisa za Azure](web-sites-staged-publishing.md)
-   [Implementacija složene aplikacije predvidljivije servisu Azure](app-service-deploy-complex-application-predictably.md)
-   [Omogućeno na Azure Voditelj resursa](../resource-group-authoring-templates.md)
-   [JSONLint – u alat za provjeru valjanosti JSON](http://jsonlint.com/)
-   [Brojka grananja – osnovni grananja i spajanje](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Wiki Kudu projekta](https://github.com/projectkudu/kudu/wiki)
