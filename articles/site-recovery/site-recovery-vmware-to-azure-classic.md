<properties
    pageTitle="Replicirati VMware virtualnim strojevima i fizičke poslužitelje Azure s oporavak web-mjesta Azure | Microsoft Azure"
    description="U ovom se članku opisuje kako implementirati Azure oporavak web-mjesta da biste orkestrirali replikacije, prebacivanje i oporavak lokalnog VMware virtualnim strojevima i Windows/Linux fizičke poslužitelji za Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Replicirati VMware virtualnim strojevima i fizičke poslužitelje Azure s oporavak Azure web-mjesta

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmware-to-azure.md)
- [Klasični Portal](site-recovery-vmware-to-azure-classic.md)
- [Klasični Portal (naslijeđeno)](site-recovery-vmware-to-azure-classic-legacy.md)


Oporavak Azure web-mjesta servisa doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Moguće je replicirati strojeva Azure ili sekundarni lokalnog podatkovnog centra. Kratak pregled pročitajte [oporavak web-mjesta Azure?](site-recovery-overview.md).

## <a name="overview"></a>Pregled

U ovom se članku opisuje kako:

- **Replicirati VMware virtualnim strojevima za Azure**– implementacija oporavak web-mjesta za koordinaciju replikacije, prebacivanje i oporavak lokalnog VMware virtualnim strojevima Azure za pohranu.
- **Replicirati fizičke poslužitelji za Azure**– implementacija oporavak Azure web-mjesta za koordinaciju replikacije, prebacivanje i oporavak lokalnog fizičke Windows i Linux poslužitelji za Azure.

>[AZURE.NOTE] U ovom se članku opisuje kako replicirati Azure. Ako želite replicirati VMware VMs ili Windows/Linux fizičke poslužitelje sekundarne podatkovnog centra, slijedite upute u [ovom članku](site-recovery-vmware-to-vmware.md).

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.

## <a name="enhanced-deployment"></a>Poboljšana implementacije

Ovaj članak sadrži sadrži upute za poboljšane implementacije na portalu klasični Azure. Preporučujemo da koristite ovu verziju za sve nove implementacije. Ako ste već uvesti pomoću starije verzije naslijeđene preporučujemo da migrirati u novu verziju. Saznajte [više](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) o migraciji.

Poboljšana implementacije je važnija ažuriranja. Ovo je sažetak poboljšanja unijeli smo:

- **Nema infrastrukture VMs u Azure**: podataka replicira izravno na račun za Azure prostora za pohranu. Uz to za replikaciju i prebacivanje nema potrebe da biste postavili sve infrastrukture VMs (poslužitelj za konfiguraciju, osnovne odredišni poslužitelj) po potrebi smo naslijeđene implementacije.  
- **Instalacija Unified**: jednom instalacijskom omogućuje jednostavno postavljanje i skalabilnost za lokalni komponente.
- **Sigurne implementacije**: sve promet šifriran i replikacije upravljanje komunikacije šalju putem HTTP 443.
- **Oporavak točaka**: podrška za pad i aplikacije dosljedan oporavak točaka za Windows i Linux okruženja, a podržava i jedan VM i Višestruki VM dosljedan konfiguracije.
- **Testiranje prebacivanje**: podrška za prebacivanje koje nisu disruptive test za Azure bez koje utječu na proizvodnje ili pauziranje replikacije.
- **Prebacivanje neplanirano**: podrška za neplanirano prebacivanje Azure poboljšane mogućnosti isključivanja VMs automatski prije prebacivanja u slučaju pogreške.
- **Failback**: integrirani failback koji umnaža samo delta promjene natrag na lokalni web-mjesta.
- **vSphere 6.0**: ograničena podrška za VMware Vsphere 6.0 implementacije.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Kako oporavak web-mjesta štiti virtualnim strojevima i fizičke poslužitelje?


- VMware administratori mogu konfigurirati službenoj zaštitu Azure radnih opterećenja tvrtke i aplikacije koje rade na virtualnim strojevima VMware. Upravitelji poslužitelja možete replicirati fizičke lokalnog sustava Windows i Linux poslužitelji za Azure.
- Konzola za oporavak Azure web-mjesta omogućuje jedno mjesto za upravljanje replikacije, prebacivanje i oporavak procesa i jednostavno postavljanje.
- Ako je replicirati VMware virtualnim strojevima upravlja vCenter poslužitelj, oporavak web-mjesta mogu automatski otkriti te VMs. Ako na računalu koje hostira ESXi strojeva oporavak web-mjesta otkrije VMs na glavnom računalu.
- Pokretanje jednostavno failovers s infrastruktura za lokalni Azure i failback (Vraćanje) iz Azure VMware VM poslužiteljima lokalnog web-mjestu.
- Konfiguriranje tarife za oporavak grupirati radnih opterećenja aplikacije koje su tiered na više računala. Uspijeva preko tih tarife i oporavak web-mjesta sadrži više VM dosljednost tako da se računala izvode iste radnih opterećenja može ujednačenim podacima točku oporaviti zajedno.


## <a name="supported-operating-systems"></a>Podržani operacijski sustavi

### <a name="windows64-bit-only"></a>Windows (samo 64-bitni)
- Windows Server 2008 R2 SP1 +
- Windows Server 2012.
- Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (samo 64-bitni)
- Crvena je vaša Enterprise Linux 6,7, 7.1, 7,2
- CentOS 6.5, 6.6, 6,7, 7.0, 7.1, 7,2
- Oracle Enterprise Linux 6,4, 6.5 pokrenut je vaša crveno kompatibilne otklanjanje ili Unbreakable Enterprise otklanjanje izdanje 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Scenarij arhitekture

Scenarij komponente:

- **Na poslužitelju za upravljanje lokalnim**: poslužitelj za upravljanje pokreće komponente oporavak web-mjesta:
    - **Poslužitelj za konfiguraciju**: koordinate komunikacije, a upravlja postupaka replikacije i oporavak podataka.
    - **Poslužitelj za postupak**: služi kao replikacije pristupnika. Ga prima podatke iz zaštićenog izvor strojeva, optimizira predmemoriranja, sažimanja i šifriranje i šalje replikacije podataka Azure prostora za pohranu. Također rukuje automatske instalacije servisa mobilnost na zaštićenom računalima i izvodi automatsko otkrivanje VMware VMs.
    - **Matrica poslužitelju ciljni**: rukuje replikacije podataka tijekom failback iz Azure.
    Možete uvesti i poslužitelj za upravljanje koji funkcionira kao poslužitelj postupak samo da bi se promijenila veličina implementaciju sustava.
- **Servis za mobilnost u**: komponente je implementiran na svakom računalu (VMware VM ili fizičke poslužitelja) koje želite replicirati Azure. Snima zapisivanje podataka na računalu i prosljeđuje ih na poslužitelj za postupak.
- **Azure**: ne morate stvoriti sve Azure VMs učiniti replikaciju i prebacivanje. Oporavak web-mjesta servisa rukuje upravljanje podacima, a podaci replicira izravno na Azure prostora za pohranu. Repliciranu Azure VMs su spun se automatski samo kada se pojavi prebacivanje na Azure. Međutim, ako želite da se neće vratiti iz Azure lokalno mjesto morat ćete postaviti programa Azure VM će poslužiti kao poslužitelj za postupak.


Grafika prikazuje način na koji rade te komponente.

