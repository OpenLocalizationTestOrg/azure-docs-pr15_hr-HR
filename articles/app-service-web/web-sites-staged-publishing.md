<properties
    pageTitle="Postavljanje pripremna okruženja za web-aplikacije u aplikacije servisa za Azure"
    description="Saznajte kako pomoću postupne objavljivanje web-aplikacijama u servisu Azure aplikacije."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Postavljanje pripremna okruženja za web-aplikacije u aplikacije servisa za Azure
<a name="Overview"></a>

Kada [Aplikacije servisa za](http://go.microsoft.com/fwlink/?LinkId=529714)implementacija web-aplikaciju programa, možete implementirati za zasebnom implementaciju blok umjesto produkcijske razdoblje zadani kada izvodi u načinu plan **Standardno** ili **Premium** aplikacije servisa. Uvođenje slobodnih su zapravo uživo web-aplikacije s vlastite hostnames. Elementi sadržaja i konfiguracija web app možete se zamjenjuju između dva slobodnih implementaciju, uključujući vremensko razdoblje radnog. Implementacija aplikacije da biste vremensko razdoblje za implementaciju ima sljedeće prednosti:

- Prije zamjena s vremensko razdoblje radnog može provjeriti valjanost web app promjene u pripremna vremensko razdoblje implementacije.

- Najprije implementacija web-aplikacijama na vremensko razdoblje i zamjena u radnog osigurava da su sve instance na vremensko razdoblje warmed prije nego što se zamjenjuju u radni. To uklanja nedostupnost kada implementacija web-aplikaciju programa. Preusmjeravanje prometa je objedinjenog i bez zahtjeva za ispuštaju se kao rezultat operacije Zamijeni. U ovom cijeli tijek rada moguće je automatizirati konfiguriranjem [Zamijenili automatski](#configure-auto-swap-for-your-web-app) prilikom provjere valjanosti stara Zamijeni nije potrebna.

- Nakon Zamijeni, vremensko razdoblje i prethodno postupnu web app sada ima prethodne radnog web-aplikaciji. Ako su promjene zamjenjuju u jedno područje radnog ne onako kako ste očekivali, možete izvršiti iste Zamijeni da biste pristupili vaše "zadnji poznati dobar web-mjesto", odmah natrag.

Svakom načinu rada za aplikacije servisa za planiranje podržava različit broj slobodnih implementacije. Da biste saznali broj slots podržava načinu web app potražite u odjeljku [Cijena servisa aplikacija](/pricing/details/app-service/).

- Kada web-aplikaciju programa ima više slobodnih, ne možete promijeniti način rada.

- Promjena veličine nije dostupna za koje nisu radnog slobodnih.

- Upravljanje resursima povezane nije podržana za koje nisu radnog slobodnih. [Portal za Azure](http://go.microsoft.com/fwlink/?LinkId=529715) samo izbjegavanje ovog utjecaj na jedno područje radnog pomicanjem privremeno vremensko razdoblje koje nisu radnog načina rada aplikacije servisa za planiranje. Imajte na umu da vremensko razdoblje koje nisu radnog morate opet zajedničko korištenje na isti način s vremensko razdoblje radnog prije nego što možete zamjena dva razdoblja.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Dodavanje vremensko razdoblje za implementaciju web App ##

U načinu za **Standardno** ili **Premium** mogli omogućiti više slobodnih implementacije mora biti pokrenut web-aplikaciji.

1. [Portal za Azure](https://portal.azure.com/)otvorite plohu web app.
2. Kliknite **Postavke**, a zatim kliknite **slobodnih implementacije**. Nakon toga u plohu **implementaciju slobodnih** kliknite **Dodavanje vremensko razdoblje**.

    ![Dodajte novi Blok za implementaciju][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Ako web-aplikaciji još nije u načinu za **Standardno** ili **Premium** , primit ćete poruku podržani načina za omogućivanje postupnu objavljivanje. Sada imate mogućnost da biste odabrali **nadogradnje** , a zatim idite na karticu **Skaliranje** web-aplikacije prije nastavka.

2. U plohu **Dodaj na vremensko razdoblje** imenujte na vremensko razdoblje i odaberite želite li Kloniraj web-aplikacije konfiguracija iz drugog postojeće implementacije vremensko razdoblje. Kliknite kvačicu da biste nastavili.

    ![Konfiguriranje izvora][ConfigurationSource1]

    Kada prvi put dodate na vremensko razdoblje, samo će imati dvije mogućnosti: Kloniraj konfiguraciju iz zadano razdoblje u proizvodnje ili uopće ne.

    Nakon stvaranja više slobodnih će moći Kloniraj konfiguraciju iz vremensko razdoblje osim onog u radnog:

    ![Konfiguriranje izvora][MultipleConfigurationSources]

5. U plohu **implementacije slobodnih** kliknite vremensko razdoblje za implementaciju da biste otvorili plohu za vremensko razdoblje, s skup metriku i konfiguracija baš kao web app. **your-Web-App-name-Deployment-slot-name** pojavit će se pri vrhu plohu podsjetiti pregledavate vremensko razdoblje implementacije.

    ![Uvođenje jedno područje naslova][StagingTitle]

5. Kliknite aplikaciju URL u plohu na vremensko razdoblje. Obratite pozornost na to vremensko razdoblje za implementaciju ima vlastiti naziv glavnog računala, a ujedno uživo aplikacije. Da biste ograničili pristup javnim vremensko razdoblje za implementaciju, potražite u članku [Web-aplikacije servisa aplikacija – blokirati web pristup slobodnih implementacije koje nisu radnog](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Postoji nikakav sadržaj nakon stvaranja implementacije vremensko razdoblje. Iz različitih spremište granu ili potpuno različite spremište može uvesti u vremensko razdoblje. Možete promijeniti i konfiguracija na vremensko razdoblje. Koristite Objavi profila i implementaciju vjerodajnicama vremensko razdoblje uvođenja sadržaja ažuriranja.  Na primjer, možete [objaviti ovaj blok s brojka](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Konfiguracija za implementaciju slobodnih ##
Kada Kloniraj konfiguraciju iz drugog vremensko razdoblje implementacije, konfiguracije kloniranu se može uređivati. Osim toga, neke elemente konfiguracije slijediti sadržaj preko Zamijeni (ne vremensko razdoblje određene) dok ostale elemente konfiguracije će ostati u istom vremensko razdoblje nakon Zamijeni (vremensko razdoblje određene). Na sljedećem su popisu Prikaži konfiguracija koja će se promijeniti kada zamjena slobodnih.

**Postavke koje su zamjenjuju**:

- Opće postavke – kao što je 32/64-bitnu verziju framework Web sockets
- Postavki aplikacije (može biti konfigurirano da bi se na vremensko razdoblje)
- Nizove veze (moguće je konfigurirati da bi se na vremensko razdoblje)
- Rukovatelj mapiranja
- Postavke nadzora i dijagnostike
- WebJobs sadržaja

**Postavke koje su zamjenjuju**:

- Objavljivanje krajnje točke
- Prilagođenih naziva domena
- SSL certifikata i poveznici
- Postavke mjerila
- Planiranje WebJobs

Da biste konfigurirali u aplikaciju postavke ili veze niz da bi se razdoblje (ne zamjenjuju), pristup plohu **Postavke aplikacije** za određene vremensko razdoblje, a zatim potvrdite okvir **Vremensko razdoblje postavke** za konfiguriranje elemente koji se moraju ostaje odabrana u vremensko razdoblje. Imajte na umu te označavanje konfiguracije element vremensko razdoblje određene ima učinak uspostavljanje taj element kao ne swappable preko svih slobodnih implementacije povezan s web-aplikaciji.

![Postavke vremensko razdoblje][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Da biste zamijenili slobodnih implementacije ##

>[AZURE.IMPORTANT] Prije zamijenili web-aplikaciji iz vremensko razdoblje za implementaciju u proizvodnje, provjerite jesu li sve postavke određene koje nisu vremensko razdoblje konfigurirani točno onako kako želite da imate u ciljnoj Zamijeni.

1. Da biste zamijenili slobodnih implementaciju, kliknite gumb **Zamjena** na naredbenoj traci web-aplikacije ili u naredbenoj traci vremensko razdoblje implementacije. Provjerite jesu li u zamjena izvorišno i odredišno zamjena postavljeni ispravno. Obično zamjena ciljne bi vremensko razdoblje proizvodnje.  

    ![Zamjena gumb][SwapButtonBar]

3. Kliknite **u redu** da biste dovršili postupak. Kada završi postupak, zamjenjuju ste je slobodnih implementaciju.

## <a name="configure-auto-swap-for-your-web-app"></a>Konfigurirajte automatsko zamjena za web-aplikacije ##

Zamjena automatski pojednostavnjuje scenariji DevOps mjesto na koje želite stalno implementacije web aplikacije s nulom Hladna start i nula nedostupnost krajnji korisnici web-aplikacije. Kada vremensko razdoblje za implementaciju konfiguriran za automatsko Zamijeni u proizvodnje, svaki put kada automatske ažuriranje kod da biste tu vremensko razdoblje, aplikacije servisa za će automatski zamijenite web-aplikaciju u radni nakon što ga je već warmed prema gore u na vremensko razdoblje.

>[AZURE.IMPORTANT] Kada omogućite automatsko zamjena za na vremensko razdoblje, provjerite je li konfiguracija vremensko razdoblje točno konfiguracije namijenjene ciljni vremensko razdoblje (obično razdoblje proizvodnje).

Jednostavno je konfiguriranje automatski zamjena za na vremensko razdoblje. Slijedite korake u nastavku:

1. U plohu **Implementaciju slobodnih** odaberite vremensko razdoblje koje nisu radnog, kliknite **Sve postavke** za plohu te vremensko razdoblje.  

    ![][Autoswap1]

2. Kliknite **Postavke aplikacije**. **Odaberite za **Automatsko zamjena**** odaberite vremensko razdoblje željeni cilj u **Automatsko zamjena vremensko razdoblje**, a naredbenoj traci kliknite **Spremi** . Provjerite je li konfiguracija na vremensko razdoblje točno konfiguracije namijenjene vremensko razdoblje cilj.

    Na kartici **obavijesti** će flash zeleni **USPJEH** nakon dovršetka postupka.

    ![][Autoswap2]

    >[AZURE.NOTE] Da biste testirali automatski zamjena za web-aplikacije koje možete odabrati vremensko razdoblje cilj koji nisu radnog u **Automatsko zamjena vremensko razdoblje** da biste se upoznali sa značajkom.  

3. Izvršavanje Pritisni kod za taj implementacije vremensko razdoblje. Zamjena automatski će se dogoditi nakon kratko vrijeme i ažuriranje će biti vidljiv na URL-u ciljnog razdoblje.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Korištenje više faza zamjena za web-aplikacije ##

Zamjena više faza dostupna je da biste pojednostavnili provjere valjanosti u kontekstu elemenata konfiguracije namijenjene da bi se s vremensko razdoblje kao što su nizovi veze. U tim slučajevima može biti korisno da biste primijenili takvih elemenata konfiguracije iz ciljnog Zamijeni s izvorom Zamijeni i provjeriti prije zamjena zapravo stupa na snagu. Kada zamjena cilj konfiguracije elemente primjenjuju se na izvor zamjena su dostupne akcije dovršavanje na zamjena ili vraćanje izvorne konfiguracije izvora zamjena koji ima učinak otkazivanje na Zamijeni. Uzorka za dostupne za više faza zamjena cmdleti za Azure PowerShell obuhvaćeni su Azure PowerShell cmdleti za implementaciju slobodnih sekcije.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Za vraćanje radnog aplikacija nakon zamjena ##

Ako su sve pogreške označena u radnog nakon vremensko razdoblje zamjena, vratiti na slobodnih stara zamjena vrijednosti po odmah zamjena isti dva razdoblja.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Prilagođeni warm-up prije zamjena ##

Neke se aplikacije možda ćete morati warm-up prilagođene akcije. Konfiguriranje element applicationInitialization u web.config omogućuje vam da navedete pokretanje prilagođene akcije koje je potrebno izvršiti prije nego što se primi zahtjev. Postupak Zamijeni čekati na ovaj prilagođeni warm-up da biste dovršili. Evo fragment za web.config uzorka.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>Da biste izbrisali vremensko razdoblje za implementaciju##

U plohu za implementaciju razdoblje, na naredbenoj traci kliknite **Izbriši** .  

![Brisanje vremensko razdoblje za implementaciju][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Azure PowerShell cmdleti za implementaciju slobodnih

Azure PowerShell je modul u kojoj se navode cmdleti za upravljanje Azure putem komponente Windows PowerShell, uključujući podršku za upravljanje web app implementacije slobodnih u servisu Azure aplikacije.

- Informacije o instaliranju i konfiguriranju Azure PowerShell te provjere autentičnosti Azure PowerShell s pretplatom Azure potražite u članku [kako instalirati i konfigurirati Microsoft Azure PowerShell](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>Stvaranje web-aplikacije

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Stvaranje implementacije Blok za web-aplikacije

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Pokretanje više faza Zamijeni i primijenite cilj vremensko razdoblje konfiguracije izvora vremensko razdoblje

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Vrati prva faza zamjena više faza i vraćanje konfiguracije izvora vremensko razdoblje

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Zamjena slobodnih implementacije

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Brisanje vremensko razdoblje implementacije

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Azure naredbe sučelje naredbenog retka (Azure EŽA) za implementaciju slobodnih

Azure EŽA sadrži više platformi naredbi za rad s Azure, uključujući podršku za upravljanje slobodnih implementaciju Web App.

- Upute za instaliranje i konfiguriranje EŽA Azure, uključujući informacije o povezivanju Azure EŽA pretplate Azure potražite u članku [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md).

-  Da biste popis naredbi koje su dostupne za aplikacije servisa za Azure u EŽA Azure, nazovite `azure site -h`.

----------
### <a name="azure-site-list"></a>Popis Azure web-mjesta
Informacije o web-aplikacije u trenutne pretplate, nazovite **popisu azure web-mjesta**, kao u sljedećem primjeru.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Stvaranje web-mjesta za Azure
Da biste stvorili vremensko razdoblje za implementaciju, poziva **Stvaranje azure web-mjesta** i navedite naziv web-aplikacije programa postojeće i vremensko razdoblje da biste stvorili, kao u sljedećem primjeru.

`azure site create webappslotstest --slot staging`

Da biste omogućili izvora kontrole za nove vremensko razdoblje, koristite mogućnost **– brojka** , kao u sljedećem primjeru.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>Zamjena Azure web-mjesta
Da biste ažurirane implementacije utor aplikaciju radnog, upotrijebite naredbu **Zamjena azure web-mjesta** za izvođenje operacije Zamijeni, kao u sljedećem primjeru. Aplikaciju radnog ćete primijetiti neki put niti će undergo Hladna start.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Brisanje Azure web-mjesta
Da biste izbrisali vremensko razdoblje za implementaciju, koji više nije potrebna, pomoću naredbe **Izbriši azure web-mjesta** , kao u sljedećem primjeru.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="next-steps"></a>Daljnji koraci ##
[Azure Web aplikacije servisa za aplikacije – blokirati web pristup slobodnih implementacije koje nisu radnog](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Besplatna probna verzija sustava Microsoft Azure](/pricing/free-trial/)

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
