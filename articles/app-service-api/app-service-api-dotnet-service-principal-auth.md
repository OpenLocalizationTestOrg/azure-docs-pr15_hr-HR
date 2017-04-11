<properties
    pageTitle="Glavni provjera autentičnosti servisa za API aplikacije u aplikacije servisa za Azure | Microsoft Azure"
    description="Saznajte kako se zaštititi aplikaciju API-JA u aplikacije servisa za Azure scenarije servisa za servis."
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
    ms.topic="article"
    ms.date="06/30/2016" 
    ms.author="rachelap"/>

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Glavni provjera autentičnosti servisa za API aplikacije u aplikacije servisa za Azure

## <a name="overview"></a>Pregled

U ovom se članku objašnjava kako pomoću provjere autentičnosti aplikacije servisa za *interne* pristup aplikacijama API-JA. Interna scenarij je kojoj imate aplikaciju API koja želite da budu potrošni samo prema kodu aplikacije. Preporučeni način u aplikacije servisa za implementaciju scenarij je koristiti Azure AD zaštititi aplikaciju pod nazivom API-JA. Nazovite zaštićeni API aplikaciju pomoću nošenja token koji ste dobili od Azure AD unosom identitet aplikacije vjerodajnice (glavnica servisa). Alternative za korištenje Azure AD, potražite u članku u odjeljku **Provjera autentičnosti servisa servisa** [Azure aplikacije servisa za provjeru autentičnosti pregled](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

U ovom se članku ćete saznati:

* Kako koristiti Azure Active Directory (Azure AD) da biste zaštitili API aplikacije iz neovlašten pristup.
* Kako se zauzeti zaštićeni aplikacije API API aplikacije, web-aplikacije ili mobilne aplikacije pomoću vjerodajnica za Azure AD servisa glavnicu (aplikacija identitet). Informacije o načinu korištenja iz aplikacije logike potražite potražite u članku [Korištenje prilagođenog API-JA hostirane na aplikacije servisa s aplikacijama logike](../app-service-logic/app-service-logic-custom-hosted-api.md).
* Kako da biste bili sigurni da aplikaciju zaštićeni API-JA ne može pozvati iz preglednika prijavljeni korisnici.
* Kako da biste bili sigurni da aplikaciju zaštićeni API-JA možete pozvati samo pomoću određenih Azure AD servisa glavnicu.

U članku sastoji se od dva dijela:

* U odjeljku [Konfiguriranje servisa glavni provjere autentičnosti u aplikacije servisa za Azure](#authconfig) Općenito objašnjava konfiguriranje provjere autentičnosti za bilo koju aplikaciju za API i zauzeti aplikaciju zaštićeni API-JA. U ovom se odjeljku jednoliko primjenjuje se na sve okviri podržava aplikacije servisa, uključujući .NET, Node.js i Java.

* Počevši od odjeljku [nastavka vodiči za .NET početak rada](#tutorialstart) , vodič će vas voditi kroz konfiguriranje programa scenarij "interni pristupa" za aplikaciju uzorka .NET izvodi u aplikacije servisa. 

## <a id="authconfig"></a>Konfiguriranje provjere autentičnosti na glavni servisa u aplikacije servisa za Azure

U ovom se odjeljku nalaze općenite upute koje se odnose na bilo koju aplikaciju za API-JA. Korake određene za primjer aplikacije da biste učinili .NET popisa, otvorite [nastavka seriji vodiča .NET API aplikacije](#tutorialstart).

1. [Portal za Azure](https://portal.azure.com/)idite na **Postavke** plohu API aplikacije koju želite zaštititi, a zatim pronađite odjeljak **značajke** i kliknite **provjere autentičnosti / autorizacije**.

    ![Provjera autentičnosti/autorizacije Azure portalu](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. U na **provjere autentičnosti / autorizacije** plohu, **kliknite**.

4. Na padajućem popisu **poduzeti kada zahtjev je autentičnost provjerena** odaberite **prijavite se pomoću servisa Azure Active Directory** .

5. U odjeljku **Davatelja usluge provjere autentičnosti**, odaberite **Azure Active Directory**.

    ![Provjera autentičnosti/autorizacije plohu Azure portalu](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Konfiguriranje plohu **Azure Active Directory postavke** da biste stvorili novu aplikaciju za Azure AD ili korištenje postojeće aplikacije Azure AD ako već imate onaj koji želite koristiti.

    Scenariji za interne obično obuhvaćaju aplikaciju API pozivanje aplikaciju API-JA. Možete koristiti odvojene Azure AD aplikacije za svaku aplikaciju API-JA ili samo jedan Azure AD aplikacije.

    Detaljne upute o ovom plohu potražite u članku [upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje prijava Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

7. Kada završite s plohu za konfiguriranje davatelja usluge provjere autentičnosti, kliknite **u redu**.

7. U na **provjere autentičnosti / autorizacije** plohu, kliknite **Spremi**.

    ![Kliknite Spremi](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Kada to učinite, aplikacije servisa za samo omogućuje zahtjeve iz pozivatelji u u konfigurirano Azure AD klijenta. Nema koda za provjeru autentičnosti ili autorizacije potreban je u aplikaciji zaštićeni API-JA. Token nošenja prosljeđuje aplikaciju API zajedno s često korištenih zahtjevima u HTTP zaglavlja, a može čitati te podatke u kodu da biste provjerili valjanost zahtjevi se s određenom pozivatelja, kao što su glavni servisa.

Ta je funkcija provjere autentičnosti funkcionira na isti način za sve jezike koje podržava aplikacije servisa, uključujući .NET, Node.js i Java. 

#### <a name="how-to-consume-the-protected-api-app"></a>Upute za aplikaciju zaštićeni API-JA

Pozivatelj mora pružati uslugu programa Azure AD nošenja token s API poziva. Da biste dobili nošenja token pomoću servisa glavni vjerodajnica pozivatelj koristi provjeru autentičnosti biblioteke imenika Active Directory (ADAL za [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)ili [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Da biste dobili token, kod koji poziva ADAL omogućuje ADAL sljedeće informacije:

* Naziv Azure AD klijentu.
* ID klijenta i tajna klijenta (aplikacija ključ) Azure AD aplikacije koja je povezana s pozivateljem.
* ID klijenta programa Azure AD povezan s zaštićenog aplikacijom API-JA. (Ako se koristi samo jednu aplikaciju za Azure AD, to je isti ID klijenta kao jedan za pozivatelja.)

Ove vrijednosti su dostupne na stranicama Azure AD [Azure klasični portal](https://manage.windowsazure.com/).

Kada je omogućeno dobivene token, Pozivatelj je obuhvaća HTTP zahtjevima u zaglavlju autorizacije.  Aplikacije servisa za Provjeri valjanost token i omogućuje zahtjeve dosegne aplikaciju zaštićeni API-JA.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Kako zaštititi aplikaciju API iz programa access tako da korisnici u istom klijentu

Tokeni nošenja za korisnike u istom klijentu smatraju vrijedi za aplikaciju zaštićeni API-JA.  Ako želite biti sigurni samo glavni servis možete nazvati aplikaciju zaštićeni API-JA, dodati kod u aplikaciji zaštićeni API-JA za provjeru valjanosti sljedeće zahtjevima iz tokena:

* `appid`mora biti ID klijenta programa Azure AD koja je povezana s pozivateljem. 
* `oid`(`objectidentifier`) mora biti servis glavni ID pozivatelja. 

Aplikacije servisa za se nalaze na `objectidentifier` zahtjeva u zaglavlju X-MS--GLAVNICU-ID KLIJENTA.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Kako zaštititi aplikaciju API iz programa access u pregledniku

Ako ne provjeru zahtjevima u kodu u aplikaciji zaštićeni API-JA, a ako koristite posebnu Azure AD aplikacija zaštićenu aplikacije API provjerite da URL adresa za odgovor Azure AD aplikacije nije isti kao aplikaciju API osnovni URL. Ako je URL odgovor izravno u aplikaciju zaštićeni API-JA, korisnika u istom klijentu Azure AD nije pronađite aplikaciju API-JA, prijavite se i uspješno poziva na API-JA.

## <a id="tutorialstart"></a>Nastavite seriji vodiča .NET API aplikacije

Ako prate Node.js ili Java vodiča nizova za API aplikacije, prijeđite na odjeljak [daljnji koraci](#next-steps) . 

Ostatak ovog članka i dalje seriji vodiča .NET API aplikacije i pretpostavlja da ste dovršili [vodič provjere autentičnosti korisnika](app-service-api-dotnet-user-principal-auth.md) i imati uzorka aplikaciju koja se izvodi u Azure s omogućena je provjera autentičnosti korisnika.

## <a name="set-up-authentication-in-azure"></a>Postavljanje provjere autentičnosti u Azure

U ovom odjeljku konfigurirate aplikacije servisa za tako da je samo HTTP zahtjeve omogućuje dosegne aplikaciju podataka sloju API one koje su valjane Azure AD nošenja tokena. 

U sljedećem odjeljku Konfiguriranje aplikaciju API Srednji sloj vjerodajnice aplikacije poslati Azure AD, vratiti nošenja token i slanje token nošenja aplikaciju za API sloju podataka. Ovaj postupak je podržali dijagram.

![Servis za provjeru autentičnosti dijagrama](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Ako naiđete na probleme tijekom slijedeći upute vodiča, u odjeljku [Otklanjanje poteškoća](#troubleshooting) na kraju vodič. 

1. [Portal za Azure](https://portal.azure.com/)idite na **Postavke** plohu API aplikacije koje ste stvorili za aplikaciju za API ToDoListDataAPI (podataka sloju), a zatim **Postavke**.

2. U plohu **Postavke** pronađite odjeljak **značajke** , a zatim kliknite **provjere autentičnosti / autorizacije**.

    ![Provjera autentičnosti/autorizacije Azure portalu](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. U na **provjere autentičnosti / autorizacije** plohu, **kliknite**.

4. Na padajućem popisu **poduzeti kada zahtjev je autentičnost provjerena** odaberite **prijavite se pomoću servisa Azure Active Directory**.

    Ovo je postavka koja uzrokuje aplikacije servisa da biste bili sigurni koje samo autorizirani zahtjeva za razgovor aplikaciju API-JA. Zahtjevi za koje imate valjan nošenja tokena, aplikacije servisa za prosljeđuje tokeni duž aplikaciju API-JA i popunjava HTTP zaglavlja s često korištenih zahtjevima da bi se jednostavnije dostupne kod te podatke.

5. U odjeljku **Davatelja usluge provjere autentičnosti**, kliknite **Azure Active Directory**.

    ![Provjera autentičnosti/autorizacije plohu Azure portalu](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. U plohu **Azure Active Directory postavke** kliknite **Express**.

    S **Express** mogućnost Azure možete automatski stvoriti aplikaciju AAD Azure AD [klijenta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Da biste stvorili klijent, ne morate jer svaki račun za Azure automatski postoji.

7. U odjeljku **način upravljanja**, kliknite **Stvaranje novog aplikacije AD** ako već nije odabran.

    Na portalu priključuje **Stvaranje aplikacije** okvir za unos sa zadanom vrijednosti. Prema zadanim postavkama, aplikacije za Azure AD pod nazivom jednaki aplikaciju API-JA. Ako želite, možete unijeti neki drugi naziv.
    
    ![Postavke za Azure AD](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Napomena**: umjesto toga, možete koristiti jedan Azure AD aplikacije za aplikaciju pozivanja API-JA i aplikaciju zaštićeni API-JA. Ako odaberete tu zamjenski, potrebno je **Stvoriti novu aplikaciju AD** mogućnost ovdje Budući da ste već stvorili programa Azure AD ranije u praktičnom vodiču za provjeru autentičnosti korisnika. Za ovaj vodič, koristit ćete odvojite Azure AD aplikacija na pozivanja API aplikacije i zaštićene API-JA.

8. Zabilježite vrijednost koja se nalazi u okviru unos **Stvaranje aplikacije** ; ćete potražiti ovu AAD aplikaciju na portalu za Azure klasični kasnije.

7. Kliknite **u redu**.

10. U na **provjere autentičnosti / autorizacije** plohu, kliknite **Spremi**.

    ![Kliknite Spremi](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    Aplikacije servisa za stvara programa Azure Active Directory URL za **prijavu** i **URL odgovori** automatski postavlja na URL-a aplikacije API-JA. Vrijednost potonjem omogućuje korisnika na klijentu AAD prijaviti i pristup aplikaciju API-JA.

### <a name="verify-that-the-api-app-is-protected"></a>Provjerite je li aplikaciju API zaštićen

1. U pregledniku idite na URL aplikaciju API-JA: plohu **API aplikacije** na portalu za Azure, kliknite vezu u odjeljku **URL-a**. 

    Bit ćete preusmjereni na zaslonu za prijavu jer Neprovjereni zahtjevi nisu dopušteni dosegne aplikaciju API-JA. 

    Ako vaš preglednik se korisničko Sučelje Swagger, vaš preglednik možda će već biti prijavljeni na – u tom slučaju otvorite programa InPrivate ili Inkognito prozor i idite na URL Swagger korisničkog Sučelja.

18. Prijavite se pomoću vjerodajnica korisnika na klijentu AAD.

    Kada ste prijavljeni, "uspješno stvorili" stranica pojavit će se u pregledniku.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfiguriranje ToDoListAPI project nabavite i slanje token Azure AD

U ovom odjeljku učinite sljedeće zadatke:

* Dodavanje koda u aplikaciji API Srednji sloj koji koristi vjerodajnice aplikacije Azure AD za dohvaćanje token i pošaljite je s HTTP zahtjevima za aplikaciju za API razina podataka.
* Vjerodajnice morate zatražite od Azure AD.
* Unesite vjerodajnice u aplikacije servisa za Azure postavki okruženja za vrijeme izvođenja u srednjem sloju API aplikacije. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfiguriranje ToDoListAPI project nabavite i slanje token Azure AD

Provjerite sljedeće promjene u programu project ToDoListAPI u Visual Studio.

1. Uklonite sve kod u datoteci *ServicePrincipal.cs* .

    Ovo je kod koji koristi ADAL za .NET dobiti nošenja token Azure AD.  Koristi nekoliko konfiguracijskih vrijednosti koje ćete postaviti u okruženju Azure runtime kasnije. Evo kod: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Bilješke:** Kod zahtijeva s ADAL paketa .NET NuGet (Microsoft.IdentityModel.Clients.ActiveDirectory), koji je već instalirana u projektu. Ako stvarate taj projekt od nule, promijenile da biste instalirali paket. Paket nije automatski instalirao novi projekt predložak za aplikacije API-JA.

2. U *Kontrolera/ToDoListController*, uklonite kod u na `NewDataAPIClient` način koji dodaje token HTTP zahtjeve u zaglavlju autorizacije.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. Implementacija ToDoListAPI projekta. (Desnom tipkom miša kliknite projekt, a zatim kliknite **Objavljivanje > Objavi**.)

    Visual Studio uvodi projekta i otvara preglednik za osnovni URL web-aplikaciji. Prikazat će na stranicu 403 pogreške koja se uobičajeno je da će prilikom pokušaja u pregledniku idite na Web API osnovni URL.

4. Zatvorite preglednik.

### <a name="get-azure-ad-configuration-values"></a>Dohvaćanje vrijednosti konfiguracije Azure AD

11. [Azure klasični portal](https://manage.windowsazure.com/)otvorite **Azure Active Directory**.

12. Na kartici **direktorija** kliknite AAD klijentu.

14. Kliknite **aplikacije > aplikacija je vlasnik Moja tvrtka**, a zatim kliknite kvačicu.

15. Na popisu programa kliknite naziv koji Azure stvara se kad ste omogućili provjere autentičnosti za aplikaciju ToDoListDataAPI (podataka sloju) API-JA.

16. Kliknite karticu **Konfiguracija** .

5. Kopirajte vrijednost **ID klijenta** i spremite ga neko mjesto ga možete preuzeti kasnije. 

8. Na portalu za Azure klasični vratite se na popisu **aplikacija koje je vlasnik tvrtke**pa kliknite AAD aplikaciju koju ste stvorili za aplikaciju ToDoListAPI API Srednji sloj (onaj koji ste stvorili u prethodnom izvješću, ne onaj koji ste stvorili pomoću ovog praktičnog vodiča).

16. Kliknite karticu **Konfiguracija** .

5. Kopirajte vrijednost **ID klijenta** i spremite ga neko mjesto ga možete preuzeti kasnije.

6. U odjeljku **tipke**, odaberite **1 godine** s padajućeg popisa **Odaberite trajanje** .

6. Kliknite **Spremi**.

    ![Generiranje ključ aplikacije](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. Kopirajte vrijednost ključa i spremite ga neko mjesto ga možete preuzeti kasnije.

    ![Kopiranje novog ključa za aplikaciju](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Konfiguriranje postavki za Azure AD u okruženju izvođenja aplikaciju Srednji sloj API-JA

1. Idite na [portal za Azure](https://portal.azure.com/), a zatim otiđite do plohu **API aplikacija** za aplikaciju API koji hostira projekta TodoListAPI (Srednji sloj).

2. Kliknite **Postavke > postavke aplikacije**.

3. U odjeljku **postavke za aplikaciju** , dodajte sljedeći ključevi i vrijednosti:

  	| **Ključ** | IDa: za izdavanje certifikata |
  	|---|---|
  	| **Vrijednost** | https://login.microsoftonline.com/ {naziv Azure AD klijentu} |
  	| **Primjer** | https://login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Ključ** | IDa: ClientId |
  	|---|---|
  	| **Vrijednost** | ID klijenta pokrenuti aplikacije (Srednji sloj - ToDoListAPI) |
  	| **Primjer** | 960adec2-b74a-484a-960adec2-b74a-484a |

  	| **Ključ** | IDa: ClientSecret |
  	|---|---|
  	| **Vrijednost** | Aplikacija ključ aplikacija (Srednji sloj - ToDoListAPI) |
  	| **Primjer** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **Ključ** | IDa: resurs |
  	|---|---|
  	| **Vrijednost** | ID klijenta naziva aplikacije (podataka sloju - ToDoListDataAPI) |
  	| **Primjer** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **Napomena**: za `ida:Resource`, provjerite jeste li koristili naziva aplikacije **ID klijenta** , a ne njegov **Aplikacije ID URI**.

    `ida:ClientId`i `ida:Resource` razlikuju se vrijednosti za ovog praktičnog vodiča zato što koristite odvojite applicaations Azure AD za Srednji sloj i razina podataka. Ako ste koristili jednu aplikaciju za Azure AD za na pozivanja API aplikacije i zaštićene API-JA, koristila iste vrijednosti u obje `ida:ClientId` i `ida:Resource`.

    Kod koristi ConfigurationManager da biste dobili te vrijednosti tako da ih nije moguće spremiti u Web.config datoteke u projekt ili u okruženje za izvođenje Azure. ASP.NET aplikacija pokrenutom u aplikacije servisa za Azure postavki okruženja automatski nadjačati postavke iz Web.config. Postavki okruženja su obično [sigurnije način spremanja povjerljive podatke u usporedbi s Web.config datoteke](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Kliknite **Spremi**.

    ![Kliknite Spremi](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Testiranje aplikacije

1. U pregledniku idite na URL HTTPS AngularJS sučelje web-aplikacije.

2. Pritisnite tabulator **Da biste učinili popisa** i prijaviti se pomoću vjerodajnica za korisnika u klijentu za Azure AD. 

4. Dodavanje stavki zadataka da biste provjerili funkcionira li aplikacija.

    ![Da biste dobili popis stranica](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Ako aplikacija ne funkcioniraju, ponovno sve postavke koje ste unijeli na portalu za Azure. Ako se svi postavke pojavit će se čini ispravnom, u odjeljku [Otklanjanje poteškoća](#troubleshooting) u nastavku ovog praktičnog vodiča.

## <a name="protect-the-api-app-from-browser-access"></a>Zaštita aplikaciju API iz programa access u pregledniku

Pomoću ovog praktičnog vodiča koje ste stvorili posebnu Azure AD aplikacije za aplikaciju API ToDoListDataAPI (podataka sloju). Kao što ste vidjeti, kada aplikacije servisa za stvara AAD aplikacije, konfigurira aplikacije AAD na način koji omogućuje korisniku idite na URL API app u pregledniku i prijavite se. To znači da je moguće krajnji korisnik u klijentu Azure AD ne samo servisa glavnica, da biste pristupili na API-JA. 

Ako želite onemogućiti pristup putem preglednika bez pisanja kod u aplikaciji zaštićeni API-JA, možete promijeniti **URL odgovora** u aplikaciji AAD tako da se razlikuje od aplikaciju API osnovni URL. 

### <a name="disable-browser-access"></a>Onemogućivanje pristup putem preglednika

1. Na kartici **Konfiguriraj** klasični portal za aplikacije AAD koji je stvoren za na TodoListService, promijenite vrijednost u polje **URL odgovor** tako da je valjan URL, ali ne aplikaciju API URL-a.
 
2. Kliknite **Spremi**.

### <a name="verify-browser-access-no-longer-works"></a>Provjerite je li pristup putem preglednika više ne funkcionira

Neke starije verzije provjeriti da možete posjetiti API URL-a aplikacije iz preglednika prijavom s vjerodajnicama pojedinačnih korisnika. U ovom se odjeljku provjerite da to više nije moguće. 

1. U novom prozoru preglednika, idite na URL aplikaciju API ponovno.

2. Prijavite se u kada se to od vas zatraži da biste to učinili.

3. Prijava uspjela, ali vodi na stranicu s pogreškom.

    Aplikaciju AAD ste konfigurirali tako da korisnici u klijentu AAD ne možete prijaviti i pristupiti na API putem preglednika. Aplikaciju API-JA i dalje možete pristupiti pomoću token glavni servisa, koji možete provjeriti tako da URL web-aplikacije i dodavanje više obaveza.

## <a name="restrict-access-to-a-particular-service-principal"></a>Ograničiti pristup glavni određenog servisa  

Desnom tipkom miša sada sve pozivatelja koje možete dobiti token za korisnika ili Upravitelj servisa u klijentu za Azure AD da biste uputili poziv aplikaciju API TodoListDataAPI (podataka sloju). Možda ćete morati provjerite je li aplikaciju za API sloju podataka samo prihvaća pozive iz aplikacije API TodoListAPI (Srednji sloj), a samo iz određene servisne glavni. 

Možete dodati ta ograničenja dodavanjem kod za provjeru valjanosti u `appid` i `objectidentifier` zahtjevima na dolazne pozive.

Za ovaj vodič staviti kod koji provjerava ID aplikacija i servisa glavni ID-om izravno u kontroler akcija.  Alternative su da biste koristili prilagođeni `Authorize` atributa ili da biste učinili ovaj provjere valjanosti u nizove za pokretanje (npr. OWIN proizvod). Primjer drugu mogućnost, potražite u članku [u ovom primjeru aplikacije](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Provjerite sljedeće promjene TodoListDataAPI projekt.

2. Otvorite datoteku *Controllers/TodoListController.cs* .

3. Uklonite retke koje postavite `trustedCallerClientId` i `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Uklonite kod u način CheckCallerId. Ta metoda naziva na početku svake akcije metoda u kontrolerom. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Ponovno implementirate projekt ToDoListDataAPI aplikacije servisa za Azure.

6. U pregledniku idite na URL HTTPS aplikacije AngularJS sučelje web app, a na početnoj stranici kliknite karticu **Učinite popisa** .

    Aplikacija ne funkcionira jer su neuspješnih poziva pozadinska. Novi kod provjerava stvarni ID programa i objectidentifier, ali još ne sadrži odgovarajuće vrijednosti za provjeru ih odnosu. Preglednik konzole za alate za razvojne inženjere izvješća poslužitelja je vraća pogrešku HTTP 401.

    ![Pogreška u programiranje Alati konzole](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    U sljedećim koracima konfigurirati očekivanim vrijednostima.

8. Korištenje Azure AD PowerShell Preuzmi vrijednost glavnice servisa za Azure AD aplikaciju koju ste stvorili za projekt TodoListWebApp.

    na. Upute za instaliranje Azure PowerShell i povezati pretplate, potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).

    b. Da biste dobili popis servisa upravitelji, izvoditi na `Login-AzureRmAccount` naredba te na `Get-AzureRmADServicePrincipal` naredbe.

    c. Pronađite u ID objekta za servis glavnicu TodoListAPI aplikacije i spremite ga na mjestu možete kopirati iz kasnije.

7. Na portalu Azure dođite do aplikacija plohu API-JA za aplikaciju API razvijena ToDoListDataAPI projekt.

9. Kliknite **Postavke > postavke aplikacije**.

3. U odjeljku **postavke za aplikaciju** , dodajte sljedeći ključevi i vrijednosti:

  	| **Ključ** | todo:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Vrijednost** | Servis za id glavni pozivanja aplikacije |
  	| **Primjer** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Ključ** | todo:TrustedCallerClientId |
  	|---|---|
  	| **Vrijednost** | ID klijenta pozivanja aplikacija – kopiraju iz aplikacije TodoListAPI Azure AD |
  	| **Primjer** | 960adec2-b74a-484a-960adec2-b74a-484a |

6. Kliknite **Spremi**.

    ![Kliknite Spremi](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. U pregledniku, vratite se URL web-aplikaciji, a na početnoj stranici kliknite karticu **Učinite popisa** .

    Ovaj put aplikacija funkcionira očekivan način jer aplikaciju pouzdanih pozivatelja ID i ID glavni servisa su očekivanim vrijednostima.

    ![Da biste dobili popis stranica](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Stvaranje projekte od početka

Dva projekata Web API stvorene pomoću predloška za projekt **Azure API aplikacije** i zamjena kontrolerom zadane vrijednosti ToDoList kontroler. Za dobivanje Azure AD servisa glavni tokeni u programu project ToDoListAPI NuGet paketa [Active Directory provjera autentičnosti biblioteke (ADAL) za .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) je instaliran.
 
Informacije o stvaranju aplikacije jednostranični AngularJS s Web API pozadini kao što su ToDoListAngular potražite u članku [ruke na Laboratorija: Izrada na jednu stranicu aplikacije (SPA) s API servisa platforme ASP.NET Web i Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informacije o tome kako dodati kod provjere autentičnosti za Azure AD potražite u članku [Zaštita AngularJS jednu stranicu aplikacije s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Otklanjanje poteškoća

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Pripazite da ne zbuniti ToDoListAPI (srednjem sloju) i ToDoListDataAPI (podataka sloju). Ako, na primjer, pomoću ovog praktičnog vodiča dodate provjere autentičnosti aplikaciju API razina podataka **, ali tipku aplikacije mora potjecati iz aplikacije za Azure AD koji ste stvorili za aplikaciju Srednji sloj API-JA**.

## <a name="next-steps"></a>Daljnji koraci

Ovo je zadnji vodič u nizu API aplikacije. 

Dodatne informacije o servisu Azure Active Directory potražite u sljedećim resursima.

* [Vodič za Azure AD razvojnim inženjerima'](http://aka.ms/aaddev)
* [Scenariji za Azure AD](http://aka.ms/aadscenarios)
* [Uzorci Azure AD](http://aka.ms/aadsamples)

    Ogledna [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) je slično kao što je prikazano u ovom ćete praktičnom vodiču, ali bez korištenja aplikacije servisa za provjeru autentičnosti.

Informacije o drugim načinima za implementaciju projektima Visual Studio aplikacije API pomoću Visual Studio ili [Automatizacija implementacije](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) iz [izvora kontrole sustava](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)potražite u članku [Kako implementirati aplikaciju aplikacije servisa za Azure](../app-service-web/web-sites-deploy.md).
