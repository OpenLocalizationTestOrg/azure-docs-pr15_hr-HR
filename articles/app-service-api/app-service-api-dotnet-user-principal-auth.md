<properties
    pageTitle="Provjera autentičnosti korisnika API aplikacije u aplikacije servisa za Azure | Microsoft Azure"
    description="Saznajte kako se zaštititi aplikaciju API-JA u aplikacije servisa za Azure dopuštanja pristupa samo korisnicima čija je autentičnost provjerena."
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

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Provjera autentičnosti korisnika API aplikacije u aplikacije servisa za Azure

## <a name="overview"></a>Pregled

U ovom se članku objašnjava zaštiti aplikaciju Azure API tako da samo provjerene korisnike možete pozvati. U članku pretpostavlja da ste pročitali [Pregled Azure aplikacije servisa za provjeru autentičnosti](../app-service/app-service-authentication-overview.md).

Ćete saznati:

* Upute za konfiguriranje davatelja provjere autentičnosti s detaljima za Azure Active Directory (Azure AD).
* Kako se zauzeti zaštićeni API aplikacije pomoću [Active Directory provjera autentičnosti biblioteke (ADAL) za JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

U članku sastoji se od dva dijela:

* U odjeljku [Konfiguriranje provjere autentičnosti korisnika u aplikacije servisa za Azure](#authconfig) Općenito objašnjava kako konfigurirati provjere autentičnosti korisnika za bilo koju aplikaciju za API-JA i jednoliko primjenjuje na sve okviri podržava aplikacije servisa, uključujući .NET, Node.js i Java.

* Počevši od [nastavka vodiči za .NET API aplikacije](#tutorialstart) odjeljku vodilice članak kroz Konfiguriranje aplikacije za uzorak s na .NET sigurnosno završetka, a završava na prednju AngularJS tako da koristi Azure Active Directory za provjeru autentičnosti korisnika. 

## <a id="authconfig"></a>Konfiguriranje provjere autentičnosti korisnika u aplikacije servisa za Azure

U ovom se odjeljku nalaze općenite upute koje se odnose na bilo koju aplikaciju za API-JA. Korake određene za primjer aplikacije da biste učinili .NET popisa, otvorite [nastavka vodiči za .NET početak rada](#tutorialstart).

1. [Portal za Azure](https://portal.azure.com/)idite na **Postavke** plohu API aplikacije koju želite zaštititi, pronađite odjeljak **značajke** , a zatim kliknite **provjere autentičnosti / autorizacije**.

    ![Portal za Azure provjere autentičnosti/autorizacije](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. U na **provjere autentičnosti / autorizacije** plohu, **kliknite**.

4. Odaberite jednu od mogućnosti s padajućeg popisa **poduzeti kada zahtjev je autentičnost provjerena** .

    * Ako želite samo čija je autentičnost provjerena pozivi dosegne API aplikacije, odaberite neku od mogućnosti **... prijavite se** . Ta mogućnost omogućuje zaštitu aplikaciju API bez pisanja kod koji se izvodi u njoj.

    * Ako želite da se pozivi svih dosegne API aplikacije, odaberite **Dopusti zahtjev (bez akcije)**. Koristite ovu mogućnost da biste usmjerili Neprovjereni pozivatelji na izbor davatelja usluge provjere autentičnosti. Uz tu se mogućnost morate pisati kod za rukovanje autorizacije.

    Dodatne informacije potražite u članku [provjere autentičnosti i autorizacije API aplikacije u aplikacije servisa za Azure](app-service-api-authentication.md#multiple-protection-options).

5. Odaberite jednu ili više od **Davatelja usluge provjere autentičnosti**.

    Slika prikazuje mogućnosti koje je potrebno sve pozivatelji proći Azure AD.
 
    ![Azure plohu provjere autentičnosti/autorizacije portala](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Kad odaberete davatelj usluge provjere autentičnosti, na portalu prikazuje konfiguracije plohu za taj davatelja usluga. 

    Detaljne upute u članku se objašnjava kako konfigurirati blades za konfiguriranje davatelja usluge provjere autentičnosti potražite u članku [upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje prijava Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Veza ide članak o Azure AD, ali sam članak sadrži kartice koje sadrže vezu do članaka za ostali davatelji provjere.)

7. Kada završite s plohu za konfiguriranje davatelja usluge provjere autentičnosti, kliknite **u redu**.

7. U na **provjere autentičnosti / autorizacije** plohu, kliknite **Spremi**.

Kada to učinite, aplikacije servisa za potvrđuje sve API poziva prije dođu aplikaciju API-JA. Servise za provjeru autentičnosti funkcionira na isti način za sve jezike koje podržava aplikacije servisa, uključujući .NET, Node.js i Java. 

Da biste pozive čija je autentičnost provjerena API-JA, pozivatelj sadrži davatelja provjere OAuth 2.0 nošenja token u zaglavlju autorizacije HTTP zahtjeva. Token se možete nabaviti pomoću SDK davatelja provjere.

## <a id="tutorialstart"></a>Nastavite vodiči za .NET API aplikacije

Ako pratite Node.js ili Java vodiče za API aplikacije, prijeđite na drugi u članku [servis glavni provjere autentičnosti za API aplikacije](app-service-api-dotnet-service-principal-auth.md). 

Ako prate .NET seriji vodiča za API aplikacije i ste već ste implementirali primjer aplikacije kao što je navedeno u vodičima [prvi](app-service-api-dotnet-get-started.md) i [drugi](app-service-api-cors-consume-javascript.md) , prijeđite na odjeljak [Postavljanje provjere autentičnosti u aplikacije servisa i Azure AD](#azureauth) .

Ako želite slijedite ovaj Praktični vodič bez prolaze kroz vodiči za prvi i drugi, učinite sljedeće korake koji objašnjavaju kako započeti pomoću automatiziranog procesa za implementaciju aplikacije uzorka.

>[AZURE.NOTE] Sljedeći koraci vam omogućiti istu početnu točku kao da jeste li prva dva praktične vodiče, uz jednu iznimku: Visual Studio već neće znati koje web-aplikacijama ili API aplikacije koja će uvesti svaki projekt. To znači da nećete imati točno upute ovog praktičnog vodiča za implementaciju desnom ciljeve. Ako niste upoznati s odrediti način implementacije koraka vlastite, bolje je da biste pratili seriji vodiča sa [prvom praktičnom vodiču](app-service-api-dotnet-get-started.md) od da biste započeli postupak automatskog implementacije.

1. Provjerite imate li sve preduvjete navedene u [prvom praktičnom vodiču](app-service-api-dotnet-get-started.md). Osim preduvjeta na popisu, te vodiči za provjeru autentičnosti pretpostavlja da ste radili s aplikacije servisa za web-aplikacije i API aplikacije u Visual Studio i Azure portal.

2. Kliknite gumb **uvođenja za Azure** u [datoteke readme spremište uzorka za učinite popisa](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) za implementaciju aplikacije API-JA i web-aplikaciji. Zabilježite grupi Azure resursa koje dohvaća stvorili, kao što je to možete koristiti kasnije da biste potražili web app i nazivi aplikacije API-JA.
 
3. Preuzimanje ili Kloniraj [učinite popisa uzorka spremište](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) preuzimate kod za koje ćete raditi s lokalno u Visual Studio.

## <a id="azureauth"></a>Postavljanje provjere autentičnosti u aplikacije servisa i Azure AD

Sada imate aplikacije koje se izvode u aplikacije servisa za Azure bez potrebe da korisnici moguće provjeriti autentičnost. U ovom odjeljku dodajte provjere autentičnosti tako da učinite sljedeće zadatke:

* Konfiguriranje servisa za aplikaciju zahtijeva provjeru autentičnosti Azure Active Directory (Azure AD) za pozivanje aplikaciju Srednji sloj API-JA.
* Stvorite aplikaciju Azure AD.
* Konfiguriranje aplikacije za Azure AD za slanje token nošenja nakon prijave AngularJS sučelje. 

Ako naiđete na probleme tijekom slijedeći upute vodiča, u odjeljku [Otklanjanje poteškoća](#troubleshooting) na kraju vodič. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Konfiguriranje provjere autentičnosti za aplikaciju Srednji sloj API-JA

1. [Portal za Azure](https://portal.azure.com/)dođite do plohu **Postavke** API aplikacije koje ste stvorili za projekt ToDoListAPI, pronađite odjeljak **značajke** , a zatim kliknite **provjere autentičnosti / autorizacije**.

    ![Portal za Azure provjere autentičnosti/autorizacije](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. U na **provjere autentičnosti / autorizacije** plohu, **kliknite**.

4. Na padajućem popisu **poduzeti kada zahtjev je autentičnost provjerena** odaberite **prijavite se pomoću servisa Azure Active Directory**.

    Ova mogućnost osigurava da zahtjevi ne unauathenticated dosegne aplikaciju API-JA. 

5. U odjeljku **Davatelja usluge provjere autentičnosti**, kliknite **Azure Active Directory**.

    ![Azure plohu provjere autentičnosti/autorizacije portala](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. U plohu **Azure Active Directory postavke** kliknite **Express**

    ![Azure portala provjere autentičnosti/autorizacije Express mogućnost](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    Uz mogućnost **Express** aplikacije servisa možete automatski stvoriti aplikaciju Azure AD Azure AD [klijenta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Da biste stvorili klijent, ne morate jer svaki račun za Azure automatski postoji.

7. U odjeljku **način upravljanja**, ako već nije odabrano, kliknite **Stvori novu aplikaciju AD** i obratite pozornost na vrijednost koja je u tekstnom okviru **Stvaranje aplikacije** ; ćete potražiti ovu AAD aplikaciju na portalu za Azure klasični kasnije.

    ![Postavke Azure portal Azure AD](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure automatski stvoriti aplikaciju za Azure AD označene imenom u klijentu za Azure AD. Prema zadanim postavkama, aplikacije za Azure AD pod nazivom jednaki aplikaciju API-JA. Ako želite, možete unijeti neki drugi naziv.
 
7. Kliknite **u redu**.

7. U na **provjere autentičnosti / autorizacije** plohu, kliknite **Spremi**.

    ![Kliknite Spremi](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Samo korisnici u klijentu za Azure AD sada možete nazvati aplikaciju API-JA.

### <a name="optional-test-the-api-app"></a>Neobavezno: Testiranje aplikaciju API-JA

1. U pregledniku idite na URL aplikaciju API-JA: plohu **API aplikacije** na portalu za Azure, kliknite vezu u odjeljku **URL-a**.  

    Bit ćete preusmjereni na zaslonu za prijavu jer Neprovjereni zahtjevi nisu dopušteni dosegne aplikaciju API-JA.

    Ako vaš preglednik prelazi na stranicu za "uspješno stvorili", web-pregledniku možda će već biti prijavljeni na – u tom slučaju otvorite programa InPrivate ili Inkognito prozor i odaberite aplikaciju API URL.

2. Prijavite se pomoću vjerodajnica za korisnika u klijentu za Azure AD.

    Kada ste prijavljeni, "uspješno stvorili" stranica pojavit će se u pregledniku.

9. Zatvorite preglednik.

### <a name="configure-the-azure-ad-application"></a>Konfiguriranje aplikacije za Azure AD

Kada ste konfigurirali Azure AD provjeru autentičnosti, aplikacije servisa za vas stvara Azure AD aplikacije. Prema zadanim postavkama novi Azure AD aplikacije konfigurirano tako da omogućuje token nošenja aplikaciju API URL-a. U ovom odjeljku konfigurirajte aplikacije Azure AD omogućuje token nošenja u AngularJS sučelje umjesto izravno u aplikaciji Srednji sloj API-JA. AngularJS sučelje poslat će token aplikaciju API prilikom poziva aplikaciju API-JA.

>[AZURE.NOTE] Da biste zadržali postupka kao jednostavni moguće, ovog vodiča koristi u jednom Azure AD za sučelje i sredine razina aplikacije API aplikacije. Druga je mogućnost da biste koristili dvije Azure AD aplikacije. U tom slučaju promijenile možete dodijeliti dozvole za aplikaciju Azure AD prednju end da biste nazvali Srednji sloj Azure AD aplikacije. Takvog više aplikacija će vam dati precizniji kontrolu nad dozvole za svaku sloju.

11. [Azure klasični portal](https://manage.windowsazure.com/)otvorite **Azure Active Directory**.

    Morate koristiti klasične portal jer određene postavke Azure Active Directory koji vam je potreban pristup još nisu dostupni na portalu za trenutni Azure.

12. Na kartici **direktorija** kliknite AAD klijentu.

    ![Azure AD klasični portalu](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Kliknite **aplikacije > aplikacija je vlasnik Moja tvrtka**, a zatim kliknite kvačicu.

    Možda morati osvježiti stranicu da biste vidjeli nove aplikacije.

15. Na popisu programa kliknite naziv koji Azure stvara se kad ste omogućili provjere autentičnosti za aplikaciju API-JA.

    ![Kartica Azure AD aplikacije](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Kliknite **Konfiguriraj**.

    ![Azure AD Konfiguriraj kartica](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. Postavite **URL za prijavu** na URL AngularJS web-aplikaciju programa, nema završne kosa crta.

    Na primjer: https://todolistangular.azurewebsites.net

    ![URL za prijavu](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. Postavljanje **URL ADRESE za odgovor** URL za web-aplikacije, bez završne kosa crta.

    Na primjer: https://todolistsangular.azurewebsites.net

16. Kliknite **Spremi**.

    ![URL adresa za odgovor](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. Pri dnu stranice kliknite **manifest upravljanje > preuzimanje manifesta**.

    ![Preuzimanje manifesta](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Preuzmite datoteku na mjesto gdje ga možete urediti.

16. U preuzetu datoteku manifesta potražite na `oauth2AllowImplicitFlow` svojstvo. Promijenite vrijednost tog svojstva iz `false` da biste `true`, a zatim spremite datoteku.

    Ta je postavka potrebni za access iz aplikacije za jednu stranicu JavaScript. Omogućuje token nošenja Oauth 2.0 se vraćaju u URL djelić.

16. Kliknite **manifest upravljanje > Prenesi manifest**, pa Prenesite datoteku koju ste ažurirali u prethodnom koraku.

    ![Prijenos manifest](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Kopirajte vrijednost **ID klijenta** i spremite je negdje ga možete preuzeti kasnije.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Konfiguriranje ToDoListAngular projekta za korištenje provjere autentičnosti

U ovom odjeljku promijenite AngularJS sučelje tako da koristi Active Directory provjera autentičnosti biblioteke (ADAL) za JS dobiti nošenja token za korisnika prijavljeni s Azure AD. Kod neće sadržavati stavku token u HTTP zahtjeva poslanih u srednjem sloju, kao što je prikazano na sljedećem su dijagramu. 

![Dijagram provjere autentičnosti korisnika](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Provjerite sljedeće promjene na datotekama u programu project ToDoListAngular.

1. Otvorite datoteku *index.html* .

2. Uklonite retke koje upućuju na Active Directory provjera autentičnosti biblioteke (ADAL) za skripte JS.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Otvorite datoteku *app/scripts/app.js* .

2. Komentar izvan blokova Šifra označene za "bez provjere autentičnosti", a zatim uklonite bloka kod označene za "s provjere autentičnosti".

    Ta promjena reference davatelja usluge provjere autentičnosti ADAL JS i njihovi konfiguracijskih vrijednosti na njega. U sljedećim koracima postavite konfiguracijskih vrijednosti za aplikaciju API-JA i Azure AD aplikacije.

8. U kodu koji se postavlja u `endpoints` varijable, postavite API URL URL aplikaciju API stvorili ToDoListAPI projekta i postavite ID aplikacije Azure AD na ID klijenta koju ste kopirali iz Azure klasični portal.

    Kod je slično kao u sljedećem primjeru.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. U pozivu na `adalProvider.init`, postavite `tenant` u vaše ime klijenta i `clientId` iste vrijednosti koje ste koristili u prethodnom koraku.

    Kod je slično kao u sljedećem primjeru.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    Te promjene na `app.js` odrediti da pozivanja kod i pod nazivom API-JA u aplikaciji Azure AD.

1. Otvorite datoteku *app/scripts/homeCtrl.js* .

2. Komentar izvan blokova Šifra označene za "bez provjere autentičnosti", a zatim uklonite bloka kod označene za "s provjere autentičnosti".

1. Otvorite datoteku *app/scripts/indexCtrl.js* .

2. Komentar izvan blokova Šifra označene za "bez provjere autentičnosti", a zatim uklonite bloka kod označene za "s provjere autentičnosti".

### <a name="deploy-the-todolistangular-project-to-azure"></a>Implementacija projekta ToDoListAngular Azure

8. U **Pregledniku rješenja**, desnom tipkom miša kliknite ToDoListAngular projekta, a zatim **Objavi**.

9. Kliknite **Objavi**.

    Visual Studio uvodi projekta i otvara preglednik za osnovni URL web-aplikaciji. Prikazat će na stranicu 403 pogreške koja se uobičajeno je da će prilikom pokušaja u pregledniku idite na Web API osnovni URL.

    I dalje imate unos promjena u aplikaciji API Srednji sloj prije nego što možete testirati i aplikacije.

10. Zatvorite preglednik.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Konfiguriranje ToDoListAPI projekta za korištenje provjere autentičnosti

Trenutno šalje projekta ToDoListAPI "*" kao na `owner` vrijednost ToDoListDataAPI. U ovom odjeljku promijenite kod da biste poslali ID prijavljeni korisnik.

Provjerite sljedeće promjene u programu project ToDoListAPI.

1. Otvorite datoteku *Controllers/ToDoListController.cs* i uklonite redak u svakom načinu akcije koje postavlja `owner` za Azure AD `NameIdentifier` zahtjeva vrijednost. Ako, na primjer:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Važno**: ne uklonite kod u na `ToDoListDataAPI` način; To ćete učiniti kasnije za vodič glavni provjere autentičnosti za servis.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Implementacija projekta ToDoListAPI Azure

8. U **Pregledniku rješenja**, desnom tipkom miša kliknite ToDoListAPI projekta, a zatim **Objavi**.

9. Kliknite **Objavi**.

    Visual Studio uvodi projekta i otvara preglednik za aplikaciju API osnovni URL.

10. Zatvorite preglednik.

### <a name="test-the-application"></a>Testiranje aplikacije

9. Idite na URL web-aplikacije **pomoću HTTPS, ne HTTP**.

8. Kliknite karticu **Da biste učinili popis** .

    Se od vas zatraži da biste se prijavili.

9. Prijavite se pomoću vjerodajnica za korisnik u klijentu AAD.

10. Pojavljuje se stranica **Učinite popisa** .

    ![Da biste dobili popis stranica](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Nema stavki obaveza prikazuju jer do sada sve su vlasnika "*". Sada Srednji sloj Traži stavke za prijavljeni korisnik i ništa stvorena još.

11. Dodajte nove zadatke da biste provjerili funkcionira li aplikacija.

12. U drugom prozoru preglednika, idite na URL za Swagger korisničko Sučelje za aplikaciju ToDoListDataAPI API-JA i kliknite **ToDoList > početak**. Unesite zvjezdicu za na `owner` parametar, a zatim **pokušajte ga**.

    Odgovor prikazuje da imaju novi obaveza stvarni Azure AD korisnički ID u svojstvu vlasnik.

    ![ID vlasnika u odgovoru JSON](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>Stvaranje projekte od početka

Dva projekata Web API stvorene pomoću predloška za projekt **Azure API aplikacije** i zamjena kontrolerom zadane vrijednosti ToDoList kontroler. 

Informacije o stvaranju aplikacije jednostranični AngularJS s pozadinskih 2 API na webu potražite u članku [ruke na Laboratorija: Izrada na jednu stranicu aplikacije (SPA) s API servisa platforme ASP.NET Web i Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informacije o tome kako dodati kod provjere autentičnosti za Azure AD potražite u sljedećim resursima:

* [Zaštita AngularJS jednu stranicu aplikacije s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).
* [Uvod u ADAL JS v1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Otklanjanje poteškoća

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Pripazite da ne zbuniti ToDoListAPI (srednjem sloju) i ToDoListDataAPI (podataka sloju). Ako, na primjer, provjerite je li dodali provjere autentičnosti aplikaciju Srednji sloj API-JA ne sloju podataka. 
* Provjerite je li izvorni kod AngularJS poziva Srednji sloj API aplikacije URL-a (ToDoListAPI, ne ToDoListDataAPI) i odgovarajuće Azure AD klijent ID-a. 

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako ga koristiti aplikacije servisa za provjeru autentičnosti za aplikaciju API i pozivanje aplikaciju API korištenjem ADAL JS biblioteke. Na sljedeći Praktični vodič ćete saznat ćete kako [sigurnog pristupa aplikacije API-JA za scenarije servisa za servis](app-service-api-dotnet-service-principal-auth.md).

