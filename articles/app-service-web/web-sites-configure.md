<properties 
    pageTitle="Konfiguriranje web-aplikacije u aplikacije servisa za Azure" 
    description="Konfiguriranje web-aplikacijama u servisa Azure aplikacije" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Konfiguriranje web-aplikacije u aplikacije servisa za Azure #

U ovoj se temi objašnjava kako konfigurirati web-aplikacijama pomoću [Portala za Azure].

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Postavke aplikacije

1. [Portal za Azure]otvorite plohu za web-aplikacije.
2. Kliknite **sve postavke**.
3. Kliknite **Postavke aplikacije**.

![Postavke aplikacije][configure01]

**Postavke aplikacije** plohu sadrži postavke koje su grupirane u odjeljku više kategorija.

### <a name="general-settings"></a>Opće postavke

**Framework verzije**. Ako aplikacija koristi neku te okviri, postavljanje tih mogućnosti: 

- **.NET Framework**: postavljanje .NET framework verzije. 
- **PHP**: postavite PHP verziju ili **ISKLJUČENO** da biste onemogućili PHP. 
- **Java**: Odaberite Java verziju ili **ISKLJUČENO** da biste onemogućili Java. Mogućnost **Web spremnik** koristite da biste odabrali jednoliko Tomcat i Jetty verzije.
- **Python**: Odaberite Python verzija ili **ISKLJUČENO** da biste onemogućili Python.

Tehnički razloga omogućivanje programskog jezika Java aplikacije onemogućuje .NET "," i "i" Python mogućnosti.

<a name="platform"></a>
**Platforme**. Odabir hoće li web-aplikaciju programa pokreće se u okruženju 32-bitne ili 64-bitni. Okruženje za 64-bitni zahtijeva Basic ili standardna načinu rada. Oslobodite a uvijek pokrenuti načini zajednički se koristi u okruženju 32-bitni.

**Web Sockets**. Postavljanje **Uključeno** da biste omogućili protokol WebSocket; na primjer, ako web-aplikaciju programa koristi [ASP.NET SignalR] ili [socket.io].

<a name="alwayson"></a>
**Stalna**. Web-aplikacijama po zadanom su učitan ako su neaktivan neke određenog vremenskog razdoblja. Omogućuje sustava Štednja resursi. U načinu Basic ili Standard možete omogućiti **Uvijek na** aplikaciju zadržati učitati cijelo vrijeme. Ako aplikaciju izvodi neprekinuti web zadacima, omogućite **Uvijek na**ili poslove web ne može se izvoditi pouzdano.

**Upravljani kanal verziju**. Postavlja IIS [način kanal]. Ostavite postavljeno integrirane (zadano) osim ako imate stari aplikacije koje je potrebno stariju verziju programa IIS.

**Automatsko Zamijeni**. Ako omogućite automatsko zamjena za implementaciju razdoblje, aplikacije servisa za će automatski zamijenite web-aplikaciju u radni kada automatske ažuriranje da biste tu vremensko razdoblje. Dodatne informacije potražite u članku [uvođenja za pripremna slobodnih web-aplikacijama u aplikacije servisa za Azure] (web-mjesta – kopirana bez postavljanja-publishing.md).

### <a name="debugging"></a>Ispravljanje pogrešaka

**Daljinsko uklanjanje programskih pogrešaka**. Omogućuje udaljene pogrešaka. Kada je omogućen, možete koristiti udaljene ispravljanje pogrešaka u Visual Studio izravno povezivanje s web-aplikaciju programa. Daljinsko uklanjanje programskih pogrešaka ostat će omogućeni za 48 sati. 

### <a name="app-settings"></a>Postavke za aplikaciju

Ova sekcija sadrži parove naziv/vrijednosti koje ste web-aplikacija će učitati na izborniku start. 

- Za .NET aplikacije, postavke su umetnutog u konfiguraciji .NET `AppSettings` tijekom izvođenja nadjačati postojeće postavke. 

