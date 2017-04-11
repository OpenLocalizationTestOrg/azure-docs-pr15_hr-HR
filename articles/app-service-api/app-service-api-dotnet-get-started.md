<properties
    pageTitle="Početak rada s aplikacijama API-JA i ASP.NET u aplikacije servisa za | Microsoft Azure"
    description="Saznajte kako stvarati, uvođenje i zauzeti aplikacije ASP.NET API na servisu Azure aplikacije pomoću Visual Studio 2015."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Početak rada s API aplikacije, ASP.NET i Swagger u aplikacije servisa za Azure

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Ovo je prvi u nizu vodiči koji pokazuju kako pomoću značajke servisa Azure aplikacije koje su korisne za hostiranje RESTful API-ji i razvoj.  Pomoću ovog praktičnog vodiča pokriva podrška za API metapodataka u obliku Swagger.

Ćete saznati:

* Kako stvoriti i implementirati [API aplikacije](app-service-api-apps-why-best-platform.md) u aplikacije servisa za Azure pomoću alata za ugrađen u Visual Studio 2015.
* Kako automatizirati API otkrivanje pomoću značajke pakiranja Swashbuckle NuGet za dinamičko stvaranje Swagger API metapodataka.
* Kako je koristiti Swagger API metapodataka za automatsko generiranje koda za klijenta za aplikaciju API-JA.

## <a name="sample-application-overview"></a>Pregled aplikacija uzorka

U ovom ćete praktičnom vodiču radite s probna aplikacija za jednostavne poslova popisa. Aplikacija ima sučelja aplikacije za jednu stranicu (SPA), srednji sloj API servisa platforme ASP.NET Web i u sloju ASP.NET Web API podataka.

