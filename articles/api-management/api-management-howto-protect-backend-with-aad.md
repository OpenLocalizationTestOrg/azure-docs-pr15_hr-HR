<properties
    pageTitle="Kako zaštititi pozadinskog Web API s Azure Active Directory i upravljanje API-JA"
    description="Saznajte kako zaštititi pozadinskog Web API s Azure Active Directory i upravljanje API-JA." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Kako zaštititi pozadinska Web API s Azure Active Directory i upravljanje API-JA

U ovom videozapisu pokazuje kako sastavljanje Web API pozadinskog i zaštititi pomoću protokola OAuth 2.0 s Azure Active Directory i upravljanje API-JA.  Ovaj članak sadrži pregled i dodatne informacije o koracima u videozapisu. U ovom videozapisu 24 minute pogledajte upute za:

-   Stvaranje Web API pozadinska i sigurne s AAD - počevši od 1:30
-   Uvoz u API u upravljanje API - počevši od 7:10
-   Konfiguriranje portala za razvojne inženjere za pozivanje API - počevši od 9:09
-   Konfiguriranje aplikaciji za stolna računala da biste uputili poziv API - počevši od 18:08
-   Konfiguriranje pravilnika o provjeri valjanosti JWT sadržaja da biste unaprijed autorizirali zahtjeve - počevši od 20:47

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Stvaranje u imeniku Azure AD

