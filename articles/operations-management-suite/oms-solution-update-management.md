<properties
    pageTitle="Ažuriranje rješenje za upravljanje OMS | Microsoft Azure"
    description="Ovaj je članak namijenjen je pomoću kojih se objašnjava kako koristiti rješenja za upravljanje ažuriranjima za računala za Windows i Linux."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Ažuriranje rješenje za upravljanje OMS](./media/oms-solution-update-management/update-management-solution-icon.png) Ažuriranje rješenje za upravljanje OMS

Upravljanje ažuriranje rješenja u OMS omogućuje upravljanje ažuriranjima za računala za Windows i Linux.  Možete brzo procijenite status dostupna ažuriranja na svim računalima agent i započeli postupak instalacije ažuriranja koja su potrebna za poslužitelje. 

## <a name="prerequisites"></a>Preduvjeti

-   Windows agenata ili mora biti konfiguriran za komunikaciju s poslužiteljem Windows Server Update Services (WSUS) ili imaju pristup Microsoft Update.  

    >[AZURE.NOTE] Agent za Windows nije moguće upravljati istovremeno po Upravitelj konfiguracije centar sustava.  
  
-   Linux agenata moraju imati pristup za ažuriranje spremište.  Agent za OMS za Linux mogu se preuzeti sa [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Konfiguracija

Izvršite sljedeće korake da biste dodali rješenja za ažuriranje upravljanja OMS radnog prostora i dodavanje agenata Linux.  Windows agenata automatski se dodaju bez dodatna konfiguracija.

1.  Dodavanje rješenja za ažuriranje upravljanja OMS radnog prostora koristeći postupak opisan u [Dodavanje OMS rješenja](../log-analytics/log-analytics-add-solutions.md) iz galerije rješenja.  
2.  Na portalu OMS odaberite **Postavke** , a zatim **Povezani izvora**.  Imajte na umu **ID radnog prostora** i u **primarni ključ** ili **sekundarni ključ**.
3.  Izvršite sljedeće korake za svako računalo Linux.

    na.  Instalirajte najnoviju verziju OMS Agent za Linux ponovnim pokretanjem sljedeće naredbe.  Zamjena <Workspace ID> prostor ID-ja i <Key> primarni ili sekundarni ključa.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Da biste uklonili agenta, pokrenite sljedeću naredbu.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Upravljanje paketi

Ako vašoj grupi za upravljanje sustava centar za komponentu Operations Manager povezan s OMS radnog prostora, zatim sljedećim paketima za upravljanje instalirat će se Operations Manager prilikom dodavanja rješenja. Postoji konfiguracije ni održavanja te upravljanje paketa potrebna. 

-   Centar za sustav Microsoft Savjetnik za ažuriranje rizika obavještavanje Service Pack (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Ažuriranje implementaciju MP

Dodatne informacije o kako je rješenje upravljanja paketi ažuriraju, potražite u članku [Povezivanje prijava analitiku u komponente Operations Manager](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Prikupljanje podataka

### <a name="supported-agents"></a>Podržani agenata

U sljedećoj tablici opisane povezanih izvora koje podržava rješenja.

Povezani izvora | Podržana | Opis|
----------|----------|----------|
Windows agenata | Da | Rješenje prikuplja informacije o ažuriranjima sustava iz Windows agenata i pokreće instalaciju ažuriranja koja su potrebna.|
Linux agenata | Da | Rješenje prikuplja informacije o ažuriranjima sustava iz agenata Linux.|
Operacije Upravitelj upravljanje grupe | Da | Rješenje prikuplja informacije o ažuriranjima sustava iz agenata u grupi Upravljanje povezani.<br>Izravni veza agent za komponentu Operations Manager prijava analitiku nije potrebna. Podaci prosljeđuju se iz grupe za Upravljanje spremištem OMS.|
Račun za Azure prostora za pohranu | ne | Azure prostora za pohranu obuhvatiti informacije o ažuriranjima sustava.|  

### <a name="collection-frequency"></a>Učestalost zbirke

Za svaki upravljani računalo sa sustavom Windows, pregled provodi dvaput dnevno.  Kada instalirate ažuriranje, njegov podaci ažurirat će se unutar 15 minuta.  

Za svako računalo upravljanih Linux pregleda provodi svaki 3 sata.  

## <a name="using-the-solution"></a>Korištenje rješenja

Kada dodate rješenje za upravljanje ažuriranje OMS radnog prostora, pločicu **Upravljanje ažuriranje** će se dodati OMS nadzorne ploče. Pločica prikazuje count i grafički prikaz broj računala u svom okruženju trenutno koje je obavezna ažuriranja sustava.<br><br>
![Ažuriranje sažetka pločica upravljanja](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Prikaz ažuriranja konfiguracije

Kliknite pločicu **Ažuriranje upravljanje** da biste otvorili na nadzornoj ploči **Upravljanja ažuriranja** . Na nadzornoj ploči obuhvaća stupaca u tablici u nastavku. Svaki stupac prikazuje najviše deset stavki koje zadovoljavaju kriterij u tom stupcu za navedeni opseg i vremenskog razdoblja. Možete pokrenuti pretraživanje zapisnika koja vraća sve zapise tako da kliknete **vidjeli sve** pri dnu stupac ili tako da kliknete zaglavlje stupca.

Stupac | Opis|
----------|----------|
**Računala ažuriranja koja nedostaju** ||
Od ključne važnosti ili sigurnosna ažuriranja | Popisi najvećih deset računala koje nedostaju ažurira sortirane prema broju ažuriranja postanu nedostaju. Kliknite naziv računala da biste pokrenuli pretraživanje zapisnika vraćanje svih ažuriranja zapisa na tom računalu.|
Od ključne važnosti ili sigurnosna ažuriranja starije od 30 dana| Određuje broj računala koja su nema ključnih ili sigurnosna ažuriranja grupirane trajanja jer je objavljen ažuriranja. Kliknite jednu od stavki da biste pokrenuli zapisnika pretraživanja vraća sva nedostaju i kritičnih ažuriranja.|
**Obavezno nedostaje ažuriranja**||
Od ključne važnosti ili sigurnosna ažuriranja | Popis klasifikacije ažuriranja da računala nedostaju sortirane prema broju računala nedostaje ažuriranja u kategoriji. Pritisnite klasifikaciju da biste pokrenuli pretraživanje zapisnika vraćanje svih ažuriranja zapisa za taj klasifikacija.|
**Ažuriranje implementacije**||
Ažuriranje implementacije | Broj implementacije trenutno zakazani ažuriranja i trajanje do sljedećeg zakazanog pokretanja.  Kliknite pločicu za prikaz rasporedima, u tijeku i dovršene ažuriranja ili da biste zakazali novi implementacije.|  
<br>  
![Ažuriranje sažetka upravljačku nadzornu ploču](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Ažuriranje računala prikaza za upravljanje nadzorne ploče](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Ažuriranje prikaza paket za upravljanje nadzorne ploče](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Instaliranje ažuriranja

Nakon ažuriranja ste je kažnjeni svih s računala sa sustavom Windows u svom okruženju, možete imati potrebna instalirana stvaranjem *Implementaciju*ažuriranja.  Implementacije sustava ažuriranje je zakazanu instalaciju ažuriranja koja su potrebna za jednu ili više računala sa sustavom Windows.  Navedite datum i vrijeme za implementaciju uz računalo ili grupe računala koje treba uključiti.  

Ažuriranja se instaliraju tako da runbooks u automatizaciji Azure.  Trenutno ne možete vidjeti te runbooks, a ne trebaju sve konfiguracije.  Pri stvaranju implementacije sustava ažuriranje stvara raspored u tom se pokreće osnovne ažuriranje runbook u određeno vrijeme za uključene računala.  U ovom osnovne runbook započinje podređeni runbook na svaki agent za Windows koja izvršava instalacije ažuriranja koja su potrebna.  

### <a name="viewing-update-deployments"></a>Prikaz ažuriranja implementacije

Kliknite pločicu **Implementaciju ažuriranja** da biste vidjeli popis postojeće implementacijama ažuriranja.  Grupirani su po statusu – **zakazano**, **pokrenuti**i **Dovršeno**.<br><br> ![Ažuriranje implementacijama rasporeda stranice](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

U sljedećoj tablici opisane su prikazana svaki implementaciju ažuriranja svojstva.

Svojstvo | Opis|
----------|----------|
Ime | Naziv implementaciju ažuriranja.|
Raspored | Vrste rasporeda.  *OneTime* trenutno samo moguću vrijednost.|
Vrijeme početka|Datum i vrijeme implementaciju ažuriranja zakazano.|
Trajanje | Broj minuta implementaciju ažuriranja dopuštena za pokretanje.  Ako se sva ažuriranja ne instaliraju unutar trajanje, zatim preostala ažuriranja mora čekati sljedeće ažuriranje implementacije.|
Poslužitelja | Broj računala utječe implementaciju ažuriranja.|
Status | Trenutni status implementaciju ažuriranja.<br><br>Moguće vrijednosti su:<br>-Nije pokrenuto<br>– Pokretanje<br>– Dovršeno|  

Kliknite implementacije sustava ažuriranje da biste vidjeli njegove pojedinosti zaslona koji uključuje stupaca u tablici u nastavku.  Ti stupci će biti ispunjena ako implementaciju ažuriranja još nije započelo.<br>

Stupac | Opis|
----------|----------|
**Rezultati računala**||
Uspješno dovršena | Prikazuje broj računala implementaciju ažuriranja po statusu.  Kliknite status da biste pokrenuli pretraživanje zapisnika vraćanje svih ažuriranja zapisa čiji je taj status za implementaciju ažuriranje.|
Stanje instalacije računala| Popis uvrštene u implementaciju ažuriranja i postotak ažuriranja koja je uspješno instaliran računala. Kliknite jednu od stavki da biste pokrenuli zapisnika pretraživanja vraća sva nedostaju i kritičnih ažuriranja.|
**Obnavlja rezultate instanci**||
Stanje instance instalacije | Popis klasifikacije ažuriranja da računala nedostaju sortirane prema broju računala nedostaje ažuriranja u kategoriji. Kliknite računalo da biste pokrenuli pretraživanje zapisnika vraćanje svih ažuriranja zapisa na tom računalu.|  
<br><br> ![Pregled rezultata za implementaciju ažuriranja](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Stvaranje implementacije sustava ažuriranja

Stvori novu implementaciju ažuriranje tako da kliknete gumb **Dodaj** pri vrhu zaslona da biste otvorili stranicu **Novi implementaciju ažuriranja** .  Navedite vrijednosti za svojstva u tablici u nastavku.

Svojstvo | Opis|
----------|----------|
Ime | Jedinstveni naziv za prepoznavanje implementaciju ažuriranja.|
Vremenska zona | Vremenska zona za vrijeme početka.|
Vrijeme početka | Datum i vrijeme da biste pokrenuli implementaciju ažuriranja.|
Trajanje | Broj minuta implementaciju ažuriranja dopuštena za pokretanje.  Ako se sva ažuriranja ne instaliraju unutar trajanje, zatim preostala ažuriranja mora čekati sljedeće ažuriranje implementacije.|
Računala | Nazivi računala ili grupe računala da biste obuhvatili implementaciju ažuriranja.  Odaberite jednu ili više stavki s padajućeg popisa.|
<br><br> ![Nova stranica za implementaciju ažuriranja](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Vremenskog raspona

Po zadanom je djelokrug analizirati u rješenje za upravljanje za ažuriranje podataka iz svih povezanih upravljanje grupa koje generira unutar posljednji 1. 

Da biste promijenili vremenski raspon podataka, odaberite **podataka na temelju** pri vrhu nadzorne ploče. Možete odabrati zapise Stvori ili ažurira pada u zadnjih sedam dana, jedan dan ili 6 sati. Ili možete odabrati **Prilagođeni** i navedite u prilagođenom rasponu datuma.<br><br> ![Mogućnost Prilagođeno vremenskog raspona](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Prijava analitiku zapisa

Upravljanje ažuriranje rješenja stvara dvije vrste zapisa u spremištu OMS.

### <a name="update-records"></a>Ažuriranje zapisa

Zapis s vrstom **Ažuriranje** se stvara za svako ažuriranje koje je instaliran ili potrebno na svakom računalu. Ažuriranje zapisa imaju svojstva u tablici u nastavku.

Svojstvo | Opis|
----------|----------|
Vrsta | *Ažuriranje*|
SourceSystem | Željeni izvor odobrene instalacije ažuriranja.<br>Moguće vrijednosti su:<br>-Microsoft Update<br>– Windows Update<br>-SCCM<br>-Poslužitelji Linux (dohvaćanja iz paketa upravitelji)|
Odobrene | Određuje je li ažuriranje odobren za instalaciju.<br> Za poslužitelje Linux Ovo nije obavezno trenutno kao zakrpa ne upravlja OMS.|
Klasifikacija za Windows | Klasifikacija ažuriranja.<br>Moguće vrijednosti su:<br>-Aplikacije<br>-Kritičnih ažuriranja<br>– Ažuriranja definicija<br>-Pakete<br>– Sigurnosna ažuriranja<br>-Servisni paketi<br>-Kumulativna ažuriranja<br>-Ažuriranja|
Klasifikacija za Linux | Cassification ažuriranja.<br>Moguće vrijednosti su:<br>-Kritičnih ažuriranja<br>– Sigurnosna ažuriranja<br>-Ostala ažuriranja|
Računalo | Naziv računala.|
InstallTimeAvailable | Određuje je li vrijeme instalacije dostupna iz drugih agenata koji instaliranja isti ažuriranja.|
InstallTimePredictionSeconds | Procijenjena instalacije vrijeme u sekundama koji se temelji na drugim agenata koji instaliranja isti ažuriranja.|
KBID | ID članak iz baze znanja koji opisuje ažuriranje.|
ManagementGroupName | Naziv grupe za upravljanje SCOM agenata.  Za ostale agenata to je AOI -<workspace ID>.|
MSRCBulletinID | ID Microsoft bilten s opisom ažuriranja.|
MSRCSeverity | Težinu bilten Microsoft.<br>Moguće vrijednosti su:<br>-Od ključne važnosti<br>-Važno<br>-Moderiranje|
Neobavezno | Određuje je li neobavezna ažuriranja.|
Proizvoda | Naziv proizvoda namijenjen ažuriranja.  Kliknite **Prikaz** da biste otvorili u članku u pregledniku.|
PackageSeverity | Težinu slabe fiksiran to ažuriranje, kao što je dojavi distro dobavljačima Linux. | 
PublishDate | Datum i vrijeme ažuriranje nije instalirano.|
RebootBehavior | Određuje ako ažuriranje navodi ponovno pokretanje.<br>Moguće vrijednosti su:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | Broj revizija ažuriranja.|
SourceComputerId | GUID za identificirati samo na računalu.|
TimeGenerated | Datum i vrijeme zadnjeg ažuriranja zapisa.|
Naslov | Naslov ažuriranja.|
UpdateID | GUID za jedinstvenu identifikaciju ažuriranja.|
UpdateState | Određuje je li instaliran ažuriranje na ovom računalu.<br>Moguće vrijednosti su:<br>-Instaliran - ažuriranje nije instalirano na ovom računalu.<br>-Potrebno – ažuriranje nije instalirano i potreban je na ovom računalu.|  

<br>
Kada dovršite sve zapisnika pretraživanja koji vraća zapise s vrstom **Ažuriranje** možete odabrati prikaz **ažuriranja** koja se prikazuje skup pločice sa sažetkom ažuriranja rezultata pretraživanja. Možete kliknuti na stavke u **nedostaje i ažuriranjima primijenjena** i **obaveznih i neobaveznih ažuriranja** pločica na opseg prikaza za taj skup ažuriranja. Odaberite prikaz **popisu** ili u **tablici** da biste se vratili u pojedinačnim zapisima.<br> 

![Prikaz ažuriranja zapisnika pretraživanja s ažuriranjem vrsta zapisa](./media/oms-solution-update-management/update-la-view-updates.png)  

U prikazu **tablice** klikom na u **KBID** za bilo koji zapis da biste otvorili u pregledniku s članak iz baze znanja. Omogućuje brzo pročitajte detalje o određenom ažuriranja.<br> 

![Prikaz zapisnika pretraživanja tablice ažuriranjima pločice vrsta zapisa](./media/oms-solution-update-management/update-la-view-table.png)

U prikazu **popisa** kliknite **Prikaži** vezu pokraj KBID da biste otvorili članak iz baze znanja.<br>

![Prikaz popisa zapisnika pretraživanja ažuriranjima pločice vrsta zapisa](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary zapisa

Zapis s vrstom **UpdateSummary** se stvara za svako računalo agent za Windows. Taj zapis ažurira se svaki put je skenirana računalo ima li ažuriranja. **UpdateSummary** zapisa imaju svojstava u tablici u nastavku.

Svojstvo | Opis|
----------|----------|
Vrsta | UpdateSummary|
SourceSystem | OpsManager |
Računalo | Naziv računala.|
CriticalUpdatesMissing | Broj kritičnih ažuriranja nedostaje na računalu.|
ManagementGroupName | Naziv grupe za upravljanje SCOM agenata. Za ostale agenata to je AOI -<workspace ID>.|
NETRuntimeVersion | Verzija .NET runtime instalirani na računalu.|
OldestMissingSecurityUpdateBucket | Objavljen grupe da biste kategorizirali vrijeme od najstarijeg nedostaju sigurnost ažuriranje na ovom računalu.<br>Moguće vrijednosti su:<br>-Stariji<br>-180 dana<br>-150 dana<br>-120 dana<br>-90 dana<br>-60 dana<br>– Otvorite 30 dana<br>-Nedavne|
OldestMissingSecurityUpdateInDays | Broj dana od najstarijeg nedostaju sigurnost ažuriranje na ovom računalu je objavljen.|
OsVersion | Verzija operacijskog sustava koja je instaliran na računalu.|
OtherUpdatesMissing | Broj ostala ažuriranja nedostaje na računalu.|
SecurityUpdatesMissing | Broj sigurnosna ažuriranja nedostaje na računalu.|
SourceComputerId | GUID za identificirati samo na računalu.|
TimeGenerated | Datum i vrijeme zadnjeg ažuriranja zapisa.|
TotalUpdatesMissing |Ukupan broj ažuriranja nedostaje na računalu.|
WindowsUpdateAgentVersion | Broj verzije agent za Windows Update na računalu.|
WindowsUpdateSetting | Postavka za način na koji će računalo instalirati važna ažuriranja.<br>Moguće vrijednosti su:<br>-Onemogućena<br>– Obavijesti prije instalacije<br>-Zakazanu instalaciju|
WSUSServer | URL WSUS poslužitelju ako računalo konfigurirano tako da biste koristili neki.|  

## <a name="sample-log-searches"></a>Ogledna zapisnika pretraživanja

Sljedeća tablica sadrži ogledne zapisnika pretraživanja za ažuriranje zapisa koji se prikupljaju rješenja. 

Upit | Opis|
----------|----------|
Svim računalima s ažuriranja koja nedostaju | Vrsta = ažuriranje UpdateState = potrebno neobavezno = false & #124; Odaberite računalo "," Naslov "," KBID "," klasifikacija "," UpdateSeverity "," PublishedDate|
Nedostaju ažuriranja za računalo "COMPUTER01.contoso.com" (replace uz naziv računala) | Vrsta = ažuriranje UpdateState = potrebno neobavezno = false računala = "COMPUTER01.contoso.com" & #124; Odaberite računalo "," Naslov "," KBID "," proizvoda "," UpdateSeverity "," PublishedDate|
Sva računala bez ključnih i sigurnosna ažuriranja | Vrsta = ažuriranje UpdateState = potrebno neobavezno = false (klasifikacija = "Sigurnosna ažuriranja" ili klasifikacija = "Kritičnih ažuriranja")|
Od ključne važnosti ili sigurnosne potrebne strojeva koji su ažuriranja ručno primijenili ažuriranja | Vrsta = UpdateState ažuriranje = potrebno neobavezno = false (klasifikacija = "Sigurnosna ažuriranja" ili klasifikacija = "Kritičnih ažuriranja") u računalo {vrsta = UpdateSummary WindowsUpdateSetting = ručno & #124; Različita računala} & #124; DISTINCT KBID|
Događaji pogrešaka za strojevima koji imaju nedostaje ključnih ili sigurnosne potrebna ažuriranja | Vrsta = događaj EventLevelName = pogreške u računalo {vrsta = ažuriranja (klasifikacija = "Sigurnosna ažuriranja" ili klasifikacija = "Kritičnih ažuriranja") UpdateState = potrebno neobavezno = false & #124; Različita računala}|
Svim računalima s nedostaje skupna ažuriranja | Vrsta = neobavezna ažuriranja = false klasifikacija = UpdateState "Skupna ažuriranja" = potrebno & #124; Odaberite računalo "," Naslov "," KBID "," klasifikacija "," UpdateSeverity "," PublishedDate|
DISTINCT ažuriranja koja nedostaju na svim računalima | Vrsta = ažuriranje UpdateState = potrebno neobavezno = false & #124; DISTINCT naslov|
Članstva WSUS računala | Vrsta = UpdateSummary & #124; Izmjerite count() po WSUSServer|
Konfiguriranje automatskog ažuriranja | Vrsta = UpdateSummary & #124; Izmjerite count() po WindowsUpdateSetting|
Računalima s automatsko ažuriranje onemogućena | Vrsta = UpdateSummary WindowsUpdateSetting = ručno|  
Popis strojeva Linux koji imaju ažuriranje paketa dostupna | Vrsta = ažuriranja i OSType = Linux i UpdateState! = "Nije potrebna" & #124; Izmjerite count() na računalu|
Popis strojeva Linux koji imaju ažuriranje paketa dostupna koje rješava od ključne važnosti ili sigurnosne slabe | Vrsta = ažuriranja i OSType = Linux i UpdateState! = "Nije potrebna" i (klasifikacija = "Kritičnih ažuriranja" ili klasifikacija = "Sigurnosna ažuriranja") & #124; Izmjerite count() na računalu|
Popis svih paketa koji imaju dostupno ažuriranje | Vrsta = ažuriranja i OSType = Linux i UpdateState! = "Nije potrebna"|
Popis svih paketa koji imaju dostupno ažuriranje koji adrese od ključne važnosti ili sigurnosne slabe | Vrsta = ažuriranja i OSType = Linux i UpdateState! = "Nije potrebna" i (klasifikacija = "Kritičnih ažuriranja" ili klasifikacija = "Sigurnosna ažuriranja")|
Popis svih strojeva "Ubuntu" s sva dostupna ažuriranja | Vrsta = ažuriranja i OSType = Linux i OSName = Ubuntu & #124; Izmjerite count() na računalu|

## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali detaljne ažuriranja podataka pomoću zapisnika pretraživanja u [Zapisnik analize](../log-analytics/log-analytics-log-searches.md) .

- [Stvorite vlastitu nadzornu ploču](../log-analytics/log-analytics-dashboards.md) s prikazom ažuriranje sukladnost za računala upravljanih.

- [Stvaranje upozorenja](../log-analytics/log-analytics-alerts.md) kada otkrije ključna ažuriranja koja nedostaje s računala ili računalu je onemogućen je automatska ažuriranja.  




