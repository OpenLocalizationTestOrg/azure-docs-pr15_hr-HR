<properties
    pageTitle="Replicirati VMware virtualnim strojevima i fizičke poslužitelji za Azure s oporavak Azure web-mjesta na portalu za Azure | Microsoft Azure"
    description="U članku se opisuje kako implementirati Azure oporavak web-mjesta da biste orkestrirali replikacije, prebacivanje i oporavak lokalnog VMware virtualnim strojevima i Windows/Linux fizičke poslužitelji za Azure pomoću portala za Azure"
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
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>Replicirati VMware virtualnim strojevima i fizičke strojeva Azure s oporavak web-mjesta za Azure pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmware-to-azure.md)
- [Klasični Azure](site-recovery-vmware-to-azure-classic.md)
- [Azure klasični (naslijeđeno)](site-recovery-vmware-to-azure-classic-legacy.md)

Dobro došli u oporavak Azure web-mjesta! U ovom se članku koristite ako želite replicirati lokalnog VMware virtualnim strojevima ili Windows/Linux fizičke poslužitelje Azure pomoću oporavak Azure web-mjesta na portalu za Azure.

> [AZURE.NOTE] Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model.md) za stvaranje i rad s resursima: Azure resursa Manager (ARM) i classic. Azure ima dvije portalima – Azure klasični portala koji podržava model klasični implementacije i portalu Azure s podrškom za oba modele implementacije.

Oporavak web-mjesta na portalu za Azure nudi brojne nove značajke:

- Sigurnosno kopiranje Azure i oporavak Azure web-mjesta servisa kombiniraju se u jednu oporavak servisa sigurnog tako da možete postaviti i upravljanje poslovanje i Izrada oporavak (BCDR) s jednog mjesta. Sjedinjene nadzornoj ploči nadzor i upravljanje operacije na web-mjesta na lokalni Azure javno poslužitelja u oblak i.
- Korisnici s Azure pretplate dodjeli programom oblaka rješenje davatelj (CSP) sada mogu upravljati operacije oporavak web-mjesta na portalu za Azure.
- Oporavak web-mjesta na portalu za Azure možete replicirati računalima s računima za pohranu OKVIRA. Pri prebacivanje, oporavak web-mjesta stvara na ARM-VMs Azure.
- Oporavak web-mjesta i dalje podržava replikaciju račune klasični prostora za pohranu. Pri prebacivanje, oporavak web-mjesta stvara VMs pomoću klasične modela.

U ovom se članku nakon čitanje objavljuju komentare pri dnu u Disqus komentare. Zamolite tehnička pitanja na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Pregled

Tvrtke i ustanove moraju strategije BCDR koja određuje kako aplikacija, radnih opterećenja i podataka ostati pokrenut i dostupne tijekom planirane i Neplanirana isključiti, a čim oporaviti normalno funkcionira uvjeta. Strategije BCDR trebali biste zadržali poslovnih podataka sigurnih i koje se mogu vratiti i provjerite je li radnih opterećenja ostaju stalno dostupne kada se pojavi Izrada.

Oporavak web-mjesta je Azure service doprinosa strategije BCDR po orchestrating replikacije lokalne poslužitelje fizičke i virtualnih računala s oblakom (Azure) ili sekundarni podatkovnog centra. Prilikom kvarove u svom primarnom mjestu, koji se neće putem sekundarne mjesto da bi aplikacija i radnih opterećenja dostupna. Ne natrag na svom primarnom mjestu kada on se vraća na običnom operacije. Dodatne informacije u [oporavak web-mjesta Azure?](site-recovery-overview.md)

Ovaj članak sadrži sve potrebne informacije za replikaciju lokalnog VMware VMs i Windows/Linux fizičke poslužitelji za Azure. Uključuje se pregled arhitekture, Planiranje informacije i koraci za implementaciju za konfiguriranje Azure, lokalne poslužitelje, postavke replikacije i planiranje kapaciteta. Nakon što ste postavili Infrastruktura možete omogućiti replikacije na koje želite zaštititi, a zatim potvrdite taj pomoćni funkcionira.

## <a name="business-advantages"></a>Prednosti tvrtke

- Oporavak web-mjesta omogućuje službenoj zaštite radnih opterećenja tvrtke i aplikacije koje rade na VMware VMs i fizičke poslužiteljima.
- Portal za oporavak servisa predstavlja jedno mjesto za postavljanje, upravljanje i praćenje replikacije, prebacivanje i oporavak.
- Oporavak web-mjesta mogu automatski otkriti VMware VMs dodaje vSphere domaćini.
- Možete jednostavno pokrenuti failovers iz infrastruktura za lokalni Azure i failback (Vraćanje) iz Azure VMware VM poslužiteljima s lokalnim web-mjesta.
- Možete omogućiti više VM i stvaranje grupa replikacije tako da se aplikacija radnih opterećenja tiered preko više strojeva replicirati u isto vrijeme. Kada oni neće funkcionirati na svim računalima u grupi replikacije će vam rušenje dosljedan i aplikacije dosljedan oporavak točke. Za prebacivanje, možete prikupiti više računala u tarifama za oporavak tako da se tiered aplikacije radnih opterećenja neće uspjeti putem zajedno.

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

Ovo su komponente scenarija:

- **Poslužitelj za konfiguraciju**: programa na lokalnom stroju koji koordinate komunikacije, a upravlja postupaka replikacije i oporavak podataka. Na ovom računalu pokrenut ćete jedan instalacijsku datoteku, a da biste instalirali poslužitelj za konfiguraciju i te dodatne komponente:
    - **Poslužitelj za postupak**: služi kao replikacije pristupnika. Ga prima replikacije podatke iz zaštićenog izvor strojeva, optimizira predmemoriranja, sažimanja i šifriranje i šalje Azure prostora za pohranu. Također rukuje automatske instalacije servisa mobilnost na zaštićenom računalima, i izvodi automatsko otkrivanje VMware VMs. Zadani poslužitelj za postupak je instaliran na poslužitelj za konfiguraciju. Možete implementirati dodatne samostalni postupak poslužitelji za implementaciju sustava za promjenu veličine.
    - **Matrica poslužitelju ciljni**: rukuje replikacije podataka tijekom failback iz Azure.

- **Servis za mobilnost**: komponente je implementiran na svakom računalu (VMware VM ili poslužitelj za fizičke) koje želite replicirati Azure. Snima zapisivanje podataka na računalu i prosljeđuje ih na poslužitelj za postupak.
- **Azure**: ne morate stvoriti sve Azure VMs učiniti replikaciju i prebacivanje na Azure.  Morate Azure pretplatu, račun Azure prostora za pohranu za pohranu repliciranu podataka i Azure virtualne mreže tako da se Azure VMs nakon prebacivanje povezani s mrežom. Račun za pohranu i mrežne mora biti u području isti kao sigurnog servise za oporavak.
- **Failback**: kada budete spremni se neće vratiti iz Azure lokalno mjesto nakon na prebacivanje, morat ćete stvoriti programa Azure VM kao poslužitelj privremeno postupak. Možete izbrisati nakon failback. Za failback, trebat ćete VPN-a (ili Azure ExpressRoute) veze između lokalno mjesto i Azure mreže u kojem se nalaze na VMs Azure. Ako je promet failback velike i možda morati postaviti namjenski matrica poslužitelju ciljni strojno lokalnog. Zadani poslužitelj osnovne cilj izvode na poslužitelj za konfiguraciju može se koristiti za svjetlija promet.


Grafika prikazuje način na koji rade te komponente.

