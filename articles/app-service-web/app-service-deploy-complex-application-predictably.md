<properties
    pageTitle="Dodjela resursa i implementacija microservices predvidljivije u Azure"
    description="Saznajte kako implementirati aplikaciju sastoji od microservices u aplikacije servisa za Azure kao jedna jedinica i predvidljivi način pomoću predložaka za grupu resursa JSON i skriptiranje PowerShell."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Dodjela resursa i implementacija microservices predvidljivije u Azure #

Pomoću ovog praktičnog vodiča pokazuje kako Dodjela resursa i implementacija aplikacije sastoji od [microservices](https://en.wikipedia.org/wiki/Microservices) [Aplikacije servisa za Azure](/services/app-service/) kao jedna jedinica i predvidljivi način pomoću predložaka za grupu resursa JSON i skriptiranje PowerShell. 

Prilikom dodjele resursa i implementacija visoke skaliranje aplikacije koja se sastoji od iznimno samostalne microservices, repeatability i predvidljivost su ključnih uspjeh. [Aplikacije servisa za Azure](/services/app-service/) vam omogućuje stvaranje microservices koji sadrže web-aplikacije, mobilne aplikacije, API aplikacije i logike aplikacije. [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md) omogućuje vam da biste upravljali sve microservices kao jedinica, zajedno s ovisnosti resursa, kao što su baze podataka i izvora kontrole postavke. Sada možete uvesti i programa aplikacije pomoću predložaka JSON i jednostavnog skriptiranje PowerShell. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Što ćete učiniti ##

U ovom praktičnom vodiču će implementacija aplikacije koja obuhvaća:

-   Dva web-aplikacije (odnosno dva microservices)
-   Pozadinske baze podataka SQL
-   Postavke za aplikaciju, nizove veze i kontrola izvora
-   Aplikacija uvide, upozorenja, a zatim postavke autoscaling

## <a name="tools-you-will-use"></a>Alati za koju ćete koristiti ##

U ovom ćete praktičnom vodiču će koristiti sljedeće alate. Budući da nije potpun rasprave Alati, ću bi s scenarij završetka do kraja i samo vam dati kratak Uvod u svaki, i gdje mogu pronaći više informacija na njemu. 

### <a name="azure-resource-manager-templates-json"></a>Predlošci Azure Voditelj resursa (JSON) ###
 
Svaki put kada stvorite web-aplikacijama u aplikacije servisa za Azure, na primjer, Voditelj resursa Azure koristi JSON predložak da biste stvorili grupu cijela resursa s resursima za komponentu. Složene predloška iz [Trgovine Azure](/marketplace) kao aplikaciju [Skalabilni WordPress](/marketplace/partners/wordpress/scalablewordpress/) možete uključiti baze podataka MySQL, račune za pohranu, aplikacije servisa za planiranje i web-aplikaciji sam upozorenja pravila, postavki aplikacije, automatsko skaliranje postavke i dodatne informacije i sve te predloške dostupne putem komponente PowerShell. Informacije za preuzimanje i korištenje tih predložaka potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).

