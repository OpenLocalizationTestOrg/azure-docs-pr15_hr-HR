<properties
    pageTitle="Stvaranje i korištenje Interna opterećenja s okruženjem aplikacije servisa | Microsoft Azure"
    description="Stvaranje i korištenje programa elika i mala slova pomoću programa ILB"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Interna opterećenja pomoću aplikacije servisa za okruženja #

Aplikacije servisa Environments(ASE) značajka je Premium servis servisa Azure aplikacije koje nudi poboljšane konfiguracije mogućnost povezivanja koja nije dostupna u žigovima više klijenta.  Značajka elika i mala slova zapravo uvodi aplikacije servisa Azure u svoje Azure virtualne Network(VNet).  Da bi se dobio veći razumijevanja mogućnosti koje nudi aplikacije servisa okruženja Saznajte [što je okruženju aplikacije servisa] [ WhatisASE] dokumentacije.  Ako ne znate prednosti u na VNet čitati [Azure virtualne mreže najčešća pitanja vezana uz][virtualnetwork].  


## <a name="overview"></a>Pregled ##


Na elika i mala slova može uvesti s krajnje pristupačnih internet ili s IP adresom u vašem VNet.  Da bi se s IP adresom postavite na adresu VNet morate za implementaciju sustava elika i mala slova s internim Balancer(ILB) za učitavanje.  Kada vaš elika i mala slova je konfiguriran za korištenje programa ILB unesete:

- vlastite domene ili poddomene.  Da biste se lakše, ovaj dokument pretpostavlja poddomene, ali možete konfigurirati oba načina.  
- certifikat koji je korišten za HTTP
- Upravljanje DNS-om za vaše poddomene.  


Povrat, biste mogli obavljati zadatke kao što su:

- glavno računalo intranetu aplikacije, kao što su poslovnim aplikacijama sigurno u oblaku kojem pristupiti putem web-mjesta web-mjestu ili ExpressRoute VPN-a
- glavno računalo aplikacije u oblak koji su navedeni u javnom DNS poslužitelja
- Stvaranje internet Izolirani pozadinskog aplikacije koje sučelja aplikacije sigurno možete integrirati s


#### <a name="disabled-functionality"></a>Onemogućeni funkcija ####

Evo nekoliko stvari koje ne možete pri korištenju programa ILB elika i mala slova.  Te stavke obuhvaćaju sljedeće:

- Korištenje IPSSL
- Dodjeljivanje IP adrese na određene aplikacije
- kupnjom i certifikat pomoću aplikacije putem portala sustava.  Naravno još uvijek možete nabaviti potvrde izravno s ustanove za izdavanje certifikata i koristiti ga s aplikacijama, no ne i putem portala za Azure.


## <a name="creating-an-ilb-ase"></a>Stvaranje programa ILB elika i mala slova ##

Stvaranje programa elika i mala slova ILB nije mnogo različitih obično stvaranje elika programa i mala slova.  Za detaljnije raspravu o stvaranju programa elika i mala slova pročitajte [kako stvoriti okruženju aplikacije servisa][HowtoCreateASE].  Postupak stvaranja programa ILB elika i mala slova se ne razlikuje između stvaranja na VNet tijekom stvaranja elika i mala slova ili odabira postojećih VNet.  Da biste stvorili programa elika i mala slova ILB: 

1.  Na portalu za Azure odaberite **Web -> novo + Mobile -> okruženja aplikacije servisa**
2.  Odaberite pretplatu
3.  Odaberite ili stvorite grupu resursa
4.  Odaberite ili stvorite na VNet
5.  Stvaranje podmreži Ako odaberete u VNet
6.  Odaberite **virtualne mrežno/mjesto-> VNet konfiguraciju** i postaviti vrstu VIP na internog
7.  Navedite naziv poddomene (to će biti poddomene koji se koriste za aplikacije stvorene u ovom elika i mala slova)
8.  Odaberite u redu, a zatim stvorite


![][1]


Unutar plohu virtualne mreže postoji mogućnost za konfiguraciju VNet.  Odaberite između vanjskih VIP ili interne VIP omogućuje.  Zadana je vrijednost vanjskog izvora.  Ako ste ga postavite na vanjski elika vaše i mala slova će koristiti VIP dostupna na internet.  Ako ste odabrali internog, vaše elika i mala slova će konfigurirati programa ILB na IP adresu unutar vaše VNet.  