![Arhitektura](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Slika 1: VMware/fizičke za Azure** (stvorio Henry Robalino)


## <a name="capacity-planning"></a>Planiranje kapaciteta

Kada ste planiranja kapaciteta ovdje, što biste trebali razmisliti o:

- **Okruženje za izvor**– planiranje kapaciteta ili VMware Infrastruktura i izvor strojno preduvjete.
- **Poslužitelj za upravljanje**– planiranje lokalne poslužitelje upravljanja koji se izvode komponente oporavak web-mjesta.
- **Propusnost mreže s izvora na ciljno**-planiranje propusnost mreže potrebnog za replikaciju između izvora i Azure

### <a name="source-environment-considerations"></a>Razmatranja okruženje izvora

- **Svakodnevno Najveća promjena stopa**– zaštićeni računala možete koristiti samo jedan poslužitelj za postupak i jedan proces poslužitelja možete rukovati do 2 TB promjena podataka po danu. Stoga 2 TB je maksimalna dnevno podaci mijenjaju stopa koja je podržana za zaštićeni računala.
- **Maksimalna propusnost**– repliciranu strojno pripadati jednog računa za pohranu u Azure. Standardni prostora za pohranu računa možete učiniti najviše 20 000 za u sekundi pa preporučujemo da ostavite broj IOPS preko izvornog računala 20 000. Za primjer ako imate izvor računala s 5 diskova i svakom disku generira 120 IOPS (8K veličina) na izvoru ona će biti unutar Azure po disk IOPS ograničenje od 500. Broj računa za pohranu potrebno = izvora zbroja IOPs/20000.


### <a name="management-server-considerations"></a>Razmatranja poslužitelj za upravljanje

Poslužitelj za upravljanje pokreće oporavak web-mjesta komponente koje rukovati optimizaciju podataka, replikacije i upravljanje. Trebali biste moći rukovanje dnevni kapacitet stopa Promijeni sve pokrenute na zaštićenom strojeva radnih opterećenja i ima li dovoljno propusnosti za stalno replicirati podataka Azure za pohranu. Konkretno:

- Poslužitelj za postupak prima replikacije podatke iz zaštićenog strojeva i optimizira predmemoriranja, sažimanja i šifriranje prije no što pošaljete Azure. Poslužitelj za upravljanje mora imati dovoljno resursa za izvođenje ovih zadataka.
- Poslužitelj za postupak koristi predmemorije diska temelje. Preporučujemo da se na disku zasebnom predmemorije 600 GB ili više učiniti promjene podataka pohranjene u slučaju usko grlo mreže ili nedostupnosti. Tijekom implementacije možete konfigurirati predmemoriju na bilo kojem pogonu koji ima najmanje 5 GB prostora za pohranu dostupno dok je 600 GB minimalne preporuke.
- Kao najbolji način preporučujemo da se poslužitelju za upravljanje nalaziti na istoj mreži i segmentu LAN-a kao strojeva koji želite zaštititi. Nalazi na drugoj mreži, ali strojeva koji želite zaštititi mora imati L3 mreže vidljivost na njega.

Veličina preporuke za poslužitelj za upravljanje su navedene u tablici u nastavku.

**Poslužitelj za upravljanje CPU-a** | **Memorije** | **Veličina predmemorije na disku** | **Podaci mijenjaju stopa** | **Zaštićeni računalima**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5) | 16 GB | 300 GB | 500 GB ili manje | Implementacija poslužitelja za upravljanje pomoću navedenih postavki za replikaciju manje od 100 strojeva.
12 vCPUs (2 sockets * 6 jezgri @ GHz 2,5) | 18 GB | 600 GB | 500 GB 1 TB | Implementacija poslužitelja za upravljanje pomoću navedenih postavki za replikaciju između 100 150 strojeva.
16 vCPUs (2 sockets * 8 jezgri @ GHz 2,5) | 32 GB | 1 TB | 1 TB za 2 TB | Implementacija poslužitelja za upravljanje pomoću navedenih postavki za replikaciju između strojeva 150 200.
Drugi poslužitelj za postupak implementacije | | | > 2 TB | Implementacija poslužitelja dodatne postupak ako ste replikaciju više od 200 strojeva ili ako promijenite dnevnih podataka stopa prelazi 2 TB.

Pri čemu je:

- Svaki izvor strojno konfiguriran 3 diskova 100 GB.
- Koristi benchmarking prostora za pohranu 8 pogona SAS 10 k RPM s RAID 10 za predmemorije diska mjere.

### <a name="network-bandwidth-from-source-to-target"></a>Propusnost mreže iz izvora cilj
Provjerite je li izračun propusnosti koji će biti potrebni za početne replikacije i delta replikacije pomoću [alata za planer kapaciteta](site-recovery-capacity-planner.md)

#### <a name="throttling-bandwidth-used-for-replication"></a>Ograničavanje propusnost za replikaciju

Promet VMware replicirati na Azure prolazi kroz poslužitelj za određeni proces. Možete throttle propusnosti koji je dostupan za replikaciju oporavak web-mjesta na tom poslužitelju na sljedeći način:

1. Otvaranje Microsoft Azure sigurnosne kopije BLOG dodatak na glavnom upravljanje poslužitelju ili na poslužitelju za upravljanje radi dodatnih dodjeli poslužitelje postupak. Prema zadanim se postavkama prečac za stvaranje sigurnosne kopije Microsoft Azure stvara se na radnu površinu ili ga možete pronaći: c:\Programske datoteke\Microsoft Azure oporavak usluge Agent\bin\wabadmin.
2. U dodatak kliknite **Promijeni svojstva**.

    ![Ograničenja propusnosti](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. Na kartici **Throttling** navedite propusnost koje je moguće koristiti za replikaciju oporavak web-mjesta i odgovarajuće zakazivanja.

    ![Ograničenja propusnosti](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Po želji možete postaviti ograničavanje pomoću komponente PowerShell. Evo jednog primjera:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Maksimiziranje korištenja propusnosti
Da biste povećali propusnost koristi se za replikaciju po oporavak web-mjesta za Azure, morat ćete promijeniti ključ registra.

Sljedeći ključ određuje broj niti po replikaciju disk koji koriste se za replikaciju

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 U odjeljku "overprovisioned" mreža ovog ključa registra morate promijeniti iz njegova zadane vrijednosti. Podržavamo maksimalno 32.  


[Dodatne informacije](site-recovery-capacity-planner.md) o planiranju detaljne kapaciteta.

### <a name="additional-process-servers"></a>Dodatni postupak poslužitelja

Ako morate zaštita više od 200 strojeva ili veća dnevnih stopa promjena te 2 TB možete dodati dodatne poslužitelji za rukovanje opterećenje. Da biste skalirali izgleda koje možete:

- Povećanje broja poslužitelji za upravljanje. Na primjer možete zaštititi do 400 računala s dva upravljanje poslužitelja.
- Dodavanje dodatnih postupak poslužitelja i koristite ih učiniti promet umjesto (ili uz) poslužitelj za upravljanje.

U ovoj su tablici opisane scenarij u kojem:

- Da biste koristili kao samo poslužitelj konfiguracije postavite izvorni poslužitelj za upravljanje.
- Postavljanje poslužitelj za dodatne postupak.
- Konfiguriranje zaštićeni virtualnim strojevima koristite dodatne postupak poslužitelj.
- Svakom računalu zaštićeni izvora je konfiguriran za korištenje tri diskova 100 GB.

**Izvorni poslužitelj za upravljanje**<br/><br/>(poslužitelj za konfiguraciju) | **Dodatni postupak poslužitelja**| **Veličina predmemorije na disku** | **Podaci mijenjaju stopa** | **Zaštićeni računalima**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5), 16 GB memorije | 4 vCPUs (2 sockets * 2 jezgri @ GHz 2,5), 8 GB memorije | 300 GB | 250 GB ili manje | Možete replicirati strojeva 85 ili manje.
8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5), 16 GB memorije | 8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5), 12 GB memorije | 600 GB | 250 GB 1 TB | Možete replicirati između 85 150 strojeva.
12 vCPUs (2 sockets * 6 jezgri @ GHz 2,5), 18 GB memorije | 12 vCPUs (2 sockets * 6 jezgri @ GHz 2,5) 24 GB memorije | 1 TB | 1 TB za 2 TB | Možete replicirati između strojeva 150 225.


Način skaliranje poslužitelja će ovisi o postavkama za skalu prema gore ili skaliranje out modela.  Proširenja uvođenjem nekoliko visokokvalitetni upravljanja i proces poslužitelja ili skalirali uvođenjem više poslužitelja s manje resursima. Na primjer: Ako vam je potrebna zaštita 220 strojeva nije učinite nešto od sljedećeg:

- Konfiguriranje izvorni poslužitelj za upravljanje s 12vCPU, 18 GB memorije, poslužitelj za dodatne procesa s 12vCPU, 24 GB memorije, i konfiguriranje zaštićeni strojeva koji koriste samo poslužitelj dodatne postupak.
- Ili umjesto toga nije konfiguriranje dva upravljanje (2 x 8vCPU, 16 GB RAM-a) i dva dodatna postupak poslužitelja (1 x 8vCPU) i 4vCPU x 1 za rukovanje 135 + 85 (220) računalima i konfiguriranje zaštićeni strojeva poslužitelje sustava dodatne postupak samo.


