<properties
    pageTitle="Azure usporedbu aplikacije servisa, virtualnim strojevima, tkanina servisa i servise u Oblaku | Microsoft Azure"
    description="Saznajte kako odabrati između aplikacije servisa za Azure, virtualnim strojevima, tkanina servisa i servise u Oblaku za hostiranje web-aplikacije."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure usporedbu aplikacije servisa, virtualnim strojevima, tkanina servisa i servise u Oblaku

## <a name="overview"></a>Pregled

Azure nudi nekoliko načina za glavno računalo web-mjesta: [Aplikacije servisa za Azure][], [virtualnim strojevima][], [Tkanina servisa][]i [Servise u Oblaku][]. U ovom se članku olakšava razumijevanje mogućnosti i provjerite odabir za web-aplikaciju.

Aplikacije servisa za Azure je najbolji odabir za većinu web-aplikacije. Implementacije i upravljanja integrirati u platforme, web-mjesta mogu mijenjati veličinu brzo učiniti opterećenje velikog prometa i ugrađene učitavanja opterećenja i promet Upravitelj omogućuju visoke dostupnosti. Možete premjestiti postojeće web-mjesta aplikacije servisa za Azure jednostavno pomoću [alata za migraciju online](https://www.migratetoazure.net/), koristite aplikaciju Otvori izvor iz galerije Web aplikacije ili stvorite novo web-mjesto pomoću framework i alati po izboru. Značajka [WebJobs][] olakšava dodavanje posao pozadini obrade na aplikacije servisa za web-aplikaciju.

Servis tkanina dobar je ako stvaranja nove aplikacije ili ponovno pisanje postojeće aplikacije da biste koristili arhitekturu microservice. Aplikacije koji se izvode na zajednički skup računala, možete započeti small i Povećaj pretraživanje velikog Skaliranje s stotine ili tisuće strojeva prema potrebi. S praćenjem stanja servisa olakšavaju dosljedno i pouzdano pohranjuju stanje aplikacije, a tkanina servisa automatski upravlja particija servisa, veličine i dostupnost umjesto vas.  Servis tkanina podržava i WebAPI s otvaranje web-sučelja za .NET (OWIN) i ASP.NET Core.  U usporedbi s aplikacije servisa za servis tkanina omogućuje više kontrole nad ili izravan pristup, temeljni infrastrukture. Možete udaljene u poslužitelja ili konfiguriranje zadaci pokretanje poslužitelja. Servisi u oblaku slično tkanina servisa u stupanj kontrole nasuprot jednostavnost, ali je sada naslijeđene servisa i servisa tkanina preporučuje se novi razvoj.

Ako imate postojeće aplikacije koje je potrebna znatno izmjene da biste pokrenuli web-mjesto servisa aplikacija ili u okvir za servis tkanina, mogli odabrati virtualnih računala da biste pojednostavnili migracije u oblak. Međutim, pravilno konfiguriranje, zaštita i održava VMs zahtijeva puno vremena i stručna znanja IT usporedbi sa servisa Azure aplikacija i servisa tkanina. Ako namjeravate Azure virtualnih računala, provjerite je li snimljenih račun potreban zakrpu, ažuriranje i upravljanje okruženja VM trud trenutno održavanje. Azure virtualnim strojevima je infrastrukture-kao-na-servis (IaaS), dok servisa aplikacija i servisa tkanina platforme kao-na-usluga (Paas). 

## <a name="features"></a>Usporedba značajki

U sljedećoj su tablici uspoređuje mogućnosti aplikacije servisa za servise u Oblaku, virtualnim strojevima i tkanina servisa da bi vam najbolje. Aktualne informacije o SLA za svaku mogućnost potražite u članku [Ugovore o razini usluge za Azure](/support/legal/sla/).

Značajka|Aplikacije servisa (web apps)|Servisi u oblaku (web uloge)|Virtualnim strojevima|Tkanina servisa|Bilješke
---|---|---|---|---|---
Pri izravnih implementacije|X|||X|Implementacija programa ili ažuriranje aplikacija na servis u Oblaku ili stvaranja VM, najmanje; traje nekoliko minuta Implementacija aplikacije na web-aplikacijama traje sekundi.
Proširenjem veće strojeva bez redeploy|X|||X|
Web server instancama zajedničko korištenje sadržaja i konfiguracije, što znači da ne morate ponovno implementirate ili konfigurirajte kao promijenite li.|X|||X|
Više okruženja implementacije (proizvodnje i pripremna)|X|X||X|Servis tkanina omogućuje vam da bi više okruženja za aplikacije ili za implementaciju različite verzije sustava vaše aplikacije po usporednu.
Automatsko upravljanje OS ažuriranja|X|X|||Automatska ažuriranja OS u planu su za buduće tkanina servisa izdanja.
Prebacivanje objedinjenog platformu (jednostavno postavite pokazivač između 32-bitni i 64-bitni)|X|X|||
Implementacija kod s BROJKA, FTP|X||X||
Implementacija kod s implementacija Web|X||X||Servisi u oblaku podržava korištenje implementacija implementacija ažuriranja uloga pojedinačne instance Web. Međutim, ne možete koristiti za početnog uvođenja uloge, a imate ako koristite implementacija Web za ažuriranje zasebno implementirati za svaku instancu uloge. Više instanci su potrebna lozinka zadovoljavate SLA servisa oblaka za radnog okruženja.
Podrška za WebMatrix|X||X||
Pristup servisima kao što su Bus servisa, za pohranu, a zatim SQL baze podataka|X|X|X|X|
Glavno računalo web ili sloj web services arhitekturu više razina|X|X|X|X|
Glavno računalo Srednji sloj arhitekturu više razina|X|X|X|X|Aplikacije servisa za web-aplikacije možete jednostavno hostirati Srednji sloj REST API-JA, a značajku [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) možete hostirati pozadine poslove. WebJobs možete pokrenuti u vlastito web-mjesto da biste postigli neovisno skalabilnost za u sloju. Značajke [aplikacije API](../app-service-api/app-service-api-apps-why-best-platform.md) pretpregleda nudi dodatnim značajkama za OSTALE usluge hostiranja.
Integrirano MySQL kao-na-usluga za podršku|X|X|X||Servisi u oblaku možete integrirati MySQL kao-na-usluga putem ClearDB-ponuda, ali ne u sklopu tijeka rada za Azure Portal.
Podrška za klasični ASP.NET ASP, Node.js, PHP, Python|X|X|X|X|Servis tkanina podržava stvaranje web-sučelja pomoću [platforme ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) , ili možete implementirati bilo koju vrstu aplikacije (Node.js, jezika Java itd.) kao [Gost izvršne datoteke](../service-fabric/service-fabric-deploy-existing-app.md).
Promjena veličine izgleda više instanci bez redeploy|X|X|X|X|Virtualnih računala mogu mijenjati veličinu odgovor na više instanci, ali servise ih moraju biti napisani učiniti skaliranje-izlaz. Morate konfigurirati opterećenja usmjeravanje zahtjeva preko strojeva pa Stvori grupu afinitet da biste spriječili istodobno ponovnog pokretanja sve instanci zbog održavanja ili hardvera.
Podrška za SSL|X|X|X|X|Aplikacije servisa za web-aplikacijama SSL za prilagođenih naziva domena podržano je samo za osnovne i standardne način. Informacije o korištenju SSL s web-aplikacije, potražite u članku [Konfiguriranje SSL certifikata na web-mjesta Azure](../app-service-web/web-sites-configure-ssl-certificate.md).
Integracija s programom Visual Studio|X|X|X|X|
Daljinsko uklanjanje programskih pogrešaka|X|X|X||
Implementacija kod s TFS|X|X|X|X|
Mrežni odvajanja s [Azure virtualne mreže](/services/virtual-network/)|X|X|X|X|Vidi također [Integracija virtualne mreže Azure web-mjesta](/blog/2014/09/15/azure-websites-virtual-network-integration/)
Podrška za [Azure Upravitelj promet](/services/traffic-manager/)|X|X|X|X|
Nadzor integrirani krajnje točke|X|X|X||
Daljinski pristup radne površine na poslužitelje||X|X|X|
Sve prilagođene MSI instalacije||X|X|X|Servis tkanina omogućuje vam da hostirati bilo koje izvršne datoteke kao [Gost izvršne datoteke](../service-fabric/service-fabric-deploy-existing-app.md) ili bilo koju aplikaciju možete instalirati na na VMs.
Mogućnost za definiranje/izvršavanje zadataka pokretanja||X|X|X|
Možete poslušati ETW događaja||X|X|X|

## <a name="scenarios"></a>Scenariji i preporuke

Evo nekih uobičajenih scenarija aplikacije s preporuke kao koje Azure možda najprikladnije za svaku web-mjesta mogućnost.

- [Je li nužno web-sučelja s pozadinskom obrada i baza podataka pozadine da biste pokrenuli poslovnim aplikacijama integriran s na lokalnu instancu resursi.](#onprem)
- [Pouzdan način za hostiranje web-mjesto tvrtke koji se mijenja veličinu dobro je potrebna i ponuda globalni dosegne.](#corp)
- [Imam aplikaciju sustavu IIS6 sustavom Windows Server 2003.](#iis6)
- [Ja sam vlasnik small business i potrebna mi je jeftini način hostira Moje web-mjesto, ali s budućeg rasta na umu.](#smallbusiness)
- [Ja sam weba ili grafički dizajner i želim dizajniranje i stvaranje web-mjesta za moj korisnike.](#designer)
- [Tijekom migracije aplikacijom sučelja web-mjesto Moje više razina u oblak](#multitier)
- [Moje aplikacije ovisi o visoko prilagođenih Windows ili Linux okruženja, a koje želite premjestiti u oblak.](#custom)
- [Moja web-mjesta koristi Otvori izvor softver i želim hostirati u Azure.](#oss)
- [Imam redak tvrtke aplikacije koje su potrebne za povezivanje s korporacijskom mrežom.](#lob)
- [Želim hostirati REST API-JA ili web-servisa za mobilne klijente.](#mobile)


### <a id="onprem"></a>Je li nužno web-sučelja s pozadinskom obrada i baza podataka pozadine da biste pokrenuli poslovnim aplikacijama integriran s na lokalnu instancu resursi.

Aplikacije servisa za Azure je odličan rješenje za složene poslovnim aplikacijama. Omogućuje vam razvoj aplikacija koje se automatski skaliranje na platformi za Balansirani opterećenja, su zaštićene sa servisom Active Directory i povezivanje s lokalnim resurse. Čini upravljanje tim aplikacijama jednostavno kroz svjetskim portal i API- a omogućuje vam da bi se dobio uvid u kako korisnici koriste ih pomoću alata za uvid aplikacije. Značajka [Webjobs][] omogućuje pokretanje pozadinski procesi i zadatke kao što su dio web razina, prilikom povezivanja hibridnog i VNET značajki olakšavaju se ponovno povezati s lokalnog resursa. Aplikacije servisa za Azure pruža tri 9 SLA za web-aplikacije i omogućuje vam da:

* Pokretanje aplikacije pouzdano na platformi oblaka koja se sama automatskog popravka, automatski zakrpa.
* Promjena veličine automatski preko globalna mreža podatkovnim centrima.
* Sigurnosno kopiranje i vraćanje za oporavak Izrada.
* Biti ISO, SOC2 i PCI usklađen.
* Integracija sa servisom Active Directory

### <a id="corp"></a>Pouzdan način za hostiranje web-mjesto tvrtke koji se mijenja veličinu dobro je potrebna i dosegne ponuda za globalnu.

Aplikacije servisa za Azure je odličan rješenje za hostiranje web-mjesta tvrtke. Omogućuje web-aplikacije za promjenu veličine brzo i jednostavno da bi odgovarao zahtjev preko globalna mreža podatkovnim centrima. Nudi lokalne razgovor, odstupanje kvara i upravljanje Inteligentna promet. Sve na platformi koje nudi alate za upravljanje svjetskim omogućujući vam da bi se dobio uvid u stanja web-mjesta i web-mjesta promet brzo i jednostavno. Aplikacije servisa za Azure pruža tri 9 SLA za web-aplikacije i omogućuje vam da:

* Pokrenite web-mjestima sustava pouzdano na platformi oblaka koja se sama automatskog popravka, automatski zakrpa.
* Promjena veličine automatski preko globalna mreža podatkovnim centrima.
* Sigurnosno kopiranje i vraćanje za oporavak Izrada.
* Upravljanje zapisnicima i promet pomoću alata za integriranu.
* Biti ISO, SOC2 i PCI usklađen.
* Integracija sa servisom Active Directory

### <a id="iis6"></a>Imam aplikaciju sustavu IIS6 sustavom Windows Server 2003.

Aplikacije servisa za Azure olakšava izbjegavanje troškove infrastrukture vezane uz migriranje starije sustavu IIS6 aplikacije. Microsoft je stvorio [Alati za migraciju jednostavno je za korištenje i migracije detaljne upute](https://www.movemetowebsites.net/) koje vam omogućuje da Provjeri kompatibilnost i definirati sve promjene koje je potrebno izvršiti. Integracija s programom Visual Studio, TFS i uobičajenih alata a CMS olakšava za implementaciju aplikacije u sustavu IIS6 izravno s oblakom. Kada implementiran, portalu Azur nudi robusne upravljanje alata koji omogućuju vam da biste skalirali prema dolje da biste upravljali troškove i do natjecanjima potražnje prema potrebi. Pomoću alata za migraciju možete učiniti sljedeće:

* Brzo i jednostavno migrirati naslijeđene web-aplikacije Windows Server 2003 u oblak.
* Odabrati da biste napustili vaše priložene SQL baze podataka na lokaciji radi stvaranja hibridnog aplikacije.
* Automatsko premještanje baze podataka sustava SQL uz naslijeđene aplikacije.

### <a id="smallbusiness"></a>Ja sam vlasnik small business i potrebna mi je jeftini način hostira Moje web-mjesto, ali s budućeg rasta na umu.

Aplikacije servisa za Azure je odličan rješenje za taj scenarij jer možete ga počnete koristiti besplatno i dodavati još mogućnosti, kada su vam potrebne. Svaku aplikaciju besplatne web dolazi s domenom nudi Azure (*your_company*. azurewebsites.net), i platforme sadrži integrirani implementacije i upravljanja alate, kao i u galeriju aplikacija koje olakšavaju početak rada. Postoji mnogo drugih servisa i mogućnosti skaliranja koje omogućuju poslovanja s povećanom korisnika zahtjev web-mjesto. Pomoću aplikacije servisa Azure, možete učiniti sljedeće:

- Počinju besplatne sloju, a zatim proširenja prema potrebi.
- Pomoću galerije aplikacije da biste brzo postavili popularnih web-aplikacije, kao što su WordPress.
- Dodatna servisa Azure i značajki u aplikaciji po potrebi dodajte.
- Sigurne web aplikacije s HTTPS.

### <a id="designer"></a>Ja sam weba ili grafički dizajner i želim dizajniranje i stvaranje web-mjesta Moje klijentima

Razvojni inženjeri i dizajneri, aplikacije servisa za Azure jednostavno integrira s raznim okviri i alati, uključuje implementacije podršku za brojka i FTP i nudi čvrsto Integracija s alatima i servise kao što su Visual Studio i baze podataka SQL. Sa servisom za aplikaciju, možete učiniti sljedeće:

- Pomoću alata za naredbenog retka za [automatizirane zadatke][scripting].
- Rad s Popularni jezika kao što su [.net][dotnet], [PHP][], [Node.js][nodejs], a [Python][].
- Odaberite tri različite skaliranja razine skaliranja prema gore da biste kapaciteta vrlo visoka.
- Integracija s drugih servisa Azure, kao što su [Baze podataka SQL][sqldatabase], [Servis Bus] [ servicebus] i [prostora za pohranu][]ili partnera ponude iz [Trgovine Azure][azurestore], kao što su MySQL i MongoDB.
- Integrirati s alatima kao što su Visual Studio, brojka, WebMatrix, WebDeploy, TFS i FTP.

### <a id="multitier"></a>Tijekom migracije aplikacijom sučelja web-mjesto Moje više razina u oblak

Ako koristite više razina aplikacija, kao što je web-poslužitelju koji se povezuje s bazom podataka aplikacije servisa za Azure je dobro mogućnost koja nudi čvrsto Integracija s bazom podataka SQL Azure. I koristite značajku WebJobs radi pozadinski procesi.

Odaberite servis tkanina za jednu ili više razine ako trebate više kontrole nad okruženju poslužitelja, kao što je mogućnost udaljene u vašem poslužitelju ili konfigurirajte Zadaci poslužitelja pri pokretanju.

Ako želite da biste koristili sliku računala ili pokrenuti poslužiteljski softver ili servise koje ne možete konfigurirati na tkanina servisa, odaberite virtualnim strojevima za jedan ili više vaše razine.

### <a id="custom"></a>Moje aplikacije ovisi o visoko prilagođenih Windows ili Linux okruženja, a koje želite premjestiti u oblak.

Ako aplikacija zahtijeva složene instalacije i konfiguracije softvera i operacijski sustav, virtualnim strojevima vjerojatno je najbolje rješenje. S virtualnim računalima, možete učiniti sljedeće:

- Pomoću galerije virtualnog računala da biste započeli s operacijskim sustavom, kao što je Windows ili Linux, a zatim ga prilagoditi za preduvjeti za aplikaciju.
- Stvoriti i prenijeti sliku prilagođene postojećeg lokalnog poslužitelja pokrenuti virtualnog računala u Azure.

### <a id="oss"></a>Moja web-mjesta koristi Otvori izvor softver i želim hostirati servisu Azure

Ako vaš framework Otvori izvor podržana za aplikacije servisa, jezika i okviri potrebne za svoju aplikaciju konfigurirane umjesto vas automatski. Aplikacije servisa omogućuje:

- Korištenje mnoge popularne Otvori izvor jezika, kao što su [.NET][dotnet], [PHP][], [Node.js][nodejs], a [Python][].
- Postavljanje WordPress, Drupal, Umbraco, DNN i mnoge druge aplikacije drugih proizvođača web.
- Migracija postojeće aplikacije ili stvorite novi iz galerije aplikacije.

Ako vaš Otvori izvor framework nije podržana na aplikacije servisa, možete je pokrenuti u jednoj aplikaciji sustava u drugi Azure web-mjesta mogućnosti. S virtualnim strojevima instalirate i konfigurirate softver na računalu slika, što može biti Windows ili Linux.

### <a id="lob"></a>Imam redak tvrtke aplikacije koje su potrebne za povezivanje s korporacijskom mrežom

Ako želite stvoriti aplikaciju redak tvrtke, web-mjesta, morat izravan pristup servisima ili podatke na mreži tvrtke. Ovo je moguće aplikacije servisa, tkanina servisa i virtualnih računala pomoću [servisa Azure virtualne mreže](/services/virtual-network/). Servis za aplikaciju možete koristiti [značajke za integraciju VNET](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), koji omogućuje Azure aplikacije da biste pokrenuli kao da su na mreži tvrtke.

### <a id="mobile"></a>Želim hostirati REST API-JA ili web-servisa za mobilne klijente

HTTP-poštu web-servisi omogućuju podržava razna klijenata, uključujući mobilne klijente. Okviri kao što su ASP.NET Web API integrirati s Visual Studio koja olakšava stvaranje i korištenje OSTALE servise.  Ove usluge ponudit će se s na krajnjoj točki web tako da je moguće koristiti bilo koji web-mjesta postupak na Azure za podržavaju taj scenarij. Međutim, aplikacije servisa za je odličan izbor za hostiranje REST API-ji. Sa servisom za aplikaciju, možete učiniti sljedeće:

- Brzo stvaranje [aplikacije za mobilne uređaje](../app-service-mobile/app-service-mobile-value-prop.md) ili [API aplikacije](../app-service-api/app-service-api-apps-why-best-platform.md) za hostiranje HTTP web-servisa na jedan od globalno raspodijeljeno podatkovnim centrima Azure korisnika.
- Migracija postojeće usluge ni stvarati nove.
- Postizanje SLA dostupnost s jednokratan ili promijeni odgovor na više računala namjenski.
- Koristite objavljenu web-mjesta za pružanje REST API-ji za sve klijente HTTP, uključujući mobilne klijente.

> [AZURE.NOTE]
> Ako želite započeti s aplikacije servisa za Azure prije registracije za račun, idite na <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, gdje možete odmah stvorite short-lived starter aplikaciju u aplikacije servisa za Azure besplatno. Nema kreditnom karticom obavezna, bez preuzete obveze.

## <a id="nextsteps"></a>Daljnji koraci

Dodatne informacije o tri web-hostinga mogućnosti potražite u članku [Uvod u Azure](../fundamentals-introduction-to-azure.md).

Da biste započeli odgovarajuće mogućnosti odaberite aplikacije, potražite u sljedećim resursima:

* [Aplikacije servisa za Azure](/documentation/services/app-service/)
* [Servisi u Oblaku za Azure](/documentation/services/cloud-services/)
* [Azure virtualnim strojevima](/documentation/services/virtual-machines/)
* [Tkanina servisa](/documentation/services/service-fabric)

<!-- URL List -->

[Aplikacije servisa za Azure]: /services/app-service/
[Servisi u oblaku]: http://go.microsoft.com/fwlink/?LinkId=306052
[Virtualnim strojevima]: http://go.microsoft.com/fwlink/?LinkID=306053
[Tkanina servisa]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Prostor za pohranu]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
