<properties 
    pageTitle="Početak rada s Azure pretraživanje upravljanje REST API-JA | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka" 
    description="Administriranje glavnom računalu oblaka servisa za pretraživanje Azure pomoću upravljanja REST API-JA" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="get-started-with-azure-search-management-rest-api"></a>Početak rada s Azure pretraživanje upravljanje REST API-JA
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST API-JA](search-get-started-management-api.md)

Upravljanje Azure pretraživanje REST API-JA je programski zamjena za izvođenje administrativne zadatke na portalu. Servis za upravljanje operacije obuhvaćaju stvaranje ili brisanje servis, skaliranje usluge i upravljanje tipke. U sklopu ovog praktičnog vodiča uzorka klijentska aplikacija koja pokazuje Upravljanje servisom API-JA. Sadrži i konfiguracija korake potrebne da biste pokrenuli uzorka u lokalnom platforme.

Da biste dovršili ovaj Praktični vodič, morate:

- Visual Studio 2012 ili 2013
- Preuzimanje aplikacije uzorka klijenta

Tijeku dovršavanja vodič, dva servisa dodijelit će se Resursi: Azure pretraživanja i Azure Active Directory (AD). Uz to, morate stvoriti AD aplikacije koja uspostavlja pouzdanosti između klijentska aplikacija i krajnje upravitelj resursa u Azure.

Trebat će račun za dovršetak ovog praktičnog vodiča za Azure.


##<a name="download-the-sample-application"></a>Preuzimanje oglednih aplikacije

Pomoću ovog praktičnog vodiča temelji se na aplikacije konzole za Windows pisane C#, koje možete uređivati i pokretanje u Visual Studio 2012 ili 2013

