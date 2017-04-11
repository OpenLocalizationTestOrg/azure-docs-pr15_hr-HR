<properties
    pageTitle="Rješenje sustava procjenu ažuriranja u zapisnik analize | Microsoft Azure"
    description="Rješenje ažuriranja sustava možete koristiti u zapisnik analize će vam ažuriranje nedostaju poslužitelji u sustavu pomoći."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>Rješenje sustava procjenu ažuriranja u zapisnik Analytics

Rješenje ažuriranja sustava možete koristiti u zapisnik analize će vam ažuriranje nedostaju poslužitelji u sustavu pomoći. Nakon što instalirate rješenje, možete pogledati ažuriranja koja nedostaju nadziranim poslužiteljima pomoću pločicu **Procjenu ažuriranja sustava** na stranici **Pregled** u OMS.

Ako se pronađe ažuriranja koja nedostaju, pojedinosti prikazane su na nadzornoj ploči za **ažuriranja** . Rad s ažuriranja koja nedostaju i razvoj plan da biste ih primijeniti na poslužiteljima na kojima su potrebni možete koristiti na nadzornoj ploči **ažuriranja** .

## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja
Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Dodavanje rješenja za procjenu ažuriranja sustava OMS radni prostor pomoću postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  Postoji bez daljnje konfiguracije obavezno.

## <a name="system-update-data-collection-details"></a>Detalji o zbirke podataka za ažuriranje sustava

Procjena ažuriranja sustava prikuplja metapodataka i stanje podataka pomoću agenata koji ste omogućili.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za procjenu ažuriranja sustava.

| platforme | Izravni Agent | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows|![Da](./media/log-analytics-system-update/oms-bullet-green.png)|![Da](./media/log-analytics-system-update/oms-bullet-green.png)|![ne](./media/log-analytics-system-update/oms-bullet-red.png)|            ![ne](./media/log-analytics-system-update/oms-bullet-red.png)|![Da](./media/log-analytics-system-update/oms-bullet-green.png)| Najmanje 2 puta po dan i 15 minuta nakon instaliranja ažuriranja|

Sljedeća tablica sadrži primjere vrste podataka koji se prikupljaju procjenu ažuriranja sustava:

|**Vrsta podataka**|**Polja**|
|---|---|
|Metapodataka|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAdresa, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP adresa, NetbiosDomainName, LogicalProcessors, DNSName, riješiti problem, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Stanje|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekst, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Da biste radili s ažuriranja

1. Na stranici **Pregled** kliknite pločicu **Procjenu ažuriranja sustava** .  
    ![Pločica ažuriranje procjenu sustava](./media/log-analytics-system-update/sys-update-tile.png)
2. Na nadzornoj ploči **ažuriranja** prikaz kategorija ažuriranja.  
    ![Nadzorna ploča ažuriranja](./media/log-analytics-system-update/sys-updates02.png)
3. Pomaknite se udesno na stranici da biste pogledali plohu **Kritično/sigurnosna ažuriranja za Windows** i potom u odjeljku **klasifikacije**kliknite **Sigurnosna ažuriranja**.  
    ![Sigurnosna ažuriranja](./media/log-analytics-system-update/sys-updates03.png)
4. Na stranici zapisnika pretraživanja prikazana je raznih informacije o sigurnosna ažuriranja koja su pronađena nedostaju s poslužitelja u sustavu. Kliknite **popis** da biste pogledali detaljne informacije o ažuriranjima.  
    ![Rezultati pretraživanja - popisa](./media/log-analytics-system-update/sys-updates04.png)
5. Na stranici zapisnika pretraživanja prikazuje se detaljne informacije o svakom ažuriranju. Uz broj KBID kliknite **Prikaz** da biste prikazali odgovarajući članak Microsoft podrška Web-mjesta.  
    ![Prijavite se rezultati pretraživanja - prikaz KB](./media/log-analytics-system-update/sys-updates05.png)
6. Web-pregledniku otvorit će se na Microsoftovoj podršci Web-stranici za ažuriranje na novoj kartici. Prikaz podataka o ažuriranjima koja nedostaje.  
    ![Microsoft Support web-stranicu](./media/log-analytics-system-update/sys-updates06.png)
7. Korištenje informacija pomoću pronađete, možete stvoriti plan da biste ručno primijenili nedostaju ažuriranje ili možete nastaviti slijedite preostale korake da biste automatski primijenili ažuriranja.
8. Ako želite primijeniti nedostaju ažuriranje, vratite se na nadzornoj ploči **ažuriranja** , a zatim u odjeljku **Ažuriranje pokreće**, kliknite **kliknite da biste zakazali ažuriranje pokrenuti**.  
    ![Ažuriranje se pokreće - zakazano kartica](./media/log-analytics-system-update/sys-updates07.png)
9. Na stranici **Ažuriranje pokreće** na kartici **zakazano** kliknite **Dodaj** da biste stvorili novi ažuriranje Pokreni.  
    ![Zakazano tabulatora – Dodavanje](./media/log-analytics-system-update/sys-updates08.png)
10. Na **Novu ažuriranje pokrenuti** stranici, upišite naziv za ažuriranje Pokreni, dodali pojedinačna računala ili grupe računala, definirati raspored i kliknite **Spremi**.  
    ![Pokreni novi ažuriranja](./media/log-analytics-system-update/sys-updates09.png)
11. Na kartici **zakazano** na prikazuje stranice **Ažuriranje pokreće** novo ažuriranje pokrenuti koji koju ste zakazano.  
    ![Planirani kartica](./media/log-analytics-system-update/sys-updates10.png)
12. Kada se pokrene ažuriranje pokrenuti, vidjet ćete informacije za nju na kartici **pokrenut** .  
    ![Izvodi kartica](./media/log-analytics-system-update/sys-updates11.png)
13. Po završetku ažuriranja pokrenuti kartici **Dovršeno** prikazuje stanje.
14. Ako su ažuriranja primijenjena Update plohu **Kritično/sigurnosna ažuriranja za Windows** , pokrenite prikazivat će se smanjuje broj ažuriranja.  
    ![Ažuriranja za Windows od ključne važnosti i sigurnosne plohu – ažuriranje count smanjene](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Daljnji koraci

- [Zapisnika pretraživanja](log-analytics-log-searches.md) da biste pogledali detaljne sustava ažuriranja podataka.
