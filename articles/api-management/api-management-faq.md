<properties
    pageTitle="Upravljanje Azure API najčešća pitanja vezana uz | Microsoft Azure"
    description="Saznajte odgovore na najčešća pitanja, uzoraka i najbolje prakse u upravljanju Azure API-JA."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Najčešća pitanja za upravljanje Azure API-JA

Saznajte odgovore na najčešća pitanja, uzoraka i najbolje prakse za upravljanje Azure API-JA.

## <a name="frequently-asked-questions"></a>Najčešća pitanja

-   [Kako možete I postaviti tima za Microsoft Azure API Management pitanje?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Što znači kada je značajka u pretpregledu?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Kako sigurnu vezu između pristupnik za upravljanje API-JA i servisima pozadinske?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Kako kopirati Moje instanca servisa za upravljanje API-JA na nove instance?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Možete upravljati Moje instancu API upravljanje programski?](#can-i-manage-my-api-management-instance-programmatically)
-   [Kako dodati korisnika u grupu administratori?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Zašto je pravilo koje želite dodati nije dostupan u uređivaču pravila?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Kako koristiti API verzija u odjeljku Upravljanje API-JA?](#how-do-i-use-api-versioning-in-api-management)
-   [Kako postaviti više okruženja u jednom API-JA?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Mogu li koristiti SOAP s upravljanjem API-JA?](#can-i-use-soap-with-api-management)
-   [Je li upravljanje API pristupnik IP adresa konstantu? Mogu li se u koristiti pravila vatrozida?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Ću konfigurirati poslužitelj za autorizaciju za OAuth 2.0 s AD FS sigurnost?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Usmjeravanje načina upravljanja API koristiti u implementacijama na više zemljopisnog mjesta?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Mogu li koristiti predložak Voditelj resursa Azure da biste stvorili instanca servisa upravljanje API-JA?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Mogu li koristiti samopotpisani SSL certifikat za pozadini?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Zašto se neuspješne provjere autentičnosti prilikom pokušaja Kloniraj BROJKA spremište?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Upravljanje API funkcionira Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Možete prijeći na servis za upravljanje API iz jedne pretplate na drugu?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Kako možete I postaviti tima za Microsoft Azure API Management pitanje?

Kontaktirajte nas pomoću neke od ovih mogućnosti:

-   Objavite svoja pitanja u našem [forum MSDN upravljanje API -JA](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Slanje e-pošte <apimgmt@microsoft.com>.
-   Pošaljite nam zahtjev za značajku u na [forumu Azure povratne informacije](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Što znači kada je značajka u pretpregledu?

Kada je značajka u pretpregledu, znači da ne možemo aktivno traže povratne informacije o kako funkcionira značajka umjesto vas. Značajka u pretpregledu dovršetka functionally, ali je moguće koje ćemo napraviti prijelom promijeniti u povratnih. Preporučujemo da ne ovise o značajke koja je u pretpregledu u vašem radnom okruženju. Ako imate povratne informacije o značajkama pretpregled, obratite nam se keyboardshortcuts@Microsoft.commailto kroz jednu od opcija kontakata u [kako možete I postaviti tima za Microsoft Azure API Management pitanje?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Kako sigurnu vezu između pristupnik za upravljanje API-JA i servisima pozadinske?

Imate nekoliko mogućnosti za sigurnu vezu između pristupnik za upravljanje API-JA i pozadinske servisa. možeš:

-   Pomoću HTTP osnovnu provjeru autentičnosti. Dodatne informacije potražite u članku [Konfiguriranje API postavke](api-management-howto-create-apis.md#configure-api-settings).
- Korištenje SSL međusobna provjera autentičnosti kao što je opisano [kako sigurnost pozadinskih pomoću provjere autentičnosti certifikat klijenta u upravljanju Azure API -JA](api-management-howto-mutual-certificates.md).
- Pomoću IP stvaranja popisa dopuštenih stavki na servisu pozadinske. Ako imate standardni prikaz ili Premium sloju API upravljanje instancu komponente, IP adresa pristupnika ostaje konstante. Možete postaviti svoje whitelist da biste omogućili ovu IP adresu. IP adresa instance upravljanje API-JA možete dobiti na nadzornoj ploči za Azure portalu.
- Povezivanje vašoj instanci API upravljanje Azure virtualne mreže. Dodatne informacije potražite u članku [upute za postavljanje VPN veze u odjeljku Upravljanje Azure API -JA](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Kako kopirati Moje instanca servisa za upravljanje API-JA na nove instance?

Ako želite da biste kopirali instancu API upravljanje novu instancu imate nekoliko mogućnosti. možeš:

-   Pomoću sigurnosnog kopiranja i vraćanja funkcija u odjeljku Upravljanje API-JA. Dodatne informacije potražite u članku [Kako implementirati Izrada oporavak pomoću servisa sigurnosno kopiranje i vraćanje u upravljanju API Azure](api-management-howto-disaster-recovery-backup-restore.md).
-   Stvorite vlastiti sigurnosnu kopiju i vraćanje značajku pomoću [API upravljanje REST API -JA](https://msdn.microsoft.com/library/azure/dn776326.aspx). Da biste spremili i vraćanje entiteti iz instanca servisa koji želite koristiti REST API-JA.
-   Preuzmite konfiguracija servisa pomoću brojka i Prenesite novu instancu. Dodatne informacije potražite u članku [Spremanje i konfiguriranje konfiguracijom usluga upravljanja API pomoću brojka](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Možete upravljati Moje instancu API upravljanje programski?

Da, možete upravljati API upravljanje programski pomoću:

-   [Upravljanje API REST API-JA](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   [Biblioteka upravljanja ApiManagement servisa Microsoft Azure SDK](http://aka.ms/apimsdk).
-   [Uvođenje servisa](https://msdn.microsoft.com/library/mt619282.aspx) i [Upravljanje servisom](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell cmdleti.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Kako dodati korisnika u grupu administratori?

Evo kako dodati korisnika u grupu administratori:

1. Prijavite se na [portal za Azure](https://portal.azure.com).
2. Idite u grupu resursa koji ima API upravljanje instancu koju želite ažurirati.
3. U odjeljku Upravljanje API-JA, dodijeliti ulogu **Suradnika upravljanje Api** korisnika.

Novododani suradnik sada možete koristiti za Azure PowerShell [cmdleti za](https://msdn.microsoft.com/library/mt613507.aspx). Evo kako se prijaviti kao administrator:

1. Korištenje na `Login-AzureRmAccount` cmdleta za prijavu.
2. Postavljanje konteksta pretplatu koja je servis pomoću `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Dohvaćanje jedne prijave URL-a pomoću `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Pristup portalu za administratore pomoću URL-a.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Zašto je pravilo koje želite dodati nije dostupan u uređivaču pravila?

Ako je zasivljen ili osjenčan u uređivaču pravila, provjerite nalazite točan opseg pravila, pojavit će se pravilo koje želite dodati. Upute za korištenje u određenim opsega i sekcije pravila namijenjena svaki izjave pravila. Da biste pregledali pravila sekcije i opsega pravila, u odjeljku pravila korištenja u [pravila upravljanja API -JA](https://msdn.microsoft.com/library/azure/dn894080.aspx).


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Kako koristiti API verzija u odjeljku Upravljanje API-JA?

Imate nekoliko mogućnosti za korištenje rada s verzijama API-JA u odjeljku Upravljanje API-JA:

-   U odjeljku Upravljanje API-JA, možete konfigurirati API-ji za predstavljanje različitih verzija. Na primjer, možda imate dvije različite API-ji, MyAPIv1 i MyAPIv2. Razvojni inženjer možete odabrati na verziju koja programer želi koristiti.
-   Također možete konfigurirati na API-JA s URL-om, koji ne uključuje verziju segment, na primjer, https://my.api. Nakon toga konfigurirati segment verziju predlošku [Dopune URL-a](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) za svaki postupak. Na primjer, imate operaciju [URL predloška](api-management-howto-add-operations.md#url-template) pod nazivom /resource i [Dopune URL](api-management-howto-add-operations.md#rewrite-url-template) predloška pod nazivom /v1/Resource. Promijenite vrijednost segmenta verzije zasebno za svaku operaciju.
-   Želite li zadržati segment verziju "Zadano" u API servisa URL-a, na odabrane operacije, postavite pravila koja koristi pravila [Postavljanje pozadinskog servisa](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) da biste promijenili put pozadinske zahtjev.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Kako postaviti više okruženja u jednom API-JA?

Da biste postavili više okruženja, na primjer, okruženje za testiranje i okruženju proizvodnje u jedan API-JA, imate dvije mogućnosti. možeš:

-   Glavno računalo različite API-ji na istom klijentu.
-   Glavno računalo isti API-ji na različitim korisnicima.

### <a name="can-i-use-soap-with-api-management"></a>Mogu li koristiti SOAP s upravljanjem API-JA?

Podrška za [SOAP prolazni](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) sada je dostupan. Administratori mogu uvesti WSDL njihove SOAP servisa i upravljanje API Azure će stvoriti sučelja SOAP. Dokumentacija portala za razvojne programere, konzole za testiranje, pravila i analize su sve dostupne za servise SOAP.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Je li upravljanje API pristupnik IP adresa konstantu? Mogu li se u koristiti pravila vatrozida?

Pri razine standardnu i Premium javnu IP adrese (VIP) klijentu za upravljanje API namijenjen statične vijek klijent, uz neke iznimke. Promjena IP adresa u sljedećim okolnostima:

-   Servis je briše i ponovno stvoriti.
-   Pretplata servis je obustavljena (na primjer, za nonpayment), a zatim ponovo uspostavljena.
-   Dodavanje i uklanjanje Azure virtualne mreže (možete koristiti virtualne mreže samo u sloju Premium).

Za više područja implementacije adresu regionalne mijenja ako je područje vacated i zatim ponovo uspostavljena (možete koristiti više područja implementacije samo u sloju Premium).

Samoposlužni sloju Premium podešena za implementaciju više područja dodjeljuju jedan javnu IP adresu po regijama.

Na stranici klijenta na portalu za Azure možete dobiti na IP adresa (ili adrese više područja implementacije).

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Ću konfigurirati poslužitelj za autorizaciju za OAuth 2.0 s AD FS sigurnost?

Da biste saznali kako konfigurirati poslužitelj za autorizaciju OAuth 2.0 s sigurnošću Active Directory Federation Services (AD FS), potražite u članku [Korištenje ADFS u odjeljku Upravljanje API -JA](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Usmjeravanje načina upravljanja API koristiti u implementacijama na više zemljopisnog mjesta?

Upravljanje API koristi [način za usmjeravanje prometa performanse](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) u implementacijama na više zemljopisnog mjesta. Promet usmjeravanja najbliže API pristupnika. Ako određenu regiju vodi izvan mreže, dolazni promet automatski usmjeravanja sljedeći najbliže pristupnika. Saznajte više o usmjeravanje metode u [upravitelju promet postupku metode](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Mogu li koristiti predložak Voditelj resursa Azure da biste stvorili instanca servisa upravljanje API-JA?

Da. Pogledajte predložaka za brzi početak rada [Servisa za upravljanje Azure API -JA](http://aka.ms/apimtemplate) .

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Mogu li koristiti samopotpisani SSL certifikat za pozadini?

Da. Evo kako pomoću samopotpisanog certifikata Secure Sockets Layer (SSL) za pozadini:

1. Stvaranje [pozadinskog](https://msdn.microsoft.com/library/azure/dn935030.aspx) entitet pomoću upravljanja API-JA.
2. Postavite svojstvo **skipCertificateChainValidation** na **true**.
3. Ako više ne želite dopustiti samopotpisane potvrde, izbrisati entitet pozadinskog ili svojstvo **skipCertificateChainValidation** postavite na **false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Zašto se neuspješne provjere autentičnosti prilikom pokušaja Kloniraj brojka spremište?

Ako koristite upravitelj vjerodajnica brojka ili ako pokušavate Kloniraj brojka spremište pomoću Visual Studio, koje možete naići poznat problem s dijaloškim okvirom vjerodajnice sustava Windows. Dijaloški okvir ograničava duljinu lozinke za 127 znakova, a zatim ga skraćuje lozinke koje generira Microsoft. Radimo na ćete skratiti lozinku. Zasad pomoću tulumu brojka Kloniraj vaše brojka spremište.

### <a name="does-api-management-work-with-azure-expressroute"></a>Upravljanje API funkcionira Azure ExpressRoute?

Da. Upravljanje API funkcionira s Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Možete prijeći na servis za upravljanje API iz jedne pretplate na drugu?

Da. Dodatne informacije potražite u odjeljku [Premještanje resursa u novu grupu resursa ili pretplate](../resource-group-move-resources.md).