Klijentska aplikacija možete pronaći na Github pri [Pokazni videozapis API Azure pretraživanje .NET upravljanje](https://github.com/Azure-Samples/search-dotnet-management-api/).


##<a name="configure-the-application"></a>Konfiguriranje aplikacije

Prije pokretanja aplikacije uzorka, potrebno je omogućiti provjeru autentičnosti tako da se prihvaćaju zahtjeva poslanih iz klijentska aplikacija krajnjoj upravitelj resursa. Obavezne provjere autentičnosti potječe s [Azure Voditelj resursa](https://msdn.microsoft.com/library/azure/dn790568.aspx), što je temelj za sve vezane uz portal operacije zatražili putem API, uključujući one koji su povezani s upravljanjem servisom za pretraživanje. Upravljanje servisom API-JA za pretraživanje Azure je jednostavno datotečni nastavak od Voditelj resursa Azure i stoga nasljeđuje njezine ovisnosti.  

Azure Voditelj resursa zahtijeva servisa Azure Active Directory kao njegov davatelja identiteta. 

Da biste nabavili token za pristup omogućit će zahtjeve dođu Voditelj resursa, klijentske aplikacije sadrži Kodni odsječak koja poziva servisa Active Directory. Kodni odsječak plus pripremni koraci za korištenje segment kod su borrowed iz ovog članka: [Provjera autentičnosti Voditelj resursa Azure zahtjeva](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Slijedite upute u gornjem vezu ili slijedite korake u ovom dokumentu ako želite da bude obrađen vodič korak po korak.

U ovom ćete odjeljku će izvršiti sljedeće zadatke:

1. Stvaranje programa servisa AD
1. Stvaranje aplikacije komponente AD
1. Konfiguriranje aplikacija za AD pomoću Registracija detalje o klijentskoj aplikaciji uzorka koji ste preuzeli
1. Učitavanje klijentska aplikacija uzorka s vrijednostima koje će se koristiti da bi se dobio autorizacije njegov zahtjeva za

> [AZURE.NOTE] Veze pružaju pozadine o korištenju Azure Active Directory za provjeru autentičnosti zahtjeve klijenta s upraviteljem resursa: [Azure Voditelj resursa](http://msdn.microsoft.com/library/azure/dn790568.aspx), [Zahtjevi za Voditelj resursa za Azure provjere autentičnosti](http://msdn.microsoft.com/library/azure/dn790557.aspx)i [Azure Active Directory](http://msdn.microsoft.com/library/azure/jj673460.aspx).

###<a name="create-an-active-directory-service"></a>Stvaranje na servisu Active Directory

1. Prijava na [Portal za Azure](https://manage.windowsazure.com).

2. Pomaknite se prema dolje u lijevom navigacijskom oknu, a zatim kliknite **Servisa Active Directory**.

4. Kliknite **NOVO** da biste otvorili **Aplikaciju servisa** | **Servisa Active Directory**. U ovom ćete koraku stvarate novu servisa Active Directory. Taj servis će hostira AD aplikacije koje ćete odrediti nekoliko koraka iz sada. Stvaranje novog servisa olakšava izdvajanja vodič iz drugih aplikacija koje možda već biti smješten u Azure.

5. Kliknite **imenika** | **stvoriti prilagođene**.

6. Unesite naziv usluge, domene i zemlj mjesto. Domene mora biti jedinstvena. Kliknite kvačicu da biste stvorili servis.

     ![][5]

###<a name="create-a-new-ad-application-for-this-service"></a>Stvaranje nove aplikacije AD za taj servis

1. Odaberite servis Active Directory "SearchTutorial" koji ste upravo stvorili.

2. Na izborniku gornji kliknite **aplikacije**. 
 
3. Kliknite **Dodaj aplikaciju**. Zatvaranje AD pohranjuje informacije o klijentskim aplikacijama koja će koristiti ga kao davateljem identiteta.  
 
4. Odaberite **Dodavanje aplikacije je moje tvrtke ili ustanove razvoju**. Ta mogućnost nudi postavke registracije za aplikacije koje se objavljuju u galeriju aplikacija. Budući da se klijentske aplikacije nije dio aplikacije galerije, to je odabir za ovog praktičnog vodiča.

     ![][6]
 
5. Unesite naziv, primjerice "Azure pretraživanje – Upravitelj".

6. Odaberite **aplikaciju za nativni klijent** za vrstu aplikacije. Ovo ispravna za aplikaciju uzorka; To se događa se aplikacija Windows klijenta (Konzola), ne web-aplikacije.

     ![][7]
 
7. U preusmjeravanje URI unesite "http://localhost/Azure-Search-Manager-App". To URI koji Azure Active Directory bit ćete preusmjereni korisnički agent kao odgovor na zahtjev za autorizaciju OAuth 2.0. Vrijednosti moraju biti fizičke krajnje točke, ali moraju biti valjane URI. 

    Za potrebe ovog praktičnog vodiča, vrijednost može biti bilo što, ali ono unesete postaje obavezan unos administratorske veze u aplikaciji uzorka. 
 
7. Kliknite kvačicu za stvaranje aplikacije servisa Active Directory. Trebali biste vidjeti "Azure-pretraživanje – Upravitelj – aplikacije" u lijevom navigacijskom oknu.

###<a name="configure-the-ad-application"></a>Konfiguriranje aplikacije servisa Active Directory
 
9. Kliknite aplikaciju AD "Azure-pretraživanje – Upravitelj – aplikacije", koji ste upravo stvorili. Trebali biste vidjeti ga u lijevom navigacijskom oknu.

10. Kliknite **Konfiguriraj** u gornji izbornik.
 
11. Pomaknite se do dozvole pa odaberite **Azure upravljanja API -JA**. U ovom ćete koraku navedete API-JA (u ovom slučaju Azure resursima API-JA) koja klijentskoj aplikaciji treba pristup, zajedno s razinu pristupa potrebne.

12. U odjeljku dodijeliti dozvole kliknite padajući popis i odaberite **Upravljanje servisom Azure programa Access (pretpregled**).
 
     ![][8]
 
13. Spremite promjene. 

Ostavite otvorenim na stranicu za konfiguriranje aplikacije. U sljedećem koraku će kopirajte vrijednosti iz ove stranice i unesite ih u aplikaciju uzorka.

###<a name="load-the-sample-application-program-with-registration-and-subscription-values"></a>Učitavanje aplikaciji uzorka s vrijednostima Registracija i pretplate

U ovom odjeljku ćete uređivati rješenja u Visual Studio, zamjenjujući valjane vrijednosti dobivenog portalu.
Vrijednosti koje će dodavanje pojavljuju se pri vrhu Program.cs:

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

Ako još niste [preuzeti aplikaciju uzorka Github](https://github.com/Azure-Samples/search-dotnet-management-api/), morate ga za ovaj korak.

1. Otvorite **ManagementAPI.sln** u Visual Studio.

2. Otvorite Program.cs.

3. Navedite `ClientId`. Iz aplikacije AD konfiguracije stranica lijevo otvorite u prethodnom koraku, kopirajte ID klijenta na stranici Konfiguracija AD aplikacije na portalu i zalijepite u Program.cs.

4. Navedite `RedirectUrl`. Kopirajte URI preusmjeravanje na istoj stranici portala i zalijepite ga u Program.cs.

    ![][9]

5. Navedite`TenantID.` 
    - Vratili u servisu Active Directory | SearchTutorial (servis). 
    - Na gornjoj traci kliknite **aplikacije** . 
    - Kliknite **Prikaz krajnje točke** pri dnu stranice. 
    - Kopirajte OAUTH 2.0 autorizacije krajnju točku pri dnu popisa. 
    - Zalijepite krajnju točku TenantID, obrezivanja vrijednost sve parametre URI osim ID klijenta

    Zadani "https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0", izbrišite sve osim "55e324c7-1656-4afe-8dc3-43efcd4ffa50".

    ![][10]

6. Navedite `SubscriptionID`.
    - Idite na glavnoj stranici portala.
    - Kliknite **Postavke** pri dnu lijevog navigacijskog okna.
    - Na kartici pretplate kopirajte ID pretplate i zalijepite ga u Program.cs.

7. Spremite i sastavljanje rješenja.


##<a name="explore-the-application"></a>Istražite aplikacije

Dodajte na točku prekida pri prvom pozivanje metode tako da možete Prođite program. Pritisnite **F5** da biste pokrenuli aplikaciju, a zatim pritisnite tipku **F11** da biste se pomicali kroz kod.

Primjer aplikacije stvara besplatnog servisa za pretraživanje Azure za postojeću pretplatu na Azure. Ako već postoji besplatan servis za pretplatu, primjer aplikacije neće uspjeti. Samo jedan besplatnog servisa za pretraživanje po pretplati je dopušteno.

1. Otvaranje Program.cs iz preglednik rješenja i idite na funkciju glavne (niz [] Poništi). 
 
3. Obratite pozornost na to **ExecuteArmRequest** služi za izvršavanje zahtjeve u odnosu na krajnjoj točki Voditelj resursa Azure `https://management.azure.com/subscriptions` za određenu `subscriptionID`. Ova metoda se koristi u programu za izvođenje operacija pomoću Azure resursima API ili API upravljanje pretraživanja.

3. Zahtjevi za Azure Voditelj resursa mora biti provjere autentičnosti i dozvolu. To se postiže metodom **GetAuthorizationHeader** naziva metodom **ExecuteArmRequest** borrowed iz [zahtjeva za Voditelj resursa za Azure provjere autentičnosti](http://msdn.microsoft.com/library/azure/dn790557.aspx). Obavijest koja poziva **GetAuthorizationHeader** `https://management.core.windows.net` da biste dobili token za pristup.

4. Se od vas zatraži prijavite se pomoću korisničkog imena i lozinke koje vrijedi za vašu pretplatu.

5. Nakon toga novog servisa Azure pretraživanje je registriran za davatelja Azure Voditelj resursa. Ponovno je način **ExecuteArmRequest** , koji se koristi ovaj put za stvaranje servisa za pretraživanje na Azure za vašu pretplatu putem `providers/Microsoft.Search/register`. 

6. Ostatak program koristi [Azure pretraživanje upravljanje REST API -JA](http://msdn.microsoft.com/library/dn832684.aspx). Primijetit ćete da se `api-version` za taj API razlikuje se od upravitelja resursa Azure api-verzija. Na primjer, `/listAdminKeys?api-version=2014-07-31-Preview` prikazuje u `api-version` od Azure pretraživanje upravljanje REST API-JA.

    Sljedeći niz operacije dohvaćanje definicije servisa ste upravo stvorili administrator api-tipki, obnavlja dohvaća tipke, mijenja replike i particija i na kraju briše servis.

    Prilikom promjene servisa replike ili particije count očekuje da tu akciju neće uspjeti ako koristite besplatni edition. Možete unijeti samo standardni edition koristite dodatne particije i replike.

    Brisanje servis je zadnja operacija.

##<a name="next-steps"></a>Daljnji koraci

Nakon ovog praktičnog vodiča pojavljuju se završi, možda ćete morati dodatne informacije o Upravljanje servisom ili provjere autentičnosti sa servisom Active Directory:

- Dodatne informacije o integraciji klijentska aplikacija sustava Active Directory. U odjeljku [Integracija aplikacije servisa Azure Active Directory](http://msdn.microsoft.com/library/azure/dn151122.aspx).
- Informirajte se o drugim operacije upravljanja servisa Azure. Potražite u članku [Upravljanje servisa](http://msdn.microsoft.com/library/azure/dn578292.aspx).

<!--Anchors-->
[Download the sample application]: #Download
[Configure the application]: #config
[Explore the application]: #explore
[Next Steps]: #next-steps

<!--Image references-->
[5]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-Service.PNG
[6]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App.PNG
[7]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App-prop.PNG
[8]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPermissions.PNG
[9]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPage.PNG
[10]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-OAuthEndpoint.PNG

<!--Link references-->
[Manage your search solution in Microsoft Azure]: search-manage.md
[Azure Search development workflow]: search-workflow.md
[Create your first azure search solution]: search-create-first-solution.md
[Create a geospatial search app using Azure Search]: search-create-geospatial.md


 