![Arhitektura](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Slika 1: VMware/fizičke za Azure**

## <a name="azure-prerequisites"></a>Preduvjeti za Azure

Evo što morate u Azure implementacija scenarij.

**Preduvjeta** | **Pojedinosti**
--- | ---
**Račun za Azure**| Potreban vam je račun sustava [Microsoft Azure](http://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.
**Azure prostora za pohranu** | Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs stvaraju kada dođe do prebacivanje. <br/><br/>Da biste pohranili podatke morat ćete račun standardno ili premium prostora za pohranu u području isti kao sigurnog oporavak servisa.<br/><br/>Možete koristiti LRS ili GRS računa za pohranu. Preporučujemo da GRS tako da se podaci prebacuju ako regionalne nestalo ili ako je primarni regija nije moguće oporaviti. [Dodatne informacije](../storage/storage-redundancy.md).<br/><br/> [Prostor za pohranu Premium](../storage/storage-premium-storage.md) obično koristi za virtualnim strojevima koje je potrebno dosljedno visoke performanse IO i niske latencije da biste intenzivno radnih opterećenja IO glavnog računala.<br/><br/> Ako želite da biste koristili premium računa da biste pohranili repliciranu podatke, morat ćete i standardne prostora za pohranu računa za spremanje zapisnika replikacije koji obuhvaćaju daljnje promjene lokalnih podataka.<br/><br/> Imajte na umu prostora za pohranu račune stvorene na portalu za Azure nije moguće premjestiti preko grupe resursa. I zaštita od premium račune za pohranu u središnjoj Indija i Južna Indija trenutno nije podržano.<br/><br/> [Doznajte](../storage/storage-introduction.md) Azure prostora za pohranu.
**Azure mreže** | Morat ćete Azure virtualne mreže koji Azure VMs će se povezati s kada dođe do prebacivanje. U području isti kao sigurnog oporavak Services mora biti Azure virtualne mreže.
**Failback iz Azure** | Potreban vam je privremenom postupak server postaviti kao VM programa Azure. To možete stvoriti kad budete spremni neće vratiti i izbrisati nakon prekida natrag.<br/><br/> Neuspjeh natrag morat ćete VPN veza (ili Azure ExpressRoute) s Azure mreže lokalno mjesto.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Poslužitelj za konfiguraciju / skaliranje out postupak preduvjeti

Koju ćete postaviti programa na lokalnom računalu kao poslužitelj za konfiguraciju / skaliranje iz poslužitelj za postupak.

**Preduvjeta** | **Pojedinosti**
--- | ---
**Poslužitelj za konfiguraciju**| Morate lokalnog fizičke ili virtualnog računala koji se izvodi Windows Server 2012 R2. Sve komponente za oporavak web-mjesta na lokalni instaliran na ovom računalu.<br/><br/>Za replikaciju VMware VM, preporučujemo da implementacija poslužitelju kao vrlo dostupna VM VMware. Ako ste replikaciju fizičke strojeva računala može biti fizičke poslužitelja.<br/><br/> Failback web-mjesto lokalnog iz Azure je uvijek VMware VMs bez obzira na to jesu li nije uspjela tijekom VMs ili fizičke poslužitelja. Ako ne implementacija poslužitelja za konfiguraciju kao VMware VM morat ćete postaviti zasebne osnovne cilj poslužitelja kao VMware VM prima failback promet.<br/><br/>Ako je poslužitelj VMware VM, Vrsta mrežnog prilagodnika mora biti VMXNET3. Ako koristite neku drugu vrstu mrežnog prilagodnika morat ćete instalirati na [VMware ažuriranje](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) na poslužitelju vSphere 5,5.<br/><br/>Poslužitelj imat statičke IP adrese.<br/><br/>Poslužitelj ne smije biti kontroler domene.<br/><br/>Naziv glavnog računala poslužitelja mora biti 15 znakova ili manje.<br/><br/>Operacijski sustav mora biti samo engleski.<br/><br/> Morat ćete instalirati VMware vSphere PowerCLI 6.0. na poslužitelj za konfiguraciju.<br/><br/>Poslužitelj za konfiguraciju potreban je pristup Internetu. Izlazni pristup potreban je na sljedeći način:<br/><br/>Privremeni pristup na HTTP 80 tijekom postavljanja komponenti za oporavak web-mjesta (da biste preuzeli MySQL)<br/><br/>Tijeku izlaznog pristup HTTPS 443 za upravljanje replikacije<br/><br/>Stalnog izlaznog pristupa na HTTPS 9443 za replikaciju promet (priključak može se mijenjati)<br/><br/>Poslužitelj će morati pristupa sljedeći URL-ovi tako da se možete povezati Azure: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.NET; *. backup.windowsazure.com; *. blob.Core.Windows.NET; *. store.core.windows.net<br/><br/>Ako imate IP utemeljen na adresi vatrozid pravila na poslužitelju, potvrdite da pravila dopustiti komunikaciju za Azure. Morat ćete dopustiti [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/confirmation.aspx?id=41653) i protokol HTTPS (443).<br/><br/>Dopusti rasponi IP adresa za Azure regija pretplate i Zapad SAD-a.<br/><br/>Dopusti ovaj URL za preuzimanje MySQL:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>Preduvjeti za glavno računalo vCenter/vSphere VMware

**Preduvjeta** | **Pojedinosti**
--- | ---
**vSphere**| Potreban vam je jedan ili više hypervisors vSphere VMware.<br/><br/>Hypervisors mora biti pokrenut vSphere verzija 6.0, 5,5 ili 5.1 s najnovijim ažuriranjima.<br/><br/>Preporučujemo da se nalaze u istom mrežom kao poslužitelj za postupak (Ta će se s mrežom u kojem poslužitelj za konfiguraciju nalazi osim ako ste postavili namjenski proces poslužitelja) vSphere domaćini i vCenter poslužitelja.
**vCenter** | Preporučujemo da implementacije VMware vCenter poslužitelju za upravljanje sustava vSphere domaćini. Ga treba imati vCenter verzija 6.0 ili 5,5 s najnovijim ažuriranjima.<br/><br/>Imajte na umu da oporavak web-mjesta ne podržava nove vCenter i vSphere 6.0 značajke kao što su unakrsno vCenter vMotion, virtualne količine i pohranu DRS. Značajke koje nisu dostupne u verziji 5,5 ograničen podršci za oporavak web-mjesta.


## <a name="protected-machine-prerequisites"></a>Preduvjeti za zaštićeni računala

**Preduvjeta** | **Pojedinosti**
--- | ---
**Lokalni (VMware VMs)** | VMware VMs koji želite zaštititi moraju imati instaliran i pokrenut Alati za VMware.<br/><br/> Strojeva koji želite zaštititi mora biti u skladu s [Azure preduvjeta](site-recovery-best-practices.md#azure-virtual-machine-requirements) za stvaranje Azure VMs.<br/><br/>Kapacitet pojedinačne disk na zaštićenom bi trebalo biti više od 1023 GB. Na VM mogu imati do 64 diskova (dakle do 64 TB). <br/><br/>Minimalna 2 GB slobodnog prostora na disku instalacije za instalaciju komponente.<br/><br/>Zaštita VMs s šifrirane diskova nije podržana.<br/><br/>Zajedničko korištenje na disku goste klastere nisu podržane.<br/><br/>**Priključak 20004** treba otvoriti na računala zaštićeni virtualne lokalni vatrozid, ako želite omogućiti **dosljednost aplikacije**.<br/><br/>Strojevima koji imaju Sjedinjeno komuniciranje Extensible firmver sučelja (UEFI) / pokretanje Extensible firmver Interface(EFI) nije podržana.<br/><br/>Nazivi računala mora sadržavati između 1 i 63 znakova (slova, brojeve i spojnice). Naziv mora započeti s slovo ili broj i završavaju slovo ili broj. Kada omogućite replikacije za stroj možete izmijeniti naziv Azure.<br/><br/>Ako je izvor VM NIC udruživanje nakon prebacivanje na Azure se pretvara jedan NIC.<br/><br/>Ako zaštićeni virtualnim strojevima na disk iSCSI zatim oporavak web-mjesta pretvara zaštićeni VM iSCSI na disku u VHD datoteke ako na VM ne uspije putem Azure. Ako cilj iSCSI proslijediti po Azure VM zatim bit će s njim povezati i zapravo potražite u članku dva diskova – VHD disk na Azure VM i izvorni iSCSI disk. U ovom slučaju morate prekinuti vezu na odredište iSCSI koji se pojavljuje na Azure VM.
**Računalima sustava Windows (fizičke ili VMware)** | Na računalu moraju imati podržani 64-bitni operacijski sustav: Windows Server 2012 R2, Windows Server 2012 ili Windows Server 2008 R2 sa pri najmanje SP1.<br/><br/> Operacijski sustav trebala biti instalirana na pogon C:\. Na disku OS mora biti Windows osnovni disk, a ne dinamičke. Podatkovni disk može biti dinamičke.<br/><br/>Oporavak web-mjesta podržava VMs na disku RDM. Tijekom failback, oporavak web-mjesta će ponovno korištenje diska RDM ako izvornog izvora VM i RDM disk. Ako nisu dostupne tijekom failback oporavak web-mjesta će stvoriti novu datoteku VMDK za svaki disk.
**Linux računalima** (phyical ili VMware)|  Potreban vam je podržana 64-bitni operacijski sustav: crvena je vaša Enterprise Linux 6.7,7.1,7.2; Centos 6.5, 6.6,6.7,7.0,7.1,7.2; Oracle Enterprise Linux 6,4, 6.5 pokrenut je vaša crveno kompatibilne otklanjanje ili Unbreakable Enterprise otklanjanje izdanje 3 (UEK3) SUSE Linux Enterprise Server 11 SP3.<br/><br/>/etc/Hosts datoteke na zaštićenom moraju sadržavati stavke koje mapiranje lokalne glavno IP adrese pridružene sve mrežnih prilagodnika.<br/><br/>Ako želite povezati Azure virtualnog računala radi Linux nakon prebacivanje pomoću ljuske za sigurnu klijenta (ssh) provjerite je li servis za sigurnu ljuske na zaštićenom računalu postavljena da biste započeli na automatsko pokretanje sustava i dopustiti pravila vatrozida programa ssh veze na njega.<br/><br/>Naziv glavnog računala, točke pogona, nazivi uređaja i Linux sustava putovima i nazivima datoteka (npr/itd /; /usr) mora biti na engleskom samo.<br/><br/>Zaštita je moguće omogućiti samo za Linux strojevima sa sljedećim prostora za pohranu: File system (EXT3, ETX4, ReiserFS, XFS); Multipath softver uređaji mapiranje (multipath)); Upravitelj glasnoće: (LVM2). Fizička poslužitelji za pohranu kontroler HP CCISS nisu podržane. Datotečnom sustavu ReiserFS podržana je samo na SUSE Linux Enterprise Server 11 SP3.<br/><br/>Oporavak web-mjesta podržava VMs na disku RDM.  Tijekom failback za Linux oporavak web-mjesta ne ponovno korištenje RDM disk. Umjesto toga stvara novu datoteku VMDK za svaki odgovarajući RDM disk.<br/><br/>Provjerite je li postavili postavku disk.enableUUID=true u konfiguraciji parametre VM u VMware. Ako ne postoji, stvorite stavku. Po potrebi možete unijeti dosljedan UUID na VMDK tako da ga pravilno mounts. Dodavanje tu postavku osigurava da se promjene samo delta prenose natrag na lokalni tijekom failback, a ne cijeli replikacije.
**Servis za mobilnost** |  **Windows**: prikazat će vam da biste automatski servis mobilnost na VMs sa sustavom Windows morat ćete unijeti administratorski račun (lokalni administrator na računalu Windows) da bi poslužitelj za postupak možete učiniti automatske instalacije.<br/><br/>**Linux**: da biste automatski servis mobilnost na VMs izvodi Linux morat ćete stvoriti račun koje je moguće koristiti poslužitelj za postupak da biste učinili automatske instalacije.<br/><br/> Po zadanom su replicirati diskova na računalo. Da biste [izuzeli disk s replikacijom](#exclude-disks-from-replication), servis za mobilnost mora biti instaliran ručno na računalu prije omogućivanja replikacije.<br/>

## <a name="prepare-for-deployment"></a>Priprema za implementaciju

Priprema za implementaciju morat ćete:

1. [Postavljanje Azure mreže](#set-up-an-azure-network) u kojem Azure VMs nalazit će se kada ih se spun nakon prebacivanje. Osim toga, za failback morat ćete za postavljanje VPN veza (ili Azure ExpressRoute) s Azure mreže lokalno mjesto.
2. [Postavljanje računa Azure prostora za pohranu](#set-up-an-azure-storage-account) za repliciranu podataka.
3. [Priprema računa](#prepare-an-account-for-automatic-discovery) na poslužitelju vCenter ili vSphere hostira tako da se oporavak web-mjesta može automatski prepoznati VMs VMware koji su dodani.
4. [Priprema poslužitelj za konfiguraciju](#prepare-the-configuration-server) da biste bili sigurni da možete pristupiti potreban URL-ovi i instalirati vSphere PowerCLI 6.0.


### <a name="set-up-an-azure-network"></a>Postavite mrežu Azure

- Mreža mora biti u istom Azure području kao u kojoj ćete implementacija sigurnog servise za oporavak.
- Ovisno o modelu resursa koji želite koristiti za nije uspjela tijekom Azure VMs, postavit ćete Azure mreže u [načinu ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) ili [Klasični](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Da bi se neće vratiti iz Azure lokalnog VMware web-mjesta potreban je VPN veza (ili povezani Azure ExpressRoute) s Azure mreže u kojem se nalaze na lokalnu mrežu u kojoj se nalazi poslužitelj za konfiguraciju repliciranu VMs Azure.
- [Dodatne informacije o](../vpn-gateway/vpn-gateway-site-to-site-create.md) podržanim implementacije modelima za VPN veze na web-mjesto, te kako [postaviti vezu](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- Umjesto toga možete postaviti [Azure ExpressRoute](../expressroute/expressroute-introduction.md). [Dodatne informacije](../expressroute/expressroute-howto-vnet-portal-classic.md) o postavljanju Azure mreže s ExpressRoute.

> [AZURE.NOTE] [Migracija mreže](../resource-group-move-resources.md) preko grupe resursa unutar iste pretplate ili putem pretplate nije podržana za mreže koji se koristi za implementaciju oporavak web-mjesta.

### <a name="set-up-an-azure-storage-account"></a>Postavljanje računa za pohranu za Azure

- Potreban vam je standardno ili račun za Azure prostora za pohranu premium veličini podataka replicirati na Azure. Račun mora biti u području isti kao sigurnog servise za oporavak. Ovisno o modelu resursa koji želite koristiti za nije uspjela tijekom Azure VMs, postavit ćete račun u [načinu ARM](../storage/storage-create-storage-account.md) ili [Klasični](../storage/storage-create-storage-account-classic-portal.md).
- Ako koristite račun sustava premium repliciranu podataka potrebnih za stvaranje dodatnih standardni račun za spremanje zapisnika replikacije koji obuhvaćaju daljnje promjene lokalnih podataka.  

> [AZURE.NOTE] [Migracije računa za pohranu](../resource-group-move-resources.md) po grupama resursa unutar iste pretplate ili uzduž pretplate nije podržana za račune za pohranu koji se koriste za implementaciju oporavak web-mjesta.

### <a name="prepare-an-account-for-automatic-discovery"></a>Priprema računa za automatsko otkrivanje

Poslužitelj za postupak oporavak web-mjesta mogu automatski otkriti VMware VMs na vSphere domaćini ili na poslužitelju vCenter koja upravlja domaćini. Da biste izvršili automatsko otkrivanje vjerodajnice oporavak web-mjesta koja možete pristupiti poslužitelju VMware. Nije važno ako ste replikaciju fizičke strojeva.

1. Da biste koristili namjenski račun za automatsko otkrivanje Stvaranje uloga (na primjer Azure_Site_Recovery) na razini vCenter s [potrebne dozvole](#vmware-account-permissions).
2. Stvaranje novog korisnika na poslužitelju glavno računalo ili vCenter vSphere i dodjeljivati ulogu korisniku. Kasnije ćete omogućiti zna vjerodajnica tako da možete izvoditi automatsko otkrivanje oporavak web-mjesta.

    >[AZURE.NOTE] VCenter korisnički račun s ulogom koja je samo za čitanje možete pokrenuti prebacivanje, ali ne može isključiti strojeva zaštićeni izvora. Ako želite isključiti tih strojeva morat ćete [Azure_Site_Recovery](#vmware-account-permissions) uloge. Ako koristite samo Migracija VMs s VMware Azure, a ne morate failback pa samo za čitanje uloga je dovoljno.

### <a name="prepare-the-configuration-server"></a>Priprema poslužitelj za konfiguraciju

1.  Provjerite je li na računalu koje koristite za poslužitelj za konfiguraciju uspostavu [preduvjeti](#configuration-server-prerequisites). Posebno provjerite je li računalo povezano s Internetom pomoću navedenih postavki:

    - Dopusti pristup URL-ove: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.NET; *. backup.windowsazure.com; *. blob.Core.Windows.NET; *. store.core.windows.net
    - Dopustiti pristup [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) da biste preuzeli MySQL.
    - Dopustiti komunikaciju Vatrozid za Azure s [podatkovnim centrom za Azure IP raspone](https://www.microsoft.com/download/confirmation.aspx?id=41653) i protokol HTTPS (443).

2.  Preuzmite i instalirajte [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) na poslužitelj za konfiguraciju. (Trenutno je još verzije sustava PowerCLI nisu podržane, uključujući R izdanja verzije 6.0.)


## <a name="create-a-recovery-services-vault"></a>Stvaranje oporavak servisa sigurnog

1. Prijavite se na [portal za Azure](https://portal.azure.com).
2. Kliknite **Novi** > **upravljanja** > **sigurnosno kopiranje i vraćanje web-mjesta (OMS)**. Umjesto toga možete kliknuti **Pregledaj** > **Oporavak servisa sigurnog** > **Dodaj**.

    ![Nove zbirke ključeva](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. U odjeljku **naziv** navedite neslužbeni naziv da biste odredili na zbirke ključeva. Ako imate više pretplata, odaberite jedan od njih.
4. [Stvori novu grupu resursa](../resource-group-template-deploy-portal.md) ili odaberite postojeći. Navedite Azure regija. Strojeva će je replicirati na tom području. Imajte na umu da Azure prostora za pohranu i mrežama za oporavak web-mjesta morat ćete na istom području. Da biste provjerili podržanih regija pogledajte geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Ako želite brzo pristupiti na sigurnog na nadzornoj ploči kliknite **Prikvači na nadzornu ploču** , a zatim kliknite **Stvori**.

    ![Nove zbirke ključeva](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Nove zbirke ključeva pojavit će se na **nadzornoj ploči** > **sve resurse**i na glavnom plohu **sefovi servise za oporavak** .

## <a name="getting-started"></a>Početak rada

Oporavak web-mjesta sadrži sučelje za početak rada namijenjenu što prije počnete i brzo moguće pokrenuti. Provjerava preduvjeti i vodi vas kroz korake morate nabaviti oporavak web-mjesta implementiran.

Najprije ćete odabrati vrstu strojeva želite za replikaciju i mjesto na koje želite replicirati na. Postavljanje infrastrukture, uključujući lokalne poslužitelje, Azure postavke, pravila za replikaciju i planiranje kapaciteta. Po završetku preduvjete infrastrukture na mjestu omogućiti replikacije VMs i fizičke poslužitelja. Zatim možete pokrenuti failovers za određene strojeva ili stvoriti tarife za oporavak uvoza na više računala.

Početak rada započeti tako da odaberete način na koji želite uvesti oporavak web-mjesta. Početak rada tijek promjene malo po vlastitom nahođenju replikacije.


## <a name="step-1-choose-your-protection-goals"></a>Korak 1: Odaberite Zaštita ciljeve

Odaberite što želite replicirati i mjesto na koje želite replicirati na.

1. U plohu **sefovi oporavak servisi** odaberite vaše zbirke ključeva, a zatim **Postavke**.
2. U odjeljku **Postavke** > **Početak rada** kliknite **Oporavak web-mjesta** > **Korak 1: Priprema infrastrukture** > **zaštitu cilj**.

    ![Odaberite ciljeva](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. U **cilju zaštite** odaberite **Za Azure**pa odaberite **Da, s VMware vSphere Hypervisor**. Zatim kliknite **u redu**.

    ![Odaberite ciljeva](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Korak 2: Postavljanje okruženja izvora

Postavljanje poslužitelja za konfiguraciju i registrirati u sigurnog servise za oporavak. Ako ste replikaciju VMware VMs odredite VMware račun koji koristite za automatsko otkrivanje.

1. Kliknite **Korak 1: Priprema infrastrukture** > **izvora**. U **izvoru Priprema** ako nemate poslužitelj za konfiguraciju kliknite **+ poslužitelj za konfiguraciju** dodavanja novih.

    ![Postavljanje izvora](./media/site-recovery-vmware-to-azure/set-source1.png)

2. Potvrdite plohu **Dodavanje Server** **Poslužitelj za konfiguraciju** će se pojaviti dodatak **poslužitelja upišite**.
3. Prije postavljanja poslužitelj za konfiguraciju provjerite je li [preduvjeti](#configuration-server-prerequisites). U određenom na računalu možete pristupiti URL-ove potreban.
4.  Preuzmite instalacijski program za Sjedinjeno komuniciranje web-mjesta oporavak instalacijsku datoteku.
5.  Preuzmite ključa za registraciju zbirke ključeva. Potreban vam je to pri pokretanju instalacijskog programa Sjedinjeno komuniciranje. Ključno je valjan 5 dana nakon ga stvoriti.

    ![Postavljanje izvora](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  Na računalu koje koristite kao poslužitelj za konfiguraciju, pokrenite Sjedinjeno komuniciranje instalacijski program za instalaciju poslužitelja za konfiguraciju, poslužitelj za postupak i osnovne odredišni poslužitelj.


### <a name="run-site-recovery-unified-setup"></a>Instalacijski program za Sjedinjeno komuniciranje oporavak izvođenja web-mjesta

1.  Pokrenite instalacijski program za Sjedinjeno komuniciranje instalacijsku datoteku.
2.  **Prije nego što počnete** odaberite **Instalacija poslužitelja za konfiguraciju i poslužitelj za postupak**.

    ![Prije početka](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. U **Proizvođača softverskoj licenci** kliknite **prihvaćam** da biste preuzeli i instalirali MySQL.

    ![Treći = proizvođača softvera](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. U **Registracija** pronađite i odaberite ključa za registraciju ste preuzeli s na zbirke ključeva.

    ![Registracija](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. U **Postavke internetske** odredite kako davatelja usluge na poslužitelju za konfiguraciju će se povezati s oporavak Azure web-mjesta putem Interneta.

    - Ako se želite povezati s proxy poslužitelja koja je trenutno postavljena na računalu odaberite **Poveži s postojeće postavke proxyja**.
    - Ako želite da se davatelj usluga za povezivanje izravno odaberite **Poveži izravno bez proxy poslužitelj**.
    - Ako postojeće proxy poslužitelj zahtijeva provjeru autentičnosti ili koji želite koristiti prilagođenu proxy davatelj veze, odaberite **Poveži s postavkama prilagođene proxy**.
        - Ako koristite prilagođeni proxy morat ćete navesti adresu, priključak i vjerodajnice
        - Ako koristite proxy poslužitelj treba ste već dopustili URL-ovi opisane u [preduvjeti](#configuration-server-prerequisites).

    ![Vatrozid](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. U **Provjeri preduvjeti** postavljanje pokreće provjeru da biste provjerili možete pokrenuti instalaciju. Ako se pojavi upozorenje o **Provjeri sinkronizacija globalni vremena** koje vrijeme na sat sustava (postavke**datuma i vremena** ) provjerite je li isto kao vremensku zonu.

    ![Preduvjeti](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. U **Konfiguraciji MySQL** stvorite vjerodajnice za prijavu na instancu MySQL poslužitelja koji će se instalirati.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. **Detalji o okruženju** odaberite li namjeravate replicirati VMware VMs. Ako ste, zatim postavljanje provjerava je li instaliran PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. **Instalacija mjesto** odaberite mjesto na koje želite instalirati na binarne datoteke i spremanje predmemoriju. Možete odabrati na pogon koji ima najmanje 5 GB prostora za pohranu dostupna, no preporučujemo da pogon predmemorije s najmanje 600 GB slobodnog prostora.

    ![Mjesto instalacije](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. U **Odabir mreže** navedite ga slušatelj (mrežnog prilagodnika i SSL priključak) na kojem poslužitelj za konfiguraciju će slanje i primanje replikacije podataka. Možete promijeniti zadani priključak (9443). Uz ovaj priključak priključak 443 koristit će web-poslužitelju koji orchestrates replikacije operacije. 443 ne bi trebalo koristiti za primanje replikacije prometa.


    ![Odabir mreže](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  U **sažetku** pregledajte podatke, a zatim kliknite **Instaliraj**. Kada se dovrši instalacija generira se pristupni izraz. Morat ćete ga kada omogućite replikacije tako da kopirate i zadržavanje na sigurnom mjestu.

    ![Sažetak](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Registracija završetku poslužitelja prikazuje se u odjeljku **Postavke** > plohu**poslužitelji** u na zbirke ključeva.



#### <a name="run-setup-from-the-command-line"></a>Pokrenite instalacijski program iz naredbenog retka

Možete postaviti poslužitelj za konfiguraciju iz naredbenog retka:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parametri:

- / ServerMode: obavezno. Određuje je li potrebno instalirati poslužitelja za konfiguraciju i procesa ili samo poslužitelj za postupak. Unos vrijednosti: CS, PS.
- InstallLocation: obavezno. Mape u kojima su instalirani komponente.
- / MySQLCredsFilePath. Obavezno. Put datoteke spremaju MySQL vjerodajnice za poslužitelj. Datoteka mora biti u ovom obliku:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- / VaultCredsFilePath. Obavezno. Mjesto datoteke vjerodajnice zbirke ključeva
- / EnvType. Obavezno. Vrsta instalacije. Vrijednosti: VMware, NonVMware
- / PSIP i /CSIP. Obavezno. IP adresa poslužitelja postupak i konfiguraciji poslužitelja.
- / PassphraseFilePath. Obavezno. Mjesto datoteke pristupni izraz.
- / BypassProxy. Neobavezno. Određuje je li poslužitelj za konfiguraciju povezuje Azure bez proxy poslužitelj.
- / ProxySettingsFilePath. Neobavezno. Postavke proxyja (zadani proxy zahtijeva provjeru autentičnosti ili prilagođeni proxy poslužitelja). Datoteka mora biti u ovom obliku:
    - [ProxySettings]
    - ProxyAuthentication = "Da/ne"
    - Proxy IP = "IP adresa >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - ProxyPassword = "<Password>"
- DataTransferSecurePort. Neobavezno. Broj priključka koji će se koristiti za replikaciju podatke.
- SkipSpaceCheck. Neobavezno. Preskočite prostora provjeri predmemoriju.
- AcceptThirdpartyEULA. Obavezno. Zastavica podrazumijeva prihvaćanje trećih strana licencni ugovor.
- ShowThirdpartyEULA. Obavezno. Prikazuje EULA drugih proizvođača. Ako je navedena kao ulaz zanemaruju se sve parametre.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Dodavanje računa VMware koji se koristi za automatsko otkrivanje

 Kada ste spremni za implementaciju imat ćete [stvoriti VMware računa](#prepare-an-account-for-automatic-discovery) koje oporavak web-mjesta mogu koristiti za automatsko otkrivanje. Dodajte račun na sljedeći način:

1. Otvorite **CSPSConfigtool.exe**. Nije dostupan prečac na radnoj površini i nalazi u mapi \home\svsystems\bin [instalirati mjesto].
2. Kliknite **upravljanje računima** > **Dodaj račun**.

    ![Dodavanje računa](./media/site-recovery-vmware-to-azure/credentials1.png)

3. U **Pojedinosti o računu** Dodajte račun koji će se koristiti za automatsko otkrivanje. Imajte na umu da se može potrajati 15 minuta ili više uz naziv računa da se pojavi na portalu. Da biste ažurirali odmah, kliknite **Konfiguraciju poslužitelja** > naziv poslužitelja > **Osvježi poslužitelja**.

    ![Pojedinosti](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Povezivanje s vSphere domaćini i vCenter poslužitelja

Ako ste replikaciju VMware VMs povezati vSphere domaćini i vCenter poslužitelja.

1. Provjerite postoji li poslužitelj za konfiguraciju mrežni pristup vSphere domaćini i vCenter poslužitelja.
2. Kliknite **Priprema infrastrukture** > **izvora**. U **izvoru Priprema** odaberite poslužitelj za konfiguraciju pa kliknite **+ vCenter** da biste dodali vSphere glavno računalo ili vCenter poslužitelja.
3. U **vCenter Dodaj** odredite neslužbeni naziv poslužitelja glavno računalo ili vCenter vSphere i navedite IP adresa ili Poslužitelja. Ostavite priključak kao 443, osim ako se vaši poslužitelji VMware konfigurirani tako da biste preslušali zahtjeva za drugi priključak za. Zatim odaberite račun koji će se koristiti za povezivanje s poslužiteljem VMware. Kliknite **u redu**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Ako dodajete vCenter poslužitelja ili vSphere glavnog računala s računom koji ne sadrži administratorske ovlasti na poslužitelju vCenter ili glavno računalo, zatim provjerite račun ima li ovlasti omogućen: podatkovnog centra, Datastore, mapu, glavno računalo, mreža, resursa, virtualne na računalu, vSphere raspodijeljenom promjenu. Uz to vCenter poslužitelja mora prikazi ovlasti za pohranu.

Oporavak web-mjesta povezuje s poslužiteljima VMware pomoću postavki određen i otkrije VMs.

## <a name="step-3-set-up-the-target-environment"></a>Korak 3: Postavljanje okruženja cilj

Provjerite imate li račun za pohranu za replikaciju i programa Azure mreža u kojoj Azure VMs će nakon prebacivanje.

1.  Kliknite **Priprema infrastrukture** > **Ciljna** i odaberite Azure pretplatu koju želite koristiti.
2.  Određivanje implementacije model koji želite koristiti za VMs nakon prebacivanje.
3.  Oporavak web-mjesta provjere imate li jednu ili više računa kompatibilne Azure prostora za pohranu i mreže.

    ![Cilj](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Ako niste stvorili račun za pohranu i želite da biste je stvorili pomoću OKVIRA kliknite **+ račun za pohranu** potrebno tom retku.  Na **račun za pohranu za stvaranje** plohu Navedite naziv računa, vrsta, pretplate i mjesto. Račun mora biti u području isti kao sigurnog servise za oporavak.

    ![Prostor za pohranu](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Imajte na umu da:

    - Ako želite stvoriti račun za pohranu pomoću klasične modela to ćete učiniti na portalu za Azure. [uči više](../storage/storage-create-storage-account-classic-portal.md)
    - Ako koristite račun za pohranu premium repliciranu podataka morat ćete postaviti dodatni prostor za pohranu standardni račun koji želite spremište replikacije evidentira promjene tijeku te snimanje lokalne podatke.

    > [AZURE.NOTE] Zaštita od premium račune za pohranu u središnjoj Indija i Južna Indija trenutno nije podržano.

4.  Odaberite Azure mreže. Ako niste stvorili mreže, a želite to učiniti pomoću OKVIRA kliknite **+ mreže** da biste učinili tom retku. Na **Stvaranje virtualne mreže** plohu Navedite naziv mreže, raspon adresu, podmreže detalje, pretplate i mjesto. Mreža mora biti na istom mjestu kao sigurnog servise za oporavak.

    ![Mreža](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Ako želite stvoriti mreže pomoću klasične modela to ćete učiniti na portalu za Azure. [Dodatne informacije](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Korak 4: Postavljanje postavki replikacije

1. Da biste stvorili novi replikacije pravila kliknite **Priprema infrastrukture** > **Replikacije postavke** > **+ Stvaranje i povezivanje**.
2. **Stvaranje** i pridruži pravila unesite naziv pravila.
3. U **RPO prag**: odredite ograničenje RPO. Kad neprekinuti replikacije premašuje ograničenje, bit će načinjena upozorenja.
5. U **oporavak pokažite zadržavanja**, navedite u satima koliko prozoru zadržavanja će biti za svaku točku za oporavak. Zaštićeni strojeva se može oporaviti postavite unutar prozora. Do 24 sata zadržavanja nije podržana za strojeva replicirati na premium prostora za pohranu.
6. U **aplikaciju dosljedan učestalost snimke**, navedite koliko često (u minutama) oporavak točke koja sadrži aplikaciju dosljedan snimke stvorit će se.
7. Kada stvorite pravilo replikacije, po zadanom odgovarajuće pravila automatski se stvara za failback. Ako je Pravilnik za replikaciju **predstavnik pravila** , a zatim failback pravila, primjerice će biti **predstavnik pravila failback**. Ovo pravilo ne koristi dok se ne pokrenete u failback.  
8. Kliknite **u redu** da biste stvorili pravilo.

    ![Pravila replikacije](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Prilikom stvaranja novog pravilnika ga automatski povezao s poslužitelja za konfiguraciju. Kliknite **u redu**.

    ![Pravila replikacije](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Korak 5: Planiranje kapaciteta

Sad kad ste na basic infrastrukture postavljanje koje možete razmislite o planiranje kapaciteta i utvrđivanje trebate li dodatne resurse.

Oporavak web-mjesta omogućuje kapaciteta planer za dodjelu prave resurse za vaše okruženje izvora, komponente oporavak web-mjesta, mreže i prostora za pohranu. Planer možete pokrenuti u brzom načinu rada za Procjena Prosječan broj VMs, diskova i prostora za pohranu na temelju ili u detaljnom načinu kojim ćete slika na razini radno opterećenje za unos. Prije nego što počnete morat ćete:

- Prikupite informacije o okruženju sustava replikacije, uključujući VMs diskova po VMs i prostor za pohranu po disk.
- Procjenu dnevnih stopa promjena (churn) morat ćete repliciranu podataka. [Planiranje potražite kapaciteta vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) možete koristiti da biste mogli učiniti ovo.

1.  Kliknite **Preuzimanje** Preuzmite alat, a zatim ga pokrenuti. [U članku čitanje](site-recovery-capacity-planner.md) koja se isporučuje se uz alat.
2.  Kada završite odaberite **da** **ste dovršili planiranje kapaciteta?**

    ![Planiranje kapaciteta](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

U tablici u nastavku spremit će broj točaka će vam pomoći u ovom scenariju planiranja kapaciteta.


**Komponenta** | **Pojedinosti**
--- | --- | ---
**Replikacije** | **Svakodnevno Najveća promjena stopa**– zaštićeni računala možete koristiti samo jedan poslužitelj za postupak i jedan proces poslužitelja možete rukovati dnevnu promjena ocijeniti do 2 TB. Stoga 2 TB je maksimalna dnevno podaci mijenjaju stopa koja je podržana za zaštićeni računala.<br/><br/> **Maksimalna propusnost**– repliciranu strojno pripadati jednog računa za pohranu u Azure. Standardni prostora za pohranu računa možete učiniti najviše 20 000 za u sekundi pa preporučujemo da ostavite broj IOPS preko izvornog računala 20 000. Za primjer ako imate izvor računala s 5 diskova i svakom disku generira 120 IOPS (8K veličina) na izvoru ona će biti unutar Azure po disk IOPS ograničenje od 500. Broj računa za pohranu potrebno = izvora zbroja IOPs/20000.
**Poslužitelj za konfiguraciju** | Poslužitelj za konfiguraciju trebali biste moći rukovanje dnevni kapacitet stopa Promijeni sve pokrenute na zaštićenom strojeva radnih opterećenja i treba li dovoljno propusnosti za stalno replicirati podataka Azure za pohranu.<br/><br/> Kao najbolji način preporučujemo nalazi li se poslužitelj za konfiguraciju na istoj mreži i segmentu LAN-a kao strojeva koji želite zaštititi. Nalazi na drugoj mreži, ali strojeva koji želite zaštititi mora imati L3 mreže vidljivost na njega.<br/><br/> Veličina preporuke za poslužitelj za konfiguraciju su navedene u tablici u nastavku.
**Poslužitelj za postupak** | Prvi poslužitelj za postupak je instaliran po zadanom na poslužitelj za konfiguraciju. Možete implementirati poslužitelje dodatne postupak za promjenu veličine u okruženju sustava. Imajte na umu da:<br/><br/> Poslužitelj za postupak prima replikacije podatke iz zaštićenog strojeva i optimizira predmemoriranja, sažimanja i šifriranje prije no što pošaljete Azure. Pokrenite postupak poslužitelj mora imati dovoljno resursa za izvođenje ovih zadataka.<br/><br/> Poslužitelj za postupak koristi predmemorije diska temelje. Preporučujemo da se na disku zasebnom predmemorije 600 GB ili više učiniti promjene podataka pohranjene u slučaju usko grlo mreže ili nedostupnosti.

### <a name="size-recommendations-for-the-configuration-server"></a>Veličina preporuke za poslužitelj za konfiguraciju

**PROCESOR** | **Memorije** | **Veličina predmemorije na disku** | **Podaci mijenjaju stopa** | **Zaštićeni računalima**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5) | 16 GB | 300 GB | 500 GB ili manje | Replicirati manje od 100 strojeva.
12 vCPUs (2 sockets * 6 jezgri @ GHz 2,5) | 18 GB | 600 GB | 500 GB 1 TB | Replicirati između 100 150 strojeva.
16 vCPUs (2 sockets * 8 jezgri @ GHz 2,5) | 32 GB | 1 TB | 1 TB za 2 TB | Replicirati između 150 200 računala.
Drugi poslužitelj za postupak implementacije | | | > 2 TB | Implementacija poslužitelja dodatne postupak ako ste replikaciju više od 200 strojeva ili ako promijenite dnevnih podataka stopa prelazi 2 TB.

Pri čemu je:

- Svaki izvor strojno konfiguriran 3 diskova 100 GB.
- Koristi benchmarking prostora za pohranu 8 pogona SAS 10 k RPM s RAID 10 za predmemorije diska mjere.

### <a name="size-recommendations-for-the-process-server"></a>Veličina preporuke za poslužitelj za postupak

Ako trebate zaštita više od 200 strojeva ili dnevni stopa promjena je veće od 2 TB možete dodati dodatne postupak poslužitelji za rukovanje opterećenje replikacije. Da biste skalirali izgleda koje možete:

- Povećajte broj konfiguraciju poslužitelja. Na primjer možete zaštititi do 400 računala s dva konfiguraciju poslužitelja.
- Dodavanje dodatnih postupak poslužitelja i koristite ih učiniti promet umjesto (ili uz) poslužitelja za konfiguraciju.

U ovoj su tablici opisane scenarij u kojem:

- Ne planirate koristiti poslužitelj za konfiguraciju kao poslužitelj za postupak.
- Postavite poslužitelj za dodatne postupak.
- Ste konfigurirali zaštićeni virtualnim strojevima koristite dodatne postupak poslužitelj.
- Svakom računalu zaštićeni izvora je konfiguriran za korištenje tri diskova 100 GB.

**Poslužitelj za konfiguraciju** | **Dodatni postupak poslužitelja**| **Veličina predmemorije na disku** | **Podaci mijenjaju stopa** | **Zaštićeni računalima**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5), 16 GB memorije | 4 vCPUs (2 sockets * 2 jezgri @ GHz 2,5), 8 GB memorije | 300 GB | 250 GB ili manje | Replicirati strojeva 85 ili manje.
8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5), 16 GB memorije | 8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5), 12 GB memorije | 600 GB | 250 GB 1 TB | Replicirati između 85 150 strojeva.
12 vCPUs (2 sockets * 6 jezgri @ GHz 2,5), 18 GB memorije | 12 vCPUs (2 sockets * 6 jezgri @ GHz 2,5) 24 GB memorije | 1 TB | 1 TB za 2 TB | Replicirati između strojeva 150 225.


Način skaliranje poslužitelja će ovisi o postavkama za skalu prema gore ili skaliranje out modela.  Proširenja uvođenjem nekoliko visokokvalitetni konfiguraciju i proces poslužitelja ili skalirali uvođenjem više poslužitelja s manje resursima. Na primjer: Ako vam je potrebna zaštita 220 strojeva nije učinite nešto od sljedećeg:

- Postavljanje poslužitelja za konfiguraciju s 12vCPU, 18 GB memorije, poslužitelj za dodatne procesa s 12vCPU, 24 GB memorije i konfiguriranje zaštićeni strojeva koji koriste samo poslužitelj dodatne postupak.
- Umjesto toga nije moguće konfigurirati dva konfiguracije (2 x 8vCPU, 16 GB RAM-a) i dva dodatna postupak poslužitelja (1 x 8vCPU) i 4vCPU x 1 za rukovanje 135 + 85 (220) strojeva i konfiguriranje zaštićeni strojeva poslužitelje sustava dodatne postupak samo.

[Slijedite ove upute](#deploy-additional-process-servers) da biste postavili poslužitelj za dodatne postupak.

### <a name="network-bandwidth-considerations"></a>Razmatranja propusnosti mreže

Možete koristiti alat planer kapaciteta za izračun propusnosti vam je potrebna za replikaciju (Početna replikacije i zatim delta). Omogućuje kontrolu količine korištenja propusnosti za replikaciju imate nekoliko mogućnosti:

- **Throttle propusnosti**: VMware prometa koji umnaža Azure prolazi kroz poslužitelj za određeni proces. Možete throttle propusnosti na računalima izvodi kao poslužitelje postupak.
- **Utjecati propusnosti**: možete utjecati propusnost za replikaciju pomoću nekoliko ključevima registra:
    - Vrijednosti u registru **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** određuje broj niti koji se koriste za prijenos podataka (početne ili delta replikacije) na disku. Veću vrijednost povećava propusnost mreže koji se koristi za replikaciju.
    - **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** određuje broj niti koje se koristi za prijenos podataka tijekom failback.

#### <a name="throttle-bandwidth"></a>Ograničenja propusnosti

1. Otvorite Microsoft Azure sigurnosne kopije BLOG dodatak na računalu ulozi poslužitelj za postupak. Prema zadanim postavkama prečac za Microsoft Azure Backup dostupna na radnoj površini ili u c:\Programske datoteke\Microsoft Azure oporavak usluge Agent\bin\wabadmin.
2. U dodatak kliknite **Promijeni svojstva**.

    ![Ograničenja propusnosti](./media/site-recovery-vmware-to-azure/throttle1.png)

3. Na kartici **Throttling** odaberite **Omogućivanje korištenja propusnosti internetske ograničavanje za sigurnosne kopije operacije**, postavite ograničenja za posao i koje nisu posla sati. Valjani raspon ćete od 512 kb/s 102 MB/s sekundi.

    ![Ograničenja propusnosti](./media/site-recovery-vmware-to-azure/throttle2.png)

[Postavljanje OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet možete koristiti i da biste postavili ograničavanje. Slijedi primjer:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Postavljanje OBMachineSetting NoThrottle** označava da nema ograničavanje potreban je.


#### <a name="influence-network-bandwidth"></a>Utjecati propusnost mreže

1. U registru pronađite **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Da biste utjecati propusnosti promet na replicira disk, promijenite vrijednost **UploadThreadsPerVM**, ili stvoriti ključ ako ne postoji.
    - Da biste utjecati propusnosti za failback promet s Azure, izmijenite vrijednost **DownloadThreadsPerVM**.
2. Zadana vrijednost je 4. U odjeljku "overprovisioned" mreža tih ključeva registra treba promijeniti iz zadane vrijednosti. Maksimalna je 32. Praćenje promet za optimiziranje vrijednost.

## <a name="step-6-replicate-applications"></a>Korak 6: Repliciranje aplikacije

Provjerite je li strojeva želite replicirati pripremljeni su za mobilnost Instalacija servisa, a zatim omogućiti replikacije.

### <a name="install-the-mobility-service"></a>Instalirajte servis za mobilnost

U prvom koraku s omogućivanjem zaštitu virtualnim strojevima i fizičke poslužitelje je instalirati mobilnost servis. To možete učiniti na nekoliko načina:

- **Automatske poslužitelj za postupak**: Kada omogućite replikacije na računalo, automatske i instalirajte komponenta za servis mobilnost poslužitelj za postupak. Imajte na umu da se automatske instalacije neće pojaviti ako strojeva već koriste gore todate verziju komponentu.
- **Automatske Enterprise**: automatski instalirati komponentu pomoću proces automatske enterprise kao što su WSUS ili Upravitelj konfiguracije centar sustava ili [Automatizacija Azure i konfiguracija Desired stanje](./site-recovery-automate-mobility-service-install.md). Postavljanje poslužitelja za konfiguraciju to učiniti.
- **Ručna instalacija**: ručno instalirati komponentu na svakom računalu koje želite replicirati. Postavljanje poslužitelja za konfiguraciju to učiniti.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Priprema za automatsko slanje na računalima sustava Windows

Evo kako pripremiti strojevima sa sustavom Windows tako da servis za mobilnost mogu automatski instalirati web-poslužitelj za postupak.

1.  Stvaranje računa koji se mogu koristiti poslužitelj za postupak da biste pristupili računalu. Račun trebala bi biti administratorske ovlasti (Lokalna ili domena), pa se koristi samo za automatske instalacije.

    >[AZURE.NOTE] Ako ne koristite račun domene morat ćete onemogućiti kontrolu za daljinski pristup korisnika na lokalnom računalu. Da biste to učinili, u dnevniku u odjeljku HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System dodajte stavku DWORD LocalAccountTokenFilterPolicy s vrijednost 1. Da biste dodali stavku registra s vrstom EŽA **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Na Vatrozid za Windows na ovom računalu želite zaštititi, odaberite **Dopusti aplikacije ili značajku u vatrozidu**. Omogući **dijeljenje datoteka i pisača** i **WMI**. Za strojeva koji pripadaju domene možete konfigurirati postavke vatrozida s GPO.

    ![Postavke vatrozida](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Dodavanje računa koji ste stvorili:

    - Otvorite **cspsconfigtool**. Nije dostupan prečac na radnoj površini i nalazi u mapi \home\svsystems\bin [instalirati mjesto].
    - Na kartici **Upravljanje računima** kliknite **Dodaj račun**.
    - Dodajte račun koji ste stvorili. Nakon dodavanja računa morat ćete unesite vjerodajnice Kada omogućite replikacije za računalo.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Priprema za automatsko slanje na poslužiteljima Linux

1.  Provjerite je li strojno Linux koji želite zaštititi podržana kao što je opisano u [zaštićeni strojno preduvjeti](#protected-machine-prerequisites). Provjerite postoji mrežne veze između računala Linux i poslužitelj za postupak.

2.  Stvaranje računa koji se mogu koristiti poslužitelj za postupak da biste pristupili računalu. Račun mora biti korijenski korisnika na s izvorišnim poslužiteljem Linux i koristi samo za instalaciju pribadače.

    - Otvorite **cspsconfigtool**. Nije dostupan prečac na radnoj površini i nalazi u mapi \home\svsystems\bin [instalirati mjesto].
    - Na kartici **Upravljanje računima** kliknite **Dodaj račun**.
    - Dodajte račun koji ste stvorili. Nakon dodavanja računa morat ćete unesite vjerodajnice Kada omogućite replikacije za računalo.

3.  Provjerite da datoteku /etc/hosts na s izvorišnim poslužiteljem Linux sadrži stavke koje mapirati lokalni naziv glavnog računala na IP adrese pridružene sve mrežnih prilagodnika.
4.  Instalirajte najnovije openssh, openssh server, openssl paketa na računalu na koje želite replicirati.
5.  Provjerite je li SSH i pokrenut na priključak 22.
6.  Omogući SFTP podsustav i lozinku za provjeru autentičnosti u datoteci sshd_config na sljedeći način:

    - Prijavite se kao korijen.
    - U datoteci /etc/ssh/sshd_config datoteke, pronađite redak koji započinje **PasswordAuthentication**.
    - Uklonite redak i promijenite vrijednost iz **ne** u **da**.
    - Pronađite redak koji započinje **podsustav** , a zatim uklonite redak.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Ručno instalirajte servis za mobilnost

Za instalaciju dostupne su na poslužitelj za konfiguraciju u **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

Izvorni operacijski sustav | Mobilnost servisa instalacijsku datoteku
--- | ---
Windows Server (samo 64-bitni) | Microsoft ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6,4, 6.5, 6.6 (samo 64-bitni) | Microsoft ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (samo 64-bitni) | Microsoft ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6,4, 6.5 (samo 64-bitni) | Microsoft ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Kliknite pločicu mobilnost servisa Windows Server


1. Preuzmite i pokrenuti odgovarajuću instalacijski program.
2. **Prije nego što počnete** odaberite **servis za mobilnost**.

    ![Servis za mobilnost](./media/site-recovery-vmware-to-azure/mobility3.png)

3. U **Detalji o poslužitelju za konfiguraciju** navedite IP adresu poslužitelja za konfiguraciju i pristupni izraz koja je stvorena kada ste pokrenuli instalaciju Sjedinjeno komuniciranje. Pristupni izraz možete dohvatiti tako da pokrenete: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – v** na poslužitelj za konfiguraciju.

    ![Servis za mobilnost](./media/site-recovery-vmware-to-azure/mobility6.png)

4. **Instalacija** mjesto ostavite zadane postavke, a zatim kliknite **Dalje** da biste započeli instalaciju.
5. U **Tijeku instalacije** nadzor instalacije i ponovno pokrenite računalo ako se to od vas zatraži. Nakon instalacije servis može potrajati oko 15 minuta za status da biste ažurirali na portalu.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Kliknite pločicu mobilnost servisa Windows server putem naredbenog retka

1. Kopirajte instalacijski program u lokalnu mapu (recimo C:\Temp) na poslužitelju na kojem želite zaštititi. Instalacijski program možete pronaći na poslužitelj za konfiguraciju u odjeljku **\home\svsystems\pushinstallsvc\repository [instalirati mjesto]**. Paket za operacijske sustave u Windows će imati slično Microsoft ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe naziv
2. **Preimenujte** datoteku da biste MobilitySvcInstaller.exe
3. Pokrenite sljedeću naredbu da biste izdvojili MSI instalacijskog programa </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>Sintaksa potpuni naredbenog retka

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parametri**

- **/Role:** Obavezno. Određuje je li moguće instalirati mobilnost servisa. Unos vrijednosti Agent | MasterTarget
- **/InstallLocation:** Obavezno. Određuje mjesto za instalaciju servis.
- **/PassphraseFilePath:** Obavezno. Pristupni izraz poslužitelja konfiguracije.
- **/LogFilePath:** Obavezno. Mjesto gdje treba stvoriti instalacijskih datoteka zapisnika.



#### <a name="uninstall-mobility-service-manually"></a>Ručna Deinstalacija servisa mobilnost

Servis za mobilnost možete deinstalirati pomoću Dodavanje uklanjanje programa na upravljačkoj ploči ili pomoću naredbenog retka.

Naredba da biste deinstalirali servisa mobilnost putem naredbenog retka

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Instalirajte servis za mobilnost na poslužitelju Linux pomoću naredbenog retka

1. Kopirajte u arhivu odgovarajuće tar temeljen na tablici iznad stroj Linux želite replicirati.
2. Otvorite program ljuske i izdvojiti zipane tar arhiva za lokalni put tako da pokrenete:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Stvaranje datoteke passphrase.txt u lokalnom direktoriju koji ste izdvojili sadržaj u arhivu tar. Kako to kopirajte pristupni izraz iz C:\ProgramData\Microsoft Azure web-mjesta Recovery\private\connection.passphrase na poslužitelj za konfiguraciju i spremite ga u passphrase.txt tako da pokrenete *`echo <passphrase> >passphrase.txt`* u ljusci.
4. Instalirajte servis za mobilnost tako da pokrenete *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Odredite internu IP adresu poslužitelja za konfiguraciju i provjerite je li priključak 443. Nakon instalacije servis može potrajati oko 15 minuta za status da biste ažurirali na portalu.

**Možete instalirati iz naredbenog retka**:

1. Kopirajte pristupni izraz iz C:\Program Files (x86) \InMage Systems\private\connection na poslužitelj za konfiguraciju, a zatim Spremi kao "passphrase.txt" na poslužitelj za konfiguraciju. Pokrenite te naredbe. U našem primjeru IP adresa za konfiguraciju poslužitelja je 104.40.75.37 i mora biti HTTPS priključak 443:

Da biste instalirali na poslužitelju proizvodnje:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Da biste instalirali na glavni ciljnom poslužitelju:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Omogućivanje replikacije

#### <a name="before-you-start"></a>Prije početka

Ako ste replikaciju VMware virtualne strojeva Imajte na umu sljedeće:

- VMware VMs slučaju otkrivanja svakih 15 minuta, a može potrajati 15 minuta ili više vremena da se prikazuju na portalu nakon otkrivanje. Isto tako otkrivanje može potrajati 15 minuta ili više njih prilikom dodavanja nove vCenter poslužitelja ili vSphere glavno računalo.
- Okruženje za promjene na virtualnog računala (kao što je instalacija Alati VMware) može potrajati i 15 minuta ili više da bi se ažurirati na portalu.
- Možete potražiti na zadnji put otkriven za VMware VMs u polje **Zadnjeg kontakt pri** glavnog računala poslužitelja/vSphere vCenter na plohu **Konfiguraciju poslužitelja** .
- Da biste dodali strojeva za replikaciju bez čekati zakazano otkrivanje, isticanje poslužitelj za konfiguraciju (nemojte kliknuti ga) i kliknite gumb **Osvježi** .
- Kada omogućite replikacije, ako je na računalu se pripremite poslužitelj za postupak automatski instalira servis mobilnost na njemu.

#### <a name="exclude-disks-from-replication"></a>Isključivanje diskova iz replikacije

Kada omogućite replikacije, po zadanom sve diskova na računalo se replicirati. Možete isključiti diskova iz replikacije. Na primjer ne želite replicirati diskova s privremene podatke ili podatke koji sadrži osvježiti svaki put kada na računalu ili ponovnog pokretanja računala (na primjer pagefile.sys ili tempdb SQL Server). Ako želite izostaviti diskova Imajte na umu da:

- Možete isključiti samo diskova koji već imate instaliran servis za mobilnost. Morat ćete [Ručno instaliranje servisa mobilnost](#install-the-mobility-service-manually) jer je servis mobilnost samo instalirana pomoću mehanizam automatske kada je omogućen replikacije.
- Samo osnovnih diskova možete izuzeti iz replikacije. Nije moguće izostaviti OS ili dinamičkih diskova.
- Kada je omogućen replikacije nije moguće je dodati ili ukloniti diskova za replikaciju. Ako želite da biste dodali ili isključiti na disku morate da biste onemogućili zaštite za računalo, a zatim je ponovno omogućili.
- Ako isključili na disku koji je potreban za funkcioniranje aplikacije, nakon prebacivanje na Azure morat ćete ga stvoriti ručno servisu Azure tako da se može pokrenuti aplikaciju repliciranu. U suprotnom se integrira Azure Automatizacija u plan za oporavak da biste stvorili disk tijekom prebacivanje na ovom računalu.
- Ponovno će biti nije diskova ručno stvoriti u Azure. Na primjer ako neće više od tri diskova te dvije izravno u Azure, svih pet će se nije uspjela natrag. Nije moguće izostaviti diskova stvoren ručno iz failback.

**Sada omogućiti replikaciju na sljedeći način**:

1. Kliknite **Korak 2: replicirati aplikacije** > **izvora**. Kada omogućite replikacije prvo ćete kliknite **+ replicirati** u sigurnog da biste omogućili replikacije za dodatnih računala.
2. U plohu **izvora** > **izvor** odaberite poslužitelj za konfiguraciju.
3. U **vrsti** odaberite **virtualnim strojevima** ili **Fizičke strojeva**.
4. U **vCenter/vSphere Hypervisor** odaberite vCenter poslužitelju na kojem se upravlja voditelju vSphere ili odaberite glavnog računala. Ta postavka nije važno ako ste replikaciju fizičke strojeva.
5. Odaberite poslužitelj za postupak. Ako niste stvorili Neki poslužitelji dodatne postupak ta će se naziv poslužitelja za konfiguraciju. Zatim kliknite **u redu**.

    ![Omogućivanje replikacije](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. U **ciljnoj** odaberite sigurnog pretplatu, a u **model implementacije objavu prebacivanje** odaberite model (klasični ili resursa za upravljanje) koji želite koristiti u Azure nakon prebacivanje.
7. Odaberite račun za Azure prostora za pohranu koji će se koristiti za replikaciju podataka. Imajte na umu da:

    - Možete odabrati premium ili računa standardnu prostora za pohranu. Ako odaberete premium računa morat ćete navesti dodatni prostor za pohranu standardni račun za daljnje replikacije zapisnika. Računi mora biti u području isti kao sigurnog servise za oporavak.
    - Ako želite da se pomoću računa za pohranu za različite od onih koje imate koje možete [stvoriti](#set-up-an-azure-storage-account). Da biste stvorili u prostor za pohranu računa pomoću OKVIRA model kliknite **Stvori novi**. Ako želite stvoriti račun za pohranu pomoću klasične modela ćete učiniti te [na portalu za Azure](../storage/storage-create-storage-account-classic-portal.md).

8. Odaberite Azure mreža i podmreže na koji će Azure VMs povezivanje kada ih se spun nakon prebacivanje. Mreža mora biti u području isti kao oporavak servisa sigurnog. Odaberite **Konfiguriraj sada za odabrani strojeva** Primjena postavki mreže na svim računalima odaberete za zaštitu. Odaberite **Konfiguriraj kasnije** da biste odabrali Azure mreže svako računalo. Ako niste u mreži morat ćete [stvoriti](#set-up-an-azure-network). Da biste stvorili mreže pomoću OKVIRA model kliknite **Stvori novi**. Ako želite stvoriti mreže pomoću klasične modela ćete učiniti te [na portalu za Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Ako je primjenjivo, odaberite podmreži. Zatim kliknite **u redu**.

    ![Omogućivanje replikacije](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. Na **virtualnim računalima sustava** > **Odaberite virtualnim strojevima** kliknite i odaberite svakom računalu koje želite replicirati. Možete odabrati samo računala za koje je moguće omogućiti replikacije. Zatim kliknite **u redu**.

    ![Omogućivanje replikacije](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. U **svojstvima** > **Konfiguriranje svojstava**, odaberite račun koji će se na poslužitelju postupak da biste automatski instalirati servis mobilnost na računalu. Prema zadanim postavkama replicirati će se sve diskova. Kliknite **Sve diskova** i poništite sve diskova želite replicirati. Zatim kliknite **u redu**. Naknadno možete postaviti dodatna svojstva.

    ![Omogućivanje replikacije](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. U odjeljku **Postavke replikacije** > **Konfiguracija postavki replikacije** provjerite je li odabran ispravan replikacije pravila. Možete izmijeniti postavke pravilnika za replikaciju u **postavkama** > **replikacije pravila** > naziv pravila > **Uređivanje postavki**. Primjenjuje se promjene primijenile pravilu replicira i novi strojeva.

12. **Višestruki VM dosljednost** omogućiti ako želite prikupiti strojeva u grupu replikacije i navedite naziv za grupu. Zatim kliknite **u redu**. Imajte na umu da:

    - Računalima replikacije grupirati replicirati i omogućili zajedničko korištenje rušenje dosljedan i aplikacije dosljedan oporavak točke kada se ne iznad.
    - Preporučujemo da prikupite fizičke poslužitelje i VMs zajedno da bi se Zrcalili vaše radnih opterećenja. Omogućavanje višestruki VM dosljednost može utjecati na performanse radno opterećenje i trebali biste koristiti samo ako strojeva koristite isti radno opterećenje i morate dosljednost.

    ![Omogućivanje replikacije](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Kliknite **Omogući replikacije**. Možete pratiti napredak posao **Omogućite zaštite** u **postavkama** > **poslove** > **Poslove oporavak web-mjesta**. Nakon što je posao **Dovršavanje zaštitu** pokreće na računalu spremna je za prebacivanje.

> [AZURE.NOTE] Ako je računalo pripremiti za instalaciju automatske komponenta za servis mobilnost instalirat će se kada je omogućen zaštitu. Nakon komponentu je instaliran na računalu pokrene zaštitu posao i ne uspijeva. Nakon neuspjeh ćete morati ručno ponovno pokrenuti svaki računala. Zaštita posao počinje nakon ponovnog pokretanja ponovno i pojavljuje se početni replikacije.

### <a name="view-and-manage-vm-properties"></a>Prikaz i upravljanje svojstvima VM

Preporučujemo da provjerite svojstva izvora računala. Imajte na umu da Azure VM naziv mora biti u skladu s [zahtjevima za Azure virtualnog računala](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Kliknite **Postavke** > **Replicated stavke** >, a zatim odaberite na računalu. **Osnove** plohu prikazuju se podaci o postavke računala i status.

2. U **svojstvima** možete pogledati informacija replikacije i prebacivanje na VM.

    ![Omogućivanje replikacije](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. U **računalnim i mrežne** > **Svojstva za izračun** možete odrediti Azure VM veličinu ime i cilj. Promijenite naziv u skladu s zahtjevima za Azure po potrebi.
Prikaz i dodajte informacije o mreži ciljne, podmreže i IP adresu koja će se dodijeliti Azure VM. Imajte na umu sljedeće:

    - Možete postaviti IP adresu cilja. Ako ne unesete adresu nije uspio putem računala koristit će DHCP. Ako postavite adresu koja nije dostupna na prebacivanje na prebacivanje neće funkcionirati. Iste ciljni IP adresa može se koristiti za testiranje prebacivanje ako je dostupan u mreži prebacivanje test adresu.
    - Broj mrežnih prilagodnika diktirali je veličinom koju navedete za virtualnog računala cilj na sljedeći način:

        - Ako je broj mrežnih prilagodnika na računalu izvor manje od ili jednak broju prilagodnika dopušteno Odredišna veličina računalu, zatim cilj će imati isti broj prilagodnika kao izvor.
        - Ako broj prilagodnika za virtualnog računala izvora premašuje broj dopušten za Odredišna veličina, a zatim Maksimalna veličina ciljne će se koristiti.
        - Ako, primjerice stroj izvora sadrži dvije mrežnih prilagodnika i odredišna veličina računalo podržava četiri, ciljnom računalu će imati dva prilagodnika. Ako na računalu izvor ima dva prilagodnika, ali veličinu podržani cilj podržava samo jedan ciljnom računalu neće imati samo jedan prilagodnika.     
    - Ako na VM ima više mrežnih prilagodnika oni će sve se povezati s istom mrežom.

    ![Omogućivanje replikacije](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. U **diskova** možete vidjeti operacijski sustav i diskova podataka na VM koji je replicirati.


## <a name="step-7-test-the-deployment"></a>Korak 7: Testiranje implementaciju

Da biste testirali implementacijskih možete pokrenuti test prebacivanje za jednu virtualnog računala ili oporavak tarifu koja sadrži jedan ili više virtualnim računalima.


### <a name="prepare-for-failover"></a>Priprema za prebacivanje

- Da biste pokrenuli test prebacivanje preporučujemo stvorite novu Azure mrežu koja sadrži Izolirani u mreži Azure proizvodnje (to je zadano ponašanje prilikom stvaranja nove mreže u Azure). [Dodatne informacije](site-recovery-failover.md#run-a-test-failover) o pokretanju test failovers.
- Da biste ostvarili najbolje performanse kada ne preko Azure, instalirajte Agent za Azure na zaštićenom računalu. Omogućuje pokretanje brže i pomaže s otklanjanjem poteškoća. Instalirajte agent za [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) ili [Linux](https://github.com/Azure/WALinuxAgent) .
- U potpunosti ispitati implementaciju sustava morat ćete do infrastrukture za repliciranu strojno funkcionirati prema očekivanjima. Ako želite testirati servisa Active Directory i DNS možete stvoriti virtualnog računala kao kontroler domene s DNS-a i replicirati to Azure pomoću oporavak Azure web-mjesta. Pročitajte više u [test prebacivanje zahtjevi za Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Provjerite je li poslužitelj za konfiguraciju nije pokrenut. U suprotnom neće uspjeti prebacivanje.
- Ako ste izuzeti diskova iz replikacije možda morati stvoriti te diskova ručno servisu Azure nakon prebacivanje tako da se pokreće aplikaciju prema očekivanjima.
- Ako želite pokrenuti neplanirano prebacivanje umjesto test prebacivanje Imajte na umu sljedeće:

    - Ako je to moguće isključite primarni strojeva prije pokretanja neplanirano prebacivanje. Na taj način nemate izvora i replike računala izvode u isto vrijeme. Ako ste replikaciju VMware VMs pa možete odrediti oporavak web-mjesta trebali napraviti najbolje trud isključiti strojeva izvora. Ovisno o stanju primarni web-mjesta to može ili možda neće funkcionirati. Ako ste replikaciju fizičke poslužitelje oporavak web-mjesta ne nude tu mogućnost.
    - Kada pokrenete neplanirano prebacivanje prestane replikacije podataka iz primarnog strojeva tako da sve delta podataka neće prenijeti nakon neplanirano prebacivanje započinje. Nadalje ako pokrenete neplanirano prebacivanje na tarifu za oporavak funkcionirat će dok se ne dovrši, čak i ako dođe do pogreške.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Priprema za povezivanje s Azure VMs nakon prebacivanje

Ako se želite povezati VMs Azure pomoću RDP nakon prebacivanje, obavezno učinite sljedeće:

**Na računalu na lokalni prije prebacivanja u slučaju pogreške**:

- Za pristup putem Interneta omogućiti RDP, provjerite je li TCP i UDP pravila dodaju za na **javno**i provjerite je li RDP dopušten u **Vatrozid za Windows** -> **dopuštene aplikacije i značajke** za sve profile.
- Za pristup putem web-mjesto veze Omogućivanje RDP na računalu i provjerite je li RDP dopušten u **Vatrozid za Windows** -> **dopuštene aplikacije i značajke** za **domenu** i **privatne** mreže.
- Instalirajte [agent za Azure VM](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) na lokalno računalo.
- [Ručna instalacija servisa mobilnost](#install-the-mobility-service-manually) na računalima umjesto korištenja poslužitelj za postupak da biste automatski automatske servis. To je zato automatske instalacije samo se događa nakon računalu omogućena za replikaciju.
- Provjerite da je operacijski sustav SAN pravilo postavljeno na OnlineAll. [uči više]( https://support.microsoft.com/kb/3031135)
- Isključite servis IPSec prije nego što pokrenete u prebacivanje.

**Na stranici Azure VM nakon prebacivanje**:

- Dodajte javno krajnja točka za RDP protocol (priključak 3389), a zatim navedite vjerodajnice za prijavu.
- Provjerite je li još nemate sva pravila domene koja onemogućiti povezivanje virtualnog računala pomoću javnog adrese.
- Pokušajte se povezati. Ako se ne možete povezati provjerite je li na VM traje. Savjete za otklanjanje poteškoća u ovom [članku](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


Ako želite da biste pristupili programa Azure VM izvodi Linux nakon prebacivanje pomoću ljuske za sigurnu klijenta (ssh), učinite sljedeće:

**Na računalu na lokalni prije prebacivanja u slučaju pogreške**:

- Provjerite je li servis za sigurnu ljuske Azure VM postavljen na automatsko pokretanje na pokretanje sustava.
- Provjerite da pravila vatrozida omogućivanje programa SSH veze na njega.

**Na stranici Azure VM nakon prebacivanje**:

- Na mreži sigurnosne grupe pravila na nije uspio nad VM i Azure podmreže s kojim je povezan morati Dopusti dolazne veze s priključkom SSH.
- Javni krajnjoj točki treba kreirati da dopušta dolazne veze na priključak SSH (TCP priključak 22 po zadanom).
- Ako na VM se pristupa putem veze za VPN-a (usmjeravanje Express ili web-mjesta za web-mjesta VPN) klijent može koristiti povezati izravno na VM putem SSH.

**Na stranici Azure Windows/Linux VM nakon prebacivanje**:

Ako imate sigurnosne grupe mreže vezane uz virtualnog računala ili podmreže kojem računalu pripada, provjerite ima li sigurnosne grupe mreže izlaznog pravilo da biste omogućili HTTP/HTTPS li. I provjerite je li da DNS mreže koju virtualnog računala je početak nije uspjelo preko ispravno konfigurirano. Ostali u prebacivanje može isteći uz poruku o pogrešci-"PreFailoverWorkflow zadatka WaitForScriptExecutionTask timed out". Da biste shvatili to detaljno, pogledajte odjeljak na oporavak [praćenja i vodiča za otklanjanje pogrešaka](site-recovery-monitoring-and-troubleshooting.md#recovery).

## <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

1. Neuspjeh na jednom računalu, u odjeljku **Postavke** > **Replicirati stavki**, kliknite na VM > **+ Test prebacivanje** ikona.

    ![Prebacivanje test](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Neuspjeh putem tarifu za oporavak, u odjeljku **Postavke** > **Oporavak tarife**, desnom tipkom miša kliknite željenu tarifu > **Prebacivanje Test**. Da biste stvorili na oporavak planiranje [, slijedite ove upute](site-recovery-create-recovery-plans.md).

3. **Prebacivanje Test** odaberite Azure mrežu na kojoj će se povezati Azure VMs nakon što se događa prebacivanje.
4. Kliknite **u redu** da biste započeli s pomoćnim. Možete pratiti napredak tako da kliknete na VM da biste otvorili svojstva ili na poslu **Test prebacivanje** u nazivu sigurnog > **Postavke** > **poslove** > **poslove oporavak web-mjesta**.
5. Kada u prebacivanje dosegne status **testiranje dovrši** , učinite sljedeće:

    1. Prikaz virtualnog računala replike Azure portalu. Provjerite je li uspješno pokrene virtualnog računala.
    2. Ako ste postavljenih najviše pristup virtualnim strojevima iz lokalne mreže možete pokrenuti udaljenu radnu površinu vezu virtualnog računala.
    3. Kliknite **dovrši testirati** da biste dovršili ga.

        ![Prebacivanje test](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Kliknite **bilješke** da biste snimili i spremili sve opažanja povezan s pomoćnim test.
    5. Kliknite **test prebacivanje dovršetka** automatski očistiti okruženje za testiranje. Po završetku to prebacivanje test prikazat će se status **Dovršeno** .
    6.  U ovoj fazi elemente ni VMs automatski stvorio oporavak web-mjesta tijekom prebacivanje test se briše. Sve dodatne elemente koji ste stvorili za testiranje prebacivanje neće se izbrisati.

    > [AZURE.NOTE] Ako testiranje prebacivanje više od dva tjedna nastavlja se dovrši po prisilno.


6. Nakon dovršetka na prebacivanje i trebali da biste vidjeli replike Azure računalu prikazuju se na portalu za Azure > **virtualnih računala**. Provjerite je li u VM na odgovarajuću veličinu, koji ima povezani s odgovarajućim mrežom i je li pokrenut.
7. Ako ste [spremni za veze nakon prebacivanje](#prepare-to-connect-to-azure-vms-after-failover) trebali povezati Azure VM.

## <a name="monitor-your-deployment"></a>Praćenje implementaciju sustava

Evo kako možete pratiti konfiguracijske postavke, status i stanja za implementaciju sustava oporavak web-mjesta:

1. Kliknite naziv sigurnog pristupa na nadzornoj ploči **Essentials** . U ovoj nadzornoj ploči možete poslovi oporavak web-mjesta, replikacije status, tarife za oporavak, dijagnostiku poslužitelja i događaje.  Možete prilagoditi Essentials da bi se prikazala pločice i rasporedima koje su najkorisniji za vas, uključujući stanje druge sefovi oporavak web-mjesta i sigurnosno kopiranje.<br>
![Osnove](./media/site-recovery-vmware-to-azure/essentials.png)

2. Na pločici **stanja** možete pratiti web-mjesta (VMM ili konfiguracija) poslužitelji koji se pojavio problem i događaje upućuju oporavak web-mjesta u zadnja 24 sata.
3. Možete upravljati i praćenje replikacije u **Replicirati stavki**, **Oporavak tarife**, i pločice **Poslovi oporavak web-mjesta** . Moguće je dubinski analizirati u zadacima u **postavkama** -> **poslove** -> **Poslove oporavak web-mjesta**.


## <a name="deploy-additional-process-servers"></a>Implementacija poslužitelja dodatne postupak

Ako imate skalirali izvan uvođenja osim 200 strojeva izvora ili ukupni dnevni churn stopa više od 2 TB, morat ćete dodatne postupak poslužitelji za rukovanje glasnoće promet.

Provjera [veličine preporuke za proces poslužitelja](#size-recommendations-for-the-process-server) , a zatim slijedite ove upute za postavljanje poslužitelja postupak. Nakon postavljanja poslužitelj ćete migrirati izvor strojeva ga koristiti.

### <a name="install-an-additional-process-server"></a>Instalacija poslužitelj za dodatne postupak

1. U odjeljku **Postavke** > **poslužitelji za oporavak web-mjesta** kliknite poslužitelj za konfiguraciju > **poslužitelj za postupak**.

    ![Dodavanje postupak poslužitelja](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. **Vrsta poslužitelja** kliknite **postupak server (lokalno)**.

    ![Dodavanje postupak poslužitelja](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Preuzmite instalacijski program za Sjedinjeno komuniciranje web-mjesta oporavak datoteka i pokrenite ga instalirati na poslužitelj za postupak i registrirati u na sigurnog.
4. **Prije nego što počnete** odaberite **Dodaj dodatne postupak poslužitelji za promjenu veličine out implementacije**.
5. Dovršite čarobnjak na isti način kao i kada niste [postavili](#step-2-set-up-the-source-environment) poslužitelj za konfiguraciju.

    ![Dodavanje postupak poslužitelja](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. U **Detalji o poslužitelju za konfiguraciju** navedite IP adresu poslužitelja za konfiguraciju i pristupni izraz. Da biste dobili pristupni izraz pokrenuti ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na poslužitelj za konfiguraciju.

    ![Dodavanje postupak poslužitelja](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migriranje strojeva koji koriste novi poslužitelj za postupak

1. U odjeljku **Postavke** > **poslužitelja za oporavak web-mjesta** kliknite poslužitelj za konfiguraciju i proširivanje **poslužitelje postupak**.

    ![Ažuriranje poslužitelja postupak](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Desnom tipkom miša kliknite poslužitelj za postupak trenutno se koristi, a zatim kliknite **Promjena**.

    ![Ažuriranje poslužitelja postupak](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. U **odaberite ciljni postupak poslužitelj**, odaberite novi poslužitelj za postupak koji želite koristiti, a zatim odaberite novi poslužitelj za postupak će obrađivati virtualnim računalima sustava. Kliknite ikonu informacije da biste dobili informacije o poslužitelju. Da bi vam učitavanje odluke, prikazuje se average prostora koji je potreban za replikaciju svaki odabrani virtualnog računala da biste novi poslužitelj za postupak. Kliknite kvačicu da biste pokrenuli replikaciju novi poslužitelj za postupak.

## <a name="vmware-account-permissions"></a>Dozvole za račun VMware

Poslužitelj za postupak mogu automatski otkriti VMs na poslužitelju vCenter. Za automatsko otkrivanje morat ćete [odrediti ulogu (Azure_Site_Recovery)](#prepare-an-account-for-automatic-discovery) da biste omogućili oporavak web-mjesta za pristup poslužitelju VMware. Imajte na umu da ako samo želite migrirati VMware strojeva Azure i ne morate failback iz Azure, možete definirati samo za čitanje ulozi koja je dovoljno. Dozvole potrebne uloga su navedene u tablici u nastavku.

**Uloga** | **Pojedinosti** | **Dozvole**
--- | --- | ---
Azure_Site_Recovery uloga | Otkrivanje VMware VM |Dodjela ovlasti za centar za v poslužitelj:<br/><br/>DataStore -> Alociraj prostor, datastore Pregledaj, niske razine datoteke operacije, datoteku, datoteke ažuriranja virtualnog računala<br/><br/>Mrežni -> Dodjela mreže<br/><br/>Resurs -> Dodjela virtualnog računala da biste skup resursa, migriranje isključeno virtualnog računala, migriranje uključen virtualnog računala<br/><br/>Zadaci -> Stvaranje zadatka, ažuriranje zadatka<br/><br/>Konfiguriranje -> virtualnog računala<br/><br/>Jezici predloška interakcije -> virtualnog računala -> odgovora pitanje, uređaj veze, konfiguriranje CD medija, Konfiguriraj meki medijski sadržaji isključeno, Power na, instalirajte VMware Alati<br/><br/>Zalihe -> virtualnog računala -> neregistriranja stvaranje dnevnika<br/><br/>Virtualnog računala -> Provisioning -> Preuzmi Dopusti virtualnog računala, prijenos datoteka Dopusti virtualnog računala<br/><br/>Virtualnog računala -> snimke -> Ukloni snimke
vCenter korisnička uloga | Otkrivanje VMware VM/prebacivanje bez zatvaranja izvora VM | Dodjela ovlasti za centar za v poslužitelj:<br/><br/>Objekt podatkovnog centra –> Propagate na podređeni objekt uloga = samo za čitanje <br/><br/>Korisnik je dodijeljen razini podatkovnog centra i s podatkovnim centrom ima pristup svim objektima.  Ako želite ograničiti pristup dodijeliti ulogu **bez pristupa** s objektom **Propagate djetetu** podređene objekte (domaćini vSphere, datastores, VMs i mreža).
vCenter korisnička uloga | Prebacivanje i failback | Dodjela ovlasti za centar za v poslužitelj:<br/><br/>Objekt podatkovnog centra – Propagate podređeni objekt, uloge = Azure_Site_Recovery<br/><br/>Korisnik je dodijeljen razini podatkovnog centra i s podatkovnim centrom ima pristup svim objektima.  Ako želite ograničiti pristup dodijeliti ulogu **bez pristupa** s **Propagate podređeni objekt** podređeni objekt (domaćini vSphere, datastores, VMs i mreža).  
## <a name="next-steps"></a>Daljnji koraci

- [Dodatne informacije](site-recovery-failover.md) o različitim vrstama prebacivanje.
- [Dodatne informacije o failback](site-recovery-failback-azure-to-vmware.md) da bi se prikazala sustava nije uspjelo putem računala izvode u Azure natrag na lokalnog okruženja.

## <a name="third-party-software-notices-and-information"></a>Obavijesti trećih strana softver i podataka

Ne šalji prijevod i Localize

Softver i opremu koji se izvodi u Microsoft proizvoda ili usluga temelji se na ili ugrađuje materijala iz projekata navedene (skupno, "trećih strana kod").  Microsoft se neće izvorni autor šifre drugih proizvođača.  Izvorni autorskim i licence, u odjeljku Microsoft primili takve kod drugih proizvođača postavljene su naprijed ispod.

Informacije u odjeljku A koji se odnosi kod drugih proizvođača komponente s projektima navedena u nastavku. Informativna svrhe su dani takve licence i informacije.  Kod drugih proizvođača je u tijeku relicensed vam Microsoft u odjeljku o licenciranju odredbe za Microsoftov softver Microsoftovih proizvoda ili usluge.  

Informacije u odjeljku B koji se odnosi komponente kod drugih proizvođača koji su se bili dostupni na Microsoft u odjeljku izvorni uvjete licenciranja.

Cijeli dokument nalazi se u [Microsoftovu centru za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft zadržava sva prava izričito ne odobren spominju u ovom dokumentu, po patentnim, estoppel ili neki drugi način.
