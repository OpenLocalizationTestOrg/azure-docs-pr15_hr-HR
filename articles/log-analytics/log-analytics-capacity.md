<properties
    pageTitle="Kapaciteta rješenje za upravljanje u prijava analitiku | Microsoft Azure"
    description="Možete koristiti rješenja za planiranje kapaciteta u prijava analitiku pomoću kojih se objašnjava kapacitet poslužitelja Hyper-V upravlja virtualnog računala upravitelja sustava centra"
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="capacity-management-solution-in-log-analytics"></a>Kapaciteta rješenje za upravljanje u zapisnik Analytics


Možete koristiti samo rješenje za planiranje kapaciteta prijava analitiku pomoću kojih se objašnjava kapacitet poslužitelja Hyper-V upravlja virtualnog računala upravitelja sustava centra. Ovo je rješenje potrebno sustava centar za komponentu Operations Manager i upravitelja virtualnog računala centar sustava. Planiranje kapaciteta nije dostupno ako koristite samo izravno vezom agenata. Instalacija rješenja da biste ažurirali agent za komponentu Operations Manager. Rješenje čita mjerača performansi na poslužitelju nadziranim i šalje podataka o korištenju OMS servis u oblaku za obradu. Logika primjenjuje se podataka o korištenju i servisa u oblaku zapisa podataka. S vremenom prepoznati uzoraka korištenja i kapacitet je predviđena, ovisno o trenutnom potrošnje.

Ako, na primjer, na projekciji mogu prepoznati kada jezgri dodatne procesora ili dodatnu memoriju neće biti potrebni za pojedinačne poslužitelj. U ovom primjeru projekciju mogu upućivati na 30 dana poslužitelj moraju dodatnu memoriju. To može pomoći u plan za Nadogradnja memorije tijekom poslužitelja sljedeći održavanja prozor, koji se može dogoditi svaka dva tjedna.

>[AZURE.NOTE] Upravljanje kapaciteta rješenje nije dostupno potrebno dodati u radni prostor. Korisnici koji imaju rješenja upravljanja kapaciteta instaliran možete nastaviti koristiti rješenja.  

Rješenje za planiranje kapaciteta je u tijeku ažuriraju u adresi kupca sljedeće prijavljenih izazove:

- Zahtjev za korištenje upravitelja virtualnog računala i Operations Manager
- Nemogućnost Prilagodba/filtar na temelju grupa
- Svaki sat prikupljanja podataka ne čestih dovoljno
- Nema VM razine uvida
- Pouzdanost podataka

Prednosti novog kapaciteta rješenja:

- Veća pouzdanost i točnost podržava prikupljanje zrnastog podataka
- Podrška za značajke Hyper-V bez VMM
- Vizualizacija metriku u PowerBI
- Uvida na razini Upotreba VM


## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja
Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Operations Manager potreban je za upravljanje kapaciteta rješenja.
- Za rješenje za upravljanje kapacitet potreban je upravitelj virtualnog računala.
- Potrebna je veza Upravitelj operacije s virtualnog računala Manager (VMM). Dodatne informacije o povezivanju sustave, potražite [u](http://technet.microsoft.com/library/hh882396.aspx)članku povezivanje VMM s Operations Manager.
- Operations Manager morate biti povezani s zapisnika analize.
- Dodajte rješenje za upravljanje kapaciteta OMS radnog prostora koristeći postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  Postoji bez daljnje konfiguracije obavezno.


## <a name="capacity-management-data-collection-details"></a>Kapaciteta upravljanje Detalji zbirke podataka

Upravljanje kapaciteta prikuplja podataka o performansama, metapodataka i podatke o stanju pomoću agenata koji ste omogućili.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za upravljanje kapaciteta.

| platforme | Izravni Agent | Agent za SCOM | Azure prostora za pohranu | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows|![ne](./media/log-analytics-capacity/oms-bullet-red.png)|![Da](./media/log-analytics-capacity/oms-bullet-green.png)|![ne](./media/log-analytics-capacity/oms-bullet-red.png)|            ![Da](./media/log-analytics-capacity/oms-bullet-green.png)|![Da](./media/log-analytics-capacity/oms-bullet-green.png)| hourly|

Sljedeća tablica sadrži primjere prikupljene putem upravljanja kapaciteta vrste podataka:

|**Vrsta podataka**|**Polja**|
|---|---|
|Metapodataka|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAdresa, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP adresa, NetbiosDomainName, LogicalProcessors, DNSName, riješiti problem, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Performanse|ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|Stanje|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekst, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Kapaciteta stranicu za upravljanje


 Nakon instalacije rješenje za planiranje kapaciteta možete vidjeti kapacitet nadziranim poslužiteljima pomoću pločicu **Planiranje kapaciteta** na stranici **Pregled** u OMS.

![Slika planiranje kapaciteta pločica](./media/log-analytics-capacity/oms-capacity01.png)

Na pločici otvorit će se na nadzornoj ploči **Kapaciteta upravljanja** gdje možete pregledati sažetak kapaciteta poslužitelja. Na stranici prikazuju se sljedeće pločice koje možete kliknuti:

- *Count virtualnog računala*: prikazuje broj dana kapaciteta virtualnim strojevima
- *Izračun*: prikazuje jezgri procesora i memorije
- *Prostor za pohranu*: prikazuje prostor na disku koristi i izračunati prosjek Latencija disk
- *Pretraživanje*: explorer podataka koje možete koristiti za traženje podataka u sustavu OMS

![Slika nadzorne ploče za upravljanje kapaciteta](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>Da biste prikazali stranicu kapaciteta

- Na stranici **Pregled** kliknite **Upravljanje kapaciteta**, a zatim kliknite **izračunati** ili **prostora za pohranu**.

## <a name="compute-page"></a>Stranica za izračun

Na nadzornoj ploči za **Izračun** u programu Microsoft Azure OMS možete koristiti da biste pogledali kapaciteta o upotreba, predviđena dana kapaciteta i učinkovitosti povezane infrastrukture. **Upotreba** područje koristite da biste pogledali procesora mapiranja core i memorije u vašem domaćini virtualnog računala. Možete koristiti alat za projekciju radi procjene koliko kapacitet se očekuje da bi bio dostupan za u određenom razdoblju. Područje **učinkovitosti** možete koristiti da biste vidjeli kako učinkovito vaše domaćini virtualnog računala. Možete pogledati detalje o povezane stavke tako da ih kliknete.

Možete generirati radne knjige programa Excel za sljedećih kategorija:

- Gornji domaćini s najvećim core Upotreba
- Gornji domaćini s najvećim korištenje memorije
- Gornji domaćini s Neučinkovit virtualnim strojevima
- Gornji domaćini po Upotreba
- Dno domaćini po Upotreba

![Slika stranice za izračun upravljanje kapaciteta](./media/log-analytics-capacity/oms-capacity03.png)


Na nadzornoj ploči za **izračunavanje** prikazana su sljedećih područja:

**Upotreba**: prikaz CPU Upotreba core i memorije u vašem domaćini virtualnog računala.

- *Koristi jezgri*: zbroj za sve domaćini (% procesora koristi pomnožena s brojem fizičke jezgri na glavno računalo).
- *Besplatni jezgri*: Ukupna fizička jezgri minus korištenih jezgri.
- *Postotak jezgri dostupna*: slobodno fizičke jezgri podijeljen ukupan broj fizičke jezgri.
- *Virtualna jezgri po VM*: Ukupno virtualne jezgri u sustavu podijeljen ukupan broj virtualnim računalima sustava.
- *Virtualna jezgri fizičke jezgri omjer*: omjer Ukupna fizička jezgri za fizičke jezgri koje koriste virtualnim računalima sustava.
- *Broj dostupnih virtualne jezgri*: virtualne core fizičke jezgri omjer pomnožen dostupna fizičke jezgri.
- *Koristi memorija*: zbroj memorije koja koristi se tako da sva domaćini.
- *Slobodne memorije*: Ukupno fizičke memorije koristi minus memorije.
- *Postotak memorije dostupne*: slobodno fizičke memorije podijeljen ukupne fizičke memorije.
- *Virtualna memorija po VM*: Ukupna virtualna memorija u sustavu podijeljen ukupan broj virtualnim računalima sustava.
- *Virtualna memorija fizičke memorije omjer*: Ukupna virtualna memorija u sustavu podijeljen ukupne fizičke memorije u sustavu.
- *Virtualna memorija dostupna*: virtualna memorija fizičke memorije omjer pomnožen dostupna fizičke memorije.

**Alat za projekcije**

Pomoću alata za projekciju za vaše Upotreba resursa možete pogledati povijesne trendova. To obuhvaća trendova korištenje virtualnih računala, memorije, core i prostora za pohranu. Mogućnost projekcija koristi algoritam projekcije da biste saznali kada su dovoljno sve resurse. Tako ćete izračun proper kapaciteta planiranje tako da znate kada je potrebno da biste kupili dodatne kapaciteta (primjerice memorije, jezgri ili prostora za pohranu).

**Učinkovitosti**

- *Neaktivno VM*: korištenje manje od 10% procesora i 10% memorije u navedenom vremenskom razdoblju.
- *Overutilized VM*: korištenje više od 90% procesora i 90% memorije u navedenom vremenskom razdoblju.
- *Neaktivno glavnog računala*: korištenje manje od 10% procesora i 10% memorije u navedenom vremenskom razdoblju.
- *Overutilized glavnog računala*: korištenje više od 90% procesora i 90% memorije u navedenom vremenskom razdoblju.

### <a name="to-work-with-items-on-the-compute-page"></a>Na stranici računalnim raditi sa stavkama

1. Na **izračunati** nadzornu ploču, u području **Upotreba** prikaz kapaciteta informacije o jezgri procesora i memorije koristi.
2. Kliknite stavku koju želite otvoriti na stranici **pretraživanja** i pregledati detaljne informacije o njemu.
3. U alatu za **projekciju** klizač datuma za prikaz projekcije kapaciteta koja će se koristiti s datumom odaberete.
4. U području **učinkovitosti** prikaz kapaciteta učinkovitosti o virtualnim strojevima i domaćini virtualnog računala.

## <a name="direct-attached-storage-page"></a>Izravni pohranu priključeni na stranicu

Na nadzornoj ploči **Izravni priložene prostora za pohranu** u OMS možete koristiti da biste pogledali kapaciteta informacije o Upotreba prostora za pohranu, performanse diska i Predviđeni dana kapacitetu diska. **Upotreba** područje koristite da biste vidjeli prostor na disku u vašem domaćini virtualnog računala. Područje **Performansi diska** možete koristiti da biste vidjeli na disku propusnost i Latencija u vašem domaćini virtualnog računala. Alat za projekciju možete koristiti i za procjenu koliko kapacitet se očekuje da bi bio dostupan za u određenom razdoblju. Možete pogledati detalje o povezane stavke tako da ih kliknete.

Radne knjige programa Excel možete stvoriti iz ove kapaciteta informacije o sljedećim kategorijama:

- Gornji prostor na disku tako da glavno računalo
- Gornji prosječne latencije po glavnog računala

Na stranici **za pohranu** prikazane su sljedećih područja:

- *Upotreba*: prikaz prostor na disku u vašem domaćini virtualnog računala.
- *Ukupan prostor na disku*: Sum (logički prostora) za sve domaćini
- *Koristi prostor na disku*: Sum (korištenih logičke prostora) za sve domaćini
- *Slobodnog prostora na disku*: ukupan prostor na disku minus korištenih prostora
- *Postotak disku koristi*: koristi prostora na disku podijeljen ukupan prostor na disku
- *Postotak Disk dostupna*: slobodnog prostora na disku podijeljen ukupan prostor na disku

![Slika stranice kapaciteta upravljanje Izravni priložene prostora za pohranu](./media/log-analytics-capacity/oms-capacity04.png)

**Performanse diska**

Pomoću OMS možete pogledati trend povijesnim korištenje prostora na tvrdom disku. Mogućnost projekcija koristi algoritam za buduće korištenje projekta. Za korištenje prostora posebno mogućnost projekcije omogućuje projekta kada pokrenete možda nema dovoljno prostora. To će vam pomoći planiranje proper prostora za pohranu i znate kad trebate kupiti dodatni prostor za pohranu.

**Alat za projekcije**

Pomoću alata za projekciju za vaše Upotreba prostora na disku možete pogledati povijesne trendova. Mogućnost projekcije omogućuje vam projekta kada su dovoljno prostora na disku. To će vam pomoći planiranje kapaciteta proper i znate kada trebate kupiti dodatne kapacitet pohrane.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Na stranici Izravni pohranu priključeni raditi sa stavkama

1. Na nadzornoj ploči za **Izravni priložena prostor za pohranu** , u području **Upotreba** možete pregledati podatke Upotreba disk.
2. Kliknite povezana stavka za otvaranje na stranici **pretraživanja** i prikaz informacija o njemu.
3. U području **Performanse diska** možete pogledati informacije o propusnost i Latencija disk.
4. U **Alat za projekciju**klizač datuma za prikaz projekcije kapaciteta koja će se koristiti s datumom odaberete.


## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali detaljne kapaciteta upravljanja podacima pomoću [zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md) .
