<properties
   pageTitle="Integracija sa operacije upravljanja paket (OMS) | Microsoft Azure"
   description="Osim pomoću standardne značajke OMS, možete integrirati s drugim aplikacijama za upravljanje i servise za davanje hibridnog upravljanje okruženja, možete unijeti prilagođene upravljanje scenarija jedinstvene svoje radno okruženje ili možete unijeti prilagođeni upravljanje sučelje za klijente.  Ovaj članak sadrži pregled različitih mogućnosti za integraciju s OMS i veze na članke pružaju detaljne tehničke informacije."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Integracija sa operacije upravljanja paket (OMS)

Paket za upravljanje operacija je Microsoftov oblaku IT rješenja upravljanja koji olakšava upravljanje i zaštita lokalnih i infrastrukture u oblaku.  Osim pomoću standardne značajke OMS, možete integrirati s drugim aplikacijama za upravljanje i servise za davanje hibridnog upravljanje okruženja, možete unijeti prilagođene upravljanje scenarija jedinstvene svoje radno okruženje ili možete unijeti prilagođeni upravljanje sučelje za klijente.  Ovaj članak sadrži pregled različitih mogućnosti za integraciju s OMS services i veze na članke pružaju detaljne tehničke informacije. 



## <a name="log-analytics"></a>Zapisnik Analytics
Upravljanje podatke prikupljene putem prijava analitiku pohranjena u spremište koje se nalazi u Azure.  Sve podatke pohranjene u spremištu dostupan je u zapisniku pretraživanja koje omogućuju Brza analiza za iznimno velike količine podataka.  Preduvjeti za integraciju možda je za popunjavanje spremište novim podacima dostupnost za analizu ili za izdvajanje podataka iz spremišta da biste nam novu vizualizaciju ili integrirati s drugom alatu za upravljanje.

Svaki dio podataka u spremištu pohranjen kao zapis.  Nakon popunjavanja spremište mora sadržavati korisnika s vrstu zapisa koju koristi rješenje i opis njezina svojstva.  Prilikom učitavanja podataka, morat ćete informacije o tome radite s podacima.

