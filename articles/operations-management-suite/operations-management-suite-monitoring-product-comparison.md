<properties 
   pageTitle="Microsoft nadzor Usporedba proizvoda | Microsoft Azure"
   description="Microsoft operacije upravljanja paket (OMS) je tvrtke Microsoft cloud koji se temelje IT rješenja upravljanja koji olakšava upravljanje i zaštita lokalnih i infrastrukture u oblaku.  U ovom se članku prepoznaje u različitim servisima obuhvaćeno OMS i navode veze na detaljne sadržaja."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>Microsoft nadzora proizvoda usporedbe

Ovaj članak sadrži usporedbe između sustava centra operacije Manager (SCOM) i zapisnika analize u paketu operacije upravljanja (OMS) pomoću njihovih arhitektura, logičku vrijednost način na koji praćenje resurse i kako ih tema oni prikupljanje podataka.  To je da bi se dobilo temeljne razumijevanja njihove razlike i relativne prednosti.  

## <a name="basic-architecture"></a>Osnovni arhitekture
### <a name="system-center-operations-manager"></a>Centar za sustav Operations Manager

Sve komponente SCOM instalirane su u podatkovnom centru.  [Agenata instaliranih](http://technet.microsoft.com/library/hh551142.aspx) na Windows i Linux kojima upravlja SCOM.  Agenata povezuje s [Poslužiteljima upravljanja](https://technet.microsoft.com/library/hh301922.aspx) koji komunicirati s SCOM skladištu baze podataka i podataka.  Agenata oslanjate domene provjere autentičnosti za povezivanje s poslužiteljima upravljanja.  One izvan pouzdana domena izvršiti provjera autentičnosti potvrda ili povezivanje s [Pristupni poslužitelj](https://technet.microsoft.com/library/hh212823.aspx).

SCOM zahtijeva dvije baze podataka SQL, jedan za upotrebljiva podataka i drugi skladištu podataka za podršku analiza podataka i izvješćivanje o pogreškama.  [Izvješćivanje o pogreškama poslužitelja](https://technet.microsoft.com/library/hh298611.aspx) pokreće SQL Reporting Services da biste prijavili na podacima iz skladištu podataka. 

SCOM možete nadzirati oblaka resurse pomoću upravljanja paketi za proizvode kao što su [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708)i [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  Paketi te upravljanje upotrijebili jedan ili više lokalne agenata proxy poslužitelja za željeli otkriti oblaka resurse i pokrenuti tijekova rada za mjerenje njihove performanse i dostupnost.  Proxy agenata koriste i na [monitoru mrežne uređaje](https://technet.microsoft.com/library/hh212935.aspx) i drugim vanjskim izvorima.

Na konzoli operacije je aplikacija za Windows koji spaja u jednu od poslužitelji za upravljanje i omogućuje administratora za prikaz i analiza podataka prikupljenih i konfigurirati okruženje SCOM.  Konzola za utemeljen na webu mogu nalaziti na bilo kojem poslužitelju IIS i njihovi analiza podataka putem preglednika.

![SCOM arhitekture](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Zapisnik Analytics

Većina OMS komponente su Azure oblaku da biste mogli implementirati i upravljanje s minimalnim trošak i trud administratora.  Sve podatke prikupljene putem prijava analitiku pohranjen u spremištu OMS.

Prijava analitiku možete prikupiti podatke neku od tri izvora:

- Fizičke i virtualnim računalima sustava Windows i [Microsoft nadzor Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) ili Linux i [Postupci upravljanja paket Agent za Linux](https://technet.microsoft.com/library/mt622052.aspx).  Ove strojeva može biti lokalnog ili virtualnim strojevima Azure ili neki drugi oblaka.
- Račun za pohranu Azure s [Azure Dijagnostika](../cloud-services/cloud-services-dotnet-diagnostics.md) podatke prikupljene putem uloga Azure tempiranja, uloga web ili virtualnog računala.
- [Veze u grupu za upravljanje SCOM](https://technet.microsoft.com/library/mt484104.aspx).  U ovoj konfiguraciji na agenata komunicirati s poslužiteljima upravljanja SCOM koji izlaganje podatke u bazu podataka SCOM ga zatim isporuke OMS spremište podataka.
Administratori analize podataka prikupljenih i konfiguriranje prijava analitiku pomoću portala za OMS koji se nalazi u Azure te im možete pristupiti s bilo kojeg preglednika.  Mobilne aplikacije da biste pristupili tih podataka dostupni su za standardne platformama.

![Arhitektura analize zapisnika](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integriranje SCOM i analize zapisnika

Kada SCOM se koristi kao izvor podataka za analizu zapisnika omogućuje korištenje značajki oba proizvoda u hibridno okruženje za nadzor.  Možete konfigurirati postojeće agenata SCOM putem konzole za operacije na upravlja OMS, osim nastavka da biste pokrenuli paketi za upravljanje iz SCOM.  
Podatke iz povezanih grupu za upravljanje SCOM je isporučen prijava analitiku na jedan od četiri načina:

- Prikupljene putem agenta i isporučuju SCOM događaja i podataka o performansama.  Upravljanje poslužiteljima u SCOM zatim isporučiti podatke analize zapisnika.
- Neke događaje kao što su IIS zapisnika i sigurnosne događaje i dalje će se dostavljati izravno u prijava analitiku agenta.
- Neka rješenja će isporuka dodatnim softverom agenta ili potrebno instalirati softver za prikupljanje dodatnih podataka.  Ove podatke obično poslat će se izravno u zapisnik analize.
- Neka rješenja će prikupljati podatke izravno iz poslužitelji za upravljanje SCOM koja proizlazi iz agenta.  Ako, na primjer, [rješenje upozorenja upravljanja](https://technet.microsoft.com/library/mt484092.aspx) prikuplja upozorenja iz SCOM kada su stvorili.

## <a name="monitoring-logic"></a>Nadzor logike
SCOM i prijava analitiku raditi s slične podatke prikupljenih agenata, ali imaju temeljne razlike u kako ih definirati i implementirati njihove logike za prikupljanje podataka i kako ih analizirali podatke koji su prikupljanje.

### <a name="operations-manager"></a>Operations Manager
Nadzor logike za SCOM je implementirana u [paketi za upravljanje](https://technet.microsoft.com/library/hh457558.aspx) koji sadrže logike za otkrivanje komponente da biste pratili, mjerenje stanje tih komponenti, kao i za prikupljanje podataka za analizu.  Nadzor podataka može biti jednostavan prikupljanje brojač se događaj ili performanse ili ga koristiti složene logike implementirana u skripti.  Upravljanje paketi koji sadrže potpuni nadzor dostupni su za različite tvrtke [Microsoft i aplikacijama drugih proizvođača](http://go.microsoft.com/fwlink/?LinkId=82105) osim hardver i mrežne uređaje.  Možete [Autor vlastite upravljanje paketi](http://aka.ms/mpauthor) za prilagođene aplikacije.

Upravljanje paketi sadrže više tijekova rada svaki izvodi neke funkcija distinct nadzora kao što su uzorkovanje performanse brojač, provjera stanja servisa i pokrenuti skriptu.  Svaki tijek rada pokreće zasebno, a definira vlastitu rezultate kao što su baze podataka koja će pisati u i je li će generirati upozorenja. 

Možete nadjačati pojedinosti tijeka rada kao što su učestalost se pokrenu prag gdje se preporučuje se pogreška i težinu upozorenje stvaraju.  Dodatnim funkcijama koje možete unijeti i dodavanjem vlastite tijekova rada.

![Nadjačava](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Upravljanje paketi su instalirani u bazi podataka za komponentu Operations Manager i automatski distribuira u agenata poslužitelji za upravljanje.  Agent za svaki automatski se preuzimanje paketa za upravljanje i učitavanje odgovarajući aplikacijama koje su instalirali tijekova rada.  Podatke prikupljene putem agenta je isporučena na poslužitelj za upravljanje za umetanje u SCOM skladištu baze podataka i podataka.  Konzola za operacije omogućuje prikaz i analiza te podatke putem prilagođene prikaze, nadzornih ploča i izvješća koji su uključeni u paket za upravljanje.

Distribucija paketi za upravljanje je što je prikazano na sljedećem su dijagramu.

![Upravljanje paket tijeka](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Zapisnik Analytics
#### <a name="event-and-performance-collection"></a>Prikupljanje performanse i događaja
Prijava analitiku prikuplja događaja i mjerača performansi agent Systems pomoću izvora kao što su zapisnik događaja sustava Windows, IIS zapisnika i Syslog.  Možete definirati kriterija za koju podaci prikupljene putem portala za zapisnika analize i stvaranje zapisnika upita da biste analizirali podatke prikupljene.  Skup kriterija standardne se definira kada stvarate OMS radnog prostora, a možete definirati dodatne podatke za određene aplikacije. 

![Definiranje zapisnika događaja u zapisnik Analytics](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Dok SCOM ima brojne tijekove rada detaljne koji obično definiraju određene kriterije za podatke i akcije koje bi trebalo odgovor, analize zapisnika ima više općenitih kriterije za prikupljanje podataka.  Zapisnik upita i rješenja sadrže više ciljano kriterije za analizu i djeluje na konkretne podatke u oblaku kada je prikupljeni.

#### <a name="solutions"></a>Rješenja
Rješenja sadrže dodatne logike za prikupljanje podataka i analize.  Možete odabrati rješenja za dodavanje pretplate OMS iz galerije rješenja.

![Galerije rješenja](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

U oblaku koja omogućuje analizu događaja i mjerača performansi prikupljeni u spremištu OMS prvenstveno pokrenuti rješenja.  Također mogu definirati dodatnih podataka mogu prikupiti koji se mogu analizirati zapisnika upita ili putem dodatne korisničko sučelje koje ste dobili od rješenje na nadzornoj ploči OMS. 

Na primjer, [Evidentiranje promjena rješenje](https://technet.microsoft.com/library/mt484099.aspx) otkriva promjene konfiguracije sustavima agent i zapisivanja događaja u spremište OMS koje možete analizirati s nekoliko grafičkog prikaza koji daju sažeti pronašao promjene.  Moguće je dubinski analizirati iz prikaza sažete u zapisnik upita koji prikazuju detaljne podatke prikupljene putem rješenja.


Dok odabirete rješenja dodati u pretplatu trenutno nemate mogućnost stvoriti vlastiti rješenja.  Možete odabrati događaja i mjerača performansi mogli ste prikupljati i stvaranje prilagođenih prikaza koji se temelje na upitima zapisnika.

Nadzor logike za analizu zapisnika je navedene u sljedećem su dijagramu.

![Tijek analize rješenje zapisnika](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Nadzor stanja
### <a name="operations-manager"></a>Operations Manager
SCOM možete modela različite komponente za aplikaciju i dati u stvarnom vremenu stanja za svaki unos.  Time ne samo prikaz otkriven pogrešaka i performansama tijekom vremena, ali i da biste provjerili valjanost stvarni stanju aplikacije ili sustav i svih njezinih komponenti u bilo kojem trenutku.  Budući da ga možete koristiti vremenskih razdoblja koja je dostupna aplikacija modul stanja u SCOM podržava servisa razinu ugovore (SLA) koji se analizirati i stvaranje izvješća o dostupnosti aplikacija tijekom vremena.

Ako, na primjer, ispod prikazuje se u stvarnom vremenu stanju modula baze podataka za SQL nadzire SCOM.  Stanje svakog baze podataka za onom modula za baze podataka prikazuje se na dnu polovici prikaza.

![Prikaz stanja](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Explorer stanja za onom modula za baze podataka prikazuje se ispod s monitorima koji se koriste za utvrđivanje njegov stanje.  Ove monitora definira u paketu za upravljanje SQL i pokrenuti SQL baza podataka moduli otkriti SCOM.

![Stanje explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Komponente na više sustava možete kombinirati za mjerenje stanja distribuirane aplikacije.  To može biti osobito korisni za skup poslovnih aplikacija koje uključuju više raspodijeljeno komponenti.  U dostupnosti za aplikaciju možete stvoriti model koji mjeri stanje svaku od njih te skupne vrijednosti.

Primjer jedan upravljanje paketa koji sadrži model da biste analizirali njegove raspodijeljeno komponente je Active Directory.  Ogledna dijagramu u nastavku prikazuje stanje cjelokupan okruženje i odnos između šuma, domena i kontrolera domena.  Svaki od tih komponenti obuhvaća potkomponente i slično gornji primjer SQL više monitora.

![Prikaz dijagrama SCOM](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Zapisnik Analytics
OMS obuhvaćaju uobičajenih modul modela aplikacijama ili mjeru njihova stanja u stvarnom vremenu.  Pojedinačne rješenja možda procijenite stanje određene usluge na temelju prikupljenih podataka, a mogu instalirati prilagođene logike na agent za analizu u stvarnom vremenu.  Jer rješenja pokrenuli u oblak pomoću programa access u spremište OMS, omogućuju možete često dublju analizu nego što obično obavlja upravljanje paketi. 

Na primjer, [AD procjenu i procjenu SQL rješenja](https://technet.microsoft.com/library/mt484102.aspx) analize podataka prikupljenih i ocjenjivanje različite aspekte okruženje.  Obuhvaća preporuke za poboljšanja koje je moguće izvršiti da biste poboljšali dostupnosti i performanse okruženje.

![AD rizika rješenja](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Analiza podataka
SCOM i prijava analitiku svaki omogućuju različite značajke da biste analizirali podatke prikupljene.  SCOM sadrži prikaze i nadzorne ploče na konzoli postupci za analizu nedavne podataka u raznim oblici i izvješća za prezentiranje podataka iz skladištu podataka u tabličnom obliku.  Prijava analitiku nudi cjelovit zapisnika jezik upita i sučelje za analiza podataka u spremištu OMS.  Kada SCOM se koristi kao izvor podataka za analizu zapisnika, spremište sadrži podatke prikupljene putem SCOM tako da se Alati za prijava analitiku može se koristiti za analizu podataka koji se oba sustava.

### <a name="operations-manager"></a>Operations Manager

#### <a name="views"></a>Prikazi
Prikazi na konzoli operacije omogućuju prikaz različitih vrsta podataka prikupi SCOM u različitim oblicima zapisa, obično tablični za događaje, upozorenja i podatke o stanju i linijski grafikoni za podataka o performansama.  Prikazi izvođenje minimalnog analizu ili konsolidacije podataka, ali jednostavan način možete filtrirati prema određenog kriterija. 

![Prikazi](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Upravljanje paketi dat će obično višestrukih prikaza za podršku aplikacije ili sustav koji je nadzire.  To može obuhvaćati stanje prikaza za različite objekte koji otkrije paket za upravljanje, upozorenja prikazi otkriveni problemi i performanse prikazi za mjerača.

Prikazi su osobito prikladna za analizu trenutno stanje okruženje uključujući otvaranje upozorenja i stanja nadziranim sustavi i objekte.  Omogućuje dubinsku analizu prema dolje do detaljne događaj ili podrške određeni upozorenje da bi se dijagnosticiranje njegov korijenski uzrok podataka o performansama. Isto tako, možete pogledati performanse i stanje komponenti za različite aplikacije u procjeni njegov trenutno stanje.

#### <a name="dashboards"></a>Nadzorne ploče
Nadzorne ploče na konzoli operacije funkcionira prvenstveno s istim podacima kao prikaza, ali su prilagodljivije a mogu sadržavati bogatiji vizualizacije.  Skup standardne nadzornih ploča dostupnih da jednostavno možete prilagoditi vlastitim svrhe.  Možete koristiti i PowerShell miniaplikacije koji može prikazati podatke vratio upit PowerShell.

![Nadzorna ploča](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Razvojni inženjeri imati mogućnost dodavanja prilagođene komponente u nadzorne ploče obuhvatiti njihove upravljanje paketi.  To može biti vrlo oblici sadrže za određenu aplikaciju kao što je nadzorna ploča u paketu za upravljanje SQL prikazano u nastavku.  Ovu nadzornu ploču može se koristiti kao predložak za prilagođene verzije.

![Nadzorna ploča za SQL](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Izvješća
Izvješća u SCOM analiza podataka iz skladištu podataka u tabličnom obliku.  Mogu se ispisati i zakazani automatiziranog isporuke u drugim oblicima datoteka uključujući PDF-a, CSV i Word.  Izvješća rad s podacima iz skladištu podataka tako da ih je osobito prikladna za analizu dugoročnu trendova.

Upravljanje paketi obično nudi prilagođena izvješća za određenu aplikaciju.  Možete odabrati i iz biblioteke generički izvješća koje možete prilagoditi vlastitim aplikacije ili za ad hoc analiziranja.

Slijedi oglednog izvješća performanse prikazuje podatke prikupljene putem paket za upravljanje Active Directory.

![Izvješće](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Zapisnik Analytics
Prijava analitiku ima [Jezik upita](https://technet.microsoft.com/library/mt484120.aspx) koji koristite za analizu preko podataka iz više aplikacija bez potrebe za stvaranje prilagođenog prikaza i izvješća.  Jer je OMS implementirana u oblaku, performansi upita i analiza podataka nisu primjenjuje ograničenja hardver i brze analize upita uključujući milijune zapisa. 

Upiti u zapisnik analize i su temelj druge funkcije.  Možete spremiti upit, Izvezi rezultate u Excel ili da se automatski pokrenuti u pravilnim vremenskim razmacima i generiranje upozorenja ako rezultatima odgovaraju određenom kriteriju.  

![Tijek zapisnika upita](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

U nastavku je prikazan primjer prijava analitiku upita.  U ovom primjeru su svi događaji s "rada" u nazivu vratio i grupirane događaj ID-a.  Korisniku omogućuje jednostavno upit, a prijava analitiku dinamički stvara korisničko sučelje za izvođenje analizu.  Odabirom bilo koju stavku na popisu vratit će se događaj detaljne podatke.

![Zapisnik upita](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Osim pruža ad-hoc analizu upita u prijava analitiku mogu spremiti za buduću upotrebu i također se dodaje na [nadzornoj ploči OMS](http://technet.microsoft.com/library/mt484090.aspx) kao što je prikazano u sljedećem primjeru.

![OMS nadzorne ploče](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Daljnji koraci

- Implementacija [sustava centra Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
- Prijavite se za [Analizu zapisnika](https://azure.microsoft.com/documentation/services/log-analytics).  
