<properties
    pageTitle="Prijavite se računalo grupe u analize zapisnika pretraživanja | Microsoft Azure"
    description="Računalo grupe u prijava analitiku omogućuju opseg pretraživanja zapisnika određeni skup računala.  U ovom se članku opisuju različite metode koje možete koristiti za stvaranje grupe za računala i kako ih koristiti u zapisniku pretraživanja."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Prijavite se računalo grupe u analize zapisnika pretraživanja
Računalo grupe u prijava analitiku omogućuju vam opseg [pretraživanja za prijavu](log-analytics-log-searches.md) određeni skup računala.  Svake grupe se popunjava s računala ili pomoću upita koje sami definirate ili uvozom grupe iz različitih izvora.  Kad se Uključi u grupu u zapisniku pretraživanja rezultate ograničeni su na zapisa koji zadovoljavaju računala u grupi.

## <a name="creating-a-computer-group"></a>Stvaranje grupe za računala
Možete stvoriti grupu računala u zapisnik analize pomoću bilo koje metode u tablici u nastavku.  Informacije o svakom načinu nalaze se u sljedećim odjeljcima. 

| Način | Opis |
|:---|:---|
| Pretraživanje zapisnika       | Stvaranje zapisnika pretraživanja koji vraća popis računala i spremiti rezultate grupno računala. |
| Pretraživanje zapisnika API-JA   | Pomoću API-JA za pretraživanje zapisnika programski Stvaranje grupe računala koji se temelji na rezultate pretrage zapisnika. |
| Active Directory | Automatski Pregledaj članstvo u grupi svim računalima agent kojima su članovi programa domene servisa Active Directory i stvaranje grupe u zapisnik Analytics za svaku grupu sigurnost.
| WSUS              | Automatski Potraži ciljne grupe WSUS poslužitelja ili klijenti i stvaranje grupe u zapisnik Analytics za svaki unos. |


### <a name="log-search"></a>Pretraživanje zapisnika

Računalo grupama koje su stvorene iz zapisnika pretraživanja će sadržavati sva računala koji je vratio upit za pretraživanje koje sami definirate.  Ovaj upit se pokreće svaki put kada se grupa za računalo koristi se tako da odražavaju promjene nakon stvaranja grupe.

Koristite sljedeći postupak da biste stvorili grupu računalo iz zapisnika pretraživanja.

1. [Stvaranje zapisnika pretraživanja](log-analytics-log-searches.md) koji vraća popis računala.  Pretraživanje mora vratiti distinct skup računala pomoću nešto kao što su **Različita računala** ili **mjeru count() na računalu** u upitu.  
2. Kliknite gumb **Spremi** pri vrhu zaslona.
3. Odaberite **da** da biste **Spremi ovaj upit kao grupu računala:**.
4. Upišite **naziv** i **kategorija** za grupu.  Ako već postoji pretraživanja s istim nazivom i kategorija, zatim zatražit će se da biste ga prebrisali.  Možete imati više pretraživanja s istim nazivom u različite kategorije. 

Slijede primjer pretraživanja koja možete spremiti kao grupu za računala.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Pretraživanje zapisnika API-JA

Računalo grupe stvorene pomoću API-JA za pretraživanje zapisnika isti su kao pretraživanja stvorena pomoću zapisnika pretraživanja.