Dodatne informacije o predlošcima Azure upravljanja resursima potražite u članku [Stvaranje predložaka za resursima Azure](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2,6 za Visual Studio ###

Od najnovijeg SDK sadrži poboljšanja podršku predložak Voditelj resursa u uređivaču JSON. Možete ta postavka omogućuje brzo stvaranje predloška grupu resursa od nule ili otvaranje postojećeg predloška JSON (kao što je predložak preuzete Galerija) za izmjenu popunjavanje parametara datoteku, a čak i uvesti grupa resursa izravno iz rješenja Azure grupu resursa.

Dodatne informacije potražite u članku [Azure 2,6 SDK za Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 ili noviji ###

Početak u verziji 0.8.0, instalacija Azure PowerShell sadrži modul Azure Voditelj resursa osim modul Azure. Modul za nove omogućuje skripte implementacije grupa resursa.

Dodatne informacije potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Explorer Azure resursa ###

Ovaj [Alat za pregled](https://resources.azure.com) omogućuje vam da biste istražili definicije JSON svih grupa resursa u svoju pretplatu i pojedinačnih resursa. U alatu za uređivanje definicija JSON resursa, izbrisali cijelu hijerarhiju resursa, i stvaranje nove resurse.  Informacije koje su uvijek su dostupne u tom alatu vrlo koristan je prilikom predloška za stvaranje jer je prikazuje koje svojstva morate postaviti za određene vrste resursa, odgovarajuće vrijednosti, itd. Možete čak stvoriti grupu resursa za [Portal za Azure](https://portal.azure.com/), a zatim Provjera njegove definicije JSON u alatu za explorer da biste lakše templatize grupu resursa.

### <a name="deploy-to-azure-button"></a>Implementacija Azure gumbu ###

Ako koristite GitHub izvor kontrole, možete staviti [uvođenja Azure gumb](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) u vašem README. MD, na koji omogućuje uključivanje ključ implementacije Sučelja za Azure. Dok je to možete učiniti za bilo koju aplikaciju za jednostavne web, možete proširiti da biste omogućili implementacija i resursa za cijelu grupu postavljanjem datoteku azuredeploy.json u korijenskoj mapi spremište. Ove datoteke JSON, koja sadrži predložak grupu resursa, koristit će uvođenja Azure gumb da biste stvorili grupu resursa. Na primjer, pogledajte [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) uzorka, koji će se koristiti u ovom ćete praktičnom vodiču.

## <a name="get-the-sample-resource-group-template"></a>Primjer predloška u grupu resursa za početak ##

Odmah recimo dobar na njega.

1.  Dođite do aplikacije servisa za uzorak [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) .

2.   U readme.md, zatim **Implementiraj za Azure**.
 
3.  Trenutno za [implementaciju-na-azure](https://deploy.azure.com) web-mjesta i od vas zatraži da ulazne parametre implementacije. Obratite pozornost na to da većinu polja su popunjen naziv spremište i neke slučajni nizove umjesto vas. Sva polja možete promijeniti ako želite, ali samo stvari koje morate unijeti prijava administratora sustava SQL Server i lozinku, a zatim kliknite **Dalje**.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Nakon toga kliknite **uvođenja** da biste pokrenuli postupak implementacije. Kada se pokreće postupak do završetka posla, kliknite http://todoapp*XXXX*. azurewebsites.net vezu da biste pregledali distribuiranih aplikacije. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    Korisničko Sučelje bi biti malo sporo kada prvi put pregledavate Internet da biste ga jer su samo pokreće aplikacije, ali sami convince je potpuno funkcionalni aplikacije.

5.  Na stranici uvođenje kliknite vezu **Upravljanje** da biste vidjeli nove aplikacije na portalu za Azure.

6.  Na padajućem popisu **Essentials** kliknite vezu za grupu resursa. Imajte na umu i web-aplikaciji već povezan s spremište GitHub u **Vanjskim projekt**. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  U grupi plohu resursa Imajte na umu da već postoje dva web-aplikacije i jedan SQL baze podataka u grupi resursa.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
Sve što ste upravo vidjeli nekoliko minuta kratki potpuno distribuiranih dva microservice aplikacije, sa svim komponente, ovisnosti, postavke, baze podataka, i neprekinuti objavljivanje postavljen tako da automatski djelovanje u Azure Voditelj resursa. Sve to je učiniti dvije stvari:

-   Uvođenje Azure gumbu
-   azuredeploy.JSON u korijenskoj mapi repo

Možete implementirati ove aplikacije isti tens, stotine ili tisuće vremena i imaju točno ista konfiguracija svaki put. U repeatability i predvidljivost takvog omogućuje vam za implementaciju aplikacije visoko Skaliranje s jednostavnost i confidence.

## <a name="examine-or-edit-azuredeployjson"></a>Pregledajte (ili uredite) AZUREDEPLOY. JSON ##

Sada Pogledajmo kako spremište GitHub postavljen. Bit ćete uređivaču JSON u Azure .NET SDK pa ako već niste instalirali [Azure .NET SDK 2,6](/downloads/), učinite to odmah.

1.  Kloniraj spremište [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) pomoću alata za omiljene brojka. U nastavku snimka li se time u programu Explorer tima u Visual Studio 2013.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  Iz korijena spremište otvorite azuredeploy.json u Visual Studio. Ako ne vidite okno strukture JSON, morate instalirati Azure .NET SDK.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Ne ću opisuje svaku detalja JSON OSNOVNI oblik, ali u odjeljku [Dodatne resurse](#resources) sadrži veze za učenje jezik predloška za grupu resursa. Ovdje, samo ću da bi se prikazala zanimljive značajke koje omogućuju početak rada u mijenjanju vlastiti prilagođeni predložak za implementaciju aplikacije.

### <a name="parameters"></a>Parametri ###

Pogledajte odjeljak parametara da biste vidjeli veći dio parametara su što gumb **uvođenja za Azure** traži unos. Web-mjesto iza gumb **uvođenja za Azure** popunjava unos korisničkog Sučelja pomoću parametara koji je definiran u azuredeploy.json. Ti Parametri se koriste u cijeloj definicije resursa, kao što su nazivi resursa, vrijednosti nekretnina itd.

### <a name="resources"></a>Resursi ###

U čvor resursa, vidjet ćete da 4 najviše razine resursi definiraju, uključujući instancu komponente SQL Server, tarifu aplikacije servisa i dva web-aplikacije. 

#### <a name="app-service-plan"></a>Aplikacije servisa za planiranje ####

Započnimo s jednostavne resursa korijenskoj razini u na JSON. U strukturi JSON kliknite aplikacije servisa za plan pod nazivom **[hostingPlanName]** da biste istaknuli odgovarajuće JSON kod. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Imajte na umu da se `type` element određuje niz za tarifu aplikacije servisa (je značajka zvala farme poslužitelja dugačke, dugo vrijeme prije) i druge elemente i svojstva popunjavaju pomoću parametara koji je definiran u datoteci JSON, a ovaj resurs nema nikakve ugniježđene resurse.

>[AZURE.NOTE] Imajte na umu i da vrijednost `apiVersion` govori Azure koju verziju REST API-JA za korištenje resursa definiciju JSON s, a može utjecati na unutar kako koje želite oblikovati resursa u `{}`. 

#### <a name="sql-server"></a>SQL Server ####

Nakon toga kliknite pod nazivom **SQLServer** strukturi JSON resurs sustava SQL Server.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Imajte na umu sljedeće informacije o istaknuti JSON kod:

-   Korištenje parametara osigurava stvoreni Resursi su pod nazivom i konfiguriran na način koji omogućuje dosljedan međusobno.
-   Resurs SQLServer ima dva ugniježđene resursa, svaka ima različite vrijednosti za `type`.
-   Ugniježđene resursi unutar `“resources”: […]`, gdje su definirana baza podataka i pravila vatrozida, imaju u `dependsOn` element koji određuje Identifikator resursa SQLServer resursa korijenskoj razini. Ovo govori Azure Voditelj resursa, "da biste mogli stvoriti ovaj resurs koji neki drugi resurs mora već postojati; a ako je definiran drugih resursa u predlošku, zatim stvorite da jedna najprije".

    >[AZURE.NOTE] Detaljne informacije o korištenju na `resourceId()` funkcija potražite u članku [Funkcije predložak za Azure Voditelj resursa](../resource-group-template-functions.md).

-   Učinak na `dependsOn` element je možete znati Voditelj resursa Azure resursa koji se može stvoriti paralelno i resursa koji se moraju se stvoriti sekvencijalno. 

#### <a name="web-app"></a>Web-aplikacije ####

Sada ćemo prijeći na stvarni web-aplikacije same, koje su složenije. Kliknite web-aplikacija [variables('apiSiteName')] u strukturi JSON da biste istaknuli njegov kod JSON. Primijetit ćete da je stvari koje se prikazuju mnogo zanimljive. U tu svrhu li objasniti značajke jedan po jedan:

##### <a name="root-resource"></a>Korijenska resursa #####

Web-aplikaciji ovisi o dvije različite resurse. To znači da Voditelj resursa Azure će stvoriti web-aplikaciji tek kada se stvaraju aplikacije servisa za planiranje i instancu sustava SQL Server.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Postavke za aplikaciju #####

Postavki aplikacije i definiraju se kao ugniježđene resursa.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

U na `properties` element `config/appsettings`, imate dvije postavke za aplikaciju u obliku `“<name>” : “<value>”`.

-   `PROJECT`je [KUDU postavku](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) koja govori Azure implementacije koje projekta za korištenje u Visual Studio rješenje više projekta. I vidjet ćete kasnije kako je konfigurirano kontrola izvora, ali jer je kod ToDoApp rješenja za Visual Studio više projekta, moramo tu postavku.
-   `clientUrl`je jednostavno aplikacije postavka koja kod aplikacija koristi.

##### <a name="connection-strings"></a>Nizove veze #####

U nizu za povezivanje i definiraju se kao ugniježđene resursa.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

U na `properties` element `config/connectionstrings`, svaki niz za povezivanje i definirana je kao vrijednost: naziva par, s određenim oblikovanjem od `“<name>” : {“value”: “…”, “type”: “…”}`. Za na `type` element, moguće vrijednosti su `MySql`, `SQLServer`, `SQLAzure`, i `Custom`.

>[AZURE.TIP] Potpuni popis vrsta niza veze, pokrenite sljedeću naredbu u Azure PowerShell: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Kontrola izvora #####

Postavke kontrola izvora i definiraju se kao ugniježđene resursa. Azure Voditelj resursa koristi ovaj resurs za konfiguriranje neprekinuti objavljivanje (potražite u članku caveat na `IsManualIntegration` kasnije) i želite izbaciti za implementaciju aplikacije kod automatski tijekom obrade JSON datoteke.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`i `branch` mora biti vrlo intuitivno i moraju pokazivati spremište brojka i naziv granu iz za objavljivanje. Ponovno, te su definirana ulazne parametre. 

Imajte na umu u na `dependsOn` element koji, osim web-aplikacije resursa sam `sourcecontrols/web` i ovisi o `config/appsettings` i `config/connectionstrings`. Razlog je tome što kada `sourcecontrols/web` je konfiguriran, proces Azure implementacije automatski će pokušati implementacije, stvaranje i pokretanje aplikacija kod. Dakle, umetanje ovaj ovisnost će vam pomoći da aplikaciju da ima pristup nizovima veze i postavki aplikacije potreban prije pokretanja aplikacije kod. 

>[AZURE.NOTE] Imajte na umu i da `IsManualIntegration` postavljen na `true`. To svojstvo nije potrebno ovog praktičnog vodiča jer nisu doista u vašem vlasništvu spremište GitHub, a time ne zapravo dati dozvolu za Azure (odnosno konfiguriranje neprekinuti objavljivanje iz [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) automatske spremište Automatska ažuriranja za Azure). Koristite zadanu vrijednost `false` za navedeni spremište samo ako ste konfigurirali vlasnika GitHub vjerodajnice [Azure portal](https://portal.azure.com/) prije. Drugim riječima, ako ste postavili izvor kontrole GitHub ili BitBucket za bilo koju aplikaciju za [Portal za Azure](https://portal.azure.com/) prethodno, pomoću korisničke vjerodajnice, zatim Azure će zapamtiti vjerodajnica te ih koristiti kad god se u budućnosti implementacija bilo koju aplikaciju iz GitHub ili BitBucket. Međutim, ako još niste učinili već, uvođenje predloška JSON neće uspjeti kada Voditelj resursa Azure pokuša Konfiguriranje postavki kontrole izvora za web-aplikaciji jer ne možete prijaviti u GitHub ili BitBucket s vlasnik spremište vjerodajnica.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>Usporedba JSON predložak s grupom distribuiranih resursa ##

Ovdje, može proći kroz sve web-aplikacije na blades [Azure Portal](https://portal.azure.com/), ali postoji neki drugi alat koji nije baš kao korisno ako više. Otvorite alat za pretpregled [Explorer Azure resursa](https://resources.azure.com) koji vam JSON prikaz svih grupa resursa u pretplatama, kao što je zapravo postoji u Azure pozadinskog. Možete vidjeti i kako grupi resursa JSON hijerarhije u Azure odgovara hijerarhije u datoteku predloška koji se koristi za stvaranje.

Ako, na primjer, kada otvorite alat za [Azure resursa Explorer](https://resources.azure.com) i proširite čvorove u pregledniku, prikazati grupu resursa i resurse korijenskoj razini koji su prikupljeni u odjeljku njihove vrste odgovarajući resursa.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Ako pretraživanje naniže do web-aplikacijama, trebali da biste vidjeli detalje za konfiguraciju web app slično kao u nastavku snimka zaslona:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Ponovno ugniježđene resursi mora imati hijerarhije vrlo je sličan s onima u datoteku predloška JSON, a trebali biste vidjeti postavki aplikacije, nizove veze, itd., ispravno odražavaju u oknu JSON. Odsutnost se postavke mogu upućivati na problem s datotekom JSON, a može pomoći pri otklanjanju JSON datoteku predloška.

## <a name="deploy-the-resource-group-template-yourself"></a>Uvođenje predloška grupa resursa ##

Sjajan je gumb **uvođenja za Azure** , ali omogućuje uvođenje predloška grupu resursa u azuredeploy.json samo ako već imate Završi azuredeploy.json GitHub. Azure .NET SDK također nudi alate za implementaciju bilo koju datoteku predloška JSON izravno s lokalnog računala. Da biste to učinili, slijedite korake u nastavku:

1.  U Visual Studio, kliknite **datoteka** > **Novo** > **projekta**.

2.  Kliknite **Visual C#** > **oblaka** > **Grupa resursa Azure**, zatim kliknite **u redu**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  **Odabir predloška Azure**, odaberite **Prazni predložak** , a zatim kliknite **u redu**.

4.  Povucite azuredeploy.json u mapu **predložaka** programa novi projekt.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  Preglednik rješenja, otvorite kopiranu azuredeploy.json.

6.  Samo radi pokazni, dodat ćemo nekih standardnih resursa aplikacije uvid naš JSON datoteku, tako da kliknete **Dodaj resursa**. Ako vas zanima samo implementacija datoteka JSON, prijeđite na korake uvođenja.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Odaberite **Uvida aplikaciju za web-aplikacije**, zatim provjerite je li na postojeće aplikacije servisa za planiranje i web-aplikacije uključen, a zatim kliknite **Dodaj**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    Sada ćete moći nekoliko nove resurse koji, ovisno o resursa i funkcija potražite u članku su međuzavisnosti aplikacije servisa za planiranje ili web-aplikaciji. Ove resurse nisu omogućeni prema svojim postojeću definiciju i želite promijeniti.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  U strukturi JSON kliknite **appInsights automatsko skaliranje** da biste istaknuli njegov kod JSON. To je skaliranja postavka za plan aplikacije servisa.

9.  Istaknuti kod JSON, pronađite u `location` i `enabled` svojstva i postavite ih kao što je prikazano u nastavku.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. U strukturi JSON kliknite **CPUHigh appInsights** da biste istaknuli njegov kod JSON. Ovo je upozorenje.

11. Pronađite na `location` i `isEnabled` svojstva i postavite ih kao što je prikazano u nastavku. Isto učinite i za druge tri upozorenja (Ljubičasta bulbs).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. Sada ste spremni za implementaciju. Desnom tipkom miša kliknite projekt, a zatim odaberite **uvođenja** > **Novu implementaciju**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Ako to još niste učinili, prijavite na račun za Azure.

14. Odaberite postojeću grupu resursa u svoju pretplatu ili stvorite novi, odaberite **azuredeploy.json**, a zatim **Uređivanje parametara**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    Sada ćete moći uređivati sve parametre definirano u datoteku predloška bolje tablice. Parametre koji određuju zadane postavke već imate njihove zadane vrijednosti, a parametre koji određuju popis dopuštenih vrijednosti će se prikazati kao dropdowns.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Ispunite prazne parametre i koristiti [adresu repo GitHub ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) u **repoUrl**. Zatim kliknite **Spremi**.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] Autoscaling je značajka koja se koristila u **standardni** sloju ili noviji, a plana razinu upozorenja su značajke koje nudi u sloju **Osnovni** ili noviji, morat ćete postaviti parametar **sku** **standardnih** ili **Premium** da biste vidjeli sve svoje nove aplikacije uvide resursi Osnovno prema gore.
    
16. Kliknite **Implementacija**. Ako ste odabrali **Spremanje lozinki**, lozinku spremit će se u u parametru datoteke **u obliku običnog teksta**. U suprotnom, koji će se zatražiti da unos lozinke baze podataka tijekom postupka implementacije.

To je to! Sada samo želite idite na [Portal za Azure](https://portal.azure.com/) i alat za [Azure resursa Explorer](https://resources.azure.com) da biste vidjeli nove upozorenja i automatsko skaliranje postavke dodane na vaše JSON implementirati aplikaciju.

Mogući su koraci u ovom odjeljku uglavnom napraviti sljedeće:

1.  Pripremite datoteku predloška
2.  Stvorili parametar datoteke za datoteku predloška
3.  Implementiran datoteku predloška s datotekom parametra

Cmdlet ljuske PowerShell jednostavno poduzeti posljednji korak. Da biste vidjeli Visual Studio radnje kada je implementirati aplikaciju, otvorite Scripts\Deploy AzureResourceGroup.ps1. Postoji mnogo kod tamo, ali samo ću da biste istaknuli važne kod morate uvesti datoteku predloška s datotekom parametar.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Zadnji cmdlet `New-AzureResourceGroup`, onaj koji zapravo izvodi akciju. Sve to treba prikazuju vama, pomoću tooling je razmjerno jednostavne za implementaciju aplikacije oblaka predvidljivije. Svaki put kada pokrenite cmdlet na isti predložak s iste parametar datoteke koje ćete dobiti isti rezultat.

## <a name="summary"></a>Sažetak ##

U DevOps, repeatability i predvidljivost su tipke s bilo kojeg uspješno implementacijom aplikacije visoko skaliranje sastoji od microservices. U ovom ćete praktičnom vodiču ste implementiran dva microservice aplikacije za Azure grupno pojedinačni resurs pomoću ovog predloška Azure Voditelj resursa. Međutim, ona vam je dala znanja potrebne za pokretanje aplikacije u Azure pretvaranja u predložak možete Dodjela resursa i njegove implementacije kod predvidljivije. 

## <a name="next-steps"></a>Daljnji koraci ##

Saznajte kako [primijeniti agilno methodologies i neprestano objavljivanje aplikacija microservices jednostavno](app-service-agile-software-development.md) i napredne implementacija tehnike kao što su [flighting implementaciju](app-service-web-test-in-production-controlled-test-flight.md) jednostavno.

<a name="resources"></a>
## <a name="more-resources"></a>Dodatni resursi ##

-   [Jezik predloška Azure Voditelj resursa](../resource-group-authoring-templates.md)
-   [Omogućeno na Azure Voditelj resursa](../resource-group-authoring-templates.md)
-   [Azure funkcije predložak Voditelj resursa](../resource-group-template-functions.md)
-   [Implementacija aplikacije pomoću predloška Azure Voditelj resursa](../resource-group-template-deploy.md)
-   [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md)
-   [Otklanjanje poteškoća s implementacijama grupa resursa u Azure](../resource-manager-troubleshoot-deployments-portal.md)




 