[Slijedite ove upute](#deploy-additional-process-servers) da biste postavili poslužitelj za dodatne postupak.

## <a name="before-you-start-deployment"></a>Prije nego što započnete implementacije

Tablica sažetak preduvjeti za implementaciju scenarij.


### <a name="azure-prerequisites"></a>Preduvjeti za Azure

**Preduvjeta** | **Pojedinosti**
--- | ---
**Račun za Azure**| Potreban vam je račun sustava [Microsoft Azure](https://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.
**Azure prostora za pohranu** | Morat ćete račun Azure prostora za pohranu za pohranu repliciranu podataka. Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs su spun se kada se pojavi prebacivanje. <br/><br/>Potreban [račun standardni zemlj suvišnih prostora za pohranu](../storage/storage-redundancy.md#geo-redundant-storage). Račun mora biti u području isti kao servis oporavak web-mjesta, a biti pridruženi iste pretplate. Imajte na umu da replikacija s računima za pohranu premium trenutno nije podržan, a ne bi trebalo koristiti.<br/><br/>Ne podržavamo Premjesti račune za pohranu stvoren pomoću [Novi Azure portal](../storage/storage-create-storage-account.md) preko grupe resursa. [Doznajte](../storage/storage-introduction.md) Azure prostora za pohranu.<br/><br/>
**Azure mreže** | Morat ćete Azure virtualne mreže koji Azure VMs će se povezati s kada dođe do prebacivanje. Azure virtualne mreže mora biti u istom području kao sigurnog oporavak web-mjesta.<br/><br/>Napomena da uvoza natrag nakon prebacivanje na Azure morat ćete VPN veza (ili Azure ExpressRoute) postavljanje Azure mrežu na web-mjesto lokalnog.


### <a name="on-premises-prerequisites"></a>Lokalni preduvjeti

**Preduvjeta** | **Pojedinosti**
--- | ---
**Poslužitelj za upravljanje** | Morate pokrenuti na virtualnog računala ili poslužitelj za fizičke lokalnog poslužitelja sustava Windows 2012 R2. Sve komponente za oporavak web-mjesta na lokalni instaliranih na poslužitelju za upravljanje<br/><br/> Preporučujemo da implementacija poslužitelju kao vrlo dostupna VM VMware. Failback web-mjesto lokalnog iz Azure je uvijek se za VMware VMs neovisno o tome koje nije uspjela tijekom VMs ili fizičke poslužitelje. Ako poslužitelj za upravljanje ne konfigurirati kao VMware VM morat ćete postaviti zasebne osnovne cilj poslužitelja kao VMware VM prima failback promet.<br/><br/>Poslužitelj ne smije biti kontroler domene.<br/><br/>Poslužitelj imat statičke IP adrese.<br/><br/>Naziv glavnog računala poslužitelja mora biti 15 znakova ili manje.<br/><br/>Regionalne sheme operacijskog sustava mora biti samo engleski.<br/><br/>Poslužitelj za upravljanje potreban je pristup Internetu.<br/><br/>Morate izlaznog pristupiti s poslužitelja na sljedeći način: privremeni pristup na HTTP 80 tijekom postavljanja komponenti za oporavak web-mjesta (da biste preuzeli MySQL); Tijeku izlaznog pristup HTTPS 443 za replikaciju upravljanja; Stalnog izlaznog pristupa na HTTPS 9443 za replikaciju promet (priključak može se mijenjati)<br/><br/> Provjerite je li poslužitelj za upravljanje dostupni URL-ove: <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Ako imate IP utemeljen na adresi vatrozid pravila na poslužitelju, potvrdite da pravila dopustiti komunikaciju za Azure. Morat ćete dopustiti [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/details.aspx?id=41653) i priključak HTTPS (443). Ćete morati bijeli popis rasponi IP adresa za Azure regija pretplate i Zapad SAD-a. URL [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") je za preuzimanje MySQL.
**Glavno računalo vCenter/ESXi VMware**: | Potreban vam je jedan ili više vMware vSphere ESX/ESXi hypervisors upravljanje na virtualnim strojevima VMware ESX/ESXi verzija 6.0, 5,5 ili 5.1 raditi s najnovijim ažuriranjima.<br/><br/> Preporučujemo da implementacija VMware vCenter poslužitelju za upravljanje sustava ESXi domaćini. Ga treba imati vCenter verzija 6.0 ili 5,5 s najnovijim ažuriranjima.<br/><br/>Imajte na umu da oporavak web-mjesta ne podržava nove vCenter i vSphere 6.0 značajke kao što su unakrsno vCenter vMotion, virtualne količine i pohranu DRS. Značajke koje nisu dostupne u verziji 5,5 ograničen podršci za oporavak web-mjesta.
**Zaštićeno strojeva**: | **AZURE**<br/><br/>Strojeva koji želite zaštititi mora biti u skladu s [Azure preduvjeta](site-recovery-best-practices.md#azure-virtual-machine-requirements) za stvaranje Azure VMs.<br><br/>Ako želite povezati Azure VMs nakon prebacivanje zatim morat ćete omogućiti veze udaljene radne površine na lokalni vatrozid.<br/><br/>Kapacitet pojedinačne disk na zaštićenom bi trebalo biti više od 1023 GB. Na VM mogu imati do 64 diskova (dakle do 64 TB). Ako imate diskova veće od 1 TB razmislite o korištenju replikacije baze podataka, kao što je SQL Server uvijek na ili straža podataka tvrtke Oracle.<br/><br/>Minimalna 2 GB slobodnog prostora na disku instalacije za instalaciju komponente.<br/><br/>Zajedničko korištenje na disku goste klastere nisu podržane. Ako imate grupirani implementacije razmislite o korištenju replikacije baze podataka, kao što je SQL Server uvijek na ili straža podataka tvrtke Oracle.<br/><br/>Sjedinjene Extensible firmver sučelja (UEFI) / pokretanje Extensible firmver Interface(EFI) nije podržana.<br/><br/>Nazivi računala mora sadržavati između 1 i 63 znakova (slova, brojeve i spojnice). Naziv mora započeti s slovo ili broj i završavaju slovo ili broj. Kada je zaštićena stroj možete izmijeniti naziv Azure.<br/><br/>**VMware VMs**<br/><br>Morat ćete instalirati VMware vSphere PowerCLI 6.0. na poslužitelju za upravljanje (poslužitelj za konfiguraciju).<br/><br/>VMware VMs koji želite zaštititi moraju imati instaliran i pokrenut Alati za VMware.<br/><br/>Ako je izvor VM NIC udruživanje nakon prebacivanje na Azure se pretvara jedan NIC.<br/><br/>Ako zaštićene VMs na disk iSCSI zatim oporavak web-mjesta pretvara zaštićeni VM iSCSI na disku u VHD datoteke ako na VM ne uspije putem Azure. Ako je odredište iSCSI proslijediti po Azure VM zatim bit će povezati odredište iSCSI i zapravo potražite u članku dva diskova – VHD disk na Azure VM i izvorni iSCSI disk. U ovom slučaju morate prekinuti vezu iSCSI cilj koji se pojavljuje na nije uspio putem Azure VM.<br/><br/>[Saznajte više](#vmware-permissions-for-vcenter-access) o korisničkim dozvolama za VMware koje su potrebne za oporavak web-mjesta.<br/><br/> **WINDOWS SERVER STROJEVA (na VMware VM ili fizičke poslužitelj)**<br/><br/>Poslužitelj trebao bi biti pokrenut podržani 64-bitni operacijski sustav: Windows Server 2012 R2, Windows Server 2012 ili Windows Server 2008 R2 sa pri najmanje SP1.<br/><br/>Operacijski sustav potrebno instalirati na pogon C:\ i OS disk mora biti Windows osnovni disk (OS se bi trebalo biti instalirana na Windows dinamički disk.)<br/><br/>Za računala za Windows Server 2008 R2 morate imati .NET Framework 3.5.1 instaliran.<br/><br/>Morat ćete unijeti administratorski račun (morate biti administrator lokalne na računalu Windows) za instalaciju automatske servis mobilnost na poslužiteljima sustava Windows. Ako je račun za navedeni račun koji nije domene morat ćete onemogućiti kontrolu za daljinski pristup korisnika na lokalnom računalu. [Dodatne informacije](#install-the-mobility-service-with-push-installation).<br/><br/>Oporavak web-mjesta podržava VMs RDM disku.  Tijekom failback oporavak web-mjesta će ponovno korištenje diska RDM ako izvornog izvora VM i RDM disk. Ako nisu dostupne tijekom failback oporavak web-mjesta će stvoriti novu datoteku VMDK za svaki disk.<br/><br/>**LINUX RAČUNALIMA**<br/><br/>Potreban vam je podržana 64-bitni operacijski sustav: crvena je vaša Enterprise Linux 6,7; Centos 6.5, 6.6,6.7; Oracle Enterprise Linux 6,4, 6.5 pokrenut je vaša crveno kompatibilne otklanjanje ili Unbreakable Enterprise otklanjanje izdanje 3 (UEK3) SUSE Linux Enterprise Server 11 SP3.<br/><br/>/etc/Hosts datoteke na zaštićenom moraju sadržavati stavke koje mapiranje lokalne glavno IP adrese pridružene sve mrežnih prilagodnika. <br/><br/>Ako želite povezati Azure virtualnog računala radi Linux nakon prebacivanje pomoću ljuske za sigurnu klijenta (ssh) provjerite je li servis za sigurnu ljuske na zaštićenom računalu postavljena da biste započeli na automatsko pokretanje sustava i dopustiti pravila vatrozida programa ssh veze na njega.<br/><br/>Zaštita je moguće omogućiti samo za Linux strojevima sa sljedećim prostora za pohranu: File system (EXT3, ETX4, ReiserFS, XFS); Multipath softver uređaji mapiranje (multipath)); Upravitelj glasnoće: (LVM2). Fizička poslužitelji za pohranu kontroler HP CCISS nisu podržane. Datotečnom sustavu ReiserFS podržana je samo na SUSE Linux Enterprise Server 11 SP3.<br/><br/>Oporavak web-mjesta podržava VMs RDM disku.  Tijekom failback za Linux oporavak web-mjesta ne ponovno korištenje RDM disk. Umjesto toga stvara novu datoteku VMDK za svaki odgovarajući RDM disk.

Samo za VM Linux – provjerite je li postavili postavku disk.enableUUID=true u konfiguraciji parametre VM u VMware. Ako redak ne postoji, dodajte ga. Potrebna je za pružanje dosljedan UUID na VMDK tako da ga pravilno mounts. Imajte na umu da bez tu postavku, failback uzrokovat će cijeli preuzimanje čak i ako je na VM dostupna na-prem. Dodavanje tu postavku će osigurati samo delta promjene prenose natrag tijekom failback.

## <a name="step-1-create-a-vault"></a>Korak 1: Stvaranje na zbirke ključeva

1. Prijava na [Portal za upravljanje](https://manage.windowsazure.com/).
2. Proširite **Data Services** > **Oporavak usluge** kliknite **Sigurnog oporavak web-mjesta**.
3. Kliknite **Stvori novi** > **brzo stvaranje**.
4. U odjeljak **naziv**unesite neslužbeni naziv da biste odredili na zbirke ključeva.
5. U **regiji**, odaberite regiji u zbirke ključeva. Da biste provjerili podržanih regija pogledajte geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Kliknite **Stvaranje sigurnog**.
    ![Nove zbirke ključeva](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Provjerite traku stanja da biste potvrdili na sigurnog uspješno je stvorena. Na sigurnog će biti navedena kao **aktivna** na glavnoj stranici **Servisa za oporavak** .

## <a name="step-2-set-up-an-azure-network"></a>Korak 2: Postavite Azure mrežu

Postavljanje Azure mrežu tako da se Azure VMs će biti povezani s mrežom nakon prebacivanje i tako da failback web-mjesto lokalnog možete funkcioniraju.

1. Na portalu za Azure > **Stvori virtualne mreže** Navedite naziv mreže. IP adresa raspon i podmreže naziv.
2. Trebat ćete dodati VPN-ExpressRoute mreži ako morate učiniti failback. VPN-ExpressRoute moguće je dodati na mrežu i nakon prebacivanje.

[Dodatne informacije potražite u](../virtual-network/virtual-networks-overview.md) Azure mrežama.

> [AZURE.NOTE] [Migracija mreže](../resource-group-move-resources.md) preko grupe resursa unutar iste pretplate ili putem pretplate nije podržana za mreže koji se koristi za implementaciju oporavak web-mjesta.

## <a name="step-3-install-the-vmware-components"></a>Korak 3: Instalacija komponente VMware

Ako želite replicirati VMware virtualnog računala instalirati sljedeće komponente VMware na poslužitelju za upravljanje:

1. [Preuzmite](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) i instalirajte VMware vSphere PowerCLI 6.0.
2. Ponovno pokrenite poslužitelj.


## <a name="step-4-download-a-vault-registration-key"></a>Korak 4: Preuzimanje ključa za registraciju zbirke ključeva

1. Iz upravljanje server Azure otvoriti konzole za oporavak web-mjesta. Na stranici **Servisa oporavak** kliknite sigurnog da biste otvorili stranicu za brzo pokretanje. Brzi početak rada može se otvoriti u bilo kojem trenutku pomoću ikone.

    ![Ikona za brzi početak rada](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. Na stranici za **Brzo pokretanje** kliknite **Priprema ciljnog resursi** > **Preuzimanje ključa za registraciju**. Datoteka registracije automatski se generira. Nije valjan 5 dana nakon generira.


## <a name="step-5-install-the-management-server"></a>Korak 5: Instalacija poslužitelju za upravljanje
> [AZURE.TIP] Provjerite je li poslužitelj za upravljanje dostupni URL-ove:
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/ncsi.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. Na stranici za **Brzi početak rada** preuzmite objedinjenih instalacijsku datoteku na poslužitelj.

2. Pokrenite instalacijsku datoteku da biste pokrenuli instalaciju u čarobnjaku za postavljanje za Sjedinjeno komuniciranje oporavak web-mjesta.

3.  **Prije nego što počnete** odaberite **Instalacija poslužitelja za konfiguraciju i poslužitelj za postupak**.

    ![Prije početka](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. U **Proizvođača softverskoj licenci** kliknite **prihvaćam** da biste preuzeli i instalirali MySQL. 

    ![Treći = proizvođača softvera](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. U **Registracija** pronađite i odaberite ključa za registraciju ste preuzeli s na zbirke ključeva.

    ![Registracija](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. U **Postavke internetske** odredite kako davatelja usluge na poslužitelju za konfiguraciju će se povezati s oporavak Azure web-mjesta putem Interneta.

    - Ako se želite povezati s proxy poslužitelja koja je trenutno postavljena na računalu odaberite **Poveži s postojeće postavke proxyja**.
    - Ako želite da se davatelj usluga za povezivanje izravno odaberite **Poveži izravno bez proxy poslužitelj**.
    - Ako postojeće proxy poslužitelj zahtijeva provjeru autentičnosti ili koji želite koristiti prilagođenu proxy davatelj veze, odaberite **Poveži s postavkama prilagođene proxy**.
        - Ako koristite prilagođeni proxy morat ćete navesti adresu, priključak i vjerodajnice
        - Ako koristite proxy poslužitelj treba ste već dopustili sljedeći URL-ovi:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Vatrozid](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. U **Provjeri preduvjeti** postavljanje pokreće provjeru da biste provjerili možete pokrenuti instalaciju. 

    
    ![Preduvjeti](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Ako se pojavi upozorenje o **Provjeri sinkronizacija globalni vremena** koje vrijeme na sat sustava (postavke**datuma i vremena** ) provjerite je li isto kao vremensku zonu.

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. U **Konfiguraciji MySQL** stvorite vjerodajnice za prijavu na instancu MySQL poslužitelja koji će se instalirati.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. **Detalji o okruženju** odaberite li namjeravate replicirati VMware VMs. Ako ste, zatim postavljanje provjerava je li instaliran PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. **Instalacija mjesto** odaberite mjesto na koje želite instalirati na binarne datoteke i spremanje predmemoriju. Možete odabrati na pogon koji ima najmanje 5 GB prostora za pohranu dostupna, no preporučujemo da pogon predmemorije s najmanje 600 GB slobodnog prostora.

    ![Mjesto instalacije](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. U **Odabir mreže** navedite ga slušatelj (mrežnog prilagodnika i SSL priključak) na kojem poslužitelj za konfiguraciju će slanje i primanje replikacije podataka. Možete promijeniti zadani priključak (9443). Uz ovaj priključak priključak 443 koristit će web-poslužitelju koji orchestrates replikacije operacije. 443 ne bi trebalo koristiti za primanje replikacije prometa.


    ![Odabir mreže](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  U **sažetku** pregledajte podatke, a zatim kliknite **Instaliraj**. Kada se dovrši instalacija generira se pristupni izraz. Morat ćete ga kada omogućite replikacije tako da kopirate i zadržavanje na sigurnom mjestu.

    ![Sažetak](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  U **sažetku** pregledajte informacije.

    ![Sažetak](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Microsoft Azure oporavak servisnog agenta proxy mora biti postavljanje.
>Nakon dovršetka instalacije pokrenite aplikaciju pod nazivom "Microsoft Azure oporavak usluge ljuske" na izborniku Start sustava Windows. U prozoru naredba koji se otvara pokrenite sljedeće skupa naredbi za postavljanje postavke proxy poslužitelja.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>Pokrenite instalacijski program iz naredbenog retka

Možete i pokrenuti čarobnjak za Sjedinjeno komuniciranje iz naredbenog retka na sljedeći način:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Pri čemu je:

- / ServerMode: obavezno. Određuje je li instalaciju trebate li instalirati postupak i konfiguraciji poslužitelja ili samo poslužitelj za postupak (koja se koristi da biste instalirali dodatne postupak poslužitelji). Unos vrijednosti: CS, PS.
- InstallDrive: obavezno. Navodi mapu u kojoj su instalirani komponente.
- / MySQLCredFilePath. Obavezno. Navodi put do datoteke koju su vjerodajnice za poslužitelj MySQL priče. Pronađite predložak da biste stvorili datoteku.
- / VaultCredFilePath. Obavezno. Mjesto datoteke sigurnog vjerodajnice
- / EnvType. Obavezno. Vrste instalacije. Vrijednosti: VMware, NonVMware
- / PSIP i /CSIP. Obavezno. IP adresa poslužitelja postupak i konfiguraciji poslužitelja.
- / PassphraseFilePath. Obavezno. Mjesto datoteke pristupni izraz.
- / ByPassProxy. Neobavezno. Određuje poslužitelj za upravljanje povezuje s Azure bez proxy poslužitelj.
- / ProxySettingsFilePath. Neobavezno. Određuje postavke za prilagođene proxy (zadani proxy na poslužitelju koji zahtijeva provjeru autentičnosti) ili prilagođeni proxy poslužitelja




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Korak 6: Postavljanje vjerodajnice za poslužitelj vCenter

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

Poslužitelj za postupak mogu automatski otkriti VMs VMware kojima upravlja vCenter poslužitelja. Za automatsko otkrivanje oporavak web-mjesta mora poslovnog subjekta i vjerodajnice pomoću kojih možete pristupiti poslužitelju vCenter. Nije važno ako ste replikaciju fizičke poslužiteljima.

To na sljedeći način:

1. Na na vCenter poslužitelj stvorite uloga (**Azure_Site_Recovery**) na razini vCenter s [potrebne dozvole](#vmware-permissions-for-vcenter-access).
2. Dodjeljivanje uloge **Azure_Site_Recovery** vCenter korisniku.

    >[AZURE.NOTE] VCenter korisničkog računa koji ima ulogu samo za čitanje možete pokrenuti prebacivanje bez isključuje strojeva zaštićeni izvora. Ako želite isključiti tih strojeva morat ćete Azure_Site_Recovery uloge. Imajte na umu da ako koristite samo Migracija VMs s VMware Azure, a ne morate failback zatim ulogu samo za čitanje potrebne.

3. Da biste dodali račun otvorite **cspsconfigtool**. Nije dostupan prečac na radnoj površini i nalazi u mapi \home\svsystems\bin [instalirati mjesto].
2. na kartici **Upravljanje računima** kliknite **Dodaj račun**.

    ![Dodavanje računa](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. U **Pojedinosti o računu** dodajte vjerodajnica koje je moguće koristiti za pristup poslužitelju vCenter. Imajte na umu da može potrajati više od 15 minuta da se pojavi na portalu, kliknite naziv računa. Da biste ažurirali odmah, kliknite Osvježi na kartici **Poslužitelji konfiguracije** .

    ![Pojedinosti](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Korak 7: Dodavanje vCenter poslužitelji i ESXi domaćini

Ako ste replikaciju VMware VMs morate dodati vCenter server (ili ESXi glavnog računala).

1. Na **poslužiteljima** > **Konfiguraciju poslužitelja** kartice, odaberite poslužitelj za konfiguraciju > **Dodaj vCenter poslužitelja**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. Dodajte vCenter poslužitelja ili pojedinosti ESXi glavno računalo, a zatim naziv računa koje ste naveli za pristup vCenter poslužitelja u prethodnom koraku, a poslužitelj za postupak koji će se koristiti da biste otkrili VMs VMware kojima upravlja vCenter poslužitelja. Imajte na umu da vCenter poslužitelja ili ESXi glavno treba nalaziti u istoj mreži kao poslužitelja na kojem je instaliran poslužitelj za postupak.

    >[AZURE.NOTE] Ako dodajete vCenter poslužitelja ili ESXi glavnog računala s računom koji ne sadrži administratorske ovlasti na poslužitelju vCenter ili glavno računalo, provjerite je li u vCenter ili ESXi računi imati ovlasti omogućen: podatkovnog centra, Datastore, mapu, Jost, mreža, resursa, virtualne na računalu, vSphere raspodijeljenom promjenu. Uz to vCenter poslužitelja mora prikazi ovlasti za pohranu.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Nakon dovršetka otkrivanja poslužitelja vCenter će se prikazati na kartici **Poslužitelji konfiguracije** .

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>Korak 8: Stvaranje grupe za zaštitu

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Zaštita grupe su logička grupiranja virtualnim strojevima ili fizičke poslužiteljima koji želite zaštititi pomoću istih postavki zaštite. Primjena postavki zaštite zaštitu grupu, a te postavke primjenjuju se na svim računalima virtualnim strojevima/fizičke koje dodate u grupu.

1. Otvaranje **Stavki zaštićena** > **Zaštitu grupu** i kliknite da biste dodali zaštitu grupu.

    ![Stvaranje grupe za zaštitu](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. Na stranici s **Postavkama grupe za zaštitu odredite** Navedite naziv za grupu, a zatim **iz** odaberite poslužitelj za konfiguraciju na kojem želite stvoriti grupu. **Cilj** je Azure.

    ![Zaštita od postavki grupe](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. Na stranici **Postavke za određivanje replikacije** konfigurirati postavke replikacije koja će se koristiti za sve strojeva u grupi.

    ![Zaštita grupe replikacije](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Višestruki VM dosljednost**: Ako to uključite stvara zajedničke aplikacije dosljedan oporavak upućuje na računalima u grupi zaštita. Ta je postavka najrelevantnije kada sve strojeva u grupi zaštita koriste isti radno opterećenje. Svim računalima će se oporaviti iste točke podataka. Ovo je dostupna hoće li se replikaciju VMware VMs ili fizičke poslužitelji Windows/Linux.
    - **Prag RPO**: postavlja na RPO. Upozorenja će biti koji su generirani prilikom zaštite replikacije neprekinute podatke premašuje konfigurirano RPO vrijednost praga.
    - **Oporavak pokažite zadržavanja**: određuje zadržavanja prozora. Zaštićeni strojeva se može oporaviti postavite u tom prozoru.
    - **Aplikacija dosljedan učestalost snimke**: određuje koliko se često oporavak točke koja sadrži aplikaciju dosljedan snimke stvorit će se.

Prilikom klika na kvačicu grupu zaštita će biti stvorena pod nazivom koje ste naveli. In addition se stvara druga grupa zaštitu s nazivom < zaštitu-grupa-naziv-Failback). Ova grupa zaštitu koristi se ako natrag na lokalno mjesto nakon prebacivanje na Azure. Zaštita grupe možete pratiti kao što su ste stvorili na stranici **Zaštićeni stavke** .

## <a name="step-9-install-the-mobility-service"></a>Korak 9: Instalirajte servis za mobilnost

U prvom koraku s omogućivanjem zaštitu virtualnim strojevima i fizičke poslužitelje je instalirati mobilnost servis. To možete učiniti na dva načina:

- Automatski automatske i instalirajte servis na svakom računalu s poslužitelja postupak. Imajte na umu da kada dodate strojeva zaštitu grupi i takvi stupci već postoje odgovarajući verziju automatske instalacije servisa mobilnost neće se pojaviti.
- Automatski instalirati servisa pomoću načina automatske enterprise kao što su WSUS ili Upravitelj konfiguracije centar sustava. Provjerite je li ste postavili poslužitelju za upravljanje to učiniti.
- Ručno instalirati na svakom računalu koje želite zaštititi. ake sigurni ste postavili poslužitelju za upravljanje to učiniti.


### <a name="install-the-mobility-service-with-push-installation"></a>Instalirajte servis mobilnost s instalacijom pribadače

Kada dodate strojeva grupi zaštita servis mobilnost se automatski pomiču i na svakom računalu instaliran poslužitelj za postupak.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Priprema za automatsko slanje na računalima sustava Windows

Evo kako pripremiti strojevima sa sustavom Windows tako da servis za mobilnost mogu automatski instalirati web-poslužitelj za postupak.

1.  Stvaranje računa koji se mogu koristiti poslužitelj za postupak da biste pristupili računalu. Račun treba imati ovlasti administratora (Lokalna ili domene). Imajte na umu da se koristi te vjerodajnice samo za instalaciju automatske mobilnost servisa.

    >[AZURE.NOTE] Ako ne koristite račun domene morat ćete onemogućiti kontrolu za daljinski pristup korisnika na lokalnom računalu. Da biste to učinili, u dnevniku u odjeljku HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System dodajte DWORD unosu vrijednost 1 LocalAccountTokenFilterPolicy u odjeljku. Da biste dodali registar stavku iz programa EŽA otvorite naredbe ili pomoću komponente PowerShell unesite **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Na Vatrozid za Windows na ovom računalu želite zaštititi, odaberite **Dopusti aplikacije ili značajku u vatrozidu** i Omogući **dijeljenje datoteka i pisača** i **WMI**. Za strojeva koji pripadaju domene možete konfigurirati pravila vatrozida s GPO.

    ![Postavke vatrozida](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. Dodavanje računa koji ste stvorili:

    - Otvorite **cspsconfigtool**. Nije dostupan prečac na radnoj površini i nalazi u mapi \home\svsystems\bin [instalirati mjesto].
    - Na kartici **Upravljanje računima** kliknite **Dodaj račun**.
    - Dodajte račun koji ste stvorili. Nakon dodavanja računa morat ćete unesite vjerodajnice prilikom dodavanja stroj grupu zaštitu.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Priprema za automatsko slanje na poslužiteljima Linux

1.  Provjerite je li računalo Linux koji želite zaštititi podržana kao što je opisano u [Lokalni preduvjeti](#on-premises-prerequisites). Provjerite je li između na računalu na koje želite zaštititi postoji veza s mrežom i poslužitelj za upravljanje koji pokreće postupak poslužitelja.

2.  Stvaranje računa koji se mogu koristiti poslužitelj za postupak da biste pristupili računalu. Račun mora biti korijenski korisnika na s izvorišnim poslužiteljem Linux. Imajte na umu da se koristi te vjerodajnice samo za instalaciju automatske mobilnost servisa.

    - Otvorite **cspsconfigtool**. Nije dostupan prečac na radnoj površini i nalazi u mapi \home\svsystems\bin [instalirati mjesto].
    - Na kartici **Upravljanje računima** kliknite **Dodaj račun**.
    - Dodajte račun koji ste stvorili. Nakon dodavanja računa morat ćete unesite vjerodajnice prilikom dodavanja stroj grupu zaštitu.

3.  Provjerite da datoteku /etc/hosts na s izvorišnim poslužiteljem Linux sadrži stavke koje mapirati lokalni naziv glavnog računala na IP adrese pridružene sve mrežnih prilagodnika.
4.  Instalirajte najnovije openssh, openssh server, openssl paketa na računalu na koje želite zaštititi.
5.  Provjerite je li SSH i pokrenut na priključak 22.
6.  Omogući SFTP podsustav i lozinku za provjeru autentičnosti u datoteci sshd_config na sljedeći način:

    - Prijavite se kao korijen.
    - U datoteci /etc/ssh/sshd_config datoteke, pronađite redak koji započinje PasswordAuthentication.
    - Uklonite redak i promijenite vrijednost iz **ne** u **da**.
    - Pronađite redak koji započinje **podsustav** , a zatim uklonite redak.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Ručno instalirajte servis za mobilnost

Za instalaciju dostupne su u C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

Izvorni operacijski sustav | Mobilnost servisa instalacijsku datoteku
--- | ---
Windows Server (samo 64-bitni) | Microsoft ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6,4, 6.5, 6.6 (samo 64-bitni) | Microsoft ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (samo 64-bitni) | Microsoft ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6,4, 6.5 (samo 64-bitni) | Microsoft ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>Ručno instalirati na Windows server


1. Preuzmite i pokrenuti odgovarajuću instalacijski program.
2. **Prije nego što počnete **odaberite **servis za mobilnost**.

    ![Servis za mobilnost](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. U **Detalji o poslužitelju za konfiguraciju** navedite IP adresu poslužitelja za upravljanje i pristupni izraz koja je stvorena prilikom instalacije komponente poslužitelj za upravljanje. Pristupni izraz možete dohvatiti tako da pokrenete: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na poslužitelju za upravljanje.

    ![Servis za mobilnost](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. **Instalacija** mjesto ostavite zadano mjesto, a zatim kliknite **Dalje** da biste započeli instalaciju.
5. U **Tijeku instalacije** nadzor instalacije i ponovno pokrenite računalo ako se to od vas zatraži.

Možete instalirati iz naredbenog retka:

UnifiedAgent.exe [/ uloga < Agent na MasterTarget >] [/ InstallLocation <Installation Directory>] [/ CSIP <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>] [/ LogFilePath <Log File Path>]

Pri čemu je:

- / Uloga: obavezno. Određuje je li moguće instalirati mobilnost servisa.
- / InstallLocation: obavezno. Određuje mjesto za instalaciju servis.
- / PassphraseFilePath: obavezno. Određuje pristupni izraz za konfiguraciju poslužitelja.
- / LogFilePath: obavezno. Određuje mjesto datoteke za postavljanje zapisnika

#### <a name="uninstall-mobility-service-manually"></a>Ručna Deinstalacija servisa mobilnost

Servis za mobilnost možete deinstalirati pomoću Dodavanje uklanjanje programa na upravljačkoj ploči ili pomoću naredbenog retka.

Naredba da biste deinstalirali servisa mobilnost putem naredbenog retka

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Izmjena IP adresu poslužitelja za upravljanje

Kada pokrenete čarobnjak možete izmijeniti IP adresu poslužitelja za upravljanje na sljedeći način:

1. Otvorite datoteku hostconfig.exe (koja se nalazi na radnoj površini).
2. Na kartici **globalnih** možete promijeniti IP adresu poslužitelja za upravljanje.

    >[AZURE.NOTE] Promijenite samo IP adresu poslužitelja za upravljanje. Broj priključka za upravljanje poslužitelja komunikacije mora biti 443 i korištenje HTTPS lijevo želite omogućiti. Pristupni izraz ne može mijenjati.

    ![IP adresa za upravljanje poslužitelja](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Ručno instalirati na poslužitelju Linux:

1. Kopirajte u arhivu odgovarajuće tar temeljen na tablici iznad stroj Linux koji želite zaštititi.
2. Otvorite program ljuske i izdvojiti zipane tar arhiva za lokalni put tako da pokrenete:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Stvaranje datoteke passphrase.txt u lokalnom direktoriju koji ste izdvojili sadržaj u arhivu tar. Kako to kopirajte pristupni izraz iz C:\ProgramData\Microsoft Azure web-mjesta Recovery\private\connection.passphrase na poslužitelju za upravljanje, a zatim ga spremite u passphrase.txt tako da pokrenete *`echo <passphrase> >passphrase.txt`* u ljusci.
4. Instalirajte servis za mobilnost unosom *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Odredite internu IP adresu poslužitelja za upravljanje i provjerite je li priključak 443.

**Možete instalirati iz naredbenog retka**:

1. Kopirajte pristupni izraz iz C:\Program Files (x86) \InMage Systems\private\connection na poslužitelju za upravljanje, a zatim Spremi kao "passphrase.txt" na poslužitelju za upravljanje. Pokrenite te naredbe. U našem primjeru IP adresu poslužitelja za upravljanje je 104.40.75.37 i mora biti HTTPS priključak 443:

Da biste instalirali na poslužitelju proizvodnje:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Da biste instalirali na glavni ciljnom poslužitelju:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Korak 10: Omogući zaštitu za računalo

Da biste omogućili zaštitu virtualnim strojevima i dodate fizičke poslužitelji u grupu zaštitu. Prije nego što počnete, imajte na umu sljedeće ako ste zaštita VMware virtualnim strojevima:

- VMware VMs slučaju otkrivanja svakih 15 minuta, a to može potrajati više od 15 minuta da se prikazuju na portalu za oporavak web-mjesta nakon otkrivanje.
- Okruženje za promjene na virtualnog računala (kao što je instalacija Alati VMware) i može potrajati više od 15 minuta ažurirati u oporavak web-mjesta.
- Možete potražiti na zadnji put otkriven za VMware VMs u polje **Zadnjeg kontakt pri** glavnog računala poslužitelja/ESXi vCenter na kartici **Poslužitelji konfiguracije** .
- Ako ste već stvorili grupu za zaštitu i dodavanje vCenter poslužitelja ili ESXi host nakon toga, možda je potrebno više od 15 minuta za portal za oporavak Azure web-mjesta da biste osvježili i virtualnim strojevima navoditi u dijaloškom okviru **Dodaj strojeva grupi zaštita** .
- Ako želite odmah nastavite s dodavanjem strojeva grupi zaštita bez čekati zakazano otkrivanje, isticanje poslužitelj za konfiguraciju (nemojte kliknuti ga) i kliknite gumb **Osvježi** .

Osim toga Imajte na umu da:

- Preporučujemo da vam mijenjanje arhitekture grupe zaštitu da bi se Zrcalili vaše radnih opterećenja. Na primjer dodati računala izvode određenu aplikaciju istoj grupi.
- Kada dodate strojeva grupi Zaštita, poslužitelj za postupak automatski ih gura i instalira servis mobilnost ako već nije instaliran. Imajte na umu da morate imati mehanizam automatske Priprema opisan u prethodnom koraku.


Dodavanje strojeva grupi zaštita:

1. Kliknite **zaštićeni stavke** > **CORBIS** > **strojeva** > Dodavanje računala. \As preporučenim načinom rada
2. U **Odaberite virtualnim strojevima** ako ste zaštite VMware virtualnim strojevima, odaberite poslužitelj vCenter za upravljanje virtualnim strojevima (ili EXSi glavnog računala na kojem se izvodi), a zatim strojeva.

    ![Omogući zaštitu](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  U **Odaberite virtualnim strojevima** ako ste zaštite fizičke poslužitelji, u čarobnjaku za **Dodavanje fizičke strojeva** navedite IP adresa i neslužbeni naziv. Zatim odaberite liniji operacijski sustav.

    ![Omogući zaštitu](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. **Određivanje ciljnog** resursa odaberite račun za pohranu koristite za replikaciju te odaberite li postavke želite koristiti za sve radnih opterećenja. Imajte na umu račune za pohranu premium trenutno nisu podržani.

    >[AZURE.NOTE] 1. Ne podržavamo Premjesti račune za pohranu stvoren pomoću [Novi Azure portal](../storage/storage-create-storage-account.md) preko grupe resursa.                           2.[migracije računa za pohranu](../resource-group-move-resources.md) po grupama resursa unutar iste pretplate ili uzduž pretplate nije podržana za račune za pohranu koji se koriste za implementaciju oporavak web-mjesta.

    ![Omogući zaštitu](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. **Određivanje računa** odaberite račun koji želite koristiti za automatsku instalaciju servisa mobilnost [konfiguriran](#install-the-mobility-service-with-push-installation) .

    ![Omogući zaštitu](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Kliknite kvačicu da biste završili dodavanje strojeva grupi Zaštita i pokrenuli početne replikacije na svakom računalu.

    >[AZURE.NOTE] Ako automatske instalacije sadrži je pripremiti mobilnost servisa automatski se instalira na računalima koji nemaju je kako se dodaju u grupu zaštitu. Servis nakon instalacije zaštitu posao pokreće i neće uspjeti. Nakon neuspjeh morat ćete ponovno pokrenuti ručno svaki stroju koji su se pojavili usluga mobilnost instalirana. Zaštita posao počinje nakon ponovnog pokretanja ponovno i pojavljuje se početni replikacije.

Možete nadzirati status na stranici **Zadaci** .

![Omogući zaštitu](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Uz to, moguće nadzirati status zaštite **Zaštićeni stavki**programa > <protection group name> > **virtualnih računala**. Nakon početnog replikacije dovršava i sinkronizacije podataka, strojno status će se mijenja u** zaštićen**.

![Omogući zaštitu](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>11 korak: Postavljanje zaštićena svojstva računala

1. Kada na računalu sa stanjem **zaštićenog** možete konfigurirati svojstva prebacivanje. U zaštitu Detalji grupe odaberite računalo i otvorite karticu **Konfiguracija** .
2. Oporavak web-mjesta automatski predlaže svojstva za Azure VM i otkrije na lokalne mreže postavke.

    ![Postavite svojstva virtualnog računala](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. Promijenite sljedeće postavke:

    -  **Naziv Azure VM**: to je naziv koji će biti odobren na računalo u Azure nakon prebacivanje. Naziv mora biti usklađene sa Azure preduvjeti.

    -  **Veličina Azure VM**: broj mrežnih prilagodnika diktirali je veličinom navedete ciljne virtualnog računala. [Dodatne informacije potražite u](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) veličine i prilagodnika. Imajte na umu da:

        - Kada mijenjati veličinu virtualnog računala i spremili postavke, broj mrežnog prilagodnika će se promijeniti kada otvorite karticu **Konfiguracija** sljedeći put. Najmanji broj mrežnih prilagodnika na izvor virtualnog računala i najveći broj mrežnih prilagodnika podržava veličina virtualnog računala odabrali je broj mrežnih prilagodnika cilj virtualnih računala.
            - Ako je broj mrežnih prilagodnika na računalu izvor manje od ili jednak broju prilagodnika dopušteno Odredišna veličina računalu, zatim cilj će imati isti broj prilagodnika kao izvor.
            - Ako broj prilagodnika za virtualnog računala izvora premašuje broj dopušten za Odredišna veličina, a zatim Maksimalna veličina ciljne će se koristiti.
            - Ako, primjerice stroj izvora sadrži dvije mrežnih prilagodnika i odredišna veličina računalo podržava četiri, ciljnom računalu će imati dva prilagodnika. Ako na računalu izvor ima dva prilagodnika, ali veličinu podržani cilj podržava samo jedan ciljnom računalu neće imati samo jedan prilagodnika.
        - Ako je virtualnog računala većeg broja mrežnih prilagodnika sve prilagodnika treba povezani s istom Azure.
    - **Azure mreže**: Navedite Azure mreže koji Azure VMs će se povezati s nakon prebacivanje. Ako ne navedete jednu Azure VMs neće povezani s mreže. Uz to morate navesti Azure mreže ako želite failback s Azure web-mjesto lokalnog. Failback zahtijeva VPN vezu između Azure mreže i lokalne mreže.
    - **Azure IP adresa/podmreže**: za svaku mrežnog prilagodnika odaberete podmreže na koju treba povezivanje Azure VM. Imajte na umu da:
        - Ako mrežnog prilagodnika na ovom računalu izvora je konfiguriran za korištenje statičke IP adrese možete odrediti statičke IP adrese za Azure VM. Ako ne unesete statičke IP adrese pa dostupna IP adresa će se dodijeliti. Ako je naveden IP adresu cilja, ali je već koristi drugi VM u Azure zatim prebacivanje neće uspjeti. Ako je mrežnog prilagodnika izvornog računala konfiguriran za korištenje DHCP ćete imati DHCP kao postavku za Azure.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Korak 12: Stvaranje plana oporavak i pokretanje programa prebacivanje

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

Pokretanje prebacivanje na jednom računalu ili neće uspjeti preko više virtualnim strojevima koji se izvoditi isti zadatak ili pokrenuti isti radno opterećenje. Neuspjeh na više računala istovremeno dodavanje tarifu za oporavak.

### <a name="create-a-recovery-plan"></a>Stvaranje plana oporavak

1. Na stranici **Tarife za oporavak** kliknite **Dodavanje Plan za oporavak** i dodali plan za oporavak. Navedite detalje za plan, a zatim odaberite **Azure** kao cilj.

    ![Konfiguriranje oporavak plan](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. U **Odaberite virtualnog računala** odaberite grupu za zaštitu, a zatim odaberite strojeva u grupu koju želite dodati tarifu za oporavak.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Možete prilagoditi plan za stvaranje grupe i slijed redoslijed kojim strojeva u planu oporavak su nije uspjela iznad. Možete dodati i skripte i upute za ručno akcije. Skripte mogu se kreirati ručno ili pomoću [Runbooks Automatizacija Azure](site-recovery-runbook-automation.md). [Dodatne informacije](site-recovery-create-recovery-plans.md) o prilagodbi tarife za oporavak.

## <a name="run-a-failover"></a>Pokretanje programa prebacivanje

Prije nego što pokrenete u prebacivanje Imajte na umu da:


- Provjerite je li poslužitelj za upravljanje sustavom i dostupan - u suprotnom prebacivanje neće uspjeti.
- Ako naiđete na bilješke neplanirano prebacivanje koji:

    - Ako je to moguće isključite primarni strojeva prije pokretanja neplanirano prebacivanje. Na taj način nemate izvora i replike računala izvode u isto vrijeme. Ako ste replikaciju VMware VMs pa kada pokrenete neplanirano prebacivanje možete odrediti oporavak web-mjesta trebali napraviti najbolje trud isključiti strojeva izvora. Ovisno o stanju primarni web-mjesta to može ili možda neće funkcionirati. Ako ste replikaciju fizičke poslužitelje oporavak web-mjesta ne nude tu mogućnost.
    - Prilikom izvršavanja neplanirano prebacivanje prestane replikacije podataka iz primarnog strojeva tako da sve delta podataka neće prenijeti nakon neplanirano prebacivanje započinje.

- Ako se želite povezati virtualnog računala replike u Azure nakon prebacivanje omogućiti udaljene radne površine na računalu izvora prije naiđete na prebacivanje te omogućuje RDP vezu kroz vatrozid. Također morate dopustiti RDP na javno krajnjoj točci Azure virtualnog računala nakon prebacivanje. Slijedite ove [najbolje prakse](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) da biste bili sigurni da RDP radi nakon na prebacivanje.

>[AZURE.NOTE] Da biste ostvarili najbolje performanse kada to učinite na prebacivanje Azure, provjerite je li Provjerite jeste li instalirali Agent za Azure u zaštićenom računala. To pomaže u pokretanje brže i pomaže u Dijagnostika u slučaju problema. Agent za Linux mogu biti pronađene [ovdje](https://github.com/Azure/WALinuxAgent) - i Windows agent nalazi se [ovdje](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

Pokrenite test prebacivanje u programu Publisher prebacivanje i oporavak procesa u Izolirani mreži koje ne utječu na radnom okruženju i obične replikacije nastavlja uobičajen. Započinje testiranje prebacivanje na izvoru i pokrenete na nekoliko načina:

- **Ne navedete Azure mreže**: Ako pokrenite test prebacivanje mreže test se jednostavno provjerite jesu li virtualnim strojevima start i prikazuju ispravno u Azure. Virtualnim strojevima neće biti povezani s mrežom Azure nakon prebacivanje.
- **Navedite Azure mreže**: ovu vrstu prebacivanje provjerava okruženje za cijelu replikacije dolazi do prema očekivanjima i Azure virtualnim strojevima povezani u navedenu mrežu.


1. Na stranici **Oporavak tarife** odaberite tarifu, a zatim kliknite **Testiraj prebacivanje**.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. U **Potvrdi testiranje prebacivanje** odaberite **ništa** da biste naznačili ne želite koristiti Azure mreže za prebacivanje test ili odaberite mrežu na kojoj će se povezati test VMs nakon prebacivanje. Kliknite kvačicu da biste započeli s pomoćnim.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. Praćenje napretka prebacivanje na kartici **Zadaci** .

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. Nakon dovršetka na prebacivanje i trebali da biste vidjeli replike Azure računalu prikazuju Azure portalu > **virtualnim računalima**. Ako želite da biste započeli RDP veza za Azure VM morat ćete otvoriti priključak 3389 na krajnjoj točki VM.

5. Kada ste završili, kada prebacivanje dosegne dovršetak testiranje fazu, kliknite dovršeno Testiraj da biste završili. U bilješkama snimiti i spremiti sve opažanja povezan s pomoćnim test.

6. Kliknite **test prebacivanje dovršetka** automatski očistiti okruženje za testiranje. Po završetku to prebacivanje test prikazat će se status **Dovršeno** . Elementi ni VMs automatski stvara tijekom prebacivanje test se briše. Imajte na umu da ako testiranje prebacivanje više od dva tjedna nastavlja se dovrši po prisilno.



### <a name="run-an-unplanned-failover"></a>Pokretanje neplanirano prebacivanje

Neplanirano prebacivanje pokrene iz Azure i može izvršiti čak i ako se primarni web-mjesta nije dostupna.


1. Na stranici **Oporavak tarife** odaberite tarifu i kliknite **Prebacivanje** > **Neplanirano prebacivanje**.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Ako ste replikaciju VMware virtualnim strojevima možete odabrati da biste se pokušali, u kojem možete isključiti lokalne VMs. Ovo je najbolje trud i prebacivanje će i dalje li truda uspješnog ili ne. Ako ga ne uspije detalje o pogrešci pojavit će se na kartici **Zadaci **> **Neplanirano Prebacivanje zadataka**.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] Ta mogućnost nije dostupna ako ste replikaciju fizičke poslužiteljima. Morat ćete ga možete isprobati i isključite one ručno ako je to moguće.

3. U **Potvrdi prebacivanje** provjerite je li prebacivanje smjera (Azure), a zatim odaberite točke oporavak koji želite koristiti za na prebacivanje. Ako je omogućeno više VM kada konfigurira svojstva replikacije možete oporaviti najnoviju aplikaciju ili rušenje dosljedan oporavak točku. Možete odabrati i **oporavak Prilagođeno pokažite** da biste oporavili ranije točke u vremenu. Kliknite kvačicu da biste započeli s pomoćnim.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Pričekajte dovršetak posla neplanirano prebacivanje. Možete pratiti napredak prebacivanje na kartici **Zadaci** . Imajte na umu da čak i ako dođe do pogreške tijekom neplanirano prebacivanje plan za oporavak izvodi sve dok ne bude dovršena. Se i moći vidjeti replike Azure računalu prikazuju se na virtualnim računalima sustava na portalu za Azure.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Povezivanje s replicirati Azure virtualnim strojevima nakon prebacivanje

Da bi se povezati s replicirati virtualnim strojevima u Azure nakon ovdje prebacivanje bit će vam potrebno:

1. Na računalu primarni želite omogućiti veze udaljene radne površine.
2. Vatrozid za Windows na primarni računalu da biste dopustili RDP.
3. Nakon prebacivanje morat ćete dodati RDP javno krajnja točka za Azure virtualnog računala.

[Dodatne informacije potražite u](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) o postavljanju.


## <a name="deploy-additional-process-servers"></a>Implementacija poslužitelja dodatne postupak

Ako imate za promjenu veličine izlaz uvođenja osim 200 strojeva izvora ili ukupni dnevni churn stopa prelazi 2 TB, morat ćete dodatne postupak poslužitelji za rukovanje glasnoće promet. Da biste postavili poslužitelj za dodatne postupak pročitajte preduvjete na [poslužiteljima dodatne proces](#additional-process-servers) , a zatim slijedite upute ovdje da biste postavili poslužitelj za postupak. Nakon postavljanja poslužitelj možete konfigurirati izvor strojeva ga koristiti.

### <a name="set-up-an-additional-process-server"></a>Postavljanje poslužitelj za dodatne postupak

Postavite poslužitelj za dodatne postupak na sljedeći način:

- Pokrenite čarobnjak za Sjedinjeno komuniciranje da biste konfigurirali poslužitelj za upravljanje kao samo poslužitelj postupak.
- Ako želite upravljati replikacije podataka pomoću samo novi postupak poslužitelj morat ćete migrirati zaštićeni strojevima sa sustavom da biste to učinili.

### <a name="install-the-process-server"></a>Instalacija poslužitelj za postupak

1. Na stranici za brzi početak rada preuzmite objedinjenih instalacijsku datoteku za oporavak web-mjesta komponente instalaciju. Pokrenite instalaciju.
2. **Prije nego što počnete** odaberite **Dodaj dodatne postupak poslužitelji za promjenu veličine out implementacije**.

    ![Dodavanje postupak poslužitelja](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. Dovršite čarobnjak na isti način kao i kada niste [postavili](#step-5-install-the-management-server) prvi poslužitelj za upravljanje.

4. U **Detalji o poslužitelju za konfiguraciju** navedite IP adresu izvorni poslužitelj za upravljanje na kojem je instaliran poslužitelj za konfiguraciju i pristupni izraz. Izvorni pokrenuti poslužitelju za upravljanje ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** da biste dobili pristupni izraz.

    ![Dodavanje postupak poslužitelja](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migriranje strojeva koji koriste novi poslužitelj za postupak

1. Otvorite **Konfiguraciju poslužitelja** > **poslužitelja** > naziv izvorni poslužitelj za upravljanje > **Detalji o poslužitelju**.

    ![Ažuriranje poslužitelja postupak](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. Na popisu **Postupak poslužitelji** kliknite **Promijeni postupak poslužitelja** pokraj poslužitelja koji želite izmijeniti.

    ![Ažuriranje poslužitelja postupak](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. **Promjena postupak**Server > **Ciljani postupak poslužitelj** odaberite novi poslužitelj za upravljanje, a zatim odaberite novi poslužitelj za postupak će obrađivati virtualnim računalima sustava. Kliknite ikonu informacije da biste dobili informacije o poslužitelju. Prosječna prostora koji je potreban za replikaciju svaki odabrani virtualnog računala da biste novi poslužitelj za postupak prikazuje se da bi vam učitavanje odluka. Kliknite kvačicu da biste pokrenuli replikaciju novi poslužitelj za postupak.

    ![Ažuriranje poslužitelja postupak](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>VMware dozvole za pristup vCenter

Poslužitelj za postupak mogu automatski otkriti VMs na poslužitelju vCenter. Da biste izvršili automatsko otkrivanje morate definirati ulogu (Azure_Site_Recovery) na razini vCenter da biste omogućili oporavak web-mjesta za pristup poslužitelju vCenter. Imajte na umu da ako samo želite migrirati VMware strojeva Azure i ne morate failback iz Azure, možete definirati samo za čitanje ulozi koja je dovoljno. Postavite dozvole kao što je opisano u [korak 6: postavljanje vjerodajnice za poslužitelj vCenter](#step-6-set-up-credentials-for-the-vcenter-server) dozvole za uloge su navedene u tablici u nastavku.

**Uloga** | **Pojedinosti** | **Dozvole**
--- | --- | ---
Azure_Site_Recovery uloga | Otkrivanje VMware VM |Dodjela ovlasti za centar za v poslužitelj:<br/><br/>DataStore -> Alociraj prostor, datastore Pregledaj, niske razine datoteke operacije, datoteku, datoteke ažuriranja virtualnog računala<br/><br/>Mrežni -> Dodjela mreže<br/><br/>Resurs -> Dodjela virtualnog računala da biste skup resursa, migriranje isključeno virtualnog računala, migriranje uključen virtualnog računala<br/><br/>Zadaci -> Stvaranje zadatka, ažuriranje zadatka<br/><br/>Konfiguriranje -> virtualnog računala<br/><br/>Jezici predloška interakcije -> virtualnog računala -> odgovora pitanje, uređaj veze, konfiguriranje CD medija, Konfiguriraj meki medijski sadržaji isključeno, Power na, instalirajte VMware Alati<br/><br/>Zalihe -> virtualnog računala -> neregistriranja stvaranje dnevnika<br/><br/>Virtualnog računala -> Provisioning -> Preuzmi Dopusti virtualnog računala, prijenos datoteka Dopusti virtualnog računala<br/><br/>Virtualnog računala -> snimke -> Ukloni snimke
vCenter korisnička uloga | Otkrivanje VMware VM/prebacivanje bez zatvaranja izvora VM | Dodjela ovlasti za centar za v poslužitelj:<br/><br/>Objekt podatkovnog centra –> Propagate na podređeni objekt uloga = samo za čitanje <br/><br/>Korisnik je dodijeljen razini podatkovnog centra i s podatkovnim centrom ima pristup svim objektima.  Ako želite ograničiti pristup dodijeliti ulogu **bez pristupa** s objektom **Propagate djetetu** podređene objekte (ESX domaćini, datastores, VMs i mreža).
vCenter korisnička uloga | Prebacivanje i failback | Dodjela ovlasti za centar za v poslužitelj:<br/><br/>Objekt podatkovnog centra – Propagate podređeni objekt, uloge = Azure_Site_Recovery<br/><br/>Korisnik je dodijeljen razini podatkovnog centra i s podatkovnim centrom ima pristup svim objektima.  Ako želite ograničiti pristup dodijeliti ulogu **bez pristupa **s **Propagate podređeni objekt** podređeni objekt (ESX domaćini, datastores, VMs i mreža).  



## <a name="third-party-software-notices-and-information"></a>Obavijesti trećih strana softver i podataka

Ne šalji prijevod i Localize

Softver i opremu koji se izvodi u Microsoft proizvoda ili usluga temelji se na ili ugrađuje materijala iz projekata navedene (skupno, "trećih strana kod").  Microsoft se neće izvorni autor šifre drugih proizvođača.  Izvorni autorskim i licence, u odjeljku Microsoft primili takve kod drugih proizvođača postavljene su naprijed ispod.

Informacije u odjeljku A koji se odnosi kod drugih proizvođača komponente s projektima navedena u nastavku. Informativna svrhe su dani takve licence i informacije.  Kod drugih proizvođača je u tijeku relicensed vam Microsoft u odjeljku o licenciranju odredbe za Microsoftov softver Microsoftovih proizvoda ili usluge.  

Informacije u odjeljku B koji se odnosi komponente kod drugih proizvođača koji su se bili dostupni na Microsoft u odjeljku izvorni uvjete licenciranja.

Cijeli dokument nalazi se u [Microsoftovu centru za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft zadržava sva prava izričito ne odobren spominju u ovom dokumentu, po patentnim, estoppel ili neki drugi način.

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o failback](site-recovery-failback-azure-to-vmware-classic.md) da bi se prikazala sustava nije uspjelo putem računala izvode u Azure natrag na lokalnog okruženja.