Nakon odabira internog mogućnost da biste dodali više IP adresa vaša elika i mala slova se uklanja, a umjesto toga morate unijeti poddomene na elika i mala slova.  U programa elika i mala slova s vanjskim VIP naziv u elika i mala slova koristi u poddomena za aplikacije stvorene u tom elika i mala slova.  
Ako vaš elika i mala slova je pod nazivom ***contosotest*** i aplikacije u koji je pozvan elika i mala slova ***mytest*** zatim poddomene će biti oblikovanje ***contosotest.p.azurewebsites.net*** i URL-a za tu aplikaciju bio ***mytest.contosotest.p.azurewebsites.net***.  
Ako vrstu VIP postavljena na internog, naziv elika i mala slova se ne koristi u poddomena za elika na i mala slova.  Navedite poddomena izričito.  Ako vaš poddomene je ***contoso.corp.net*** , a vi ste aplikacije iz tog elika i mala slova naziva ***timereporting*** URL-a za tu aplikaciju bio bi ***timereporting.contoso.corp.net***.


## <a name="apps-in-an-ilb-ase"></a>Aplikacija u programa ILB elika i mala slova ##

Stvaranje aplikacije u programa elika i mala slova ILB je isti kao obično stvaranje aplikacije programa elika i mala slova.  

1. Na portalu za Azure odaberite **Web -> novo + Web -> mobilni** **Mobile** ili **Aplikacije API -JA**
2. Unesite naziv aplikacije
2. Odaberite pretplatu
3. Odaberite ili stvorite grupu resursa
4. Odaberite ili stvorite aplikaciju servisa Plan(ASP).  Stvaranje nove ASP elika vaše i mala slova kao odaberite mjesto pa odaberite skup tempiranja želite vaše ASP će biti stvoren u.  Prilikom stvaranja ASP kao mjesto i skup radnih odaberete elika vaše i mala slova.  Kada odredite naziv aplikacije vidjet ćete da poddomene ispod naziva aplikacije zamjenjuje poddomena za svoje elika i mala slova.   
5. Odaberite Stvori.  Trebali biste potvrdite okvir **Prikvači na nadzornoj ploči** ako želite da se u aplikaciju koja će se prikazivati na nadzornu ploču.  

![][2]


U odjeljku naziv aplikacije u poddomene ažurira se u skladu s vizualnim poddomene vaše elika i mala slova.  


## <a name="post-ilb-ase-creation-validation"></a>Stvaranje provjere valjanosti za objavu ILB elika i mala slova ##

Na elika i mala slova ILB je malo drugačije nego u ne - ILB elika i mala slova.  Što su već Primijetit ćete morate upravljati vlastitim DNS te morate unijeti vlastiti certifikat za HTTPS veze.  


Kada stvorite vaše elika i mala slova Primijetit ćete da poddomena prikazuje poddomena koju ste naveli, a nalazi se nove stavke u izborniku **Postavke** naziva **ILB certifikata**.  U elika i mala slova stvara se pomoću samopotpisanog certifikata, što olakšava testiranje HTTPS.  Na portalu će vas obavještava da vam trebaju vlastiti certifikat za HTTPS, ali to je na pogon vam certifikat koji odgovara vašem poddomene.  

![][3]


Ako samo isprobavate stvari, a ne znate kako stvoriti certifikat, možete koristiti aplikacije konzole IIS BLOG da biste stvorili prošao na samostalnom traje certifikata.  Nakon stvaranja izvesti kao .pfx datoteka, a zatim prijenos u korisničkom Sučelju ILB certifikata. Prilikom pristupa web-mjestu sigurnu vezu s samopotpisani certifikat, vaš preglednik steći ćete upozorenje da web-mjesto kojem pristupate nije sigurna zbog nemogućnosti za provjeru valjanosti certifikata.  Ako želite da biste izbjegli taj upozorenje morate pravilno traje certifikat koji odgovara vašem poddomene i ima lanac pouzdanost koji prepoznaje vaš preglednik.

![][6]

Ako želite isprobati tijek s vlastitih potvrda i testiranje HTTP i HTTPS pristup vašem elika i mala slova:

