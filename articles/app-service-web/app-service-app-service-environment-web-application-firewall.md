<properties 
    pageTitle="Konfiguriranje aplikacije vatrozid Web (WAF) za aplikacije servisa za okruženje" 
    description="Saznajte kako konfigurirati web vatrozid ispred okruženja aplikacije servisa za aplikacije." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfiguriranje aplikacije vatrozid Web (WAF) za aplikacije servisa za okruženje

## <a name="overview"></a>Pregled ##
Web-aplikacije vatrozidima kao što je [Barracuda WAF za Azure](https://www.barracuda.com/programs/azure) koji je dostupan na sigurno web-aplikacije provjerom dolazni promet web da biste blokirali SQL injections, web-mjesta skriptiranje, zlonamjerni softver prijenosi i aplikacije DDoS i druge napada pomaže [Trgovine Windows Azure](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) . Ga i pregledava odgovore od poslužitelje web pozadinske za sprječavanje podataka gubitka (DLP). U kombinaciji s odvajanja i dodatne skaliranja nudi aplikacije servisa okruženja, to nudi idealna okruženje za glavno računalo tvrtke ključnih web-aplikacije koje su potrebne za withstand zlonamjernih zahtjeve i promet velike količine.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Postavljanje ##
Za taj dokument ne možemo će konfigurirati naš aplikacije servisa okruženje iza više opterećenja raspoređen pojavljivanja Barracuda WAF tako da samo promet na WAF možete doći do servisa okruženja aplikacije i neće moći pristupiti na DMZ. Ne možemo dodijelit će Azure promet Upravitelj ispred naš instance Barracuda WAF da biste učitali saldo preko centre za Azure podataka i regije. Više razine dijagram postavljanje će izgledati kao što je prikazano u nastavku.

![Arhitektura][Architecture] 

> Napomena: Uz Uvod [ILB podrške za aplikaciju servisa okruženju](app-service-environment-with-internal-load-balancer.md), možete konfigurirati elika i mala slova se ne može pristupiti iz sustava DMZ i samo dostupnosti privatne mreže. 

## <a name="configuring-your-app-service-environment"></a>Konfiguriranje okruženja aplikacije servisa ##
Da biste konfigurirali okruženju aplikacije servisa pogledajte [naše dokumentaciju](app-service-web-how-to-create-an-app-service-environment.md) o toj temi. Nakon što dodate okruženju servisa aplikacija stvorili, možete stvoriti [Web Apps](app-service-web-overview.md), [Aplikacije API -JA](../app-service-api/app-service-api-apps-why-best-platform.md) i [Mobilne aplikacije](../app-service-mobile/app-service-mobile-value-prop.md) u ovom okruženju u kojem će sve biti zaštićene iza WAF ćemo konfiguriranje u sljedećem odjeljku.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfiguriranje sustava Barracuda WAF u Oblaku ##
Barracuda sadrži [detaljne članak](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) o implementaciji njegov WAF na virtualnog računala u Azure. No jer smo želite zalihosti i ne predstavljanje jednu točku pogreške, želite implementirati najmanje 2 VMs instancu WAF u istom servis u Oblaku kada slijedite ove upute.

### <a name="adding-endpoints-to-cloud-service"></a>Dodavanje krajnje točke za servis u Oblaku ###
Nakon što dodate 2 ili više instanci WAF VM servis u Oblaku [Azure portal](https://portal.azure.com/) možete koristiti da biste dodali HTTP i HTTPS krajnje točke koje koriste program kao što je prikazano na slici u nastavku.

![Konfiguriranje krajnje točke][ConfigureEndpoint]

Ako aplikacija koristite druge krajnje točke, provjerite je li da biste dodali onima na ovaj popis. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfiguriranje Barracuda WAF do njegova Portal za upravljanje ###
Barracuda WAF koristi 8000 TCP priključak za konfiguraciju do njegova portal za upravljanje. Budući da imamo više instanci VMs WAF morat ćete ponovite ove korake za svaku instancu VM. 


> Napomena: Kada ste gotovi s konfiguracijom WAF, uklanjanje krajnju točku TCP/8000 sve svoje WAF VMs u zaštiti vaše WAF.

Dodajte krajnju točku upravljanje kao što je prikazano na slici u nastavku da biste konfigurirali svoje WAF Barracuda.

![Dodavanje krajnje upravljanja][AddManagementEndpoint]
 
Pomoću preglednika da biste pronašli krajnju točku upravljanja na servis u Oblaku. Ako servis u Oblaku za pozivanje test.cloudapp.net želite pristupiti ovom krajnjoj točki pregledavanjem http://test.cloudapp.net:8000. Trebali biste vidjeti stranicu za prijavu kao što su ispod koje možete prijaviti pomoću vjerodajnica koje ste naveli u fazi postavljanje WAF VM.

![Upravljanje stranicu za prijavu][ManagementLoginPage]

Jednom vam prijavu trebali biste vidjeti na nadzornoj ploči onoj slike u nastavku koje će prikazati osnovne statističkih podataka o WAF zaštitu.

![Upravljanje nadzorne ploče][ManagementDashboard]

Klikom na karticu servisi omogućuju možete konfigurirati svoje WAF za servise ga štiti. Dodatne informacije o konfiguraciji vašeg WAF Barracuda možete pokušati pronaći [njihovu dokumentaciju](https://techlib.barracuda.com/waf/getstarted1). U primjeru niže web-aplikaciju programa Azure posluživanje promet na HTTP i HTTPS nije konfiguriran.

![Upravljanje dodati servise][ManagementAddServices]

> Napomena: Ovisno o konfiguraciji aplikacije i koje značajke se koriste u svom okruženju aplikacije servisa, morat ćete prosljeđivanje promet za TCP priključci osim 80 i 443, primjerice ako imate postavljanje IP SSL za web-aplikacije. Popis mrežni priključci koji se koristi u okruženju aplikacije servisa, pogledajte odjeljak mrežne priključke [kontrola dolazni promet dokumentaciju](app-service-app-service-environment-control-inbound-traffic.md) .

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfiguriranje Microsoft Azure promet Manager (NEOBAVEZNO) ##
Ako je dostupan u više područja aplikacije, a zatim bi želite učitati saldo ih iza [Upravitelj promet Azure](../traffic-manager/traffic-manager-overview.md). Da biste to učinili možete dodati krajnje [Azure klasični portal](https://manage.azure.com) s nazivom servis u Oblaku za svoje WAF u profilu promet Upravitelj kao što je prikazano na slici u nastavku. 

![Upravitelj promet krajnje točke][TrafficManagerEndpoint]

Ako aplikacija zahtijeva provjeru autentičnosti, provjerite imate nekih resursa koje ne zahtijevaju sve provjere autentičnosti za promet Upravitelj pomoću naredbe ping dostupnost vaša aplikacija. Možete konfigurirati URL-a u odjeljku Konfiguriranje [Azure klasični portal](https://manage.azure.com) kao što je prikazano u nastavku.

![Konfiguriranje upravitelja promet][ConfigureTrafficManager]

Da biste proslijedili Pingovi Upravitelj promet vašeg WAF aplikacije, morate postavljanje prijevoda web-mjesta na vašem WAF Barracuda proslijediti promet na vaše aplikacije kao što je prikazano u primjeru u nastavku.

![Web-mjesto prijevoda][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Zaštita promet aplikacije servisa za okruženje pomoću mreže sigurnosnih grupa (NSG)##
Slijedite [kontrola dolazni promet dokumentaciju](app-service-app-service-environment-control-inbound-traffic.md) detalje na ograničavanje promet svoje radno okruženje aplikacije servisa za WAF samo pomoću VIP adresu servis u Oblaku. Evo oglednih naredbe ljuske Powershell za izvođenje ovog zadatka za priključak TCP 80.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Zamijenite na SourceAddressPrefix virtualne IP adrese (VIP) na WAF u Oblaku.

> Napomena: VIP servis u Oblaku će se promijeniti kada izbrišete i ponovno stvoriti servisa u Oblaku. Provjerite je li ažurirati IP adresa u grupi mrežnom resursu kada to učinite. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
