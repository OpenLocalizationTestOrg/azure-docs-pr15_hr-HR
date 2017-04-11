<properties
    pageTitle="Konfiguraciji Procjena rješenja u prijava analitiku | Microsoft Azure"
    description="Konfiguraciji Procjena rješenja u prijava analitiku pruža detaljne informacije o trenutnom stanju infrastruktura za poslužitelj sustava centar za komponentu Operations Manager pri korištenju agenata Operations Manager ili u grupi Upravljanje Operations Manager."
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

# <a name="configuration-assessment-solution-in-log-analytics"></a>Konfiguraciji Procjena rješenja u zapisnika Analytics

Konfiguraciji Procjena rješenja u prijava analitiku pomaže vam pronalazi potencijalne probleme konfiguraciju poslužitelja putem upozorenja i preporuke znanja.

Ovo je rješenje potrebno sustava centar za komponentu Operations Manager. Konfiguraciji procjena nije dostupno ako koristite samo agente izravno vezom.

Prikaz neke informacije u konfiguraciji Procjena rješenje potreban je dodatak za Silverlight za vaš preglednik.

>[AZURE.NOTE] Početak srpanj 5, 2016, rješenje konfiguraciji Procjena više nije moguće dodati prijava analitiku radnih prostora i rješenje više neće biti dostupan za postojeće korisnike nakon kolovoza 1, 2016. Za korisnike koji se koriste rješenja za SQL Server ili servisa Active Directory, preporučujemo da umjesto toga koristite [Sustava SQL Server](log-analytics-sql-assessment.md), [Procjenu Active Directory](log-analytics-ad-assessment.md) i [Active Directory replikacije Status](log-analytics-ad-replication-status.md) rješenja. Za korisnike koji koriste konfiguraciji Procjena za Windows, Hyper-V i upravitelja virtualnog računala centar sustava, preporučujemo korištenje zbirki događaja i promijeniti mogućnosti da biste dobiti jednu zaokruženu prikaz problema unutar vašeg okruženja za praćenje.

