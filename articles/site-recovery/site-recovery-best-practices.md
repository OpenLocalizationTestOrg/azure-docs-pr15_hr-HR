<properties
    pageTitle="Priprema za implementaciju web-mjesta oporavak | Microsoft Azure"
    description="U ovom se članku opisuje kako pripremiti za implementaciju replikacije uz oporavak Azure web-mjesta."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Priprema za implementaciju oporavak Azure web-mjesta

Ovaj članak više razine pregled implementacije preduvjeta za svaki scenarij replikacije podržava servisa Azure oporavak web-mjesta. Kada čitate općeniti preduvjeti za svaki scenarij, možete se povezati s određene implementacije pojedinosti svake implementacije.

Nakon čitanje u ovom se članku objavljuju komentare ni pitanja pri dnu članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Pregled

Tvrtke i ustanove moraju strategije BCDR koja određuje kako aplikacija, radnih opterećenja i podataka ostati pokrenut i dostupne tijekom planirane i Neplanirana isključiti, a čim oporaviti normalno funkcionira uvjeta. Strategije BCDR trebali biste zadržali poslovnih podataka sigurnih i koje se mogu vratiti i provjerite je li radnih opterećenja ostaju stalno dostupne kada se pojavi Izrada. 

Oporavak web-mjesta je Azure service doprinosa strategije BCDR po orchestrating replikacije lokalne poslužitelje fizičke i virtualnih računala s oblakom (Azure) ili sekundarni podatkovnog centra. Prilikom kvarove u svom primarnom mjestu, koji se neće putem sekundarne mjesto da bi aplikacija i radnih opterećenja dostupna. Ne natrag na svom primarnom mjestu kada on se vraća na običnom operacije. Dodatne informacije u [oporavak web-mjesta?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Odaberite model za uvođenje

Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model.md) za stvaranje i rad s resursima: model upravljanja resursima Azure i model upravljanja klasičnom usluga. Azure ima dvije portalima – [Azure klasični portal](https://manage.windowsazure.com/) podržava model klasični implementacije i [Azure portal](https://ms.portal.azure.com/) s podrškom za oba modele implementacije.

Oporavak web-mjesta dostupan je u klasični portal i Azure portal. Na portalu za Azure klasični podržavaju oporavak web-mjesta s modelom upravljanja klasičnom usluga. Na portalu za Azure podržavaju klasični modela ili implementacijama Voditelj resursa. [Dodatne informacije potražite u](site-recovery-overview.md#site-recovery-in-the-azure-portal) o implementaciji pomoću portala za Azure.

Kada vam se odabir bilješku model implementacije koji:

- Preporučujemo da koristite [Azure portal](https://portal.azure.com) i resursima modela, gdje je to moguće.
- Oporavak web-mjesta omogućuje u jednostavniji i više intuitivno Uvod sučelje na portalu za Azure.
- Pomoću portala za Azure, možete replicirati strojeva klasični i resursima za pohranom na servisu Azure. Uz to, na portalu za Azure možete koristiti LRS ili GRS račune za pohranu.
- Portal za Azure spaja services oporavak web-mjesta i sigurnosne kopije u jednom oporavak servisa sigurnog tako da možete postaviti i upravljanje uslugama za BCDR s jednog mjesta.
- Korisnici s Azure pretplate dodjeli programom oblaka rješenje davatelj (CSP) sada mogu upravljati operacije oporavak web-mjesta na portalu za Azure.
- Replikaciju VMware VMs ili fizičke strojeva Azure na portalu za Azure nudi brojne nove značajke uključujući podršku za pohranu premium i mogućnost za isključivanje određene diskova iz replikacije.


## <a name="select-your-deployment-scenario"></a>Odaberite scenariju implementacije

**Uvođenje** | **Pojedinosti** | **Portal za Azure** | **Klasični portal** | **PowerShell**
---|---|---|---|---
**VMware VMs za Azure** | Replicirati VMware VMs Azure za pohranu | U na Azure portal VMs uspijeva putem classic ili Voditelj resursa za pohranu<br/><br/> Uvođenje [Azure portal](site-recovery-vmware-to-azure.md) sadrži pojednostavnjeno implementacije sučelje i brojne prednosti značajke. | Na portalu za Azure klasični možete implementirati uz [poboljšani model](site-recovery-vmware-to-azure-classic.md) i se neće putem klasične Azure prostora za pohranu.<br/><br/> Postoji naslijeđene modelu koji se ne bi trebalo koristiti za nove implementacije. |  PowerShell trenutno nije podržano.
**Hyper-V VMs za Azure** | Hyper-V VMs Azure za pohranu. Na Hyper-V domaćini upravlja oblaka VMM centar sustava ili bez VMM može biti VMs. | U na Azure portal VMs uspijeva putem classic ili Voditelj resursa za pohranu.<br/><br/> Portal za Azure sadrži pojednostavnjeno implementacije sučelje. [Saznajte više](site-recovery-vmm-to-azure.md) o replikaciju Hyper-V VMs u VMM oblaka. [Saznajte više](site-recovery-hyper-v-site-to-azure.md) o replikaciju VMs Hyper-V (bez VMM).| Na portalu klasični Azure možete uspjeti VMs putem klasične Azure za pohranu | Podržana je PowerShell implementacije.
**Fizička poslužitelji za Azure** | Replicirati fizičke poslužitelji Windows/Linux Azure za pohranu | U na Azure portala poslužitelje uspijeva putem classic ili Voditelj resursa za pohranu.<br/><br/> Uvođenje [Azure portal](site-recovery-vmware-to-azure.md) nudi iskustvo pojednostavnjeno implementacije i brojne prednosti značajke. | Na portalu za Azure klasični možete implementirati uz [poboljšani model](site-recovery-vmware-to-azure-classic.md) i se neće putem klasične Azure prostora za pohranu.<br/><br/> Postoji naslijeđene modelu koji se ne bi trebalo koristiti za nove implementacije. | PowerShell implementacije trenutno nije podržano.
**VMs fizičke VMware poslužitelji za sekundarnog web-mjesta* | Replicirati VMware VMs ili fizičke poslužitelji Windows/Linux sekundarne web-mjesto. [Dodatne informacije i preuzimanje](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) korisničkom priručniku InMage Scout. | Nije dostupno na portalu za Azure | Na portalu klasični ćete preuzmite InMage Scout iz zbirke ključeva za oporavak web-mjesta. | Uvođenje PowerShell trenutno nije podržano
**Hyper-V VMs sekundarne web-mjesto** | Sekundarni oblak replicirati Hyper-V VMs u VMM oblaka | [Portal za Azure](site-recovery-vmm-to-vmm.md) možete replicirati Hyper-V VMs u VMM oblaka sekundarne web-mjestu samo pomoću značajke Hyper-V replike. SAN trenutno nije podržano. | Na portalu za Azure klasični mogli ponoviti Hyper-V VMs u VMM oblaka sekundarnog web-mjesta pomoću [Značajke Hyper-V replike](site-recovery-vmm-to-vmm-classic.md) ili [SAN replikacije](site-recovery-vmm-san.md) | Podržana je PowerShell implementacije



## <a name="check-what-you-need-for-deployment"></a>Provjerite što vam je potrebno za implementaciju

### <a name="replicate-to-azure"></a>Replicirati Azure

**Preduvjet** | **Pojedinosti** 
---|---
**Račun za Azure** | Potreban vam je račun sustava [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.
**Azure prostora za pohranu** | Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs stvaraju se kada se pojavi prebacivanje. Za replikaciju Azure, morat ćete [račun za Azure prostora za pohranu](../storage/storage-introduction.md).<br/><br/>Ako ste implementacija oporavak web-mjesta na portalu klasični morat ćete neke [standardne račune GRS prostora za pohranu](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Ako ste implementacija na portalu za Azure možete koristiti GRS ili LRS prostora za pohranu.<br/><br/> Ako ste replikaciju VMware VMs ili fizičke poslužitelji u spremište Azure portala premium podržana. Imajte na umu da ako koristite račun za pohranu premium trebat ćete račun standardne prostora za pohranu za spremanje zapisnika replikacije koji obuhvaćaju daljnje promjene lokalnih podataka. [Prostor za pohranu Premium](../storage/storage-premium-storage.md) obično koristi za virtualnim strojevima koje je potrebno dosljedno visoke performanse IO i niske latencije da biste intenzivno radnih opterećenja IO glavnog računala.<br/><br/> Ako želite da biste koristili premium računa da biste pohranili repliciranu podatke, morat ćete i standardne prostora za pohranu računa za spremanje zapisnika replikacije koji obuhvaćaju daljnje promjene lokalnih podataka.
**Azure mreže** | Za replikaciju Azure, morat ćete Azure mreže koji Azure VMs će se povezati s kada ih se stvorene nakon prebacivanje.<br/><br/> Ako ste implementacija na portalu klasični će se koristiti klasične mreže. Ako ste implementirate na portalu za Azure, možete koristiti na klasični ili mreže Voditelj resursa.<br/><br/> Mreža mora biti u području isti kao u zbirke ključeva.
**Preslikavanje (VMM za Azure)** | Ako ste replikaciju iz VMM za Azure, [preslikavanje](site-recovery-network-mapping.md) osigurava da Azure VMs povezani točan mrežama nakon prebacivanje.<br/><br/> Da biste postavili preslikavanje morate konfigurirati VM mrežama na portalu VMM.
**Lokalni** | **VMware VMs**: morat ćete je lokalnog računala radi oporavak web-mjesta komponente, VMware vSphere domaćini/vCenter poslužitelj i VMs želite replicirati. [Dodatne informacije potražite u](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Fizička poslužitelji**: Ako ste replikaciju fizičke poslužitelje morat ćete lokalnim računalima sustava pokretanje komponente oporavak web-mjesta i fizičke poslužitelje želite replicirati. [Dodatne informacije potražite u](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Ako želite [ponovno uspjeti](site-recovery-failback-azure-to-vmware.md) nakon prebacivanje na Azure morat ćete VMware infrastrukture da to učinite.<br/><br/> **Hyper-V VMs**: Ako želite replicirati Hyper-V VMs u VMM oblaka morat ćete VMM poslužitelja, a nalaze Hyper-V domaćini na koji VMs koji želite zaštititi. [Dodatne informacije potražite u](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Ako želite replicirati Hyper-V VMs bez VMM morat ćete Hyper-V domaćini na kojem se nalaze VMs. [Dodatne informacije potražite u](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Zaštićeni računalima** | Zaštićeni strojevima koji će replicirati za Azure mora biti usklađene sa [Azure preduvjeti](#azure-virtual-machine-requirements) opisanim.


### <a name="replicate-to-a-secondary-site"></a>Replikaciju sekundarnu web-mjesta

**Preduvjet** | **Pojedinosti** 
---|---
**Račun za Azure** | Potreban vam je račun sustava [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.
**Lokalni** | **VMware VMs**: primarni web-mjestu morat ćete poslužitelj za postupak za predmemoriranje, sažimanje i šifriranje podataka replikacije prije no što pošaljete sekundarnog web-mjesta. Sekundarni web-mjestu ćete instalirati poslužitelj za konfiguraciju za upravljanje i nadzirati uvođenje i osnovne cilj poslužitelja. Preporučujemo i vContinuum poslužitelja radi lakšeg upravljanja. Osim toga, potreban je jedan ili više domaćini vSphere VMware i po želji vCenter poslužitelja. [Saznajte više](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V VMs u VMM oblaka**: morat ćete postaviti VMM poslužitelji i Hyper-V domaćini koja sadrži VMs želite replicirati. [Dodatne informacije potražite u](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Preslikavanje (VMM za VMM)** | Ako ste replikaciju iz VMM za VMM, [preslikavanje](site-recovery-network-mapping.md) osigurava da VMs replike povezani s odgovarajućim mrežama nakon prebacivanje i optimalnog stavite na Hyper-V domaćini. Da biste postavili preslikavanje morate konfigurirati VM mrežama na poslužiteljima VMM.
**Mapiranje prostora za pohranu** | Ako ste replikaciju VMM za VMM po želji postavljate [Mapiranje prostora za pohranu](site-recovery-storage-mapping.md) za određivanje ciljnog prostora za pohranu repliciranu VMs. Mapiranje prostora za pohranu možete postaviti za replikaciju replike Hyper-V i SAN.<br/><br/> Prostor za pohranu mapiranje trenutno nisu podržane na portalu za Azure.


## <a name="verify-url-access"></a>Provjerite je li pristup URL-a

Provjerite jesu li URL-ove dostupne s poslužitelja.

**URL-A** | **VMM za VMM** | **VMM za Azure** | **Hyper-V tako da Azure** | **VMware za Azure**
---|---|---|---|---
*. accesscontrol.windows.net | Dopusti | Dopusti | Dopusti | Dopusti
*. backup.windowsazure.com | Nije potrebno | Dopusti | Dopusti | Dopusti
*. hypervrecoverymanager.windowsazure.com | Dopusti | Dopusti | Dopusti | Dopusti
*. store.core.windows.net | Dopusti | Dopusti | Dopusti | Dopusti
*. blob.core.windows.net | Nije potrebno | Dopusti | Dopusti | Dopusti
https://www.msftncsi.com/ncsi.txt | Dopusti | Dopusti | Dopusti | Dopusti
https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Nije potrebno | Nije potrebno | Nije potrebno | Dopusti

## <a name="azure-virtual-machine-requirements"></a>Preduvjeti za Azure virtualnog računala

Možete implementirati oporavak web-mjesta za replikaciju virtualnim strojevima i fizičke poslužitelje s programom operacijske sustave podržava Azure. To obuhvaća Većina verzija sustava Windows i Linux. Morat ćete da biste bili sigurni koji lokalnog u skladu s zahtjevima za Azure virtualnim strojevima koji želite zaštititi.


**Značajka** | **Preduvjeti** | **Pojedinosti**
---|---|---
Glavno računalo Hyper-V | Trebao bi biti pokrenut Windows Server 2012 R2 | Preduvjeti potvrdite neće uspjeti ako operacijski sustav nije podržana
VMware hypervisor  | Podržani operacijski sustav | [Provjera preduvjeti](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Goste operacijski sustav | Hyper-V tako da Azure replikacije: oporavak web-mjesta podržava svim operacijskim sustavima koji su [podržava Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Za VMware i replikacije fizičke poslužitelja: provjera za Windows i Linux [preduvjeti](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Preduvjeti potvrdite neće uspjeti ako nije podržana.
Arhitektura goste operacijski sustav | 64-bitni | Preduvjeti potvrdite neće uspjeti ako nije podržana
Veličina diska operacijski sustav |  Do 1023 GB | Preduvjeti potvrdite neće uspjeti ako nije podržana
Count operacijski sustav na disku | 1 | Preduvjeti potvrdite neće uspjeti ako nije podržana.
Brojanje podataka na disku | 16 ili manje (funkcija veličine virtualnog računala stvoren je maksimalna vrijednost. 16 = XL) | Preduvjeti potvrdite neće uspjeti ako nije podržana
Veličina VHD disk podataka | Do 1023 GB | Preduvjeti potvrdite neće uspjeti ako nije podržana
Mrežnih prilagodnika | Podržani su više prilagodnika |
Statičke IP adrese | Podržana | Ako se primarni virtualnog računala koristi statičke IP adrese možete odrediti statičke IP adrese za virtualnog računala koje će se stvoriti u Azure. Imajte na umu da statičke IP adrese za virtualni stroj linux sustavom Hyper-v nije podržana.
iSCSI disk | Nije podržano | Preduvjeti potvrdite neće uspjeti ako nije podržana
Zajednički VHD | Nije podržano | Preduvjeti potvrdite neće uspjeti ako nije podržana
FC disk | Nije podržano | Preduvjeti potvrdite neće uspjeti ako nije podržana
Oblikovanje na tvrdom disku| VHD <br/><br/> VHDX | Iako VHDX trenutno nije podržano u Azure, oporavak web-mjesta automatski pretvara VHDX VHD kada ne preko Azure. Kada ne natrag na lokalni virtualnim strojevima nastaviti koristiti oblikovanje VHDX.
BitLocker | Nije podržano | BitLocker mora biti onemogućena prije zaštite virtualnog računala.
Naziv virtualnog računala| Između 1 i 63 znakova. Ograničeno na slova, brojeve i spojnice. Trebali biste početka i završetka s slovo ili broj | Ažuriranje vrijednosti u svojstva virtualnog računala oporavak web-mjesta
Vrsta virtualnog računala | <p>Generiranje 1</p> <p>Generiranje 2 - Windows</p> |  Generiranje 2 virtualnog računala OS na disku vrstu osnovni Disk koji uključuje 1 ili 2 količine podataka oblikom disk kao VHDX koji je manji od 300GB podržana. 2 za generiranje Linux virtualnim strojevima nisu podržani. [Pročitajte dodatne informacije](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Optimiziranje implementaciju sustava

Koristite sljedeće savjete za optimiziranje i promjena veličine implementaciju sustava.

- **Veličina jedinice operacijski sustav**: kada replicirati virtualnog računala da biste Azure jedinica s operacijskim sustavom mora biti manja od 1 TB. Ako imate više količine od ovog ih možete ručno premjestiti na neki drugi disk prije nego što počnete implementacije.
- **Podaci na disku veličina**: Ako ste replikaciju za Azure može imati najviše 32 diskova podataka na virtualnog računala, svaka s najviše 1 TB. Učinkovito možete replicirati i neće uspjeti putem ~ 32 TB virtualnog računala.
- **Ograničenja za plan oporavak**: oporavak web-mjesta mogu mijenjati veličinu tisućama slika s virtualnim računalima. Oporavak planovi osmišljeni su kao model za aplikacije koje treba neće uspjeti putem zajedno tako da ne možemo ograničiti broj strojeva u planu za oporavak do 50.
- **Ograničenja servisa Azure**: pretplatu u svakoj Azure dolazi sa skupom zadani ograničenja na jezgri, cloud usluge itd. Preporučujemo da pokrenete prebacivanje test da biste provjerili dostupnost resursa u svoju pretplatu. Možete mijenjati ta ograničenja putem Azure podrška.
- **Planiranje kapaciteta**: Saznajte više o [Planiranje kapaciteta](site-recovery-capacity-planner.md) za oporavak web-mjesta.
- **Replikacija propusnosti**: Ako ste kratki na propusnost replikacije Imajte na umu da:
    - **ExpressRoute**: oporavak web-mjesta radi s Azure ExpressRoute i WAN optimizers kao što su Riverbed. [Dodatne informacije potražite u](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) ExpressRoute.
    - **Promet replikacije**: oporavak web-mjesta koristi izvodi pametna početne replikacije pomoću samo blokovi podataka, a ne cijelu VHD. Samo promjene se replicirati tijekom tijeku replikacije.
    - **Mrežni promet**: možete kontrolirati mrežnog prometa koji se koristi za replikaciju postavljanje [Windows QoS](https://technet.microsoft.com/library/hh967468.aspx) pravila na temelju odredišni IP adresa i priključaka.  Uz to, ako ste replikaciju za oporavak web-mjesta za Azure pomoću agent za sigurnosno kopiranje Azure možete konfigurirati Reguliranje za taj agent. [Dodatne informacije potražite u](https://support.microsoft.com/kb/3056159).
- **RTO**: za mjerenje cilj oporavak vrijeme (RTO) možete očekivati s web-mjesta oporavak predlažemo da pokrenete test prebacivanje i prikaz poslove oporavak web-mjesta da biste analizirali koliko vremena potrebnog za operacije. Ako pokazivač se Nemogućnost Azure, za najbolje RTO preporučujemo automatizirati sve akcije ručno tako da integriranje Azure Automatizacija i oporavak tarife.
- **RPO**: oporavak web-mjesta podržava blizu sinkronizirano oporavak točke cilj (RPO) kada je replicirati na Azure. Pretpostavlja da se li dovoljno propusnosti između podatkovnog centra i Azure.


##<a name="service-urls"></a>URL-ove usluge
Provjerite jesu li URL-ove dostupne s poslužitelja


**URL-ovi** | **VMM za VMM** | **VMM za Azure** | **Hyper-V web-mjesta za Azure** | **VMware za Azure**
---|---|---|---|---
 \*. accesscontrol.windows.net | Pristupa  | Pristupa  | Pristupa  | Pristupa
 \*. backup.windowsazure.com |  | Pristupa  | Pristupa  | Pristupa
 \*. hypervrecoverymanager.windowsazure.com | Pristupa  | Pristupa  | Pristupa  | Pristupa
 \*. store.core.windows.net | Pristupa  | Pristupa  | Pristupa  | Pristupa
 \*. blob.core.windows.net |  | Pristupa  | Pristupa  | Pristupa
 https://www.msftncsi.com/ncsi.txt | Pristupa  | Pristupa  | Pristupa  | Pristupa
 https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Pristupa


## <a name="next-steps"></a>Daljnji koraci

Nakon učenje i Usporedba preduvjeti Općenito implementacije pročitajte detaljne preduvjeti i pokretanje implementacija svaki scenarij.

- [Replicirati VMware virtualnim strojevima za Azure](site-recovery-vmware-to-azure-classic.md)
- [Replicirati fizičke poslužitelji za Azure](site-recovery-vmware-to-azure-classic.md)
- [Replicirati Hyper-V poslužitelja u VMM oblaka za Azure](site-recovery-vmm-to-azure.md)
- [Replicirati Hyper-V virtualnim strojevima (bez VMM) za Azure](site-recovery-hyper-v-site-to-azure.md)
- [Replicirati Hyper-V VMs sekundarne web-mjesto](site-recovery-vmm-to-vmm.md)
- [Replicirati Hyper-V VMs sekundarne web-mjesto s SAN](site-recovery-vmm-san.md)
- [Replicirati Hyper-V VMs s poslužiteljem jedan VMM](site-recovery-single-vmm.md)