Detalje o stvaranju grupu računala pomoću API-JA za pretraživanje zapisnika potražite u članku [računalo grupe u analize zapisnika prijave pretraživanje REST API -JA](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory

Kada konfigurirate prijava analitiku u uvoz članstva u grupi servisa Active Directory, će analizirati članstvo u grupi sve domene koje se spajaju računalima s OMS agent.  Grupe računala stvorenim u zapisnika Analytics za svaki sigurnosne grupe u servisu Active Directory i svako računalo dodaje se odgovara sigurnosne grupe čiji su članovi grupe računala.  Članstvo u ovom se neprestano ažuriraju svaka 4 sata.  

Konfiguriranje zapisnika Analytics za uvoz servisa Active Directory sigurnosne grupe s izbornika na **Računalu grupe** prijava analitiku **Postavke**.  Odaberite **Automatizacija** , a zatim **članstva u grupi uvoz Active Directory s računala**.  Postoji bez daljnje konfiguracije obavezno.

![Računalo grupe iz servisa Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Nakon uvoza grupe na izborniku prikazat će se popis broj računalima s članstvo u grupi provjeravati i broj grupa uvesti.  Kliknite na bilo koju od ovih veza da biste se vratili **ComputerGroup** zapisa koji sadrže te podatke.

### <a name="windows-server-update-service"></a>Servis za ažuriranje sustava Windows Server

Kada konfigurirate zapisnika analize da biste uvezli članstva u grupi WSUS će analiza ciljnih članstvo u grupi sve računala s OMS agent.  Ako koristite klijentsko ciljanja bilo kojem računalu koje je povezano s OMS te je dio sve WSUS ciljne grupe dodijelit njegov članstvo u grupi uvezeni u zapisnik analize. Ako koristite poslužiteljsko ciljanja na OMS agent trebala biti instalirana na poslužitelju WSUS da bi informacija o članstvu grupe da biste se uvesti OMS.  Članstvo u ovom se neprestano ažuriraju svaka 4 sata. 

Konfiguriranje zapisnika Analytics za uvoz servisa Active Directory sigurnosne grupe s izbornika na **Računalu grupe** prijava analitiku **Postavke**.  Odaberite **Servisa Active Directory** , a zatim **članstva u grupi uvoz Active Directory s računala**.  Postoji bez daljnje konfiguracije obavezno.

![Računalo grupe iz servisa Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Nakon uvoza grupe na izborniku prikazat će se popis broj računalima s članstvo u grupi provjeravati i broj grupa uvesti.  Kliknite na bilo koju od ove veze da biste se vratili **ComputerGroup** zapise pomoću tih informacija.

## <a name="managing-computer-groups"></a>Upravljanje grupama za računala

Možete pogledati računalo grupe koje su stvorene iz zapisnika pretraživanja ili API zapisnika pretraživanja na izborniku **Računalo grupe** u prijava analitiku **Postavke**.  Kliknite **x** u stupcu **Ukloni** da biste izbrisali grupu računala.  Kliknite ikonu **Prikaz članova** grupe da biste pokrenuli pretraživanje zapisnika grupe koji vraća članovima. 

![Grupa spremljenu računala](media/log-analytics-computer-groups/configure-saved.png)

Da biste izmijenili grupi, stvorite novu grupu s istim **kategorija** i **naziv** da biste prebrisali izvornu grupu.

## <a name="using-a-computer-group-in-a-log-search"></a>Korištenje grupe računala u zapisniku pretraživanja
Pomoću sljedeće sintakse za upućivanje na računalu grupe u zapisniku pretraživanja.  Određivanje **kategorija** je neobavezno i samo potreban ako imate grupe na računalu s istim nazivom u različite kategorije. 

    $ComputerGroups[Category: Name]

Prilikom pokretanja pretraživanja članovi grupe računalo obuhvaća pretraživanje najprije razriješiti.  Ako grupi temelji se na zapisnika pretraživanja, pretraživanje je pokrenite da biste se vratili članova grupe prije gornje razine zapisnika pretraživanja.

Grupa računala obično se koriste **u** uvjetu u zapisniku pretraživanja kao u sljedećem primjeru.

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Računalo Grupiranje zapisa

Zapis je stvoren u spremištu OMS za članstvo grupe računala stvorene iz servisa Active Directory ili WSUS.  Ti se zapisi vrstu **ComputerGroup** i imate svojstava u tablici u nastavku.  Zapisi koji ne stvaraju za računalo grupa, ovisno o zapisniku pretraživanja.

| Svojstvo | Opis |
|:--|:--|
| Vrsta                | *ComputerGroup* |
| SourceSystem        | *SourceSystem*  |
| Računalo            | Naziv računala člana. |
| Grupa               | Naziv grupe. |
| GroupFullName       | Cijeli put do grupi uključujući izvora i naziv izvora.
| GroupSource         | Izvor toj grupi je prikupljene od. <br><br>Konzolu ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | Naziv izvorišnog web-mjesta koje je prikupljenih grupe.  Za Active Directory, to je naziv domene. |
| ManagementGroupName | Naziv grupe za upravljanje SCOM agenata.  Za ostale agenata to je AOI -\<ID radnog prostora\> |
| TimeGenerated       | Datum i vrijeme grupi računala stvoren ili ažurirati. |



## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) radi analize podataka prikupljenih iz izvora podataka i rješenja.  