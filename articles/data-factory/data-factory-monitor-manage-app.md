<properties 
    pageTitle="Nadzor i upravljanje kanali tvorničke Azure podataka" 
    description="Saznajte kako koristiti nadzor i upravljanje aplikacija za nadzor i upravljanje factories Azure podataka i kanali." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Nadzor i upravljanje kanali tvorničke Azure podataka pomoću novih nadzor i upravljanje aplikacije
> [AZURE.SELECTOR]
- [Pomoću komponente PowerShell za Azure Portal/Azure](data-factory-monitor-manage-pipelines.md)
- [Korištenje nadzor i upravljanje aplikacije](data-factory-monitor-manage-app.md)

U ovom se članku opisuje kako nadzor, upravljanje i vaše kanali za ispravljanje pogrešaka i stvaranje upozorenja za primanje obavijesti o neuspjeha pomoću **nadzor i upravljanje aplikacija**. Možete pogledati i sljedeći videozapis da biste saznali o korištenju nadzor i upravljanje aplikacija.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Pokretanje za nadzor i upravljanje aplikacija na
Da biste pokrenuli nadzor i upravljanje aplikaciju, kliknite **nadzor i upravljanje** pločica plohu **TVORNIČKE podataka** za vaše podatke tvorničke.

![Nadzor pločica na tvorničke podataka početnoj stranici](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Trebali biste vidjeti nadzor i upravljanje App pokrene u zasebnom kartica/prozoru.  

![Nadzor i upravljanje aplikacije](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Ako vam se prikazuje pri "Dopuštanja..." je se zamrzne u web-preglednik, onemogućivanje/poništite postavka **blokiranje kolačića drugih proizvođača i podataka za web-mjesta** (ili) Zadrži omogućena i stvoriti iznimku **login.microsoftonline.com** i pokušajte ponovno pokretanje aplikacije.


Ako ne vidite aktivnosti windows na popisu pri dnu, kliknite gumb **Osvježi** na alatnoj traci da biste osvježili popis. Osim toga, postavite desno vrijednosti filtara **vrijeme početka** i **vrijeme završetka** .  


## <a name="understanding-the-monitoring-and-management-app"></a>Objašnjenje za nadzor i upravljanje aplikacije
Postoje tri kartice (**Explorer resursa**, **Nadzor prikaza**i **upozorenja**) na lijevoj strani, a po zadanom je potvrđen na prvu karticu (resursa Explorer). 

### <a name="resource-explorer"></a>Explorer resursa
Potražite u sljedećim člancima: 

- Resurs za Explorer **Prikaz stabla** u lijevom oknu.
- **Prikaz dijagrama** na vrhu.
- Popis **Aktivnosti Windows** pri dnu u srednjem oknu.
- **Svojstva**/**Explorer aktivnosti prozora** kartica u desnom oknu. 

U programu Explorer resursa vidite sve resursa (kanali, skupova podataka, povezani servisi) u tvorničke podataka u prikazu stabla. Kad odaberete objekta u programu Explorer resursa, možete primijetiti sljedeće: 

- Pridruženi entitet podataka tvorničke istaknuta je u prikazu dijagrama.
- povezane aktivnosti windows (kliknite [ovdje](data-factory-scheduling-and-execution.md) da biste saznali o aktivnosti windows) istaknute su na popisu aktivnosti Windows pri dnu.  
- svojstva odabranog objekta u prozoru svojstva u desnom oknu. 
- Definicija JSON odabranog objekta ako je primjenjivo. Na primjer: povezane servisa ili skup podataka ili na kanal. 

![Explorer resursa](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Potražite u članku [Zakazivanje sastanka te izvođenja](data-factory-scheduling-and-execution.md) članak konceptualnih informacija o prozor aktivnost. 

### <a name="diagram-view"></a>Prikaz dijagrama
Dijagram prikaza podataka tvorničke sadrži jedan oknu staklenom za nadzor i upravljanje tvorničke podataka i njegov resursi. Kad odaberete entitet tvorničke podataka (skupa podataka na kanal) u prikazu dijagrama, možete primijetiti sljedeće:
 
- tvorničke entitet podataka odabran u prikazu stabla
- windows pridružene aktivnosti istaknute su na popisu Windows aktivnosti.
- svojstva odabranog objekta u prozoru svojstva

Kada kanal je omogućen (ne u pauziran), prikazuje se zelenom crtom. 

![Kanal koji se izvodi](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Primijetite da postoje tri naredbeni gumbi za kanal u prikazu dijagrama. Drugi gumbom za zadržite pokazivač na kanal. Pauziranje ne prekid trenutno izvode aktivnosti i obavijestite tu osobu prijeđite do kraja. Treći gumb zaustavlja kanal, a završava postojeće izvršavanje aktivnosti. Prvi gumb nastavlja kanal. Kada je pauziran na kanal, primijetit ćete promjena boje za kanal pločica na sljedeći način.

![Zaustavi/nastavi na pločici](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

Odabir više stavki možete dva ili više kanali (pomoću CTRL) i pomoću naredbeni gumbi traka Zaustavi/nastavi više kanali odjednom.

![Zaustavi/nastavi naredbenoj traci](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

Sve aktivnosti u kanalu, možete vidjeti klikom desnom tipkom miša kliknete pločicu kanal, a zatim kliknite **Otvori kanal**.

![Otvaranje izbornika za kanal](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

U prikazu otvorena kanal vidjeti sve aktivnosti u kanalu. U ovom primjeru je samo jedan aktivnosti: kopiranje aktivnosti. Da biste se vratili na prethodni prikaz, kliknite naziv tvorničke podataka u izbornik za hijerarhijsku navigaciju pri vrhu.

![Otvorena kanal](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

U prikazu za kanal kada kliknete na izlazni skup podataka ili kada pomaknite miš preko dataset izlaz vidite aktivnosti Windows skočni prozor za taj skup podataka.

![Skočni prozor Windows aktivnosti](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Možete kliknuti prozor programa aktivnosti da biste vidjeli detalje za njega u prozoru **Svojstva** u oknu na desnoj strani. 

![Aktivnosti svojstava prozora](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

U desnom oknu prijelaz **Aktivnosti prozor Explorer** karticu da biste vidjeli dodatne detalje.

![Aktivnosti prozor Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

Prikazat će se i **riješiti varijable** za svaki Pokreni aktivnosti pokušaj u odjeljku **prilikom pokušaja** . 

![Riješi varijable](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Prijelaz na karticu **skriptu** da biste vidjeli definiciju JSON skripte za odabrani objekt.   

![Kartica skripte](./media/data-factory-monitor-manage-app/ScriptTab.png)

Možete vidjeti aktivnost na tri mjesta:

- Aktivnosti Windows skočni prozor u prikazu dijagrama (srednjem oknu).
- Aktivnosti prozor programa Explorer u desnom oknu.
- Popis Windows aktivnosti u dnu okna.

U sustavu Windows aktivnosti skočni i Explorer prozor aktivnosti, možete se pomicati prethodni tjedan i sljedeći tjedan pomoću strelice lijevo i desno.

![Strelica lijevo/desno za aktivnosti prozor programa Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Pri dnu prikaza dijagrama, vidjet ćete gumbi za zumiranje uvećani, Smanji, Prilagodi, Zumiraj 100%, zaključajte raspored. Gumb za zaključavanje izgleda sprječava nenamjerno premještanje tablice i kanali u prikazu dijagrama, a po zadanom je Uključeno. Možete isključiti tu mogućnost i kretanje entiteti u dijagramu. Kada je ISKLJUČENO, koristite gumb posljednji da biste automatski postavili tablica i kanali. Možete i zumirati / smanjivanje pomoću kotačića.

![Naredbe za zumiranje prikaz dijagrama](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Popis aktivnosti sustava Windows
Windows popis aktivnosti u srednjem oknu prikazuju se sve prozore aktivnosti za skup podataka koje ste odabrali u explorer resursa ili prikaz dijagrama. Po zadanom je popis silaznim redoslijedom, što znači da vidite najnovije prozor aktivnost na vrhu. 

![Popis aktivnosti sustava Windows](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Ovaj popis ne automatski osvježavati, pa gumb Osvježi na alatnoj traci za ručno osvježavanje.  


Windows aktivnosti može biti u jednom od sljedećih statusa:

<table>
<tr>
    <th align="left">Status</th><th align="left">Substatus</th><th align="left">Opis</th>
</tr>
<tr>
    <td rowspan="8">Čekanje</td><td>ScheduleTime</td><td>Vrijeme je dobili prozora aktivnosti da biste pokrenuli.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Upstream ovisnosti su spremni.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Resursi računalnim nisu dostupni.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Sve instance aktivnosti zauzeti sa sustavom windows druge aktivnosti.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Aktivnost je pauziran i windows aktivnosti ne može se pokrenuti sve dok ga se nastaviti.</td>
</tr>
<tr>
<td>Pokušajte ponovno</td><td>Izvršavanje aktivnosti se ponoviti.</td>
</tr>
<tr>
<td>Provjera valjanosti</td><td>Provjera valjanosti još nije započelo.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Čekanje provjere valjanosti za povlačenje.</td>
</tr>
<tr>
<tr
<td rowspan="2">U tijeku</td><td>Provjera valjanosti</td><td>Provjera valjanosti u tijeku.</td>
</tr>
<td></td>
<td>Prozor aktivnost obrade.</td>
</tr>
<tr>
<td rowspan="4">Nije uspjela</td><td>Prekoračili vremensko ograničenje</td><td>Izvršavanje traje dulje nego je koji je dopušteno mjerodavnim aktivnosti.</td>
</tr>
<tr>
<td>Otkazano</td><td>Otkazao korisničku akciju.</td>
</tr>
<tr>
<td>Provjera valjanosti</td><td>Provjera nije uspjela.</td>
</tr>
<tr>
<td></td><td>Generiranje i/ili provjeriti prozor aktivnost nije uspjelo.</td>
</tr>
<td>Jeste li spremni</td><td></td><td>Prozor aktivnost spremna je za potrošnju.</td>
</tr>
<tr>
<td>Preskočeno</td><td></td><td>Prozor aktivnost nije obrađen.</td>
</tr>
<tr>
<td>Ništa</td><td></td><td>Prozor aktivnosti koje koriste postoje uz neki drugi status, ali vraćena.</td>
</tr>
</table>


Kada kliknete prozor programa aktivnosti na popisu, pogledajte detalje o njoj u prozoru **Programa Windows Explorer aktivnosti** ili **Svojstva** na desnoj strani.

![Aktivnosti prozor Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Osvježavanje windows aktivnosti  
Pojedinosti se neće automatski osvježiti, pa koristite **Osvježavanje** gumb (drugi gumb) na naredbenoj traci da biste ručno osvježili popis windows aktivnosti.  
 

### <a name="properties-window"></a>Prozor svojstva
Prozor svojstva je u oknu krajnjeg desnog aplikacije nadzor i upravljanje njima. 

![Prozor svojstva](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Prikazuje svojstva stavke koje ste odabrali u explorer resursa (prikaz stabla) (ili) dijagrama prikaz (ili) popis windows aktivnosti. 

### <a name="activity-window-explorer"></a>Aktivnosti prozor Explorer

Prozor **Aktivnosti prozora preglednika** nalazi se u oknu krajnjeg desnog nadzor i upravljanje aplikacija. Prikazuje detalje o prozoru aktivnosti koje ste odabrali u Windows aktivnosti skočni ili popis Windows aktivnosti. 

![Aktivnosti prozor Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Možete se prebacivati u neki drugi prozor aktivnosti tako da kliknete u prikazu kalendara na vrhu. **Strelica lijevo**možete koristiti/**strelica desno** gumbi pri vrhu da biste vidjeli prozore aktivnosti iz prethodnih/sljedeći tjedan.

U oknu možete koristiti gumbi na alatnoj traci u donjem oknu da biste **ponovno pokrenite** prozor aktivnost ili **osvježili** detalje. 

### <a name="script"></a>Skripta 
Kartica **skriptu** da biste pogledali definiciju JSON odabrani entitet tvorničke podataka (povezane servisa, skup podataka i kanal). 

![Kartica skripte](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Korištenje prikaza sustava
Nadzor i upravljanje App uključuje unaprijed pripremljeni sustava prikaze (**Nedavne aktivnosti sustava windows**, **windows nije uspjelo aktivnosti**, **Windows aktivnosti u tijeku**) koja omogućuje prikaz prozora aktivnosti nedavne/nije uspjela/u tijeku za vaše podatke tvorničke. 

Prijelaz na karticu **Nadzor prikaza** na lijevoj strani tako da ga kliknete. 

![Nadzor prikaza kartica](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Trenutno postoje tri Sistemski prikazi podržane. Odaberite mogućnost da biste vidjeli nedavne aktivnosti windows (ili) windows nije uspjelo aktivnosti (ili) u tijeku aktivnosti windows na popisu aktivnosti Windows (pri dnu okna u sredini). 

Kada odaberete mogućnost **Nedavno korištene windows aktivnosti** , vidjet ćete sve nedavne aktivnosti windows silaznim redoslijedom od **posljednjeg pokušaj vrijeme**. 

Prikaz **nije uspjelo aktivnosti windows** možete koristiti da biste vidjeli sve prozore nije uspjelo aktivnosti na popisu. Odaberite prozor nije uspjelo aktivnost na popisu da biste vidjeli detalje o njoj u **Svojstva** prozor (ili) **Explorer prozora aktivnosti**. Možete preuzeti i sve zapisnicima prozor nije uspjelo aktivnost. 


## <a name="sorting-and-filtering-activity-windows"></a>Sortiranje i filtriranje windows aktivnosti
Promjena postavki **vrijeme početka** i **vrijeme završetka** na naredbenoj traci sustava Windows aktivnosti filtar. Nakon što promijenite vrijeme početka i vrijeme završetka, kliknite gumb uz vrijeme završetka da biste osvježili popis Windows aktivnosti.

![Vrijeme početka i završetka](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] Trenutno su cijelo vrijeme u obliku UTC-a u nadzor i upravljanje aplikacija. 

**Popis aktivnosti Windows**, kliknite naziv stupca (na primjer: stanje). 

![Izbornik za stupac na popis Windows aktivnosti](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Možete učiniti sljedeće:

- Sortiranje uzlaznim redoslijedom.
- Sortiranje silaznim redoslijedom.
- Filtriranje po jednu ili više vrijednosti (spremno, na čekanju itd.)

Kada odredite filtra na stupcima, vidjet ćete gumb filtar omogućen za taj stupac da biste naznačili da su vrijednosti u stupcu filtriranih vrijednosti. 

![Filtriranje u stupcu popis aktivnosti Windows](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Da biste očistili filtre možete koristiti isti skočnog prozora. Da biste očistili sve filtre za windows popis aktivnosti, kliknite gumb Očisti filtar na naredbenoj traci. 

![Čišćenje svih filtara na popisu Windows aktivnosti](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Izvođenje akcije za obradu

### <a name="rerun-selected-activity-windows"></a>Pokrenite windows odabrane aktivnosti
Odaberite prozor programa aktivnosti, kliknite strelicu prema dolje za naredbeni gumb za traka i odaberite **ponovno pokrenite** / **ponovno pokrenite s upstream u kanalu**. Kada odaberete mogućnost **ponovno pokrenite s upstream u kanalu** , na ga ponovno pokreće kao i sve prozore upstream aktivnosti. 
    ![Ponovno pokrenite prozor programa aktivnosti](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Na popisu odaberite više prozora aktivnosti i ponovno pokrenite u isto vrijeme. Želite filtrirati aktivnosti windows na temelju statusa (na primjer: **nije uspjelo**) i pokrenite windows nije uspjelo aktivnosti kada riješite problem koji uzrokuje windows aktivnosti da bi se neće uspjeti. U odjeljku sljedeće dodatne informacije o filtriranju aktivnosti windows na popisu.  

### <a name="pauseresume-multiple-pipelines"></a>Zaustavi/nastavi više kanali
Možete odabir više stavki dva ili više kanali (pomoću CTRL) i pomoću naredbe trake (istaknute crveno pravokutnik na sljedećoj slici) Zaustavi/nastavi ih odjednom.

![Privremeno obustavljanje/životopis na naredbenoj traci](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Stvaranje upozorenja 
Stranici upozorenja omogućuje vam stvaranje upozorenja, prikaz i uređivanje i brisanje postojećih upozorenja. Možete i Onemogući ili omogućiti upozorenja. Da biste vidjeli stranicu upozorenja, kliknite karticu upozorenja.

![Kartica upozorenja](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Stvaranje upozorenja

1. Kliknite **Dodaj upozorenje** da biste dodali upozorenja. Pogledajte stranicu s detaljima. 

    ![Stvaranje upozorenja - stranicu s detaljima](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Navedite **naziv** i **Opis** upozorenja, a zatim kliknite **Dalje**. Trebali biste vidjeti **filtre** stranice.

    ![Stvaranje upozorenja – Filtri stranice](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Odaberite **događaj**, **status**i **substatus** (neobavezno) na kojem želite da se servis tvorničke podataka da biste se upozorenje pa kliknite **Dalje**. Prikazat će se stranica za **primatelje** .

    ![Stvaranje upozorenja - primatelja stranice](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. Odaberite mogućnost **e-pošte pretplate administratori** i/ili unesite **Dodatne administrator e-pošte**pa kliknite **Završi**. Prikazat će se upozorenje na popisu. 
    
    ![Popis upozorenja](./media/data-factory-monitor-manage-app/AlertsList.png)

Na popisu upozorenja pomoću gumba povezan s upozorenjem retka za uređivanje i brisanje/Onemogući/Omogući upozorenja. 

### <a name="eventstatussubstatus"></a>Status/događaj/substatus
Sljedeća tablica sadrži popis dostupnih događaja i statusi (i substatuses).

Naziv događaja | Status | Sub status
-------------- | ------ | ----------
Aktivnosti pokrenite prvi koraci | Koraci | Pokretanje
Aktivnosti pokrenuti dovršeno | Uspješna | Uspješna 
Aktivnosti pokrenuti dovršeno | Nije uspjela| Dodjela resursa nije uspio<br/><br/>Nije uspjelo izvođenja<br/><br/>Isteklo<br/><br/>Neuspjele provjere valjanosti<br/><br/>Odbačeni
Stvaranje rada na zahtjev HDI klaster | Koraci | &nbsp; |
Na zahtjev HDI klaster uspješno stvorena | Uspješna | &nbsp; |
Na zahtjev HDI klaster izbrisane | Uspješna | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>Za uređivanje ili brisanje/onemogućiti upozorenja


![Gumbi za upozorenja](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