- PHP, Python, jezika Java i čvor aplikacije možete pristupiti ove postavke kao varijable okruženja tijekom rada. Za sve postavke aplikacije stvorit će se dvije varijable okruženja; jedan s nazivom stavku postavku aplikacije, a drugi s prefiksom APPSETTING_. Obje sadrže istu vrijednost.

### <a name="connection-strings"></a>Nizove veze

Nizove veze za povezane resurse. 

Za .NET aplikacije koje se ti veze nizovi su umetnutog u konfiguraciji .NET `connectionStrings` postavke tijekom izvođenja nadjačati postojeće stavke gdje tipku jednako je naziv povezanu bazu podataka. 

Za aplikacije PHP, Python, jezika Java i čvor te postavke bit će dostupni kao varijable okruženja tijekom rada na mjestu s vrstom veze. Prefiksi varijable okruženja su na sljedeći način: 

- SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- SQL baza podataka:`SQLAZURECONNSTR_`
- Prilagođeno:`CUSTOMCONNSTR_`

Na primjer, ako su pod nazivom niza za povezivanje MySql `connectionstring1`, želite pristupiti putem varijablu okruženja `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Zadane dokumente

Zadani dokument je web-stranicu koja se prikazuje na korijenski URL za web-mjesto.  Koristi se prvi odgovarajuće datoteke na popisu. 

Web-aplikacije mogu koristiti module da usmjeravanje na temelju URL-a, a ne postoji u tom slučaju posluživanje statički sadržaj je kao nema zadani dokument.    

### <a name="handler-mappings"></a>Rukovatelj mapiranja

Pomoću ovog područja dodajte procesora prilagođene skripte za rukovanje zahtjevima za određene datotečne nastavke. 

- **Nastavak**. Datotečni nastavak treba obraditi, kao što su *.php ili handler.fcgi. 
- **Put procesor skripte**. Apsolutni put procesor skripte. Zahtjevi za datoteke koje odgovaraju datotečni nastavak će obradili procesor skripte. Korištenje put `D:\home\site\wwwroot` da biste se pozvali na aplikaciju korijenski direktorij.
- **Dodatne argumente**. Neobavezni Argumenti naredbenog retka za skripte procesor 


### <a name="virtual-applications-and-directories"></a>Virtualna aplikacije i direktorija 
 
Da biste konfigurirali virtualne aplikacije i direktorija, navedite svaki virtualnog direktorija i njegove odgovarajuće fizički put u odnosu korijenskog web-mjesta. Po želji možete odabrati **aplikacije** potvrdni okvir da biste označili virtualni imenik kao aplikaciju.


## <a name="enabling-diagnostic-logs"></a>Omogućivanje dijagnostičkih zapisnika

Da biste omogućili dijagnostičkih zapisnika:

1. U plohu za web-aplikaciju, kliknite **sve postavke**.
2. Kliknite **dijagnostičke zapisnike**. 

Mogućnosti za pisanje dijagnostičke zapisnike iz web-aplikacije koja podržava zapisivanja: 

- **Prijava u aplikaciju**. Ispisuje zapisnika aplikacije u datotečni sustav. Zapisivanje traje neko od 12 sati. 

**Razina**. Kada je omogućen aplikacije zapisivanje, ta mogućnost određuje količinu informacija koje će biti naveden (pogreška, upozorenje, informacije ili tekstni).

**Bilježenje u zapisnik web-poslužitelj**. Zapisnici spremaju se u obliku datoteke W3C prošireni zapisnika. 

**Detaljnih poruka o pogreškama**. Detaljne pogreške sprema poruke .htm datoteke. Datoteke spremljene u odjeljku /LogFiles/DetailedErrors. 

**Praćenje zahtjeva nije uspjelo**. Zapisnici nije uspjelo zahtjevi za XML datoteke. Datoteke spremljene u odjeljku /LogFiles/W3SVC*xxx*, pri čemu je xxx Jedinstveni identifikator. Ova mapa sadrži XSL datoteka i XML datoteke. Provjerite je li za preuzimanje datoteke XSL jer pruža funkcija za oblikovanje i filtriranje sadržaja XML datoteke.

Da biste prikazali datoteke zapisnika, morate stvoriti FTP vjerodajnice, na sljedeći način:

1. U plohu za web-aplikaciju, kliknite **sve postavke**.
2. Kliknite **vjerodajnice za implementaciju**.
3. Unesite korisničko ime i lozinku.
4. Kliknite **Spremi**.

![Postavljanje vjerodajnica za implementaciju][configure03]

Puno ime korisnika FTP je "app\username" pri čemu *aplikacija* je naziv web-aplikacije. Korisničko ime nalazi se u aplikaciju plohu web koji se nalazi u odjeljku **Osnove**.  

![Vjerodajnice za implementaciju FTP][configure02]

## <a name="other-configuration-tasks"></a>Ostali zadaci konfiguracija

### <a name="ssl"></a>SSL 

U načinu rada Basic ili Standard, možete prenijeti SSL certifikata za prilagođenu domenu. Dodatne informacije potražite u članku [omogućiti HTTPS za web-aplikaciji]. 

Da biste pogledali prenesene certifikata, kliknite **Sve postavke** > **prilagođenih domena i SSL**.

### <a name="domain-names"></a>Nazivi domena

Dodavanje prilagođenih naziva domena za web-aplikacije. Dodatne informacije potražite u članku [konfiguriranje prilagođenog naziva domene za web-aplikaciju u aplikacije servisa za Azure].

Da biste pogledali naziva domene, kliknite **Sve postavke** > **prilagođenih domena i SSL**.

### <a name="deployments"></a>Implementacije

- Postavljanje neprekinuti implementacije. Potražite u članku [Korištenje brojka za implementaciju Web Apps na servisu Azure aplikacije](./web-sites-deploy.md).
- Uvođenje slobodnih. Potražite u članku [Implementacija pripremna okruženja za Web Apps na servisu Azure aplikacije].


Da biste pogledali svoje slobodnih implementaciju, kliknite **Sve postavke** > **slobodnih implementacije**.

### <a name="monitoring"></a>Nadzor

U načinu Basic ili Standard možete testirati i dostupnost HTTP ili HTTPS krajnje točke, s najviše tri zemlj distributed mjesta. Nadzor test neće uspjeti ako HTTP odgovor kod pogreške (4xx ili 5xx) ili odgovor traje više od 30 sekundi. Krajnje smatra dostupno samo ako je nadzor testova uspjeti iz određene lokacije. 

Dodatne informacije potražite u članku [Kako: praćenje stanja krajnjoj točki web].

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa], gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="next-steps"></a>Daljnji koraci

- [Konfiguriranje prilagođenog naziva domene u aplikacije servisa za Azure]
- [Omogućivanje HTTPS za aplikaciju u aplikacije servisa za Azure]
- [Promijenite veličinu web-aplikacijama u aplikacije servisa za Azure]
- [Nadzor osnove web-aplikacijama u aplikacije servisa za Azure]

<!-- URL List -->

[SignalR platforme ASP.NET]: http://www.asp.net/signalr
[Portal za Azure]: https://portal.azure.com/
[Konfiguriranje prilagođenog naziva domene u aplikacije servisa za Azure]: ./web-sites-custom-domain-name.md
[Implementacija pripremna okruženja web-aplikacijama u aplikacije servisa za Azure]: ./web-sites-staged-publishing.md
[Omogućivanje HTTPS za aplikaciju u aplikacije servisa za Azure]: ./web-sites-configure-ssl-certificate.md
[Kako: praćenje stanja web krajnje točke]: http://go.microsoft.com/fwLink/?LinkID=279906
[Nadzor osnove web-aplikacijama u aplikacije servisa za Azure]: ./web-sites-monitor.md
[način rada za kanal]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Promijenite veličinu web-aplikacijama u aplikacije servisa za Azure]: ./web-sites-scale.md
[Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Pokušajte aplikacije servisa]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