![Popunjavanje OMS spremište](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>Popunjavanje spremište zapisnika Analytics
Za popunjavanje OMS spremište na više načina.  Metoda koju ćete koristiti ovise o čimbenicima kao što su gdje se nalazi izvorišne podatke, oblika podataka i koje klijenti morate za podršku.  Kada se podaci se pohranjuju u spremištu, nije važno kako je prikupljeni.

U sljedećim odjeljcima opisuju se različite mogućnosti za popunjavanje OMS spremište.

#### <a name="connected-sources-and-data-sources"></a>Povezani izvore i izvore podataka 
Povezani izvora su mjesta za spremište OMS koju je moguće dohvatiti podatke.  Izvori podataka i rješenja se izvoditi na povezani izvora i definirali određeni podaci koji se prikupljaju.  Ako aplikacija zapisuje podatke u neku od tih izvora podataka, možete ga prikupiti konfiguriranjem izvora podataka.  Ako, na primjer, ako aplikacija stvara Syslog događaje, pa ih mogu prikupiti Syslog izvor podataka na Linux agent.

- [Izvori podataka u zapisniku analize](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Rješenja

Rješenja proširuju mogućnosti OMS.  Rješenje prikupiti podatke iz povezanih izvora ili ga izvesti analizu na zapisa koji su već prikupljeni u spremištu.  Svakog rješenja pruža Microsoft sadrži pojedinačne članak koji sadrži detalje na podatke koje prikupi.

- [Rješenja u zapisnik Analytics](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>Prikupljanje podataka HTTP API-JA

API za prikupljanje zapisnika analize HTTP podataka je REST API-JA koji omogućuje dodavanje JSON podataka u spremište zapisnika analize.  Ovaj API-JA možete koristiti kad imate aplikaciju koja ne nudi podataka pomoću jednog od drugih izvora podataka ili rješenja.  Može se koristiti za popunjavanje spremište iz bilo kojeg klijenta koji možete nazvati u API-JA ne ovise o rasporedu zbirke bilo koje izvora podataka i rješenja.

- [Sakupljač web-aplikacija zapisnika analize podataka HTTP API-JA](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Dohvaćanje podataka iz spremišta zapisnika analize

Za dohvaćanje podataka iz spremišta OMS na više načina.  Možda želite da korisnici za dohvaćanje podataka pomoću konzole za OMS i pošaljite im različite vrste vizualizacija i analize.  Također možete dohvatiti podatke iz vanjskih postupak kao što su drugo rješenje upravljanja.

#### <a name="log-searches"></a>Zapisnika pretraživanja

Sve podatke pohranjene u spremištu OMS je dostupan putem zapisnika pretraživanja.  Korisnici mogu tema vlastite ad-hoc na konzoli OMS ili stvaranje nadzorne ploče s vizualizacije za određeni zapisnika pretraživanja.  Rješenja sadrže prilagođene prikaze s vizualizacijama utemeljenog na unaprijed definirane pretraživanja.  API-JA za pretraživanje zapisnika možete koristiti za pristup podacima u spremištu OMS na vanjskom alatu za aplikaciju ili upravljanje.  

- [Zapisnika pretraživanja u zapisnik Analytics](../log-analytics/log-analytics-log-searches.md)
- [Zapisnik analize zapisnika pretraživanja REST API-JA](../log-analytics/log-analytics-log-search-api.md)
- [Prijava analitiku cmdleta](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Prilagođeni prikazi 
Dizajner prikaza omogućuje stvaranje prilagođenih prikaza na konzoli OMS koji korisnicima omogućili vizualizaciju i analiza podataka u rješenje.  Svaki prikaz sadrži pločica koji se prikazuje na glavnoj stranici konzole i bilo koji broj dijelova vizualizacije temeljene na zapisnika pretraživanja koju ste definirali.
  
- [Dizajner prikaza analitičkih podataka za zapisnik](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Prijava analitiku automatski podatke možete izvesti u spremištu OMS u Power BI tako da omogućuje korištenje njegove vizualizacije i alata za analizu.  Ovaj izvoz izvodi na raspored tako da se podaci se ažurni. 

- [Izvoz zapisnika analize podataka u Power BI](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automatizacija

OMS procesi mogu se automatizirati Upoznajte prikupljene podatke ili izvoditi druge funkcije za upravljanje.  Može prikupiti podatke iz aplikacije i umetnite je u spremištu OMS ili možda automatizirati ispravak poznatih problema u odgovoru podaci pronađeni u spremištu. 

![Automatizacija](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Runbooks u automatizaciji Azure pokrenite skripte komponente PowerShell i tijekove rada Azure oblaka.  Možete ih koristiti da biste upravljali resursa u Azure ili ostale resurse koji se može pristupiti iz oblaka.  Runbooks mogu se izvoditi u lokalnom podatkovnog centra pomoću hibridnog Runbook tempiranja.  Na runbook možete započeti s portala za Azure ili vanjski procesa pomoću brojne načine, kao što su PowerShell ili Automatizacija API-JA.

- [Pokretanje programa runbook u automatizaciji Azure](../automation/automation-starting-a-runbook.md)
- [Azure cmdleti za automatizaciju](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automatizacija REST API-JA](https://msdn.microsoft.com/library/mt662285.aspx)
- [Automatizacija .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Upozorenja

Upozorenja pravila automatski pokrenuti zapisnika pretraživanja prema rasporedu.  Ako rezultate odgovaraju određenom kriteriju dobivene upozorenje možete pokrenuti na runbook u automatizaciji Azure ili poziva webhook koje možete pokrenuti vanjski postupak.  Obje te odgovora detalje upozorenja uključujući podatke koji se vraćaju u zapisniku pretraživanja možete unijeti.

- [Upozorenja u zapisnik Analytics](../log-analytics/log-analytics-alerts.md)
- [Zapisnik analize upozorenja API-jem](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Sigurnosno kopiranje i vraćanje web-mjesta

Azure sigurnosno kopiranje i vraćanje web-mjesta ponudite usluge za zaštitu podataka tvrtke i Provjera dostupnosti poslužitelje i programe.  Omogućuje korištenje tih servisa da biste izvršili takve scenariji kao pruža sigurnosne kopije usluge za korištenje aplikacije ili pokretanje prebacivanje od virtualnog računala.

- [Azure cmdleta sigurnosnog kopiranja](https://msdn.microsoft.com/library/mt619253.aspx)
- [Oporavak Azure web-mjesta REST API-JA](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Cmdleti za oporavak Azure web-mjesta](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Prilagođena rješenja

Integracija logike možete Enkapsulacija u Prilagođena rješenja za pokretanje u radnom prostoru ili u prostoru klijenta.  Rješenje možete sadrži neki od načina za integraciju u ovom članku osim drugih resursa možete unijeti scenarij dovršeno upravljanja.  Resursa u rješenje su pakirat tako da se kada je rješenje ukloniti, svi resursa koji je stvorio uklonjeni su iz radnog prostora OMS i Azure pretplate.

Na primjer, rješenje može obuhvaćati programa automatizacije runbook prikupljanje i obrada podataka i popunjavanje spremište prijava analitiku korištenja API za prikupljanje podataka HTTP-a.  Nije moguće uključiti i prilagođeni prikaz koji predstavlja i analiziraju prikupljene podatke.  

- Stvaranje prilagođenih rješenja (uskoro dostupno)    

## <a name="next-steps"></a>Daljnji koraci
- Reference [OMS SDK](operations-management-suite-sdk.md) tehničke informacije o automatizaciji OMS usluga.  