![API aplikacije uzorka aplikacije dijagrama](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Evo snimka zaslona s [AngularJS](https://angularjs.org/) sučelje.

![API aplikacije poslušajte aplikacije popis obveza](./media/app-service-api-dotnet-get-started/todospa.png)

Rješenja za Visual Studio obuhvaća tri projekata:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - sučelja: programa AngularJS SPA koja poziva Srednji sloj.

* **ToDoListAPI** - Srednji sloj: projekta programa ASP.NET Web API poziva sloju podataka za izvođenje operacija CRUD na obaveza.

* **ToDoListDataAPI** - sloju podataka: projekta programa API servisa platforme ASP.NET Web koji se izvodi postupke CRUD na obaveza.

Arhitektura tri sloja jedan je od mnogo arhitekturi koje možete provesti pomoću aplikacije API-JA i koristi se ovdje samo za potrebe pokazni. Kod u svakom sloju jednostavno je moguće da bismo pokazali značajki aplikacije API-JA na primjer, razina podataka koristi poslužitelj memorije umjesto bazu podataka kao njegov postojanost mehanizam.

Na dovršetak ovog praktičnog vodiča, imat ćete dva Web API projekata prema gore i izvodi u oblak u aplikacijama za API usluge za aplikacije.

Na sljedeći Praktični vodič u nizu uvodi sučelje SPA u oblak.

## <a name="prerequisites"></a>Preduvjeti

* API servisa platforme ASP.NET Web - vodiča upute pretpostavlja da ste osnovni znanje o tome kako raditi s ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) u Visual Studio.

* Račun za Azure – možete [besplatno otvorite račun za Azure](/pricing/free-trial/?WT.mc_id=A261C142F) ili [aktivirali Visual Studio pretplatnika prednosti](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751). Postoji, možete odmah stvoriti short-lived starter aplikaciju u aplikacije servisa za – **potrebna bez kreditne kartice**, a ne p.

* Visual Studio 2015 s [Azure SDK za .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - u SDK automatski instalira Visual Studio 2015 ako već nemate.

    * U Visual Studio, kliknite Pomoć -> o programu Microsoft Visual Studio i provjerite jesu li imati "Alati za aplikacije servisa za Azure v2.9.1" ili noviji instaliran.

    ![Azure vesion Alati za aplikaciju](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Ovisno o tome koliko ovisnosti SDK već imate na računalu, instalacije SDK može potrajati, od nekoliko minuta pola sata ili više.

## <a name="download-the-sample-application"></a>Preuzimanje oglednih aplikacije

1. Preuzmite [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) spremište.

    Možete kliknuti gumb **Preuzimanje POŠTANSKI** ili Kloniraj spremište na lokalnom računalu.

2. Otvorite rješenje ToDoList u Visual Studio 2015 ili 2013.
   1. Morat ćete pouzdanost svakog rješenja.
        ![Sigurnosno upozorenje](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Stvaranje rješenja (CTRL + SHIFT + B) da biste vratili NuGet paketa.

    Ako želite da biste vidjeli aplikaciju u operaciji prije njegove implementacije kod, možete ga pokrenuti lokalno. Provjerite je li ToDoListDataAPI pokretanje projekta i pokrenite rješenja. Možete očekivati da biste vidjeli HTTP 403 pogreške u pregledniku.

## <a name="use-swagger-api-metadata-and-ui"></a>Korištenje Swagger API metapodataka i korisničkog Sučelja

Podrška za [Swagger](http://swagger.io/) 2.0 metapodataka API ugrađen u aplikacije servisa za Azure. Svaku aplikaciju API-JA možete odrediti URL krajnju točku koja vraća metapodataka za API na Swagger JSON OSNOVNI oblik. Metapodaci vratio te krajnje točke se može koristiti za generiranje koda za klijenta.

Projekt programa API servisa platforme ASP.NET Web dinamički mogu uzrokovati Swagger metapodataka pomoću značajke pakiranja NuGet [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) . Paket Swashbuckle NuGet već je instalirana na projektima ToDoListDataAPI i ToDoListAPI koji ste preuzeli.

U ovom odjeljku vodič pogledajte generirani Swagger 2.0 metapodatke, a zatim isprobajte test korisničkog Sučelja koji se temelji na Swagger metapodataka.

1. Postavite ToDoListDataAPI projekta (**ne** projekta ToDoListAPI) kao projekt prilikom pokretanja.

    ![Postavljanje ToDoDataAPI kao pokretanje projekta](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Pritisnite F5 ili kliknite **ispravljanje pogrešaka > pokretanje ispravljanje pogrešaka** da biste pokrenuli projekta u načinu rada za ispravljanje pogrešaka.

    Web-pregledniku otvara i prikazuje pogrešku stranicu HTTP 403.

3. Na adresnoj traci preglednika dodavanje `swagger/docs/v1` na kraj retka, a zatim pritisnite Return. (Je URL `http://localhost:45914/swagger/docs/v1`.)

    To je zadani URL koji se koristi Swashbuckle za vraćanje Swagger 2.0 JSON metapodataka na API-JA.

    Ako koristite Internet Explorer, web-pregledniku od vas traži da biste preuzeli datoteke *v1.json* .

    ![Preuzimanje JSON metapodataka u preglednika Internet Explorer](./media/app-service-api-dotnet-get-started/iev1json.png)

    Ako koristite Chrome, Firefox ili rub, web-pregledniku na JSON prikazuje u prozoru preglednika. Različite preglednike rukovati JSON drugačije i prozor preglednika možda neće izgledati isto kao i u primjeru.

    ![JSON metapodataka u pregledniku Chrome](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Sljedeći primjer prikazuje prvi dio Swagger metapodataka za API s definicijom za metodu Get. Ovi metapodaci je što pogoni Swagger korisničkog Sučelja koje koristite u sljedećim koracima pa ga koristiti u nastavku vodiča za automatsko generiranje koda za klijenta.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Zatvorite preglednik i prekinuti ispravljanje pogrešaka u Visual Studio.

5. U projektu ToDoListDataAPI u **Pregledniku rješenja**, otvorite datoteku *App_Start\SwaggerConfig.cs* , a zatim pomaknite se do odjeljka redak 174 i uklonite sljedeći kod.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    Datoteka *SwaggerConfig.cs* stvara se kada instalirate paket Swashbuckle u projektu. Datoteka sadrži brojne načine da biste konfigurirali Swashbuckle.

    Kod koji ste uncommented omogućuje Swagger korisničkog Sučelja koje koristite u sljedećim koracima. Kada stvorite projekta Web API pomoću predloška za projekt API aplikacije, kod je komentar po zadanom kao sigurnosnu mjeru.

6. Ponovno pokrenite projekt.

7. Na adresnoj traci preglednika dodavanje `swagger` na kraj retka, a zatim pritisnite Return. (Je URL `http://localhost:45914/swagger`.)

8. Kada se prikaže stranica Swagger korisničkog Sučelja, kliknite **ToDoList** da biste vidjeli dostupne načine.

    ![Dostupni načini za swagger korisničkog Sučelja](./media/app-service-api-dotnet-get-started/methods.png)

9. Kliknite prvi **se** gumb na popisu.

10. U odjeljku **parametara** unesite zvjezdicu kao vrijednost na `owner` parametar, a zatim **pokušajte ga**.

    Kada dodate provjere autentičnosti u noviji vodičima, srednji sloj pronaći ćete stvarni korisničkog ID-a u sloju podataka. Zasad, svi se zadaci će imati zvjezdica kao njihova vlasnika ID-a tijekom aplikacije bez omogućena provjera autentičnosti.

    ![Swagger korisničkog Sučelja isprobajte sami](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Korisničko Sučelje Swagger poziva ToDoList Get način, i prikazuje šifra odaziva i JSON rezultate.

    ![Swagger korisničkog Sučelja probu rezultata](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Kliknite **Objavi**, a zatim potvrdite okvir ispod **Shemom modela**.

    Okvir za unos možete odrediti vrijednost parametra za metodu objavu kliknete shemom modela prefills. (Ako to ne funkcionira u programu Internet Explorer, koristite neki drugi preglednik ili Ručni unos vrijednosti parametra u sljedećem koraku).  

    ![Swagger korisničkog Sučelja probu objavu](./media/app-service-api-dotnet-get-started/post.png)

12. Promjena JSON u na `todo` unos parametra okvir tako da izgleda kao u sljedećem primjeru ili zamjene teksta opisa:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Kliknite **Isprobajte ga**.

    ToDoList API vraća kod odgovor za HTTP 204 koji označava uspjeh.

14. Kliknite gumb za prvi **se** , a zatim u toj sekciji stranice kliknite gumb **Isprobajte ga** .

    Način odgovor Get sada obuhvaća novi korisnik stavke.

15. Neobavezno: I pokušajte stavi, izbrisati i tako da ID metode.

16. Zatvorite preglednik i prekinuti ispravljanje pogrešaka u Visual Studio.

Swashbuckle funkcionira s bilo kojeg projekta API servisa platforme ASP.NET Web. Ako želite dodati Swagger metapodataka generacije postojeći projekt samo instalirajte paket Swashbuckle.

>[AZURE.NOTE] Metapodaci swagger obuhvaćaju jedinstveni ID za svaku operaciju API-JA. Prema zadanim postavkama Swashbuckle može generirati duplicirane Swagger operacija ID-a za kontroler načinima Web API. To se događa ako upravljaču sadrži preopterećene HTTP načine, kao što su `Get()` i `Get(id)`. Informacije o tome kako rukovati overloads potražite u članku [Prilagodba Swashbuckle generira API definicije](app-service-api-dotnet-swashbuckle-customize.md). Ako stvorite Web API projekta u Visual Studio pomoću predloška Azure API aplikacije, kod koji generira operacija jedinstveni ID-a automatski se dodaju *SwaggerConfig.cs* datoteku.  

## <a id="createapiapp"></a>Stvaranje aplikacije API-JA u Azure i implementacija kod

U ovom ćete odjeljku pomoću Azure alata integrirani u čarobnjaku za Visual Studio **Objavljivanje Web** da biste stvorili novu aplikaciju API-JA u Azure. Zatim uvesti projekt ToDoListDataAPI nove aplikacije API-JA i poziva na API tako da pokrenete Swagger korisničkog Sučelja.

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite ToDoListDataAPI projekta, a zatim **Objavi**.

    ![Kliknite Objavi u Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  U **profilu** koraku čarobnjaka za **Objavljivanje Web** kliknite **Servis za aplikaciju Microsoft Azure**.

    ![Kliknite Azure aplikacije servisa u Objavi Web](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Prijavite se na račun za Azure ako već niste učinili, ili osvježavanje vjerodajnice ako ih niste istekao.

4. U dijaloškom okviru aplikacije servisa odaberite Azure **pretplatu** koju želite koristiti, a zatim **Novo**.

    ![Kliknite Novo u dijaloškom okviru aplikacije servisa](./media/app-service-api-dotnet-get-started/clicknew.png)

    Pojavit će se kartica **Hosting** dijaloškog okvira **Stvaranje aplikacije servisa** .

    Budući da ste implementacija Web API projekta koji ima Swashbuckle instaliran, Visual Studio pretpostavlja želite li stvoriti aplikaciju API-JA. To je naznačeno po naslovu **API naziv aplikacije** i fact padajućeg popisa **Promjena vrste** postavljeno na **Aplikaciju API -JA**.

    ![Vrsta aplikacije u dijaloški okvir aplikacije servisa](./media/app-service-api-dotnet-get-started/apptype.png)

5. Unesite **Naziv aplikacije API** koja je jedinstvena *azurewebsites.net* domene. Možete prihvatiti zadani naziv koji Predloži Visual Studio.

    Ako unesete naziv koji je netko drugi već koristio, vidjet ćete crveni uskličnik udesno.

    Bit će URL aplikaciju API `{API app name}.azurewebsites.net`.

6. U **Grupi resursa** padajući popis, kliknite **Novo**, a zatim unesite "ToDoListGroup" ili neki drugi naziv, po želji.

    Grupa resursa je zbirka Azure resursa, kao što su API aplikacija, baza podataka, VMs itd. Za ovaj vodič, preporučuje se da biste stvorili novu grupu resursa jer je koji olakšava da biste izbrisali u jednom koraku svih Azure resursa koje ste stvorili za vodič.

    Ovaj okvir omogućuje odabir postojeću [grupu resursa](../azure-resource-manager/resource-group-overview.md) ili stvorite novi upisivanjem u naziv koji se razlikuje od bilo kojeg postojeću grupu resursa u svoju pretplatu.

7. Kliknite gumb **Novo** uz **Aplikaciju servisa planiranje** padajući popis.

    Snimka zaslona prikazuje ogledne vrijednosti za **API naziv aplikacije**, **pretplate**i **Grupa resursa** – razlikovat će se vrijednosti.

    ![Dijaloški okvir aplikacije servisa za stvaranje](./media/app-service-api-dotnet-get-started/createas.png)

    U sljedećim koracima stvorite tarifu aplikacije servisa za novu grupu resursa. Tarifu aplikacije servisa za određuje računalnim resursi koji se izvodi aplikacije API-JA na. Na primjer, ako odaberete besplatne sloju, aplikacije API pokreće zajedničke VMs tijekom za neke plaćenu razine ga na namjenski VMs. Informacije o tarifama za aplikacije servisa za potražite u članku [Pregled aplikacije servisa za tarife](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

8. U dijaloškom okviru **Konfiguriranje planiranje usluga aplikacije** unesite "ToDoListPlan" ili neki drugi naziv po želji.

9. Na padajućem popisu **mjesto** odaberite mjesto na koji je najsličniji vama.

    Tom se postavkom određuje koje Azure podatkovnog centra aplikacije funkcionirat će u. Odaberite mjesto blizu minimiziranje [Latencija](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

10. U **Veličina** padajući popis, kliknite **besplatno**.

    Za ovaj vodič besplatne cijene sloju dat će dovoljno dobre performanse.

11. U dijaloškom okviru **Konfiguriranje planiranje usluga aplikacije** kliknite **u redu**.

    ![Kliknite u redu u Konfiguriranje aplikacije servisa za planiranje](./media/app-service-api-dotnet-get-started/configasp.png)

12. U dijaloškom okviru **Stvaranje aplikacije servisa** kliknite **Stvori**.

    ![Kliknite Stvori u dijaloškom okviru Stvaranje aplikacije servisa](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio stvara aplikaciju API-JA i objavljivanje profil koji sadrži sve potrebne postavke za aplikaciju API-JA. Zatim otvara čarobnjak za **Objavljivanje Web** koji će se koristiti za implementaciju projekta.

    Otvorit će se čarobnjak za **Objavljivanje Web** na kartici **veza** (prikazano u nastavku).

    Na kartici **veza** postavke **poslužitelja** i **naziv web-mjesta** pokažite na aplikaciju API-JA. **Korisničko ime** i **Lozinka** su vjerodajnice za implementaciju Azure stvara umjesto vas. Nakon implementacije, Visual Studio otvara preglednika na **Odredišni URL** (to je jedini svrhu **Odredišni URL**).  

13. Kliknite **Dalje**.

    ![Kliknite Dalje na kartici veze objavljivanje web-mjesta](./media/app-service-api-dotnet-get-started/connnext.png)

    Na sljedeću karticu je kartica **Postavke** (prikazano u nastavku). Ovdje možete promijeniti karticu konfiguracija Sastavi za implementaciju Sastavi ispravljanje pogrešaka za [daljinsko uklanjanje programskih pogrešaka](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Na kartici nudi nekoliko **Mogućnosti objavljivanja datoteke**:

    * Uklanjanje dodatne datoteke na odredištu
    * Precompile tijekom objavljivanja
    * Isključi datoteke iz mape App_Data

    Za ovaj vodič nije potrebno nešto od sljedećeg. Detaljna objašnjenja čemu služe, potražite u članku [Kako: implementacija web Web projekt pomoću jednim klikom objaviti u Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).

14. Kliknite **Dalje**.

    ![Kliknite Dalje na kartici postavke objavljivanje web-mjesta](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Sljedeći je **Pretpregled** karticu (prikazan dolje), koji daje priliku da biste vidjeli koje datoteke namjeravate kopirat s projektom aplikaciju API-JA. Kada ste implementacije projekta za API aplikacije koje ste već implementirano u prethodnom, kopiraju se samo promijenjene datoteke. Ako želite da biste vidjeli popis što će se kopirati, možete kliknuti gumb **Start pretpregled** .

15. Kliknite **Objavi**.

    ![Kliknite Objavi u Pretpregled na kartici objavljivanje web-mjesta](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio uvodi ToDoListDataAPI projekta za novu aplikaciju API-JA. U **izlaznom** prozoru zapisnike uspješno uvođenje i stranicu "uspješno stvorili" pojavljuje se u prozoru preglednika otvoriti URL aplikaciju API-JA.

    ![Izlaz prozor uspješno uvođenje](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Nova stranica aplikacije uspješno stvorili API-JA](./media/app-service-api-dotnet-get-started/appcreated.png)

16. Dodajte "swagger" URL u adresnoj traci web-preglednika, a zatim pritisnite tipku Enter. (Je URL `http://{apiappname}.azurewebsites.net/swagger`.)

    Web-pregledniku prikazuje iste Swagger korisničkog Sučelja koje vidjeli ranije, ali sada je pokrenuta u oblaku. Isprobajte metodu Get i vidjeti da ste natrag na zadani 2 obaveza. Promjene unesene u prethodno spremljene u memorije na lokalnom računalu.

17. Otvorite [portal za Azure](https://portal.azure.com/).

    Portal za Azure je web-sučelja za upravljanje Azure resurse kao što je aplikacija API-JA.

18. Kliknite **dodatnih servisa > aplikacije servisa**.

    ![Pronađite aplikaciju servisa](./media/app-service-api-dotnet-get-started/browseas.png)

19. U plohu **Aplikacije usluge** pronađite i kliknite novu aplikciju API-JA. (Na portalu Azure windows koje otvorite udesno nazivaju *blades*.)

    ![Aplikacije servisa plohu](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Dva blades otvorite. Jedan plohu sadrži pregled aplikaciju API-JA, a jedan ima dugačak popis možete pregledati i promijeniti postavke.

20. U plohu **Postavke** pronađite odjeljak **API-JA** , a zatim kliknite **Definicija API -JA**.

    ![Definicija API-JA u postavkama plohu](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    **Definicija API** plohu omogućuje određivanje URL-a koji vraća Swagger 2.0 metapodataka u JSON OSNOVNI oblik. Kada Visual Studio stvara aplikaciju API-JA, postavlja URL definicija API-JA na zadane vrijednosti za Swashbuckle generira metapodatke koje ste vidjeli ranije, što je aplikacija API je osnovni URL plus `/swagger/docs/v1`.

    ![URL definicija API-JA](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Kada odaberete aplikacije API-JA za generiranje koda za klijenta za nju, Visual Studio dohvaća metapodatke iz ovog URL-a.

## <a id="codegen"></a>Generiranje koda za klijenta za sloju podataka

Jedna od prednosti integracije Swagger u Azure API aplikacije je kod za automatsko generiranje. Klase generira klijent olakšavaju napišite kod koji se poziva aplikaciju API-JA.

ToDoListAPI projekt već ima kod generira klijent, ali u sljedećim koracima ćete ga izbrisati i Obnovi da biste vidjeli kako generiranje koda.

1. U Visual Studio **Preglednik rješenja**u programu project ToDoListAPI izbrišite mapu *ToDoListDataAPI* . **Oprez: Izbrisati samo mapu, a ne ToDoListDataAPI projekta.**

    ![Brisanje generira klijent kod](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Ova mapa je stvorena pomoću postupka generacije kod koji ćete proći kroz.

2. Desnom tipkom miša kliknite ToDoListAPI projekta, a zatim kliknite **Dodaj > REST API klijent**.

    ![Dodavanje REST API-JA klijenta u Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. U dijaloškom okviru **Dodavanje REST API klijent** kliknite **Swagger URL**, a zatim **Odaberite Azure resursa**.

    ![Odaberite Azure resursa](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. U dijaloškom okviru **Aplikacije servisa za** proširivanje grupe resursa koristite za ovog praktičnog vodiča i odaberite aplikaciju API-JA, a zatim **u redu**.

    ![Odaberite aplikaciju API-JA za generiranje koda](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Obratite pozornost na to da kada se vratite u dijaloški okvir **Dodavanje REST API klijent** , tekstni okvir popunjeno pomoću definiciju API URL vrijednost koja se prikazivalo ranije na portalu.

    ![URL definicija API-JA](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Drugi način da biste dobili metapodataka za generiranje koda je unesite URL izravno umjesto prolaze kroz dijaloški okvir Pregledaj. Ili ako želite generiranje koda za klijenta prije no što implementirate Azure, nije moguće projekta Web API izvodi lokalno, idite na URL koji omogućuje Swagger JSON datoteku spremite datoteku, a pomoću mogućnosti **Odaberite postojeću datoteku Swagger metapodataka** .

5. U dijaloškom okviru **Dodavanje REST API klijent** , kliknite **u redu**.

    Visual Studio stvara mapu pod nazivom nakon aplikaciju API-JA i generira klijent klase.

    ![Kod datoteka za generirani klijenta](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. U programu project ToDoListAPI otvorite *Controllers\ToDoListController.cs* da biste vidjeli šifru u retku 40 poziva na API-JA pomoću generirani klijenta.

    Sljedeći isječak prikazuje način na koji kod instancira klijentskog objekta i poziva metodu Get.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Parametar Graditelj dobiva URL krajnje točke na `toDoListDataAPIURL` postavke aplikacije. U datoteci Web.config tu vrijednost je postavljena na Express URL lokalne IIS API projekta, tako da možete pokrenuti aplikaciju lokalno. Ako ispustite parametar Graditelj zadana krajnja točka jest URL koji je generirao koda iz.

7. Svojoj učionici klijenta bit će načinjena pod drugim nazivom na temelju naziva aplikacije API; Promijenite kod u *Controllers\ToDoListController.cs* tako da odgovara nazivu vrste što je generirana u projektu. Na primjer, ako pod nazivom vaše ToDoListDataAPI071316 aplikacije API promijeniti kod:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

Da biste to:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Stvaranje aplikacije za API-JA za hostiranje Srednji sloj

Starije možete [stvoriti aplikaciju za API sloju podataka i implementirati kod da biste ga](#createapiapp).  Sada slijedite isti postupak za aplikaciju Srednji sloj API-JA.

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite Srednji sloj ToDoListAPI projekta (ne u podataka sloju ToDoListDataAPI), a zatim kliknite **Objavi**.

    ![Kliknite Objavi u Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Na kartici **profil** Čarobnjak za **Objavljivanje Web** kliknite **Servis za aplikaciju Microsoft Azure**.

3. U dijaloškom okviru **Aplikacije servisa** kliknite **Novo**.

4. Na kartici **Hosting** dijaloškog okvira **Stvaranje aplikacije servisa** prihvatite zadani **Naziv aplikacije API -JA** ili unesite naziv koji je jedinstven *azurewebsites.net* domene.

5. Odaberite Azure **pretplatu** koju koristite.

6. U **Grupi resursa** padajućem popisu, odaberite istoj grupi resursa koji ste prethodno stvorili.

7. **Plan usluge za aplikaciju** padajućem popisu, odaberite istu tarifu koju ste ranije stvorili. Zadana će s tom vrijednošću.

8. Kliknite **Stvori**.

    Visual Studio stvara aplikaciju API-JA, stvara Objavi profil za njega i prikazuje **vezu** korak čarobnjaka za **Objavljivanje Web** .

9.  U koraku čarobnjaka za **Objavljivanje Web** **veze** , kliknite **Objavi**.

    Visual Studio uvodi ToDoListAPI projekta za novu aplikaciju API-JA i otvara preglednika na URL aplikaciju API-JA. Pojavljuje se "uspješno stvorili" stranica.

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>Konfiguriranje Srednji sloj da biste nazvali sloju podataka

Ako sada zove aplikaciju Srednji sloj API pokušajte želite nazvati sloju podataka pomoću localhost URL koji se još nalazi u datoteci Web.config. U ovom odjeljku unesite URL za aplikaciju programa podataka sloju API-JA u u okruženju postavka u aplikaciji API Srednji sloj. Kada kod u aplikaciji Srednji sloj API dohvaća URL postavku razina podataka, nadjačat će postavku okruženje što je u datoteci Web.config.

1. Idite na [portal za Azure](https://portal.azure.com/), a zatim pronađite **Aplikaciju API** plohu API aplikacije koji ste stvorili za vođenje projekta TodoListAPI (Srednji sloj).

2. U aplikaciji API plohu **Postavke** , kliknite **Postavke aplikacije**.

3. U aplikaciji API plohu **Postavke aplikacije** , pomaknite se prema dolje do odjeljka **postavki aplikacije** i dodajte sljedeće ključ i vrijednost. Vrijednost će biti URL aplikaciju API prvi ste objavili ovog praktičnog vodiča.

  	| **Ključ** | toDoListDataAPIURL |
  	|---|---|
  	| **Vrijednost** | .azurewebsites https://{your podataka sloju API naziv aplikacije} .net |
  	| **Primjer** | https://todolistdataapi.azurewebsites.NET |

4. Kliknite **Spremi**.

    ![Kliknite Spremi za postavki aplikacije](./media/app-service-api-dotnet-get-started/asinportal.png)

    Kada kod pokreće se u Azure, tu vrijednost nadjačat će sada localhost URL-a koji se nalazi u datoteci Web.config.

## <a name="test"></a>Test

1. U prozoru preglednika, otvorite URL nove aplikacije API Srednji sloj koji ste upravo stvorili za ToDoListAPI. Možete dobiti postoji tako da kliknete URL u glavnom plohu aplikaciju API-JA na portalu.

2. Dodajte "swagger" URL u adresnoj traci web-preglednika, a zatim pritisnite tipku Enter. (Je URL `http://{apiappname}.azurewebsites.net/swagger`.)

    Web-pregledniku prikazuje u istu Swagger korisničkog Sučelja koje ste vidjeli ranije za ToDoListDataAPI, ali sada `owner` nije obavezno polje za operaciju Get jer aplikaciju Srednji sloj API šalje tu vrijednost aplikaciju za API sloju podatke umjesto vas. (Nakon vodiči za provjeru autentičnosti, srednji sloj poslat će stvarni korisnički ID-a na `owner` parametar; za sada ga je kodiranje zvjezdicu.)

3. Isprobajte metodu Get i druge načine za provjeru valjanosti da aplikaciju API Srednji sloj je uspješno pozivanje aplikaciju za API sloju podataka.

    ![Swagger način dobiti korisničkog Sučelja](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Ako naiđete na problem kao proći kroz ovaj praktični su nekoliko savjeta za otklanjanje poteškoća:

* Provjerite koristite li najnoviju verziju programa [Azure SDK za .NET](http://go.microsoft.com/fwlink/?linkid=518003).

* Dva projekta imena su slične (ToDoListAPI, ToDoListDataAPI). Što ne izgledaju kao što je opisano u uputama kada radite s projektom, provjerite je li otvorene točan projekta.

* Ako se na mreži tvrtke i pokušavate implementacija aplikacije servisa za Azure kroz vatrozid, provjerite je li priključke 443 i 8172 otvoreno za implementaciju Web. Ako ne možete otvoriti te priključke, možete koristiti druge metode implementacije.  Potražite u članku [Implementacija aplikacije za aplikacije servisa za Azure](../app-service-web/web-sites-deploy.md).

* "Usmjeravanje imena mora biti jedinstvena" pogreške – ne može dobit te ako slučajno pogrešan projekta implementirati aplikaciju API-JA, a kasnije implementacija onaj s njim. Da biste to ispravili, redeploy točan projekta aplikaciju API-JA, a zatim na kartici **Postavke** čarobnjaka za **Objavljivanje Web** odaberite **Ukloni dodatne datoteke na odredištu**.

Nakon što ste ASP.NET API aplikaciju u aplikacije servisa za Azure, preporučujemo vam da biste saznali više o značajkama programa Visual Studio koje Pojednostavnite otklanjanje poteškoća. Informacije o zapisivanje daljinsko uklanjanje programskih pogrešaka i drugo, potražite u članku [Otklanjanje poteškoća Azure aplikacije servisa za aplikacije u Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Daljnji koraci

Kako implementirati postojeće projekte Web API aplikacije API, generiranje koda za klijenta za API aplikacije i zauzeti API aplikacije klijenata .NET ste vidjeti. Na sljedeći Praktični vodič u ovoj seriji pokazuje kako [koristiti CORS za korištenje aplikacije API klijenata JavaScript](app-service-api-cors-consume-javascript.md).

Dodatne informacije o klijentskim kod generacije potražite u članku [Azure/AutoRest](https://github.com/azure/autorest) spremište na GitHub.com. Pomoć za probleme sa pomoću klijentskog programa generirani, otvorite [problem u spremištu AutoRest](https://github.com/azure/autorest/issues).

Ako želite da biste stvorili novih projekata aplikacije API ispočetka, pomoću predloška **Azure API aplikacije** .

![Predložak za aplikaciju API-JA u Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Predložak projekta **Azure API aplikacije** je odabir u **prazan** ASP.NET 4.5.2 predložak, kliknite potvrdni okvir da biste dodali podršku Web API i instalirate paket Swashbuckle NuGet. Osim toga, predložak dodaje neke koda za konfiguraciju Swashbuckle namijenjenu onemogućuju stvaranje duplikata Swagger operacija ID-a. Nakon što stvorite projekta programa API aplikacije, možete njegove implementacije kod aplikaciju API-JA na isti način kao i se prikazivalo u ovom ćete praktičnom vodiču.