Sigurnost Web API-jem sigurnosno pomoću Azure Active Directory najprije morate u klijent za AAD. U ovom ćete videozapisu koristi se klijentu pod nazivom **APIMDemo** . Da biste stvorili klijent AAD, prijave [Azure klasični Portal](https://manage.windowsazure.com) i kliknite **Novo**->**Aplikacije servisa**->**Servisa Active Directory**->**direktorija**->**Stvoriti prilagođene**. 

![Azure Active Directory][api-management-create-aad-menu]

U ovom primjeru direktorij pod nazivom **APIMDemo** stvara se pomoću zadanu domenu pod nazivom **DemoAPIM.onmicrosoft.com**. Taj imenik koristiti u cijeloj videozapis.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Stvaranje Web API servisa osigurava Azure Active Directory

U ovom ćete koraku Web API pozadinskog se stvara pomoću Visual Studio 2013. U ovom koraku videozapis započinje 1:30. Da biste stvorili Web API pozadinskog projekta u Visual Studio kliknite **datoteku**->**Novo**->**projekta**, a zatim odaberite **ASP.NET web-aplikaciju** na popisu predlošci za **Web** . U ovom ćete videozapisu projekta pod nazivom **APIMAADDemo**. Kliknite **u redu** da biste stvorili projekta. 

![Visual Studio][api-management-new-web-app]

Kliknite **Web API** iz u **Odaberite predložak popisa** da biste stvorili Web API projekta. Konfiguriranje provjere autentičnosti direktorija Azure kliknite **Promjena provjere autentičnosti**.

![Novi projekt][api-management-new-project]

Kliknite **Računi tvrtke ili ustanove**i navedite **domenu** AAD klijentu. U ovom primjeru domena je **DemoAPIM.onmicrosoft.com**. Domene direktorija možete dobivenog karticu **domene** direktorija.

![Domena][api-management-aad-domains]

Konfiguriranje željene postavke u dijaloškom okviru **Promjena provjere autentičnosti** , a zatim kliknite **u redu**.

![Promjena provjere autentičnosti][api-management-change-authentication]

Kada kliknete **u redu** Visual Studio će pokušati morate registrirati aplikacije Azure AD direktorija, a možete dobiti upit da biste se prijavili u Visual Studio. Prijavite se pomoću računa za administratora za direktorija.

![Prijavite se u Visual Studio][api-management-sign-in-vidual-studio]

Da biste konfigurirali taj projekt kao Azure Web API potvrdite okvir za **glavno računalo u oblaku** , a zatim kliknite **u redu**.

![Novi projekt][api-management-new-project-cloud]

Možete dobiti upit da biste se prijavili Azure, a zatim možete konfigurirati web-aplikaciji.

![Konfiguriranje][api-management-configure-web-app]

U ovom primjeru navedeni su nove **aplikacije servisa za planiranje** pod nazivom **APIMAADDemo** .

Kliknite **u redu** da biste konfigurirali web-aplikacije i stvorite projekt.

## <a name="add-the-code-to-the-web-api-project"></a>Dodavanje koda za projekt API na webu

Sljedeći korak u videozapisu dodaje kod u projekt Web API. Ovaj korak počinje 4:35.

API Web u ovom primjeru implementira osnovni Kalkulator servis pomoću modela i kontroler. Da biste dodali modela za servis, desnom tipkom miša kliknite **modela** u **Pregledniku rješenja** i odaberite **Dodaj**, **Predmet**. Naziv klase `CalcInput` i kliknite **Dodaj**.

Dodajte sljedeće `using` izjava na vrh na `CalcInput.cs` datoteku.

    using Newtonsoft.Json;

 Zamijenite klasu generirani sljedeći kod.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Desnom tipkom miša kliknite **kontrolera** u **Pregledniku rješenja** , a zatim odaberite **Dodaj**->**kontroler**. Odaberite **2 kontroler API Web - prazan** , a zatim kliknite **Dodaj**. Naziv kontrolera upišite **CalcController** , a zatim kliknite **Dodaj**.

![Dodavanje kontrolera][api-management-add-controller]

Dodajte sljedeće `using` izjava na vrh na `CalcController.cs` datoteku.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

Zamijenite predmete generirani kontroler sljedeći kod. Implementira kod u `Add`, `Subtract`, `Multiply`, a `Divide` operacije osnovni Kalkulator API-JA.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Pritisnite **F6** da biste Sastavljanje i provjerite je li rješenje.

## <a name="publish-the-project-to-azure"></a>Objavite projekt Azure

U ovom ćete koraku Visual Studio projekta Objavi Azure. U ovom koraku videozapis počinje 5:45.

Objavljivanje projekta Azure, desnom tipkom miša kliknite projekt **APIMAADDemo** u Visual Studio, a zatim odaberite **Objavi**. Zadrži zadane postavke u dijaloškom okviru **Objavljivanje Web** , a zatim kliknite **Objavi**.

![Objavljivanje na webu][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Dodjela dozvola za servisnu aplikaciju pozadinskog Azure AD

Nove aplikacije servisa za pozadinski se stvara u direktoriju Azure AD tijekom procesa konfiguriranje i objavljivanje Web API projekta. U ovom ćete koraku videozapisa, počevši od 6:13, dozvole odobravaju pozadinskog Web API.

![Aplikacija][api-management-aad-backend-app]

Kliknite naziv aplikacije da biste konfigurirali potrebne dozvole. Idite na karticu **Konfiguracija** i pomaknite se prema dolje do odjeljka **dozvole drugim aplikacijama** . Kliknite **Dozvole za aplikaciju** padajućeg izbornika pokraj **sustava Windows** **Azure Active Directory**, potvrdite okvir za **čitanje direktorija podatke**pa kliknite **Spremi**.

![Dodavanje dozvola][api-management-aad-add-permissions]

>[AZURE.NOTE] Ako **Windows** **Azure Active Directory** nije naveden u odjeljku dozvole drugim aplikacijama, kliknite **Dodaj aplikaciju** i dodajte ga s popisa.

Zabilježite **Id URI aplikacije** za korištenje u sljedećem koraku kada aplikacija Azure AD konfiguriran za upravljanje API portala za razvojne inženjere.

![Id aplikacije URI-JA][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Uvoz u API na webu u upravljanje API-JA

API-ji konfigurirane s portala za publisher API kojoj se pristupa putem klasične portala za Azure. Dosegne portal za publisher, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa upravljanje API -JA][] u vodič za [Upravljanje prvi API-JA][] .

![Portal za Publisher][api-management-management-console]

Operacije može biti [dodali API-ji](api-management-howto-add-operations.md)ili moguće je uvesti. U ovom ćete videozapisu operacije uvoza u obliku Swagger počevši od 6:40.

Stvorite datoteku pod nazivom `calcapi.json` sljedeće sadržajem i spremite ga na računalo. Pobrinite se da u `host` atributa točke u vašem pozadinskog Web API. U ovom primjeru `"host": "apimaaddemo.azurewebsites.net"` koristi.

{"swagger": "2.0", "informacije": {"Naslov": "Kalkulator", "Opis": "Arithmetics putem HTTP-a!", "verzije": "1.0"}, "glavno računalo": "apimaaddemo.azurewebsites.net", "basePath": "/ API-ja", "sheme": ["http"], "putova": {"/ Dodavanje? na = {a} i b = {b}": {"Dohvati": {"Opis": "Odgovori s funkcijom sum dvaju brojeva.", "operationId": "Dodavanje dvije decimale", "parametara": [{"naziv": "a", "u": "za upite", "Opis": "prvi operand. Zadana vrijednost je <code>51</code>. ","potrebno": true,"Zadano":"51","redni broj": ["51"]}, {"naziv":"b";"u":"za upite","Opis":"drugi operand. Zadana vrijednost je <code>49</code>. ","potrebno": true,"Zadano":"49","redni broj": ["49"]}],"odgovora": {}}}," / sub?a = {a} & b = {b} ": {"Dohvati": {"Opis":"Odgovori s razlika između dvaju brojeva.","operationId":"Oduzmi dvije decimale","parametara": [{"naziv":"a","u":"za upite","Opis":"prvi operand. Zadana vrijednost je <code>100</code>. ","potrebno": true,"Zadano":"100","redni broj": ["100"]}, {"naziv":"b";"u":"za upite","Opis":"drugi operand. Zadana vrijednost je <code>50</code>. ","potrebno": true,"Zadano":"50","redni broj": ["50"]}],"odgovora": {}}}," / div?a = {a} & b = {b} ": {"Dohvati": {"Opis":"Odgovori s na Kvocijent dvaju brojeva.","operationId":"Dijeljenje dvije cijele brojeve","parametara": [{"naziv":"a","u":"za upite","Opis":"prvi operand. Zadana vrijednost je <code>100</code>. ","potrebno": true,"Zadano":"100","redni broj": ["100"]}, {"naziv":"b";"u":"za upite","Opis":"drugi operand. Zadana vrijednost je <code>20</code>. ","potrebno": true,"Zadano":"20,""redni broj": ["20"]}],"odgovora": {}}}," / mul?a = {a} & b = {b} ": {"Dohvati": {"Opis":"Odgovori s proizvodom dvaju brojeva.","operationId":"Pomnoži dvije cijele brojeve","parametara": [{"naziv":"a","u":"za upite","Opis":"prvi operand. Zadana vrijednost je <code>20</code>. ","potrebno": true,"Zadano":"20,""redni broj": ["20"]}, {"naziv":"b";"u":"za upite","Opis":"drugi operand. Zadana vrijednost je <code>5</code>. ","potrebno": true,"Zadano":"5","redni broj": ["5"]}],"odgovora": {}}}}}

Da biste uvezli Kalkulator API-JA, kliknite **API-ji** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim **Uvoza API -JA**.

![Gumb Uvezi API-JA][api-management-import-api]

Izvršite sljedeće korake da biste konfigurirali Kalkulator API-JA.

1. Kliknite **iz datoteke**, pronađite u `calculator.json` ste spremili datoteku, a zatim kliknite izborni gumb **Swagger** .
2. U **web-URL API sufiks** tekstni okvir upišite **Izračunaj** .
3. Kliknite u okvir **proizvoda (nije obavezno)** i odaberite **Starter**.
4. Kliknite **Spremi** da biste uvezli u API-JA.

![Dodavanje novog API-JA][api-management-import-new-api]

Nakon uvoza s API prikazat će se sažetak stranica za API Sučelja na portalu za publisher.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Poziv u API neuspješno s portala za razvojne inženjere

Sada na API uvezena je u API upravljanje, ali ne još može pozvati uspješno s portala za razvojne inženjere jer servis pozadinska je zaštićena Azure AD provjere autentičnosti. To je planirati videozapis počevši od 7:40 na sljedeći način.

Kliknite **portala za razvojne inženjere** od vrha desnog ruba portala za publisher.

![Portala za razvojne inženjere][api-management-developer-portal-menu]

Kliknite **API-ji** pa **Kalkulator** API-JA.

![Portala za razvojne inženjere][api-management-dev-portal-apis]

Kliknite **isprobajte**.

![Isprobajte][api-management-dev-portal-try-it]

Kliknite **Pošalji** i zabilježite status odgovora **401 Unauthorized**.

![Pošalji][api-management-dev-portal-send-401]

Zahtjev nije ovlašten jer pozadinskog API zaštićen Azure Active Directory. Prije uspješno pozivanja u API razvojni portal morate ga konfigurirati da biste autorizirali razvojnim inženjerima pomoću OAuth 2.0. Ovaj postupak je opisan u sljedećim odjeljcima.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Registrirajte se kao aplikaciju AAD portala za razvojne inženjere

Prvi korak u konfiguraciji portala za razvojne inženjere da biste autorizirali razvojnim inženjerima pomoću OAuth 2.0 je da biste registrirali portala za razvojne inženjere kao AAD aplikaciju. To je planirati počevši od 8:27 u videozapisu.

Dođite do klijentu Azure AD iz prvog koraka u ovom primjeru **APIMDemo** ovaj videozapis, a zatim otvorite karticu **programi** .

![Nove aplikacije][api-management-aad-new-application-devportal]

Kliknite gumb **Dodaj** da biste stvorili novu aplikaciju Azure Active Directory, a zatim odaberite **Dodaj je moje tvrtke ili ustanove razvoju aplikacija**.

![Nove aplikacije][api-management-new-aad-application-menu]

Odaberite **web-aplikacije i/ili Web API**, unesite naziv pa kliknite strelicu za sljedeće. U ovom se primjeru koristi **APIMDeveloperPortal** .

![Nove aplikacije][api-management-aad-new-application-devportal-1]

**URL za prijavu** unesite URL servis za upravljanje API-JA i dodavanje `/signin`. U ovom se primjeru koristi **https://contoso5.portal.azure-api.net/signin **.

Za **Aplikacije Id URL** unesite URL servis za upravljanje API-JA, a zatim dodati neke jedinstvene znakove. To može biti sve željene znakove i u ovom primjeru se koristi **https://contoso5.portal.azure-api.net/dp** . Kada niste konfigurirali željenim **Svojstva aplikacije** , kliknite kvačicu da biste stvorili aplikacije.

![Nove aplikacije][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Konfiguriranje poslužitelja za autorizaciju API upravljanje OAuth 2.0

Sljedeći je korak konfiguriranje poslužitelja za autorizaciju OAuth 2.0 u odjeljku Upravljanje API-JA. Videozapis počevši od 9:43 je planirati ovaj korak.

Na izborniku Upravljanje API-JA na lijevoj strani kliknite **Sigurnost** , kliknite **OAuth 2.0**, a zatim **Dodaj provjeru autentičnosti** poslužitelja.

![Dodavanje provjeru autentičnosti poslužitelja][api-management-add-authorization-server]

Unesite naziv i neobavezan opis u poljima **naziv** i **Opis** . Ta polja koriste se za identificiranje autorizacija server OAuth 2.0 unutar instanca servisa za upravljanje API-JA. U ovom se primjeru koristi **provjeru autentičnosti poslužitelja pokazni** . Kasnije će kada Odredite poslužitelj za OAuth 2.0 koja će se koristiti za provjeru autentičnosti API, odaberite taj naziv.

Za **URL stranice za registraciju klijenta** unesite vrijednost za rezervirano mjesto kao što su `http://localhost`.  **URL stranice za registraciju klijent** upućuje na stranici Korisnici mogu koristiti za stvaranje i konfiguriranje svoje račune za OAuth 2.0 davatelji usluga koji podržavaju upravljanje korisnicima računa. U ovom primjeru korisnici ne stvorite i svoje račune konfigurirati tako da koristi rezervirano mjesto.

![Dodavanje provjeru autentičnosti poslužitelja][api-management-add-authorization-server-1]

Nakon toga navedite **URL krajnje autorizacije** i **Token URL ADRESU krajnje točke**.

![Provjeru autentičnosti poslužitelja][api-management-add-authorization-server-1a]

Te vrijednosti možete biti dohvaćeni iz stranici **Krajnje točke aplikacije** programa AAD koju ste stvorili za portala za razvojne inženjere. U programu access krajnjih točaka idite na karticu **Konfiguracija** za AAD aplikaciju i kliknite **Prikaz krajnje točke**.

![Aplikacija][api-management-aad-devportal-application]

![Prikaz krajnje točke][api-management-aad-view-endpoints]

Kopirajte **krajnja točka za autorizaciju OAuth 2.0** i zalijepite u tekstni okvir **URL autorizacije krajnjoj točki** .

![Dodavanje provjeru autentičnosti poslužitelja][api-management-add-authorization-server-2]

Kopirajte **OAuth 2.0 tokena krajnjoj točki** i zalijepite u tekstni okvir **Token krajnju točku URL** .

![Dodavanje provjeru autentičnosti poslužitelja][api-management-add-authorization-server-2a]

Osim lijepite u krajnju točku tokena, dodajte je parametar dodatne tijelo pod nazivom **resursa** i za korištenje vrijednost **URI Id aplikacije** iz AAD aplikacije servisa za pozadinski koja je stvorena kada je objavljen projekta za Visual Studio.

![Id aplikacije URI-JA][api-management-aad-sso-uri]

Nakon toga navedite vjerodajnice klijenta. To su vjerodajnice za resurs koji želite pristupiti, u ovom slučaju pozadinskog servisa.

![Vjerodajnice za klijenta][api-management-client-credentials]

Da biste dobili **Id klijenta**, idite na karticu **Konfiguracija** AAD aplikacije servisa za pozadinskog sustava i kopirajte **Id klijenta**.

Da biste dobili kliknite **Klijent tajna** **Odaberite trajanje** padajućeg popisa u odjeljku **tipke** i odredite interval. U ovom se primjeru koristi godinu.

![ID klijenta][api-management-aad-client-id]

Kliknite **Spremi** da biste spremili konfiguraciju i prikaz tipku. 

>[AZURE.IMPORTANT] Zabilježite ovog ključa. Nakon toga zatvorite prozor konfiguracije Azure Active Directory, ključ nije moguće ponovno prikazati.

Kopiranje tipku u međuspremnik, vratite se na portal za publisher, zalijepite ključ u tekstni okvir **Tajna klijenta** i kliknite **Spremi**.

![Dodavanje provjeru autentičnosti poslužitelja][api-management-add-authorization-server-3]

Odmah slijede vjerodajnice za klijent je programa grant autorizacije kod. Kopirajte kod za provjeru autentičnosti i vratite se na portal aplikacije za razvojne inženjere Azure AD konfiguriranje stranica i lijepljenje autorizacija dodijeliti u polje **URL odgovor** pa ponovno kliknite **Spremi** .

![URL adresa za odgovor][api-management-aad-reply-url]

Sljedeći je korak Konfiguriranje dozvola za aplikaciju AAD portala za razvojne inženjere. Kliknite **Dozvole za aplikaciju** , a zatim potvrdite okvir za **čitanje direktorija podatke**. Kliknite **Spremi** da biste spremili te promjene, a zatim kliknite **Dodaj aplikaciju**.

![Dodavanje dozvola][api-management-add-devportal-permissions]

Kliknite ikonu za pretraživanje upišite **APIM** u započinje s okviru odaberite **APIMAADDemo**i kliknite kvačicu da biste spremili.

![Dodavanje dozvola][api-management-aad-add-app-permissions]

Kliknite **Dodijeliti dozvole** za **APIMAADDemo** , potvrdite okvir pokraj **Access APIMAADDemo**i kliknite **Spremi**. Time se omogućuje programer aplikacije portala za pristup servisu pozadinskog.

![Dodavanje dozvola][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Omogućivanje OAuth 2.0 autorizacija korisnika za Kalkulator API-JA

Sad kad je konfiguriran server OAuth 2.0, možete je odrediti u sigurnosnim postavkama za vaše API-JA. Ovaj korak je planirati videozapis počevši od 14:30.

Na lijevom izborniku kliknite **API-ji** pa kliknite **Kalkulator** da biste pogledali i konfigurirati svoje postavke.

![Kalkulator API-JA][api-management-calc-api]

Idite na karticu **Sigurnost** , potvrdite okvir **OAuth 2.0** , odaberite željeni autorizacije poslužitelj iz **provjeru autentičnosti poslužitelja** padajući popis i kliknite **Spremi**.

![Kalkulator API-JA][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Uspješno pozvati Kalkulator API-JA s portala za razvojne inženjere

Sad kad autorizacije OAuth 2.0 konfiguriran na na API-JA, operacije može uspješno pozvati iz centra za razvojne inženjere. Videozapis počevši od 15:00 se planirati ovaj korak.

Vratite se na postupka **Dodaj dvije decimale** Kalkulator servisa na portalu za razvojne inženjere, a zatim kliknite **isprobajte**. Imajte na umu nove stavke u odjeljku **autorizacije** odgovara provjeru autentičnosti poslužitelja koji ste upravo dodali.

![Kalkulator API-JA][api-management-calc-authorization-server]

Odaberite **autorizacija kod** s padajućeg popisa za provjeru autentičnosti i unesite vjerodajnice za račun koji želite koristiti. Ako ste se već prijavili pomoću računa koji se možda neće biti zatražit.

![Kalkulator API-JA][api-management-devportal-authorization-code]

Kliknite **Pošalji** i zabilježite **status odgovora** **200 u redu** i rezultata operacije u sadržaju odgovor.

![Kalkulator API-JA][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Konfiguriranje aplikaciji za stolna računala da biste uputili poziv na API-JA

Sljedeći postupak u videozapisu počinje 16:30 i konfigurira jednostavne aplikaciji za stolna računala da biste uputili poziv na API-JA. Prvi korak je registrirati aplikaciji za stolna računala u Azure AD i dajte mu pristup imeniku i sa servisom pozadinskog. Pri 18:25 postoji pokazni o aplikaciji za stolna računala za pozivanje operaciju na kalkulator API-JA.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Konfiguriranje pravila provjere valjanosti JWT da biste unaprijed autorizirali zahtjeva

Konačni postupak u videozapisu počinje 20:48 i pokazuje kako koristiti pravila za [Provjeru valjanosti JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) da biste unaprijed autorizirali zahtjevi po pristupna tokena svakog dolaznih zahtjeva za provjeru valjanosti. Ako je zahtjev ne provjerava pravila za provjeru valjanosti JWT, zahtjev je blokirala upravljanje API-JA i se ne prenose duž pozadinski.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Drugi pokazni konfiguriranje i korištenje ovo pravilo potražite u članku [177 epizode pokrivaju oblaka: više API značajke upravljanja](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i brzi prijelaz naprijed do 13:50. Fokusiranje 15:00 do potražite u članku pravila konfiguriran u uređivaču pravila, a zatim 18:50 pokazni pozivanja operaciju s portala za razvojne inženjere s i bez potrebna ovlaštenja.

## <a name="next-steps"></a>Daljnji koraci
-   Pogledajte [videozapise](https://azure.microsoft.com/documentation/videos/index/?services=api-management) o upravljanju API-JA.
-   Drugi načini vaše pozadinskog servis za sigurnu, potražite u članku [Provjera autentičnosti međusobna potvrda](api-management-howto-mutual-certificates.md) i [Povezivanje putem VPN-a ili ExpressRoute](api-management-howto-setup-vpn.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance
[Upravljanje prvi API-JA]: api-management-get-started.md