1.  Idite na korisničko Sučelje elika i mala slova nakon stvaranja elika i mala slova **elika i mala slova -> Postavke -> certifikati ILB**
2.  Postavite ILB certifikata tako da odaberete datoteka certifikata pfx i lozinku.  Ovaj korak vodi neko vrijeme da obradi i prikazat će se poruka koja je u tijeku skaliranja operacija.
3.  Dobiti adresu ILB za svoje elika i mala slova (**elika i mala slova -> Svojstva -> virtualna IP adresa**)
4.  Stvaranje web-aplikacijama u elika i mala slova nakon stvaranja 
5.  Stvaranje na VM ako ga nemate u tom VNET (ne u istoj podmreži prijelom elika i mala slova ili elemente)
6.  Postavljanje DNS-a za vaše poddomene.  Možete koristiti zamjenski znak s vašeg poddomene u DNS-a ili ako želite učiniti nekoliko jednostavnih testira uredite datoteku hosts na vaše VM da biste postavili naziv aplikacije na web-mjesta na VIP IP adresa.  Ako vaš elika i mala slova naziva poddomene. ilbase.com, a vi unijeli mytestapp aplikacije web tako da bi se adresirana pri mytestapp.ilbase.com zatim postavili u datoteci domaćini.  (U sustavu Windows hosts datoteka nalazi na C:\Windows\System32\drivers\etc\)
7.  Korištenje preglednika na tom VM i idite na http://mytestapp.ilbase.com (ili ono naziva web app je vaš poddomene)
8.  Korištenje preglednika na tom VM i idite na https://mytestapp.ilbase.com morat ćete prihvatiti nedostatak sigurnost ako pomoću samopotpisanog certifikata.  


IP adresa za vaše ILB navedena u svojstvima kao virtualna IP adresa

![][4]


## <a name="using-an-ilb-ase"></a>Korištenje programa ILB elika i mala slova ##

#### <a name="network-security-groups"></a>Mrežni sigurnosne grupe ####

Na elika i mala slova ILB omogućuje odvajanja mreže za aplikacije kao što je aplikacija nisu dostupne ili čak i poznati putem Interneta.  Ovo je izvrstan za hostiranje intranet web-mjesta kao što je skup poslovnih aplikacija.  Kada je potrebno ograničiti pristup čak i Dodatno možete i dalje koristiti Groups(NSGs) sigurnost mreže za kontrolu pristupa na razini mreže. 


Ako želite koristiti NSGs da biste dodatno ograničiti pristup morate provjerite ne prekinete komunikacije koje na elika i mala slova treba da bi mogla funkcionirati.  Čak i ako je pristup HTTP/HTTPS samo putem ILB koji se koriste u elika i mala slova u elika i mala slova i dalje ovisi o resursa izvan na VNet.  Da biste vidjeli koje pristup mreži i dalje potreban je izgled na podatke u dokumentu na [Kontroliranje dolazni promet na servis okruženju aplikacije] [ ControlInbound] i dokumenata na [Mreži konfiguracije detalje aplikacije servisa okruženjima ExpressRoute][ExpressRoute].  


Da biste konfigurirali svoje NSGs što trebate znati IP adresu koja koristi Azure upravljanja elika vaše i mala slova.  Taj IP adresa i je izlaznih IP adresa iz vaše elika i mala slova ako će se zahtjevi.  Da biste pronašli ovu IP adresa idite na **Postavke -> Svojstva** i **Izlaznih IP adresa**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Upravljanje Općenito ILB elika i mala slova ####

Upravljanje programa ILB elika i mala slova uglavnom ista je kao obično upravljanje programa elika i mala slova.  Morate skaliranja vaše grupe radnih hostira više instanci ASP i proširenja poslužitelja sučelje za rukovanje povećanog HTTP/HTTPS prometa.  Za opće informacije o upravljanju Konfiguracija programa elika i mala slova, čitati dokument o [konfiguriranju okruženju aplikacije servisa][ASEConfig].  


Upravljanje dodatne stavke su upravljanje certifikatom i upravljanje DNS-om.  Morate nabaviti i prijenos certifikat koji je korišten za HTTPS nakon stvaranja ILB elika i mala slova i zamijeniti ga prije no što istekne.  Budući da Azure posjeduje osnovni domenu dajemo certifikata za ASEs s vanjskim VIP.  Jer poddomene koristi se elika i mala slova ILB može biti bilo što, morate unijeti vlastiti certifikat za HTTPS. 


#### <a name="dns-configuration"></a>Konfiguriranje DNS-a ####

Kada se koristi vanjski VIP Azure upravlja DNS-om.  Bilo koju aplikaciju stvorene u vašem elika i mala slova automatski se dodaju DNS Azure što je javno DNS-a.  U programa elika i mala slova ILB morate upravljati vlastitim DNS.  Za dani poddomene kao što su contoso.corp.net potrebnih za stvaranje odgovora DNS zapise koji upućuju na adresu ILB za:

    * 
    objavljivanje *.scm ftp 


## <a name="getting-started"></a>Početak rada
Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Za početak rada s aplikaciju servisa okruženja, potražite u članku [Uvod u aplikaciju servisa okruženja][WhatisASE]

Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
