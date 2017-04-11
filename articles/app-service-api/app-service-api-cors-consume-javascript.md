<properties
    pageTitle="CORS podržava u aplikacije servisa za | Microsoft Azure"
    description="Saznajte kako koristiti CORS podrška u aplikacije servisa za Azure Azure."
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
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Korištenje aplikacije API iz JavaScript pomoću CORS

Aplikacije servisa za nudi ugrađenu podršku za [Više polazište resursa zajedničko korištenje (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), koji omogućuje JavaScript klijenti mogli upućivati pozive domenama na API-ji koji se nalaze u aplikacijama za API-JA. Aplikacije servisa možete konfigurirati CORS pristup vašem API bez pisanja kod u vašem API-JA.

U ovom se članku sastoji se od dva dijela:

* U odjeljku [Konfiguriranje CORS](#corsconfig) Općenito objašnjava kako konfigurirati CORS za bilo koju aplikaciju API-JA, web-aplikacije ili mobilne aplikacije. Jednako primjenjuje na sve okviri koje podržava aplikacije servisa, uključujući .NET, Node.js i Java. 

* Počevši od odjeljku [nastavka vodiči za .NET početak rada](#tutorialstart) , u članku je vodič koji pokazuje CORS podršku tako da na radnje u [prvom aplikacije API početak rada vodič](app-service-api-dotnet-get-started.md). 

## <a id="corsconfig"></a>Konfiguriranje CORS u aplikacije servisa za Azure

CORS možete konfigurirati na portalu za Azure ili pomoću alata za [Upravljanje resursima Azure](../azure-resource-manager/resource-group-overview.md) .

#### <a name="configure-cors-in-the-azure-portal"></a>Konfiguriranje CORS na portalu za Azure

8. U pregledniku idite na [portal za Azure](https://portal.azure.com/).

2. Kliknite **Aplikacije servisa**, a zatim kliknite naziv aplikacije API.

    ![Odaberite aplikaciju API-JA na portalu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. U **Postavke** plohu koji će se otvoriti s desne strane plohu **API aplikacije** pronađite odjeljak **API-JA** , a zatim **CORS**.

    ![Odaberite CORS plohu postavke](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. U okvir tekst okvir unesite URL ili URL koji želite omogućiti JavaScript pozive dolazi iz.


    Ako, na primjer, ako implementiran JavaScript aplikaciji web App pod nazivom todolistangular, unesite "https://todolistangular.azurewebsites.net". Umjesto toga, možete unijeti zvjezdicu (*) da biste odredili sve polazište domene prihvaćaju.


13. Kliknite **Spremi**.

    ![Kliknite Spremi](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Nakon što kliknete **Spremi**, aplikaciju API prihvaća JavaScript pozive iz određenih URL-ova.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Konfiguriranje CORS pomoću alata za upravljanje resursima za Azure

Možete konfigurirati i CORS API aplikacije pomoću [predložaka Azure upravljanja resursima](../resource-group-authoring-templates.md) u alatima za naredbenog retka kao što su [Azure PowerShell](../powershell-install-configure.md) i [Azure EŽA](../xplat-cli-install.md). 

Na primjer Voditelj resursa Azure predloška koji postavlja svojstva CORS, otvorite [datoteku azuredeploy.json u spremištu ovog praktičnog vodiča uzorka aplikacije](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Pronađite odjeljak predloška koji izgleda kao u sljedećem primjeru:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Nastavite vodič za početak rada .NET

Ako pratite Node.js ili Java početak rada niz za API aplikacije, dovršite dohvaćanje niza rada. Prijeđite na odjeljak [sljedeće korake](#next-steps) da biste pronašli prijedloge za daljnje učenje o aplikacijama API-JA.

Ostatak ovog članka je nastavka niza .NET početak rada i pretpostavlja uspješno dovršena [prvom praktičnom vodiču](app-service-api-dotnet-get-started.md).

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Implementacija projekta ToDoListAngular novu web-aplikaciju

U [prvom praktičnom vodiču](app-service-api-dotnet-get-started.md)ste stvorili u srednjem sloju API aplikacije i u podataka sloju API-JA. Pomoću ovog praktičnog vodiča stvaranja web-aplikacijama jednu stranicu aplikacija (SPA) te pozive API Srednji sloj aplikacije. SPA rad koju ste da biste omogućili CORS na aplikaciju Srednji sloj API-JA. 

U [ToDoList primjer aplikacije](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)projekta ToDoListAngular je jednostavno klijent AngularJS koja poziva ToDoListAPI Web API projekta Srednji sloj. JavaScript kod u datoteci *app/scripts/todoListSvc.js* poziva na API-JA pomoću davatelja AngularJS HTTP. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Stvaranje nove web app za ToDoListAngular project

Postupak da biste stvorili novu aplikacije servisa za web-aplikaciju i implementacija projekta je slično kao što se prikazivalo za [Stvaranje i implementacija aplikacije API-JA u prvom praktičnom vodiču u ovoj seriji](app-service-api-dotnet-get-started.md#createapiapp). Jedina razlika jest da je vrstu aplikacije **Web-aplikacije** umjesto **Aplikaciji API -JA**.  Snimke zaslona u dijaloškim okvirima, potražite u članku 

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite ToDoListAngular projekta, a zatim **Objavi**.

3.  Na kartici **profil** Čarobnjak za **Objavljivanje Web** kliknite **Servis za aplikaciju Microsoft Azure**.

5. U dijaloškom okviru **Aplikacije servisa** kliknite **Novo**.

3. Na kartici **Hosting** dijaloškog okvira **Stvaranje aplikacije servisa** unesite **Naziv aplikacije na web-mjesta** koja je jedinstvena *azurewebsites.net* domene. 

5. Odaberite Azure **pretplatu** koju želite raditi.

6. Na padajućem popisu **Grupa resursa** odaberite istoj grupi resursa koji ste prethodno stvorili.

4. Na padajućem popisu **Plan usluge za aplikaciju** odaberite istu tarifu koju ste ranije stvorili. 

7. Kliknite **Stvori**.

    Visual Studio stvara web-aplikaciju, stvara Objavi profil za njega i prikazuje **vezu** korak čarobnjaka za **Objavljivanje Web** .

    Nemojte još kliknuti **Objavi** . U sljedećem odjeljku, konfigurirati novu web-aplikaciju da biste nazvali aplikaciju API Srednji sloj koja se izvodi u aplikacije servisa. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Postavljanje Srednji sloj URL-a u web-aplikacije postavke

1. Idite na [portal za Azure](https://portal.azure.com/), a zatim dođite do **Web-aplikacije** plohu za web-aplikacije koje ste stvorili za vođenje projekta TodoListAngular (sučelje).

2. Kliknite **Postavke > postavke aplikacije**.

3. U odjeljku **postavke za aplikaciju** , dodajte sljedeći ključ i vrijednost:

  	|Ključ|Vrijednost|Primjer
  	|---|---|---|
  	|toDoListAPIURL|Naziv aplikacije Srednji sloj API https://{your} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Kliknite **Spremi**.

    Kada kod pokreće se u Azure, tu vrijednost nadjačava localhost URL-a koji se nalazi u datoteci *Web.config* . 

    Kod koji se dobiva vrijednost postavke nalazi se u *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    Kod u *todoListSvc.js* koristi postavku:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Implementacija web projekta ToDoListAngular novu web-aplikaciju

*  U Visual Studio u koraku čarobnjaka za **Objavljivanje Web** **veze** kliknite **Objavi**.

    Visual Studio uvodi ToDoListAngular projekta za novu web-aplikaciju i otvara preglednika na URL web-aplikacije. 

### <a name="test-the-application-without-cors-enabled"></a>Testiranje aplikacije bez CORS omogućeno 

2. U pregledniku alate za razvojne inženjere, otvorite prozor konzole.

3. U prozoru preglednika koji se prikazuje korisničko Sučelje AngularJS, kliknite vezu **Do popisa** .

    Pokušava JavaScript kod da biste nazvali Srednji sloj API aplikacije, ali poziv neće uspjeti jer je sučelje izvodi u drugu domenu od pozadinska. Prozora konzole za alate za razvojne inženjere web-preglednika prikazuje se poruka pogreške unakrsno polazište.

    ![Poruka o pogrešci više izvora](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Konfiguriranje CORS za aplikaciju Srednji sloj API-JA

U ovom ćete odjeljku Konfiguriranje postavka CORS u Azure Srednji sloj ToDoListAPI API aplikacije. Ta postavka omogućuje Srednji sloj API aplikacije primanje poziva JavaScript iz web-aplikacije koje ste stvorili za projekt ToDoListAngular.

8. U pregledniku idite na [portal za Azure](https://portal.azure.com/).

2. Kliknite **Aplikaciju servisa**, a zatim aplikaciju API ToDoListAPI (Srednji sloj).

    ![Odaberite aplikaciju API-JA na portalu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. U **Postavke** plohu koji će se otvoriti s desne strane plohu **API aplikacije** pronađite odjeljak **API-JA** , a zatim **CORS**.

    ![Odaberite CORS portalu](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. U tekstni okvir, unesite URL za web-aplikaciji ToDoListAngular (sučelje). Ako, na primjer, ako projekt ToDoListAngular implementiran na web-aplikacijama pod nazivom todolistangular0121, Dopusti pozive iz URL `https://todolistangular0121.azurewebsites.net`.

    Umjesto toga, možete unijeti zvjezdicu (*) da biste odredili sve polazište domene prihvaćaju.

13. Kliknite **Spremi**.

    ![Kliknite Spremi](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Nakon što kliknete **Spremi**, aplikaciju API prihvaća JavaScript pozive iz navedenom URL-a. U ovom snimka zaslona aplikacije ToDoListAPI0223 API prihvaća JavaScript klijent pozive iz web-aplikacije ToDoListAngular.

### <a name="test-the-application-with-cors-enabled"></a>Testiranje aplikacije s CORS omogućeno

* Otvorite preglednik HTTPS URL web-aplikacije. 

    Ovaj put aplikacija omogućuje prikaz, dodavanje, uređivanje i brisanje stavki s popisa obaveza. 

    ![Da biste dobili popis stranica aplikacije uzorka](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Aplikacije servisa za CORS nasuprot CORS API na webu

U projektu API na webu, možete instalirati paket [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet da biste odredili u kodu koji domene vaše API prihvaća JavaScript poziva s.
 
Podrška za API CORS web je fleksibilnije od CORS aplikacije usluge podrške. Ako, na primjer, u kod možete odrediti različite prihvaćenom drugačijeg izvora za različite načine, dok za aplikaciju servisa CORS navedete jedan skup prihvaćenom drugačijeg izvora za sve načine aplikaciju API-JA.

> [AZURE.NOTE] Ne pokušajte koristiti Web API CORS i CORS servisa aplikacija u aplikaciji jedan API-JA. Aplikacije servisa za CORS će prednost i Web API CORS neće imati utjecaja. Na primjer, Ako omogućite jednu domenu izvor u aplikacije servisa za, a Omogući sve polazište domene u kodu Web API aplikacije Azure API samo prihvaćanje poziva s domenom koju ste naveli u Azure.

### <a name="how-to-enable-cors-in-web-api-code"></a>Kako omogućiti CORS u kodu API na webu

Sljedeći koraci sažetak postupak za omogućivanje podrške CORS API na webu. Dodatne informacije potražite u članku [Omogućivanje zahtjeva unakrsno polazište u ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. U projektu Web API, instalirajte paket NuGet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) .

1. Uključiti u `config.EnableCors()` redak koda u način **registrirati** klase **WebApiConfig** , kao u sljedećem primjeru. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. U upravljaču Web API dodajte na `using` izjava za na `System.Web.Http.Cors` prostor naziva, i dodajte u `EnableCors` atribut klase kontroler ili pojedinačne akcije metoda. U sljedećem primjeru CORS podršku primjenjuje se na cijelu kontroler.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Upravljanje Azure API pomoću aplikacije API-JA

Ako koristite upravljanje API Azure s API aplikacije, konfigurirati CORS u odjeljku Upravljanje API umjesto u aplikaciji API-JA. Dodatne informacije potražite u sljedećim resursima:

* [Azure pregled upravljanja API-JA (videozapis: CORS započinje 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Upravljanje API unakrsni pravila domene](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Otklanjanje poteškoća

Ako naiđete na problem prilikom prolaska kroz ovaj Praktični vodič, Evo nekoliko savjeta za otklanjanje poteškoća.

* Provjerite koristite li najnoviju verziju programa [Azure SDK za .NET za Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).

* Provjerite jeste li unijeli `https` u postavci CORS i provjerite koristite li `https` da biste pokrenuli sučelja web-aplikaciji.

* Provjerite jeste li unijeli postavku CORS u aplikaciji API Srednji sloj, a ne u sučelja web-aplikaciji.

* Ako ste konfiguriranja CORS u kod aplikacije i aplikacije servisa za Azure, imajte na umu da postavku aplikacije servisa CORS nadjačat će sve što radite u kodu aplikacije. 

Da biste saznali više o značajkama programa Visual Studio koje Pojednostavnite otklanjanje poteškoća, potražite u članku [Otklanjanje poteškoća Azure aplikacije servisa za aplikacije u Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Daljnji koraci 

U ovom se članku vidjeli kako omogućiti aplikacije servisa CORS podršku tako da se klijent JavaScript kod da biste uputili poziv API u drugu domenu. Dodatne informacije o aplikacijama API pročitajte [Uvod u provjere autentičnosti u aplikacije servisa za](../app-service/app-service-authentication-overview.md), a otvorite vodič za [provjeru autentičnosti korisnika za API aplikacije](app-service-api-dotnet-user-principal-auth.md) .
