<properties 
    pageTitle="Upute za postavljanje vozila telemetrijskih analize rješenje predložak nadzorne ploče PowerBI | Microsoft Azure" 
    description="Korištenje mogućnosti programa obavještavanje Cortana da bi se dobio u stvarnom vremenu i predvidljivu uvida na vozila stanja i vožnju navike." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Vozila telemetrijskih analize rješenje predložak nadzorne ploče PowerBI upute za postavljanje

U ovom veze prema **izborniku poglavlja u ovom playbook** . 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


Rješenje Analytics za Telemetriju vozila showcases kako dealerships automobila, kupnju proizvođači i osiguranja tvrtki omogućuje korištenje mogućnosti obavještavanje Cortana da bi se dobio u stvarnom vremenu i predvidljivu uvida na stanje vozila i vožnju navike na pogon poboljšanja u području klijenta programa, R & D i marketinških kampanja. Ovaj dokument sadrži korak po korak upute kako možete konfigurirati PowerBI izvješća i nadzorne ploče kada je rješenje uveden u svoju pretplatu. 


## <a name="prerequisites"></a>Preduvjeti
1.  Implementacija rješenja Analytics za Telemetriju vozila tako da odete [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3)  
2.  [Instalirajte Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3.  Za [Azure pretplate](https://azure.microsoft.com/pricing/free-trial/). Ako nemate pretplatu na Azure, početak rada s Azure besplatna pretplata
4.  PowerBI Microsoftov račun
    

## <a name="cortana-intelligence-suite-components"></a>Cortana obavještavanje paket komponente
Kao dio rješenja predložak Analytics za Telemetriju vozila, sljedeći servisi obavještavanje Cortana su implementiran u svoju pretplatu.

- **Događaj koncentratora** za ingesting milijune vozila telemetrijskih događaje u Azure.
- **Analitički strujanje**s pokušavaju uvida u stvarnom vremenu na stanje vozila i nastavi tih podataka u Dugoročne prostora za pohranu za bogatiji obrade analize.
- **Strojnog učenja** za otkrivanje značajkom u stvarnom vremenu i skupna obrada da bi se dobio predvidljivu uvide.
- **HDInsight** je leveraged za pretvaranje podataka na razini
- **Tvorničke podataka** rukuje djelovanje, Izrada rasporeda, Upravljanje resursima te praćenje obrade kanala.

**Power BI** daje rješenja obogaćenog nadzorne ploče za podatke u stvarnom vremenu i vizualizacije predvidljivu analize. 

Rješenje koristi dvije različitih izvora podataka: **signale Simulirani vozila i skup dijagnostičkih podataka** i **vozila kataloga**.

Simulator za telematics vozila je dio paketa rješenja. Emits dijagnostičke informacije i signale odgovara stanje vozila i uputa za vožnju uzorak u određenom trenutku u vremenu. 

Katalog vozila je dataset reference koje sadrže VIN za mapiranje modela


## <a name="powerbi-dashboard-preparation"></a>Priprema PowerBI nadzorne ploče

### <a name="deployment"></a>Uvođenje

Kada implementacijskih dovrši, trebali biste vidjeti na sljedećem su dijagramu sa svim te komponente označena ZELENOM bojom. 

- Da biste došli na odgovarajuće services da biste provjerili koristi li se sve te uspješno uveli, kliknite strelicu u gornjem desnom kutu čvorove zelena.
- Da biste preuzeli paket simulator podataka, kliknite strelicu na gornjem izravno na čvor **Simulator Telematics vozila** . Spremite i izdvojiti datoteke lokalno na vašem računalu. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Sada ste spremni za konfiguriranje PowerBI nadzornu ploču s obogaćenim vizualizacijama da bi se dobio u stvarnom vremenu i navike predvidljivu uvida na vozila stanja i vožnju. Jedan sat da biste stvorili sva izvješća i nadzorne ploče za konfiguriranje traje 45 minuta. 


### <a name="setup-power-bi-real-time-dashboard"></a>Postavljanje nadzorne ploče Power BI u stvarnom vremenu

**Generiranje Simulirani podataka**

1. Na lokalnom računalu, otvorite mapu u kojoj dobivenih vozila Telematics Simulator paketa![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  Izvršavanje aplikacije ***CarEventGenerator.exe***.
3.  Emits dijagnostičke informacije i signale odgovara stanje vozila i uputa za vožnju uzorak u određenom trenutku u vremenu. To je objavljeno na instancu koncentrator Azure događaja koji je konfiguriran kao dio implementaciju sustava.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**Pokretanje aplikacije u stvarnom vremenu nadzorne ploče**

Rješenje sadrži aplikacije koje generira nadzorne ploče komponente u stvarnom vremenu u PowerBI. Instancu koncentrator događaj iz kojeg strujanje analize objavljuje događaje neprestano očekuje podatke te aplikacije. Za svaki događaj koji prima ovu aplikaciju obrađuje podataka pomoću krajnjoj točki bilježenja rezultata strojnog učenja-odgovor na zahtjev. Konačni dataset Objavi automatske PowerBI API-ji za vizualizaciju. 

Da biste preuzeli aplikacije:

1.  Kliknite čvor PowerBI u prikazu dijagrama, a zatim **Preuzmite aplikaciju nadzorne ploče u stvarnom vremenu**"veze u oknu svojstva.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Izdvajanje i spremiti lokalno aplikacije![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  Izvršavanje aplikacije **RealtimeDashboardApp.exe**
4.  Unesite vjerodajnice za valjane Power BI, prijavite se i kliknite **Prihvati**
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>Konfiguriranje PowerBI izvješća
U stvarnom vremenu izvješća i nadzorne ploče da biste dovršili potrajati otprilike 30 45 minuta. Idite na web-mjesto [http://powerbi.com](http://powerbi.com) i u okvir za prijavu.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

U dodatku Power BI generira se novi skup podataka. Kliknite **ConnectedCarsRealtime** skupu podataka.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Spremite Prazno izvješće pomoću **Ctrl + s**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Navedite naziv izvješća *vozila Telemetrijskih analitičke podatke u stvarnom vremenu - izvješća*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>U stvarnom vremenu izvješća
Postoje tri izvješća u stvarnom vremenu u rješenja:

1.  Vozila u postupku
2.  Vozila potrebno održavanja
3.  Statističkih podataka o stanju vozila

Možete konfigurirati sva izvješća tri u stvarnom vremenu ili zaustavljanje nakon sve razine i nastavite sa sljedećim odjeljkom konfiguriranja obradu izvješća. Preporučujemo vam da stvorite tri izvješća vizualizirati cijelog uvida u stvarnom vremenu puta rješenja.  

### <a name="1-vehicles-in-operation"></a>1. vozila u postupku
  
Dvokliknite **stranica 1** , a zatim je preimenovati "Vozila u operaciji"  
    ![Povezani automobilima - vozila u postupku](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Odaberite polje **vin** iz **polja** , a zatim odaberite vrstu vizualizacije kao **"Kartica"**.  

Vizualizacija kartica stvara se kao što je prikazano na slici.  
    ![Povezani automobilima – odabir vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Kliknite prazno područje da biste dodali novu vizualizaciju.  

Odaberite **Grad** i **vin** polja. Promjena vizualizacije **"**kartu". Povucite **vin** u područje vrijednosti. Povucite **Grad** iz polja u područje **legende** .   
    ![Povezani automobilima - vizualizacija kartica](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
Odaberite **oblik** sekcije iz **vizualizacije**, kliknite **Naslov** i promijenite **tekst** u **"Vozila u operaciji po gradu"**.  
    ![Povezani automobilima - vozila u operaciji po gradu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Konačni vizualizacije izgleda kao što je prikazano na slici.    
    ![Povezani automobilima - konačni vizualizacije](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Kliknite prazno područje da biste dodali novu vizualizaciju.  

Odaberite **Grad** i **vin**, promijenite vrstu vizualizacije u **Grupirani stupčasti grafikon**. Provjerite polja **Grad** u **područje os** i **vin** u **području vrijednosti**  

Sortirajte grafikon **"Broj vin"**  
    ![Povezani automobilima - broj vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Promijenite **Naslov** grafikona u **"Vozila u operaciji po gradu"**  

Kliknite **oblik** sekciju, a zatim odaberite **Podataka boja**, kliknite na **"U"** da biste **Prikazali sve**  
    ![Povezani automobilima - Prikaži sve boje podataka](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Promjena boje pojedinačne grad tako da kliknete ikonu boja.  
    ![Povezani automobilima – a zatim Promijeni boje](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Kliknite prazno područje da biste dodali novu vizualizaciju.  

Odaberite vizualizaciju **Grupirani stupčasti grafikon** s vizualizacije, povucite polje **Grad** u područje **os** , **Model** u području **legende** i **vin** u područje **vrijednosti** .  
    ![Povezani automobilima – Grupirani stupčasti grafikon](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Povezani automobilima - vizualizacije](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
Razmještanje sve vizualizacije na ovoj stranici, kao što je prikazano na slici.  
    ![Povezani automobilima - vizualizacije](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Uspješno ste konfigurirali izvješća u stvarnom vremenu "Vozila u operaciji". Možete nastaviti stvaranje sljedeći u stvarnom vremenu izvješća ili ovdje prekinite i konfiguriranje na nadzornoj ploči. 

### <a name="2-vehicles-requiring-maintenance"></a>2. vozila potrebno održavanja
  
Kliknite ![Dodaj](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) da biste dodali novo izvješće, preimenujte u **"vozila potrebno održavanja"**

![Povezani automobilima - vozila potrebno održavanja](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Odaberite polje **vin** i promijenite vrstu vizualizacije **karticu**.  
    ![Povezani automobilima - vizualizacija kartica Vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Imamo polje pod nazivom "MaintenanceLabel" u skupu podataka. To polje može imati vrijednost "0" ili "1"." Postavlja se model Azure strojnog učenja dodjeli kao dio rješenja i integriran s put u stvarnom vremenu. Vrijednost "1" pokazuje na vozila zahtijeva održavanja. 

Da biste dodali **Razini stranice** filtra za prikaz podataka vozila koje zahtijevaju održavanje: 

1. Povucite polje **"MaintenanceLabel"** u **Filtri na razini stranice**.  
![Povezani automobilima – filtri na razini stranice](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. Kliknite **Osnovno filtriranje** izbornik Prikaz pri dnu MaintenanceLabel stranice razine filtar.  
![Povezani automobilima - Osnovno filtriranje](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  Postavite vrijednost filtra na **"1"**    
![Povezani automobilima – vrijednost filtra](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Kliknite prazno područje da biste dodali novu vizualizaciju.  

**Grupirani stupčasti grafikon** s odaberite vizualizacije  
![Povezani automobilima - vizualizacija kartica Vind](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Povezani automobilima – Grupirani stupčasti grafikon](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Povucite polje **modela** u područje **os** , **Vin** u područje **vrijednosti** . Zatim sortirajte vizualizaciju po **broj vin**.  Promijenite **Naslov** grafikona u **"Vozila potrebno održavanja model"**  

Povucite polja **vin** u **Zasićenost boje** na **polja** ![polja](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) dio **vizualizacije** kartica  
![Povezani automobilima - zasićenost boje](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Promjena **Boja podataka** u vizualizacije iz odjeljka **Oblikovanje**  
Promjena boje Minimum: **F2C812**  
Promjena boje najviše: **FF6300**  
![Povezani automobilima – promjena boja](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Povezani automobilima - nove vizualizacije boje](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Kliknite prazno područje da biste dodali novu vizualizaciju.  

**Grupirani stupčasti grafikon** s odaberite vizualizacije, povucite **vin** polje u područje **vrijednosti** , povucite polje **Grad** u područje **os** . Sortirajte grafikon **"Broj vin"**. Promijenite **Naslov** grafikona u **"Vozila potrebno održavanja po gradu"**   
![Povezani automobilima - vozila potrebno održavanja po gradu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Kliknite prazno područje da biste dodali novu vizualizaciju.  

Odaberite vizualizacija **Kartica više redaka** iz vizualizacije, povucite **Model** i **vin** u područje **polja** .  
![Povezani automobilima – kartica više redaka](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Promjena rasporeda sve vizualizacije, konačni izvještaj izgleda kako na sljedeći način:  
![Povezani automobilima – kartica više redaka](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Uspješno ste konfigurirali izvješća u stvarnom vremenu "Vozila potrebno održavanja". Možete nastaviti stvaranje sljedeći u stvarnom vremenu izvješća ili ovdje prekinite i konfiguriranje na nadzornoj ploči. 

### <a name="3-vehicles-health-statistics"></a>3. statističkih podataka o stanju vozila
  
Kliknite ![Dodaj](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) da biste dodali novo izvješće, preimenujte u **"vozila statističkih podataka o stanju"**  

Odaberite vizualizaciju **mjerača** vizualizacije, a zatim povucite polje **brzinu** u područja **vrijednosti, minimalne vrijednosti, Najveća vrijednost** .  
![Povezani automobilima – kartica više redaka](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Promjena zadanog skupljanja od **brzinu** u **područje vrijednosti** u **prosjeka** 

Promjena zadanog skupljanja od **brzinu** u **području Minimum** **najmanje**

Promjena zadanog skupljanja od **brzinu** u **području najviše** **Maksimalna**

![Povezani automobilima – kartica više redaka](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Preimenujte **Mjerača naslov** **"Prosječna brzina"** 
 
![Povezani automobilima - mjerača](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Kliknite prazno područje da biste dodali novu vizualizaciju.  

Na sličan način dodajte **mjerača** za **ULJE average modul**, **Prosječno goriva**i **average modul temperate**.  

Promjena zadanog skupljanja polja u svakom mjerača kao po iznad koraka u **"Prosječna brzina"** procijeni.

![Povezani automobilima - Gauges](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Kliknite prazno područje da biste dodali novu vizualizaciju.

Odaberite **linijski i Grupirani stupčasti grafikon** s vizualizacije, a zatim povucite polje **Grad** u **Zajednički osi**, povucite **brzinu**, **tirepressure i engineoil polja** u područje **Vrijednosti stupaca** , promijenite njihove zbrajanja vrstu **Average**. 

Povucite polje **engineTemperature** u područje **Vrijednosti retka** , promijenite vrstu zbrajanja u **prosjek**. 

![Povezani automobilima - vizualizacija polja](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Promijenite **Naslov** grafikona **"Prosječna brzina, guma tlaka, engine ULJE**i temperatura modul".  

![Povezani automobilima - vizualizacija polja](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Kliknite prazno područje da biste dodali novu vizualizaciju.

Odaberite vizualizaciju **karte** vizualizacije, povucite polje **modela** u područje **grupe** i povucite polje **MaintenanceProbability** u područje **vrijednosti** .

Promijenite **Naslov** grafikona s **"Koje je obavezna održavanja vozila modelima"**.

![Povezani automobilima – Promjena naslova grafikona](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Kliknite prazno područje da biste dodali novu vizualizaciju.

Odaberite **100% složeni trakasti grafikon** s vizualizacije, povucite polje **Grad** u područje **os** i povucite **MaintenanceProbability** **RecallProbability** polja u područje **vrijednosti** .

![Povezani automobilima - dodavanje nove vizualizacije](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Kliknite **oblik**, odabir **Boja podataka**i postavljanje boje **MaintenanceProbability** vrijednost **"F2C80F"**.

Promijenite **Naslov** grafikona **"vjerojatnost od vozila održavanja & opozvati tako da Grad"**.

![Povezani automobilima - dodavanje nove vizualizacije](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Kliknite prazno područje da biste dodali novu vizualizaciju.

Odaberite **Površinski grafikon** iz vizualizacije iz vizualizacije, povucite polje **modela** u područje **os** pa povucite **engineOil, tirepressure, brzinu i MaintenanceProbability** polja u područje **vrijednosti** . Promijenite njihovu vrstu zbrajanja u **"Prosječna"**. 

![Povezani automobilima – Promjena vrste zbrajanja](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Promjena naslova grafikona za **"Prosječna ULJE modul, guma tlaka, brzinu i održavanje vjerojatnost model"**.

![Povezani automobilima – Promjena naslova grafikona](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Kliknite prazno područje da biste dodali novu vizualizaciju:

1. Odaberite vizualizaciju **Raspršeni grafikon** s vizualizacije.
2. Povucite polje **modela** u područje **pojedinosti** i **legendu** .
3. Povucite polje **goriva** u područje **os x** , promijenite skupljanja u **prosjek**.
4. Povući **engineTemparature** u **područje os y**, promijenite skupljanja u **prosjek**
5. Povucite polje **vin** u područje **Veličina** .


![Povezani automobilima - dodavanje nove vizualizacije](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Promijenite **Naslov** grafikona u **"Prosjeci goriva, Engine Temperature model"**.

![Povezani automobilima – Promjena naslova grafikona](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Konačni izvještaj će izgledati kao što je prikazano u nastavku.

![Povezani automobilima konačne izvješća](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Vizualizacija PIN-a iz izvješća u stvarnom vremenu nadzorne ploče
  
Stvaranje prazne nadzorne ploče i to klikom na ikonu plus pokraj stavke nadzorne ploče. Možete nazvati "Vozila analize nadzorna ploča za Telemetriju"

![Povezani automobilima-nadzorne ploče](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Prikvačite vizualizacije iz iznad izvješća na nadzornu ploču. 
 
![Povezani automobilima-nadzorne ploče](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Na nadzornoj ploči na sljedeći način trebao bi izgledati kada se stvaraju tri izvješća, a odgovarajuće vizualizacije su prikvačiti na nadzornoj ploči. Ako niste stvorili sva izvješća, nadzorne ploče ne može izgledati drugačije. 

![Povezani automobilima-nadzorne ploče](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Čestitamo! Uspješno ste stvorili u stvarnom vremenu nadzorne ploče. U nastavku za izvršavanje CarEventGenerator.exe i RealtimeDashboardApp.exe, trebali biste vidjeti uživo ažuriranja na nadzornoj ploči. Traje oko 10 do 15 minuta da biste dovršili sljedeće korake.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>Postavljanje nadzorne ploče za obradu grupe za Power BI

>[AZURE.NOTE] Traje oko dvije sati (iz uspješan dovršetak implementacijskih) za kanal end da biste završetka obrade da biste završili izvođenja i obrada godine vrijedi generirani podataka. Pa pričekajte za obradu da biste dovršili prije nego što nastavite sa sljedećim koracima. 

**Preuzimanje datoteke dizajnera PowerBI**

-   Unaprijed konfigurirana datoteka dizajnera PowerBI je dio implementacije
-   Kliknite čvor PowerBI u prikazu dijagrama, a zatim kliknite vezu za **Preuzimanje datoteke dizajnera PowerBI** u oknu svojstva![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Lokalno spremanje

**Konfiguriranje PowerBI izvješća**

-   Otvorite designer datoteku 'VehicleTelemetryAnalytics - Report.pbix radnu površinu' putem PowerBI radne površine. Ako već nemate, instalirajte [radnu površinu PowerBI instalacija](http://www.microsoft.com/download/details.aspx?id=45331)na radnu površinu PowerBI. 

-   Kliknite **Uređivanje upita**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Dvokliknite **izvora**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Ažuriranje poslužitelja niz za povezivanje s poslužiteljem Azure SQL koji imate dodjeli kao dio uvođenje. Pritisnite čvor Azure SQL na dijagramu i pregledajte naziv poslužitelja okna sa svojstvima.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- Ostavite **bazu podataka** kao *connectedcar*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Kliknite **u redu**.
- Prikazat će se **Windows vjerodajnica** kartica odabrao zadane, promijenite u **bazu podataka vjerodajnica** tako da kliknete na karticu **baze podataka** na desnoj.
- Navedite **korisničko ime** i **lozinku** Azure SQL baze podataka koja je određena tijekom postavljanja njegove implementacije.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Kliknite **Povezivanje**
- Ponovite gore navedene korake za svaku od tri preostale upiti prezentacija u desnom oknu, a ažuriranje pojedinosti veze za izvor podataka.
- Kliknite **Zatvori, a zatim učitati**. Datoteka skupova podataka za Power BI Desktop niste povezani s bazom podataka SQL Azure tablice.
- **Zatvori** Power BI Desktop datoteka.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Kliknite gumb **Spremi** da biste spremili promjene. 
 
Sada ste konfigurirali sva izvješća koja odgovara obradu Obrada put unutar rješenja. 


## <a name="upload-to-powerbicom"></a>Prijenos *powerbi.com*
 
1.  Dođite do web-portal PowerBI na web-mjesto http://powerbi.com i u okvir za prijavu.
2.  Kliknite **dohvaćanje podataka**  
3.  Prijenos datoteke za Power BI Desktop.  
4.  Da biste prenijeli, kliknite **dohvaćanje podataka -> dohvaćanje datoteka -> Lokalna datoteka**  
5.  Dođite do **"VehicleTelemetryAnalytics – radne površine Report.pbix"**  
6.  Nakon prijenosa datoteka bit ćete preusmjereni natrag u radni prostor Power BI.  

Skup podataka, izvješća i nadzorne ploče komponente prazne stvorit će se za vas.  
 

Prikvačite grafikoni da biste postojeću nadzornu ploču **Analize Telemetrijskih nadzorna ploča vozila** u **Dodatku Power BI**. Kliknite prazan nadzorne ploče stvorena iznad, a zatim pronađite odjeljak kliknite **izvješća** prenesenu izvješća.  

![PowerBI.com Telemetrijskih vozila](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Imajte na umu izvješće sadrži šest stranice:**  
Stranica 1: Gustoće vozila  
2 stranice: Stanje u stvarnom vremenu vozila  
Stranica 3: Omogućuju agresivno utemeljenima na vozila   
Stranica 4: Opozvati vozila  
Stranica 5: Goriva učinkovito kontroliranje vozila  
6 stranice: Logotip tvrtke Contoso  

![Povezani automobilima PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Od 3 stranice**, PIN-a na sljedeći način:  

1.  Broj VIN  
    ![Povezani automobilima PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Omogućuju agresivno utemeljenima vozila model – grafikona u obliku Vodopada  
    ![Vozila Telemetrijskih - grafikoni PIN-a 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Od 5 stranice**, PIN-a na sljedeći način: 
 
1.  Broj vin    
    ![Vozila Telemetrijskih - Pin grafikoni 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Potiču učinkovitog vozila model: Grupirani stupčasti grafikon  
    ![Vozila Telemetrijskih - Pin grafikoni 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Iz stranica 4**, PIN-a na sljedeći način:  

1.  Broj vin  
    ![Vozila Telemetrijskih - Pin grafikoni 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Model opozvane vozila po gradu: karte u obliku stabla  
    ![Vozila Telemetrijskih - Pin grafikoni 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Od 6 stranice**, PIN-a na sljedeći način:  

1.  Logotip tvrtke Contoso Motors  
    ![Vozila Telemetrijskih - Pin grafikoni 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Organiziranje nadzorne ploče**  

1.  Nadzorne ploče
2.  Prijeđite pokazivačem preko svake grafikon i Preimenuj koji se temelji na imenovanja navedenih u Dovršeno nadzornoj ploči slici u nastavku. I kretanje grafikone izgledaju kao ispod nadzorne ploče.  
    ![Telemetrijskih vozila - organiziranje nadzorne ploče 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![PowerBI.com Telemetrijskih vozila](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Ako ste stvorili sva izvješća, kao što je navedeno u ovom dokumentu, konačni nadzorne ploče dovršene trebao izgledati kao na sljedećoj slici. 

![Telemetrijskih vozila - organiziranje nadzorne ploče 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Čestitamo! Uspješno ste stvorili izvješća i nadzorne ploče da biste dobiti u stvarnom vremenu, predvidljivu i skupnim uvida na vozila stanja i vožnju navike.  