![Konfiguraciji Procjena pločica](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Prikupljene nadziranim poslužitelja i slati OMS servis u oblaku za obradu podataka konfiguracije. Logika primjenjuje se na primljene podataka i servisa u oblaku zapisa podataka. Obrađeni podataka na poslužiteljima na kojima se prikazuje za sljedećih područja:

- **Upozorenja:** Prikazuje upozorenja vezane uz konfiguraciju, određene proaktivne koje upućuju za nadziranim poslužiteljima. Ovo je stvorio pravila autor web-mjesto Microsoft Customer i u okvir za podršku tvrtke ili ustanove (CSS) najbolja iskustva iz polja.
- **Preporuke znanja:** Prikazuje članci iz Microsoftove baze znanja koje su preporučena radnih opterećenja koje se nalaze u infrastrukture; te se automatski predložiti ovisno o vašoj konfiguraciji do korištenje strojnog učenja.
- **Poslužitelji i radnih opterećenja analizirati:** Prikazuje poslužitelji i radnih opterećenja koji se nadzire OMS.

![Nadzorna ploča konfiguraciji Procjena](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Tehnologije možete analizirati s konfiguraciji Procjena

OMS konfiguraciji Procjena analizira radnih opterećenja za sljedeće:

- Windows Server 2012 i Microsoft Hyper-V Server 2012.
- Windows Server 2008 i Windows Server 2008 R2, uključujući:
    - Active Directory
    - Glavno računalo Hyper-V
    - Općenito operacijski sustav
- SQL Server 2008 i noviji
    - Modul za baze podataka SQL Server
- Microsoft SharePoint 2010
- Microsoft Exchange Server 2010 i Microsoft Exchange Server 2013
- Microsoft Lync Server 2013 i Lync Server 2010
- 2012 SP1 centar sustava – Upravitelj virtualnog računala

Za SQL Server za analizu podržane su sljedeće 32-bitni i 64-bitna izdanja:

- SQL Server 2016 – sva izdanja
- SQL Server 2014. – sva izdanja
- SQL Server 2008 i 2008 R2 - sva izdanja

Modul za baze podataka SQL Server je analizirati i na sve podržane izdanja. Osim toga, 32-bitno izdanje sustava SQL Server je podržano kada se pokrene u implementaciji WOW64.

## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja
Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Operations Manager potreban je za konfiguraciji Procjena rješenja.
- Agent za komponentu Operations Manager morate imati na svim računalima na mjesto na koje želite procijenite svoju konfiguraciju.
- Dodavanje rješenja konfiguraciji Procjena OMS radni prostor pomoću postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  Postoji bez daljnje konfiguracije obavezno.

## <a name="configuration-assessment-data-collection-details"></a>Detalji o zbirke podataka za procjenu konfiguracija

Konfiguraciji Procjena prikuplja konfiguracijske podatke, metapodataka i podatke o stanju pomoću agente koje ste omogućili.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za konfiguraciji Procjena.

| platforme | Izravni Agent | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows|![ne](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Da](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![ne](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Da](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Da](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| dvaput prema danu|

Sljedeća tablica sadrži primjere prikupljene putem konfiguraciji Procjena vrste podataka:

|**Vrsta podataka**|**Polja**|
|---|---|
|Konfiguracija|IdKlijenta "," AgentID "," EntityID "," ManagedTypeID "," ManagedTypePropertyID "," parametar Sadašnjavrijednost, ChangeDate|
|Metapodataka|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAdresa, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP adresa, NetbiosDomainName, LogicalProcessors, DNSName, riješiti problem, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Stanje|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekst, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Konfiguraciji Procjena upozorenja
Možete pregledavati i upravljanje upozorenjima iz konfiguraciji Procjena sa stranicom upozorenja. Upozorenja reći problem otkrivenog, uzrok i kako da biste riješili problem. Također pružaju informacije o postavkama konfiguracije u svom okruženju koji mogu uzrokovati probleme s performansama.

![Prikaz upozorenja](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] Upozorenja o konfiguraciji Procjena razlikuju se od druga upozorenja zapisnika analize. Prikaz upozorenja potreban je dodatak za Silverlight za vaš preglednik.

Kad odaberete stavke ili kategorije stavki na stranici upozorenja, vidjet ćete popis poslužitelja ili radnih opterećenja upozorenja koji se odnose na svaku stavku.

![Popis upozorenja](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Svakom upozorenju sadrži vezu na članak iz Microsoftove baze znanja. Ti članci sadrže dodatne informacije o upozorenje.

>[AZURE.TIP] Maksimalni broj upozorenja koja se prikazuju se po zadanom je 2000. Da biste pogledali dodatne upozorenja, kliknite traku obavijesti iznad popisa upozorenja.

Možete kliknuti bilo koju stavku na popisu da biste pogledali članak iz baze znanja koji vam mogu pomoći pri rješavanju uzrok problema koji je generirao upozorenja.

![Članak iz baze znanja](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Možete upravljati upozorenja pravila da biste zanemarili određena pravila ili klase pravila.

![Upravljanje pravilima upozorenja](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Preporuke znanja
Kada pregledavate preporuke znanja, se prikazivati rezultati pretraživanja zapisnika stavku Microsoft KB članaka za radnih opterećenja i računalima na kojima ćete pronaći dodatne informacije o upozorenje.

![Rezultati pretraživanja za preporuke znanja](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Poslužitelji i radnih opterećenja analizirati
Kada pregledavate preporuke znanja, se prikazivati rezultati pretraživanja zapisnika popisom poslužitelji i radnih opterećenja koji gube OMS iz Operations Manager.

![Poslužitelji i radnih opterećenja](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali detaljne konfiguraciji Procjena podataka pomoću [zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md) .
