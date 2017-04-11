<properties
    pageTitle="Pregled metriku u Microsoft Azure | Microsoft Azure"
    description="Pregled metriku i njihovo korištenje u Microsoft Azure"
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Pregled metriku u Microsoft Azure 

U ovom se članku opisuje što metriku su u Microsoft Azure njihove prednosti te upute za početak rada s njima.  

## <a name="what-are-metrics"></a>Što su metriku?

Azure Monitor omogućuje korištenje telemetrijskih da bi se dobio uvid u performanse i stanju sustava radnih opterećenja na Azure. Najvažnije vrstu Azure telemetrijskih podataka su metriku (naziva se i mjerača performansi) čuje najčešće Azure resursi. Azure Monitor nudi nekoliko načina za konfiguriranje i korištenje ove metriku za nadzor i otklanjanje poteškoća.


## <a name="what-can-you-do-with-metrics"></a>Što možete učiniti s metriku?

Metriku su koristan izvor telemetrijskih i jednostavan način možete učiniti sljedeće:

- **Praćenje performansi** resursa (primjerice VM, web-mjesto ili aplikaciju logike) iscrtavanje njegov mjernih podataka na grafikonu portala i prikvačivanje taj grafikon na nadzornu ploču.
- **Primanje obavijesti o problemu** koje utječu na performanse sustava resursa kada metrike presjek određeni prag.
- **Konfiguriranje automatski akcije**, kao što su autoscaling resursa ili aktiviraju na runbook kada metrike presjek određeni prag.
- **Izvedi napredne analize** i izvješćivanje o pogreškama na performanse i korištenje trendova vaše potrebni.
- **Arhiviranje** povijest performanse ili stanje resursa **za usklađenost/nadzor** svrhe.

##  <a name="metric-characteristics"></a>Značajke mjerenja
Metriku sa sljedećim karakteristikama:

- Sva mjerenja imati **Učestalost 1 minute**. Primit ćete metričkim vrijednost svake minute iz resursa, što vam omogućuje blizu u stvarnom vremenu uvid u stanje i stanju sustava resursa.
- Metriku su **dostupne u novom alatu-tvorničke bez potrebe za uključivanje** ili postavljanje dodatnih Dijagnostika.
- Možete pristupiti **30 dana povijesti** za svaki metriku. Možete brzo pogledati mjesečnu i nedavne trendova u performanse ili stanje vašeg resursa.

možeš:

- Lako otkrivanje, access i **Prikaz svih metriku** putem portala Azure kada odaberete resursa i ih prikazati na grafikonu. 
- Konfiguriranje metričkim **upozorenja pravilo koje šalje obavijest ili poduzima akcije automatiziranog** kada metriku presjek prag koji ste postavili. Automatsko skaliranje je posebno automatiziranog akcija koja omogućuje skaliranje out resurs zadovoljavaju dolazni zahtjevi za učitavanje na web-mjesta ili resurse za izračun. Pravilo za automatsko skaliranje postavku za promjenu veličine u/izvan možete konfigurirati na temelju metrike presjeka praga.
- **Arhiviranje** metriku više ili ih koristiti za izvješćivanje izvanmrežno. Možete usmjeriti na metriku za bloba prostora za pohranu Kada konfigurirate dijagnostičkih postavki za vaš resursa.
- **Strujanje** metriku koncentrator događaj, što vam omogućuje da ih usmjeravanje Azure strujanje analize ili prilagođenih aplikacija za analizu blizu stvarnom vremenu. To možete učiniti na pomoću dijagnostičkih postavki.
- **Usmjeravanje** sve metriku za OMS zapisnika analize da biste otključali izravne analize, pretraživanje i prilagođene upozorenjem na metriku podatke iz resursa.
- **Utrošak** metriku putem novi Azure Monitor REST API-ji.
- Metriku **upita** pomoću cmdleta ljuske PowerShell ili više platformi REST API-JA.

 ![Usmjeravanje mjerenja monitoru za Azure](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Pristup metriku putem portala
Ovo je vodič za brzi stvaranja metričkim grafikona putem portala za Azure

### <a name="view-metrics-after-creating-a-resource"></a>Prikaz metriku nakon stvaranja resursa
1. Otvaranje Azure portal
2. Stvaranje novog servisa aplikacija – Web-mjesta.
3. Kada stvorite web-mjesta, idite na pregled plohu web-mjesta.
4. Možete pogledati nove metrike kao pločicu "Praćenja". Možete uređivati pločicu i odabrati više mjerenja

 ![Metriku na resursa u Azure monitora](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Pristup svim metriku na jednom mjestu
1. Otvorite portal sustava Azure 
2. Idite na novoj kartici Monitor, a zatim odaberite mogućnost metriku ispod nje 
3. S padajućeg popisa možete odabrati pretplate, grupa resursa i naziv resursa. 
4. Sada možete vidjeti popis dostupnih metriku. 
5. Odaberite metriku zanima te iscrtajte ga. 
6. Možete je prikvačiti na nadzornu ploču tako da kliknete na Prikvači u gornjem desnom kutu.

 ![Pristup sva mjerenja na jednom mjestu u Azure monitora](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Glavno računalo razinom metriku iz VMs (Azure Voditelj resursa koji se temelje) i VM skaliranje skupove možete pristupiti bez dodatne postavke dijagnostičkih. Ove nove metrike glavno računalo razinom dostupni su za Windows i Linux instance. Ove metriku ćete neće se zbuniti s metriku razine goste OS kojima imate pristup kad uključite Dijagnostika Azure na VMs ili VMSS. Dodatne informacije o konfiguriranju Azure Dijagnostika potražite u članku [što je Microsoft Azure Dijagnostika](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Pristup metriku putem REST API-JA
Azure metriku moguće pristupiti putem API Azure Monitor. Postoje dvije API-ji koji olakšavaju otkrivanje i pristup metriku. Korištenje sustava: 

- [Azure Monitor metriku definicije REST API -JA](https://msdn.microsoft.com/library/mt743621.aspx) za pristup popisu metriku dostupna za servis.
- [Azure Monitor metriku REST API -JA](https://msdn.microsoft.com/library/mt743622.aspx) za pristup podacima stvarni mjerenja

>[AZURE.NOTE] U ovom se članku opisuje metriku putem [Novi API-JA za metriku](https://msdn.microsoft.com/library/dn931930.aspx) za Azure resurse. API verzija ima li novih metričkim definicija API je 2016 03 01 i verzija za metriku API je 2016-09-01. Naslijeđene metričkim definicije i metrike možete pristupiti s api-verzija 2014.-04-01.

Objašnjenje detaljnije pomoću REST API-ji Monitor Azure, potražite u članku [Azure Monitor REST API vodič](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Mogućnosti za metriku izvoza
Možete i otvoriti plohu zapisnika dijagnostike na kartici Monitor i mogućnosti izvoza za metriku prikaza. Možete odabrati metriku (i dijagnostičke zapisnike) usmjerava blobova, koncentratora događaj ili OMS prijava analitiku za korištenje slučajeve prethodno spominju u ovom članku. 

 ![Mogućnosti za metriku Azure monitoru izvoza](./media/monitoring-overview-metrics/MetricsOverview3.png)   

To možete konfigurirati putem resursima predlošci [PowerShell](insights-powershell-samples.md), [EŽA](insights-cli-samples.md) ili [REST API -ji](https://msdn.microsoft.com/library/dn931943.aspx). 

## <a name="take-action-on-metrics"></a>Akcija mjerenja
Možete primati obavijesti ili automatiziranog poduzeti metričkim podataka. Možete konfigurirati pravila za upozorenja ili automatsko skaliranje postavke da biste to učinili.

### <a name="alert-rules"></a>Upozorenja pravila
Na određene parametre možete konfigurirati upozorenja pravila. Ta pravila za upozorenja možete provjeriti ako metrike sadrži su prijeđene određeni prag i vas obavijesti putem e-pošte ili pokrenuti webhook koji se zatim može koristiti za izvođenje bilo koje prilagođene skripte. Da biste konfigurirali 3 integracije proizvoda možete koristiti i u webhook.

 ![Upozorenja pravilima Azure monitoru i mjerenja](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Automatsko skaliranje
Neke Azure resurse podržava više instanci za promjenu veličine izgleda ili u rukovati na radnih opterećenja. Automatsko skaliranje odnosi na aplikaciju (Web apps), virtualnog računala skaliranje skupove (VMSS) i klasični oblaka Services. Možete konfigurirati pravila za automatsko skaliranje za promjenu veličine izgleda ili u kada određene metrike koje utječu na svoje radno opterećenje presjek prag koju navedete. Dodatne informacije potražite u članku [Pregled autoscaling](monitoring-overview-autoscale.md).

 ![Metriku i automatsko skaliranje monitoru za Azure](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Podržane usluge i metrike
Azure Monitor je novi metriku infrastrukture. Pruža podršku za sljedeće Azure servisa Azure portal i novu verziju Azure Monitor API-JA:

- VMs (Azure Voditelj resursa koji se temelje)
- Skupovi VM skala
- Grupe
- Prostor naziva koncentrator za događaj 
- Prostor za naziv servisa Bus (premium SKU samo)
- SQL (verzija 12)
- Skup elastic SQL
- Web-mjesta
- Farme poslužitelja web
- Logika aplikacije
- IoT koncentratora
- Redis predmemorije
- Povezivanje s mrežom: Aplikacija pristupnika
- Pretraživanje

Možete pogledati na detaljni popis podržanih servisa i njihovih metriku na [monitoru Azure metriku - podržani metriku po vrsti resursa](monitoring-supported-metrics.md). 


## <a name="next-steps"></a>Daljnji koraci

Pogledajte veze u ovom članku. Uz to, dodatne:  

- o [uobičajenih metriku autoscaling](insights-autoscale-common-metrics.md)
- Kako [stvoriti pravila za upozorenja](insights-alerts-portal.md)




