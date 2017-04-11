<properties 
    pageTitle="Kako da biste autorizirali račune za razvojne inženjere pomoću OAuth 2.0 u upravljanju API Azure" 
    description="Saznajte kako da biste autorizirali korisnika putem OAuth 2.0 u odjeljku Upravljanje API-JA." 
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

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Kako da biste autorizirali račune za razvojne inženjere pomoću OAuth 2.0 u upravljanju API Azure

Mnoge API-ji podržavaju [OAuth 2.0](http://oauth.net/2/) sigurne na API-JA i provjerite je li valjan korisnici imaju pristup, i oni mogli pristupati resursa na koji imate pravo. Da biste koristili Azure API Management Console za programiranje interaktivne s takve API-ji, servis omogućuje konfiguriranje vaše instanca servisa da biste radili s vašeg OAuth 2.0 omogućeno API-JA.

## <a name="prerequisites"> </a>Preduvjeti

Ovaj vodič objašnjava kako konfigurirati svoje instanca servisa API upravljanje da biste koristili OAuth 2.0 autorizacije za račune za razvojne inženjere, ali nije vam pokazati kako konfigurirati davateljem OAuth 2.0. Konfiguracija za svaku tražilicu OAuth 2.0 razlikuje, iako slični su koraci, a isti su potrebne dijelove informacije koje se koriste u konfiguriranju OAuth 2.0 u instanci upravljanje API servisa. Ova tema prikazuje primjere pomoću servisa Azure Active Directory kao davateljem OAuth 2.0.

>[AZURE.NOTE] Dodatne informacije o konfiguriranju OAuth 2.0 pomoću servisa Azure Active Directory potražite u članku [WebApp GraphAPI DotNet][] uzorka.

## <a name="step1"> </a>Konfiguriranje poslužitelja autorizacije OAuth 2.0 u odjeljku Upravljanje API-JA

Da biste započeli, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Tako ćete doći do portala za upravljanje API programa publisher.

![Portal za Publisher][api-management-management-console]

>[AZURE.NOTE] Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

Na izborniku **Upravljanje API -JA** na lijevoj strani kliknite **Sigurnost** , kliknite **OAuth 2.0**, a zatim **Dodaj provjeru autentičnosti poslužitelja**.

![OAuth 2.0][api-management-oauth2]

Nakon što kliknete **Dodaj provjeru autentičnosti poslužitelja**, prikazuje se novi obrazac za autorizaciju poslužitelja.

![Novi poslužitelj][api-management-oauth2-server-1]

Unesite naziv i neobavezan opis u poljima **naziv** i **Opis** . 

>[AZURE.NOTE] Ta polja koje se koriste za prepoznavanje autorizacije server OAuth 2.0 unutar trenutne instance servisa za upravljanje API-JA i njihove vrijednosti ne dolaze iz server OAuth 2.0.

Unesite **URL stranice za registraciju klijenta**. Ta je stranica gdje korisnici možete stvoriti i upravljati svoje račune i ovisi o OAuth 2.0 davatelj koji se koristi. **URL stranice za registraciju klijent** upućuje na stranici Korisnici mogu koristiti za stvaranje i konfiguriranje svoje račune za OAuth 2.0 davatelji usluga koji podržavaju upravljanje korisnicima računa. Neke tvrtke i ustanove konfigurirati ili koristite tu funkciju čak i ako to podržava usluga OAuth 2.0. Ako vaš davatelj usluga OAuth 2.0 Upravljanje korisnicima računa konfigurirana, unesite URL rezervirano mjesto ovdje kao što su URL-a vaše tvrtke ili URL-a kao što su `https://placeholder.contoso.com`.

U sljedećem odjeljku obrazac sadrži postavke **autorizacije kod dodijeliti vrste**, **URL ADRESU krajnje točke autorizacije**i **način zahtjev za provjeru autentičnosti** .

![Novi poslužitelj][api-management-oauth2-server-2]

Navedite **autorizacije kod dodijeliti vrste** tako da željene vrste. **Kod autorizacije** navedena je prema zadanim postavkama.

Unesite **URL ADRESU krajnje točke za autorizaciju**. Za Azure Active Directory, URL će biti sličan sljedeći URL, gdje `<client_id>` je zamijenjena funkcijom id klijenta koja služi za identifikaciju aplikacije s poslužiteljem OAuth 2.0.

    https://login.windows.net/<client_id>/oauth2/authorize

**Način zahtjev za autorizaciju** određuje način na koji je zahtjev za autorizaciju šalju server OAuth 2.0. Po zadanom **se** odabran.

U sljedećem odjeljku je u kojoj su navedeni **u Token krajnjoj točki URL**, **metoda provjere autentičnosti za klijenta**, **Slanje način token za pristup**i **Zadani opseg** .

![Novi poslužitelj][api-management-oauth2-server-3]

Za poslužitelj za Azure Active Directory OAuth 2.0 **Token krajnjoj točki URL** će automatski dobiti sljedećem obliku, gdje `<APPID>` je oblik `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

Zadana postavka **klijent načina** provjere autentičnosti **Osnovni**, a **token za pristup slanja način** **autorizacije zaglavlja**. U ovom se odjeljku obrasca, uz **Zadani opseg**konfigurirana te vrijednosti.

U odjeljku **klijentski vjerodajnice** sadrži **ID klijenta** i **tajna klijenta**, koji se dobivaju se tijekom postupka stvaranja i konfiguraciji poslužitelja OAuth 2.0. Kada su navedeni **ID klijenta** i **tajna klijent** , generirat ćete **redirect_uri** za **autorizaciju kod** . U ovom URI koristi se za konfiguriranje URL adresa za odgovor u konfiguraciju poslužitelja OAuth 2.0.

![Novi poslužitelj][api-management-oauth2-server-4]

Ako je **kod autorizacije dodijeliti vrste** postavljeno na **Lozinka vlasnika resursa**, u odjeljku **resursa vlasnik lozinke vjerodajnice** se koristi za određivanje te vjerodajnice u suprotnom možete ostaviti je prazno.

![Novi poslužitelj][api-management-oauth2-server-5]

Nakon dovršetka obrasca, kliknite **Spremi** da biste spremili konfiguraciju poslužitelja za autorizaciju API upravljanje OAuth 2.0. Nakon spremanja konfiguraciju poslužitelja možete konfigurirati API-ji da biste koristili tu konfiguraciju, kao što je prikazano u sljedećem odjeljku.

## <a name="step2"> </a>Konfiguriranje API-JA za korištenje autorizacija korisnika OAuth 2.0

Kliknite **API-ji** na izborniku **Upravljanje API -JA** na lijevoj strani, kliknite naziv željene API, kliknite **Sigurnost**, a zatim potvrdite okvir pokraj **OAuth 2.0**.

![Autorizacija korisnika][api-management-user-authorization]

Odaberite željeni **provjeru autentičnosti poslužitelja** s padajućeg popisa, a zatim kliknite **Spremi**.

![Autorizacija korisnika][api-management-user-authorization-save]

## <a name="step3"> </a>Testiranje autorizacija korisnika OAuth 2.0 na portalu za razvojne inženjere

Nakon što ste konfigurirali poslužitelj za autorizaciju OAuth 2.0 i konfigurirali API-JA za korištenje tog poslužitelja, možete je testirati tako da portala za razvojne inženjere i pozivanje API.  Kliknite **portala za razvojne inženjere** u gornji desni izbornik.

![Portala za razvojne inženjere][api-management-developer-portal-menu]

Kliknite **API-ji** u gornji izbornik i odaberite **API jeku**.

![Echo API][api-management-apis-echo-api]

>[AZURE.NOTE] Ako imate samo jedan API konfiguriran ili vidljiva na vaš račun, zatim kliknete API-ji vas vodi izravno operacije za taj API-JA.

Odabir **Resursa za početak** operacije, kliknite **Otvorene konzole**, a zatim s padajućeg popisa odaberite **autorizacija kod** .

![Otvorene konzole][api-management-open-console]

Kada je odabrana **autorizacije kod** , skočni prozor prikazuju se obrazac za prijavu davatelja OAuth 2.0. U ovom primjeru obrazac za prijavu je nudi Azure Active Directory.

>[AZURE.NOTE] Ako imate skočnih prozora onemogućuje koji će se zatražiti da ih omogućiti web-pregledniku. Nakon što ih omogućili, odaberite **autorizacija kod** ponovno i u obrazac za prijavu će se prikazati.

![prijavi se][api-management-oauth2-signin]

Kada ste prijavljeni, **zaglavlja zahtjeva** popunjavaju se programa `Authorization : Bearer` zaglavlje da neadministratorskog zahtjev.

![Zahtjev za zaglavlje tokena][api-management-request-header-token]

Sada možete konfigurirati željene vrijednosti za parametre preostale i pošaljite zahtjev. 

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korištenju OAuth 2.0 i upravljanje API potražite u ovom videozapisu i popratnim [članka](api-management-howto-protect-backend-with-aad.md).

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp GraphAPI DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

